![[Pasted image 20260509075838.png]]
![[Pasted image 20260509075851.png]]
![[Pasted image 20260509080438.png]]

Woah thats longer than usual
So the flow here is:
```
input -> reverse -> twisted_bubble_sort -> swap byte 2 and 3 -> 3x reverse -> twisted_bubble sort
```
twisted is the keyword here, as the range shrink everytime a new character is iterated
a regular bubble sort should be something like:
```
for(int i=0; i<len; i++)
for(int j=i; j<len-1; j++) if(a[j]>a[j+1]) swap(a[j+1],a[j]);
```
an acceptable key is "zxyxxwwvvutstqnnmkjjiiihgggffeeeeddc" (oh wow its the exact string from the image, crazy stuff)