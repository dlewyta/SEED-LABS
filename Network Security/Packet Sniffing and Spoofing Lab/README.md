# SEED LABS - Network Security - Packet Sniffing and Spoofing Lab

## Task Set 1: Using Scapy to Sniff and Spoof Packets

### 1.1A
#### With Root Permission
<img width="1832" height="722" alt="image" src="https://github.com/user-attachments/assets/a28cd948-c9c4-4464-9135-a07e1068bcf2" />
<img width="1833" height="717" alt="image" src="https://github.com/user-attachments/assets/0d7eecc5-e98d-4e95-98a2-80c590adb7cd" />
<img width="1838" height="731" alt="image" src="https://github.com/user-attachments/assets/8b8f8a2a-c9e4-4fe2-a754-174853ba17ae" />
<img width="1829" height="713" alt="image" src="https://github.com/user-attachments/assets/13e3e633-50c5-4619-8cb8-d03edda8663c" />

With root permission enabled, sniffer.py will show different packets that are sent to and from the host IP. I used sniffer.py to sniff for packets and I tested the program on two different Ip addresses. First I pinged 8.8.8.8 (Google Public DNS), and then I pinged 1.1.1.1 (CloudFlare DNS). Running sniffer.py will show the packets structural layers and field values. This means you will see Ethernet data (dst, src, type), IP data (version, ihl, id, chksum, src, dst, etc) and ICMP data (type, code, chksum, etc). For each packet, scapy will display the data in both directions. Although, this has nothing to do with the TCP handshake and instead, because scapy scans all packets being sent somewhere even if it is the same packet but from a different src/dst.

#### Without Root Permission
<img width="1268" height="388" alt="image" src="https://github.com/user-attachments/assets/08b92efe-b6a2-468d-8279-4af1c09a6d94" />

Without root permission sniffer.py will not run. This is because all sniffers put the network adapter into promiscuous mode and this requires root permissions. Without root permissions, any user could control and see the full traffic going in and out of your device. Promiscuous mode is a network setting that allows a device to intercept and read all data packets passing through its network adapter (rather than just those data packets addressed to it).

### 1.1B

**A. Only capture ICMP packets**
#### IPv4 Packet
<img width="773" height="65" alt="image" src="https://github.com/user-attachments/assets/573d11fd-65c9-4740-a15c-44764bf13f71" />

#### Ipv6 Packets
<img width="1835" height="593" alt="image" src="https://github.com/user-attachments/assets/f5f77d90-210d-4625-a507-f7dc72978c78" />
<img width="927" height="615" alt="image" src="https://github.com/user-attachments/assets/b6825b2c-3954-4cd6-93c8-0cb03d2916d6" />




the command for capturing ICMP packets in BPF syntax is "icmp" for IPv4, or "icmp6" for IPv6. When ICMP for IPv4 is run the results are exactly like in 1.1A. However, when only ICMP is filtered for only IPv6 packets there is nothing from the test I ran. Instead When I run Google's public DNS (2001:4860:4860::8888). When doing this 3 parts came up: An ICMPv6 Echo Request, ICMPv6 Neighbour Discovery, ICMPv6 Neighbor Discovery option.
An ICMPv6 Echo Request is a network diagnostic message that is sent from one device to another to make sure it is online and reachable.
An ICMPv6 Neighbour Discovery is a part of the Neighbor Discovery Protocol for IPv6 and allows two devices on the same local network to determine each others MAC address, find routers and verify active connections (It is the IPv6 equivilant of IPv4's ARP)
An ICMPv6 Neighbour Discovery Option is a variable-length data field unit within ICMPv6 messages. They are primarily used to share MAC addresses, provide specific link parameters (like MTUs), and support IPv6 Stateless Address Autoconfiguration (SLAAC)

**B. Capture any TCP packet that comes from a particular IP and with a destination port number 23**
