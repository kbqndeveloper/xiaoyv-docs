---
params: ""
returns: "info: XiaoYvWorkerInfo（table）"
---

# getWorkerInfo

**方法名称：** 获取当前 Worker 的编号、位数和进程 ID。

**语法：** `getWorkerInfo()`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| 无 | - | - | 不需要参数。 |

| 返回值 | 说明 |
|---|---|
| `info` | 当前 Worker 信息表，字段见下表。 |

**类型：** `XiaoYvWorkerInfo`（table）

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `id` | `string` | 否 | 中控分配的 Worker ID，例如 `"w1"`；独立运行时为 `nil`。 |
| `arch` | `"x86"\|"x64"` | 是 | 当前 Worker 的进程位数，不是操作系统位数。 |
| `pid` | `integer` | 是 | 当前 Worker 的 Windows 进程 ID。 |

**详细说明：**

直接调用即可，不需要 `require`。主任务和同一 Worker 的子线程读取到相同身份信息。

`id` 对应中控的 `workerId`，不是设备编号、列表位置或 Lua 线程编号。它只在所属中控内标识窗口；强制结束或空闲回收后，在同一列表项重新启动仍沿用此 ID，进程 PID 则可能变化或被系统复用。不要把 `id` 当作多开中控之间的全局唯一编号。

每次调用返回一张新的信息表，修改其中字段不会改变 Worker 身份，也不会影响后续调用。本函数不查询运行状态、不控制其他 Worker。

**使用示例：**

```lua
local info = getWorkerInfo()
print("窗口：", info.id or "独立运行")
print("位数：", info.arch)
print("进程：", info.pid)

if info.arch == "x64" then
    print_user("当前使用 64 位 Worker")
end
```
