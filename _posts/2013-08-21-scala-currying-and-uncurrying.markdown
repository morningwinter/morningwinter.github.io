---
layout: post
title:  "Scala Currying and Uncurrying"
date:   2013-08-21 23:03:52
categories: General
---

This post is about the currying and uncurrying in Scala. The example shows how to use the currying function and how to convert between currying function and uncurrying function.

{% highlight scala %}
object Test
{
  def main(args: Array[String])
  {
    // Basic Usage

    // Error
    // println(currying("a", "b"))
    println(currying("a")("b"))

    println(uncurrying("x", "y"))

    // Error
    //println(uncurrying("a")("b"))

    // Conversion

    // From currying to uncurrying
    val toUncurrying = Function.uncurried(currying _)
    println(toUncurrying("a", "b"))

    // From uncurrying to currying
    // Depreciated
    val toCurrying1 = Function.curried(uncurrying _)
    println(toCurrying1("x")("y"))

    // Use this instead of the depreciated Function.curried()
    val toCurrying2 = (uncurrying _).curried
    println(toCurrying2("x")("y"))

    // Partial Currying
    val animals = List("cat", "fish", "dog")
    val hiAnimals = decorateAnimal(animals, currying("hi"))
    println(hiAnimals)

    // partial currying assignment, need a place holder.
    val partialCurrying = currying("hello")_
    val hoAnimals = decorateAnimal(animals, partialCurrying)
    println(hoAnimals)
  }

  // Currying function
  def currying(s1:String)(s2:String) = s1 + " " + s2

  // Uncurrying function
  def uncurrying(s1:String, s2:String) = s1 + " " + s2

  def decorateAnimal(animals:List[String], f: String => String) : List[String] =
  {
    var ret = List[String]()
    for ( animal <- animals )
    {
      ret = f(animal) :: ret
    }
    return ret
  }
}
{% endhighlight %}

output:

{% highlight text %}
a b
x y
a b
x y
x y
List(hi dog, hi fish, hi cat)
List(hello dog, hello fish, hello cat)
{% endhighlight %}