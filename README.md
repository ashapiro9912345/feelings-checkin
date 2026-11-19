# Feelings Check-in

A tiny, private web page where a child can share how he feels by tapping one
of three faces. Tapping the sad face also records a short video clip (up to ten
seconds) automatically. A parent can open the read-only log on the same phone
and in the same Safari browser.

This is a family support tool, not a medical or diagnostic tool. It does not
score, interpret, summarize, or send check-ins anywhere.

## How the app stores data

All check-ins and clips (video, or audio for older or camera-less check-ins)
live in Safari's IndexedDB storage on the device. There is no backend,
account, cookie, analytics service, or cloud database. The page makes no
network requests after it has loaded.

Because storage belongs to one browser and one website origin:

- The parent log must be opened in Safari on the same iPhone.
- Localhost data and GitHub Pages data are separate.
- Private Browsing data is separate and may be discarded.
- Clearing Safari website data removes the check-ins and clips.

On load the page asks Safari to keep its storage (`navigator.storage.persist()`)
so iOS is less likely to clear it when space runs low. This is a request, not
a guarantee.

## Upgrading from v1.0 (custom face photos)

Version 1.1 removes the custom face photos. The first time the new page opens
on his phone, it deletes the stored photos. Check-ins and voice clips are kept,
and every log row shows the standard emoji.

If you want copies of the photos, open `recovery.html` on his phone and save a
backup **before** publishing v1.1. After the new page has opened once, the
recovery page can no longer read the database.

## Allow the camera and microphone (one time)

The sad face starts recording without another tap, so Safari must already be
allowed to use the camera and microphone for the site. Otherwise Safari shows
a permission popup, which he would have to read.

1. Open the GitHub Pages URL in Safari on his iPhone.
2. Tap **aA** in the address bar, then **Website Settings**.
3. Set **Camera** to **Allow** and **Microphone** to **Allow**.

If the camera is denied or unavailable, the app records audio only. If the
microphone is denied too, the sad check-in is still saved with no clip.

## Run locally

There is no build or install step. From this directory, run:

```sh
python3 -m http.server 8000
```

Then open:

- Child screen: <http://localhost:8000>
- Parent log: <http://localhost:8000/#log>

Browsers treat `localhost` as a secure context, so camera and microphone
testing works on the computer running the server. An iPhone opening the
computer's LAN address does not normally get that exception; use the HTTPS
GitHub Pages URL for the full iPhone recording test.

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
check-ins or recordings stored by Safari.

## Set up the iPhone Shortcut automation

Create one automation for each time of day when the page should open:

1. Open **Shortcuts** and select **Automation**.
2. Create a personal automation and choose **Time of Day**.
3. Set the time and the days when it should run.
4. Add a **URL** action containing the GitHub Pages child-screen URL. Do not
   add `#log`.
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
- **Database version:** v1.1 opens the database at version 5. The version bump
  is what lets the upgrade step delete the old photo stores. It is 5, not 4,
  because `recovery.html` may already have moved the database to 4, and
  opening with a lower version than the database has fails.
- **Recording on iOS:** After the sad check-in is saved, the app asks for the
  front camera (about 640x480) plus the microphone, falling back to audio only
  if the camera is unavailable. The camera feed is never shown on screen.
  `MediaRecorder.isTypeSupported` picks the format (`video/mp4` on iOS
  Safari), and the bitrate is kept low so ten seconds is about 1 MB.
  Recording stops after ten seconds or when he taps All done, and every
  camera and microphone track is stopped immediately afterward.
- **No speech:** The app never talks. The sad screen is a large 😢 and one
  💛 All done button, so no reading is needed.

## Version label

The small label in the bottom-right corner of every screen (for example
`v1.1`) comes from `APP_VERSION` near the top of the script in `index.html`.
Bump it whenever you publish a change. If his phone shows an older number
than you expect, Safari has a cached copy: close the tab, wait up to ten
minutes for GitHub Pages' cache, and open the URL again.

## Manual testing

Follow [`TESTING.md`](TESTING.md) on the target iPhone. There is no automated
test runner, linter, package manager, or build system in this MVP.
