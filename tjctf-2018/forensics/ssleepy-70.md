# Ssleepy (70 Points)  
  
This challenge gives us a .pcap file containing data about a file transfer.  
Scrolling down, we see a request for a file called key.zip.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/ssleepy-1.PNG)  
  
We can copy the bytes of the ZIP file and export it to a zip file.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/ssleepy-2.PNG)  
Hover over the FTP Data and right click.  
In the menu that opens select **Export Selected Packet Bytes**.  
Save it as type **All files** with the .zip extension.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/ssleepy-3.PNG)  
  
Opening the zip reveals a keyfile that we can put into Wireshark to look at previously inaccessible data.  
Extract it to a location you'll remember, as it will be used later.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/ssleepy-4.PNG)  
  
To use the keyfile, go to Wireshark's settings by following the path **Edit > Preferences**.  
Expand the Protocols tab and scroll down to SSL.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/ssleepy-5.PNG)  

Select the Edit button next to RSA keys list.  
Then, add the keyfile with the following settings:  
* IP Address > 10.142.03  
* Port > 443  
* Protocol > http  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/ssleepy-6.PNG)  
  
Now if we scroll down, we can see a GET request being made to flag.jpg.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/ssleepy-7.PNG)  
  
Right clicking and selecting **Follow > SSL Stream** will get us to the decrypted image data.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/ssleepy-8.PNG)  
  
Only select the useful portion of the data, and show the data as raw hex.  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/ssleepy-9.PNG)  
  
Copy the hex data into a hex editor, preferably HxD.  
Using the known jpeg start and end markers, FFD8 and FFD9, we can trim the file to be a proper jpeg file.    
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/ssleepy-10.PNG)  
  
Save the image as a jpg file and open in your favorite image viewer:  
![](https://github.com/jazon-liu/ctf-writeups/blob/master/tjctf-2018/images/ssleepy-11.jpg)  
  
Flag: ```tjctf{wireshark_or_sharkwire?}```
