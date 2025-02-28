# reverse_cipher
## Difficulty
Hard
## Hints
objdump and Gihdra are some tools that could assist with this
## Problem Description
We have recovered a [binary](https://jupiter.challenges.picoctf.org/static/31c9b832d036a10daeef52d8b4290ef0/rev) and a [text file](https://jupiter.challenges.picoctf.org/static/31c9b832d036a10daeef52d8b4290ef0/rev_this) file. 
Can you reverse the flag.
## Solution 
First, we are given 2 files. One is a binary file and a text file. 

Open the text file it gives us a string :
picoCTF{w1{1wq85jc=2i0<}

So we need to reverse this string.. 

Now as usual, open the binary file in Ghidra. 

**Note: I have edited some variable's names for reading easier so it will be different from the original from Ghidra!**

```C
void main(void)

{
  size_t sVar1;
  char flag [23];
  char local_41;
  int local_2c;
  FILE *local_28;
  FILE *local_20;
  uint j;
  int i;
  char local_9;
  
  local_20 = fopen("flag.txt","r");
  local_28 = fopen("rev_this","a");
  if (local_20 == (FILE *)0x0) {
    puts("No flag found, please make sure this is run on the server");
  }
  if (local_28 == (FILE *)0x0) {
    puts("please run this on the server");
  }
  sVar1 = fread(flag,0x18,1,local_20);
  local_2c = (int)sVar1;
  if ((int)sVar1 < 1) {
                    /* WARNING: Subroutine does not return */
    exit(0);
  }
  for (i = 0; i < 8; i = i + 1) {
    local_9 = flag[i];
    fputc((int)local_9,local_28);
  }
  for (j = 8; (int)j < 0x17; j = j + 1) {
    if ((j & 1) == 0) {
      local_9 = flag[(int)j] + '\x05';
    }
    else {
      local_9 = flag[(int)j] + -2;
    }
    fputc((int)local_9,local_28);
  }
  local_9 = local_41;
  fputc((int)local_41,local_28);
  fclose(local_28);
  fclose(local_20);
  return;
}
```
We can see that the program open the flag file as read mode file and rev_this file as append mode. 
```C
  local_20 = fopen("flag.txt","r");
  local_28 = fopen("rev_this","a");
```
Through the pseudocode, it clearly says that this program turn the content of flag file into a new file called rev_this, which is need to be reversed.
```C
  for (i = 0; i < 8; i = i + 1) {
    local_9 = flag[i];
    fputc((int)local_9,local_28);
  }
  for (j = 8; (int)j < 0x17; j = j + 1) {
    if ((j & 1) == 0) {
      local_9 = flag[(int)j] + '\x05';
    }
    else {
      local_9 = flag[(int)j] + -2;
    }
    fputc((int)local_9,local_28);
  }
  local_9 = local_41;
  fputc((int)local_41,local_28);
```
Because from position 0 to 7 still "picoCTF{" , so we will jump the second for loop!
```C
  for (j = 8; (int)j < 0x17; j = j + 1) {
    if ((j & 1) == 0) {
      local_9 = flag[(int)j] + '\x05';
    }
    else {
      local_9 = flag[(int)j] + -2;
    }
    fputc((int)local_9,local_28);
  }
```
As you can see, it takes content of flag file and put into rev_this by fputc() function.
All we need to do : reverse this for loop ! 

Just a simple python script.. :D 
```python
# this is rev_this file's content
s="picoCTF{w1{1wq85jc=2i0<}"
r=""
for i in range(8,23) :
    if (i&1==0) :
       # in the encode for loop it adds 5 so we just need to minus the same amount
       r+=chr(ord(s[i])-5)
    else :
       # same here, it minus 2 so we add 2 here 
       r+=chr(ord(s[i])+2)
print(s[:8] + r + "}")
```
All done! This must give you the flag right away! 
