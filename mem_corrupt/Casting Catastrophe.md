![](img/Casting%20Catastrophe%20(inproc)-1780586973654.webp)
![](img/Casting%20Catastrophe%20(inproc)-1780587019189.webp)
it seems that we are allowed to determine a size for our input, and our main objective seems to be to overflow the buffer
however, the binary does a check after our input to make sure that v8 * v9 <= 0x74
![](img/Casting%20Catastrophe%20(inproc)-1780587144008.webp)
![](img/Casting%20Catastrophe%20(inproc)-1780587153701.webp)
looking at v8, v9 and nbytes we discover that v8 and v9 are unsigned intergers, and nbyte is unsigned int64
with this information, we can technically overflow v8 * v9 while not overflowing nbyte, as uint limit is 2 ^ 32 and uint64 limit is 2 ^ 64
for example we can do it with something like
```
Number of payload records to send: 2
Size of each payload record: 2147483649
Computed total payload size: 4294967298
```
here, 4294967298 would be equal to 2 in uint because of interger overflow
the rest of the task would be crafting a payload to rewrite the return address, which would be too simple to even bother explaining