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
We can immediately see which lines of code is important to bypass the debugger
```C
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
As you can see, Bvar4 stands for the existant of AntiDebug and the program it checks BVar4 to see if it's 0 or not. 

- Specifically if Bvar4 is 0, then our Debugger has been detected.
- Otherwise, the flag will appear means we just bypassed the AntiDebug successfully. 


