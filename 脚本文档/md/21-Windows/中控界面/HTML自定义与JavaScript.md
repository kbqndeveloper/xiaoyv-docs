---
params: ""
returns: ""
---

# HTML 中控自定义与 JavaScript 接口

`center.html` 是完整中控页面，位置按[开发与打包](../开发与打包.md)的启动来源规则确定。整个文件都可以修改，没有必须保留的 DOM、CSS 或按钮。中控只向本地顶层页面自动注入只读的 `center` 和 `ui`；不要直接调用 `window.chrome.webview` 或依赖内部消息格式。

没有 `center.html` 时使用原生中控；只有 `center.default.html` 不会进入 HTML 模式。HTML 存在但加载失败时不会自动回退。原生模式下有效的 Lua `ui.call` 返回 `nil, nil`，详见 [中控交互](../中控交互.md)。

保存页面后按 `Ctrl+R` 刷新。刷新会清空当前页面的事件监听、`ui.expose` 注册和未完成请求，但不会停止 Worker；正在等待页面结果的 Lua `ui.call` 会失败。关闭整个中控窗口仍会结束全部外置 Worker，并等待进程内脚本停止及清理。页面渲染进程异常时不会自动重连或恢复，请按提示刷新页面。

## center

除 `center.on` 外，以下方法都返回 `Promise`；失败时 Promise 以 `Error` 拒绝。可以先等待 `center.ready`，也可以直接调用中控请求类方法，它们会在桥就绪后执行。剪贴板方法是浏览器 API 的直接封装，不等待中控桥就绪。

| 接口 | 参数 | 返回值与说明 |
|---|---|---|
| `center.ready` | 无 | `Promise<void>`；页面桥完成初始化。 |
| `center.capabilities()` | 无 | `Promise<{inProcess: boolean}>`；是否支持进程内 Worker。 |
| `center.on(name, listener)` | 事件名、回调 | 取消订阅函数。公开事件见下表。 |
| `center.workers()` | 无 | `Promise<Worker[]>`；返回全部 Worker 快照。 |
| `center.get(workerId)` | Worker ID | `Promise<Worker\|null>`；按 ID 查询当前快照。 |
| `center.add(options?)` | `{path?, name?, arch?}` | `Promise<string>`；增加窗口但不启动脚本，返回 `workerId`。`arch` 可选 `"x86"`、`"x64"` 或 `"inProcess"`，默认外置 x86；进程内模式使用中控自身位数。省略 `path` 使用中控默认脚本。 |
| `center.remove(workerId)` | Worker ID | `Promise<object>`；外置窗口结束进程并删除；进程内窗口须先停止并完成清理。 |
| `center.start(workerId)` | Worker ID | `Promise<object>`；请求启动。 |
| `center.pause(workerId)` | Worker ID | `Promise<object>`；请求暂停运行中的 Worker。 |
| `center.resume(workerId)` | Worker ID | `Promise<object>`；请求继续已暂停的 Worker。 |
| `center.stop(workerId)` | Worker ID | `Promise<object>`；请求脚本正常停止。 |
| `center.terminate(workerId)` | Worker ID | `Promise<object>`；强制结束外置进程，保留列表项和日志；进程内窗口拒绝此操作。 |
| `center.logs(workerId, options?)` | `{after?, limit?}` | `Promise<{first,next,lines}>`；按序号拉取日志，默认最多 2000 行。 |
| `center.clearLogs(workerId)` | Worker ID | `Promise<object>`；清空该 Worker 的中控日志。 |
| `center.pickFile()` | 无 | `Promise<string\|null>`；打开系统文件选择器，取消时返回 `null`。 |
| `center.pickDirectory()` | 无 | `Promise<string\|null>`；打开系统目录选择器，取消时返回 `null`。 |
| `center.readFile(path)` | 文件路径 | `Promise<string>`；返回原始字节的 Base64 字符串。 |
| `center.writeFile(path, base64)` | 文件路径、Base64 字符串 | `Promise<boolean>`；写入成功返回 `true`。 |
| `center.copyText(text)` | 任意可转字符串值 | `Promise<void>`；写入系统剪贴板。 |
| `center.readClipboard()` | 无 | `Promise<string>`；读取系统剪贴板文本。 |
| `center.reload()` | 无 | `Promise<object>`；刷新中控页面，不停止 Worker。 |

文件按原始字节读写，不判断文件类型或编码；Base64 由页面负责转换。相对路径以中控 EXE 所在目录为基准；写入会覆盖目标文件，不会自动创建父目录。失败时 Promise 拒绝。复制任意格式文件可直接传递读取结果：

```javascript
await center.writeFile("copy.png", await center.readFile("image.png"));
```

`Worker` 常用字段如下：

| 字段 | 类型 | 说明 |
|---|---|---|
| `workerId` | `string` | 列表项的稳定 ID；进程重建后不变，不是 PID。 |
| `workerPid` | `number` 或未提供 | 当前 Worker 进程 ID；连接中、已释放或断联时可能没有。 |
| `workerArch` | `string` | `"x86"` 或 `"x64"`，创建时确定，重新启动不改变。 |
| `workerInProcess` | `boolean` | 是否为进程内窗口；为 `true` 时 PID 与中控相同。 |
| `name` | `string` | 显示名称。 |
| `path` | `string` | `.lua` 或 `.alpkg` 路径。 |
| `state` | `string` | 当前真实状态。 |
| `text` | `string` | 当前常驻状态文本。 |
| `pending` | `boolean` | 是否仍有一条运行控制命令等待 Worker 响应。 |

`start`、`pause`、`resume`、`stop` 等控制 Promise 完成只表示中控已接纳命令。页面应在 `workers.changed` 后重新读取 `center.workers()`，以 Worker 上报的真实 `state` 为准。

`center.logs` 返回的 `lines` 元素为 `{sequence, text}`，`first` 是本次可显示内容的首序号，`next` 可作为下次 `after`。中控每个 Worker 最多保留 2000 行，另有总字符数上限；单次拉取还受响应大小限制，因此页面要能丢弃早于 `first` 的旧 DOM。通知会合并，不会逐条推送日志正文。

| 事件名 | 回调参数 | 用法 |
|---|---|---|
| `workers.changed` | 消息对象 | Worker 列表、状态、常驻文本或 pending 发生变化；重新调用 `workers()`。 |
| `logs.changed` | 含 `workerId` 的消息对象 | 指定 Worker 有日志变化；重新调用 `logs()`。 |
| `error` | 含 `error` 的消息对象 | 显示中控或 Worker 控制错误。 |

## ui

| 接口 | 参数 | 返回值与说明 |
|---|---|---|
| `ui.expose(name, callback)` | 函数名、回调 | 注册供 Lua `ui.call` 调用的函数，并返回取消注册函数。回调参数为 `(data, {workerId})`，可返回值或 Promise。 |
| `ui.send(workerId, name, data)` | Worker ID、事件名、JSON 数据 | `Promise<object>`；向运行中或已暂停的 Worker 投递事件。 |

重复 `expose` 同名函数会替换旧回调。回调结果必须是 JSON 数据；返回 `undefined` 等同返回 `null`。`ui.send` 成功表示 Worker 邮箱已经确认接收，不表示 Lua 已调用 `ui.receive` 或业务处理已经完成。等待确认超时表示送达状态未知，不应盲目重复有副作用的事件。

`ui.expose` 参数无效或 `ui.send` 的数据不能转换为 JSON 时，调用可能在返回 Promise 前同步抛出异常；其余异步失败通过 Promise 拒绝报告。

## HTML 与 Lua 配套示例

默认 `center.default.html` 已提供完整运行按钮、x86/x64/进程内选择和“自定义示例”面板，可直接修改。下面是可放入自定义 `center.html` 的 `<script>` 内运行的最小示例，`arch` 只决定新增窗口：

```javascript
(async () => {
  await center.ready;
  document.body.innerHTML = `
    <select id="arch"><option>x86</option><option>x64</option><option value="inProcess">进程内</option></select>
    <button id="add">增加 Worker</button>
    <select id="workers"></select>
    <button id="start">启动选中项</button>
    <input id="account" value="demo" aria-label="账号">
    <button id="send">发送设置</button>
    <p id="status"></p>
    <pre id="output"></pre>`;

  let selected = null;
  const output = document.querySelector("#output");
  const list = document.querySelector("#workers");
  async function refresh() {
    const workers = await center.workers();
    selected = workers.some(item => item.workerId === selected)
      ? selected : workers[0]?.workerId ?? null;
    list.replaceChildren(...workers.map(item => new Option(
      `${item.name} (${item.workerArch})`, item.workerId, false, item.workerId === selected)));
    output.textContent = JSON.stringify(workers, null, 2);
  }

  center.on("workers.changed", () => refresh().catch(console.error));
  center.on("error", message => console.error(message.error));
  list.onchange = () => { selected = list.value; };
  document.querySelector("#add").onclick = async () => {
    selected = await center.add({name: "采集窗口", arch: document.querySelector("#arch").value});
    await refresh();
  };
  document.querySelector("#start").onclick = async () => {
    if (selected) await center.start(selected);
  };
  document.querySelector("#send").onclick = async () => {
    if (selected) await ui.send(selected, "settings", getSettings());
  };
  function getSettings() { return {account: document.querySelector("#account").value}; }
  ui.expose("getSettings", getSettings);
  ui.expose("setStatus", (text, {workerId}) => {
    document.querySelector("#status").textContent = `${workerId}：${text}`;
    return true;
  });
  await refresh();
})().catch(console.error);
```

对应的 `dev.lua`：

```lua
local settings, err = ui.call("getSettings")
assert(not err, err)
settings = settings or {account = ""} -- 原生模式下页面调用正常返回空值
print("初始账号：", settings.account)
ui.call("setStatus", "等待设置")

while true do
    local name, data = ui.receive(1000)
    if name == "settings" then
        settings = data
        print("新账号：", settings.account)
        ui.call("setStatus", "设置已更新")
    elseif name == nil and data ~= nil then
        error(data)
    end
end
```

先准备页面和 `dev.lua`，再启动中控与脚本；脚本运行后修改账号并点“发送”。该最小页面的账号输入由全部窗口共用，默认出厂模板则演示了按 `workerId` 保存各自的会话设置，并可通过文件接口保存或读取配置；`sessionStorage` 本身不是配置文件。若要让一个 x64 窗口提供业务服务、多个 x86 窗口调用它，见 [RPC 示例](../RPC/跨位数示例.md)，不要通过页面转发大块业务数据。

桥只允许本地顶层中控页面使用，跳转外部网站或 iframe 不能继承权限，新窗口请求也不会打开。它不新增 TCP/HTTP 监听端口，也不提供独立弹窗 API。需要系统路径选择器时使用 `pickFile`/`pickDirectory`；文件接口直接由中控读写，不经过 Worker。

JavaScript 与 Lua 的 JSON 类型、64 KiB 大小和 `ui.null` 规则见 [中控交互](../中控交互.md#json-数据规则)。

要在中控启动时自动执行 Lua 逻辑，见[中控脚本](中控脚本.md)。页面可使用 `ui.send("__center__", name, data)` 向该脚本投递事件；刷新页面不会重新运行它。
