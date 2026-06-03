# 5  REST Service URI conventions with Spring MVC   Java Success.com

## Table of Contents

The high level pattern for the RESTful URI is
http(s)://myserver .com:8080/app-name/ {version-no} /{domain} /{rest-r eour ce-convetion}
For example :
http(s)://myserver .com:8080/accounting-services/ 1.0/forecasting /accounts
to list all the accounts . This is a plural r esour ce returning a collections of accounts. The URI contains nouns representing the resources in a hierarchical
structure. For example, if you want a to get a particular transaction value under an account you can do
http(s)://myserver .com:8080/accounting-services/ 1.0/forecasting/ account/123/transaction/567
Where 123 is the account number and 567 is the transaction number or id. This is a singular r esour ce returning a single transaction. 
What if you want to list a collection of transactions that are greater than a particular date?
http(s)://myserver .com:8080/accounting-services/ 1.0/forecasting/ account/123/transactions/sear ch?txn-date=20120201
The verbs are defined via the HTTP methods GET , POST , PUT , and DELETE . The above examples are basically GET requests returning accounts or
transactions. If you want to create a new transaction request, you do a POST with the following URL.
http(s)://myserver .com:8080/accounting-services/ 1.0/forecasting/ account/123/transaction
 The actual transaction data will be sent in the body of the request as JSON data. The above URI will be used with a PUT http method to modify an existing
transaction record.
Finally , you can also control which method gets executed with the help of HTTP headers or host names in the URL. Let’s see some Spring MVC examples in a
controller class as to how it maps a request URI, headers, etc to execute the relevant method on the server side.
Here is the sample code with GET requests
@Controller
@RequestMapping ("/forecasting" )

public class CashForecastController
{
 @RequestMapping (
 value = "/accounts,
 method = RequestMethod.GET ,
 produces = " application /json")
 @ResponseBody
 public AccountResult getAllAccounts(HttpServletResponse response) throws Exception
 { 
 //get the accounts via a service and a dao layers
 } 
@RequestMapping(
 value = " /accounts .csv,
 method = RequestMethod .GET ,
 produces = "text/csv" )
 @ResponseBody
 public void getAllAccounts (HttpServletResponse response ) throws Exception
 { 
 //produces a CSV download file
 } 
 @RequestMapping (
 value = "/account/{accountCd}" ,
 method = RequestMethod .GET ,
 produces = "application/json" )
 @ResponseBody
 public Account getAccount (
 @PathV ariable (value = "accountCd" ) String accountCode , HttpServletResponse response ) throws Exception
 {
 //get the accounts via a service and a dao layers
/accept only if there is a special header
@RequestMapping (
 value = "/account/{accountCd}" ,
 method = RequestMethod .GET ,
 headers =
 {
 "operation=special"
 }
 produces = "application/json" )
 @ResponseBody

Here is the sample code with POST and PUT requests public Account getAccountSpecial (
 @PathV ariable (value = "accountCd" ) String accountCode , HttpServletResponse response ) throws Exception
 {
 //get the accounts via a service and a dao layers
//special handling based
 @RequestMapping (
 value = "/account/{accountCd}/transaction/{transactionId}" ,
 method = RequestMethod .GET ,
 produces = "application/json" )
 @ResponseBody
 public Transaction getTransaction (
 @PathV ariable (value = "accountCd" ) String accountCode ,
 @PathV ariable (value = "transactionId" ) String txnId ,
 HttpServletResponse response ) throws Exception
 {
 //get the accounts via a service and a dao layers
//accountCode and txnId can be used here
@RequestMapping (
 value = "/account/{accountCd}/transactions/search" ,
 method = RequestMethod .GET ,
 produces = "application/json" )
 @ResponseBody
 public TransactionResult getTransactions (
 @PathV ariable (value = "accountCd" ) String accountCode ,
 @RequestParam (value = "txn-date" , required = true) @DateT imeFormat (pattern = "yyyyMMdd" ) Date txnDate ,
 HttpServletResponse response ) throws Exception
 {
 //get the accounts via a service and a dao layers
//accountCode and txnDate can be used here

Don’ t use query parameters to alter state. Use query parameters for sub-selection of a resource like pagination, filtering, search queries, etc
Don’ t use implementation-specific extensions in your URIs (.do, .py , .jsf, etc.). Y ou can use .csv , .json, etc.
Don’ t ever use GET to alter state. Use GET for as much as possible. Favor POST over PUT when in doubt. 
Don’ t perform an operation that is not idempotent with PUT . 
Do use DELETE in preference to POST to remove resources.
Don’ t clutter your URL with verbs or stuf f that should be in a header or body . Move stuf f out of the URI that should be in an HTTP header or a body .
Whenever it looks like you need a new verb in the URL, think about turning that verb into a noun instead. For example, turn ‘activate’ into ‘activation’,@Controller
@RequestMapping ("/forecasting" )
public class CashForecastController
{
 @RequestMapping (
 value = "/account/transaction" ,
 method = RequestMethod .POST )
 public @ResponseBody Transaction addT ransaction (@RequestBody Transaction txn, HttpServletResponse response )
 throws Exception
 {
 
 //logic to create a new T ransaction records via service and dao layers
 
 }
@RequestMapping (
 value = "/account/transaction" ,
 method = RequestMethod .PUT )
 public @ResponseBody Transaction modifyT ransaction (@RequestBody Transaction txn, HttpServletResponse response )
 throws Exception
 {
 
 //logic to modify a T ransaction record via service and dao layers
 
 }

and ‘validate’ into ‘validation’.



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
