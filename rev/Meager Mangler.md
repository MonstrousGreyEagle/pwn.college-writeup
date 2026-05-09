```
hacker@reverse-engineering~meager-mangler-easy:~$ cd /challenge
hacker@reverse-engineering~meager-mangler-easy:/challenge$ ./*
###
### Welcome to ./meager-mangler-easy!
###

This license verifier software will allow you to read the flag. However, before you can do so, you must verify that you
are licensed to read flag files! This program consumes a license key over stdin. Each program may perform entirely
different operations on that input! You must figure out (by reverse engineering this program) what that license key is.
Providing the correct license key will net you the flag!

Ready to receive your license key!

asdghs
Initial input:

        61 73 64 67 68 73 0a 00 00 00 00 00 00 00 00 00 00 

This challenge is now mangling your input using the `xor` mangler with key `0x899e30`

This mangled your input, resulting in:

        e8 ed 54 ee f6 43 83 9e 30 89 9e 30 89 9e 30 89 9e 

This challenge is now mangling your input using the `xor` mangler with key `0x9ce8`

This mangled your input, resulting in:

        74 05 c8 06 6a ab 1f 76 ac 61 02 d8 15 76 ac 61 02 

This challenge is now mangling your input using the `swap` mangler for indexes `3` and `14`.

This mangled your input, resulting in:

        74 05 c8 ac 6a ab 1f 76 ac 61 02 d8 15 76 06 61 02 

The mangling is done! The resulting bytes will be used for the final comparison.

Final result of mangling input:

        74 05 c8 ac 6a ab 1f 76 ac 61 02 d8 15 76 06 61 02 

Expected result:

        7a 01 cb d9 6c aa 6c 03 c3 06 68 be 7d 10 1b 0d 70 

Checking the received license key!

Wrong! No flag for you!
```

reverse the key, manually, nothing much to show here

the key is "6f77677a6e7279756f676a666866756c72", which is, conveniently, a printable string: "owgznryuogjfhfulr"