# CloneTTS 0.8.0 PC 端与第三方 OpenAI 服务端接入指南

> 适用版本：CloneTTS Android 0.8.0 及以上  
> 更新日期：2026-11-25

CloneTTS 0.8.0 在保留原有手机端本地离线克隆引擎（CPU / 骁龙 NPU）的同时，新增了**远程 PC 端 / OpenAI 兼容 TTS 接入能力**，支持以下两种远程音色来源：

1. **官方配套 PC 端：[IndexTTS-Server](https://github.com/sipeter/IndexTTS-Server)（专有协议 + OpenAI 兼容双协议）** — 基于 IndexTTS 1.5 的纯 C++ ONNX Runtime 推理引擎与 Windows 原生桌面控制台，免装 Python，支持应用内一键下载魔搭轻量 INT4+FP16 模型包，免填参考文本，支持手机端直接上传参考音频并自动同步音色列表。
2. **通用 OpenAI 兼容 TTS（`POST /v1/audio/speech`）** — 只需填写兼容 OpenAI `/v1/audio/speech` 协议的服务端地址、模型名和音色名，即可直接接入 **[audio.cpp](https://github.com/0xShug0/audio.cpp)**（推荐搭配我们开源的图形化桌面控制台 **[AudioCpp-Desktop](https://github.com/sipeter/AudioCpp-Desktop)**，支持 GGUF 格式的 IndexTTS 2.5 / Qwen3-TTS / M5-TTS / CosyVoice 3 / VoxCPM 2 等）、**vLLM-Omni / SGLang-Omni**、**GPT-SoVITS / Fish Speech OpenAI 兼容包装服务**，以及标准云端 OpenAI TTS 服务。

---

## 一、方案 A：官方配套 `IndexTTS-Server`（开箱即用，推荐首选）

- 开源仓库与下载：**[https://github.com/sipeter/IndexTTS-Server](https://github.com/sipeter/IndexTTS-Server)**（[Releases 下载页](https://github.com/sipeter/IndexTTS-Server/releases)）

### 1. PC 端下载与启动（Windows x64）
1. 前往 [IndexTTS-Server Releases](https://github.com/sipeter/IndexTTS-Server/releases) 下载 Windows CUDA 12 一体包（例如 `IndexTTS-Server-v0.1.0-win-x64-cuda12.7z`，免装 Python / CUDA / .NET）并解压。
2. 双击运行 **`IndexTTS-Desktop.exe`**：
   - **首次下载模型**：切换到 **模型管理** 页，点击 **一键下载模型**，控制台会自动从魔搭社区（`ziansoft/indextts-1.5-onnx-int4`）高速下载约 923 MB 的 INT4+FP16 轻量模型包并静默解压到 `models/` 目录（也支持从本地导入已下载的 `.zip` 模型包）。
   - **启动服务**：回到 **服务与 API** 页，选择计算后端（CUDA / TensorRT / CPU）并点击 **启动服务**：
     - 默认监听：`http://<PC局域网IP>:9966`（可在控制台自定义端口，支持可选 Bearer Token 鉴权）。
     - 同时提供专有管理接口（`/v1/health`、`/v1/tts/refs`、`/v1/tts/upload_ref`、`/v1/tts/synthesize`）与标准 OpenAI 兼容接口（`/v1/models`、`/v1/audio/voices`、`/v1/audio/speech`）。

### 2. 手机端配置步骤
1. 确保手机与 PC 处于同一局域网（Wi-Fi），并放行 PC 防火墙对应端口（默认 `9966`）。
2. 打开 CloneTTS → **音色管理** → 点击右上角菜单 → **添加远程音色源**。
3. 类型选择 **IndexTTS-Server（官方配套）**：
   - **名称**：例如 `家里台式机 IndexTTS`
   - **服务地址**：例如 `http://192.168.1.100:9966`
   - **API Key**：如 PC 端开启了鉴权 Token 则填写，未开启留空即可。
4. 点击 **测试连接**，确认返回健康状态后点击保存。
5. 切换到顶部新出现的远程源标签页，点击 **刷新音色** 即可自动拉取 PC 端 `refs/` 目录下的全部参考音频（支持 `.wav` 与 `.mp3`，无需参考文本），也支持直接点击 **上传参考音频** 从手机端添加新音色。

---

## 二、方案 B：推荐第三方整合服务端 `audio.cpp`（一服多模型 GGUF）

如果想在 PC 上体验 **Qwen3-TTS**、**M5-TTS**、**IndexTTS 2.5**、**CosyVoice 3**、**Fish Audio S2 Pro**、**VoxCPM 2** 等多种最新开源声音克隆模型，且不想折腾繁琐的 Python / PyTorch 环境，推荐使用开源 C++ 引擎 **[audio.cpp](https://github.com/0xShug0/audio.cpp)**。

- **优点**：单可执行文件、纯 C++/CUDA（类似 `llama.cpp`）、全模型统一采用 `.gguf` 单文件格式、**原生内置标准 OpenAI `POST /v1/audio/speech` 接口**、支持按需懒加载与多模型切换。

### 1. 下载服务端与模型（Windows NVIDIA GPU 示例）
1. **图形化一键方式（推荐）**：直接前往 **[AudioCpp-Desktop Releases](https://github.com/sipeter/AudioCpp-Desktop/releases)** 下载免装 CUDA 12 一体包，开箱即带 `audiocpp_server.exe` 与原生桌面控制台，可在界面内直接从魔搭（国内高速）或 HuggingFace 一键断点续传下载全部模型，无需手动编写 JSON 配置。
2. **手动命令行方式**：从 `audio.cpp` GitHub Releases 下载对应 CUDA 版本的 Windows 压缩包（已实测 `v0.9.6` CUDA 12.4 版本），再从 HuggingFace `audio-cpp/audio.cpp-gguf` 或魔搭镜像 `ziansoft/audiocpp-tts-gguf` 下载所需 `.gguf` 量化模型（8 GB 及以上显存推荐 `Q8_0`）：
   - **M5-TTS 50M VoiceClone**（`m5-tts-50m-voiceclone-q8_0.gguf`，约 895 MB，轻量极速）
   - **Qwen3-TTS 0.6B Base**（`qwen3-tts-12hz-0.6b-base-q8_0.gguf`，约 1.90 GB，中英自然度高）
   - **IndexTTS 2.5**（`index-tts2_5-q8_0.gguf`，约 3.34 GB，情感与音色还原强）

### 2. 准备参考音频目录（`voice-library`）
创建一个文件夹（例如 `F:/TTS_Test_Lab/voice-library`），放入：
- 参考音频文件：`ln.wav`（**必须为 `.wav` 格式**，`audio.cpp` 的 `voice-library` 目录扫描不识别 `.mp3`；建议 5~10 秒清晰人声。若使用官方 `IndexTTS-Server` 则 `.wav` 与 `.mp3` 均支持）
- 同目录新建纯文本文件 `prompt_text`（无后缀，UTF-8 编码），每行格式为 `音色ID|参考音频对应的真实文本`：
  ```text
  ln|这里是参考音频里实际说出的那句话。
  ```
> **实测避坑提示**：`Qwen3-TTS Base`、`M5-TTS` 等上下文续写类克隆模型**高度依赖参考文本准确性**，`voice-library/prompt_text` 中的文字**必须与参考音频实际说的内容逐字一致**；若文本不匹配，`Qwen3-TTS` 会出现明显卡顿跳帧，`M5-TTS` 会出现胡言乱语。如果希望免填参考文本、直接丢音频文件就克隆，推荐优先使用官方配套的 `IndexTTS-Server`。

### 3. 编写服务端配置文件（`clonetts_audiocpp.json`）
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
      "id": "m5-clone",
      "family": "m5_tts",
      "path": "F:/TTS_Test_Lab/models/m5-tts-50m-voiceclone-q8_0.gguf",
      "task": "clon",
      "mode": "offline",
      "default_request_options": {
        "language": "zh",
        "seed": 2026
      }
    },
    {
      "id": "qwen3-clone",
      "family": "qwen3_tts",
      "path": "F:/TTS_Test_Lab/models/qwen3-tts-12hz-0.6b-base-q8_0.gguf",
      "task": "tts",
      "mode": "offline",
      "default_request_options": {
        "seed": 123
      }
    }
  ]
}
```
> 注意：`host` 必须设为 `"0.0.0.0"`，局域网内的手机才能访问。

启动命令：
```powershell
.\audiocpp_server.exe --config .\clonetts_audiocpp.json
```
> 提示：配置文件中开启 `"ui": true` 后，浏览器访问 `http://127.0.0.1:18080` 即可打开 `audio.cpp` 内置的 Web 控制台进行调试试听；多模型混用时建议保持 `"max_loaded_models": 1`，避免单卡同时驻留多个大模型导致显存吃紧或返回 `503 server_busy`。
>
> **更省心的图形化方式（推荐）**：如果不想手动编辑 JSON 配置与命令行启动，可以直接使用我们开源的 Windows 原生桌面控制台 **[AudioCpp-Desktop](https://github.com/sipeter/AudioCpp-Desktop)**（Apache-2.0，提供免装 CUDA 12 一键整合包），支持一键启停、系统托盘常驻、魔搭/HuggingFace 双源断点续传下载模型、可视化管理 `voice-library` 与 `prompt_text`，以及内置 ASR 自动识别参考文本。

### 3.1 常见模型是否需要参考文本（`prompt_text`）速查
| 模型 | 是否需要 `prompt_text` | 备注 |
| --- | --- | --- |
| **IndexTTS 1.5**（官方配套 PC 端） | **不需要** | 直接放 WAV/MP3 即可，最省心 |
| **IndexTTS 2.5**（`audio.cpp`） | **不需要** | 纯参考音频即可克隆音色与情感 |
| **CosyVoice 3**（`audio.cpp`） | **需要逐字一致** | 速度快（24 kHz），自然度高 |
| **VoxCPM 2**（`audio.cpp`） | **需要逐字一致** | 48 kHz 高解析音频，表现力强 |
| **Qwen3-TTS Base**（`audio.cpp`） | **需要逐字一致** | 0.6B / 1.7B 均有 GGUF |
| **M5-TTS 50M**（`audio.cpp`） | **需要逐字一致** | 体积仅 ~895 MB，轻量极速 |

### 4. 手机端配置步骤
1. 打开 CloneTTS → **音色管理** → **添加远程音色源**。
2. 类型选择 **OpenAI 兼容接口**：
   - **名称**：例如 `PC audio.cpp (M5)`
   - **服务地址**：`http://<PC局域网IP>:18080`（填写根地址、`/v1` 或完整 `/v1/audio/speech` 均可，App 会自动规范化）
   - **模型名（model）**：填写配置文件中的模型 `id`，例如 `m5-clone` 或 `qwen3-clone`
   - **返回格式（response_format）**：推荐 `wav`（无损、零解码延迟；也支持 `mp3` 通过系统硬件解码）
   - **API Key**：局域网直连留空即可。
3. 点击 **测试连接** 确认正常后保存。
4. 进入该源标签页，点击 **刷新音色** 即可通过 `GET /v1/audio/voices` **自动拉取** `voice-library` 中配置好的全部音色（如 `ln`、`demo`），无需手动逐个填写；同时也支持点击 **添加音色** 手动补充自定义音色 ID，添加或拉取后即可像本地音色一样点击试听、设为默认、在阅读器（Legado / 系统 TTS / HTTP 服务）和多角色配音中使用。

---

## 三、方案 C：其他兼容 OpenAI `/v1/audio/speech` 的本地或云端服务

只要服务支持标准 OpenAI TTS 请求格式：
- `POST <baseUrl>/v1/audio/speech`
- 请求体：`{"model": "...", "input": "...", "voice": "...", "response_format": "wav"|"mp3", "speed": 1.0}`
- 鉴权头（可选）：`Authorization: Bearer <apiKey>`
- 返回体：标准 WAV（PCM 16-bit / Float32 / 流式未知长度）或 MP3/AAC 音频流

均可直接通过 **OpenAI 兼容接口** 接入，包括但不限于：
- **vLLM-Omni / SGLang-Omni**（部署 Qwen3-TTS / CosyVoice 等）
- **GPT-SoVITS / Fish Speech / CosyVoice** 的 OpenAI API 兼容代理或包装服务
- **云端 OpenAI 兼容 TTS 服务**（如 OpenAI TTS、SiliconFlow、Fish Audio 等）

---

## 四、离线自动回退（本地兜底）

在系统 TTS 朗读或内置 HTTP 服务（Legado 接入）中使用远程音色时，如果外出离开 Wi-Fi 或 PC 端已关机：
- 默认开启 **远程失败自动回退本地音色**（`remote_fallback_enabled`），当远程请求连接失败或超时后，CloneTTS 会自动无缝回退到当前选中的本地离线音色继续朗读，不会中断听书体验。
- 如希望严格报错提示而不回退，可在设置中关闭该选项。

---

## 五、本机实测兼容性记录（RTX 4070 Ti SUPER 16 GB）

| 服务端 | 协议类型 | 模型 / 引擎 | 实测接口 | 返回格式 | 结果 |
| --- | --- | --- | --- | --- | --- |
| 官方 `IndexTTS-Server` (C++ CUDA) | `HTTP_INDEXTTS` | IndexTTS 1.5 ONNX CUDA | `/v1/health`, `/v1/tts/refs`, `/v1/tts/synthesize` | WAV 24kHz PCM16 | 通过 |
| 官方 `IndexTTS-Server` (C++ CUDA) | `HTTP_OPENAI_TTS` | `indextts-1.5` | `/v1/models`, `/v1/audio/voices`, `/v1/audio/speech` | WAV 24kHz PCM16 (RTF ~0.64) | 通过 |
| `audio.cpp v0.9.6` (CUDA 12.4) | `HTTP_OPENAI_TTS` | `m5-tts-50m-voiceclone-q8_0.gguf` | `/health`, `/v1/models`, `/v1/audio/speech` | WAV 24kHz PCM16 | 真机通过 |
| `audio.cpp v0.9.6` (CUDA 12.4) | `HTTP_OPENAI_TTS` | `qwen3-tts-12hz-0.6b-base-q8_0.gguf` | `/health`, `/v1/models`, `/v1/audio/speech` | WAV 24kHz PCM16 | 真机通过 |
| `audio.cpp v0.9.6` (CUDA 12.4) | `HTTP_OPENAI_TTS` | `index-tts2_5-q8_0.gguf` | `/health`, `/v1/models`, `/v1/audio/speech` | WAV 22.05kHz PCM16 | 真机通过 |
| `audio.cpp v0.9.6` (CUDA 12.4) | `HTTP_OPENAI_TTS` | `cosyvoice3-q8_0.gguf` | `/health`, `/v1/models`, `/v1/audio/speech` | WAV 24kHz PCM16 | 真机通过 |
| `audio.cpp v0.9.6` (CUDA 12.4) | `HTTP_OPENAI_TTS` | `voxcpm2-q8_0.gguf` | `/health`, `/v1/models`, `/v1/audio/speech` | WAV 48kHz PCM16 | 真机通过 |
