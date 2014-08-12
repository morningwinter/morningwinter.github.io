---
layout: post
title:  "Install Java in Fedora"
date:   2013-03-28 23:03:52
categories: General
---

<strong>1. Download Java RPM</strong><br>
For example: jdk-7u17-linux-x64.rpm

<strong>2. Install Java</strong>
{% highlight bash %}
# yum install jdk-7u17-linux-x64.rpm
{% endhighlight %}

<strong>3. Ignore the following error messages:</strong>
{% highlight bash %}
Error: Could not open input file: /usr/java/jdk1.7.0_17/jre/lib/rt.pack
jsse.jar...
Error: Could not open input file: /usr/java/jdk1.7.0_17/jre/lib/jsse.pack
charsets.jar...
Error: Could not open input file: /usr/java/jdk1.7.0_17/jre/lib/charsets.pack
tools.jar...
Error: Could not open input file: /usr/java/jdk1.7.0_17/lib/tools.pack
localedata.jar...
Error: Could not open input file: /usr/java/jdk1.7.0_17/jre/lib/ext/localedata.pack
{% endhighlight %}

<strong>4. Setup Java</strong>
{% highlight bash %}
# alternatives --install /usr/bin/java java /usr/java/jdk1.7.0_17/bin/java 2
# alternatives --config java
{% endhighlight %}

<strong>Reference:</strong><br>
https://forums.oracle.com/forums/thread.jspa?messageID=10751653<br>
http://www.if-not-true-then-false.com/2010/install-sun-oracle-java-jdk-jre-7-on-fedora-centos-red-hat-rhel/