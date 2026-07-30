File type:
```
macho: Mach-O 64-bit x86_64 executable, flags:<NOUNDEFS|DYLDLINK|TWOLEVEL|PIE>
```

Opening the binary in IDA, we examine the main function's pseudocode:

<img width="1231" height="845" alt="ảnh" src="https://github.com/user-attachments/assets/458074fe-3817-4d24-a6ab-485a4100dc42" />

The program takes user input and passes it to an `auth` function. If `auth` returns 0, the program enters an else block that decrypts and prints the flag.

```
__int64 __fastcall auth(__int64 a1, int a2, const char *a3)
{
  int v4;
  int v5;
  int i;
  unsigned int v8;

  *(_BYTE *)(a1 + strcspn((const char *)a1, "\n")) = 0;
  v5 = strnlen((const char *)a1, 0x20u);
  if (ptrace(0, 0, (caddr_t)1, 0) == -1)
  {
    v8 = 1;
    printf("%s\n", a3);
  }
  else
  {
    v4 = (*(char *)(a1 + 3) ^ 0x1337) + 6221293;
    for (i = 0; i < v5; ++i)
    {
      if (*(char *)(a1 + i) < 32)
        return 1;
      v4 += (v4 ^ (unsigned int)*(char *)(a1 + i)) % 0x539;
    }
    return a2 != v4 || a2 != 6235464;
  }
  return v8;
}
```

This function contains two parts: debug checking and a validation loop. For `auth` to return 0, parameter `a2` only needs to equal `6235464`.

<img width="732" height="426" alt="ảnh" src="https://github.com/user-attachments/assets/2d8311b5-35d6-41de-b8df-a751c53afe0d" />

The else block's output does not depend on our string input — we only need the key (`6235464`). Here is a Python script to illustrate the decryption:

```python
key = 0x5f2548
dat = list(".what r u trying 2 do?.")

for i in range(len(dat)):
    dat[i] = (key ^ ord(dat[i])) % 0x7f

dat[15] = 0x2d
dat[16] = 0x64
dat[18] = 0x62

print("".join([chr(x) for x in dat[10:]]))
print(dat[10:])
```

This is our output and answer:

<img width="805" height="150" alt="ảnh" src="https://github.com/user-attachments/assets/496c5a41-768d-4962-91e1-67e10c05191e" />
