# 02 · OneBook-智能伴读（`com.linyi.OneBook` v3.7.24）

原生 Swift 语言学习阅读器：EPUB/PDF/MOBI/漫画 + 音频 + 逐词对齐 + 端上 ASR/TTS + AI 解析。

| 项 | 值 |
| :--- | :--- |
| 版本 | 3.7.24（2026-10-30 上架） |
| 最低系统 | iOS 15.0 |
| 包体 | 商店 234.8 MB / 解密后 `.app` 214.7 MB |
| 二进制 | 13 个 Mach-O，全部 `cryptid=0`，23,184 页 |
| 技术栈 | SwiftUI / GRDB+FTS5 / Foliate(JS) / FFmpeg / whisper.cpp+CoreML+ONNX / sherpa-onnx / LiteRT+Gemma / FSRS / Swifter(WebDAV) / Python 3.10 |

## 先看这三份

1. **`REPORT.md`** —— 结论、七层架构、五个真难点、复刻优先级，以及**对旧报告的三处纠正**（旧版"957 个类""有 Anki 导出"均不成立）。
2. **`evidence/04-capabilities.md`** —— 8 大类能力逐条带证据，每条都能指到类名/二进制/资源文件。
3. **`SAMPLE.md`** —— 双哈希与复现命令，任何数字都可复算。

## 本轮新增的证据

| 文件 | 内容 |
| :--- | :--- |
| `evidence/01-inventory.md` | 13 个二进制清单 + 包体构成（38 MB 词典、16 MB Python、45 语言包） |
| `evidence/02-symbols.md` | 模块归属（OneBook 257 类 / SwiftSoup 55 / GRDB 38 / FSRS 9 / Swifter 9 / StoryAlignCore 8 …） |
| `evidence/03-pages.md` | 290 个页面类，16 组 |
| `evidence/04-capabilities.md` | 阅读/音频/语音/词典/AI/内容源/同步/商业化 |
| `evidence/05-endpoints.md` | 3 个自有域名、14 条接口、20 家 TTS/LLM 供应商、OPDS/RSS 内容源 |
| `evidence/06-data.md` | SQLite 表结构、FTS5、CEFR 难度画像、四条同步通道 |
| `evidence/*.json` | 机器可读的类表、模块表、能力探针、二进制清单 |

## 四条最有价值的发现

1. **阅读器是 Foliate（JS）**，跑在 WKWebView 里 —— 不自己写排版，直接复用开源引擎。
2. **整个 yt-dlp 被塞进 App**（1,268 个 `.py`，16 MB），内容抓取放在端上而不是服务器。
3. **内嵌 WebDAV 服务器**（Swifter），本地起服务供「文件」App 读写书库。
4. **TTS 是 20 家供应商的可插拔枚举**，端上另有 sherpa-onnx 的 39 个音色（含德语 `vits-mms-deu`、泰语 `vits-mms-tha`）。

## 目录

```
REPORT.md              主报告
SAMPLE.md              取证基座（哈希/复现）
evidence/              01–06 证据文档 + JSON
深度架构分析报告.md      旧版（保留对照）
全功能零遗漏规格说明书.md 旧版（口径已被 evidence/02 取代）
recon/                 旧轮字符串抓取结果
sample/ baseline/      原始包与解包树（不入库）
```

## 方法论

解密 → 读编译器元数据（`__objc_classname` / `__swift5_types`）而不是 grep 字符串 → 交叉验证（类型表 ∩ 字符串正则）→ 只把能指到具体类/文件/资源的写进结论。

> 分析方式：静态。未运行 App、未抓包。凡涉及运行时行为的推断在 `REPORT.md` 末尾"未验证项"中单列。
