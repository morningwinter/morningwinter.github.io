---
layout: post
title:  "Scala Syntax Highlighting in Emacs"
date:   2012-12-19 23:03:52
categories: Scala
---

1. Download the tool from Scala Tool Support

2. Unzip the zip file: scala-dist-master.zip, move it to somewhere that you want to.

3. In your home directory, modify the .emacs file, create one if it doesn’t exist. The file should look like the following:

{% highlight lisp %}
(add-to-list 'load-path "path to the scala-mode ")
(require 'scala-mode-auto)
{% endhighlight %}