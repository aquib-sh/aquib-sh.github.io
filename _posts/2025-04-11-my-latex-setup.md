---
layout: post
title: "Fix for WhatsApp PDF Warning: ‘This Document Might Be Harmful for Your Device’"
date: 2025-04-11
categories: [Technical, Setup]
tags: [personal, latex, productivity, overleaf, whatsapp, resume, pdf]
permalink: /:year/:month/:title.html
description: "If WhatsApp shows 'This document might be harmful for your device' when you send a PDF made on Overleaf, here's why I switched to a local LaTeX setup—and how you can too."
keywords: ["WhatsApp PDF warning", "This document might be harmful for your device", "Overleaf WhatsApp issue", "LaTeX setup", "fix Overleaf resume WhatsApp", "PDF flagged WhatsApp"]
---

We all love LaTeX (at least I do!).  
It helps me create and edit my resume without making a mess.  
What's better? I can maintain it in Git—create branches, roll back if needed—way better than fiddling with Word documents.  
I can even send it to GPT for a review.
in.

Have you ever sent a PDF via WhatsApp and seen this warning?

> **This document might be harmful for your device. Make sure you trust the sender before you open it.**

This happened to me when I sent my resume, generated on **Overleaf**, to a hiring manager. It was a clean file—no malware, no scripts—but WhatsApp flagged it anyway.  
Apparently, Overleaf PDFs trigger something in WhatsApp’s automated filters.

I found a temporary workaround using Google Docs, but it wasn’t a clean or dependable fix for this. That’s when I decided to move away from Overleaf and build a local LaTeX setup that wouldn’t trigger warnings—and would give me more control in the long run.

**In this post, I’ll share my LaTeX setup and how switching to a local compiler solved the issue.**

---
<br/>

## My Local LaTeX Setup (Ubuntu)

I use **Ubuntu**, so the commands and package names below might vary depending on your OS or distro.  
Here’s what I use:

- **LaTeX Engine**: `texlive-full` with `pdflatex`
- **Editor**: Mostly **VIM**, sometimes **VS Code** with the *LaTeX Workshop* extension

---
<br/>

## Installation

There are many packages available for compiling LaTeX to PDF, but I found this one to be the most reliable and comprehensive.  
Other minimal packages often miss important dependencies like color packages, styles, and fonts—which you'll likely need anyway.

```shell
sudo apt install texlive-full pdflatex
```

> ⚠️ **Note:** This package is big—around a gigabyte—but it’ll save you a ton of headache later by including everything you might need.

---
<br/>

## Usage

To compile a `.tex` file to PDF, use the command below:

```shell
pdflatex filename.tex
```
This command creates a filename.pdf and a .log file with detailed output.
Unlike the Overleaf version, this locally compiled PDF no longer triggers WhatsApp’s “This document might be harmful” warning. 🎉
