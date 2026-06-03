# XML Fundamentals

> **Module**: JSON XML XSD  
> **Topic**: XML Fundamentals

---

## 📋 Table of Contents



- [Q1: What is an XML?](#q1)
- [Q2: Why is XML popular and important?](#q2)
- [Q3: When would you not use an XML?](#q3)
- [Q4: How would you decide to store data as elements or as attributes?](#q4)
- [Q5: What is XSD? How does it dif fer from DTD?](#q5)
- [Q6: What is an XSL?](#q6)
- [Q7: What is an XPath?](#q7)
- [Q8: What is version information in XML?](#q8)
- [Q9: What is a CDA TA section in an XML?](#q9)
- [Q10: How will you embed an XML content within an XML document?](#q10)
- [Q11: How do you write comments in an XML document?](#q11)
- [Q12: How do you write an attribute value with single quotes? How do you write an elem](#q12)
- [Q13: What is a well-formed XML document?](#q13)
- [Q14: What is a valid XML document?](#q14)
- [Q15: How will you write an empty element?](#q15)
- [Q16: What is a namespace in an XML document?](#q16)
- [Q17: Explain where your project needed XML documents?](#q17)

---

## Q1: What is an XML?

**Answer:**

XML stands for e Xtensible Markup Language. XML is a grammatical system for constructing custom markup languages for describing business data,
mathematical data, chemical data etc. XML loosely couples disparate applications or systems utilizing JMS, W eb services etc. XML uses the same building
blocks that HTML does: elements, attributes and values.

---

## Q2: Why is XML popular and important?

**Answer:**

XML is protocol and language neutral.
Scalable: Since XML is not in a binary format you can create and edit files with anything and it’ s also easy to debug. XML can be used to ef ficiently store small
amounts of data like configuration files (web.xml, application.xml, struts-config.xml, etc) to large company wide data with the help of XML stored in the
database.
Fast Access: XML documents benefit from their hierarchical structure. Hierarchical structures are generally faster to access because you can drill down to the
section you are interested in.
Easy to identify and use: XML not only displays the data but also tells you what kind of data you have. The mark up tags identifies and groups the
information so that dif ferent information can be identified by dif ferent application.
Stylability: XML is style-free and whenever dif ferent styles of output are required the same XML can be used with dif ferent style-sheets (XSL) to produce
output in XHTML, PDF, TEXT, another XML format, etc.
Linkability, in-line usability, universally accepted standard with free/inexpensive tools etc.

---

## Q3: When would you not use an XML?

**Answer:**

XML is verbose and it can be 4-6 times larger in size compared JSON or csv data. If your network lacked bandwidth and/or your content is too large and
network throughput is vital to the application then you may consider using JSON or csv data instead of an XML. If your end points support JSON, it might be a
better alternative if markup is not required.

---

## Q4: How would you decide to store data as elements or as attributes?

**Answer:**

A question arising in the mind of XML designers is whether to model and encode certain information using an element, or alternatively, using an attribute.
The answer to the above question is not clear -cut. But the general guideline is:
Using an element: : If you consider the information in question to be part of the essential material that is being expressed or communicated in the XML, put it in
an element.

Using an attribute: : If you consider the information to be peripheral or incidental to the main communication, or purely intended to help applications process
the main communication, use attributes.
The principle is data goes in elements and metadata goes in attributes. Elements are also useful when they contain special characters like “<”, “>”, etc which
are harder to use in attributes. The most important reason to use element is its extensibility. It is far easier to create child elements to reflect complex content
than to break an attribute into pieces. Y ou can use attributes along with elements to refine your understanding of that element with extra information. Attributes
are less verbose but using attributes instead of child elements with the view of optimizing document size is a short term strategy, which can have long term
consequences.

---

## Q5: What is XSD? How does it dif fer from DTD?

**Answer:**

XSD stands for Xml Schema Definition, which is a successor of DTD (Document T ype Definition). So XSD is a building block of an XML document. If
you have DTD then why use XSD you may ask?
XSD is more powerful and extensible than DTD. XSD has:
— Support for simple and complex data types.
— Uses XML syntax. So XSD are extensible just like XML because they are written in XML.
— Better data communication with the help of data types. For example a date like 03-04-2005 will be interpreted in some countries as 3rd of April 2005 and in
some other countries as 04th March 2005.<book ><title>Lord of the Rings </title</book >
<book title="Lord of the Rings" />
<?xml version ="1.0" ?>
<xs:schema xmlns :xs="http://www .w3.or g/2001/XMLSchema" targetNamespace ="http://www .w3schools.com" xmlns ="http://www .w3schools.com"
elementFormDefault ="qualified" >

---

## Q6: What is an XSL?

**Answer:**

XSL stands for e Xtensible Stylesheet Language. The XSL consists of 3 parts:
1) XSLT: Language for transforming XML documents from one to another .
2) XPath: Language for defining the parts of an XML document.
3) XSL-FO: Language for formatting XML documents. For example to convert an XML document to a PDF document etc.
XSL can be thought of as a set of languages that can :
— Define parts of an XML.
— Transform an XML document to XHTML (eXtensible Hyper T ext Markup Language) document.
— Convert an XML document to a PDF document.
— Filter and sort XML data.<xs:element name ="note" >
 <xs:complexT ype>
 <xs:sequence >
 <xs:element name ="to" type="xs:string" />
 <xs:element name ="from" type="xs:string" />
 <xs:element name ="title" type="xs:string" />
 <xs:element name ="content" type="xs:string" />
 </xs:sequence >
 </xs:complexT ype>
 <xs:attribute name ="language" type=”xs:string ” use=”Required ” />
</xs:element >
</xs:schema >
<?xml version ="1.0" ?>
<xsl:stylesheet xmlns :xsl="http://www .w3.or g/TR/WD-xsl" >
<xsl:template match ="/">
 <xsl:apply -templates select ="note " />
</xsl:template >
<xsl:template match ="note" >

---

## Q7: What is an XPath?

**Answer:**

Xml Path Language, a language for addressing parts of an XML document, designed to be used by both XSL T and XPointer. We can write both the
patterns (context-free) and expressions using the XPath Syntax. XPathis also used in XQuery .

---

## Q8: What is version information in XML?

**Answer:**

Version information in an XML is a processing instruction.
Tags that begin and end with “?” are called processing instructions. The processing instructions can also be used to call a style sheet for an XML as shown
below: <html>
 <head >
 <title><xsl:value -of select ="content/@language" >
 </title>
 </head >
 </html> 
</xsl:template >
</xsl:stylesheet >
<xsl:template match =”content [@language =’English ’]”>
………
<td><xsl:value -of select =”content /@language ” /></td>
<?xml version =”1.0” ?>

---

## Q9: What is a CDA TA section in an XML?

**Answer:**

If you want to write about elements and attributes in your XML document then you will have to prevent your parser from interpreting them and just display
them as a regular text. T o do this, you must enclose such information in a CDA TA section.

---

## Q10: How will you embed an XML content within an XML document?

**Answer:**

By using a CDA TA section.

---

## Q11: How do you write comments in an XML document?

**Answer:**

<?xml-stylesheet type=”text/css” href=”MyStyle .css” ?>
<![CDA TA[ <customername id=”123” > John </customername > ]]>
<message >
 <from >LoansSystem </from >
 <to>DocumentSystem </to>
 <body >
 <![CDA TA[ 
 <application >
 <number >456</number >
 <name >Peter </name >
 <detail >blah blah</detail >
 </application >
 ]]>
 </body >
</message >

---

## Q12: How do you write an attribute value with single quotes? How do you write an element value of “> 500.00”?

**Answer:**

You need to use an internal entity reference like < for <, > for >, & for &, " for “, ' for ‘.

---

## Q13: What is a well-formed XML document?

**Answer:**

A well formed document adheres to the following rules for writing an XML.
— A root element is required. A root element is an element, which completely contains all the other elements.
— Closing tags are required. abc or 
— Elements must be properly nested.
— XML is case sensitive. and elements are considered completely separate.
— An attribute’ s value must always be enclosed in either single or double quotes.
— Entity references must be declared in a DTD before being used except for the 5 built-in (<, > etc) discussed in the previous question.

---

## Q14: What is a valid XML document?

**Answer:**

For an XML document to be valid, it must conform to the rules of the corresponding DTD (Document T ype Definition – internal or external) or XSD
(XML Schema Definition).

---

## Q15: How will you write an empty element?

**Answer:**

<!-- This is an XML comment -->
<customer name =”"Mr. Smith" ” />
<cost> > 500.00 </cost>

or

---

## Q16: What is a namespace in an XML document?

**Answer:**

Namespaces are used in XML documents to distinguish one similarly titled element from another. A namespace must have an absolutely unique and
permanent name. In an XML, name space names are in the form of a URL. A default namespace for an element and all its children can be declared as follows:
Individual elements can be labeled as follows:<name age=”25”></name >
<name age=”25” />
<accounts xmlns =”http://www .bank1.com/ns/account”>
 …
</accounts >
<accounts xmlns =”http://www .bank1.com/ns/account” xmlns:bank2=”http://www .bank2.com/ns/account”>
 <name >FlexiDirect </name > <!-- uses the default name space -->
 <bank2 :name >Loan </bank2 :name > <!-- uses the bank2 namespace -->
 …
</accounts >

---

## Q17: Explain where your project needed XML documents?

**Answer:**

It is hard to find a project, which does not use XML documents.
— XML is used to communicate with disparate systems via messaging or W eb Services.
— XML based protocols and standards like SOAP, ebXML, WSDL etc are used in W eb Services.
— XML based deployment descriptors like web.xml, ejb-jar .xml, etc are used to configure the JEE containers.
— XML based configuration files are used by open-source frameworks like Hibernate, Spring, and Struts to name a few .
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03