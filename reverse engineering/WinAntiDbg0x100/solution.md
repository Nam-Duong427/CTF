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
Download and unzip the file given, we have a .exe file.
```
└─$ file WinAntiDbg0x100.exe
WinAntiDbg0x100.exe: PE32 executable (console) Intel 80386, for MS Windows, 5 sections
```
This is a PE32 so I use x32dbg to debug the program. 

Run the program in x32dbg, we see the hint!
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
DebugString: "### Level 1: Why did the clever programmer become a gardener? Because they discovered their talent for growing a 'patch' of roses!"
DebugString: "### Level 1: Why did the clever programmer become a gardener? Because they discovered their talent for growing a 'patch' of roses!"
```

Okay.. now I use Ghidra to see the pseudocode.
```C
undefined4 FUN_00401580(void)

{
  uint uVar1;
  int iVar2;
  BOOL BVar3;
  LPWSTR lpOutputString;
  undefined in_stack_fffffff4;
  
  uVar1 = FUN_00401130();
  if ((uVar1 & 0xff) == 0) {
    FUN_00401060(PTR_s__________________________________00405020,in_stack_fffffff4);
    FUN_00401060("### To start the challenge, you\'ll need to first launch this program using a debugger!\n"
                 ,in_stack_fffffff4);
  }
  else {
    OutputDebugStringW(L"\n");
    OutputDebugStringW(L"\n");
    FUN_004011b0();
    iVar2 = FUN_00401200();
    if (iVar2 == 0) {
      OutputDebugStringW(L"### Error reading the \'config.bin\' file... Challenge aborted.\n");
    }
    else {
      OutputDebugStringW(
                        L"### Level 1: Why did the clever programmer become a gardener? Because they discovered their talent for growing a \'patch\' of roses!\n"
                        );
      FUN_00401440(7);
      BVar3 = IsDebuggerPresent();
      if (BVar3 == 0) {
        FUN_00401440(0xb);
        FUN_00401530(DAT_00405404);
        lpOutputString = FUN_004013b0(DAT_00405408);
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
      }
      else {
        OutputDebugStringW(
                          L"### Oops! The debugger was detected. Try to bypass this check to get the flag!\n"
                          );
      }
    }
    free(DAT_00405410);
  }
  OutputDebugStringW(L"\n");
  OutputDebugStringW(L"\n");
  return 0;
}
```
According the code, to get the flag, we have to go through the condition If.
```C
      if (BVar3 == 0) {
        FUN_00401440(0xb);
        FUN_00401530(DAT_00405404);
        lpOutputString = FUN_004013b0(DAT_00405408);
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
      }
      else {
        OutputDebugStringW(
                          L"### Oops! The debugger was detected. Try to bypass this check to get the flag!\n"
                          );
      }
```
Looks like we have to modify the value of BVar3 to 0 to bypass the debugger. 
```C
00D115FC  | FF15 1430D100            | call dword ptr ds:[<IsDebuggerPresent>]      |
00D11602  | 85C0                     | test eax,eax                                 |
00D11604  | 74 15                    | je winantidbg0x100.D1161B                    |
00D11606  | 68 C835D100              | push winantidbg0x100.D135C8                  | D135C8:L"### Oops! The debugger was detected. Try to bypass this check to get the flag!\n"
```
  
