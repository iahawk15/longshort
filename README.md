# Long / Short

A phone scorekeeper and payout ledger for the golf betting game **Long/Short**. It runs entirely in the browser, works offline once installed, and saves your game on the device so an accidental close never loses a round.

## Files

All six must sit together at the **root** of the repository (not in a subfolder):

- `index.html` — the app
- `manifest.webmanifest` — makes it installable
- `sw.js` — service worker for offline use and self-updating
- `icon-192.png`, `icon-512.png` — app icons
- `apple-touch-icon.png` — home-screen icon for iPhone

## Publish it (GitHub Pages)

1. Create a **public** repository (Pages is free only on public repos), e.g. `longshort`.
2. **Add file → Upload files**, then drag in all six files. Commit.
3. **Settings → Pages** → Source: **Deploy from a branch**, branch **main**, folder **/ (root)**. Save.
4. Wait about a minute, reload that page, and copy the live URL: `https://<your-username>.github.io/longshort/`.

## Install on your phone

Open the Pages URL on your phone, then:

- **Android (Chrome):** menu → **Install app** (or Add to Home screen).
- **iPhone (Safari):** Share → **Add to Home Screen**. Use Safari, not Chrome, on iOS.

It launches full-screen with its own icon and works with no signal.

## Update to a new version

Re-upload the changed file(s) to the repo (usually just `index.html`). The service worker fetches the latest version the next time the phone has a connection, then keeps serving it offline. If a new version doesn't appear, close and reopen the app while online.

## Notes

- **Saved games survive updates.** Progress lives in the browser's local storage for this URL, separate from the app files, so replacing files does not wipe an in-progress round. Starting a "New round" in the app, or clearing the browser's site data, are the only things that reset it.
- **Filenames are case-sensitive** on GitHub Pages. Keep them exactly as provided (all lowercase).
- If you fork or rename the repo, no path changes are needed — the app uses relative links, so it works under any `.../reponame/` path.

## How the game works

Six (or four, or eight) players. Off the first drive, the longest and shortest drivers form Team A, the 2nd-longest and 2nd-shortest form Team B, and so on. Each hole is worth two points: **Low** (lowest single score) and **Total** (lowest two-person team total). A tie on either point awards nothing and carries that point to the next hole.

When a single hole clears both points at once, the points are "out," and what happens next depends on the following hole:

- **If it's a par 3,** the teams stay together (long/short pairings are hard to set on a par 3), nobody settles, and points keep accumulating on the same teams. A leg can run through several par 3s this way.
- **If it isn't,** the leg ends: teams settle up at the chosen stake and new teams are drawn off the next drive.

Settling only ever happens when new teams are assigned, when you end the round early, or when the round is over. Any points still carrying at those moments go unsettled and forgotten.
