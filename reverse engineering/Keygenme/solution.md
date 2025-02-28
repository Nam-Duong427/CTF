# Keygenme
## Difficulty
Hard
## Hints
(None)
## Problem Description
Can you get the flag?

Reverse engineer this [binary](https://artifacts.picoctf.net/c/53/keygenme). 
## Solution 
Download the given file, we got an excutable file. 
```
└─$ file keygenme
keygenme: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=41df02426e2542baf206c4373631e30f1592e541, for GNU/Linux 3.2.0, stripped
```
Now try to run the program. 
```
└─$ ./keygenme
Enter your license key: 123
That key is invalid.
```
Open it in Ghidra to see the pseudocode.
```C
undefined8 FUN_0010148b(void)

{
  char cVar1;
  long in_FS_OFFSET;
  char local_38 [40];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  printf("Enter your license key: ");
  fgets(local_38,0x25,stdin);
  cVar1 = FUN_00101209(local_38);
  if (cVar1 == '\0') {
    puts("That key is invalid.");
  }
  else {
    puts("That key is valid.");
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
```
According to the code, the program checks that if we input the correct string or not, which is indeed the flag. 
And the flag is 36 characters long. 

So cVar1 decides it, let's see what's inside FUN_00101209() function.
```C
undefined8 FUN_00101209(char *param_1)

{
  size_t sVar1;
  undefined8 uVar2;
  long in_FS_OFFSET;
  int local_d0;
  int local_cc;
  int local_c8;
  int local_c4;
  int local_c0;
  uchar local_ba [2];
  byte local_b8 [16];
  byte local_a8 [16];
  uchar local_98 [32];
  char local_78 [12];
  undefined local_6c;
  undefined local_66;
  undefined local_5f;
  undefined local_5e;
  char local_58 [32];
  uchar auStack_38 [40];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  builtin_memcpy(local_98,"picoCTF{br1ng_y0ur_0wn_k3y_",0x1c);
  local_ba[0] = '}';
  local_ba[1] = '\0';
  sVar1 = strlen((char *)local_98);
  MD5(local_98,sVar1,local_b8);
  sVar1 = strlen((char *)local_ba);
  MD5(local_ba,sVar1,local_a8);
  local_d0 = 0;
  for (local_cc = 0; local_cc < 0x10; local_cc = local_cc + 1) {
    sprintf(local_78 + local_d0,"%02x",(ulong)local_b8[local_cc]);
    local_d0 = local_d0 + 2;
  }
  local_d0 = 0;
  for (local_c8 = 0; local_c8 < 0x10; local_c8 = local_c8 + 1) {
    sprintf(local_58 + local_d0,"%02x",(ulong)local_a8[local_c8]);
    local_d0 = local_d0 + 2;
  }
  for (local_c4 = 0; local_c4 < 0x1b; local_c4 = local_c4 + 1) {
    auStack_38[local_c4] = local_98[local_c4];
  }
  auStack_38[0x1b] = local_66;
  auStack_38[0x1c] = local_5e;
  auStack_38[0x1d] = local_5f;
  auStack_38[0x1e] = local_78[0];
  auStack_38[0x1f] = local_5e;
  auStack_38[0x20] = local_66;
  auStack_38[0x21] = local_6c;
  auStack_38[0x22] = local_5e;
  auStack_38[0x23] = local_ba[0];
  sVar1 = strlen(param_1);
  if (sVar1 == 0x24) {
    for (local_c0 = 0; local_c0 < 0x24; local_c0 = local_c0 + 1) {
      if (param_1[local_c0] != auStack_38[local_c0]) {
        uVar2 = 0;
        goto LAB_00101475;
      }
    }
    uVar2 = 1;
  }
  else {
    uVar2 = 0;
  }
LAB_00101475:
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return uVar2;
}
```
Oh.. We just have been given a part of our flag right away!! That's great !
```C
builtin_memcpy(local_98,"picoCTF{br1ng_y0ur_0wn_k3y_",0x1c);
```
And now we need to find the rest of it.

After a for loop to put 27 characters "picoCTF{br1ng_y0ur_0wn_k3y_" to auStack_38.
```C
  for (local_c4 = 0; local_c4 < 0x1b; local_c4 = local_c4 + 1) {
    auStack_38[local_c4] = local_98[local_c4];
  }
```
So auStack_38 is indeed our flag!

After putting 27 characters into it (which is in 26 position), we can see auStack_38 is being put value from 27 to 35 position.
```C
  auStack_38[0x1b] = local_66;
  auStack_38[0x1c] = local_5e;
  auStack_38[0x1d] = local_5f;
  auStack_38[0x1e] = local_78[0];
  auStack_38[0x1f] = local_5e;
  auStack_38[0x20] = local_66;
  auStack_38[0x21] = local_6c;
  auStack_38[0x22] = local_5e;
  auStack_38[0x23] = local_ba[0];
```
So in Ghidra, I can not see these value except the one at 35 position.
```C
local_ba[0] = '}';
```
Now time for GDB debugger.
But we need to know where should we jump to debug first. 
```asm
        001013c8 0f  b6  45  a2    MOVZX      EAX ,byte ptr [RBP  + local_66 ]
        001013cc 88  45  eb       MOV        byte ptr [RBP  + local_1d ],AL
        001013cf 0f  b6  45  aa    MOVZX      EAX ,byte ptr [RBP  + local_5e ]
        001013d3 88  45  ec       MOV        byte ptr [RBP  + local_1c ],AL
        001013d6 0f  b6  45  a9    MOVZX      EAX ,byte ptr [RBP  + local_5f ]
        001013da 88  45  ed       MOV        byte ptr [RBP  + local_1b ],AL
        001013dd 0f  b6  45  90    MOVZX      EAX ,byte ptr [RBP  + local_78 ]
        001013e1 88  45  ee       MOV        byte ptr [RBP  + local_1a ],AL
        001013e4 0f  b6  45  aa    MOVZX      EAX ,byte ptr [RBP  + local_5e ]
        001013e8 88  45  ef       MOV        byte ptr [RBP  + local_19 ],AL
        001013eb 0f  b6  45  a2    MOVZX      EAX ,byte ptr [RBP  + local_66 ]
        001013ef 88  45  f0       MOV        byte ptr [RBP  + local_18 ],AL
        001013f2 0f  b6  45  9c    MOVZX      EAX ,byte ptr [RBP  + local_6c ]
        001013f6 88  45  f1       MOV        byte ptr [RBP  + local_17 ],AL
        001013f9 0f  b6  45  aa    MOVZX      EAX ,byte ptr [RBP  + local_5e ]
        001013fd 88  45  f2       MOV        byte ptr [RBP  + local_16 ],AL
        00101400 0f  b6  85       MOVZX      EAX ,byte ptr [RBP  + local_ba ]
                 4e  ff  ff  ff
        00101407 88  45  f3       MOV        byte ptr [RBP  + local_15 ],AL
```
We have already now value in 00101400 which is '}' so the ideal breakpoints should be at 001013f9. 

Now run GDB and set breakpoint at 001013f9 then run the code with a random string to jump to it. 
```asm
gef➤  b *0x5555555553f9
Breakpoint 1 at 0x5555555553f9
gef➤  r
Starting program: /home/kali/Downloads/keygenme 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
Enter your license key: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```
Final step! just print all it out !
```asm
●→ 0x5555555553f9                  movzx  eax, BYTE PTR [rbp-0x56]
   0x5555555553fd                  mov    BYTE PTR [rbp-0xe], al
   0x555555555400                  movzx  eax, BYTE PTR [rbp-0xb2]
   0x555555555407                  mov    BYTE PTR [rbp-0xd], al
   0x55555555540a                  mov    rax, QWORD PTR [rbp-0xd8]
   0x555555555411                  mov    rdi, rax
──────────────────────────────────────────────────────────────────────────────────────────────────── threads ────
[#0] Id 1, Name: "keygenme", stopped 0x5555555553f9 in ?? (), reason: BREAKPOINT
────────────────────────────────────────────────────────────────────────────────────────────────────── trace ────
[#0] 0x5555555553f9 → movzx eax, BYTE PTR [rbp-0x56]
[#1] 0x5555555554e2 → test al, al
[#2] 0x7ffff7a33d68 → __libc_start_call_main(main=0x55555555548b, argc=0x1, argv=0x7fffffffded8)
[#3] 0x7ffff7a33e25 → __libc_start_main_impl(main=0x55555555548b, argc=0x1, argv=0x7fffffffded8, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffdec8)
[#4] 0x55555555514e → hlt 
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
gef➤  x/s $rbp-0x56
0x7fffffffdd1a: "d43882cbb184dd8e05c9709e5dcaedaa0495cfpicoCTF{br1ng_y0ur_0wn_k3y_..redacted..0\377\377\377\177"
```
And that's our flag! Don't forget to add "}" at the end! 

