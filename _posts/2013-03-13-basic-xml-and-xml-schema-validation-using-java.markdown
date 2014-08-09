---
layout: post
title:  "Basic XML and XML Schema Validation Using Java"
date:   2013-03-13 23:03:52
categories: General
---

<strong>XML Schema</strong>
Let's start from a basic XML schema which defines one XML element named <strong>Dog</strong>. It is in a file named MySchema.xsd.
{% highlight xml %}
<?xml version="1.0"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
<xs:element name="Dog" type="xs:string"/> 
</xs:schema>
{% endhighlight %}

<strong>Sample XML File</strong>
We have a very simple XML file that contains the <strong>Dog</strong> information. Let's put this into a file named sample.xml.
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<Dog>MyDog</Dog>
{% endhighlight %}

<strong>XML validation using Java</strong>
Here is the sample of Java code for validating the XML file with the XML schema above.
{% highlight java %}
package com.zample.test;

import java.io.File;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

public class XmlValidation {
    static final String JAXP_SCHEMA_LANGUAGE = 
        "http://java.sun.com/xml/jaxp/properties/schemaLanguage";
    static final String W3C_XML_SCHEMA = 
        "http://www.w3.org/2001/XMLSchema";
    static final String schemaSource = 
        "src/com/zample/test/MySchema.xsd";
    static final String JAXP_SCHEMA_SOURCE = 
        "http://java.sun.com/xml/jaxp/properties/schemaSource";
    
    public static void main(String[] args)
    {
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        factory.setValidating(true);
        try
        {
            // Define the XML schema language.
            factory.setAttribute(JAXP_SCHEMA_LANGUAGE, W3C_XML_SCHEMA);

            // Define the XML schema file.
            factory.setAttribute(JAXP_SCHEMA_SOURCE, new File(schemaSource));

            // Parse the sample XML file.
            DocumentBuilder builder = factory.newDocumentBuilder();
            builder.parse(new File("src/com/zample/test/sample.xml"));
            System.out.println("Done");
            
        }
        catch(Exception e)
        {
            e.printStackTrace();
        }
    }
}
{% endhighlight %}