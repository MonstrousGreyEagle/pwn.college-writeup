```
hacker@reverse-engineering~tangled-ticket-easy:~$ cd /challenge
hacker@reverse-engineering~tangled-ticket-easy:/challenge$ ./*
###
### Welcome to ./tangled-ticket-easy!
###

This license verifier software will allow you to read the flag. However, before you can do so, you must verify that you
are licensed to read flag files! This program consumes a license key over stdin. Each program may perform entirely
different operations on that input! You must figure out (by reverse engineering this program) what that license key is.
Providing the correct license key will net you the flag!

Ready to receive your license key!

abcde             
Initial input:

        61 62 63 64 65 

This challenge is now mangling your input using the `swap` mangler for indexes `0` and `2`.

This mangled your input, resulting in:

        63 62 61 64 65 

The mangling is done! The resulting bytes will be used for the final comparison.

Final result of mangling input:

        63 62 61 64 65 

Expected result:

        74 64 63 78 67 

Checking the received license key!

Wrong! No flag for you!
```

the binary turn our input into hex and swap byte 1 and 2

easy enough, our key here is "cdtxg"