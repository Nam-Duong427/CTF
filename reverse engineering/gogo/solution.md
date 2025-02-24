# gogo 
## Difficulty 
Hard
## Hints 
use go tool objdump or ghidra
## Problem Description
Hmmm this is a weird file... [enter_password](https://mercury.picoctf.net/static/eb7ca66cba87f2df20ea754c89148343/enter_password). There is a instance of the service running at mercury.picoctf.net:34256.
# Solution 
Download the given file, we see an executable file.
```
└─$ file enter_password     
enter_password: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, Go BuildID=--bYZi77Z7HtN3dmSbXU/bBls8YFmgs0HmTo3HGIV/JvDvpaxTrympQWzzT33m/yPEm7CK8x0gHuOidfIK5, with debug_info, not stripped
```
Run the program to see what it looks like. 
```
└─$ ./enter_password 
Enter Password: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
Try again!
```
The hints said use objdump or ghidra but in this challenge, IDA pro stays with me. 
Opening IDA pro, and look for main function. 
You can see several functions like main.checkPassword, main.getFlag.. 
