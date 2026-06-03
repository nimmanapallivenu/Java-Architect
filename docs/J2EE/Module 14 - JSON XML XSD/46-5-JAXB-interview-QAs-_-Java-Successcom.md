# 46 5 JAXB interview Q&As   Java Success.com

## Table of Contents

- [Q1: What does JAXB stand for? What is an XML Binding?](#q1)
- [Q2: Why use JAXB?](#q2)
- [Q3: Can you create your Java objects from XSDs?](#q3)
- [Q4: Why would be the motivating factor to use MOXy implementation of JAXB as opposed](#q4)
- [Q5: Can you use JAXB to marshal and unmarshal JSON objects?](#q5)

---

## Q1: What does JAXB stand for? What is an XML Binding?

**Answer:**

JAXB means Java API for Xml Binding. XML binding maps an XML to in-memory Java objects. The principle advantage of using JAXB when
marshalling and demarshalling XML is that is simplifies the programming model by allowing us to simply annotate a few POJOs and use the JAXB API’ s and
you can serialise to XML and deserialise from XML very easily . This makes it much simpler than alternatives such as DOM and SAX.

---

## Q2: Why use JAXB?

**Answer:**

JAXB is a reference interface for which you can have a number of dif ferent implementations like default reference implementation shipped JDK6,
JaxMeAPI, MOXy , Metro, etc. JAXB is available in JDK6 onwards, so it doesn’ t require any external library and it doesn’ t require a XML schema to work.
XML schema is optional. Easy to use as you can use annotations on your model classes. It is supported by RESful frameworks like Jersey , Spring MVC,
RESTEasy , etc.
Here is an example of Unmarshalling XML to Java object:
Step 1: Sample XML to unmarshall
Step 2: POJO with JAXB annotations.<employee >
 <id>123</id>
 <name >Peter </name >
</employee >
mport javax .xml.bind.annotation .XmlRootElement ;
@XmlRootElement
public class Employee
{

Step 3: The JAXB unmrshaller utility class that can be used to private Integer id;
 private String name ;
 
 //JAXB requires a default constructor .
 private Employee (){}
 
 public Employee (Integer id, String name )
 {
 super ();
 this.id = id;
 this.name = name ;
 }
 
 public Integer getId ()
 {
 return id;
 }
 
 public void setId (Integer id)
 {
 this.id = id;
 }
 
 public String getName ()
 {
 return name ;
 }
 
 public void setName (String name )
 {
 this.name = name ;
 }
 
 @Override
 public String toString ()
 {
 return "Person [id=" + id + ", name=" + name + "]";
 } 

Step 4: The main class that unmarshalls XML string to Employee object.mport java.io.StringReader ;
mport javax .xml.bind.JAXBContext ;
mport javax .xml.bind.JAXBException ;
mport javax .xml.bind.Unmarshaller ;
mport javax .xml.transform .stream .StreamSource ;
public class JAXBUnMarshaller
{
 public Object unmarshalObject (Class <?> classT ype, String xmlString )
 {
 Object object = new Object ();
 try
 {
 StringReader stringReader = new StringReader (xmlString );
 StreamSource streamSource = new StreamSource (stringReader );
 
 JAXBContext jaxbContext = JAXBContext .newInstance (classT ype);
 
 Unmarshaller unMarshaller = jaxbContext .createUnmarshaller ();
 object = unMarshaller .unmarshal (streamSource );
 return object ;
 }
 catch (JAXBException e)
 {
 e.printStackT race();
 throw new RuntimeException ("Error unmarshalling class : " + classT ype + "\n" + e.getMessage ());
 }
 }
public class Test

Similar approach can be used for marshaling.

---

## Q3: Can you create your Java objects from XSDs?

**Answer:**

Yes, you can by binding a schema. Binding a schema means generating a set of Java classes that represents the schema. All JAXB implementations provide
a tool called a binding compiler to bind a schema (the way the binding compiler is invoked can be implementation-specific).
The -p option identifies a package for the generated classes, and the -d option identifies a tar get directory . So for this command, the classes are packaged in
myapp13.model within the work directory . In response, the binding compiler generates a set of interfaces and a set of classes that implement the interfaces. The
ObjectFactory .java is generated conatining methods for generating instances of the interfaces.

---

## Q4: Why would be the motivating factor to use MOXy implementation of JAXB as opposed to the default implementation provided by the JDK6
implementation?

**Answer:**

Using the @XmlPath annotation and other extensions provided by MOXy will make your implementation cleaner .
Step 1: You need to bring in the MOXy implementation via maven pom.xml file{
 
 private static final String XML_STRING = "<employee>\r\n" +
 " <id>123</id>\r\n" +
 " <name>Peter</name>\r\n" +
 "</employee>" ;
 
 public static void main (String [] args)
 {
 JAXBUnMarshaller unmashaller = new JAXBUnMarshaller ();
 Employee unmarshalObject = (Employee ) unmashaller .unmarshalObject (Employee .class , XML_STRING );
 System .out.println (unmarshalObject );
 }
xjc.sh -p myapp13 .model employee .xsd -d work

Step 2: Tell JAXB to use MOXy via jaxb.pr operties file and place it on where the POJOs like Employee is packaged.
Step 3: You will only need Employee.java and Department.java will no longer required. Thanks to the power of annotation @XmlPath(“faculty/name/text( )”).
Here is the revised Employee.java unmarshalling the same XML file with within employee.<dependency >
 <groupid >org.eclipse .persistence </groupId >
 <artifactid >eclipselink </artifactId >
 <version >2.3.2 </version >
</dependency >
avax .xml.bind.context .factory =org.eclipse .persistence .jaxb.JAXBContextFactory
mport javax .xml.bind.annotation .XmlAccessT ype;
mport javax .xml.bind.annotation .XmlAccessorT ype;
mport javax .xml.bind.annotation .XmlRootElement ;
mport org.eclipse .persistence .oxm.annotations .XmlPath ;
@XmlRootElement
@XmlAccessorT ype(XmlAccessT ype.FIELD )
public class Employee
{
 
 private Integer id;
 private String name ;
 
 @XmlPath ("faculty/name/text()" )
 private String departmentName ;

---

## Q5: Can you use JAXB to marshal and unmarshal JSON objects?

**Answer:**

Yes. If Y ou are using MOXy library , then you can specify a JSON mrshaling and unmarshling library like Jackson or Jettison . Here is an example with the
Jettison library .
Step 1: JSON string. XML equivalent JSON is more concise. 
 public Integer getId ()
 {
 return id;
 }
 
 public void setId (Integer id)
 {
 this.id = id;
 }
 
 public String getName ()
 {
 return name ;
 }
 
 public void setName (String name )
 {
 this.name = name ;
 }
 
protected String getDepartmentName () {
return departmentName ;
@Override
public String toString () {
return "Employee [id=" + id + ", name=" + name + ", departmentName=" + departmentName + "]";

Step 2: Jettison with JAXB library .
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »{"Employee" : {"id":"123" , "name" :"Peter" }
mport javax .xml.bind.*;
mport javax .xml.stream .XMLStreamReader ;
mport org.codehaus .jettison .json.JSONObject ;
mport org.codehaus .jettison .mapped .*;
public class UnmarshalDemo {
 public static void main (String [] args) throws Exception {
 JAXBContext jc = JAXBContext .newInstance (Employee .class );
 JSONObject obj = new JSONObject ("{\"Employee\":{\"id\":123,\"name\":\"Peter\"}" );
 Configuration config = new Configuration ();
 MappedNamespaceConvention con = new MappedNamespaceConvention (config );
 XMLStreamReader xmlStreamReader = new MappedXMLStreamReader (obj, con);
 Unmarshaller unmarshaller = jc.createUnmarshaller ();
 Employee employee = (Employee ) unmarshaller .unmarshal (xmlStreamReader );
 Marshaller marshaller = jc.createMarshaller ();
 marshaller .setProperty (Marshaller .JAXB_FORMA TTED_OUTPUT , true);
 marshaller .marshal (employee , System .out);
 }

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
