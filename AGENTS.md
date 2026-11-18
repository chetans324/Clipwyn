# Working on Clipwyn by Braivex

A technical architecture map for contributors and AI engineering agents.

## Core Directives

- Clipwyn is an institutional AI video clipping engine maintained by **Braivex**.
- All user-facing branding and styling must conform to the **Braivex Deep Studio design system** (`#070606` Obsidian, Electric Cyan `#00F0FF`, and Emerald `#10B981`).
- The presentation layer in `web/` must remain pure **Vanilla HTML5, modern CSS3, and JavaScript** with zero NPM build steps.
- Security and admin-gated access control must be strictly preserved.

## Codebase Map

| Component | Path | Description |
|---|---|---|
| **Server & Gate** | `studio/server.py` | FastAPI application, security middleware, upload caps, and route handlers |
| **Configuration** | `studio/config.py` | Environment variable loader, default paths, and data directory resolution |
| **Authentication** | `studio/auth.py` | Salted scrypt password hashing, session tokens, and gated access controls |
| **Governance** | `studio/admin.py` | Admin authorization, user management, and pending access approval queue |
| **AI Engine** | `studio/ai.py` | AI provider connector (Gemini, OpenAI, Groq, Claude) and token telemetry |
| **Prompts & Logic** | `studio/brain.py` | Prompt templates for hook detection, virality scoring, and titles |
| **Pipeline & Queue** | `studio/jobs.py` | Asynchronous clip pipeline and multi-user fair scheduling queue |
| **Live Stream** | `studio/live.py` | Rolling memory stream recording and live highlight extraction |
| **Transcription** | `studio/transcribe.py` | Local on-server speech-to-text with sherpa-onnx (Parakeet model) |
| **Render Engine** | `studio/render.py` | FFmpeg video reframing (9:16) and kinetic subtitle drawing with Pillow |
| **Media Probe** | `studio/media.py` | yt-dlp downloader, media probe, and audio extraction utilities |
| **Email Service** | `studio/mailer.py` | Branded transactional emails for verification and access approval |
| **Web UI** | `web/` | Dark studio frontend (landing, auth, reset, legal, app, admin) |

## Testing

Run the test suite before submitting changes:
```bash
python tests/run.py
```
This runs against an isolated temporary environment without touching real production data.
