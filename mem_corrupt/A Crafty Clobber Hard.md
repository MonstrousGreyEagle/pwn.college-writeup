![](./A%20Crafty%20Clobber%20Hard-1780626858877.webp)
![](./A%20Crafty%20Clobber%20Hard-1780626979558.webp)
inspecting the pseudo code and the vmmap, we acknowledge that this challenge is a copy of [Canary Conundrum Hard](Canary%20Conundrum%20Hard.md) with an extra checker at the end
this means 2 things:
	1. The solution is almost the same as [Canary Conundrum Hard](Canary%20Conundrum%20Hard.md)
	2. we have 0x28 bytes for our setuid and shell-spawning code (see the location of buf and v10)
that said,