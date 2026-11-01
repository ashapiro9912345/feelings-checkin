# Feelings Check-in — Web MVP Brief

## Purpose
A tiny web page that lets my son tell me how he's feeling during the day.
The page is opened on his iPhone by a Shortcuts time-of-day automation
("Open URLs"). He taps one of three faces. That's the whole interaction.

This is a family support tool, not a medical or diagnostic one. Nothing in
the app should score, interpret, diagnose, or summarize his feelings.

This MVP exists to answer one question: **will he actually use it?**
Keep it as small as possible.

## Who uses it
- **My son (primary user).** Low demand: one tap, no typing, no reading
  required. Must feel warm and safe, never like a test.
- **Me (parent).** Occasionally opens a simple log view on his phone to see
  check-ins and play clips.

## User stories and acceptance criteria

### 1. Check in with a face
As my son, I want to tap a face so I can say how I feel without words.

- WHEN the page loads THE SYSTEM SHALL show the question "How are you
  feeling?" and three large face buttons: happy, neutral, sad.
- The faces SHALL be understandable without reading (large emoji or simple
  drawings), each with an accessible label for screen readers.
- Each face button SHALL be at least 30% of the screen width and easy to hit
  with a thumb.
- WHEN any face is tapped THE SYSTEM SHALL respond within 100 ms with a
  friendly visual response (e.g. a gentle animation and a short warm
  message like "Thanks for telling me!").

### 2. Happy or neutral
- WHEN happy or neutral is tapped THE SYSTEM SHALL save a check-in with the
  face and the current timestamp.
- THE SYSTEM SHALL then show a friendly thank-you and return to the
  three-face screen after a few seconds.

### 3. Sad, with an optional voice clip
As my son, I can say why I'm sad if I want to, and skip it if I don't.

- WHEN sad is tapped THE SYSTEM SHALL immediately save a check-in with the
  face and timestamp, before anything else happens. The check-in must
  never depend on the recording.
- THE SYSTEM SHALL then show "Tell us why you're sad" and speak it aloud
  using the browser's speech synthesis (the tap counts as the user gesture
  iOS requires).
- THE SYSTEM SHALL show two big buttons: a microphone button to record and
  a "No thanks" button. Both are equally prominent. "No thanks" is never
  hidden, small, or styled as the wrong choice.
- WHEN the microphone button is tapped THE SYSTEM SHALL record audio for up
  to 10 seconds, stopping automatically at 10 seconds. He can also tap to
  stop early.
- There SHALL be NO visible countdown, number, or progress-pressure timer.
  A calm "listening" indicator (e.g. a softly pulsing circle) is fine.
- WHEN recording stops THE SYSTEM SHALL attach the clip to the sad check-in
  that was already saved, show a warm thank-you, and return to the
  three-face screen.
- WHEN "No thanks" is tapped THE SYSTEM SHALL show a warm thank-you (no
  disappointment, no second prompt) and return to the three-face screen.

### 4. Things going wrong, gracefully
- IF microphone permission is denied or recording fails THEN THE SYSTEM
  SHALL keep the sad check-in, show the same warm thank-you, and never
  show an error message to him. (Log details to the console only.)
- IF speech synthesis is unavailable THEN THE SYSTEM SHALL just show the
  text.
- Tapping multiple times quickly SHALL NOT create duplicate check-ins
  (ignore taps during the response animation).

### 5. Parent log view
As the parent, I want to see his check-ins and play clips on his phone.

- THE SYSTEM SHALL provide a log view at the same page with `#log` in the
  URL. There is no link to it from his screen.
- The log SHALL list check-ins newest first, grouped by day, showing the
  face, the time, and a play button when a clip exists.
- The log is read-only in this MVP: no delete, no export, no charts, no
  summaries, no counts or streaks.

## Technical requirements
- One file: `index.html` with inline CSS and vanilla JavaScript. No
  frameworks, no build step, no package manager, no external scripts,
  fonts, or CDNs.
- Storage: IndexedDB in the browser. One object store for check-ins:
  `{ id, face: "happy" | "neutral" | "sad", timestamp (ISO 8601),
  audio: Blob | null, audioType: string | null }`.
- Audio: `navigator.mediaDevices.getUserMedia` + `MediaRecorder`. iOS
  Safari records `audio/mp4`; pick the type with
  `MediaRecorder.isTypeSupported` rather than hard-coding `audio/webm`.
  Release the microphone (stop all tracks) as soon as recording ends.
- Must work in current iOS Safari on iPhone. Portrait layout, safe-area
  insets respected, no horizontal scrolling, no zoom on tap.
- Must be served over HTTPS (microphone requires a secure context).
  Hosting: a static host only (GitHub Pages, AWS Amplify Hosting, or
  S3 + CloudFront). No backend.
- Comment the JavaScript clearly. I'm learning, so explain the why, not
  just the what, especially for IndexedDB, MediaRecorder, and the iOS
  quirks.

## Privacy (non-negotiable)
- Nothing leaves the device. No network requests after the page loads: no
  analytics, no logging services, no fonts, no APIs, no cloud storage.
- No accounts, no login, no cookies, no identifiers.
- Recordings live only in this browser's IndexedDB on his phone.

## Explicitly out of scope (do not add, do not decide)
These are open questions I'm deciding separately. Do not implement or
choose defaults for them:
- Asking "why" for happy or neutral.
- Reviewing data from my own phone, syncing, or exporting.
- Deleting or aging out clips.
- Any weekly summary, trends, insights, or "mood score."
- Notifications or reminders inside the page (the iOS Shortcut handles
  prompting).
- Home Screen / PWA install, service workers, offline caching. (On iOS,
  Home Screen web apps have separate storage from Safari, and the Shortcut
  opens Safari, so the MVP stays Safari-only.)

## Deliverables
1. `index.html` meeting everything above.
2. `README.md` with: how to test locally (`python3 -m http.server`, then
   `localhost:8000`), how to deploy to the chosen static host, and how to
   set up the iPhone Shortcuts automation (Time of Day → Open URLs → Run
   Immediately).
3. A short manual test checklist I can run on his iPhone.
