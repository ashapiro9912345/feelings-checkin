# AGENTS.md — Feelings Check-in (web MVP)

Project-level instructions for coding agents. Read this before every task.

## What this is
A one-page web app on a child's iPhone. He taps one of three faces (happy,
neutral, sad) to say how he feels. An iOS Shortcuts automation opens the
page in Safari a few times a day. It's a family support tool, not a
medical or diagnostic one.

**The full spec is `mvp-brief.md`. It is the source of truth.** If this
file and the brief disagree, follow the brief and point out the conflict.

## Repo layout
```
index.html      the whole app: HTML, inline CSS, vanilla JS
README.md       local testing, deployment, iPhone Shortcut setup
TESTING.md      manual test checklist for his iPhone
mvp-brief.md    the spec (don't edit unless asked)
AGENTS.md       this file
```
Do not add other files or folders unless a task asks for them.

## Commands
- Run locally: `python3 -m http.server 8000`, then open
  `http://localhost:8000` (localhost counts as secure, so the mic works).
- Parent log view: `http://localhost:8000/#log`
- There is no build step, install step, linter, or test runner. Don't add one.

## Hard rules (never break these)
**Privacy**
- No backend, server code, database server, cloud storage, or APIs.
- No network requests after page load: no analytics, telemetry, CDNs, web
  fonts, external scripts, or images from other domains.
- No accounts, logins, cookies, or identifiers.
- Data lives only in the browser's IndexedDB on the device.

**The child's experience**
- One tap. No typing, no reading required.
- Every tap gets an instant, warm response.
- No timers, countdowns, scores, streaks, badges, or "wrong" answers.
- The voice step is optional. "No thanks" is as big and friendly as the
  record button. The sad check-in is saved *before* the voice step and
  never depends on it.
- Never show him an error message. Fail quietly and kindly; log details
  to the console only.

**Not clinical**
- No mood scores, trends, summaries, interpretations, or alerts. The log
  shows raw check-ins only.

**Keep v1 small**
- Single `index.html`, vanilla JS, inline CSS.
- No frameworks, bundlers, npm, TypeScript, Docker, service workers, PWA
  manifests, or infrastructure-as-code.
- Don't build anything in the brief's "Explicitly out of scope" list. If
  you think something is missing, ask; don't add it.

## Code conventions
- Comment for a learner. The owner is an experienced database engineer
  but new to front-end and iOS. Explain *why*, especially for IndexedDB,
  MediaRecorder, speech synthesis, and iOS Safari quirks. Skip comments
  explaining basic data concepts.
- Prefer plain, readable code over clever code. Small named functions.
- Use `async/await` and wrap IndexedDB in small promise helpers.
- Pick the audio type with `MediaRecorder.isTypeSupported` (iOS Safari
  records `audio/mp4`). Stop all mic tracks as soon as recording ends.
- Guard against double taps so one tap makes exactly one check-in.
- Target current iOS Safari: portrait, safe-area insets, no horizontal
  scroll, no zoom on tap, large touch targets.

## How to work
- Before coding, give a short plan (a few bullets) of what you'll change.
- Make the smallest change that completes the task. Don't refactor
  unrelated code.
- After changes, list how to check them by hand (which taps, what should
  happen), since there are no automated tests.
- Update `README.md` or `TESTING.md` when behavior or setup changes.
- When unsure, ask one specific question instead of guessing.
