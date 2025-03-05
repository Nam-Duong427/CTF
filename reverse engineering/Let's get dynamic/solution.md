# Let's get dynamic
## Difficulty
Hard
## Hints
Running this in a debugger would be helpful
## Problem Description
Can you tell what this file is reading? [chall.S](https://mercury.picoctf.net/static/4b062ca73355f923a41be8d673206a78/chall.S)
## Solution 
The given file is an assembler source file, so we cannot run debugger with it.
```
└─$ file chall.S       
chall.S: assembler source, ASCII text
```
So I use [gcc](https://gcc.gnu.org/) to turn it into a excutable file. 
```
└─$ gcc -g chall.S -o dyna
```
I call the program **dyna**.
Let's try to run it!
```
└─$ ./dyna 
123
Correct! You entered the flag.
```
Correct? Nope. It's certainly not the flag.. :) 

Now go to ghidra to see pseudocode.
```C
/* WARNING: Unknown calling convention -- yet parameter storage is locked */

void main(void)

{
  int iVar1;
  size_t sVar2;
  long in_FS_OFFSET;
  int local_11c;
  byte local_118 [64];
  char local_d8 [64];
  byte local_98 [64];
  byte local_58 [56];
  long local_20;
  
  local_20 = *(long *)(in_FS_OFFSET + 0x28);
  local_98[0] = 0x87;
  local_98[1] = 0xca;
  local_98[2] = 0xc4;
  local_98[3] = 0xf9;
  local_98[4] = 199;
  local_98[5] = 0x6d;
  local_98[6] = 0xbd;
  local_98[7] = 0x33;
  local_98[8] = 0x26;
  local_98[9] = 0x56;
  local_98[10] = 0x1d;
  local_98[0xb] = 0x41;
  local_98[0xc] = 0;
  local_98[0xd] = 0x99;
  local_98[0xe] = 0x5d;
  local_98[0xf] = 0xcc;
  local_98[0x10] = 0x7e;
  local_98[0x11] = 0x15;
  local_98[0x12] = 0xe5;
  local_98[0x13] = 0x95;
  local_98[0x14] = 0xe3;
  local_98[0x15] = 0x7f;
  local_98[0x16] = 0xb;
  local_98[0x17] = 0x4d;
  local_98[0x18] = 0x8c;
  local_98[0x19] = 0x1c;
  local_98[0x1a] = 0x53;
  local_98[0x1b] = 0x4a;
  local_98[0x1c] = 0x47;
  local_98[0x1d] = 0xa6;
  local_98[0x1e] = 0x69;
  local_98[0x1f] = 0xfa;
  local_98[0x20] = 0x16;
  local_98[0x21] = 0x1a;
  local_98[0x22] = 0x33;
  local_98[0x23] = 0x40;
  local_98[0x24] = 0x1b;
  local_98[0x25] = 0x6a;
  local_98[0x26] = 0x57;
  local_98[0x27] = 0x84;
  local_98[0x28] = 0xf8;
  local_98[0x29] = 0xc9;
  local_98[0x2a] = 0x7d;
  local_98[0x2b] = 0x91;
  local_98[0x2c] = 0xe8;
  local_98[0x2d] = 0x54;
  local_98[0x2e] = 0x9e;
  local_98[0x2f] = 0x70;
  local_98[0x30] = 0x72;
  local_98[0x31] = 0;
  local_58[0] = 0xe4;
  local_58[1] = 0xb1;
  local_58[2] = 0xb6;
  local_58[3] = 0x86;
  local_58[4] = 0x93;
  local_58[5] = 0x2f;
  local_58[6] = 0xee;
  local_58[7] = 0x5c;
  local_58[8] = 0x59;
  local_58[9] = 0x35;
  local_58[10] = 0x6a;
  local_58[0xb] = 0x6d;
  local_58[0xc] = 0x72;
  local_58[0xd] = 0xb6;
  local_58[0xe] = 0x23;
  local_58[0xf] = 0x8f;
  local_58[0x10] = 0x49;
  local_58[0x11] = 0x79;
  local_58[0x12] = 0xd0;
  local_58[0x13] = 0xf9;
  local_58[0x14] = 0x9d;
  local_58[0x15] = 0x48;
  local_58[0x16] = 0x7d;
  local_58[0x17] = 0x16;
  local_58[0x18] = 0xb6;
  local_58[0x19] = 0x65;
  local_58[0x1a] = 5;
  local_58[0x1b] = 0x77;
  local_58[0x1c] = 0x3d;
  local_58[0x1d] = 0xd8;
  local_58[0x1e] = 0x57;
  local_58[0x1f] = 0x84;
  local_58[0x20] = 0x7a;
  local_58[0x21] = 0x5d;
  local_58[0x22] = 0x71;
  local_58[0x23] = 0x43;
  local_58[0x24] = 0x4a;
  local_58[0x25] = 0x29;
  local_58[0x26] = 0xe;
  local_58[0x27] = 0xef;
  local_58[0x28] = 0xfa;
  local_58[0x29] = 0xc1;
  local_58[0x2a] = 0x72;
  local_58[0x2b] = 0x9f;
  local_58[0x2c] = 0xb1;
  local_58[0x2d] = 0xb;
  local_58[0x2e] = 0x9b;
  local_58[0x2f] = 0x7e;
  local_58[0x30] = 0x2c;
  local_58[0x31] = 0;
  fgets(local_d8,0x31,stdin);
  local_11c = 0;
  while( true ) {
    sVar2 = strlen((char *)local_98);
    if (sVar2 <= (ulong)(long)local_11c) break;
    local_118[local_11c] = (byte)local_11c ^ local_98[local_11c] ^ local_58[local_11c] ^ 0x13;
    local_11c = local_11c + 1;
  }
  iVar1 = memcmp(local_d8,local_118,0x31);
  if (iVar1 == 0) {
    puts("No, that\'s not right.");
  }
  else {
    puts("Correct! You entered the flag.");
  }
  if (local_20 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return;
}
```
According to the code, the right flag must be shown "Wrong" when run the prorgram in this situation.. !

Some main points : 
- Input is 49 characters long.
- local_118 is computed through a while loop. 
- User input as local_d8, it takes local_d8 and compare with local_118 throught memcmp() function. 
- Important variable is iVar1.
- memcmp() is at 0x5555555552ed

So local_118 is what we are trying to see ! We have to check the memcmp() function. Specifically the local_118.

To do this, I use GDB debugger.
Let's set a breakpoint at 0x5555555552ed.
```C++
gef➤  b *0x5555555552ed
Breakpoint 1 at 0x5555555552ed
gef➤  r
Starting program: /home/kali/Downloads/dyna 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```
Now check what's inside the function. 
```C++
memcmp@plt (
   $rdi = 0x00007fffffffdd00 → "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
   $rsi = 0x00007fffffffdcc0 → "picoCTF{dyn4",
   $rdx = 0x0000000000000031,
   $rcx = 0x00007fffffffdcc0 → "picoCTF{dyn4"
)
──────────────────────────────────────────────────────────────────────────────────── threads ────
[#0] Id 1, Name: "dyna", stopped 0x5555555552ed in main (), reason: BREAKPOINT
────────────────────────────────────────────────────────────────────────────────────── trace ────
[#0] 0x5555555552ed → main()
─────────────────────────────────────────────────────────────────────────────────────────────────
gef➤  
```
That's a good signal! $rsi must be the first part of the flag. 

.. But where is the rest of it ??? 

Let's go back to pseudocode.
```C
  while( true ) {
    sVar2 = strlen((char *)local_98);
    if (sVar2 <= (ulong)(long)local_11c) break;
    local_118[local_11c] = (byte)local_11c ^ local_98[local_11c] ^ local_58[local_11c] ^ 0x13;
    local_11c = local_11c + 1;
  }
```
Oh the break condition got my eyes !
```C
sVar2 = strlen((char *)local_98);
if (sVar2 <= (ulong)(long)local_11c) break;
```
Due to the lack of the flag content.. the length which is sVar2 is the problem. 

I'm a simple thinker.. Just edit it and export a new program. The length must be same as out input which is local_d8 in pseudocode. 
Now let's just modify the sVar2! I can find it at 001012c0. 
```asm
001012c0 48  8d  85       LEA        RAX =>local_98 ,[RBP  + -0x90 ]
         70  ff  ff  ff
```
Right-click and choose Patch Intruction and set it as local_d8 which is **RBP + -0xd0** that can be easily spotted in ghidra. 
```asm
001012db 48  8d  85       LEA        RAX =>local_d8 ,[RBP  + -0xd0 ]
         30  ff  ff  ff

```

Now it should look like this
```asm
001012c0 48  8d  85       LEA        RAX ,[RBP  + -0xd0 ]
         30  ff  ff  ff
```
```C
sVar2 = strlen(local_d8);
```
Go export it as a new program! Don't forget to save it as Original file!

Now we have a modified program.. Time for GDB again. 
```C++
gef➤  b *0x5555555552ed
Breakpoint 1 at 0x5555555552ed
gef➤  r
Starting program: /home/kali/Downloads/dyna 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```
Remember the flag is at $rsi when we hit the breakpoint at memcmp() . 
```C++
memcmp@plt (
   $rdi = 0x00007fffffffdd00 → "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
   $rsi = 0x00007fffffffdcc0 → 0x7b4654436f636970,
   $rdx = 0x0000000000000031,
   $rcx = 0x00007fffffffdcc0 → 0x7b4654436f636970
)
──────────────────────────────────────────────────────────────────────────────────── threads ────
[#0] Id 1, Name: "dyna", stopped 0x5555555552ed in main (), reason: BREAKPOINT
────────────────────────────────────────────────────────────────────────────────────── trace ────
[#0] 0x5555555552ed → main()
─────────────────────────────────────────────────────────────────────────────────────────────────
gef➤  
```
Now just print the flag out :) 
```C++
gef➤  x/s $rsi
0x7fffffffdcc0: "picoCTF{dyn4..redated..\230\335\377\377\377\177"
gef➤  
```
All done!

## Another simple way to solve
When I spot the main trouble that we need to solve, just implement the same script that we see. 
I use python for it!
```python
flag=[0]*49
local_98 = [0]*49
local_58 = [0]*49
local_98[0] = 0x87
local_98[1] = 0xca
local_98[2] = 0xc4
local_98[3] = 0xf9
local_98[4] = 199
local_98[5] = 0x6d;
local_98[6] = 0xbd;
local_98[7] = 0x33;
local_98[8] = 0x26;
local_98[9] = 0x56;
local_98[10] = 0x1d;
local_98[0xb] = 0x41;
local_98[0xc] = 0;
local_98[0xd] = 0x99;
local_98[0xe] = 0x5d;
local_98[0xf] = 0xcc;
local_98[0x10] = 0x7e;
local_98[0x11] = 0x15;
local_98[0x12] = 0xe5;
local_98[0x13] = 0x95;
local_98[0x14] = 0xe3;
local_98[0x15] = 0x7f;
local_98[0x16] = 0xb;
local_98[0x17] = 0x4d;
local_98[0x18] = 0x8c;
local_98[0x19] = 0x1c;
local_98[0x1a] = 0x53;
local_98[0x1b] = 0x4a;
local_98[0x1c] = 0x47;
local_98[0x1d] = 0xa6;
local_98[0x1e] = 0x69;
local_98[0x1f] = 0xfa;
local_98[0x20] = 0x16;
local_98[0x21] = 0x1a;
local_98[0x22] = 0x33;
local_98[0x23] = 0x40;
local_98[0x24] = 0x1b;
local_98[0x25] = 0x6a;
local_98[0x26] = 0x57;
local_98[0x27] = 0x84;
local_98[0x28] = 0xf8;
local_98[0x29] = 0xc9;
local_98[0x2a] = 0x7d;
local_98[0x2b] = 0x91;
local_98[0x2c] = 0xe8;
local_98[0x2d] = 0x54;
local_98[0x2e] = 0x9e;
local_98[0x2f] = 0x70;
local_98[0x30] = 0x72;
local_58[0] = 0xe4;
local_58[1] = 0xb1;
local_58[2] = 0xb6;
local_58[3] = 0x86;
local_58[4] = 0x93;
local_58[5] = 0x2f;
local_58[6] = 0xee;
local_58[7] = 0x5c;
local_58[8] = 0x59;
local_58[9] = 0x35;
local_58[10] = 0x6a;
local_58[0xb] = 0x6d;
local_58[0xc] = 0x72;
local_58[0xd] = 0xb6;
local_58[0xe] = 0x23;
local_58[0xf] = 0x8f;
local_58[0x10] = 0x49;
local_58[0x11] = 0x79;
local_58[0x12] = 0xd0;
local_58[0x13] = 0xf9;
local_58[0x14] = 0x9d;
local_58[0x15] = 0x48;
local_58[0x16] = 0x7d;
local_58[0x17] = 0x16;
local_58[0x18] = 0xb6;
local_58[0x19] = 0x65;
local_58[0x1a] = 5;
local_58[0x1b] = 0x77;
local_58[0x1c] = 0x3d;
local_58[0x1d] = 0xd8;
local_58[0x1e] = 0x57;
local_58[0x1f] = 0x84;
local_58[0x20] = 0x7a;
local_58[0x21] = 0x5d;
local_58[0x22] = 0x71;
local_58[0x23] = 0x43;
local_58[0x24] = 0x4a;
local_58[0x25] = 0x29;
local_58[0x26] = 0xe;
local_58[0x27] = 0xef;
local_58[0x28] = 0xfa;
local_58[0x29] = 0xc1;
local_58[0x2a] = 0x72;
local_58[0x2b] = 0x9f;
local_58[0x2c] = 0xb1;
local_58[0x2d] = 0xb;
local_58[0x2e] = 0x9b;
local_58[0x2f] = 0x7e;
local_58[0x30] = 0x2c;
i=0
re=""
while(True) :
    if 49 <= i : break
    flag[i]=i^local_98[i]^local_58[i]^0x13
    re+=chr(flag[i])
    i+=1
print(re)
```
Thanks for your time !

