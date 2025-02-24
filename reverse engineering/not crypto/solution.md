# not crypto
## Difficulty 
Hard
## Hints
(None)
## Problem Description
there's crypto in here but the challenge is not crypto... 🤔 

Download [not-crypto](https://artifacts.picoctf.net/picoMini+by+redpwn/Reverse+Engineering/not-crypto/not-crypto)

## Solution
First, we can see the file is an executable file, which is an ELF 64-bit file, and we have to make sure the file has permission to run.
```
└─$ chmod +x not-crypto
```
I will try to run to see what it looks like. 
```
└─$ ./not-crypto
I heard you wanted to bargain for a flag... whatcha got?
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
Nope, come back later
```
Like others, we will open Ghidra or IDA pro to have a look inside.
In this problem, I choose Ghidra for that. 
When opening the file in Ghidra, we look for functions that may help us. To do that, choose Window then Defined Strings and look for the string appearing in the program.
And we can see this pseudocode :
```C
bool FUN_00101070(void)

{
  undefined auVar1 [16];
  undefined auVar2 [16];
  undefined auVar3 [16];
  undefined auVar4 [16];
  undefined auVar5 [16];
  undefined auVar6 [16];
  undefined auVar7 [16];
  byte bVar8;
  byte bVar9;
  byte bVar10;
  byte bVar11;
  byte bVar12;
  byte bVar13;
  byte bVar14;
  byte bVar15;
  byte bVar16;
  int iVar17;
  undefined4 uVar18;
  undefined4 uVar19;
  undefined4 *puVar20;
  byte bVar21;
  byte bVar22;
  byte bVar23;
  byte bVar24;
  long lVar25;
  byte bVar26;
  byte bVar27;
  byte bVar28;
  ulong uVar29;
  byte bVar30;
  uint uVar31;
  ulong uVar32;
  byte bVar33;
  byte bVar34;
  byte bVar35;
  byte bVar36;
  byte bVar37;
  byte bVar38;
  byte bVar39;
  byte bVar40;
  byte *pbVar41;
  byte *pbVar42;
  long in_FS_OFFSET;
  byte local_1fe;
  byte local_1fd;
  undefined4 local_1fc;
  undefined4 local_1f8;
  byte local_1f4;
  byte local_1f3;
  byte local_1f2;
  byte local_1f1;
  byte local_1f0;
  byte local_1ef;
  byte local_1ee;
  byte local_1ed;
  byte local_1ec;
  undefined4 *local_1e8;
  undefined local_198 [64];
  undefined4 local_158;
  undefined4 uStack_154;
  undefined4 uStack_150;
  undefined4 uStack_14c;
  byte local_148 [144];
  byte local_b8;
  byte local_b7;
  byte local_b6;
  byte local_b5;
  byte local_b4;
  byte local_b3;
  byte local_b2;
  byte local_b1;
  byte local_b0;
  byte local_af;
  byte local_ae;
  byte local_ad;
  byte local_ac;
  byte local_ab;
  byte local_aa;
  byte local_a9;
  byte local_a8 [16];
  undefined local_98 [16];
  undefined4 local_88;
  undefined4 uStack_84;
  undefined4 uStack_80;
  undefined4 uStack_7c;
  undefined4 local_78;
  undefined4 uStack_74;
  undefined4 uStack_70;
  undefined4 uStack_6c;
  undefined4 local_68;
  undefined4 uStack_64;
  undefined4 uStack_60;
  undefined4 uStack_5c;
  undefined4 local_58;
  undefined4 uStack_54;
  undefined4 uStack_50;
  undefined4 uStack_4c;
  undefined4 local_48 [2];
  long local_40;
  
  local_40 = *(long *)(in_FS_OFFSET + 0x28);
  puts("I heard you wanted to bargain for a flag... whatcha got?");
  bVar35 = 0x98;
  bVar27 = 0x32;
  bVar21 = 0x6c;
  bVar23 = 0x1c;
  local_158 = 0xff8c6cf7;
  uStack_154 = 0x8e2f875c;
  uStack_150 = 0xd49e439e;
  uStack_14c = 0x1c6c3298;
  uVar32 = 4;
  puVar20 = &local_158;
  do {
    if ((uVar32 & 3) == 0) {
      uVar29 = (ulong)bVar27;
      bVar27 = (&DAT_001020a0)[bVar21];
      bVar21 = (&DAT_001020a0)[bVar23];
      bVar23 = (&DAT_001020a0)[bVar35];
      bVar35 = (&DAT_001020a0)[uVar29] ^ (&DAT_00102080)[uVar32 >> 2];
    }
    bVar35 = bVar35 ^ *(byte *)puVar20;
    uVar31 = (int)uVar32 + 1;
    uVar32 = (ulong)uVar31;
    bVar27 = bVar27 ^ *(byte *)((long)puVar20 + 1);
    bVar21 = bVar21 ^ *(byte *)((long)puVar20 + 2);
    bVar23 = bVar23 ^ *(byte *)((long)puVar20 + 3);
    *(byte *)(puVar20 + 4) = bVar35;
    *(byte *)((long)puVar20 + 0x11) = bVar27;
    *(byte *)((long)puVar20 + 0x12) = bVar21;
    *(byte *)((long)puVar20 + 0x13) = bVar23;
    puVar20 = puVar20 + 1;
  } while (uVar31 != 0x2c);
  local_a8[0] = 0xbe;
  local_a8[1] = 0xd9;
  local_a8[2] = 0xe7;
  local_a8[3] = 0xce;
  local_a8[4] = 0x25;
  local_a8[5] = 0x52;
  local_a8[6] = 0xa8;
  local_a8[7] = 0x6d;
  local_a8[8] = 0x4d;
  local_a8[9] = 0xcf;
  local_a8[10] = 0xd7;
  local_a8[0xb] = 0xa2;
  local_a8[0xc] = 0xae;
  local_a8[0xd] = 0x61;
  local_a8[0xe] = 3;
  local_a8[0xf] = 0x2f;
  fread(local_198,1,0x40,_stdin);
  local_88 = 0x28dcea23;
  uStack_84 = 0xb2008495;
  uStack_80 = 0x7aca5778;
  uStack_7c = 0xc0419c59;
  local_78 = 0x31948f68;
  uStack_74 = 0xde862756;
  uStack_70 = 0xd75abc8b;
  uStack_6c = 0x71d19611;
  local_68 = 0x3bea1fb2;
  uStack_64 = 0xe76b3672;
  uStack_60 = 0xa1709dd5;
  uStack_5c = 0xf7ed23ed;
  local_58 = 0x8fc973b8;
  uStack_54 = 0x8bb70cc0;
  uStack_50 = 0x2d560843;
  uStack_4c = 0xaeea8540;
  iVar17 = 0x10;
  local_1e8 = &local_88;
  do {
    if (iVar17 == 0x10) {
      auVar1[1] = local_a8[1];
      auVar1[0] = local_a8[0];
      auVar1[2] = local_a8[2];
      auVar1[3] = local_a8[3];
      auVar1[4] = local_a8[4];
      auVar1[5] = local_a8[5];
      auVar1[6] = local_a8[6];
      auVar1[7] = local_a8[7];
      auVar1[8] = local_a8[8];
      auVar1[9] = local_a8[9];
      auVar1[10] = local_a8[10];
      auVar1[0xb] = local_a8[0xb];
      auVar1[0xc] = local_a8[0xc];
      auVar1[0xd] = local_a8[0xd];
      auVar1[0xe] = local_a8[0xe];
      auVar1[0xf] = local_a8[0xf];
      uVar18 = vpextrb_avx(auVar1,4);
      local_1ee = (&DAT_001020a0)[(byte)uStack_150 ^ local_a8[8]];
      uVar19 = vpextrb_avx(auVar1,0xc);
      local_1ef = (&DAT_001020a0)[(byte)((byte)uVar19 ^ (byte)uStack_14c)];
      uVar19 = vpextrb_avx(auVar1,1);
      local_1f4 = (&DAT_001020a0)[(byte)((byte)uVar19 ^ local_158._1_1_)];
      uVar19 = vpextrb_avx(auVar1,5);
      local_1fd = (&DAT_001020a0)[(byte)((byte)uVar19 ^ uStack_154._1_1_)];
      uVar19 = vpextrb_avx(auVar1,9);
      local_1fe = (&DAT_001020a0)[(byte)((byte)uVar19 ^ uStack_150._1_1_)];
      uVar19 = vpextrb_avx(auVar1,0xd);
      local_1f0 = (&DAT_001020a0)[(byte)((byte)uVar19 ^ uStack_14c._1_1_)];
      uVar19 = vpextrb_avx(auVar1,2);
      bVar27 = (&DAT_001020a0)[(byte)((byte)uVar19 ^ local_158._2_1_)];
      uVar19 = vpextrb_avx(auVar1,6);
      local_1ec = (&DAT_001020a0)[(byte)((byte)uVar19 ^ uStack_154._2_1_)];
      uVar19 = vpextrb_avx(auVar1,10);
      local_1f1 = (&DAT_001020a0)[(byte)((byte)uVar19 ^ uStack_150._2_1_)];
      uVar19 = vpextrb_avx(auVar1,0xe);
      local_1f2 = (&DAT_001020a0)[(byte)((byte)uVar19 ^ uStack_14c._2_1_)];
      uVar19 = vpextrb_avx(auVar1,3);
      local_1ed = (&DAT_001020a0)[(byte)((byte)uVar19 ^ local_158._3_1_)];
      uVar19 = vpextrb_avx(auVar1,7);
      bVar21 = (&DAT_001020a0)[(byte)((byte)uVar19 ^ uStack_154._3_1_)];
      uVar19 = vpextrb_avx(auVar1,0xb);
      bVar23 = (&DAT_001020a0)[(byte)((byte)uVar19 ^ uStack_150._3_1_)];
      uVar19 = vpextrb_avx(auVar1,0xf);
      local_1f3 = (&DAT_001020a0)[(byte)((byte)uVar19 ^ uStack_14c._3_1_)];
      pbVar41 = local_148;
      local_1f8._0_1_ = (&DAT_001020a0)[(byte)local_158 ^ local_a8[0]];
      local_1fc._0_1_ = (&DAT_001020a0)[(byte)((byte)uVar18 ^ (byte)uStack_154)];
      do {
        bVar22 = local_1fd ^ (byte)local_1f8;
        bVar26 = local_1f3 ^ local_1f1;
        bVar37 = bVar22 ^ bVar26;
        bVar40 = local_1f3 ^ (byte)local_1f8;
        bVar30 = local_1fe ^ (byte)local_1fc;
        bVar33 = local_1ed ^ local_1f2;
        bVar34 = bVar30 ^ bVar33;
        bVar38 = local_1ed ^ (byte)local_1fc;
        bVar10 = bVar21 ^ bVar27;
        bVar8 = local_1f0 ^ local_1ee;
        bVar11 = local_1ee ^ bVar21;
        bVar36 = bVar8 ^ bVar10;
        bVar12 = local_1ec ^ bVar23;
        bVar35 = local_1ec ^ local_1f4;
        bVar9 = local_1f4 ^ local_1ef;
        bVar13 = local_1ef ^ bVar23;
        bVar14 = bVar9 ^ bVar12;
        bVar39 = pbVar41[7] ^ bVar34 ^ local_1ed;
        bVar15 = bVar27 ^ bVar36 ^ pbVar41[10];
        bVar28 = pbVar41[0xd] ^ bVar14 ^ local_1f4;
        bVar24 = pbVar41[0xe] ^ bVar14 ^ local_1ec;
        bVar16 = bVar23 ^ pbVar41[0xf] ^ bVar14;
        local_1f8._0_1_ =
             (&DAT_001020a0)
             [(byte)((byte)local_1f8 ^ *pbVar41 ^ bVar37 ^
                    ((char)bVar22 >> 7) * -0x1b ^ bVar22 * '\x02')];
        bVar22 = pbVar41[4] ^ bVar34 ^ (byte)local_1fc;
        local_1ee = (&DAT_001020a0)
                    [(byte)(pbVar41[8] ^ bVar36 ^ local_1ee ^
                           ((char)bVar8 >> 7) * -0x1b ^ bVar8 * '\x02')];
        local_1ef = (&DAT_001020a0)
                    [(byte)(bVar14 ^ pbVar41[0xc] ^ local_1ef ^
                           bVar9 * '\x02' ^ ((char)bVar9 >> 7) * -0x1b)];
        local_1f4 = (&DAT_001020a0)
                    [(byte)(pbVar41[1] ^ bVar37 ^ local_1fd ^
                           ((char)(local_1f1 ^ local_1fd) >> 7) * -0x1b ^
                           (local_1f1 ^ local_1fd) * '\x02')];
        local_1fd = (&DAT_001020a0)
                    [(byte)(pbVar41[5] ^ bVar34 ^ local_1fe ^
                           (local_1f2 ^ local_1fe) * '\x02' ^
                           ((char)(local_1f2 ^ local_1fe) >> 7) * -0x1b)];
        local_1fe = (&DAT_001020a0)
                    [(byte)(local_1f0 ^ bVar36 ^ pbVar41[9] ^
                           (local_1f0 ^ bVar27) * '\x02' ^ ((char)(local_1f0 ^ bVar27) >> 7) * -0x1b
                           )];
        local_1f0 = (&DAT_001020a0)
                    [((uint)(bVar35 >> 7) * 0x1b ^ (uint)bVar35 + (uint)bVar35 ^ (uint)bVar28) &
                     0xff];
        pbVar42 = pbVar41 + 0x10;
        bVar27 = (&DAT_001020a0)
                 [(byte)(pbVar41[2] ^ bVar37 ^ local_1f1 ^
                        ((char)bVar26 >> 7) * -0x1b ^ bVar26 * '\x02')];
        local_1ec = (&DAT_001020a0)
                    [(byte)(local_1f2 ^ pbVar41[6] ^ bVar34 ^
                           bVar33 * '\x02' ^ ((char)bVar33 >> 7) * -0x1b)];
        local_1f1 = (&DAT_001020a0)
                    [((uint)bVar10 * 2 ^ (uint)(bVar10 >> 7) * 0x1b ^ (uint)bVar15) & 0xff];
        local_1f2 = (&DAT_001020a0)
                    [((uint)bVar12 * 2 ^ (uint)(bVar12 >> 7) * 0x1b ^ (uint)bVar24) & 0xff];
        bVar23 = (&DAT_001020a0)
                 [((uint)(bVar11 >> 7) * 0x1b ^ (uint)bVar11 * 2 ^
                  (uint)(byte)(bVar21 ^ bVar36 ^ pbVar41[0xb])) & 0xff];
        local_1ed = (&DAT_001020a0)
                    [(byte)(pbVar41[3] ^ bVar37 ^ local_1f3 ^
                           bVar40 * '\x02' ^ ((char)bVar40 >> 7) * -0x1b)];
        bVar21 = (&DAT_001020a0)[(byte)(bVar39 ^ ((char)bVar38 >> 7) * -0x1b ^ bVar38 * '\x02')];
        local_1f3 = (&DAT_001020a0)
                    [((uint)(bVar13 >> 7) * 0x1b ^ (uint)bVar13 * 2 ^ (uint)bVar16) & 0xff];
        pbVar41 = pbVar42;
        local_1fc._0_1_ =
             (&DAT_001020a0)[(byte)(bVar22 ^ ((char)bVar30 >> 7) * -0x1b ^ bVar30 * '\x02')];
      } while (&local_b8 != pbVar42);
      local_1f8 = CONCAT31(local_1f8._1_3_,(byte)local_1f8 ^ local_b8);
      auVar1 = vmovd_avx((uint)(bVar27 ^ local_ae));
      local_1fc = CONCAT31(local_1fc._1_3_,local_1f2 ^ local_b2);
      auVar2 = vmovd_avx((uint)(local_1ec ^ local_aa));
      auVar3 = vmovd_avx((uint)(local_1f1 ^ local_b6));
      auVar7 = vpinsrb_avx(auVar1,(uint)(local_ad ^ bVar21),1);
      auVar1 = vmovd_avx((uint)((&DAT_001020a0)
                                [(byte)(bVar22 ^ ((char)bVar30 >> 7) * -0x1b ^ bVar30 * '\x02')] ^
                               local_b4));
      lVar25 = 0xf;
      auVar4 = vmovd_avx((uint)(local_1ee ^ local_b0));
      auVar5 = vmovd_avx(local_1f8);
      auVar6 = vmovd_avx(local_1fc);
      auVar3 = vpinsrb_avx(auVar3,(uint)(local_1f3 ^ local_b5),1);
      auVar5 = vpinsrb_avx(auVar5,(uint)(local_1fd ^ local_b7),1);
      auVar4 = vpinsrb_avx(auVar4,(uint)(local_1f0 ^ local_af),1);
      auVar5 = vpunpcklwd_avx(auVar5,auVar3);
      auVar1 = vpinsrb_avx(auVar1,(uint)(local_1fe ^ local_b3),1);
      auVar3 = vpinsrb_avx(auVar6,(uint)(local_1ed ^ local_b1),1);
      auVar4 = vpunpcklwd_avx(auVar4,auVar7);
      auVar3 = vpunpcklwd_avx(auVar1,auVar3);
      auVar1 = vmovd_avx((uint)(local_1ef ^ local_ac));
      auVar3 = vpunpckldq_avx(auVar5,auVar3);
      auVar1 = vpinsrb_avx(auVar1,(uint)(local_1f4 ^ local_ab),1);
      auVar2 = vpinsrb_avx(auVar2,(uint)(bVar23 ^ local_a9),1);
      auVar1 = vpunpcklwd_avx(auVar1,auVar2);
      auVar1 = vpunpckldq_avx(auVar4,auVar1);
      local_98 = vpunpcklqdq_avx(auVar3,auVar1);
      bVar27 = local_a8[0xf];
      if (local_a8[0xf] == 0xff) {
        local_a8[0xf] = 0;
        lVar25 = 0xe;
        bVar27 = local_a8[0xe];
        if (local_a8[0xe] == 0xff) {
          local_a8[0xe] = 0;
          lVar25 = 0xd;
          bVar27 = local_a8[0xd];
          if (local_a8[0xd] == 0xff) {
            local_a8[0xd] = 0;
            lVar25 = 0xc;
            bVar27 = local_a8[0xc];
            if (local_a8[0xc] == 0xff) {
              local_a8[0xc] = 0;
              lVar25 = 0xb;
              bVar27 = local_a8[0xb];
              if (local_a8[0xb] == 0xff) {
                local_a8[0xb] = 0;
                lVar25 = 10;
                bVar27 = local_a8[10];
                if (local_a8[10] == 0xff) {
                  local_a8[10] = 0;
                  lVar25 = 9;
                  bVar27 = local_a8[9];
                  if (local_a8[9] == 0xff) {
                    local_a8[9] = 0;
                    lVar25 = 8;
                    bVar27 = local_a8[8];
                    if (local_a8[8] == 0xff) {
                      local_a8[8] = 0;
                      lVar25 = 7;
                      bVar27 = local_a8[7];
                      if (local_a8[7] == 0xff) {
                        local_a8[7] = 0;
                        lVar25 = 6;
                        bVar27 = local_a8[6];
                        if (local_a8[6] == 0xff) {
                          local_a8[6] = 0;
                          lVar25 = 5;
                          bVar27 = local_a8[5];
                          if (local_a8[5] == 0xff) {
                            local_a8[5] = 0;
                            lVar25 = 4;
                            bVar27 = local_a8[4];
                            if (local_a8[4] == 0xff) {
                              local_a8[4] = 0;
                              lVar25 = 3;
                              bVar27 = local_a8[3];
                              if (local_a8[3] == 0xff) {
                                local_a8[3] = 0;
                                lVar25 = 2;
                                bVar27 = local_a8[2];
                                if (local_a8[2] == 0xff) {
                                  local_a8[2] = 0;
                                  lVar25 = 1;
                                  bVar27 = local_a8[1];
                                  if (local_a8[1] == 0xff) {
                                    local_a8[1] = 0;
                                    lVar25 = 0;
                                    bVar27 = local_a8[0];
                                    if (local_a8[0] == 0xff) {
                                      local_a8[0] = 0;
                                      iVar17 = 0;
                                      goto LAB_00101385;
                                    }
                                  }
                                }
                              }
                            }
                          }
                        }
                      }
                    }
                  }
                }
              }
            }
          }
        }
      }
      local_a8[lVar25] = bVar27 + 1;
      iVar17 = 0;
    }
LAB_00101385:
    lVar25 = (long)iVar17;
    iVar17 = iVar17 + 1;
    *(byte *)local_1e8 = *(byte *)local_1e8 ^ local_98[lVar25];
    local_1e8 = (undefined4 *)((long)local_1e8 + 1);
    if (local_48 == local_1e8) {
      iVar17 = memcmp(&local_88,local_198,0x40);
      if (iVar17 != 0) {
        puts("Nope, come back later");
      }
      else {
        puts("Yep, that\'s it!");
      }
      if (local_40 == *(long *)(in_FS_OFFSET + 0x28)) {
        return iVar17 != 0;
      }
                    // WARNING: Subroutine does not return
      __stack_chk_fail();
    }
  } while( true );
}
```
As you can see, it looks like a crypto method, and it seems like too long to go through all or reverse the code immediately. 

But the description tells us it is not about crypto.. ?!

So I try GDB with gef to see inside the stack. If we find nothing, we will go back and reverse the code.  
```
└─$ gdb not-crypto
```
In the code, I can see "memcmp" function. Looks like it plays an important role in the condition If, decides if your input is correct or not.
```C
      iVar17 = memcmp(&local_88,local_198,0x40);
      if (iVar17 != 0) {
        puts("Nope, come back later");
      }
      else {
        puts("Yep, that\'s it!");
      }
```
So I set my break point at that memcmp function and see what's in the stack. If you wonder where the address is, go back to Ghidra ! 
```C++
gef➤  b *0x5555555553b9
Breakpoint 1 at 0x5555555553b9
```
And then run the program with a random string. 
```asm
[ Legend: Modified register | Code | Heap | Stack | String ]
──────────────────────────────────────────────────────────────────────────────────────────────────────────────── registers ────
$rax   : 0x10              
$rbx   : 0x00007fffffffdd80  →  0x0000000000000000
$rcx   : 0xa4              
$rdx   : 0x40              
$rsp   : 0x00007fffffffdbc0  →  0x0000000000000000
$rbp   : 0xa1              
$rsi   : 0x00007fffffffdc30  →  "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[...]"
$rdi   : 0x00007fffffffdd40  →  "picoCTF{..redacted[...]"
$rip   : 0x00005555555553b9  →   call 0x555555555060 <memcmp@plt>
$r8    : 0xba              
$r9    : 0x96              
$r10   : 0xf0              
$r11   : 0x6a              
$r12   : 0x97              
$r13   : 0x73              
$r14   : 0xf9              
$r15   : 0x3a              
$eflags: [ZERO carry PARITY adjust sign trap INTERRUPT direction overflow resume virtualx86 identification]
$cs: 0x33 $ss: 0x2b $ds: 0x00 $es: 0x00 $fs: 0x00 $gs: 0x00 
```
$rdi is what we are looking for !!! But it is an incomplete flag..

Easy peasy.. just print it out !!!
```asm
gef➤  x/s $rdi
0x7fffffffdd40: "picoCTF{..redacted..}\n"
```
Done ! Thanks for your time ! 
