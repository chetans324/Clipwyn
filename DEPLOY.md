# Deploying Clipwyn by Braivex

Clipwyn runs as three isolated containers on a single Linux server: the studio app, PostgreSQL, and Caddy (which handles automatic HTTPS/SSL).

## Server Requirements

- A Linux server with Docker (e.g. Google Cloud Compute Engine `e2-standard-2`, Ubuntu 24.04 LTS, 100 GB SSD).
- Clipping is CPU and RAM intensive: plan on **2+ vCPUs, 8+ GB RAM, and 80+ GB disk**.
- A domain name pointing to the server (e.g. `clipwyn.braivex.com` with an `A` record on Cloudflare).
- An AI API key (Google Gemini 2.5 recommended).

## Deployment Steps

1. Clone repository to your server:
   ```bash
   git clone https://github.com/chetans324/Clipwyn.git ~/clipwyn
   cd ~/clipwyn
   ```

2. Copy and configure the environment:
   ```bash
   cp .env.example .env
   nano .env
   ```
   Configure these key variables:
   ```ini
   PEAKCUT_DOMAIN=clipwyn.braivex.com
   POSTGRES_PASSWORD=your_secure_password
   COMPOSE_PROFILES=own-https
   PEAKCUT_ACCOUNTS=1
   ```

3. Launch the container stack:
   ```bash
   docker compose up -d --build
   ```
   The first start automatically downloads the speech model (~480 MB) and initializes the PostgreSQL tables.

4. Open `https://clipwyn.braivex.com` in your browser and register your administrator account.

5. Bootstrap your admin role via the server terminal:
   ```bash
   docker compose exec app python -m studio.admin grant you@braivex.com
   ```
   The admin governance dashboard is at `https://clipwyn.braivex.com/admin`.

6. Sign in, open **Settings > AI**, and save your Gemini API key (or specify `PEAKCUT_AI_KEY` in `.env`).

## Connecting Social Platforms (YouTube, Instagram, TikTok)

Users connect their accounts in **Settings > Connected apps**. To enable this, register developer apps with the platforms using these redirect URIs:

- **YouTube** (Google Cloud Console):
  - `https://clipwyn.braivex.com/auth/google/callback`
  - `https://clipwyn.braivex.com/oauth/youtube/callback`
- **Instagram** (Meta for Developers):
  - `https://clipwyn.braivex.com/oauth/instagram/callback`
- **TikTok** (TikTok for Developers):
  - `https://clipwyn.braivex.com/oauth/tiktok/callback`

## Cloudflare CDN Configuration

When proxying `clipwyn.braivex.com` through Cloudflare (Orange Cloud):
- Set `PEAKCUT_CLIENT_IP_HEADER=CF-Connecting-IP` in `.env` so rate limiters track the actual visitor IP.
- Large uploads are automatically chunked in 40 MB pieces to comply with Cloudflare's upload limits.

## Updating

To pull updates and restart the service:
```bash
git pull && docker compose up -d --build
```
Your persistent data, uploaded videos, and database records remain intact inside Docker volumes.
