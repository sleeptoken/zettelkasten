
2024-12-23 16:25

Source: #AOC24 

Tags: [[Wi-Fi Hacking]]

access points (often the routers) that are broadcasting Wi-Fi signals with a unique **SSID** (network name). You can connect to any of these if you know the correct password, also known as a pre-shared key (**PSK**)
### Attacks on Wi-Fi

#### Evil twin attack

- In this attack, the attacker creates a fake access point that has a similar name to one of your trusted Wi-Fi access points.
- The attack starts with the attacker sending de-authentication packets to all the users connected to their legitimate Wi-Fi access points. The users would face repeated disconnections from the network after this. With frustration, the users are likely to open the Wi-Fi access points list for troubleshooting and will find the attacker's Wi-Fi with almost similar name and with greater signal strength. They would go to connect it, and once connected, the attacker could see all their traffic to or from the Internet.
#### Rogue access point

- objective is similar to that of the evil twin attack. 
- In this attack, the attacker sets up an open Wi-Fi access point near or inside the organization's physical premises to make it available to users with good signal strength. 
#### WPS attack

- The attack is made by initiating a WPS (Wi-Fi Protected Setup) handshake with the router and capturing the router's response, which contains some data related to the PIN and is vulnerable to brute-force attacks. 
#### WPA/WPA2 cracking

- While cracking WPA, attackers start by sending de-authentication packets to a legitimate user of the Wi-Fi network.
- Once the user disconnects, they try to reconnect to the network, and a 4-way handshake with the router takes place during this time.
- Meanwhile, the attacker turns its adaptor into monitor mode and captures the handshake. 
- After the handshake is captured, the attacker can crack the password by using brute-force or dictionary attacks on the captured handshake file.
### 4-way handshake

The WPA 4-way handshake is a process that helps a client device (like your phone or laptop) and a Wi-Fi router confirm they both have the right "password" or Pre-Shared Key (PSK) before securely connecting.

Here's a simplified rundown of what happens:
- **Router sends a challenge:** The router (or access point) sends a challenge" to the client, asking it to prove it knows the network's password without directly sharing it.
- **Client responds with encrypted information:** The client takes this challenge and uses the PSK to create an encrypted response that only the router can verify if it also has the correct PSK.
- **Router verifies and sends confirmation:** If the router sees the client’s response matches what it expects, it knows the client has the right PSK. The router then sends its own confirmation back to the client.
- **Final check and connection established:** The client verifies the router's response, and if everything matches, they finish setting up the secure connection.

### The Vulnerability

The vulnerability lies in the fact that an attacker can capture this 4-way handshake if they’re listening when a device connects. With the handshake data, they can use it as a basis to attempt offline brute-force or dictionary attacks. Essentially, they try different possible passwords and test each one to see if it would produce the captured handshake data, eventually cracking the PSK if they get a match.
### Lab

```sh
$ iw dev # This will show any wireless devices and their configuration that we have available for us to use
phy#2
	Interface wlan2 # we would scan for nearby Wi-Fi networks using our wlan2 device
		ifindex 5
		wdev 0x200000001 
		addr 02:00:00:00:02:00 
		type managed 
		txpower 20.00 dBm
```

1. The `addr` is the **MAC/BSSID** of our device. BSSID stands for Basic Service Set Identifier, and it's a unique identifier for a wireless device or access point's physical address.
2. The `type` is shown as **managed**. This is the standard mode used by most Wi-Fi devices (like laptops, phones, etc.) to connect to Wi-Fi networks. In managed mode, the device acts as a client, connecting to an access point to join a network.

```shell
glitch@wifi:~$ sudo iw dev wlan2 scan
BSS 02:00:00:00:00:00(on wlan2)
	last seen: 520.388s [boottime]
	TSF: 1730575383370084 usec (20029d, 19:23:03)
	freq: 2437
	beacon interval: 100 TUs
	capability: ESS Privacy ShortSlotTime (0x0411)
	signal: -30.00 dBm
	last seen: 0 ms ago
	Information elements from Probe Response frame:
	SSID: MalwareM_AP
	Supported rates: 1.0* 2.0* 5.5* 11.0* 6.0 9.0 12.0 18.0 
	DS Parameter set: channel 6
	ERP: Barker_Preamble_Mode
	Extended supported rates: 24.0 36.0 48.0 54.0 
	RSN:	 * Version: 1
		 * Group cipher: CCMP
		 * Pairwise ciphers: CCMP
		 * Authentication suites: PSK
		 * Capabilities: 1-PTKSA-RC 1-GTKSA-RC (0x0000)
	Supported operating classes:
		 * current operating class: 81
	Extended capabilities:
		 * Extended Channel Switching
		 * Operating Mode Notification                                                                               
```

- The **BSSID** and **SSID** of the device are `02:00:00:00:00:00` and `MalwareM_AP` respectively. Since the SSID is shown, this means the device is advertising a network name, which access points do to allow clients to discover and connect to the network.
- The presence of **RSN (Robust Security Network)** indicates the network is using WPA2, as RSN is a part of the WPA2 standard. WPA2 networks typically use RSN to define the encryption and authentication settings.
- The `Group and Pairwise ciphers` are **CCMP**. Counter Mode with Cipher Block Chaining Message Authentication Code Protocol (CCMP) is the encryption method used by WPA2.
- The `Authentication suites` value inside RSN is **PSK** indicating that this is a WPA2-Personal network, where a shared password is used for authentication.
- Another important detail is the `DS Parameter set` value, which shows **channel 6**. The channel, in terms of Wi-Fi, refers to a specific frequency range within the broader Wi-Fi spectrum that allows wireless devices to communicate with each other. There are various Wi-Fi channels, and they all help distribute network traffic across various frequency ranges, which reduces interference. The two most common Wi-Fi channels are 2.4 GHz and 5GHz. In the 2.4 GHz band, channels 1, 6, and 11 are commonly used because they don’t overlap, minimizing interference. In the 5 GHz band, there are many more channels available, allowing more networks to coexist without interference.

#### Switching to monitor mode 


```shell
glitch@wifi:~$ sudo ip link set dev wlan2 down # turn our device off
glitch@wifi:~$ sudo iw dev wlan2 set type monitor # change wlan2 to monitor mode
glitch@wifi:~$ sudo ip link set dev wlan2 up # turn our device back on
```

We can confirm that our interface is in monitor mode with the command `sudo iw dev wlan2 info`.\

**Note:** By default, `airodump-ng` will automatically switch the selected wireless interface into monitor mode if the interface supports it.

1st Terminal

we start by capturing Wi-Fi traffic in the area, specifically targeting the WPA handshake packets

```sh
sudo airodump-ng wlan2 # provides a list of nearby Wi-Fi networks (SSIDs) and shows important details
```

```sh
sudo airodump-ng -c 6 --bssid 02:00:00:00:00:00 -w output-file wlan2 # This command targets the specific network channel and MAC address (BSSID) of the access point for which you want to capture the traffic
```




### References
Day 11