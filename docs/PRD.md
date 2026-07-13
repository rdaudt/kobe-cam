# Kobe-Cam — Product Requirements Document

## Overview

Kobe-Cam is a mobile pet monitoring app that transforms a spare smartphone into a dedicated pet camera. Inspired by Barkio (market leader in phone-as-camera pet apps with 400K+ downloads), Kobe-Cam aims to deliver a modern, AI-powered pet monitoring experience without requiring dedicated hardware.

## Core Concept

Two devices, two roles:

- **Pet Station** (camera device) — an old/spare phone placed at home, streaming video and audio of the pet.
- **Owner Station** (viewer device) — the user's primary phone, receiving the stream and alerts.

Both devices run the same app; the role is selected during setup.

---

## User Personas

| Persona | Description | Key Need |
|---------|-------------|----------|
| Anxious first-time pet parent | Worries about leaving puppy alone | Real-time alerts, separation anxiety insights |
| Multi-pet household | 2-3 dogs/cats in different rooms | Multi-camera, multi-room view |
| Working professional | Away 8+ hours/day | Background monitoring, activity summary |
| Family with shared pet | Multiple people want to check in | Multi-owner access, shared activity log |

---

## Feature Specifications

### F1 — Device Pairing & Setup

| Attribute | Detail |
|-----------|--------|
| Goal | Connect Pet Station and Owner Station in under 2 minutes |
| Flow | Install app → Create account → Assign role (Pet Station / Owner Station) → Auto-pair via account |
| Multi-device | Up to 4 Pet Stations per account |
| Multi-owner | Unlimited Owner Stations per account |
| Platforms | iOS, Android (minimum); stretch: macOS, Windows, Web |

### F2 — Live Video Streaming

| Attribute | Detail |
|-----------|--------|
| Resolution | 720p default, 1080p on WiFi |
| Latency | < 2 seconds |
| Protocol | WebRTC peer-to-peer (fallback: TURN relay) |
| Night vision | IR-assisted or software-enhanced low-light mode |
| Night light | Toggle LED/screen flash to illuminate and attract pet's attention |
| Zoom | Digital zoom (2x–4x) |
| Orientation | Landscape and portrait support |

### F3 — Two-Way Audio

| Attribute | Detail |
|-----------|--------|
| Direction | Bidirectional — owner hears pet, pet hears owner |
| Push-to-talk | Default mode on Owner Station |
| Echo cancellation | Required to prevent feedback loops |
| Latency | < 500ms |

### F4 — Two-Way Video

| Attribute | Detail |
|-----------|--------|
| Description | Owner's face is shown on the Pet Station screen so the pet can see them |
| Trigger | Manual toggle from Owner Station |
| Use case | Calming anxious pets by showing familiar face |

### F5 — AI Bark & Noise Detection

| Attribute | Detail |
|-----------|--------|
| Model | On-device ML classifier |
| Classes | Bark, howl, whine, meow, silence, ambient noise |
| False alarm reduction | AI distinguishes pet sounds from footsteps, doors, traffic |
| Sensitivity | User-adjustable (low / medium / high) |
| Notification options | Bark only, all noise, or muted |
| Audio snippet | Attach 5-second audio clip to notification |

### F6 — Motion Detection

| Attribute | Detail |
|-----------|--------|
| Method | Frame-diff analysis or on-device ML |
| Zones | Optional user-defined detection zones |
| Sensitivity | Adjustable threshold |
| Output | Notification + short video clip saved to activity log |

### F7 — Activity Log & Insights

| Attribute | Detail |
|-----------|--------|
| Data captured | Noise events, motion events, barking duration, rest periods |
| Timeline | Visual timeline with time-of-day color coding (morning/afternoon/evening/night) |
| Per-session stats | Rest %, noise %, movement %, bark count & duration |
| Trends | Daily/weekly behavior trends over time |
| Use case | Spot separation anxiety patterns, post-surgery recovery monitoring, age-related behavior changes |
| Export | Shareable summary (PDF or link) for vet visits |

### F8 — Pre-Recorded Voice Commands

| Attribute | Detail |
|-----------|--------|
| Description | User records custom voice commands (e.g., "sit", "no", "good boy") |
| Storage | Up to 10 commands, stored locally + synced to cloud |
| Trigger | Manual (tap) or automatic (triggered by bark detection) |
| Use case | Calm anxious dog without needing to speak live |

### F9 — Smart Notifications

| Attribute | Detail |
|-----------|--------|
| Types | Bark alert, motion alert, connection lost, connection restored |
| Delay alerts | Configurable delay (5–120s) to avoid brief-drop false alarms |
| Repeat alerts | Re-notify every N minutes until acknowledged |
| Quiet hours | Scheduled mute periods |
| Channels | Push notification + in-app |

### F10 — Background Monitoring Mode

| Attribute | Detail |
|-----------|--------|
| Description | Owner Station runs in background; video/audio not streamed to save battery |
| Alerts | Push notifications still delivered on noise/motion events |
| Battery optimization | Minimal CPU/network on Owner Station |
| Resume | Tap notification to open live stream instantly |

### F11 — Video & Photo Recording

| Attribute | Detail |
|-----------|--------|
| Manual capture | Owner can take photo or start recording at any time |
| Auto-capture | Short clip saved on motion/bark events |
| Storage | Local device + optional cloud sync |
| Gallery | In-app media gallery with date/event filters |

### F12 — Monitoring Scheduling

| Attribute | Detail |
|-----------|--------|
| Description | Automatic start/stop monitoring at configured days and times |
| Use case | Auto-enable when owner leaves for work (M–F 9am–5pm) |
| Integration | Optional: geofence trigger (leave home → start monitoring) |

### F13 — Multi-Room / Multi-Pet View

| Attribute | Detail |
|-----------|--------|
| Pet Stations | Up to 4 cameras simultaneously |
| Layout | Grid view (2x2) or swipe between feeds |
| Individual alerts | Per-camera notification settings |
| Labels | User-assignable names ("Living Room", "Kitchen", "Kobe's crate") |

---

## Non-Functional Requirements

| Category | Requirement |
|----------|-------------|
| Performance | Stream startup < 3s; notification delivery < 5s |
| Reliability | Auto-reconnect on network drop; offline queuing of events |
| Security | E2E encryption on video/audio streams; account auth via OAuth/email |
| Privacy | No video stored on servers unless user opts into cloud; on-device AI processing |
| Battery | Pet Station: sustain 8+ hours on charger; Owner Station background: < 3% battery/hour |
| Accessibility | VoiceOver/TalkBack support; high-contrast mode |
| Localization | English (launch); Spanish, Portuguese (v2) |

---

## Monetization Model

| Tier | Price | Includes |
|------|-------|----------|
| **Free** | $0 | 1 Pet Station, live stream, two-way audio, basic noise alerts, 24h activity log |
| **Premium** | ~$5.99/mo or $39.99/yr | Up to 4 Pet Stations, AI bark detection, motion detection, video recording, full activity log history, pre-recorded commands, scheduling, cloud storage (7-day rolling) |
| **Family** | ~$8.99/mo or $59.99/yr | Premium + unlimited Owner Stations, shared activity log, 30-day cloud storage |

---

## Technical Architecture (High-Level)

```
┌─────────────────┐         WebRTC / TURN          ┌─────────────────┐
│   Pet Station   │◄──────────────────────────────►│  Owner Station  │
│  (camera app)   │                                 │  (viewer app)   │
└────────┬────────┘                                 └────────┬────────┘
         │                                                   │
         │  Events, metadata                                 │  Auth, settings
         ▼                                                   ▼
┌─────────────────────────────────────────────────────────────────────┐
│                          Backend Services                            │
│  ┌───────────┐  ┌───────────┐  ┌──────────┐  ┌──────────────────┐ │
│  │   Auth    │  │  Signaling│  │  Push /   │  │  Cloud Storage   │ │
│  │  Service  │  │  Server   │  │  Alerts   │  │  (recordings)    │ │
│  └───────────┘  └───────────┘  └──────────┘  └──────────────────┘ │
└─────────────────────────────────────────────────────────────────────┘
```

### Key Technology Decisions

| Layer | Choice | Rationale |
|-------|--------|-----------|
| Mobile framework | React Native or Flutter | Cross-platform with single codebase |
| Video streaming | WebRTC | Low-latency P2P; TURN fallback for NAT traversal |
| AI/ML | TensorFlow Lite / Core ML | On-device inference for bark/motion detection |
| Backend | Node.js or Python (FastAPI) | Signaling server, auth, push notifications |
| Push notifications | FCM (Android) + APNs (iOS) | Platform-native delivery |
| Cloud storage | S3-compatible (Vercel Blob, AWS S3) | Video clip storage for premium users |
| Auth | Firebase Auth or custom JWT | Email + social login |

---

## User Flows

### First-Time Setup (Pet Station)

1. Download app → Sign up / Log in
2. Select role: "This device will watch my pet"
3. Grant permissions: camera, microphone, notifications, battery optimization bypass
4. Position device, preview camera angle
5. Tap "Start Monitoring" → device enters Pet Station mode (screen dims, camera active)

### First-Time Setup (Owner Station)

1. Download app → Log in with same account
2. Select role: "I want to watch my pet"
3. Grant permissions: notifications
4. See paired Pet Station(s) → tap to connect
5. Live stream begins

### Daily Usage

1. Owner leaves home → monitoring starts (manual or scheduled)
2. Pet barks → AI classifies → push notification with audio snippet
3. Owner taps notification → live stream opens
4. Owner talks to pet or plays pre-recorded command
5. Pet calms down → owner returns to background mode
6. Owner returns home → stops monitoring
7. Reviews activity log: "Kobe barked 3 times, was calm 94% of the time"

---

## Success Metrics

| Metric | Target |
|--------|--------|
| Setup completion rate | > 80% |
| Daily active monitoring sessions | > 60% of registered users |
| Alert-to-open latency | < 10 seconds (user taps within 10s) |
| Premium conversion | > 8% of free users within 30 days |
| App Store rating | ≥ 4.5 stars |
| Churn (monthly) | < 5% for premium subscribers |

---

## Competitive Advantages over Barkio

| Differentiator | Detail |
|----------------|--------|
| Modern UI/UX | Contemporary design system vs Barkio's dated interface |
| Better AI | More granular sound classification; behavior pattern recognition |
| Open architecture | Potential for community plugins, integrations |
| Privacy-first | On-device processing default; no cloud unless opted in |
| Vet-sharing | Export activity reports for veterinary consultations |
| Geofence automation | Auto-start/stop based on location |

---

## Milestones

| Phase | Scope | Timeline |
|-------|-------|----------|
| **MVP** | Pairing, live stream, two-way audio, basic noise alerts | 6–8 weeks |
| **v1.0** | AI bark detection, activity log, motion detection, recordings | +4 weeks |
| **v1.5** | Pre-recorded commands, scheduling, multi-camera | +4 weeks |
| **v2.0** | Premium tier, cloud storage, family sharing, web app | +6 weeks |
