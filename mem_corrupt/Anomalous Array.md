![](img/Anomalous%20Array%20(inproc)-1780578641376.webp)
at this level we can view the stack by providing a number
assuming the index is signed, we can try to input -214 (as the array start is 214 bytes away from the flag)
![](img/Anomalous%20Array%20(inproc)-1780578721090.webp)
our result on local machine is ALF_EKAF, which is FAKE_FLA(G) in reverse. Though it seems that the data is limited to 8 bytes, which will require us to perform multiple action to obtain the flag