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

To get started with, open the port 80 page of the webserver on the target and try to navigate a couple of links on the home page. Observede information disclosure of Apache version. Also concluded that Apache uses php from knowledge and so tried running a couple of tools on port 80 of the target. Ran Nikto to enumerate port 80 and also ran Dirbuster for directory enumeration on port 80 of the target.

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/4.jpg)

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/5.jpg)


With Nikto we got the following output:

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/6.jpg)

With dirbuster found some directory under http://192.168.179.129/usage/ which revealed that Webalizer 2.01 was used on the target web server.

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/7.jpg)

The screenshot of the Webalizer information disclosure is given at the below screenshot.

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/8.jpg)

## Next lets enumerate SMB on the target.


