---
layout: single
title: "Clipboard Images in Claude Code CLI"
date: 2026-01-12 00:00:00 -0600
categories: ai development
tags: ai claude cli productivity
permalink: /:year/:month/:title/
---

Anyone using Claude Code CLI in a terminal has probably hit the friction of sharing an image with it to see a problem. I would have to take a screenshot, save it somewhere, get the file path, and tell Claude in the prompt to look at it.

On Windows (and other platforms) you can grab images off the clipboard. The green light hit me today and I set up a custom Claude command with Claude's help. Just put this in your `~/.claude/commands` folder:

**clip.md**
```markdown
---
allowed-tools: Bash(powershell:*), Read
description: Grab an image from clipboard for analysis
---

Capture the clipboard image by running this PowerShell command:

powershell -Command "Add-Type -AssemblyName System.Windows.Forms; $img = [System.Windows.Forms.Clipboard]::GetImage(); if ($img) { $path = \"$env:TEMP\clipboard_$(Get-Date -Format 'yyyyMMdd_HHmmss').png\"; $img.Save($path); Write-Host $path } else { Write-Error 'No image in clipboard'; exit 1 }"

Then read the image file at the output path to analyze it. After analyzing, address the user's request: $ARGUMENTS
```

## Usage

```
/clip can you fix the button alignment shown here to be below the cancel button
```

## What Happens

1. Claude runs the PowerShell script to capture the clipboard image
2. Saves it to `%TEMP%\clipboard_TIMESTAMP.png`
3. Reads the image file
4. Analyzes it and responds to your prompt

## Try It

1. Copy an image to your clipboard (e.g., take a screenshot with `Win+Shift+S`)
2. Type `/clip describe what you see`

The command is stored at `~/.claude/commands/clip.md` so it works in any Claude Code session.
