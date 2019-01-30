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
