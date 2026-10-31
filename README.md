# PhonoByte World website

News homepage for [PhonoByte World](https://www.youtube.com/@PhonoByteWorld), served with GitHub Pages. Plain HTML,
CSS and a little JavaScript; no build step.

The front page shows the channel's latest videos as news stories (top story, latest list, more stories with
hashtag topics); a video plays in the page when clicked. Until the channel has public videos, it shows a
"first stories are on the way" card.

| File | Purpose |
| --- | --- |
| `index.html` | front page |
| `privacy.html`, `terms.html` | privacy policy (also used for the Google OAuth consent screen) and terms |
| `data/videos.json` | latest videos, written by the workflow below |
| `.github/workflows/update-videos.yml` | every 30 minutes reads the channel's public YouTube feed and commits `data/videos.json` when it changed |
| `assets/` | stylesheet, logo, favicon, social preview image |

## Publish

1. Push everything (including the hidden `.github/` folder and `.nojekyll`) to the `phonobyteworld.github.io` repo in the `phonobyteworld` organization.
2. Settings → Pages → Source: **Deploy from a branch**, branch `main`, folder `/ (root)` → Save.
3. Settings → Actions → General → Workflow permissions: **Read and write permissions** → Save.
4. Actions → "Update latest videos" → **Run workflow** once to check it works.

The site is then at `https://phonobyteworld.github.io/`. New YouTube uploads appear within about 30 minutes.

GitHub pauses scheduled workflows after 60 days without commits to the repo; if the channel goes quiet that long,
re-enable it under Actions.

## Google OAuth consent screen (Branding page)

- Application home page: `https://phonobyteworld.github.io/`
- Application privacy policy link: `https://phonobyteworld.github.io/privacy.html`
- Application terms of service link: `https://phonobyteworld.github.io/terms.html`
- Authorized domains: `phonobyteworld.github.io`
