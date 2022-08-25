# Eternal Blue Exploited!

To discover the ip address of the target Blue machine, perform an arpscan and ping to it to check reachablility.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/1.png)


Perform an nmap scan using the below command on the target ip of the Blue machine:

```nmap -sV -A -v -p- 192.168.179.131
```

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/2.png)


Nmap yields the open ports which are 135, 139 and 445 indicating that the machine has SMB service running. Also the OS enumeration tells us that that machine is a Windows 7 Ultimate 7601 Service Pack 1 machine, and its not part of any AD domain.

To enumerate further on SMB shares, try logging in to the smbclient with a anonymous login with blank credentials:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/3.png)


Try logging in to each of the shares and we find that only the IPC$ share allows login with blank creds and however we reach a dead end as we cannot move further as "dir" and all other commands are disabled.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/4.png)


The only option left is to do a google search on the Windows version i.e., Windows 7 Ultimate 7601 SP 1 and we find that "EthernalBlue" keeps popping up in the search for this OS version. So we then launch Metasploit and search for the exploit "EternalBlue" and we find a couple of scanners and exploits. We then choose the exploit for windows and populate the options fields. Running the check command tells us that the target is possibly vulnerable to the exploit.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/5.png)


Running the exploit yields a meterpreter shell when the default payload is used. Issue commands like "shell", "whoami" and going back to the meterpreter shell do a "hashdump" and collect the hashes. We will store these hashes for later post exploitation tasks in future blogs.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/6.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/7.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/8.png)


Now we shall do the manual method of exploiting the machine however we do not get a shell here but effectively we crash the machine which was the intention of the exploit in the first place; to perform a DOS Attack(BSOD). On googling for an exploit on github found the AutoBlue exploit. Cloned the repository and ran the pip command to install the requirements needed for this exploit:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/9.png)


Link to the github repository given below:

[Link to AutoBlue github repo](https://github.com/3ndG4me/AutoBlue-MS17-010)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/10.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/11.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/12.png)


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/13.png)

After that ran the shell_prep.sh file and generated the payload in raw format. Thereafter started the listener using listener_prep.sh. Then ran the python command for the exploiat and got the Blue Screen Of Death!


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/Blue/14.png)








#### <i class="fa-solid fa-circle-arrow-left"></i> [Kioptrix Level 1 Gain Root using Manual Method](https://vandanarach.github.io/TCM-Courses/PracticalEthicalHacking/KioptrixGainRootManual.thtml)
####  <i class="fa-solid fa-circle-arrow-right"></i>

