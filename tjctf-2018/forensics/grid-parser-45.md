# Grid Parser (45 Points)  
  
In this challenge we are given a file with the extension of .grid.  
First, I moved the file over to my Linux server using wget, and renamed it to something typable.  
Running the ```file``` command on this file shows that it's a Microsoft OOXML file.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/grid-parser-1.PNG)  
  
An OOXML file is a zipped, XML-based, collection of office files.  
Here, we aren't interested in the contents, but rather the fact that it's zipped.  
Unzipping the file reveals a multitude of directories and files, but there is a file called 'password.png' in /xl/media.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/grid-parser-2.PNG)  
  
Initial inspection of the file reveals it to be a normal PNG file.  
However, running a binwalk on the file reveals a ZIP archive hidden at the end of the file.
The binwalk command allows you to find hidden files and executables within other files.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/grid-parser-3.PNG)  
  
Upon attemping to unzip this file, we are confronted with a password, as well as a warning of extra bytes.  
This is the PNG data, and will need to be removed before attempting to crack the password.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/grid-parser-4.PNG)  
  
Removing the extra data can be done using the ```dd``` command after changing the PNG to a ZIP file using ```mv [filename].png [filename.zip]```  
  
```dd bs=1780 skip=1 if=password.zip of=passwordtrim.zip```  
  
```bs``` = bytes to read/write  
```skip``` = chunks of size ```bytes``` to skip  
```if``` = input file  
```of``` = output file  
This skips the first 1780 bytes, the PNG data, and writes the output to passwordtrim.zip  
  
Now to move on to cracking the actual password.  
We can use the fcrackzip package to brute force the password.  
  
```fcrackzip -c a -b -l 1-6 passwordtrim.zip```  
  
```-c a``` = character set to use, here 'a' means lowercase alphabet  
```-b``` = bruteforce mode for password  
```-l 1-6``` = try all combinations between 1 and 6 characters long  
  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/grid-parser-5.PNG)  
  
This gives us 3 possible passwords to use, and testing them reveals that the last one is the correct password.  
Using that password, we can unzip the file and look at the contents:  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/grid-parser-6.PNG)  
  
Flag: ```tjctf{n0t_5u5_4t_4LL_r1gHt?}```
