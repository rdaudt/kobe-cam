# Kobe-Cam

A zero-cost pet camera that turns an old iPhone into a motion- and sound-detecting pet monitor. Uses [VDO.Ninja](https://vdo.ninja/) for free P2P video streaming and a Telegram bot for instant alerts with snapshot photos.

**No servers. No subscriptions. No dedicated hardware.**

## How It Works

```
Old iPhone (Safari)                    Your Phone
┌─────────────────────┐              ┌──────────────────┐
│  Camera + mic       │──P2P WebRTC──►│  Live viewer     │
│  Motion detection   │               │  (with audio)    │
│  Bark detection     │              └──────────────────┘
│  On motion/bark:    │
│   snapshot → ───────│── Telegram ──► Family Group Chat
└─────────────────────┘                 📸 + reason + live link
```

- The iPhone runs the **Pet Station** page in Safari — it streams video and audio via VDO.Ninja (P2P WebRTC) and watches for motion and barking locally
- When motion or barking is detected, it captures a snapshot and sends it to your **Telegram group** via bot API, labeled with what triggered it
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
2. Enter your **Stream ID** (letters and numbers only, e.g., "kobecam123" — a random one is pre-filled)
3. Enter your **Telegram Bot Token** and **Chat ID**
4. Choose back or front camera, motion sensitivity, and sound sensitivity
5. Tap **Start Monitoring** — grant both **camera and microphone** permission
6. Position the phone where it can see your pet

Settings are saved on the device — you only configure once.

#### Tuning sound alerts

While monitoring, a **Sound** level meter appears under the camera with a tick marking your
threshold. Watch it to calibrate: note where ambient noise sits versus an actual bark, then
pick the sensitivity whose tick falls between them.

| Setting | Threshold | Catches |
|---|---|---|
| High | 55 | Quiet whines and whimpers |
| Medium (default) | 70 | Barking |
| Low | 82 | Loud, sustained barking only |
| Off | — | Sound alerts disabled (stream audio still works) |

Detection is loudness-based, not bark-specific — any sustained loud noise triggers it. Since
the dog is usually home alone, that's nearly always the dog. A sound must stay above the
threshold for ~200 ms, so single clicks and pops don't fire alerts.

### 5. Watch Live (Optional)

Share this link with family:
```
https://YOUR_USERNAME.github.io/kobe-cam/viewer.html?id=YOUR_STREAM_ID
```

Or watch directly on VDO.Ninja without our viewer page:
```
https://vdo.ninja/?view=YOUR_STREAM_ID&password=false
```

## Features

- **Motion detection** — runs on-device using canvas frame diffing, no cloud processing
- **Bark / sound detection** — on-device mic level analysis with a live meter for calibration
- **Telegram alerts with snapshots** — JPEG photo sent to your group, labeled "Motion detected", "Barking detected", or "Motion + barking detected", with a one-tap "Watch live" link
- **30-second shared cooldown** — a barking, moving dog sends one alert, not two
- **Live streaming with audio** — P2P via VDO.Ninja, near-zero latency; viewers can hear the room
- **Wake lock** — keeps the iPhone screen and camera active
- **Configurable sensitivity** — independent thresholds for motion and sound
- **Multi-viewer** — anyone with the stream ID can watch live
- **Multi-subscriber** — anyone in the Telegram group gets alerts

## Requirements

- An old iPhone (6s or newer) with Safari
- A WiFi connection for the camera phone
- A Telegram account

## Privacy & Security

- Video and audio streams are **peer-to-peer encrypted** (WebRTC DTLS-SRTP)
- Signaling goes through VDO.Ninja's free servers — only connection metadata, no media
- Motion and sound detection run **entirely on-device** — no video or audio is uploaded to any server
- Only snapshot photos are sent to Telegram on alert events — audio is never recorded or sent
- The live stream carries room audio, so anyone with the stream ID can listen in — set Sound Alerts appropriately and keep the ID hard to guess
- Bot token and chat ID are stored in the iPhone's localStorage — never committed to the repo
- The stream ID is the only "password" for live viewing — choose something hard to guess

## Files

| File | Purpose |
|------|---------|
| `index.html` | Pet Station — camera, mic, motion + sound detection, Telegram alerts, P2P streaming |
| `viewer.html` | Live viewer — embeds the VDO.Ninja stream |
| `docs/PRD.md` | Product requirements document |

## License

MIT
