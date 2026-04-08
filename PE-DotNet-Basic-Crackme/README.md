[Challenge link](https://www.root-me.org/en/Challenges/Cracking/PE-DotNet-Basic-Crackme)

first use dnSpy to read Main function to understand logic and we will see sth like that 

<img width="915" height="272" alt="image" src="https://github.com/user-attachments/assets/15ef8696-d160-4c58-9cf1-d8f564adf6a1" />

<img width="1765" height="1260" alt="image" src="https://github.com/user-attachments/assets/bb0b2813-8d78-4b3c-ab3b-3349e148efff" />

it means that is a while loop, in each loop 
    read itself as byte data
    split into 2 parts, one is encrypted text and another is key, if cannot split, break the loop 
    decode the encrypted text with key and it will make a file .exe with this decoded text 
    if this file can run, repeat our actions
then in the last file made, we use dnSpy again to read file 

<img width="1614" height="302" alt="image" src="https://github.com/user-attachments/assets/8ddec7db-31af-45a6-8dd4-5aa7342ce3da" />

and this is our answer 
