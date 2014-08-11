---
layout: post
title:  "XML Schema and XML II"
date:   2013-03-19 23:03:52
categories: General
---

Based on the content of my last post, we can move foward for having more requirements in the XML file. We want to have my finshes information in the XML file, so we can define the Cat tag in the XML schema. Additonally, we should define one more level for the Pets tag to hold those dogs and fishes.

{% highlight xml %}
<?xml version="1.0"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="Pets">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="Dogs">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="Dog" 
                          minOccurs="0" 
                          maxOccurs="unbounded"
                          type="DogType"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
    
        <xs:element name="Fishes">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="Fish" 
                          minOccurs="0" 
                          maxOccurs="unbounded"
                          type="FishType"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
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

  <xs:complexType name="FishType">
    <xs:sequence>
      <xs:element name="Name"  type="xs:string"/>
      <xs:element name="Age"   type="xs:string"/>
      <xs:element name="Breed" type="xs:string"/>
      <xs:element name="WaterDegree" type="xs:string" />
    </xs:sequence>
    <xs:attribute name="ID"    type="xs:string"/>
  </xs:complexType>
</xs:schema>
{% endhighlight %}

The XML file can looks like the following:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<Pets>
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

  <Fishes>
    <Fish ID="FISH001">
      <Name>MyFish</Name>
      <Age>1</Age>
      <Breed>Goldfish</Breed>
      <WaterDegree>15</WaterDegree>
    </Fish>
  </Fishes>
</Pets>
{% endhighlight %}

The schema looks nice, however, there are some duplicate information that are defined in both of the Dog and Fish. Name, Agent, Breed elements and the ID attribute are common for dog and fishes. Those elements can be abstracted further. We should define a baseType element. The Dog element and fish element should be extended from it. Use complexContent and extesion to achieve this.

{% highlight xml %}
<?xml version="1.0"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="Pets">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="Dogs">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="Dog" 
                          minOccurs="0" 
                          maxOccurs="unbounded"
                          type="DogType"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
    
        <xs:element name="Fishes">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="Fish" 
                          minOccurs="0" 
                          maxOccurs="unbounded"
                          type="FishType"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>

  <xs:complexType name="baseType">
    <xs:sequence>
      <xs:element name="Name"  type="xs:string"/>
      <xs:element name="Age"   type="xs:string"/>
      <xs:element name="Breed" type="xs:string"/>
    </xs:sequence>
    <xs:attribute name="ID"    type="xs:string"/>
  </xs:complexType>
  <xs:complexType name="DogType">
    <xs:complexContent>
      <xs:extension base="baseType">
        <xs:sequence>
          <xs:element name="MaxSpeed"  type="xs:string"/>
        </xs:sequence>
      </xs:extension>
    </xs:complexContent>
  </xs:complexType>

  <xs:complexType name="FishType">
    <xs:complexContent>
      <xs:extension base="baseType">
    <xs:sequence>
      <xs:element name="WaterDegree" type="xs:string" />
    </xs:sequence>
      </xs:extension>
    </xs:complexContent>
  </xs:complexType>
  
</xs:schema>
{% endhighlight %}

Here we go.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<Pets>
  <Dogs>
    <Dog ID="DOG001">
      <Name>MyDog</Name>
      <Age>4</Age>
      <Breed>Golden</Breed>
      <MaxSpeed>10</MaxSpeed>
    </Dog>
    <Dog ID="DOG002">
      <Name>HisDog</Name>
      <Age>5</Age>
      <Breed>Shapei</Breed>
      <MaxSpeed>5</MaxSpeed>
    </Dog>
  </Dogs>

  <Fishes>
    <Fish ID="FISH001">
      <Name>MyFish</Name>
      <Age>1</Age>
      <Breed>Goldfish</Breed>
      <WaterDegree>15</WaterDegree>
    </Fish>
  </Fishes>
</Pets>
{% endhighlight %}
