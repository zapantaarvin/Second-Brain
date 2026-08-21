---
type: inbox
captured: 2026-08-20
status: pending
---

# Screenshot workflow — prompt for location and rename

Arvin wants his Mac's screenshot behavior changed: every time he takes a screenshot, get prompted to choose a save location and rename it, instead of it auto-saving with a default name. Wants a `Screenshots/` folder inside `Documents/Brain` as part of this.

## What this actually needs (research before building)
- macOS's built-in screenshot tool doesn't natively support "always prompt for filename" — `defaults write com.apple.screencapture location <path>` only changes the *default save folder*, it doesn't add a prompt-and-rename step.
- Getting an actual "choose location + rename" dialog on every capture likely needs one of: a Shortcuts/Automator "Folder Action" watching a capture folder and triggering a rename dialog, a small script bound to the screenshot keyboard shortcut instead of the default `screencapture`, or third-party software.
- Decide: does "a Screenshots folder inside Brain" mean the vault should literally hold raw screenshots (probably not, that's binary/media clutter in a text-based vault) — or is `Documents/Brain/Screenshots/` meant to sit *alongside* `Second-Brain`/`Second-Brain-Personal`, outside either git repo, just for organizational convenience? Worth confirming before setting anything up.
