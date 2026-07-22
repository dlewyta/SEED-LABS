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

Without root permission sniffer.py will not run. This is because all sniffers put the network adapter into promiscuous mode and this requires root permissions. Without root permissions, any user could control and see the full traffic going in and out of your device. Promiscuous mode is a network setting that 

