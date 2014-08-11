---
layout: post
title:  "XML Schema and XML I"
date:   2013-03-15 23:03:52
categories: General
---

<strong>Sub Elements</strong>

Now we would like to develop the XML schema a little bit more. We would like to have sub elements under the Dog element to have more information about the dog. The dog elements is a complex type, which contains a sequence of XML elements.

{% highlight xml %}
<?xml version="1.0"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="Dog">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="Name"  type="xs:string"/>
        <xs:element name="Age"   type="xs:string"/>
        <xs:element name="Breed" type="xs:string"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
{% endhighlight %}

The XML file can be:

{% highlight xml %}
<Dog>
  <Name>MyDog</Name>
  <Age>4</Age>
  <Breed>Golden</Breed>
</Dog>
{% endhighlight %}

<strong>Attribute</strong>
So far so good. We can have attribute for the XML element. Let's define the ID attribute the Dog element. Attribute should be always defined at the end of the elements.
{% highlight xml %}
<?xml version="1.0"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="Dog">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="Name"  type="xs:string"/>
        <xs:element name="Age"   type="xs:string"/>
        <xs:element name="Breed" type="xs:string"/>
      </xs:sequence>
      <xs:attribute name="ID"    type="xs:string"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
{% endhighlight %}

XML file looks like:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<Dog ID="DOG001">
  <Name>MyDog</Name>
  <Age>4</Age>
  <Breed>Golden</Breed>
</Dog>
{% endhighlight %}

<strong>Collections</strong>
OK. We need more dogs, so let's add a Dogs element to collect more dogs.

{% highlight xml %}
<?xml version="1.0"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="Dogs">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="Dog" minOccurs="0" 
                    maxOccurs="unbounded">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="Name"  type="xs:string"/>
              <xs:element name="Age"   type="xs:string"/>
              <xs:element name="Breed" type="xs:string"/>
            </xs:sequence>
            <xs:attribute name="ID"    type="xs:string"/>
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
{% endhighlight %}

We need an XML complex type to enclose Dog element. Sets attribute minOccurs to 0, indicates the Dog element is optional. Sets attribute maxOccurs to unbounded, indicates the Dog element can be repeated. The default value of both minOccurs and maxOccurs is 1. The following is the valid XML file.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<Dogs>
  <Dog ID="DOG001">
    <Name>MyDog</Name>
    <Age>4</Age>
    <Breed>Golden</Breed>
  </Dog>
  <Dog ID="DOG002">
    <Name>HisDog</Name>
    <Age>5</Age>
    <Breed>Shapei</Breed>
  </Dog>
</Dogs>
{% endhighlight %}

<strong>Customized Type</strong>
We can clean up the schema a little bit by defining a customized dog type. Sets the type of the Dog element to DogType. Moves the definitions of the DogType out of the Dogs element. This way, it makes the schema more readable and maintainable.

{% highlight xml %}
<?xml version="1.0"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="Dogs">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="Dog" minOccurs="0" 
                    maxOccurs="unbounded"
                    type="DogType"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>

  <xs:complexType name="DogType">
    <xs:sequence>
      <xs:element name="Name"  type="xs:string"/>
      <xs:element name="Age"   type="xs:string"/>
      <xs:element name="Breed" type="xs:string"/>
    </xs:sequence>
    <xs:attribute name="ID"    type="xs:string"/>
  </xs:complexType>
</xs:schema>
{% endhighlight %}