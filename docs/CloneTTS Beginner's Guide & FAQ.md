# CloneTTS 新手使用指南与常见问题 (FAQ)
# CloneTTS Beginner's Guide & FAQ

🌐 **Language / 语言**: [English](#english) | [简体中文](#简体中文)

---

<a name="english"></a>

# English

💡 **Version Notice**
This guide is written based on the latest stable release of CloneTTS. For the best experience and to avoid known issues that have already been fixed, please always keep the app updated to the latest version.

Welcome to CloneTTS! This is a native Text-to-Speech (TTS) engine that runs entirely on-device on Android. Without any internet connection, it can clone your personal voice from just a few seconds of audio sample, and connect directly with popular reading apps for audiobook playback.

---

## 🌟 Part 1: Installation & Initial Setup

### 1. Download & Install
- Download the latest version from the pinned link in our channel or from the GitHub Releases page.
- First launch: When you open the app for the first time, it will unpack model data in the background — please wait a moment.
- **⚠️ Performance Test (Very Important)**: After installation, run the "Performance Test" from the startup guide. If your **RTF value is greater than 1**, your device may not have sufficient processing power for smooth real-time speech synthesis.

### 2. 🔋 Battery Optimization & Background Persistence (Required!)
To prevent the system's power-saving mechanism from killing the TTS engine while you listen with the screen off, you **must** complete both of the following:
- **Disable battery restrictions**: Go to System Settings → App Management → find CloneTTS → set battery optimization to "Unrestricted" or "Allow full background activity."
- **Lock the app in Recents**: In the recent apps (multitasking) screen, lock CloneTTS (usually by swiping down on the card or tapping the lock icon — varies by device manufacturer).

---

## 🌟 Part 2: App Interface & Core Features

The app is organized into 4 main sections via the bottom navigation bar:

### 1. Voice Hall

This is where you manage and preview all your available voices, and monitor the engine log during playback.

#### 🎙️ How to Create a Custom Voice (Add a Voice)
On the "Voice Hall" page, tap the top-right "More options (⋮)" → "Add Voice."
First, you need to obtain a reference audio clip (up to 30 seconds):
- **Option 1 (Record in-app)**: Tap to open the built-in recorder, then record 1–3 seconds of speech. Play it back to check quality. Re-record if not satisfied, then tap Save. The remaining configuration steps are the same as Option 2.
- **Option 2 (Import external file)**: Select an audio file from your device. The speech must be extremely clear with **absolutely no** background music or noise. A single sentence is ideal; 3–5 seconds is the sweet spot (even 1 second can work on low-end devices).
- **Audio Processing (Optional)**: If the audio quality is poor, use this tool to apply noise reduction and silence removal. Adjust the intensity level and preview the result. Tap Save if satisfied, or "Restore Default" to revert to the original.
- **Reference Text (Critical)**: You must transcribe, word-for-word, exactly what is spoken in the audio. Any mismatch between the text and the audio will significantly degrade synthesis quality or cause garbled output.
- **Inference Steps (num_steps)**: Default is 4, which balances quality and speed. If your device is powerful, you can increase to 8. Higher values yield diminishing returns while substantially increasing generation time.

Tap "Save & Enable" at the bottom when done.

#### 🔄 Import, Export & Batch Management
- **Import/Export**: From the top-right menu, you can import `.zip` voice packages or export existing voices. When importing, you can import all at once or in batches, and choose between "Overwrite" or "Append" mode. When exporting, freely select which voices to share.
- **Batch Management**: Under "Batch Management" in the top-right menu, you can batch-enable, disable, delete, or change the inference precision of multiple voices. You can also select multiple voices for "round-robin" dialogue playback (alternating voices), though this is a basic feature. *For high-quality multi-character audiobook narration, use the HTTP API mode with a compatible reader plugin.*

#### 🎛️ Voice List & Engine Log Console
- **Reordering**: Voices are displayed in the list (newest at top). Long-press a card to drag and reorder.
- **Edit details**: Tap the more menu on a voice card and select "Edit" to change its name, alias, inference precision, and in multi-character setups, its role tags for narrator / age / gender style matching.
- **Re-verify**: In the edit screen, tap "Play Raw" to listen to the original sample and verify text alignment; you can also re-open the Audio Processing panel.
- **Volume Gain**: The default volume is based on the master console volume (up to 200%). If a particular voice is too quiet, open its Edit menu and apply an additional "Volume Gain" of up to 150% (reaching approximately 300% of the original). The delete option is also here.
- **Playback Console (top)**: Contains a preview text field and sliders for speed and volume. Speed adjustment here applies **globally**.
- **Engine Log (bottom)**: Displays real-time scheduling status — whether the engine is ready, how many CPU cores are in use, how many threads are active. Most importantly, you can monitor the **RTF** value to confirm your device can handle real-time synthesis.

### 2. Replacement Rules
A powerful pronunciation correction system:
- Plain-text substitution: e.g., force a mispronounced name to use the correct homophone.
- Advanced regex substitution: for users comfortable with regex, handle unusual novel formatting that confuses the TTS engine.
- Supports bulk import/export of rule sets, and one-tap enable/disable/delete.

### 3. Service & Engine
Core configuration for advanced features:
- **Toggle Local HTTP API Service**: For advanced usage and per-character voice control.
- **Multi-character Reading Config**: Added in v0.6.0. Configure narrator voice, male/female lead, fixed role bindings, and the role-tag parsing rules.
- **Ambience Management**: Configure BGM and SFX mappings for audiobook-style playback.
- **Advanced Sentence-splitting Regex**: Controls the maximum sentence length the engine processes at once. If you accidentally break it, tap "Restore System Default" to recover.
- **Verbose Engine Logging**: Enables more detailed background logs for deep diagnostics.

### 4. About & Help
Quick access to this guide, the disclaimer, and battery/background-keep-alive guides for common device brands.

---

## 📖 Part 3: Integrating with Reading Apps

The app supports two output modes:

### Mode 1: Replace System TTS (Recommended for most users)
The simplest setup. Works with virtually any app that supports the Android system TTS engine — including screen readers and accessibility services.
1. Open your device's Settings and search for "TTS Settings" or "Text-to-Speech Output."
2. Change the preferred engine to CloneTTS.
3. Open your reading app and tap play — speed can be controlled within the reader.
- **⚠️ Known Limitation**: This mode is constrained by Android's system TTS scheduling architecture and **cannot pre-buffer audio**. If your device lacks processing power, noticeable pauses between paragraphs are unavoidable. This is a fundamental system limitation that cannot be resolved in this mode.

### Mode 2: HTTP API Mode (For smooth, seamless playback)
If you want to eliminate paragraph-gap pauses, enable pre-buffered seamless playback, or use the multi-character reading features added in v0.6.0, use this mode.
1. Switch to the "Service & Engine" tab in CloneTTS and enable "Local HTTP API Service."
2. Note the local IP and port shown on screen (typically `127.0.0.1:PORT`).
3. Open an app like Legado (开源阅读) → navigate to its "Network TTS" settings → tap "Import from URL" in the top-right menu.
4. Choose the route based on your actual use case:
	- **Single default voice (recommended)**: Import `http://127.0.0.1:PORT/api/legado/default`. This route always follows the currently selected default voice in CloneTTS.
	- **Manual switching between many voices**: Import `http://127.0.0.1:PORT/api/legado/all`. This exports all readable voices into the reader as separate entries.
	- **Multi-character reading**: First tag your voices inside CloneTTS and configure narrator / fixed roles if needed, then import `http://127.0.0.1:PORT/api/legado/multirole`.
5. Two important notes for multi-character reading:
	- CloneTTS did not have this current multi-character workflow before v0.6.0. These are newly added features and should be set up from scratch.
	- Voice pools are optional fallback logic. The new multi-character matching checks voice tags first, then falls back to pools if needed.
6. Once imported correctly, the reader can stream local audio with pre-buffering. If you use the default or multi-character route, you no longer need to import every voice into the reader first.

---

## ❓ Part 4: Frequently Asked Questions (FAQ)

> 🚨 **Before You Ask (Please Read):**
> Before reporting an issue or asking for help, **please read this guide carefully first.** Check the following:
> 1. Did you run the "Performance Test"? Is your device simply too slow (RTF > 1) to handle real-time synthesis?
> 2. Are you on the latest version? Many reported issues may already be fixed in the latest release.
> 3. When seeking help, **always provide context**: your device model, CloneTTS version number, which mode you're using (System TTS or HTTP API), etc. Questions like "it doesn't work" or "no sound" with no details cannot be answered.

**Q1: No audio at all? Doesn't work in my reading app? Keeps cutting out?**
A1: Please re-read Parts 1 and 3 of this guide carefully.
1. Have you correctly set the battery to "Unrestricted" background mode?
2. Did you run the Performance Test? A device with RTF > 1 may time out before synthesis completes.
3. Check the log console on the home screen for errors, and verify your setup matches the correct mode workflow.

**Q2: Why is playback extremely slow? Paragraphs have 10–30 second gaps between them?**
A2: See Part 3 for the explanation of Mode 1 vs Mode 2. In short: your device lacks sufficient power for real-time synthesis in System TTS mode, which cannot pre-buffer. To fix paragraph gaps, switch to HTTP API mode with pre-buffering, or reduce the voice's inference precision (num_steps).

**Q3: I can't adjust the volume / the voice is too quiet. What do I do?**
A3: See Part 2 of this guide. The reader app's volume is hardware-limited. CloneTTS provides per-voice Volume Gain of up to 150–300% in the voice's Edit menu.

**Q4: The synthesized voice sounds robotic, noisy, or has a weird tone. How do I fix it?**
A4: Most likely, your reference audio is low quality, or the reference text does not match the recording. Ensure the audio is clean with only the human voice — no background music or reverb. Use the built-in Noise Reduction tool, and re-enter the exact matching reference text.

**Q5: The TTS cuts off mid-sentence or skips words. What do I do?**
A5: The engine splits sentences based on punctuation. Novels with no punctuation (only spaces) can trigger buffer overflow. Go to "Service & Engine" and modify or restore the sentence-splitting regex, or switch to a reading app with better text-segmentation support.

If none of the above resolves your issue, please post in the QQ channel with your device model, app version, and a detailed description of the situation. We will be happy to help!

---

<a name="简体中文"></a>

# 简体中文

💡 **版本适用说明**
本手册当前的操作说明基于最新的 CloneTTS 稳定版编写。为了获得最佳体验并避免遇到已修复的问题，请务必保持应用更新到最新版本。

欢迎使用 CloneTTS！这是一款运行在安卓系统本地的文字转语音 (TTS) 原生引擎。它可以在无网络的情况下，只需为您提供几秒钟的声音样本，就能在手机本地克隆您的专属音色，并直接对接各大阅读软件进行听书。

---

## 🌟 一、安装与重要配置

### 1. 下载与安装
- 您可以通过我们频道的置顶链接或 GitHub Releases 页面下载安装最新版。
- 首次启动：第一次打开 App 时，系统会在后台解压模型数据，请耐心等待片刻。
- **⚠️ 性能测试（非常重要）**：安装完成后，请务必使用开机引导中的"性能测试"进行测速。如果测试出的 **RTF 值大于 1**，则表明您的手机可能无法正常使用本软件进行流畅的实时朗读。

### 2. 🔋 电池优化与后台保活（必做！）
为了防止您息屏听书期间，手机的省电机制强行彻底关闭发音引擎导致听书突然中断，请**务必**完整进行以下两项设定：
- **修改电池限制**：前往手机的系统设置 -> 应用管理 -> 找到 CloneTTS，将其电池优化策略改为"无限制"或"允许完全后台行为"。
- **锁定多任务卡片**：在手机的后台多任务（最近任务）卡片界面给本应用上锁（一般为下拉卡片或点击卡片上的小锁图标，不同手机品牌操作略有不同）。

---

## 🌟 二、软件界面与核心功能

本软件的界面主要构成由底部导航栏分为 4 个主要板块，请根据您的需求进入相应的页面：

### 1. 音色大厅

这里用于管理、试听您的所有可用音色，并在运行期间查看引擎日志。

#### 🎙️ 如何制作专属音色？（添加音色）
在"音色大厅"页面，点击右上角的"更多选项 (⋮)" -> "添加音色"。
首先需要获取参考音频（最长支持 30 秒）：
- **方式一（录音室采音）**：点击进入内置录音机，点击开始键录制 1~3 秒声音，录完后可播放回听。如果不满意可以重复录制，满意后点击保存。后续的配置项与外部选取模式一致。
- **方式二（外部选取）**：从手机选择外部音频文件。对于语音的要求是发音吐字必须极其清晰，**绝不可有**任何背景音乐或噪音干扰。单句话为佳，音频时间无需太长，3 到 5 秒最好（如果手机配置低，1 秒的素材也可以使用）。
- **音频加工（可选功能）**：如果所选用/录制的音频质量较差，可使用此工具进行加工，包含消除底噪和静音消除。可以根据需求调整强度级别并试听。如果满意可以点击保存；如果不满意，点击"恢复默认"即可将参考音频复原为最初状态。
- **填写参考文本（极其关键）**：您必须一字不差地将刚刚录下或上传声音里面说的话转录成纯文本。如果文本内容和录音对不上，模型发音质量会大幅下降甚至出现诡异发音。
- **算力精度设定 (num_steps)**：默认推荐为 4，兼顾音质和极快的生成速度。若是手机性能较好，可设为 8。数值更高不仅听感没有明显提升，且耗时会大幅度增加。

完成后点击底部"保存并启用"即可。

#### 🔄 音色的导入导出与批量管理
- **导入/导出**：在右上角菜单中，我们可以导入 `.zip` 格式的音色包，也可以将已有音色导出保存。导入时，可以一次性导入全部也可以分批导入，并可选择是"覆盖导入"还是"追加导入"。导出时可自由勾选部分音色分享给朋友。
- **批量管理**：在右上角的"批量管理"中，可以对音色进行批量停用、启用、删除，或者批量修改算力精度。也可以选中多个音色进行"轮播图"模式的对话播放（你读一段我读一段的方式），不过效果较为初级。*如需高质量多角色朗读书籍，建议使用 HTTP API 调用相关的阅读器插件。*

#### 🎛️ 音色列表展示与日志控制台
- **音色排序**：添加好的音色会在音色列表中集中展示（新加的通常在最上方），您可以按住卡片自如拖动调整它们的相互排序。
- **编辑详情**：点击某一音色右侧的更多菜单并选择"编辑"，您可以修改它的名称、代号（Alias）、算力精度；在支持多角色的版本中，也可以为音色补充角色标签，用于自动匹配旁白、男女老少等角色属性。
- **重新校验**：在编辑界面，您还可以点击"Play Raw"在此收听原始素材以核对外文文本是否匹配；也可以再次进入"音频加工"面板。
- **音量增益调节**：默认的音量是基于控制台总音量（最高 200%）发出的。若遇到个别声音实在太小，可在该音色的编辑菜单内额外调节"音量增益"，在此还能额外最高再增加扩音 150%（相当于最多提升到了原先的 300% 左右）。单点删除该卡片功能也在此处。
- **听控台功能（顶部）**：包含"试听文本"框和速度、音量的滑块。这个播放速度的调整是优先对**全局生效**的。
- **运行日志（底部）**：用于实时观察发音调度状态。引擎是否就绪、占用了几颗 CPU 核心、开启了几个线程都会在这里打印出来。最重要的是，您可以观察这里打印出的具体 **RTF** 值是否够维持您的实际实时朗读需求。

### 2. 替换规则
用于强大的自主发音纠错规划。
- 纯文本替换：例如将错误的特定人名拼音强制纠正为正确同音字。
- 高级正则替换：若您有一定语法基础，可用正则表达式处理各类小说作者独特的排版导致的发音乱局。
- 此板块下我们同样提供了规则包批量导入导出、一键启用/禁用与删除的功能。

### 3. 服务与引擎
配置高级服务的核心层级：
- **开启关闭本地 HTTP API 服务**：用于进阶使用和分角色控制。
- **多角色朗读配置**：0.6.0 新增。用于设置旁白音色、男主/女主、固定角色绑定，以及角色标签解析规则。
- **氛围音管理**：用于配置 BGM 与 SFX 事件映射，适合小说的背景音乐和音效场景。
- **高阶断句正则配置**：决定引擎一次性能够处理的最大长句门槛。如果自己改乱了倒读错读，可随时点击"恢复系统默认"急救。
- **详细推理引擎日志**：启用后，为您开启更细致的后台输出日志，用于深入排查运算信息。

### 4. 关于与帮助
收录了本指南、免责声明、各类常见机型的详细保电杀后台求生指南的快捷传送门。

---

## 📖 三、如何在阅读软件中应用

本应用支持两种不同的发声输出模式：

### 模式 1：接管系统默认 TTS（普通用户适用）
这是最简单、最方便直接的设置模式。适用于市面上绝大部分支持普通系统语音朗读的应用，甚至可以全盘接替系统 TTS 引擎执行诸如屏幕朗读、无障碍服务播报等任务。
1. 手机内进入系统"设置"搜索打开"TTS 设置"或"文字转语音输出"。
2. 将操作系统的首选发音引擎变更为 CloneTTS。
3. 打开阅读软件直接点击听书发音即可，语速在阅读器内部即可自由控制。
- **⚠️ 核心缺陷提醒**：此模式严重受制于安卓系统规范的调度限制，**无法做任何提前预判缓存处理**。如果您的手机运行速度或算力不够拔尖，"段落与段落之间的明显停顿延迟"是在所难免的，这是系统架构决定的客观现象，无法在这种模式下解决！

### 模式 2：HTTP API 进阶模式（追求平滑连贯体验）
如果您希望彻底打破模式 1 中带来的"段落间的长时间停顿"，实现行云流水的提前缓存接续朗读功能，或者想要使用 0.6.0 新加入的多角色朗读能力，您可以采用此模式。
1. 切换到 CloneTTS 的"服务与引擎"界面，打开"本地 HTTP API 服务"。
2. 记下界面上给出的本机 IP 和端口地址（通常为 `127.0.0.1:端口号`）。
3. 打开类似 Legado（开源阅读）的"网络发音 (网络发音引擎)"界面，选择右上角菜单中的"网络导入"。
4. 根据您的需求，选择导入不同的路由：
	- **单音色听书（最推荐）**：导入 `http://127.0.0.1:端口号/api/legado/default`。这条规则不绑定具体音色，而是始终跟随 CloneTTS 当前选中的默认音色。
	- **手动切换多个音色**：导入 `http://127.0.0.1:端口号/api/legado/all`。这会把当前可朗读音色全部导入到阅读器里，适合手动选人声。
	- **多角色朗读**：先在 CloneTTS 里给音色打好角色标签，并按需配置旁白、男主女主、固定角色后，再导入 `http://127.0.0.1:端口号/api/legado/multirole`。
5. 如果您使用多角色模式，需要记住两点：
	- 0.6.0 之前 CloneTTS 还没有现在这套多角色流程，所以这是新增功能，可以按这里从头配置。
	- 声音池不是上手门槛；多角色会先按音色标签匹配，声音池主要作为兜底层。
6. 正确导入后，阅读器就能直接拉取本地流，实现提前缓存播放；如果您用的是默认路由或多角色路由，也不需要再把所有音色都一次性导入阅读器。

---

## ❓ 四、常见问题解答 (FAQ)

> 🚨 **提问须知（必看）：**
> 在您打算提出各类报错或听书不正常的问题前，**请务必仔细且完整地看完上面的说明指南！** 请您先自查：
> 1. 您是否完成了"开机性能测试"？您机子的配置是否已经太低（RTF > 1）本来就根本无法胜任实时演算朗读？
> 2. 您在使用的是不是最新版本？许多群内反馈的顽疾可能在最新的安装包中已明确修复过。
> 3. 您在寻求互助求解答时，**一定要提供充分的上下文**！诸如："我用的是什么手机型号"，"下载的 CloneTTS 的哪个版号"，"我是在以什么模式应用它的（系统 TTS 还是 HTTP 网络）"等；那些诸如"我的没声了""我的断断续续""静读用不起来"之类什么信息都没有的问题，没有任何人可以帮您盲猜解答。

**Q1：这个软件怎么完全没有声音？静读天下 APP 里用不了？我的也是用不了，断断续续的怎么回事？**
A1：请您仔细查看指南第一部分、第三部分的内容并寻找答案。
1. 查看您是否正确开启了"无限制后台"？
2. 发音是否有进行"性能测试"，您的机器算力太低也会导致无法在正常超时时间内完成合成。
3. 查看首页底部日志区是否有报错；以及您具体应用该软件的使用模式流程是否正确。

**Q2：为什么听书播报速度极其慢？段落与段落之间停顿等待的时间长达十几秒甚至半分钟？**
A2：请您前往查看本指南【三、模式 1 与模式 2 的区别讲解】寻找答案。简单来说：因为您手机性能本来就不够强，并依然在使用"系统 TTS 模式"，受制于系统设计无法提前缓存。若要解决段间停顿问题，请转用性能更强悍的 HTTP API 缓存策略机制或尝试调低音色的"算力精度"。

**Q3：遇到发音问题，为什么我在这无法调整音量？**
A3：请您回到本指南【二、软件界面与核心功能】寻找答案。阅读器系统自带音量有其硬件上限，如果您仍然觉得不够，我们提供在音色列表中针对单一音色高达 150%~300% 的底层音频超强增益设计。请去编辑详情查阅。

**Q4：我合成出的声音夹杂机器音、底噪声和各种奇奇怪怪调调该怎么办？**
A4：极大可能是您参考音档不合格，或者参考文本没有严格一一对应！请保证音频环境只有干干静静的人声清唱，建议回到大厅打开内置工具进行【消除底噪】，重新设定一遍精准匹配文本。

**Q5：长句念到一半会有停顿或者干脆漏字怎么办？**
A5：发音引擎切割音频依赖文本自带标点符号进行截取；遇到满篇只有空格的连体小说容易触发模型截断溢位崩坏。您可以在【服务与引擎】面板内修改或者恢复"断句正则"规则补救，或更换带有连词预测修补服务的第三方专业读书器排查。

如以上方法仍未能定位问题，欢迎在 QQ 频道内发帖探讨并务必遵守提问须知带上您的配置与发生情境，我们将为您详细解答！
