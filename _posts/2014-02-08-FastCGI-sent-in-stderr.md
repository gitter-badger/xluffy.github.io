---
layout: post
title: FastCGI sent in stderr "Primary script unknown"
author: xluffy
date:   2014-02-08
tags: nginx, system
description: FastCGI sent in stderr "Primary script unknown"
---

### Vấn đề:


I know this isn’t a unique question, but perhaps the manifesting situation is unique. Answers found elsewhere don’t match what I believe are my circumstances.

I have a box hosting a few sites and, although there are no functional issues as far as I can tell, I see this message sprinkled throughout my Nginx error logs for almost every site. Everything I read points to a problem with my FastCGI params, specifically SCRIPT_FILENAME, but my value seems to be inline with what’s recommended:

`fastcgi_param SCRIPT_FILENAME   $request_filename;`

Am I reading the wrong recommendations? I’ve also noticed that in some cases, but not all, the host value in the log is one whose A record points to the box, but isn’t one that Nginx is listening for.

Any idea what might be going on? Is this error “expected” in the case of a 404, maybe?

I answered:

Nope, that’s completely out of line. The SCRIPT_FILENAME refers to the path of the file on the filesystem, not the path in the request URI.

Typically it should be something like:

`fastcgi_param SCRIPT_FILENAME   $document_root$fastcgi_script_name;`

or

`fastcgi_param SCRIPT_FILENAME /usr/share/nginx/html$fastcgi_script_name;`

