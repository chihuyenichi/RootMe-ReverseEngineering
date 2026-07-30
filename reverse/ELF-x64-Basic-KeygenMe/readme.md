Use `objdump -d -M intel -S ctf-name > ctf-name.txt` to get the assembly code.

**Hint:**  
Find the serial for the "root-me.org" user. The validation password is the serial's SHA256 hash.

Reading the assembly:

```
   0x4001a5:    mov    eax,0x0
   0x4001aa:    mov    edi,0x0
   0x4001af:    movabs rsi,0x600260
   0x4001b9:    mov    edx,0x20
   0x4001be:    syscall
```

This reads 0x20 bytes from stdin into address `0x600260` (the login input).

```
   0x4001c0:    mov    eax,0x2
   0x4001c5:    movabs rdi,0x40012e
   0x4001cf:    mov    esi,0x0
   0x4001d4:    syscall
   0x4001d6:    cmp    rax,0xfffffffffffffffe
   0x4001da:    je     0x400215
   0x4001dc:    push   rax
   0x4001dd:    mov    rdi,rax
   0x4001e0:    mov    eax,0x0
   0x4001e5:    movabs rsi,0x600280
   0x4001ef:    mov    edx,0x20
```

This reads 0x20 bytes from the file at offset `0x40012e` (which is `.m.key`) into `0x600280`. This is the key we need.

The data from stdin is the login — according to the hint, it is `root-me.org`.

```
   0x400161:    cmp    rcx,rbx
   0x400164:    je     0x400182
   0x400166:    mov    dl,BYTE PTR [rdi+rcx*1]
   0x400169:    mov    dh,BYTE PTR [rsi+rcx*1]
   0x40016c:    mov    al,dl
   0x40016e:    sub    al,cl
   0x400170:    add    al,0x14
   0x400172:    cmp    al,dh
   0x400174:    jne    0x40017b
   0x400176:    inc    rcx
   0x400179:    jmp    0x400161
```

From this loop: `key[i] = login[i] - i + 0x14`

Then compute the SHA256 hash of the key to get the serial.