# 25-Hanas 全量逆向架构图纸

- **Bundle ID**: `tokyo.hanas.shadowing.ios`
- **版本**: `2.4.3`    **最低系统**: `15.1`
- **技术栈**: `Native Swift/ObjC`
- **主二进制**: `15.42 MB`，包内文件 `568` 个，动态库 `4` 个
- **提取规模**: 原生类 `1338` 个 / 应用源文件路径 `251` 条 / 接口 `34` 条 / 自有域名 `2` 个
- **扩展**: `HanasLiveActivity`

## 资产清单

| 文件 | 内容 |
|---|---|
| `recon/all_classes.txt` | 1338 个原生类/控制器全表 |
| `recon/source_files.txt` | 251 条应用源文件 + 33 条内嵌库源文件（编译期 `#file` 路径） |
| `recon/endpoints.txt` | 34 条 URL + 0 条接口路径 + 2 个自有域名 + 50 个依赖域名 |
| `recon/swift_types.txt` | Swift 类型分类（class/struct/enum/protocol） |
| `recon/objc_classes.txt` | 1327 个 Mach-O 内定义的 ObjC 类（含父类） |
| `recon/objc_methods.txt` | 9328 条 ObjC 方法选择器（801 个类） |
| `recon/objc_protocols.txt` | 210 个 ObjC 协议 |
| `app_graph.yaml` | REA 6.8.0 投影的 Application Graph 证据链 |
| `全功能零遗漏规格说明书.md` | 按功能维度的组件分布与职责 |
