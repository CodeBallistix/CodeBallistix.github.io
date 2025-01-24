---
title: "Streaming Logs from 1 Django Instance to Another"
date: 2023-11-28
draft: false
summary: "Ideas on how logs may be streamed from 1 django instance to another."
description: "Ideas on how logs may be streamed from 1 django instance to another."
tags: [django, python, logging]
series: ["Django Object Sync"]
series_order: 3
showAuthor: true
authors:
  - codeballistix
showAuthorBadges: true
---
{{< lead >}}
We were working on an online-offline product that required changes done to a local server to be synced to a global central server. 
Changes could only be made on the local server and a complete view of the database must be available on the global central server. 
All of this in Django and python3.

This article, is the 3rd in a [series of 3](/series/django-object-sync/), describes how to stream objects from 1 django instance to another,
with focus on the transfer of objects from 1 server to another.
{{< /lead >}}

## TL;DR Summary

There is none. Take this as a output of a lot of research. No shortcuts here.

## Introduction

In the previous articles we developed the capacity to log all 
Database Operations to files and consequesntly ingest these files
on the other instance. The problem now reamins of shupping these 
lines of logs or objects from one system to another.

## Let's Dive

### Third Party Applications
Considering that we have already written these logs to a file, the obvious
solution to this will be to use a third party application to stream this file
line by line to another system. This also decouples the handling of streaming
the logs from your python code.

1.  Stream as Syslog using Rsyslog
    
    This provides many types of configurations. Mutual Authentication using TLS,
    or just plain UDP.
1.  Stream as a File using Filebeat to a Logstash Endpoint and then write back into a file.

    This is another flexible post-processing capable way streaming JSON objects from 
    a file on 1 server to another.
1.  Periodically syncing this file between 2 servers using rsync is another way. This
    is not a very nice way to be honest but could work as a cheap linux workaround.
1.  Pure Socket Programming to in python to ship lines from here to there.

    This requires a pair of compatible programs running at both the ends. 

### Integrating this into Django/Python

This would require tweaking your code, but makes the application ship well as a single unit.

1.  Python and Django provide many kinds of loggers. One of those is the `SocketHandler`.
    Using this logger you may directly ship the logs from 1 server to another without
    the need to create a file as a buffer. Further using a custom `manage` command, the 
    stream of JSON objects over the socket could be directly ingested into Django.

1.  Similar to the above option, we could use `SyslogHandler` to ship logs to the server.
    This will make the logs being written to a file on the other server. The manage command
    written previously could be tweaked to ingest these logs. Consider using a TLS wrapped custom
    Syslog Handler. Look at <https://gitlab.com/thelabnyc/python-tls-syslog> or the 
    [`tls-syslog`](https://pypi.org/project/tls-syslog/) python library.

1.  Yet another option is to create a `multiprocessing.Queue` in a separate process, and use the
    `QueueHandler` as the log handler. This allows for many felxible alternatives:

    1.  This `QueueHandler` may send objects as single HTTP calls over the network. The
        central server can have an HTTP endpoint to ingest these objects. This will work
        beautifully for low **rate** of changes being ingested.
    
    1.  Along the same lines, using Django Channels, a websocket can be created to ingest
        streams of JSON objects from various processes. This may work for high **rate** of changes
        being ingested.

    Why I had put in a QueueHandler you ask? Well because using HTTP or asynchronous calls may change the
    order of application of changes, which may break foreign key contstraints. The Queue in between
    will serialize (in the sense of database operations) all the operations.

## Conclusion

We laid down so many ideas. We implemented a few of these as well. 
We let the decision of using appropriate technologies to you. Contact us for creating such apps
professionally.

