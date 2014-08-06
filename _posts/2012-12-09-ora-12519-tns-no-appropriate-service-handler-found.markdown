---
layout: post
title:  "ORA-12519 TNS no Appropriate Service Handler Found"
date:   2012-12-09 23:03:52
categories: Database
---

When working with Oracle XE 10g, the following error shows up:
ORA-12519 TNS no appropriate service handler found

The issue that I am facing is that too many database connections are opened simultaneously. We could use database connection pool to solve the issue or simply modify the database system configuration.

To fix the issue, we need to increase the number of processes:
{% highlight sql %}
ALTER SYSTEM SET PROCESSES=500 SCOPE=SPFILE;
{% endhighlight %}

Afterward, restart the database. Database might not be able to restart with a large number of processes.