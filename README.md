# Raycast Mac Automation

Built in one day as part of my developer/data analyst journey.

## What This Does

A macOS automation system built with Raycast that:
- Captures clipboard text and saves it simultaneously to Notion (via API) and a local text file
- Triggered by a global hotkey (`Cmd+Shift+N`) from anywhere on the Mac
- Includes a 41-snippet library for data analysis and AI prompting

## Files

- `notion-clipper.sh` — Bash/Python script that calls the Notion API and appends to a local file
- `raycast-snippets-v3.json` — 41 custom Raycast snippets for data analysis and Claude prompts

## Tech Used

- Bash + Python
- Notion API
- Raycast Script Commands
- macOS clipboard (`pbpaste`)

## What I Learned

- How REST APIs work (HTTP requests, authentication tokens, JSON payloads)
- Bash scripting and Python subprocess handling
- Debugging JSON encoding issues with special characters
- Connecting local Mac automation to cloud services
