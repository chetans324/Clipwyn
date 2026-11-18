# Gaming Mode in Clipwyn by Braivex

Clipwyn includes specialized intelligence for gaming streams. While talk-based streams rely heavily on transcripts, gaming streams depend on fast action, game context, facecams, and emotional streamer reactions.

## 1. Game Context Awareness

`studio/games.py` automatically detects the game being played from stream titles, video metadata, or channel settings. Known games include GTA VI, GTA V, Fortnite, Minecraft, Valorant, Elden Ring, Palworld, Helldivers 2, Cyberpunk 2077, Marvel Rivals, and Rocket League.

When a game is identified:
- **Game-Specific Hook Prompts** (`studio/brain.py`): The AI is instructed to look for clutch plays, jump scares, glitches, clutches, or streamer rage reactions.
- **Automated Social Hashtags**: Relevant tags (e.g. `#GTA6`, `#FortniteClutch`) are automatically attached to generated clips.

## 2. Crowd Sentiment & Chat Velocity

`studio/live.py` measures chat message velocity and emoji spikes (such as `KEKW`, `POG`, `LUL`, `W`) during live Twitch and Kick streams to detect viral moments even before the streamer finishes reacting.

## 3. Facecam Detection & Auto-Layout

Using frame analysis in `studio/render.py`, Clipwyn detects facecam placement to generate multi-tier vertical clips (gameplay centered with the streamer's reaction pinned on top).
