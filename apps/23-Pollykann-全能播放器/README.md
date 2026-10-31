# 23-Pollykann 全量逆向架构图纸

- **Bundle ID**: `com.pollykann.app`
- **版本**: `4.4`    **最低系统**: `15.6`
- **技术栈**: `libmpv`
- **主二进制**: `61.83 MB`，包内文件 `1140` 个，动态库 `55` 个
- **提取规模**: 原生类 `718` 个 / 应用源文件路径 `2963` 条 / 接口 `912` 条 / 自有域名 `11` 个
- **扩展**: `PollykannActionExt, PollykannNotificationServiceExt`

## 资产清单

| 文件 | 内容 |
|---|---|
| `recon/all_classes.txt` | 718 个原生类/控制器全表 |
| `recon/source_files.txt` | 2963 条应用源文件 + 345 条内嵌库源文件（编译期 `#file` 路径） |
| `recon/endpoints.txt` | 912 条 URL + 0 条接口路径 + 11 个自有域名 + 3325 个依赖域名 |
| `recon/swift_types.txt` | Swift 类型分类（class/struct/enum/protocol） |
| `app_graph.yaml` | REA 6.8.0 投影的 Application Graph 证据链 |
| `全功能零遗漏规格说明书.md` | 按功能维度的组件分布与职责 |
