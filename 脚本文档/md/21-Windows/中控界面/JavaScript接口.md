---
params: ""
returns: ""
---

# JavaScript 接口

中控页面可直接使用 `center` 和 `ui`。下面标注为 Promise 的结果使用 `await` 获取，失败时抛出错误。

## 窗口控制

| 方法 / 属性 | 参数 | 返回值 |
|---|---|---|
| `center.ready` | 无 | `Promise<void>`；等待页面接口就绪。 |
| `center.capabilities()` | 无 | `Promise<{inProcess: boolean}>`；是否支持进程内窗口。 |
| `center.workers()` | 无 | `Promise<Worker[]>`；所有窗口。 |
| `center.get(workerId)` | 窗口 ID | `Promise<Worker\|null>`；指定窗口。 |
| `center.add(options?)` | `{path?, name?, arch?}` | `Promise<string>`；新窗口 ID，创建后需调用 `start`。 |
| `center.start(workerId)` | 窗口 ID | `Promise<object>`；启动。 |
| `center.pause(workerId)` | 窗口 ID | `Promise<object>`；暂停。 |
| `center.resume(workerId)` | 窗口 ID | `Promise<object>`；继续。 |
| `center.stop(workerId)` | 窗口 ID | `Promise<object>`；停止。 |
| `center.terminate(workerId)` | 窗口 ID | `Promise<object>`；强制结束，保留列表和日志；进程内窗口不支持。 |
| `center.remove(workerId)` | 窗口 ID | `Promise<object>`；结束并删除窗口，进程内窗口须先停止。 |

`add` 的 `path` 为脚本路径，省略时使用默认脚本；`arch` 可选 `"x86"`（默认）、`"x64"`、`"inProcess"`（与中控位数相同）。

`Worker` 字段：`workerId`（窗口 ID，非 PID）、`workerPid`（进程 ID，可能缺省）、`workerArch`（x86/x64）、`workerInProcess`（是否进程内）、`name`、`path`、`state`（状态）、`text`（状态文字）、`pending`（是否正在等待控制结果）。

启动、暂停等方法返回表示请求已接受；实际状态通过 `center.workers()` 查询。

## 日志与事件

| 方法 | 参数 | 返回值 |
|---|---|---|
| `center.logs(workerId, options?)` | 窗口 ID、`{after?, limit?}` | `Promise<{first,next,lines}>`；默认最多 2000 行。 |
| `center.clearLogs(workerId)` | 窗口 ID | `Promise<object>`；清空日志。 |
| `center.on(name, listener)` | 事件名、回调 | 取消订阅函数。 |

`lines` 的每项为 `{sequence, text}`。将 `next` 作为下次查询的 `after`；页面应移除序号小于 `first` 的旧日志。

| 事件 | 回调数据 | 用法 |
|---|---|---|
| `workers.changed` | 消息对象 | 重新调用 `center.workers()` 更新列表。 |
| `logs.changed` | 含 `workerId` 的对象 | 调用 `center.logs(workerId)` 更新日志。 |
| `error` | 含 `error` 的对象 | 显示错误文本。 |

## 与 Lua 交互

| 方法 | 参数 | 返回值 |
|---|---|---|
| `ui.expose(name, callback)` | 函数名、回调 `(data, {workerId})` | 取消注册函数；回调可返回值或 Promise。 |
| `ui.send(workerId, name, data)` | 窗口 ID、事件名、数据 | `Promise<object>`；向运行中或暂停的脚本发送事件。 |

Lua 使用 [ui.call](call.md) 调用已注册函数，使用 [ui.receive](receive.md) 接收事件。向中控脚本发送时，窗口 ID 用 `"__center__"`。重复注册同名函数会替换旧回调。

参数无效可能直接抛出错误；调用时可用 try/catch 处理。传参和回调结果遵循[交互数据](交互数据.md)规则。`ui.send` 成功表示事件已送达，脚本处理结果需另行反馈；超时后不要直接重复提交写入等操作。

## 文件与页面

| 方法 | 参数 | 返回值 |
|---|---|---|
| `center.pickFile()` | 无 | `Promise<string\|null>`；选择文件，取消返回 `null`。 |
| `center.pickDirectory()` | 无 | `Promise<string\|null>`；选择目录，取消返回 `null`。 |
| `center.readFile(path)` | 文件路径 | `Promise<string>`；文件字节的 Base64。 |
| `center.writeFile(path, base64)` | 文件路径、Base64 数据 | `Promise<boolean>`；成功返回 `true`。 |
| `center.copyText(text)` | 文本 | `Promise<void>`；复制到剪贴板。 |
| `center.readClipboard()` | 无 | `Promise<string>`；读取剪贴板。 |
| `center.reload()` | 无 | `Promise<object>`；刷新页面。 |

文件的相对路径以中控 EXE 所在目录为准。写入会覆盖文件，父目录需预先存在；文本编码与 Base64 转换由页面处理。

刷新页面不会停止脚本；页面的事件监听和 `ui.expose` 回调需重新注册，未完成的页面调用会失败。
