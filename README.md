# CloneTTS (Voice Cloning Engine)

🌐 **[English](#english-version)** | **[简体中文](#简体中文版)**

---

## 简体中文版

CloneTTS 是一款运行在安卓系统本地的文字转语音 (TTS) 原生引擎。它可以让您在手机上离线克隆所需的声音，并直接使用这个声音来朗读书籍或长文本。无需联网，所有推理计算均在本地完成。

### 功能特色

- **离线音色克隆**：录制 1~3 秒人声即可生成专属音色
- **系统级 TTS**：注册为安卓系统 TTS 引擎，兼容开源阅读、Moon+ 等阅读器
- **HTTP API 服务**：内置本地 HTTP 服务器，支持 GET 和 POST 请求
- **语速与音量调节**：0.5x ~ 2.0x 语速，0% ~ 200% 音量
- **发音纠错**：支持添加自定义的纯文本或强大的正则表达式替换规则
- **音色备份**：ZIP 格式导入/导出，支持追加或覆盖导入

### 📢 参与 Google Play 封闭内测 (Beta Testing)
如果您希望提前体验具备神经网络降噪与设备性能基准测试的测试版本，欢迎加入官方内测通道（设备须支持 Google Play 服务）：
1. **加入测试组**（必需，用于获取测试权限）：[Google Groups 链接](https://groups.google.com/g/clonetts)
2. **接受内测邀请**：[同意成为测试员](https://play.google.com/apps/testing/com.sipeter.clonetts)
3. **下载或更新应用**：[前往 Google Play 商店](https://play.google.com/store/apps/details?id=com.sipeter.clonetts)

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
- **选择与激活音色**：点击音色卡片即可选中为当前默认音色。
- **自定义音色代号 (Alias)**：支持为音色设定专属的名称与唯一代号 (Alias)。该代号是开源阅读软件（如 Legado）通过 HTTP API 进行**“分角色听书”**精准调用的核心键值。
- **编辑属性**：点击卡片右侧的 `⋮` 菜单可随时修改名字、代号及发音参考文本。系统会自动执行代号重名校验防护。
- **备份与迁移**：支持通过 `⋮` 菜单进行音色的批量删除与 `.zip` 格式打包导出。导入时可智能选择“追加”或“覆盖”模式。

### 五、高级进阶：发音纠错与性能监控
在底部 **“发音规则”** 与 **“高级设置”** 栏中，您可以对合成体验进行底层的专业级干预：
- **添加发音正则替换**：在 "发音规则" 界面，您可以针对特定的错误多音字添加文本或正则规则替换（例如遇到特定人名强制拼音纠错）。
- **高阶断句正则配置**：这个选项决定了引擎切分小说长句时的停顿界限。配置紊乱时可点击“恢复系统默认”一键重置。
- **实时 RTF 监控瀑布台**：在高级设置开启详细日志后，前端界面直通底层协程，您可直观捕捉并在页面底端查看每一句生成的 **RTF（实时推理速率）**和算力耗时，彻底终结盲盒合成！

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
- **官方 QQ 交流群**：`640190207`
- **Telegram 频道**：[CloneTTS_channel](https://t.me/CloneTTS_channel)
- **Telegram 交流群**：[CloneTTS](https://t.me/CloneTTS)

### 八、免责声明
本软件所有语音合成渲染及生物特征提取均在本地脱机完成，无法触网且不留存您的私密发音特征。**未经版权所有人明确授权，严禁私自提取或滥用他人的声音进行克隆**。对于各种侵权欺诈或非法使用产生的任何纠纷，概由用户个人自行承担，本软件概不负责。

### 九、核心技术与第三方组件致谢 (Acknowledgements)
本项目的实现离不开以下优秀的开源项目，在此致以诚挚的感谢：
- **[sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx)**: 提供了高效的端侧 C++ 推理流水线架构。
- **[ZipVoice](https://github.com/AnyVoiceAI/ZipVoice)**: 提供了零样本音色克隆的声学模型。
- **[ONNX Runtime](https://github.com/microsoft/onnxruntime)**: 提供了跨平台的高性能推理引擎。
- **[NanoHTTPD](https://github.com/NanoHttpd/nanohttpd)**: 提供了轻量级的嵌入式 HTTP 服务器。
- *(完整的第三方开源许可协议与组件清单，请详查 App 内的致谢页面)*
---

## English Version

CloneTTS is an offline, native Text-to-Speech (TTS) engine for Android devices. It allows you to clone desired voices and use them directly to read books or long texts on your phone — no internet connection required. All inference runs locally on-device.

### Key Features

- **Offline Voice Cloning**: Record 1~3 seconds of speech to create a custom voice
- **System TTS**: Registers as an Android system TTS engine, compatible with Legado, Moon+ Reader, etc.
- **HTTP API Server**: Built-in local HTTP server supporting GET and POST requests
- **Speed & Volume Control**: 0.5x–2.0x speech rate, 0%–200% volume
- **Pronunciation Correction**: Add powerful custom plain text or regex-based substitution rules
- **Voice Backup**：ZIP-based export/import with append or overwrite modes

### 📢 Join the Google Play Closed Beta
To preview our upcoming release featuring neural noise reduction and built-in device benchmarking, please enroll in our official beta testing track:
1. **Join the testing group** (Required to obtain beta access): [Google Groups Link](https://groups.google.com/g/clonetts)
2. **Accept testing invitation**: [Testing Opt-in](https://play.google.com/apps/testing/com.sipeter.clonetts)
3. **Download or update**: [View on Google Play](https://play.google.com/store/apps/details?id=com.sipeter.clonetts)

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
- **Select & Activate Voice**: Tap a voice card to set it as the active default voice.
- **Custom Voice Alias**: Assign a unique name and Alias to your voices. The Alias serves as the critical binding key for external readers (like Legado) when performing **Role-Based TTS** via the HTTP API.
- **Edit Properties**: Tap the `⋮` menu on a card to modify the name, alias, and reference text. The system features real-time alias duplication checking.
- **Backup & Migration**: Supports bulk deletion and exporting/importing voices as `.zip` archive backups (with append/overwrite modes).

### 5. Advanced: Pronunciation Tuning & Performance Monitoring
In the **"Rules"** and **"Settings"** tabs, you can heavily customize your synthesis environment:
- **Custom Pronunciation Rules**: Add plain text or Regex-based substitution rules to fix mispronounced characters or specific names.
- **Sentence Split Regex**: Customize how the engine parses and slices long paragraphs into speakable sentences. Reset to defaults if misconfigured.
- **Real-Time RTF Monitoring Console**: By enabling detailed logging in settings, you can monitor the precise **RTF (Real-Time Factor)** and inference latency for every single audio chunk directly within the app's waterfall log panel, eliminating any guesswork regarding your device's capabilities.

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
- **QQ Group 1**: `640190207`
- **QQ Group 2**: `1083816618`
- **Telegram Channel**: [CloneTTS_channel](https://t.me/CloneTTS_channel)
- **Telegram Group**: [CloneTTS](https://t.me/CloneTTS)

### 8. Disclaimer
All speech synthesis and voiceprint extraction in this software operate strictly offline on your local device. No data is transmitted to any server. **Unauthorized extraction or use of another person's voice is strictly prohibited.** The end user bears full responsibility for any disputes arising from misuse.

### 9. Core Technology & Third-Party Acknowledgements
This project is made possible by the following open-source projects. We sincerely thank their authors and communities:
- **[sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx)**: Efficient on-device C++ inference pipeline architecture.
- **[ZipVoice](https://github.com/AnyVoiceAI/ZipVoice)**: Zero-shot voice cloning acoustic models.
- **[ONNX Runtime](https://github.com/microsoft/onnxruntime)**: High-performance cross-platform ML inference backend.
- **[NanoHTTPD](https://github.com/NanoHttpd/nanohttpd)**: Lightweight embedded HTTP server.
- *(For the complete list of third-party libraries and licenses, please refer to the Acknowledgements page within the App.)*
