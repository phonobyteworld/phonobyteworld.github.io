# PhonoByte World website

News map for [PhonoByte World](https://www.youtube.com/@PhonoByteWorld), served with GitHub Pages. Plain HTML,
CSS and a little JavaScript; no build step.

The front page is a world map ([OpenFreeMap](https://openfreemap.org/) "liberty" vector tiles with English names,
drawn by [MapLibre GL JS](https://maplibre.org/)) with a pin for every video that has a location, and a month calendar
next to it.
Click a day, then another day, to pick a range (or use All time / Last 7 days / Last 30 days / This month); the pins
and the list below the calendar show only the videos published in that range (dates in IST). Clicking a pin opens
the video on YouTube; a pin with several videos at the same place opens a list. The map zooms with the +/− buttons,
pinch, double-click, or Ctrl/⌘ + mouse wheel (two fingers on phones), so the page still scrolls over it. Videos without a location are in the list only.

| File | Purpose |
| --- | --- |
| `index.html` | front page: map, calendar, video list |
| `privacy.html`, `terms.html` | privacy policy (also used for the Google OAuth consent screen) and terms |
| `data/videos.json` | every public video: id, title, link, date, thumbnail, place and coordinates; written by the workflow |
| `data/places.json` | place name → coordinates cache, so each place is geocoded once |
| `.github/workflows/update-videos.yml` | twice a day (09:30 and 21:30 IST) updates the two data files and commits them when they changed |
| `assets/` | stylesheet, logo, favicon, social preview image |

## Where the locations come from

1. A location set in YouTube Studio (video → Show more → Recording date and location). Read only when the
   `YOUTUBE_API_KEY` secret is set.
2. Otherwise a line in the description: `📍 Kharkiv, Ukraine` (or `Location: Kharkiv, Ukraine`). The PhonoByte
   Shorts studio adds it to every upload that has a location. The workflow looks the place up on
   [Nominatim](https://nominatim.org/) (OpenStreetMap) once and keeps the result in `data/places.json`; a name it
   can't find is listed without a pin and tried again on the next run.

To pin an older video, add the `📍` line to its description (or set the Studio location) and run the workflow.

## Where the video list comes from

- With a `YOUTUBE_API_KEY` repository secret (Settings → Secrets and variables → Actions): the YouTube Data API.
  All public uploads, as soon as they are live; deleted or private videos drop off. About 2 quota units a run.
  Create the key in the Google Cloud project that has the YouTube Data API v3 enabled, and restrict it to that API.
- Without it: the public channel feed. No key needed, but YouTube caches it (new uploads can take hours) and it only
  has the latest 15, so the workflow adds new videos to the list and keeps the older ones.

## Publish

1. Push everything (including the hidden `.github/` folder and `.nojekyll`) to the `phonobyteworld.github.io` repo in the `phonobyteworld` organization.
2. Settings → Pages → Source: **Deploy from a branch**, branch `main`, folder `/ (root)` → Save.
3. Settings → Actions → General → Workflow permissions: **Read and write permissions** → Save.
4. Actions → "Update video map" → **Run workflow** once to check it works.

The site is then at `https://phonobyteworld.github.io/`. New uploads appear at the next run (or run the workflow by
hand).

GitHub pauses scheduled workflows after 60 days without commits to the repo; if the channel goes quiet that long,
re-enable it under Actions.

India's border: OpenStreetMap-based maps draw Kashmir and Aksai Chin with the international lines, so the page
shows India's official view:

- `assets/india-border.geojson`, India's official land boundary all the way round (DataMeet's CC0
  `india-composite.geojson`, after the Survey of India, with the coastline removed — points within 4 km of Natural
  Earth's 10 m coastline — and simplified to about 250 m), drawn in the map's own country-border grey.
- Disputed lines are hidden unless they lie wholly outside South Asia (Crimea, Western Sahara etc. still show).
- Below zoom 5 OpenFreeMap's generalised country borders draw the Line of Control as a plain border, so
  `assets/world-borders.geojson` replaces them there: Natural Earth's 50 m land borders (public domain) as India sees
  them (`FCLASS_IN`), without the lines along India's border or inside Jammu, Kashmir and Ladakh.
- Region names like Gilgit-Baltistan and Azad Kashmir are hidden (`NOT_INDIA` in `index.html`).

Map tiles: OpenFreeMap is free with no key and no limits, commercial use included; the attribution is shown bottom
right. If it does not answer within 8 s the page switches to VersaTiles (free, but a rate-limited public server).

## Google OAuth consent screen (Branding page)

- Application home page: `https://phonobyteworld.github.io/`
- Application privacy policy link: `https://phonobyteworld.github.io/privacy.html`
- Application terms of service link: `https://phonobyteworld.github.io/terms.html`
- Authorized domains: `phonobyteworld.github.io`
