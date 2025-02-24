# WinAntiDbg0x100
## Difficulty 
Medium
## Hints 
Hints will be displayed to the Debug console. Good luck!
## Problem Description
This challenge will introduce you to 'Anti-Debugging.' Malware developers don't like it when you attempt to debug their executable files because debugging these files reveals many of their secrets! That's why, they include a lot of code logic specifically designed to interfere with your debugging process.
Now that you've understood the context, go ahead and debug this Windows executable!
This challenge binary file is a Windows console application and you can start with running it using cmd on Windows.
Challenge can be downloaded [here](https://artifacts.picoctf.net/c_titan/54/WinAntiDbg0x100.zip) . Unzip the archive with the password picoctf
## Solution
Download and unzip the file given, we have a .exe file 
```
└─$ file WinAntiDbg0x100.exe
WinAntiDbg0x100.exe: PE32 executable (console) Intel 80386, for MS Windows, 5 sections
```
This is a PE32 so I use x32dbg to debug the program. 
