# 32-Rex 全量逆向架构图纸

- **Bundle ID**: `tv.rexnow.app`
- **版本**: `1.0.2`    **最低系统**: `17.0`
- **技术栈**: `ONNX`
- **主二进制**: `85.75 MB`，包内文件 `1215` 个，动态库 `24` 个
- **提取规模**: 原生类 `2862` 个 / 应用源文件路径 `1351` 条 / 接口 `83` 条 / 自有域名 `12` 个

## 资产清单

| 文件 | 内容 |
|---|---|
| `recon/all_classes.txt` | 2862 个原生类/控制器全表 |
| `recon/source_files.txt` | 1351 条应用源文件 + 8 条内嵌库源文件（编译期 `#file` 路径） |
| `recon/endpoints.txt` | 83 条 URL + 0 条接口路径 + 12 个自有域名 + 947 个依赖域名 |
| `recon/swift_types.txt` | Swift 类型分类（class/struct/enum/protocol） |
| `app_graph.yaml` | REA 6.8.0 投影的 Application Graph 证据链 |
| `全功能零遗漏规格说明书.md` | 按功能维度的组件分布与职责 |
