# Weird Logo (5 Points)  
In this challenge we are given an image that appears to be a logo:  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/weird-logo-1.png)  
  
Using the information given:  
"This company's logo stands in contrast of those of other leading edge tech companies with its poor design"
  
The mention of contrast leads us to believe that this may include text hidden in the image.  
We use [StegSolve](http://www.caesum.com/handbook/Stegsolve.jar) to look through the bit planes:  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/weird-logo-2.png)  
  
After reaching the red plane, we see the flag suddenly appear:
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/weird-logo-3.png)  
  
Flag: ```tjctf{in_plain_sight}```
