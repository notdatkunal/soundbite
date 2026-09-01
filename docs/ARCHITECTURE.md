# 🎧 SoundBite Engineering & Audio Pipeline Architecture

## 1. System Components

The SoundBite architecture is engineered to balance two distinct audio domains:
1. **Asynchronous Audio Stickers:** Tiny, highly optimized audio snippets (0.5s - 5s) stored in S3/R2 and cached on the client for instantaneous playback in chats.
2. **Synchronous Real-Time Voice Rooms:** Low-latency WebRTC streams powered by a Selective Forwarding Unit (SFU) with an in-stream audio injector for live soundboard drops.

---

## 2. Audio Processing Pipeline

```
  User Audio Upload / Clip Recording (0.5s - 5s)
                       │
                       ▼
         ┌───────────────────────────┐
         │     FFmpeg Edge Worker    │
         │ • Normalization (EBU R128)│
         │ • Trim Silence            │
         │ • Encode Opus @ 32-48kbps │
         │ • Generate Waveform JSON  │
         └─────────────┬─────────────┘
                       │
         ┌─────────────┴─────────────┐
         ▼                           ▼
┌──────────────────┐        ┌──────────────────┐
│ Cloudflare R2    │        │ PostgreSQL DB    │
│ Audio Asset CDN  │        │ Metadata & Peaks │
└──────────────────┘        └──────────────────┘
```

---

## 3. Real-Time Soundboard Injection in WebRTC

In live party rooms, soundboard reactions can be played using two methods:

### Method A: Client-Side Audio Mixing (Recommended for low server load)
- The client fetches the audio sticker via CDN.
- Decodes it using the browser/app `AudioContext`.
- Mixes the decoded buffer into the WebRTC `MediaStreamTrack` before sending it to the SFU.
- **Advantage:** Low latency, no server-side transcode needed.

### Method B: Server-Side SFU Ingestion
- The user triggers a soundboard action via WebSocket.
- The SFU injects the audio file directly into the room's mixed audio channel.
- **Advantage:** Guarantees synchronized audio timing across all listeners regardless of individual client network lag.

---

## 4. WebSocket Event Schema for Soundboard Drops

```json
{
  "event": "ROOM_SOUNDBOARD_TRIGGER",
  "room_id": "room_party_india_tech_99",
  "timestamp": 1756723200,
  "sender": {
    "user_id": "usr_kunal_01",
    "display_name": "Kunal",
    "avatar_url": "https://cdn.soundbite.app/avatars/kunal.png"
  },
  "sound_clip": {
    "clip_id": "clip_bruh_sound_effect",
    "title": "Bruh Sound Effect #2",
    "duration_ms": 1200,
    "audio_url": "https://cdn.soundbite.app/clips/bruh.opus",
    "volume_gain": 0.85
  }
}
```

---

## 5. Security & 2FA Implementation

- **Authentication:** JWT (Access + Refresh tokens) with mandatory 2FA challenge via TOTP (RFC 6238) or WebAuthn / Passkeys.
- **Soundboard Spam Protection:** Token-bucket rate limiting (maximum 3 soundboard reactions per 10 seconds per speaker) with room moderator mute overrides.
- **Content Moderation:** Automated audio fingerprinting and hate speech / NSFW sound moderation on upload.
