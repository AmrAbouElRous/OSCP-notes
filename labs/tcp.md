## TCP
**Transmission Control Protocol (TCP)**  
- Reliable, connection-oriented protocol  
- Ensures data is delivered in order, without loss  
- Allows two devices to establish and manage a communication session
---

### TCP 3-Way Handshake
```
       Client                      Server
         ___      SYN(seq:100,ACK:0)        ___
        |   | -------------------------->  |   |
        |___|                              |___|

         ___      SYN/ACK(seq:600,ACK:101)  ___
        |   | <--------------------------  |   |
        |___|                              |___|

         ___      ACK(seq:101,ACK:601)      ___
        |   | -------------------------->  |   |
        |___|                              |___|

```

---
#### 192.168.1.4 (Target) listening
```
# you can do -w 500 to make the port open for 500 seconds
# you can do nc -nluvp 4444 -w 500 if you want UDP
msfadmin@metasploitable:~$ nc -nlvp 4444
listening on [any] 4444 ...
connect to [192.168.1.4] from (UNKNOWN) [192.168.1.5] 55838
hi
how ware you
i am hacker hahahaha
```

#### 192.168.1.5 (Attacker)
```
┌──(amro㉿amro)-[~]
└─$ nc 192.168.1.4 4444
hi
how ware you
i am hacker hahahaha
```

---

### Experiment on Wireshark Walkthrough
#### Step 1 (Target 192.168.1.4 opens a port)
```
msfadmin@metasploitable:~$ nc -nlvp 4444 -w 700
listening on [any] 4444 ...
connect to [192.168.1.4] from (UNKNOWN) [192.168.1.5] 54226
```

#### Step 2 (Attacker)
```bash
sudo wireshark
# tcp.port == 4444 then apply in the filter then run and wait
```

#### Step 3 (Attacker scanning)
```bash
# -z zero I/O used for scanning
nc -nv -z 192.168.1.4 4444
(UNKNOWN) [192.168.1.4] 4444 (?) open
```

Now return to Wireshark and observe the packets.

---
### Sample Wireshark Output
```
No.	Time		Source		Destination  Protocol  Length 	Infor
234	199.530007429	192.168.1.5	192.168.1.4	TCP	74	47812 → 4444 [SYN] Seq=0 Win=64240 Len=0 MSS=1460 SACK_PERM TSval=1290731211 TSecr=0 WS=128
235	199.530181529	192.168.1.4	192.168.1.5	TCP	74	4444 → 47812 [SYN, ACK] Seq=0 Ack=1 Win=5792 Len=0 MSS=1460 SACK_PERM TSval=553294 TSecr=1290731211 WS=32
236	199.530201899	192.168.1.5	192.168.1.4	TCP	66	47812 → 4444 [ACK] Seq=1 Ack=1 Win=64256 Len=0 TSval=1290731211 TSecr=553294
237	199.530262569	192.168.1.5	192.168.1.4	TCP	66	47812 → 4444 [FIN, ACK] Seq=1 Ack=1 Win=64256 Len=0 TSval=1290731211 TSecr=553294
238	199.530391389	192.168.1.4	192.168.1.5	TCP	66	4444 → 47812 [FIN, ACK] Seq=1 Ack=2 Win=5792 Len=0 TSval=553294 TSecr=1290731211
239	199.530414869	192.168.1.5	192.168.1.4	TCP	66	47812 → 4444 [ACK] Seq=2 Ack=2 Win=64256 Len=0 TSval=1290731211 TSecr=553294
```

---
#### Useful commands
```
# on the target machine to show all open ports
netstat -nl   # all ports
netstat -nlt  # TCP only
netstat -nlu  # UDP only

#connect to somewhere:   nc [-options] hostname port[s] [ports] ... 
#listen for inbound:     nc -l -p port [-options] [hostname] [port]
# difference between -p port or putting th port at the end , putting port at the end mean its the destination port
```

---
### Another Example — TCP port 55555 on Metasploitable2
```
msfadmin@metasploitable:~$ nc -nlvp 55555 -w 1000
listening on [any] 55555 ...
connect to [192.168.1.4] from (UNKNOWN) [192.168.1.5] 37224
hi
```

```
┌──(amro㉿amro)-[~]
└─$ nc -nv 192.168.1.4 55555
(UNKNOWN) [192.168.1.4] 55555 (?) open
hi
^c
```

Wireshark Filter: **tcp.port == 55555**
```bash
No.	Time		Source		Destination  Protocol  Length 	Infor
13	21.143064595	192.168.1.5	192.168.1.4	TCP	74	37224 → 55555 [SYN] Seq=0 Win=64240 Len=0 MSS=1460 SACK_PERM TSval=1302828536 TSecr=0 WS=128
14	21.143225285	192.168.1.4	192.168.1.5	TCP	74	55555 → 37224 [SYN, ACK] Seq=0 Ack=1 Win=5792 Len=0 MSS=1460 SACK_PERM TSval=1762920 TSecr=1302828536 WS=32
15	21.143246055	192.168.1.5	192.168.1.4	TCP	66	37224 → 55555 [ACK] Seq=1 Ack=1 Win=64256 Len=0 TSval=1302828536 TSecr=1762920
20	27.578219482	192.168.1.5	192.168.1.4	TCP	69	37224 → 55555 [PSH, ACK] Seq=1 Ack=1 Win=64256 Len=3 TSval=1302834971 TSecr=1762920
21	27.578366489	192.168.1.4	192.168.1.5	TCP	66	55555 → 37224 [ACK] Seq=1 Ack=4 Win=5792 Len=0 TSval=1763563 TSecr=1302834971
754	796.260735921	192.168.1.5	192.168.1.4	TCP	66	37224 → 55555 [FIN, ACK] Seq=4 Ack=1 Win=64256 Len=0 TSval=1303603654 TSecr=1763563
755	796.260956290	192.168.1.4	192.168.1.5	TCP	66	55555 → 37224 [FIN, ACK] Seq=1 Ack=5 Win=5792 Len=0 TSval=1840438 TSecr=1303603654
756	796.260980220	192.168.1.5	192.168.1.4	TCP	66	37224 → 55555 [ACK] Seq=5 Ack=2 Win=64256 Len=0 TSval=1303603654 TSecr=1840438
```

- **SYN** consumes 1 , **FIN** consumes 1 , **Len** consumes lenght of data sent 
- Wireshark normalizes sequence numbers by default. It sets the first packet’s Seq = 0 even if the real ISN is random (e.g., 345345345).
- So everything you see is relative seq numbers easy mode , if u want the real numbers turn off it from : View → Coloring Rules → Relative sequence numbers.
- if u didn`t specify the source port of netcat (in the attacker machine) it generate a random port for our example its 37224
- anti-enumeration techniques : Many ports like **HTTP:80** can change its default port.

---
### Full Explanation of Packets (13 → 756)
#### Packet 13 — SYN
```
192.168.1.5 → 192.168.1.4  [SYN]
Seq=0  Len=0
What this means:
Client starts the connection.
SYN flag = “I want to start TCP”
Seq = 0 (Wireshark made it 0, real number is random)
Len = 0 (SYN carries no data)
📌 SYN consumes 1 sequence number
So next client SEQ will become 1.
```
#### Packet 14 — SYN/ACK
```
192.168.1.4 → 192.168.1.5  [SYN, ACK]
Seq=0  Ack=1
Meaning:
Server replies with its own SYN + ACK.
Seq = 0 (server’s first sequence number)
Ack = 1 → “I got your SYN, which used 1 seq number”
Because client SYN = 0 → the next expected is 1.
📌 Server SYN also consumes 1 seq number
So server’s next sequence number will be 1.
```

#### Packet 15 — ACK
```
192.168.1.5 → 192.168.1.4  [ACK]
Seq=1 Ack=1
✔ Meaning:
Client acknowledges the server SYN.
Seq = 1 (client consumed a sequence number because SYN consumed 1)
Ack = 1 (server’s SYN=0 → next expected = 1)
🎉 Connection is now fully established.
```
### 📨 Sending data (“hi\n”)
```
Netcat sends 3 bytes:
h i \n → 3 bytes
```
#### Packet 20 — PSH/ACK containing data
```
Seq=1 Ack=1 Len=3
Meaning:
Data starts at Seq=1 (because 0 was SYN)
3 bytes of data → ranges from 1,2,3
Next expected from client = 1 + 3 = 4
```
#### Packet 21 — ACK from server
```
Seq=1 Ack=4
Meaning:
Server acknowledges receiving 3 bytes.
Ack=4 → “I got up to sequence number 3, next expected is 4”
Server's Seq=1 (because its SYN used 0; ACK used no seq)
```

### 🔚 Closing (FIN from client)

FIN behaves like SYN → consumes 1 sequence number.

#### Packet 754 — FIN/ACK (client → server)
```
Seq=4 Ack=1
✔ Meaning:
Client’s next seq is 4 because:
SYN = 1
"hi" = 3 bytes
→ next = 4
FIN uses 1 sequence number.
After FIN:
Client’s next seq will be 5
```
#### Packet 755 — FIN/ACK (server → client)
```
Seq=1 Ack=5
✔ Meaning:
Server FIN starts at Seq=1 (server hasn't sent data, only SYN)
Ack=5 → “I acknowledge your FIN (which consumed sequence number 4 → so next = 5)”
FIN consumes one seq → server’s next seq becomes 2.
```

#### Packet 756 — Final ACK (client → server)
```
Seq=5 Ack=2
✔ Meaning:
Client acknowledges the server’s FIN.
Client Seq = 5 (from earlier FIN)
Ack = 2 (server SYN=0, FIN=1 → next = 2)
🎉 Connection closed cleanly.
```
### Diagram
```
            192.168.1.5 (Kali)                     192.168.1.4 (Metasploitable2)

======================== 3‑WAY HANDSHAKE ========================

                 ___        SYN (seq:100, ACK:0)                ___
                |   | ---------------------------------------> |   |
                |___|                                          |___|

                 ___      SYN/ACK (seq:600, ACK:101)            ___
                |   | <--------------------------------------- |   |
                |___|                                          |___|

                 ___        ACK (seq:101, ACK:601)              ___
                |   | ---------------------------------------> |   |
                |___|                                          |___|


=========================== DATA TRANSFER ===========================
# Example data: "hi\n" (3 bytes)

                 ___   PSH/ACK (seq:101, ACK:601, Len=3)        ___
                |   | ---------------------------------------> |   |
                |___|                                          |___|

                 ___        ACK (seq:601, Ack=104)              ___
                |   | <--------------------------------------- |   |
                |___|                                          |___|


========================= 4‑WAY TERMINATION =========================

# Kali wants to close the connection

                 ___          FIN/ACK (seq:104, ACK:601)        ___
                |   | ---------------------------------------> |   |
                |___|                                          |___|

                 ___          ACK (seq:601, ACK:105)            ___
                |   | <--------------------------------------- |   |
                |___|                                          |___|

# Server closes its side

                 ___          FIN/ACK (seq:601, ACK:105)        ___
                |   | <--------------------------------------- |   |
                |___|                                          |___|

                 ___          ACK (seq:105, ACK:602)            ___
                |   | ---------------------------------------> |   |
                |___|                                          |___|

```
- SYN consumes 1 sequence number
- ACK does NOT consume sequence numbers
- Data consumes its own length (Len)
- FIN consumes 1 sequence number
- After 4 packets of FIN exchange, the connection closes cleanly.
