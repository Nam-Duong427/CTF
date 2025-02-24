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
When I go for seeing TLS stream content, it's empty.. 

Following the hints, I find this useful [tutorial](https://blog.didierstevens.com/2020/12/14/decrypting-tls-streams-with-wireshark-part-1/) about how to decrypt the TLS stream.

Go to Edit -> Preferences -> Protocols -> TLS
Hit edit in RSA key list to import the given key. 

Go back to the TLS stream, you will see the flag !!

```
GET / HTTP/1.1
Host: ec2-18-223-184-200.us-east-2.compute.amazonaws.com
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Pragma: no-cache
Cache-Control: no-cache

HTTP/1.1 200 OK
Date: Fri, 23 Aug 2019 15:56:36 GMT
Server: Apache/2.4.29 (Ubuntu)
Last-Modified: Mon, 12 Aug 2019 16:50:05 GMT
ETag: "5ff-58fee50dc3fb0-gzip"
Accept-Ranges: bytes
Vary: Accept-Encoding
Content-Encoding: gzip
Pico-Flag: picoCTF{..redacted..}
Content-Length: 821
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html

...........T]s.:.|N~...........$4.g(.#@.&l..+KB...._...f.N.\^,.-G.{..."..B`...v....(b.oafu...R..ra...
.x.&.G ......l.m.*.).
k.Z..n[.....om..
...B.4f..%.N..oH....
.F4A.V!..w..J%a?l...h.q..D..s..D..O&'F...HL}K..b.bl.M%.}+.Z.. T..?....<6	#..<....p...C.N5''...e.j.H..sL.....$.b\#...`../..Q.1.^F=...V...f..I0.=..p.[..`..........o$W.K...[.qV.|....._+.sp..b..N.....".?.p.l.J..}....6.h.&..N.S....K.]x.P,......<*:.g^D6 .H).*g.....2.g?..f.......cjF.....L.Aa...l.u...cKj..6g.7M....AqB4`.X.....&.f.....zP|`.
.RI..l.........B.......I(..'.K@6ZcY..H...t0.O\.,.L...r.|..:4S2<.4..v.U....ai..`:....c..8.....o.....&.-.|l..D....Y2...r..U.x...x..]..RO..O...=.}.=x..'.....R..b...%{....d..8:.].m8-...c....._..v/z.h...i.....H.S..g7.....t.
....V................R..n.....k9A6.gI..D],.\9&...........5g2..E.1d..}...UqcW....w.V6......>T.	U...).?.....
```

Thanks for reading !

