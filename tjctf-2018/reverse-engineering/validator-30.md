# Validator (30 Points)  
  
This challenge gives us a 32-bit Linux binary.  
It's easier to use Linux to solve this challenge, but if you're smart and/or are a masochist, you can use a tool such as IDA Pro.  
Using Linux either requires you to wget the file from the download link, or download it directly if Linux is your main OS.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/validator-1.PNG)  
  
Running strings on this binary shows us what appears to be a flag, but trying to submit it reveals that it was a scam.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/validator-2.PNG)  
  
It looks like we're gonna have to go deeper.  
  
Using gdb (GNU Debugger) we can take a closer look into the registers of the binary.  
First off, we'll need to install [peda](https://github.com/longld/peda).  
This makes the debugger display the registers, disassembly codes and memory addresses in a nice, pretty format.  
To install:  
1. Clone the source code from the repository: ```git clone https://github.com/longld/peda.git ~/peda```  
2. Add the path to the .gdbinit file: ```echo "source ~/peda/peda.py" >> ~/.gdbinit```  
  
After you have peda installed, you can begin to disassemble the file.  
Using a binary decompiler, such as IDA Pro, decompile the file into a C file.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/validator-3.PNG)  
  
Looking at the strcopy() on line 41,
