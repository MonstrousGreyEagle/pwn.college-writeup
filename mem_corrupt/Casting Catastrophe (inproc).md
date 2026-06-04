![](./Casting%20Catastrophe%20(inproc)-1780586973654.webp)
![](./Casting%20Catastrophe%20(inproc)-1780587019189.webp)
it seems that we are allowed to determine a size for our input, and our main objective seems to be to overflow the buffer
however, the binary does a check after our input to make sure that v8 * v9 <= 0x74
![](./Casting%20Catastrophe%20(inproc)-1780587144008.webp)
![](./Casting%20Catastrophe%20(inproc)-1780587153701.webp)
looking at v8, v9 and nbytes we discover that v8 and v9 are unsigned intergers, and nbyte is unsigned int64
with this information, we can technically overflow v8 * v9 while
2
2147483649