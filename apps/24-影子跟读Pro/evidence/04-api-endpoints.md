# 接口清单（Dart AOT 字面量，2026-11-02）

来源：`Frameworks/App.framework/App` 的 printable strings，正则 `^/api/[a-zA-Z][\w/\-]{1,60}$`。命中 **61 条**。
基址域名：`https://video-app-api.chengmu.cc`（另有 H5 域名 `yd-h5.chengmu.cc`，见 03-dart-layer.md）。

> 注意：部分字面量带尾部 `r`（如 `/api/home/configr`、`/api/plan/creater`、`/api/baiduWp/dirr`），这是字符串池里的原始形态，实际路径可能是 `config`/`create`/`dir`。**未逐个请求验证前不要当定论**。

## agent（1）

- `/api/agent/bind`

## ams（1）

- `/api/ams/auth/qr/login`

## auth（4）

- `/api/auth/login`
- `/api/auth/updateUser`
- `/api/auth/uploadAvatar`
- `/api/auth/user`

## baiduWp（5）

- `/api/baiduWp`
- `/api/baiduWp/baiduSync`
- `/api/baiduWp/dirr`
- `/api/baiduWp/fileDownloadInfo`
- `/api/baiduWp/fileInfo`

## crash（1）

- `/api/crash/log`

## getCode（1）

- `/api/getCode`

## home（2）

- `/api/home/configr`
- `/api/home/section/media`

## media（4）

- `/api/media/categoryMediaList`
- `/api/media/episode`
- `/api/media/report`
- `/api/media/single`

## pan（1）

- `/api/pan/dir`

## pay（6）

- `/api/pay/creditConfig`
- `/api/pay/creditInfo`
- `/api/pay/creditProducts`
- `/api/pay/getPurchased`
- `/api/pay/iosPayVerify`
- `/api/pay/productsNew`

## plan（6）

- `/api/plan`
- `/api/plan/creater`
- `/api/plan/days`
- `/api/plan/days/detail`
- `/api/plan/days/detail/check`
- `/api/plan/deleter`

## public-study-plan（7）

- `/api/public-study-plan/clip/complete`
- `/api/public-study-plan/day`
- `/api/public-study-plan/detail`
- `/api/public-study-plan/join`
- `/api/public-study-plan/list`
- `/api/public-study-plan/quit`
- `/api/public-study-plan/reset`

## soe（2）

- `/api/soe/v2/assess`
- `/api/soe/v2/quota`

## statics（4）

- `/api/statics/heatmap`
- `/api/statics/newr`
- `/api/statics/ranking`
- `/api/statics/videoPageVisit`

## subtitle（1）

- `/api/subtitle/collectionSubtitleStatement`

## third（1）

- `/api/third/bili/parse`

## version（1）

- `/api/version`

## voice-recognition（3）

- `/api/voice-recognition/task`
- `/api/voice-recognition/tasks`
- `/api/voice-recognition/upload`

## wishlist（3）

- `/api/wishlist/create`
- `/api/wishlist/list`
- `/api/wishlist/toggleLike`

## word（7）

- `/api/word/add`
- `/api/word/category/add`
- `/api/word/category/del`
- `/api/word/category/get`
- `/api/word/deleteWord`
- `/api/word/get`
- `/api/word/wordList`
