---
layout: post
title: "RhodeCode: remove outdated session data"
description: "RhodeCode uses Beaker for session management. By default the
              Beaker backend is set to file storage. Cleaning up those files
              by time is necessary."
category:
tags: [Mercurial, RhodeCode, Monitoring]
---
{% include JB/setup %}

**RhodeCode uses Beaker for session management. By default the Beaker backend is
set to file storage. Cleaning up those files by time is necessary.**

We use [RhodeCode](http://rhodecode.org/) for our internal
[Mercurial](http://mercurial.selenic.com/) repositories. It's great for what it
does: user and group management, repository groups, etc.

But when I looked at our server today I was kind of shocked about the inode
usage on one of our partitions. Checking out the Munin graphs I realized that
it had something todo with our repos. But the repository folder wasn't that
big, only about 160MB. So I was looking for the inode usage:

    $ find /opt/repositories/ -type f | wc -l
    14110

That's not too much. Also because df shows a usage of about 500.000 inodes. Ok,
checking the RhodeCode directory:

    $ find /opt/rhodecode/ -type f | wc -l
    483992

Yes! But wait. Where does all the files come from?

RhodeCode uses Beaker for session management. And by default, file storage is
used. This isn't that bad, but from the Beaker documentation you can see that
it could be:

> Beaker does not automatically delete expired or old cookies on any of its
> back-ends. This task is left up to the developer based on how sessions are
> being used, and on what back-end. The database backend records the last
> accessed time as a column in the database so a script could be run to delete
> session rows in the database that haven’t been used in a long time.

So for the file backend that means that all the session files are kept.
Forever!

But help is here! You can remove outdated session files with a simple command:

    $ find /opt/rhodecode/data/sessions -mtime +3 -exec rm {} \;

This removes all the session files older than 3 days. The result is amazing:

    $ find /opt/rhodecode/data/session/ -type f | wc -l
    4528

If you're lazy like me, simply add a cronjob and let your system do the rest:

    # m h  dom mon dow   command
    0 1 1 * * find /opt/rhodecode/apps/propertyshelf/data/sessions -type f -mtime +3 -exec rm {} \;

This removes old session files on every 1st of a month at 1:00am.
