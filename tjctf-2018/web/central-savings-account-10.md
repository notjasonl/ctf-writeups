# Central Savings Account (10 Points)  
  
The challenge provides us with a website which contains what looks like a login page:  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/central-savings-account-1.PNG)  
  
Further inspection of the source reveals the MD5 hash of the password it's checking against:
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/central-savings-account-2.PNG)  
  
Plugging this MD5 hash into a hash database reveals the password and the flag:
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/central-savings-account-3.PNG)  
  
Flag: ```avalon```
