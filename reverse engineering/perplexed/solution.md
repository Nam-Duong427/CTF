# perplexed 
## Difficulty
Medium 
## Hints 
(None)
## Problem Description
Download the binary [here](https://challenge-files.picoctf.net/c_verbal_sleep/2326718ce11c5c89056a46fce49a5e46ab80e02d551d87744306ae43a4767e06/perplexed).
## Solution
Firstly, I open ghidra as usual to analyze the binary. And see that the program requires me to enter a password then use a function named check() to check if it correct or not. 
```C
undefined8 check(char *param_1)

{
  size_t sVar1;
  undefined8 uVar2;
  size_t sVar3;
  char local_58 [36];
  uint local_34;
  uint local_30;
  undefined4 local_2c;
  int local_28;
  uint local_24;
  int local_20;
  int local_1c;
  
  sVar1 = strlen(param_1);
  if (sVar1 == 0x1b) {
    local_58[0] = -0x1f;
    local_58[1] = -0x59;
    local_58[2] = '\x1e';
    local_58[3] = -8;
    local_58[4] = 'u';
    local_58[5] = '#';
    local_58[6] = '{';
    local_58[7] = 'a';
    local_58[8] = -0x47;
    local_58[9] = -99;
    local_58[10] = -4;
    local_58[0xb] = 'Z';
    local_58[0xc] = '[';
    local_58[0xd] = -0x21;
    local_58[0xe] = 'i';
    local_58[0xf] = 0xd2;
    local_58[0x10] = -2;
    local_58[0x11] = '\x1b';
    local_58[0x12] = -0x13;
    local_58[0x13] = -0xc;
    local_58[0x14] = -0x13;
    local_58[0x15] = 'g';
    local_58[0x16] = -0xc;
    local_1c = 0;
    local_20 = 0;
    local_2c = 0;
    for (local_24 = 0; local_24 < 0x17; local_24 = local_24 + 1) {
      for (local_28 = 0; local_28 < 8; local_28 = local_28 + 1) {
        if (local_20 == 0) {
          local_20 = 1;
        }
        local_30 = 1 << (7U - (char)local_28 & 0x1f);
        local_34 = 1 << (7U - (char)local_20 & 0x1f);
        if (0 < (int)((int)param_1[local_1c] & local_34) !=
            0 < (int)((int)local_58[(int)local_24] & local_30)) {
          return 1;
        }
        local_20 = local_20 + 1;
        if (local_20 == 8) {
          local_20 = 0;
          local_1c = local_1c + 1;
        }
        sVar3 = (size_t)local_1c;
        sVar1 = strlen(param_1);
        if (sVar3 == sVar1) {
          return 0;
        }
      }
    }
    uVar2 = 0;
  }
  else {
    uVar2 = 1;
  }
  return uVar2;
}
```
check() will return 0 if the password is correct. But the problem here is that the given hex string to be compared with the input is somehow separated and not in a normal format of a hex string. 

So I try Binary Ninja on [Decompile Explorer](https://dogbolt.org/) to see if we can get the compare hex in a normal format of a hex string. And it works!
```C
int64_t check(char* arg1)
{
    if (strlen(arg1) != 0x1b)
        return 1;
    
    int64_t var_58;
    __builtin_memcpy(&var_58, 
        "\xe1\xa7\x1e\xf8\x75\x23\x7b\x61\xb9\x9d\xfc\x5a\x5b\xdf\x69\xd2\xfe\x1b\xed\xf4\xed\x67\xf4", 
        0x17);
    int32_t var_1c_1 = 0;
    int32_t var_20_1 = 0;
    int32_t var_2c_1 = 0;
    
    for (int32_t i = 0; i <= 0x16; i += 1)
    {
        for (int32_t j = 0; j <= 7; j += 1)
        {
            if (!var_20_1)
                var_20_1 += 1;
            
            int32_t rax_17;
            rax_17 = (arg1[var_1c_1] & 1 << (7 - var_20_1)) > 0;
            
            if (rax_17 != (*(&var_58 + i) & 1 << (7 - j)) > 0)
                return 1;
            
            var_20_1 += 1;
            
            if (var_20_1 == 8)
            {
                var_20_1 = 0;
                var_1c_1 += 1;
            }
            
            if (var_1c_1 == strlen(arg1))
                return 0;
        }
    }
    
    return 0;
}
```
So we now got the hex string which is used to compare with the input! 
```
e1a71ef875237b61b99dfc5a5bdf69d2fe1bedf4ed67f4
```
Get back to ghidra, jump to lines where the input string is compared 
```C
    for (local_24 = 0; local_24 < 0x17; local_24 = local_24 + 1) {
      for (local_28 = 0; local_28 < 8; local_28 = local_28 + 1) {
        if (local_20 == 0) {
          local_20 = 1;
        }
        local_30 = 1 << (7U - (char)local_28 & 0x1f);
        local_34 = 1 << (7U - (char)local_20 & 0x1f);
        if (0 < (int)((int)param_1[local_1c] & local_34) !=
            0 < (int)((int)local_58[(int)local_24] & local_30)) {
          return 1;
        }
        local_20 = local_20 + 1;
        if (local_20 == 8) {
          local_20 = 0;
          local_1c = local_1c + 1;
        }
```
**_local_58** stands for the given hex string to be compared with the input string which is called **param_1** in pseudocode

We can see that there is an **if statement** is used to compared 2 small statements together, each statement takes one by one element in each string and performs AND bitwise operator and return a value

Now all I need is just to figure out what special in these 2 statements? I can see that the input string will be performed AND bitwise operator with local_34 and local_34 goes from 64 - 32 - 16 ... 1 ! 

To let that statement return a value > 0, we need the element in the string input has at least 1 bit 1!

And here is how it goes!

```python
def solve():
    # binary ninja
    given_hex = "e1a71ef875237b61b99dfc5a5bdf69d2fe1bedf4ed67f4"
    l1c = 0
    l20 = 0
    l2c = 0
    # base on pseudocode in ghidra, input text has 27 characters
    flag = [0] * 0x1b
    for i in range(0, len(given_hex) - 1, 2):
        b = given_hex[i] + given_hex[i + 1]
        for j in range(8):
            if l20 == 0:
                l20 = 1
            l30 = 1 << ((7 - j) & 0x1f)
            l34 = 1 << ((7 - l20) & 0x1f)
            # statement1 = int(int(plain[l1c]) & l34) > 0
            statement2 = int(int(b, 16) & l30) > 0
            if statement2:
                flag[l1c] |= l34
                # using OR bitwise operator to make sure that when AND bitwise operator
                # is used with l34 will return a value has at least 1 bit 1 in it
                # this returns a value > 0 to meet the requirements of the statement1
            l20 += 1
            if l20 == 8:
                l20 = 0
                l1c += 1
    # l1c goes from 0 -> 0x1b - 1
    if l1c == 0x1b - 1:
        print("".join(chr(i) for i in flag))


def main():
    solve()


if __name__ == "__main__":
    main()
```
That's it :) 
