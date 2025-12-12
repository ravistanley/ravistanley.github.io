---
title: Gamer
author: b33tl3
description: Are you ready to play some game?
date: "2025-05-31"
categories: [Reverse Engineering]
tags: [Rev, Cyberlympics]
---

### Challenge Description
![Challenge](gamer.png) <br>
"Are you ready to play some game?" <br>
We are given an ELF binary named "gamer". When executed, it asks for a password. Our task is to reverse engineer the binary and find the correct input that will trigger the success message - and hopefully the flag.<br>

### Binary Inspection
Before diving into disassembly, we should inspect the binary to understand it's structure. I did this using cli commands: file and checksec. 
Using _file_, we get: ![Challenge](file.png) <br>
This tells us that: <br>
    - The binary is a 64-bit Linux executable. <br>
    - It uses Position Independent Execution (PIE) - so code addresses are randomized at runtime. <br>
    - Stripped binary - no symbols (function names) to help us, making reversing slightly harder. <br>

Using _checksec_, we get: <br> ![Challenge](checksec.png) <br>
This tells us: <br>
    - No stack canary: Buffer overflows might be exploitable. <br>
    - NX enabled: Prevents shellcode execution from the stack. <br>
    - PIE: Makes static addresses useless - needs runtime analysis or relative offsets. <br>

### Running the binary
We need to change the binary to an executable using 'chmod +x'. Executing the binary: <br>![Challenge](execute.png)  <br>
It requires a password argument. I tried a dummy input, 'test', and it printed a taunt. Let's figure out what is happening on under the hood.

### String Analysis
I ran strings to get the readable content from the binary: strings gamer
I found some interesting stuff. "Welcome to the fam mate!!!" - Likely the success message.
Several failure messages like "No, you are not getting it.", "Oops! That wasn't it.", "Even my gramps could solve this."
There is a strange string: "~|{y\KYd@wrrr@m,i,ml,@kw,@s,q@b". 

### Using ltrace
To observe runtime behavior without disassembly, I used 'ltrace': <br> ![Challenge](ltrace.png) <br>
We observe: <br>
    - The program seeds 'rand()' with the current time (for randomness). <br>
    - It then calls 'strlen()' on the input. <br>
    - A call to 'rand()' is used, probably to pick a random failure message. <br>
    - Then the program prints a string. <br>
The key logic must be hiding in a function that processes the password input.

### Static Analysis (via Ghidra)
I found some interesting functions via ghidra.
#### Functions Overview
Here is a piece of the first function: <br> ![Challenge](main.png) <br>
To interpret the function, I made a few changes to the variable names. The function seeds randomness for error message selection, calls another function, 'FUN_0010127f'. 
If the function returns '0' (false), you get a random failure message. If it returns 'non-zero' (true), you get the winning message.
The other interesting function is FUN_0010127f, which is called by the first function.
![Challenge](func.png) <br>
I noticed a hardcorded byte array being compared with transformed input characters. The function performs XOR-based validation. For it to return true, each character must satisfy: input[i] = hardcoded[i] ^ len
The hardcorded array (exactly what we saw earlier) is: ~|{y\KYd@wrrr@m,i,ml,@kw,@s,q@b
We know that the password length must be equal to the length of the array, which is 32.

Now to reconstruct the password, we need to write a script to reverse the XOR logic. <br>
![Challenge](code.png) <br>
Running the script, I get the flag. <br>
![Challenge](result.png) <br>
The output - <br>
Recovered password: acdfCTF{_hmmm_r3v3rs3_th3_l3n_}
