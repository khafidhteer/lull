# lull

Ultra-dim bedtime story reader. A web app designed for reading stories to your child at night — true black background, adjustable text brightness down to nearly invisible, so the phone screen emits minimal light.

## Features

- **True black background** (#000000) — OLED-friendly, near-zero light emission
- **Adjustable text brightness** — 5% to 100% white opacity in 5% steps
- **Adjustable font size** — 14px to 48px in 2px steps
- **Screen wake lock** — prevents phone from sleeping during reading
- **Reading mode overlay** — tap to reveal controls (font, brightness, exit), auto-hides after 3s
- **Persistence** — saves story, brightness, font size, and scroll position to LocalStorage. Close and reopen to pick up where you left off.
- **Single HTML file** — no build step, no dependencies, no backend

## Usage

1. Open the app
2. Paste your story text into the textarea
3. Adjust font size and brightness to your comfort
4. Tap **Start Reading**
5. Tap anywhere on the screen to show/hide controls
6. Tap **New Story** to paste a different story

## Controls

| Control | Button | Range |
|---------|--------|-------|
| Font size | `[−] Aa [++]` | 14–48px (±2px) |
| Text brightness | `[−] ☀ [++]` | 5–100% (±5%) |

## Deployment

### Subdomain (lull.jtr-lab.com)

1. Point DNS `lull.jtr-lab.com` → your VPS IP
2. Copy the nginx config:
   ```bash
   sudo cp nginx.conf /etc/nginx/sites-available/lull
   sudo ln -s /etc/nginx/sites-available/lull /etc/nginx/sites-enabled/ll-lull
   sudo nginx -t && sudo systemctl reload nginx
   ```
3. The wildcard SSL cert for `*.jtr-lab.com` is used automatically.
   For a proper Let's Encrypt cert:
   ```bash
   sudo certbot certonly --manual --preferred-challenges dns -d lull.jtr-lab.com
   ```
   Then update the `ssl_certificate` paths in nginx.conf.

### Direct port access

For quick testing on an available port:

```nginx
server {
    listen 3000;
    root /home/ubuntu/lull;
    index index.html;
    location / { try_files $uri $uri/ /index.html; }
}
```

## Tech

- Vanilla HTML/CSS/JS — no frameworks
- Georgia serif font — optimal for long-form reading
- LocalStorage for persistence
- Screen Wake Lock API to keep screen on
- No cookies, no tracking, no network requests after load

## License

MIT
