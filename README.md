# 小鱼精灵公开文档

宣传首页按长期产品范围介绍 JavaScript、Lua、Go 与 PC、Android、iOS。当前 Android 和 Windows
已接入 Lua，JavaScript、Go 与 iOS 尚未接入。帮助按 Android、Windows 和工具分类，包含当前可用
API、工具用法，也可以包含 C ABI、扩展开发资料和
明确标注状态的未完成或实验能力；只有标明当前可用的脚本接口才可直接依赖，具体可用性以
函数页和 GitHub Releases 为准。

## 阅读入口

- [小鱼精灵官网](index.html)：IDE 工具、JavaScript/Lua/Go 与 PC/Android/iOS 产品介绍。
- [交互式脚本文档](脚本文档.html)：目录、搜索、参数摘要和函数正文集中在一个页面中。
- [Markdown 脚本文档](脚本文档.md)：适合在 Git 仓库中阅读和链接。

脚本 API 按对应平台的概述和功能分类查找；远控连接与部署说明在“工具 → 小鱼远控”中。

## 发布说明

本目录是文档发布白名单。部署时以 `docs/public/` 为站点根目录，并保留以下相对结构：

```text
index.html
assets/
脚本文档.html
脚本文档/
  catalog.json
  md/
```

宣传首页是独立的纯静态 HTML；脚本文档会按需读取目录 JSON 和 Markdown 正文，因此发布时
仍不能只复制 HTML 文件。内部架构、源码说明、计划和历史记录不属于公开站点。

官网品牌主图保存在 `assets/xiaoyv-brand-master.png`。网页展示图、社交分享图和图标都放在
同一目录；调整展示尺寸时应保留主图，不要反复压缩后覆盖源文件。

## 反馈问题

反馈脚本 API 问题时，请同时提供：

- 使用的函数名和最小复现脚本。
- 实际参数、返回值或完整错误信息。
- 使用的平台与系统版本、普通脚本或 `.alpkg` 运行方式；Android 另提供是否启用 Root。
- 预期结果和实际结果。
