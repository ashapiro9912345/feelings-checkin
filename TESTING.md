# Feelings Check-in — iPhone manual test checklist

Run the final acceptance check in current iOS Safari using the HTTPS GitHub
Pages URL. Use the local URLs in `README.md` for preliminary desktop testing.

## Before testing

- [ ] Use normal Safari, not a Home Screen web app or Private Browsing.
- [ ] Confirm the child URL does not end in `#log`.
- [ ] Keep the phone in portrait orientation.
- [ ] In Safari, tap **aA → Website Settings** for the site and set **Camera**
      and **Microphone** to **Allow** (see `README.md`).

## Upgrade from v1.0 (only once, on a phone that had custom photos)

- [ ] If you want the photos, save a backup with `recovery.html` first.
- [ ] Open the v1.1 child URL. Confirm the three faces are the standard emoji
      😊 😐 😢 with no photos.
- [ ] Open `#log`. Confirm older check-ins are still listed, now with emoji,
      and older voice clips still have ▶ Play and still play.
- [ ] Open the old `#faces` URL. Confirm it simply shows the child screen.

## Child screen

- [ ] The page shows “How are you feeling?” and three large emoji faces.
- [ ] All three faces fit without horizontal scrolling.
- [ ] Each face is easy to tap with a thumb and Safari does not zoom on a tap.
- [ ] With VoiceOver enabled, the buttons are announced as Happy, Neutral,
      and Sad.
- [ ] There is no link to the parent log.
- [ ] Nothing is ever spoken aloud.

## Happy and neutral

- [ ] Tap Happy. The selected face and “Thanks for telling me!” respond
      immediately, then the three faces return after a few seconds.
- [ ] Tap Neutral and confirm the same warm response and return.
- [ ] Rapidly tap one face several times. Only one new entry should appear in
      the parent log.

## Sad: automatic video recording

- [ ] Tap Sad. The tap receives the same immediate warm response.
- [ ] The next screen shows a large 😢, one big 💛 All done button, and the
      calm “I’m listening” indicator. There is no question text, no speech,
      no countdown, and he does not see himself.
- [ ] The green camera dot appears at the top of the screen without any
      further tap and without a permission popup.
- [ ] Make a face and speak, then wait. After about ten seconds recording stops
      on its own, the green dot goes away, a warm thank-you appears, and the
      face screen returns.
- [ ] Tap Sad again and tap All done after two or three seconds. Recording
      stops right away and the same thank-you appears.
- [ ] Tap Sad and quickly double-tap All done. Only one thank-you appears and
      the clip still saves.
- [ ] In the parent log, both sad entries show a video player. Each video shows
      his face and plays its sound, and plays inline without forcing full screen.

## Permission and failure behavior

- [ ] Set **Camera** to **Deny** (keep Microphone on Allow) and tap Sad.
      Confirm an audio-only clip is saved: the log shows ▶ Play, not a video.
- [ ] Set **Microphone** to **Deny** as well and tap Sad. Confirm the sad
      check-in still appears in the log with no clip, and the child sees the
      same warm thank-you and no technical error.
- [ ] Set both back to **Allow** when done.

## Parent log and persistence

- [ ] Open the same site with `#log` appended to its URL.
- [ ] Entries are newest first and show the emoji face plus the local time.
- [ ] Entries from different calendar days are grouped under day headings.
- [ ] Only entries with clips show a video player (or ▶ Play for audio).
- [ ] The log has no delete, export, chart, summary, score, count, or streak.
- [ ] Reload Safari and confirm the saved check-ins remain.
- [ ] Close and reopen Safari, return to the same `#log` URL, and confirm the
      saved check-ins and clips remain and still play.
- [ ] Optional (needs a Mac with Safari's Develop menu): inspect the page and
      confirm the console shows “Persistent storage granted” or “not granted”,
      with no errors either way.

## Shortcut

- [ ] Run the Time of Day automation manually while the iPhone is unlocked.
- [ ] Safari opens the GitHub Pages child URL, not the `#log` URL.
- [ ] The three-face screen is ready for one-tap use.
