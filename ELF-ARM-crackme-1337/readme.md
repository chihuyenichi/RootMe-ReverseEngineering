check file type 
```
chall9.bin: ELF 32-bit LSB executable, ARM, EABI4 version 1 (SYSV), statically linked, for GNU/Linux 2.6.16, with debug_info, not stripped
``` 

so we use ghidra to decompile it to pseudocode c code<br>
then we will find the main function 
```

undefined4 main(int param_1,int param_2)

{
  int iVar1;
  undefined4 uVar2;
  char cVar3;
  undefined4 local_34;
  int local_20;
  
  if (param_1 < 2) {
    local_34 = 0xffffffff;
  }
  else {
    local_20 = 0;
    iVar1 = xmalloc(0x20);
    for (; local_20 != 8; local_20 = local_20 + 1) {
      uVar2 = xmalloc(0x20);
      *(undefined4 *)(iVar1 + local_20 * 4) = uVar2;
      memset(*(void **)(iVar1 + local_20 * 4),10,0x20);
    }
    *(undefined4 *)(iVar1 + 0x20) = 0;
    cVar3 = 'A';
    for (local_20 = 0; local_20 != 0x1f; local_20 = local_20 + 1) {
      *(char *)(*(int *)(iVar1 + 0xc) + local_20) = cVar3;
      cVar3 = cVar3 + '\x01';
    }
    *(undefined1 *)(*(int *)(iVar1 + 0xc) + 0x1f) = 0;
    for (local_20 = 0; *(char *)(*(int *)(param_2 + 4) + local_20) != '\0'; local_20 = local_20 + 1)
    {
      if (*(char *)(*(int *)(param_2 + 4) + local_20) != *(char *)(*(int *)(iVar1 + 0xc) + local_2 0)
         ) {
        return 0xffffffff;
      }
    }
    local_34 = 0x539;
  }
  return local_34;
}
```

it is simple to illustrating it by python code 
```
tmp = 'A'
res = []
for i in range(0x1f) : 
	res.append(tmp) 
	tmp = chr(ord(tmp) + 1)

print("".join(res))
```

