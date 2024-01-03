# This file explains what a subnet mask is and how to calculate it for your specific needs.

### What is a subnet mask?
The subnet mask is a 32-bit number that specifies what portions of the IP address represent the network and which represent the host.
The bits have the following meaning:
- '1' bits represent the network portion of the IP address.
- '0' bits represent the host portion of the IP address.
The subnet mask must be contiguous. That is, all '1' bits must be on the left and all '0' bits must be on the right.
When an IP address is combined with its subnet mask, the network and host portions are clearly defined. This division allows routers to determine which part of the IP address represents the destination network and which part represents the specific host within that network.

### Special Adresses
The “255” address is always assigned to a broadcast address, and the “0” address is always assigned to a network address. Neither can be assigned to hosts, as they are reserved for these special purposes.

### Network Classes based on Subnet Mask
The 32 binary bits are divided into the host and network sections by the subnet mask but they are also broken into four 8-bit octets.
There are 3 classes of network subnetting, based on the portion of the IP address that is used for the network/host:
```
________________________________________________________________________________________________________
Class     |  Subnet Mask  | Network Bits  |  Host Bits  |  Number of Networks  |     Number of Hosts
A         |   255.0.0.0   |       8       |      24     |        126           |    65,536 - 16,777,214
B         |  255.255.0.0  |      16       |      16     |       16,382         |    256    - 65,534
C         | 255.255.255.0 |      24       |       8     |      2,097,150       |    0      - 254
__________|_______________|_______________|_____________|______________________|________________________

```

### CIDR Notation (IP Masks)
IPv4 addresses represented in CIDR notation (Classless Inter-Domain Routing) are called network masks, and they specify the number of bits in the prefix to the address after a forward slash (/) separator.
An Example would be an *"IP Mask"* (combination of IP address and subnet mask) such as: 10.0.1.1/24
This IP Mask is equivalent to and IP address of 10.0.1.1 and subnet mask of 255.255.255.0

### How to calculate the subnet mask
A useful Netmask calculator can be found [here](https://jodies.de/ipcalc?host=192.168.0.1&mask1=16&mask2=)