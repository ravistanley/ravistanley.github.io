---
title: Labyrinth
author: b33tl3
description: "Find your way through the labyrinth and recover the hidden treasure within."
date: 2025-06-05 00:00:00+0000
categories: [Reverse Engineering]
tags: [Rev, Cyberlympics]
---

### Introduction
![Challenge](laby.png)
We are provided with a 64-bit stripped ELF binary named _labyrinth_. Our goal is to reverse engineer it and extract a valid key (flag) that the binary will accept.

### Initial Recon
Using _file_: <br>
![Challenge](file.png)
  - Stripped: No debug symbols
  - PIE-enabled: Randomized load addresses
  - Dynamically linked: Uses standard libraries at runtime

Using _checksec_: <br>
![Challenge](checksec.png)

Runtime behavior: <br>
![Challenge](run.png)
We are prompted for a key. 

Using _ltrace_: <br>
![Challenge](ltrace.png)
This confirms that it takes input with 'scanf'.
It immediately checks the key and outputs a result - no system-level calls, so everything is done internally.

### Static Analysis using Ghidra
I loaded the binary into Ghidra and started exploring. I found some interesting functions. <br>
FUN_00101159:  <br> ![Challenge](159.png)
This is an XOR operation implemented bitwise. The function takes two 32-bit integers and applies a bit-by-bit XOR operation. Returns the 32-bit XOR result of param_1 and param_2.

FUN_001011dd: <br> ![Challenge](1dd.png)
This is the main key-checking routine. It:
  - Prompts the user for input.
  - Loops over 23 characters of the input. 'local_28 = &DAT_00102004;' points to 23 reference bytes stored in the binary. These represent the correct characters XORed with 0x65.
  - Compares each character after some obfuscation.
  - Aggregates errors using a bitwise OR (|).
  - If there is no error, the key  is correct. <br>

Reference bytes, DAT_00102004: <br>
![Challenge](ref.png)
Using this, we can write a decoding script.
![Challenge](script.png)

Then we get our flag: ![Challenge](flag.png)
Flag: _acdfCTF{x0r1ng_w1th_65}_
