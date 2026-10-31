# 24-影子跟读Pro 全量逆向架构图纸

- **Bundle ID**: `com.chenhuan.video`
- **版本**: `1.3.0`    **最低系统**: `15.1`
- **技术栈**: `Flutter(Dart AOT) / libmpv / FFmpegKit / ONNX`
- **主二进制**: `70.20 MB`，包内文件 `1040` 个，动态库 `42` 个
- **提取规模**: 原生类 `1345` 个 / 应用源文件路径 `1787` 条 / 接口 `99` 条 / 自有域名 `0` 个

## 资产清单

| 文件 | 内容 |
|---|---|
| `recon/all_classes.txt` | 1345 个原生类/控制器全表 |
| `recon/source_files.txt` | 1787 条应用源文件 + 213 条内嵌库源文件（编译期 `#file` 路径） |
| `recon/endpoints.txt` | 99 条 URL + 0 条接口路径 + 0 个自有域名 + 1051 个依赖域名 |
| `recon/swift_types.txt` | Swift 类型分类（class/struct/enum/protocol） |
| `recon/objc_classes.txt` | 3271 个 Mach-O 内定义的 ObjC 类（含父类） |
| `recon/objc_methods.txt` | 44740 条 ObjC 方法选择器（2498 个类） |
| `recon/objc_protocols.txt` | 537 个 ObjC 协议 |
| `app_graph.yaml` | REA 6.8.0 投影的 Application Graph 证据链 |
| `全功能零遗漏规格说明书.md` | 按功能维度的组件分布与职责 |
