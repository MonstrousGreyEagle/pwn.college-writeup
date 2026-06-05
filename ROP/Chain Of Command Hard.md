![](./Chain%20Of%20Command%20Hard-1780651709693.webp)
![](./Chain%20Of%20Command%20Hard-1780651733139.webp)
![](./Chain%20Of%20Command%20Hard-1780651775716.webp)

the binary contain many fragments of win function, each with a checker

our objective is clear: find a way to manipulate rdi in our ROP chain

![](./Chain%20Of%20Command%20Hard-1780651841752.webp)

with ROPgadget, we can find a gadget which pop rdi for us

the structure of our payload is

```
pop1 + [1] + pop2 + [2] + pop3 + [3] + pop4 
```