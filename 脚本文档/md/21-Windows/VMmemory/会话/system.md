---
params: ""
returns: "info, nil, status 或 nil, error, status"
---

**方法名称：** 查询会话和目标系统信息。

**语法：** `session:system()`

**参数说明：**

无。

| 返回值 | 说明 |
|---|---|
| `info, nil, vm.OK` | 系统信息表，字段见下表。 |
| `nil, error, status` | 会话不可用。 |

| 字段 | 说明 |
|---|---|
| `abi_version` | 插件接口版本。 |
| `capabilities` | `CAP_*` 能力位组合。 |
| `system_type` | MemProcFS 系统类型；Windows x64/x86 为 `2/4`。 |
| `memory_model` | x86、x86 PAE、x64、ARM64 分别为 `1/2/3/4`。 |
| `windows_major/minor/build` | Guest Windows 版本。 |
| `writable`、`live` | `0/1` 整数，分别表示可写和实时数据源。 |
| `vmm_major/minor/revision` | MemProcFS 版本。 |
| `leechcore_major/minor/revision` | LeechCore 版本。 |

**使用示例：**

```lua
local info = assert(session:system())
print(info.windows_major, info.windows_minor, info.windows_build)
print(info.writable == 1, info.live == 1)
```

**详细说明：**

位标志使用按位与判断，例如 `(info.capabilities & vm.CAP_SEARCH) ~= 0`。能力标志说明见[常量与返回状态](../常量与返回状态.md)。这里表示打开会话时取得的信息，不是目标内存的一致性快照。
