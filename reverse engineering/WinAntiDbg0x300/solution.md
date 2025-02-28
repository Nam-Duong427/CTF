# WinAntiDbg0x300 
## Difficulty
Medium 
## Hints
- There is an infinite loop to constantly check for the debugger.
- Get past that infinite loop. Maybe 'Patch' the binary to jump to the appropriate location?
- If you've done everything correctly, the flag should pop-up on your screen after 5 seconds of launching the program. The flag will also be printed to the Debug output to make it easy for you to copy the flag to the clipboard. See 'DebugView' program in Sysinternals Suite.
## Problem Description
This challenge is a little bit invasive. It will try to fight your debugger. With that in mind, debug the binary and get the flag!
This challenge executable is a GUI application and it requires admin privileges. And remember, the flag might get corrupted if you mess up the process's state.
Challenge can be downloaded [here](https://artifacts.picoctf.net/c_titan/25/WinAntiDbg0x300.zip). Unzip the archive with the password picoctf
If you get "VCRUNTIME140D.dll" and "ucrtbased.dll" missing error, then that means the Universal C Runtime library and Visual C++ Debug library are not installed on your Windows machine.
The quickest way to fix this is:
Download Visual Studio Community installer from https://visualstudio.microsoft.com/vs/community/
After the installer starts, first select 'Desktop development with C++' and then, in the right side column, select 'MSVC v143 - VS 2022 C++ x64/x86 build tools' and 'Windows 11 SDK' packages.
This will take ~30 mins to install any missing DLLs.
## Solution 
Firstly, we download the file, unzip and see another .pdb file besides the .exe file. 

Run the .exe file as admin.
And a window pops up says that we can not use debugger for this challenge. 
I tried and it kicks me out of the debugger. Debugger can't help.. 

Open the .exe file in Ghidra

**Note: Before analyze, please load the .pdb file into it!**

Follow the asm, we can see the file has been packed by UPX.
Now we have to unpack it and re-run in Ghidra.

I use [this](https://github.com/upx/upx/releases/tag/v5.0.0) to unpack the file.

```
D:\upx-5.0.0-win64>upx -d WinAntiDbg0x300.exe
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2025
UPX 5.0.0       Markus Oberhumer, Laszlo Molnar & John Reiser   Feb 20th 2025

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
     75264 <-     26624   35.37%    win32/pe     WinAntiDbg0x300.exe

Unpacked 1 file.
```

Now import it again to Ghidra and don't forget to load .pdb file before analyze !

Following the "you got the flag" in Defined Strings window, I get into this function.
```C

/* WARNING: Removing unreachable block (ram,0x004038e0) */
/* WARNING: Removing unreachable block (ram,0x00403911) */
/* WARNING: Removing unreachable block (ram,0x00403929) */

ulong __cdecl ChallengeThreadFunction(void *param_1)

{
  undefined local_384 [520];
  char local_17c [272];
  undefined4 local_6c [18];
  int local_24;
  undefined4 local_20;
  undefined4 local_1c;
  undefined4 local_18;
  undefined4 local_14;
  undefined4 local_10;
  int local_8;
  
  _memset(local_6c,0,0x44);
  local_6c[0] = 0x44;
  local_1c = 0;
  local_18 = 0;
  local_14 = 0;
  local_10 = 0;
  local_8 = 0;
  local_20 = GetCurrentProcessId();
  GetModuleFileNameW(0,local_384,0x104);
  snprintf(local_17c,0x110,"%ws %d");
  ComputeHash(2);
  do {
    local_24 = CreateProcessA(0,local_17c,0,0,0,0,0,0,local_6c,&local_1c);
    if (local_24 == 0) {
      MessageBoxW(appWindow,L"[FATAL ERROR]  Unable to create the child process. Challenge aborted. "
                  ,szTitle,0x10);
      Terminate(0xff);
    }
    WaitForSingleObject(local_1c,0xffffffff);
    GetExitCodeProcess(local_1c,&local_8);
    if (local_8 == 0xff) {
      MessageBoxW(appWindow,L"Something went wrong. Challenge aborted.",szTitle,0x10);
      Terminate(0xff);
    }
    else if (local_8 == 0xfe) {
      MessageBoxW(appWindow,
                  L"The debugger was detected but our process wasn\'t able to fight it. Challenge ab orted."
                  ,szTitle,0x10);
      Terminate(0xff);
    }
    else if (local_8 == 0xfd) {
      MessageBoxW(appWindow,
                  L"Our process detected the debugger and was able to fight it. Don\'t be surprised if the debugger crashed."
                  ,szTitle,0x10);
    }
    CloseHandle(local_1c);
    CloseHandle(local_18);
    Sleep(5000);
  } while( true );
}
```
As you can see, the hint tells us "There is an infinite loop to constantly check for the debugger."
And the while loop here.. 

We have to get past this loop. Let's check the asm code. 

Here is the loop :
```asm
        004038db e9  e0  fe       JMP        LAB_004037c0
                 ff  ff
```

We just simply get through it by delete the JMP, we need hit the clear code bytes option -> then hit patch instruction to edit.

Replace all of the question mark by NOP. This will get you past the loop. 

```asm
        004038db 90              NOP
        004038dc 90              NOP
        004038dd 90              NOP
        004038de 90              NOP
        004038df 90              NOP
```
And the pseudocode should be like this :
```C
  CloseHandle(local_1c);
  CloseHandle(local_18);
  Sleep(5000);
  ComputeHash(1);
  DecryptFlag(HASH);
  local_c = CharToWChar((char *)FLAG);
  if (local_c == (wchar_t *)0x0) {
    OutputDebugStringW(L"### Something went wrong...\n");
    Terminate(0xff);
  }
  MessageBoxW(appWindow,local_c,L"You got the flag!",0x40);
  OutputDebugStringW(L"### Good job! Here\'s your flag:\n");
  OutputDebugStringW(L"### ~~~ ");
  OutputDebugStringW(local_c);
  OutputDebugStringW(&DAT_00409738);
  OutputDebugStringW(
                    L"### (Note: The flag could become corrupted if the process state is tampered wi th in any way.)\n\n"
                    );
  local_28 = local_c;
  operator_delete[](local_c);
  return 0;
```

Good news !! Now just export the edited program and run again.
Because we have the decrypt flag function get everything done for us. 

Go to File -> Export program -> Choose Orginal File Format then hit OK

Now just run as Admin with the new .exe file !!

The flag should pops up as a small window in 5 seconds !!

All done ! 


