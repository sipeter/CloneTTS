# CloneTTS (Voice Cloning Engine)

🌐 **[English](#english-version)** | **[简体中文](#简体中文版)**

---

## 简体中文版

CloneTTS 是一款运行在安卓系统本地的文字转语音 (TTS) 原生引擎。它可以让您在手机上离线克隆所需的声音，并直接使用这个声音来朗读书籍或长文本。无需联网，所有推理计算均在本地完成。

### 功能特色

- **离线音色克隆**：录制 1~3 秒人声即可生成专属音色
- **系统级 TTS**：注册为安卓系统 TTS 引擎，兼容开源阅读、Moon+ 等阅读器
- **HTTP API 服务**：内置本地 HTTP 服务器，支持 GET 和 POST 请求
- **多音色轮换**：启用多个音色后自动按句切换，适合多角色听书
- **语速与音量调节**：0.5x ~ 2.0x 语速，0% ~ 200% 音量
- **发音纠错**：自定义替换规则（纯文本或正则），可导入多音字修正词库
- **音色备份**：ZIP 格式导入/导出，支持追加或覆盖导入

### 一、配置必读：电池优化与后台保活
> **非常重要！必做！**  
在开始一切操作前，为了防止手机在您听书息屏时中断朗读（被系统强杀后台），请务必在应用程序底部的 **“帮助说明”** 页面，参考卡片上的提示：
1. 前往手机系统设置，将本应用的电池优化策略改为 **“无限制”** 或 **“允许完全后台行为”**。
2. 在手机的多任务（最近任务）界面，下拉本应用的卡片或点击小锁图标，防止被一键清理掉（不同品牌操作方式略有不同，部分手机可能没有此功能）。

### 二、下载与初次体验
1. **下载与安装**：前往本 GitHub 的 **Releases** 页面下载最新版 `.apk` 并安装。
2. **首次启动**：第一次打开 App 时，系统会在后台解压模型数据，请耐心等待几秒钟。

### 三、如何克隆并添加专属音色
1. 在最下方的 **“音色管理”** 页面中，点击右上角的 **“⋮ (更多选项)”**，选择 **“添加音色”**。
2. **提供参考声音**：使用 **录音室采音** 进行高保真采音，或通过 **外部选取** 上传本地音频（时长要求 1~3 秒，单句清晰的无背景音人声）。
3. **填写发音参考文本**：**一字不差**地填写刚刚录制或上传声音里的纯文本。（文字必须完全匹配，否则发出的声音质量会极度不可视）。
4. **算力精度 (num_steps)**：推荐保留默认的 `2` 步以获得最佳速度体验。如果您追求更高音质，可改为 `4`，但合成速度会相应变慢。最大值为 `8`，超过 8 不会有进一步的音质提升。
5. 点击底部 **保存并启用** 即可。之后在"音色管理"列表点击该卡片即可激活它。

### 四、音色管理
- **选择音色**：点击音色卡片即可选中为当前音色
- **多音色轮换**：长按音色卡片可启用/取消多音色轮换模式，朗读时自动按句切换音色
- **编辑/删除**：点击音色卡片右侧的 ⋮ 菜单进行编辑或删除
- **批量操作**：支持批量删除、批量导出
- **导入/导出**：通过 ⋮ 菜单导出为 ZIP 备份文件，导入时可选择追加或覆盖模式

### 五、高级进阶：发音语调与纠错
在底部 **“发音规则”** 与 **“高级设置”** 栏中，您可以对合成体验进行微调：
- **添加发音替换规则**：如果你发现某个字一直念错或有特殊读音需求，到 **"发音规则"** 界面点击右上角 **"⋮"** -> **"新添规则"**，支持纯文本和正则表达式替换。
- **系统级纠错大词库**：对于大量的多音字批量错音，您可以在 **"高级设置"** 选项卡点击 **"系统级纠错大词库"**，手动选取并导入第三方的 `system_dict.json` 强正则补充词库。
- **高阶断句正则配置**：这个选项决定了引擎切分长句时的停顿感（逗点、句号间断）。点击修改自定义。如果配置出现紊乱，可在此弹窗内点击 **"恢复系统默认"** 一键重置。
- **推理日志开关**：在高级设置中可开启详细日志，查看每句的 RTF（实时率）和 Encoder/Decoder 耗时。

### 六、如何接管朗读

**模式 1：接管系统 TTS（兼容性最广）**
在手机的"设置"中，搜索 **"TTS 设置"**（或文字转语音输出），将默认引擎改为 **CloneTTS**。随后在"开源阅读"等应用中点击"朗读"即可生效，支持**无级调速**体验。

**模式 2：HTTP API 模式（适合高级用户）**
切换到 **"高级设置"** 界面并打开首行的 **"本地 HTTP API 服务"**。将显示的接口地址填入外部阅读软件（如 Legado）的网络发音设置中即可。

支持两种请求方式：
- **GET 请求**：`http://127.0.0.1:8080/api/tts?text={{speakText}}&voice=音色名`
- **POST 请求**：支持 `application/x-www-form-urlencoded` 和 `application/json` 两种格式，可发送超长文本

`voice` 参数支持音色名称或音色代号 (Alias) 匹配。

### 七、交流与反馈
- **项目主页**：[https://github.com/sipeter/CloneTTS](https://github.com/sipeter/CloneTTS)
- **官方 QQ 交流群**：`640190207` (欢迎加群探讨技术配置与反馈问题)

### 八、免责声明
本软件所有语音合成渲染及生物特征提取均在本地脱机完成，无法触网且不留存您的私密发音特征。**未经版权所有人明确授权，严禁私自提取或滥用他人的声音进行克隆**。对于各种侵权欺诈或非法使用产生的任何纠纷，概由用户个人自行承担，本软件概不负责。

---

## English Version

CloneTTS is an offline, native Text-to-Speech (TTS) engine for Android devices. It allows you to clone desired voices and use them directly to read books or long texts on your phone — no internet connection required. All inference runs locally on-device.

### Key Features

- **Offline Voice Cloning**: Record 1~3 seconds of speech to create a custom voice
- **System TTS**: Registers as an Android system TTS engine, compatible with Legado, Moon+ Reader, etc.
- **HTTP API Server**: Built-in local HTTP server supporting GET and POST requests
- **Multi-Voice Rotation**: Enable multiple voices for automatic per-sentence switching
- **Speed & Volume Control**: 0.5x–2.0x speech rate, 0%–200% volume
- **Pronunciation Correction**: Custom text/regex replacement rules, with importable polyphone dictionary
- **Voice Backup**: ZIP-based export/import with append or overwrite modes

### 1. Mandatory Setup: Battery Optimization & Background Execution
> **CRITICAL! MUST DO!**  
Before any operations, to prevent the system from killing the reading process when your screen is off, please follow the instructions on the **"Help & About"** tab:
1. Go to your phone's system settings and change this app's battery optimization policy to **"Unrestricted"** or **"Allow background activity"**.
2. In your phone's recent tasks (multitasking) view, swipe down on the app card or tap the lock icon to prevent it from being cleared (varies by device brand; some phones may not have this option).

### 2. Download & First Launch
1. **Download**: Navigate to the **Releases** page of this GitHub repository and download the latest `.apk` file.
2. **Installation**: Tap the downloaded APK to install. (Allow installation from unknown sources if prompted.)
3. **First Launch**: On the very first launch, the app extracts AI model data in the background. Please wait a few seconds.

### 3. How to Clone & Add a Custom Voice
1. On the **"Voices"** tab, tap **"⋮ (More options)"** in the top right and select **"Add Voice"**.
2. **Provide Reference Audio**: Use the built-in **Studio Recording** for high-fidelity capture, or **Select External** to upload a local audio file (1~3 seconds, single clear sentence, no background noise).
3. **Reference Transcript**: Type the EXACT text spoken in your audio. (Must match perfectly — mismatches will severely degrade synthesis quality.)
4. **Compute Precision (num_steps)**: Default `2` is recommended for best speed. Set to `4` for higher quality at the cost of slower synthesis. Maximum is `8`; values above 8 do not improve quality further.
5. Tap **Save & Enable**. The new voice card appears in the list — tap it to make it the active voice.

### 4. Voice Management
- **Select Voice**: Tap a voice card to set it as the active voice
- **Multi-Voice Rotation**: Long-press a voice card to toggle multi-voice rotation mode — sentences auto-switch between enabled voices
- **Edit/Delete**: Tap the ⋮ menu on a voice card for edit or delete options
- **Batch Operations**: Supports batch delete and batch export
- **Import/Export**: Export voices as ZIP backup via the ⋮ menu; import with append or overwrite mode

### 5. Advanced: Pronunciation Tuning & Corrections
In the **"Rules"** and **"Settings"** tabs, you can fine-tune the synthesis experience:
- **Custom Pronunciation Rules**: Add text or regex-based substitution rules to fix mispronounced characters.
- **System Dictionary Patching**: Import a community-built `system_dict.json` for bulk polyphone corrections.
- **Sentence Split Regex**: Customize how the engine splits long text into sentences. Reset to defaults if misconfigured.
- **Detailed Logging**: Enable verbose logging in settings to view per-sentence RTF and Encoder/Decoder timing.

### 6. Reading Integration

**Mode 1: System TTS (Widest Compatibility)**
In your phone's Settings, search for **"TTS Settings"** (or Text-to-Speech Output) and set the preferred engine to **CloneTTS**. Then tap "Read Aloud" in any reader app (Legado, Moon+ Reader, etc.) — supports **variable speech rate**.

**Mode 2: HTTP API (For Power Users)**
Go to **"Settings"** tab and enable the **"Local HTTP API Service"**. Enter the displayed API address into your reader app's web-TTS configuration.

Two request methods are supported:
- **GET**: `http://127.0.0.1:8080/api/tts?text={{speakText}}&voice=voiceName`
- **POST**: Supports `application/x-www-form-urlencoded` and `application/json` formats for sending long texts

The `voice` parameter matches by voice name or alias.

### 7. Communication & Feedback
- **Project Homepage**: [https://github.com/sipeter/CloneTTS](https://github.com/sipeter/CloneTTS)
- **QQ Group**: `640190207` (For discussions, feature requests, and issue reports)

### 8. Disclaimer
All speech synthesis and voiceprint extraction in this software operate strictly offline on your local device. No data is transmitted to any server. **Unauthorized extraction or use of another person's voice is strictly prohibited.** The end user bears full responsibility for any disputes arising from misuse.
