# Investigative Reversing 0 
## Difficulty
Hard
## Hints 
- Try using some forensics skills on the image
- This problem requires both forensics and reversing skills
- A hex editor may be helpful
## Problem Description
We have recovered a [binary](https://jupiter.challenges.picoctf.org/static/70fd416f817ab1e59beaf19dc2b586cd/mystery) and an [image](https://jupiter.challenges.picoctf.org/static/70fd416f817ab1e59beaf19dc2b586cd/mystery.png). See what you can make of it. There should be a flag somewhere.
## Solution 
The problem gives us 2 files, one is a normal png, the other is an excutable file.
First of all, we open IDA pro to get closer to the program through pseudocode
```C
int __fastcall main(int argc, const char **argv, const char **envp)
{
  int i; // [rsp+4h] [rbp-4Ch]
  int j; // [rsp+8h] [rbp-48h]
  FILE *stream; // [rsp+10h] [rbp-40h]
  FILE *v8; // [rsp+18h] [rbp-38h]
  _BYTE ptr[40]; // [rsp+20h] [rbp-30h] BYREF
  unsigned __int64 v10; // [rsp+48h] [rbp-8h]

  v10 = __readfsqword(0x28u);
  stream = fopen("flag.txt", "r");
  v8 = fopen("mystery.png", "a");
  if ( !stream )
    puts("No flag found, please make sure this is run on the server");
  if ( !v8 )
    puts("mystery.png is missing, please run this on the server");
  if ( (int)fread(ptr, 0x1AuLL, 1uLL, stream) <= 0 )
    exit(0);
  puts("at insert");
  fputc(ptr[0], v8);
  fputc(ptr[1], v8);
  fputc(ptr[2], v8);
  fputc(ptr[3], v8);
  fputc(ptr[4], v8);
  fputc(ptr[5], v8);
  for ( i = 6; i <= 14; ++i )
    fputc((char)(ptr[i] + 5), v8);
  fputc((char)(ptr[15] - 3), v8);
  for ( j = 16; j <= 25; ++j )
    fputc((char)ptr[j], v8);
  fclose(v8);
  fclose(stream);
  return __readfsqword(0x28u) ^ v10;
}
```
According to the pseudocode given : 
- v8 stands for the image.
- The image was appended by ptr[i] by fputc()
- Append followed by 26 characters and it means our flag.
We just basically follow the code line by line to get the flag.

The flag is 26 characters, so we will take the last 26 bytes in the image.

Our solution script base on the pseudocode : 

Note: ptr stands for flag, and file stands for the image. 

```python
file=[0x70, 0x69, 0x63, 0x6f, 0x43, 0x54, 0x4b, 0x80, 0x6b, 0x35, 0x7a, 0x73,
 0x69, 0x64, 0x36, 0x71, 0x5f, 0x64, 0x31, 0x64, 0x65, 0x65, 0x64, 0x61, 0x61, 0x7d]
# last 26 bytes of the image

flag = [0 for i in range(26)]
# an array to start!

for i in  range(0,6) : 
    flag[i] = file[i]
for i in range(6,15) : 
    flag[i] = file[i] - 5
flag[15] = file[15] + 3 
for i in range(16,26) : 
    flag[i] = file[i] 
r=""    
for i in flag : 
    r+=chr(i)
print(r)
```
All done !
```
└─$ python s.py
picoCTF{..redacted..}
```

