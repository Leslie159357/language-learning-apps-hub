# 05 · 网络与接口契约

数据来源：主二进制字符串中提取的 220 个 URL（去重后按主机归组），以及 14 条 `/api/...` 路径字面量。所有条目都是包内原样字符串。

## 1. 自有后端（三个域名分工明确）

| 域名 | 职责 | 已见路径 |
| :--- | :--- | :--- |
| `base-ser.onebook2025.top` | 账号、额度、订阅、模型代理、同步 | `/api/auth/account`、`/api/auth/logout`、`/api/auth/refresh`、`/api/llm/v1/chat/completions`、`/api/llm/v1/invoke`、`/api/paas/v4/audio/speech`、`/api/quota/consume`、`/api/subscription/ios/verify`、`/api/subscription/invite/redeem`、`/api/sync/v1/push`、`/api/v1/tts`、`/api/v1/voices` |
| `podcast.onebook2025.top` | 内容代理：音频、字幕、翻译、书库 | `/get_audio`、`/proxy/stream`、`/proxy/transcript`、`/v1/news`、`/v1/translate`、`/v1/youtube`、`/v1/baidu-shelf`、`/v1/baidu-video-shelf` |
| `assets.onebook2025.top` | 静态资源与 TTS 素材 | `/v1/`、`/v1/tts` |
| `www.onebook2025.top` | 官网与隐私政策 | `/privacy/` |

本地保存的会话键（UserDefaults / Keychain 键名原样）：

```
onebook.baseser.session_id
onebook.baseser.username
refresh_token
```

这三条说明鉴权是**自建账号体系**（邮箱/密码或验证码换 session），不是纯 Apple 登录，也不是 RevenueCat。

## 2. 第三方 AI / TTS 供应商（用户可插拔）

这是它设计上最值得抄的一点：**TTS 供应商是一个可配置的枚举，不是写死一家**。包内同时存在下列端点的协议实现：

| 供应商 | 端点 |
| :--- | :--- |
| OpenAI | `https://api.openai.com/v1/audio/speech` |
| ElevenLabs | `https://api.elevenlabs.io/v1/text-to-speech` |
| Azure Speech | `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1` |
| Google Cloud TTS | `https://texttospeech.googleapis.com/v1/text:synthesize`、`/v1/voices` |
| 火山引擎 | `https://openspeech.bytedance.com/api/v1/tts`、`/api/v1/voices` |
| 阿里云百炼 CosyVoice | `https://dashscope.aliyuncs.com/api/v1/services/audio/tts/SpeechSynthesizer` |
| 百度语音 | `https://tsn.baidu.com/text2audio`、`https://aip.baidubce.com/oauth/2.0/token` |
| 讯飞 | 官方文档地址 + 在线 TTS 协议 |
| MiniMax | `https://api.minimaxi.com/v1/t2a_v2` |
| 智谱 GLM-TTS | `https://open.bigmodel.cn/api/paas/v4/audio/speech` |
| 小米 MiMo | `https://api.xiaomimimo.com/v1/chat/completions` |
| 华为云 | `https://sis.cn-north-4.myhuaweicloud.com/v1/tts` |

LLM 侧：`api.openai.com`、`api.anthropic.com/v1/messages`、`generativelanguage.googleapis.com`（Gemini）、`api.siliconflow.cn/v1/{chat/completions,models}`。

设计含义：**它自己不承担推理成本，把 key 交给用户**，同时用自己的 `base-ser` 做一层代理（`/api/llm/v1/invoke`、`/api/paas/v4/audio/speech`）方便不想填 key 的人走额度制。这既省成本又留住付费用户。

## 3. 内容源（含"为什么它要内置 Python"）

| 来源 | 端点 | 备注 |
| :--- | :--- | :--- |
| YouTube | `podcast.onebook2025.top/v1/youtube` + 端上 `yt_dlp` | 端上直接调用 yt-dlp |
| B 站 | `api.bilibili.com/x/player/playurl?bvid=`、`/x/web-interface/view?bvid=` | 直连官方接口 |
| 抖音 | `aweme.snssdk.com/aweme/v1/play/`、`www.iesdouyin.com/share/video/` | 分享链解析 |
| 小红书 | `www.xiaohongshu.com/discovery/item/`、`sns-video-bd.xhscdn.com` | — |
| Reddit | `api.pullpush.io/reddit/search/submission/?ids=` | 第三方镜像 |
| OPDS 书库 | `www.gutenberg.org/ebooks/search.opds/`、`archive.elephanteditions.net/opds`、`theanarchistlibrary.org/opds` | 公版书/独立出版 |
| RSS 播客/杂志 | `quantamagazine.org/feed/`、`aeon.co/feed.rss`、`nautil.us/feed/`、`scientificamerican.com/.../rss/`、`feeds.bbci.co.uk`、`www.theguardian.com` 等 14 个源 | 内置推荐源 |
| 词典 | `dict.youdao.com/jsonapi?q=`、`api.dictionaryapi.dev/api/v2/entries/en/`、`mdx.mdict.org/Recommend/`（含 **Duden 10 Volume** 德语词典包） | 在线查词 + MDX 下载 |
| arXiv | `arxiv.maplepop.com/catalog/` | 第三方目录站 |
| 百度网盘 | `pan.baidu.com/rest/2.0/xpan/{file,multimedia}`、`c.pcs.baidu.com/rest/2.0/pcs/superfile2`、OAuth `openapi.baidu.com/oauth/2.0/authorize` | 自有书库 + 用户网盘双通道 |
| 模型下载 | `huggingface.co`（50 次）、`hf-mirror.com`（4 次）、`gh-proxy.com`/`ghfast.top`/`ghproxy.com`/`mirror.ghproxy.com` | 国内镜像冗余做得很足 |

## 4. 鉴权与订阅链（复刻时的关键约束）

```
登录   POST /api/auth/account      → session_id + refresh_token
续期   POST /api/auth/refresh
额度   POST /api/quota/consume     → 服务端扣减
订阅   POST /api/subscription/ios/verify   → 服务端校验 StoreKit 收据
邀请   POST /api/subscription/invite/redeem
同步   POST /api/sync/v1/push
```

结论写死在这里：**会员/额度状态由服务端持有，端上只读**。任何"本地改布尔值解锁"的方案对它无效——这是与 `影子跟读Pro`（端上有判定逻辑）最大的差别，也解释了为什么这类 App 的网络层插件基本打不动。

## 5. 本地回环服务

字符串里存在 `http://127.0.0.1:` 与 `http://localhost:`，配合模块 `Swifter`（内嵌 HTTP 服务器）和 80 处 `webdav` 命中，指向同一件事：**它在设备上起了一个本地 WebDAV 服务**，供系统「文件」App 或其他设备读写书库。复刻时这条比云端同步更实用（不花钱、不限速）。
