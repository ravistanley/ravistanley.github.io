---
title: Labyrinth
description: "Find your way through the labyrinth and recover the hidden treasure buried deep within."
slug: rev
date: 2025-06-05 00:00:00+0000
image: /assets/img/posts/logo.png
categories:
    - Reverse Engineering
tags:
    - Rev
    - Cyberlympics
points:
    - 250
flag format:
    - acdfctf{}
---

### Introduction
![Challenge](/assets/img/posts/labyrinth/laby.png) <br>
We are provided with a 64-bit stripped ELF binary named _labyrinth_. Our goal is to reverse engineer it and extract a valid key (flag) that the binary will accept.

### Initial Recon
Using _file_: <br>
![Challenge](/assets/img/posts/labyrinth/file.png) <br>
  - Stripped: No debug symbols
  - PIE-enabled: Randomized load addresses
  - Dynamically linked: Uses standard libraries at runtime

Using _checksec_: <br>
![Challenge](/assets/img/posts/labyrinth/checksec.png) <br>

Runtime behavior: <br>
![Challenge](/assets/img/posts/labyrinth/run.png) <br>
We are prompted for a key. 

Using _ltrace_: <br>
![Challenge](/assets/img/posts/labyrinth/ltrace.png) <br>
This confirms that it takes input with 'scanf'.
It immediately checks the key and outputs a result - no system-level calls, so everything is done internally.

### Static Analysis using Ghidra
I loaded the binary into Ghidra and started exploring. I found some interesting functions. <br>
FUN_00101159:  <br> ![Challenge](/assets/img/posts/labyrinth/159.png) <br>
This is an XOR operation implemented bitwise. The function takes two 32-bit integers and applies a bit-by-bit XOR operation. Returns the 32-bit XOR result of param_1 and param_2.

FUN_001011dd: <br> ![Challenge](/assets/img/posts/labyrinth/1dd.png) <br>
This is the main key-checking routine. It:
  - Prompts the user for input.
  - Loops over 23 characters of the input. 'local_28 = &DAT_00102004;' points to 23 reference bytes stored in the binary. These represent the correct characters XORed with 0x65.
  - Compares each character after some obfuscation.
  - Aggregates errors using a bitwise OR (|).
  - If there is no error, the key  is correct.

Reference bytes, DAT_00102004:
![Challenge](/assets/img/posts/labyrinth/ref.png) <br>
Using this, we can write a decoding script.
![Challenge](/assets/img/posts/labyrinth/script.png) <br>

Then we get our flag: ![Challenge](/assets/img/posts/labyrinth/flag.png) <br>
Flag: _acdfCTF{x0r1ng_w1th_65}_
