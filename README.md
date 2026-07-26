# 🍺 Sav Happy Hour

A shared, live-updating happy hour tracker for you and your friends. Add deals, edit them, browse by map or list — everyone sees the same tab, updated in real time. Built as a single self-contained HTML file, no build step, hosted free on GitHub Pages.

Started as a Savannah, GA project (hence the name), but works anywhere — see [Using it in another city](#using-it-in-another-city) below.

## Features

- **Live sync** — Firestore keeps everyone's view in sync. Add a deal on your phone, your friends see it appear instantly.
- **List and map view** — browse deals as cards, or see them pinned on an interactive map (OpenStreetMap/Leaflet, no API key needed).
- **Search** — filter by venue name, neighborhood, notes, or any drink/food item.
- **Filters** — Pouring now, Open now, All deals, or a specific day of the week.
- **Sort** — A–Z, Starting soon, Newest, or Near me (uses your device's location).
- **Favorites** — star your go-to spots; they float to the top of any sort.
- **Multi-day deals** — a single venue can have different specials on different days (e.g. Taco Tuesday vs. Whiskey Wednesday), each shown in its own block.
- **Structured operating hours** — separate from happy hour times, so you can see if a place is open at all right now, with an "Open now" badge and filter.
- **Cash-only badge** — flag venues that don't take cards.
- **Directions, Website, Instagram, Facebook** — quick links on every listing.
- **Share** — send a deal to a group chat via your device's native share sheet (or copies plain text on desktop).
- **City filter** — the app supports tracking more than one city; a city dropdown appears automatically once there's more than one in use.
- **Group by area** — optional neighborhood section headers in list view.
- **Surprise me** — jumps to a random spot that's pouring right now.
- **Install as an app** — has a manifest, icons, and install support for the home screen on iOS/Android (via the browser's "Add to Home Screen" / "Install app").
- **Cash- and edit-safe** — no login required; anyone with the link can add, edit, or delete deals (fine for a private friend-group link; see [Security note](#security-note) if you want to lock it down further).

## Setup

### 1. Create a Firebase project

1. Go to [console.firebase.google.com](https://console.firebase.google.com) and create a new project (the free "Spark" plan is enough).
2. Go to **Build → Firestore Database → Create database** and start it in **test mode**.
3. Go to **Project settings → General → Your apps → Web app (`</>`)**, register a new app, and copy the `firebaseConfig` object it gives you.

### 2. Add your config to the app

Open `happy-hour-tab.html` and find this block near the top of the `<script>` section:

```js
const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};
```

Replace the values with the ones from your Firebase project. (Note: Firebase web API keys are meant to be public — see [Security note](#security-note).)

### 3. Deploy to GitHub Pages

1. Push `happy-hour-tab.html` to a GitHub repo — name it `index.html` if you want it at your repo's root URL (e.g. `https://yourname.github.io/repo-name/`) instead of a longer path.
2. In the repo, go to **Settings → Pages**, set the source to your branch and root folder, and save.
3. Share the resulting URL with your friends.

## Using it in another city

The app doesn't hardcode a location beyond an initial map center. To add a second city:

1. Add a deal with a full street address including city and state (e.g. `123 Main St, Asheville, NC`).
2. A **City** filter dropdown will automatically appear in the header once more than one city is in use — it derives city/state from each venue's address, so no manual tagging is required.
3. Use the 🏠 button on the map to zoom to all pins in whichever city is currently selected.

## Security note

Firebase web API keys are not secrets — Google's own docs confirm this. Access control is enforced by **Firestore Security Rules**, not by hiding the key. The default rule used here (`allow read, write: if true`) means anyone with your Firebase config (which is public in this file) can read and write the `deals` collection — fine for a small private link shared with friends, but worth knowing. If you want to tighten it (e.g. restrict to specific fields, add basic auth), that's a Firestore Rules change, not a code change.

## Tech

- Vanilla JS, no framework, no build step — one HTML file.
- [Firebase Firestore](https://firebase.google.com/docs/firestore) (compat SDK via CDN) for shared, real-time data.
- [Leaflet](https://leafletjs.com/) + OpenStreetMap tiles for the map.
- [Nominatim](https://nominatim.org/) (OpenStreetMap's free geocoding service) to turn addresses into map pins automatically when you save a deal.
- Google Fonts (Fraunces, Inter, IBM Plex Mono) via CDN.
- Data persists in Firestore; each person's name and favorites are stored locally in their own browser (`localStorage`), not shared.

## Notes

- Happy hour menus change often — treat any starter/seed data as a starting point, not gospel.
- The app was originally seeded with real Savannah, GA venues and their published happy hour info as of mid-2026. That data lives in the `SAVANNAH_STARTER_DEALS` constant near the top of the script, kept for reference even though the sync/import feature that used it has since been removed (it risked overwriting hand-edited entries).
