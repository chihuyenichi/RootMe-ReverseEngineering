undecompyle6 is tool that can decompile .pyc to python source code 

```
(base) root@chihuyenich:/mnt/c/daohuyenchi_server/CTF_downloads/rootme/reverse/PYC-ByteCode# uncompyle6 ch19.pyc > 1.py
(base) root@chihuyenich:/mnt/c/daohuyenchi_server/CTF_downloads/rootme/reverse/PYC-ByteCode# ls
1.py  ch19.pyc  exploit.py
```

read the 1.py 
```
# uncompyle6 version 3.9.3
# Python bytecode version base 3.1 (3151)
# Decompiled from: Python 3.12.3 (main, Mar  3 2026, 12:15:18) [GCC 13.3.0]
# Embedded file name: crackme.py
# Compiled at: 2013-07-02 22:00:05
if __name__ == "__main__":
    print("Welcome to the RootMe python crackme")
    PASS = input("Enter the Flag: ")
    KEY = "I know, you love decrypting Byte Code !"
    I = 5
    SOLUCE = [57, 73, 79, 16, 18, 26, 74, 50, 13, 38, 13, 79, 86, 86, 87]
    KEYOUT = []
    for X in PASS:
        KEYOUT.append((ord(X) + I ^ ord(KEY[I])) % 255)
        I = (I + 1) % len(KEY)

    if SOLUCE == KEYOUT:
        print("You Win")
    else:
        print("Try Again !")

# okay decompiling ch19.pyc
```

if we want "You Win", we need find the suitable PASS

and this is the solution code for finding flag 

<img width="967" height="631" alt="image" src="https://github.com/user-attachments/assets/67cb3768-046d-4e64-a783-37b855b86418" />
