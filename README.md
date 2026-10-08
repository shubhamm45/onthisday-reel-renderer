# OnThisDay Reel Renderer

Renders 1080x1920 Instagram Reels for the **On This Day** history channel via GitHub Actions.
Takes a render payload (background image/video URL + headline + caption cards) and produces
an MP4 with branded overlays, published to GitHub Pages.

## How it works

1. **Dispatch**: POST to `https://api.github.com/repos/shubhamm45/onthisday-reel-renderer/actions/workflows/render-reel.yml/dispatches`
   with `request_id` (unique) and `render_json` (the render payload).
2. **Render**: The Action downloads the input, runs ffmpeg with the OnThisDay brand treatment
   (brand lockup, event date badge, headline, timed caption cards, source credit).
3. **Publish**: MP4 + cover JPG go to `https://shubhamm45.github.io/onthisday-reel-renderer/reels/<request_id>/`,
   manifest at `.../requests/<request_id>.json` with `status: "ready"`.

## Render payload schema

```json
{
  "reelVideoUrl": "https://.../image.png",
  "inputKind": "image",
  "targetDurationSec": 15,
  "headline": "On October 3, 1985: ...",
  "captionCards": ["card 1 text", "card 2 text"],
  "instagramCaption": "...",
  "sourceTitle": "...",
  "sourceUrl": "https://...",
  "sourcePublisher": "Wikipedia",
  "eventDate": "OCTOBER 3",
  "brandName": "ON THIS DAY",
  "backgroundMusicUrl": "https://... (optional)"
}
```

## Local test

```bash
node render.mjs --request-id test-001 --payload sample-payload.json --public public --base-url https://example.com
```
