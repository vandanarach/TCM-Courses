# Scanning and Enumeration Kioptrix Level 1




After powering up the Kioptrix Level-1 machine in VMWARE, it obtains an ip address through DHCP with NAT enabled on the VM.

Issue "ifconfig" command on the Attacker machine and note down the subnet of your network:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/zero.jpg)


Perform an ARP scan using the following netdiscover command:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/one.jpg)


From the arp scan identify the ip address of the Kioptrix machine and perform an nmap scan on it:


![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/two.jpg)

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/three.jpg)


