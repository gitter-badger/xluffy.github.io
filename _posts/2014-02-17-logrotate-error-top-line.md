---
layout: post
title:  "Error reading top line, Logrotate"
date:   2014-02-18
tags: log, nginx, apache, linux
description: "Error reading top line, Logrotate"
---

If you get an error saying

```
    /etc/cron.daily/logrotate:
    error: error reading top line of /var/lib/logrotate.status
```

delete /var/lib/logrotate.status, and then run logrotate once with -f flag, like

```
    $ ~logrotate -f /etc/logrorate.d/syslog
```

This should initialize the status file and the error should not be repeating.
