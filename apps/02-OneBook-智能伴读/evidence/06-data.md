# 06 · 本地数据与存储

## 1. 数据库引擎

`GRDB`（SQLite 封装，模块内有 38 个类）+ 直接 `libsqlite3`。迁移表 `grdb_migrations` 说明版本化迁移是标准 GRDB 流程，不是手写 SQL 补丁。

全文检索用 **FTS5**：`FTS5` 29 次、`fts5` 6 次、`MATCH` 45 次，分词器 `porter`（英文词干）+ `unicode61`。这说明它的词表检索是 SQLite 原生 FTS5，不需要外部搜索引擎。

## 2. 应用表（从 `INSERT INTO` / `CREATE TABLE` 原文提取）

| 表 | 字段线索 | 用途 |
| :--- | :--- | :--- |
| `book_stats_cache_v2` | `bookId TEXT PK`、`totalWordCount`、`vocabularyCount`、`cefrCount`、`a1Count`、`a2Count`、`b1Count`… | 每本书的词频画像，**按 CEFR 等级统计**（A1/A2/B1…），用来判断这本书适不适合当前水平 |
| `vocabularyWordTapEvent` | 事件表 | 记录每一次点词，用于"常查词"排序与复习队列 |
| `word_export_history_v2` | 导出历史 | 生词导出记录（注意：是"导出历史"，**不是 Anki 包**） |
| `vector_dictionary` | 向量表 | 词向量，做近义词/语义查词 |
| `news_feed_item` | 资讯条目 | `/v1/news` 的本地缓存 |
| `grdb_migrations` | `identifier TEXT PK` | 迁移版本 |

`book_stats_cache_v2` 的 CEFR 字段是这份图纸里最容易被忽略、但对德语学习最值钱的一列：它说明"给这本书打分、判断难度"是端上算的，不依赖服务端。

## 3. 词典三件套

| 文件 | 大小 | 形态 | 说明 |
| :--- | ---: | :--- | :--- |
| `dictionary.sqlite.lzfse` | 38.0 MB | LZFSE 压缩的 SQLite | 首发内置词典，首次启动解压到应用支持目录 |
| `phrase_lexicon.sqlite` | 4.4 MB | 明文 SQLite | 短语/搭配库，表 `lexicon_term`、`lexicon_meta` |
| `ExtraPython/site-packages/mdict_utils` | — | Python 包 | 用户自己导入的 MDX/MDD 词典由 Python 侧解析 |

在线兜底：`dict.youdao.com/jsonapi?q=`（中文用户习惯）、`api.dictionaryapi.dev/api/v2/entries/en/`（英文免费）、`mdx.mdict.org/Recommend/`（词典包下载，包内直接写了 **Duden 10 Volume** 德语词典的 URL）。

## 4. 存储位置与格式

- `Application Support` 与 `Documents/` 各有 2 处引用 → 书库与数据库分开放（数据库在 Application Support，用户可见文件在 Documents）。
- `-wal` 存在 → 开了 WAL 模式，读写并发安全。
- 书本资源用 `ZIPFoundation` 处理（EPUB/CBZ 本质都是 zip）。
- 音频不进数据库：只存路径与时间轴，音频文件与书同目录（`BookBackgroundDownloadCoordinator` 负责后台续传）。
- LZFSE 是 Apple 自带压缩（`libcompression`），没有引入 zstd（`zstd` 零命中）。

## 5. 同步与备份的落地形态

| 通道 | 数据边界 | 证据 |
| :--- | :--- | :--- |
| 本地 WebDAV 服务 | 对外暴露书库目录 | `Swifter` 模块、`127.0.0.1:`、`webdav` 80 处 |
| iCloud / CloudKit | 进度、书签、生词 | `CKRecord`、`CloudSyncCoordinator` |
| 百度网盘 | 整本备份 | `BaiduNetdiskBackupService`、`/rest/2.0/xpan/file` |
| 自有服务端 | 增量 push | `/api/sync/v1/push` |

四种通道并存说明它的同步层是抽象过的（一个 `CloudSyncCoordinator` 分发），复刻时值得照这个层次做，而不是把同步写死在某个云里。
