# SAMPLE · OneBook v3.7.24 取证基座

任何结论都可以用下面这段复现。哈希是双份（加密包 / 解密包），解密结果逐个二进制核过 `cryptid=0`。

## 标识

| 项 | 值 |
| :--- | :--- |
| 名称 | OneBook- LearnMultilingual Hub |
| Bundle ID | `com.linyi.OneBook` |
| 版本 | 3.7.24（build 1，2026-10-30 上架） |
| 最低系统 | iOS 15.0 |
| 体积（商店标称） | 234,754,048 B |
| 开发者 | 翊 林 |
| App Store ID | 6749743594 |

## 哈希

```
enc.ipa  sha256  bc04437ac81fdf3a720c35b5d6009961c3e7c54085aa26cd3431999ef91dc9f2   165.6 MB
dec.ipa  sha256  d59bcfb8a8ad2199659ab4299ec46619234aa20092f38934ba171ce1e0a2c0b1   113.9 MB
```

## 复现

```bash
# 1. 取包（需要该 Apple ID 拥有此 App 的 license）
/opt/data/tools/ipatool/ipatool download -b com.linyi.OneBook -o sample/enc.ipa \
  --keychain-passphrase "$IPATOOL_KEYCHAIN_PASSPHRASE" --non-interactive

# 2. 解密（凭据为越狱设备提取的 16 字节 FairPlay 原语）
python3 /opt/data/tools/fairplay-decrypt-v2/decrypt.py \
  --credential /opt/data/tools/fairplay-decrypt-v2/cred/credential.bin \
  sample/enc.ipa sample/dec.ipa

# 3. 展开
python3 -c "import zipfile;zipfile.ZipFile('sample/dec.ipa').extractall('baseline')"
```

## 解密结果

```
decrypted-binaries=13  pages=23184  output=sample/dec.ipa
所有二进制 cryptid=0（无加密残留）
```

| 二进制 | 页数级别 | 大小 |
| :--- | ---: | ---: |
| `OneBook` | 13,320 页 | 57.24 MB |
| `CLiteRTLM` | 主体 | 42.24 MB |
| `libav*` 8 个 | 合计约 3,000 页 | 14.4 MB |
| `OnDeviceWhisperKit` / `OnDeviceGemmaKit` / `ffmpegkit` | — | 2.3 MB |
| 小组件 | 8 页 | 0.11 MB |

## 本目录结构

```
02-OneBook-智能伴读/
├── README.md                    入口
├── REPORT.md                    主报告（结论 + 复刻清单）
├── SAMPLE.md                    本文件：取证基座
├── 深度架构分析报告.md            旧版（保留，已被 REPORT 取代）
├── 全功能零遗漏规格说明书.md      旧版（保留，方法已被 02-symbols 取代）
├── sample/  enc.ipa / dec.ipa   不进 git
├── baseline/ 解包树              不进 git
├── recon/  旧轮抓取的字符串与类名  保留供对照
└── evidence/
    ├── 01-inventory.md      13 个二进制 + 包体构成
    ├── 02-symbols.md        模块归属与 257 个应用自有类
    ├── 03-pages.md          290 个页面类，16 组
    ├── 04-capabilities.md   8 大类能力，逐条带证据
    ├── 05-endpoints.md      3 个自有域名 + 14 条接口 + 20 家供应商
    ├── 06-data.md           SQLite 表、38 MB 词典、FTS5、四条同步通道
    ├── symbols.json         机器可读：类/类型/URL/接口
    ├── modules.json         模块 → 类名清单
    ├── class-map.json       角色 × 功能域矩阵
    ├── probes.json          能力探针命中统计
    └── inventory.json       二进制清单
```

## 边界声明

- 只做了静态分析，**没有运行 App、没有真机抓包**。凡涉及运行时行为的结论（例如某接口实际返回什么）都标为未验证。
- 主二进制已 strip，函数内部实现未逐函数反编译；`04-capabilities.md` 的判断基于符号、类名、资源文件和字符串三类证据交叉，不做"看汇编猜逻辑"。
- `sample/` 与 `baseline/` 合计约 280 MB，只在本地，不入库。
