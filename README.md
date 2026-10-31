# 全球语言学习与精听赛道 · 16 大主流 App 全量逆向与自研架构母库 (Language Learning Apps Hub)

> 本仓库汇编了全网 16 款主流视频精听、有声书阅读、影子跟读与双语字幕商业 App 的**100% 真实离线脱壳与 REA (Reverse Engineer Anything) v6.8.0 架构图谱**。
> 统一为自研沉浸式多语种学习软件提供最顶级的工业级架构蓝图。

---

## 一、 16 大商业 App 全景目录规范

```text
apps/
├── 01-Aisten-播客学语言          (Swift / CoreData+GRDB / whisper.cpp 逐句手势)
├── 02-OneBook-智能伴读           (SwiftUI / GRDB / FFmpegKit / WhisperKit / Gemma 端侧模型)
├── 03-DejaVocab-得记语料库       (Readium 引擎 / 语料上下文提取器 / 3秒原声切片)
├── 04-Miraa-AI音视频跟读         (Flutter / Mpv C++ 原生播放内核 / 语法块切分)
├── 05-Linguagua-呱呱跟读         (Flutter / Mpv / whisper_ggml_plus 端侧转录)
├── 06-Trancy-沉浸式字幕          (SwiftUI / 网页与App双端双语字幕排版)
├── 07-ListenLeap-精听跃迁        (Flutter / 79 个跨平台插件全能播放管线)
├── 08-TingYue-听阅全能阅读       (SwiftUI / 71 个独立模块 / 权威分级排版)
├── 09-EchoSpeak-影子跟读         (SwiftUI / libavcodec / 毫秒级跟读状态机)
├── 10-Tiklingo-短视频精听        (React Native / 短视频滑动跟读流派)
├── 11-Lispod-英语分级播客        (SwiftUI / 轻量级播客流式播放)
├── 12-EchoTalk-双语跟读转录      (SwiftUI / 双语字幕与波形同步)
├── 13-LingoReactor-播客双语字幕  (React Native / 沉浸式字幕播放)
├── 14-Echotter-小獭播客          (SwiftUI 极简播客)
├── 15-ListenGenius-播客学英语    (轻量级播客精听)
└── 16-PearDog-啤酒狗            (单音频播放)
```

---

## 二、 自研播放器（Lumina Player）终极选型决策

通过对上述 16 个商业 App 的全量横向代码审计，自研播放器必须坚持以下四条铁律：

1. **播放内核（坚决摒弃原生 AVPlayer）**：
   - 采用 **`libmpv` (C/C++)** 或 **`FFmpegKit`**（与 OneBook、Miraa、Linguagua、EchoSpeak 保持一致）；
   - **收益**：通吃 MKV/MP4，真正实现**多音轨（德/英/日）无缝热切换**、软字幕毫秒级对齐与精准拖动！
2. **界面视觉与波形设计**：
   - 吸收 **Echo-Loop** 的声学波形视图算法与拖动交互；
   - 吸收 **OneBook** 的横屏深空灰剧场台词流（上 65% 视频，下 35% 自动滚屏字幕）。
3. **分词与多语言通用引擎**：
   - 调用标准 **`Intl.Segmenter`**，通吃德语超长复合词、泰语无空格切分、日英汉分词。
4. **背词与 Anki 导出**：
   - 吸收 **DejaVocab** 的 `CorpusContextExtractor`，点一颗星自动截取**原声 3 秒音频 + 视频截图 + 上下文句式**无感导出 Anki！
