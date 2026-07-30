Use Ghidra to decompile the file to pseudocode C. We find the main function:

```
void main(int param_1, int param_2)
{
  size_t lenInput;
  byte *__s;
  int __status;
  int local_14;

  if (param_1 != 2) {
    puts("Please input password");
    exit(1);
  }
  __s = *(byte **)(param_2 + 4);
  printf("Checking %s for password...\n", __s);
  lenInput = strlen((char *)__s);
  if (lenInput != 6) {
    puts("Loser...");
    exit(lenInput);
  }
  lenInput = strlen((char *)__s);
  local_14 = -lenInput + 6;
  if (*__s != __s[5])
    local_14 = -lenInput + 7;
  if (*__s + 1 != (uint)__s[1])
    local_14 = local_14 + 1;
  if (__s[3] + 1 != (uint)*__s)
    local_14 = local_14 + 1;
  if (__s[2] + 4 != (uint)__s[5])
    local_14 = local_14 + 1;
  if (__s[4] + 2 != (uint)__s[2])
    local_14 = local_14 + 1;
  __status = local_14 + (__s[3] ^ 0x72) + (uint)__s[6];
  if (__status == 0) {
    puts("Success, you rocks!");
    exit(0);
  }
  puts("Loser...");
  exit(__status);
}
```

Key observations:
- `lenInput = 6`
- `s = *(byte **)(param_2 + 4)`
- `local_14 + (s[3] ^ 0x72) + s[6] = 0` and `s[6] = 0`
- Therefore `s[3] ^ 0x72 = -local_14 = 0` → `s[3] = 0x72 = 'r'`
- `s[0] = s[5]`, `s[0] + 1 = s[1]`, `s[3] + 1 = s[0]`, `s[2] + 4 = s[5]`, `s[4] + 2 = s[2]`

Solving these constraints gives: **s = "storms"** — this is the flag. 
