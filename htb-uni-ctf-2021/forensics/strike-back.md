# (forensics) Strike Back

### Proof of Concept

This challenge appears to be a pcap capture of Cobalt Strike data, along with its associated Beacon memory dump.  
Proof of this can be seen spotted in an example [traffic dump](https://isc.sans.edu/forums/diary/Example+of+Cleartext+Cobalt+Strike+Traffic+Thanks+Brad/27300/), and the exported HTTP objects from our pcap can be compared and seen to match the structure shown in the example traffic.  

Didier Stevens, bless his heart, has composed a suite of [scripts](https://github.com/DidierStevens/Beta) that can be used to manipulate CobaltStrike pcaps and Beacon memory dumps.  

First, we use `cs-parse-http-traffic.py` to get the key that the HTTP traffic in the pcap is encrypted with, using the memory dump we're given.  

```
python3 cs-parse-http-traffic.py -k unknown capture.pcap
```  

Passing a unknown key means the script will extract the encrypted data from the pcap, which we can then use with `cs-extract-key.py` and the process dump to try and figure out the key used for encryption.  

We grab a smaller chunk of encrypted data from the rather large output this spits out:  

```
Packet number: 69
HTTP response (for request 66 GET)
Length raw data: 48
a4940d6ff0a59421822467d80d1b620bc7ecfa661c452a85c0486b56aa752e908c4aeb3f2f0a64d9c02d7025713867ee
```

Then we toss it into `cs-extract-key.py`:

```
python3 cs-extract-key.py -t a4940d6ff0a59421822467d80d1b620bc7ecfa661c452a85c0486b56aa752e908c4aeb3f2f0a64d9c02d7025713867ee freesteam.dmp
```

Thankfully, this works as it should, and we get the AES and HMAC keys from the dump file:

```
File: freesteam.dmp
Searching for AES and HMAC keys
Searching after sha256\x00 string (0x4048a)
AES key position: 0x00447f81
AES Key:  3ae7f995a2392c86e3fa8b6fbc3d953a
HMAC key position: 0x0044b2a1
HMAC Key: bf2d35c0e9b64bc46e6d513c1d0f6ffe
SHA256 raw key: bf2d35c0e9b64bc46e6d513c1d0f6ffe:3ae7f995a2392c86e3fa8b6fbc3d953a
Searching for raw key
Searching after sha256\x00 string (0x441a49)
AES key position: 0x00447f81
AES Key:  3ae7f995a2392c86e3fa8b6fbc3d953a
HMAC key position: 0x0044b2a1
HMAC Key: bf2d35c0e9b64bc46e6d513c1d0f6ffe
```

Again using `cs-parse-http-traffic.py`, this time with the combined key that we've gleaned from the key extraction, we are able to decrypt all of the encrypted data that was sent.  

```
python3 cs-parse-http-traffic.py -k bf2d35c0e9b64bc46e6d513c1d0f6ffe:3ae7f995a2392c86e3fa8b6fbc3d953a capture.pcap
```

After decrypting our data, we can see a curious section of it which appears to show a list of files, which (hint, hint) ends up being pertinent information for this whole flag-hunting thing we're doing.  

```
C:\Users\npatrick\Desktop\*
D       0       11/19/2021 12:24:08     .
D       0       11/19/2021 12:24:08     ..
F       5175    11/11/2021 03:24:13     cheap_spare_parts_for_old_blimps.docx
F       282     11/10/2021 07:02:24     desktop.ini
F       24704   11/11/2021 03:22:16     gogglestown_citizens_osint.xlsx
F       62393   11/19/2021 12:24:10     orders.pdf
```
Reading a tiny bit deeper into the documentation for `cs-parse-http-traffic.py` reveals that we can append the `-e` flag to automatically decrypt and dump any payloads in the file to disk, which would let us view all of these curious little files for ourselves.  

```
python3 cs-parse-http-traffic.py -k -e bf2d35c0e9b64bc46e6d513c1d0f6ffe:3ae7f995a2392c86e3fa8b6fbc3d953a capture.pcap
```

Looking through the files we dumped:  

```
payload-00f542efefccd7a89a55c133180d8581.vir: PDF document, version 1.4
payload-2211925feba04566b12e81807ff9c0b4.vir: data
payload-b0cfbef2bd9a171b3f48e088b8ae2a99.vir: writable, executable, regular file, no read permission
payload-b25952a4fd6a97bac3ccc8f2c01b906b.vir: ASCII text, with no line terminators
```

Nothing is of interest in these files, with the notable exception of the PDF file...  
Opening it reveals to us our holy grail:  

![flag!](https://i.imgur.com/aW5SxGy.png)

Flag: `HTB{Th4nk_g0d_y0u_f0und_1t_0n_T1m3!!!!}`

### Solvers/Scripts Used

From [Didier Stevens](https://github.com/DidierStevens/Beta):
```
cs-parse-http-traffic.py
cs-extract-key.py
```
