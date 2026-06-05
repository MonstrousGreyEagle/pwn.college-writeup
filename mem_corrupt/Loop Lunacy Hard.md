![](./Loop%20Lunacy%20Hard-1780623878573.webp)
it seems that the binary read each byte into the stack (see read( ,  , 1uLL))
![](./Loop%20Lunacy%20Hard-1780623952591.webp)
check the stack we discover that the binary store the pointer after our input start (note that the pointer update after our input, so here)