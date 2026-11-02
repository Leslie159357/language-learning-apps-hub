# 02 · 符号普查

统计口径：`__objc_classname`（含 `_TtC*` 运行时类名，可反解所属模块）与 `__swift5_types`（类型描述符）。两者都是编译器写进二进制的元数据，不是日志或界面文案。

## 模块归属

| 模块 | 类数 | 性质 |
| :--- | ---: | :--- |
| `OneBook` | 257 | 应用自有代码（本文分析主体） |
| `<objc/other>` | 162 | 系统/ObjC 未带 Swift 前缀，含 Foundation 协议等 |
| `SwiftSoup` | 55 | HTML 解析（第三方源码内联） |
| `GRDB` | 38 | SQLite 封装（第三方源码内联） |
| `StreamingTtsPlayerKit` | 18 | 自研或第三方的流式 TTS 播放器 |
| `FSRS` | 9 | 间隔重复算法（现代替代 SM-2） |
| `Swifter` | 9 | 内嵌 HTTP 服务器（供 WebDAV 用） |
| `StoryAlignCore` | 8 | 字幕/音频强制对齐内核 |
| `SwiftWhisper` | 2 | 端上 Whisper 封装 |
| `ZIPFoundation` | 2 | ZIP 处理 |
| `PythonSupport` | 1 | Python 3.10 运行时桥接 |

## 应用自有类角色分布

| 角色 | 数量 |
| :--- | ---: |
| Service | 133 |
| other | 58 |
| Data | 33 |
| Parsing | 27 |
| UI | 6 |

## 应用自有类（257）按功能域

### 未归类（76）

```
AppDatabase
ArticleFullTextService
BERTTokenizer
BookManager
BookPageScannerService
BookSourceHubModeStore
BookSourceStore
CBRToCBZConverter
CacheManager
ClaudeProvider
ContactMeLoader
CoreMLEmbeddingProvider
CustomTextView
DBManager
DeveloperWorksLoader
DocxToEPUBConverter
DocxXMLParser
DouyinParser
EPUB3NavigationParser
EPUB3OPFParser
EPUB3Parser
EPUB3SMILParser
EPUBPackageNCXParser
EPUBPackageOPFParser
EPUBPackageService
EmbeddingService
EvaluationEngine
GeminiProvider
ImageCacheService
ImageOCRBoxProvider
LastContinueOpenCoordinator
LastContinueStore
LearnCommunicateSession
LearnTalkLiveOrbRenderer
LocalGemmaProvider
LocalRAGEngine
ManualGuideLoader
MediaOfflineCacheService
MediaWatchStore
MockQwenService
NetworkConnectivitySnapshot
OneBook
OneBookAppDelegate
OneBookAuthStore
OneBookGatewayAuth
OnlineTTSProfileStore
PDFExclusionZoneCanvasController
PDFOCRBoxProvider
PDFOCRIndexingProgress
PDFToEPUBConverter
PythonManager
ReadWorldAuthStore
ReadiumDeobfuscator
ReadiumEncryptionParser
ReadiumOPFParser
ReadiumTOCParser
RemoteAssetService
RemoteBGMService
RemoteContactService
RemoteDeveloperWorksService
RemoteManualGuideService
ScanPageService
ScreenAnnotationTapGestureRecognizer
SiliconFlowService
SmartTextAnalyzer
SpeechRecognizer
SqliteVecManager
StorageInspector
TTSBackgroundMusicManager
TTSManager
TTSObserver
TxtToEPUBConverter
VectorKnowledgeBase
WiFiTransferService
XiaohongshuParser
YTDLPWrapper
```

### AI 分析（41）

```
AIGatewayConfigStore
AnalysisChatStore
BaiduNetdiskBackupCoordinator
BaiduNetdiskBackupService
BaiduNetdiskOAuthService
BaiduNetdiskTokenStore
BookCoverThumbnailService
BookPageScannerViewModel
BookSelectionViewModel
BookSourceViewModel
CommunityViewModel
ContextPhraseSectionViewModel
DialogueViewModel
EPUBPackageContainerParser
KeychainHelper
LLMGatewayResolvedModelStore
LLMProviderFactory
LocalGemmaInferenceService
LocalGemmaModelManager
MainContentVerticalScrollMonitor
MyBooksViewModel
NewsListViewModel
NewsThumbnailImageCache
OpenAIProvider
PodcastLanguageGridViewModel
PodcastLanguageRecommendViewModel
ProfileViewModel
ReadiumContainerParser
RemoteAnalysisService
SavedLLMProfileStore
ScanPageSentenceActionViewModel
ScanPageViewModel
ScanViewModel
ScannedPageDetailViewModel
SentenceAnalysisService
VocabularyExportViewModel
VocabularySentenceTranslateModel
VocabularyViewModel
WhisperModelManager
YouTubeEmbedContainerView
YoutubeRSSViewModel
```

### 导入/添加（31）

```
ApplePodcastImportService
BookBackgroundDownloadCoordinator
BookBackgroundDownloadManager
BookDownloadService
BookImportService
ClipboardVideoImportPromptStore
DouyinShareWebFetcher
DownloadProgressDelegate
EPUBImportService
IncomingBookImportService
PDFImportService
RecommendedChannelsService
ShareActivityItemSource
VideoDownloadService
VideoImportService
WebArticleParser
WebDAVAutoBackupScheduler
WebDAVBackgroundUploadCoordinator
WebDAVBackgroundUploadManager
WebDAVBackupConfig
WebDAVBackupHistoryStore
WebDAVBackupOrchestrator
WebDAVBackupRecoveryManager
WebDAVBackupService
WebDAVBackupSnapshotPreparer
WebDAVRestoreService
WebDownloadBookmarkStore
WebDownloadRecommendStore
WebToEPUBConverter
YouTubeEmbedWebHost
YoutubeDirectImportService
```

### 阅读器（27）

```
ArticleReaderContentResolver
EPUBChapterTextExtractionCache
EPUBReaderWebViewPrewarmService
FoliateBookMetadataService
FoliateEPUBZipAssetStore
FoliateImportSectionSplitService
FoliateReaderWebView
FoliateURLSchemeHandler
HTMLReaderLiveAppearanceController
HTMLReaderSchemeHandler
ImportedReaderFontStore
PDF3ReaderViewModel
PDFReaderInkCanvasAdapter
PDFReaderInkViewportTracker
PlayableUITextView
ReaderInkCanvasHost
ReaderInkPageStore
ReaderInkPencilOnlyCanvasView
ReaderInkSideNotebookStore
ReaderInkViewportSyncScheduler
ReaderSentenceActionViewModel
ReaderSettings
ReaderWordDetailViewModel
ReadingActivityService
RubyUIView
UniversalReaderViewModel
VideoShadowReaderViewModel
```

### 书架/库（19）

```
BaiduShelfAPIClient
BaiduShelfAudioOnDemandActivity
BaiduShelfAudioOnDemandFetcher
BaiduShelfBookIntroViewModel
BaiduShelfDownloadQueue
BaiduShelfDownloadService
BaiduShelfMySubmissionsViewModel
BaiduShelfRecommendViewModel
BaiduShelfSubmissionAPIClient
BaiduShelfSubmitViewModel
BaiduShelfTextLayerPackageBuilder
BaiduVideoShelfAPIClient
BaiduVideoShelfDownloadService
BaiduVideoShelfRecommendViewModel
CatalogRemoteImageCache
OPDSCatalogService
PodcastRecommendCatalogStore
RecommendedBundledCatalogService
RemoteCatalogLocalPresence
```

### 音频/播放（13）

```
AudioRecorderManager
BookPlaybackSleepTimer
BookSleepTimerLiveActivityManager
ExclusiveMediaPlaybackCoordinator
M4BAudiobookPlayerViewModel
MediaPlaybackCacheRangeFetcher
MediaPlaybackCacheResourceLoader
NativeAudioTextParser
NativeAudioTextParserStack
PDFEmbeddedAudioPlaybackService
ShadowingAudioPlayer
VideoPlayerViewModel
YouTubeEmbedPlaybackSession
```

### 翻译（10）

```
AppLanguageManager
AppleNaturalLanguageEmbedding
AppleTranslationManager
AppleTranslationPrepareRequest
CoreMLTranslationService
ONNXTranslationService
TencentTranslationService
TranslationConfig
TranslationManager
YoudaoTranslationProvider
```

### 字幕/对齐（9）

```
CloudSyncAPIClient
CloudSyncCoordinator
HeadlessSubtitleFetcher
NarratedEpubAlignLogger
NarratedEpubAlignmentViewModel
SubtitlePDFExporter
SystemSpeechTranscriptionService
VideoTranscriptionService
WhisperKitAlignTranscriber
```

### 生词/复习（7）

```
GradedWordSuppressionStore
ImmersiveWordTagsScrollViewController
NonGradedWordHighlightStore
OnlineTTSPreviewService
VocabularyContextEnrichmentGate
VocabularyPDFExportService
WordAnnotationService
```

### 播客（6）

```
ApplePodcastHeadlessFetcher
ITunesPodcastSearchService
NewsFeedStorage
NewsRSSService
NewsReadStore
PodcastRSSService
```

### B站/视频源（5）

```
BilibiliParser
VideoDatabaseMigrationService
YouTubeStreamCache
YoutubeRSSService
YoutubeVideoMetadataService
```

### 词典/查词（5）

```
DictionaryService
ExternalDictionaryStore
LocalPhraseLexiconService
OnlineDictionaryService
WordLookupUserFocusTipsStore
```

### 设置/其他（4）

```
AboutOneBookLoader
RemoteAboutService
SpeechHelper
TTSSettingsManager
```

### 跟读/录音（2）

```
ComicPageImageLoader
ComicPageOCRProvider
```

### 订阅/额度（2）

```
OneBookQuotaStore
SubscriptionStore
```
