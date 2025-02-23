# not crypto
## Problem Description
there's crypto in here but the challenge is not crypto... 🤔 

Download [not-crypto](https://artifacts.picoctf.net/picoMini+by+redpwn/Reverse+Engineering/not-crypto/not-crypto)

## Solution
First of all, we have to make sure the file that have permission to run
``` ┌──(kali㉿kali)-[~/Downloads]
└─$ chmod +x not-crypto
``` 
We can see the file is an executable file, which is ELF 64-bit file. Like others, we will open Ghidra or IDA pro to have a look inside.


