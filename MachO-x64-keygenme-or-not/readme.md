```
(base) root@chihuyenich:/mnt/c/daohuyenchi_server/CTF_downloads/rootme/reverse/MachO-x64-keygenme-or-not/ch26-unzip-folder# file macho
macho: Mach-O 64-bit x86_64 executable, flags:<NOUNDEFS|DYLDLINK|TWOLEVEL|PIE>
```
first, use ida to read the pseudocode of this file<br>

<img width="1231" height="845" alt="ảnh" src="https://github.com/user-attachments/assets/458074fe-3817-4d24-a6ab-485a4100dc42" />

we see that it have a check function is "auth", so we should analyze it 

```
__int64 __fastcall auth(__int64 a1, int a2, const char *a3)
{
  int v4; // [rsp+14h] [rbp-2Ch]
  int v5; // [rsp+18h] [rbp-28h]
  int i; // [rsp+1Ch] [rbp-24h]
  unsigned int v8; // [rsp+3Ch] [rbp-4h]

  *(_BYTE *)(a1 + strcspn((const char *)a1, "\n")) = 0;
  v5 = strnlen((const char *)a1, 0x20u);
  if ( ptrace(0, 0, (caddr_t)1, 0) == -1 )
  {
    v8 = 1;
    printf("%s\n", a3);
  }
  else
  {
    v4 = (*(char *)(a1 + 3) ^ 0x1337) + 6221293;
    for ( i = 0; i < v5; ++i )
    {
      if ( *(char *)(a1 + i) < 32 )
        return 1;
      v4 += (v4 ^ (unsigned int)*(char *)(a1 + i)) % 0x539;
    }
    return a2 != v4 || a2 != 6235464;
  }
  return v8;
}
```
this function will check we use anything to debug<br>then it will check parameter a2 with 6235464, i call this is key<br> 
back to the main function, goto the else block when auth function return 0 

<img width="732" height="426" alt="ảnh" src="https://github.com/user-attachments/assets/2d8311b5-35d6-41de-b8df-a751c53afe0d" />

we saw this code (in else block) isn't dependent with our string input, we only the key (6235464)<br> 
from this i will write a python to illustration this part

```
key = 0x5f2548 
dat = list(".what r u trying 2 do?.")

for i in range(len(dat)) : 
	dat[i] = (key ^ ord(dat[i])) % 0x7f 

dat[15] = 0x2d 
dat[16] = 0x64 
dat[18] = 0x62 

print("".join([chr(x) for x in dat[10: ]]))
print(dat[10: ])
```

this is our output and answer<br>
<img width="805" height="150" alt="ảnh" src="https://github.com/user-attachments/assets/496c5a41-768d-4962-91e1-67e10c05191e" />
