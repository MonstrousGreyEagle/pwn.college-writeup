![img/Pasted-image 20260509071828.png](img/Pasted-image 20260509071828.png)

![img/Pasted-image 20260509071843.png](img/Pasted-image 20260509071843.png)

reverse the key from unk, we get "626a777775636c6865636b7763766f73"
```
import sys
payload=bytes.fromhex("626a777775636c6865636b7763766f73")
sys.stdout.buffer.write(payload)
```

```
hacker@reverse-engineering~meager-mangler-hard:/challenge$ python3 /tmp/code.py | ./*
###
### Welcome to ./meager-mangler-hard!
###

This license verifier software will allow you to read the flag. However, before you can do so, you must verify that you
are licensed to read flag files! This program consumes a license key over stdin. Each program may perform entirely
different operations on that input! You must figure out (by reverse engineering this program) what that license key is.
Providing the correct license key will net you the flag!

Ready to receive your license key!

Checking the received license key!

Wrong! No flag for you!
```

well thats strange
```
hacker@reverse-engineering~meager-mangler-hard:/challenge$ gdb *
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
Reading symbols from meager-mangler-hard...
(No debugging symbols found in meager-mangler-hard)
(gdb) b memcmp
Breakpoint 1 at 0x5fbb3b6ed170
(gdb) r
Starting program: /challenge/meager-mangler-hard 
###
### Welcome to /challenge/meager-mangler-hard!
###

This license verifier software will allow you to read the flag. However, before you can do so, you must verify that you
are licensed to read flag files! This program consumes a license key over stdin. Each program may perform entirely
different operations on that input! You must figure out (by reverse engineering this program) what that license key is.
Providing the correct license key will net you the flag!

Ready to receive your license key!

12345678abcdefgh            
Checking the received license key!


Breakpoint 1.1, __memcmp_sse4_1 () at ../sysdeps/x86_64/multiarch/memcmp-sse4.S:43
warning: 43     ../sysdeps/x86_64/multiarch/memcmp-sse4.S: No such file or directory
(gdb) 
(gdb) x/2gx $rsi
0x59f6d50c0010: 0xda48d45cdc5dd058      0xdd41c85cca48d343
(gdb) x/2gx $rdi
0x7ffc19686690: 0x8949dc4fda4dd843      0x8e198c1f8a4a8813
```

well run in debug and check rsi and rdi value at memcmp (AMD64 use registers rsi and rdi as 2 argument for memcmp), we realise that the pseudo code ida produce is probably false (thats rare)
not only that, the final value (at rsi) is false too (it supposed to be 58... or something according to ida)
xor "8949dc4fda4dd8438e198c1f8a4a8813" with "bf2bbf2bbf2bbf2bbf2bbf2bbf2bbf2b" produce "36626364656667683132333435613738", which is "6bcdefgh12345a78"
with to this logic we can reverse the intended key
the intended key is "626A777775656C6863636B7763766F73", which is, conveniently, "bjwwuelhcckwcvos"