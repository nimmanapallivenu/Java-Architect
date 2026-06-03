# 3  Q13  12 Rules  for REST API design & development   Java Success.com

## Table of Contents

These are more like rules to develop RESTFul web services as opposed to being the best practices. REST is an architectural style without any contracts or
specifications. So, it is imperative to apply the following rules for better REST API design.
#1. W rite stateless RESTFul services
This means each request from a client to server must contain all of the information necessary to understand the request, and should not rely on any stored
session data on the server . Stateless web services are
1) Scalable as requests can go to any server . You don’ t need to have sticky sessions.
2) More reliable & simpler to use. When you try to handle states in your APIs, you not only create a session handling nightmare, but also create lots of repetitive
logic for each service.
#2. If stateless, how to pass login data fr om client to server using REST API?
1) Basic Authentication (i.e. credentials in HTTP headers) where the credentials are only Base64 encoded. So, use it in conjunction with SSL, which
encrypts the credentials. This approach has some potential risks as the “user name” and “password” are passed in from a client to the server for each request,
which leaves it open to “man in the middle SSL exploits”.
2) OAuth 2 makes use of an “access token”, which is an identifier to be used by the client to access protected resources.
Step 1: Call an endpoint to receive an access token.
Step 2: Access token is provided via the authorization header in each request.
#3. Use nouns and avoid verbs in the URIs
Avoid verbs like getUsers or updateUser in the URIs. Instead use nouns like “/accounts” or “accounts/123”. These are known as the resour ces. The HTTP
methods in the HTTP headers like “GET”, “POST”, “PUT”, and “DELETE” determine the CRUD (i.e. Create or Update use POST , Read use Get, Update use
PUT, Delete use DELETE) operation to be performed on the resources.
PUT must be idempotent . PUT must be used for updating all the fields, and if you want a partial update, use a POST . PUT can be used for “Create” when the
identifier is supplied by a client. If you want the server to create an id, then use a POST request, and the server needs to return a response status code of “ 201 –
Create” with a location header

URI s are:
Use a plural . As in accounts, transactions, employees, etc. Also needs to be coarse grained . Don’ t have fine grained nouns like “accountNumber” or
“accountName”.
method s are HTTP verbs: GET , HEAD, POST , PUT , DELETE, etc.
resour ces are: /accounts, /accounts/123, /accounts/123/transactions, /accounts/123/transactions/ 6789, etc.
[Learn Mor e: RESTFul W eb Service URI conventions with Spring MVC examples .]
#4. GET & HEAD methods must only fetch data, never alter the state
If you alter your resources with GET , you put your resources (i.e. Data) at risk. A “GET” is defined in HTTP protocol to be idempotent & safe. A GET request
can be cached in the browser , and refreshed multiple times. So, if you use GET to alter , you run the risk of unintentionally & repeatedly altering (i.e. inserting,
updating or deleting). If your GET is a link, it may get crawled repeatedly by a search engine, resulting in duplicate records in your database.
Don’t do :Location : accounts /123
https ://myserver .com:8080/accounting-services/1.0/accounts (i.e. all the accounts)
https ://myserver .com:8080/accounting-services/1.0/accounts/123 (1 account with id 123), etc.
GET : /accounts /123/activate

or with a query string as in
GET must only fetch the data.
#5. Cache your GET s for performance
Why unnecessarily slow the application down if the clients are repeatedly requesting the same data? Y ou can send conditional GET requests. The HTTP
Protocol defines a caching mechanism, in which the proxy web-servers can cache pages, files, images etc. The proxy web servers sit between the client and
the web servers.
When you cache your GET for performance, how do you
1) Prevent the data being stale?
Server -To-Client This can be accomplished by the web server sending the “ Last-Modified ” header with each GET call. This header indicates the date the
resource was last modified.
Client-T o-Proxy-Server The “Last-Modified” header is used in conjunction with the “ If-Modified-Since I retrieved the data” header , which is sent from the
client with the “Last-Modified” value to the server with each GET or HEAD so that the proxy web server can send “ 304 Not Modified ” if the cached date of the
resource matches the “Last-Modified” date. If the dates don’ t match, client can refresh its stale data by requesting the server for the up-to-date data.
2) Prevent concurrent updates that can corrupt the data.
For example user 1 & user 2 can concurrently update the same data.
Server -To-Client This can be accomplished by the web server sending the “ Last-Modified ” header with each GET call. This header indicates the date the
resource was last modified.GET : /accounts /123?activate

Client-T o-Server The “Last-Modified” header is used in conjunction with the “ If-Unmodified-Since I retrieved the data” header , which is sent from the
client with the “Last-Modified” value to the server with each PUT (i.e. update) so that the web server can either reject with “ 412 Pr econdition Failed ” or accept
and perform the update by comparing the dates.
Use ETags for better control
Use ETags and headers If-Match and If-None-Match for optimizing the caching of individual resources. ET ags are used for 1. caching and 2. conditional
requests . Using an If-xxxx header turns a standard GET request into a conditional GET .
The ET ag value is a hash computed out of response body . Spring MVC framework supports ET ag with Servelet filters.
#6. Wher e applicable, use HEAD over GET for better bandwidth
HEAD request for a resource say “/accounts/123” will only return the HTTP headers (i.e. meta-data) without the contents.
#7. V ersion your W eb Service API
Each URI should have a version number as in ” https://myserver .com:8080/accounting-services/v1.0/accounts, https://myserver .com:8080/v2.0/accounts, etc.
Alternatively , you can use version numbers via a custom media type in the “ Accept ” header , which is sent by a client telling the server what media type is
required.<filter >
 <filter -name >etagFilter </filter -name >
 <filter -class >org.springframework .web.filter .ShallowEtagHeaderFilter </filter -class >
</filter >
<filter -mapping >
 <filter -name >etagFilter </filter -name >
 <url-pattern >/api/*</url-pattern >
</filter -mapping >

Server sets the “ Content-T ype” header to say what media type it is sending.
Versioning is a necessary evil for REST API design, but don’ t unnecessarily increment the version numbers for things like:
1) Adding new endpoints.
2) Adding new fields to the response.
Versioning via the “Accept” header is favored as this prevents the URI from changing, but versioning via the URI is easier to implement.
#8. PUT methods must not have any side effects
An application could submit a PUT multiple times. So, if you are changing a product’ s price, send the updated price like $5.00. Don’ t send the deltas like
“+$0.50” or “-$0.25” as they can corrupt the data when invoked multiple times.
#9. Handle exceptions with HTTP r esponse codes
HTTP provides response codes to inform clients of the status of their request. Don’ t just send the success response code “200” with the error message. Use 200
– OK, 201 – Created, 400 – Bad Request, 401 – Unauthorized, 404 – Not Found, 500 – Internal Server Error ,412 – Precondition Failed, 304 – Not Modified,
etc.
The error response includes:Accept : application /json;version =1.0
Accept : application /json;version =2.0
{
"status" :401,
"code" : 40145 ,
"proparty" : "authorizationT oken" ,

#10.Consider the use of overloaded POST only to handle situations wher e PUT and DELETE ar e unavailable
This is a very very rare situation, but if you want to allow your API to support clients without PUT and DELETE, use overloaded POST . This can be done
via query parameters as in
#11. Use HA TEOAS for better navigation
“Hypermedia as the Engine of Application State” is a principle that states hypertext links should be used to create a better navigation through the API. A client
should not be smart enough to determine what operations are possible on an account. The server should return the allowed actions and the relevant URIs via the
“href“s.
The hypermedia message shown below tells the consumer (i.e. client) of the message that the possible actions are “deposits” & “withdrawals”, and the
corresponding links are “/accounts/123/deposits” and “/accounts/123/withdrawals”."message" : "Token " a5yte456ef " not authorized to access account 123" ,
"developerMessage" : "Calidate the access token....."
}
POST : /accounts /123?method =put
POST : /accounts /123?method =delete
{
"account" : {
 "accountName" : "John Samuel" ,
 "accountNumber" : "123" ,

#12. Pr ovide filtering, sorting, field selection and pagination for collections
What if you want to return an account with its recent 100 transactions in a single request as you don’ t want to make an extra round trip for the transactions? "balance" : "56070.30" ,
 "link" : [
 {
 "rel": "self" ,
 "href" : "/accounts/123" ,
 "method" : "GET"
 },
 {
 "rel": "deposit" ,
 "href" : "/accounts/123/deposits" ,
 "method" : "POST"
 },
 {
 "rel": "withdraw" ,
 "href" : "/accounts/123/withdrawals" ,
 "method" : "POST"
 }
 ]
}
GET : /accounts ?status =active
GET : /accounts ?sort=+status ,-createdDate
GET : /accounts ?fields =accountNumber ,status ,createdDate
GET : /accounts ?offset=20&limit =10

How do you represent a many to many relationship?
For example, a user can have many roles and a role can can have many users. Y ou can create a new noun called “userRoles” to represent a many to many
mapping.
then provide links to user & roles.GET : /accounts /123?expand =transactions &limit =100
GET : /userRoles /456
{
"userRoles" : {
 "link" : [
 {
 "rel": "self" ,
 "href" : "/userRoles/456" ,
 "method" : "GET"
 },
 {
 "rel": "user" ,
 "href" : "/users/789" ,

 "method" : "GET"
 },
 {
 "rel": "role" ,
 "href" : "/roles/866" ,
 "method" : "GET"
 }
 ]
}



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
