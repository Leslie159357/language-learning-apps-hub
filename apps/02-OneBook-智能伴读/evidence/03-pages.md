# 03 · 页面与界面结构

页面类来源：主二进制字符串中匹配 `*View/Screen/Page/Sheet/Panel/Row/Cell` 的 304 个候选，其中 **290 个**在 `__swift5_types` 类型描述符表中逐一命中（编译器落盘的类型，非猜测）。未命中的候选不列入，避免把 SDK 类型或日志字符串当成页面。

页面类合计 **290**，按功能归入 **16** 组。

## 未归类（90）

| 页面类 |
| :--- |
| `AnimatedTabView` |
| `BookMediaTileGridView` |
| `BookSelectionView` |
| `BookSourceHubChromeRow` |
| `BookSourceHubView` |
| `BookSourceView` |
| `CategorySelectionView` |
| `CircularProgressView` |
| `ClickActionFlagRow` |
| `CompositionDonutView` |
| `ContactMeView` |
| `ContactRow` |
| `CoverImagePickerView` |
| `CurvedTextView` |
| `CustomTextView` |
| `DayCell` |
| `DeveloperWorkIconView` |
| `DeveloperWorkSectionView` |
| `DeveloperWorksView` |
| `DocumentPickerView` |
| `DonutSliceView` |
| `DummyRow` |
| `EditBookView` |
| `EditPageContentView` |
| `EmptyNotebookContentView` |
| `ExclusionZoneOverlayView` |
| `FullPageNotebookView` |
| `GridBookItemView` |
| `HorizontalLockScrollView` |
| `HorizontalOnlyScrollView` |
| `ImmersiveFrostedPanel` |
| `ImmersiveGrammarAskResultView` |
| `ImmersiveGrammarSentenceView` |
| `ImmersiveGrammarTreeView` |
| `ImmersiveSentenceView` |
| `InfoMessageView` |
| `IntrinsicHeightTextView` |
| `LaunchScreenView` |
| `LearnCommunicateStageView` |
| `LearnHomePracticeQueueRow` |
| `LearnPracticeQueuePickerView` |
| `LearnTalkLiveOrbView` |
| `LearnUnderstandContentView` |
| `LearnUnderstandSentencePanel` |
| `LearnUseLayerView` |
| `LexileAudienceCapsuleView` |
| `LexileLevelCapsulesView` |
| `LexileMeasureCapsuleView` |
| `LineSelectionView` |
| `ListBookCoverView` |
| `ListeningLevelCapsuleView` |
| `LiveTextScannerView` |
| `LoadingOverlayView` |
| `LyingDownBookView` |
| `MediaWatchHistoryRow` |
| `MediaWatchHistoryView` |
| `MenuRow` |
| `MyBooksView` |
| `MyLearnHomeView` |
| `NarratedEpubProgressStepRow` |
| `OPDSBookCoverView` |
| `OneBookManualGuideView` |
| `OnlineTTSEditProfileView` |
| `OnlineTTSManageView` |
| `PDF3KitHostView` |
| `PadPhoneLikeBottomSheet` |
| `PageContentView` |
| `ParticleRingView` |
| `ParticleWaveView` |
| `PhotoPickerView` |
| `PrefetchedRowsView` |
| `ProbeHostView` |
| `ProfileView` |
| `ReorderPagesView` |
| `RubyTextView` |
| `ScanPageView` |
| `ScannedPage` |
| `ScopesTreeView` |
| `ScopesView` |
| `SentenceActionView` |
| `SourceManagementView` |
| `StatItemView` |
| `StickyNoteTabsView` |
| `StorageItemRow` |
| `StorageTreeNodeView` |
| `UnifiedCategoryRow` |
| `UsernameEditView` |
| `VectorRow` |
| `WiFiTransferView` |
| `ZoomableScrollView` |

## AI 分析（31）

| 页面类 |
| :--- |
| `AIAnalysisItemRow` |
| `AIAnalysisResultView` |
| `AIAnalysisView` |
| `AIConfigView` |
| `AnalysisChatPanel` |
| `AnalysisQuickAskAccessoryPanel` |
| `AskAIAnswerContentView` |
| `AskAIInputView` |
| `AskAILoadingStatusRow` |
| `BaiduNetdiskBackupSettingsView` |
| `BaiduNetdiskBrowserView` |
| `BaiduProgressMaskView` |
| `DetailMetricRow` |
| `GrammarAnalysisRow` |
| `ImmersiveGrammarAnalysisPanel` |
| `LearnUnderstandAIDetailPanel` |
| `LocalGemmaModelRow` |
| `LocalPhraseAnalysisContentView` |
| `MainTabView` |
| `ModelRow` |
| `NewsThumbnailView` |
| `SavedLLMProfileCurrentSelectionRow` |
| `SavedLLMProfileSwitcherSheet` |
| `ScannedPageDetailView` |
| `StatisticsDetailView` |
| `StorageDetailView` |
| `VocabularyAnalysisHistoryView` |
| `VocabularyAnalysisResultView` |
| `WhisperModelSettingsView` |
| `WordDetailSheetView` |
| `YouTubeEmbedContainerView` |

## 书架/库（28）

| 页面类 |
| :--- |
| `ArchivedBookRow` |
| `ArchivedBooksSheet` |
| `BaiduShelfBookIntroView` |
| `BaiduShelfDownloadCenterRow` |
| `BaiduShelfDownloadCenterView` |
| `BaiduShelfDownloadChoiceSheet` |
| `BaiduShelfMySubmissionsView` |
| `BaiduShelfRecommendView` |
| `BaiduShelfSubmitView` |
| `BaiduVideoShelfAlbumDetailView` |
| `BaiduVideoShelfCategoryAllView` |
| `BaiduVideoShelfEntryRow` |
| `BaiduVideoShelfPlaylistDetailView` |
| `BaiduVideoShelfRecommendView` |
| `BookGridView` |
| `BookListRowView` |
| `BookListView` |
| `BookShelfReaderDestinationView` |
| `BookStackRow` |
| `BookStackView` |
| `CatalogBookCoverView` |
| `CatalogBookListRow` |
| `CatalogCoverCollageView` |
| `CatalogEmptyStateView` |
| `CatalogPopularDownloadCardView` |
| `CatalogSeriesGroupRow` |
| `CatalogTextCapsuleView` |
| `YouTubeRecommendCatalogDisabledView` |

## 阅读器（28）

| 页面类 |
| :--- |
| `BookReaderDestinationView` |
| `CurrentlyReadingCardView` |
| `FoliateReaderWebView` |
| `FontOptionView` |
| `FontSizeAdjustmentView` |
| `ImmersiveReadingView` |
| `ImportedFontOptionView` |
| `NewsArticleReaderView` |
| `PDF3ReaderView` |
| `PDFBookReaderView` |
| `PlayableUITextView` |
| `ReaderInkPage` |
| `ReaderInkPencilOnlyCanvasView` |
| `ReaderInkSideNotebookCanvasView` |
| `ReaderOperationTipsView` |
| `ReaderOutlineBookmarkRow` |
| `ReaderOutlineMarkRow` |
| `ReaderOutlineSheet` |
| `ReaderSettingsSheet` |
| `ReaderTTSQuickPanel` |
| `ReadingCalendarView` |
| `ReadingLevelGuideView` |
| `RubyUIView` |
| `TypographyToggleRow` |
| `UniversalReaderFavoritesView` |
| `UniversalReaderView` |
| `VideoShadowReaderView` |
| `WebBookReaderView` |

## 导入/添加（24）

| 页面类 |
| :--- |
| `AddBookImportFileHintsView` |
| `AddBookPasteTextSheet` |
| `AddBookProgressPanel` |
| `AddBookView` |
| `AddVideoView` |
| `AddWebDownloadBookmarkSheet` |
| `BilibiliSiteWebView` |
| `BilibiliWebRecordView` |
| `ChannelItemView` |
| `ChannelSearchResultRow` |
| `ChannelSearchSheet` |
| `ChannelSubscriptionListView` |
| `ImportSelectionSheet` |
| `MdxDefinitionWebView` |
| `NarratedEpubAddBookEntryView` |
| `PodcastRecommendChannelDetailView` |
| `ShareSheet` |
| `WebDAVBackupHistoryView` |
| `WebDAVBackupSettingsView` |
| `WebDAVProgressMaskView` |
| `WebDownloadBookmarksView` |
| `WebDownloadBrowserContainerView` |
| `YouTubeSiteWebView` |
| `YouTubeWebImportView` |

## 生词/复习（23）

| 页面类 |
| :--- |
| `BookStatsExportSelectionView` |
| `CalendarView` |
| `GradedWordSettingsRow` |
| `GradedWordSettingsView` |
| `HeatmapView` |
| `ImmersiveWordTagsView` |
| `LearnFlipDeckView` |
| `LearnHomeStatsRow` |
| `LearnUnderstandWordDrillView` |
| `PageVocabularyView` |
| `PreviewRow` |
| `ThreeDScoreView` |
| `VocabularyCardView` |
| `VocabularyEmptyStateView` |
| `VocabularyExportView` |
| `VocabularyLearningStudioView` |
| `VocabularyNotedListView` |
| `VocabularyRowView` |
| `VocabularySearchView` |
| `VocabularyTapTimelineSheet` |
| `VocabularyView` |
| `VocabularyWordListView` |
| `WordOccurrenceSheet` |

## 词典/查词（16）

| 页面类 |
| :--- |
| `ClickActionExternalDictionaryRow` |
| `ContextPhraseSectionView` |
| `DictionaryCardView` |
| `LearnUnderstandPhrasePanel` |
| `LexiconPhraseCardView` |
| `LexiconPhraseRowView` |
| `LexiconTermRow` |
| `LocalPhraseInsightRow` |
| `PhraseCardView` |
| `PhraseNoteReadableContentView` |
| `PhraseRowView` |
| `SQLiteDictionaryLookupView` |
| `SimpleLookupView` |
| `VocabularyLexiconPhraseListView` |
| `VocabularyPhraseListView` |
| `WordLookupUserFocusTipEditorSheet` |

## 字幕/对齐（8）

| 页面类 |
| :--- |
| `CloudSyncSettingsView` |
| `ElegantSubtitleLoadingView` |
| `InteractiveSubtitleView` |
| `ListSubtitleTrackPickerSheet` |
| `NarratedEpubAlignmentView` |
| `OnVideoSubtitleCaptionView` |
| `SubtitleListView` |
| `SubtitleRowView` |

## 设置/其他（7）

| 页面类 |
| :--- |
| `AboutOneBookView` |
| `ClickActionSettingsView` |
| `MarkConfigurationView` |
| `MediaOfflineCacheSettingsView` |
| `SettingsView` |
| `TTSSettingsView` |
| `TextSettingsSheet` |

## 翻译（7）

| 页面类 |
| :--- |
| `ImmersiveTranslationRow` |
| `KokoroLanguageRow` |
| `LanguageRow` |
| `PodcastLanguageGridView` |
| `PodcastLanguageRecommendView` |
| `SelectableTranslationRow` |
| `TTSLanguageSettingsView` |

## 播客（7）

| 页面类 |
| :--- |
| `NewsArticleRow` |
| `NewsListView` |
| `NewsReadHistoryRow` |
| `NewsReadHistoryView` |
| `PodcastEpisodeRow` |
| `PodcastSearchResultRow` |
| `PodcastTabRootView` |

## B站/视频源（6）

| 页面类 |
| :--- |
| `BilibiliPage` |
| `EmptyVideoStateView` |
| `RSSVideoListRow` |
| `VideoListRow` |
| `VideoRecommendHubView` |
| `VideoTabRootView` |

## 音频/播放（5）

| 页面类 |
| :--- |
| `M4BAudiobookPlayerView` |
| `PlayerContainerView` |
| `VideoPlayerLoadingView` |
| `VideoPlayerView` |
| `YouTubeEmbedPlayerView` |

## 同步/备份（4）

| 页面类 |
| :--- |
| `BackupStorageRow` |
| `ExportPhaseRow` |
| `ExportProgressFullScreenView` |
| `LocalBackupSettingsView` |

## 订阅/额度（3）

| 页面类 |
| :--- |
| `RecommendedSubscriptionsView` |
| `SubscriptionPaywallView` |
| `SubscriptionStatusView` |

## 跟读/录音（3）

| 页面类 |
| :--- |
| `ShadowingHistoryView` |
| `ShadowingSheetView` |
| `VideoShadowingScoreDetailView` |
