# endianness-v2 
## Difficulty
Medium
## Hints 
(None)
## Problem Description
Here's a file that was recovered from a 32-bits system that organized the bytes a weird way. We're not even sure what type of file it is.
Download it [here](https://artifacts.picoctf.net/c_titan/112/challengefile) and see what you can get out of it
## Solution 
First, we will be given a data file. 
```
└─$ file challengefile
challengefile: data
```
I tried strings the file but found nothing..
```
└─$ strings challengefile            
 '.$
#,")7(
410,'
4428=943.<
!2222222222222222222222222222222222222222222222222
)('&654*:987FEDCJIHGVUTSZYXWfedcjihgvutszyxw
5*)(9876EDC:IHGFUTSJYXWVedcZihgfutsjyxwv
H%..
PEIZn
Zm}m!aJ
bzh\
[JRd
8YQi
sr.PE

...
```
Again, its a data file so I will check the file's hex by using hexdump
```
└─$ hexdump -X challengefile
0000000  e0  ff  d8  ff  46  4a  10  00  01  00  46  49  01  00  00  01
0000010  00  00  01  00  43  00  db  ff  06  06  08  00  08  05  06  07
0000020  09  07  07  07  0c  0a  08  09  0b  0c  0d  14  12  19  0c  0b
0000030  1d  14  0f  13  1d  1e  1f  1a  20  1c  1c  1a  20  27  2e  24
0000040  1c  23  2c  22  29  37  28  1c  34  31  30  2c  27  1f  34  34
0000050  32  38  3d  39  34  33  2e  3c  00  db  ff  32  09  09  01  43
0000060  0c  0b  0c  09  18  0d  0d  18  21  1c  21  32  32  32  32  32

... 
```
The header is weird.. but the title mention "endianess". IF you don't know what is endianess. Check [here](https://www.geeksforgeeks.org/little-and-big-endian-mystery/). 
So basically I use [CyberChef](https://gchq.github.io/CyberChef/) to swap the endian, that will make us a new file. 
Choose raw data option then download the new file from it.

A jpg file should be shown and that is your flag ! Good luck !
