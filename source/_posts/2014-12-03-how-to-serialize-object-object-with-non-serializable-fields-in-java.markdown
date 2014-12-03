---
layout: post
title: "How to serialize object object with non-serializable fields in java?"
date: 2014-12-03 12:59:10 +0100
comments: true
categories: [java, serialization]
categories: 
---

There might be a situation when you wan’t to serialize your custom object, but it contains fields of types that are not serializable. The one pupular example would be a joda DateTime. Here is the code:

{% codeblock lang:java %}
public class MyCustomClass implements Serializable{
     
    //serializable fields
    private String name;
    private int count;
     
    //non-serializable fields
    private DateTime date;
}
{% endcodeblock %}

Serializing that would give you an exception. But there is still hope. Java allows us to make a custom serialization.

First we need to make non-serializable fields transient, for further convinience:

{% codeblock lang:java %}
//non-serializable fields
    private transient DateTime date;
{% endcodeblock %}

Then just write those methods into your class:

{% codeblock lang:java %}
private void readObject(ObjectInputStream input) throws IOException, ClassNotFoundException {
    // deserialize the non-transient data members first;
    input.defaultReadObject();
    // Read the date
    setDate((DateTime) input.readObject());
}
 
private void writeObject(ObjectOutputStream output) throws IOException, ClassNotFoundException {
    // serialize the non-transient data members first;
    output.defaultWriteObject();
    // Write the date
    output.writeObject(date);
}
{% endcodeblock %}

That’s it. Final code of your class would be now this:

{% codeblock lang:java %}
public class MyCustomClass implements Serializable {
 
    //serializable fields
    private String name;
    private int count;
 
    //non-serializable fields
    private transient DateTime date;
 
    private void readObject(ObjectInputStream input) throws IOException, ClassNotFoundException {
        // deserialize the non-transient data members first;
        input.defaultReadObject();
        // Read the color
        setDate((DateTime) input.readObject());
    }
 
    private void writeObject(ObjectOutputStream output) throws IOException, ClassNotFoundException {
        // serialize the non-transient data members first;
        output.defaultWriteObject();
        // Write the color
        output.writeObject(date);
    }
 
}
{% endcodeblock %}