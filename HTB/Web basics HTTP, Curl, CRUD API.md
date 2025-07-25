Login credentials can be viewed in clear text when using http. This owuld make it easy for someone on the same entwork to capture the reqeust and reuse the credentials for malicious purposes. ![http_clear](https://academy.hackthebox.com/storage/modules/35/https_clear.png)![https_google_enc](https://academy.hackthebox.com/storage/modules/35/https_google_enc.png)
In contrast when someone intercepts and analyzes traffic from an HTTPS request, they would see something like the picture above. The data is transferred as a single encrypted stream, which makes it very difficult for anyone to capture information such as credentials or any sensitive data.

Although the data transferred through HTTPS protocol may be encrypted, the request may still reveal the visited URL if it contacted a clear text DNS server. For this reason,, it is recommeded to utilize DNS server (8.8.8.8 or 1.1.1.1) or utilize a VPN service to ensure all traffic is properly encrypted.

How is it handled?

![HTTPS_Flow](https://academy.hackthebox.com/storage/modules/35/HTTPS_Flow.png)
If we type http:// instead of https:// to visit a website that enforces HTTPS,. The browser atempts to resolve the domain and redirects the suer to the webserver hostign the target website. A reques is sernt to port 80 first, which is the unencrypted HTTP portocol. The server detects and redirects the client to 443 HTTPs port instead. This is done 301 moved permanently response code, which we will discuss in an upcoming section. 

Next the client sends a packet, giving information about itself. After this, the server replies followed by a key exchange to exchange SSL certificates. The client verifies the key/certificate and sends one of its own. Aftert this an encrypted handshake is initiated to confirm whether the encryption and transfer are working correctly. Once the handshake completes successfully, normal HTTP communicatin ois continued, which is encrypted after that. 

---
## CURL
CuRL should automatically handle all HTTPS communication standards and perform a secure handshake and then encrypt and decrypt data automatically. However, if we ever contact a website with invalid SSL certificates or an outdated one, then cuRL by default would not proceed with the communication to protect against the earlier mention MITM attacks

To skip this SSL certificate on local web or web  app, you can use -k flag.

--- 

## HTTP request


![raw_request](https://academy.hackthebox.com/storage/modules/35/raw_request.png)

The image shows an HTTP GET request to the URL
The next set of lines contain HTTP header value pairs, like HOST, User-Agent, cookie and many other possible headers. These headers are used to specify various attributes of a request. The headers are terminated with a new line, which is necessary for the server to validate the request. Finally, a request may end witht he reqeust body and data.


> [!faq] HTTP Versions
> 1.X sends requests as clear text, and uses a new line character to seperate different fields and different requests. 2. X on the other hand, sends reqeusts as binary data in a dictionary form. 

![raw_response](https://academy.hackthebox.com/storage/modules/35/raw_response.png) the first line contains two fields by spaces. Http version and the second denotes the HTTP response code.

> [!note] 
> Curl with -v (verbose) prints both the request and response. 

---

## HTTP Headers

Http headers pass informatino between the client and the server. Some headers are only used with either requests or responses, while some other general headers are common to both. Headers can have one or multiple values, appended after the header name and seperated by a colon. 

> [!info]  Category
> 1. General headers
> 2. Entity headers
> 3. Request headers
> 4. Response headers
> 5. Security headers

1. Used in both HTTP request and responses. Used to describe th message rather than its contents.
2. Entityheaders ccan ve both the request and responses. Used to describe the content transferred by a message. They are usaully found in response and Post or Put requests.
3. Request heaaders are used in an HTTP requesty and do not relate to the content of the message. 
4. Response headers can be used in an HTTP response and do not relate to the content.
5. Security headers are a class of response headers used to specify certain rules and policies

---
## Curl 
Using -v shows both request and response. -I flag only display the response header. -i flag to display both the headers and the response body. Difference is that -I sends a HEAD request, while -i sends any request we specify and prints the headers as well.

-I sends a HEAD request, while -i sends any reqeust we speicfy and prints the headers as well. 
```
kwc0827@htb[/htb]$ curl -I https://www.inlanefreight.com

Host: www.inlanefreight.com
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/605.1.15 (KHTML, like Gecko)
Cookie: cookie1=298zf09hf012fh2; cookie2=u32t4o3tb3gg4
Accept: text/plain
Referer: https://www.inlanefreight.com/
Authorization: BASIC cGFzc3dvcmQK

Date: Sun, 06 Aug 2020 08:49:37 GMT
Connection: keep-alive
Content-Length: 26012
Content-Type: text/html; charset=ISO-8859-4
Content-Encoding: gzip
Server: Apache/2.2.14 (Win32)
Set-Cookie: name1=value1,name2=value2; Expires=Wed, 09 Jun 2021 10:18:14 GMT
WWW-Authenticate: BASIC realm="localhost"
Content-Security-Policy: script-src 'self'
Strict-Transport-Security: max-age=31536000
Referrer-Policy: origin
```

Also there is -H flag, some ehadersw, like the user-agent or cookie headers, have their own flags. We can also sue the -A rto set out user agent. 
```
kwc0827@htb[/htb]$ curl https://www.inlanefreight.com -A 'Mozilla/5.0'

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
...SNIP...
```

---
## Browser DevTools
Network tab in developers tools can show the HTTP response.

---
## Request Methods

Following are some commonly used methods
GET POST PUT DELETE (CRUD) + HEAD OPTIONS PATCH

Head requests the header that would be returned if a GET request was made to the server. It does not return body and is usually made to check the response length before downloading resources.

Options returns informations about the server, such as the methods accepted by it

Patch applies partial modifications to the resource at the specific location

---
## Response Codes

HTTP status codes are used to tell the client the status of their request. An http server can return five types of response codes:

> [!note] Response Codes
> - 1 XX provides information and does not affect the processing of the request
> - 2 XX returned when a request succeeds
> - 3 XX returned when the server redirects the client
> - 4 XX signifies improper requests from the client. I.e, requesting a resource that does not exist or requesting a bad format
> - 5 XX returned when there is some problem with the HTTP server itself.

> [!info] Common codes
> - 200 OK Returned on a successful request, and the response body usually contains the requested resource
> - 302 Found Redirects the client to another URL. I.e, rediercting the user to their dashboard after a successful login
> - 400 Bad request returned on encountering malformed requests such as requests with missing the terminators
> - 403 Forbidden signifies that the client does not have appropriate access to the resource. It can also be returned wheb the server detects malicious input from the user
> - 404 not found returned when the client request a resource that does not exist on the server
> - 500 internal server error returned when the server cannot process the request

---
## GET
Whenever we visit any URL, our browsers default to a GET request to obtain the remote resources hosted at that URL. Once the browser receives the initial page it is requesting; it may send other requests using various HTTP methods. This can be observed through the network tab in the browser devtools

---
## HTTP Basic Auth

Usual login forms utilizes HTTP parameters to validate the user credentials. Http basic authentication utilizes a basic HTTP authentication, which is handled directly byu the webserver to protect a specific page/directory wirthout directly interacting with the web app. 

To access the page, we have to enter a valid pair of credentials.
After we enter the credentials, we would get access to the page.

Without the access, curl command fails
```
kwc0827@htb[/htb]$ curl -i http://<SERVER_IP>:<PORT>/
HTTP/1.1 401 Authorization Required
Date: Mon, 21 Feb 2022 13:11:46 GMT
Server: Apache/2.4.41 (Ubuntu)
Cache-Control: no-cache, must-revalidate, max-age=0
WWW-Authenticate: Basic realm="Access denied"
Content-Length: 13
Content-Type: text/html; charset=UTF-8

Access denied
```
We get access denied in the response body, we also get Basic realm='‘access denied’ in the WWW-Authenticate header, which confirms that this page indeed uses basic HTTP auth, as discussed in the Headers section. To provide the credentials through cURL we can use the -u flag

```
kwc0827@htb[/htb]$ curl -u admin:admin http://<SERVER_IP>:<PORT>/

<!DOCTYPE html>
<html lang="en">

<head>
...SNIP...
```
This time, we get the response. There is also another method we cn provide the basic HTTP auth credentials, which is directly through the URL as (username: password@url )
```
kwc0827@htb[/htb]$ curl http://admin:admin@<SERVER_IP>:<PORT>/

<!DOCTYPE html>
<html lang="en">

<head>
...SNIP...
```
---
## HTTP authorization header
If we add -v flag to either of our earlier cURL commands
```
kwc0827@htb[/htb]$ curl -v http://admin:admin@<SERVER_IP>:<PORT>/

*   Trying <SERVER_IP>:<PORT>...
* Connected to <SERVER_IP> (<SERVER_IP>) port PORT (#0)
* Server auth using Basic with user 'admin'
> GET / HTTP/1.1
> Host: <SERVER_IP>
> Authorization: Basic YWRtaW46YWRtaW4=
> User-Agent: curl/7.77.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< Date: Mon, 21 Feb 2022 13:19:57 GMT
< Server: Apache/2.4.41 (Ubuntu)
< Cache-Control: no-store, no-cache, must-revalidate
< Expires: Thu, 19 Nov 1981 08:52:00 GMT
< Pragma: no-cache
< Vary: Accept-Encoding
< Content-Length: 1453
< Content-Type: text/html; charset=UTF-8
< 

<!DOCTYPE html>
<html lang="en">

<head>
...SNIP...
```
Using basic http auth, we see that our http request sets the authorization header to ==Authorization: Basic YWRtaW 46 YWRtaW 4== which is the basic base 64 encoded value of admin: admin. If we were using a modern method of authentication, the authorization would be of type bearer and would contain a longer encrypted token. It is possible to manually set the authorization, without supplying the credentials, to see if it allows the access to the page. We can set the header with the -H flag, and will use the same value from the above HTTP request. We can add the -H flag multiple times to specify multiple headers.
```
kwc0827@htb[/htb]$ curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/

<!DOCTYPE html
<html lang="en">

<head>
...SNIP...
```

___
## GET Parameters

![network_clear_requests](https://academy.hackthebox.com/storage/modules/35/web_requests_get_search.jpg)

When clicked on the request, it gets sent to search. Php with the GET parameter search=le used in the URL. This helps us understand that the search function requests another page for the results. Now, we can send the same request directly to search. Php to get the full search results, though it will probably return them in a specific format such as JSON without having HTML layout shown in the above screenshot.

To send a GET request with cRUL, we can use the exact sma eURL seen in the above screenshots since GET requests plave their parameters in the URL. However, brwoser devtools provide a more convenient method of obtaining the cURL command. We can right click on the request and select Copy>Copy as cURL. Then we can paste the copied command in our terminal and execute it, and we should get the exact same reponse.

```
kwc0827@htb[/htb]$ curl 'http://<SERVER_IP>:<PORT>/search.php?search=le' -H 'Authorization: Basic YWRtaW46YWRtaW4='

Leeds (UK)
Leicester (UK)
```

The copied command will contain all headers used in the HTTP request. However we can remove most of them and only keep necessary authentication headers, like the authorization header.
We can also repeat the exact request right within the browser devtools, by selecting Copy>copy as fetch. They will copy the same HTTP request using the javascript fetch library. Then we can go to the javascript console tab by clicking ctrl shift k, paste out fetch command and hit enter to send the request.
![network_clear_requests](https://academy.hackthebox.com/storage/modules/35/web_requests_fetch_search.jpg)
As we see, the browser sent out request, and we can see the response returned after it. We can click on the response to view its details, expand various details and read them.

---
## POST 
In the previous section, we saw how GET requests may be used for search and accessing pages. Whenever web application need to transfer files or move the user parameters from the URL, they utilize POST requests.

Unlike GET, POST places user parameter within the HTTP request body. 
> [!tip] Benefits of placing parameters with the body
> 1. Lack of logging: as post requests may transfer large files, it would not be efficient for the server to log all uploaded files as part of the requested URL, as would be the case with a file uploaded through a GET request
> 2. Less encoding requirements: URLs are designed to be shared, which means they need to conform to characters that can be converted to letters. The post request places data in the body which can accept binary data. They only characters that need to be encoded are those that are used to separate parameters.
> 3. More data can be sent: while the maximum URL length varies between browsers, it should be kept under 2000 characters so they can not handle a lot of data.


![[Pasted image 20250209133350.png]]
![web_requests_login_request](https://academy.hackthebox.com/storage/modules/35/web_requests_login_request.jpg) It is possible to filter out external requests by using filter with the server ip, so it would only show requests going to the web application’s web server.

By clicking request tab, and then Raw button to show the raw request data, we see the following data is being sent as the POST request data
![[Pasted image 20250209133931.png]]
We could grab the admin and password information. With the request data at hand, we can try to send a similar request with cURL, to see whether this would allow us to login as well. Furthermore, as we did in the previous section, we can simply right click on the request and select copy as curl. 

-X POST flag is used to send a POST request. Then to add our POST data, we can use the -d flag and add the above data after it, as follows;

```
curl -X POST -d 'username=admin&password=admin' http://<IP>:<PORT>
```

When we examine the HTML code, we will not see the login form code but we will see the search function code, which indicates that we did indeed get authenticated. 

---
## Authenticated cookies
If we were successfully authenticated, we should have received a cookie so our brwosers can persist our authentication and we dont need to login every time we visit the page. We can use the -v or -i flags to view the repsonse, which should contain the Set cookie header with our authenticated cookies. 

```
(kali㉿kali)-[~]
└─$ curl -X POST -d 'username=admin&password=admin' http://94.237.48.92:31905 -i
HTTP/1.1 200 OK
Date: Sun, 09 Feb 2025 21:50:30 GMT
Server: Apache/2.4.41 (Ubuntu)
Set-Cookie:  ; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Vary: Accept-Encoding
Content-Length: 1554
Content-Type: text/html; charset=UTF-8


<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>City Search</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/meyer-reset/2.0/reset.min.css">
    <link rel='stylesheet' href='https://fonts.googleapis.com/css?family=Roboto:400,500,700'>
    <link rel='stylesheet' href='https://static.fontawesome.com/css/fontawesome-app.css'>
    <link rel='stylesheet' href='https://pro.fontawesome.com/releases/v5.2.0/css/all.css'>
    <link rel="stylesheet" href="./style.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/normalize/5.0.0/normalize.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/prefixfree/1.0.7/prefixfree.min.js"></script>
</head>

<body>
            <div class="icon" style="position: absolute; top: 50px; left: 50px;">
        <a href="/?logout" class="btn btn-success"><i class="fab fas fa-sign-out-alt fa-2x"></i></a>
    </div>
    <div>
        <div class="search">
            <div class="bar">
                <div class="icon">
                    <i></i>
                </div>
            </div>
            <form>
                <input type="text">
            </form>
            <div class="close"></div>
            <ul id="results_list">

            </ul>
        </div>
        <em>Type a city name and hit <strong>Enter</strong></em>
    </div>
    <!-- partial -->
    <script src='https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js'></script>
    <script src="./script.js"></script>
    
</body>

</html>                                                                             

```
We can find out ==Set-Cookie: PHPSESSID=ud 8 cq 5 vlk 7 gc 7 b 1 mlbi 8 ui 7 ml 6; path== . With our authenticated cookie, we should now be able to interact with the wev application without needing to provide our credentials every time. To test this, w e can set the above cookie with the -b flag in curl, as follows

```
curl -b 'PHPSESSID=ud8cq5vlk7gc7b1mlbi8ui7ml6' http://94.237.48.92:31905

```
As we can see, we were authenticated and got to the search function. It is also possible to specify the cookie as a header 

```
curl -H 'Cookie: PHPSESSID=ud8cq5vlk7gc7b1mlbi8ui7ml6' http://94.237.48.92:31905
```
It is possible to try the same thing with browsers. Devtools→storage tab
When logging out, we can see the cookie value has changed.
We can manually get authentication without log in directly, by manipulating cookie value on the storage tab.

___
## JSON Data

Observing when we interact with the city search function. To do so, we  will go to the network tab in the dev tool, and then clear the requests. Then we can make any serach query to see what requests get sent.
On the network tab, we can find out the POST reqeust was made to search. Php file with the following data
```
{"search":"london"}
```
The post data appear to be in JSON format, so our request must have specified the content type header to be application/json. It is possible to confirm this by right clicking on the request, and selecting copy>copy request headers.

```
POST /search.php HTTP/1.1
Host: 94.237.62.180:30732
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://94.237.62.180:30732/
Content-Type: application/json
Content-Length: 19
Origin: http://94.237.62.180:30732
Connection: keep-alive
Cookie: PHPSESSID=556ku0f6up41k5tq1dv50t78jq
Priority: u=0
```
We do have content type: application/json. Lets try to replicate this request as we did earlier, but include both the cookie and content-type headers, and send our request to search. Php.

```
curl -X POST -d {"search":"london"} -b 'PHPSESSID=ud8cq5vlk7gc7b1mlbi8ui7ml6' -H 'Content-Type: application/json' http://94.237.62.180:30732/search.php
```
We were able to interact with the search function directly using the curl command, instead of interacting with web app directly. This can be an essential skill when performing web application assessments or bug bounty exercise as it is much faster to test web application this way. 

Finally, we can repeat the same above request by using fetch, 

```
await fetch("http://94.237.62.180:30732/search.php", {
    "credentials": "include",
    "headers": {
        "User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0",
        "Accept": "*/*",
        "Accept-Language": "en-US,en;q=0.5",
        "Content-Type": "application/json",
        "Priority": "u=0"
    },
    "referrer": "http://94.237.62.180:30732/",
    "body": "{\"search\":\"London\"}",
    "method": "POST",
    "mode": "cors"
});
```
Same result can be derived by running the snippet above in console. 


---
## Exercise

Obtain a session cookie through a valid login, and then use the cookie with cUrl to search for the flag

Step 1. Find cookie

```
curl -X post -d 'username=admin&password=admin' IP -v
```

Step 2. Now we can use -b flag on cURL method. 

```
curl -X POST -d '{"search":"flag.txt"}' -b 'PHPSESSID=qcg63btb0c5gpr1jcl47l4vd36' -H 'Content-Type: application/json' http://94.237.62.180:30732/search.php

```

---

## CRUD API

### APIs
Several types of APIs, used to interact with a database, so user would be able to specify the requested table and the requested now within our API query then use an HTTP method to perform the operation needed. For the api. Php endpoint in our example, if we wanted to update the city table in the database, and the row will be updating has a city name of london, then the URL will look like
```
curl -X PUT <ip>:<port>/api/php/city/london ...
```
### CRUD
As we can see, we can easily specify the table and the row we want to perform an operation on through such APIs. Then we may utilize different HTTP methods perform different operaitons on that row. In general APis perfrom 4 main operations on the requestedf database entity.

> [!note] Operation of CRUD
> Create - POST : adds the specified data to the database table
> Read - GET : reads the specified entity from the database table
> Update - PUT : updates the data of the specified database table
> Delete - DELETE : removes the specified row from the database table

These four operations are mainly linked to the commonly known CRUD APIs, but the same principle is also used in REST APIs and several other types of APIs. Of course, not all APIs work in the same way, and the user access control will limit what actions we can perform and what results we can see. 

### Read
The first thing we do when interacting with an API is reading data. As mentioned earlier, we can simply specify the table name after the API and then specify our search team.

```
kwc0827@htb[/htb]$ curl http://<SERVER_IP>:<PORT>/api.php/city/london

[{"city_name":"London","country_name":"(UK)"}]
```
The result is sent as the JSON string. To have it properly formatted, we can pipe the result to the jq utility. We will also silent unneeded cURL output with -s as follows

```
kwc0827@htb[/htb]$ curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq

[
  {
    "city_name": "London",
    "country_name": "(UK)"
  }
]
```

### Create
To add a new entry, we can use an HTTP POST request, which is quite similar ro what we have performed in the previous section. We can simply POST out JSON data, and it will be added to the table. As this API is using JSON datam we will also set the Content-type header to JSON

```
curl -X POST http://<IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```
Now we can read the content of the city we added (HTB_City) to see if it was successfully added,

```
curl -s http://<ip>:<port>/api.php/city/HTB_City | jq

[
  {
    "city_name": "HTB_City",
    "country_name": "HTB"
  }
]
```
Now the new city was created

### Update

PUT and DELETE are included here. PUT is used to to update API entries and modify their details, while DELETE is used to remove a specific entity.

> [!note] Patch
> HTTP Patch method may also be used to update API entries instead of PUT. To be precise, PATCH is used to partially update and entry, while PUT is used to update the entire entry. We may also use the HTTP OPTIONS method to see which of the two is accepted by the server, and then use the appropriate method accordingly. In this section, we will be focusing on the PUT method, through their usage is quite similar. 

Using PUT is quite similar to POST in this case, with the only difference being that we have to specify the name of the entity we want to edit in the URL, otherwise the API will note know which entity to edit. 

```
curl -X PUT http://<ip>:<port>/api.php/city/london -d '{"city_name":"New HTB CITY", "country_name":"HTB"}' -H 'Content-type: application/json'
```
We first specified /city/london as our city, and passed a JSON string that contained “city_name”: “new htb city” in the request data. So the london city should no longer exist, and a new city with the name NEW htb city should exist

### DELETE
```
curl -X DELETE http://<IP>:<PORT>/api.php/city/New_HTB_City
```
To check if it worked,
```
curl -s http://<IP>:<PORT>/api.php/city/NEWHTBCITY | jq
```
As we can see after we deleted New HTB City we get an empty array when we try reading it, meaning it no longer exists.

