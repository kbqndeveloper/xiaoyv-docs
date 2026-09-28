---
params: "options: table"
returns: "session, nil, status 或 nil, error, status"
---

**方法名称：** 打开虚拟机内存会话。

**语法：** `vm.open(options)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `options` | `table` | 是 | 连接选项，见下表。 |

| 返回值 | 说明 |
|---|---|
| `session, nil, vm.OK` | 会话 userdata；复用它进行后续操作。 |
| `nil, error, status` | 打开失败，不应使用第 4 项中可能存在的无效会话。 |

| 选项 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| `host_pid` | `integer` | `0` | VMware 模式必填，宿主 `vmware-vmx.exe` 的 PID。 |
| `device` | `string` | 空 | 可选的 LeechCore 数据源字符串。 |
| `runtime_directory` | `string` | 插件所在目录 | UTF-8 运行库目录。 |
| `write` | `boolean` | `false` | 是否请求写入权限。 |
| `verbose` | `boolean` | `false` | 是否启用第三方诊断输出。 |

**使用示例：**

```lua
local vm = require("VMmemory")
local session <close> = assert(vm.open({host_pid = 1234})) -- 替换实际宿主 PID
print(assert(session:system()).windows_build)
```

**详细说明：**

默认只读，且 VMware 数据源同时按只读方式打开。`device` 非空时改用该 LeechCore 数据源，不再用 `host_pid` 定位 VMware；是否可写仍取决于数据源。

默认在 `VMmemory.dll` 同目录加载 `vmm.dll`、`leechcore.dll` 及其配套文件；标准发布位置是 `runtime/x64/`，许可证在 `runtime/licenses/vmmemory/`。`runtime_directory` 是完整运行库目录，不是单个 DLL 路径；移动运行库时建议传绝对路径。不要混用不同发行版本的运行库。

仅支持 x64 Worker；加载与 Worker 接口不匹配时 `require` 会报错。打开成功后用 [close](会话/close.md)、Lua `<close>` 或 GC 释放。
