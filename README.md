# ClipPortal connect pages

TikTok only sends people back to an `https://` address after they sign in, but ClipPortal runs on your own computer
at `http://localhost:8765`. `tiktok/index.html` is a tiny static page that receives TikTok's answer at an https
address and passes it straight on to ClipPortal on the computer you signed in from.

It is published on its own, in a separate public repository with GitHub Pages, so the ClipPortal code itself stays
private. The page holds no keys, tokens or account details. It forwards only TikTok's `code`, `state` and error
fields, checked against a fixed pattern, and only to `http://localhost:8765`. It loads nothing else (a strict
Content-Security-Policy), sends no referrer, and removes the code from the address bar.

The sign-in code is single-use, expires within minutes, and is worthless without the app's secret, which only
ClipPortal on your computer holds. ClipPortal also refuses any answer to a sign-in it did not start.

To use it, set in ClipPortal's `.env`:

```ini
PEAKCUT_TIKTOK_REDIRECT_URL=https://<your-github-name>.github.io/<repository>/tiktok/
```

and register that same address as the redirect URI of your TikTok app. After changing the script, update the
`sha256-` value in the page's Content-Security-Policy, or the browser will refuse to run it.
