# Steam Music Bot

A lightweight Steam chat bot that joins a Steam voice channel and plays audio from uploaded sounds, direct URLs, and searchable sources like YouTube and MyInstants.

This project is built around browser automation and Steam chat integration, so it can run in a container and sit in a Steam group chat while reacting to commands like `play`, `pause`, `stop`, and `youtube`.

## Features

- Joins a Steam group voice channel automatically
- Plays audio from:
  - uploaded local sound files
  - direct URLs
  - YouTube search results
  - MyInstants searches
- Exposes a simple web UI for login and bot controls
- Works with Docker Compose for easy local deployment
- Supports Steam Guard handling via a browser-based flow

## Architecture

The bot uses:

- Node.js for the main process
- Puppeteer/Chromium to automate Steam in a browser
- Steam chat APIs to monitor messages and voice state
- A PostgreSQL database for sound metadata and persistence
- A small Express web app for local control and login flow

## Requirements

Before running the bot, make sure you have:

- Docker and Docker Compose installed
- A Steam account with access to the target Steam group and voice channel
- Your Steam credentials available as environment variables or in `config.json`
- A machine that can run Chromium-based browser automation

## Quick start

1. Create a `config.json` file in the project root.
2. Set your Steam account and target room details.
3. Start the app with Docker Compose.

Example `config.json`:

```json
{
  "baseUrl": "http://localhost:8080/",
  "port": 8081,
  "mode": "web",
  "steam": {
    "userName": "your_steam_username",
    "password": "your_steam_password",
    "groupName": "Gamers Hub",
    "channelName": "music"
  },
  "plugins": ["myinstants", "youtubesearch"],
  "volume": 0.3
}
```

Or use environment variables when running with Docker:

```bash
export STEAM_USERNAME="your_username"
export STEAM_PASSWORD="your_password"
```

Then start the app:

```bash
docker compose -f docker-compose.web.yml up --build -d
```

Open the UI in your browser:

```text
http://localhost:8080/
```

If Steam Guard or login verification is required, complete the flow in the browser before the bot joins the voice channel.

## Useful Docker commands

Start the bot and database:

```bash
docker compose -f docker-compose.web.yml up -d
```

Stop the bot and database:

```bash
docker compose -f docker-compose.web.yml stop
```

Start existing containers again:

```bash
docker compose -f docker-compose.web.yml start
```

Restart only the bot container:

```bash
docker compose -f docker-compose.web.yml restart node-chrome
```

Check container status:

```bash
docker compose -f docker-compose.web.yml ps
```

## Bot commands

Once the bot is running and mentioned in Steam chat, use commands such as:

```text
@ghmusic play
@ghmusic play some-sound-name
@ghmusic playurl https://youtube.com/watch?v=VIDEO_ID
@ghmusic stop
@ghmusic pause
@ghmusic say hello from the bot
@ghmusic youtube lo-fi hip hop
@ghmusic youtube dramatic chipmunk #2
@ghmusic instant mario death sound
```

### Command reference

- `play [sound]` — play a saved/uploaded sound, or resume the current one
- `playurl <URL>` — play audio from a direct URL, including YouTube links
- `stop` / `pause` — stop or pause playback
- `youtube <query>` — search YouTube and play the first result or a specific result with `#2`, `#3`, etc.
- `instant <query>` — look up a sound from MyInstants
- `say <text>` / `tts <text>` — speak text via text-to-speech
- `help [command]` — show command help

The project also includes a reference file with command examples in [commands.md](commands.md).

## Project layout

```text
.
├── src/                 # bot logic, Steam integrations, plugins
├── web/                # browser UI assets
├── docker/             # Docker helper files
├── config.json         # bot configuration
├── docker-compose.web.yml
├── Dockerfile.web
├── index.js            # application entry point
├── package.json
├── commands.md         # command examples
├── LICENSE
└── README.md
```

## Development

Install dependencies:

```bash
npm install
```

Run the app locally:

```bash
node index.js
```

You can also use the Docker-based setup for a more realistic Steam/browser environment.

## Notes

- This project automates a real browser session with Steam, so it is best suited for a personal or controlled environment.
- Steam login and voice chat behavior may require periodic re-authentication or manual resolution of login prompts.
- The bot is not a general-purpose Discord or Slack bot; it is specifically built around Steam voice and browser automation.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
