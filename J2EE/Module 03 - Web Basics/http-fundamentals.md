# HTTP Fundamentals

> **Module**: Web Basics  
> **Topic**: HTTP Fundamentals

---

## 📋 Table of Contents



- [Q1: What happens when you open up a browser and type a URL to request a W eb page or](#q1)
- [Q2: What are HTTP headers? Why do you need them?](#q2)
- [Q3: What is an HTTP-Only cookie?](#q3)
- [Q4: How will you go about debugging the data sent from the server on the client-side](#q4)

---

## Q1: What happens when you open up a browser and type a URL to request a W eb page or RESTFul web service data?

**Answer:**

HTTP is a stateless protocol on top of TCP (T ransmission Control Protocol).
1) When the IP address is obtained, the browser will attempt to open a TCP connection to the web server, usually on port 80.
2) Once the TCP connection is made, the browser will issue an HTTP r equest to the server using the connection.
3) The HTTP request comprises a header section, and possibly a body section, which is where POST data go, and in GET request the parameters are passed in
the URI.
4) Once the request is sent, the browser will wait for the response.
5) When the web server has assembled the response, it is sent back to the browser for rendering. The HTTP response consists of a header section and a body .
The header section tells the browser how to treat the body content and the browser renders the content for viewing. Each HTTP response includes a status code,
which indicates the status of
the request.
6) “headers” are sent before the actual page content. These headers are invisible, but can be viewed via development tools like Firefox plugins and in Chrome
with “Developer tools”. The browser uses the Content-T ype header to determine the type of data sent like text, xml, json, etc. This is also known as the MIME
type of the particular W eb resource.
7) The common response status codes include, 200 OK, 404 NOT FOUND, 500 Internal Error, etc.
8) Most HTTP responses will also contain references to other objects within the body that will cause the browser to automatically request these objects as well.
Web pages
often contain more than 50+ other object references like style sheets (i.e. CSS), images, JavaScript files, etc to complete the page. Y our browser will create
additional TCP connections for these referenced references. For example, 2 to 3 connections per host.
The basic request is comprised of
1) a method –> GET, POST, PUT, DELETE, HEAD, and OPTIONS
2) the URI (Uniform Resource Indicator) –> a RESTFul API ends up being simply a collection of URIs. T o read a customer with Customer ID# 725,
http://www .myhost.com/customers/725
3) HTTP version desired –> 1.0 or 1.1

---

## Q2: What are HTTP headers? Why do you need them?

**Answer:**

HTTP headers carry information about behavior between the browser and the web server. The headers are sent by the web server to tell the browser how to
treat the content. For example, the “ Content-T ype” header tells what type of data to expect XML, JSON, HTML, etc. The “ Content-Disposition ” header tells
browser to display the content on the browser (i.e. inline) or as a download (i.e. attachment) to be saved on to the file system with a popup window .

“Connection : Keep-Alive” header will reuse TCP connections for subsequent requests and will save on the latency incurred especially in applications that
utilize W eb 2.0 technology such as AJAX (Asynchronous JavaScript and XML) to perform real-time updates of content as it reduces the overhead associated
with opening and closing TCP connections.
Cookies are sent by the web server to the browser as an HTTP header and used to store all sorts of information about a user ’s interaction with the site. A cookie
is a small plain text file without any executable code that is stored by a browser on the user ’s machine. A web server specifies a cookie to be stored by sending
an HTTP header called Set-Cookie.
When a cookie is present, and the optional rules allow, the cookie value is sent to the server with each subsequent request. The cookie value is stored in an
HTTP header called Cookie .

---

## Q3: What is an HTTP-Only cookie?

**Answer:**

The idea behind HTTP-only cookies is to instruct a browser that a cookie should never be accessible via JavaScript through the document.cookie property .
This feature was designed as a security measure to help prevent cross-site scripting (XSS) attacks perpetrated by stealing cookies via JavaScript.

---

## Q4: How will you go about debugging the data sent from the server on the client-side like header info, cookies, resources sent, debugging CSS, JavaScript, etc?

**Answer:**

With the help of browser debug tools like Fire fox plugins like FireBug, Live HTTP Headers, Modify Headers or in Chrome Mor e Tools –> Developer
Tools from the main.
menu.
Ctrl+Shift+I for the interactive development on Google Chrome
Type: www .java-success.com on Google chrome and then press “Ctrl+Shift+I for the interactive development” split/popup window .
HTTP /1.0 200 OK
Content -type: text/html
Set-Cookie : name =value
Set-Cookie : name2 =value2; Expires =Wed, 09 Jun 2021 10:18:14 GMT

Client side debugging of HTTP
Here is a Java W eb Service server/client code snippet
Using “response.header(“Content-Disposition”, “attachment; filename=test.csv”);” on the server side and “client.type(“text/csv”).accept(“text/csv”);” on the
client side with MIME type headers .
On the Java Server side RESTFul Web service snippet
/...
@Path("/downloadservice/" )
@Produces ("application/xml" )
public interface FileDownloadW ebService {
@GET
@Path("/get/{id}" )
public DownloadFileInformation getFileInformation (@PathParam ("id") long id) throws DatatypeConfigurationException ;

On the Java client side snippet/....
public class FileDownloadW ebServiceImpl implements FileDownloadW ebService {
private FileDownloadService downloadService ;
@Override
public Response getFile (long id) {
 FileUpload fileInfo = downloadService .getFileById (id);
 ResponseBuilder response = Response .ok(new File(fileInfo .getFilePath (), fileInfo .getFileName ()));
 response .header ("Content-Disposition", "attachment; filename=test.csv" );
 return response .build ();
}
//..
public String invokeRestfulWs (....) {
 String errorMsg = null;
 FileW riter fw = null;
 BufferedReader br = null;
 try {
 String urlAddress = "http://myhost:8080/service-ws" + "/" + "downloadservice/get/" + 5; //file id is 5
 
 WebClient client = WebClient .create (urlAddress, configLocation );
 //headers
 client .type("text/csv" ).accept ("text/csv" );

Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » InputStream is = client .get(InputStream .class );
 int index = fileNameT oDownload .indexOf ('.');
 //writ back to file system
 String fileNameT oSave = fileNameT oDownload .substring (0, index ) + "_ReturnFile.csv" ;
 
 File file = new File(localDir + File.separator + fileNameT oSave );
 fw = new FileW riter(file);
 
 InputStreamReader isr = new InputStreamReader (is);
 br = new BufferedReader (isr);
 
 String read = br.readLine ();
 
 while (read != null){
 fw.write (read);
 logger .info(read);
 read = br.readLine ();
 }
 
 } catch (Exception e) {
 logger .error ("Error Downloading: " + e);
 errorMsg = "Error Downloading: " + e.getMessage ();
 } finally {
 try {
 br.close ();
 fw.close ();
 } catch (IOException e) {
 e.printStackT race();
 }
 }
 return errorMsg ;
}

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03