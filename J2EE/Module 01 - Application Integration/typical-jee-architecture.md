# Typical JEE Architecture

> **Module**: Application Integration  
> **Topic**: Typical JEE Architecture

---

## 📋 Table of Contents



This question is a very popular white board session question for both Java architects and experienced JEE Developers. Y ou need to draw on your experience to tackle this question as there
are no right or wrong answers. These high level diagrams and summary will help you refresh your memory .
Q. What should be a typical Java EE architecture for let’ s say, a medium-size web-based application?
A. Before start drawing the components on the whit board, you need to show that you gather requirements.
#1. Ask some questions like How many transactions per minute or hour should the system handle? How many concurrent users should it handle?, etc.
#2. Draw a big picture diagram. Mark the tiers, layers, key components, frameworks, etc. This is a 100 feet bird’ s eye view of the components.

JEE High Level Architecture
Be prepared for drill down questions like
1) Client pull Vs Server push. Client pull requires a special directive either in the HTML document header. This directive instructs the client to retrieve a specified document after a certain
amount of time. In other words, the client opens a new connection to the server. Server push involves sending packets of data to the client periodically. The HTTP connection between the

client and the server is kept open indefinitely. For example, you can use an asynchronous servlet.
2) When to use queue vs topic. Queue is for single receiver and a topic is for multiple subscribers.
3) When to use LDAP vs Database. LDAP stores data hierarchically and more suited for read intensive operations like looking up users, roles, etc for authorization, and database is more
suited for CRUD operations.
4) What is Rich Internet Application (RIA)? Modern web applications are rich single page applications making use of JavaScript code and frameworks like angularjs. A single rich page
makes ajax calls to back end systems to populate a particular section or tab of a rich page. This architecture is summarized below with an architecture diagram.
The modern web applications using AngulaJS or any other JavaScript based MVC frameworks may have two war files. One for the GUI and the second one for exposing the data via web
services in JSON format.

Separate war files for UI and data
When using multiple domains as shown above, you need to use CORS (Cr oss Origin Resour ce Sharing) to overcome the JavaScript restriction on cross domains. CORS allows you to
share GET, POST, PUT, and DELETE requests and CORS is supported by the modern browsers. The CORS make use of 2 requests.
Request 1 : “OPTIONS ” request as part of the handshake to determine if cross domain is allowed by the server .

Request 2 : GET, POST, PUT, or DELETE request that performs the actual operation on the server .
CORS
#3. Security considerations
Enterprise applications make use of SSO (Single-Sign-On) with enterprise level products like SiteMinder, Tivoli Access Manager, etc.
For example, SSO application like SiteMinder is configured to intercept the calls to authenticate the user. Once the user is authenticated, a HTTP header “SM_USER” is added with the
authenticated user name. For example “123”. The user header is passed to Spring 3 security. The “Security .jar” is a custom component that knows how to retrieve user roles for a given user

like 123 from a database or LDAP server. This custom component is responsible for creating a UserDetails Spring object that contains the roles as authorities. Once you have the authorities
or roles for a given user, you can restrict your application URLs and functions to provide proper access control.
Learn more about SSO

SSO with SiteMinder
#4. Transaction Management
Transaction management takes place at the service layer. You can use a Spring transaction manager with annotations such as @Transactional to demarcate transactional boundaries. The
service class can call multiple data access object (DAOs) within a transactional context.
Transaction Manager
#5. Quality of Service (QoS) considerations
Quality of service (QoS) requirements are technical specifications that specify the system quality of features such as availability, scalability, serviceability, etc. This is covered in detail
under QoS interview questions and answers .
Finally, the modern applications are highly distributed making use of various architecture and integration styles described in:
Java integration styles and Java architectures. A typical enterprise application will make use of a combination of these integration styles and architecture. For example, here is a very
simplified trading application making use of “ synchr onous ” and “ asynchr onous ” calls. This allow traders to place buy/sell trades online.

FIX to send trades to stock exchange
as you can see, it makes use of
1) Message Oriented Middle wares (MOM) like W ebMethods or T ibco to publish or subscribe messages (i.e. trades).
2) Web services to perform CRUD (Create Read Update and Delete) operations.
3) MVC architecture to display data on the GUI to provide a user interface.
4) JDBC to persist/read user interactions via CRUD operations.
5) FIX protocol to communicate to the stock exchange. It is a standard to exchange financial information.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03