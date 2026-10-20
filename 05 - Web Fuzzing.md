# Table of Contents  
* **Introduction**
    * [Fuzzing vs. Brute-forcing](#fuzzing-vs-brute-forcing)
    * [Why Fuzzing ?](#why-fuzzing-)
    * [Essential Concepts](#essential-concepts)
    * [Tooling : Installing Go, Python and PIPX](#tooling--installing-go-python-and-pipx)
    * [Tooling : FFuf](#tooling--ffuf)
        * [FFuf - Use Cases](#ffuf---use-cases)
    * [Tooling : Gobuster](#tooling--gobuster)
        * [Gobuster - Use Cases](#gobuster---use-cases)
    * [Tooling : FeroxBuster](#tooling--feroxbuster)
        * [FeroxBuster - Use Cases](#feroxbuster---use-cases)
    * [Tooling : wfuzz/wenum](#tooling--wfuzzwenum)
        * [wfuzz/wenum - Use Cases](#wfuzzwenum---use-cases)
* **Directory & File Fuzzing**
    * [Uncovering Hidden Assets](#uncovering-hidden-assets)
    * [Wordlists](#wordlists)
    * **Actual Fuzzing**
        * [Directory Fuzzing : ffuf](#directory-fuzzing--ffuf)
        * [File Fuzzing : ffuf](#file-fuzzing--ffuf)
* **Parameter & Value Fuzzing**
    * [GET Parameters](#get-parameters)
    * [POST Parameters](#post-parameters)
    * [GET Parameters Tool : wfuzz / wenum](#get-parameters-tool--wfuzz--wenum)
    * [POST Parameters Tool : ffuzz](#post-parameters-tool--ffuzz)
* [Virtual Host & Subdomain Fuzzing](#virtual-host-and-subdomain-fuzzing)
    * [Gobuster VHost Fuzzing](#gobuster-vhost-fuzzing)
    * [Gobuster Subdomain Fuzzing](#gobuster-subdomain-fuzzing)
* **Filtering Fuzzing Output**
* **Validate Findings**
    * [Why Validate Findings](#why-validate-findings)
    * [Validate Findings : Manual Verification](#validate-findings--manual-verification)
* [Web APIs](#web-apis)
    * [REST](#rest)
    * [SOAP](#soap)
    * [GraphQL](#graphql)
    * [Identifying Endpoints : REST](#identifying-endpoints--rest)
        * [Discovering REST Endpoints and Parameters](#discovering-rest-endpoints-and-parameters)
    * [Identifying Endpoints : SOAP](#identifying-endpoints--soap)
        * [Discovering SOAP Endpoints & Parameters](#discovering-soap-endpoints--parameters)
    * [Identifying GraphQL API Endpoints & Parameters](#identifying-graphql-api-endpoints--parameters)
        * [GraphQL Queries](#graphql-queries)
        * [GraphQl Mutation](#graphql-mutation)
        * [Discovering Queries & Mutation](#discovering-queries--mutation)
    * [API Fuzzing](#api-fuzzing)
        * [Why Fuzz APIs](#why-fuzz-apis-)
        * [Types of API Fuzzing](#types-of-api-fuzzing)
        * [Exploring the API](#exploring-the-api)
        * [Fuzzing the API](#fuzzing-the-api)


# Fuzzing vs. Brute-forcing
* Fuzzing 
    * Fuzzing involves feeding the web application with unexpected inputs, including malformed data, invalid characters, and nonsensical combinations. 
    * Wordlists contains common patterns, mutations of existing parameters, or even random character sequences to generate a diverse set of payloads
* Brute-forcing
    * Targeted approach
    * Predefined lists or dictionaries (like password dictionaries) to guess the correct value through trial and error

# Why Fuzzing ?
* Uncovering Hidden Vulnerabilities
* Automating Security Testing
* Simulating Real-World Attacks
* Strengthening Input Validation
* Improving Code Quality
* Continuous Security

# Essential Concepts

|Concept	|Description	|Example|
|--|--|--|
|Wordlist	|A dictionary or list of words, phrases, file names, directory names, or parameter values used as input during fuzzing.	|Generic: admin, login, password, backup, config <br> Application-specific: productID, addToCart, checkout|
|Payload	|The actual data sent to the web application during fuzzing. Can be a simple string, numerical value, or complex data structure.	|' OR 1=1 -- (for SQL injection)|
|Response Analysis	|Examining the web application's responses (e.g., response codes, error messages) to the fuzzer's payloads to identify anomalies that might indicate vulnerabilities.	|Normal: 200 OK <br> Error (potential SQLi): 500 Internal Server Error with a database error message|
|Fuzzer	|A software tool that automates generating and sending payloads to a web application and analyzing the responses.	|ffuf, wfuzz, Burp Suite Intruder|
|False Positive	|A result that is incorrectly identified as a vulnerability by the fuzzer.	|A 404 Not Found error for a non-existent directory.|
|False Negative	|A vulnerability that exists in the web application but is not detected by the fuzzer.	|A subtle logic flaw in a payment processing function.|
|Fuzzing Scope	|The specific parts of the web application that you are targeting with your fuzzing efforts.	|Only fuzzing the login page or focusing on a particular API endpoint.|

# Tooling : Installing Go, Python and PIPX
```shell
apt update

#Install go language
apt install -y golang

#Install Python
apt install -y python3 python3-pip

#Install & Configure pipx
apt install pipx
pipx ensurepath
pipx ensurepath --global

# Check versions
go version
python3 --version
```

# Tooling : FFuf
* Fuzz Faster U Fool
* Go Language
```shell
go install github.com/ffuf/ffuf/v2@latest
```

## FFuf - Use Cases
* Enumerating directories, files, and parameters
|Use Case	|Description|
|--|--|
|Directory and File Enumeration	|Quickly identify hidden directories and files on a web server.|
|Parameter Discovery |Find and test parameters within web applications.|
|Brute-Force Attack	|Perform brute-force attacks to discover login credentials or other sensitive information.|

# Tooling : Gobuster
* Web directory and file fuzzer
```shell
apt install github.com/OJ/gobuster/v3@latest
```

## Gobuster - Use Cases
|Use Case	|Description|
|Content Discovery	|Quickly scan and find hidden web content such as directories, files, and virtual hosts.|
DNS Subdomain Enumeration	Identify subdomains of a target domain.
|WordPress Content Detection	|Use specific wordlists to find WordPress-related content.|

# Tooling : FeroxBuster
* fast, recursive content discovery tool - discovery of unlinked content in web applications & for identifying hidden directories and files {Forced Browsing}
* Rust Laanguage
```shell
curl -sL https://raw.githubusercontent.com/epi052/feroxbuster/main/install-nix.sh | sudo bash -s $HOME/.local/bin
```

## FeroxBuster - Use Cases
|Use Case	|Description|
|--|--|
|Recursive Scanning	|Perform recursive scans to discover nested directories and files.|
|Unlinked Content Discovery	|Identify content that is not linked within the web application.|
|High-Performance Scans	|Benefit from Rust's performance to conduct high-speed content discovery.|

# Tooling : wfuzz/wenum
* Particularly well-suited for parameter fuzzing
```shell
pipx install git+https://github.com/WebFuzzForge/wenum
pipx runpip wenum install setuptools
```

## wfuzz/wenum - Use Cases
|Use Case	|Description|
|--|--|
|Directory and File Enumeration	|Quickly identify hidden directories and files on a web server.|
|Parameter Discovery	|Find and test parameters within web applications.|
|Brute-Force Attack	|Perform brute-force attacks to discover login credentials or other sensitive information.|


# Uncovering Hidden Assets
[Directories, Files and Endpoints]
* Sensitive data
* Outdated content
* Development resources
* Hidden functionalities

# Wordlists
* [SecLists](https://github.com/danielmiessler/SecLists)
* SecLists contains wordlists for:
    * Common directory and file names
    * Backup files
    * Configuration files
    * Vulnerable scripts
    * And much more
* Commonly used wordlists for web directories & files:
    * Discovery/Web-Content/common.txt
    * Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt
    * Discovery/Web-Content/raft-large-directories.txt
    * Discovery/Web-Content/big.txt

# Directory Fuzzing : ffuf
```shell
# Recursive fuzzing
ffuf -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ -recursion -t 80
```

# File Fuzzing : ffuf
* Common Extensions:
    * `.php`: Files containing PHP code, a popular server-side scripting language.
    * `.html`: Files that define the structure and content of web pages.
    * `.txt`: Plain text files, often storing simple information or logs.
    * `.bak`: Backup files are created to preserve previous versions of files in case of errors or modifications.
    * `.js`: Files containing JavaScript code add interactivity and dynamic functionality to web pages.

```shell
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://IP:PORT/direcotry/FUZZ -recursion -e .php,.html,.txt,.bak,.js -v
```

> TIP: Be Responsible while recursion
> ```shell
> ffuf -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ -e .php,.html,.txt,.bak,.js -ic -recursion -recursion-depth 2 -rate 500 -timeout 80
> ```
ffuf -u http://154.57.164.77:32717/recursive_fuzz/FUZZ -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -recursion -e .php,.txt,.html -ic -t 80


# GET Parameters
```http
https://example.com/search?query=fuzzing&category=security
```
* query is a parameter with the value "fuzzing"
* category is another parameter with the value "security"

# POST Parameters
```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=your_username&password=your_password
```
> * Note : Encoding
> * `application/x-www-form-urlencoded`: This format encodes the data as key-value pairs separated by ampersands (&), similar to GET parameters but placed within the request body instead of the URL.
> * `multipart/form-data`: This format is used when submitting files along with other data. It divides the request body into multiple parts, each containing a specific piece of data or a file.

# GET Parameters Tool : wfuzz / wenum
```shell
# Understanding the application & it's response
curl http://IP:PORT/get.php
curl http://IP:PORT/get.php?x=1

# Install wenum
pipx install git+https://github.com/WebFuzzForge/wenum
pipx runpip wenum install setuptools

wenum -w /usr/share/seclists/Discovery/Web-Content/common.txt --hc 404 -u "http://IP:PORT/get.php?x=FUZZ"

# Alternative Tool : ffuf
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://IP:PORT/get.php?x=FUZZ -v
```

# POST Parameters Tool : ffuzz
```shell
# Understanding the application & it's response
curl http://IP:PORT/post.php
curl -d "" http://IP:PORT/post.php
curl -d "y=1" http://IP:PORT/post.php

ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v -t 80
```

> * Note : 
> * Content-Type header in POST request is very important. Without ot won't find the flag.


# Virtual Host and Subdomain Fuzzing
* vhost enables multiple websites or domains to be served from a single server or IP address.
* Subdomains are resolved to specific IP addresses through DNS records.

|Feature	|Virtual Hosts	|Subdomains|
|--|--|--|
|Identification	|Identified by the Host header in HTTP requests.	|Identified by DNS records, pointing to specific IP addresses.|
|Purpose	|Primarily used to host multiple websites on a single server.	|Used to organize different sections or services within a website.|
|Security Risks	|Misconfigured vhosts can expose internal applications or sensitive data.	|Subdomain takeover vulnerabilities can occur if DNS records are mismanaged.|

# Gobuster VHost Fuzzing
```shell
# Add the vhost to your local hosts file
echo "IP inlanefreight.htb" | tee -a /etc/hosts

gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain --exclude-status 400

#Using Another Tool
ffuf -u http://fuzzing_fun.htb:33679 -w /usr/share/seclists/Discovery/Web-Content/common.txt -H 'Host: FUZZ.fuzzing_fun.htb:33679' -ac
```

# Gobuster Subdomain Fuzzing
```shell
gobuster dns --domain inlanefreight.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt 
```


# Why Validate Findings
* Confirming Vulnerabilities
* Understanding Impact
* Reproducing the Issue
* Gather Evidence

# Validate Findings : Manual Verification
1. Reproducing the Request [Using curl or Web Browser]
2. Analyzing the Response
3. Exploitation


# Web APIs
* Allowing diverse software components to exchange data and services seamlessly
* A Web API serves as a bridge between a server (hosting the data and functionality) and a client (such as a web browser, mobile app, or another server) that wants to access or utilize that data or functionality. 

## REST
* Representational State Transfer
* Use a stateless, client-server communication model 
* REST APIs utilize standard HTTP methods (GET, POST, PUT, DELETE) to perform CRUD (Create, Read, Update, Delete) operations on resources identified by unique URLs
* Typically exchange data in lightweight formats like JSON or XML
```shell
GET /users/123
```

## SOAP
* Simple Object Access Protocol
* More formal and standardized protocol for exchanging structured information
* Use XML to define messages, which are then encapsulated in SOAP envelopes and transmitted over network protocols like HTTP or SMTP
* SOAP APIs often include built-in security, reliability, and transaction management features, making them suitable for enterprise-level applications requiring strict data integrity and error handling.
```xml
 <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
   <soapenv:Header/>
   <soapenv:Body>
      <tem:GetStockPrice>
         <tem:StockName>AAPL</tem:StockName>
      </tem:GetStockPrice>
   </soapenv:Body>
</soapenv:Envelope>
```

## GraphQL
* A relatively new query language and runtime for APIs
* Uses a single endpoint where clients can request the data they need using a flexible query language.
* GraphQL's strong typing and introspection capabilities make it easier to evolve APIs over time without breaking existing clients
```graphql
query {
  user(id: 123) {
    name
    email
  }
}
```

# Identifying Endpoints : REST
```
/products - Represents a collection of product resources.
/products/456 - Represents a specific product with the ID 456.
```

|Parameter Type	|Description	|Example|
|--|--|--|
|Query Parameters	|Appended to the endpoint URL after a question mark (?). Used for filtering, sorting, or pagination.	|/users?limit=10&sort=name|
|Path Parameters	|Embedded directly within the endpoint URL. Used to identify specific resources.	|/products/{id}pen_spark|
|Request Body Parameters	|Sent in the body of POST, PUT, or PATCH requests. Used to create or update resources.	|{ "name": "New Product", "price": 99.99 }|

## Discovering REST Endpoints and Parameters
1. API Documentation : Swagger (OpenAPI) or RAML
2. Network Traffic Analysis : BurpSuite or Web Browser
3. Parameter Name Fuzzing : ffuf or wfuzz


# Identifying Endpoints : SOAP
* Rely on XML-based messages and Web Services Description Language (WSDL) files to define their interfaces and operations.
* SOAP APIs typically expose a single endpoint. This endpoint is a URL where the SOAP server listens for incoming requests.
* The content of the SOAP message itself determines the specific operation you want to perform.
* The parameters are defined in the Web Services Description Language (WSDL) file, an XML-based document that describes the web service's interface, operations, and message formats
* Example : The WSDL file might define an operation called SearchBooks with the following input parameters
    * keywords (string): The search terms to use.
    * author (string): The name of the author (optional).
    * genre (string): The genre of the book (optional).
A sample SOAP request to this API might look like this:
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:lib="http://example.com/library">
   <soapenv:Header/>
   <soapenv:Body>
      <lib:SearchBooks>
         <lib:keywords>cybersecurity</lib:keywords>
         <lib:author>Dan Kaminsky</lib:author>
      </lib:SearchBooks>
   </soapenv:Body>
</soapenv:Envelope>
```

## Discovering SOAP Endpoints & Parameters
1. WSDL Analysis : You can analyze the WSDL file manually or use tools designed to parse and visualize WSDL structures.
2. Network Traffic Analysis : Wireshark, tcpdump
3. Fuzzing for Parameter Names and Values : send malformed or unexpected values

# Identifying GraphQL API Endpoints & Parameters
* Endpoint is usually a URL like /graphql and serves as the entry point for all queries and mutations sent to the API.

## GraphQL Queries
|Component	|Description	|Example|
|--|--|--|
|Field	|Represents a specific piece of data you want to retrieve (e.g., name, email).	|name, email|
|Relationship	|Indicates a connection between different types of data (e.g., a user's posts).	|posts|
|Nested Object	|A field that returns another object, allowing you to traverse deeper into the data graph.	|posts { title, body }|
|Argument	|Modifies the behavior of a query or field (e.g., filtering, sorting, pagination).	|posts(limit: 5) (retrieves the first 5 posts of a user)|

```graphql
query {
  user(id: 123) {
    name
    email
    posts(limit: 5) {
      title
      body
    }
  }
}
```

## GraphQl Mutation
* Mutations are the counterparts to queries designed to modify data on the server.

|Component	|Description	|Example|
|--|--|--|
|Operation	|The action to perform (e.g., createPost, updateUser, deleteComment).	|createPost|
|Argument	|Input data required for the operation (e.g., title and body for a new post).	|title: "New Post", body: "This is the content of the new post"|
|Selection	|Fields you want to retrieve in the response after the mutation completes (e.g., id, title of new post).	|id, title|

```graphql
mutation {
  createPost(title: "New Post", body: "This is the content of the new post") {
    id
    title
  }
}
```
> This mutation creates a new post with the specified title and body, returning the id and title of the newly created post in the response.

## Discovering Queries & Mutation
1. Introspection
2. API Documentation
3. Network Traffic Analysis


# API Fuzzing
* Altering parameter values
* Modifying request headers
* Changing the order of parameters
* Introducing unexpected data types or formats

# Why Fuzz APIs ?
* Uncovering Hidden Vulnerabilities
* Testing Robustness
* Automating Security Testing
* Simulating Real-World Attacks

# Types of API Fuzzing
1. Parameter Fuzzing 
2. Data Format Fuzzing
3. Sequence Fuzzing : sequences of requests

# Exploring the API
* Understand the functionality of the A˜PIs by accessing documentation.

# Fuzzing the API 
```shell
# Installing the api fuzzer
git clone https://github.com/PandaSt0rm/webfuzz_api.git
cd webfuzz_api
pip3 install -r requirements.txt

# Executing the API fuzzer and Dont display output containing 404 status code
python3 api_fuzzer.py http://IP:PORT | grep -vi "404"

# Validate the undocumented or hidden endpoint
curl http://localhost:8000/czcmdcvt
```
