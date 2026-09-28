---
params: "name?: string"
returns: "processes, nil, status 或 nil, error, status"
---

**方法名称：** 枚举或按名称查找 Guest 进程。

**语法：**

```lua
session:processes([name])
session:find_processes([name])
```

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `name` | `string` | 否 | 省略、`nil` 或空字符串枚举全部；非空则按完整名称匹配，不区分大小写。 |

| 返回值 | 说明 |
|---|---|
| `processes, nil, vm.OK` | 进程数组；无匹配时为空数组。 |
| `nil, error, status` | 枚举失败。 |

| 字段 | 说明 |
|---|---|
| `pid`、`parent_pid` | Guest 进程 PID、父进程 PID。 |
| `name` | 进程名称，UTF-8。 |
| `state` | 底层进程状态值。 |
| `pointer_bits` | Guest 进程指针位宽，供指针链等调用使用。 |
| `dtb`、`user_dtb` | 页表地址，完整 64 位地址对象。 |

**使用示例：**

```lua
local matches = assert(session:processes("notepad.exe"))
for _, process in ipairs(matches) do
    print(process.pid, process.name, process.pointer_bits)
end
```

**详细说明：**

两个名字是同一 Lua 方法，不自动选择第一个同名进程。PID 可能随进程退出、重启而变化，不应把示例或上次运行的 PID 写死。
