```
int __fastcall main(int argc, const char **argv, const char **envp)
{
  char v4[104]; // [rsp+10h] [rbp-70h] BYREF
  unsigned __int64 v5; // [rsp+78h] [rbp-8h]

  v5 = __readfsqword(0x28u);
  init(argc, argv, envp);
  printf("Key: ");
  __isoc99_scanf("%s", v4);
  check(v4);
  return 0;
}

int __fastcall check(const char *a1)
{
  int i; // [rsp+1Ch] [rbp-14h]

  for ( i = 0; i < strlen(a1); ++i )
    a1[i] ^= key[i % 8];
  if ( *(_QWORD *)a1 == 0xA377AD570465FDF9LL )
    return puts("C'est correct !");
  else
    return puts("Essaie encore !");
}

```

the .exe will get our input and xor it with key, if the result is same to the target, it will return true and means that our input is right <br>

```
.data:0000000000004048 key             db 0A8h, 96h, 4Fh, 7Fh, 3Eh, 94h, 0Ah, 95h
```

so if we do it normally, we will get "Qk*{i9}6"<br>
but it's wrong

```
signed __int64 __fastcall _do_global_ctors_aux()
{
  unsigned __int64 v0; // r10
  signed __int64 result; // rax
  int i; // [rsp+0h] [rbp-10h]
  _QWORD *v3; // [rsp+8h] [rbp-8h]

  result = sys_ptrace(0, 0, 0, v0);
  result = (unsigned int)result;
  if ( (_DWORD)result != -1 )
  {
    result = (signed __int64)&data_start;
    v3 = &data_start;
    for ( i = 0; i <= 255; ++i )
    {
      result = 0x950A943E7F4F96A8LL;
      if ( *v3 == 0x950A943E7F4F96A8LL )
      {
        *v3 ^= 0x119011901190119uLL;
        return (signed __int64)v3;
      }
      ++v3;
    }
  }
  return result;
}
```

there're a function like that

<img width="994" height="592" alt="image" src="https://github.com/user-attachments/assets/a0d3d90f-edf2-4943-83a0-f271dc3869c3" />

so it will check whether we using gdb or not<br>
if not, it will xor the **key** with **0x119011901190119uLL**<br> 
so the real key we using in xor is [b1 97 56 7e 27 95 13 94]
