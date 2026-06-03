![](./Latent%20Leak%20Hard-1780487828388.webp)
the output stop before the canary, which, again, mean that we cant leak the canary
![](./Latent%20Leak%20Hard-1780491015116.webp)
inspecting the stack, in which our input start at 0x7fffffffc770 (bc my input is 2 As), we find that the canary actually has a copy before $rbp-8, which is likely because of 