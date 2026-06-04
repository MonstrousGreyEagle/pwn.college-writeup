![](./Casting%20Catastrophe%20(inproc)-1780586973654.webp)
![](./Casting%20Catastrophe%20(inproc)-1780587019189.webp)
it seems that we are allowed to determine a size for our input, and our main objective seems to be to overflow the buffer
however, the binary does a check after our input to make sure that v8 * v9 <= 0x74
![](./Casting%20Catastrophe%20(inproc)-1780587144008.webp)
![](./Casting%20Catastrophe%20(inproc)-1780587153701.webp)

2
2147483649