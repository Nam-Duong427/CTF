# flags are stepic
## Difficulty
Medium 
## Hints 
In the country that doesn't exist, the flag persists
## Problem Description
A group of underground hackers might be using this legit site to communicate. Use your forensic techniques to uncover their message
Try it here! (launch the instance to get the challenge's link)  
## Solution 
When open the challenge's website, we see alot of flags of countries. 
The hint tells us that the flag persists in the country that doesn't exist!

Scroll all the way down, I found a flag called **"Upanzi, Republic The" **
![](https://github.com/Nam-Duong427/CTF/blob/main/forensics/flags%20are%20stepic/image.png)

Moreover, the width and height of the image is larger than others. 
![](https://github.com/Nam-Duong427/CTF/blob/main/forensics/flags%20are%20stepic/img_size.png)

So I decide to download the image and check it out if it has any data was hidden by using steganography technique 
I can tell that this problem has another hint in the title :) 

There is a python library called **stepic** - is used for finding hidden data in a image. 
[stepic library](https://pypi.org/project/stepic/)

Now let's it handle the rest.. 
```python
import stepic
from PIL import Image

def main():
    # dont forget to modify your image's location
    flag_img = Image.open(r'upz.png')
    print(stepic.decode(flag_img))

if __name__ == "__main__":
    main()
```


