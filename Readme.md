# Mr Robot CTF

📡 **[https://tryhackme.com/room/mrrobot](https://tryhackme.com/room/mrrobot)**

## One percent of the one percent

First of all, let's obey the one percent of the one percent and then as always scan the ip with nmap.

```bash
sudo nmap -sS -sV -O --script vuln HOST-IP -oN ./output_vuln.txt
```

- -sS : This is TCP SYN scan and it is looking that if the port ready for connection.
- -sV : Service and version information.
- -O : Operating System Detection.
- —script vuln : Looking for vulnerabilities on host by nmap script.
- -oN : Scan output to a file.

[Nmap Scan Output](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Nmap%20Scan%20Output%2038bf357171f348b986c5bca710bd0891.md)

We have a webserver on the host and there is an ssh port closed.

Let's check the website.

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled.png)

I am checked robots.txt file of the site.

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%201.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%201.png)

There are 2 files and one of them is key-1-of-3.txt

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%202.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%202.png)

We've found our first key.

I am looking at other .dic file and there is huge wordlist.

As I can see in my nmap scan, there was login page.

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%203.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%203.png)

I will try to login with elliot as username and password as this list.

I was trying this with burpsuite but it was so slow. So I will use hydra.

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%204.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%204.png)

It is giving a message about username password. So elliot username is probably true but what is the password? 

I am researching about sending http-post with hydra. Found this.

[https://github.com/gnebbia/hydra_notes](https://github.com/gnebbia/hydra_notes)

```bash
hydra -l elliot -P /home/kali/Desktop/Labs/TryHackMe/Mr_Robot_CTF/fsocity.dic 10.10.95.108 http-post-form "/wp-login/:log=^USER^&pwd=^PASS^:F=The password you entered for the username"
```

This gives me the password. What am I doing here exactly is trying password with username elliot, I am using http-post method because this is a website login panel. My post request body contains username password. My failure string is written because I tested that in the screenshot above. It takes so long I will prepare coffee myself. Enjoy waiting.

When it finds the password I am logging in wordpress admin panel. 

We can use wpscan tool for scanning wordpress and gethering information about it.

```bash
wpscan --url http://10.10.25.207 -t 50 -U elliot -P ./fsocity.dic
```

- -U : Username
- -P : Password

There it is!

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%205.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%205.png)

Now we need to get acces to operating system. I am researching about accessing reverse shell over wordpress. I found this [https://www.hackingarticles.in/wordpress-reverse-shell/](https://www.hackingarticles.in/wordpress-reverse-shell/)

There are two type of methot that we can use. One of them is changing a php page to php reverse shell code. Other method is using metasploit framework.

Let's try metasploit.

```bash
search wordpress admin
```

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%206.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%206.png)

I will use that number 2, I am going to upload shell and I am going to access the server over that.

```bash
use 2
info
```

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%207.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%207.png)

It is the information page for the module. There are informations for what parameter needs to set. 

- It needs Password to authenticate with.
- It needs Target Host IP
- It needs Target Host Port because http service port could be different
- It needs Username for authenticating.
- It needs TargetURI which is "/". This is the path of worpress application.
- It needs Lhost for [localhost](http://localhost) to set reverse shell adress.

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%208.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%208.png)

There is an error. I know it is using worpress but module cannot see this.

I need to check advanced options

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%209.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%209.png)

Module is trying to check if the website using wordpress.

I am going to disable it.

```bash
set WPCHECH false
run
```

We have another error. There is easier way but lots of people showed that on their writeups.

I will try the hard way.

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2010.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2010.png)

Actually this plugin is installed when I checked the plugin page but module is not detecting it.

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2011.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2011.png)

Ok if there is no error and it is installing, let's try to pass checking part of the module code.

I will edit the module code.

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2012.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2012.png)

I am going to edit with vscode editor. 

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2013.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2013.png)

There is a code that file_with, it is checking if plugin is uploaded or not. I commented that row, save this code like this and reload the module.

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2014.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2014.png)

and done!

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2015.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2015.png)

We are in! 

I am going to /home directory and going into the user files. There is a key file and password file.

```bash
cd /home/robot
ls -al
```

as we can see we can only read password file. We need to crack it because there is robot username and md5 hashed password.

Use [https://crackstation.net/](https://crackstation.net/)

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2016.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2016.png)

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2017.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2017.png)

we need to change user to robot. Write "shell" command to change meterpreter to normal shell

```bash
shell
python -c 'import pty; pty.spawn("/bin/bash")'
cat key-2-of-3.txt
```

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2018.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2018.png)

Now I am "robot" and I know the second key.

I need to escalate my privileges and be root.

```bash
find / -perm -u=s -type f 2>/dev/null
```

This code gives you suid bit list. Check the [https://gtfobins.github.io/#+suid](https://gtfobins.github.io/#+suid) site with this listed programs.

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2019.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2019.png)

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2020.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2020.png)

![Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2021.png](Mr%20Robot%20CTF%200fa81336be4741df90a38eb1336fb19b/Untitled%2021.png)

I use the interactive mod commands and I am root now. Check the root folder and take third key.

```bash
cd /root
cat key-3-of-3.txt
```

Done!
