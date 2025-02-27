# WinAntiDbg0x200 
## Difficulty
Medium 
## Hints 
Hints will be displayed to the Debug console. Good luck!
## Problem Description
If you have solved WinAntiDbg0x100, you'll discover something new in this one. Debug the executable and find the flag!
This challenge executable is a Windows console application, and you can start by running it using Command Prompt on Windows.
This executable requires admin privileges. You might want to start Command Prompt or your debugger using the 'Run as administrator' option.
Challenge can be downloaded [here](https://artifacts.picoctf.net/c_titan/147/WinAntiDbg0x200.zip). Unzip the archive with the password picoctf
## Solution 
The file given is a PE32. So again x32dbg will be with us this time. 
```
└─$ file WinAntiDbg0x200.exe
WinAntiDbg0x200.exe: PE32 executable (console) Intel 80386, for MS Windows, 5 sections
```
Open it in Ghira to see pseudocode. 
```C
undefined4 __cdecl FUN_004016e0(int param_1,int param_2)

{
  char cVar1;
  int iVar2;
  HANDLE hObject;
  DWORD DVar3;
  BOOL BVar4;
  uint uVar5;
  LPWSTR lpOutputString;
  undefined in_stack_fffffff0;
  
  iVar2 = FUN_004012f0();
  if (iVar2 == 0) {
    FUN_00401910("[ERROR] There are permission issues. This program requires debug privileges and hence you might want to run it as an Admin.\n"
                 ,in_stack_fffffff0);
    FUN_00401910("Challenge aborted. Please run this program as an Admin. Exiting now...\n",
                 in_stack_fffffff0);
                    // WARNING: Subroutine does not return
    exit(0xff);
  }
  hObject = CreateMutexW((LPSECURITY_ATTRIBUTES)0x0,0,L"WinAntiDbg0x200");
  if (hObject == (HANDLE)0x0) {
    FUN_00401910("[ERROR] Failed to create the Mutex. Exiting now...\n",in_stack_fffffff0);
                    // WARNING: Subroutine does not return
    exit(0xff);
  }
  DVar3 = GetLastError();
  if (DVar3 == 0xb7) {
    if (param_1 != 2) {
      FUN_00401910("[ERROR] Expected an argument\n",in_stack_fffffff0);
                    // WARNING: Subroutine does not return
      exit(0xbeef);
    }
    DVar3 = atoi(*(char **)(param_2 + 4));
    BVar4 = DebugActiveProcess(DVar3);
    if (BVar4 != 0) {
                    // WARNING: Subroutine does not return
      exit(0);
    }
                    // WARNING: Subroutine does not return
    exit(0xbeef);
  }
  FUN_00401910(PTR_s__________________________________00405000,in_stack_fffffff0);
  uVar5 = FUN_00401600();
  if ((uVar5 & 0xff) == 0) {
    FUN_00401910("### To start the challenge, you\'ll need to first launch this program using a debugger!\n"
                 ,in_stack_fffffff0);
    goto LAB_004018de;
  }
  OutputDebugStringW(L"\n");
  OutputDebugStringW(L"\n");
  FUN_00401400();
  iVar2 = FUN_00401450();
  if (iVar2 == 0) {
    OutputDebugStringW(L"### Error reading the \'config.bin\' file... Challenge aborted.\n");
  }
  else {
    OutputDebugStringW(
                      L"### Level 2: Why did the parent process get a promotion at work? Because it had a \"fork-tastic\" child process that excelled in multitasking!\n"
                      );
    FUN_00401090(3);
    cVar1 = FUN_004011d0();
    if (cVar1 == '\0') {
      BVar4 = IsDebuggerPresent();
      if (BVar4 == 0) {
        FUN_00401090(1);
        FUN_00401180(DAT_0040509c);
        lpOutputString = FUN_00401000(DAT_004050a0);
        if (lpOutputString == (LPWSTR)0x0) {
          OutputDebugStringW(L"### Something went wrong...\n");
        }
        else {
          OutputDebugStringW(L"### Good job! Here\'s your flag:\n");
          OutputDebugStringW(L"### ~~~ ");
          OutputDebugStringW(lpOutputString);
          OutputDebugStringW(L"\n");
          OutputDebugStringW(
                            L"### (Note: The flag could become corrupted if the process state is tampered with in any way.)\n\n"
                            );
          free(lpOutputString);
        }
        goto LAB_004018ce;
      }
    }
    OutputDebugStringW(
                      L"### Oops! The debugger was detected. Try to bypass this check to get the flag!\n"
                      );
  }
LAB_004018ce:
  free(DAT_00405098);
LAB_004018de:
  CloseHandle(hObject);
  OutputDebugStringW(L"\n");
  OutputDebugStringW(L"\n");
  return 0;
}
```
We can immediately see which lines of code is important to bypass the debugger.
```C
  if ((uVar5 & 0xff) == 0) {
    FUN_00401910("### To start the challenge, you\'ll need to first launch this program using a debugger!\n"
                 ,in_stack_fffffff0);
    goto LAB_004018de;
  }

-----------------------------------------------------------------------------------------------------------------------

    if (cVar1 == '\0') {
      BVar4 = IsDebuggerPresent();
      if (BVar4 == 0) {
        FUN_00401090(1);
        FUN_00401180(DAT_0040509c);
        lpOutputString = FUN_00401000(DAT_004050a0);
        if (lpOutputString == (LPWSTR)0x0) {
          OutputDebugStringW(L"### Something went wrong...\n");
        }
        else {
          OutputDebugStringW(L"### Good job! Here\'s your flag:\n");
          OutputDebugStringW(L"### ~~~ ");
          OutputDebugStringW(lpOutputString);
          OutputDebugStringW(L"\n");
          OutputDebugStringW(
                            L"### (Note: The flag could become corrupted if the process state is tampered with in any way.)\n\n"
                            );
          free(lpOutputString);
        }
        goto LAB_004018ce;
      }
    }
    OutputDebugStringW(
                      L"### Oops! The debugger was detected. Try to bypass this check to get the flag!\n"
                      );
  }
```
**Main points** : 
- (uVar5 & 0xff) must not be 0 to run the program. 
- iVar2 must be 0 to avoid error. 
- cVar1 must be 0 to go next.
- If Bvar4 is not 0, then our Debugger will be detected.
- Otherwise if Bvar4 is indeed 0, the flag will appear means we just bypassed the AntiDebug successfully. 

Now we will open x32dbg to modify these variables.
```asm
00D117BD  | 85C9                     | test ecx,ecx                                 |
00D117BF  | 75 12                    | jne winantidbg0x200.D117D3                   |
00D117C1  | 68 8836D100              | push winantidbg0x200.D13688                  | D13688:"### To start the challenge, you'll need to first launch this program using a debugger!\n"
00D117C6  | E8 45010000              | call winantidbg0x200.D11910                  |
00D117CB  | 83C4 04                  | add esp,4                                    |
00D117CE  | E9 0B010000              | jmp winantidbg0x200.D118DE                   |
00D117D3  | 68 E036D100              | push winantidbg0x200.D136E0                  |
00D117D8  | FF15 4830D100            | call dword ptr ds:[<OutputDebugStringW>]     |
00D117DE  | 68 E436D100              | push winantidbg0x200.D136E4                  |
00D117E3  | FF15 4830D100            | call dword ptr ds:[<OutputDebugStringW>]     |
00D117E9  | E8 12FCFFFF              | call winantidbg0x200.D11400                  |
00D117EE  | E8 5DFCFFFF              | call winantidbg0x200.D11450                  |
00D117F3  | 85C0                     | test eax,eax                                 |
00D117F5  | 75 10                    | jne winantidbg0x200.D11807                   |
00D117F7  | 68 E836D100              | push winantidbg0x200.D136E8                  | D136E8:L"### Error reading the 'config.bin' file... Challenge aborted.\n"
00D117FC  | FF15 4830D100            | call dword ptr ds:[<OutputDebugStringW>]     |
00D11802  | E9 C7000000              | jmp winantidbg0x200.D118CE                   |
00D11807  | 68 6837D100              | push winantidbg0x200.D13768                  | D13768:L"### Level 2: Why did the parent process get a promotion at work? Because it had a \"fork-tastic\" child process that excelled in multitasking!\n"
00D1180C  | FF15 4830D100            | call dword ptr ds:[<OutputDebugStringW>]     |
00D11812  | 6A 03                    | push 3                                       |
00D11814  | E8 77F8FFFF              | call winantidbg0x200.D11090                  |
00D11819  | 83C4 04                  | add esp,4                                    |
00D1181C  | E8 AFF9FFFF              | call winantidbg0x200.D111D0                  |
00D11821  | 0FB6D0                   | movzx edx,al                                 |
00D11824  | 85D2                     | test edx,edx                                 |
00D11826  | 75 0A                    | jne winantidbg0x200.D11832                   |
00D11828  | FF15 5030D100            | call dword ptr ds:[<IsDebuggerPresent>]      |
00D1182E  | 85C0                     | test eax,eax                                 |
00D11830  | 74 15                    | je winantidbg0x200.D11847                    |
00D11832  | 68 8838D100              | push winantidbg0x200.D13888                  | D13888:L"### Oops! The debugger was detected. Try to bypass this check to get the flag!\n"
```
Read it carefully. 
- We will set the breakpoints for each of these address 00D117BD, 00D117F3, 00D11824 and 00D1182E.
- Then we will set EIP and hit run to go by each by each to follow the program.

**Note : to modify value, please go to FPU session and double click the value that you want to modify.**

First variale need to be changed is at 00D117BD, which is $ecx. Base on the code, $ecx must be 1 for the program to run correctly.

Go to FPU and see if $ecx is 1. If not change it to 1.

Then set EIP at the current address.
Then hit run to execute and to go to the next one.
In log tab should appear this : 
```
DebugString: "_            _____ _______ ______  
       (_)          / ____|__   __|  ____| 
  _ __  _  ___ ___ | |       | |  | |__    
 | '_ \| |/ __/ _ \| |       | |  |  __|   
 | |_) | | (_| (_) | |____   | |  | |      
 | .__/|_|\___\___/ \_____|  |_|  |_|      
 | |                                       
 |_|                                       
  Welcome to the Anti-Debug challenge!"
DebugString: "_            _____ _______ ______  
       (_)          / ____|__   __|  ____| 
  _ __  _  ___ ___ | |       | |  | |__    
 | '_ \| |/ __/ _ \| |       | |  |  __|   
 | |_) | | (_| (_) | |____   | |  | |      
 | .__/|_|\___\___/ \_____|  |_|  |_|      
 | |                                       
 |_|                                       
  Welcome to the Anti-Debug challenge!"
INT3 breakpoint at winantidbg0x200.00D117F3!
Breakpoint at 00D117BD set!
00D117BD (13703101d) winantidbg0x200.00D117BD
INT3 breakpoint at winantidbg0x200.00D117BD!
```
After that, you should go in the next breakpoint, which is 00D117F3. If not, please set EIP for it and execute again until you see like below. 
And this time, $eax must be not 0 to avoid the error.

Now set $eax = 1. 

Now you should see this in log tab : 
```
DebugString: "### Level 2: Why did the parent process get a promotion at work? Because it had a "fork-tastic" child process that excelled in multitasking!"
DebugString: "### Level 2: Why did the parent process get a promotion at work? Because it had a "fork-tastic" child process that excelled in multitasking!"
Thread 4644 created, Entry: ntdll.77E35B70, Parameter: 011C3B18
INT3 breakpoint at winantidbg0x200.00D11824!
```
So in the next breakpoint, $edx should be 0.
Do the same thing. 
Hit run the move to the next one!

In final step, which is the $eax, set it to 0 based on the given pseudocode.
Then hit run again! 
```
DebugString: "### Good job! Here's your flag:"
DebugString: "### Good job! Here's your flag:"
DebugString: "### ~~~"
DebugString: "### ~~~"
DebugString: "picoCTF{..redacted..}"
DebugString: "picoCTF{..redacted..}"
DebugString: "### (Note: The flag could become corrupted if the process state is tampered with in any way.)"
DebugString: "### (Note: The flag could become corrupted if the process state is tampered with in any way.)"
DLL Loaded: 74E20000 C:\Windows\SysWOW64\kernel.appcore.dll
Thread 7704 exit
Thread 10816 exit
Thread 23144 exit
Process stopped with exit code 0x0 (0)
Saving database to D:\snapshot_2025-01-17_12-45 (1)\release\x32\db\WinAntiDbg0x200.exe.dd32 16ms
Debugging stopped!
```
Done for now.. !

