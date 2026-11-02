# SAMPLE — 影子跟读Pro 样本留档

`sample_sha256`（原件加密 IPA）：`f44a631686037537e0ffed8cd3ffaa9ac7483f02e5b3b781a51e3bb79187b635`

本目录按 `rea-reconstruct` 的样本留档铁律建立：`sample/`（原件只读）、`baseline/`（解包基准）、`evidence/`（rea 输出）。
补这一套的原因：此前本 App 的四份报告（`深度架构分析报告.md`、`全功能零遗漏规格说明书.md` 等）成文时原件未留档，报告里的每个计数都无法重算。现在起可复验。

## 样本台账

| 项 | 值 |
| :--- | :--- |
| Bundle ID | `com.chenhuan.video` |
| 显示名 / 版本 | 影子跟读Pro / 1.3.0（build 1.3.0） |
| MinimumOSVersion | 15.1 |
| 加密原件 | `sample/enc.ipa`，261.9 MB，sha256 `f44a6316…87b635` |
| 解密件 | `sample/dec.ipa`，164.7 MB，sha256 `2f4fbe7e…905093` |
| 主二进制 | `Payload/Runner.app/Runner`，73.61 MB，Mach-O 64 arm64 |
| 加密状态 | `LC_ENCRYPTION_INFO_64 cryptid = 0`（已解密，cryptoff 16384 / cryptsize 65044480） |
| 解包基准 | `baseline/`，1040 条目，包内 790 文件，Frameworks 41 个 |
| 解密日期 | 2026-11-02 |

## 可重跑命令

```bash
# 1. 取原件（需该 Apple ID 持有 license）
/opt/data/tools/ipatool/ipatool download -b com.chenhuan.video \
  -o sample/enc.ipa --non-interactive --keychain-passphrase "$IPATOOL_KEYCHAIN_PASSPHRASE"

# 2. 解密（Linux 离线，credential.bin 16 字节）
cd /opt/data/tools/fairplay-decrypt-v2 && ./.venv/bin/python decrypt.py \
  sample/enc.ipa sample/dec.ipa --credential cred/credential.bin

# 3. 解包基准
python3 -c "import zipfile;zipfile.ZipFile('sample/dec.ipa').extractall('baseline')"

# 4. 静态分析（必须走 wrapper，见下）
/opt/data/bin/rea-cli analyze baseline/Payload/Runner.app/Runner --provider ida --format json
```

## 环境要点（2026-11-02 实测）

- `/opt/data/bin/rea-cli` 是 REA CLI 包装：从 `plugins/rea/mcp.json` 抽出 25 个 `REA_*` 变量写入 `/opt/data/docker/rea-agent/rea-cli.env`，再用 `docker exec -i --env-file` 注入。
- **不套这个 wrapper 直调 `docker exec rea-agent rea` 会落到 Ghidra**：容器 `Config.Env` 里没有任何 `REA_*`，`rea analyze` 的 provider 候选只剩 ghidra/hopper/ida/kuna，`ida` 报 `not_configured`（缺 `REA_IDA_MCP_CONFIG`），`hopper` 缺 launcher，`kuna` 未配置。
- 实测代价：65 MB 的 `Reel` 二进制在默认 Ghidra 路径下 330 s 超时失败（`provider_timeout`，`ghidra.startup_timeout_ms=330000`，JVM 堆 2G）。

## 已知不一致（待清）

- `/opt/data/docker/rea-agent/rea-plugin-local/mcp.json` 与 `.../rea-upstream/rea/mcp.json` 仍指向 `ida-mcp-9.4.json`，而现役 `/opt/data/plugins/rea/mcp.json` 指向 `ida-mcp-9.5.json`。IDA 已升到 9.5，这两份属陈旧注册，若被当 fallback 启用会绑错引擎。
