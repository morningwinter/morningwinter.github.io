---
layout: post
title:  "Simple Scala Samples for Anonymous Function"
date:   2012-12-26 23:03:52
categories: Scala
---

Recently I played scala with anonymous function. Here are the samples. They might look silly, but work for me. I just want to show some concept in Scala, not how to write an elegant code.

{% highlight scala %}
object HelloWorld
{
  def main(args: Array[String])
  {
    println("Hello World")

    // Use parenthese.
    HelloWorld.doJob(() => println("Do Job..."))

    // Use curly brace.
    HelloWorld.doJob{() => println("Do Job...{}")}

    /*
    * HelloWorld.scala:10: error: type mismatch;
    * found : Unit
    * required: () => Unit
    * HelloWorld.doJob{println("Do Job... Simple Form")}
    *
    * */

    // Pass in an anonymous function with parameter.
    HelloWorld.doJob((param) => param * 10 )

    // An anonymous function with curly brace.
    HelloWorld.doJob((param) =>
    {
      var _tmp : Int = param * 10
      _tmp = _tmp + 1
      _tmp
    }
    )

    // Define an anonymous function.
    var f : (Int) => Int = (param) => { param + 10 }
    HelloWorld.doJob(f)

    // Define an anonymous function using place holder.
    var g : (Int) => Int = { _ + 11 }
    HelloWorld.doJob(g)

    // Pass in an anonymous function with a parameter t,
    // which is another anonymous function.
    HelloWorld.doFunc( t => t() )

  }

  // Accept an anonymous function.
  def doJob(func: () => Unit) =
  {
    func()
  }

  // Accept an anonymous function with a parameter.
  def doJob(func: (Int) => Int) =
  {
    println()
    println("=== Start ===")
    var output : Int = func(10)
    output = output * 10
    println("OUTPUT: " + output)
    println("=== End ===")
  }

  // Accept an anonymous function with a parameter,
  // which is another anonymous function.
  def doFunc(func: ( () => Unit ) => Unit) =
  {
    var f : () => Unit = () => println("Func f")
    func(f)
  }
}
{% endhighlight %}

Out put is:

{% highlight text %}
Hello World
Do Job...
Do Job...{}

=== Start ===
OUTPUT: 1000
=== End ===

=== Start ===
OUTPUT: 1010
=== End ===

=== Start ===
OUTPUT: 200
=== End ===

=== Start ===
OUTPUT: 210
=== End ===
Func f
{% endhighlight %}