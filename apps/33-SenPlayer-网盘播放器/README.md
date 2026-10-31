# 33-SenPlayer 全量逆向架构图纸

- **Bundle ID**: `com.wuziqi.SenPlayer`
- **版本**: `6.2.3`    **最低系统**: `16.4`
- **技术栈**: `FFmpegKit`
- **主二进制**: `61.04 MB`，包内文件 `525` 个，动态库 `28` 个
- **提取规模**: 原生类 `870` 个 / 应用源文件路径 `796` 条 / 接口 `75` 条 / 自有域名 `0` 个

## 资产清单

| 文件 | 内容 |
|---|---|
| `recon/all_classes.txt` | 870 个原生类/控制器全表 |
| `recon/source_files.txt` | 796 条应用源文件 + 609 条内嵌库源文件（编译期 `#file` 路径） |
| `recon/endpoints.txt` | 75 条 URL + 0 条接口路径 + 0 个自有域名 + 151 个依赖域名 |
| `recon/swift_types.txt` | Swift 类型分类（class/struct/enum/protocol） |
| `app_graph.yaml` | REA 6.8.0 投影的 Application Graph 证据链 |
| `全功能零遗漏规格说明书.md` | 按功能维度的组件分布与职责 |
