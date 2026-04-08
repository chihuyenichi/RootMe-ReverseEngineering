# [challenge link](https://www.root-me.org/en/Challenges/Cracking/WASM-Introduction)

first go to the inspect, goto network tab and download file .wasm 

<img width="1486" height="264" alt="image" src="https://github.com/user-attachments/assets/67b55016-3ce1-49e6-8999-6593b02c605f" />

use wasm-decompile to decompile to c code and analyze this c code 
```
(base) root@chihuyenich:/mnt/c/daohuyenchi_server/CTF_downloads/rootme/reverse/WASM-Introduction# wasm-decompile index-1.wasm -o index-1.txt
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
- this function is get our input and check it with data at 2704
- i will illustrate above code 
```
k = h = e = d[6]
l = i = d[5] = strlen(h)
m = 3012
s = n = 3168
o = 3012 
MD5_Updata(m, k, l) // data store at m, input is k, length is l
MD5_Final(n, o) // copy data from o to n
r = p = d
q = 2704
hex_to_bytes(q, p) // r = hex at q
t = strcmp(s, r) 
```
this is data at 2704 
```
data data(offset: 2704) =
  "936fff76f378ace4cf83a4360cafcc20\00\00\00\00\00\00\00\00\00\00\00\00\00"
  "\00\00\005 1,(\067\09\06\14N3\0b\06\09\01M\00\00\00\00\00\00\00\00\00\00"
  "\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00"
  "\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00"
  "\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00"
  "\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00"
  "\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00"
  "\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00"
  "\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\b0\0c\00"
  "\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00"
  "\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00\00"
  "\00\00\00\00\00\c0\0eP\00";
```
-> so we have input that is equal to md5_decoded of "0x936fff76f378ace4cf83a4360cafcc20" = babaaurhum
and it is only the input we should fill in, to have flag we need to goto show_flag function
```
function show_flag(a:int) {
  var b:int = stack_pointer;
  var c:int = 128;
  var d:int_ptr = b - c;
  stack_pointer = d;
  d[31] = a;
  var e:int = d[31];
  var f:int = strlen(e);
  d[30] = f;
  var g:int_ptr = 0;
  var h:int = g[752];
  if (h) goto B_a;
  var i:int = 0;
  d[29] = i;
  loop L_c {
    var j:int = d[29];
    var k:int = 17;
    var l:int = j;
    var m:int = k;
    var n:int = l < m;
    var o:int = 1;
    var p:int = n & o;
    if (eqz(p)) goto B_b;
    var q:ubyte_ptr = d[29];
    var r:int = q[2752];
    var s:int = 255;
    var t:int = r & s;
    var u:int = d[31];
    var v:int = d[29];
    var w:int = 10;
    var x:int = v % w;
    var y:ubyte_ptr = u + x;
    var z:int = y[0];
    var aa:int = 24;
    var ba:int = z << aa;
    var ca:int = ba >> aa;
    var da:int = t ^ ca;
    var ea:byte_ptr = d[29];
    ea[2752] = da;
    var fa:int = d[29];
    var ga:int = 1;
    var ha:int = fa + ga;
    d[29] = ha;
    continue L_c;
  }
  unreachable;
  label B_b:
  label B_a:
  var ia:int = 1;
  var ja:int_ptr = 0;
  ja[752] = ia;
  var ka:int = 16;
  var la:int = d + ka;
  var ma:int = la;
  var na:int = 2752;
  d[0] = na;
  var oa:int = 1354;
  var pa:int = 100;
  snprintf(ma, pa, oa, d);
  var qa:int = 16;
  var ra:int = d + qa;
  var sa:int = ra;
  send_message(sa);
  var ta:int = d[31];
  dlfree(ta);
  var ua:int = 128;
  var va:int = d + ua;
  stack_pointer = va;
}
```
this is illustration of above code 
```
a = e = d[31] // i think it is the decrypted text we get
f = d[30] = strlen(e)
g = 0
i = d[29] = 0 // pointer to iterative
loop L_c {
    l = j = d[29]
    k = 17
    n = l < m
    o = 1
    p = n & o
    if (p == 0) break;
    q = d[29]
    r = q[2752]
    s = 255
    t = r & s
    u = d[31] = e
    v = d[29] 
    w = 10
    x = v % 10
    y = u + x
    z = y[0]
    da = t ^ z // t = q[2752][i], z = d[31][v % 10]
    ea = d[29]
    ea[2752] = da
    d[29] += 1
    continue L_c;
}
```
it is xor of data at 2752 and our decrypted data 
