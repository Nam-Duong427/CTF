# Binary Instrumentation 1
## Difficulty 
Medium 
## Hints 
- Frida is an easy-to-install, lightweight binary instrumentation toolkit
- Try using the CLI tools like frida-trace to auto-generate handlers
## Problem Description
I have been learning to use the Windows API to do cool stuff! Can you wake up my program to get the flag?
Download the exe [here](https://challenge-files.picoctf.net/c_verbal_sleep/c71239e2890bd0008ff9c1da986438d276e7a96ba123cb3bc7b04d5a3de27fe7/bininst1.zip). Unzip the archive with the password picoctf
## Solution
Base on the hint, the problem can be solved by using a tool called Frida to handle. But I figure out a interest point!
This problem can be esily solve with binwalk! You can read all the data from the excuteable file as string!

Firstly, download the given file .zip file, we got a .exe file. 
```
└─$ ls    
 bininst1.exe 
```
To be able to see if there is any hidden files, we use binwalk
```
└─$ binwalk -e bininst1.exe
```
Then we see another excutable file
```
└─$ cd _bininst1.exe.extracted; ls
6000  6000.7z
```
```
└─$ file 6000
6000: PE32+ executable (console) x86-64, for MS Windows, 6 sections
```
Now let's use strings command to read.. 
```
└─$ strings 6000 | grep flag
Hi, I have the flag for you just right here!
Ok, I'm Up! The flag is: REDACTED
```

