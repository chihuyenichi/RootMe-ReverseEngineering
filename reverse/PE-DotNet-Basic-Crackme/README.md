Challenge: [PE-DotNet-Basic-Crackme](https://www.root-me.org/en/Challenges/Cracking/PE-DotNet-Basic-Crackme)

First, use dnSpy to read the `Main` function and understand the logic:

<img width="915" height="272" alt="image" src="https://github.com/user-attachments/assets/15ef8696-d160-4c58-9cf1-d8f564adf6a1" />

<img width="1765" height="1260" alt="image" src="https://github.com/user-attachments/assets/bb0b2813-8d78-4b3c-ab3b-3349e148efff" />

It is a while loop. In each iteration:
- Read itself as byte data
- Split into two parts: encrypted text and a key. If it cannot split, break the loop
- Decrypt the encrypted text with the key and write the result to a `.exe` file
- If the new file runs, repeat the process

In the final extracted file, use dnSpy again to read it. The code illustrating the above instructions is in the same folder.

<img width="1614" height="302" alt="image" src="https://github.com/user-attachments/assets/8ddec7db-31af-45a6-8dd4-5aa7342ce3da" />

This is our answer. 
