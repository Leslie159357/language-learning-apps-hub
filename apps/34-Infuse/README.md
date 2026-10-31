# 34-Infuse 全量逆向架构图纸

- **Bundle ID**: `com.firecore.infuse`
- **版本**: `8.5.9`    **最低系统**: `18.0`
- **技术栈**: `Native Swift/ObjC`
- **主二进制**: `76.00 MB`，包内文件 `1605` 个，动态库 `61` 个
- **提取规模**: 原生类 `3098` 个 / 应用源文件路径 `1548` 条 / 接口 `109` 条 / 自有域名 `15` 个

## 资产清单

| 文件 | 内容 |
|---|---|
| `recon/all_classes.txt` | 3098 个原生类/控制器全表 |
| `recon/source_files.txt` | 1548 条应用源文件 + 154 条内嵌库源文件（编译期 `#file` 路径） |
| `recon/endpoints.txt` | 109 条 URL + 0 条接口路径 + 15 个自有域名 + 202 个依赖域名 |
| `recon/swift_types.txt` | Swift 类型分类（class/struct/enum/protocol） |
| `app_graph.yaml` | REA 6.8.0 投影的 Application Graph 证据链 |
| `全功能零遗漏规格说明书.md` | 按功能维度的组件分布与职责 |
