# Deesha B Raj — portfolio

Two static pages, no build step, no dependencies.

| File | What it is |
| --- | --- |
| `index.html` | Hero. Webcam hand tracking scrubs a paper-crumple reel; three clips rotate with three chapter quotes. |
| `work.html` | Decisions → Experience → Impact → Selected work → Background → Contact. |
| `*-scrub.mp4` | The three clips, re-encoded so every frame is a keyframe (this is what makes scrubbing instant). |
| `poster*.jpg` | First-frame stills, shown before each clip decodes. |
| `vercel.json` | Tells Vercel this is a static site with no build. |

## Editing

Open the file, change it, commit, push. Vercel redeploys in about a minute.

Common edits:

- **Chapter quotes / clip order** — the `CLIPS` array near the top of the script in `index.html`.
- **Scheduler link** — search `calendly.com` in `work.html`.
- **Adding a fourth clip** — add one line to `CLIPS`, drop the mp4 in the root. Prep it first:
  ```
  ffmpeg -i source.mp4 -x264-params keyint=1:min-keyint=1 -pix_fmt yuv420p -movflags +faststart new-scrub.mp4
  ffmpeg -i source.mp4 -frames:v 1 -q:v 5 poster-new.jpg
  ```
  Each clip must start on its flat frame and end on its tightest ball.

## Requirements

- **HTTPS** — the webcam needs a secure context. Vercel provides this.
- **HTTP Range requests** — without them the reel freezes on frame one. Vercel supports them.

Testing locally: `npx serve`. Do not use `python3 -m http.server` — it ignores Range requests and the scrubbing will look broken when it isn't.

## Behaviour worth knowing

- The reel is always paused. An open hand flattens the paper, a fist crumples it.
- A clip hands over to the next only after being crushed to 100% and returned to flat.
- No camera, or permission denied: the reel falls back to click-to-play, one scene per click.
- `prefers-reduced-motion` skips the scrub entirely and holds the poster.
- Console helper: `__scrunch(0…1)` drives the reel by hand for testing.
