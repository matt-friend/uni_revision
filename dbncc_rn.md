# Databases and Cloud Concepts

## Lecture 1: Introduction

### The Internet
* World-wide computer network
* Connects millions of computers worldwide 
* Devices include desktops, laptops, mobile phones, security cameras, smart devices etc
* These devices are called *hosts* or *end systems*
* Connected by communications links e.g copper wire, fibre optic, radio waves which transfer data at differnent rates.
* Most devices connected indirectly via routers, which handle the receiving and forwarding of data packets.

A program or machine that responds to requests is a server

A program or machine that sends requests to a server is a client

The internet is possible due to the work of the IETF (Internet Engineering Task Force). THese people develop, test and implement Internet standards. The documents they produce are known as RFCs, *requests for commenst*. They are technical and detailed and define protocols such as TCP, IP, HTTP and SMTP (email)

### Protocols
A protocol is an agreement on how to communicate.

This dictates the actions taken in response to a message/ request etc

**Request-response** - A simple protocol; one machine sends a request and another replies with a response.

### Internet layers
* **HTTP** - *Application Layer* - Makes request, reads and handles responses
* **TCP** - *Transmission Layer* - Breaks data into numbered pakcets, reassembles at other end
* **IP** - *Network Layer* - Routes data through network using home and destination addresses 
* **Physical Internet** - *Physical Layer* - How bits are moved arounf through the network

### HTTP
**Hyper Text Transfer Protocol** is one of the most common protocols on the internet, most common application protocol.

Operates CRUD methods:
* **Create** - HTTP POST, creates new resource at server assigned location 
* **Read** - HTTP GET, accesses current resource content
* **Update** - HTTP PUT, creates new current version of resource, or new resource if none with id
* **Delete** - HTTP DELETE, removes current (existing) resource

### HTTP structure
* line-based, request-response system
* header + data, header ends with empty line to signify start of data
* Content-type - says what the resource type is. HTTP response content-type overrides any information in the resource itself.

### URLs
URL = **Uniform Resource Locator**; tells you where resource is (address)

Consists of scheme, host, path

* Scheme = protocol
* Host = web address name (represents IP address)
* Path = up to individual server; originally just location of resource needed

Queries - data often sent to server via URL - this is a query

e.g. ?name=David&action=view

---

## Lecture 4: Representing Data as trees - XML and JSON

### Representing Data
* list: 1-dimensional data
* table: 2-dimensional data
* 3 dimensions isn't a problem (pick a 3rd seperator character)

What if we want to store/ transmit arbitrary structures of objects?

### Tree representations
* A tree structure consists of nodes and branches
* A way of representing the hierarchical structure of data
* E.g. file systems, Job positions withing departments, mathematical operations (RPN), documents (nested HTML)
* Most data can be represented as a tree
* To represent a structure as a tree we just need three separators: START, END and FIELD (plus an escape character)

### Escape characters
* Signals that a character needs to be interpreted differently from the same character without that prefix
* For example in C it is '\'.
* The compiler expects another character after that to complete the escape sequence.

## XML

### Design goals
1. XML shall be straightforwardly usable over the internet
2. It shall be easy to write programs which process XML documents
3. The number of optional features in XML is to be kept to the absolute minimum, ideally zero.
4. XML documents shall be easy to create.

### XML vs HTML
* HTML is about *displaying* information
* XML is about *describing* information
* XML is the most common tool for data manipulation and data transmission and can be used for data storage. 
* XML is both human and machine readable, whilst being flexible enough to support platform and architecture independent data interchange
* XML allows a software engineer to **create** a vocabulary and use it to describe data - it is an **extensible** language

### XML properties
* Information identification - meaningful names for data items
* Information storage - portable and non-proprietary (store info oon any platform)
* Information structure - nesting allows for storage and identification of any type of hierarchical information
* Data transfer - provides a data format for the exchange of information between computers (e.g client-server)
* XML files are meant to be human readable; e.g. root element shows what data represents

### XML validation
* In XML there are separate steps for validation and processing
* Client side: XML document sent
* Server side: validation and processing
* TWo validation methods:
	1. DTD (Document Type Definition)
	2. schema

### Schema validation
* More powerful than DTD
* XML schema use XML to describe an XML document structure
* XSD = XML Schema Definition

### XML entities 
* These are like escape sequences, also used in HTML
* e.g < is represented by '&lt\;'

### XPath/ XQuery
* Query language for XML
* Provide ways to address nodes in an XML document
* Also works for non-XML versions of HTML and can be very useful in web applications.
* Same as CSS selectors, but different syntax

### XSLT
* eXtensible Stylesheet Transformation Language
* Way of transforming XML documents into other documents
* XML -> XSLT process -> new file type e.g XML, HTML, PDF

## JSON

### JSON 
* JavaScript Object Notation
* Widely used data format for web applications
* JSON is text so it can be used to exchange data between a server and client browser
* Easy to read and write for humans
* As text, can be parsed and generated by most programming languages

### JSON syntax
* Derived from JavaScript object notation syntax:
	* Data is in name/ value pairs
	* Data is separated by commas
	* Curly braces hold objects
	* Square brackets hold arrays

### JSON vs XML 2
* JSON doesn't use end tags
* JSON is shorter
* JSON is quicker to read and write
* JSON can use arrays

The biggest difference is: 

XML has to be parsed with an XML parser. JSON can be parsed by a standard JavaScript function or in Java by the Gson library

## Templating

### FreeMarker
* Apache FreeMarker - Java templating library
* Java library to generate text output (HTML web pages, e-mails etc) based on templates and changing data
* Templates are written in the FreeMarker Template Language
* General-purpose langauge like Java used to prepare the data then AFM displays the data using templates

---
