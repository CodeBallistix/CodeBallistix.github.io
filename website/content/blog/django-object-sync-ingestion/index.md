---
title: "Ingesting Objects from another Django Instance"
date: 2023-11-28
draft: false
summary: "Ingesting Django objects from a log or a stream of JSONified django objects."
description: "Ingesting Django objects from a log or a stream of JSONified django objects."
tags: [django, python, signals, django orm, orm, object relational model, python3, operational log, oplog, ingestion, management, management command, manage.py, django admin, django-admin, admin, django-admin command]
series: ["Django Object Sync"]
series_order: 2
showAuthor: true
authors:
  - codeballistix
showAuthorBadges: true
---
{{< lead >}}
We were working on an online-offline product that required changes done to a local server to be synced to a global central server. 
Changes could only be made on the local server and a complete view of the database must be available on the global central server. 
All of this in Django and python3.

This article, is the 2nd in a [series of 3](/series/django-object-sync/), describes how to ingest objects from operational log of the additions, changes and deletion to a database using Django. We begin by describing the Task.
{{< /lead >}}

## TL;DR Summary

We created a repo for this code on github.
If you have no time then head over there and hack it.

[{{< icon "github" >}}Django Objects Sync](https://github.com/CodeBallistix/djangoObjectsSync)

## Introduction

### Task

Consider a stream of Django JSONified objects arriving in a file from another django instance.
We need to ingest these objects in our django instance. These records might either be
**SAVE** records or **DELETE** records. **SAVE** records can be for insertions or updates.

Plus we must make sure we do not miss any records in this stream.
Reingesting records is not a problem as it will not lead to inconsistency.

### Ideas

1. The code must be a part of the Django framework so as to know the models
    in the django project but not an API as the ingestion happens from a file.
1. The program must continue from where it last stopped so that no records are
    missed. Even after program restarts. Must store the last ingestion point
    of the file.
1. What if we use a Django-admin command to watch for changes in a file
    and ingest the changes?

### Django-Admin Commands

When this blog is being made the current django version in vogue is 4.2. This
version has **django-admin** commands. 

> <cite>Applications can register their own actions with manage.py. 
> For example, you might want to add a manage.py action for a Django app that 
> you’re distributing. [^1]</cite>

[^1]: <https://docs.djangoproject.com/en/4.2/howto/custom-management-commands/>

## Deep Dive

### Custom Django Admin Command

We create a new Django Admin command by creating a file at the location
`polls/management/commands/ingest_objects.py`. 
In this file we create a new class as follows:

```python3
class Command(BaseCommand):
    help = 'Read serialized objects from a file and save them, watching for changes in the file.'

    def add_arguments(self, parser):
        parser.add_argument('file_path', type=str, help='Path to the file to watch and ingest objects from.')

    def handle(self, *args, **options):
        pass
```

This is where we setup the command. We define that the command takes
a file path as arguments, from where it must ingest the logs.


### Watching for Changes

Let's talk about how this command must work?

We must observe for changes in the said file and start ingestion as soon as possible.
Notice that we do not know whether a single log has appeared or many. So we must
ingest all the logs which have arrived and then continue watching after that.

We will use [watchdog](https://pypi.org/project/watchdog/) python module to watch
for changes in a file. Let's code this in the command's `handle` method.

{{< highlight python "linenos=inline,hl_lines=3 6,linenostart=1" >}}
def handle(self, *args, **options):
    file_path = options['file_path']
    handler = ObjectIngestHandler(file_path)
    handler.ingest_objects()
    observer = Observer()
    observer.schedule(handler, os.path.dirname(file_path), recursive=False)
    observer.start()
    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        observer.stop()
    observer.join()
{{< / highlight >}}

So we find the `file_path` required to be observed and pass this to the observer
which is an instance of the `ObjectIngestHandler` class.

Let us define what this class does.

### ObjectIngestHandler

{{< highlight python "linenos=inline,hl_lines=7 11 15,linenostart=1" >}}
class ObjectIngestHandler(FileSystemEventHandler):
    def __init__(self, file_path):
        self.file_path = file_path

    def on_modified(self, event):
        if event.src_path == self.file_path and event.event_type == 'modified':
            // what happens when file is modified.
            self.ingest_objects()
    
    def on_created(self, event):
        if event.src_path == self.file_path:
            // what happens when file is created.
            self.ingest_objects()
    
    def ingest_objects(self):
        try:
            // this is where the file will be processed
        except EOFError:
            // The file has ended so nothing more to do
            pass
        except Exception as e:
            logger.error(msg=f'ingest_objects {e}')
            logger.error(msg=f'ingest_object ERROR {traceback.format_exc()}')
{{< / highlight >}}

### Processing the File

We must ascertain whether the said file has been processed already and till
what line number. If the file has been recreated, start from position 0 otherwise
start from the last place where the processing was completed.

Let's create a Django Model for storing this information. 
I know I know, this is a hack! But this is the simplest solution for now.
We keep the other complex solutions as food for thought.

Following is a simple model to track the position of the file 
tracked till now.

```python3
class ProcessedFile(models.Model):
    file_path = models.CharField(max_length=255, unique=True)
    last_processed_position = models.PositiveIntegerField(default=0)
```

We will use this model for saving the progress over a file so that no records are missed.
Follow along the code with the **comments** to understand what is happening.

{{< highlight python3 "linenos=inline,linenostart=1" >}}
def ingest_objects(self):
    try:
        // 'fetch last position of this file if available.'
        processed_file, created = ProcessedFile.objects.get_or_create(file_path=self.file_path)
        // 'open the file'
        with open(self.file_path, 'r') as file:
            file_size = os.fstat(file.fileno()).st_size
            // 'check whether the file has been truncated after the last run'
            if processed_file.last_processed_position > file_size:
                processed_file.last_processed_position = 0
            // 'seek to the appropriate position'
            file.seek(processed_file.last_processed_position)
            // 'read line by line till the end of the file'
            line = file.readline()
            while line:
                // 'we will create this next'
                ingest_object(line)
                line = file.readline()
            // 'find the end of file location, and save it for next run'
            processed_file.last_processed_position = file.tell()
            processed_file.save()
    except EOFError:
        pass
    except Exception as e:
        logger.error(msg=f'ingest_objects {e}')
        logger.error(msg=f'ingest_object ERROR {traceback.format_exc()}')
{{< / highlight >}}



### Ingesting the Object

Let us develop the `ingest_object` method that de-serializes the JSON objects to django ORM.

For logs like:
```txt
1698395500.971553 SAVE [{"model": "polls.question", "pk": 1, "fields": {"id": "a3f4729c-b3c2-4264-82a6-8ef7abeaec96", "question_text": "What's new?", "pub_date": "2023-10-27T08:31:40.182Z"}}]
```

we need to split the logs on spaces and separate the time, operation and the actual object.

Let us ignore the time field for now.

```python3
s = line[line.find(' ')+1:]
```
Now in the variable `s` we have the remaining line, formatted as `OPERATION JSON-OBJECT`.

Let us find the operation now.

```python3
op = s[:s.find(' ')]
```
The `op` variable now contains the operation


Let us now find the object, and deserialize it.

```python3
data = serializers.deserialize('json', s[s.find(' ')+1:],)
```

The `data` variable now contains the deserialized object in an python list-like object.

For the **DELETE** operation we must find the relevant object ID and have it deleted from the ORM.
The code is highlighted in the next code block

{{< highlight python3 "linenos=inline,hl_lines=12,linenostart=1" >}}
def ingest_object( line):
    try:
        s = line[line.find(' ')+1:]
        op = s[:s.find(' ')]
        data = serializers.deserialize('json', s[s.find(' ')+1:],)
        if op == 'SAVE':
            for obj in data:
                obj.save()
                logger.info(msg=f'ingest_object SAVED Object ID {obj.object.id}')
        elif op == 'DELETE':
            for obj in data:
                o = obj.object.__class__.objects.get(id=obj.object.id)
                o.delete()
                logger.info(msg=f'ingest_object DELETE Object ID {obj.object.id}')
    except Exception as e:
        logger.error(msg=f'ingest_object {e} {line}')
        logger.error(msg=f'ingest_object ERROR {traceback.format_exc()}')
{{< / highlight >}}

Bringing it all together the code is available at 
[{{< icon "github" >}}Django Objects Sync](https://github.com/CodeBallistix/djangoObjectsSync/blob/blog/polls/management/commands/ingest_objects.py).

## Demo

1. Download or clone the code at [{{< icon "github" >}}Django Objects Sync](https://github.com/CodeBallistix/djangoObjectsSync/tree/blog).

1. Switch to the `blog` branch if cloned.

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

1. Create another clone of the repository or download and unzip the code at another location.

1. Run the migrations at the new location.

1. Find the absolute path of the oplog from the last run of the previous clone.

1. Run the `ingest_objects` command using `manage.py` with the absolute path of the oplog from the last run of the previous code.

1. Inspect Django ORM of the new clone using Django shell of SQLite Viewer.

## Conclusion

So far so good. We now know how to create the oplog, and how to ingest the oplog.
Let us find ways to stream these logs over to the new instance in the next blog
in this series.
