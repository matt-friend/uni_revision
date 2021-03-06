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

## Lecture 8 - Product and join

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

---

## Lecture 9 - Aggregration and nesting

### Aggregration: part 1
* Student field in Enrol relation - {Student, unit, grade}
* What is the average grade of each student over all units?

* Group relation by field x: x becomes a key on the output table
* Can then use aggregation functions to specify other columns
* e.g. aggregrate avg(y) would output the average of all y values for each unique x
* Can't say **group by x, aggregate avg(y), z** as would not output three columns with same nunber of rows (z not aggregated)

### Aggregation: part 2
* Another aggregate function - max(z)
* 'foo' - fill z with foo
* **group by x, z aggregate avg(y)** all x, z values grouped together (need to appear in same field on original table)

### Aggregation in SQL
* Order: SELECT columns FROM table WHERE condition GROUP BY key column
* Note: SQL requires that you list the key column in columns if you wish to have it appear in the output

### SQL aggregate functions
* MIN, MAX, AVG, SUM, COUNT, COUNT(DISTINCT x), COUNT (\*) (this includes null rows in the count)
* Able to have basic math functions of the column inside the function call e.g. **SUM(10+grade * 2)**

### How to avoid problems
* Each column specifier in SELECT should return at most one value when evaluated on a group
* This is guaranteed if each col is either:
	* Mentioned in the GROUP BY clause
	* An aggregate function application
	* A constant

### Selecting aggregates: HAVING
* After groupin by key column and aggregating columns, you can once again select data conditionally
* Use the HAVING operator after the GROUP BY clause
* e.g. HAVING average >= 40 

* Other functions after HAVING are ORDER BY (for ordering, by field ASC or DESC) and LIMIT n (display at most n rows)

### Nested Queries
* Using queries within queries for advanced data selection
* Mostly used in WHERE or FROM
* Needs to output specfic table size in WHERE depending on where query:
	* WHERE field = value - sub-query needs to output a 1x1 value
	* WHERE field IN - sub-query needs to output exactly one column
	* WHERE EXISTS - **Correlated** sub-queries run once for each row of the input table 
* If using sub-query in FROM, need to name table after sub-query e.g. SELECT x, y FROM (...sub-query...) AS T ...

---

## Lecture 10 - Normalisation 1/2

### The problem
* Table with lots of repeated data
* For example Department/Faculty - will modern lang. ever not be in the arts faculty?
* Problem - what if we change the department but not the faculty?
* Repeated data - what if one piece is changed; all data would need to be changed in the table and if just one piece of data is left unchanged there is the problem of inconsistent data

### Normalisation
* Informal definition: a way to make your schema more complicated in order to make operations easier
* Eliminates redundancy and dependency

### Functional dependencies
* Suppose we want a database on the regions of England
* {city, c\_pop, region, r\_pop, country}

### Anomalies
* INSERT anomly - We can't have a city thats not in a region or a region with no cities
* UPDATE anomaly - if the population of a region updates, we have to update many rows
* DELTE anomaly - removing the last city in a region would erase the regions total population too

### Problems with the table
* Stores properties of two entities in the table; information about cities and regions are stored in the same row
* Repeated data

### Functional dependencies
* Definition - An attribute A **fucntionally depends on** a set of attributes  XS just if the value of A is uniquely determined after fixing values for XS
* We write the functional dependency as XS -> A

* XS -> YS iff For all A in YS, XS -> A
* So: XS is a superkey iff For all A of T,  XS -> A
* i.e all other attributes in the table are uniquely determined once the superkey is fixed

* A candidate key is a key where if you look at some subset of the attribute in the candidate key ther e is no functional dependency between that subset and the rest of the table

* {uname} -> fname
* {uname} -> lname
* {uname, fname} -> lname
* {lname} -> lname
* {fname} -/-> lname

### Laws of functional dependencies
* Reflexivity: {X} -> X
* Weakening: XS -> A so XS Union YS -> A
* Definition: A dependency XS -> A is said to be **trivial** just if A is in XS

* Resolution: {XS} -> Y<sub>i</sub> , {YS} -> A so {Y<sub>1</sub>..Y<sub>i-1</sub>, XS, Y<sub>i+1</sub>... Y<sub>k</sub>} -> A

### Keys from fundeps
* If we know the functional dependencies in a table we can figure out a key
* Apply the transitive properties of functional dependencies to find a minimal set of attributes that uniquely determine all other attributes in the table.

---

Lecture 12: JDBC: SQL in Java

Application uses a db api to communicate with the database. The api will be based on a database library and drivers which will be particular to the programming language used.

The Java datbase api is called JDBC. Its classes are in the java.sql and java.api packages.

Connection is managed using the java.sql.Connection and .DriverManager classes/ packages.

Connection c = DriverManager.getConnection("connectionstring"); c.close() -> make sure to close the connection, in a widely used database you dont want open connections causing network traffic and congestion.

When using SQL in Java you want to wrap the main function in a try-catch block for the conection as there are many ways it can fail. This allows you to catch the SQL exception.

Makes sense to prepare a SQL statement once with placeholders as takes time to prepare; dont want to do it multiple times as will be slow. PreparedStatement s = c.prepareStatement(....sql...); -> statement is a method of the connection object.

Definition: A transaction is a

Most DBMSs have a transaction manger that takes care of the ACID properties of a database. Atomicity, Consistency, Isolation, Durability. Using JDBC, every statement runs in its own transaction unless manual transaction management has been enabled (c.setAutoCommit(false)). commit calls the transaction. rollback if their is a fail and any changes na==made need to be undone (usually used in a try/ catch).

---

## Web Security 

### Security is hard
Kids Pass - Aug 2017
* change account no. in url - see someone elses account
* passwords stored in plaintext
* vulnerable to sql injection
* anecdotal evidence of admin password being adminadmin

Oil and gas international
* Notified of weak security
* Disagreed
* Posted to reddit, got hacked

### Rule \#1 of web security

All data coming from the client is assumed to be malicious util you have properly validated it.

### HTTP is stateless
* You send a request, you get a response, the connection ends
* If you send another request, unless you do something/ give the server something explicit to go on the server has no way of determining if you are the same person
* This is solved by cookies

### Cookies
* Browsers implement cookies to get state between requests
* If a response contains a cookie header then all further requests to the same server will include this header
* This can be used for features like preferences, remembering passwords etc (good)
* Can also be used for tracking (bad)

### Sessions
* Can be built on top of cookies
* If the server believes you are who you say you are, it will generate and store a long number (session token) that it will store in a database and give to you.
* Multiple HTTP requests will be able to confirm that the session is the same using the token.
* When you log out, the session token is deleted from the server session database, invalidating usage of that token in the future

### Session hijacking
* When someone tries to break into an account by stealing a copy of their session data
* Can be made harder by not using predictable session tokens

### Session tokens
* Use type 4 pseudorandom UUID
* cryptographically secure, 128~ bit security
* No user-related data in token

### What not to do
* Never include secret data in a cookuie like passwords or other data, even if encrypted

### Secure cookies
* HTTPOnly: Cookie not accessible to javascript; prtoects against scripting attacks
* Secure: cookie will only be sent for TLS encrypted requests
* These should both be set for cookies in production

### XSS
* XSS: Cross-Site Scripting - if your website displays user-generated content make sure it is validated; it could be malicious and scripted messages will be executed if not pattern-matched for scripting tags.
* Defemse 1: There are librarys to prevent this from happening
* Defense 2: HTTP security headers

### CSRF
* Cross-Site Request Forgery: You click a link on one site that causes an action on another site that you are logged in to
* Most of the reponsibilty lies with the target
* Defence 1: Check the referrer and origin HTTP headers - browsers only send origin for POST requests
* Defence 2: If necessary use per-request CSRF security tokens
* Defence 3: Re-authenticate before critical operations (e.g. changing passwords requires re-entry of password)
* Defence 4: For an API, you can add and require custom "X-" headers

### SOP and CORS
* Same Origin Policy - browsers only allow javascript requests to the same origin (protocol, host, port) as the source of the script.

* CORS - Cross Origin Resource Sharing
* To bypass SOP for legitmate resons, e.g hosting some open source data that other sites should have acess to, you need to allow that access through scripts. 
* Then you should set the header Access-Control-Allow-Origin: \* in the responses

### TLS
* Encrypting traffic with a key so that noone else can understand the data, even if they can see it. 
* Problem - the attacker could be on the path to the server i.i malicious router
* You may set up a secure connection with this bad guy who will be able to pass on the information you send to the actual site, decrypting your information and details in the process
* Solution - browsers have root certificates from CAs built in
* Servers buy certificates from CAs
* Certificates are authentication for the browser
* Let's encrypt by Mozilla is a free CA that should be good enough for most sites
* You should be using TLS if you have a domain of your own

### Password Storage
* Obviously never store passwords plaintext
* It is equally stupid to store passwords using your own encoding or encryption scheme - this will be very weak security
* Hashing is still not much better than nothing at all
* The most frequent hash will be 123456

### Theory: hash-salt-stretch
* Pick a random salt for each user and store a hash of the salt and password
* Then stretch the hash 

### Practice
* Use wg/scrypt passowrd storage method
* Uses a number of methods to reliably and with a certain level of security store passwords in a databse
* Methods to produce tag of a password to store and to check if password are correct.
Use a library - dont implement own crypto 
* Use library with one of the functions bcrypt, scrypt or argon2

### Password rules
* Only rule really needed is a minimum length of 8 characters
* Using a combination of non-letter characters and capitals does not contribute much to a passwords strength for the majority of the population
* NIST SP 800-63-3

---

## Java EE (Enterprise Edition)

### Maven
* Java can get very complicated with dependencies and packages and config files etc - maven (and ant) try to simplify this for you.
* Maven is a java build and dependency management system
* Number of operations - clean, compile, package - creates JAR or WAR archive

#### Artifacts
* In maven, everthing is an artifact and has a (groupID, artifactID, version) "primary key".

#### Dependency management
* artifacts have their primary key
* artifacts can be hosted in repositories
* artifacts can have dependencies, maven solves these

#### pom.xml
* Most important file
* project object mapping
* definition of project
* has "primary keys" - lets others use artifacts from your repository
* properties, dependencies, build -> plugins

### servlets

#### WAR	
* JAR - java archive
* WAR - web archive (jar with extra web-inf folder)
* java application servers (tomcat, jetty etc) and Google app engine run this
* a jee server can host multiple applications in different war files, with different context paths

* the premise of servlets is that you convert a standard java class by extending and overriding httpservlet.
* override the doGet and doPut etc methods
* remember to set content type
* Set headers first - setting after body output causes excpetions and errors


* Resources - you usually cannot read the file sustem from a web application
* The GAE way: place the files you want to be shared in src/main/resources and load the as a stream

### JSP, JSTL, EL
* companion to servlets, giving access to other outputs

#### Templating
* Idea: separate application logic and presentation
* Logic produces data, template displays it nicely
* Old JSP very poor, small and difficult to read tags, especially with large applications. Managing becomes very hard
* Did have some useful features such as java code between percent tags

* Set attributes to export the data to JSP templates.
* better way is to (in servlets doGet) put JSP templates in WEB-INF. forward the request to the template to render.

* EL - expression language
* JSTL - JSP standard tag library
* basic operations to make things easier
* e.g. if, choose, when, otherwise, for
* inside \<c: tag

* remember to always HTML-escape user generated data (for security)

* JSTL doesnt do anything that JSP doesnt do (does less) but very useful if you know what you are doing

### Google App Engine
* Use to deploy application to the cloud
