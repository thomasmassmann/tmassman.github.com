---
layout: post
title: "Plone: Move instance to another host without downtime"
description: "Sometimes it might be necessary to move a Plone instance to
              another host. This article describes how do this without almost
              any downtime."
category:
tags: [Nginx, Plone]
---
{% include JB/setup %}

At [Inqbus](http://inqbus.de) we're hosting a lot of Plone sites. Therefor we
use virtual machines. When sites within an instance grow, it might be necessary
to add or extend some components, like adding more ZEO clients or caching
(using Varnish or Squid). But what if the virtual machines don't have enough
power to handle this? Then those Plone instances have to be migrated to
another, more powerful machine.

The problem then is to do this without any downtime. Of course you first have
to create a copy of the existing Plone instance on the new machine, create a
snapshot backup of all the related data, and restore this data on the new
machine. But what about the connected domains? The IP addresses differ (if you
don't use one webserver that does all the domain handling). It can take up to
several hours to update all the domains on the root DNS servers. But you want
your sites online, with up to date data.

## Nginx configuration: proxy_pass

We're using Nginx as webserver for several reasons (of course Apache does the
job as well). So the following examples are taken from our Nginx config.

Imagine you have the following config on your actual server:

    server {
      listen ip-of-actual-server:80;
      server_name your-domain.com;
      location / {
        proxy_pass http://127.0.0.1:8080/VirtualHostBase/http/your-domain.com:80/your-site/VirtualHostRoot/;
      }
    }

On your new server, add the same config. The only thing that changes is the IP
address:

    server {
      listen ip-of-new-server:80;
      server_name your-domain.com;
      location / {
        proxy_pass http://127.0.0.1:8080/VirtualHostBase/http/your-domain.com:80/your-site/VirtualHostRoot/;
      }
    }

Now, on your old server, adjust the config for your domain. Use the proxy_pass
directive to point to your new server. The important part is the
proxy_set_header directive.

    server {
      listen ip-of-actual-server:80;
      server_name your-domain.com;
      location / {
        proxy_pass          http://your-new-server;
        proxy_redirect      off;
        proxy_set_header    Host $host;
      }
    }

That's it. Now you can move your instance, restart Nginx and update your DNS
settings. It's save to remove the config on the old server after about one
week. Then all the root DNS servers should be updated and have the new IP
address for your domain.
