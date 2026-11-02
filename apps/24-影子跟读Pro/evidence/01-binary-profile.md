# 二进制画像 · Runner（主二进制，2026-11-02 实测）

样本：`baseline/Payload/Runner.app/Runner`，73.61 MB，sha256 `2f4fbe7e…`（见 SAMPLE.md）
分析库：`evidence/Runner.i64`（977 MB，IDA 9.5 离线建库，`idat -B -A` 约 18 分钟）
查询方式：`mcp__ida__open_database` + `execute_python`（毫秒级，不重新分析）

## 硬指标

| 指标 | 值 | 取得方式 |
| :--- | :--- | :--- |
| 函数数 | 329,014 | `len(list(idautils.Functions()))` |
| 字符串数 | 129,489 | `len(list(idautils.Strings()))` |
| 符号总数 | 346,053 | `len(list(idautils.Names()))` |
| ObjC 类 | 3,537 | 名字前缀 `_OBJC_CLASS_$_` |
| Swift 类/类型符号 | 1,313 | `_$s` / `__TtC` 前缀 |
| Flutter/Dart 相关符号 | 2,545 | 名字含 `flutter`/`dart`（含 `FFmpegKitFlutterPlugin`） |

这几个数字解释了为什么 REA 的 `analyze` 在这台机器上跑不动：它要把 32.9 万个函数逐条枚举进 dossier，单核 45 分钟仍没结束（实测 100% CPU，最后手动终止）。**33 万函数量级的样本，analyze 不是可用路径。**

## 与前次报告的差异

| 指标 | 旧报告 | 本次实测 | 说明 |
| :--- | :--- | :--- | :--- |
| 主二进制 | 70.20 MB | 73.61 MB | 不同 build 或不同解密产出，待核 |
| 原生类 | 1,345 | — | 旧口径未注明是"原生类"还是"ObjC+Swift 去重"，无法对齐 |
| ObjC 类（Mach-O 定义） | 3,271 | 3,537 | 口径接近（都是 `_OBJC_CLASS_$_`），差异 266，可能是版本差 |
| 方法选择器 | 44,740 | — | 未复算（需遍历 method_list，成本较高） |

结论层（Flutter / libmpv / FFmpegKit / ONNX 技术栈，订阅与权益判定链）与本次实测一致：`FFmpegKitFlutterPlugin`、`GeneratedPluginRegistrant`、`_TtC6Runner11AppDelegate` 均在符号表里可见。

## 对流程的意义

- **>50 MB 或 >10 万函数**：先 `idat -B -A` 离线建 `.i64`，再用 `mcp__ida__open_database` + `execute_python` 直查。建库是一次性成本，查询是毫秒级。
- **REA 不吃 `.i64`**：`rea analyze Runner.i64` 返回 `target_unavailable / unsupported binary format`。REA 只接原始二进制，且自己重新驱动 idalib 导入 —— 所以"离线建库给 REA 复用"这条路不存在。
- REA 的正确位置是**封装与比对**（evidence_id、limitations、compare、跨版本），不是大样本的首次枚举。
