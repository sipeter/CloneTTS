# CloneTTS (Voice Cloning Engine)

🌐 **[English](#english-version)** | **[简体中文](#简体中文版)**

---

## 简体中文版

CloneTTS 是一款运行在安卓系统本地的文字转语音 (TTS) 原生引擎。它可以让您在手机上离线克隆所需的声音，并直接使用这个声音来朗读书籍或长文本。

### 一、配置必读：电池优化与后台保活
> **非常重要！必做！**  
在开始一切操作前，为了防止手机在您听书息屏时中断朗读（被系统强杀后台），请务必在应用程序底部的 **“帮助说明”** 页面，参考卡片上的提示：
1. 前往手机系统设置，将本应用的电池优化策略改为 **“无限制”** 或 **“允许完全后台行为”**。
2. 在手机的多任务（最近任务）界面，把本应用 **加锁固定**。

### 二、下载与初次体验
1. **下载与安装**：前往本 GitHub 的 **Releases** 页面下载最新版 `.apk` 并安装。
2. **首次启动**：第一次打开 App 时，系统会在后台解压模型数据，请耐心等待几秒钟。

### 三、如何克隆并添加专属音色
1. 在最下方的 **“音色管理”** 页面中，点击右上角的 **“⋮ (更多选项)”**，选择 **“添加音色”**。
2. **提供参考声音**：使用 **录音室采音** 进行高保真采音，或通过 **外部选取** 上传本地音频（时长要求 1~3 秒，单句清晰的无背景音人声）。
3. **填写发音参考文本**：**一字不差**地填写刚刚录制或上传声音里的纯文本。（文字必须完全匹配，否则发出的声音质量会极度不可视）。
4. **算力精度 (num_steps)**：推荐保留默认的 `4` 步。如果您追求极低的发热开销，可改为 `2`，但音质可能会略微粗糙。
5. 点击底部 **保存并启用** 即可。之后在“音色管理”列表点击该卡片即可激活它。

### 四、高级进阶：发音语调与纠错
在底部 **“发音规则”** 与 **“高级设置”** 栏中，您可以对合成体验进行微调：
- **添加发音替换规则**：如果你发现某个字一直念反了或有特殊读音需求，到 **“发音规则”** 界面点击右上角 **“⋮”** -> **“新添规则”**，支持纯文本和正则表达式替换。
- **系统级纠错大词库**：对于大量的多音字大批量错音，您可以在 **“高级设置”** 选项卡点击 **“系统级纠错大词库”**，手动选取并导入第三方的 `system_dict.json` 强正则补充外挂。
- **高阶断句正则配置**：这个选项决定了引擎切分长句时的停顿感（逗点、句号间断）。点击修改自定义。如果配置出现紊乱，可在此弹窗内点击 **“恢复系统默认”** 一键重置。

### 五、如何接管朗读：开启双擎读卡

**模式 1：接管系统 TTS（兼容性最广）**
在手机的“设置”中，搜索 **“TTS 设置”**（或文字转语音输出），将默认引擎改为 **CloneTTS**。随后在“开源阅读”等应用中点击“朗读”即可生效，完美**支持无级调速**体验。

**模式 2：HTTP API 模式（应对极速免停顿）**
切换到 **“高级设置”** 界面并打开首行的 **“本地 HTTP API 服务”**。将记录下的接口地址（如 `http://127.0.0.1:8080/api/tts?text={{speakText}}`）完整填入外部阅读软件（如 Legado）的网络发音设置中即可。

### 六、交流与反馈
- **项目主页**：[https://github.com/sipeter/CloneTTS](https://github.com/sipeter/CloneTTS)
- **官方 QQ 交流群**：`640190207` (欢迎加群探讨技术配置与反馈问题)

### 七、免责声明
本软件所有语音合成渲染及生物特征提取均在本地脱机完成，无法触网且不留存您的私密发音特征。**未经版权所有人明确授权，严禁私自提取或滥用他人的声音进行克隆**。对于各种侵权欺诈或非法使用产生的任何纠纷，概由用户个人自行承担，本软件概不负责。

---

## English Version

CloneTTS is an offline, native Text-to-Speech (TTS) engine for Android devices. It allows you to quickly clone desired voices and utilize them directly to read books or block messages on your phone without an internet connection.

### 1. Mandatory Setup: Battery Optimization & Background Execution
> **CRITICAL! MUST DO DILIGENTLY!**  
Before ANY operations, to prevent the system from inexplicably terminating the reading process when your screen is off, please follow the instructions on the **"Help & About"** tab:
1. Go to your phone's system settings and change this app's battery optimization policy to **"Unrestricted"** or **"Allow background activity"**.
2. **Lock** this app in your phone's recent tasks (multitasking) card list to prevent being swiped away.

### 2. Download & First Launch
1. **Download**: Navigate to the **Releases** page of this GitHub repository, locate the latest `.apk` file, and download it.
2. **Installation**: Tap the downloaded apk file on your Android phone to install it. (Please allow App installation from unknown sources if prompted).
3. **First Launch**: Upon launching the App for the very first time, the underlying system extracts the essential AI model data. Please wait patiently for a few seconds.

### 3. How to Clone & Add a Custom Voice
1. On the **"Voices"** tab at the very bottom, tap the **"⋮ (More options)"** menu in the top right corner and select **"Add Voice"**.
2. **Provide Reference Audio**: Utilize the internal **Studio Recording** for absolute high-fidelity audio capture, or click **Select External** to directly upload a local audio file (Requirement: 1~3 seconds, single clear spoken sentence, no background noise).
3. **Reference Transcribe Text**: Type out the EXACT text spoken in your recorded or uploaded audio piece in the input box. (Note: Everything must match the audio perfectly letter-by-letter; otherwise the synthetic quality will suffer significantly).
4. **Compute Precision (num_steps)**: It's highly recommended keeping the default `4` steps. If you are pursuing extreme response speed and cool battery temperatures, you can alter it to `2`, though the audio quality may appear somewhat distorted.
5. Tap **Save & Enable** at the bottom. The newly generated voice card will appear in the "Voices" list. Tap on it again to verify that it is the active system voice.

### 4. Advanced Settings: Pronunciation Tuning & Regular Corrections
In the **"Rules"** and **"Engine Settings"** tab, you can micro-adjust the entire synthesis sensation:
- **Punctual Custom Phonetic Rules**: If there is a particular character natively mispronounced, navigate to the **"Rules"** panel, tap **"⋮"** in the top right -> **"Add Rule"**. Both literal plain text and regex substitutions carry out flawlessly.
- **System-level Dictionary Patching**: Intended to resolve massive multi-pronunciation character disasters. Simply click **"System Error Correction Dict"** inside **"Engine Settings"** to manually deploy an external, community-driven `system_dict.json` dictionary patch.
- **Intricate Pausing Expressions**: Governs how the algorithmic engine halts sequentially when rendering long compound sentences (e.g. gaps between commas or semicolons). Tap to customize. If you mess up the complex configuration, you can safely reset everything via the **"Restore Default"** command.

### 5. How to Utilize Real-time Reading: Dual-Engine Deployment

**Mode 1: System TTS Architecture (Maximum Cross-app Compatibility)**
Search for **"TTS Settings"** (or Text-to-Speech Output) inside your phone's System "Settings", and designate the preferred engine to **CloneTTS**. Subsequently, tap "Read Aloud" in any third-party app (like Legado, Moon+ Reader) to listen to it immediately! It perfectly bolsters dynamic sliding **Speech Rate adjustments**.

**Mode 2: Local HTTP API Server (Zero-Latency Interception)**
Switch over to the **"Engine Settings"** tab and slide on the topmost switch for the **"Local HTTP API Service"**. Key in the API address string displayed just below into external reading apps' web-TTS configurations (e.g., `http://127.0.0.1:8080/api/tts?text={{speakText}}`). 

### 6. Communication & Feedback
- **Project Homepage**: [https://github.com/sipeter/CloneTTS](https://github.com/sipeter/CloneTTS)
- **Official QQ Group**: `640190207` (For discussions, feature requests, and issue reports)

### 7. Privacy Clauses & Legal Accountability
All local speech synthesis and neural fingerprint extractions inside this software operate strictly offline on your localized device environment. Nothing is backed up onto the untrusted cloud. **Without the copyright owner's valid authorization, strictly NO unauthorized extraction or abuse of any third party's persona is permitted.** The end user is personally and completely accountable for any disputes springing up from multifarious infringement operations.
