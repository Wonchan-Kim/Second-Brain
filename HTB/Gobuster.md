After discovering a web application, it is always worth checking to see if we can uncover any hidden files or directories on the web server that are not intended for public access. Fluff and gobuster is a useful tool to perform the directory enumeration. Sometimes we will find hidden functionality or pages/directories exposing sensitive data that can be leveraged to access the web application or even remote code execution on the web server itself. 

---
### Gobuster

Is a versatile (다목적의) tool that allows for performing DNS, vhost, and directory brute-forcing. The tool has additional functionality such as enumeration of public AWS S 3 buckets. For this module's purposes, we are interested in the directory brute forcing modes specified with the switch dir. Following is the simple scan using dirbcommon. Txt.

```
gobuster dir -u http://10.10.10.121/ -w /usr/share/seclists/Discovery/Web-Content/common.txt

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.10.121/
[+] Threads:        10
[+] Wordlist:       /usr/share/seclists/Discovery/Web-Content/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/12/11 21:47:25 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htpasswd (Status: 403)
/.htaccess (Status: 403)
/index.php (Status: 200)
/server-status (Status: 403)
/wordpress (Status: 301)
===============================================================
2020/12/11 21:47:46 Finished
===============================================================
```
Status code 200 reveals that the resource's request was successful, while a 403 HTTP status code indicates that we are forbidden to access the resource. 301 code indicates that we are being redirected, which is not a failure case. 
The scan was completed successfully, and it identifies a wordpress installation at wordpress, which is the most commonly used CMS (content management system) and has an enourmous potential attack surface. In this case, visiting ip address/wordpress in a browser reveals that wordpress is still in setup mode which will allow us to gain remote code execution (RCE) on the server. 

---
## DNS subdomain enumeration

There also may be essential resources hosted on subdomains, such as admin panels or applications with additional functionality that could be exploited. Gobuster can be used to enumerate available subdomains of a given domain using the dns flag to specify DNS mode. First clone the Seclists github repos, containing many useful lists for fuzzing and exploration. 

After installing seclists, add a dns server such as 1.1.1.1 to the /etc/resolv. Conf file. We will target the domain inlanefreight. Com, the website for a fictional freight and logistics company. 
```
kwc0827@htb[/htb]$ gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Domain:     inlanefreight.com
[+] Threads:    10
[+] Timeout:    1s
[+] Wordlist:   /usr/share/SecLists/Discovery/DNS/namelist.txt
===============================================================
2020/12/17 23:08:55 Starting gobuster
===============================================================
Found: blog.inlanefreight.com
Found: customer.inlanefreight.com
Found: my.inlanefreight.com
Found: ns1.inlanefreight.com
Found: ns2.inlanefreight.com
Found: ns3.inlanefreight.com
===============================================================
2020/12/17 23:10:34 Finished
===============================================================
```

---
## Web Enumeration Type

1) banner grabbing/web server headers
Web server headers provide a good picture of what is hosted on a web server. They can reveal the specific application framework in use, the authentication options, and whether the server is missing essential security options or has been misconfigured. We can use cURL to retrieve server header information from the command line. Curl is another essential addition to our penetration (침투) testing toolkit, and familiarity with its many options is encouraged. 
```
curl -IL https://www.inlanefrieght.com


HTTP/1.1 200 OK
Date: Fri, 18 Dec 2020 22:24:05 GMT
Server: Apache/2.4.29 (Ubuntu)
Link: <https://www.inlanefreight.com/index.php/wp-json/>; rel="https://api.w.org/"
Link: <https://www.inlanefreight.com/>; rel=shortlink
Content-Type: text/html; charset=UTF-8
```

2) whatweb
We can extract the version of web servers, supporting frameworks and applications using the cli whatweb. The information can help us pinpoint the technologies in use and begin to search for potential vulnerabilities.
```

http://10.10.10.121 [200 OK] Apache[2.4.41], Country[RESERVED][ZZ], Email[license@php.net], HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[10.10.10.121], Title[PHP 7.4.3 - phpinfo()]
```

3) Certificates
	SSL/TLS certificates are another potentially valuable source of information if HTTPS is in use. Browsing and viewing the certificates revleas the details ![[Pasted image 20250114221438.png]]
	These could potentially be used to conduct a phishing attack if this is within the scope of an assessment. 
4) Robots. Txt
	It is common for websites to contain a robots. Txt file, whose purpose is to instruct search engine web crawlers which resources can and cannot be accessed for indexing. The robots. Txt file can provide valuable information such as the location of private files and admin pages. 