use ida and ghidra to read the assembly code, i will give summary of the function (in assembly) 
```
mov eax, offset start = 08048080 
mov ebx, 8048123h
sub_8048115 
	sub ebx, eax 
	xor ecx, ecx 
	
loc_8048119 
add cl, [eax] 
rol ecx, 3 
inc eax 
dec ebx 
jnz loc_8048119 


mov edx, ecx = 0xac77e166
mov ecx, 0x19 

loc_80480C1: 
mov eax, offset byte_8049155 
mov ebx, offset myInput 
ror edx, 0x1 
mov al, [eax + ecx - 1] 
mov bl, [ebx + ecx - 1] 
xor al, bl 
xor al, dl 
jnz     short loc_80480F6 (if not zero -> false) 

dec ecx 
jnz loc_80480C1

... 
```

first, we need to calculate value of ecx<br> 
then we need to illustrate the below function by python to find the input that we need to put 

```
import struct

def find_ecx_value():
    file_path = 'ch20.bin'
    
    try:
        with open(file_path, 'rb') as f:
            data = f.read()
    except FileNotFoundError:
        print(f"Error: {file_path} not found.")
        return

    # Configuration based on your assembly snippet
    start_addr = 0x08048080
    end_addr = 0x8048123
    base_addr = 0x08048000  # Standard ELF base address
    
    # Calculate file offset and number of iterations
    start_offset = start_addr - base_addr
    num_iterations = end_addr - start_addr
    
    ecx = 0x00000000
    
    # Check if the file is large enough
    if len(data) < start_offset + num_iterations:
        print("Warning: File is smaller than the calculated address range.")
        num_iterations = len(data) - start_offset

    # Run the loop logic
    print(num_iterations)
    for i in range(num_iterations):
        # Get the byte at current EAX
        byte_val = data[start_offset + i]
        
        # add cl, [eax]
        # We only modify the lowest 8 bits (cl)
        cl = (ecx & 0xFF)
        cl = (cl + byte_val) & 0xFF
        ecx = (ecx & 0xFFFFFF00) | cl
        
        # rol ecx, 3
        # 32-bit Rotate Left
        ecx = ((ecx << 3) & 0xFFFFFFFF) | (ecx >> (32 - 3))
        ecx &= 0xFFFFFFFF

    print(f"Final ECX value: {hex(ecx)}")


def solve() : 
    file_path = 'ch20.bin'
    
    try:
        with open(file_path, 'rb') as f:
            data = f.read()
    except FileNotFoundError:
        print(f"Error: {file_path} not found.")
        return

    ebx = 0x8049155
    # Combined data from 08049155 to 0804917D
    ebx = [
    0x1E, 0xCD, 0x2A, 0xD5, 0x34, 0x87, 0xFC, 0x78, 0x64, 0x35, # 08049155
    0x9D, 0xEC, 0xDE, 0x15, 0xAC, 0x97, 0x99, 0xAF, 0x96, 0xDA, # 0804915F
    0x79, 0x26, 0x4F, 0x32, 0xE0,                               # 08049169
    0x20, 0x20, 0x20, 0x20, 0x20, 0x20, 0x20, 0x20, 0x20, 0x20, # 19h dup(20h) starts
    0x20, 0x20, 0x20, 0x20, 0x20, 0x20, 0x20, 0x20, 0x20, 0x20, 
    0x20, 0x20, 0x20, 0x20, 0x20,                               # 19h dup(20h) ends
    0x0A                                                        # Final byte
]
    base_addr = 0x8048000  # Standard ELF base address

    edx = 0xac77e166 
    ecx = 0x19 

    res = []

    while (ecx > 0) : 
        tmp = edx & 1
        edx = (edx >> 1) | (tmp << (32 - 1))
        edx &= 0xFFFFFFFF

        bl = ebx[ecx - 1]
        dl = (edx & 0xFF)

        res.append(bl ^ dl)

        ecx -= 1    

    print("".join([chr(x) for x in res][::-1]))

if __name__ == "__main__":
    find_ecx_value()
    solve()
```



