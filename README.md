# Verizon API Standards & Goverance
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
REST==REpresentional State Transfer. REST is resource based. Here Representation refers to the payload which we will be exchangeing  between client and server. In this REST world we will be talking about 
 * Things vs Actions
 * Nouns Vs Verbs.
 * Multiple URIs may refering to the same resource (ex: Employee ,Invoice are resources)

The term REpresention is how the resource are manipulated.Typically the resource are represented as JSON or XML, but we may consider using CSV, Image ,HTML etc. 

For example if we consider Two Pizza team as resource . we have the following properties and represnted in JOSN as follows 

### Good examples

    "team": [
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
