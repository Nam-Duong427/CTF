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
I use IDA pro and Ghidra in this challenge. 

Opening IDA pro, and look for main function. 
You can see several functions like main.checkPassword, main.getFlag.. 

The program tells us to enter password so firstly I check the checkPassword function.

Go to View -> Open subviews and choose Generate pseudocode or hit F5

And this code appear : 
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

To get the correct input, we need to perform the reverse XOR. 
Accoring to the given code, v4 ^ key = input 

We already have the key string, but where is the v4? 

So in next steps, we need to find v4. 

Go to Graph view, we find 2 parts where the XOR is. 
```asm
movzx   ebp, byte ptr [ecx+eax]
cmp     eax, 20h ; ' '
jnb     short loc_80D4B66
```
```asm
movzx   esi, [esp+eax+44h+key]
xor     ebp, esi
movzx   esi, [esp+eax+44h+var_20]
xchg    eax, ebp
xchg    ebx, esi
cmp     al, bl
xchg    ebx, esi
xchg    eax, ebp
jnz     short loc_80D4B0E
```
And there we go.. it XORs and then compare with esp+eax+44h+var_20 with is v4 !! 

We found it! But how to see the hex string ? 
Choose text view for the second part of the ASM code.
We will get the address of v4, which is 0x080D4B28 



