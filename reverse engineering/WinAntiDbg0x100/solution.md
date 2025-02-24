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

Press Alt + F9 to go to debug session, don't forget to turn FPU on 

We will see the ASM code and some strings of the program
```C++
00D1163A  | E8 71FDFFFF              | call winantidbg0x100.D113B0                  |
00D1163F  | 83C4 04                  | add esp,4                                    |
00D11642  | 8945 FC                  | mov dword ptr ss:[ebp-4],eax                 | [ebp-04]:BaseThreadInitThunk
00D11645  | 837D FC 00               | cmp dword ptr ss:[ebp-4],0                   | [ebp-04]:BaseThreadInitThunk
00D11649  | 75 0F                    | jne winantidbg0x100.D1165A                   |
00D1164B  | 68 6836D100              | push winantidbg0x100.D13668                  | D13668:L"### Something went wrong...\n"
00D11650  | FF15 0830D100            | call dword ptr ds:[<OutputDebugStringW>]     |
00D11656  | EB 4A                    | jmp winantidbg0x100.D116A2                   |
00D11658  | EB 48                    | jmp winantidbg0x100.D116A2                   |
00D1165A  | 68 A836D100              | push winantidbg0x100.D136A8                  | D136A8:L"### Good job! Here's your flag:\n"
00D1165F  | FF15 0830D100            | call dword ptr ds:[<OutputDebugStringW>]     |
00D11665  | 68 EC36D100              | push winantidbg0x100.D136EC                  | D136EC:L"### ~~~ "
00D1166A  | FF15 0830D100            | call dword ptr ds:[<OutputDebugStringW>]     |
00D11670  | 8B4D FC                  | mov ecx,dword ptr ss:[ebp-4]                 | ecx:EntryPoint, [ebp-04]:BaseThreadInitThunk
00D11673  | 51                       | push ecx                                     | ecx:EntryPoint
00D11674  | FF15 0830D100            | call dword ptr ds:[<OutputDebugStringW>]     |
00D1167A  | 68 0037D100              | push winantidbg0x100.D13700                  |
00D1167F  | FF15 0830D100            | call dword ptr ds:[<OutputDebugStringW>]     |
00D11685  | 68 0837D100              | push winantidbg0x100.D13708                  | D13708:L"### (Note: The flag could become corrupted if the process state is tampered with in any way.)\n\n"
```

  
