
# Table of Contents  
* [Introduction](#introduction)
* **Types Of Reconnaissance**
    * [Active Reconnaisance]()
    * [Passive Reconnaisance]()
* [WHOIS](#whois)
* [DNS](#dns-domain-name-system)
    * [Hosts File](#the-hosts-file)
    * [DNS Zone](#dns-key-concepts)
    * [DNS Concepts](#dns-concepts)
    * [DNS Record Types](#dns-record-types)
    * [Pentester's View : DNS](#pentesters-view-dns)
    * [Digging DNS : Tools](#digging-dns--tools)
        * [dig](#tool-the-domain-information-groper-dig)
    * [Subdomains](#subdomains)
        * [Active Subdomain Enumeration](#active-subdomain-enumeration)
        * [Passive Subdomain Enumeration](#passive-subdomain-enumeration)
        * [Subdomain Bruteforcing](#subdomain-brute-forcing)
            * [Subdomain Bruteforcing Tools](#subdomain-bruteforcing-tools)
                * [DNSEnum](#dnsenum)
        * [DNS Zone Transfer](#dns-zone-transfer)
    * [Virtual Hosts](#virtual-hosts)
        * [VHosts Working](#vhosts-working)
        * [Server VHosts Lookup](#server-vhost-lookup)
        * [Types of Virtual Hosting](#types-of-virtual-hosting)
        * [Virtual Host Discovery Tools](#virtual-host-discovery-tools)
            * [gobuster](#gobuster)
    * [Certificate Transparency Logs](#certificate-transparency-logs)
        * [What are Certificate Transparency Logs ?](#what-are-certificate-transparency-logs)
* [Fingerprinting](#fingerprinting)
    * [Fingerprinting Techniques](#fingerprinting-techniques)
    * [Fingerprinting Tools](#fingerprinting-tools)
        * [Banner Grabbing: cURL](#banner-grabbing--curl)
        * [Wafw00f](#wafw00f)
        * [Nikto](#nikto)
* [Crawling / Spidering](#crawling--spidering)
    * [Crawler Working](#crawler-working)
    * [Type 01 : Breadth-First Crawling](#type-01--breadth-first-crawling)
    * [Type 02 : Depth-First Crawling](#type-02--depth-first-crawling)
    * [Pentester's View : Extracting Valuable Information](#pentesters-view--extracting-valuable-information)
    * [robots.txt](#robotstxt)
        * [robot's.txt Structure](#structure)
        * [Pentester's View : robots.txt Web Reconnaissance](#pentesters-view--robotstxt-web-reconnaissance)
    * [Well-known URIs](#well-known-uris)
        * [Pentester's View : Web Recon & .well-known](#pentesters-view--web-recon--well-known)
    * [Creepy Crawlies](#creepy-crawlies)
        * [Web Crawler Tools](#web-crawler-tools)
            * [Tools: Scrapy](#tool--scrapy)    
* [Search Engine Discovery / OSINT](#search-engine-discovery--osint-open-source-intelligence)
    * [Search Operators](#search-operators)
    * [Google Dorking / Hacking](#google-dorking--hacking)
* [Web Archieves](#web-archieves)
    * [Pentester's View: Why the Wayback Machine Matters for Web Reconnaissance](#pentesters-view-why-the-wayback-machine-matters-for-web-reconnaissance)    
* [Automating Recon]()




</br>
</br>

# Introduction
- [x] Identifying Assets [Web Pages, Subdomains, IP Addresses, Technologies used, etc.]
- [x] Discovering Hidden Information [Backup Files, Configuration Files, internal Documents, etc.]
- [x] Analysing the Attack Surface [Vulnerability Assessment - Technologies used, Configurations and Possible entry points]
- [x] Gathering Intelligence [Identifying Key Personnel, Email Addresses or Patterns of behaviour, etc.]

# Active Reconnaissance
* Directly interacts with the target system
* Provides a direct and often more comprehensive view of the target's infrastructure and security posture
* Trigger alerts or raise suspicion

|Technique|Description|Example|Tools|Risk of Detection|
|--|--|--|--|--|
|Port Scanning|Identifying open ports and services running on the target.|Using Nmap to scan a web server for open ports like 80 (HTTP) and 443 (HTTPS).|Nmap, Masscan, Unicornscan|High: Direct interaction with the target can trigger intrusion detection systems (IDS) and firewalls.|
|Vulnerability Scanning|Probing the target for known vulnerabilities, such as outdated software or misconfigurations.|Running Nessus against a web application to check for SQL injection flaws or cross-site scripting (XSS) vulnerabilities.|Nessus, OpenVAS, Nikto|High: Vulnerability scanners send exploit payloads that security solutions can detect.|
|Network Mapping|Mapping the target's network topology, including connected devices and their relationships.|Using traceroute to determine the path packets take to reach the target server, revealing potential network hops and infrastructure.|Traceroute, Nmap|Medium to High: Excessive or unusual network traffic can raise suspicion.|
|Banner Grabbing|Retrieving information from banners displayed by services running on the target.|Connecting to a web server on port 80 and examining the HTTP banner to identify the web server software and version.|Netcat, curl|Low: Banner grabbing typically involves minimal interaction but can still be logged.|
|OS Fingerprinting|Identifying the operating system running on the target.|Using Nmap's OS detection capabilities (-O) to determine if the target is running Windows, Linux, or another OS.|Nmap, Xprobe2|Low: OS fingerprinting is usually passive, but some advanced techniques can be detected.|
|Service Enumeration|Determining the specific versions of services running on open ports.|Using Nmap's service version detection (-sV) to determine if a web server is running Apache 2.4.50 or Nginx 1.18.0.|Nmap|Low: Similar to banner grabbing, service enumeration can be logged but is less likely to trigger alerts.|
|Web Spidering|Crawling the target website to identify web pages, directories, and files.|Running a web crawler like Burp Suite Spider or OWASP ZAP Spider to map out the structure of a website and discover hidden resources.|Burp Suite Spider, OWASP ZAP Spider, Scrapy (customisable)|Low to Medium: Can be detected if the crawler's behaviour is not carefully configured to mimic legitimate traffic.|

# Passive Reconnaissance
* Without directly interacting with target.

|Technique|Description|Example|Tools|Risk of Detection|
|--|--|--|--|--|
|Search Engine Queries|Utilising search engines to uncover information about the target, including websites, social media profiles, and news articles.|Searching Google for "[Target Name] employees" to find employee information or social media profiles.|Google, DuckDuckGo, Bing, and specialised search engines (e.g., Shodan)|Very Low: Search engine queries are normal internet activity and unlikely to trigger alerts.|
|WHOIS Lookups|Querying WHOIS databases to retrieve domain registration details.|Performing a WHOIS lookup on a target domain to find the registrant's name, contact information, and name servers.|whois command-line tool, online WHOIS lookup services|Very Low: WHOIS queries are legitimate and do not raise suspicion.|
|DNS|Analysing DNS records to identify subdomains, mail servers, and other infrastructure.|Using dig to enumerate subdomains of a target domain.|dig, nslookup, host, dnsenum, fierce, dnsrecon|Very Low: DNS queries are essential for internet browsing and are not typically flagged as suspicious.|
|Web Archive Analysis|Examining historical snapshots of the target's website to identify changes, vulnerabilities, or hidden information.|Using the Wayback Machine to view past versions of a target website to see how it has changed over time.|Wayback Machine|Very Low: Accessing archived versions of websites is a normal activity.|
|Social Media Analysis|Gathering information from social media platforms like LinkedIn, Twitter, or Facebook.|Searching LinkedIn for employees of a target organisation to learn about their roles, responsibilities, and potential social engineering targets.|LinkedIn, Twitter, Facebook, specialised OSINT tools|Very Low: Accessing public social media profiles is not considered intrusive.|
|Code Repositories|Analysing publicly accessible code repositories like GitHub for exposed credentials or vulnerabilities.|Searching GitHub for code snippets or repositories related to the target that might contain sensitive information or code vulnerabilities.|GitHub, GitLab|Very Low: Code repositories are meant for public access, and searching them is not suspicious.|


# WHOIS
* Query and response `protocol` designed to access databases that store information about registered internet resources. Primarily associated with domain names
```shell
whois inlanefreight.com
```
* WHOIS records:
    * Domain Name: The domain name itself (e.g., example.com)
    * Registrar: The company where the domain was registered (e.g., GoDaddy, Namecheap)
    * Registrant Contact: The person or organization that registered the domain.
    * Administrative Contact: The person responsible for managing the domain.
    * Technical Contact: The person handling technical issues related to the domain.
    * Creation and Expiration Dates: When the domain was registered and when it's set to expire.
    * Name Servers: Servers that translate the domain name into an IP address.
* Pentester's View :
    - [x] Identifying Key Personnel [Social Enginnering & Phishing Attacks]
    - [x] Discovering Network Infrastructure [Entry Points / Misconfiguration - Name Servers or IP Addresses]
    - [x] Historical Data Analysis [Track evolution of digital presence]
        * Tool = [WhoisFreaks](https://whoisfreaks.com/)


# DNS (Domain Name System)
* Working:
    1. Your Computer Asks for Directions (DNS Query)
    2. The DNS Resolver Checks its Map (Recursive Lookup)
    3. Root Name Server Points the Way
    4. TLD Name Server Narrows It Down
    5. Authoritative Name Server Delivers the Address
    6. The DNS Resolver Returns the Information
    7. Your Computer Connects

## The Hosts File
* Map hostnames to IP addresses
* Bypasses the DNS process
* Useful for development, troubleshooting, or blocking websites
* Location :
    * Windows = C:\Windows\System32\drivers\etc\hosts
    * Linux = /etc/hosts
```shell
# Local Server Development
127.0.0.1       myapp.local

# Testing Connectivity by specifying an IP Address
192.168.1.20    testserver.local

#Blocking unwanted websites by redirecting their domains to a non-existent IP address
0.0.0.0 unwanted-site.com
```

## DNS Key concepts
* `Zone` a virtual container for a set of domain names. 
    * Example.com and all its subdomains (like mail.example.com or blog.example.com) would typically belong to the same DNS zone
    * The zone file, a text file residing on a DNS server, defines the resource records (discussed below) within this zone, providing crucial information for translating domain names into IP addresses.
    ```dns-zone
        $TTL 3600 ; Default Time-To-Live (1 hour)
        @       IN SOA   ns1.example.com. admin.example.com. (
                        2024060401 ; Serial number (YYYYMMDDNN)
                        3600       ; Refresh interval
                        900        ; Retry interval
                        604800     ; Expire time
                        86400 )    ; Minimum TTL
        @       IN NS    ns1.example.com.
        @       IN NS    ns2.example.com.
        @       IN MX 10 mail.example.com.
        www     IN A     192.0.2.1
        mail    IN A     198.51.100.1
        ftp     IN CNAME www.example.com.
    ```

## DNS Concepts
|DNS Concept|Description|Example|
|--|--|--|
|Domain Name|A human-readable label for a website or other internet resource.|www.example.com
|IP Address|A unique numerical identifier assigned to each device connected to the internet.|192.0.2.1
|DNS Resolver|A server that translates domain names into IP addresses.|Your ISP's DNS server or public resolvers like Google DNS|(8.8.8.8)|
|Root Name Server|The top-level servers in the DNS hierarchy.|There are 13 root servers worldwide, named A-M: a.root-servers.net|
|TLD Name Server|Servers responsible for specific top-level domains (e.g., .com, .org).|Verisign for .com, PIR for .org|
|Authoritative Name Server|The server that holds the actual IP address for a domain.|Often managed by hosting providers or domain registrars.|
|DNS Record Types|Different types of information stored in DNS.|A, AAAA, CNAME, MX, NS, TXT, etc.|

## DNS Record Types
|Record Type|Full Name|Description|Zone File Example|
|--|--|--|--|
|A|Address Record|Maps a hostname to its IPv4 address.|www.example.com. IN A 192.0.2.1|
|AAAA|IPv6 Address Record|Maps a hostname to its IPv6 address.|www.example.com. IN AAAA 2001:db8:85a3::8a2e:370:7334|
|CNAME|Canonical Name Record|Creates an alias for a hostname, pointing it to another hostname.|blog.example.com. IN CNAME webserver.example.net.|
|MX|Mail Exchange Record|Specifies the mail server(s) responsible for handling email for the domain.|example.com. IN MX 10 mail.example.com.|
|NS|Name Server Record|Delegates a DNS zone to a specific authoritative name server.|example.com. IN NS ns1.example.com.|
|TXT|Text Record|Stores arbitrary text information, often used for domain verification or security policies.|example.com. IN TXT "v=spf1 mx -all" (SPF record)|
|SOA|Start of Authority Record|Specifies administrative information about a DNS zone, including the primary name server, responsible person's email, and other parameters.|example.com. IN SOA ns1.example.com. admin.example.com. 2024060301 10800 3600 604800 86400|
|SRV|Service Record|Defines the hostname and port number for specific services.|_sip._udp.example.com. IN SRV 10 5 5060 sipserver.example.com.|
|PTR|Pointer Record|Used for reverse DNS lookups, mapping an IP address to a hostname.|1.2.0.192.in-addr.arpa. IN PTR www.example.com.|

> * Note : The "IN" in the examples stands for "Internet." It's a class field in DNS records that specifies the protocol family. In most cases, you'll see "IN" used, as it denotes the Internet protocol suite (IP) used for most domain names. Other class values exist (e.g., CH for Chaosnet, HS for Hesiod) but are rarely used in modern DNS configurations.
> * References:
>   * https://en.wikipedia.org/wiki/Chaosnet
>   * https://chaosnet.net/
>   * https://arxiv.org/abs/1910.02423
>   * https://jpmens.net/2012/06/28/hesiod-a-lightweight-directory-service-on-dns/

## Pentester's View: DNS
* Uncovering Assets [Subdomains, Mail Server, Name Servers, etc.]
* Mapping the Network Infrastructure  [Understand how different systems are connected, identify traffic flow, and pinpoint potential choke points or weaknesses]
* Monitoring for Changes [TXT record containing a value like _1password=... = May help identify technologies or services used]

## Digging DNS : Tools
|Tool|Key Features|Use Cases|
|--|--|--|
|dig|Versatile DNS lookup tool that supports various query types (A, MX, NS, TXT, etc.) and detailed output.|Manual DNS queries, zone transfers (if allowed), troubleshooting DNS issues, and in-depth analysis of DNS records.|
|nslookup|Simpler DNS lookup tool, primarily for A, AAAA, and MX records.|Basic DNS queries, quick checks of domain resolution and mail server records.|
|host|Streamlined DNS lookup tool with concise output.|Quick checks of A, AAAA, and MX records.|
|dnsenum|Automated DNS enumeration tool, dictionary attacks, brute-forcing, zone transfers (if allowed).|Discovering subdomains and gathering DNS information efficiently.|
|fierce|DNS reconnaissance and subdomain enumeration tool with recursive search and wildcard detection.|User-friendly interface for DNS reconnaissance, identifying subdomains and potential targets.|
|dnsrecon|Combines multiple DNS reconnaissance techniques and supports various output formats.|Comprehensive DNS enumeration, identifying subdomains, and gathering DNS records for further analysis.|
|theHarvester|OSINT tool that gathers information from various sources, including DNS records (email addresses).|Collecting email addresses, employee information, and other data associated with a domain from multiple sources.|
|Online DNS Lookup Services|User-friendly interfaces for performing DNS lookups.|Quick and easy DNS lookups, convenient when command-line tools are not available, checking for domain availability or basic information|

### Tool: The Domain Information Groper (dig)
* Common dig Commands

    |Command|Description|
    |--|--|
    |dig domain.com|	Performs a default A record lookup for the domain.|
    |dig domain.com A|	Retrieves the IPv4 address (A record) associated with the domain.|
    |dig domain.com AAAA|	Retrieves the IPv6 address (AAAA record) associated with the domain.|
    |dig domain.com MX|	Finds the mail servers (MX records) responsible for the domain.|
    |dig domain.com NS|	Identifies the authoritative name servers for the domain.|
    |dig domain.com TXT|	Retrieves any TXT records associated with the domain.|
    |dig domain.com CNAME|	Retrieves the canonical name (CNAME) record for the domain.|
    |dig domain.com SOA|	Retrieves the start of authority (SOA) record for the domain.|
    |dig @1.1.1.1 domain.com|	Specifies a specific name server to query; in this case 1.1.1.1|
    |dig +trace domain.com|	Shows the full path of DNS resolution.|
    |dig -x 192.168.1.1|	Performs a reverse lookup on the IP address 192.168.1.1 to find the associated host name. You may need to specify a name server.|
    |dig +short domain.com|	Provides a short, concise answer to the query.|
    |dig +noall +answer domain.com|	Displays only the answer section of the query output.|
    |dig domain.com ANY|	Retrieves all available DNS records for the domain (Note: Many DNS servers ignore ANY queries to reduce load and prevent abuse, as per RFC 8482).|

> **Caution**: Some servers can detect and block excessive DNS queries. Use caution and respect rate limits. Always obtain permission before performing extensive DNS reconnaissance on a target.

> * Note: An opt pseudosection can sometimes exist in a dig query. This is due to Extension Mechanisms for DNS (EDNS), which allows for additional features such as larger message sizes and DNS Security Extensions (DNSSEC) support.
>   ```shell
>   #Remove the OPT PSEUDOSECTION
>   dig example.com +noedns
>   ```

```shell
dig google.com
dig +short hackthebox.eu
dig example.com ANY
```

## Subdomains
* Usually presented under `A` or `AAAA` or `CNAME`.

### Active Subdomain Enumeration
* Directly interacting with the target domain's DNS servers
* 2 Methods:
    1. DNS Zone Transfer
    2. Brute-force Enumeration [Tools: dnsenum, ffuf and gobuster]

### Passive Subdomain Enumeration
* Certificate Transparency (CT) logs [Tools: [cert.sh](https://crt.sh/)]
    * Public repositories of SSL/TLS certificates.
    * These certificates often include a list of associated subdomains in their Subject Alternative Name (SAN) field, providing a treasure trove of potential targets.
* Search Engine Dorks [site:*.example.com]

### Subdomain Brute-forcing
* Process : 
    1. Wordlist Selection [General-Purpose, Targeted or Custom Wordlist]
    2. Iteration and Querying
    3. DNS Lookup
    4. Filtering & Validation

### Subdomain Brute-forcing Tools
|Tool|	Description|
|--|--|
|dnsenum|	Comprehensive DNS enumeration tool that supports dictionary and brute-force attacks for discovering subdomains.|
|fierce|	User-friendly tool for recursive subdomain discovery, featuring wildcard detection and an easy-to-use interface.|
|dnsrecon|	Versatile tool that combines multiple DNS reconnaissance techniques and offers customisable output formats.|
|amass|	Actively maintained tool focused on subdomain discovery, known for its integration with other tools and extensive data sources.|
|assetfinder|	Simple yet effective tool for finding subdomains using various techniques, ideal for quick and lightweight scans.|
|puredns|	Powerful and flexible DNS brute-forcing tool, capable of resolving and filtering results effectively.|

### DNSEnum
* Key Functions:
    * DNS Record Enumeration
    * Zone Transfer Attempts
    * Subdomain Brute-Forcing
    * Google Scrapping
    * Reverse Lookup
    * WHOIS Lookup

```shelll
dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -r
```

## DNS Zone Transfer
* Replicating DNS records between name servers
* A DNS zone transfer is essentially a wholesale copy of all DNS records within a zone (a domain and its subdomains) from one name server to another.
* Zone Transfer Process:
    1. Zone Transfer Request (AXFR)
    2. SOA Record Transfer
    3. DNS Records Transmission
    4. Zone Transfer Complete
    5. Acknowledgement (ACK)
* Exploiting Zone Transfers
```shell
dig axfr @nsztm1.digi.ninja zonetransfer.me
```

## Virtual Hosts
* Web servers like Apache, Nginx, or IIS are designed to host multiple websites or applications on a single server. They achieve this through virtual hosting, which allows them to differentiate between domains, subdomains, or even separate websites with distinct content.

### VHosts Working
* Virtual hosting is the ability of web servers to distinguish between multiple websites or applications sharing the same IP address.This is achieved by leveraging the HTTP Host header, a piece of information included in every HTTP request sent by a web browser
* VHosts vs Subdomains
    * Subdomains: These are extensions of a main domain name (e.g., blog.example.com is a subdomain of example.com). Subdomains typically have their own DNS records, pointing to either the same IP address as the main domain or a different one. They can be used to organise different sections or services of a website.
    * Virtual Hosts (VHosts): Virtual hosts are configurations within a web server that allow multiple websites or applications to be hosted on a single server. They can be associated with top-level domains (e.g., example.com) or subdomains (e.g., dev.example.com). Each virtual host can have its own separate configuration, enabling precise control over how requests are handled

> * Note : The hosts file allows you to map a domain name to an IP address manually, bypassing DNS resolution.

* Websites often have subdomains that are not public and won't appear in DNS records. These subdomains are only accessible internally or through specific configurations. `VHost fuzzing` is a technique to discover public and non-public subdomains and VHosts by testing various hostnames against a known IP address.
* Virtual hosts can also be configured to use different domains, not just subdomains.
```apacheconf
    # Example of name-based virtual host configuration in Apache in /etc/apache/sites-available/
    <VirtualHost *:80>
        ServerName www.example1.com
        DocumentRoot /var/www/example1
    </VirtualHost>

    <VirtualHost *:80>
        ServerName www.example2.org
        DocumentRoot /var/www/example2
    </VirtualHost>

    <VirtualHost 192.168.1.60:80>
        ServerName ://site2.com
        DocumentRoot /var/www/site2
        
        <Directory /var/www/site2>
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/site2_error.log
        CustomLog ${APACHE_LOG_DIR}/site2_access.log combined
    </VirtualHost>
```

### Server VHost Lookup
1. Browser Requests a Website
2. Host Header Reveals the Domain
3. Web Server Determines the Virtual Host
4. Serving the Right Content

### Types of Virtual Hosting
1. Name-Based Virtual Hosting [HTTP Host Header][Limitations : SSL/TLS Protocol]
2. IP-Based Virtual Hosting [Unique IP address to each website hosted on the server][Limitation : expensive and less scalable]
3. Port-Based Virtual Hosting [Different ports on the same IP address.]

### Virtual Host Discovery Tools
|Tool|Description|Features|
|--|--|--|
|gobuster|A multi-purpose tool often used for directory/file brute-forcing, but also effective for virtual host discovery.|Fast, supports multiple HTTP methods, can use custom wordlists.|
|Feroxbuster|Similar to Gobuster, but with a Rust-based implementation, known for its speed and flexibility.|Supports recursion, wildcard discovery, and various filters.|
|ffuf|Another fast web fuzzer that can be used for virtual host discovery by fuzzing the Host header.	Customizable wordlist input and filtering options.|

### gobuster
*  It systematically sends HTTP requests with different Host headers to a target IP address and then analyses the responses to identify valid virtual hosts.
* Pre-requisites:
    * Tager Identification {IP Address}
    * Wordlist Preparation

```shell
gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain
gobuster vhost -u http://inlanefreight.htb:SMTPO -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -t 60 --append-domain
```

> * Note : Virtual host discovery can generate significant traffic and might be detected by intrusion detection systems (IDS) or web application firewalls (WAF). Exercise caution and obtain proper authorization before scanning any targets.

## Certificate Transparency Logs
* Secure Sockets Layer/Transport Layer Security (SSL/TLS) protocol
* Digital certificate, a small file that verifies a website's identity and allows for secure, encrypted communication.

> * Background:  The process of issuing and managing these certificates isn't foolproof. Attackers can exploit rogue or mis-issued certificates to impersonate legitimate websites, intercept sensitive data, or spread malware. This is where Certificate Transparency (CT) logs come into play.

### What are Certificate Transparency Logs
* Public, append-only ledgers that record the issuance of SSL/TLS certificates. 
* Whenever a Certificate Authority (CA) issues a new certificate, it must submit it to multiple CT logs. Independent organisations maintain these logs and are open for anyone to inspect.
* CT logs = `global registry of certificates` 
* They provide a transparent and verifiable record of every SSL/TLS certificate issued for a website
* Purpose :
    * Early Detection of Rogue Certificates
    * Accountability for Certificate Authorities
    * Strengthening the Web PKI (Public Key Infrastructure)

> * Note: A rogue certificate is an unauthorized or fraudulent digital certificate issued by a trusted certificate authority

> Bookmarks:
> [Certificate Tansparency](https://certificate.transparency.dev/)

### Pentester's View : CT Logs & Web Recon
* CT logs provide a definitive record of certificates issued for a domain and its subdomains.
* CT logs can unveil subdomains associated with old or expired certificates.  

### Searching CT Logs
|Tool|Key Features|Use Cases|Pros|Cons|
|--|--|--|--|--|
|[crt.sh](https://crt.sh/)|User-friendly web interface, simple search by domain, displays certificate details, SAN entries.|Quick and easy searches, identifying subdomains, checking certificate issuance history.|Free, easy to use, no registration required.|Limited filtering and analysis options.|
|[Censys](https://search.censys.io/)|Powerful search engine for internet-connected devices, advanced filtering by domain, IP, certificate attributes.|In-depth analysis of certificates, identifying misconfigurations, finding related certificates and hosts.|Extensive data and filtering options, API access.|Requires registration (free tier available).|

### crt.sh Lookup
```shell
curl -s "https://crt.sh/?q=facebook.com&output=json" | jq -r '.[]| select(.name_value | contains("dev")) | .name_value' | sort -u
``` 


# Fingerprinting
* Importantance
    * Targeted Attacks
    * Identifying Misconfigurations
    * Prioritising Targets
    * Building a Comprehensive Profile

## Fingerprinting Techniques
* Banner Grabbing
* Analysing HTTP Headers
* Probing for Specific Responses
* Analysing Page Content

## Fingerprinting Tools
|Tool|Description|Features|
|--|--|--|
|Wappalyzer|Browser extension and online service for website technology profiling.|Identifies a wide range of web technologies, including CMSs, frameworks, analytics tools, and more.|
|BuiltWith|Web technology profiler that provides detailed reports on a website's technology stack.|Offers both free and paid plans with varying levels of detail.|
|WhatWeb|Command-line tool for website fingerprinting.|Uses a vast database of signatures to identify various web technologies.|
|Nmap|Versatile network scanner that can be used for various reconnaissance tasks, including service and OS fingerprinting.|Can be used with scripts (NSE) to perform more specialised fingerprinting.|
|Netcraft|Offers a range of web security services, including website fingerprinting and security reporting.|Provides detailed reports on a website's technology, hosting provider, and security posture.|
|wafw00f|Command-line tool specifically designed for identifying Web Application Firewalls (WAFs).|Helps determine if a WAF is present and, if so, its type and configuration.|

### Banner Grabbing : cURL
```shell
curl -I https://inlanefreight.com
curl -I http://inlanefreight.htb:37705
```

### Wafw00f
```shell
wafw00f inlanefeight.com
```

### Nikto
* Open-source web server scanner
```shell
nikto -url inlanfreight.com -Tuning b
nikto -h inlanfreight.htb -p 31981 -Tuning b
```


# Crawling / Spidering
* Automated process of systematically browsing the World Wide Web

## Crawler Working
1. Fetches the seed page
2. Parses it's content
3. Extracts all its links
4. Adds links to queue and crawls them

## Type 01 : Breadth-First Crawling
* Explore's websites width before going deep
* Useful for getting a broad overview of a website's structure and content

## Type 02 : Depth-First Crawling
* Follows a single path of links as far as possible before backtracking and exploring other paths.
* Useful for finding specific content or reaching deep into a website's structure

## Pentester's View : Extracting Valuable Information
* Links (Internal and External)
* Comments
* Metadata
* Sensitive Files [backup files (e.g., .bak, .old), configuration files (e.g., web.config, settings.php), log files (e.g., error_log, access_log), Passwords, API Keys, DB Credentials or Encryption Keys]

## robots.txt
* Adheres to the Robots Exclusion Standard, guidelines for how web crawlers should behave when visiting a website.
* Contains instructions in the form of "directives" that tell bots which parts of the website they can and cannot crawl
```txt
User-agent: *
Disallow: /admin/
Disallow: /private/
Allow: /public/

User-agent: Googlebot
Crawl-delay: 10

Sitemap: https://www.example.com/sitemap.xml
```

### Structure
1. User-Agent
2. Directive

|Directive|Description|Example|
|--|--|--|
|Disallow|Specifies paths or patterns that the bot should not crawl.|Disallow: /admin/ (disallow access to the admin directory)|
|Allow|Explicitly permits the bot to crawl specific paths or patterns, even if they fall under a broader Disallow rule.|Allow: /public/ (allow access to the public directory)|
|Crawl-delay|Sets a delay (in seconds) between successive requests from the bot to avoid overloading the server.|Crawl-delay: 10 (10-second delay between requests)|
|Sitemap|Provides the URL to an XML sitemap for more efficient crawling.|Sitemap: https://www.example.com/sitemap.xml|

### Pentester's View : robots.txt Web Reconnaissance
* Uncovering Hidden Directories
* Mapping Website Structure
* Detecting Crawler Traps

## Well-Known URIs
* Typically accessible via the /.well-known/ path on a web server, centralizes a website's critical metadata, including configuration files and information related to its services, protocols, and security mechanisms.
* For instance, to access a website's security policy, a client would request https://example.com/.well-known/security.txt

|URI Suffix|Description|Status|Reference|
|--|--|--|--|
|security.txt|Contains contact information for security researchers to report vulnerabilities.|Permanent|RFC 9116|
|/.well-known/change-password|Provides a standard URL for directing users to a password change page.|Provisional|https://w3c.github.io/webappsec-change-password-url/#the-change-password-well-known-uri|
|openid-configuration|Defines configuration details for OpenID Connect, an identity layer on top of the OAuth 2.0 protocol.|Permanent|http://openid.net/specs/openid-connect-discovery-1_0.html|
|assetlinks.json|Used for verifying ownership of digital assets (e.g., apps) associated with a domain.|Permanent|https://github.com/google/digitalassetlinks/blob/master/well-known/specification.md|
|mta-sts.txt|Specifies the policy for SMTP MTA Strict Transport Security (MTA-STS) to enhance email security.|Permanent|RFC 8461|

> Bookmark:
> * [IANA Registry Well-Known URIs](https://www.iana.org/assignments/well-known-uris)

### Pentester's View : Web Recon & .well-known
* Discovering endpoints and configuration details
* Example : The openid-configuration URI is part of the OpenID Connect Discovery protocol.
```json

// https://example.com/.well-known/openid-configuration
// This endpoint returns a JSON document containing metadata about the provider's endpoints, supported authentication methods, token issuance, and more

{
  "issuer": "https://example.com",
  "authorization_endpoint": "https://example.com/oauth2/authorize",
  "token_endpoint": "https://example.com/oauth2/token",
  "userinfo_endpoint": "https://example.com/oauth2/userinfo",
  "jwks_uri": "https://example.com/oauth2/jwks",
  "response_types_supported": ["code", "token", "id_token"],
  "subject_types_supported": ["public"],
  "id_token_signing_alg_values_supported": ["RS256"],
  "scopes_supported": ["openid", "profile", "email"]
}
```

> * Note : JWKS URI: The jwks_uri reveals the JSON Web Key Set (JWKS), detailing the cryptographic keys used by the server. 

## Creepy Crawlies
* Web crawling tools automate the crawling process, making it faster and more efficient, allowing you to focus on analyzing the extracted data

### Web Crawler Tools
* Burp Suite Spider
* OWASP ZAP (Zed Attack Proxy)
* Scrapy (Python Framework)
* Apache Nutch (Scalable Crawler)

#### Tool : Scrapy
```shell
# Step 01 : Installing Scrapy
pip3 install scrapy

# Step 02 : Custom scrapy spider (ReconSpider)
wget -O ReconSpider.zip https://cdn.services-k8s.prod.aws.htb.systems/content/modules/144/ReconSpider.v1.2.zip
unzip unzip ReconSpider.zip 

# Step 03 : Executing Reconspider
python3 ReconSpider.py http://inlanefreight.com
python3 ReconSpider.py http://dev.web1337.inlanefreight.htb:31981

# The output will be stored in results.json file
```

* Each key in the JSON file represents a different type of data extracted from the target website:

|JSON Key|Description|
|--|--|
|emails|Lists email addresses found on the domain.|
|links|Lists URLs of links found within the domain.|
|external_files|Lists URLs of external files such as PDFs.|
|js_files|Lists URLs of JavaScript files used by the website.|
|form_fields|Lists form fields found on the domain (empty in this example).|
|images|Lists URLs of images found on the domain.|
|videos|Lists URLs of videos found on the domain (empty in this example).|
|audio|Lists URLs of audio files found on the domain (empty in this example).|
|comments|Lists HTML comments found in the source code.|

* By exploring this JSON structure, you can gain valuable insights into the web application's architecture, content, and potential points of interest for further investigation.


# Search Engine Discovery / OSINT [Open Source Intelligence]
* Involves using search engines as powerful tools to uncover information about target websites, organisations, and individuals.

## Search Operators
|Operator|Operator Description|Example|Example Description|
|--|--|--|--|
|site:|Limits results to a specific website or domain.|site:example.com|Find all publicly accessible pages on example.com.|
|inurl:	|Finds pages with a specific term in the URL.	|inurl:login	|Search for login pages on any website.|
|filetype:	|Searches for files of a particular type.	|filetype:pdf	|Find downloadable PDF documents.|
|intitle:	|Finds pages with a specific term in the title.	|intitle:"confidential report"	|Look for documents titled "confidential report" or similar variations.|
|intext: or inbody:	|Searches for a term within the body text of pages.	|intext:"password reset"	|Identify webpages containing the term “password reset”.|
|cache:	|Displays the cached version of a webpage (if available).	|cache:example.com|	View the cached version of example.com to see its previous content.|
|link:	|Finds pages that link to a specific webpage.	|link:example.com	|Identify websites linking to example.com.|
|related:	|Finds websites related to a specific webpage.	|related:example.com	|Discover websites similar to example.com.|
|info:	|Provides a summary of information about a webpage.	|info:example.com	|Get basic details about example.com, such as its title and description.|
|define:	|Provides definitions of a word or phrase.	|define:phishing	|Get a definition of "phishing" from various sources.|
|numrange:	|Searches for numbers within a specific range.	|site:example.com numrange:1000-2000	|Find pages on example.com containing numbers between 1000 and 2000.|
|allintext:	|Finds pages containing all specified words in the body text.	|allintext:admin password reset	|Search for pages containing both "admin" and "password reset" in the body text.|
|allinurl:	|Finds pages containing all specified words in the URL.	|allinurl:admin panel	|Look for pages with "admin" and "panel" in the URL.|
|allintitle:	|Finds pages containing all specified words in the title.	|allintitle:confidential report 2023	|Search for pages with "confidential," "report," and "2023" in the title.|
|AND	|Narrows results by requiring all terms to be present.	|site:example.com AND (inurl:admin OR inurl:login)	F|ind admin or login pages specifically on example.com.|
|OR	|Broadens results by including pages with any of the terms.	|"linux" OR "ubuntu" OR "debian"|	Search for webpages mentioning Linux, Ubuntu, or Debian.|
|NOT	|Excludes results containing the specified term.	|site:bank.com NOT inurl:login	|Find pages on bank.com excluding login pages.|
|* (wildcard)	|Represents any character or word.	|site:socialnetwork.com filetype:pdf user* manual	|Search for user manuals (user guide, user handbook) in PDF format on socialnetwork.com.|
|.. (range search)	|Finds results within a specified numerical range.	|site:ecommerce.com "price" 100..500	|Look for products priced between 100 and 500 on an e-commerce website.|
|" " (quotation marks)	|Searches for exact phrases.	|"information security policy"	|Find documents mentioning the exact phrase "information security policy".|
|- (minus sign)	|Excludes terms from the search results.	|site:news.com -inurl:sports	|Search for news articles on news.com excluding sports-related content.|

## Google Dorking / Hacking
* [Google hacking Database](https://www.exploit-db.com/google-hacking-database)

* Finding Login Pages:
    ```
    site:example.com inurl:login
    site:example.com (inurl:login OR inurl:admin)
    ```
* Identifying Exposed Files:
    ```
    site:example.com filetype:pdf
    site:example.com (filetype:xls OR filetype:docx)
    ```
* Uncovering Configuration Files:
    ```
    site:example.com inurl:config.php
    site:example.com (ext:conf OR ext:cnf) (searches for extensions commonly used for configuration files)
    ```
* Locating Database Backups:
    ```
    site:example.com inurl:backup
    site:example.com filetype:sql
    ```


# Web Archieves
* [Internet Archjieve's Wayback Machine](https://web.archive.org/)

## Pentester's View: Why the Wayback Machine Matters for Web Reconnaissance
* Uncovering Hidden Assets and Vulnerabilities
* Tracking Changes and Identifying Patterns
* Gathering Intelligence
* Stealthy Reconnaissance


# Automate Recon Frameworks
* [FinalRecon](https://github.com/thewhiteh4t/FinalRecon)
* [Recon-ng](https://github.com/lanmaster53/recon-ng)
* [theHarvester](https://github.com/laramies/theHarvester)
* [SpiderFoot](https://github.com/smicallef/spiderfoot)
* [OSINT Framework](https://osintframework.com/)

## Tool : FinalRecon
```shell
git clone https://github.com/thewhiteh4t/FinalRecon.git
cd FinalRecon
pip3 install -r requirements.txt
chmod +x ./finalrecon.py
./finalrecon.py --help

./finalrecon.py --headers --whois --url http://inlanefreight.com
```