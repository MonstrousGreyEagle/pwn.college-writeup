first thing first, launch the program

```
hacker@program-security~login-leakage-easy:/challenge$ ./* The challenge() function has just been launched! Before we do anything, let's take a look at challenge()'s stack frame: 
[the display is corrupted, ill save your eye]
Our stack pointer points to 0x7fffd7706f40, and our base pointer points to 0x7fffd7707560. This means that we have (decimal) 198 8-byte words in our stack frame, including the saved base pointer and the saved return address, for a total of 1584 bytes. The input buffer begins at 0x7fffd7706f70, partway through the stack frame, ("above" it in the stack are other local variables used by the function). Your input will be read into this buffer. The buffer is 58 bytes long, but the program will let you provide an arbitrarily large input length, and thus overflow the buffer. This challenge is a password checker that will check your input against a randomly generated password. The password is stored at 0x7fffd7707545, 1493 bytes after the start of your input buffer. We have disabled the following standard memory corruption mitigations for this challenge: - the canary is disabled, otherwise you would corrupt it before overwriting the return address, and the program would abort. Payload size:
```

inspecting the data at address 0x7fffd7707545
![[Login Leakage-1778680709898.webp]]