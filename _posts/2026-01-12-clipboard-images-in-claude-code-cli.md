---
layout: single
title: "Clipboard Images in Claude Code CLI"
date: 2026-01-12 00:00:00 -0600
categories: ai development
tags: ai claude cli productivity
permalink: /:year/:month/:title/
---

Anyone using Claude Code CLI in a terminal has probably hit the friction of sharing an image with it to see a problem. I would have to take a screenshot, save it somewhere, get the file path, and tell Claude in the prompt to look at it.

On Windows (and other platforms) you can grab images off the clipboard. The green light hit me today and I set up a custom Claude command with Claude's help.

## Setup

Create two files in your `~/.claude/` folder:

**commands/clip.md**
```markdown
---
allowed-tools: Bash(powershell.exe:*), Read
description: Grab an image from clipboard for analysis
---

Capture the clipboard image by running the clipboard-image.ps1 script:

powershell.exe -NoProfile -ExecutionPolicy Bypass -File "C:\Users\YOUR_USERNAME\.claude\clipboard-image.ps1"

The script outputs the saved image path (with timestamp) to stdout. Read the image file at that path to analyze it.

After analyzing the image, address the user's request: $ARGUMENTS
```

**clipboard-image.ps1**
```powershell
# clipboard-image.ps1
# Saves clipboard image to temp folder with timestamp

Add-Type -AssemblyName System.Windows.Forms

$img = [System.Windows.Forms.Clipboard]::GetImage()

if ($img) {
    $timestamp = Get-Date -Format "yyyyMMdd_HHmmss"
    $path = "$env:TEMP\clipboard_$timestamp.png"
    $img.Save($path)
    Write-Host $path
} else {
    Write-Error "No image in clipboard"
    exit 1
}
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

## Update: January 13, 2026

I refactored to use an external PowerShell script instead of an inline command. On Windows, being more explicit with `powershell.exe` and the full script path prevents Claude from iterating on a working script each time the command runs.

Real-world test: My wife had knee surgery yesterday and I copied one of the doctor-supplied arthroscopic images to the clipboard:

```
❯ /clip what is this an image of

● Bash(powershell.exe -NoProfile -ExecutionPolicy Bypass -File "...")
  ⎿  C:\Users\toddw\AppData\Local\Temp\clipboard_20260113_103327.png

● Read(~\AppData\Local\Temp\clipboard_20260113_103327.png)
  ⎿  Read image (1.5MB)

● Arthroscopic surgery image. Looks like a view inside a joint
  (likely shoulder or knee) through a scope. You can see:

  - Smooth cartilage surface (left side, cream-colored dome)
  - Torn/damaged tissue (white fibrous material in center)
  - Surgical instrument (metal probe on right)
  - Typical circular arthroscope view with black border

  Possibly a labral tear repair or rotator cuff procedure based
  on the anatomy visible.
```

Claude nailed it.
