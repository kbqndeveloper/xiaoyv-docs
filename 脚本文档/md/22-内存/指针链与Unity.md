---
params: "见下方方法表"
returns: "见下方方法表"
---

# 指针链与 Unity 字符串

按偏移链定位字段，或访问 Unity 字符串对象。

**方法名称：** 指针链与 Unity 字符串（Android）。

**语法：** 见下方方法表。

**参数说明：** `chain` 是偏移文本，separator 默认 +；Unity 的 address 是字符串对象地址，instance 是宿主实例地址，offset 是字段的字节偏移。

| 方法 | 参数 | 返回值 | 说明 |
|---|---|---|---|
| `mem:fuckChain(address: integer, chain: string, separator?: string)` | address：链起点；chain：偏移文本；separator：分隔符，默认 + | address: integer 或 nil, error: string | 逐级解引用，默认分隔符为 + |
| `mem:readUnityString(address: integer)` | address：Unity 字符串对象地址 | text: string\|nil | 从 Unity 字符串对象读取 UTF-8 |
| `mem:readUnityStringFromInstance(instance: integer, offset?: integer)` | instance：实例地址；offset：字段偏移，默认 16；32 位修正规则见下文 | text: string\|nil | 读取实例字段中的字符串对象指针，默认偏移 16 |
| `mem:writeUnityString(address: integer, value: string)` | address：Unity 字符串对象地址；value：UTF-8 文本 | ok: boolean | 在现有字符串容量内写入 UTF-8 内容 |
| `mem:writeUnityStringFromInstance(instance: integer, offset: integer, value: string)` | instance：实例地址；offset：真实字段偏移，必填；value：UTF-8 文本 | ok: boolean | 使用明确的实例字段偏移写入 |

| 返回值 | 说明 |
|---|---|
| address | 指针链最终地址；中途读取失败返回 nil, error |
| text | UTF-8 字符串；读取失败或对象 length 小于 1 时返回 nil |
| ok | Unity 写入结果，不表示写入内容一定未被截短 |

**详细说明：**

## 示例

```lua
-- base 和偏移由目标对象布局确定。
local address, err = mem:fuckChain(base, "0x10+0x20+0x30")
assert(address, err)
local value, readError = mem:read_memory(address, mem.i32)
assert(value ~= nil, readError)
print(value)

-- fieldOffset 是实例中保存字符串对象指针的字段偏移。
local text = mem:readUnityStringFromInstance(instance, fieldOffset)
assert(text ~= nil, "字符串为空或读取失败")
print(text)
assert(mem:writeUnityStringFromInstance(instance, fieldOffset, "新名称"))
```

## 使用说明

- 指针链除最后一项外，均先加偏移再解引用；最后只加偏移。指针宽度取 `mem.arch`。分隔符按普通文本匹配且不能为空；空片段跳过，非数值片段按 0 处理。
- Unity 方法接收字符串对象地址，不是普通 UTF-16 字节区。64 位对象 length/字符区偏移为 16/20，32 位为 8/12。
- FromInstance 先读字段中的字符串指针。读取默认 offset=16，32 位目标会将 16 改为 8；写入无此修正，必须传真实偏移。
- 写入不扩容，超过当前 length 会截短；缩短会更新 length，但不清空旧尾部、不补终止符。长度按 UTF-16 码元计算，非 BMP 字符占两个码元。
