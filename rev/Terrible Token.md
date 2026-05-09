```
hacker@reverse-engineering~terrible-token-easy:~$ cd /challenge
hacker@reverse-engineering~terrible-token-easy:/challenge$ ./*
###
### Welcome to ./terrible-token-easy!
###

This license verifier software will allow you to read the flag. However, before you can do so, you must verify that you
are licensed to read flag files! This program consumes a license key over stdin. Each program may perform entirely
different operations on that input! You must figure out (by reverse engineering this program) what that license key is.
Providing the correct license key will net you the flag!

Ready to receive your license key!

abcde
Initial input:

        61 62 63 64 65 

The mangling is done! The resulting bytes will be used for the final comparison.

Final result of mangling input:

        61 62 63 64 65 

Expected result:

        70 75 6f 62 71 

Checking the received license key!

Wrong! No flag for you!
```

it seems that the program just turn your input into hex and check it with the true key

translate it back to string, we get "puobq"