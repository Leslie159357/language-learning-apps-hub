# P2 定向问诊记录（MCP + IDA，2026-11-02）

样本：`baseline/Payload/Runner.app/Frameworks/TTSDKReachability.framework/TTSDKReachability`
sha256 `453c2564dfd764d8e36d0b7a43fa3eef50caf311df16fe799e0b1ab02e5753ac`

## 会话时序（MCP）

| 步骤 | 工具 | 结果 |
| :--- | :--- | :--- |
| 1 | `open_binary`（provider_id=ida） | 成功，`format=mach-o` `architecture=arm64`，sha256 同上 |
| 2 | `list_procedures` detail=summary | 162 条，`coverage.status=complete-within-view`，`exhausted=true`，evidence `ev_c41a171e…` |
| 3 | `list_procedures` detail=complete | address/value 全表，evidence `ev_734259ae…` |
| 4 | `procedure_pseudo_code` | 14 行 Hex-Rays 伪代码，evidence `ev_a405962c…` |

上游身份（`raw_result.upstream`）：`ida-pro-mcp 1.0.0`；IDB：`/tmp/rea-ida-DaE5j3/TTSDKReachability.i64`；`imagebase=0x0`；`hexrays_ready=true`；`strings_cache_size=293`。

## 抽检函数

`-[TTReachability networkStatusForFlags:]` @ `0x890c`（size 0x34）：

```c
signed __int64 __cdecl -[TTReachability networkStatusForFlags:](TTReachability *self, SEL a2, unsigned int a3)
{
  signed __int64 v3; // x8
  v3 = (a3 & 4) == 0;                     /*0x8910*/
  if ( (a3 & 0x10) == 0 && (a3 & 0x28) != 0 ) /*0x8920*/
    v3 = 1;                                /*0x8924*/
  if ( (a3 & 0x40000) != 0 )               /*0x8930*/
    v3 = 2;                                /*0x8930*/
  if ( (a3 & 2) != 0 )                     /*0x8938*/
    return v3;                             /*0x8938*/
  else
    return 0;                              /*0x8938*/
}
```

语义可读：SCNetworkReachabilityFlags → 0/1/2 状态映射（`kSCNetworkReachabilityFlagsReachable` / `ConnectionRequired` / `IsWWAN`）。

## 值得记住的两条

1. **MCP 是有状态会话**：不先 `open_binary` 直接查会返回 `target_unavailable`（"No app is open"）；CLI 是无状态的，每条命令自己带路径。这是我前面"批量走 CLI、定向走 MCP"之所以成立的底层原因，不是风格问题。
2. **IDA adapter 的 limitations 不能一概而论**：`list_procedures` 的 limitations 里写着 "Complete function body ranges, typed references, unresolved indirect calls, referenced data, and basic blocks are unavailable in this adapter"，但 `procedure_pseudo_code` 实际能出完整 Hex-Rays 输出（`hexrays_ready=true`）。前者描述的是 dossier 结构化面，不是反编译能力。**引用 limitation 前先测一次**，否则会把能力误判成缺失。

## 会话收尾

`close_binary` 释放 IDA worker（headless 进程随会话生命周期回收，不关会常驻）。
