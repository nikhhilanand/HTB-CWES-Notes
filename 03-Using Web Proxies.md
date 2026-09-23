
## Table of Contents
* **Web Proxy**
    * [What are Web Proxies?](#what-are-web-proxies-)
    * [Uses of Web Proxies](#uses-of-web-proxies)
    * [Tool: Burp Suite](#tool-burp-suite)
    * [Tool: OWASP ZAP](#tool-owasp-zed-attack-proxy-zap)
    * [Proxy Setup](#proxy-setup)
    * [Intercepting Web Requests](#intercepting-web-request)
    * [Intercepting Web Responses](#intercepting-web-responses)
    * [Automatic Modification](#automatic-modification)
    * [Proxy History](#proxy-history)
    * [Repeating Requests](#repeating-requests)
    * [URL Encoding](#url-encoding)
    * [Decoding](#decoding)
    * [Encoding](#encoding)
    * [Proxying Tools : ProxyChains](#proxying-tools--proxychains)
        * [Usage](#usage)
    * [Proxying Tools : Metasploit](#proxying-tools--metasploit)         
* **Web Fuzzer**
    * [Burp Intruder](#burp-intruder)
        * [Positions](#positions)
        * [Payloads](#payloads)
            * Payload Position & Payload Type
            * Payload Configuration
            * Payload Processing
            * Payload Encoding
        * [Settings](#settings)
        * [References](#references-)
    * [ZAP Fuzzer](#zap-fuzzer)
* **Web Scanner**
    * [Burp Scanner](#burp-scanner)
        * [Target Scope](#target-scope)
        * [Crawler](#crawler)
        * [Passive Scanner](#passive-scanner)
        * [Active Scanner](#active-scanner)
        * [Scan Configuration](#scan-config)
        * [Reporting](#reporting)
    * [ZAP Scanner](#zap-scanner)
        * [ZAP Spider](#zap-spider)
        * [ZAP Passive Scanner](#zap-passive-scanner)
        * [ZAP Active Scanner](#zap-active-scanner)
        * [ZAP Reporting](#zap-reporting)
* **Extensions**
    * [Burp BApp Store](#burp-bapp-store)
    * [ZAP Marketplace](#zap-marketplace)


</br>
</br>

# What are Web Proxies ?
* Network Sniffer (wireshark, tcap) = Web Sniffer (Burpsuite or ZAP)
* Tool that sits between client and server. Similar to man-in-the-middle (MITM) attack.
* Captures request and response.
* Mainly works on Port 80 (http) and Port 443 (https)


# Uses of Web Proxies
* Web application vulnerability scanning
* Web fuzzing
* Web crawling
* Web application mapping
* Web request analysis
* Web configuration testing
* Code reviews


# Tool: [Burp Suite](https://portswigger.net/burp)
* Versions [Community, Professional,  DAST and AT(Agentic AI extends human-led pentesting)]



```shell
# Requires [Java Runtime Environment (JRE)](https://docs.oracle.com/goldengate/1212/gg-winux/GDRAD/java.htm#BGBFJHAB) to execute Burp.
java -jar <burpsuite.jar>
```

> Notes: 
> * If you prefer to use to a dark theme, you may do so in Burp by going to (Burp>Settings>User interface>Display) and selecting "dark" under (theme).
> * DAST is used for DevSecOps projects
> * If you have an educational or business email address, then you can apply for a free trial of Burp Pro at this [link](https://portswigger.net/burp/pro/trial) to be able to follow along with some of the Burp Pro only features showcased later in this module.


# Tool: [OWASP Zed Attack Proxy (ZAP)](https://www.zaproxy.org/)
* Free & Open-source Project by [Open Web Application Security Project (OWASP)](https://owasp.org/)

```shell
# Can download cross-platform JAR file
java -jar <zapproxy.jar>
```

> * TIP: If you prefer to use to a dark theme, you may do so in ZAP by going to (Tools>Options>Display) and selecting "Flat Dark" in (Look and Feel).


# Proxy Setup
1. Pre-Configured Browser
    * Burp - Burp Chromium Browser
    * ZAP - Mozilla Firefox
2. Browser (Firefox = Best for pentesting)
    * about:preferences#General -> Network Settings
    * Plugins [FoxyProxy Standard](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/)

> * Note: In case we wanted to serve the web proxy on a different port, we can do that in Burp under (Proxy>Proxy settings>Proxy listeners), or in ZAP under (Tools>Options>Network>Local Servers/Proxies). In both cases, we must ensure that the proxy configured in Firefox uses the same port.
> * ZAP Interception On (Ctrl + B)
3. Installing CA Certificate
    * Burp = http://burp -> CA Certificate
    * ZAP = Tools -> Options -> Network -> Server Certificates -> Generate / Save
    * Import in Firefox Browser [about:preferences#privacy ->View Certificates -> Authorities -> Import]

# Intercepting Web Request
1. Burp Suite
2. ZAP & Head's Up Display (HUD)


# Intercepting Web Responses
* Useful when like enabling disabled fields or showing certain hidden fields
1. Burp (Settings -> Tools -> Proxy -> Response Interception rules -> Enable Intercept Responses based on the following rules)
2. ZAP (Hidden Fields)


# Automatic Modification
* Burp
    * Automatic Request Modification = Burp Match and Replace [Proxy -> HTTP match and Replace Rules]
    * Automatic Response Modification
* ZAP
    * ZAP Replacer [Tools -> Replacer Options -> Replacer (Ctrl + R)]


# Proxy History
* Burp [Proxy -> HTTP History]
* ZAP [History]
> * Tip: While ZAP only shows the final/modified request that was sent, Burp provides the ability to examine both the original request and the modified request. If a request was edited, the pane header would say Original Request, and we can click on it and select Edited Request to examine the final request that was sent.


# Repeating Requests
[Burp Repeater, ZAP Request Editor & ZAP HUD]
* Burp [Repeater (Ctrl + R)]
* ZAP [Right-click on HTTP Request and select Open/Resend with Request Editor]


# URL Encoding
* Some of the key characters we need to encode are:
    * Spaces: May indicate the end of request data if not encoded
    * `&`: Otherwise interpreted as a parameter delimiter
    * `#`: Otherwise interpreted as a fragment identifier
* Tools
    * Burp [Right Click -> Convert Selection -> URL -> URL-encode key characters (Ctrl + U)]
    * ZAP [ZAP should automatically URL-encode all of our request data in the background before sending the request, though we may not see that explicitly.]
* There are other types of URL-encoding, like Full URL-Encoding or Unicode URL encoding, which may also be helpful for requests with many special characters.


# Decoding
* Some of the other types of encoders:
    * HTML
    * Unicode
    * Base64
    * ASCII hex
* Tools
    * Burp [Decoder -> Decode as ...]
    * ZAP [Right Click -> Encode/Decode/Hash (Ctrl + E)]
        > * Tip: We can create customized tabs in ZAP's Encoder/Decoder/Hash with the "Add New Tab" button, and then we can add any type of encoder/decoder we want the text to be shown in. Try to create your own tab with a few encoders/decoders.

> * Formats:
>> * xxxxx== : Base64
>> * 34%7d : URL

# Encoding
* Tools
    * Burp [Decoder -> Encode as ....]
    * ZAP [Right Click -> Encode/Decode/Hash (Ctrl + E)]
> * Tip: Burp Decoder output can be directly encoded/decoded with a different encoder. Select the new encoder method in the output pane at the bottom, and it will be encoded/decoded again. In ZAP, we can copy the output text and paste it into the input field above.    


# Proxying Tools : [ProxyChains](https://github.com/haad/proxychains)
* Useful tool in linux
* Routes all traffic coming from any command-line tool to any proxy we specify

## Usage
* First configure `/etc/proxychains.conf`
    ```shell
    #comment the final line
    #socks4         127.0.0.1 9050
    http 127.0.0.1 8080
    ```
* Using tool with cURL
    ```shell
    proxychains -q curl http://SERVER_IP:PORT
    ```
* Check the Burp proxy and the request in intercepted


# Proxying Tools : Metasploit
* Let's try to proxy web traffic made by Metasploit modules to better investigate and debug them.
    ```shell
    msfconsole

    use auxiliary/scanner/http/robots_txt
    set PROXIES HTTP:127.0.0.1:8080
    set RHOST SERVER_IP
    set RPORT PORT

    run
    ```
* Check the Burp proxy and the request in intercepted


# Burp Intruder
> * Web fuzzers are powerful tools that act as web fuzzing, enumeration, and brute-forcing tools.
> * CLI-based fuzzers we use, like ffuf, dirbuster, gobuster, wfuzz, etc.

* Burp Intruder, and can be used to fuzz pages, directories, sub-domains, parameters, parameters values, and many other things.
* We can even use Intruder to perform password spraying against applications that use Active Directory (AD) authentication, such as Outlook Web Access (OWA), SSL VPN portals, Remote Desktop Services (RDS), Citrix, custom web applications that use AD authentication, and more. 
* Community = 1 request /second ; CLI Tools = 10k request / second
* Proxy History -> Right Click on Request -> Send to Intruder (Ctrl + I)
* Intruder Tab (Ctrl + Shift + I)

## Positions
* Payload Position = By either wrapping it with § or by selecting the word and clicking on the Add § button

> * Note: Be sure to leave the extra two lines at the end of the request, otherwise we may get an error response from the server.

## Payloads
* 4 main configs:
    * Payload Position & Payload Type
    * Payload Configuration
    * Payload Processing
    * Payload Encoding

* Various Attack Types :
    1. Sniper = Places each payload from a single set into each defined payload position one at a time (in turn)
    2. Battering Ram = Places the exact same payload from a single set into all defined payload positions at the same time (simultaneously).
    3. Pitchfork = Uses a separate payload set for each position and cycles through them simultaneously. Request 1 uses payload 1 from all sets, request 2 uses payload 2 from all sets, and so on.
    4. Cluster Bomb = Uses a separate payload set for each position and tests every possible combination of all sets (an iterative approach).

* [Burp Intruder Payload Type](https://portswigger.net/burp/documentation/desktop/tools/intruder/configure-attack/payload-types)

* Payload Configuration :
    * Common Payloads = /usr/share/[seclists](https://www.kali.org/tools/seclists/)/Discovery/Web-Content/common.txt
    * Add from list [Usernames, Passwords, Short Words, etc.]

> * Tip: In case you wanted to use a very large wordlist, it's best to use Runtime file as the Payload Type instead of Simple List, so that Burp Intruder won't have to load the entire wordlist in advance, which may throttle memory usage.

* Payload Processing :
    * Add prefix, sufix, encode, decode the payload, etc.
    * Example : skips any lines that start with a `.`  
        * Rule Type = Skip if matches regex
        * Match Regex = `^\..*$`

* Payload Encoding
    * Enabling us to enable or disable Payload URL-encoding.

## Settings
* Example : Set the Number of retries on network failure and Pause before retry to 0
* Grep - Match
* Grep - Extract

> * Note: We may also use the Resource Pool tab on the right side vertical bar to specify how much network resources Intruder will use, which may be useful for very large attacks. For our example, we'll leave it at its default values.

> ## References : 
> * https://portswigger.net/burp/documentation/desktop/tools/intruder/getting-started
> * https://medium.com/@chai.exe/burp-suite-intruder-in-depth-notes-7ee56c5e42c8


# ZAP Fuzzer
* Missing some of the features provided by Burp Intruder.
* ZAP Fuzzer, however, does not throttle the fuzzing speed, which makes it much more useful than Burp's free Intruder.
* Proxy History -> Right Click on Request -> Attack -> Fuzz
* 4 Main Config:
    * Fuzz Location = To place our location on a certain word, we can select it and click on the Add button on the right pane
    * Payloads = File, File Fuzzers, Numberzz, String, etc.
    * Processors = MD5 Hash, Prefix String, URL Decode/Encode, etc.
    * Options = Concurrent Threads, etc.
        * Depth first, which would attempt all words from the wordlist on a single payload position before moving to the next (e.g., try all passwords for a single user before brute-forcing the following user)
        * Breadth first, which would run every word from the wordlist on all payload positions before moving to the next word (e.g., attempt every password for all users before moving to the following password).

> Note :
> * ZAP Fuzzer has built-in wordlists we can choose from
> * More databases can be installed from the ZAP Marketplace


# Burp Scanner
* A powerful scanner for various types of web vulnerabilities
* Using a `Crawler` for building the website structure, and `Scanner` for passive and active scanning
* Burp Scanner is a Pro-Only feature

## Target Scope
* To start a scan in Burp Suite, we have the following options:
    1. Start scan on a specific request from Proxy History
    2. Start a new scan on a set of targets
    3. Start a scan on items in scope
* Target -> Scope

## Crawler 
* Dashboard -> New Scan

> * Note: A Crawl scan only follows and maps links found in the page we specified, and any pages found on it. It does not perform a fuzzing scan to identify pages that are never referenced, like what dirbuster or ffuf would do. This can be done with Burp Intruder or Content Discovery, and then added to scope, if needed.

## Passive Scanner
* A Passive Scan does not send any new requests but analyzes the source of pages already visited in the target/scope and then tries to identify potential vulnerabilities.
* Traget -> Site Map -> Passively scan this target or a Burp Proxy -> HTTP History -> Right-click on any request -> Passive Scan selected message
> TIP: Look at the Critical & High severity issues and with Confident or Firm Confidence

## Active Scanner
* Runs a more comprehensive scan
* Traget -> Site Map -> Actively scan this target or a Burp Proxy -> HTTP History -> Right-click on any request -> Active Scan selected message

## Scan Config
* Audit Behavior - Threads, Timeout, etc.
* Scan Checks - Findings to be checked

## Reporting
Target -> Site Map -> right-click on our target -> Issue -> Report issues for this host


# ZAP Scanner
* Using a `ZAP Spider` for building the website structure, and `ZAP Scanner` for passive and active scanning

## ZAP Spider
* History Tab -> Request -> Attack -> Spider

> Note: 
> * When we click on the Spider button, ZAP may tell us that the current website is not in our scope, and will ask us to automatically add it to the scope before starting the scan, to which we can say 'Yes'. The Scope is the set of URLs ZAP will test if we start a generic scan, and it can be customized by us to scan multiple websites and URLs. Try to add multiple targets to the scope to see how the scan would run differently.
> * In some versions of browsers, the ZAP's HUD might not work as intended.

> * Tip: ZAP also has a different type of Spider called Ajax Spider, which can be started from the third button on the right pane. The difference between this and the normal scanner is that Ajax Spider also tries to identify links requested through JavaScript AJAX requests, which may be running on the page even after it loads. Try running it after the normal Spider finishes its scan, as this may give a better output and add a few links the normal Spider may have missed, though it may take a little bit longer to finish.

## ZAP Passive Scanner
* Identified while running spider
* Alerts Tab = to see all identified issues

## ZAP Active Scanner
* History Tab -> Request -> Attack -> Active Scan

## ZAP Reporting
* Reports -> Generate HTML Report
* Report Formats = HTML, XML, JSON, Markdown, etc.


# Extensions
## [Burp BApp store](https://portswigger.net/bappstore)
* Extensions Tab -> BApp Store -> [Sort By Popularity]

> * Note: Some extensions are for Pro users only, while most others are available to everyone.
> * Note: Some extensions have requirements that are not usually installed on Linux/macOS/Windows by default, like `Jython`, so you have to install them before being able to install the extension.

||Useful Extensions||
|---|---|---|
|NET Beautifier|J2EEScan|Software Vulnerability Scanner|
|Software Version Reporter|Active Scan++|Additional Scanner Checks|
|AWS Security Checks|Backslash Powered Scanner|Wsdler|
|Java Deserialization Scanner|C02|Cloud Storage Tester|
|CMS Scanner|Error Message Checks|Detect Dynamic JS|
|Headers Analyzer|HTML5 Auditor|PHP Object Injection Check|
|JavaScript Security|Retire.JS|CSP Auditor|
|Random IP Address Header|Autorize|CSRF Scanner|
|JS Link Finder|

## [ZAP Marketplace](https://www.zaproxy.org/addons/)
* Manage Add-Ons
* Marketplace
    * Release = Stable
    * Alpha/Beta = May produce errors/issues

> * Examples : 
> Install from Marketplace : FuzzDB Files & FuzzDB Offensive
> Fuzzer -> File Fuzzers -> fuzzdb -> attack -> os-cmd-execution