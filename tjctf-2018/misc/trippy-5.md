# Trippy (5 Points)  
  
In this challenge we are given a gif:  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/trippy-1.gif)  
  
As there are no hints given, we're on our own.  
My steps for this challenge were to wget the gif onto a server, where I could further analyze it.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/trippy-2.PNG)  
  
Since there might be data hidden within the data of the gif, I ran a strings + grep on it.  
Strings prints text strings hidden within non-text files, and grep searches for a string within text.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/trippy-3.PNG)  
  
Flag: ```tjctf{w0w}```
