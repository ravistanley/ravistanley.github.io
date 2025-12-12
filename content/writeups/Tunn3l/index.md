---
title: TUNN3L
author: b33tl3
description: A compromised host on the network is exfiltrating sensitive data by tunneling it over DNS queries to an external, attacker-controlled domain. Your task is to identify the compromised host, reconstruct the exfiltrated data, and ultimately find the flag.
date: 2025-06-24 00:00:00+0000
categories: [CTFRoom, Forensics]
tags: [Forensics, Easy]
---

## Analysis: What juicy information do you get from your analysis?
This was an easy forensics challenge. <br>
We're given a PCAP file containing only two DNS packets, making this a very focused and quick forensic investigation. Let's walk through the process.

### Question 1: What is the IP address of the machine exfiltrating data?
Using Wireshark, we observe a DNS query sent to 8.8.8.8. The source IP address of that packet is:
192.168.1.74
![Challenge](ip.png) <br>
This indicates the machine initiating the outbound DNS request — the likely compromised host. <br>
Answer: 192.168.1.74

### Question 2: What is the full domain name used by the attacker for data exfiltration?
Let’s analyse the first packet. Inspecting the query name gives us a domain name: crazzyc4t[.]com
![Challenge](query.png) <br>
Answer: crazzyc4t[.]com

### Question 3: What type of DNS record is primarily being used for the data exfiltration?
From the DNS packet details, we see the query is of type: A <br>
Answer: A

### Question 4: What encoding method was used for the exfiltrated data within the DNS queries?
In the packet details, there is a subdomain string: _Q1RGe1RVTk4zTEwxTkdfRE5TX0wxSzNfNF9QUjB9_. This definitely looks like Base64 encoded. <br>
Answer: Base64

### Question 5: After extracting and reassembling the fragmented data from the DNS queries, what is the complete decoded string in the final flag?
Let’s decode this using python.
![Challenge](flag.png) <br>
Final flag: CTF{TUNN3LL1NG_DNS_L1K3_4_PR0}