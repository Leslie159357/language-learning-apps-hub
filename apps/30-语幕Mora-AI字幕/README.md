# 30-语幕 Mora 全量逆向架构图纸

- **Bundle ID**: `com.kkapp.mora`
- **版本**: `1.3`    **最低系统**: `17.0`
- **技术栈**: `Native Swift/ObjC`
- **主二进制**: `10.95 MB`，包内文件 `123` 个，动态库 `0` 个
- **提取规模**: 原生类 `713` 个 / 应用源文件路径 `111` 条 / 接口 `11` 条 / 自有域名 `0` 个
- **扩展**: `VideoTransAction, VideoTransShare`

## 资产清单

| 文件 | 内容 |
|---|---|
| `recon/all_classes.txt` | 713 个原生类/控制器全表 |
| `recon/source_files.txt` | 111 条应用源文件 + 0 条内嵌库源文件（编译期 `#file` 路径） |
| `recon/endpoints.txt` | 11 条 URL + 0 条接口路径 + 0 个自有域名 + 67 个依赖域名 |
| `recon/swift_types.txt` | Swift 类型分类（class/struct/enum/protocol） |
| `app_graph.yaml` | REA 6.8.0 投影的 Application Graph 证据链 |
| `全功能零遗漏规格说明书.md` | 按功能维度的组件分布与职责 |
