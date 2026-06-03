# 116 16 JMS interview questions & answers

## Table of Contents

- [Q1: What types of messaging paradigms are provided by JMS?](#q1)
- [Q2: How do you determine whether it would be better to use a T opic or Queue?](#q2)
- [Q3: What is Message Oriented Middleware (MOM)?](#q3)
- [Q4: Why use a messaging system as opposed to using Data T ransfer Objects (aka DT Os](#q4)
- [Q5: Why use JMS with MOM?](#q5)
- [Q6: What are the components of the JMS architecture?](#q6)
- [Q7: What are the components of the JMS 2.0 architecture introduced in JEE 7?](#q7)
- [Q8: What are some of the key message characteristics defined in a message header?](#q8)
- [Q9: What are the dif ferent body types (aka payload types) supported for messages?](#q9)
- [Q10: What is a message broker?](#q10)
- [Q11: Discuss some of the design decisions you need to make regarding your message del](#q11)
- [Q12: How does XML over HTTP compare with XML using JMS?](#q12)
- [Q13: Why favor using XML with JMS?](#q13)
- [Q14: Why do you need AMQP when there is JMS?](#q14)
- [Q15: How does AMQP dif fer from JMS?](#q15)
- [Q16: Can you explain the following messaging terms?
1) Poison messages.
2) Queue dept](#q16)

---

## Q1: What types of messaging paradigms are provided by JMS?

**Answer:**

Point-to-Point : provides a traditional queue based mechanism where the client application sends a message through a queue to typically one receiving
client that receives messages sequentially . A JMS message queue is an administered object that represents the message destination for the sender and the
message source for the receiver . A Point-to-Point application has the following characteristics:
Queue based

– A Point-to-Point producer is a sender (i.e. QueueSender).
– A Point-to-Point consumer is a receiver (i.e. QueueReceiver).
– A Point-to-Point destination is a queue (i.e. Queue).
– A message can only be consumed by one r eceiver .
Example : A call center application may use a queue based Point-to-Point domain to process all the calls where all the phone calls do not go to all the operators,
but only one.
Publish/Subscribe : is a one-to-many publishing model where client applications publish messages to topics, which are in turn subscribed by other interested
clients. All subscribed clients will receive each message. A Publish/Subscribe application has the following characteristics:

Topic based
– A Publish/Subscribe producer is a publisher (i.e. T opicPublisher).
– A Publish/Subscribe consumer is a subscriber (i.e. T opicSubscriber).
– A Publish/Subscribe destination is a topic (i.e. T opic).
– A message can be consumed by multiple subscribers .

If a message publisher is also a subscriber , then a publisher can receive its own message sent to the destination. This behavior is only applicable to
publish/subscribe model. This behavior can be controlled by setting the “noLocal” attribute to true when creating the publisher or the subscriber .
Example: A bulletin board application may use a topic based publish/subscribe model where everyone who is interested in particular news becomes a
subscriber and when a message is published, it is sent to all its subscribers.

---

## Q2: How do you determine whether it would be better to use a T opic or Queue?

**Answer:**

You must choose to use a T opic if one of the following conditions applies:
1) Same message must be replicated to multiple consumers (W ith Queue a message can only be consumed by one receiver).
2) A message should be dropped if there are no active consumers that would select it.
3) There are many subscribers each with a unique selector .

---

## Q3: What is Message Oriented Middleware (MOM)?

**Answer:**

Message Oriented Middleware (MOM) is generally defined as a software infrastructure that asynchronously communicates with other disparate systems
(e.g. Mainframe system, C++ System, etc) through the production and consumption of messages. A message may be a request, a report, or an event sent from
one part of an enterprise application to another .

---

## Q4: Why use a messaging system as opposed to using Data T ransfer Objects (aka DT Os, V alue Objects) or W eb services with JSON on XML data?

**Answer:**

Firstly , messaging enables loosely coupled distributed communication. A component sends a message to a destination, and the recipient can retrieve the
message from the destination. However , the sender and the receiver do not have to be available at the same time in order to communicate and also they are not
aware of each other . In fact, the sender does not need to know anything about the receiver; nor does the receiver need to know anything about the sender . The
sender and the receiver need to know only what message format and what destination to use. In this respect, messaging dif fers from tightly coupled
technologies, such as Remote Method Invocation (RMI) and W eb services.

---

## Q5: Why use JMS with MOM?

**Answer:**

Message Oriented Middleware (MOM) systems like W ebsphereMQ, SonicMQ, W ebMethods, etc are proprietary systems. Java Message Service (JMS) is a
Java API that allows applications to create, send, receive, and read messages in a standard way . The JMS API defines a common set of interfaces and associated
semantics that allow programs written in the Java programming language to communicate with other messaging implementations (e.g. SonicMQ, TIBCO etc).
The JMS API minimizes the set of concepts a programmer must learn to use messaging products but provides enough features to support sophisticated
messaging applications. It also strives to maximize the portability of JMS applications across JMS providers. Similar to JDBC, a good example of the adapter
design pattern in action.
Many companies have spent decades developing their legacy systems. So, XML can be used in a non-proprietary way to move data from legacy systems to
distributed systems like JEE over the wire using MOM (i.e. Implementation) and JMS (i.e. Interface).

---

## Q6: What are the components of the JMS architecture?

**Answer:**

JMS 2.0 (year 2013) released in Java EE 7 makes the JMS architecture much easier and simpler to work with. The following components are based on JMS
1.1 (i.e. from year 2002).
Message pr oducers : A component that is responsible for creating a message. E.g. QueueSender , and T opicPublisher . An application can have several message
producers. Each producer might be responsible for creating dif ferent types of messages and sending them to dif ferent destinations (i.e. T opic or Queue). A
message producer will send messages to a destination regardless of whether or not a consumer is there to consume it.
Message consumers: A component which resides on the receiving end of a messaging application. Its responsibility is to listen for messages on a destination
(i.e. T opic or Queue) . E.g. QueueReceiver , TopicSubscriber , MessageDrivenBean (MDB). A MDB is simply a JMS message consumer . A client cannot access a
MDB directly as you would do with Session or Entity beans. Y ou can only interface with a MDB by sending a JMS message to a destination (i.e. T opic or
Queue) on which the MDB is listening.
Message destinations: A component which a client uses to specify the tar get of messages it sends/receives. E.g. T opic (publish/Subscribe domain) and Queue
(Point-to-Point domain). Message destinations typically live on a MOM, which is remote to the clients. Message destinations are administered objects that need
to be configured.
JMS messages: A message is a component that contains the information (aka payload) that must be communicated to another application or component. E.g.
TextMessage (XML file), ObjectMessage (serialized object) etc.
JMS Administered objects: JMS administered objects are objects containing configuration information that are set up during application deployment or
configuration and later used by JMS clients. They make it practical to administer the JMS API in the enterprise. These administered objects are initialized when
the application server starts. When a producer or a consumer needs to get a connection to receive or send a JMS message, then you need to locate the configured
administered objects QueueConnectionFactory or T opicConnectionFactory . Message destinations are administered objects that need to be configured as well.
These administered objects hide provider -specific details from JMS clients.
JNDI naming service: For a producer and consumer to be able to use the administered objects to send and receive messages, they must know how to locate
things such as the destination and connection factories.
Example: To publish a message to a topic: (Note: exception handling etc are omitted for brevity)
String factoryJndiName = "WSMQT opicConnectionFactory" ;
String destinationJndiName = "wsmq/topic/ProductManagerT opic" ;
/JNDI lookup of administered ConnectionFactory object

To consume a messageContext iniCtx = new InitialContext ();
TopicConnectionFactory topicCF = (TopicConnectionFactory ) iniCtx .lookup (factoryJndiName );
/JNDI lookup of administered destination (i.e. T opic)
Topic topicDestination = (Topic) iniCtx .lookup (destinationJndiName );
/get a connection from the T opicConnectionFactory
TopicConnection publishConnection = topicCF .createT opicConnection ();
/get a session from the connection. Session should be accessed by only one thread.
TopicSession publishSession = 
 publishConnection .createT opicSession (false ,TopicSession .AUT O_ACKNOWLEDGE );
/create a publisher from the session
TopicPublisher publisher = publishSession .createPublisher (reqDestination );
/create a JMS message to send
TextMessage message = publishSession .createT extMessage ();
message .setText("JMS test message" );
/send the message
publisher .publish (message , DeliveryMode .NON_PERSISTENT , 4, 0);
String factoryJndiName = "WSMQT opicConnectionFactory" ;
String destinationJndiName = "wsmq/topic/ProductManagerT opic" ;
/JNDI lookup of administered ConnectionFactory object
Context iniCtx = new InitialContext ();
TopicConnectionFactory topicCF = (TopicConnectionFactory ) iniCtx .lookup (factoryJndiName );
/JNDI lookup of administered destination (i.e. T opic)
Topic topicDestination = (Topic) iniCtx .lookup (destinationJndiName );
/get a connection from the T opicConnectionFactory
TopicConnection subscribeConnection = topicCF .createT opicConnection ();

If you use JEE container with a Message Driven Bean (MDB) or Spring Container with a message listener , the container will provide the infrastructure for
receiving messages, and invokes the onMessage(Message message) to process the message.

---

## Q7: What are the components of the JMS 2.0 architecture introduced in JEE 7?

**Answer:**

JMS 2.0 (year 2013) released in Java EE 7 makes the JMS architecture much easier and simpler to work with. The new simplified API require fewer
objects like JMSContext , JMSProducer , and JMSConsumer . In JEE, JMSContext can be injected and managed by the container ./get a session from the connection
TopicSession subscribeSession =
 subscribeConnection .createT opicSession (false ,TopicSession .AUT O_ACKNOWLEDGE );
/create a subscriber from the session
TopicSubscriber subscriber = subscribeSession .createsubscriber (reqDestination );
/look for messages every 1 second
while (true) {
 Message response = subscriber .receive ();
 if (response != null && response instanceof T extMessage) {
 System.out.println (((T extMessage) response).getT ext());
 }
 Thread .sleep (1000 );
public void onMessage (Message message ) {
 String text = null;
 if (message instanceof TextMessage ) {
 text = ((TextMessage )message ).getText();
 }
 log.info(text);
}

The JMS1.1 annotation driven way
Drawbacks with JMS 1.1 code:
1) You have to create many intermediary objects like session, messageProducer , and textMessage.
2) Redundant and misleading ar guments are used like Session.AUT O_ACKNOWLEDGE
3) Connections must be explicitly closed in the finally block.
4) Exceptions thrown are checked exceptions and need to be handled.
JMS 2.0 is still backward compatible , but has new methods to make things simpler and easier . JMS objects implement java.jang.AutoCloseable, hence
requires Java 7.0. It has objects such as Connection , Session , MessagePr oducer , MessageConsumer , and QueueBr owser .@Resource (lookup = "java:global/jms/myAppConnectionFactory" )
ConnectionFactory connectionFactory ;
 
@Resource (lookup = "java:global/jms/myAppQueue" )
Queue myAppQueue ;
 
public void sendMessage (String payload ) {
 try {
 Connection connection = connectionFactory .createConnection ();
 try {
 Session session = connection .createSession (false ,Session .AUT O_ACKNOWLEDGE );
 MessageProducer messageProducer = session .createProducer (myAppQueue );
 TextMessage textMessage = session .createT extMessage (payload );
 messageProducer .send(textMessage );
 } finally {
 connection .close ();
 }
 } catch (JMSException ex) {
 ex.printStackT race();
 }

The pluses for JMS 2.0:
1) The JMSContext combines Connection and Session.
2) Payload can be sent directly without wrapping it in a T extMessage.
3) Runtime exception is thrown and connections are closed automatically with the Java 7.0 AutoCloseable interface.

---

## Q8: What are some of the key message characteristics defined in a message header?

**Answer:**

JMS Header

---

## Q9: What are the dif ferent body types (aka payload types) supported for messages?

**Answer:**

All JMS messages are read-only once posted to a queue or a topic.@@Resource (lookup = "java:global/jms/myAppConnectionFactory" )
ConnectionFactory connectionFactory ;
 
@Resource (lookup = "java:global/jms/myAppQueue" )
Queue myAppQueue ;
public void sendMessageNew (String payload ) {
 try (JMSContext context = connectionFactory .createContext ();){
 context .createProducer ().send(myAppQueue , payload );
 } catch (JMSRuntimeException ex) {
 Logger .getLogger (getClass ().getName ()).log(Level .SEVERE , null, ex);
 }

Text message : body consists of java.lang.String (e.g. XML).
Map message : body consists of key-value pairs.
Stream message : body consists of streams of Java primitive values, which are accessed sequentially .
Object message : body consists of a Serializable Java object.
Byte message : body consists of arbitrary stream of bytes.

---

## Q10: What is a message broker?

**Answer:**

A message broker acts as a server in a MOM. A message broker performs the following operations on a message it receives:
Processes message header information.
Performs security checks and encryption/decryption of a received message.
Handles errors and exceptions.
Routes message header and the payload (aka message body).
Invokes a method with the payload contained in the incoming message (e.g. calling onMessage(..) method on a Message Driven Bean (MDB)).
Transforms the message to some other format. For example XML payload can be converted to other formats like HTML etc with XSL T.

---

## Q11: Discuss some of the design decisions you need to make regarding your message delivery and transaction management?

**Answer:**

Acknowledgement mode and transaction modes are used to determine if a message will be lost or re-delivered on failure during message processing by
the tar get application. Acknowledgement modes are set when creating a JMS session.
In the above code sample, the transaction mode is set to false and acknowledgement mode is set to auto mode. Let us look at acknowledgement modes:
AUT O_ACKNOWLEDGE: The messages sent or received from the session are automatically acknowledged. This mode also guarantees once only delivery . If
a failure occurs while executing onMessage() method of the destination MDB, then the message is re-delivered. A message is automatically acknowledged
when it successfully returns from the onMessage(…) method.
DUPS_OK_ACKNOWLEDGE: This is just like AUT O_ACKNOWLEDGE mode, but under rare circumstances like during failure recovery messages might
be delivered more than once. If a failure occurs then the message is re-delivered. This mode has fewer overheads than AUT O_ACKNOWLEDGE mode.nitialContext ic = new InitialContext (…);
QueueConnectionFactory qcf =
 (QueueConnectionFactory )ic.lookup (“AccountConnectionFactory ”);
QueueConnection qc = qcf.createQueueConnection ();
QueueSession session = qc.createQueueSession (false , Session .AUT O_ACKNOWLEDGE );

CLIENT_ACKNOWLEDGE: The messages sent or received from sessions are not automatically acknowledged. The destination application must
acknowledge the message receipt. This mode gives an application full control over message acknowledgement at the cost of increased complexity . This can be
acknowledged by invoking the acknowledge() method on javax.jms.Message class.
Transactional behavior is controlled at the session level. When a session is transacted, the message oriented middleware (MOM) stages the message until the
client either commits or rolls back the transaction. The completion of a session’ s current transaction automatically begins a new transaction.
The use of transactions in messaging af fects both the producers and consumers of the messages as shown below:
JMS T ransaction
Producers [As per the above diagram]:
1’s Commit: The MOM send the group of messages that have been staged.
2’s Rollback: The MOM disposes of the group of messages that have been staged.
Consumers [As per the above diagram] :
3’s Commit: The MOM disposes of the group of messages that have been staged.
4’s Rollback: The MOM resends the group of messages that have been staged.
In JMS, a transaction organizes a message or a gr oup of messages into an atomic pr ocessing unit . So, if a message delivery is failed, then the failed
message may be re-delivered. Calling the commit() method commits all the messages the session receives and calling the rollback method rejects all the
messages.

In the above code sample, the transaction mode is set to true and acknowledgement mode is set to -1, which means acknowledgement mode has no use in this
mode. Let us look at transaction modes:
Message Driven Bean (MDB) with container managed transaction demar cation : A MDB participates in a container transaction by specifying the
transaction attributes in its deployment descriptor . A transaction automatically starts when the JMS provider removes the message from the destination and
delivers it to the MDB’ s onMessage(…) method. T ransaction is committed on successful completion of the onMessage() method. A MDB can notify the
container that a transaction should be rolled back by setting the MessageDrivenContext to setRollBackOnly(). When a transaction is rolled back, the message is
re-delivered.
Message Driven Bean (MDB) with bean managed transaction demar cation : If a MDB chooses not to participate in a container managed transaction then the
MDB programmer has to design and code programmatic transactions. This is achieved by creating a UserT ransaction object from the MDB’ s
MessageDrivenContext as shown below and then invoking the commit() and rollback() methods on this UserT ransaction object.nitialContext ic = new InitialContext (…);
QueueConnectionFactory qcf =
 (QueueConnectionFactory )ic.lookup (“AccountConnectionFactory ”);
QueueConnection qc = qcf.createQueueConnection ();
QueueSession session = qc.createQueueSession (true, -1);
public void onMessage (Message aMessage ) {
…
if(someCondtionIsT rue) {
 mdbContext .setRollbackOnly ();
}
else{
 //everything is good. T ransaction will be committed automatically on 
 //completion of onMessage(..) method.
} 

Transacted session: An application completely controls the message delivery by either committing or rolling back the session. An application indicates
successful message processing by invoking Session class’ s commit() method. Also it can reject a message by invoking Session class’ s rollback() method. This
committing or rollback is applicable to all the messages received by the session.
Q. What happens to rolled-back messages?
A. Rolled back messages are re-delivered based on the re-delivery count parameter set on the JMS provider . The re-delivery count parameter is very important
because some messages can never be successful and this can eventually crash the system. When a message reaches its re-delivery count, the JMS provider can
either log the message or forward the message to an error destination. Usually it is not advisable to retry delivering the message soon after it has been rolled-public void onMessage (Message aMessage ) {
 UserT ransaction uT = mdbContext .getUserT ransaction ();
 uT.begin ();
 ….
 if(someCondtionIsT rue) {
 uT.rollback ();
 }
 else{
 uT.commit ();
 } 
public void process (Message aMessage , QueueSession qs) {
 ….
 if(someCondtionIsT rue) {
 qs.rollback ();
 }
 else{
 qs.commit ();
 }
 … 

back because the tar get application might still not be ready . So we can specify a time to re-deliver parameter to delay the re-delivery process by certain amount
of time. This time delay allows the JMS provider and the tar get application to recover to a stable operational condition.
Care should be taken not to make use of a single transaction when using the JMS request/response paradigm where a JMS message is sent, followed by the
synchronous receipt of a reply to that message. This is because a JMS message is not delivered to its destination until the transaction commits, and the receipt of
the reply will never take place within the same transaction.

---

## Q12: How does XML over HTTP compare with XML using JMS?

**Answer:**

XML over HTTP: Simpler to implement, widely compatible and has less performance overhead but HTTP does not provide reliability in terms of guaranteed
delivery because there is no message persistence, no inherent reporting facility for failed message delivery and no guaranteed once only delivery . The
application programmer must build these services into the application logic to provide reliability & persistence, which is not an easy task.
XML over JMS is reliable, scalable, loosely couples systems and robust. The main disadvantage of this approach is that the JMS providers (i.e. Message
Oriented Middleware) use a proprietary pr otocol between producer and consumer . So, to communicate, you and your partners need to have the same MOM
software (E.g. W ebMethods). JMS allows you to toss one MOM software and plug-in another but you cannot mix providers without having to buy or build some
sort of a bridge.

---

## Q13: Why favor using XML with JMS?

**Answer:**

#1. Organizations can leverage years or even decades of investment in Business-to-Business (B2B) Electr onic Data Inter change (EDI) by using JMS with
XML. XML is an open standard and it r epresents the data in a non-pr oprietary way .
#2. Sending XML messages as text r educes coupling even more compared to sending serializable objects. XML also solves the data representation dif ferences
with XML based technologies such as XSL T. For example, the way “Enterprise X” defines a purchase order will be dif ferent from the way “Enterprise Y”
defines it. So the representation of XML message by “Enterprise X” can be transformed into the format understood by “Enterprise Y” using XSL T.
#3. Both enterprises may be using differ ent applications to run their business . For example Enterprise “X” may be using Java/JEE, while “Enterprise Y”
may be using SAP . XML can solve the data formatting problems since it is an open standard with a self describing data format, which allows the design of
business specific markup languages and standards like FIXML (Financial Information eXchange Markup Language), FpML (Financial products Markup
Language – derivative products), WML (W ireles Markup Language – for wireless devices ), SAML (Security Assertion Markup Language), etc. The structure
of an XML document is similar to that of business objects with various attributes. This allows for the natural conversion of application-specific objects to XML
documents and vice versa.
#4. XML digital signatur e technology can be used to pr ovide authentication, data integrity (tamper pr oofing) and non-r epudiation . Unlike SSL, XML
encryption can be used to encrypt and decrypt a section of a data. For example encrypt only the credit card information in a purchase order XML document.

You also need to consider sending messages across each or ganization’ s corporate firewall. Not every or ganization will open a port in the firewall other than the
well-known port 80 for HTTP traf fic. The solution is to make use of HTTP tunneling, which involves sending the data as HTTP traf fic through well-known port
number 80 for HTTP and then, once inside the firewall, convert this data into messages. For example JProxy is a JEE based HTTP tunnel with SSL and JAAS
with support for EJB, RMI, JNDI, JMS and CORBA.

---

## Q14: Why do you need AMQP when there is JMS?

**Answer:**

AMQP stands for Advanced Message Queuing Protocol, and was developed to address the problem of interoperability by creating a standard for how
messages should be structured and transmitted between platforms the same way as SMTP , HTTP , FTP , etc. have created interoperable systems. This standard
binary wire level protocol for messaging would therefore allow hetrogeneous disparate systems between and within companies to exchange messages regrdless
of the message broker vendor or platform.
RabbitMQ, Apache Qpid, StormMQ, etc are open source message broker softwares (i.e. MOM – message-oriented middlewares) that implements the Advanced
Message Queuing Protocol (AMQP).

---

## Q15: How does AMQP dif fer from JMS?

**Answer:**

JMS is a standard messaging API for the Java platform. It provides a level of abstraction that frees developers from having to worry about specific
implementation and wire protocols. This is similar to the JDBC API that allows you to easily switch databases. W ith JMS, you can switch from one JMS
complian message broker (e.g. W eb Methods) with another one (e.g. MQSeries or W ebspehreMQ) with little or no changes to your source code. It also provides
interoperability between other JVM based languages like Scala and Groovy . Altough JMS brokers can be used in .NET applications, the whole JMS
specification does not guarantee interoperability , and integration between Java to .NET or Java to Ruby , is proprietary and can be quite tricky . In scenarios
where you want to send a message from a Java based message producer to a .NET based message consumer , then you need a message based cross platform
interoperability that is what AMQP does. W ith AMQP , you can use any AMQP compliant client library , and AMQP compliant message broker .

---

## Q16: Can you explain the following messaging terms?
1) Poison messages.
2) Queue depth.
3) Message correlation id
4) Dead letter queue

**Answer:**

1. Poison messages , are messages the application can never successfully process due to being badly-formatted. Such a message might make the receiving
application fail and back out the receipt of the message. In this situation, such a message might get into an enless llop where get received, and then returned to
the queue, repeatedly . These messages are known as poison messages. The ConnectionConsumer must be able to detect poison messages and reroute them to an
alternative destination.
2. Queue Depth means the number of messages in a Queue. The JMS API does not provide an explicit method to pro-grammatically determine the queue depth.
You need to rely on vendor specific plugins to pro-grammatically determine this. Queue depth need to be monitored in production systems to raise alerts if they

get closer to their capacity . MOM provider specific tools can monitor queue depths.
3. There are dif ferent JMS messaging patterns like send only messages, receive only messages, request-response messages, etc. When you have JMS request-
response messages, you need to correlate a response message with a request message. Messaging is highly distributed, hence correlation ids are handy to trace
messages. So, MessageID and CorrelID fields are very handy for trace-ability .
4. Failed messages ar e recorded in a dead-letter queue . The failed delivery can be caused by reasons such as network issues, poison messages, queue not
found, queue is full, etc
Differ ences between JMS 1.1 and JMS 2.0
 
JMS 1.1 JMS 2.0
JMSProducer is lighter weight and supports method chaining.MessageProducer producer = session .createProducer ();
producer .send(destination ,message );JMSProducer producer = context .createProducer ();
producer .send(destination ,message );
MessageProducer producer = session .createProducer ();
producer .setDeliveryMode (DeliveryMode .NON_PERSISTENT );
producer .setPriority (1);
producer .setTimeT oLive (2000 );
producer .send(destination ,message );context .createProducer ()
 .setDeliveryMode (DeliveryMode .NON_PERSISTENT )
 .setPriority (1)
 .setTimeT oLive (1000 )
 .send(destination ,message );

Verbose with repeated “producer”. Less verbose with the “Builder design pattern”.
Extracting message body from the payloadExtracting message body from the payload
In JEE 7, The JMSContext objects can be injected into W eb or EJB containers with the @Inject annotation.
JMS 2.0 has made the message property “JMSXDeliveryCount” to mandatory from being optional in JMS 1.0 to handle poisonous messages.Message message = consumer .receive (2000 );
TextMessage textMessage = (TextMessage ) message ;
String body = textMessage .getText();Message message = consumer .receive (1000 );
String body = message .getBody (String .class );
@Inject
@JMSConnectionFactory ("jms/myAppConnectionFactory" )
@JMSSessionMode (JMSContext .AUT O_ACKNOWLEDGE )
private JMSContext context ;
@Resource (mappedName = "jms/myAppQueue" )
private Queue myAppQueue ;
public void sendMessage (String payload ) {
 context .createProducer ().send(myAppQueue , payload );

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
