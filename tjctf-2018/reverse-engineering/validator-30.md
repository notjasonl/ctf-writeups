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
  
Looking at the strcopy() on line 41, we can spot the fake flag that was found in strings.  
Down 3 lines, on line 44, we see the check for the length of the input.  
Since its checking for an input of length 43, we know how long of an input to pass in.  
  
Jumping down to strcmp() on line 51 shows the input string being compared against a variable, s1.  
Since s1 must be the correct flag, we need to set a breakpoint before there to get the value.  
  
Back in gdb, we disassemble the main function in the binary:  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/validator-4.PNG)  
  
As we can see, the strcmp is seen at an offset of +192 from the beginning of main.
We can set a breakpoint here by typing ```break *main+192``` into the gdb shell.  
  
Then, type ```r``` followed by 43 random characters into the gdb shell, and the flag should show in the registers.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/validator-5.PNG)  
  
Flag: ```tjctf{ju57_c4ll_m3_35r3v3r_60d_fr0m_n0w_0n}```
