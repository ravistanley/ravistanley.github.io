---
title: See Ya
Author: b33tl3
description: 'You a fan fan of Zines? Well here''s my fav from the 90''s 🙂 , The "Hacker''s Manifesto". Not every one will SEE it, but oh well, good luck. Of course if you''re SEEing this, here''s the original one: https://phrack.org/issues/7/3.html'
date: 2024-11-20 00:00:00+0000
image: /assets/img/posts/perfectroot.png
categories: [Misc, Easy]
tags: [Misc, Easy, Perfectroot]
---

### Challenge Overview
You are a fan of Zines? Well here's my fav from the 90's 🙂, he "**Hacker's Manifesto**". Not every one will SEE it, but oh well, good luck.

Of course if you're SEEing this, here's the original one: https://phrack.org/issues/7/3.html

### Analysis
"Zines are self-published magazines"

From the description, it seems to be a "**Hacker's Manifesto**" but encrypted:- "_Not every one will SEE it, but oh well, good luck_". We a given a text file, **Volume_One_Issue_7_Phile_3_of_10.txt**.

Let's read the file using 'cat' command. <br>
![image](https://gist.github.com/user-attachments/assets/ea779ea5-e7cb-4503-9064-68b57d10ee81) <br>
On reading the file, we see that it is some braille text. We should decode the braille text to readable text.

### Solution
Time to do some baking using CyberChef. We can either use CyberChef or dcode, but CyberChef it's my favourite. Let's copy the text and paste it in CyberChef. 

We will use '_From Braille'_ as the recipe.
![cyberchef](https://gist.github.com/user-attachments/assets/4e2f0c34-df70-48a6-9961-bd9a95c70a0f)

Now we have readable text. Let's read through the Manifesto.
We find the flag hidden in the text.
![image](https://gist.github.com/user-attachments/assets/704257a2-1b2b-4f17-ae12-bea9fbf3477a)

Boom! Another flag down!
