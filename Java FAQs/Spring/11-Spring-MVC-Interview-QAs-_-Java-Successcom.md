# 11 Spring MVC Interview Q&As   Java Success.com

## Table of Contents

- [Q1: Can you describe the Spring W eb MVC framework architecture?](#q1)
- [Q2: What are the advantages of Spring MVC?](#q2)
- [Q3: What are some of the important annotations?](#q3)
- [Q4: What is a “ResponseEntity” in Spring MVC?](#q4)
- [Q5: What is an “HttpMessageConverter” in Spring MVC?](#q5)
- [Q6: What are interceptors in Spring MVC?](#q6)
- [Q7: How do you validate form data in Spring MVC?](#q7)
- [Q8: How do you handle exceptions in Spring MVC?](#q8)
- [Q9: How do you enable security in Spring MVC?](#q9)
- [Q10: How do you achieve localization in Spring MVC?](#q10)
- [Q11: How do you get handles on ServletContext and ServletConfig objects Spring MVC?](#q11)

---

## Q1: Can you describe the Spring W eb MVC framework architecture?

**Answer:**

Spring W eb MVC framework provides a Model View Controller architecture to develop loosely coupled 1) web applications 2) RESTful web services.
Here is the high level diagram.
Spring MVC Flow
Step 1: All requests are handles by the Spring DispatcherServlet , which applies the Front Contr oller design pattern.

Steps 2, 3, 4, 5 & 6: Spring Handler Mapping will find the appropriate Controller classes, which are annotated via @Contr oller for web applications and
@RestContr oller for the RESTful web services. The controller executes the request and creates a Model and stores it in a relevant scope like Request, Session,
etc. Finally , selects a View if it is web application or sends the model data as XML or JSON back to the client (e.g. Browser) if its a RESTful web service.
Steps 7, 8, 9 & 10: If it is web application the DispatcherServlet resolves the view name and redirects to the view template (e.g. JSP , Thymeleaf, Freemarker ,
etc). The response html is returned to DispatcherServlet, and then to the browser .
This is covered in detail: Spring MVC beginner tutorial step by step | ⏯ Spring MVC beginner video tutorial step by step .

---

## Q2: What are the advantages of Spring MVC?

**Answer:**

1) Spring provides a very clear separation of controllers, Java Bean models, and view technologies like JSPs, Thymeleaf, V elocity , FreeMarker , etc. Y ou
can also roll out your own custom templating language by implementing the Spring V iew interface. Just about every part of MVC is configurable by plugging in
an implementation to its interface, and thanks to Spring’ s adherence to the OOP design principle “ open/closed principle (OCP) “.
2) Spring Controllers are configured via IoC, making it easier to unit test. Spring Controller Unit T esting .
3) Controller can either select a view name and prepare model map for it or write the response directly to response stream in XML, JSON, Atom, and many
other types of content to enable RESTful web services. Spring 4 MVC RESTful W eb Service Beginner T utorial step by step .

---

## Q3: What are some of the important annotations?

**Answer:**

Refer to the Spring MVC tutorials for details.
@Controller : This class will serve as a controller .
@RestController : This class will serve as a controller to service RESTful web services. Under the hood, a “@RestController = @Controller +
@ResponseBody”.
@RequestMapping : Maps URI path to a class or method depending on defined at the class or method level.
@PathV ariable : Used to map a dynamic value in the URI to a method ar gument.
@RequestBody : Spring will bind the incoming HTTP request body in XML, JSON, etc based on the Accept header .
@ResponseBody : Spring will bind the return value to outgoing HTTP response body . Spring will use HTTP Message converters to convert to XML, JSO,
etc.

---

## Q4: What is a “ResponseEntity” in Spring MVC?

**Answer:**

ResponseEntity is used to represent an entire “ HTTP r esponse “.

---

## Q5: What is an “HttpMessageConverter” in Spring MVC?

**Answer:**

An HttpMessageConverter is responsible for marshall & unmarshall Java POJO objects like Account, User , etc to and from JSON, XML, etc over
HTTP(s). The Spring MVC applications need to be annotated with “ @EnableW ebMvc “.
Spring is configured out-of-box for many default HttpMessageConverters depending on presence of certain libraries in the project classpath. For example, add
the following libraries to work with XML & JSON conversions. If the Content-T ype in request header was one of application/json or application/xml , and
the relevant libraries shown below are in the claspath, then Spring will delegate the conversion to MappingJackson2HttpMessageConverter for JSON or
MappingJackson2XmlHttpMessageConverter for XML.@RequestMapping (value = "/account" , method = RequestMethod .POST )
 public ResponseEntity <Account > saveAccount (@RequestBody Account account ) {
 System .out.println ("Creating:" + account );
 //TODO: Save to database
 return new ResponseEntity <Account >(account , HttpStatus .OK);
 }
<!-- JSON data binding -->
 <dependency >
 <groupId >com.fasterxml .jackson .core</groupId >
 <artifactId >jackson -databind </artifactId >
 <version >2.8.9 </version >
 </dependency >
 <!-- XML data binding -->
 <dependency >
 <groupId >com.fasterxml .jackson .dataformat </groupId >
 <artifactId >jackson -dataformat -xml</artifactId >
 <version >2.8.9 </version >
 </dependency >

The “HttpMessageConverter” is a good example of the “ Strategy design pattern ” with dif ferent implementations like
“MappingJackson2HttpMessageConverter” to read and write JSON using Jackson’ s ObjectMapper , and “MappingJackson2XmlHttpMessageConverter” to read
and write XML using Jackson XML extension’ s XmlMapper , “StringHttpMessageConverter” to read and write Strings from the HTTP request and response,
“FormHttpMessageConverter” to read and write form data from the HTTP request and response, and so on. Y ou can also create your own custom
implementation of the “HttpMessageConverter”.

---

## Q6: What are interceptors in Spring MVC?

**Answer:**

Handler interceptors are used when you want to apply specific behaviour to certain requests. Handler Interceptors should implement the interface
“HandlerInter ceptor ” to provide specific behaviours via “preHandle(..)”, “postHandle(..)”, and “afterCompletion(..)” methods.
You can also extend/....
public class LoggingInterceptor implements HandlerInterceptor {
 
 @Override
 public boolean preHandle (HttpServletRequest request , HttpServletResponse response , Object handler )
 throws Exception {
 //....
 }
 
 @Override
 public void postHandle ( HttpServletRequest request , HttpServletResponse response ,
 Object handler , ModelAndV iew modelAndV iew) throws Exception {
 //...
 }
 
 @Override
 public void afterCompletion (HttpServletRequest request , HttpServletResponse response ,
 Object handler , Exception ex) throws Exception {
 //... 
 }

finally the Java Configuration class will have an entry as

---

## Q7: How do you validate form data in Spring MVC?

**Answer:**

Spring MVC supports validation by means of a validator object that implements the Validator interface. Y ou need to create a class and implement the
Validator interface.

---

## Q8: How do you handle exceptions in Spring MVC?

**Answer:**

To apply to entire application – Spring Java Configpublic class TransactionInterceptor extends HandlerInterceptorAdapter {
 @Override
 public boolean preHandle (HttpServletRequest request , HttpServletResponse response , Object handler )
 throws Exception {
 //.....
 }
}
@Override
public void addInterceptors (InterceptorRegistry registry ) {
 registry .addInterceptor (new LoggingInterceptor ());
 registry .addInterceptor (new TransactionInterceptor ()).addPathPatterns ("/account" );
}

To apply to a specific handler method with @ExceptionHandler annotation

---

## Q9: How do you enable security in Spring MVC?

**Answer:**

Spring Security is used to implement Authentication and Authorization for a web application.
Spring Security dependencies jars@Bean
public SimpleMappingExceptionResolver exceptionResolver () {
 SimpleMappingExceptionResolver exceptionResolver = new SimpleMappingExceptionResolver ();
 Properties exceptionMappings = new Properties ();
 exceptionMappings .put("java.lang.Exception" , "error/error" );
 exceptionMappings .put("java.lang.RuntimeException" , "error/error" );
 exceptionResolver .setExceptionMappings (exceptionMappings );
 Properties statusCodes = new Properties ();
 statusCodes .put("error/404" , "404" );
 statusCodes .put("error/error" , "500" );
 exceptionResolver .setStatusCodes (statusCodes );
 return exceptionResolver ;
@ExceptionHandler ({IOException .class , java.sql.SQLException .class })
public ModelAndV iew handleIOException (Exception ex) {
 ModelAndV iew model = new ModelAndV iew("IOError" );
 model .addObject ("exception" , ex.getMessage ());
 return model ;
}

Configure Security
Configure Spring Web with SecurityConfiguration class<!-- Spring Security -->
<dependency >
 <groupId >org.springframework .security </groupId >
 <artifactId >spring -security -web</artifactId >
 <version >4.0.4.RELEASE </version >
</dependency >
<dependency >
 <groupId >org.springframework .security </groupId >
 <artifactId >spring -security -config </artifactId >
 <version >4.0.4.RELEASE </version >
</dependency >
<dependency >
 <groupId >org.springframework .security </groupId >
 <artifactId >spring -security -taglibs </artifactId >
 <version >4.0.4.RELEASE </version >
</dependency >
@Configuration
@EnableW ebSecurity
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {
 //.....
}

---

## Q10: How do you achieve localization in Spring MVC?

**Answer:**

LocaleResolver is shipped with Spring to support Internationalization (i18n) and Localization (L10n).
/i18n/meesages_en.properties
Controller@Configuration
@EnableW ebMvc
@ComponentScan (basePackages = "com.mytutorial.controller" )
@Import (value = { SecurityConfiguration .class })
public class SimpleW ebConfiguration {
 //......
}
abel.title=Login Page
abel.submit =Login
@Controller
@RequestMapping ("/")
public class LoginController {
 @RequestMapping (value = "/login" , method = RequestMethod .GET )
 public String home (Locale locale ) {

JSP View
Spring Java Configuration logger .info("Welcome! The client's locale is {}." , locale );
 return "home" ;
 }
<%@taglib uri="http://www .springframework.or g/tags" prefix ="spring" %>
<%@ page session ="false" %>
<html>
<head >
<title><spring :message code ="label.title" /></title>
</head >
<body >
 //......
</body >
</html>
@Bean
public MessageSource messageSource () {
 ReloadableResourceBundleMessageSource messageSource = new ReloadableResourceBundleMessageSource ();
 messageSource .setBasename ("/i18n/messages" );
 messageSource .setDefaultEncoding ("UTF-8" );
 return messageSource ;
}

---

## Q11: How do you get handles on ServletContext and ServletConfig objects Spring MVC?

**Answer:**

@Autowir ed annotation to the rescue.
@RestController
@RequestMapping ("/v1/forecasting" )
public class AccountController {
 @Autowired
 private ServletContext context ;
 @Autowired
 private ServletConfig config ;
 @RequestMapping (value = "/accounts" , method = RequestMethod .GET , produces ={MediaT ype.APPLICA TION_JSON_V ALUE })
 public Account [] getAccounts () {
 Account [] accounts = new Account [] { new Account ("123" , "John R" , BigDecimal .valueOf (235.00 )),
 new Account ("345" , "Peter J" , BigDecimal .valueOf (2505.60 )) };
 return accounts ;
 }

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
