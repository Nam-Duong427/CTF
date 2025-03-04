# Need For Speed
## Difficulty
Hard
## Hints
What is the final key?
## Problem Description
The name of the game is [speed](https://www.youtube.com/watch?v=8piqd2BWeGI). Are you quick enough to solve this problem and keep it above 50 mph? [need-for-speed](https://jupiter.challenges.picoctf.org/static/f9abc386dfb1309e687344783f208b20/need-for-speed).

## Solution 
The given file is an excutable file. 
```
└─$ file need-for-speed
need-for-speed: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=b4b1e824082c140091043151ab990149efa44806, not stripped
```
Firstly, run the program to see what it looks like. 
```
└─$ ./need-for-speed
Keep this thing over 50 mph!
============================

Creating key...
Not fast enough. BOOM!
```
Decomplie in Ghidra we can see the program's pseudocode.
```C
undefined8 main(void)

{
  header();
  set_timer();
  get_key();
  print_flag();
  return 0;
}
```
According to the pseudocode we check one by one function.. not thing special except a function called set_timer(). 
```C
void set_timer(void)

{
  __sighandler_t p_Var1;
  
  p_Var1 = __sysv_signal(0xe,alarm_handler);
  if (p_Var1 == (__sighandler_t)0xffffffffffffffff) {
    puts("\n\nSomething bad happened here. ");
                    /* WARNING: Subroutine does not return */
    exit(0);
  }
  alarm(1);
  return;
}
```
What is alarm_handler?
```C
void alarm_handler(void)

{
  puts("Not fast enough. BOOM!");
                    /* WARNING: Subroutine does not return */
  exit(0);
}
```
Just a simple function to print a string. 
Seems like set_timer() is a function that prevent user to be able to go to get_key() and print_flag() function. 

Let's try to delete it it from the program. 

That function is at 00100938.
```C
00100938 e8  f2  fe       CALL       set_timer 
         ff  ff
```
Right-click the hit Clear Code Bytes and replace everything in it with NOP by Ctrl+Shift+G. 
That's how we can remove the function. 
It should looks like this
```asm
00100938 90              NOP
00100939 90              NOP
0010093a 90              NOP
0010093b 90              NOP
0010093c 90              NOP
```
```C

undefined8 main(void)

{
  header();
  get_key();
  print_flag();
  return 0;
}
```
And now the set_timer() function has dissapeared!

Go to File -> Export Program and save it as Original File. 
Now just the new program!
```
└─$ ./need-for-speed
Keep this thing over 50 mph!
============================

Creating key...

Finished
Printing flag:                                                                         
PICOCTF{..redacted..}                                                  
```
All done!




