# 框架清单（解密阶段实测页数，2026-11-02）

来源：`fairplay-decrypt-v2` 解密日志。总计 **42 个二进制 / 41,376 页**。
页数≈代码体量，用来判断"哪块是重资产"，比文件大小更准（资源文件不占页）。

## 按页数排序（只列 ≥40 页的）

| 页数 | 二进制 | 指向 |
| ---: | :--- | :--- |
| 15,880 | `Runner`（主程序） | Flutter 宿主 + 业务层 |
| 6,024 | `sherpa_onnx.framework` | 端上 ASR / VAD（k2-fsa sherpa-onnx） |
| 4,564 | `libavcodec.framework` | FFmpeg 编解码 |
| 1,960 | `libavfilter.framework` | FFmpeg 滤镜（含 subtitles/libass 链路） |
| 1,748 | `dartcv.framework` | OpenCV 的 Dart 绑定（图像处理） |
| 980 | `libavformat.framework` | FFmpeg 容器/解复用 |
| 588 | `TTSDKPlayerCoreLiveLite.framework` | 火山引擎播放内核（直播 Lite） |
| 440 | `Xml2.framework` | XML 解析 |
| 316 | `TTSDKLivePlayerLite.framework` | 火山直播播放器 |
| 144 | `TTSDKLiveBase.framework` | 火山直播基础库 |
| 104 | `libavutil.framework` | FFmpeg 工具库 |
| 100 | `libswscale.framework` | 色彩空间转换 |
| 80 | `ffmpegkit.framework` | FFmpegKit 封装 |
| 48 | `libswresample.framework` | 音频重采样 |
| 44 | `Uchardet.framework` | 字符编码探测 |
| 12 | `libavdevice.framework` | 采集设备 |

## 从清单能直接读出的三件事

1. **端上 ASR 是真的**：`sherpa_onnx` 6024 页，仅次于主程序。这不是"预留接口"，是打包进包里的完整推理栈。之前报告说"AI 语音打分"大概率走它 + 自有服务端两条路，这里至少端上那条有实证。
2. **图像处理走 OpenCV 而非 CoreImage**：`dartcv` 1748 页，说明字幕 OCR / 截帧处理是 Flutter 侧调 OpenCV，OCR 引擎要看 Vision 的符号（下一轮查）。
3. **视频播放是双栈**：FFmpeg（libav* + ffmpegkit）负责本地/自选素材，火山 `TTSDK*` 负责直播拉流。做复刻时这两条要分开处理，不能只抄一条。
