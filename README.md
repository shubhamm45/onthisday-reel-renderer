# OnThisDay Reel Renderer

Renders 1080x1920 Instagram Reels for the **On This Day** history channel via GitHub Actions.
SSH-only: no API tokens needed.

## How it works

1. **Request**: Push a JSON file to `render-requests/<request_id>.json` (via git/SSH).
2. **Render**: The Action triggers on the push, downloads the input image/video,
   runs ffmpeg with the OnThisDay brand treatment (brand lockup, event date badge,
   headline, timed caption cards, source credit).
3. **Publish**: MP4 + cover JPG go to `https://shubhamm45.github.io/onthisday-reel-renderer/reels/<request_id>/`,
   manifest at `.../requests/<request_id>.json` with `status: "ready"`.
4. **Poll**: The caller polls the manifest until `status` is `"ready"`, then uses `video_url`.

## Render request schema

`render-requests/<request_id>.json`:
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

`request_id` must be 8-80 chars: letters, digits, hyphens, underscores.

## Local test

```bash
node render.mjs --request-id test-001 --payload sample-payload.json --public public --base-url https://example.com
```
