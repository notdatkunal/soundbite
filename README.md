# 🔊 SoundBite

> **Social Audio Chat & Live Voice Party Rooms with Audio Stickers & Soundboard Reactions**

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-active%20development-orange.svg)]()
[![Platform](https://img.shields.io/badge/platform-Web%20%7C%20iOS%20%7C%20Android-green.svg)]()

---

## 📌 Overview

**SoundBite** is the first social messaging and live voice platform built entirely around **Short Audio Stickers, Voice Memes, and Live Soundboards**. 

While modern messaging apps support text, emojis, GIFs, and image stickers, audio reactions have been left behind. SoundBite brings the expressiveness of short soundbites (1–5s audio clips, movie lines, meme sounds, custom vocal snippets) into daily chats, paired with **Clubhouse/Discord-style live voice party rooms** where participants can drop soundboard reactions in real time.

---

## 🚀 Key Features

### 💬 1. 2FA Secured WhatsApp-Style Chat Interface
- **End-to-End Encrypted & 2FA Protected:** Two-Factor Authentication (TOTP / SMS / Passkeys) ensures secure private & group messaging.
- **Audio Sticker Keyboard:** Dedicated drawer next to emoji/GIF selectors allowing users to instantly send playable soundbites and meme clips in chat threads.
- **Waveform & Tap-to-Play Preview:** Instant inline audio playback with dynamic waveform visualizations and zero lag.

### 🎙️ 2. Live Voice Party Rooms (Clubhouse Meets Discord)
- **Global & Private Audio Stages:** Join open public party rooms or create private listening lounges for friends with speaker/listener roles.
- **Ultra-Low Latency Spatial Audio:** Built on WebRTC / SFU for crisp, crystal-clear real-time conversation.
- **Stage Moderation:** Hand-raising, co-hosting, speaker muting, and room recording features.

### 🎛️ 3. Interactive Live Soundboard Reactions
- **In-Call Sound Drops:** Play short sound effects, meme sounds, or personalized audio clips directly into live voice calls and party stages.
- **Personal Audio Collections & Packs:** Curate, upload, and organize personal soundboards into categorized packs (e.g. *Gaming FX*, *Bollywood Lines*, *Sarcastic Laughs*, *Anime Sounds*).
- **Sound Clip Marketplace & Creator Discovery:** Discover trending sound clips, follow creators, and save community-uploaded soundbites to your personal vault.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Client Layer                           │
│           (React / React Native / Electron)                 │
│  ┌─────────────────────────┐   ┌─────────────────────────┐  │
│  │   2FA Chat & Messages   │   │ Live Voice Room Stage   │  │
│  │  • Audio Sticker Drawer │   │ • WebRTC Audio Stream   │  │
│  │  • Waveform Visualizer  │   │ • Real-time Soundboard  │  │
│  └────────────┬────────────┘   └────────────┬────────────┘  │
└───────────────┼─────────────────────────────┼───────────────┘
                │ HTTPS / WSS                 │ WebRTC (Opus)
                ▼                             ▼
┌─────────────────────────────┐ ┌─────────────────────────────┐
│       API Gateway &         │ │    LiveKit / Mediasoup      │
│   Chat Signaling Server     │ │      WebRTC SFU Node        │
│   (FastAPI / NestJS)        │ │  (Low-Latency Audio Mixer)  │
│ • Auth & 2FA (TOTP/Passkey) │ └──────────────┬──────────────┘
│ • Audio Sticker Catalog     │                │
│ • Push Notifications        │                │
└──────────────┬──────────────┘                │
               │                               │
               ▼                               ▼
┌─────────────────────────────────────────────────────────────┐
│                      Storage & Edge                         │
│  • S3 / Cloudflare R2 (Audio Clip CDN - Opus/m4a)           │
│  • PostgreSQL (User profiles, chat rooms, metadata)         │
│  • Redis (Presence, active room states & rate limits)       │
└─────────────────────────────────────────────────────────────┘
```

---

## 📚 Documentation

- [System Architecture & Audio Pipeline](docs/ARCHITECTURE.md)

---

## 🛠️ Tech Stack

- **Frontend:** React / React Native, Tailwind CSS, Wavesurfer.js, Web Audio API
- **Real-Time Voice & Media:** WebRTC, LiveKit / Mediasoup SFU
- **Backend:** Node.js (NestJS / Express) or Python (FastAPI), WebSocket signaling
- **Storage & CDN:** Cloudflare R2 / AWS S3 with edge audio compression (Opus 48kbps)
- **Database:** PostgreSQL + Redis

---

## 📄 License

This project is licensed under the MIT License.
