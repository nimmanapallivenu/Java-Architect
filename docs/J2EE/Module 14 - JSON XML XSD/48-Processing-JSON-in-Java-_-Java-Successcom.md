# 48 Processing JSON in Java   Java Success.com

## Table of Contents

Step 1: Create a new simple-json project using Maven CLI.
press “Enter” for all the prompts. This creates a new “simple-json” folder with MVN artefacts like pom.xml, etc.
Step 2: Import the maven project into your IDE. E.g. in eclipse File –> Import –> Existing Maven Projects and browse the “simple-json” folder with the
pom.xml you created in Step 1. Press finish.
Step 3: Make changes to the pom.xml to include JSON librariesmvn archetype :generate -DgroupId =com.mytutorial -DartifactId =simple -json
<project xmlns ="http://maven.apache.or g/POM/4.0.0" xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://maven.apache.or g/POM/4.0.0 http://maven.apache.or g/xsd/maven-4.0.0.xsd" >
 <modelV ersion >4.0.0 </modelV ersion >
 <groupId >com.mytutorial </groupId >
 <artifactId >simple -json</artifactId >
 <version >1.0-SNAPSHOT </version >
 <packaging >jar</packaging >
 <name >simple -json</name >
 <url>http://maven.apache.or g</url>
 <properties >
 <project .build .sourceEncoding >UTF -8</project .build .sourceEncoding >
 <jackson .version >2.4.4 </jackson .version >
 </properties >
 <dependencies >

Step 4: Write the domain Java object with the annotations to be able to Marshall & Unmarshall. Aslo, not the serialization classes to covert LocalDate object to
String and vice-versa. <dependency >
 <groupId >junit</groupId >
 <artifactId >junit</artifactId >
 <version >4.11</version >
 <scope >test</scope >
 </dependency >
 <!-- JSON library -->
 <dependency >
 <groupId > com.fasterxml .jackson .core</groupId >
 <artifactId >jackson -databind </artifactId >
 <version >${jackson .version }</version >
 </dependency >
 <dependency >
 <groupId > com.fasterxml .jackson .core</groupId >
 <artifactId >jackson -core</artifactId >
 <version >${jackson .version }</version >
 </dependency >
 <dependency >
 <groupId > com.fasterxml .jackson .core</groupId >
 <artifactId >jackson -annotations </artifactId >
 <version >${jackson .version }</version >
 </dependency >
 </dependencies >
</project >
package com.mytutorial .model ;
mport java.io.IOException ;
mport java.io.Serializable ;
mport java.time.LocalDate ;
mport java.time.format .DateT imeFormatter ;
mport java.util.Objects ;

mport com.fasterxml .jackson .annotation .JsonInclude ;
mport com.fasterxml .jackson .core.JsonGenerationException ;
mport com.fasterxml .jackson .core.JsonGenerator ;
mport com.fasterxml .jackson .core.JsonParser ;
mport com.fasterxml .jackson .core.JsonProcessingException ;
mport com.fasterxml .jackson .databind .DeserializationContext ;
mport com.fasterxml .jackson .databind .JsonDeserializer ;
mport com.fasterxml .jackson .databind .JsonSerializer ;
mport com.fasterxml .jackson .databind .ObjectMapper ;
mport com.fasterxml .jackson .databind .SerializerProvider ;
mport com.fasterxml .jackson .databind .annotation .JsonDeserialize ;
mport com.fasterxml .jackson .databind .annotation .JsonSerialize ;
/Java 8 code
public class ClientEvent implements Serializable {
 private static final long serialV ersionUID = 1L;
 public static final String DATE_FORMA T = "dd/MM/yyyy" ;
 private String clientCode ;
 @JsonDeserialize (using = CustomLocalDateDeserializer .class )
 @JsonSerialize (using = CustomLocalDateSerializer .class )
 private LocalDate eventDate ;
 public ClientEvent () {
 super ();
 }
 public String getClientCode () {
 return clientCode ;
 }
 public void setClientCode (String clientCode ) {
 this.clientCode = clientCode ;
 }
 public LocalDate getEventDate () {
 return eventDate ;
 }
 public void setEventDate (LocalDate eventDate ) {
 this.eventDate = eventDate ;

 }
 @Override
 public boolean equals (Object obj) {
 if (obj == null) {
 return false ;
 }
 if (getClass () != obj.getClass ()) {
 return false ;
 }
 final ClientEvent other = (ClientEvent ) obj;
 return Objects .equals (this.clientCode , other .clientCode )
 && Objects.equals(this.eventDate, other .eventDate);
 }
 @Override
 public int hashCode () {
 return Objects .hash(this.clientCode , this.eventDate );
 }
 @Override
 public String toString () {
 return Objects .toString (this);
 }
 public String toJson () {
 ObjectMapper mapper = new ObjectMapper ();
 mapper .setSerializationInclusion (JsonInclude .Include .NON_NULL );
 try {
 String json = mapper .writeV alueAsString (this);
 return json;
 } catch (JsonProcessingException e) {
 e.printStackT race();
 throw new RuntimeException (e);
 }
 }
 public static ClientEvent fromJson (String json) {
 ObjectMapper mapper = new ObjectMapper ();
 mapper .setSerializationInclusion (JsonInclude .Include .NON_NULL );
 try {
 ClientEvent clientEvent = mapper .readV alue(json, ClientEvent .class );

 return clientEvent ;
 } catch (IOException e) {
 e.printStackT race();
 throw new RuntimeException (e);
 }
 }
**
 Custom LocalDate deserializer from JSON using the clientEvent.DA TE_FORMA T
 format.
/
lass CustomLocalDateDeserializer extends JsonDeserializer <LocalDate > {
 public CustomLocalDateDeserializer () {
 }
 @Override
 public LocalDate deserialize (JsonParser jp, DeserializationContext ctxt)
 throws IOException , JsonProcessingException {
 String stringV alue = jp.getValueAsString ();
 LocalDate dateT ime = LocalDate .parse (stringV alue,
 DateT imeFormatter .ofPattern (ClientEvent .DATE_FORMA T));
 return dateT ime;
 }
**
 Custom LocalDate serializer to JSON using the ClientEvent.DA TE_FORMA T format.
/
lass CustomLocalDateSerializer extends JsonSerializer <LocalDate > {
 public CustomLocalDateSerializer () {
 }
 @Override
 public void serialize (LocalDate value , JsonGenerator jgen,
 SerializerProvider provider ) throws IOException ,
 JsonGenerationException {
 DateT imeFormatter formatter = DateT imeFormatter
 .ofPattern (ClientEvent .DATE_FORMA T);
 jgen.writeString (formatter .format (value ));
 }

Step 5: Finally , the JUnit test class to Marshall & Unmarshall.
package com.mytutorial .model ;
mport java.io.IOException ;
mport java.io.StringW riter;
mport java.time.LocalDate ;
mport java.time.format .DateT imeFormatter ;
mport org.junit.Assert ;
mport org.junit.Test;
mport com.fasterxml .jackson .core.JsonGenerationException ;
mport com.fasterxml .jackson .databind .JsonMappingException ;
mport com.fasterxml .jackson .databind .ObjectMapper ;
public class ClientEventT est {
 
 private static final String JSON_TEXT = "{\"clientCode\":\"CLIENT -123\",\"eventDate\":\"20/10/2015\"}" ;
 
 @Test
 public void testMarshall () throws JsonGenerationException , JsonMappingException , IOException {
 ClientEvent ce = new ClientEvent ();
 ce.setClientCode ("CLIENT -123" );
 ce.setEventDate (LocalDate .now());
 ObjectMapper objMapper = new ObjectMapper ();
 StringW riter sw = new StringW riter();
 objMapper .writeV alue(sw, ce);
 
 Assert .assertNotNull (sw.toString ());
 Assert .assertEquals (JSON_TEXT , sw.toString ());
 }
 
 @Test

Step 6: Run “ClientEventT est” as a Junit test.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » public void testUnMarshall () throws JsonGenerationException , JsonMappingException , IOException {
 
 ClientEvent ce = ClientEvent .fromJson (JSON_TEXT );
 
 Assert .assertNotNull (ce);
 Assert .assertEquals ("CLIENT -123" , ce.getClientCode ());
 DateT imeFormatter df = DateT imeFormatter .ofPattern (ClientEvent .DATE_FORMA T);
 Assert .assertEquals ("20/10/2015" , df.format (ce.getEventDate ()));
 }



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
