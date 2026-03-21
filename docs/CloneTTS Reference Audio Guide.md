# CloneTTS 参考音频制作指南
# CloneTTS Reference Audio Guide

🌐 **Language / 语言**: [English](#english) | [简体中文](#简体中文)

---

<a name="english"></a>

# English

> This guide is for new CloneTTS users. It explains how to select and prepare high-quality reference audio for the best voice cloning results.

---

## Part 1: How Reference Audio Works

CloneTTS voice cloning is based on **Zero-Shot learning**: the model extracts two types of information from a reference audio clip:

1. **Timbre** (vocal characteristics): vocal cord traits, resonance chamber features, tonal texture
2. **Prosody** (speech rhythm): speaking pace, pause habits, intonation patterns, breath feel

The model "imprints" both types of information into the generated speech. Therefore, **the quality of your reference audio directly determines the ceiling of your synthesis quality**.

---

## Part 2: Core Principles for Selecting Reference Audio

### 2.1 Real Human Voice vs. High-Quality Synthetic Voice?

**Key conclusion: A perfect human recording sets the "highest ceiling" for clone quality, while high-quality synthetic speech provides a "reliable floor" for acoustic purity.**

**1. Why should we always pursue real human recordings?**
Human natural speech contains **subtle micro-features** that machines struggle to replicate: the breath in a voice, the micro-vibrations of vocal cords under different emotions, the tension and rhythm of natural phrasing. If you can provide an **extremely clean, echo-free** human voice recording, the cloned voice will be more vivid, rich, and natural than any machine-generated voice. After all, the whole point of voice cloning is to preserve each person's unique vocal character. Using machine-generated dry audio as your reference sample risks losing this liveliness, resulting in a robotic or announcer-like output.

**2. Why does cloning from a high-quality TTS sometimes sound "better"?**
Many users find that cloning from a high-quality commercial TTS (e.g., Microsoft TTS voices) produces better results than recording on their phone. This isn't because machines sound better than humans — it's due to **acoustic environment pollution**:
- **Typical home recordings (high failure rate)**: Most people record in their bedroom or living room without any acoustic treatment. This inevitably captures **ambient noise** and the dreaded **room reflection/reverb**. CloneTTS's zero-shot model is extremely sensitive — it treats these echoes and reflections as part of "your vocal characteristics" and preserves them. The result is a generated voice that sounds like it's speaking in a "hollow bathroom" or "a bad hallway," completely destroying the natural liveliness of the original voice.
- **High-quality synthetic voice (stable baseline)**: Top commercial TTS engines produce audio with virtually **zero** room reverb or background noise. This allows the model to cleanly extract the frequency profile of the voice. Users feel it "sounds better" simply because it has a studio-level cleanliness by default — it never triggers the acoustic pollution failure mode.

**Ultimate strategy:**
- **Pursue maximum realism (preferred)**: Do whatever it takes to obtain high-quality **dry human voice audio** — cleanly extracted from professional audiobooks or podcasts without background music; or recorded in a walk-in closet (clothes absorb room reflections well) or a quiet car with windows closed, with your phone close to your mouth.
- **Prioritize convenience and acoustic purity (valid alternative)**: If your physical environment makes it impossible to avoid reverb, or if you just want a clean stable voice for listening to ebooks, using a top AI TTS to generate a 3–5 second dry audio clip as a "clean base sample" is a completely safe and practical lower-barrier option.

### 2.2 Keep Emotions Consistent

**A single reference audio clip should contain only one emotional state.**

When extracting prosodic patterns, the model "averages" the entire audio clip. If the clip contains mixed emotions:
- Calm first half + excited second half → model learns a strange "neither calm nor excited" average prosody
- Wildly varying intonation → model becomes confused, output becomes unstable

**Best practice**: Prepare separate reference audio clips for different emotions, and switch between voice profiles to achieve style variation:

| Voice Name | Emotion | Use Case |
|-----------|---------|----------|
| Xiaoyi-Calm | Peaceful, natural | News, expository text |
| Xiaoyi-Happy | Light, cheerful | Dialogue, casual content |
| Xiaoyi-Serious | Deep, composed | Formal settings, solemn content |

### 2.3 Stable Intonation, Avoid Large Fluctuations

The ideal reference audio should be in a **calm, steady** reading state:
- ✅ Even speaking pace, no sudden speed-up/slow-down
- ✅ Consistent volume, no sudden loudness changes
- ✅ Natural intonation, not exaggerated or theatrical
- ❌ Avoid: loud laughter, crying, sighing, exclamations, strong emotional outbursts
- ❌ Avoid: mid-sentence corrections, hesitations, stumbles

### 2.4 Quiet Environment, No Background Noise

Background noise (air conditioner, fan, ambient sounds) will be learned by the model as part of the "speaker's characteristics," causing the generated voice to carry similar background noise.

- ✅ Record indoors in a quiet environment, turn off AC/fans
- ✅ Use a near-field microphone (phone 20–30cm from mouth)
- ✅ Extract from professional audiobook or podcast recordings
- ❌ Avoid: outdoor recordings, noisy environments
- ❌ Avoid: audio with background music

> Although CloneTTS includes RNNoise neural network denoising, denoising is a "subtractive process" — it inevitably sacrifices some voice detail while removing noise. **Always prioritize clean original recordings rather than relying on post-processing denoising.**

---

## Part 3: Duration

### 3.1 Longer Is Not Always Better

This is the most common misconception. More reference audio **does not mean better results**.

The model uses **cross-attention** to process the reference audio globally. This means:
- **3 seconds**: Already sufficient to extract clear timbre and basic prosody
- **5–8 seconds**: More complete prosodic patterns — the quality/performance sweet spot
- **10+ seconds**: Prosodic information becomes redundant, but inference time increases significantly
- **20–30 seconds**: Marginal additional information, but substantially slower speed

### 3.2 Recommended Duration

| Use Case | Recommended Duration | Notes |
|---------|---------------------|-------|
| Quick mobile experience | **3–5 seconds** | Fastest speed, already excellent quality |
| Pursuing higher quality | **5–8 seconds** | More natural prosody, the golden range |
| Maximum | 30 seconds | App hard limit — clips longer than this are rejected |

### 3.3 Duration Affects Inference Speed

Reference audio length directly affects generation latency — the model must encode and run attention over the reference clip. On mobile (e.g., Snapdragon 865), doubling the reference length increases inference time by approximately 15–25%. Choosing 3–5 seconds delivers both good quality and the fastest response time.

---

## Part 4: Audio Format

### 4.1 High Sample Rates Are Unnecessary

When processing reference audio, CloneTTS normalizes all input to:

| Parameter | Target Value |
|-----------|-------------|
| Sample Rate | **16,000 Hz** |
| Bit Depth | **16-bit** |
| Channels | **Mono** |
| Loudness | **RMS 0.05** (approx. -26 dBFS) |

This means:
- A 48kHz / 24-bit high-spec recording → will be downsampled to 16kHz / 16-bit
- Frequency information above 8kHz (the Nyquist limit of 16kHz) is not used by the model
- **Any sample rate above 16kHz provides no additional quality benefit**

That said, this does not mean you can use low-quality recordings — audio below 8kHz (e.g., telephone quality) will lose significant frequency information and produce noticeably worse results.

### 4.2 Supported Input Formats

The app supports common audio formats: WAV, MP3, M4A, FLAC, OGG. Any format works — the app will automatically decode and convert.

---

## Part 5: Handling Silence

### 5.1 Leading/Trailing Silence: Remove It

Silent gaps before and after speech carry no information — they only waste processing time and slow inference. The app's built-in VAD (Voice Activity Detection) automatically detects and removes leading/trailing silence.

### 5.2 Internal Pauses: Do NOT Remove Them

Natural pauses during speech are an **essential part of prosodic information**:

- Pauses between sentences = breathing rhythm, speaking style
- Brief pauses at commas/periods = phrasing habits
- Micro-pauses before emphasis = expressive intent

If you manually cut out all the internal pauses, the model learns a "rushed, no-pause" speaking style, and the generated voice will sound like a machine gun — words firing in rapid succession with no natural rhythm.

**Rule of thumb**: To shorten reference audio, remove leading/trailing silence rather than cutting natural internal pauses.

---

## Part 6: In-App Audio Processing

After importing a reference audio as a voice, the app automatically applies the following processing (configurable on the import screen):

### 6.1 Processing Pipeline

```
Raw audio (any format)
  ↓ ① Format normalization: resample to 16kHz / mono / RMS normalization
  ↓ ② [Optional] Neural network denoising (RNNoise)
  ↓ ③ [Optional] Silence trimming (remove leading/trailing silence)
  → Save as final reference audio
```

### 6.2 Denoising Level Selection

| Level | Intensity | Recommended For |
|-------|-----------|----------------|
| **Off** | No processing | Studio-quality clean recordings |
| **Light** | Single pass | Slight noise (quiet indoor recording) |
| **Standard** | Double pass | Noticeable noise (AC / fan sounds) |
| **Heavy** | Triple pass | Heavy noise (outdoor / noisy environments) |

> Higher denoising intensity removes more noise but also loses more voice detail. Prioritize clean source recordings; use denoising only as a remediation tool.

### 6.3 Silence Trimming

When enabled, the app automatically detects and removes silent segments at the beginning and end of the audio, preserving only the core speech content. Natural internal pauses are not trimmed.

After processing, the app displays a summary:
```
5.20s → 3.85s (26% silence removed), denoised (standard)
```

If you're not satisfied with the result, tap "Restore Original Audio" at any time to revert to the state at import.

---

## Part 7: Quick Start Checklist

1. ✅ Find a **3–8 second** extremely clean human voice clip (prefer extracting from professional audiobooks/podcasts, or record in a walk-in closet or quiet car with closed windows)
2. ✅ If your environment unavoidably has reverb, use a **high-quality synthetic voice** (e.g., Microsoft TTS) as a clean fallback baseline
3. ✅ Ensure **consistent emotion** — calm and natural delivery
4. ✅ Ensure the **sentence is complete** — don't cut off mid-utterance
5. ✅ Enable **noise reduction** (light or standard) and **silence trimming** when importing
6. ✅ Preview the result — if unsatisfied, try a different reference audio clip
7. ❌ Never use **poor-quality, obviously mechanical/electronic** synthetic audio from outdated models as reference
8. ❌ Don't chase excessively long reference audio (30 seconds is the limit, but 3–5 seconds is sufficient)
9. ❌ Don't manually cut out natural internal pauses from the speech

---

<a name="简体中文"></a>

# 简体中文

> 本文档面向 CloneTTS 新用户，帮助你理解如何挑选和制作高质量的参考音频，以获得最佳的语音克隆效果。

---

## 一、参考音频的作用原理

CloneTTS 的语音克隆基于 **零样本学习**（Zero-Shot）：模型通过一段参考音频，同时提取两类信息：

1. **音色特征**（Timbre）：声带特性、共鸣腔体特征、音质纹理
2. **韵律模式**（Prosody）：语速节奏、停顿习惯、语调起伏、气息感

模型会把这两类信息"印刻"到生成的语音中。因此，**参考音频的质量直接决定合成效果的上限**。

---

## 二、挑选参考音频的核心原则

### 2.1 到底该用真人录音还是高质量合成语音？

**核心结论：完美的真人录音决定了克隆效果的"最高上限"，而高质量合成语音则是保证声音纯净度的"安全底线"。**

**1. 为什么我们始终追求"真人录音"？**
人类自然语言中包含着机器极难模拟的**微特征**（如呼吸的气息感、声带在不同情绪下的共振缝隙、抑扬顿挫的话语张力）。如果您能提供一段**极致干净、毫无环境回音**的真人原声，它克隆出的专属音色绝对会比任何机器发声都要生动、饱满、有灵魂。毕竟，语音克隆的本意就是保留每个人真实的特色。如果长期用机器干音来做样本，便容易丢失这份灵动感，不可避免地带来机械或播音腔。

**2. 解答困惑：为什么有时觉得用高质量 TTS 克隆的效果"好像更好听"？**
在不少用户的实际体验中，反而觉得用高质量商业 TTS（如微软发音人）克隆出的效果好于直接用手机录制。这并非因为机器本身比人强，而是由**"环境声学污染"**造成的客观现象：
- **普通真人录音（极易翻车）**：大多数人在卧室或客厅随手拿起手机录音，由于普通房间没有做过专业吸音，不可避免地会录下**环境底噪**和致命的**空间反射回音（空间混响）**。CloneTTS 的零样本模型极其敏感，它会把这些回声和反射杂音当作"您的发音特点"一并保留。最终导致生成的人声像是在"空旷浴室"或"劣质通道"里说话，这种糟糕浑浊的听感直接摧毁了真人原本的灵动。
- **高质量合成语音（稳定保底）**：顶尖商业 TTS 生成的干声，其物理混响和底噪几乎是**绝对的零**。这让模型能轻松无误地提取出干净的频率声线。大家觉得它"更好听"，其实是因为它天然具备演播室级的纯净基础，绝不会触发环境杂音的翻车。

**【终极指引策略】：**
- **追求极致生动（首选方案）**：想尽一切办法获取高质量的**真人干声**！比如从没有配乐的专业有声书、电台播客中纯净截取；在您亲自为亲友采音时，请务必选择在衣帽间（挂满衣物能极大吸走室内回音）、或紧闭车窗的安静汽车内部等极低混响的环境里，用手机靠近嘴边（防喷麦）进行录制。
- **追求省事与纯净音质（有效备选）**：如果您身处的物理环境确实无法避免回音的影响，或者您只想快速获得一个绝对清晰稳定的音色来听电子书。那么退而求其次，预先利用顶级 AI 生成一段 3~5 秒极其明彻的干音作为"纯净底模"，是非常"安全且稳妥"的低门槛替代方案。

### 2.2 情绪保持一致

**一段参考音频只应包含一种情绪状态。**

模型在提取韵律模式时，会对整段音频做"平均化"处理。如果同一段音频中混合了多种情绪：
- 前半段平静 + 后半段激动 → 模型学到一个"不平不激"的奇怪折中韵律
- 忽高忽低的语调 → 模型困惑，输出不稳定

**正确做法**：为不同情绪准备不同的参考音频，通过切换音色来实现风格变化：

| 音色名称 | 参考音频情绪 | 适用场景 |
|---------|------------|---------|
| 晓伊-平和 | 平静、自然 | 新闻、说明文 |
| 晓伊-开心 | 轻快、愉悦 | 对话、轻松内容 |
| 晓伊-严肃 | 低沉、稳重 | 正式场合、庄严内容 |

### 2.3 语调平稳，避免大幅波动

理想的参考音频应该是 **平和、稳定** 的朗读状态：
- ✅ 语速匀称，没有突然加速/减速
- ✅ 音量均匀，没有突然变大/变小
- ✅ 语调自然，不夸张不做作
- ❌ 避免：大笑、哭泣、叹气、惊呼等强烈情绪表达
- ❌ 避免：念到一半改口、卡顿、口误

### 2.4 环境安静，无底噪

底噪（空调声、风扇声、环境嘈杂音）会被模型当作"说话人特征"的一部分学习进去，导致生成的语音也带有类似的背景噪声。

- ✅ 在安静的室内录制，关闭空调/风扇
- ✅ 使用近场麦克风（手机贴近嘴巴 20~30cm）
- ✅ 从有声书、播客等专业录音中截取
- ❌ 避免：户外录音、嘈杂环境
- ❌ 避免：带有背景音乐的音频

> 虽然 CloneTTS 内置了 RNNoise 神经网络降噪，但降噪是"减法处理"——它在去除噪声的同时不可避免地会损失一部分语音细节。**始终优先选择干净的原始录音，而不是依赖后期降噪。**

---

## 三、时长选择

### 3.1 并非越长越好

这是最常见的误区。参考音频 **不是越长效果越好**。

模型对参考音频的利用方式是通过 **cross-attention**（交叉注意力）机制进行全局关注。这意味着：
- **3 秒**：已经足够提取清晰的音色和基本韵律
- **5~8 秒**：韵律模式更完整，是质量/性能的最佳区间
- **10 秒以上**：韵律信息开始冗余，但推理时间显著增加
- **20~30 秒**：增加的信息量微乎其微，却大幅拖慢速度

### 3.2 推荐时长

| 场景 | 推荐时长 | 说明 |
|------|---------|------|
| 手机端快速体验 | **3~5 秒** | 速度最快，效果已经很好 |
| 追求高质量 | **5~8 秒** | 韵律更自然，推荐的黄金区间 |
| 上限 | 30 秒 | App 硬限制，超过此时长会被拒绝 |

### 3.3 时长影响推理速度

参考音频的时长直接影响每次生成的延迟——模型需要对参考音频做编码和注意力计算。在手机端（如 Snapdragon 865），时长翻倍大约会增加 15~25% 的推理时间。选择 3~5 秒既能保证效果，又能获得最快的响应速度。

---

## 四、音频格式

### 4.1 不需要追求高采样率

CloneTTS 在处理参考音频时，会将所有输入统一转换为：

| 参数 | 目标值 |
|------|-------|
| 采样率 | **16,000 Hz** |
| 位深 | **16-bit** |
| 声道 | **单声道** |
| 响度 | **RMS 0.05**（约 -26 dBFS） |

这意味着：
- 48kHz / 24-bit 的高规格录音 → 会被降采样到 16kHz/16-bit
- 高于 8kHz 的频率信息（16kHz 的奈奎斯特极限）不会被模型使用
- **任何超过 16kHz 的采样率都不会带来额外的音质提升**

当然，这不意味着可以使用低质量录音——8kHz 以下（如电话音质）的音频会丢失大量关键频率信息，效果会明显变差。

### 4.2 支持的输入格式

App 支持常见音频格式：WAV、MP3、M4A、FLAC、OGG。选择任意格式即可，App 会自动解码和转换。

---

## 五、关于静音的处理

### 5.1 首尾静音：应该去除

录音前后的空白静音不携带任何信息，只会白白增加音频时长、拖慢推理速度。App 内置的静音裁剪功能（VAD）会自动检测并去除首尾静音。

### 5.2 中间停顿：不要去除

说话过程中的自然停顿是 **韵律信息的重要组成部分**：

- 语句间的停顿 = 呼吸节奏、说话风格
- 逗号/句号处的短暂停顿 = 断句习惯
- 强调前的微小停顿 = 语气表达

如果你手动把中间的停顿全部剪掉，模型学到的韵律就变成了"急促无停顿"的风格，生成出来的语音会像机关枪一样连珠炮，缺乏自然的节奏感。

**原则**：为了缩短参考音频，应该优先去除首尾静音，而不是剪掉中间的自然停顿。

---

## 六、导入后的加工处理

将参考音频添加为音色后，App 会自动执行以下处理（可在导入界面配置）：

### 6.1 处理流程

```
原始音频（任意格式）
  ↓ ① 格式归一化：重采样至 16kHz / 单声道 / RMS 标准化
  ↓ ② [可选] 神经网络降噪（RNNoise）
  ↓ ③ [可选] 静音裁剪（去除首尾静音）
  → 保存为最终参考音频
```

### 6.2 降噪等级选择

| 等级 | 处理强度 | 适用场景 |
|------|---------|---------|
| **关闭** | 不处理 | 录音室级别的干净录音 |
| **轻度** | 单遍降噪 | 轻微底噪（安静房间录制） |
| **标准** | 双遍降噪 | 明显底噪（空调/风扇声） |
| **强力** | 三遍降噪 | 较大底噪（户外/嘈杂环境） |

> 降噪强度越高，去除底噪越彻底，但也会损失越多的语音细节。建议优先选择干净的录音源，降噪只作为补救手段。

### 6.3 静音裁剪

开启后，App 会自动检测并去除音频首尾的静音段，保留核心语音内容。中间的自然停顿不会被裁剪。

处理完成后，App 会显示处理统计：
```
5.20s → 3.85s (去静音26%), 已降噪(标准)
```

如果对处理结果不满意，可以随时点击"恢复原始音频"回退到导入时的状态。

---

## 七、快速上手清单

1. ✅ 找一段 **3~8 秒** 的极度干净的真人语音（优先从专业有声书、播客截取，或在衣帽间等防回音极好的环境自己录音）
2. ✅ 若录音环境回音严重根本无法达标，妥协使用 **高质量合成语音**（如微软 TTS）生成的干音作为保底替代
3. ✅ 确保 **情绪一致**，平和自然地朗读
4. ✅ 确保 **语句完整**，不要截取到一半
5. ✅ 导入时开启 **降噪**（轻度或标准）和 **静音裁剪**
6. ✅ 试听效果，不满意就换一段参考音频再试
7. ❌ 绝对不要使用**低劣、带有明显机械电子杂音**的老旧模型合成音作为参考
8. ❌ 不要追求超长参考音频（30 秒是上限，但 3~5 秒就够了）
9. ❌ 不要手动剪掉说话中间的自然停顿
