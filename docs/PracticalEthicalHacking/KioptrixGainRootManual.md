# Kioptrix Level 1 Gain Root Using Manual Method


We are going to use the exploit-db exploit 764.c for this method. The link for the exploit is given below:

[Exploit-db 764.c exploit](https://www.exploit-db.com/exploits/764)

Compile the C file to get an executable and then run the exploit using the comaptible options, we chose 0x6a and 0x6b as they are compatible with the version of Apache and Red Hat Linux that we have on target.

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/16.png)

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/17.png)

Both the options 0x6a and 0x6b did not yield the shell as it was an outdated exploit. So we have to make the following changes to the file and rerun the exploit:

1. Add Headers:

a. #include <openssl/rc4.h>

b. #include <openssl/md5.h>

2. Update the url:

a. Search for wget

b. Replace with http://packetstormsecurity.nl/0304-exploits/ptrace-kmod.c with https://dl.packetstormsecurity.net/0304-exploits/ptrace-kmod.c

3. Replace parameters:

a. Search for get_server_hello

b. Scroll down to line 961

c. Replace unsigned char *p, *end; with const unsigned char *p, *end;

4. Instead of installing libssl-dev, use the updated package of libssl1.0-dev

After making the above changes compile the new program and execute it.

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/18.png)

Still we dont get a shell and also observed that the exploit is not working due to the fact that we are getting a 404 Page Not Found error for the link which we edited earlier.

I googled a bit and was able to locate the file in question, i.e. ptrace-kmod.c at the below github location:

[Link to ptrace-kmod.c](https://raw.githubusercontent.com/piyush-saurabh/exploits/master/ptrace-kmod.c)

Download the file from above location:

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/19.png)

After this create a local copy of 764.c file and edit the line in the C file that has wget by replacing the link for ptrace-kmod.c with the local path on the attack machine where we downloaded the file to. Start a python http server using the command "python3 -m http.server 8000". Recompile the local C file and rerun the exploit as shown below:

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/20.png)

![Image](https://github.com/vandanarach/TCM-Courses/raw/main/docs/PracticalEthicalHacking/images/21.png)


WE have successfully rooted Kioptrix Level 1 box!




#### <i class="fa-solid fa-circle-arrow-left"></i> [Kioptrix Level 1 Gaining Root using Metasploit](https://vandanarach.github.io/TCM-Courses/PracticalEthicalHacking/KioptrixGainRootMetasploit.html)
####  <i class="fa-solid fa-circle-arrow-right"></i> 

