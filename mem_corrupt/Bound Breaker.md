![](./Bound%20Breaker-1778742247503.webp)

the program gave us all the address we need, the only problem is that we cant really reach >55 bytes without some tricks

try entering "-1" bytes to see if we can overflow the number (for example, a -1 in dword can be displayed 0xffffffff (this only happen with signed variables (even doe most recent program use signed variables)))

it work,