# CloneTTS 0.8.0 — PC & OpenAI-Compatible TTS Server Setup Guide

> Applies to: CloneTTS Android 0.8.0 and above  
> Last updated: 2026-11-25

CloneTTS 0.8.0 keeps the on-device offline voice cloning engine (CPU / Snapdragon NPU) while adding **remote PC and OpenAI-compatible TTS support**:

1. **Official Companion PC Server ([IndexTTS-Server](https://github.com/sipeter/IndexTTS-Server))** — Open-source pure C++ ONNX Runtime (CUDA / TensorRT / CPU) server & native Windows WPF desktop console based on IndexTTS 1.5. Zero Python required, one-click ~923 MB INT4+FP16 model download, no reference transcript needed, plus direct voice sync & upload from your phone.
2. **Generic OpenAI-Compatible TTS (`POST /v1/audio/speech`)** — Connect any local or cloud server implementing the OpenAI TTS API, including **[audio.cpp](https://github.com/0xShug0/audio.cpp)** (recommended with our open-source Windows desktop console **[AudioCpp-Desktop](https://github.com/sipeter/AudioCpp-Desktop)** for **Qwen3-TTS**, **CosyVoice 3**, **VoxCPM 2**, **IndexTTS 2.5**, **M5-TTS**, **Kokoro**, **PocketTTS**, etc.), **vLLM-Omni / SGLang-Omni**, **GPT-SoVITS / Fish Speech OpenAI wrappers**, or cloud APIs (OpenAI TTS, Fish Audio, etc.).

### Bonus for International / Multilingual Users
The built-in on-device engine (`ZipVoice`) focuses on Chinese & English. By connecting CloneTTS 0.8.0 to a remote OpenAI-compatible server (such as `audio.cpp` running **Qwen3-TTS** / **CosyVoice 3** / **Kokoro** / **PocketTTS**, or a cloud OpenAI-compatible API), you immediately unlock **Japanese, Korean, French, German, Spanish, Russian, Italian, Portuguese, and more languages** across Android System TTS, e-book readers (e.g. Legado / Moon+ Reader / Librera), and multi-role narration!

---

## Option A: Official Companion PC Server (`IndexTTS-Server`, Plug-and-Play)

Best for Chinese & English voice cloning with zero command-line setup and no reference transcripts required.

- Open-source repository & downloads: **[https://github.com/sipeter/IndexTTS-Server](https://github.com/sipeter/IndexTTS-Server)** ([Releases](https://github.com/sipeter/IndexTTS-Server/releases))

### 1. Download & Start on PC (Windows x64)
1. Download the Windows x64 CUDA 12 all-in-one archive from [IndexTTS-Server Releases](https://github.com/sipeter/IndexTTS-Server/releases) (e.g., `IndexTTS-Server-v0.1.0-win-x64-cuda12.7z` — no Python, .NET, or CUDA Toolkit required) and extract it.
2. Double-click **`IndexTTS-Desktop.exe`**:
   - **Download Model (first run)**: Open the **Models** tab and click **Download Model** to fetch and silently extract the ~923 MB INT4+FP16 ONNX model bundle from ModelScope (`ziansoft/indextts-1.5-onnx-int4`) into `models/` (importing a local `.zip` bundle is also supported).
   - **Start Server**: Switch to **Server & API**, choose CUDA / TensorRT / CPU, and click **Start Server**.
3. Note your PC's LAN IP address and port (default `9966`, configurable in the console, e.g., `http://192.168.1.100:9966`; optional Bearer token auth is also supported).
4. Make sure Windows Firewall allows inbound LAN access on that port.

### 2. Configure in CloneTTS (Android)
1. Connect your phone and PC to the same Wi-Fi network.
2. Open CloneTTS → **Voice Manager** → top-right menu → **Add Remote Source**.
3. Select **IndexTTS-Server**:
   - **Name**: e.g. `Home PC IndexTTS`
   - **Server URL**: e.g. `http://192.168.1.100:9966`
   - **API Key / Token**: leave blank unless you configured an auth token in the PC console.
4. Tap **Test Connection**, then **Save**.
5. Switch to the new remote tab and tap **Refresh Voices** to sync all voices from the PC `refs/` folder (`.wav` and `.mp3` supported, no transcript needed), or tap **Upload Reference Audio** to add new voices directly from your phone.

---

## Option B: Recommended All-in-One PC Server (`audio.cpp`, Multi-Model GGUF)

If you want to run **Qwen3-TTS** (10+ languages), **CosyVoice 3**, **VoxCPM 2** (48 kHz), **IndexTTS 2.5**, **M5-TTS**, **Kokoro**, or **PocketTTS** on your PC without dealing with Python / PyTorch dependency hell, **[audio.cpp](https://github.com/0xShug0/audio.cpp)** is the easiest choice:
- Single C++/CUDA/Metal/CPU binary (similar to `llama.cpp`)
- Single-file `.gguf` quantized models
- Native OpenAI `POST /v1/audio/speech` and `GET /v1/audio/voices` endpoints (automatic voice sync supported!)

### 1. Download `audio.cpp` and GGUF Models
1. **Recommended Windows GUI**: Download the all-in-one CUDA 12 bundle from **[AudioCpp-Desktop Releases](https://github.com/sipeter/AudioCpp-Desktop/releases)** — it bundles `audiocpp_server.exe` with a native Windows desktop console, one-click HuggingFace/ModelScope model downloads, visual `voice-library` management, and built-in ASR auto-transcription.
2. **Manual CLI setup**: Download the release package for your OS/GPU from `audio.cpp` GitHub Releases (verified with `v0.9.6` CUDA 12.4 on Windows), then download one or more `.gguf` models from HuggingFace `audio-cpp/audio.cpp-gguf` (or ModelScope mirror `ziansoft/audiocpp-tts-gguf`):
   - **Multilingual / Western languages + Chinese**:
     - `Qwen3-TTS-12Hz-1.7B-Base-GGUF/qwen3-tts-12hz-1.7b-base-q8_0_v2.gguf` (2.57 GB, voice cloning, 10+ languages) or `0.6B-Base` (1.90 GB)
     - `Qwen3-TTS-12Hz-1.7B-CustomVoice-GGUF` (built-in speakers, no reference audio needed)
     - `Qwen3-TTS-12Hz-1.7B-VoiceDesign-GGUF` (create voices via text `instructions`, no reference audio needed)
     - `CosyVoice3-GGUF/cosyvoice3-q8_0.gguf` (2.15 GB, fast, multilingual)
     - `Kokoro-82M-GGUF` / `PocketTTS-GGUF` (ultra-lightweight, runs fast even on CPU)
   - **High-fidelity voice cloning**:
     - `IndexTTS2.5-GGUF/index-tts2_5-q8_0.gguf` (3.34 GB, no reference transcript needed)
     - `VoxCPM2-GGUF/voxcpm2-q8_0.gguf` (2.82 GB, 48 kHz studio-grade audio)
     - `M5-TTS-GGUF/m5-tts-50m-voiceclone-q8_0.gguf` (895 MB, very fast)

### 2. Create a Shared Voice Library (`voice-library`)
Create one folder (e.g., `F:/TTS_Test_Lab/voice-library`). **All cloning models in `audio.cpp` share this single folder**:
- Place clean 5–10s reference recordings inside (e.g., `narrator.wav`). **Must be `.wav` format** — `audio.cpp`'s `voice-library` scanner does not recognize `.mp3` files (whereas our official `IndexTTS-Server` supports both `.wav` and `.mp3`).
- Create a plain UTF-8 text file named exactly `prompt_text` (no `.txt` extension), one line per voice in `voice_id|exact spoken transcript` format:
  ```text
  narrator|The exact words spoken in narrator.wav go here.
  ```
> **Important tip**: In-context cloning models (**Qwen3-TTS Base**, **CosyVoice 3**, **VoxCPM 2**, **M5-TTS**) require the transcript in `prompt_text` to match the reference audio word-for-word. Mismatched text causes stuttering or garbled speech. Models like **IndexTTS 2.5**, **PocketTTS**, and our official **IndexTTS-Server 1.5** do **not** require transcripts.

### 3. Create `server.json`
```json
{
  "host": "0.0.0.0",
  "port": 18080,
  "backend": "cuda",
  "device": 0,
  "threads": 4,
  "lazy_load": true,
  "max_loaded_models": 1,
  "ui": true,
  "ui_management": false,
  "voice_dir": "F:/TTS_Test_Lab/voice-library",
  "models": [
    {
      "id": "qwen3-clone",
      "family": "qwen3_tts",
      "path": "F:/TTS_Test_Lab/models/qwen3-tts-12hz-0.6b-base-q8_0.gguf",
      "task": "tts",
      "mode": "offline",
      "default_request_options": { "seed": 123 }
    },
    {
      "id": "cosyvoice-3",
      "family": "cosyvoice3",
      "path": "F:/TTS_Test_Lab/models/cosyvoice3-q8_0.gguf",
      "task": "clon",
      "mode": "offline",
      "default_request_options": { "seed": 123 }
    },
    {
      "id": "indextts-2_5",
      "family": "index_tts2",
      "path": "F:/TTS_Test_Lab/models/index-tts2_5-q8_0.gguf",
      "task": "tts",
      "mode": "offline",
      "default_request_options": { "seed": 123 }
    }
  ]
}
```
> Tips:
> - Set `"host": "0.0.0.0"` so your phone on LAN can connect.
> - Keep `"max_loaded_models": 1` when configuring multiple large models on a single GPU to avoid VRAM exhaustion or `503 server_busy`.
> - With `"ui": true`, you can open `http://127.0.0.1:18080` in your PC browser to inspect and test models visually.

Start the server:
```powershell
.\audiocpp_server.exe --config .\server.json
```

> **Easier GUI Option (Recommended for Windows)**: If you prefer not to edit JSON configs or run CLI commands manually, try our open-source native Windows desktop console **[AudioCpp-Desktop](https://github.com/sipeter/AudioCpp-Desktop)** (Apache-2.0, all-in-one CUDA 12 bundle available). It offers one-click start/stop, system tray hosting, resumable dual-source model downloads (HuggingFace & ModelScope), visual `voice-library` & `prompt_text` management, and built-in ASR auto-transcription.

### 4. Configure in CloneTTS (Android)
1. Open CloneTTS → **Voice Manager** → **Add Remote Source** → select **OpenAI Compatible**.
2. Fill in:
   - **Name**: e.g. `PC Qwen3-TTS`
   - **Server URL**: `http://<PC-LAN-IP>:18080` (root URL, `/v1`, or full `/v1/audio/speech` are all auto-normalized)
   - **Model**: the `id` from `server.json` (e.g. `qwen3-clone`, `cosyvoice-3`, `indextts-2_5`)
   - **Audio Format**: `WAV` recommended (lossless, lowest latency); `MP3` is also supported via hardware `MediaCodec` decoding.
   - **Style / Emotion Instructions (optional)**: e.g. `Speak warmly and calmly` (only sent when non-empty; supported by OpenAI `gpt-4o-mini-tts`, Qwen3 VoiceDesign/CustomVoice, CosyVoice 3, etc.).
   - **API Key**: leave empty for local LAN servers.
3. Tap **Test Connection** → **Save**.
4. Open the new tab and tap **Refresh Voices** — CloneTTS automatically fetches all voices from `GET /v1/audio/voices` (you can also tap **Add Voice** manually if a third-party server does not expose `/v1/audio/voices`).

---

## Option C: Cloud & Other OpenAI-Compatible TTS Services

Any service accepting standard `POST /v1/audio/speech` works out of the box:
- **OpenAI TTS API** (`https://api.openai.com/v1`, models `tts-1`, `tts-1-hd`, `gpt-4o-mini-tts`, with Bearer API Key and manual voice names like `alloy`, `nova`, `coral`, plus optional `instructions`)
- **vLLM-Omni / SGLang-Omni** self-hosted servers
- **GPT-SoVITS / Fish Speech / Kokoro-FastAPI / Edge-TTS OpenAI proxies**

---

## Offline Automatic Local Fallback

When you leave Wi-Fi or turn off your PC, CloneTTS defaults to **Auto-fallback to local offline voice** (`remote_fallback_enabled`). If a remote request times out or fails to connect, playback seamlessly falls back to your local on-device voice so your reading session never stops. You can toggle this behavior in the remote source dialog.

---

## Verified Compatibility Matrix (RTX 4070 Ti SUPER 16 GB + OnePlus 8T LAN)

| Server | Protocol | Model | Verified Audio Output | Status |
| --- | --- | --- | --- | --- |
| Official `IndexTTS-Server` (CUDA) | `HTTP_INDEXTTS` | IndexTTS 1.5 ONNX | WAV 24 kHz PCM16 | Verified on device |
| Official `IndexTTS-Server` (CUDA) | `HTTP_OPENAI_TTS` | `indextts-1.5` | WAV 24 kHz PCM16 (RTF ~0.50) | Verified on device |
| `audio.cpp v0.9.6` (CUDA 12.4) | `HTTP_OPENAI_TTS` | `index-tts2_5-q8_0.gguf` | WAV 22.05 kHz PCM16 | Verified on device |
| `audio.cpp v0.9.6` (CUDA 12.4) | `HTTP_OPENAI_TTS` | `cosyvoice3-q8_0.gguf` | WAV 24 kHz PCM16 | Verified on device |
| `audio.cpp v0.9.6` (CUDA 12.4) | `HTTP_OPENAI_TTS` | `voxcpm2-q8_0.gguf` | WAV 48 kHz PCM16 | Verified on device |
| `audio.cpp v0.9.6` (CUDA 12.4) | `HTTP_OPENAI_TTS` | `qwen3-tts-12hz-0.6b-base-q8_0.gguf` | WAV 24 kHz PCM16 | Verified on device |
| `audio.cpp v0.9.6` (CUDA 12.4) | `HTTP_OPENAI_TTS` | `m5-tts-50m-voiceclone-q8_0.gguf` | WAV 24 kHz PCM16 | Verified on device |
