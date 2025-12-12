---
title: 300
date: "2025-05-31"
description: Now Shit is getting real a bit, but it's easy, don't over think, it's no maze and you are no runner
author: b33tl3
image: logo.png
categories: [Reverse Engineering]
tags: [Rev, Cyberlympics]
---

### Challenge Overview
![](300.png)<br>
"Now Shit is getting real a bit, but it's easy, don't over think, it's no maze and you are no runner" <br>
We are given a 64-bit PIE ELF binary that prompts for a key and verifies it. From the description, we are told not to overthink it cause it is easy. <br>
Our goal is to find the correct input key to get the flag.

### Initial Recon
Using 'file' and 'checksec', we get: <br>
![](recon.png) <br>
We see that the binary is a PIE executable with no canary, NX enabled, and partially RELRO.

Running the binary prompts for a key, and a random key yields 'Wrong key!: <br>
![](run.png)

### Analysis
#### String Extraction
Running 'strings obfuscated' revealed some interesting strings: <br>
    - Rev3rs3MH <br>
    - s3Maze! <br>
    - acdfCTF{ObfuscatedSuccess} (likely the flag) <br>
    - Wrong key! <br>
    - Correct! Flag: %s <br>

#### Tracing library cells (ltrace)
Running 'ltrace ./obfuscated' and entering some random input shows: <br>
![](ltrace.png)
The program calls 'strlen("Rev3rs3Maze!")' internally. This is a strong hint that "Rev3rs3Maze!" is the key or closely related.

#### Key hypothesis
The program checks for the input key to match "Rev3rs3Maze!". <br>
This string is a combination of or close variant of the strings seen in 'strings' output.

### Solution
Let's enter the key: Rev3rs3Maze! <br>
![](flag.png) 
Entering the key gives us the flag. <br>
Flag: _acdfCTF{ObfuscatedSuccess}_
