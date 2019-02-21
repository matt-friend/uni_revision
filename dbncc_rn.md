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

## Tables in SQL

### Concept - Tables and keys
* rows/ records
* column names/ fields/ attributes
* schema: information about the table without saying any of the informantion in it

### How to address data in a table
* SUPERKEY - a combinatoin of fields that uniquely determines a row
* CANDIDATE KEY/ KEY - a superkey that is also minimal

### SUPERKEY
* This means: after choosing data for the fields in the superkey. there is no choice over the rest of the data in that row
* E.g. {house, postcode} is a superkey for address{house, street, town, postcode} - if I fix (house, postcode) then there is no choice for the rest of the fields
* E.g. {countryName} is a superkey for Countries(countryName, capitalCity, continent) - continent is not a superkey as it can be present in multiple records
* If something is uniquely determined then there is no choice (sic)

### Candidate key
* If you remove any field from a key it ceases to be a superkey
* Minimal because there is mp smaller subset that works
* E.g. {house, postcode} is a key
* {postcode} is not a key
* {house, postcode, street} is not a key because it is not minimal
* sometimes need to make assumptions for a key e.g. assuming that there are no two bank branches of the same bank on the same road to get the key of {bank, road, town} from the table bankBranch{bank, road, town, sortCode}

### SQL

### INSERT INTO
* use to put record into table
* provide fields and data
* e.g. INSERT INTO Address (house, postcode) VALUES (16, 'WR11 3XY')
* it is not necessary to populate all the fields in a table - if fields have default value table will take that if field missing, otherwise null
* Strings must be single quoted

### DELETE FROM
* DELETE FROM Address;
* must give semicolon to process function
* DELETE FROM Address WHERE town = 'Bristol' 
* UPDATE TABLE Address SET town = 'Bristol' WHERE postcode = 'XXXX YYY'

### DROP TABLE
* gives error if field doesnt exist; use if exists to avoid
* DROP TABLE lecturer
* DROP TABLE IF EXISTS lecturer

### CREATE TABLE
* CREATE TABLE BankBranches ( // column definitions // table constraints )
* Column definitions e.g (name VARCHAR(30), sortcode CHAR(8)) - varchar has variable length wheras char has fixed length (efficiency)
* type = INT | VARCHAR (n) | CHAR (n) | BIGINT | TIME | REAL | DATE | TIME | DATETIME etc
* Table constraints are a way to tell the DBMS about your table
* e.g CONSTRAINT key_constr UNIQUE (name, street, town) where:
	* key_constr is an identifier to be used in error messages
	* (name, street, town) together form a key
* this allows the DBMS to throw an error if you enter a tuple that contains a duplicaate key combination to an existing record
	* ERROR: Duplicate entry for key 'key-constr'

* Every table must have exactly one PRIMARY KEY. It is often a good odea to invent an artficial numeric key for this purpose
* Otherwise, constraints are an oppotunity for you to give more information to the DBMS and in return will stop you from making mistakes

### FOREIGN KEY
* Used to link tables together
* Good idea to have many tables where the subject of the table is distinct from others
* e.g. Unit table might contain 'director' that is a FOREIGN KEY that links to the primary key of another table 'Lecturer'
* CREATE TABLE Unit ( ... CONSTRAINT unit_dir FOREIGN KEY (director) REFERENCES Lecturer (id))
* If you then try to DROP TABLE Lecturer there will be an error as there will now be dangling reference in the table Unit to a table that no longer exists

### CREATE-DROP scripting
* script to create a table
* contains creation of tables in correct order to ensure foreign key establishment correctness
* often contains a drop-table if exists at the top to ensure no pre-existing tables with same name as ones being created (helps if re-running script multiple times when first implementing and optimising)

### NOT NULL
* id INT PRIMARY KEY AUTO INCREMENT - auto increment assigns value to primary key of one more than previous record if not assigned
* username VARCHAR(10) NOT NULL - not null guarantees property of data that it cannot be null either by ommission in record creation or by any other methods
* num_dogs INT DEFAULT 0 - sets default value for field assignnemt if not specified in record creation 

---

## Lecture 6?

### Motivation
* If a column/ field is linked to a primary key field in another table/ relation then you cannot enter a value into that column that does not already exist in the relation that has been linked to
* Sending queries is expensive; want to find a way to reduce that number

### Relational programming concept - Cartesian product
* The product of tables you have two input tables that are combined to form a Cartesian product table consisting of all possible combinations of the rows in each table i.e two tables with 2 and 6 rows respectively would form a cartesian product table with 12 rows (all possible 'glueings'
* Ways of determing meaningful rows - if some of the fields are related i.e if the primary key of one field is a foreign key in a field of the table it is being cartesianed with - can use this relation to filter out some more meaningful rows
* Can infer meaning from table combinations like this - often large proportion of rows are removed 
* This combination of processes is very prevalent - concept of JOIN
* The result of this process is the JOIN of two tables based on a condition (usually one primary key = a foreign key)
* So process is product followed by selection
* e.g. Join of Mage and MageSpell on id = mage
* DBMS does not typically perform automatic removal of fields - extra programming must be done to extract relevant information 
* e.g. SELECT field FROM table (joins table on p.key = f.key) WHERE condition

* Selecting names from table where field need to match unkown constant; JOIN to itself on field match
* e.g. SELECT R.name FROM Lecturer L JOIN Lecturer R ON L.rgroup = R.rgroup / WHERE L.name = 'Peter'

### SQL Join variants: Natural join
* "Join on common columns"
* If the two tables being joined have columns with the same name then join on those columns
* Shorter but not recommended for use

### SQL Join variants: Left join
* Part of class of outer joins
* Computes the inner join; looks at all rows on the left of the join that are not present in the join table and adds the record as a new row with missing fields populated as null
* Ensures all data is present even if the result of the comparison for selecting join records is null

### Join variants
* Inner (just JOIN), natural, left outer (LEFT JOIN), right outer, full outer, cross (',' - this is just the product)

### Set operations
* Join is to glue tables
* Query1 UNION ALL Query2 sticks all data from one table to data from another assuming all columns are the same
* UNION does the same but removes duplicate records? 
* INTERSECT
* EXCEPT

### How to read the E-R diagram of a schema
* Entity relation diagrams
* floating boxes (tables)
* tables will have highlighted fields (primary key) - remember can be combination of multiple fields
* entities are related by lines signifying a PK-FK relationship
* often contextual information on the relationship such as relationship multiplicity - one to one, one to many
* 


