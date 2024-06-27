1. Recon
    - `nmap -sV -O -sC -T5 -sT -o scan -Pn 10.10.171.212`
    ```bash
    port 22
    port 80: Apache httpd 2.4.18
    ```

2. dirbuster
    -  dirbuster -l /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.10.171.212
    ```bash
    found a database file inside /content/
    going into /content/ shows up a possibly interesting page
    ```
    - searched for more paths inside /content/
    - dirbuster -l /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.10.171.212/content/
    ```bash
    found /content/inc & /content/as
    as -> login page for the admin page of sweet rice
    inc -> contains a bunch of files that could be useful
    ```
    - taking a better look at the mysql database, i found the hash of a password of a user named: 'manager'
    - decrypted it from md5 into plaintext: Password123
    - logged into manager's account in /content/as
    
