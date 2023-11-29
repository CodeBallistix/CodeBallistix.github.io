---
title: "Streaming Objects from a Django Instance to Another"
date: 2023-11-28
draft: false
summary: "How to sync Django ORM Model Obects from a instance of Django to another?"
description: "How to sync Django ORM Model Obects from a instance of Django to another?"
tags: [django, python, signals, django orm, orm, object relational model, python3, operational log, oplog]
series: ["Django Object Sync"]
series_order: 1
showAuthor: true
authors:
  - mrinank
showAuthorBadges: true
---
{{< lead >}}
We were working on an online-offline product that required changes done to a local server to be synced to a global central server. 
Changes could only be made on the local server and a complete view of the database must be available on the global central server. 
All of this in Django and python3.

This article, in a [series of 3](/series/django-object-sync/), describes how to create an operational log of the additions, changes and deletion to a database using Django. We begin by describing the Task.
{{< /lead >}}

## TL;DR Summary

We created a repo for this code on github.
If you have no time then head over there and hack it.

[{{< icon "github" >}}Django Objects Sync](https://github.com/CodeBallistix/djangoObjectsSync)

## Introduction

### Task

Imagine you have local [Django](https://www.djangoproject.com/) application that saves and stores a lot of information. 
This information is constantly changing and increasing in size. 
This information must now be sent to a central server where the same information is to be viewed.

This solves 2 problems:
1. What if a LAN installed django app requires a view-only client on the internet?
1. What if you need a Real-time operation-by-operation log that can be used as a backup or streamed to a separate machine?
  This can be a data availability solution too.

### Ideas

1. [Django Database Backup](https://django-dbbackup.readthedocs.io/en/master/) provides an easy way to backup the database.
  But does not provide the ability to stream live changes.
1. Synchronising the underlying databases.
  The databases could be [PostgreSQL](https://www.postgresql.org/), [MySQL](https://www.mysql.com/),
  or any other that django supports. But this limits interoperability.
  What if the other instance uses [SQLite](https://www.sqlite.org/index.html), which is the default anyway?
1. Make a custom Operational Log of the changes being made on the database through Django. 
  Stream this Log. This seems interesting.
1. We can use Django Serializers for translating Django Objects to JSON.

### Django Signals

At the time of creating this solution the major version of Django is 4.2.
This version of Django 

> <cite>includes a “signal dispatcher” which helps decoupled applications 
> get notified when actions occur elsewhere in the framework. 
> In a nutshell, signals allow certain senders to notify 
> a set of receivers that some action has taken place. 
> They’re especially useful when many pieces of code may be interested in the same events.
> [^1]</cite>

[^1]: <https://docs.djangoproject.com/en/4.2/topics/signals/>

### Django Serializers

> <cite>
> Django’s serialization framework provides a mechanism for 
> “translating” Django models into other formats. 
> Usually these other formats will be text-based and used for 
> sending Django data over a wire, but it’s possible for a serializer 
> to handle any format (text-based or not).
> [^2]
> </cite>

[^2]: <https://docs.djangoproject.com/en/4.2/topics/serialization/>

So, we can serialize our objects to JSON and ship them through streams
or store them in text file.

## Deep Dive

Let us start by writing code for creating the said Operational Log.

We assume that you know enough about django to create a Project
and an Application. Let's create a project named `signalsTest`,
and create an application named `polls` in it.

### Special UUID Key
> for Multiple Instances shipping to a Single Instance. This is not necessary for
> single writable instance to single readable instance streaming.

Consider Relational References between objects in the Django ORM.
Django ORM creates an incremental number primary key by default.
All relationships between objects, by default, use this primary key.
If we ship this primary key in the Operational Log as is and it is
ingested by the single global instance, the relationships could be become
invalid in case of primary key overlaps. This is inevitable.

To counter this we created a special `UUIDModel` which must be
inherited by all models that must be shipped to a single instance 
from multiple instances. In this model we create a special `id`
column typed as a `UUIDField`, as can be seen below in code provided.

> This allows your global application instance to be a single tenant from 
> multiple local source application instances. This is not necessary for
> single writable instance to single readable instance streaming.

{{< highlight python "linenos=inline,hl_lines=3,linenostart=1" >}}
class UUIDModel(models.Model):
    pkid = models.BigAutoField(primary_key=True, editable=False)
    id = models.UUIDField(default=uuid.uuid4, editable=False, unique=True)

    class Meta:
        abstract = True
{{< / highlight >}}

Example usage of this model is given in the 
[{{< icon "github" >}}Django Objects Sync](https://github.com/CodeBallistix/djangoObjectsSync/blob/blog/polls/models.py):

{{< highlight python "linenos=inline,hl_lines=1 11-12,linenostart=14" >}}
class Question(UUIDModel):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField("date published")
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
    def __str__(self):
        return self.question_text
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)

class Choice(UUIDModel):
    question = models.ForeignKey(Question, to_field='id', on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
    def __str__(self):
        return self.choice_text
{{< / highlight >}}

As you can see on the code above, Foreign Key to the `Question` Model is made using
`UUID` typed `id` from the `UUIDModel`.

### Django Logging

We can use the built in Django Logging to create a logging 
mechanism for the Operational Log. Example for this is given at
[{{< icon "github" >}}Django Objects Sync](https://github.com/CodeBallistix/djangoObjectsSync/blob/blog/signalsTest/settings.py):

{{< highlight python "linenos=inline,hl_lines=8-10 21-28 36-40,linenostart=126" >}}
LOGGING = {
    'version': 1,
    'disable_existing_loggers': True,
    'formatters': {
        'standard': {
            'format': '%(asctime)s [%(levelname)s] %(name)s: %(message)s'
        },
        'oplog_formatter': {
            'format': '%(created)f %(message)s'
        },
    },
    'handlers': {
        'default': {
            'level':'DEBUG',
            'class':'logging.handlers.RotatingFileHandler',
            'filename': './logs/app.log',
            'maxBytes': 1024*1024*5, # 5 MB
            'backupCount': 5,
            'formatter':'standard',
        },
        'oplog_handler': {
            'level':'DEBUG',
            'class':'logging.handlers.RotatingFileHandler',
            'filename': './logs/oplog.log',
            'maxBytes': 1024*1024*10, # 10 MB
            'backupCount': 30,
            'formatter':'oplog_formatter',
        },
    },
    'loggers': {
        '': {
            'handlers': ['default'],
            'level': 'DEBUG',
            'propagate': True
        },
        'oplogger': { 
            'handlers': ['oplog_handler'],
            'level': 'DEBUG',
            'propagate': False
        },
    },
}
{{< / highlight >}}

Here we have forced the logging system to provide an epoch timestamp.
This is for keeping track of last time we ingested the log.

### Operational Log

This is fairly simple. We need to listen to all `post_save` and `post_delete`
events, and write to the `oplogger` logger. 
In addition we must treat the **SAVE**
and **DELETE** events separately as they must be ingested differently.
To do this we add the string `SAVE` before the **SAVE** oplogs, and 
the string `DELETE` before the **DELETE** oplogs.

We create a new file named `signals.py` in the `polls` app for this. View the code for this at 
[{{< icon "github" >}}Django Objects Sync](https://github.com/CodeBallistix/djangoObjectsSync/blob/blog/polls/signals.py):

{{< highlight python "linenos=inline,hl_lines=10 13-14 20 23-24,linenostart=1" >}}
from django.db.models.signals import post_save, post_delete
import polls.models
from django.dispatch import receiver
from django.core import serializers
import logging

oplogger = logging.getLogger('oplogger')

 
@receiver(post_save) 
def model_saved(sender, instance, **kwargs):
    print(instance.__class__.__name__)
    if instance.__class__.__name__ == 'ProcessedFile' or instance.__class__.__name__ == 'Migration':
        return
    // serializers explained next
    data = serializers.serialize('json', [instance, ])
    log = f'SAVE {data}'
    print(log)
    oplogger.info(msg=log)

@receiver(post_delete) 
def model_deleted(sender, instance, **kwargs):
    print(instance.__class__.__name__)
    if instance.__class__.__name__ == 'ProcessedFile' or instance.__class__.__name__ == 'Migration':
        return
    // serializers explained next
    data = serializers.serialize('json', [instance, ])
    log = f'DELETE {data}'
    print(log)
    oplogger.info(msg=log)
{{< / highlight >}}



## Demo

1. Download or clone the code at [{{< icon "github" >}}Django Objects Sync](https://github.com/CodeBallistix/djangoObjectsSync/tree/blog).

1. Switch to the `blog` branch.

1. Install the `pip` requirements from the `REQUIREMENTS.TXT` file.

1. Run the migrations for django.

1. Run the Following Commands in django shell after all migrations have been made to verify functionality.

    ```python3
    from polls.models import Choice, Question
    from django.utils  import timezone
    q = Question(question_text="What's new?", pub_date=timezone.now())
    q.save()
    q.choice_set.create(choice_text='Nothing',votes=0)
    c=q.choice_set.create(choice_text='sky',votes=0)
    c.delete()
    c.save()
    ```

1. The following output will be created and similar logs will be appended to the file at logs/oplog.log.
    ```txt
    1698395500.971553 SAVE [{"model": "polls.question", "pk": 1, "fields": {"id": "a3f4729c-b3c2-4264-82a6-8ef7abeaec96", "question_text": "What's new?", "pub_date": "2023-10-27T08:31:40.182Z"}}]
    1698395542.826431 SAVE [{"model": "polls.choice", "pk": 1, "fields": {"id": "d6d2c0a0-6dfa-4515-875e-f1edfd3dbf01", "question": "a3f4729c-b3c2-4264-82a6-8ef7abeaec96", "choice_text": "Nothing", "votes": 0}}]
    1698395568.857164 SAVE [{"model": "polls.choice", "pk": 2, "fields": {"id": "d8e48b8d-9ccb-4728-b90c-2833592b6fd2", "question": "a3f4729c-b3c2-4264-82a6-8ef7abeaec96", "choice_text": "sky", "votes": 0}}]
    1698395573.036682 DELETE [{"model": "polls.choice", "pk": 2, "fields": {"id": "d8e48b8d-9ccb-4728-b90c-2833592b6fd2", "question": "a3f4729c-b3c2-4264-82a6-8ef7abeaec96", "choice_text": "sky", "votes": 0}}]
    1698395577.627465 SAVE [{"model": "polls.choice", "pk": 3, "fields": {"id": "d8e48b8d-9ccb-4728-b90c-2833592b6fd2", "question": "a3f4729c-b3c2-4264-82a6-8ef7abeaec96", "choice_text": "sky", "votes": 0}}]
    ```

## Conclusion

Ingestion of these logs is fairly simple, and will be dealt with
in the next blog in this series.
