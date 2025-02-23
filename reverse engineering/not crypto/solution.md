# not crypto
## Problem Description
there's crypto in here but the challenge is not crypto... 🤔 

Download [not-crypto](https://artifacts.picoctf.net/picoMini+by+redpwn/Reverse+Engineering/not-crypto/not-crypto)

## Solution
First of all, we can see the file is an executable file, which is ELF 64-bit file and we have to make sure the file that have permission to run.
```
└─$ chmod +x not-crypto
```
I will try to run to see what it look like. 
```
└─$ ./not-crypto
I heard you wanted to bargain for a flag... whatcha got?
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
Nope, come back later
```
Like others, we will open Ghidra or IDA pro to have a look inside.
In this problem, I choose Ghidra for that. 
When opening the file in Ghidra, we look for the main function of the program. To do that, go Window then choose Defined Strings and look for the string appear in the program.


