
# Table of Contents  
* **Introduction**
    * [Introduction](#introduction)
    * [Source Code](#source-code)
        * [HTML](#html)
        * [CSS](#css)
        * [Javascript](#javascript)
* **Obfuscation**
    * [Code Obfuscation](#code-obfuscation)
        * [Use Cases](#use-cases)
    * [Basic Obfuscation](#basic-obfuscation)
        * [Running JavaScript Code](#running-javascript-code)
        * [Minifying Javascript Code](#minifying-javascript-code)
        * [Packing JavaScript Code](#packing-javascript-code)
    * [Advanced Obfuscation](#advanced-obfuscation)
        * [Obfuscation](#obfuscator)
        * [More Obfuscator](#more-obfuscation)
    * **Deobfuscation**
        * [Beautify](#deobfuscation--beautify)
        * [Deobfuscate](#deobfuscation--deobfuscate)
        * [Reverse Engineering](#deobfuscation--reverse-engineering)         
* **Deobfuscation Examples**
* **Skills Assessment**

[Youtube : Do You Need Javascript Obfuscator? How to Deobfuscate JS?](https://youtu.be/s0GqIX5AA6Q?si=VbixGtxPBCQwN3yY)


# Introduction
* Import while conducting code analysis and reverse engineering.
* Important concepts :
    * Locating Javascript Code
    * Intro to Code Obfuscation
    * How to Deobfuscate JavaScript Code
    * How to decode encoded messages
    * Basic Code Analysis
    * Sending basic HTTP requests


# Source Code
* HTML = Website's main fields and parameters
* CSS = Design
* Javascript = Perform functions

> TIP : View Source Code [Ctrl + U]

## HTML
* The HTML source code can hold various information, such as comments, to further facilitate a better understanding of the code. Developers may leave sensitive information, which can be further taken advantage of. It is worth reading through the HTML comments.

## CSS
* Internally : Between `<style>` elements
    ```css
    <style>
            *,
            html {
                margin: 0;
                padding: 0;
                border: 0;
            }
        ...SNIP..
            h1 {
                font-size: 144px;
            }

            p {
                font-size: 64px;
            }
    </style>
    ```
* Externally : Seperate .css file and referenced
    ```css
    <head>
        <link rel="stylesheet" href="style.css">
    </head>
    ```

## Javascript
* Internally : Between `<script>` elements
    ```javascript
    <script>alert(document.cookie)</script>
    ```
* Externally : Seperate .js file and referenced
    ```javascript
    <script src ="secret.js"></script>
    ```


# Code Obfuscation
* Obfuscation is a technique used to make a script more difficult to read by humans but allows it to function the same from a technical point of view, though performance may be slower
* Achieved automatically by using an obfuscation tool
* Example : 
    * Code obfuscators often turn the code into a dictionary of all of the words and symbols used within the code and then attempt to rebuild the original code during execution by referring to each word and symbol from the dictionary.
    * [Tool: BeautifyTools - Javascript Obfuscator](https://beautifytools.com/javascript-obfuscator.php)
        ```javascript
        console.log("HTB Javascript Deobfuscation Module");
        ```

> * Important : Codes written in many languages are published and executed without being compiled in interpreted languages, such as Python, PHP, and JavaScript. While Python and PHP usually reside on the server-side and hence are hidden from end-users, JavaScript is usually used within browsers at the client-side, and the code is sent to the user and executed in cleartext. This is why obfuscation is very often used with JavaScript.

## Use Cases
* To hide the original code and its functions to prevent it from being reused or copied without the developer's permission.
* Provide a security layer when dealing with authentication or encryption to prevent attacks on vulnerabilities that may be found within the code.
* For malicious actions, obfuscate their malicious scripts to prevent Intrusion Detection and Prevention systems from detecting their scripts

# Basic Obfuscation
* Not done manually, as there are many tools for various languages that do automated code obfuscation.

> * Important : Many malicious actors and professional developers develop their own obfuscation tools to make it more difficult to deobfuscate

## Running JavaScript Code
* [Tool : JSConsole](https://jsconsole.com/)
```javascript
console.log('HTB JavaScript Deobfuscation Module');
```

## Minifying Javascript Code
* A common way of reducing the readability of a snippet of JavaScript code while keeping it fully functional is JavaScript minification.
* `Code minification` means having the entire code in a single (often very long) line. 
* `Code minification` is more useful for longer code, as if our code only consisted of a single line, it would not look much different when minified.

* [Tool: Javascript-minifier](https://www.toptal.com/developers/javascript-minifier)
```javascript
function log() {
    console.log('HTB JavaScript Deobfuscation Module');
}
```
* we can copy the minified code to JSConsole, and run it, and we see that it runs as expected.

> **Note:** 
> * Minified JavaScript code is saved with the extension `.min.js`. 
> * Code minification is not exclusive to JavaScript, and can be applied to many other languages, as can be seen on javascript-minifier.

## Packing JavaScript Code
1. Enter and obfuscate the js code in [BeautifyTools - Javascript Obfuscator](https://beautifytools.com/javascript-obfuscator.php)
2. Copy and paste the code in JSConsole and execute it.

> **Note:** 
> * The above type of obfuscation is known as "packing", which is usually recognizable from the six function arguments used in the initial function "function(p,a,c,k,e,d)".
> * A packer obfuscation tool usually attempts to convert all words and symbols of the code into a list or a dictionary and then refer to them using the (p,a,c,k,e,d) function to re-build the original code during execution. The (p,a,c,k,e,d) can be different from one packer to another. However, it usually contains a certain order in which the words and symbols of the original code were packed to know how to order them during execution.While a packer does a great job reducing the code's readability, we can still see its main strings written in cleartext, which may reveal some of its functionality. This is why we may want to look for better ways to obfuscate our code.


# Advanced Obfuscation
* Packer Obfuscation Drawback :  the code still contains strings in cleartext, which may reveal its original functionality.

## Obfuscator
* [Tool: Obfuscator.io](https://obfuscator.io/legacy-playground)
* Paste the js code and before we click obfuscate, we will change String Array Encoding to Base64.

> Note: Try playing with the obfuscation settings in https://obfuscator.io to generate even more obfuscated code, and then try rerunning it in https://jsconsole.com or browser console to verify it still performs its original function.

## More Obfuscation
```javascript
[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(!
...SNIP...
[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]](!+[]+!+[]+[+[]])))()
```
> * **Note:** The above code was snipped as the full code is too long, but the full code should successfully run.

* Tools:
1. [JSF](https://jsfuck.com/)
2. [JJ Encode](https://utf-8.jp/public/jjencode.html)
3. [AA Encode](https://utf-8.jp/public/aaencode.html)

> * **IMPORTANT :** Such obfuscators usually make code execution/compilation very slow, so it is not recommended to be used unless for an obvious reason, like bypassing web filters or restrictions.


# Deobfuscation : Beautify
* Minified javascript code can be beautified using Browser Debugger[Inspect -> Debugger -> Select .js file -> Click `{}` ]
* Tools : 
1. [Prettier](https://prettier.io/playground/)
2. [Beautifier](https://beautifier.io/)

# Deobfuscation : Deobfuscate
* Tool : [UnPacker](https://matthewfl.com/unPacker.html)

> * Tip : Ensure you do not leave any empty lines before the script, as it may affect the deobfuscation process and give inaccurate results.
> * Tip : Another way of unpacking such code is to find the return value at the end and use console.log to print it instead of executing it.

# Deobfuscation : Reverse Engineering
* Once the code becomes more obfuscated and encoded, it would become much more difficult for automated tools to clean it up. 
* This is especially true if the code was obfuscated using a custom obfuscation tool.
* We would need to manually reverse engineer the code to understand how it was obfuscated and its functionality for such cases

# Deobfuscation : Code Analysis
```javascript
function generateSerial()
	{
	var flag="HTB
		{
		1_4m_7h3_53r14l_g3n3r470r!
	}
	";
	var xhr=new XMLHttpRequest();
	var url="/serial.php";
	xhr.open("POST",url,true);
	xhr.send(null)
}
```
* Understand the Code Variables and Functions
* HTTP POST Request
```shell
curl -w "\n" -s -X POST http://STMIP:STMPO/serial.php
```
* Decode
> Check the hash/encoding using [MD5Hashing Hash Type Identifier](https://md5hashing.net/hash_type_checker) or [Boxebtriq - Cipher Identifier](https://www.boxentriq.com/analysis/cipher-identifier)
```shell
#Base64
echo 'HackThePlanet' | base64
echo aHR0cHM6Ly93d3cuaGFja3RoZWJveC5ldS8K | base64 -d

#Hex
echo https://www.hackthebox.eu/ | xxd -p
echo 68747470733a2f2f7777772e6861636b746865626f782e65752f0a | xxd -p -r

#[Caesar/Rot13](https://rot13.com/)
echo https://www.hackthebox.eu/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'
echo uggcf://jjj.unpxgurobk.rh/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

> * Note : Other than encoding, many obfuscation tools utilize encryption, which is encoding a string using a key, which may make the obfuscated code very difficult to reverse engineer and deobfuscate, especially if the decryption key is not stored within the script itself.


# Summary
The following is a summary of what we learned:
* First, we uncovered the HTML source code of the webpage and located the JavaScript code.
* Then, we learned about various ways to obfuscate JavaScript code.
* After that, we learned how to beautify and deobfuscate minified and obfuscated JavaScript code.
* Next, we went through the deobfuscated code and analyzed its main function
* We then learned about HTTP requests and were able to replicate the main function of the obfuscated JavaScript code.
* Finally, we learned about various methods to encode and decode strings.