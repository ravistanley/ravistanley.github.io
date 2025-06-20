---
title: Mystique
description: "The antivirus says your system is clean... but are you sure?"
slug: forensics
date: 2025-06-16 00:00:00+0000
image: /assets/img/posts/logo.png
categories:
    - Forensics
tags:
    - forensics
    - email analysis
    - traffic analysis
    - CTFRoom
---

## Introduction
I was going through CTFRoom vaults looking for a CTF I did earlier. Bad news is I didn't find it - maybe because it hasn't been uploaded yet. Good news is I found another interesting vault called Mystique - a forensics vault. This one has some email analysis in it. Considering that I was almost 'phished' the other day, I had to go through it. I even researched on the tools to use. In this vault I will use phishtool and ANYRUN. <br>
Let's gooo!!

This vault contains two modules: <br>
- Email Forensics
- Network Analsis

### Email Forensics
#### Description
I recently encountered a potentially malicious email and thought it would be interesting to turn it into a CTF challenge. The email has an attachment that seems suspicious. Can you help analyze and decipher its purpose? <br>
The creator has also shared some guidelines: <br>
- The provided files are genuine malware samples, so treat them with caution.
- Always examine them in a sandboxed environment to ensure safety.
- For your convenience, the zip files containing these samples are password-protected. Use the password "infected" to access them.
- If needed, feel free to use online resources for assistance. One recommended online sandbox tool is AnyRun.

The file shared is: infected.zip. <br>
Let's analyze it. <br>
Unzipping it using the password shared, we get 'infected.eml'. I loaded the file in Phishtool for analysis. <br>

#### Question 1
Who is the sender of this email? <br>
We are asked to provide the sender of the email as it is in the email headers. <br>
