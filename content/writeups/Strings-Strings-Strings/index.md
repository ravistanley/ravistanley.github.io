---
title: Strings Strings Strings
author: b33tl3
description: The CTI intercepted the attached file. Analyze it and respond to the questions. Download the attached file and make sense out of it.
date: 2024-11-02 00:00:00+0000
categories: [Misc, Easy]
tags: [Misc, Easy, Bsides]
---

### Introduction
Welcome to my first blog! I'm excited! In this blog post, we'll explore a Misc challenge.It was a challenge set by Bsides to select the first fifteen hackers and sponsor them to the event.I hope you will enjoy it!
![Challenge](strings_strings.png)

We are required to answer the following questions: <br>
Question 1: What is the SHA256 Hash of the file ? Enter your answers in upper case<br>
Question 2: How many emails from the BackdoorDiplomacy Planning Report have Senegal ccTLD<br>
Question 3: Based on the previous questions, how many unique domains are in the report?<br>
Question 4: Which domain is referenced the most ? eg example.com<br>
Question 5: Which attack vectors are being used by the threat actor?<br>
            Ransomware attack<br>
            Phishing Campaigns<br>
            Exploiting Vulnerabilities<br>
            Insider Threats<br>
            DDoS Attacks<br>

We are given a zip file: _masheveve.zip_. After downloading the file, we do a 'unzip misheveve.zip' command on our terminal. ![Unzip](unzip.png)

### Question 1
**SHA256 Hash of the file.**<br>
We need to check the type of our file first, using command _file <filename>_. Upon checking, we find out it has ASCII text.
![file](file.png)
Since we are asked to provide the SHA256 Hash of the file, we _'sha256sum <filename>'_. 
![hash](hash.png)
We find the hash of the file, but we are asked to submit it in uppercase. So we _tr_ command. 
![upper](upper.png)
Our flag: _5A602BB8BA0F56285408093EEFE5F5DEE9F9134706D2788CAC40A89BAD2FBF66_

### Question 2
**We are asked to provide the number of emails from BackdoorDiplomacy Planning report that have Senegal ccTLD.**<br>
We are to look for emails with '_.sn_'.

Let's go on and cat the file. ![cat](cat.png) Woah! We find that it a long bunch of text that we can't read. But at the end of file, we see a sign that it might be base64 encoded. 
Let's move it to our '_cyber kitchen'_ and see what we can do about the unreadable text. On CyberChef, we decode our text using base64 decode, and sadly we get a bunch of 'loren ipsum' text. ![cyberchef](cyberchef.png)
![Confused](confused.gif)

Reading through the 'loren ipsum' text, we can find a block of base64 encoded text. ![base64](base64.png) Interesting. Let's decode it again and see what we have. After decoding, we get a readable report. Yippee! ![report](report.png)

Smooth. Now that we have a readable report, let's copy it and save it as _report.txt_. Let's open our file using sublime text. We are asked the number of emails with _.sn_. Using _'ctrl find'_, we find that there are 15 emails.<br>
Our flag: _15_

### Question 3
**Unique domains in the report.**<br>
Let's write a python script that will help us get the number of unique domains from the report. We will use the script to solve both Question 3 and Question 4.
![sol](sol.png)
On running the script, we find that the number of unique domains is 39.
![domains](domains.png)
Our flag: _39_

### Question 4
**Most referenced domain**<br>
From our script, we get that the most referenced domain is: example.tg<br>
Our flag: _example.tg_

### Question 5
**Attack vectors**<br>
This question is quite straight forward as it asks us to select the attack vectors used by the threat actor. From our report, at the end of the file, there is a section on Attack Vector Overview.
![vector](vector.png)
We select all the four except _Ransomware attack_.

Yay! We are done!
I hope you enjoyed the blog:)
