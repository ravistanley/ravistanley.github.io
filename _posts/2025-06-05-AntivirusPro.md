---
title: AntivirusPro
description: The antivirus says your system is clean... but are you sure?
slug: rev
date: 2025-06-05 00:00:00+0000
image: /assets/img/posts/logo.png
categories:
    - Reverse Engineering
tags:
    - Rev
    - Cyberlympics
points:
    - 1000
flag format:
    - acdfctf{}
---

### Introduction
In this reverse engineering challenge, we're given an executable called AntivirusPro and told: <br>
“The antivirus says your system is clean... but are you sure?”

![Challenge](/assets/img/posts/antiviruspro/antiviruspro.png) <br>
This sounds like a classic misdirection challenge — where the program pretends to be doing something straightforward, but hides its real behavior. The challenge tests our ability to reverse engineer binary logic, understand obfuscation, and recover hidden data.

### Initial Recon
We start by examining the binary properties using standard Linux tools. <br>
_File_ output: ![Challenge](/assets/img/posts/antiviruspro/file.png) <br>
  - It's 64-bit Linux ELF binary.
  - Statically linked: means no dynamic dependencies.
  - Not stripped: function names are preserved.

_Checksec_ output: ![Challenge](/assets/img/posts/antiviruspro/checksec.png) <br>

_Running the binary_:
![Challenge](/assets/img/posts/antiviruspro/run.png) <br>
We get a fake-looking flag: _CLEAN_SYSTEM_. This is clearly not the real flag (no acdfctf{} format).

### Static Analysis in Ghidra
Since the binary is not stripped, I imported it into Ghidra and inspected various functions. <br>
_Observations in main function_
Looking at the _main_ function, we see:
  - Strings and fake scanning output.
  - A suscipious call to a function called _isDebugged()_.
  - If _isDebugged()_ returns true, it runs a second code path. It shows "Debugger detected! Extracting hidden data..." Then it decrypts and prints a hidden string using _xorCipher_.<br>
  ![Challenge](/assets/img/posts/antiviruspro/main.png) <br>
  - "xorCipher(local_98, local_78);" This is the key decryption step. _local_78_ contains the intermediate XOR-encoded data, obtained by applying 'xorCipher()' once on the obfuscated string earlier. Now, 'xorCipher()' is being applied again on that result (local_78) to produce the final flag in _local_98_.
  - That means if we debug the the binary, it will show the real flag, encypted - we just have to extract and decrypt it.

### Dynamic Analysis
I ran the binary in _gdb_: ![Challenge](/assets/img/posts/antiviruspro/gdb.png) <br>
We see the encrypted flag: #!&$!6$9.q61&rwr/q0q'd'?42

### Reversing xorCipher
From Ghidra: _std::__cxx11::string::operator+=(param_1, *iterator ^ key);_
This confirms a single-byte XOR cipher. <br>
We know that the string was encrpyted twice:
- The original flag --> XORed --> intermediate result.
- Intermediate --> XORed again --> the obfuscated string we see: #!&$!6$9.q61&rwr/q0q'd'?42
All we need now is to bruteforce-decrypt this with all possible XOR keys.

### XOR Decryption
I came up with the following python script to solve this:
![Challenge](/assets/img/posts/antiviruspro/script.png) <br>

Output:
![Challenge](/assets/img/posts/antiviruspro/flag.png) <br>
We get the flag: _acdfctf{l3tsd050m3r3e&e}_
  
