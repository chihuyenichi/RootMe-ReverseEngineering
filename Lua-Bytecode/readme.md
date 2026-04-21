```
(base) root@chihuyenich:/mnt/c/daohuyenchi_server/CTF_downloads/rootme/reverse/Lua-Bytecode# file ch45.out 
ch45.out: Lua bytecode, version 5.1
```

we will use unluac to decompile this byte code to a pseudocode 

`` java -jar unluac.jar ch45.out > ch45-decompile.txt ``

we will get something like that 

<img width="1894" height="1049" alt="image" src="https://github.com/user-attachments/assets/44673da0-afc9-4bbd-a699-7896587c33d3" />

read it and write a python code that illustrating it<br>
from this we can write solution code 

```
shift_array = [None, 33, 1, 84, -104, -65, 46, -28, -49, 15, 110, -18, 40, -59, -25, 0, 22, -46, -88, -33, -13, 88, -50, 129]
end_array   = [None, 99, 96, 192, 201, 45, 53, 73, 144, 99, 1, 92, 55, 22, 142, 111, 89, 33, 199, 78, 92, 167, 155, 162]

password = []

def getInput() : 
	for i in range(1, len(shift_array)) : 
		x = chr(end_array[i] + shift_array[i] * (-1 if (i % 2 == 1) else +1))
		password.append(x)

getInput()

print("".join(password))
```


