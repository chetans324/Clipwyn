# Clipwyn by Braivex

Turn long videos and live streams into captioned vertical clips, ready to post. A private AI clipping studio by **Braivex**.

Paste a YouTube, Twitch or Kick link (or upload a file) and Clipwyn finds the moments worth clipping, cuts them to 9:16, adds word-by-word kinetic captions and a hook, and can post them to your connected accounts. It can also record a creator's live stream and clip the highlights while the stream is still running.

## What it does

- **Neural Hook Detection.** Transcribes the audio locally, uses AI to analyze engagement and drop-off triggers, and renders each clip with dynamic captions.
- **Live Stream Clipping.** Records Twitch, Kick, or YouTube streams into a rolling buffer and clips as it happens, using audio energy, chat activity, and transcripts.
- **Creator Workspaces.** Dedicated channels, clip settings, posting rules, and activity telemetry for each creator.
- **Direct Multi-Platform Auto-Posting.** Connect YouTube, Instagram, and TikTok in one click, then queue or auto-post finished clips with spacing and daily caps.
- **Private Studio Governance.** Strict admin-gated access control. New registrations are placed into an approval queue, and only admins configure institutional AI access.
- **Local Speech-to-Text.** Audio is transcribed directly on the server with `sherpa-onnx` and the Parakeet model. Raw audio never leaves your private cloud instance.

## Quick start

You need Python 3.10 or newer and `ffmpeg`.
- On a Mac: `brew install ffmpeg`
- On Linux (Ubuntu/Debian): `sudo apt install ffmpeg`
- On Windows: `winget install Gyan.FFmpeg`

Clipwyn detects `ffmpeg` automatically and informs you in the dashboard if it is missing.

Clone and install:

```bash
git clone https://github.com/chetans324/Clipwyn.git ~/clipwyn
cd ~/clipwyn
pip install -r requirements.txt
```

Start the studio server:

```bash
python -m studio.server
```

The first start downloads the speech recognition model automatically (~480 MB, once).

Open `http://127.0.0.1:8765`. With accounts enabled (`PEAKCUT_ACCOUNTS=1`), sign up with your email, and grant your account admin rights via the server terminal:

```bash
python -m studio.admin grant you@example.com
```

Sign in as admin, navigate to **Settings > AI**, choose your AI service (e.g. Gemini), enter your key, and press **Save and test**.

## Docker & Server Deployment

Clipwyn is optimized for single-server production deployment with Docker: the studio application, PostgreSQL database, and Caddy (automatic HTTPS/SSL).

```bash
cp .env.example .env
nano .env
```

Set your domain and database credentials:

```ini
PEAKCUT_DOMAIN=clipwyn.braivex.com
POSTGRES_PASSWORD=your_secure_password
COMPOSE_PROFILES=own-https
PEAKCUT_ACCOUNTS=1
```

Launch the stack:

```bash
docker compose up -d --build
```

Bootstrap your admin account:

```bash
docker compose exec app python -m studio.admin grant you@braivex.com
```

See [DEPLOY.md](DEPLOY.md) for full server setup and Cloudflare DNS instructions.

## Architecture

| Component | Responsibility |
|---|---|
| `studio/server.py` | FastAPI application, security gate, rate limiting, and route handlers |
| `studio/auth.py` | Scrypt password hashing, session tokens, and admin-gated access logic |
| `studio/admin.py` | Admin governance, pending access requests queue, and telemetry |
| `studio/jobs.py` | Video processing pipeline, background worker orchestration, and fair queuing |
| `studio/live.py` | Real-time live stream recording buffer and highlight cutter |
| `studio/transcribe.py` | Local on-server speech-to-text with sherpa-onnx (Parakeet model) |
| `studio/render.py` | Video reframing (9:16) and kinetic subtitle rendering via FFmpeg and Pillow |
| `web/` | Dark studio UI/UX: landing page, auth, core studio, and admin dashboard (Vanilla HTML/CSS/JS) |

## License

Maintained by **Braivex**. Caption fonts in `studio/fonts/` (Anton, Archivo Black, Poppins) are under the SIL Open Font License.
