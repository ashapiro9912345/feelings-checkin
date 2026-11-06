# Feelings Check-in

A tiny, private web page where a child can share how he feels by tapping one
of three faces. Sad check-ins offer an optional voice recording. A parent can
open the read-only log on the same phone and in the same Safari browser.

This is a family support tool, not a medical or diagnostic tool. It does not
score, interpret, summarize, or send check-ins anywhere.

## How the app stores data

All check-ins, voice clips, and custom face photos live in Safari's IndexedDB
storage on the device. There is no backend, account, cookie, analytics service,
or cloud database. The page makes no network requests after it has loaded.

Because storage belongs to one browser and one website origin:

- The parent log must be opened in Safari on the same iPhone.
- Localhost data and GitHub Pages data are separate.
- Private Browsing data is separate and may be discarded.
- Clearing Safari website data removes the check-ins, clips, and face photos.

## Choose your own face photos

Open the site in Safari on the child's iPhone and add `#faces` to the URL. For
example: `https://YOUR-USERNAME.github.io/REPOSITORY-NAME/#faces`.

There is one photo slot for each feeling: happy, neutral, and sad. Tap **Take or
choose photo** and use iOS's picker to take a new picture or select one from the
photo library. The app center-crops the photo, resizes it, removes its camera
metadata, and saves the result only in this browser. **Use emoji** removes that
custom photo and restores the original face.

The child buttons and parent log both use the currently configured photo for
each feeling. Replacing a photo updates how earlier log entries are displayed;
it does not change their stored feeling, timestamp, or recording.

The setup page is intentionally not linked from the child screen. Configure the
photos using the same Safari browser and exact site address that the Shortcut
will open; browser storage is separate for every site address.

## Run locally

There is no build or install step. From this directory, run:

```sh
python3 -m http.server 8000
```

Then open:

- Child screen: <http://localhost:8000>
- Parent log: <http://localhost:8000/#log>
- Face photo setup: <http://localhost:8000/#faces>

Browsers treat `localhost` as a secure context, so microphone testing works on
the computer running the server. An iPhone opening the computer's LAN address
does not normally get that exception; use the HTTPS GitHub Pages URL for the
full iPhone microphone test.

Stop the local server with `Control-C`.

## Deploy with GitHub Pages

Do not put any real check-in data in the repository. The repository contains
only the static application; browser data remains on the iPhone.

1. Create a GitHub repository and push these project files to its `main`
   branch.
2. On GitHub, open the repository and go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Choose the `main` branch and the `/(root)` folder, then select **Save**.
5. Wait for GitHub Pages to report the published HTTPS URL. It is normally
   `https://YOUR-USERNAME.github.io/REPOSITORY-NAME/`.
6. Open that URL in Safari on the child's iPhone and complete the checks in
   `TESTING.md` before setting up the automation.

GitHub's current instructions are in
[Configuring a publishing source for your GitHub Pages site](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).
GitHub Pages publishes the application source publicly; it does not publish the
check-ins, recordings, or face photos stored by Safari.

## Set up the iPhone Shortcut automation

Create one automation for each time of day when the page should open:

1. Open **Shortcuts** and select **Automation**.
2. Create a personal automation and choose **Time of Day**.
3. Set the time and the days when it should run.
4. Add a **URL** action containing the GitHub Pages child-screen URL. Do not
   add `#log` or `#faces`.
5. Add **Open URLs** after the URL action.
6. Choose **Run Immediately** and save the automation. On iOS versions that
   show **Ask Before Running** instead, turn that option off.
7. Run the automation once while the phone is unlocked and confirm that it
   opens the three-face screen in Safari.

Apple documents personal automations in the
[Shortcuts User Guide](https://support.apple.com/guide/shortcuts/create-a-new-personal-automation-apdfbdbd7123/ios).

Keep the parent log URL private from the child screen. To review the log,
manually add `#log` to the end of the same GitHub Pages URL in Safari.

## Browser behavior worth knowing

- **IndexedDB:** The app wraps IndexedDB's event-based operations in promises
  so each save can be awaited. If storage is unavailable, details go only to
  the console and the child still receives the normal warm response.
- **Face photos:** Check-ins and custom photos use separate IndexedDB object
  stores, so replacing a photo never changes the check-in history. Camera-sized
  images are center-cropped and reduced to at most 720 by 720 pixels before
  storage.
- **Speech on iOS:** Safari can require speech synthesis to begin from a user
  gesture. The sad tap primes the speech engine silently; the audible prompt
  is spoken only after the sad check-in save attempt finishes. If speech is
  unavailable, the prompt remains visible.
- **Recording on iOS:** The app asks `MediaRecorder.isTypeSupported` which
  format to use, preferring `audio/mp4` for iOS Safari. Recording stops after
  ten seconds or when the child taps Stop. Every microphone track is stopped
  immediately afterward.

## Manual testing

Follow [`TESTING.md`](TESTING.md) on the target iPhone. There is no automated
test runner, linter, package manager, or build system in this MVP.
