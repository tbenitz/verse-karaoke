# VERSE

A browser karaoke room you can host on GitHub Pages. No server. The DeepSeek key never leaves the browser.

## What it does

- Upload an MP3. The room host plays it and the mix is streamed live over PeerJS, so everyone hears it without a send button. A guest upload is pushed to the host automatically, then played for the room.
- On every track, including a switch from a known song to an unknown one, VERSE searches LRCLIB and applies synced lyrics when the match is good. If that misses, it transcribes with Whisper Tiny and, when a DeepSeek key is saved, cleans and sections the draft. A track that already has lyrics keeps them.
- Word highlighting follows LRC timings. Unsynced lines get even word timing you can nudge.
- Stereo vocal cancel: pull the centered vocal down or the side (instrumental) down. Works best on studio mixes with a centered vocal. It is not Demucs.
- Record your mic, or mic plus the backing mix, and download a WebM.
- Rooms: share a code. Guests join the same queue, chat, and playback clock over PeerJS. Optional “send this track” pushes the file to people in the room.
- Cameras and mics for the party sit in a [VDO.Ninja](https://vdo.ninja) frame on the same room id. Reconnect restores the last room, name, and key.

## Deploy

1. Create a public GitHub repo.
2. Upload `index.html` to the repo root (this folder is the site).
3. Settings → Pages → Deploy from branch → `main` → `/ (root)`.
4. Open `https://<you>.github.io/<repo>/`.

Local preview: open `index.html` in a browser, or `python -m http.server 8080` in this folder. Whisper and PeerJS need a normal http origin, not `file://`.

## Keys and privacy

Settings (gear) → paste a [DeepSeek](https://platform.deepseek.com) API key. It is stored in `localStorage` under `verse.settings` and sent only to `api.deepseek.com` (or the optional CORS proxy you set).

DeepSeek’s API often rejects browser calls (no CORS). If cleanup fails, set a proxy prefix in Settings, for example `https://corsproxy.io/?`, or paste lyrics and use Structure only after a proxy works. The proxy will see the key. Prefer a proxy you control.

Transcription model cache lives in the browser (Hugging Face / Transformers.js). Lyrics search goes to `lrclib.net`. Party video goes to `vdo.ninja`. Room sync uses PeerJS’s public broker (`0.peerjs.com`).

## Room model

The host’s PeerJS id is `verse-<code>`. Guests connect to the host. The host owns play/pause and the clock. If the host drops, everyone can reconnect; the same browser becomes host again if the id is free.

VDO.Ninja is independent of the PeerJS data channel. Leave the camera frame on so people hear each other. Use headphones if you monitor the backing track, or you will sing the instrumental back into the room.

## Vocal split limit

Center-channel cancel (`L − R`) is instant and offline. It will not cleanly separate a mono file, a hard-panned vocal, or a wide vocal effect. The sliders are Vocal (center) and Band (sides).

## License

MIT. Lyrics and recordings belong to whoever owns them. Don’t publish other people’s songs.
