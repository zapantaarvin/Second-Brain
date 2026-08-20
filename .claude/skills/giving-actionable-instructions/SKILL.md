---
name: giving-actionable-instructions
description: Use whenever handing Arvin an action item — something only he can do, or a manual step outside what Claude can execute (open an app, run a command, make a decision). Always give the concrete step-by-step instructions for how to do it, not just a description of what needs to happen.
---

# Giving Actionable Instructions

## Purpose
Arvin's standing rule (2026-08-20): when Claude Code hands something to Arvin to do, it should read as steps he can immediately follow, not a task description he has to figure out how to execute himself.

## Rule
Any time an action item is handed to Arvin — a "needs Arvin, not Claude" item on a checklist, a manual verification step, a one-time setup action — include:
- The concrete sequence of steps, numbered, in the order to do them
- The exact command, path, menu location, or click sequence where applicable — not "run the installer," the actual command; not "open the settings," which app and which menu
- What "done" looks like, so Arvin can confirm he did it right without a follow-up question

## What this replaces
Naming a task without saying how to do it. "You need to open Obsidian and reload the vault" names the task but not the steps — say where to click, what should appear once it's worked, and how to tell if it didn't.

## Where this shows up
Anywhere a plan/checklist (see `planning-before-execution`) or a status report has a Tier-2-style "needs Arvin" item — that item's instructions should already be spelled out there, not left for Arvin to ask "how do I do that" afterward.
