# 20-德语助手 全量逆向架构图纸

- **Bundle ID**: `eusoft.dehelper.ip`
- **版本**: `26.10.2`    **最低系统**: `15.0`
- **技术栈**: `Native Swift/ObjC`
- **主二进制**: `31.44 MB`，包内文件 `1708` 个，动态库 `4` 个
- **提取规模**: 原生类 `2064` 个 / 应用源文件路径 `508` 条 / 接口 `50` 条 / 自有域名 `1` 个
- **扩展**: `BroadcastUploadExtension_de, Lightpeek_de, SafariExtension_de, TodayDailyWord_de, Today_de, WidgetExtension_de`

## 资产清单

| 文件 | 内容 |
|---|---|
| `recon/all_classes.txt` | 2064 个原生类/控制器全表 |
| `recon/source_files.txt` | 508 条应用源文件 + 3 条内嵌库源文件（编译期 `#file` 路径） |
| `recon/endpoints.txt` | 50 条 URL + 0 条接口路径 + 1 个自有域名 + 111 个依赖域名 |
| `recon/swift_types.txt` | Swift 类型分类（class/struct/enum/protocol） |
| `recon/objc_classes.txt` | 2271 个 Mach-O 内定义的 ObjC 类（含父类） |
| `recon/objc_methods.txt` | 39899 条 ObjC 方法选择器（1902 个类） |
| `recon/objc_protocols.txt` | 334 个 ObjC 协议 |
| `app_graph.yaml` | REA 6.8.0 投影的 Application Graph 证据链 |
| `全功能零遗漏规格说明书.md` | 按功能维度的组件分布与职责 |
