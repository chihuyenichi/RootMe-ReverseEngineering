```
Crack: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.8, stripped
```

Use IDA to read the decompiled code. The main function:

```
int __cdecl main(int a1, char **a2)
{
  if (ptrace(PTRACE_TRACEME, 0, 1, 0) < 0)
  {
    puts("Don't use a debuguer !");
    abort();
  }
  if (a1 != 2)
  {
    puts("You must give a password for use this program !");
    abort();
  }
  printRightPass(a2[1]);
  return 0;
}
```

When we pass a string as a parameter, it calls `printRightPass`.

<img width="641" height="647" alt="image" src="https://github.com/user-attachments/assets/e329e6c0-91c8-4f26-8b06-d1f19904d46b" />

The important part is the `modify_xor` function — it transforms the `s1` string. Meanwhile, `s2` (our input) is compared to `s1`. We can simulate `modify_xor` in Python to recover the expected value of `s1`:

```python
def modify_xor(p):
    p = [ord(c) for c in p]

    p[0] ^= 0xab

    v4 = 1
    v5 = p[1]

    if v5:
        v6 = 1
        v7 = 1
        while True:
            v8 = (v5 - v6) & 0xFF
            p[v4] = v8

            v9 = v7 - 1
            v10 = (p[v9] ^ v8) & 0xFF
            p[v4] = v10

            v11 = (p[v9] + v10) & 0xFF
            p[v4] = v11

            v12 = (p[v9] ^ v11) & 0xFF
            p[v4] = v12

            v13 = (p[8] ^ v12) & 0xFF
            p[v4] = v13

            if not v13:
                p[v4] = 1

            v7 = v6 + 1
            v6 += 1
            v4 = v6
            v5 = p[v6]

            if not v5:
                break

    s = ""
    i = 0
    result = p[0]
    while result & 0xFF:
        s += f"{result:02x}"
        i += 1
        result = p[i]

    return s


needed_input = modify_xor("THEPASSWORDISEASYTOCRACK\0")
print(needed_input)

with open("payload", "wb") as file:
    file.write(needed_input.encode())
```

Since our input must equal `s1`, the result is the flag:

```
./Crack $(cat payload)
Good work, the password is :

ff07031d6fb052490149f44b1d5e94f1592b6bac93c06ca9
```


