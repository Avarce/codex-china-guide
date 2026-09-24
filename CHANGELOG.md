# 更新日志

Codex 改了安装方式、登录流程、报错或额度规则，这里记一笔。日期为北京时间。

## 2026-09-24（晚）

- 新增「5. 桌面版打不开」：Windows 安装未完成（helper_failed）按官方 Windows sandbox 文档写处理顺序，附备用沙盒配置和社区里 19 人认可的 `.sandbox-bin` 办法；更新后打不开、只有启动动画、白屏，列出 26.820 / 26.609 / 26.915 的已知问题；「已在另一个应用中打开」的含义。
- 新增「8. 怎么设置中文」：桌面版在设置 → 常规 → 语言里选「中文（中国）」；这一项受 OpenAI 后台开关控制；VS Code 插件用 `chatgpt.localeOverride`；CLI 没有中文界面；不建议装第三方汉化包的原因。
- 章节重新编号：原 5–8 节顺延为 6–9 节。

## 2026-09-24

- 首次发布。
- 收录 12 类常见报错，每条附原因、处理顺序和出处：断线重连、Token exchange failed、refresh token 失效、地区不支持、Selected model is at capacity、模型不支持、429、上下文满了、证书错误、额度用完，以及手机号验证、登录回调被挡。
- 收录国内终端代理设置（macOS / Linux / Windows）和 `codex doctor` 自查方法。核对版本：Codex CLI 0.156.1（npm 与 npmmirror 一致），ChatGPT 桌面版 26.915。
- 收录中文界面里对应的报错写法，以及 100 多条中英界面对照（[UI-GLOSSARY.md](UI-GLOSSARY.md)）。
- 收录 AGENTS.md 中文模板（[templates/AGENTS.md](templates/AGENTS.md)）。
- 记录 GPT-5.5 于 2026 年 10 月 14 日从 ChatGPT 和 Codex 下线，写死 `gpt-5.5` 的设置和脚本需要改。
