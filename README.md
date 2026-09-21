# Eleet Proxy

A small personal remote browser/proxy experiment.

## Architecture

GitHub → jsDelivr → `index.html` → Cloudflare Tunnel → local Python server → internet

## Requirements

- Windows
- Python 3.10+
- Git
- `cloudflared`

## 1. Start the backend

Double-click:

`start.bat`

It creates a virtual environment, installs dependencies, starts the backend, and checks:

`http://127.0.0.1:8080/health`

## 2. Start Cloudflare

Open a second Command Prompt in this folder:

```bat
cloudflared tunnel --url http://127.0.0.1:8080
```

Copy the HTTPS `trycloudflare.com` URL it prints.

Quick Tunnels are temporary. If the URL changes, update `PROXY_URL` in `index.html`.

## 3. Put the frontend on GitHub

Create a GitHub repository and upload:

```text
index.html
assets/
README.md
```

Then the frontend can be retrieved with:

```text
https://cdn.jsdelivr.net/gh/USERNAME/REPOSITORY@main/index.html
```

## 4. Important CORS note

The backend defaults to allowing the `https://cdn.jsdelivr.net` origin. If you serve the page from another origin, set the environment variable before starting the server:

```bat
set ALLOWED_ORIGINS=https://cdn.jsdelivr.net
```

For testing locally, you can include your local origin too:

```bat
set ALLOWED_ORIGINS=https://cdn.jsdelivr.net,http://localhost:8000
```

## 5. Test in order

1. `http://127.0.0.1:8080/health`
2. `https://YOUR-TUNNEL.trycloudflare.com/health`
3. jsDelivr `index.html`
4. Click `Test Cloudflare`
5. Try a normal public website

## Limitations

This is a lightweight HTTP proxy, not a complete Chromium browser. Complex sites using WebSockets, DRM, strict CSP, anti-bot systems, authentication flows, or highly dynamic JavaScript may not work correctly.

The backend blocks localhost, private IP ranges, link-local addresses, and cloud metadata-style destinations to reduce SSRF risk.

Do not expose the backend directly to the public internet without authentication/access controls.
