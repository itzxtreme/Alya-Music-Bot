<p align="center">
  <img src="dashboard/public/branding/kreo-banner-v2.webp" alt="Alya Music Bot" width="960" />
</p>

<h1 align="center">Alya — Discord Music Bot</h1>

<p align="center">
  An advanced, feature-packed Discord music bot with 24/7 voice recovery, no-prefix permission system, animated application emojis, and an optional real-time web dashboard.
</p>

<p align="center">
  <a href="./package.json"><img src="https://img.shields.io/badge/Node.js-20+-5FA04E?style=flat-square" alt="Node.js 20+" /></a>
  <a href="./package.json"><img src="https://img.shields.io/badge/discord.js-14.26-5865F2?style=flat-square" alt="discord.js 14" /></a>
  <a href="./package.json"><img src="https://img.shields.io/badge/Lavalink-v4-FF5555?style=flat-square" alt="Lavalink v4" /></a>
  <a href="./package.json"><img src="https://img.shields.io/badge/Architecture-Hybrid%20Sharding-A78BFA?style=flat-square" alt="Hybrid Sharding" /></a>
  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue?style=flat-square" alt="License" /></a>
</p>

---

## Table of Contents

- [Features](#features)
- [Pterodactyl Deployment Guide](#pterodactyl-deployment-guide)
- [Local / VPS Setup](#local--vps-setup)
- [Environment Variables (.env)](#environment-variables-env)
- [Emoji Synchronization](#emoji-synchronization)
- [Commands](#commands)
- [Architecture](#architecture)
- [License](#license)

---

## Features

- **Multi-Source Audio Playback:** Stream high-quality audio seamlessly from YouTube, YouTube Music, Spotify, Apple Music, Deezer, JioSaavn, SoundCloud, and direct media streams.
- **24/7 Voice Support & Auto-Reconnect:** Stays in voice channels indefinitely with automatic voice health monitoring and session recovery if Lavalink drops.
- **No-Prefix (NP) System:** Enable prefixless command execution for specific users or entire servers with configurable durations (`24h`, `7d`, `30d`, `1y`, or `perm`).
- **Dedicated Music Channel (`musicchannel`):** Restrict music commands exclusively to designated text channels or active voice chat text threads.
- **Voice Channel Status Integration:** Automatically broadcasts currently playing song titles and animated equalizer icons directly to Discord's Voice Channel Status bar.
- **Dynamic Bot Presence:** Automatically rotates status when idle and dynamically displays active song titles when audio is playing.
- **Custom Application Emojis:** Native support for 23 custom application emojis for buttons, indicators, and player controls without requiring external server emojis.
- **13 Built-in Audio Filters:** Bassboost, Nightcore, 8D audio, Vaporwave, Tremolo, Vibrato, Pop, Soft, Treble, and more.
- **Slash & Prefix Commands:** 68 slash commands and 72 prefix commands with interactive Discord Components v2 UI buttons.
- **Optional Web Dashboard:** Next.js real-time web dashboard with Discord OAuth login, interactive queue management, song search, and server leaderboards.

---

## Pterodactyl Deployment Guide

Alya is optimized to run smoothly on any Pterodactyl-based hosting panel (Node.js Egg / Generic Bot Egg) using external databases like MongoDB Atlas and remote Lavalink nodes.

### Step 1: Create Your Server on Pterodactyl
1. In your Pterodactyl panel, create or open your Node.js bot server.
2. Ensure the server's Node.js version is set to **Node.js 20**, **22**, or newer.

### Step 2: Upload Files
Upload the project files to `/home/container/` using the panel's file manager or via SFTP:
- Do **not** upload `node_modules` (Pterodactyl will install them).
- Make sure `package.json`, `index.js`, `devrock.js`, `src/`, `scripts/`, and `.env.example` are present.

### Step 3: Configure Environment Variables
In the Pterodactyl File Manager:
1. Rename or copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```
2. Fill in the required variables:
   - `DISCORD_TOKEN`: Your Discord bot token from the [Discord Developer Portal](https://discord.com/developers/applications).
   - `DISCORD_CLIENT_ID`: Your bot's Application ID.
   - `OWNER_IDS`: Your Discord user ID (comma-separated for multiple owners).
   - `MONGO_URI`: Your MongoDB Atlas connection URI (`mongodb+srv://user:pass@cluster.mongodb.net/alya?retryWrites=true&w=majority`).
   - `LAVALINK_URL`: Your Lavalink v4 server address (e.g. `freelavalinkvinster.in:443`).
   - `LAVALINK_PASSWORD`: Lavalink node password (e.g. `youshallnotpass`).
   - `LAVALINK_SECURE`: `true` if your node uses SSL/WSS (port 443), or `false` for standard ports.

### Step 4: Configure the Startup Command
In the **Startup** tab of your Pterodactyl server:
- **Startup Command:** Leave default (`/usr/local/bin/node {{MAIN_FILE}}`) or set to:
  ```bash
  npm start
  ```
- **Main File:** `index.js` or `devrock.js`.
  *(Alya includes an auto-detection wrapper: running either `node index.js`, `node devrock.js`, or `npm start` automatically starts the cluster sharding manager without errors).*

### Step 5: Start the Bot
Click **Start** in your Pterodactyl console. Pterodactyl will automatically run `npm install` and boot up Alya.

---

## Local / VPS Setup

### Prerequisites
- [Node.js](https://nodejs.org/) v20.19.0 or higher
- [MongoDB](https://www.mongodb.com/) (Local instance or free MongoDB Atlas cluster)
- [Lavalink v4](https://github.com/lavalink-devs/Lavalink) node

### 1. Clone & Install
```bash
git clone https://github.com/your-username/alya-music-bot.git
cd alya-music-bot
npm install
```

### 2. Configure Environment
```bash
# Windows
copy .env.example .env

# Linux / macOS
cp .env.example .env
```
Edit `.env` and fill in your Discord Bot Token, Client ID, MongoDB URI, and Lavalink credentials.

### 3. Sync Emojis (Optional but Recommended)
To upload and bind the custom application emojis directly to your bot application:
```bash
npm run sync:emojis
```

### 4. Start Alya
```bash
# Production start (Bot only)
npm start

# Development mode
npm run dev

# Start both Bot and Dashboard (if dashboard is installed)
npm run start:all
```

---

## Environment Variables (.env)

| Variable | Required | Default | Description |
| :--- | :---: | :---: | :--- |
| `DISCORD_TOKEN` | **Yes** | — | Bot token from Discord Developer Portal |
| `DISCORD_CLIENT_ID` | **Yes** | — | Bot application client ID |
| `OWNER_IDS` | **Yes** | — | Comma-separated user IDs of bot owners |
| `MONGO_URI` | **Yes** | — | MongoDB connection string (Atlas or local) |
| `BOT_PREFIX` | No | `>` | Default text command prefix |
| `BOT_COLOR` | No | `#00D4FF` | Default hex color for Discord embeds |
| `LAVALINK_NAME` | No | `Main Node` | Name of the primary Lavalink node |
| `LAVALINK_URL` | **Yes** | `localhost:2333` | Lavalink host and port (`host:port`) |
| `LAVALINK_PASSWORD` | **Yes** | `youshallnotpass`| Lavalink server password |
| `LAVALINK_SECURE` | No | `false` | Set `true` if Lavalink runs on HTTPS/WSS (port 443) |
| `LAVALINK_SOURCE` | No | `ytmsearch` | Default search source (`ytmsearch`, `spsearch`, `ytsearch`) |
| `PORT` | No | `2000` | Bot Bridge API port for dashboard integration |
| `SPOTIFY_CLIENT_ID` | No | — | Spotify API client ID for metadata resolution |
| `SPOTIFY_CLIENT_SECRET` | No | — | Spotify API client secret |

---

## Emoji Synchronization

Alya includes an automated synchronization tool (`scripts/sync-emojis.js`) that uploads official, high-quality music control icons directly to your Discord Application via the Discord API v10 (`POST /applications/{id}/emojis`).

```bash
npm run sync:emojis
```
This registers all 23 player and UI emojis (Play, Pause, Skip, Loop, Shuffle, Autoplay, Animated Equalizer, etc.) as permanent bot-owned application emojis.

---

## Commands

Alya supports both Slash Commands (`/`) and Message Prefix Commands (`>`).

### 🎵 Music Commands
- `play <song/url>` — Play a song from YouTube, Spotify, Apple Music, Deezer, etc.
- `pause` / `resume` — Pause or resume the currently playing track.
- `skip` — Skip to the next track in queue.
- `previous` — Return to the previously played track.
- `stop` — Stop audio playback and clear the queue.
- `queue` — View the current server queue with interactive pagination.
- `nowplaying` — Show detailed information and a progress bar for the active song.
- `loop <track/queue/none>` — Toggle looping mode for track or queue.
- `shuffle` — Randomize songs in the current queue.
- `autoplay` — Toggle smart autoplay when the queue ends.
- `seek <time>` — Seek to a specific timestamp (e.g. `1:30`).
- `volume <1-100>` — Adjust audio volume.
- `lyrics` — Fetch synchronized or plain lyrics for the current song.
- `similar` — Find and queue recommendations matching the active song.

### ⚙️ Configuration & Management
- `247` — Toggle 24/7 mode to keep the bot connected in voice channels permanently.
- `musicchannel <#channel>` — Designate a specific channel for music commands.
- `setprefix <prefix>` — Change the server's command prefix.
- `ignore <#channel>` — Ignore commands in a specific text channel.

### 👑 Owner & No-Prefix System
- `np add @user [duration]` — Grant prefixless command usage to a user (`24h`, `7d`, `30d`, `perm`).
- `np add server [guild_id] [duration]` — Grant prefixless commands to an entire server.
- `np remove <@user/server>` — Revoke no-prefix permissions.
- `np list` / `np status` — Check active no-prefix grants and expiration timers.
- `node` — Inspect Lavalink node connection status, memory, and CPU usage.
- `restart` — Restart the bot clusters cleanly.

---

## Architecture

Alya uses a decoupled, cluster-based architecture:
- **`devrock.js`:** The Cluster Manager powered by `discord-hybrid-sharding`. Manages bot worker processes, restarts, and the Express REST Bridge API for dashboard communication.
- **`index.js`:** The worker entrypoint that initializes `MusicClient`, loads command handlers, sets up Kazagumo audio managers, and connects to Discord Gateway.
- **`src/utils/voiceHealthMonitor.js`:** Real-time heartbeat that verifies voice state integrity, auto-recovers dropped sessions, and updates Discord's Voice Channel Status bar.

---

## License

This project is licensed under the [MIT License](LICENSE).
