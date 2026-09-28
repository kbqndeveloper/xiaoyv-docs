---
params: "flags?: integer"
returns: "true, nil, status 或 nil, error, status"
---

**方法名称：** 刷新会话缓存。

**语法：** `session:refresh([flags])`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `flags` | `integer` | 否 | 默认刷新数据、地址转换和进程；用 `vm.REFRESH_*` 按位或组合。 |

| 返回值 | 说明 |
|---|---|
| `true, nil, vm.OK` | 操作成功。 |
| `nil, error, status` | 操作未完成。 |

**使用示例：**

```lua
assert(session:refresh())
assert(session:refresh(vm.REFRESH_DATA | vm.REFRESH_TRANSLATION))
```

**详细说明：**

默认读取绕过数据缓存，但不等于每次重建页表或进程信息。目标映射变化时可主动刷新；不要为了读取单个字节每次刷新全部缓存。

`flags` 不能为 `0`。常量见[常量与返回状态](../常量与返回状态.md)。
