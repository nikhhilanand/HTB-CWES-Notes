
## Table of Contents
* [HTTP](#hypertext-transfer-protocol-http)
* [URL](#url)
  * [HTTP Flow](#http-flow)
  * [cURL](#curl)
* [HTTPS](#hypertext-transfer-protocol-secure-https)
  * [HTTPS Flow](#https-flow)
  * [cURL for HTTPS](#curl-for-https)
* [HTTP Request & Response](#http-requests--responses)
  * [HTTP Request](#http-request)
  * [HTTP Response](#http-response)
  * [cURL](#request--response-headers---curl)
  * [Browser DevTools](#request--response-headers---browser-devtools)
* [HTTP Headers](#http-headers)
  * [Categories](#categories)
  * [General Headers](#general-headers)
  * [Entity headers](#entity-headers)
  * [Request Headers](#request-headers)
  * [Response Headers](#response-headers)
  * [Security Headers](#security-headers)
  * [cURL](#headers---curl)
  * [Browser DevTools](#headers---browser-devtools)
* [HTTP Methods & Codes](#http-methods--codes)
  * [Request Methods](#request-methods)
  * [Status Code](#status-codes)
* [HTTP Basic Auth](#http-basic-auth)
  * [Browser DevTools](#http-basic-auth---browser-devtools)
  * [cURL](#http-basic-auth---curl)
  * [Console Tab - JS Script](#http-basic-auth---console-tab-js-script)
* [HTTP Post](#http-post)
  * [Login Form - Browser DevTools](#login-forms---browser-devtools)
  * [Login Form - cURL](#login-forms---curl)
  * [Authenticated Cookie - Browser DevTools](#authenticated-cookie---browser-devtools)
  * [Authenticated Cookie - cURL](#authenticated-cookie---curl)
  * [JSON Data - Browser DevTools](#json-data---browser-devtools)
  * [JSON Data  - cURL](#json-data---curl)
  * [JSON Data  - Console Tab - JS Script](#json-data---console-tab-js-script)
* [CRUD API](#crud-api)
  * [Read - cURL](#read)
  * [Create](#create)
  * [Create - cURL](#create---curl)
  * [Create - Console Tab - JS Script](#create---console-tab-js-script)
  * [Update](#update)
  * [Update - cURL](#update---curl)
  * [Update - Console Tab - JS Script](#update---console-tab-js-script)
  * [Delete](#delete)
  * [Delete - cURL](#delete---curl)
  * [Delete - Console Tab - JS Script](#delete---console-tab-js-script)


# HyperText Transfer Protocol (HTTP)
* Application layer protocol
* Client (Requester) <-> Server (Provider)
* Default Port 80

> Fully Qualified Domain Name (FQDN) = Uniform Resource Locator (URL)

# URL
`http://admin:password@inflanefrieght.com:80/dashboard.php?login=true#status`

* Scheme = http://
* User Info = admin:password
* Host = inlanefrieght.com
* Port = :80
* Path = /dashboard.php
* Query String = ?login=true
* Fragements = #status

## HTTP Flow

Browser (Client) -> Request -> DNS [CNAME record to A Record] -> Server -> Response -> Browser (Client)

> Note: Our browsers usually first look up records in the local '/etc/hosts' file, and if the requested domain does not exist within it, then they would contact other DNS servers. We can use the '/etc/hosts' to manually add records to for DNS resolution, by adding the IP followed by the domain name.]

**HTTP Request**
```
GET / HTTP/1.1
Host: inlanefrieght.com
```
**HTTP Response**
```
HTTP/1.1 200 OK
```
* Browser sends a GET request to the default HTTP port (e.g. 80), asking for the root / path.
* By default, servers are configured to return an index file when a request for / is received.

## cURL
* [cURL](https://curl.se/) (client URL)
* Command-line tool supports HTTP & Other protocols
```shell
curl http://info.cern.ch
```
* Doesn't render but displays in raw format [HTML, JS, CSS]

```shell
curl -h
man curl
curl --help all
curl --help category

curl -h http
curl -s -O http://info.cern.ch/index.html
curl -o output http://info.cern.ch

# Display only Response Header & Not the response content
curl -I http://info.cern.ch/index.html
# Read the contents of the page
curl -w "\n" http://info.cern.ch/index.html
```

# HyperText Transfer Protocol Secure (HTTPS)
* [HTTPS](https://tools.ietf.org/html/rfc2660)
* Transferred in an encrypted format

> Note: Although the data transferred through the HTTPS protocol may be encrypted, the request may still reveal the visited URL if it contacted a clear-text DNS server. For this reason, it is recommended to utilize encrypted DNS servers (e.g. 8.8.8.8 or 1.1.1.1), or utilize a VPN service to ensure all traffic is properly encrypted.

## HTTPS Flow
* Browser -> GET / HTTP/1.1 [Port 80] -> Server
* Browser <- HTTP/1.1 301 Moved Permanently <- Server
* Browser -> Client Hello (Port 443) -> Server
* Browser <- Server Hello (Server Key Exchange) <- Server
* Browser -> Client Key Exchange (Encrpterd Handshake) -> Server
* Browser <- Encrpterd Handshake (Finished) <- Server
* Browser <-> Encrpterd HTTP Communication <-> Server

https://dev.to/nayetwolf/how-does-https-works-35mh

> Note: Depending on the circumstances, an attacker may be able to perform an HTTP downgrade attack, which downgrades HTTPS communication to HTTP, making the data transferred in clear-text. This is done by setting up a Man-In-The-Middle (MITM) proxy to transfer all traffic through the attacker's host without the user's knowledge. However, most modern browsers, servers, and web applications protect against this attack.

## cURL for HTTPS
*  If we ever contact a website with an invalid SSL certificate or an outdated one, then cURL by default would not proceed with the communication to protect against the earlier mentioned MITM attacks.
```shell
curl https://inlanefreight.com

# Skip certificate check
curl -k https://www.inlanefreight.com

```

# HTTP Requests & Responses
* [Request](#http-request) = Client -> Server
* [Response](#http-response) = Server -> Client

## HTTP Request
```
GET /users/login.html HTTP/1.1
Host: inlanefrieght.com
Cookie: PHPSESSID=c14ggtxxxxxxxxx
```
* HTTP Method = GET
* HTTP Path = /users/login.html
* HTTP Version = HTTP/1.1
* HTTP Headers = Host...... , Cookie.....
* Header Values = inlane..... , PHPSESS......

> Note: HTTP version 1.X sends requests as clear-text, and uses a new-line character to separate different fields and different requests. HTTP version 2.X, on the other hand, sends requests as binary data in a dictionary form.

## HTTP Response
```
HTTP/1.1 200 OK
Server: Apache/2.4.41 (Ubuntu)
Set-Cookie: PHPSESSID=m4uxxxxxxxxxxx
Connection: Close

<html .......>
```
* HTTP Version = HTTP/1.1
* Response Code = 200 (OK)
* Response Header = Server.... , Set-Cookie.... , Connection....
* Response Body = <html............

## Request & Response Headers - cURL
```shell
# Display the request & response using verbose flag
curl -v inlanefrieght.com
```

> The -vvv flag shows an even more verbose output. Which include hanshake and also SSL certificate details

## Request & Response Headers - Browser DevTools
* Chrome + Firefox = CTRL+SHIFT+I or F12
* Network Tab => Filter URLs

# HTTP Headers
* Multiple values
* Seperated by colon (;)
* [List of HTTP Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

## Categories
1. [General Headers](#general-headers)
2. [Entity Headers](#entity-headers)
3. [Request Headers](#request-headers)
4. [Response Headers](#response-headers)
5. [Security Headers](#security-headers)

### [General Headers](https://www.w3.org/Protocols/rfc2616/rfc2616-sec4.html)
* Used in both Request & Response
* Contextual & used to describe the message rather than its contents

| Header | Example | Description |
| :--- | :--- | :--- |
| Date: | Date: Wed, 16 Feb 2022 10:38:44 GMT | Holds the date and time at which the message originated. It's preferred to convert the time to the standard UTC time zone. |
| Connection | Connection: close | Dictates if the current network connection should stay alive after the request finishes. Two commonly used values for this header are close and keep-alive. The close value from either the client or server means that they would like to terminate the connection, while the keep-alive header indicates that the connection should remain open to receive more data and input. |

### [Entity Headers](https://www.w3.org/Protocols/rfc2616/rfc2616-sec7.html)
* Can be common to both the request and response
* Describe the content

| Header | Example | Description |
| :--- | :--- | :--- |
| Content-Type|Content-Type: text/html|Used to describe the type of resource being transferred. The value is automatically added by the browsers on the client-side and returned in the server response. The charset field denotes the encoding standard, such as UTF-8.|
|Media-Type|Media-Type: application/pdf|The media-type is similar to Content-Type, and describes the data being transferred. This header can play a crucial role in making the server interpret our input. The charset field may also be used with this header.|
|Boundary|boundary="b4e4fbd93540"|Acts as a marker to separate content when there is more than one in the same message. For example, within a form data, this boundary gets used as --b4e4fbd93540 to separate different parts of the form.|
|Content-Length|Content-Length: 385|Holds the size of the entity being passed. This header is necessary as the server uses it to read data from the message body, and is automatically generated by the browser and tools like cURL.|
|Content-Encoding|Content-Encoding: gzip|Data can undergo multiple transformations before being passed. For example, large amounts of data can be compressed to reduce the message size. The type of encoding being used should be specified using the Content-Encoding header.|

### [Request Headers](https://datatracker.ietf.org/doc/html/rfc2616)
* Used in an HTTP request and do not relate to the content
* [List of Request Headers](https://datatracker.ietf.org/doc/html/rfc7231#section-5)

| Header | Example | Description |
| :--- | :--- | :--- |
|Host|Host: www.inlanefreight.com|Used to specify the host being queried for the resource. This can be a domain name or an IP address. HTTP servers can be configured to host different websites, which are revealed based on the hostname. This makes the host header an important enumeration target, as it can indicate the existence of other hosts on the target server.|
|User-Agent|User-Agent: curl/7.77.0|The User-Agent header is used to describe the client requesting resources. This header can reveal a lot about the client, such as the browser, its version, and the operating system.|
|Referer|Referer: http://www.inlanefreight.com/|Denotes where the current request is coming from. For example, clicking a link from Google search results would make https://google.com the referer. Trusting this header can be dangerous as it can be easily manipulated, leading to unintended consequences.|
|Accept|Accept: */*|The Accept header describes which media types the client can understand. It can contain multiple media types separated by commas. The */* value signifies that all media types are accepted|
|Cookie|Cookie: PHPSESSID=b4e4fbd93540|Contains cookie-value pairs in the format name=value. A cookie is a piece of data stored on the client-side and on the server, which acts as an identifier. These are passed to the server per request, thus maintaining the client's access. Cookies can also serve other purposes, such as saving user preferences or session tracking. There can be multiple cookies in a single header separated by a semi-colon.|
|Authorization|Authorization: BASIC cGFzc3dvcmQK|Another method for the server to identify clients. After successful authentication, the server returns a token unique to the client. Unlike cookies, tokens are stored only on the client-side and retrieved by the server per request. There are multiple types of authentication types based on the webserver and application type used.|

### [Response Headers](https://datatracker.ietf.org/doc/html/rfc7231#section-7)
* Used in an HTTP response and do not relate to the content.
* Certain response headers such as Age, Location, and Server are used to provide more context about the response.

| Header | Example | Description |
| :--- | :--- | :--- |
|Server|Server: Apache/2.2.14 (Win32)|Contains information about the HTTP server, which processed the request. It can be used to gain information about the server, such as its version, and enumerate it further.|
|Set-Cookie|Set-Cookie: PHPSESSID=b4e4fbd93540|Contains the cookies needed for client identification. Browsers parse the cookies and store them for future requests. This header follows the same format as the Cookie request header.|
|WWW-Authenticate|WWW-Authenticate: BASIC realm="localhost"|Notifies the client about the type of authentication required to access the requested resource.|

### [Security Headers](https://owasp.org/www-project-secure-headers/)
* A class of response headers used to specify certain rules and policies.
| Header | Example | Description |
| :--- | :--- | :--- |
|Content-Security-Policy|Content-Security-Policy: script-src 'self'|Dictates the website's policy towards externally injected resources. This could be JavaScript code as well as script resources. This header instructs the browser to accept resources only from certain trusted domains, hence preventing attacks such as Cross-site scripting (XSS).|
|Strict-Transport-Security|Strict-Transport-Security: max-age=31536000|Prevents the browser from accessing the website over the plaintext HTTP protocol, and forces all communication to be carried over the secure HTTPS protocol. This prevents attackers from sniffing web traffic and accessing protected information such as passwords or other sensitive data.|
|Referrer-Policy|Referrer-Policy: origin|Dictates whether the browser should include the value specified via the Referer header or not. It can help in avoiding disclosing sensitive URLs and information while browsing the website.|

## Headers - cURL
```shell
# Displays the Response Headers using HTTP HEAD Method
curl -I https://www.inlanefreight.com

# Displays the Response Headers and its Contents
curl -i https://www.inlanefreight.com

# Adds custom Headers to Request
curl --header 'myHeader: HackThePlanet' https://www.inlanefreight.com -v

# Adds custom user-agent to Request
# References : https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/User-Agent
curl --user-agent 'Mozilla/5.0' https://www.inlanefreight.com -v
```

## Headers - Browser DevTools
* Network Tab -> Headers Tab

# HTTP Methods & Codes

## Request Methods
[List of HTTP Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)
| Method | Description |
| :--- | :--- |
|GET|Requests a specific resource. Additional data can be passed to the server via query strings in the URL (e.g. ?param=value).|
|POST|Sends data to the server. It can handle multiple types of input, such as text, PDFs, and other forms of binary data. This data is appended in the request body present after the headers. The POST method is commonly used when sending information (e.g. forms/logins) or uploading data to a website, such as images or documents.|
|HEAD|Requests the headers that would be returned if a GET request was made to the server. It doesn't return the request body and is usually made to check the response length before downloading resources.|
|PUT|Creates new resources on the server. Allowing this method without proper controls can lead to uploading malicious resources.|
|DELETE|Deletes an existing resource on the webserver. If not properly secured, can lead to Denial of Service (DoS) by deleting critical files on the web server.|
|OPTIONS|Returns information about the server, such as the methods accepted by it.|
|PATCH|Applies partial modifications to the resource at the specified location.|
|TRACE|Performs a message loop-back test along the path to the target.|

## Status Codes
* Provides status of their request
* [List of HTTP Status Code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status)
* [Cloudflare - List of HTTP Status Code](https://developers.cloudflare.com/support/troubleshooting/http-status-codes/)
* [AWS - List of HTTP Status Code](https://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/APIError.html)


|Class|Description|
| -- | -- |
|1XX|INFO - Provides information and does not affect the processing of the request.|
|2XX| SUCCESS - Returned when a request succeeds.|
|3XX| REDIRECT - Returned when the server redirects the client.|
|4XX| CLIENT - Signifies improper requests from the client. For example, requesting a resource that doesn't exist or requesting a bad format.|
|5XX| SERVER - Returned when there is some problem with the HTTP server itself.|

|Class|Description|
| -- | -- |
|200 OK|Returned on a successful request, and the response body usually contains the requested resource.|
|302 Found|Redirects the client to another URL. For example, redirecting the user to their dashboard after a successful login.|
|400 Bad Request|Returned on encountering malformed requests such as requests with missing line terminators.|
|403 Forbidden|Signifies that the client doesn't have appropriate access to the resource. It can also be returned when the server detects malicious input from the user.|
404 Not Found|Returned when the client requests a resource that doesn't exist on the server.|
500 Internal Server Error|Returned when the server cannot process the request.|

# HTTP Basic Auth
* Handled directly by the webserver to protect a specific page/directory, without directly interacting with the web application.
* HTTP Response = `Basic realm="Access denied"` in the `WWW-Authenticate` header, which confirms that this page indeed uses basic HTTP auth
* HTTP Response = `Basic realm="YWRtaW46YWRtaW4="` Base64 encoded value of admin:admin

## HTTP Basic Auth - Browser DevTools
* Copy as cURl (cURL command)
* Copy as Fetch (JS Console code)

## HTTP Basic Auth - cURL
```shell
curl -i http://server_ip:port

# Server user credentials
curl -u username:password http://server_ip:port -i -v
curl http://username:password@server_ip:port -i -v

# Adding the Authorization Header
curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/
```

> Note : If we were using a modern method of authentication (e.g. JWT), the Authorization would be of type Bearer and would contain a longer encrypted token

## HTTP Basic Auth - Console Tab [JS Script] 
```javascript
// Check the response in the Network Tab
fetch("http://154.57.164.77:32234/search.php", {
  "headers": {
    "accept": "*/*",
    "accept-language": "en,en-US;q=0.9,en-GB;q=0.8",
    "content-type": "application/json"
  },
  "referrer": "http://154.57.164.77:32234/index.php",
  "body": "{\"search\":\"London\"}",
  "method": "POST",
  "mode": "cors",
  "credentials": "include"
});
```

# HTTP POST 
* HTTP POST places user parameters within the HTTP Request body. [HTTP GET, which places user parameters within the URL]
* Main benefits:
    * Lack of Logging: As POST requests may transfer large files (e.g. file upload), it would not be efficient for the server to log all uploaded files as part of the requested URL, as would be the case with a file uploaded through a GET request.
    * Less Encoding Requirements: URLs are designed to be shared, which means they need to conform to characters that can be converted to letters. The POST request places data in the body which can accept binary data. The only characters that need to be encoded are those that are used to separate parameters.
    * More data can be sent: The maximum URL Length varies between browsers (Chrome/Firefox/IE), web servers (IIS, Apache, nginx), Content Delivery Networks (Fastly, Cloudfront, Cloudflare), and even URL Shorteners (bit.ly, amzn.to). Generally speaking, a URL's lengths should be kept to below 2,000 characters, and so they cannot handle a lot of data.

## Login Forms - Browser DevTools
* Copy as cURl (cURL command)

## Login Forms - cURL
```shell
# Login into application using POST HTTP Method and POST data
curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/

# Many login forms would redirect us to a different page once authenticated (e.g. /dashboard.php). If we want to follow the redirection with cURL, we can use the -L flag.
curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/ -L
```

## Authenticated Cookie - Browser DevTools
* Copy as cURl (cURL command)
* Storage Tab -> Cookies

## Authenticated Cookie - cURL
```shell
# Using authenticated Cookie PHPSESSION ID
curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/

# Specify Cookie as Header
curl -H 'Cookie: PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```

## JSON Data - Browser DevTools
* Copy > Copy Request Headers - Check the Content-Type
* Copy > Copy as cURL
* Copy > Copy as fetch [Console Tab - JS Script]

## JSON Data - cURL
```shell
curl -X POST -d '{"search":"london"}' -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php
```

## JSON Data - Console Tab [JS Script] 
```javascript
// Check the response in the Network Tab
fetch("http://154.57.164.77:32234/search.php", {
  "headers": {
    "accept": "*/*",
    "accept-language": "en,en-US;q=0.9,en-GB;q=0.8",
    "content-type": "application/json"
  },
  "referrer": "http://154.57.164.77:32234/index.php",
  "body": "{\"search\":\"London\"}",
  "method": "POST",
  "mode": "cors",
  "credentials": "include"
});
```

# CRUD API
* Same principle also used in REST and several other types of APIs.
* User access control will limit what actions we can perform and what results we can see

```shell
# Update City Table in Database using API
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london ...SNIP...
```

|Operation|HTTP Method|Description|
|--|--|--|
|Create|POST|Adds the specified data to the database table|
|Read|GET|Reads the specified entity from the database table|
|Update|PUT|Updates the data of the specified database table|
|Delete|DELETE|Removes the specified row from the database table|

## Read
```shell
# City = Table and London = Seacrh Term
curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq

curl -s http://<SERVER_IP>:<PORT>/api.php/city/le | jq

# Pass empty string in search term
curl -s http://<SERVER_IP>:<PORT>/api.php/city/ | jq
```

## Create
* POST JSON Data
* Content-Type: application/json

### Create - cURL
```shell
# Add an entry in the city table
curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'

# Validate the newly added city
curl -s http://<SERVER_IP>:<PORT>/api.php/city/HTB_City | jq
```

### Create - Console Tab [JS Script] 
```javascript
fetch("http://154.57.164.82:32257/api.php/city/", {
  "headers": {
    "accept": "*/*",
    "accept-language": "en,en-US;q=0.9,en-GB;q=0.8",
    "content-type": "application/json"
  },
  "referrer": "http://154.57.164.82:32257/index.php",
  "body": "{\"city_name\":\"HackThePlanet\",\"country_name\":\"HTB\"}",
  "method": "POST",
  "mode": "cors",
  "credentials": "include"
});
```

## Update
* PUT is used to update API entries and modify their details.
* The HTTP PATCH method may also be used to update API entries instead of PUT
* To be precise, PATCH is used to partially update an entry (only modify some of its data "e.g. only city_name"), while PUT is used to update the entire entry. 
* We may also use the HTTP OPTIONS method to see which of the two is accepted by the server, and then use the appropriate method accordingly.

### Update - cURL
```shell
# Update the city name from london to New_HTB_City
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'

# Validate the newly added city
curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq
```

### Update - Console Tab [JS Script] 
```javascript
fetch("http://154.57.164.82:32257/api.php/city/london", {
  "headers": {
    "accept": "*/*",
    "accept-language": "en,en-US;q=0.9,en-GB;q=0.8",
    "content-type": "application/json"
  },
  "referrer": "http://154.57.164.82:32257/index.php",
  "body": "{\"city_name\":\"New_HTB_City\",\"country_name\":\"HTB\"}",
  "method": "PUT",
  "mode": "cors",
  "credentials": "include"
});
```

> Note: In some APIs, the Update operation may be used to create new entries as well. Basically, we would send our data, and if it does not exist, it would create it.

## Delete
* DELETE is used to remove a specific entity

### Delete - cURL
```shell
# Delete the city
curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City -i

# Validate the newly added city
curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq
```

### Delete - Console Tab [JS Script] 
```javascript
fetch("http://154.57.164.82:32257/api.php/city/hacker", {
  "headers": {
    "accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7",
    "accept-language": "en-US,en;q=0.9",
    "upgrade-insecure-requests": "1"
  },
  "body": null,
  "method": "DELETE",
  "mode": "cors",
  "credentials": "omit"
});
```