---
title: "KeyGen Challenge"
date: 2026-07-09
categories: [ctf]
tags: [ctf]

image:
    path: /assets/img/posts/keygen/reverse.webp
    alt: Pico Pi
---

#### Lab Objectives
- No patching
- Create a KeyGen
- Upload a solution + tutorial

---

### Starting

Run the program:

![image1](/assets/img/posts/keygen/1.png)

### Step 2: Open x64gdb for Analysis

![image2](/assets/img/posts/keygen/2.png)

1. Right Click → Search For → All Modules → String References
2. Go to the `name` string
3. Observe the breakpoint:

![image3](/assets/img/posts/keygen/3.png)

- The instruction `cmp eax, dword ptr ss:[ebp-30]` is present.
- Examine the `eax` register while the program runs.
- ![image4](/assets/img/posts/keygen/4.png)
- This reveals the program's behavior.
- View the `eax` register:
- ![image5](/assets/img/posts/keygen/5.png)
- The input series is stored in the `EAX` register.

4. To understand the program, examine the instruction `dword ptr ss:[ebp-30]`:
   ![image6](/assets/img/posts/keygen/6.png)
   - `eax` is stored at `dword ptr ss:[ebp-30]`.
   - Set a breakpoint at this memory address, run the program, and inspect the `EAX` register.
   - The serial key is revealed:
   ![image7](/assets/img/posts/keygen/7.png)
   - This identifies the target key. To create a key generator, the logic must be derived from the instructions.

5. At memory address `00401442`, a function call is present. Open Ghidra to decompile this function and understand the key generation logic:
   ![image8](/assets/img/posts/keygen/8.png)

### Step 6: Open Ghidra and Locate the CALL Instruction

- ![image9](/assets/img/posts/keygen/9.png)

- The key generation logic is now understood:
  - `len` stores the length of the user input.
  - `local_34` takes the input length and computes:
    - `len(user_input) + 0xcaU ^ 0x3d8d40f`

### Step 7: Create a Python KeyGen

```python
user_input = input("enter a name : ")
GenKey = len(user_input) + 0xca ^ 0x3d8d40f
print(f"KeyGen is : {GenKey}")
```

### Step 8: Finished

Return to the write-ups for more challenges.
