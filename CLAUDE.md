# CLAUDE.md: Feelings Check-in (web MVP)

Read this before every task.

## What this is
A one-page web app on my son's iPhone. He taps one of three faces (happy,
neutral, sad) to say how he feels. An iOS Shortcuts automation opens the
page in Safari a few times a day. It's a family support tool, not a
medical or diagnostic one.

The spec is mvp-brief.md. It is the source of truth. If this file and
the brief disagree, follow the brief and point out the conflict.

## Current goal (v1.1): simplify
He found the custom photo faces and the spoken sad prompt confusing.
This version makes the app simpler, not bigger.

## Repo layout
```
index.html     the whole app: HTML, inline CSS, vanilla JS
README.md      local testing, deployment, Shortcut setup
TESTING.md     manual test checklist for his iPhone
mvp-brief.md   the spec (don't edit unless asked)
CLAUDE.md      this file
```
Don't add files or folders unless a task asks for them.

## Commands
- Run locally: `python3 -m http.server 8000`, open `http://localhost:8000`
- Parent log: `http://localhost:8000/#log`
- No build step, installer, linter, or test runner. Don't add one.

## Hard rules (never break these)
**Privacy**
- No backend, server, cloud storage, APIs, accounts, cookies, or IDs.
- No network requests after page load: no analytics, CDNs, web fonts,
  or external scripts.
- Data lives only in this browser's IndexedDB on his phone.

**His experience**
- One tap. No typing. No reading required: pictures carry the meaning.
- Faces are the standard emoji only. No custom photos.
- Every tap gets an instant, warm response.
- No timers, countdowns, scores, streaks, badges, or wrong answers.
- No speech synthesis anywhere. The app never talks.
- The voice step is optional. "All done" is exactly as big and friendly
  as "Talk". The sad check-in is saved before the voice step.
- Never show him an error. Log details to the console only.

**Not clinical**
- No scores, trends, summaries, interpretations, or alerts. The log
  shows raw check-ins only.

**Keep it small**
- Single index.html, vanilla JS, inline CSS. No frameworks, npm,
  bundlers, TypeScript, service workers, or PWA manifests.
- Don't build anything in the brief's out-of-scope list. If something
  seems missing, ask instead of adding it.

## Code conventions
- Comment for a learner: I'm an experienced database engineer, new to
  front-end and iOS. Explain why, especially for IndexedDB,
  MediaRecorder, and iOS Safari quirks. Skip basic data concepts.
- Plain, readable code. Small named functions. async/await.
- Pick the audio type with `MediaRecorder.isTypeSupported`. Stop all
  mic tracks as soon as recording ends.
- Guard against double taps.

## How to work
- Before coding, give a short plan of what you'll change.
- Make the smallest change that does the task. No unrelated refactors.
- After changes, tell me how to check them by hand.
- Update README.md and TESTING.md when behavior or setup changes.
- Explain what you changed and why, so I learn it.
- When unsure, ask one specific question instead of guessing.
