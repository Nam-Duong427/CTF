# WebNet0
## Difficulty 
Hard
## Hints
- Try using a tool like Wireshark.
- How can you decrypt the TLS stream?
## Problem Description
We found this [packet capture](https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/capture.pcap) and [key](https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/picopico.key). Recover the flag.
## Solution
First, read the hints we can see that we have to decrypt the TLS stream to see the flag.
The problem gives us a key and a pcap file. We will use wireshark to analyze all of it. 

Open the given pcap file by wireshark and look for TLS protocol.
```
6	0.028881	128.237.140.23	172.31.22.220	TLSv1.2	583	Client Hello (SNI=ec2-18-223-184-200.us-east-2.compute.amazonaws.com)
8	0.029538	172.31.22.220	128.237.140.23	TLSv1.2	1073	Server Hello, Certificate, Server Hello Done
10	0.058387	128.237.140.23	172.31.22.220	TLSv1.2	583	Client Hello (SNI=ec2-18-223-184-200.us-east-2.compute.amazonaws.com)
13	0.058743	172.31.22.220	128.237.140.23	TLSv1.2	1073	Server Hello, Certificate, Server Hello Done
14	0.059645	128.237.140.23	172.31.22.220	TLSv1.2	384	Client Key Exchange, Change Cipher Spec, Finished
15	0.061383	172.31.22.220	128.237.140.23	TLSv1.2	324	New Session Ticket, Change Cipher Spec, Finished
19	0.092429	128.237.140.23	172.31.22.220	TLSv1.2	384	Client Key Exchange, Change Cipher Spec, Finished
21	0.094104	172.31.22.220	128.237.140.23	TLSv1.2	324	New Session Ticket, Change Cipher Spec, Finished
23	0.122203	128.237.140.23	172.31.22.220	TLSv1.2	583	Client Hello (SNI=ec2-18-223-184-200.us-east-2.compute.amazonaws.com)
25	0.122552	172.31.22.220	128.237.140.23	TLSv1.2	1073	Server Hello, Certificate, Server Hello Done
28	0.152210	128.237.140.23	172.31.22.220	TLSv1.2	384	Client Key Exchange, Change Cipher Spec, Finished
29	0.153206	172.31.22.220	128.237.140.23	TLSv1.2	324	New Session Ticket, Change Cipher Spec, Finished
```
Choose one of them and we know all of it has been encrypted by RSA. 
```
Transport Layer Security
    TLSv1.2 Record Layer: Handshake Protocol: Server Hello
        Content Type: Handshake (22)
        Version: TLS 1.2 (0x0303)
        Length: 53
        Handshake Protocol: Server Hello
            Handshake Type: Server Hello (2)
            Length: 49
            Version: TLS 1.2 (0x0303)
            Random: e00b1ec743ceb390afa2009449c9c337933181144fa5a1562eb55ac5c98fc0af
                GMT Unix Time: Feb  9, 2089 17:05:59.000000000 EST
                Random Bytes: 43ceb390afa2009449c9c337933181144fa5a1562eb55ac5c98fc0af
            Session ID Length: 0
            Cipher Suite: TLS_RSA_WITH_AES_256_GCM_SHA384 (0x009d)
            Compression Method: null (0)
            Extensions Length: 9
            Extension: renegotiation_info (len=1)
            Extension: session_ticket (len=0)
            [JA3S Fullstring: 771,157,65281-35]
            [JA3S: 7c02dbae662670040c7af9bd15fb7e2f]
```


