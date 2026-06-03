# Processing XML in Java

> **Module**: JSON XML XSD  
> **Topic**: Processing XML in Java

---

## 📋 Table of Contents



- [Q1: What libraries can you use in Java to process XML documents?](#q1)
- [Q2: What is a StAX Parser and when will you use it?](#q2)
- [Q3: Why use StAX when there is DOM?](#q3)
- [Q4: What is the major advantage of StAX over SAX?](#q4)
- [Q5: What does JAXB stand for? What is an XML Binding?](#q5)
- [Q6: Why use JAXB?](#q6)
- [Q7: Why would be the motivating factor to use MOXy implementation of JAXB as opposed](#q7)

---

## Q1: What libraries can you use in Java to process XML documents?

**Answer:**

#1. DOM Parser loads the whole XML structure into memory, and you can read and write. This is useful for smaller XML documents which don’ t impact
memory usage. JDOM is another alternative to DOM4J.
#2. SAX Parser to only read an XML document. Good to parse large XML documents. SAX ‘pushes’ XML events, leaving it up to you to determine where the
XML events belong in your code logic.
#3. StAx Reader/W riter works with a datastr eam oriented interface. The program asks for the next element when it’ s ready just like a cursor/iterator. StAX
‘pulls’ XML events, leaving it up to you to determine where in your program / data to receive the XML events. In general, StAX is as ef fecient as SAX.
#4. JAXB for object to XML and XML to Object mapping. V ery widely used in W eb services. The POJOs are annotated to map XML or JSON to object and
vice versa. MOXy implementation of JAXB is powerful as it supports XPath for mapping, and can be used for both XML and JSON.
Java 6 onwards, there is built in support for all the dif ferent types of parses listed above. In general, will be favoring StAX, and move to SAX if performance is
a real concern. When using OXM (Object T o Xml Mapping) then JAXB is used.

---

## Q2: What is a StAX Parser and when will you use it?

**Answer:**

The StAX Java API for XML processing is designed for parsing XML streams, just like the SAX API’ s, but
– StAX is a “ pull” API whereas SAX is a “push” API.
– StAX can do both XML r eading and writing whereas SAX can only do XML reading.

---

## Q3: Why use StAX when there is DOM?

**Answer:**

The DOM parsing involves cr eating in-memory objects representing an entire document tree for an XML document. Once in memory, DOM trees can be
navigated and parsed arbitrarily, providing the maximum flexibility for developers. However this flexibility comes at a cost of large memory footprint and
significant processor requirements, as the entire representation of the document must be held in memory as objects for the duration of the document processing.
This may not be an issue when working with small documents, but memory and processor requirements can escalate quickly for larger size documents.
StAX (Str eaming Api for XML) involves str eaming where streaming refers to a programming model where XML data sets are transmitted and parsed serially
at application runtime as events like StartElementEvent, CharacterEvent, EndElementEvent, etc, often in real time and dynamically where contents are not
precisely known beforehand. These stream-based parsers can start generating output immediately, and data set elements can be discarded and garbage collected
immediately after they are used. This ensures smaller memory footprint, reduced processor requirements, and higher performance in certain scenarios. This
memory and processing ef ficiency comes at the cost where streaming can only see the data set state at one location at a time in the document.
Here is how the events are sequentially processed:

StAX Parser

---

## Q4: What is the major advantage of StAX over SAX?

**Answer:**

The major advantage of StAX over SAX is that the pull model allows sub parsing of the XML input. Y ou can extract out the element name, then the
attributes, and then the characters (i.e. content).

---

## Q5: What does JAXB stand for? What is an XML Binding?

**Answer:**

JAXB means Java API for Xml Binding. XML binding maps an XML to in-memory Java objects. The principle advantage of using JAXB when
marshalling and demarshalling XML is that is simplifies the programming model by allowing us to simply annotate a few POJOs and use the JAXB API’ s and
you can serialise to XML and deserialise from XML very easily. This makes it much simpler than alternatives such as DOM and SAX.

---

## Q6: Why use JAXB?

**Answer:**

JAXB is a reference interface for which you can have a number of dif ferent implementations like default reference implementation shipped JDK6,
JaxMeAPI, MOXy, Metro, etc. JAXB is available in JDK6 onwards, so it doesn’ t require any external library and it doesn’ t require a XML schema to work.
XML schema is optional. Easy to use as you can use annotations on your model classes. It is supported by RESful frameworks like Jersey, Spring MVC,
RESTEasy, etc.

---

## Q7: Why would be the motivating factor to use MOXy implementation of JAXB as opposed to the default implementation provided by the JDK6
implementation?

**Answer:**

MOXy supports @XmlPath annotation and other extensions will make your implementation cleaner .
1. @XmlPath extension which is inspired from XPath
2. The Jersey JAX-RS reference implementation provides JSON binding via MOXy .
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03