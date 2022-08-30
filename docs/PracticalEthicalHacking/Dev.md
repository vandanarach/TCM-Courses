# Hacking Dev!

To discover the ip address of the target Academy machine, perform an arp-scan and ping to it to check reachablility.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/1.png)


Perform an nmap scan using the below command on the target ip of the Academy machine:

```
nmap -sV -A -v -p- 192.168.179.166

```

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/2.png)


Nmap yields the open ports which are 22, 80, 111, 2049, 8080 and others indicating that the machine has services - SSH, NFS and HTTP running on it. Also the OS enumeration tells us that that machine is a Linux Debian machine.


## HTTP port 80 and port 8080 Enumeration


To enumerate further navigate to HTTP session. Simultaneously run dirb tool to enumerate the directories on port 8080. Some of the interesting directories found are:


```
index.php
/dev/config/
/dev/forms/
/dev/pages/
/dev/files/
/dev/index.php
```


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/3.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/4a.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/4.png)


The page index.php shows Apache 2.4.38 running on target machine and the user was www-data.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/6.png)


The /dev/index.php page has a login page link on it.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/7.png)


The http page /dev/pages listed a few files. Also the page /dev/pages/ has a member.admin page which lists a password "I_love_java". Saving it for use later.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/5.png)


## Gaining Initial Access


Tried logging into the login page using the credentials "admin/I_love_java". The login was successful and there was a create page on the web server of the target. Downloaded the php reverse shell code from the pentestmonkey website and copy pasted the code in the create page with the name of the file as shell.php.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/8.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/9.png)


Navigated to /dev/pages/ directory and found the shell.php file uploaded there. Clicked on it after starting a netcat listener and it spawned a shell to the target machine with the user "www-data". Did a "cat /etc/passwd" on the target and found a user "jeanpaul".


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/10.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/11.png)


## Privilege Escalation


Did a "cat /etc/exports" to find the list of shares on the target machine. Found the following shares on it and on checking the shared folder on the target found the file "save.zip" on it.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/12.png)


Started a python3 http server on the target machine and uploaded the file to the attacker machine.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/13.png)


Tried unzipping the file and found that it was password protected. Used zip2john to extract the password for the zipped files one at a time. Used john with the wordlist "rockyou.txt" on the extracted hash file from zip2john. The zip file password was "java101".


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/14.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/15.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/16.png)


Unzipped the file save.zip giving the password "java101" and extracted 2 files "id_rsa" and "todo.txt".


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/17.png)


Changed the file permissions of id_rsa to 600.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/18.png)


Logged into the target using ssh and the username "jeanpaul" with the id_rsa file and gave the passphrase for it as "I_love_java". After logging in issued "sudo -l" and found the binary /usr/bin/zip can be executed without a password. Navigated to the GTFO.bins page and got the code snippet for zip with sudo and issued the commands on the shell and thus rooted the box.


[GTFO Bins](https://gtfobins.github.io/gtfobins/zip/#shell)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Dev/19.png)







#### <i class="fa-solid fa-circle-arrow-left"></i> [Hacking Academy](https://vandanarach.github.io/TCM-Courses/PracticalEthicalHacking/Academy.html)
####  <i class="fa-solid fa-circle-arrow-right"></i>[Escalating Butler](https://vandanarach.github.io/TCM-Courses/PracticalEthicalHacking/Butler.html)

