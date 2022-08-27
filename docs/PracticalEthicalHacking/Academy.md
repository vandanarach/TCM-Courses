# Hacking Academy!

To discover the ip address of the target Academy machine, perform an arp-scan and ping to it to check reachablility.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/1.png)


Perform an nmap scan using the below command on the target ip of the Academy machine:

```
nmap -sV -A -v -p- 192.168.179.132

```

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/2.png)


Nmap yields the open ports which are 21, 22 and 80 indicating that the machine has services - FTP, SSH and HTTP running on it. Also the OS enumeration tells us that that machine is a Linux Debian machine.


## FTP Enumeration


To enumerate further login to FTP. The nmap scan shows us that the anonymous login is allowed and it also locates a file called "note.txt" on the FTP server. Do a get on the file on FTP server after logging in usinf anonymous login.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/3.png)

Read the contents of the downloaded file "note.txt" and observed a message was left for "Heath" from "jdelta" user and also a mention of another user called "grimmie". Also there was an SQL command displayed in the file that gave up the login credentials of user "Rum Ham". The credentials are however hashed and look suspiciously like a md5 hash.

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/4.png)


On decrypting the has using an online tool observed that the login credentials were "10201321/student" for user "Rum Ham"


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/5.png)


## HTTP port 80 enumeration


To enumerate port 80, ran dirbuster tool, searching for php files, as the homepage as per nmap scan "works" and thus reveals an apache server. The directory enumeration yields a lot of interesting dirs as shown below:

```
/academy/index.php
/academy/admin/index.php
/academy/includes/config.php
/academy/admin/includes/config.php
/phpmyadmin/index.php
```


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/6.png)

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/8.png)

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/9.png)


## Exploitation using enumerated details


Login to the /academy/index.php page of the web server on target using the credentials of "Rum Ham" as obtained from "note.txt".


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/7.png)

On the myprofile page of the successful login above we see that there is an option to upload a picture. Just to give it a shot we can upload a file and check. Upload here a reverse shell php file as obtained from pentestmonkey:


[Pentestmonkey PHP Reverse shell file link](https://web.archive.org/web/20200919070309/http://pentestmonkey.net/tools/web-shells/php-reverse-shell)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/10.png)


The file was successfully uploaded as seen in below screenshot on the server using directory traversal:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/11.png)


On upload itself we can get a reverse shell on our attacker machine to the target, however since I forgot to start the listener I just navigated to the folder using directory traversal and clicked on the reverse shell file, to obtain the reverse shell.



![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/12.png)


## Privilege Escalation


Now that we got our initial login shell, we can explore on how to escalate our privileges and obtain root shell on this target. For this, uploaded a script called "linpeas.sh" from the attacker machine using python http server, and executed it. Found lots of interesting information on the target.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/13.png)


The same file that we found through dirbuster, config.php popped up here as well.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/14.png)


There was also a backup.sh file in the /home/grimmie folder:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/15.png)


There was a mysql password stored on the machine in a config.php file.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/16.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/17.png)


Did a cat on the config.php file to obtain username along with the above password revelation:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/18.png)


Just to give it a try used ssh to login using these credentials as according to the linpeas file there is a /home/grimmie directory available. The login was successful and could see a backup.sh file in the home directory of the user. 


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/19.png)


On doing a cat on the backup.sh file found that it was doing a backup of the files in the /var/www/html/academy/includes directory. Just as a guess checked the /etc/crontab file and found that this backup.sh file was being run periodically to backup all the files.
Since this is was a bash shell script, navigated to below link to get a code snippet to add to the backup.sh file so as to obtain root shell.

[GTFO Bins](https://gtfobins.github.io/gtfobins/bash/#reverse-shell)


Edited the backup.sh file for the bash commands on GTFO Bins and voila! successfully obtained root access on the netcat listener. Navigated to the /root directory and captured the flag!!


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/20.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Academy/21.png)




#### <i class="fa-solid fa-circle-arrow-left"></i> [Eternal Blue Exploited!](https://vandanarach.github.io/TCM-Courses/PracticalEthicalHacking/Blue.html)
####  <i class="fa-solid fa-circle-arrow-right"></i>[Hacking Dev!](https://vandanarach.github.io/TCM-Courses/PracticalEthicalHacking/Dev.html)

