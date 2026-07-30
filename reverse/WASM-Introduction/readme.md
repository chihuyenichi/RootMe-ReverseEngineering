Challenge: [WASM-Introduction](https://www.root-me.org/en/Challenges/Cracking/WASM-Introduction)

First, open the browser's inspector, go to the Network tab, and download the `.wasm` file.

<img width="1486" height="264" alt="image" src="https://github.com/user-attachments/assets/67b55016-3ce1-49e6-8999-6593b02c605f" />

Use `wasm-decompile` to decompile it to C code:

```
wasm-decompile index-1.wasm -o index-1.txt
```

```
function check_password(a:int) {
  var b:int = stack_pointer;
  var c:int = 32;
  var d:int_ptr = b - c;
  stack_pointer = d;
  d[7] = a;
  var e:int = get_password();
  d[6] = e;
  var f:int = d[6];
  var g:int_ptr = d[7];
  g[1] = f;
  var h:int = d[6];
  var i:int = strlen(h);
  d[5] = i;
  var j:int = 3012;
  MD5_Init(j);
  var k:int = d[6];
  var l:int = d[5];
  var m:int = 3012;
  MD5_Update(m, k, l);
  var n:int = 3168;
  var o:int = 3012;
  MD5_Final(n, o);
  var p:int = d;
  var q:int = 2704;
  hex_to_bytes(q, p);
  var r:int = d;
  var s:int = 3168;
  var t:int = strcmp(s, r);
  if (t) goto B_b;
  var u:byte_ptr = d[7];
  var v:int = 1;
  u[0] = v;
  goto B_a;
  label B_b:
  var w:byte_ptr = d[7];
  var x:int = 0;
  w[0] = x;
  label B_a:
  var y:int = 32;
  var z:int = d + y;
  stack_pointer = z;
}
```

This function checks our input against data at offset 2704. Illustration:

```
k = h = e = d[6]
l = i = d[5] = strlen(h)
m = 3012
s = n = 3168
o = 3012
MD5_Update(m, k, l)  // data at m, input is k, length is l
MD5_Final(n, o)      // copy data from o to n
r = p = d
q = 2704
hex_to_bytes(q, p)   // r = hex at q
t = strcmp(s, r)
```

Data at offset 2704:

```
data data(offset: 2704) =
  "936fff76f378ace4cf83a4360cafcc20\00\00\00\00\00\00\00\00\00\00\00\00\00"
  "\00\00\005 1,(\067\09\06\14N3\0b\06\09\01M\00\00\00\00\00\00\00\00\00\00"
  ...
```

The input is the MD5 decryption of `936fff76f378ace4cf83a4360cafcc20` = `babaaurhum`. This is only the password. To get the flag, we need the `show_flag` function:

```
function show_flag(a:int) {
  ...
  loop L_c {
    if (eqz(p)) goto B_b;
    var q:ubyte_ptr = d[29];
    var r:int = q[2752];
    var t:int = r & 255;
    var z:int = d[31][d[29] % 10];
    var da:int = t ^ z;
    ea[2752] = da;
    d[29] += 1;
    continue L_c;
  }
  ...
}
```

This XORs data at offset 2752 with our decrypted password. Solution:

```python
res = [0] * 17
data_at_2752 = [0x35, 0x20, 0x31, 0x2c, 0x28, 0x37, 0x09, 0x06, 0x14, 0x4e, 0x33, 0x0b, 0x06, 0x09, 0x01, 0x4d, 0x00]
inp_data = "babaaurhum"
for i in range(17):
    res[i] = ord(inp_data[i % 10]) ^ data_at_2752[i]
print("".join(chr(x) for x in res))
```

**Flag: `WASMIsEasy,Right?`**