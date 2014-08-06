---
layout: post
title:  "Set Properties Based on OS in Ant"
date:   2013-01-10 23:03:52
categories: Ant
---

Here is an example of how to set property based on OS in Ant:
{% highlight xml %}
<!--
Do not set the property up front like:
<property name="configFilePath" value="/etc/sysconfig/myconfig">
As long as the property has been defined, the following condition 
code won't have any effect.
-->
<condition property="configFilePath" value="/etc/sysconfig/myconfig">
  <os name="Linux"/>
</condition>
<condition property="configFilePath" value="/etc/opt/myconfig">
  <os name="SunOS"/>
</condition>
{% endhighlight %}

[Ant Conditions Reference][ant-ref]

[ant-ref]: http://ant.apache.org/manual/Tasks/conditions.html