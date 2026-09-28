---
params: "pid: integer"
returns: "modules, nil, status 或 nil, error, status"
---

**方法名称：** 枚举进程模块。

**语法：** `session:modules(pid)`

**参数说明：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pid` | `integer` | 是 | Guest 内的进程 PID，不是宿主 PID。 |

| 返回值 | 说明 |
|---|---|
| `modules, nil, vm.OK` | 模块数组；每项字段见下表。 |
| `nil, error, status` | 枚举失败。 |

| 字段 | 说明 |
|---|---|
| `base`、`entry` | 模块基址、入口地址，均为地址对象。 |
| `size` | 映像大小，字节。 |
| `name`、`path` | 模块名、路径，UTF-8。 |
| `wow64` | `0/1`，是否为 WOW64 模块。 |
| `section_count`、`export_count`、`import_count` | 节、导出、导入数量。 |

**使用示例：**

```lua
for _, image in ipairs(assert(session:modules(pid))) do
    print(image.name, tostring(image.base), image.size)
end
```

**详细说明：**

这里的模块来自 Guest 进程，不是 Worker 自身加载的 DLL。已知模块名时可直接用 [module](module.md)。
