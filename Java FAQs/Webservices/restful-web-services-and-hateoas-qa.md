# 6  RESTful Web services and HATEOAS Q&A   Java Success.com

## Table of Contents

- [Q1: What is HA TEOAS? How does it provide state transition, scalability , and loose ](#q1)

---

## Q1: What is HA TEOAS? How does it provide state transition, scalability , and loose coupling?

**Answer:**

HATEOAS (Hypermedia as the Engine of Application State) is considered the final level of REST . This means that each link is presumed to implement the
standard REST verbs of GET , POST , PUT , and DELETE (or a subset).
REST Layers
Layers of REST & wher e does HA TEOAS sit?
Level 0: Pr otocol – Transporting protocol (normally HTTP , but it doesn’ t have to be).
Level 1: Resour ces – This level uses multiple URIs, where every URI is the entry point to a specific resource.
Level 2: HTTP verbs – This level indicates that your API should use the protocol properties in order to deal with scalability and failures. Don’ t use a single
POST method for all, but make use of GET when you are requesting resources, and use the DELETE method when you want to delete a resources.
Level 3: Hypermedia contr ols – uses HA TEOAS to deal with discovering the possibilities of your API towards the clients. The point of hypermedia controls is
that they tell us what we can do next, and the URI of the resource we need to manipulate to do it.
Benefits?

1) One obvious benefit of hypermedia controls is that it allows the server to change its URI scheme without br eaking clients . So, loosely couples the clients
from the server .
2) The entire service is discover -able starting from the root URI, hence the documentation is not required. The links give client developers a hint as to what may
be possible next. So, it is a form of documentation.
HATEOAS with JSON example
The key to implementing HA TEOAS is quite simple by including links in responses that go from the server to the client.
JSON Response without HATEOAS
JSON Response with HATEOAS{ Book {
 "id" : 1234 ,
 "title" : "Java/JEE Job Interview Companion"
}
}
{ Book {
 "id" : 1234 ,
 "title" : "Java/JEE Job Interview Companion"
 "links" : [ {
 "rel": "BookInfo" ,
 "href" : "http://localhost:8080/estore/catalog/books/id/1234"
 } ]
} 
}

rel: means relationship. Link gives more info about the book with id = 1234.
href: means absolute URL that uniquely defines the resource.
Here are more links to get book reviews and ratings.
State & Behavior
So, HA TEOAS refers to allowing clients to navigate through appropriate application states using hyperlinks. The client can simply look at the response for
presence of tags “BookInfo”, “BookReview”, and “BookRating” to navigate and get additional information. It is also important to note that the client of the
service doesn’ t have to figure out possible “states” or “outcomes”of a request. The possible next states are all captured with hyperlinks by the server .
For example, in an an order processing system, the “rel” and “href” will give you the possible outcomes of placing an order . When browsing a book, you have
the choices of “BookInfo”, “BookReview”, “BookRating”, “CheckOut”, etc. Once you have checked out, you will have the options of “OrderStatus”,
“CancelOrder” , and “W riteReview”.{ Book {
 "id" : 1234 ,
 "title" : "Java/JEE Job Interview Companion"
 "links" : [ {
 "rel": "BookInfo" ,
 "href" : "http://localhost:8080/estore/catalog/books/id/1234"
 }, {
 "rel": "BookReview" ,
 "href" : "http://localhost:8080/estore/reviews/id/98958788"
 }, {
 "rel": "BookRating" ,
 "href" : "http://localhost:8080/estore/ratings/id/2135478"
 }, {
 "rel": "CheckOut" ,
 "href" : "http://localhost:8080/order/book/id/1234"
 }]
} 

So, by adding hyperlinks to control next steps on the server side and NOT making the clients construct URIs, the application becomes a lot mor e flexible . This
promotes loose coupling , and makes the order processing service to evolve with new business rules without br eaking the existing clients . The state
transitions and business rules are responsibilities of the server . In a service oriented architecture, a service may have many clients. Making the changes in every
client is not flexible.
HATEOAS is a design principle that states : “clients should only interact with network applications using hypermedia controls. Neither the client software,
and its developer , require specialized knowledge about how to interface with the server beyond a URL and a general understanding of hypertext protocols such
as http. This improves the flexibility , security , and scalability of REST as an application architecture.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
