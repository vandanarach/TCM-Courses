# Scanning and Enumeration Kioptrix Level 1




After powering up the Kioptrix Level-1 machine in VMWARE, it obtains an ip address through DHCP with NAT enabled on the VM.

Issue "ifconfig" command on the Attacker machine and note down the subnet of your network:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/zero.jpg)


Perform an ARP scan using the following netdiscover command:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/one.jpg)


From the arp scan identify the ip address of the Kioptrix machine and perform an nmap scan on it:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/two.jpg)

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/three.jpg)

In the nmap scan we found many open ports like 22, 80, 111, 443, 139, 32768. 



## Lets begin by enumerating ports 80 and 443.

To get started with, open the http page on port 80 of the webserver on the target and try to navigate a couple of links on the home page. Observed information disclosure of Apache version on one of the pages. Also concluded that Apache uses php from knowledge and so tried running a couple of tools on port 80 of the target. Ran Nikto for application enumeration and also ran Dirbuster for directory enumeration on port 80 of the target.

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/4.jpg)

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/5.jpg)


With Nikto we got the following output:

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/6.jpg)

With dirbuster found some directory under http://192.168.179.129/usage/ which revealed that Webalizer 2.01 was used on the target web server.

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/7.jpg)

The screenshot of the Webalizer information disclosure is given at the below screenshot.

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/8.jpg)




## Enumerating SMB


From the nmap scan we can see that SMB version 2 is running however we are not really sure as of now. However when we run enum4linux tool on the target we see that anonymous login is allowed.

So we can try connecting to the SMB server on the target using anonymous login.

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/9.jpg)

Tried login to the SMB server with anonymous login and found that there are 2 shares available on server, IPC$ and ADMIN$. We were able to login to IPC$ with anonymous login but could not get further. With ADMIN$ share we were kicked out of trying to login itself.


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/10.jpg)


We can use Metasploit for SMB Auxillary scanner to enumerate the version of Samba:

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/10.5.jpg)




## Enumerating SSH


Enumerating SSH is as much as just trying to login to SSH port on the target as in some cases we would get a login banner that can help us with some information due to disclosure. Here we found that since the machine is an old one, the algorithms and the cipher were not matching and we had to enter them manually to try and login. 


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/11.jpg)




## Researching Potential Vulnerabilities


Google is our best tool for researching on the potential vulnerabilities and then followed by searchsploit.

1. We can first google for the port 80/443 vulnerabilities based on Apache mod_ssl 2.8.4 version and we came across the following exploits:

[Link](https://www.exploit-db.com/exploits/764)

[Link](https://github.com/heltonWernik/OpenLuck)

In addition to this, searchsploit reveals a couple of exploits:

![Image]((https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/12.jpg)


2. Next we can google for the Samba server version that we detected using Metasploit auxillary scanner for version 2.2.1a. The following exploits were found:

[Link](https://www.exploit-db.com/exploits/10)

[Link](https://www.rapid7.com/db/modules/exploit/linux/samba/trans2open/)

[Link](https://www.exploit-db.com/exploits/7)


And searchsploit listed the following output:

![Image]((https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/13.jpg)


This concludes this blog. In the next blog we shall see Vulnerability Scanning using Nessus. See you there!







[Link]()

