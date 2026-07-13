# Kobe-Cam

A zero-cost pet camera that turns an old iPhone into a motion-detecting pet monitor. Uses [VDO.Ninja](https://vdo.ninja/) for free P2P video streaming and a Telegram bot for instant motion alerts with snapshot photos.

**No servers. No subscriptions. No dedicated hardware.**

## How It Works

```
Old iPhone (Safari)                    Your Phone
┌─────────────────────┐              ┌──────────────────┐
│  Camera stream      │──WebRTC P2P─►│  Live viewer     │
│  Motion detection   │              │  (optional)      │
│  On motion:         │              └──────────────────┘
│   snapshot → ───────│── Telegram ──► Family Group Chat
└─────────────────────┘                 📸 + "Motion!"
```

- The iPhone runs the **Pet Station** page in Safari — it streams video via VDO.Ninja and monitors for motion locally
- When motion is detected, it captures a snapshot and sends it to your **Telegram group** via bot API
- Anyone in the group gets push notifications with the photo
- Anyone with the viewer link can watch the live stream

## Setup

### 1. Create a Telegram Bot

1. Open Telegram and search for **@BotFather**
2. Send `/newbot`
3. Choose a name (e.g., "KobeCam") and a username (e.g., "kobe_cam_bot")
4. BotFather will give you a **bot token** — copy it, you'll need it later

### 2. Create a Telegram Group

1. Create a new group in Telegram (e.g., "Pet Alerts")
2. Add your bot to the group
3. Send any message in the group (e.g., "hello")
4. Open this URL in your browser, replacing `YOUR_TOKEN` with your bot token:
   ```
   https://api.telegram.org/botYOUR_TOKEN/getUpdates
   ```
5. Look for `"chat":{"id":-100XXXXXXXXXX}` in the response — that number (including the minus sign) is your **Chat ID**
6. Add family members or caretakers to the group — they'll all receive alerts

### 3. Deploy to GitHub Pages

1. Fork or clone this repo
2. Go to your repo's **Settings → Pages**
3. Under "Source", select **Deploy from a branch**
4. Choose **main** branch, **/ (root)** folder
5. Click **Save**
6. Your site will be live at `https://YOUR_USERNAME.github.io/kobe-cam/`

### 4. Start Monitoring

1. Open `https://YOUR_USERNAME.github.io/kobe-cam/` on the old iPhone in Safari
2. Enter your **Stream ID** (any name you choose, e.g., "kobe-123")
3. Enter your **Telegram Bot Token** and **Chat ID**
4. Choose back or front camera
5. Tap **Start Monitoring**
6. Position the phone where it can see your pet

Settings are saved on the device — you only configure once.

### 5. Watch Live (Optional)

Share this link with family:
```
https://YOUR_USERNAME.github.io/kobe-cam/viewer.html?id=YOUR_STREAM_ID
```

Or open [VDO.Ninja](https://vdo.ninja/) directly with `?view=YOUR_STREAM_ID`.

## Features

- **Motion detection** — runs on-device using canvas frame diffing, no cloud processing
- **Telegram alerts with snapshots** — JPEG photo sent to your group on each motion event
- **30-second cooldown** — prevents notification spam
- **Live streaming** — P2P via VDO.Ninja, near-zero latency
- **Wake lock** — keeps the iPhone screen and camera active
- **Configurable sensitivity** — high / medium / low thresholds
- **Multi-viewer** — anyone with the stream ID can watch live
- **Multi-subscriber** — anyone in the Telegram group gets alerts

## Requirements

- An old iPhone (6s or newer) with Safari
- A WiFi connection for the camera phone
- A Telegram account

## Privacy & Security

- Video streams are **peer-to-peer encrypted** (WebRTC DTLS-SRTP)
- Motion detection runs **entirely on-device** — no video is uploaded to any server
- Only snapshot photos are sent to Telegram on motion events
- Bot token and chat ID are stored in the iPhone's localStorage — never committed to the repo
- The stream ID is the only "password" for live viewing — choose something hard to guess

## Files

| File | Purpose |
|------|---------|
| `index.html` | Pet Station — camera, motion detection, Telegram alerts |
| `viewer.html` | Live viewer — embeds VDO.Ninja stream |
| `docs/PRD.md` | Product requirements document |

## License

MIT
