---
title: "How I Built a Better Browser History Tool for Volatility 3"
date: 2025-11-23T22:26:18+01:00
draft: false
tags: ["cybersecurity", "volatility3", "memory-forensics", "incident-response", "tools"]
categories: ["Digital Forensics", "Project"]
author: "Ayoub Nait Lamine"
description: "My journey from cybersecurity student to tool creator - building a multi-browser history extraction plugin for Volatility 3"
cover:
    image: "images/volatility3.png"
    alt: "Volatility 3 Browser History Tool"
    caption: "Building tools to solve real problems in memory forensics"
ShowToc: true
TocOpen: false
---

## How It Started

My journey began with **Malware Incident Response Training** from [maltrak.com](https://maltrak.com). I was studying Incident Response Process and learning how professionals investigate cyber attacks. 

The training showed me how important memory analysis is for finding what happened during an attack. I wanted to practice these skills, so I started looking for more ways to analyze memory dumps.

## Finding a Cool Technique

While searching for more learning materials, I found a [**CTF writeup**](https://blog.bi0s.in/2019/09/24/Forensics/InCTFi19-NotchItUp/). The author used a special tool to extract Chrome browsing history from a memory dump to find a hidden flag.

They used this command:

```bash
volatility --plugins=volatility-plugins/ -f Challenge.raw --profile=Win7SP1x64 chromehistory
```

I thought this was amazing! You could actually see what websites someone visited directly from their computer's memory.

## The Problem I Faced

When I tried to use this technique myself, I discovered a big problem. The tool they used was **Volatility 2**, but the cybersecurity world had moved to **Volatility 3**. 

The old plugins didn't work with the new version. It was like trying to use an iPhone app on an Android phone - they're just not compatible.

## Finding a Solution

I searched for a solution and found a blog post by Edmund Hughes where he created a **Chrome plugin for Volatility 3**. This was exactly what I needed!

But when I tried to use his plugin, I ran into problems:

- It didn't work with my version of Volatility 3
- It only supported Chrome, not other browsers
- The output was hard to read
- There were error messages I didn't understand

## Taking Matters Into My Own Hands

Instead of giving up, I decided to **build my own solution**. I combined:

1. The official Volatility 3 documentation
2. The good ideas from Edmund Hughes' plugin
3. My own needs for a better tool

## What I Built

I created a new plugin that:

### Supports Multiple Browsers

- **Chrome** - the most popular browser
- **Firefox** - many privacy-conscious users
- **Microsoft Edge** - built into Windows
- **Brave** - growing in popularity

### Shows Complete Information

- **Full URLs** - not cut off or shortened
- **Readable dates** - not confusing number timestamps
- **Clear formatting** - easy to understand output
- **Visit counts** - how many times each site was visited

### Easy to Use

```bash
# Simple command to see all browser history
vol.exe -f memory.dump windows.browserhistoryextractor

# Focus on one browser
vol.exe -f memory.dump windows.browserhistoryextractor --browser firefox

# Look at one specific program
vol.exe -f memory.dump windows.browserhistoryextractor --pid 2440
```

## Real-World Example

Here's what the tool found in a recent investigation:

```
Browser  Source   ID  URL                                                    Title                           Domain      Visits  Typed  Last Visit        Hidden
Chrome   History  197 https://www.win-rar.com/postdownload.html?&L=0         WinRAR download: Post-Download   win-rar.com 1       0      2025-01-15 11:36:45 No
Chrome   History  196 https://www.win-rar.com/predownload.html?&L=0          WinRAR download: Pre-Download    win-rar.com 1       0      2025-01-15 11:36:35 No  
Chrome   History  194 https://www.win-rar.com/download.html                  WinRAR download: Download        win-rar.com 1       0      2025-01-15 11:36:12 No
```

This output shows:

- **Someone downloaded WinRAR** on January 15, 2025
- **Complete timeline** of the download process
- **Full URLs** for complete forensic analysis
- **Clear formatting** that's easy to understand

## How It Helps Investigators

This tool helps cybersecurity professionals:

1. **Understand attack scope** - What websites did the hacker visit?
2. **Build timelines** - When did the malicious activity happen?
3. **Find evidence** - What software was downloaded?
4. **Save time** - No more manual database analysis

In the example above, we can see exactly when someone downloaded WinRAR, which could be important if:

- Malware was hidden in a compressed archive
- The attacker used compression tools to hide stolen data
- We need to understand what software was installed during an incident

## What I Learned

1. **Persistence pays off** - When something doesn't work, keep trying different approaches
2. **Building is learning** - Creating the tool taught me more than just using it
3. **Community matters** - Sharing knowledge helps everyone improve
4. **Simple is better** - Tools should be easy to use and understand

## You Can Use It Too

If you're learning cybersecurity or working in incident response, you can use this tool too. It's available on [GitHub](https://github.com/aynaitlamine/volatility3-browser-history) and works with the latest Volatility 3.

The journey from watching training videos to building a real tool has been incredible. It shows that with enough curiosity and persistence, anyone can create solutions to real problems in cybersecurity.

---

*Have you faced similar challenges with memory analysis tools? What tools do you wish existed to make your investigation work easier?*
