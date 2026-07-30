use ida to decompile the .exe file 

then i see something in assembly code that is important 
```
 8048401:	c7 45 f4 88 28 0c 08 	mov    DWORD PTR [ebp-0xc],0x80c2888
.rodata:080C2888 aKsuiealohgy    db 'ksuiealohgy',0      ; DATA XREF: main+11↑o


dl = [ebp + (-0x16)] 
eax = ebp + (-0xc)
eax += 4 
al = [eax] 


cmp dl, al 


dl = [ebp + (-0x15)] 
eax = ebp + (-0xc)
eax += 5 
al = [eax] 


cmp dl, al 



DL = [ebp + (-0x14)] 
eax = ebp + (-0xc)
eax += 1 
al = [eax] 


cmp dl, al 


dl = [ebp + (-0x13)] 
eax = ebp + (-0xc)]
eax += 0xa 
al = [eax] 
cmp dl, al 

```

from [ebp - 0x16] is our input, and it is compared to string in .rodata

if go through all cmp command, we will get "Good password"
