# Long / Short

A phone scorekeeper and payout ledger for the golf betting game **Long/Short**. It runs entirely in the browser, works offline once installed, and saves the game on your device so an accidental close never loses a round.

Supports **4–8 players** (including odd games with a solo player), **9 or 18 holes**, and stakes of **25¢, 50¢, or $1** per point.

## Files

All files must sit together at the **root** of the repository (not in a subfolder):

- `index.html` — the app
- `manifest.webmanifest` — makes it installable
- `sw.js` — service worker for offline use and self-updating
- `icon-192.png`, `icon-512.png` — app icons
- `apple-touch-icon.png` — home-screen icon for iPhone
- `README.md` — this file

## Publish it (GitHub Pages)

1. Create a **public** repository (Pages is free only on public repos), e.g. `longshort`.
2. **Add file → Upload files**, then drag in all the files. Commit.
3. **Settings → Pages** → Source: **Deploy from a branch**, branch **main**, folder **/ (root)**. Save.
4. Wait about a minute, reload that page, and copy the live URL: `https://<your-username>.github.io/longshort/`.

## Install on your phone

Open the Pages URL on your phone, then:

- **Android (Chrome):** menu → **Install app** (or Add to Home screen). The install option can take a few seconds to appear the first time.
- **iPhone (Safari):** Share → **Add to Home Screen**. Use Safari, not Chrome, on iOS.

It launches full-screen with its own icon and works with no signal. To share it with someone, just send them the same URL — they install it the same way. Each phone keeps its own saved game, so only one phone should keep score for a given match.

## Update to a new version

Re-upload the changed file(s) to the repo (usually just `index.html`). The service worker fetches the latest version the next time the phone has a connection, then keeps serving it offline. If a new version doesn't appear, close the app fully and reopen it while online.

## Notes

- **Saved games survive updates.** Progress lives in the browser's local storage for this URL, separate from the app files, so replacing files does not wipe an in-progress round. Starting a **New round**, or clearing the browser's site data, are the only things that reset it.
- **Filenames are case-sensitive** on GitHub Pages. Keep them exactly as provided (all lowercase).
- The app uses relative links, so it works under any `.../reponame/` path with no changes.

---

## How the game works

### Setting teams
At the start of each **leg**, players are ranked by drive on the **Rank the drives** screen — longest at the top, shortest at the bottom. Teams then form themselves by pairing top with bottom and working inward:

- **Team A** = longest + shortest drive
- **Team B** = 2nd-longest + 2nd-shortest
- **Team C** = 3rd + 3rd … and so on

With an **odd number of players**, the middle-ranked player is a **solo team** of one (Team C in a 5-player game, Team D in a 7-player game).

You can re-open this screen any time before a leg settles (**Edit → Re-rank teams**) to fix a misspelled name or a wrong drive order; the leg's holes and points so far are kept.

### The solo player (odd games)
At the start of each leg you're asked whether the solo player is **in** or **out** for that leg. A solo player has **double exposure** — they represent two players, so they win or lose twice as much. If they sit out, their team is greyed out and can't score that leg.

### Scoring each hole
Every hole is worth **two points**:

- **Low** — the single lowest score on the hole
- **Total** — the lowest two-person team total

A **tie** on either point awards nothing and **carries** that point to the next hole (so a hole can be worth Low ×2, Total ×3, etc. — the app tracks the running pot for you).

### Par 3s keep teams together
When one hole clears both points at once, the points are "out." The app asks whether the next hole is a **par 3**:

- **Par 3 → keep teams.** Nobody settles, the same teams carry on, and points keep accumulating in the same leg. A leg can run through several par 3s this way.
- **Not a par 3 → settle & continue.** The leg ends, teams settle up, and new teams are ranked off the next drive.

### Settlement is per player
The team amounts shown for a leg (e.g. "C pays A $3") are **per pairing**. In an even game, "Team A owes Team B $1" means each A player pays their counterpart $1, so every player's own result is ±$1. A **solo player covers both pairings**, so their result is **double** the team amount.

Settlement only happens when **new teams are assigned**, when you **end the round early**, or when the **round is over**. Any points still carrying at that moment go unsettled and are forgotten.

### Winnings & the day summary
- **Winnings** (top bar) shows the running per-player totals at any moment during the round.
- At the end of the round (or on **End round**), **Settle & continue** opens the **day summary** — each player's final money won or lost for the day, with the solo doubling already baked in. It always nets to zero.

## Controls quick reference
- **Edit** — change hole count anytime; change the stake until the first leg settles; re-rank the current teams.
- **Winnings** — running per-player money totals.
- **End round** — kill switch to stop early and settle what's been won; forgotten points are dropped.
- **Undo last hole** — fix a mis-tap.
- **Earlier legs** — expand for the hole-by-hole action, team assignments, points, and payouts of every finished leg.
