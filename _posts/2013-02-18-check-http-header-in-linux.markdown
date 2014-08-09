---
layout: post
title:  "Check HTTP Header in Linux"
date:   2013-02-18 23:03:52
categories: General
---

We can use the command CURL to check the http header in Linux:
{% highlight text %}
curl -I http://www.ws.org/2006/03/addressing/ws-addr.xsd
HTTP/1.1 200 OK
Date: Mon, 18 Feb 2013 20:28:42 GMT
Server: Apache/1.3.27 (Unix)
Connection: close
Content-Type: text/html
{% endhighlight %}