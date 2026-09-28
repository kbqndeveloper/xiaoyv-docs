---
params: "option: integer|string|userdata"
returns: "value, nil, status 或 nil, error, status"
---

**方法名称：** 读取底层配置。

**语法：** `session:config_get(option)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `option` | `integer\|string\|userdata` | 是 | MemProcFS 64 位配置编号；可用十六进制字符串避免精度丢失。 |

| 返回值 | 说明 |
|---|---|
| `value, nil, vm.OK` | 无符号配置值；超出 Lua 有符号整数范围时为地址对象。 |
| `nil, error, status` | 不支持、禁止访问或读取失败。 |

**使用示例：**

```lua
-- VMMDLL_OPT_CONFIG_TICK_PERIOD：缓存基础周期，单位毫秒。
local milliseconds = assert(session:config_get("0x2000000400000000"))
print(milliseconds)
```

**详细说明：**

进阶接口，不把全部 MemProcFS 常量注入 Lua。不会导出底层 LeechCore 句柄或 VMM 实例标识。常用可设置编号见 [config_set](config_set.md)；一般缓存刷新直接用 [refresh](refresh.md)。
