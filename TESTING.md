# Feelings Check-in — iPhone manual test checklist

Run the final acceptance check in current iOS Safari using the HTTPS GitHub
Pages URL. Use the local URLs in `README.md` for preliminary desktop testing.

## Before testing

- [ ] Use normal Safari, not a Home Screen web app or Private Browsing.
- [ ] Confirm the child URL does not end in `#log` or `#faces`.
- [ ] Keep the phone in portrait orientation.
- [ ] For the first recording test, be ready to allow microphone access.

## Face photo setup

- [ ] Open the same site with `#faces` appended to its URL.
- [ ] For Happy, tap **Take or choose photo**, take a new picture, and confirm
      the preview changes from the emoji to the photo.
- [ ] Choose separate pictures for Neutral and Sad and confirm all three
      previews remain distinct.
- [ ] Return to the child URL without `#faces`. Confirm the three large buttons
      show the custom photos and still fit without horizontal scrolling.
- [ ] Reload Safari, then close and reopen it. Confirm all three photos persist.
- [ ] Return to `#faces`, replace one photo, and confirm only that feeling
      changes on the child screen.
- [ ] Tap **Use emoji** for one feeling and confirm only that face returns to
      its original emoji.
- [ ] Confirm existing entries and recordings in `#log` were not changed by
      replacing or resetting photos.

## Child screen

- [ ] The page shows “How are you feeling?” and three large faces.
- [ ] Configured photos appear; any feeling without a photo uses its emoji.
- [ ] All three faces fit without horizontal scrolling.
- [ ] Each face is easy to tap with a thumb and Safari does not zoom on a tap.
- [ ] With VoiceOver enabled, the buttons are announced as Happy, Neutral,
      and Sad.
- [ ] There is no link to the parent log.

## Happy and neutral

- [ ] Tap Happy. The selected face and “Thanks for telling me!” respond
      immediately, then the three faces return after a few seconds.
- [ ] Tap Neutral and confirm the same warm response and return.
- [ ] Rapidly tap one face several times. Only one new entry should appear in
      the parent log.

## Sad without a recording

- [ ] Tap Sad. The tap receives the same immediate warm response.
- [ ] “Tell us why you’re sad” appears and is spoken aloud.
- [ ] Record and “No thanks” are both large and equally prominent.
- [ ] Tap “No thanks.” A warm thank-you appears without another prompt, then
      the face screen returns.
- [ ] Open the parent log and confirm that the sad check-in was saved even
      though no recording was made.

## Sad with a recording

- [ ] Tap Sad, then Record, and allow microphone access if Safari asks.
- [ ] A calm listening indicator appears with no countdown or progress timer.
- [ ] Speak briefly, then tap Stop. Confirm the microphone indicator turns off,
      a warm thank-you appears, and the face screen returns.
- [ ] In the parent log, tap Play on that sad entry and confirm the clip plays.
- [ ] Make another sad recording without tapping Stop. Confirm it ends on its
      own after about ten seconds without showing a countdown, then saves.

## Permission and failure behavior

- [ ] Reset the site's microphone permission if necessary, start a sad
      recording, and choose **Don't Allow** at Safari's permission prompt.
- [ ] Confirm the already-saved sad check-in remains in the parent log.
- [ ] Confirm the child sees the same warm thank-you and no technical error.
- [ ] If speech is muted or unavailable, confirm the written sad prompt still
      appears and the rest of the flow works.

## Parent log and persistence

- [ ] Open the same site with `#log` appended to its URL.
- [ ] Entries are newest first and show a face and local time.
- [ ] Entries from different calendar days are grouped under day headings.
- [ ] Only entries with recordings show a Play button.
- [ ] The log has no delete, export, chart, summary, score, count, or streak.
- [ ] Reload Safari and confirm the saved check-ins remain.
- [ ] Close and reopen Safari, return to the same `#log` URL, and confirm the
      saved check-ins and recordings remain and still play.

## Shortcut

- [ ] Run the Time of Day automation manually while the iPhone is unlocked.
- [ ] Safari opens the GitHub Pages child URL, not the `#log` or `#faces` URL.
- [ ] The three-face screen is ready for one-tap use.
