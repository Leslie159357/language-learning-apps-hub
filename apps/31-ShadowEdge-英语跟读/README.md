# 31-ShadowEdge 全量逆向架构图纸

- **Bundle ID**: `com.goshadowing.shadowedge`
- **版本**: `1.1.6`    **最低系统**: `17.0`
- **技术栈**: `Native Swift/ObjC`
- **主二进制**: `11.24 MB`，包内文件 `182` 个，动态库 `0` 个
- **提取规模**: 原生类 `654` 个 / 应用源文件路径 `213` 条 / 接口 `17` 条 / 自有域名 `4` 个

## 资产清单

| 文件 | 内容 |
|---|---|
| `recon/all_classes.txt` | 654 个原生类/控制器全表 |
| `recon/source_files.txt` | 213 条应用源文件 + 0 条内嵌库源文件（编译期 `#file` 路径） |
| `recon/endpoints.txt` | 17 条 URL + 0 条接口路径 + 4 个自有域名 + 29 个依赖域名 |
| `recon/swift_types.txt` | Swift 类型分类（class/struct/enum/protocol） |
| `app_graph.yaml` | REA 6.8.0 投影的 Application Graph 证据链 |
| `全功能零遗漏规格说明书.md` | 按功能维度的组件分布与职责 |
