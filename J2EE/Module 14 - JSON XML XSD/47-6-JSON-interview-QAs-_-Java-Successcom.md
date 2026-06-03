# 47 6 JSON interview Q&As   Java Success.com

## Table of Contents

- [Q1: What is JSON and how does it dif fer from an XML?](#q1)
- [Q2: What is JSON and how does it dif fer from an XML?](#q2)
- [Q3: What are some of the popular JSON libraries for Java?](#q3)
- [Q4: How do you tell your RESTful web services to accept JSON data as opposed to XML?](#q4)
- [Q5: What is JSONP?](#q5)
- [Q6: Is JSONP an industrial strength solution to overcome JavaScript cross domain res](#q6)

---

## Q1: What is JSON and how does it dif fer from an XML?

**Answer:**

JSON (JavaScript Object Notation) is a lightweight, text-based, language-netral like XML, but less verbose than XML data exchange format. JSON is used
in W eb services to exchange data between client and server . Client makes ajax requests to get snippets of data in JSON format from the server via RESTful web
service calls. JSON is easy for humans and machines to read and write. JSON can be represented as objects and arrays.
XML
JSON employee object:
JSON with Name as a separate object<Employee > 
 <name type="first" >Peter </name > 
 <age>25</age> 
</Employee > 
{
"name" :"Peter" ,
"nameT ype":"first" ,
"age" :"25"
}
{

JSON Array:

---

## Q2: What is JSON and how does it dif fer from an XML?

**Answer:**

When will you favor XML over JSON for data transfer , and when will you favor JSON over XML?
Favor XML over JSON
When you need to validate your messages using XSDs, Schematron, etc
When you need to transform your messages using XSL T. For example, XML to HTML, etc
When you need to inter -operate with environments that don’ t support JSON
When you need to have a lots of strong mark-ups.
Favor JSON over XML
When messages don’ t need to be validated or transformed
When messages predominantly have data and no marked up texts
When messaging end-points have JSON support. For example, JSON/HTTP , JSON/SMTP , etc
JSON is less verbose, simpler , and performs better .

---

## Q3: What are some of the popular JSON libraries for Java?

**Answer:**

Jackson , google-gson , and JSON-lib to name a few ."name" : {
 "name" :"Peter" ,
 "type" :"first"
}
"age" :"25"
}
{"employees" :[
 {"name" :"Peter" , nameT ype="first" , "age" :"25"},
 {"name" :"John" , nameT ype="first" , "age" :"52"},
 {"name" :"Simon" , nameT ype="first" , "age" :"34"}
}

---

## Q4: How do you tell your RESTful web services to accept JSON data as opposed to XML?

**Answer:**

You tell it via the “ Accept ” HTTP header . Accept=application/json
For example, in Spring MVC
The JAX-RS annotation @Consumes and @Pr oduces

---

## Q5: What is JSONP?

**Answer:**

JSONP is a simple way to overcome browser restrictions when sending JSON responses from dif ferent domains from the client.@Controller
public class MyAppController {
@Resource (name = "myAppService" )
private MyAppService myAppService ;
@RequestMapping (value = "/addOrModifyAdjustment" , method = RequestMethod .POST , headers ="Accept=application/json" )
public @ResponseBody MyAppDetail addOrModifyAdjustment (@RequestBody MyAppDetail adjDetail ) throws Exception
{
 MyAppDetail addOrModifyAdjustment = myAppService .addOrModifyAdjustment (adjDetail );
 return addOrModifyAdjustment ;
} 
@PUT
@Consumes ("application/json" )
@Produces ("application/json" )
@Path("{accountId}" )
public RestResponse <Account > update (Account account ) {
 ...
}

If you have a GUI application (i.e. a war) and a separate RESTful service application as a separate application running on two dif ferent domains , the you
need JSONP for your Ajax to make cross domain calls. For example, if you have 2 domains local and dev . The initial “sum” page will be loaded from the
“local” domain, and once you click on the “add” button, the Ajax call will be made to the “dev” domain to get the calculated sum via the RESTful web service
call via jsonp callback.
<script type="text/javascript" >
function add() {
var url = 'http://DEV :8080/aes-gui/simple/poc/main/add?callback=?' ;
console .log("logging..............." );
$.ajax({
 type : 'GET' ,
 url : url,
 data : {
 inputNumber1 : $("#inputNumber1" ).val(),
 inputNumber2 : $("#inputNumber2" ).val()
 },
 async : false ,
 //contentT ype : "application/json",
 dataT ype : 'jsonp' ,
 //jsonp: "callback",
 //jsonpCallback: processJSON(jsonData),
 success : function (response , textStatus , jqXHR ) {
 console .log("reached here" );
 // data contains the result
 // Assign result to the sum id
 $("#sum" ).replaceW ith('<span id="sum">' + response + '</span>' );
 console .log(response );
 },
 error : function (jqXHR , textStatus , errorThrown ) {
 console .log(errorThrown );
 }
});
;
</script>

---

## Q6: Is JSONP an industrial strength solution to overcome JavaScript cross domain restriction?

**Answer:**

No. CORS is the industrial strength solution for the cross domain Ajax calls.
JSONP has a number of limitations like, it supports only GET requests and not PUT , POST , DELETE, etc and it does not also send headers across. CORS
stands for Cross Origin Resour ce Sharing , which allows you to share GET , POST , PUT , and DELETE requests and CORS is supported by the modern
browsers.The CORS make use of 2 requests.
Request 1 : “OPTIONS ” request as part of the handshake to determine if cross domain is allowed by the server .
Request 2 : GET , POST , PUT , or DELETE request that performs the actual operation on the server .

CORS
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed

« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
