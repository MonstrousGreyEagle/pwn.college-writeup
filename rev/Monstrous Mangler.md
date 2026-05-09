```
hacker@reverse-engineering~monstrous-mangler-easy:~$ cd /challenge
hacker@reverse-engineering~monstrous-mangler-easy:/challenge$ ./*
###
### Welcome to ./monstrous-mangler-easy!
###

This license verifier software will allow you to read the flag. However, before you can do so, you must verify that you
are licensed to read flag files! This program consumes a license key over stdin. Each program may perform entirely
different operations on that input! You must figure out (by reverse engineering this program) what that license key is.
Providing the correct license key will net you the flag!

Ready to receive your license key!

12345678abcdefgh
Initial input:

        31 32 33 34 35 36 37 38 61 62 63 64 65 66 67 68 0a 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 

This challenge is now mangling your input using the `swap` mangler for indexes `12` and `32`.

This mangled your input, resulting in:

        31 32 33 34 35 36 37 38 61 62 63 64 00 66 67 68 0a 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 65 00 00 00 

This challenge is now mangling your input using the `reverse` mangler.

This mangled your input, resulting in:

        00 00 00 65 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0a 68 67 66 00 64 63 62 61 38 37 36 35 34 33 32 31 

This challenge is now mangling your input using the `swap` mangler for indexes `17` and `23`.

This mangled your input, resulting in:

        00 00 00 65 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0a 68 67 66 00 64 63 62 61 38 37 36 35 34 33 32 31 

This challenge is now mangling your input using the `xor` mangler with key `0xf8f6359f`

This mangled your input, resulting in:

        f8 f6 35 fa f8 f6 35 9f f8 f6 35 9f f8 f6 35 9f f8 f6 35 95 90 91 53 9f 9c 95 57 fe c0 c1 03 aa cc c5 07 ae 

This challenge is now mangling your input using the `reverse` mangler.

This mangled your input, resulting in:

        ae 07 c5 cc aa 03 c1 c0 fe 57 95 9c 9f 53 91 90 95 35 f6 f8 9f 35 f6 f8 9f 35 f6 f8 9f 35 f6 f8 fa 35 f6 f8 

This challenge is now mangling your input using the `sort` mangler.

This mangled your input, resulting in:

        03 07 35 35 35 35 35 53 57 90 91 95 95 9c 9f 9f 9f 9f aa ae c0 c1 c5 cc f6 f6 f6 f6 f6 f8 f8 f8 f8 f8 fa fe 

This challenge is now mangling your input using the `swap` mangler for indexes `3` and `17`.

This mangled your input, resulting in:

        03 07 35 9f 35 35 35 53 57 90 91 95 95 9c 9f 9f 9f 35 aa ae c0 c1 c5 cc f6 f6 f6 f6 f6 f8 f8 f8 f8 f8 fa fe 

The mangling is done! The resulting bytes will be used for the final comparison.

Final result of mangling input:

        03 07 35 9f 35 35 35 53 57 90 91 95 95 9c 9f 9f 9f 35 aa ae c0 c1 c5 cc f6 f6 f6 f6 f6 f8 f8 f8 f8 f8 fa fe 

Expected result:

        40 41 4c 8e 4f 59 59 5e 5f 81 82 86 8b 8b 8b 8c 8c 4c 8f 8f 92 93 95 97 98 99 9e e7 e9 ed ef f0 f4 f8 f9 fa 

Checking the received license key!

Wrong! No flag for you!
```

okay so um,  just reverse it
one valid key (because of the sort mangler this challenge has many valid key) is "df74bab4d06cafa6c0b4747e6bbe7d7413bb14770da6636f07ac681f76d8190879cd0f02"
