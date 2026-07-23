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
<img width="1841" height="732" alt="image" src="https://github.com/user-attachments/assets/ba227440-dafe-4ddb-b1ea-7ca95c5b9936" />
The image shown is the results of one packet when the command "Telnet 10.9.0.5" is ran while the sniffer is running. Upon running the Telnet command I was asked to log in. I did this by using the username "seed" and the password "dees". The reason I ran a Telnet command is because port 23 is the port for Telnet
The packets printed only run on a given TCP through port 23 (Telnet). When I tried to send some data through a different port (port 80 for HTTP) I got the following results:
<img width="844" height="84" alt="image" src="https://github.com/user-attachments/assets/6de39e48-d01e-4575-80e5-09f9909aa9cf" />

If I try to use the machine home IP or really any other IP than the specified IP on the correct port (port 23) I get the following response. Even though the connection is succussful, no data is sniffed because the IP address is incorrect.
<img width="727" height="51" alt="image" src="https://github.com/user-attachments/assets/02a49441-b928-4565-be82-f7b5d16bfd31" />

The code I used is very similar to sniffer.py. I simply added in a bpf_filter line which takes tcp (so only tcp connection works). It also sets the destination host to 10.9.0.5 (the container for host A, this can be changed I just used 10.9.0.5 as an example) and I set the destination port to 23. This is done because the data being sent ends up going to the set IP address and port 23. If I set the host 10.9.0.5 to src (source) then it fails because that would block packets sent from 10.9.0.5 to some other address.
<img width="1052" height="301" alt="image" src="https://github.com/user-attachments/assets/e55656b8-2db5-414c-b2d2-86ad597d1d35" />

**C. Capture packets coming from or going to a particular subnet. You can pick any subnet, such as 128.230.0.0/16; you should not pick the subnet that your VM is attached to**
<img width="1836" height="722" alt="image" src="https://github.com/user-attachments/assets/f753a41e-b886-4a53-9dab-bf2e6ca35e06" />
The packet shown is from the subnet 128.230.0.0, and the IP 128.230.0.11/16 is pinged. 

<img width="762" height="306" alt="image" src="https://github.com/user-attachments/assets/b9110d10-804e-432c-92f7-ed2887122845" />
The code I used was just like the last 2 examples, only this time I added in the line subnet=('net 128.230.0.0/16') and made that the filter. In order to run the program successfully I needed to put one terminal in the attacker container and run subnet_sniffer.py, and open the host B container in another terminal and run the command ping 128.230.0.11. There is no reason I chose 128.230.0.11, it could have been any other value as long as it was in the same subnet and existed (i.e. 128.230.0.1)

