```
hacker@reverse-engineering~terrible-token-hard:~$ cd /challenge
hacker@reverse-engineering~terrible-token-hard:/challenge$ ./*
###
### Welcome to ./terrible-token-hard!
###

This license verifier software will allow you to read the flag. However, before you can do so, you must verify that you
are licensed to read flag files! This program consumes a license key over stdin. Each program may perform entirely
different operations on that input! You must figure out (by reverse engineering this program) what that license key is.
Providing the correct license key will net you the flag!

Ready to receive your license key!

abcde
Checking the received license key!

Wrong! No flag for you!
```

No output 

```
hacker@reverse-engineering~terrible-token-hard:/challenge$ gdb *
GNU gdb (GDB) 16.3
Copyright (C) 2024 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-unknown-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
/home/hacker/.gdbinit:1: Error in sourced command file:
No symbol table is loaded.  Use the "file" command.
Reading symbols from terrible-token-hard...
(No debugging symbols found in terrible-token-hard)
(gdb) disas main
No symbol table is loaded.  Use the "file" command.
```

scrambled binary

![[img/Pasted image 20260508212027.png]]

IDA clean everything up anyway (yay), so we just need to read the source
It seems that the hard version does the exact same thing, just with a different key
double click the variable "aUmjpc" to see the key

![[img/Pasted image 20260508212438.png]]

so our key is "umjpc"