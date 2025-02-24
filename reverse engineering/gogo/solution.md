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
The hints tells me to use objdump or ghidra, but IDA pro stays with me in this challenge. 

Opening IDA pro, and look for main function. 
You can see several functions like main.checkPassword, main.getFlag.. 

The program tells us to enter password so firstly I check the checkPassword function.

Go to View -> Open subviews and choose Generate pseudocode or hit F5

And this code appears : 
```C
// main.checkPassword
bool __golang main_checkPassword(string_0 input)
{
  int v1; // eax
  int v2; // ebx
  uint8 key[32]; // [esp+4h] [ebp-40h] BYREF
  _BYTE v4[32]; // [esp+24h] [ebp-20h]

  if ( input.len < 32 )
    os_Exit(0);
  ((void (*)(void))loc_8090B18)();
  qmemcpy(key, "861836f13e3d627dfa375bdb8389214e", sizeof(key));
  ((void (*)(void))loc_8090FE0)();
  v1 = 0;
  v2 = 0;
  while ( v1 < 32 )
  {
    if ( (unsigned int)v1 >= input.len || (unsigned int)v1 >= 0x20 )
      runtime_panicindex();
    if ( (key[v1] ^ input.str[v1]) == v4[v1] )
      ++v2;
    ++v1;
  }
  return v2 == 32;
}
```
As you can see, it takes our input and XOR with a given key string. 

To get the correct input, we need to perform a reverse XOR. 
Accoring to the given code, v4 ^ key = input 

We already have the key string, but where is the v4? 

So in next steps, we need to find v4. 

Go to Text view of checkPassword function, we find the part where the XOR is at. 
```asm
.text:080D4B0F                 cmp     eax, 20h ; ' '
.text:080D4B12                 jge     short loc_80D4B3A
.text:080D4B14                 cmp     eax, edx
.text:080D4B16                 jnb     short loc_80D4B66
.text:080D4B18                 movzx   ebp, byte ptr [ecx+eax]
.text:080D4B1C                 cmp     eax, 20h ; ' '
.text:080D4B1F                 jnb     short loc_80D4B66
.text:080D4B21                 movzx   esi, [esp+eax+44h+key]
.text:080D4B26                 xor     ebp, esi
.text:080D4B28                 movzx   esi, [esp+eax+44h+var_20]
.text:080D4B2D                 xchg    eax, ebp
.text:080D4B2E                 xchg    ebx, esi
.text:080D4B30                 cmp     al, bl
.text:080D4B32                 xchg    ebx, esi
.text:080D4B34                 xchg    eax, ebp
.text:080D4B35                 jnz     short loc_80D4B0E
.text:080D4B37                 inc     ebx
.text:080D4B38                 jmp     short loc_80D4B0E
```
And there we go.. it XORs and then compare, we can see v4 is in 0x80D4B28 !

We found it! But how to see the hex string ? 
Hex view in IDA may help but the address is 0x80D4B28, we may find the incomplete hex string. 

So I use GDB debugger to see it. Open GDB I set breakpoint at v4, which is 0x80D4B28.
```C++
gef➤  b *0x80d4b28
Breakpoint 1 at 0x80d4b28: file /opt/hacksports/shared/staging/gogo_3_1238727778909769/problem_files/enter_password.go, line 71
```
And this is v4's hex string !
```C++
gef➤  hexdump byte $esp+$eax*1+0x24
0x1843df48     4a 53 47 5d 41 45 03 54 5d 02 5a 0a 53 57 45 0d    JSG]AE.T].Z.SWE.
0x1843df58     05 00 5d 55 54 10 01 0e 41 55 57 4b 45 50 46 01    ..]UT...AUWKEPF.
0x1843df68     c2 48 0d 08 80 61 41 18 39 00 00 00 ac df 43 18    .H...aA.9.....C.
0x1843df78     01 00 00 00 01 00 00 00 01 00 00 00 00 00 00 00    ................
```
Take that hex and XOR with the key's hex, then we will get the password ! 
```python
hex=(str(hex(0x3836313833366631336533643632376466613337356264623833383932313465^
          0x4a53475d414503545d025a0a5357450d05005d555410010e4155574b45504601)))
hex2=hex[2:]
print(bytes.fromhex(hex2).decode('utf-8'))
```
Then enter the password to the program
```
└─$ ./enter_password 
Enter Password: reverseengineericanbarelyforward
=========================================
This challenge is interrupted by psociety
What is the unhashed key?
```
The program asks us unhashed key.. Just unhash the key [here](https://md5hashing.net/hash/md5).
Connect to the server to get the flag!
```
└─$ nc mercury.picoctf.net 34256   
Enter Password: reverseengineericanbarelyforward
=========================================
This challenge is interrupted by psociety
What is the unhashed key?
goldfish
Flag is:  picoCTF{..redacted..}
```

