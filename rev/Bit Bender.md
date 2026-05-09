![img/Pasted-image 20260508220405.png](img/Pasted-image 20260508220405.png)

Now we have a somewhat complicated transformation
The right key is literally showed in strcpy
Reverse it and we have a hex array of our intended input: 

```
7e5ea4618102dd215da03e9f9f03a3bf
```

problem: some of these bytes doesnt translate into readable string
solution: print them as bytes with the help of python

```
import sys
payload=bytes.fromhex("7e5ea4618102dd215da03e9f9f03a3bf")
sys.stdout.buffer.write(payload)
```

then run it with

```
python3 /[codeadress] | ./*
```
