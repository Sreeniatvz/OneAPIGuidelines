# Verizon RESTful API Standards
Verizon One API Platform API standards 
* [What is an API?](#guidelines)
* [REST Constraints](#rest-constraints)
* [Pragmatic REST](#pragmatic-rest)
* [API Design Process](#design-process)
* [RESTful URLs](#restful-urls)
* [HTTP Verbs](#http-verbs)
* [HTTP Status codes](#http-statuscods)
* [Responses](#responses)
* [Error handling](#error-handling)
* [Versioning](#versions)
* [CORS](#cors)
* [Pagination &Querying](#record-limits)
* [Request & Response Examples](#request--response-examples)
* [Mock Responses](#mock-responses)
* [JSONP](#jsonp)
* [Safe &Idempotent](#safe-idempotent)
* [Best Practices](#best-practices)

## Guidelines

This document provides standards, guidelines and examples for Verizon One API platform Web APIs, encouraging consistency, maintainability, and best practices across applications. We want our APIs aim to balance a truly RESTful API interface with a positive developer experience. But at the end of day the deveopers have Power of Choice. 

When we ask API who are you?. I am sure any API answer to your question saying that I am best integrator and I make magic happen.

## Rest-Constraints
    * Client-Server
    * Stateless
    * Cacheable 
    * Layered Systems
    * Code-On-Demand(Optional)
    * Uniform Interface 

What is REST?
REST==REpresentional State Transfer. REST is resource based. Here Representation refers to the payload which we will be exchangeing  between client and server. In this REST world we will be talking about. 
 * Things vs Actions
 * Nouns Vs Verbs.
 * Multiple URIs may refering to the same resource (ex: Employee ,Invoice are resources)

The term REpresention is how the resource are manipulated.Typically the resource are represented as JSON or XML, but we may consider using CSV, Image ,HTML etc. 

For example if we consider Two Pizza team as resource . we have the following properties and represnted in JOSN as follows 

### Good examples

    "teams": [
      {"id": "1", "name": "nanda kumar"},
      {"id": "2", "name": "suresh jonnagadla"},
      {"id": "3", "name": "ross clanton"},
      {"id": "4", "name": "nagarjuna garige"},
      {"id": "5", "name": "jose stone"}
      {"id": "6", "name": "lawrance taylor"}
    ],

## Pragmatic-REST
Well, RESTful API != Good API. 

## Design-Process
 * Design
   1) Do not be very creative,be KISS - Keep It Simple , Supid. 
   2) Provide what is necessary information no more or less.
   3) When you are desinging your APIs please use handful of HTTP status codes.
 * Implement 
 * Document
 * Maintain 
## HTTP-Verbs

|HTTP Method |	RFC	    |  Request Has Body	|Response Has Body|	Safe|	Idempotent	|Cacheable|
|------------|-----------|-------------------|-----------------|------|-------------|---------|
|GET	       |  RFC 7231 |	   No             |	Yes          |     Yes |	   Yes |	   Yes|
|HEAD	       |  RFC 7231 |	   No	            |  No           |  	Yes|	   Yes	 |  Yes|
|POST	       |  RFC 7231 |	   Yes	         |   Yes	       |     No |   	No     |Yes|
|PUT	       | RFC 7231	 |    Yes	         |   Yes	       |     No  |     Yes  | 	No|
|DELETE	    |  RFC 7231 |	   No             |	Yes	       |     No  |  	Yes   |	No|
|CONNECT	    |  RFC 7231 |	   Yes	         |   Yes         |   	No   |    No    |	No|
|OPTIONS	    |  RFC 7231 |	   Optional       |	Yes	       |     Yes |     Yes  |    No|
|TRACE	    |  RFC 7231 |	   No	            |   Yes         |   	Yes  | 	Yes   |	No|
|PATCH	    |  RFC 5789 |	   Yes	         |   Yes         |      No |      No  |  	Yes|




## CORS
Browser security prevents a web page from making AJAX requests to another domain. This restriction is called the same-origin policy, and prevents a malicious site from reading sensitive data from another site. However, sometimes you might want to let other sites make cross-origin requests to your web app.
Cross Origin Resource Sharing (CORS) is a W3C standard that allows a server to relax the same-origin policy. Using CORS, a server can explicitly allow some cross-origin requests while rejecting others. CORS is safer and more flexible than earlier techniques such as JSONP.

### What is "same origin"?
Two URLs have the same origin if they have identical schemes, hosts, and ports. (RFC 6454)

### These two URLs have the same origin:
>     http://verizon.com/itwworkbench.html
>     http://verizon.com/policyserver.html
### These URLs have different origins than the previous two:
>     http://verizon.net - Different domain
>     http://verizon.com:9000/vzonepaigee.html - Different port
>     https://verizon.com/vzonepaigee.html - Different scheme
>     http://www.verizon.com/vzonepaigee.html - Different subdomain

## JSONP
JSONP stands for JSON with Padding.
JSONP is a method for sending JSON data without worrying about cross-domain issues.
JSONP does not use the XMLHttpRequest object.
JSONP uses the <script> tag instead.

Requesting a file from another domain can cause problems, due to cross-domain policy.
Requesting an external script from another domain does not have this problem.
JSONP uses this advantage, and request files using the the script tag instead of the XMLHttpRequest object.

## Safe-Idempotent

Idempotence, in programming and mathematics, is a property of some operations such that no matter how many times you execute them, you achieve the same result

| HTTP Method	| Idempotent |	Safe |
|--------------|------------|------|
| OPTIONS	   |   yes	    |  yes |
| GET	         |   yes	    |  yes | 
| HEAD	      |   yes	    |  yes |
| PUT	         |   yes	    |  no  |
| POST	      |   no	    |  no  |
| DELETE	      |   yes      |	 no  |
| PATCH	      |   no	    |  no  |

## Implementing safe methods
In HTTP, safe methods are not expected to cause side effects. Clients can send requests with safe methods without worrying about causing unintended side effects. To provide this guarantee, implement safe methods as read-only operations.

Safety does not mean that the server must return the same response every time. It just means that the client can make a request knowing that it is not going to change the state of the resource. For instance, both the following requests may be safe:

### First request
>     GET /quote?symb=VZ HTTP/1.1
>     Host: www.sreenistock.com

>     HTTP/1.1 200 OK
>     Content-Type: text/plain;charset=UTF-8
>     48.96
### Second request 10 minutes later
>     GET /quote?symb=GOOG HTTP/1.1
>     Host: www.sreenistock.com

>     HTTP/1.1 200 OK
>     Content-Type: text/plain;charset=UTF-8
>     416.10
### Idempotent methods
Its an mathematical term, an idempotent HTTP method is a HTTP method that can be called many times without different outcomes. It would not matter if the method is called only once, or ten times over. The result should be the same. Again, this only applies to the result, not the resource itself. This still can be manipulated (like an update-timestamp, provided this information is not shared in the (current) resource representation.

## Best-Practices 
### 1. Use Nouns but no Verbs

| HTTP METHOD | POST             | GET                 | PUT         | DELETE |
| ----------- | ---------------  | ------------------  | ----------- | ------ |
| CRUD OP     | CREATE           | READ                | UPDATE      | DELETE |
| /orders     | Create new orders| List all orders     | Bulk update | Delete all orders |
| /orders/1234| Error            | Show order 1234     | If exists, update order; If not, error | Delete order |

### Do not use verbs
>     /getAllOrders
>     /createOrder
>     /deleteAllOrders

### 2. GET method and query parameters should not alter the state
   Use PUT,PATCH,POST and DELETE methods instead of the GET method to alter the state.
   Use PUT, POST and DELETE methods  instead of the GET method to alter the state.
### Do not use GET for state changes:
>     GET /users/5018?activate or
>     GET /users/5018/activate
   
### 3. Use plural nouns.
Please Keep it simple and use only plural nouns for all resources. Donot mix singular and plurals.

>     /orders instead of /order
>     /users instead of /user
>     /teams instead of /team
>     /settings instead of /setting

### 4. Use HTTP headers for serialization formats[Content Negoeation].
Both, client and server, need to know which representation or format is used for the communication. The format has to be specified in the HTTP-Header.

Content-Type defines the request format.
Accept defines a list of acceptable response formats.

### 5. Use sub-resources for relations.
### 6. Provide filtering, sorting, field selection and paging for collections.
### 7. Handle Errors with HTTP status codes.

| HTTP Status Code   |  Short Description   |   Details
|--------------------|----------------------|----------------|
| 200                |   OK                 |  The request is successful.|
| 201| Created |A new resource is created.|
| 202| Accepted | The request has been accepted for processing.|
| 400| Bad Request| The request contained an error.|
| 401| Unauthorized Access was denied.| You may have entered your credentials incorrectly, or you might not have access to the requested | resource or operation|.
| 403| Forbidden |The request is for something forbidden. Authorization will not help.|
| 404| Not Found |The requested resource was not found.|
| 429| Too Many Requests| The user has sent too many requests in a given amount of time. The account is being rate limited.|
| 500| Internal Server Error | Your request could not be completed because there was a problem with the service.|
| 503| Service Unavailable| There's a problem with the service right now. Please try again later.|
     
### 9. Use error payloads
All exceptions should be mapped in an error payload. Here is an example how a JSON payload should look like.
>     {
>     "errors": [
>     {
>        "userMessage": "Sorry, the requested resource does not exist",
>        "internalMessage": "No order found in the database",
>        "code": 34,
>        "more info": "http://dev.verizon.com/api/v1/errors/12345"
>     }
>     ]
>     } 

### 8. Version your API
>     Never release an API without a version number.
>     Versions should be integers, not decimal numbers, prefixed with ‘v’.
>     For example: 
>     Good: v1, v2, v3
>     Bad: v-1.1, v1.2, 1.3
>     Maintain APIs at least one version back.




