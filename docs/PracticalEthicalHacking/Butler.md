# Escalating Butler

To discover the ip address of the target Academy machine, perform an arp-scan and ping to it to check reachablility.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/1.png)


Perform an nmap scan using the below command on the target ip of the Academy machine:

```
nmap -sV -A -v -p- 192.168.179.167

```

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/2.png)


Nmap shows open port 8080 and others indicating that the machine has HTTP running on it. Also the OS enumeration tells us that that machine is a Windows one.


To enumerate further navigate to HTTP session. Observed that Jenkins is running on the system. Only way to get in an initial access is to brute force the credentials of Jenkins.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/3.png)


## Gaining Initial Shell Access



Start Burp Suite and turn on proxy and login to the Jenkins page with a random username and password. The proxy intercepts the page and we can forward the login page to Intruder on Burp Suite.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/4.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/5.png)


Using ClusterBomb on BurpSuite Intruder tool, perform brute forcing of Jenkins credentials:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/6.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/7.png)


The credentials are "jenkins/jenkins" and we were able to successfully login to the website.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/8.png)


Exploring the Jenkins dashboard we find Script console and on googling about it we find an exploit.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/9.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/10.png)


We get a link to the github exploit for Script Console on Jenkins:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/11.png)


[Github Reverse shell exploit repository](https://gist.github.com/frohoff/fed1ffaab9b9beeb1c76)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/12.png)


[Reverse shell raw code](https://gist.githubusercontent.com/frohoff/fed1ffaab9b9beeb1c76/raw/7cfa97c7dc65e2275abfb378101a505bfb754a95/revsh.groovy)


Running the reverse shell code in Script console with a netcat listener running on attack machine we get a reverse shell.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/14.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/15.png)



## Privilege Escalation



To explore the possible exploits to get an escalated shell, we can run winPEAS.exe on the windows initial shell. For this we upload the winPEASany.exe file to the windows machine using certutils.exe.


[Link to winPEAS.exe release](https://github.com/carlospolop/PEASS-ng/releases/tag/20220828)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/13.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/16.png)


Execute winpeas on initial shell and enumerate the possible exploits. One thing that stands out is the tokens. We can abuse the tokens for which we have privileges and get an escalated shell.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/17.png)


From the image below we can see that we have access enabled to a token called SeImpersonatePrivilege.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/18.png)


On googling about privilege escalation abusing the token SeImpersonatePrivilege, we find the below link with links to the exploits.


[Link to the list of exploits for exploiting tokens for LPE](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/privilege-escalation-abusing-tokens)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/19.png)


[Link to PrintSpoofer64.exe exploit](https://github.com/itm4n/PrintSpoofer)

[Link to exploit exe file](https://github.com/itm4n/PrintSpoofer/releases/download/v1.0/PrintSpoofer64.exe)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/20.png)


Upload the exe file to the target windows machine and execute it to get an privilege escalated shell of NT System Authority:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/21.png)


We have thus successfully got escalated privileges on Windows Jenkins machine.

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Butler/22.png)








#### <i class="fa-solid fa-circle-arrow-left"></i> [Hacking Dev!](https://vandanarach.github.io/TCM-Courses/PracticalEthicalHacking/Dev.html)
####  <i class="fa-solid fa-circle-arrow-right"></i>

