# 更新日志

Codex 改了安装方式、登录流程、报错或额度规则，这里记一笔。日期为北京时间。

## 2026-10-11

- 新增「10. 接入 DeepSeek 和第三方 API」：Codex 现在只认 Responses 协议（`wire_api = "chat"` 已删）；DeepSeek 的两种接法，一种照官方文档设成默认，一种用 profile 文件按需切换、不动原来的 ChatGPT 登录；CC Switch 和中转站各是什么；13 条接第三方时的报错。提醒一处容易踩的地方：DeepSeek 官方配置里的 `forced_login_method = "api"` 会让 Codex 退掉当前的 ChatGPT 登录。Codex 一侧的行为在本机隔离目录里实测过，并对照了 0.162.1 的源码。
- 新增「更新、降级、卸载」：`codex update` 按安装方式实际执行的命令、各种装法怎么卸载、装了两份 codex 导致版本不变怎么查、想卸干净要删哪些目录。
- 新增「无法加载工作空间设置（Couldn't load workspace settings）」：10 月 7 日起桌面版新对话发不出去，Windows 和 macOS 都有，最新版仍未修。按日志分三种情况（`cloudflare_challenge`、`devicecheck_token_generation_failed`、10 月 7 日已修的那一版），列了 6 个用户确认过的办法。
- 「模型满载」补 10 月 9–10 日这一波：每条消息都报 `Selected model is at capacity`，会话日志里是 `server_overloaded`，状态页没有事故记录；附怎么在日志里确认，以及用户试出来的几条办法。同时更正一处说法：别人的账号正常、自己的不行，不一定是账号被限，也可能是被分到了不同机房。
- 「Windows 沙盒 (os error 32)」：更多用户确认桌面版 26.1007.21434 已修，并说明它和 Microsoft Store 包版本 26.1007.2314.0 是同一版；CLI 0.161.0–0.162.1 仍未修，补两个不用降级的办法（改成 `unelevated` 沙盒；只用 CLI 的注释掉 `[mcp_servers.node_repl]`）。
- 「WSL (os error 2)」：10 月 11 日的 26.1007.21434 仍有；补上用户查到的原因，以及重启后不失效的 `/etc/fstab` 写法和必须重启这一步。
- 「后台服务报错」补 `failed to load workspace requirements (code -32603)`。
- 「最近两周的已知问题」加 2 行，24 个 issue 的状态逐个复核到 10 月 11 日；GitHub 代码审查那一行补充：打开 use credits 也不一定管用。
- 页首加「这几天问得最多的」四条直达链接。断线重连一节补上中文界面的实际写法「正在重新连接 1/5」。
- 中英界面对照补 5 条报错提示，共 115 条。

## 2026-10-10

- 核对版本更新为 Codex CLI 0.162.1（npm 与 npmmirror 一致）、Windows 桌面版 26.1007。
- 新增「Windows 沙盒：setup refresh had errors (os error 32)」：10 月 7 日 Windows 桌面版 26.1002 起所有命令失败，26.1007.21434 已修；单独安装的 CLI 0.162.x 还没修，可先装回 0.160.1。
- 新增「WSL：Failed to create unified exec process (os error 2)」：Windows 桌面版开了 Run agent in WSL 之后每条命令都失败，附用户给出的挂载目录临时办法。
- 新增「网页版：Unable to determine project root for task」：云端任务第一条消息发不出去，未修。
- 「后台服务报错」补两条：`Server is draining; retry after reconnecting`、`Background server has incompatible feature settings`。
- 「最近两周的已知问题」加 9 条：除上面几条外，还有 VS Code 语音输入 403、Linux 沙盒 `mountinfo path is not absolute`、10 月 2 日重置没到账、套餐额度用完后输入框变灰、dot 建不了本地任务。状态核对到 10 月 10 日；VS Code 插件丢消息那条改为已修。
- 「国内怎么开通」补充 Pro 10×（官方 Pro 200）。
- 「没修」的已知问题逐条回到原 issue 复核：全部仍未关闭；VS Code 语音输入 403 补上用户确认的办法（`"http.electronFetch": true`）；安卓 Codex Remote 改为「没确认修好」。
- 从 LINUX DO 补 4 条：`failed to open daemon process (os error 5)` 的原因和处理；只有某个旧会话一直重连（历史里图片太多，`os error 10054`）；Windows 桌面版闪退的简单修法；重新订阅不刷新周额度。另加 macOS 的 `untrusted-process-ancestry`。
- 从 OpenAI 开发者社区补 5 条：Windows 桌面版登录后几十秒自己退出（新增小节，附排查位置）；CLI `workspace routing discovery failed`（重启后台服务）；VS Code 日志里的 `Failed to release queued message send lock`；重置卡到期时间的官方答复；GitHub 代码审查 `usage limits for code reviews`。

## 2026-10-03

- 核对版本更新为 Codex CLI 0.160.0（npm 与 npmmirror 一致）、ChatGPT 桌面版 26.930。
- 新增「最近两周的已知问题」：9 月 24 日以来 openai/codex 里反馈最多的 11 个问题，逐条写明出在哪个版本、修了没有、现在怎么办。
- 新增「后台服务（daemon）报错」：CLI 0.157.0 起默认启动后台服务，收录 `start the Windows daemon from a non-elevated terminal`、`host Job Object prevents daemon detachment`、Windows 终端窗口闪烁，以及 `--no-daemon`、`/daemon`、停用后台服务的命令。
- 新增「401 Unauthorized」：区分官方故障（报错带 `Incorrect API key provided: sk-svcac…`，北京时间 9 月 26 日 06:58–07:54 出过一次）和本机登录失效，两种处理办法相反。
- 新增「终端里复制不了文字」：CLI 0.157.0 起默认全屏界面，Mac 自带终端里 Cmd + C 失效的三个办法。
- 「国内网络」补充：改了代理不生效时，先停掉后台服务。
- 「桌面版打不开」补充 26.924 的问题：Linux 卡在 `Starting your task`（已修）、Windows `Unable to load organization settings`（未修）。
- 「模型不支持」更新：新增 `gpt-6.1-sol`（9 月 29 日发布）；GPT-5.5 10 月 14 日下线后的官方替换改为 `gpt-6-sol`（Free 和 Go 是 `gpt-6-luna`）。
- 「额度用完了」更新：Pro 分为 Pro 100 / 200 / 500 三档（Plus 的 5、10、25 倍）；加上 Plus 每 5 小时各模型的官方估算条数；重置时间线改链到单独的重置时间表。
- 中英界面对照按桌面版 26.930 的语言包逐条复核：103 条里 100 条没变；Steer 从「调整方向」改成「引导」，Queue 从「加入队列」改成「排队」，Work locally 从「在本地工作」改成「在本地处理」；补 7 条 Windows 设置和启动任务的提示，共 110 条。
- 「Windows 安装未完成」一节改名为「Windows 设置未完成」，按 26.930 的界面文字更新：现在显示「Windows 设置未完成」，按钮是「重新设置」和「在没有管理员访问权限的情况下继续」；更正了后一个按钮的作用（走的是不需要管理员权限的沙盒，不是只能聊天）。
- 额度用完的中文提示按 26.930 更新（「…或在 … 后重试」，以及带「充值额度」的两种写法）。
- 手机号验证：逐篇核对帮助中心的五篇文章，规则没有变化；补上能用 WhatsApp 收验证码的 12 个国家、澳门也不在支持名单里，以及一条旧号码不用了之后进不了 Codex 的社区案例。
- 修正：Mac 上查桌面版自带 Codex 版本的路径（现在在 `ChatGPT.app` 里）；429 一节里「第 6 节」应为「第 7 节」。

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
