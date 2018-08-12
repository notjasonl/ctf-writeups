# Central Savings Account (10 Points)  
  
The challenge provides us with a website which contains what looks like a login page:  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/central-savings-account-1.PNG)  
  
However, we will not be compromising this login page, as the flag for this challenge is the password.  
Peeking into the source reveals a javascript file:
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/central-savings-account-4.PNG)

Further inspection of the file shows that it is hashing the password using MD5, and checking that against an MD5 hash:
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/central-savings-account-2.PNG)  
  
This hash is easily decrypted by plugging it into a hash database:
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/central-savings-account-3.PNG)  
  
Flag: ```avalon```
