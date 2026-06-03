# XSD Schema

> **Module**: JSON XML XSD  
> **Topic**: XSD Schema

---

## 📋 Table of Contents



- [Q1: How do you restrict an element in XSD?](#q1)
- [Q2: Can you explain as per the XSD shown above the term “tar getNamespace”?](#q2)
- [Q3: What is the difference between complexT ype and simpleT ype attribute in XSD?](#q3)
- [Q4: What are the dif ferent data types in XSD? How do you define it?](#q4)
- [Q5: Can you import XSDs?](#q5)
- [Q6: What is the difference betwen “xsd:sequence” and “xsd:all”?](#q6)
- [Q7: How do you provide documentation for your XSDs?](#q7)
- [Q8: How do you make an element in XML schema optional?](#q8)
- [Q9: How do you make an element in XML schema compulsary?](#q9)
- [Q10: What is the difference between the terms “Optional” and “Not Required” with res](#q10)
- [Q11: How will you represent the secnario in XSD where you can have elements “Pending”](#q11)

---

## Q1: How do you restrict an element in XSD?

**Answer:**

With “xs:restriction”. In the XSD shown below, the “AddressT ype” is restricted to “Mailing”, “Of fice”, and “Home”.
<?xml version ="1.0" encoding ="UTF-8" ?>
<xs:schema xmlns ="http://www .w3.or g/2001/XMLSchema"
 targetNamespace ="http://www .mycompany .com"
 xmlns :xs="http://www .w3.or g/2001/XMLSchema"
 xmlns :tns="http://www .mycompany .com"
 elementFormDefault ="qualified" attributeFormDefault ="unqualified" >
 <xs:element name ="Party" minOccurs ="0" maxOccurs ="unbounded" >
 <xs:complexT ype>
 <xs:sequence >
 <xs:element name ="id" type="xs:string" ></xs:element >
 <xs:element name ="PartyName" type="xs:string" />
 <xs:element name ="PartyT ype" type="xs:string" />
 <xs:element name ="AddressDetails" minOccurs ="0" maxOccurs ="unbounded" >
 <xs:complexT ype>
 <xs:sequence >
 <xs:element name ="AddressT ype">
 <xs:simpleT ype>
 <xs:restriction base="xs:string" >
 <xs:enumeration value ="Mailing" />
 <xs:enumeration value ="Office" />
 <xs:enumeration value ="Home" />
 </xs:restriction >
 </xs:simpleT ype>
 </xs:element >
 <xs:element name ="AddressLine1" type="xs:string" ></xs:element >
 <xs:element name ="AddressLine2" type="xs:string" ></xs:element >
 <xs:element name ="Suburb" type="xs:string" ></xs:element >
 <xs:element name ="State" type="xs:string" ></xs:element >
 <xs:element name ="Postcode" type="xs:string" ></xs:element >
 </xs:sequence >
 </xs:complexT ype>
 </xs:element > 
 </xs:sequence >

---

## Q2: Can you explain as per the XSD shown above the term “tar getNamespace”?

**Answer:**

Placing the tar getNamespace attribute at the top of your XSD schema means that all entities defined in it are part of this namespace. In the above example,
The “Ref_Adviser” is in the tar getNamespace “http://www .mycompany .com”
and then “xmlns:tns” prefix in the declaration uses the same tar getNamespace “http://www .mycompany .com”. The element “Adviser” is of custom defined type
“Ref_Adviser” in the “tns” prefix namesapce. The other elements are in the “xs” prefix, which is in the “http://www .w3.or g/2001/XMLSchema” namespace. </xs:complexT ype>
 
 <xs:element name ="Adviser" type="tns:Ref_Adviser" />
 
 <xs:complexT ype name ="Ref_Adviser" >
 <xs:sequence >
 <xs:element name ="firstName" type="xs:string" />
 <xs:element name ="surname" type="xs:string" />
 </xs:sequence >
 </xs:complexT ype>

</xs:schema >
<xs:complexT ype name ="Ref_Adviser" >
 <xs:sequence >
 <xs:element name ="firstName" type="xs:string" />
 <xs:element name ="surname" type="xs:string" />
 </xs:sequence >
</xs:complexT ype>
<xs:element name ="Adviser" type="tns:Ref_Adviser" />

---

## Q3: What is the difference between complexT ype and simpleT ype attribute in XSD?

**Answer:**

A simple element is an XML element that can contain only text. In the above example the “AddressT ype” is defined as a “simpleT ype” with enumerated
values.
A complex element is an XML element that contains other elements and/or attributes. “Party” is defined as a “complexT ype”.

---

## Q4: What are the dif ferent data types in XSD? How do you define it?

**Answer:**

There are dif ferent datatypes like int, string, date, decimal, etc.
You can have custom datatype as in the Java classes. For example, “ExpiryDate” is a custom type in the format “MMYY”
and use it as
Note : Namespace prefix is “tns”, which is the “tar getNamespace.”<xs:element name ="id" type="xs:string" >
<xs:simpleT ype name ="ExpiryDate" >
<xs:restriction base="xs:string" >
 <xs: pattern value ="(0[1-9]|1[0-2])[0-9][0-9]" />
</xs:restriction >
</xs:simpleT ype>
<xs:element name ="validT illDate" type="tns:ExpiryDate" />

---

## Q5: Can you import XSDs?

**Answer:**

Yes.

---

## Q6: What is the difference betwen “xsd:sequence” and “xsd:all”?

**Answer:**

“<xsd:all>” indicates that the child elements can appear in any order .
“<xsd:sequence>” indicates that the child elements can only appear in the order mentioned.

---

## Q7: How do you provide documentation for your XSDs?

**Answer:**

with the “annotation” and “documentation” tags.

---

## Q8: How do you make an element in XML schema optional?

**Answer:**

With the “minOccurs” attribute.<xsd:import schemaLocation ="../common/MyAppCommon.xsd"
 namespace ="http://www .mycompany .com/myapp/common/dataT ypes" />
<xsd:complexT ype name ="CaseDetailsDto" >
 <xsd:annotation >
 <xsd:documentation >
 This object represents details pertaining to the creation of a new case in the system
 </xsd:documentation >
 </xsd:annotation >
 ......
</xsd:complexT ype>

---

## Q9: How do you make an element in XML schema compulsary?

**Answer:**

With the “minOccurs” attribute set to 1.

---

## Q10: What is the difference between the terms “Optional” and “Not Required” with respect to XSDs?

**Answer:**

Optional: means the element does NOT need to be present in the XML.
Not Requir ed: means the element can be present, but it does NOT need to have a “value”. Y ou define this with the “nillable” attribute.

---

## Q11: How will you represent the secnario in XSD where you can have elements “Pending” followed by “Authorised” or “Pending” followed by “Cancelled”?

**Answer:**

You can use the “choice” element.<xs:element name ="description" type="xs:string" minOccurs ="0" maxOccurs ="1" />
<xsd:complexT ype name ="DocumentT ypeDto" >
 <xsd:sequence >
 <xsd:element name ="Code" type="xsd:string" minOccurs ="1" />
 <xsd:element name ="Name" type="xsd:string" minOccurs ="1" />
 </xsd:sequence >
</xsd:complexT ype>
<xsd:element name ="Date" type="xsd:date" nillable ="true" minOccurs ="0"/>

Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »<<xsd:complexT ype name ="OrderProcessing" >
 <xsd:choice >
 <xsd:sequence >
 <xsd:element name ="Pending" type="…" />
 <xsd:element name ="Authorise" type="…" />
 </xsd:sequence >
 <xsd:sequence >
 <xsd:element name ="Pending" type="…" />
 <xsd:element name ="Cancelled" type="…" />
 </xsd:sequence >
 </xsd:choice >
</xsd:complexT ype>

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03