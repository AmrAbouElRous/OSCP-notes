# UDP
- UDP (User Datagram Protocol) is a connectionless, unreliable protocol that prioritizes speed by sending data quickly without guaranteeing delivery or order. In contrast, TCP (Transmission Control Protocol) is a connection-oriented, reliable protocol that establishes a connection, sequences data, and ensures all packets are delivered in the correct order with error checking, making it slower but more accurate.

Target (192.168.1.4) - Attacker (192.168.1.5) - Wireshark packets

## ✅ UDP Behavior in Wireshark (Open, Closed, Filtered)

### 1️⃣ When connecting to an OPEN UDP port
- The server does NOT reply (UDP has no handshake).
- If you send a message like "hi":
- Wireshark shows your UDP packet with a length value (Len=3).
- No response from the server unless the application explicitly sends something back.
### 2️⃣ When connecting to a CLOSED UDP port
- You send a UDP message.
- The target OS replies with:
- ICMP Destination Unreachable – Port Unreachable
### 3️⃣ When connecting to a FILTERED UDP port
- You send a UDP message.
- Firewall drops it, and also drops the ICMP error.
- Wireshark shows only your UDP packet, same as open port.
- No response at all.
- ⚠️ This is the tricky part:
- Filtered UDP looks the same as open UDP in Wireshark.

## Example 1 , Port is Open
Target:192.168.1.4 udp:4444
```bash
msfadmin@metasploitable:~$ nc -nuvl -p 4444 -w 1000
listening on [any] 4444 ...
connect to [192.168.1.4] from (UNKNOWN) [192.168.1.5] 33875
hi

```
Attacker:192.168.1.5 udp:4444
```bash
┌──(amro㉿amro)-[~]
└─$ nc -nuv 192.168.1.4 4444
(UNKNOWN) [192.168.1.4] 4444 (?) open
hi
```
Wireshark
No	Time		Source		Destination   Protocol length   info
15	54.075502774	192.168.1.5	192.168.1.4	UDP	45	33875 → 4444 Len=3

- UDP port is Open --> no reply

## Example 2 , Port is closed
- Target:192.168.1.4
- Netstat shows that there`s no udp port 4444 open
```bash
msfadmin@metasploitable:~$ netstat -lu
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
udp        0      0 *:nfs                   *:*                                
udp        0      0 192.168.1.4:netbios-ns  *:*                                
udp        0      0 *:netbios-ns            *:*                                
udp        0      0 192.168.1.4:netbios-dgm *:*                                
udp        0      0 *:netbios-dgm           *:*                                
udp        0      0 *:42137                 *:*                                
udp        0      0 192.168.1.4:snmp        *:*                                
udp        0      0 localhost:snmp          *:*                                
udp        0      0 192.168.1.4:domain      *:*                                
udp        0      0 localhost:domain        *:*                                
udp        0      0 *:bootpc                *:*                                
udp        0      0 *:tftp                  *:*                                
udp        0      0 *:979                   *:*                                
udp        0      0 *:51032                 *:*                                
udp        0      0 *:38493                 *:*                                
udp        0      0 *:sunrpc                *:*                                
udp        0      0 *:60020                 *:*                                
udp6       0      0 [::]:domain             [::]:*                             
udp6       0      0 [::]:52814              [::]:* 
```
Attacker 192.168.1.5
```bash
┌──(amro㉿amro)-[~]
└─$ nc -nuv 192.168.1.4 4444
(UNKNOWN) [192.168.1.4] 4444 (?) open
hi
      
```
Wireshark 
```bash
No      Time             Source          Destination   Protocol   length    info
209	130.079457968	 192.168.1.5	 192.168.1.4	 UDP	    45	    38470 → 4444 Len=3
210	130.079614039	 192.168.1.4	 192.168.1.5	 ICMP	    73	    Destination unreachable (Port unreachable)
```
