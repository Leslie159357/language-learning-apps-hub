# 36-EplayerX 全量逆向架构图纸

- **Bundle ID**: `com.snowlee.eplayerx`
- **版本**: `1.9.5`    **最低系统**: `15.1`
- **技术栈**: `Native Swift/ObjC`
- **主二进制**: `36.92 MB`，包内文件 `517` 个，动态库 `29` 个
- **提取规模**: 原生类 `2037` 个 / 应用源文件路径 `667` 条 / 接口 `80` 条 / 自有域名 `5` 个

## 资产清单

| 文件 | 内容 |
|---|---|
| `recon/all_classes.txt` | 2037 个原生类/控制器全表 |
| `recon/source_files.txt` | 667 条应用源文件 + 5 条内嵌库源文件（编译期 `#file` 路径） |
| `recon/endpoints.txt` | 80 条 URL + 0 条接口路径 + 5 个自有域名 + 156 个依赖域名 |
| `recon/swift_types.txt` | Swift 类型分类（class/struct/enum/protocol） |
| `recon/objc_classes.txt` | 1235 个 Mach-O 内定义的 ObjC 类（含父类） |
| `recon/objc_methods.txt` | 5695 条 ObjC 方法选择器（664 个类） |
| `recon/objc_protocols.txt` | 132 个 ObjC 协议 |
| `app_graph.yaml` | REA 6.8.0 投影的 Application Graph 证据链 |
| `全功能零遗漏规格说明书.md` | 按功能维度的组件分布与职责 |
