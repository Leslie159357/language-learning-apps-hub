# 28-木瓜跟读 全量逆向架构图纸

- **Bundle ID**: `cn.luffy.echo`
- **版本**: `1.0.1`    **最低系统**: `17.6`
- **技术栈**: `Native Swift/ObjC`
- **主二进制**: `31.86 MB`，包内文件 `356` 个，动态库 `18` 个
- **提取规模**: 原生类 `794` 个 / 应用源文件路径 `2448` 条 / 接口 `861` 条 / 自有域名 `1` 个

## 资产清单

| 文件 | 内容 |
|---|---|
| `recon/all_classes.txt` | 794 个原生类/控制器全表 |
| `recon/source_files.txt` | 2448 条应用源文件 + 205 条内嵌库源文件（编译期 `#file` 路径） |
| `recon/endpoints.txt` | 861 条 URL + 0 条接口路径 + 1 个自有域名 + 3198 个依赖域名 |
| `recon/swift_types.txt` | Swift 类型分类（class/struct/enum/protocol） |
| `app_graph.yaml` | REA 6.8.0 投影的 Application Graph 证据链 |
| `全功能零遗漏规格说明书.md` | 按功能维度的组件分布与职责 |
