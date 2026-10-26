# Codex 国内使用与报错速查（2026）：安装、登录、手机号、代理、额度

> **最后核对：2026 年 10 月 27 日**（北京时间）· Codex CLI 0.164.0 · 桌面版 26.1021 · VS Code 插件 26.1021
> 遇到报错，直接 Ctrl + F 搜报错里的几个词。终端、英文界面、中文界面三种写法都收了。
> 觉得有用就点右上角 ⭐ Star，下次报错不用再到处搜。

![Codex 国内使用与报错速查：按报错原文查原因和处理步骤](images/cover.webp)

国内用 Codex，卡住的地方基本就那几处：装不上、打不开、登录跳不回来、要验证手机号、终端连不上、用着用着断线、额度用完。这页按「你看到的报错」来排，每条写清楚多半是什么原因、按什么顺序处理，并附上官方文档或 openai/codex 仓库里维护者回复的出处，可以自己点进去核对。想接 DeepSeek 这类第三方 API 的，直接看[第 10 节](#10-接入-deepseek-和第三方-api)。

**各套餐能用的 Codex 不一样。** Free 和 Go 额度少，官方写的是在桌面版里用 GPT-5.6 Terra、GPT-6 Luna；GPT-6.1 Sol、GPT-6.1 Terra、Astra 和网页版的云端任务要 Plus 或 Pro。没有海外卡，可以微信付款，由 [AONIR](https://aonir.com/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=brand_home) 充值到你自己的 ChatGPT 账号：[Plus ¥168/月](https://aonir.com/chatgpt-plus/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=top_plus) · [Pro 100（5×）¥730/月](https://aonir.com/chatgpt-pro/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=top_pro_5x) · [Pro 200（10×）¥1400/月](https://aonir.com/chatgpt-pro-10x/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=top_pro_10x) · [Pro 500 ¥3500/月](https://aonir.com/chatgpt-pro-500/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=top_pro_500)（客服开通）。想自己用礼品卡订阅，或者先比较几种办法，见文末[国内怎么开通](#国内怎么开通)。

**这几天问得最多的（10 月 26 日）：**

- VS Code 里的 Codex 插件 10 月 24 日自动更新到 26.1021.22027 之后打不开，侧边栏只有一句 `Codex could not start`（`The extension could not start its user interface.`）：十几位用户确认退回 26.1007.21434 就好（也有一两位没用），退回后记得关掉这个插件的自动更新：[看这里](#vs-code-插件打不开codex-could-not-start)
- 输入框上方出现「此聊天已被标记为可能存在网络安全风险」（`This chat was flagged for possible cybersecurity risk`）：网络安全方面的安全检查拦下了这次请求，这一条本身不是封号。换个说法再发，觉得是误判就用 `/feedback` 反馈：[看这里](#被标记为网络安全风险flagged-for-possible-cybersecurity-risk)
- 周额度还剩八九成，却一直报 `exceeded retry limit, last status: 429 Too Many Requests`：10 月 25 日起的新一波，多数用的是 GPT-6.1 Sol，有人抓到的返回原文是 `Codex is at capacity, please try again later`，和前几天的「模型满载」是一类问题，没有官方回复：[看这里](#请求太多429-too-many-requests)
- Windows 桌面版更新到 26.1021 后一打开就是「无法加载组织设置」，点重试、退出登录都回到这一页：三位用户查到是电脑里留着一个旧的 `CODEX_CLI_PATH` 环境变量，删掉就能打开：[看这里](#更新后打不开只有启动动画或白屏)
- 重置：Tibo 10 月 25 日、26 日又各发了一次全体重置。OpenAI 帮助中心写明：用重置卡、全体重置、付费重置都可能改掉你下一次额度刷新的时间；点「使用重置」至少要有一项剩余不到 80%：[看这里](#重置机会用不了你当前的用量暂时无需重置)

## 目录

- [按报错找](#按报错找)
- [最近两周的已知问题](#最近两周的已知问题)
- [1. 安装](#1-安装)
- [2. 登录](#2-登录)
- [3. 要验证手机号](#3-要验证手机号)
- [4. 国内网络：终端要单独设代理](#4-国内网络终端要单独设代理)
- [5. 桌面版打不开](#5-桌面版打不开)
- [6. 报错逐条处理](#6-报错逐条处理)
- [7. 额度用完了](#7-额度用完了)
- [8. 怎么设置中文](#8-怎么设置中文)
- [9. AGENTS.md 中文模板](#9-agentsmd-中文模板)
- [10. 接入 DeepSeek 和第三方 API](#10-接入-deepseek-和第三方-api)
- [常见问题](#常见问题)
- [国内怎么开通](#国内怎么开通)
- [官方来源](#官方来源) · [更新日志](CHANGELOG.md) · [投稿一条报错](#投稿一条报错)

## 按报错找

| 你看到的 | 多半是 | 看这里 |
| --- | --- | --- |
| `stream disconnected before completion`、`Reconnecting... 1/5`、正在重新连接 1/5（重连 5 次后失败）、一直重新连接、服务器繁忙，正在重新连接；`Reconnecting... waiting for network`，网页能打开、Codex 一直重连 | 看冒号后面那半句：网络或代理，或者 OpenAI 服务器出错 | [断线重连](#断线重连stream-disconnected-before-completion) |
| `stream disconnected before completion: An error occurred while processing your request`，后面有 `Please include the request ID`；压缩时是 `Error running remote compact task` | OpenAI 服务器返回的错误，不是你的网络。10 月 9 日起变多，多见于图片多的长会话 | [服务器处理失败](#服务器处理失败an-error-occurred-while-processing-your-request) |
| `error sending request for url (https://chatgpt.com/backend-api/codex/...)` | 终端没走代理 | [断线重连](#断线重连stream-disconnected-before-completion) |
| 一直「正在思考」（Thinking）、「已处理 …」（Working for …）不结束；CLI 停在 `Working (… • esc to interrupt)` | 在等你批准、某条命令没退出、任务本身慢，或者连接断了 | [一直正在思考](#一直正在思考卡住不动thinking) |
| 浏览器显示登录成功，终端一直在等 | 本机回调被挡 | [2. 登录](#2-登录) |
| 登录完浏览器跳到 `localhost`（新版是 `127.0.0.1`）：该网页无法正常运作、localhost 未发送任何数据、`ERR_EMPTY_RESPONSE`；或 localhost 拒绝了我们的连接请求、`ERR_CONNECTION_REFUSED` | Codex 不在这台电脑上（VS Code、Cursor 连着 SSH、WSL），或 1455 端口上是别的程序 | [登录后跳到 localhost](#登录后跳到-localhost-打不开err_empty_response) |
| 设备码登录时 Cursor 弹出 `Do you want Cursor to open the external website?`、`Configure Trusted Domains`（是否要 Cursor 打开外部网站） | 编辑器的链接保护，不是报错 | [登录后跳到 localhost](#登录后跳到-localhost-打不开err_empty_response) |
| Windows 上 `failed to start login server`，后面是 `os error 10013`（以一种访问权限不允许的方式做了一个访问套接字的尝试），或者 1457 端口被占、却查不到是谁 | Windows 把这段端口留给了 Hyper-V、WSL 2 或 Docker | [登录后跳到 localhost](#登录后跳到-localhost-打不开err_empty_response) |
| `Token exchange failed`、桌面版「登录失败：Token exchange failed …」、令牌交换失败；浏览器里一页 `Sign-in could not be completed` 或 `State mismatch` | 登录时网络不通；`State mismatch` 是用了一次旧的登录页 | [登录失败](#登录失败token-exchange-failed) |
| 登录、新建任务、开对话时 `timed out`、`timeout`、`Operation timed out`、`Request timed out`、请求超时；`workspace routing discovery timed out` | 请求没走代理，等到超时 | [4. 国内网络](#4-国内网络终端要单独设代理) |
| `refresh token was already used`、此设备上的 ChatGPT 会话已过期 | 登录凭证失效 | [登录过期](#登录过期refresh-token-was-already-used) |
| `Verify your phone number`、`Phone number required` | 手机号验证 | [3. 要验证手机号](#3-要验证手机号) |
| `unsupported_country_region_territory`、我们的服务在你所在的国家或地区不可用 | 节点所在地区 | [地区不支持](#地区不支持unsupported_country_region_territory) |
| `Selected model is at capacity`，会话日志里是 `server_overloaded`，有时带 `503`；所有模型都报、只有自己的账号报（有人叫它「被限流」「被风控」「进小黑屋」） | 服务器满载（10 月 9–10 日、12–14 日各有一波，15 日官方加了算力，19–23 日又一波，24 日起少了），或账号被临时限制 | [模型满载](#模型满载selected-model-is-at-capacity) |
| `The '...' model is not supported when using Codex with a ChatGPT account`（`status: 400`） | 模型名写错、旧设置、还没开放；刚升级套餐的，登录凭证里还是旧套餐 | [模型不支持](#模型不支持model-is-not-supported) |
| 只写着状态码：`400 Bad Request`、`402 Payment Required`、`403 Forbidden`、`404`、`500`、`502`、`503` | 每个码对应的情况不一样 | [按状态码查](#按状态码查400403404429500502503) |
| `approval_policy = "untrusted" is no longer supported; remove this setting`，或更新后一打开就是 `Unable to load organization settings` | `config.toml` 里还留着已经废弃的设置 | [更新后打不开](#更新后打不开只有启动动画或白屏) |
| 周额度几十秒内从七八成掉到一成甚至 0；额度掉得太快、额度被砍 | 官方 10 月 21 日认了：约 4 万个账号的额度上限和套餐对不上，已修正，补 credits | [7. 额度用完了](#7-额度用完了) |
| `exceeded retry limit, last status: 429 Too Many Requests`，周额度明明还有八九成；日志里 `turn_rate_limit_exceeded`、`Codex is at capacity, please try again later` | 10 月 25 日起的新一波，多是 GPT-6.1 Sol；也可能是官方故障或请求太多 | [429](#请求太多429-too-many-requests) |
| `unexpected status 401 Unauthorized`、`Incorrect API key provided: sk-svcac…` | 官方故障，或登录失效 | [401](#登录失效或官方故障401-unauthorized) |
| `start the Windows daemon from a non-elevated terminal`、`host Job Object prevents daemon detachment` | 0.157 起新增的后台服务 | [后台服务报错](#后台服务daemon报错) |
| Windows 上终端窗口一闪一闪 | 旧版后台服务的问题，已修 | [后台服务报错](#后台服务daemon报错) |
| 改了代理，终端里还是连不上 | 后台服务还在用旧的代理设置 | [4. 国内网络](#4-国内网络终端要单独设代理) |
| Mac 终端里 Cmd + C 复制不了 | CLI 改成了全屏界面 | [复制不了文字](#终端里复制不了文字) |
| `ran out of room in the model's context window` | 这个会话的上下文满了 | [上下文满了](#上下文满了ran-out-of-room-in-the-models-context-window) |
| 证书错误、`certificate` | 公司网络或抓包软件 | [证书错误](#证书错误公司网络抓包软件) |
| `You've hit your usage limit`、你已达到使用上限 | 额度用完 | [7. 额度用完了](#7-额度用完了) |
| 点「使用重置」提示「你当前的用量暂时无需重置」（`Your usage does not need a reset right now`），或者「没有可用的重置次数」「此重置额度已被使用」；重置卡用不了、不给用 | 重置有使用条件：覆盖的用量里至少一项剩余不到 80% | [重置机会用不了](#重置机会用不了你当前的用量暂时无需重置) |
| 「无法重置用量。请重试。」「无法加载用量限制重置信息」「用量详情不可用」；用了重置，额度或刷新时间看着不对 | 用量页面没加载出来，或者还没更新 | [重置机会用不了](#重置机会用不了你当前的用量暂时无需重置) |
| 续费、重新订阅会不会刷新额度；用了重置之后，下次刷新的时间变了 | 重新订阅不刷新；重置会改刷新时间，是规则 | [7. 额度用完了](#7-额度用完了)、[重置机会用不了](#重置机会用不了你当前的用量暂时无需重置) |
| Windows 设置未完成（旧版本写的是「Windows 安装未完成」）、设置无法完成、`helper_failed`、完成 Windows 设置以继续、设置智能体沙盒以继续（`Set up Agent sandbox to continue`）；CLI 里是 `Couldn't set up your sandbox with Administrator permissions`（无法设置管理员沙盒） | Windows 沙盒没装好 | [Windows 设置未完成](#windows-设置未完成helper_failed) |
| `自动审核超时`、`命令被自动审核拒绝`、`本轮操作已被自动审查终止` | 「帮我批准」的自动审核拦下了这条命令 | [权限](#权限不能联网写不了文件完全访问权限是灰的) |
| 命令连不上网，`npm install`、`pip install` 失败；改项目外的文件要批准 | 沙盒默认不让命令联网，这是正常的 | [权限](#权限不能联网写不了文件完全访问权限是灰的) |
| `rejected: blocked by policy`、`exec_command failed: CreateProcess … rejected: blocked by policy`，开着完全访问权限，删文件这类命令被拒，`/approve` 里也找不到 | 完全访问不会停下来问你，被标记的命令就直接拒掉 | [权限](#权限不能联网写不了文件完全访问权限是灰的) |
| 完全访问权限是灰的：已被 requirements.toml 禁用（`Disabled by requirements.toml`）、由组织管理、沙盒模式受此次安装的限制 | 管理员策略，本机设置改不动 | [权限](#权限不能联网写不了文件完全访问权限是灰的) |
| 无法启用远程控制。请重试、请确保仅有一个 ChatGPT 实例在运行、无法更新远程控制状态（`Couldn’t enable remote control`、`Couldn’t update remote control availability`） | 手机连电脑的远程控制打不开 | [远程控制](#远程控制无法启用远程控制couldnt-enable-remote-control) |
| 恢复对话失败、此任务正在其他位置运行；归档是什么意思、归档的对话在哪里、怎么删除对话 | — | [常见问题](#常见问题) |
| 已在另一个应用中打开 | 这个会话在别处开着 | [5. 桌面版打不开](#5-桌面版打不开) |
| 更新后打不开、只有启动动画、窗口空白 | 多半是新版本的问题 | [更新后打不开](#更新后打不开只有启动动画或白屏) |
| Windows 上 `Unable to locate Codex CLI` | 8 月 26.820 那一版的问题，官方当时加急修了，更新到最新版 | [#40752](https://github.com/openai/codex/issues/40752) |
| Windows 上每次更新后都打不开，任务管理器里挂着 `codex-windows-sandbox-service.exe` | 桌面版的沙盒服务没退出，一位用户结束它之后就能打开 | [更新后打不开](#更新后打不开只有启动动画或白屏) |
| `Unable to load organization settings`、无法加载组织设置、应用已暂停，等待安全加载组织设置后才能恢复；一直停在 `Starting your task`（正在启动你的任务） | Windows 上更新到 26.1021 后才出现的，多半是电脑里留着旧的 `CODEX_CLI_PATH` 环境变量；Agent 设成在 WSL 里运行的，是 10 月 14 日以后的另一个问题；还有桌面版 26.924 的老问题 | [更新后打不开](#更新后打不开只有启动动画或白屏) |
| 桌面版新对话一发就报「启动聊天时出错」（也有人记成「启动对话时出错」），下面一行是 `Cannot serialize value: (couldn't stringify value)`（英文界面 `Error starting chat`）；旧对话、「本地模式」的新对话正常 | 桌面版 26.1016.51333（10 月 21 日）的问题，只出在「工作树」模式，已修 | [启动聊天时出错](#新对话发不出去启动聊天时出错cannot-serialize-value) |
| 无法加载工作空间设置、请重试以继续聊天、`Couldn't load workspace settings`，新对话的发送按钮是灰的 | 桌面版 10 月 7 日那一版起的问题，到 26.1013 还没修完 | [无法加载工作空间设置](#无法加载工作空间设置couldnt-load-workspace-settings) |
| 无法检查锁定模式、`Couldn't check Lockdown mode`，日志里 `cloudflare_challenge` 或 `devicecheck_token_generation_failed` | 同上，桌面版取账号设置的请求失败了 | [无法加载工作空间设置](#无法加载工作空间设置couldnt-load-workspace-settings) |
| 无法加载你的账户、请尝试重新加载，或退出登录后重新登录、`We couldn't load your account`，点「重新加载」没用 | 桌面版登录后查不出这个账号的信息。原因不止一种，日志里写着是哪一种 | [无法加载你的账户](#无法加载你的账户we-couldnt-load-your-account) |
| `helper_unknown_error: setup refresh had errors`、`trusted Node process exited unexpectedly`、日志里 `os error 32` | Windows 沙盒的问题，10 月 7 日那一版开始 | [Windows 沙盒](#windows-沙盒setup-refresh-had-errors-os-error-32) |
| `CreateProcessSecurityEnvironment failed (HRESULT = 0x80070020)`，Windows 上连 `Get-Location` 都跑不了 | Windows 新的 MXC 沙盒 | [Windows 设置未完成](#windows-设置未完成helper_failed) |
| `Failed to create unified exec process: No such file or directory (os error 2)` | Windows 桌面版开了 Run agent in WSL | [WSL](#wslfailed-to-create-unified-exec-process-os-error-2) |
| 「提交消息时出错」（`Error submitting message`）、`Unable to determine project root for task` | Codex 网页版云端任务的问题，还没修 | [网页版](#网页版unable-to-determine-project-root-for-task) |
| 「提交消息时出错」（`Error submitting message`）、`Can't find variable: DisposableStack` | Safari 和 iPad 上的 Codex 网页版，10 月 19 日起 | [网页版](#网页版unable-to-determine-project-root-for-task) |
| 桌面版每条消息都「提交消息时出错」（`Error submitting message`），下面一行或日志里是 `Work browser access requires an authenticated account`；用的是 API Key 或第三方模型 | `config.toml` 里开了 Work 的显示方式，它要 ChatGPT 账号 | [接第三方时的报错](#接第三方时的报错) |
| 「无法生成提交信息」、提交信息时出错（在「提交」窗口里，提交信息留空让它自动生成时） | 和上面发消息的「提交消息时出错」不是一回事：这是自动生成 Git 提交信息失败，原因写在冒号后面 | 自己在「提交信息」里写一句再提交，就不用它生成；冒号后面是网络类的错误，见[第 4 节](#4-国内网络终端要单独设代理) |
| `OAuth state not found`，桌面版里连接 GitHub、Slack 这类插件，浏览器授权完回来报错 | 桌面版的连接流程出错 | [插件连不上](#插件和-mcp-连不上oauth-state-not-found) |
| `Server is draining; retry after reconnecting`、`Background server has incompatible feature settings` | 后台服务自己更新了 | [后台服务报错](#后台服务daemon报错) |
| VS Code 里语音输入一闪就停，显示 `Dictation stopped`，日志里 `HTTP 403 Forbidden` | 插件的语音接口被拒，OpenAI 10 月 14 日说已在服务器端修好，不用更新插件，重新加载窗口再试 | [#49351](https://github.com/openai/codex/issues/49351) |
| `failed to open daemon process`、`Access is denied. (os error 5)` | 后台服务留下的进程号过期了 | [后台服务报错](#后台服务daemon报错) |
| `failed to send websocket request`、`os error 10054`（远程主机强迫关闭了一个现有的连接）/ `10053`（你的主机中的软件中止了一个已建立的连接），只有某个旧会话一直重连 | 这个会话的历史里图片太多，请求太大 | [断线重连](#断线重连stream-disconnected-before-completion) |
| `untrusted-process-ancestry`，内置浏览器或浏览器控制用不了 | macOS 上的问题，还没修 | [最近的已知问题](#最近两周的已知问题) |
| `account/read failed during TUI bootstrap`、`workspace routing discovery failed`、`failed to load workspace requirements` | 后台服务版本旧了，或者还记着旧的代理端口 | [后台服务报错](#后台服务daemon报错) |
| 桌面版 `Sign-in failed: workspace routing discovery timed out`，常和 `token_exchange_failed` 一起 | 桌面版不读终端里的代理变量 | [4. 国内网络](#4-国内网络终端要单独设代理) |
| `workspace routing discovery failed`，日志里 `/backend-api/codex/models` 返回 `404`；CLI、插件、桌面版全都这样，网页版正常 | 10 月 20 日起账号那一侧的问题，换网络、重装没用 | [最近的已知问题](#最近两周的已知问题) |
| `Couldn't verify feedback permissions`、未能验证反馈权限。请重试 | `/feedback` 自己出错 | [常见问题](#常见问题) |
| Windows 桌面版登录后几十秒自己退出，没有任何提示 | 10 月 7 日那一版装运行时组件时崩溃 | [登录后自己退出](#windows-桌面版登录后几十秒自己退出) |
| VS Code 日志里 `Failed to release queued message send lock`、`"undefined" is not valid JSON` | 插件 10 月 1 日那一版的问题，已修：更新插件，还卡住就重启 VS Code | [#49988](https://github.com/openai/codex/issues/49988) |
| `You have reached your Codex usage limits for code reviews` | GitHub 代码审查有单独的额度 | [最近的已知问题](#最近两周的已知问题) |
| `mountinfo path is not absolute` | Linux 桌面版 9 月下旬几个版本的沙盒问题，已修：更新到 26.930 以上 | [#47345](https://github.com/openai/codex/issues/47345) |
| `` `wire_api = "chat"` is no longer supported ``、接 DeepSeek 或第三方 API 时 `404 Not Found … /responses` | Codex 只认 Responses 协议，或地址填得不对 | [接第三方时的报错](#接第三方时的报错) |
| `API key login is required, but ChatGPT is currently being used. Logging out.` | 配置里的 `forced_login_method = "api"` 把 ChatGPT 登录退掉了 | [10. 接入 DeepSeek](#10-接入-deepseek-和第三方-api) |
| `Missing environment variable`、`Model metadata for … not found`、`unrecognized configuration setting` | 第三方模型的配置没写全，或有新版不认的项 | [接第三方时的报错](#接第三方时的报错) |
| 换了中转站后旧会话报 `invalid_encrypted_content`、`encrypted content hydration failed` | 旧会话里的推理内容是上一家加密的 | [接第三方时的报错](#接第三方时的报错) |
| `Not inside a trusted directory and --skip-git-repo-check was not specified.` | `codex exec` 只在 Git 仓库里跑 | [常见问题](#常见问题) |
| `command failed; retry without sandbox?` | 命令在沙盒里失败了，问你要不要在沙盒外重跑 | [权限](#权限不能联网写不了文件完全访问权限是灰的) |
| 装 CLI 时 `curl: (22) The requested URL returned error: 404`；`codex update` 里也是这句 404，最后却显示更新成功 | 官方安装脚本的地址 10 月 23–24 日时不时返回 404，OpenAI 26 日说是临时的路由问题，已恢复 | [安装脚本 404](#官方安装脚本-404codex-update-没更新上) |
| `codex update` 之后版本没变；想卸载、降级 | 电脑里装了两份，或不知道当初怎么装的 | [更新、降级、卸载](#更新降级卸载) |
| Microsoft Store 下载不动、一直「正在开始下载」、此应用在您所在的地区不可用 | 商店缓存或地区设置 | [1. 安装](#1-安装) |
| 界面是英文，想切成中文 | 设置里就能改 | [8. 怎么设置中文](#8-怎么设置中文) |
| 设置了中文，重启后还是英文，界面还变回旧版 | 桌面版启动那一刻连不上 OpenAI | [选了中文界面还是英文](#选了中文界面还是英文) |
| Windows 上读出来的中文是乱码、改完的文件里中文变乱码，或者 `apply_patch verification failed` | 中文 Windows 默认 GBK 编码，PowerShell 按它读 | [Windows 上中文乱码](#windows-上中文乱码) |
| `MCP client for … timed out after 30 seconds`、`timed out handshaking with MCP server` | 这个 MCP 服务器 30 秒内没启动完 | [插件和 MCP 连不上](#插件和-mcp-连不上oauth-state-not-found) |
| Windows 上桌面版有进程、没窗口，日志里 `Failed to get 'appData' path` | Windows 用户名是中文 | [5. 桌面版打不开](#5-桌面版打不开) |
| Windows 上一打开就弹「无法遍历该路径，因为它包含不受信任的装入点」（`The path cannot be traversed because it contains an untrusted mount point`） | 开着阿里云盘，或者应用装在了 C 盘以外的盘上 | [5. 桌面版打不开](#5-桌面版打不开) |
| Mac 的磁盘空间莫名少了几十上百 GB | 桌面版的崩溃报告一直往磁盘里写，不提示 | [常见问题](#常见问题) |
| Computer Use（中文界面叫「电脑操控」）插件不可用、「应用内浏览器不可用」；设置里浏览器插件写着「已被你的组织停用，或在你所在地区不可用」（有的版本写「已被你的组织停用或暂不支持你所在地区使用」） | 插件没装或没开、系统权限没给，或者所在地区不提供 | [常见问题](#常见问题) |
| VS Code 插件侧边栏只显示 `Codex could not start`、`The extension could not start its user interface.`，日志里 `renderer_ready_timeout`；一直转圈、一直加载 | 插件 10 月 24 日更新的 26.1021.22027 的问题 | [VS Code 插件打不开](#vs-code-插件打不开codex-could-not-start) |
| 「此聊天已被标记为可能存在网络安全风险」「你的对话中有多个可能的网络安全风险的标记」「此内容无法显示」「正在进一步审慎考虑此请求」；`This chat was flagged for possible cybersecurity risk`；CLI 里 `Your account was flagged for potentially high-risk cyber activity` | 网络安全方面的安全检查，拦下或放慢了这次请求，这一条本身不是封号 | [被标记为网络安全风险](#被标记为网络安全风险flagged-for-possible-cybersecurity-risk) |
| `codex resume` 里看不到以前的会话 | 默认只列当前目录的会话 | [常见问题](#常见问题) |
| 字太小、想调字号 | 设置 → 外观 | [常见问题](#常见问题) |
| 桌面版卡顿、整台电脑跟着变慢、鼠标卡，内存占用高、CPU 占满 | Mac 上常见的是 `syspolicyd`、`trustd` 占满 CPU，6 月起的老问题，还没修 | [常见问题](#常见问题)（「Codex 为什么这么慢」） |

不知道是哪一类，先在终端跑一次 `codex doctor`。它会检查安装、配置、登录和网络，每一项打 ✓ 或 ⚠，最后一栏 Connectivity 就是网络情况。

## 最近两周的已知问题

9 月 25 日的 CLI 0.157.0 改动很大：默认启动一个后台服务（daemon），界面改成全屏。9 月 24 日到 10 月 3 日，openai/codex 新开的 issue 里有 280 个提到 daemon。下面是最近两周反馈最多、现在还没完全解决或者刚修好的问题，状态核对到 10 月 26 日，新出的排在前面。桌面版 10 月 21 日推了 26.1016.51333，22 日接连推了 26.1016.61520 和 26.1016.62342（Microsoft Store 里显示的包版本号是另一套编号，比如 26.1016.8622.0）；23 日前后先有 26.1021.22027（build 21747），24 日起大多数人收到的是 **26.1021.31345**（build 21828，自带 CLI 0.164.0-alpha.11.1；Windows 商店包版本 26.1021.3705.0）。**VS Code 插件也有一个 26.1021.22027**，北京时间 10 月 24 日早上自动更新，下表第一行就是它的问题，别和桌面版弄混。CLI 0.164.0 正式版北京时间 10 月 23 日中午发布，到 26 日没有新的正式版，npm 国内镜像和 Homebrew 也都是 0.164.0；它修了长会话恢复失败、Windows 公司代理要认证时连不上，新加了 `/presets` 存常用提示词，`/model`、`/permissions` 可以直接带参数（[发布说明](https://github.com/openai/codex/releases/tag/rust-v0.164.0)）。表里注明了哪些问题在新版上修好了、哪些还在。先看你的问题是不是已经修了，升级就能解决的不用再折腾。已经修好的，和 9 月下旬到现在都没有进展的问题，收在表下面的折叠区里。

| 现象 | 出在哪个版本 | 现在的状态 | 怎么办 |
| --- | --- | --- | --- |
| VS Code 插件打不开：侧边栏只有 `Codex could not start` / `The extension could not start its user interface.`，日志里 `Webview renderer did not become ready … reason=renderer_ready_timeout`；或者一直转圈 | 插件 26.1021.22027（预览版 26.51021.22027），北京时间 10 月 24 日早上自动更新之后；Windows、Linux、Intel Mac、Remote-SSH、WSL 都有，报告的人大多同时用着 VS Code 1.143.1 | 没有官方回复。10 月 24 日以来至少 16 个 issue（[#56655](https://github.com/openai/codex/issues/56655)、[#56691](https://github.com/openai/codex/issues/56691)、[#56660](https://github.com/openai/codex/issues/56660)、[#56617](https://github.com/openai/codex/issues/56617)），到 26 日插件市场还没有新版本。退回 26.1007.21434 十几位用户确认管用，也有一两位说没用 | 见 [VS Code 插件打不开](#vs-code-插件打不开codex-could-not-start) |
| 周额度还剩八九成，却一直报 `exceeded retry limit, last status: 429 Too Many Requests`，或者「正在思考」好几分钟才动 | 10 月 25 日起；报的多是 GPT-6.1 Sol，也有人 Astra 一样；桌面版 26.1021.31345、VS Code 插件、CLI 0.164.0 都有，Plus、Pro 都有 | 没有官方回复，状态页没有记录。报告的人说 25 日开始遇到，26 日 openai/codex 里新开了 5 个带这句原文的 issue（[#57072](https://github.com/openai/codex/issues/57072)、[#57073](https://github.com/openai/codex/issues/57073)、[#57084](https://github.com/openai/codex/issues/57084)）。一位用户抓到了原始返回：HTTP 429，`turn_rate_limit_exceeded`，`Codex is at capacity, please try again later`，让 205 秒后再试 | 见 [429](#请求太多429-too-many-requests) |
| Windows 桌面版更新后，一打开就是「无法加载组织设置」（`Unable to load organization settings`），点重试、退出登录都回到这一页；修复、重置、重装都没用 | 26.1021.31345（商店包 26.1021.2667.0、26.1021.3705.0），10 月 24 日起，到 26 日 8 个 issue | 没有官方回复。三位用户查到同一个原因：电脑里留着一个旧的用户环境变量 `CODEX_CLI_PATH`，指着早先自己装或复制的 `codex.exe`，删掉就能打开（[#48324](https://github.com/openai/codex/issues/48324#issuecomment-6381489469)、[#56620](https://github.com/openai/codex/issues/56620#issuecomment-6382621764)、[#57051](https://github.com/openai/codex/issues/57051#issuecomment-6416185957)）。也有人删了这个变量还是打不开，日志里是另一种错误（[#56716](https://github.com/openai/codex/issues/56716)） | 见[更新后打不开](#更新后打不开只有启动动画或白屏) |
| 点「使用重置」提示「你当前的用量暂时无需重置」（`Your usage does not need a reset right now`），周额度明明只剩八成多 | 北京时间 10 月 23 日凌晨那次全体重置之后，好几个人遇到（[#56262](https://github.com/openai/codex/issues/56262)、[#56258](https://github.com/openai/codex/issues/56258)） | **是规则，不是故障。** OpenAI 帮助中心现在写明：重置覆盖的用量里至少一项剩余不到 80% 才能用，不满足时重置留着，到期前还能用。[#56262](https://github.com/openai/codex/issues/56262) 里有人翻出这篇文章 9 月 6 日的存档，当时没有这条门槛 | 用掉两成以上再点；注意到期时间，见[重置机会用不了](#重置机会用不了你当前的用量暂时无需重置) |
| Windows 桌面版更新后，旧对话恢复不了、新对话里命令也跑不了，日志里是 `failed to load AGENTS.md instructions for environment 'local'` 和 `MXC launcher: The supplied process.env cannot launch a Windows process container because it is missing the required variable(s): SYSTEMROOT` | 26.1016.62342 起，26.1021.31345（商店包 26.1021.3705.0）上仍有；权限菜单里选的是「自定义 (config.toml)」 | 没有官方回复（[#56264](https://github.com/openai/codex/issues/56264)，24 日以来又有好几位用户报）。修复 [#56468](https://github.com/openai/codex/pull/56468) 10 月 23 日已经合并，但到 26 日还没进任何一个 CLI 或桌面版。一开始以为只和 `deny` 规则有关；24 日一位用户逐项对照：桌面版里**所有**自定义权限配置都失败，默认权限和「请求批准」正常，同样的配置在单独装的 CLI 里正常。另一位对照了版本：商店包 26.1016.8120.0（桌面版 26.1016.61520）正常，26.1016.8622.0 和 26.1021.3705.0 都不行 | 权限菜单里先换回「请求批准」，或者先用单独装的 CLI；等带上 #56468 的新版本 |
| CLI、插件、桌面版全都报 `workspace routing discovery failed`，日志里 `/backend-api/codex/models` 返回 `404 {"detail":"Not Found"}`；同一个账号网页版正常 | 10 月 20 日起，Windows CLI、VS Code 插件都有，个人账号、Pro 都有 | 没有官方回复（[#55566](https://github.com/openai/codex/issues/55566)、[OpenAI 社区](https://community.openai.com/t/1408110)）。报告人用全新的 `CODEX_HOME` 重新登录、换手机热点都一样；**10 月 21 日他说什么都没改，大约 10 小时后自己好了** | 不是你的网络和安装，别重装。先用网页版，等几个小时；一直不好，带着出错时间找 OpenAI 客服 |
| Codex 网页版发消息就报 `Error submitting message`、`Can't find variable: DisposableStack`；iPhone、iPad 的 ChatGPT 里是 `Codex server returned an error` | 10 月 19 日起，Mac 上的 Safari 26.5、27，iPad 上的 Safari 和 Chrome | 北京时间 10 月 24 日早上，两个 issue 都被 OpenAI 的工单机器人标成已完成（[#55271](https://github.com/openai/codex/issues/55271)、[#55361](https://github.com/openai/codex/issues/55361)），没有说明。到 26 日没有用户回来确认修好，24 日以后也没有人再报这一句 | 刷新后再试一次；还报，见[网页版](#网页版unable-to-determine-project-root-for-task) |
| 用 API Key 或第三方模型（没登 ChatGPT 账号），桌面版每条消息都 `Error submitting message`，日志里是 `Work browser access requires an authenticated account`；有人 Work 模式一直停在「Loading models…」 | Windows 桌面版 26.1013.40934 起，26.1016 上仍有 | 没有官方回复（[#54841](https://github.com/openai/codex/issues/54841)，30 多条回复，不少是中文用户）。**用户查到了原因和改法**，至少三个人确认管用；10 月 23 日又有两位用户确认，用 CC Switch 的把连接方式从「直连」换成「路由」也能发 | 见[接第三方时的报错](#接第三方时的报错) |
| 周额度几十秒内掉了一大截：77% 变 10%、58% 变 0%，重置时间没变 | 北京时间 10 月 17 日凌晨起，Windows、Linux 桌面版都有 | **官方认了。** 北京时间 10 月 21 日 01:10，Tibo 说约 4 万个账号实际用的额度上限和所选套餐对不上，已经修正，会逐个联系受影响的人并补 credits（[原帖](https://x.com/thsottiaux/status/2112592264686149749)）。[#54677](https://github.com/openai/codex/issues/54677)、[#54718](https://github.com/openai/codex/issues/54718) 的发帖人收到了邮件，两位都是从 Pro 200 换到了更便宜的套餐，各补 $500（12,500 credits，2027 年 1 月 20 日到期）；周额度本身没有退回。issue 里也有人说自己没换过套餐，是不是同一个原因还不清楚 | 先看邮箱有没有 OpenAI 的「Correcting your usage limit」；没收到又觉得被多扣了，截下 Usage 页面，找 OpenAI 客服，见[第 7 节](#7-额度用完了) |
| 开着完全访问权限，删文件、清理目录这类命令报 `rejected: blocked by policy`，`/approve` 里也看不到被拒的记录 | 10 月 10 日起，Windows 桌面版居多，CLI、插件也有 | **OpenAI 的人回复了**（[#54476](https://github.com/openai/codex/issues/54476#issuecomment-6207845526)，10 月 16 日）：完全访问不会停下来问你，被标记的命令就直接拒掉，也不经过自动审核。这句报错写得不清楚，他们也承认。10 月 10 日以来至少 35 个 issue 报这一句 | 改成「帮我批准」，见[权限](#权限不能联网写不了文件完全访问权限是灰的) |
| Windows 上所有命令都报 `CreateProcessSecurityEnvironment failed (HRESULT = 0x80070020)`，连 `Get-Location` 都跑不了 | Windows 桌面版 26.1013.40934 起，10 月 19 日起；26.1016 上仍有 | 没有官方回复（[#55230](https://github.com/openai/codex/issues/55230)）。第二位报告人查到和新的 MXC 沙盒有关：CLI 加上 `prefer_mxc=true` 就复现，不加就正常。北京时间 10 月 24 日又有两位在 26.1016 上报类似的情况，其中一位连 `cmd.exe /c echo` 都一直挂着不返回；24–26 日又新开了 3 个（[#56770](https://github.com/openai/codex/issues/56770)、[#56788](https://github.com/openai/codex/issues/56788)、[#57069](https://github.com/openai/codex/issues/57069)） | 见 [Windows 设置未完成](#windows-设置未完成helper_failed) |
| 桌面版里连接插件（GitHub、Figma、Firecrawl、自己加的 MCP 服务器等），浏览器里授权完回到应用报 `OAuth state not found` | 10 月 16 日起，Windows 26.1013 居多 | 没有官方回复（[#54626](https://github.com/openai/codex/issues/54626)、[#54924](https://github.com/openai/codex/issues/54924)、[#55314](https://github.com/openai/codex/issues/55314)）。几位用户确认：改在网页版连接，桌面版里就能用 | 见[插件连不上](#插件和-mcp-连不上oauth-state-not-found) |
| Windows 上把 Agent 设成在 WSL 里运行，一打开就弹 `Unable to load organization settings`，关不掉 | 10 月 14 日桌面版自动更新内核之后，26.1007.9446.0 到 26.1013.4894.0 都有 | 没有官方回复（[#54478](https://github.com/openai/codex/issues/54478)、[#54300](https://github.com/openai/codex/issues/54300)）。至少三位用户来回切换确认过：设回 Windows 就正常 | 见[更新后打不开](#更新后打不开只有启动动画或白屏) |
| 回复到一半失败：`stream disconnected before completion: An error occurred while processing your request`，带 request ID，重连 5/5 后停下；压缩上下文时是 `Error running remote compact task` | 10 月 9 日起明显变多；Windows、macOS 桌面版 26.1007，VS Code 插件，CLI 0.162 | 没有官方回复，状态页没有对应的事故记录；相关 issue 都还开着（[#52919](https://github.com/openai/codex/issues/52919)、[#52814](https://github.com/openai/codex/issues/52814)、[#53127](https://github.com/openai/codex/issues/53127)），10 月 26 日还有人在 26.1021 上遇到 | 见[服务器处理失败](#服务器处理失败an-error-occurred-while-processing-your-request) |
| 桌面版登录后停在「无法加载你的账户 / 请尝试重新加载，或退出登录后重新登录」（`We couldn't load your account`），点「重新加载」没用；同一台电脑换个账号可能就正常 | Windows、macOS 桌面版，9 月下旬起陆续有人报；10 月 12 日有读者在 macOS 26.1007 上遇到 | 没有官方回复（[#51548](https://github.com/openai/codex/issues/51548)、[#47969](https://github.com/openai/codex/issues/47969)、[#48571](https://github.com/openai/codex/issues/48571)） | 见[无法加载你的账户](#无法加载你的账户we-couldnt-load-your-account) |
| 桌面版新对话发不出去：「无法加载工作空间设置 / 请重试以继续聊天」（`Couldn't load workspace settings`），旧对话和网页版正常 | Windows、macOS 桌面版 26.1002 起（10 月 7 日） | 没修完。26.1013.40934 上 macOS（10 月 17–19 日，实体机和虚拟机都有）、Windows（10 月 20 日）都还有人遇到，加起来二十多个 issue，没有官方回复。26.1016.61520 上有好有坏：一位 Mac 用户说更新后好了，自己关掉了 issue（[#52342](https://github.com/openai/codex/issues/52342)）；另一位 Mac 用户 10 月 22 日在同一版上照样被拦，日志里是 `cloudflare_challenge` 的 403（[#51917](https://github.com/openai/codex/issues/51917)）；10 月 26 日又有 Mac 用户在 26.1021.31345 上遇到 | 见[无法加载工作空间设置](#无法加载工作空间设置couldnt-load-workspace-settings) |
| 每条消息都报 `Selected model is at capacity`，换模型没用，周额度几乎没动；会话日志里是 `server_overloaded` | 10 月 9–10 日一波，10 月 12–14 日又一波，发帖的人大多在东八区；桌面版、CLI、网页版都有 | 9–10 日、12 日状态页都没有记录；13 日、14 日状态页记了两次 Codex 性能下降，都已标记解决。10 月 15 日 Codex 负责人 Tibo 说已经上了一批改动、加了算力，之后每天新开的相关 issue 从 13 日的 18 个降到 17 日的 0 个。**10 月 19 日起又多了**：19 日 8 个、20 日 19 个、21 日 29 个、22 日 22 个、23 日 18 个（按标题或正文里有这句原文统计，UTC 日期），issue 里没有官方回复，Tibo 也没再提。状态页这几天只记了两次：北京时间 21 日 10:26–10:48 ChatGPT、Codex 和 API 性能下降；22 日 10:18–16:50 印度、日本、韩国的用户 ChatGPT 对话和 Responses API 报错增多，用日本、韩国节点的可能碰上（[#55444](https://github.com/openai/codex/issues/55444)、[#55260](https://github.com/openai/codex/issues/55260)、[#55740](https://github.com/openai/codex/issues/55740)）。**24 日起少了**：24 日 2 个、25 日 0 个、26 日到 UTC 15 点 6 个；不过 25 日起换了个样子，变成周额度还有、却一直报 429，见上面第二行 | 见[模型满载](#模型满载selected-model-is-at-capacity) |
| 会话突然断开：`Server is draining; retry after reconnecting`，恢复时提示 `Background server has incompatible feature settings` | 后台服务自动更新的时候 | 没修（[#51763](https://github.com/openai/codex/issues/51763)，10 月 7 日） | 按提示重启后台服务，见[后台服务报错](#后台服务daemon报错) |

<details>
<summary>已经修好的，和 9 月下旬到现在都没有进展的（点开看）</summary>

| 现象 | 出在哪个版本 | 现在的状态 | 怎么办 |
| --- | --- | --- | --- |
| 桌面版新对话一发就报「启动聊天时出错 / Cannot serialize value: (couldn't stringify value)」 | 26.1016.51333（10 月 21 日），Mac、Windows、Linux 都有；只在「工作树」模式下新建对话时出现，「本地模式」和旧对话正常 | **已修。** 10 月 22 日起多位用户确认更新到 26.1016.61520 后好了，Windows 上也有人确认 26.1016.62342 正常；最多人报的那个 issue 168 个 👍，已关闭（[#55744](https://github.com/openai/codex/issues/55744)、[#55827](https://github.com/openai/codex/issues/55827)）。24 日以后没有新的报告，26.1021 上也没人再遇到 | 更新应用，见[启动聊天时出错](#新对话发不出去启动聊天时出错cannot-serialize-value) |
| `curl -fsSL https://chatgpt.com/codex/install.sh` 报 `curl: (22) The requested URL returned error: 404`；`codex update` 里也是这句，最后却显示更新成功 | 北京时间 10 月 23 日凌晨起断断续续，CLI 0.164.0 发布前后 | **已恢复。** 北京时间 10 月 26 日上午，OpenAI 的人关掉了 [#56630](https://github.com/openai/codex/issues/56630#issuecomment-6406215988) 和 [#56616](https://github.com/openai/codex/issues/56616)，说是一次临时的路由问题，现在已经好了；用户最后一次报 404 是 24 日上午。`codex update` 失败了还显示成功这一点，官方没说 | 还遇到的话，换 GitHub Releases 上的同一个脚本，或者 npm 镜像，见[安装脚本 404](#官方安装脚本-404codex-update-没更新上) |
| Windows 上所有命令都在执行前失败：`helper_unknown_error: setup refresh had errors`，日志里是 `os error 32` | Windows 桌面版 26.1002（10 月 7 日）；CLI 0.162.0、0.162.1 | 桌面版：10 月 9–10 日多位用户确认 26.1007.21434 **已修**（[#51601](https://github.com/openai/codex/issues/51601)，120 条回复）。CLI：0.162.1 上仍有人复现（[#51634](https://github.com/openai/codex/issues/51634)）；北京时间 10 月 16 日发布的 0.163.0 带上了修复（[#51822](https://github.com/openai/codex/pull/51822)），**当天先后有两位用户确认好了**（[#51590](https://github.com/openai/codex/issues/51590)），10 月 18 日又一位确认（[#51634](https://github.com/openai/codex/issues/51634)）。其中一位说内置浏览器控制还会报 `trusted Node process exited unexpectedly`，那是另一个问题 | 桌面版更新到 26.1007.21434；CLI 先 `codex update` 到 0.163.0，见 [Windows 沙盒](#windows-沙盒setup-refresh-had-errors-os-error-32) |
| Windows 桌面版开了 Run agent in WSL，每条命令都是 `Failed to create unified exec process: No such file or directory (os error 2)` | 桌面版 26.928 起 | 10 月 12 日 OpenAI 的人回复「会修」并关闭了 issue，但 10 月 14 日有人在新版 26.1007.71326 上仍遇到（[#49731](https://github.com/openai/codex/issues/49731)、[#54063](https://github.com/openai/codex/issues/54063)）。10 月 16 日一位用户确认 **26.1013.31256** 修好了，重启后也正常（[#49731](https://github.com/openai/codex/issues/49731)，对应的修复是 [#53412](https://github.com/openai/codex/pull/53412)） | 先更新到 26.1013.31256；还报的，用户给了挂载目录的办法，见 [WSL](#wslfailed-to-create-unified-exec-process-os-error-2) |
| GitHub 上 `@codex review` 被拒：`You have reached your Codex usage limits for code reviews`，Usage 页面的周额度却还有 | 9 月 29 日起有人反馈 | 没有官方回复（[OpenAI 社区](https://community.openai.com/t/1402074)）。代码审查的用量是单独算的 | 帖子里用户的办法：账号里有 credits 的，在 Code review 设置里打开 use credits。这个办法不是都管用：10 月 9 日有人开着这个设置、credits 余额也是正的，照样被拒（[#52549](https://github.com/openai/codex/issues/52549)） |
| Codex 网页版第一条消息发不出去：`Unable to determine project root for task` | 9 月 30 日起，云端环境 | 没修，还没有官方回复（[#49497](https://github.com/openai/codex/issues/49497)） | 见[网页版](#网页版unable-to-determine-project-root-for-task) |
| 套餐额度用完后，桌面版输入框直接变灰发不出去，即使模型走的是自己配置的 provider | 桌面版 26.924 起 | 没修（[#48816](https://github.com/openai/codex/issues/48816)，另有 [#48650](https://github.com/openai/codex/issues/48650)、[#48369](https://github.com/openai/codex/issues/48369)） | 等套餐额度恢复，或等官方修复 |
| 内置浏览器或浏览器控制用不了，报 `untrusted-process-ancestry` | macOS，9 月 27 日起 | 没修（[#48786](https://github.com/openai/codex/issues/48786)、[#49301](https://github.com/openai/codex/issues/49301)）；10 月 10 日 [LINUX DO](https://linux.do/t/topic/3002787) 上也有人问，没有人给出办法 | 暂时没有办法，等修复 |
| dot 建不了本地任务，提示 `not attached to an active Aeon`，或者在已保存的项目里没法接着做 | Pro 的 dots，9 月 30 日起 | 没修（[#51730](https://github.com/openai/codex/issues/51730)、[#49729](https://github.com/openai/codex/issues/49729)） | 先在 Codex 里自己建任务 |
| `start the Windows daemon from a non-elevated terminal` | CLI 0.157.0 起，Windows 管理员终端 | 没修，10 月 7 日还有人在问（[#48043](https://github.com/openai/codex/issues/48043)） | 用普通（非管理员）终端打开，或者 `codex --no-daemon`。报的是 `failed to open daemon process` 的，见[后台服务报错](#后台服务daemon报错) |
| Windows 桌面版 `Unable to load organization settings`，或更新后一直转圈 | 桌面版 26.924 起 | 没修，10 月 6 日有人在 26.930.6422.0 上仍复现（[#48324](https://github.com/openai/codex/issues/48324)、[#48463](https://github.com/openai/codex/issues/48463)）。10 月 15 日有用户查到自己的原因：日志里是 `approval_policy = "untrusted" is no longer supported`，从 `config.toml` 删掉这一行就好了 | 先查 `config.toml`，见[更新后打不开](#更新后打不开只有启动动画或白屏) |
| Mac 桌面版白屏 | 桌面版 26.915 | 没修，issue 到 10 月 21 日还开着，没有官方回复（[#46641](https://github.com/openai/codex/issues/46641)） | 同上 |
| 安卓手机连电脑（Codex Remote），一直回到 `Authorize this phone` | ChatGPT 安卓版 1.2026.265 | 没确认修好：10 月 4 日有人说安卓新版已经好了，也有人更新后还是不行（[#48774](https://github.com/openai/codex/issues/48774)、[#48555](https://github.com/openai/codex/issues/48555)） | 先把安卓版 ChatGPT 更新到最新试试。#48555 是电脑端换过 ChatGPT 账号之后出现的 |
| 重置卡到了到期那天，没到晚上就不见了 | 所有版本 | **官方答复**（OpenAI_Support，10 月 6 日，[OpenAI 社区](https://community.openai.com/t/1403334)）：每张卡有具体的到期时间，过期不能恢复 | 网页版 Settings → Usage 里，把鼠标悬停在到期日期上能看到具体时间和时区。没到时间就消失的，找客服查 |
| Tibo 说重置已全部到账，自己的额度却没变 | 10 月 2 日那次全体重置 | 没有官方回复（[#50451](https://github.com/openai/codex/issues/50451)）。Pro 500、Business 都有人反馈，Tibo 当晚说 Pro 500 已补上 | 先刷新 Usage 页面、确认账号和工作区；过一两天还没有，找 OpenAI 客服并附 Usage 截图 |
| Mac 自带终端里 Cmd + C 复制不了 | CLI 0.157.0 起 | 官方不改，是 Terminal.app 的限制（[#48122](https://github.com/openai/codex/issues/48122)） | 见[复制不了文字](#终端里复制不了文字) |

</details>

## 1. 安装

Codex 有三种用法，都是 OpenAI 官方的，用同一个 ChatGPT 账号登录：

| 用法 | 适合谁 | 怎么装 |
| --- | --- | --- |
| 桌面版 | 想先用起来 | 下载 ChatGPT 桌面应用，打开后在顶部下拉菜单里选 Codex |
| Codex CLI | 习惯在终端里写代码 | 一行命令，见下面 |
| 编辑器插件 | 主要在 VS Code、Cursor 里写代码 | 扩展市场搜 Codex，认准发布者 OpenAI、ID `openai.chatgpt` |

桌面版下载入口在官方文档的 [ChatGPT desktop app](https://learn.chatgpt.com/docs/app) 页面，点 Download ChatGPT 右边的小箭头选系统：Mac（Apple 芯片 / Intel）、Windows、Linux（预览版）。Windows 也可以用命令装：

```powershell
winget install --id 9PLM9XGG6VKS -s msstore
```

Microsoft Store 里一直「正在开始下载」「已排队」，或者下载报错的：按 Win + R 运行 `wsreset`，清掉商店缓存再试；也可以直接跑上面这条 winget 命令。提示「此应用在您所在的地区不可用」的，看 设置 → 时间和语言 → 语言和区域 →「国家或地区」，网上的办法是先改成美国再装，我们没有实测。官方只给了 Microsoft Store 这一个来源，网上转存的 `.msix` 安装包别装。

CLI 选一种装法就行。官方安装脚本要从 chatgpt.com 下载，国内经常卡住，**用 npm 国内镜像最省事**（需要 Node.js 16+）：

```bash
# npm 国内镜像（10 月 26 日核对：镜像版本和 npm 官方一致，都是 0.164.0；Windows 上 0.161–0.162 有沙盒问题，见「Windows 沙盒」一节）
npm install -g @openai/codex --registry=https://registry.npmmirror.com

# 官方安装脚本（macOS / Linux）
curl -fsSL https://chatgpt.com/codex/install.sh | sh

# 官方安装脚本（Windows PowerShell）
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"

# Homebrew
brew install --cask codex
```

装完检查一下，以后升级用 `codex update`：

```bash
codex --version
```

从 0.156 或更早的版本升上来，会发现两处不一样：界面占满整个终端窗口（全屏模式），以及多了一个常驻的后台服务。不习惯的话都能关，见[终端里复制不了文字](#终端里复制不了文字)和[后台服务报错](#后台服务daemon报错)。

> 别从网盘、群文件下载所谓的 Codex「中文版」「破解版」。CLI 是开源的，桌面版本身就能切中文（设置 → 常规 → 语言）。来路不明的安装包可能偷走你的登录凭证。

### 官方安装脚本 404、codex update 没更新上

```text
curl: (22) The requested URL returned error: 404
```

北京时间 10 月 23 日凌晨到 24 日上午，`https://chatgpt.com/codex/install.sh` 断断续续返回 404，换电脑、换网络都一样，openai/codex 里有好几个 issue 在报（[#56254](https://github.com/openai/codex/issues/56254)、[#56607](https://github.com/openai/codex/issues/56607)、[#56630](https://github.com/openai/codex/issues/56630)）。**已恢复**：北京时间 10 月 26 日上午，OpenAI 的人关掉了其中两个，说是一次临时的路由问题，现在已经好了（[#56630](https://github.com/openai/codex/issues/56630#issuecomment-6406215988)）。这个地址现在会跳转到 `releases.openai.com/codex/install.sh`，26 日我们再试，官方地址和下面 GitHub 上的两个地址都能下载。以后再碰上 404，用下面的办法。

**`codex update` 也受影响，而且会骗你。** 用官方脚本装的 CLI，`codex update` 背后跑的就是这个脚本。有人贴出的输出（[#56630](https://github.com/openai/codex/issues/56630)）：

```text
Updating Codex via `sh -c 'curl -fsSL https://chatgpt.com/codex/install.sh | CODEX_NON_INTERACTIVE=1 sh'`...
curl: (22) The requested URL returned error: 404

🎉 Update ran successfully! Please restart Codex.
```

中间那行 404 说明根本没下载到，最后一句「更新成功」不用信。更新完跑一次 `codex --version`，版本号没变就是没更新上。

换一个地址装，三选一：

```bash
# 1. GitHub Releases 上的同一个安装脚本（openai/codex 每个正式版都附了 install.sh 和 install.ps1）
curl -fsSL https://github.com/openai/codex/releases/latest/download/install.sh | sh

# 2. Windows PowerShell
powershell -ExecutionPolicy ByPass -c "irm https://github.com/openai/codex/releases/latest/download/install.ps1 | iex"

# 3. npm 国内镜像，不经过上面两个地址
npm install -g @openai/codex --registry=https://registry.npmmirror.com
```

第 1 条是 issue 里用户给的办法（[#56607](https://github.com/openai/codex/issues/56607)），第 2 条是同一个发布里的 Windows 版。我们核对过：这两个地址指向 0.164.0 正式版的附件，和官方地址是同一份脚本；脚本里先从 `releases.openai.com` 下载程序，那边不通会提示 `releases.openai.com is unavailable; falling back to GitHub Releases.`，自己改从 GitHub 下。GitHub 在国内也可能慢，慢就用第 3 条。原来用脚本装的，改用 npm 后电脑里可能有两份，按下面[更新了，版本还是旧的](#更新降级卸载)处理。

### 更新、降级、卸载

`codex update` 会按你当初的安装方式去更新，背后跑的就是下表「更新」一栏的命令（对照 0.162.1 的源码）。它卡住的时候，自己跑那一条就行。

| 当初怎么装的 | 更新 | 卸载 |
| --- | --- | --- |
| npm | `npm install -g @openai/codex`，想走国内镜像就加 `--registry=https://registry.npmmirror.com` | `npm uninstall -g @openai/codex` |
| Homebrew | `brew upgrade --cask codex` | `brew uninstall --cask codex` |
| 官方安装脚本（macOS / Linux） | 再跑一次安装脚本 | 删掉 `~/.local/bin/codex`、`~/.local/bin/codex-code-mode-host` 和 `~/.codex/packages/standalone` |
| 官方安装脚本（Windows） | 再跑一次安装脚本 | 删掉 `%LOCALAPPDATA%\Programs\OpenAI\Codex\bin` 和 `%USERPROFILE%\.codex\packages\standalone` |
| 桌面版 | 应用内检查更新；Windows 也可以在 Microsoft Store 里更新 | Mac：退出后把 ChatGPT 拖进废纸篓。Windows：设置 → 应用 → 已安装的应用 → ChatGPT → 卸载 |

官方脚本没有提供卸载命令，上面两行是按安装脚本写入的位置列的（[install.sh](https://github.com/openai/codex/blob/main/scripts/install/install.sh)、[install.ps1](https://github.com/openai/codex/blob/main/scripts/install/install.ps1)）。

**降级**只有 npm 方便，在包名后面写版本号：

```bash
npm install -g @openai/codex@0.160.1 --registry=https://registry.npmmirror.com
```

**更新了，`codex --version` 还是旧的？** 多半是电脑里装了两份，比如早先用 Homebrew 装过，后来又用 npm 装了一次。哪一份在 PATH 里排前面，运行的就是哪一份。把多余的那份卸掉：

```bash
which -a codex     # macOS / Linux：列出所有的 codex
where codex        # Windows
```

**想卸干净。** 上面删的只是程序。配置、登录凭证和所有对话记录都在 `~/.codex`（Windows 是 `%USERPROFILE%\.codex`）：`config.toml` 是配置，`auth.json` 是登录凭证，`sessions` 是对话记录。Mac 桌面版另有日志在 `~/Library/Logs/com.openai.codex/`。

1. 先停掉后台服务：`codex app-server daemon stop`。0.157 起它是常驻的，窗口关了也还在。
2. 按上表卸载程序。
3. 确定不要对话记录了，再删 `~/.codex`。只是想通过重装解决问题的，把这个目录改个名字留着，别直接删：删了要重新登录，之前没验证过手机号的账号，重新登录时可能会被要求[验证](#3-要验证手机号)。

## 2. 登录

第一次运行 `codex`，或者第一次打开桌面版、插件，都会让你选登录方式：

- **Sign in with ChatGPT**（中文界面：通过 ChatGPT 登录）：用你 ChatGPT 套餐里的 Codex 额度。大多数人选这个。
- **API Key**：按 API 用量另外计费，不走套餐额度；Codex 云端任务必须用 ChatGPT 登录。

登录会打开浏览器，你登录完，浏览器再把凭证交回本机的 Codex（默认走 `localhost:1455`）。**浏览器显示成功、终端却一直在等**，或者**登录完浏览器跳到 localhost、显示打不开**，通常是这个本机回调被网络设置挡住了，或者 Codex 不在这台电脑上（装在远程服务器上，或 VS Code、Cursor 连着 SSH）。这时改用设备码登录：

1. 先在 ChatGPT 网页版的安全设置（`chatgpt.com/settings/security`）里打开 `Enable device code sign-in for Codex, Excel, PowerPoint, and Word`（中文界面：为 Codex、Excel、PowerPoint 和 Word 启用设备代码登录），不开这一步会失败。
2. 运行 `codex login --device-auth`，或在登录界面选 Sign in with Device Code。VS Code、Cursor 插件是在登录页点 `More options`（更多登录选项）→ `Use device code`（使用设备码）。
3. 打开它给的链接，先登录，登录后输入一次性代码。

怎么认自己是不是远程环境、每一步容易卡在哪，见[登录后跳到 localhost 打不开](#登录后跳到-localhost-打不开err_empty_response)。

```bash
codex login status          # 看当前用哪种方式登录
codex login --device-auth   # 设备码登录
codex logout                # 退出登录
```

还有几件事要知道：

- 登录信息存在 `~/.codex/auth.json` 或系统钥匙串里。**这个文件等于账号钥匙**，别发给别人、别传到 GitHub、别贴进 issue。
- CLI 和编辑器插件共用一份登录信息，一边退出，另一边也要重新登录。
- 登录出问题要查日志，直接运行 `codex login` 时会在日志目录写一份 `codex-login.log`。

## 3. 要验证手机号

网页版 ChatGPT 好好的，一到 Codex 就跳出 `Verify your phone number` 或 `Phone number required`。这是现在问得最多的问题。

OpenAI 帮助中心的说法（手机验证那篇 9 月 23 日更新；相关的四篇 10 月 3 日逐篇核对过）：

- 在桌面版里使用 Codex 可能要求验证手机号。官方社区里不少人说 CLI 和 VS Code 插件登录也会被带到同一个验证页。
- 验证码只发短信，不发邮件，也不打电话。12 个国家的号码可以改用 WhatsApp 收：阿联酋、埃及、印度尼西亚、以色列、印度、马来西亚、尼日利亚、巴基斯坦、沙特阿拉伯、土耳其、乌克兰、越南。**邮箱和验证器 App（2FA）都代替不了。**
- 座机、收费号码、Google Voice 这类网络电话（VoIP）都不行。中国大陆、香港、澳门不在支持的国家和地区名单里，国内手机号一般用不了。
- **号码绑定后不能更换**，帮助中心写明 ChatGPT 和 API 都不提供改号。好在正常情况下验证一次就够了，不会反复要求。所以要用一个以后还收得到短信的号码：官方社区里有人因为几年前绑的旧号码早就不用了，进不了 Codex，客服 9 月 24 日的答复只是让他重新提交工单（[帖子](https://community.openai.com/t/codex-desktop-still-requires-verification-of-inaccessible-old-phone-number-case-11547509/1387193)）。

9 月 30 日的 CLI 0.159.3 起，用 ChatGPT 登录的本地会话里可能出现一条提醒，让你「完成账号安全设置」。发布说明写的是可选提醒（optional reminders），给谁显示由 OpenAI 服务器决定，不是每个人都有。

手边没有境外手机号：有实体卡或 eSIM 的直接用；没有的可以用接码平台（比如 hero-sms.com）临时买一个号码收码。不是每个号码都收得到，收不到就换一个；这类号码大多是临时的，OpenAI 又不支持改号，万一以后又要求验证，原来的号码可能已经用不了。

> 网上流行的 codex-auth-helper 这类浏览器插件，是把网页版的登录状态导出成 Codex 用的 auth.json，绕开 Codex 自己的登录。它要读取你完整的 ChatGPT 登录会话，等于把账号钥匙交给一个非官方插件。不建议用。

更详细的号码规则、截图和官方原文：[Codex 登录要验证手机号怎么办](https://aonir.com/guides/codex-install-login/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_phone#phone)。

## 4. 国内网络：终端要单独设代理

最常见的情况是：**浏览器能上 ChatGPT，终端里的 codex 却连不上。** 原因是 CLI 默认不读系统代理设置。`codex doctor` 的 Connectivity 一栏里能看到 `respect system proxy: disabled`，也就是「跟随系统代理」是关着的。

两种解决办法，选一种：

**办法一：给终端设代理环境变量。** 端口号在代理软件的设置里看，常见是 7890，下面的端口记得换成你自己的。

```bash
# macOS / Linux：只对当前终端生效
export HTTPS_PROXY=http://127.0.0.1:7890
export HTTP_PROXY=http://127.0.0.1:7890

# 想每次打开终端都生效：把上面两行加到 ~/.zshrc（bash 用户是 ~/.bashrc），然后
source ~/.zshrc
```

```powershell
# Windows PowerShell：只对当前窗口生效
$env:HTTPS_PROXY="http://127.0.0.1:7890"
$env:HTTP_PROXY="http://127.0.0.1:7890"

# 长期生效（写入当前用户的环境变量，重开终端后生效）
[Environment]::SetEnvironmentVariable("HTTPS_PROXY", "http://127.0.0.1:7890", "User")
[Environment]::SetEnvironmentVariable("HTTP_PROXY", "http://127.0.0.1:7890", "User")
```

**办法二：打开代理软件的 TUN（虚拟网卡）模式**，让所有程序的流量都走代理，不用单独设变量。桌面版里内置浏览器、Node 工具这类功能连不上时，TUN 模式也往往更省事，官方仓库里有相关报告（[#21713](https://github.com/openai/codex/issues/21713)、[#44364](https://github.com/openai/codex/issues/44364)）。

设好后跑 `codex doctor`，看 Connectivity 这几行：

```text
Connectivity
  ✓ network      network-related environment looks readable
      proxy env vars present   HTTP_PROXY, HTTPS_PROXY    ← 读到了代理变量
      respect system proxy     disabled                   ← 不跟随系统代理，正常
  ✓ websocket    connected (HTTP 101 Switching Protocols) ← 这行是 connected 就说明通了
```

**改了代理却不生效（CLI 0.157.0 起）。** 新版 CLI 会启动一个常驻的后台服务，真正连 OpenAI 的是它。官方仓库里有用户报告，后台服务一直用它启动那一刻的代理变量，之后换端口、换变量，新开的窗口也不认（[#48195](https://github.com/openai/codex/issues/48195)，用户报告，官方还没回复）。改完代理后把它停掉再开：

```bash
codex app-server daemon stop   # 停掉后台服务，下次运行 codex 会带着新的代理变量重新启动
codex --no-daemon              # 或者这一次不用后台服务
```

**办法三：写进 `~/.codex/.env`。** CLI 启动时会读 Codex 目录（`~/.codex`，Windows 是 `%USERPROFILE%\.codex`）下的 `.env` 文件，把里面的变量当成环境变量（对照 0.163.0 源码的 `load_dotenv`）。这样不用改系统设置，也不影响别的程序。端口换成你自己的：

```text
http_proxy=http://127.0.0.1:7890
https_proxy=http://127.0.0.1:7890
NO_PROXY=localhost,127.0.0.1,::1
```

`NO_PROXY` 让发往本机的请求不走代理，issue 里的人都带着这一行，照抄就行。[#52928](https://github.com/openai/codex/issues/52928) 里两位用户报 `workspace routing discovery timed out`，就是这样解决的。改完执行一次 `codex app-server daemon stop`，让后台服务带着新变量重启（见下面）。

**桌面版不读这个文件，也不读终端里设的变量。** Windows 桌面版登录时报 `Sign-in failed: workspace routing discovery timed out`、`token_exchange_failed` 的，[#53261](https://github.com/openai/codex/issues/53261) 里一位用 Clash 的用户试过：写进 `.codex\.env` 没用，用上面「长期生效」那两行把 `HTTP_PROXY`、`HTTPS_PROXY` 设成 Windows 用户变量，再加一个 `NO_PROXY`（值是 `localhost,127.0.0.1,::1`），注销 Windows 或重启电脑后就能登录了。开 TUN 模式也行。

登录、新建任务、开对话时报 `timed out`、`timeout`、`Operation timed out` 一类的，多半也是这一节的问题：请求根本没走代理，等到超时。先跑 `codex doctor` 看 Connectivity。只有某个 MCP 工具超时的不一样，见[插件和 MCP 连不上](#插件和-mcp-连不上oauth-state-not-found)。

还有一条：**节点尽量固定，别频繁切换地区。** OpenAI 帮助中心把「从陌生的地点登录」列为账号被临时限制的安全原因之一。

## 5. 桌面版打不开

先看是哪一种：

| 你看到的 | 多半是 | 怎么办 |
| --- | --- | --- |
| Windows 设置未完成（旧版本写的是「Windows 安装未完成」）、设置无法完成、`helper_failed`、完成 Windows 设置以继续 | Windows 沙盒没装好 | [看下面](#windows-设置未完成helper_failed) |
| 已在另一个应用中打开 | 这个会话还在终端、编辑器或另一个窗口里开着 | 在那边关掉这个会话，回来点「重试」 |
| 更新后双击没反应、只有启动动画、窗口一片空白 | 多半是新版本的问题 | [看下面](#更新后打不开只有启动动画或白屏) |
| 无法加载组织设置（`Unable to load organization settings`），点重试没用 | Windows 上更新到 26.1021 后才出现的，先查旧的 `CODEX_CLI_PATH` 环境变量；其次是旧配置里的 `approval_policy = "untrusted"`、Agent 设成在 WSL 里运行，或者 Windows 桌面版 26.924 的老问题 | [看下面](#更新后打不开只有启动动画或白屏) |
| 能打开，但新对话发不出去：无法加载工作空间设置（`Couldn't load workspace settings`） | 桌面版 10 月 7 日那一版起的问题，还没修完 | [看下面](#无法加载工作空间设置couldnt-load-workspace-settings) |
| 能打开，新对话一发就是「启动聊天时出错 / Cannot serialize value」 | 26.1016.51333 在「工作树」模式下的问题，已修 | [看下面](#新对话发不出去启动聊天时出错cannot-serialize-value) |
| 登录后只有一页「无法加载你的账户」（`We couldn't load your account`），点「重新加载」没用 | 桌面版查不出这个账号的信息，原因不止一种 | [看下面](#无法加载你的账户we-couldnt-load-your-account) |
| 任务一直停在 `Starting your task`（中文界面：正在启动你的任务） | Linux 桌面版 26.924 的问题，已修 | 更新到最新版 |
| 能打开，但登录页出不来、一直转圈 | 网络 | [第 4 节](#4-国内网络终端要单独设代理) |
| Windows 上一直停在启动画面，以前登过公司或拼车的 Team 工作空间 | 保存的登录指着一个已经停用的工作空间（`402`，`deactivated_workspace`） | 完全退出，把 `%USERPROFILE%\.codex\auth.json` 改名成 `auth.json.bak`，再打开重新登录，见[按状态码查](#按状态码查400403404429500502503)的 402 |
| Windows 上任务管理器里有好几个 `ChatGPT.exe`，就是不出窗口；重装没用 | 一位中文用户查到，Windows 用户名是中文时，应用算出的用户目录路径成了乱码，日志里是 `Desktop network requirements prevented startup` 和 `Failed to get 'appData' path`（[#54322](https://github.com/openai/codex/issues/54322)，26.1013.3656.0 上仍有，没有官方回复） | 他用管理员 PowerShell 建了一个目录联接，把乱码路径指回真实的用户目录，步骤见[他的回复](https://github.com/openai/codex/issues/54322#issuecomment-6228899299)。这是用户的办法，动之前看清楚每条命令；不急的话先用 CLI 或网页版 |
| Windows 上一打开就弹「无法遍历该路径，因为它包含不受信任的装入点」，前面是 `C:\Program Files\WindowsApps\OpenAI.Codex_…\ChatGPT.exe` 这样的路径（英文 `The path cannot be traversed because it contains an untrusted mount point`） | 用户报告了两种情况，都没有官方回复：**开着阿里云盘**（[#39449](https://github.com/openai/codex/issues/39449)：阿里云盘开着就打不开，完全退出阿里云盘后就能打开；为什么会互相影响没查清）；**应用装在了 C 盘以外的盘上**，比如在「存储 → 新内容的保存位置」里把新应用设到了 D 盘（[#46277](https://github.com/openai/codex/issues/46277)：同一个版本装在 C 盘就正常） | 先完全退出阿里云盘（任务栏右下角的图标里退出，不是只关窗口）再打开 ChatGPT。装在别的盘上的：把「新内容的保存位置」里的新应用改回 C 盘，卸载 ChatGPT 再从 Microsoft Store 重装 |

### Windows 设置未完成（helper_failed）

Windows 版的 Codex 要在你电脑上改代码、跑命令，先得建一个沙盒（隔离环境），这一步需要一次管理员授权。界面会提示「完成 Windows 设置以继续」，失败时显示「Windows 设置未完成」（旧版本写的是「Windows 安装未完成」），下面一行小字是原因：「设置无法完成」对应错误码 `helper_failed`，「设置超时」对应 `setup_timeout`，另外还有「审批已取消」「启动设置未成功」。

官方文档列的常见原因：弹出「用户账户控制」时点了「否」；电脑不允许创建本地用户和组、不允许改防火墙；公司的管理策略挡住了其中一步。

CLI 里失败时是一个选择框，标题是 `Couldn't set up your sandbox with Administrator permissions`（搜「无法设置管理员沙盒」的就是这一句），下面两个选项：`Try setting up admin sandbox again`（再试一次管理员沙盒）和 `Use Codex with non-admin sandbox`（改用非管理员沙盒）。选第二个，效果和下面第 3 步一样。公司电脑上要是写着 `Your organization requires the default sandbox before Codex can continue`，就只能找 IT（对照 0.163.0 的 `windows_sandbox_prompts.rs`）。

要管理员授权的是老的那种沙盒（`elevated`）。官方 Windows 沙盒文档现在推荐一种新的 MXC 沙盒：用 Windows 自带的进程隔离，**不需要管理员授权**，也不用建额外的 Windows 账号和防火墙规则；桌面版在支持它的电脑上会自动用，单独装的 CLI 可以在 `config.toml` 里加上下面两行打开。电脑不支持的，还是走老的设置流程。

```toml
[features]
prefer_mxc = true
```

**加了这两行以后所有命令都报 `CreateProcessSecurityEnvironment failed (HRESULT = 0x80070020)` 的，先删掉。** 10 月 19 日起有 Windows 用户在桌面版 26.1013.40934 上遇到这一句，连 `Get-Location` 都跑不了；第二位报告人在 CLI 上对照过，加 `prefer_mxc=true` 就复现，不加就正常（[#55230](https://github.com/openai/codex/issues/55230)，没有官方回复）。桌面版自己会用 MXC，这个设置改不了；桌面版报这一句的，先用单独装的 CLI。

按顺序试：

1. 点「重新设置」（英文 Retry setup），弹出「用户账户控制」时点「是」。
2. 公司电脑：问 IT 是否允许这类管理员授权的设置（创建本地用户和组、改防火墙规则、给沙盒用户登录权限）。
3. 急着用，可以换成官方的备用沙盒。它的隔离比默认的弱一些，但能继续改代码、跑命令。在 `%USERPROFILE%\.codex\config.toml` 里加：

   ```toml
   [windows]
   sandbox = "unelevated"
   ```

   界面上的「在没有管理员访问权限的情况下继续」（Continue without administrator access）走的也是不需要管理员权限的沙盒。如果提示的是「你可以继续聊天，但 ChatGPT 无法创建文件、编辑代码或执行操作」，那是公司的管理策略不允许更新沙盒，这时只能聊天，要找 IT。
4. 打开日志 `%USERPROFILE%\.codex\.sandbox\setup_error.json`。如果里面是 `helper_sandbox_lock_failed ... SetNamedSecurityInfoW ... 5`（拒绝访问），openai/codex 里有人这样解决（[#45003](https://github.com/openai/codex/issues/45003)，这条回复有 19 人点赞）：完全退出 Codex，用管理员身份打开 PowerShell，删掉下面这个目录，再打开 Codex 重新走一遍设置。

   ```powershell
   Remove-Item "$env:USERPROFILE\.codex\.sandbox-bin" -Recurse -Force
   ```

   这是社区里的办法，不是官方步骤；日志里是别的错误就别用。
5. 看到 Windows 错误 `1385`：说明 Windows 策略不允许沙盒用户登录，要找 IT 处理（官方说明）。
6. 要发日志给官方，发 `%USERPROFILE%\.codex\.sandbox\sandbox.log`，**不要**发 `.sandbox-secrets` 目录里的东西。

系统要求：官方推荐 Windows 11；Windows 10 要 1809 或更新的版本，而且要有 `winget`。

### 更新后打不开、只有启动动画或白屏

这类问题大多出在新版本本身，而且往往很多人同时遇到，一般要等官方发修复版。先到上面的[最近两周的已知问题](#最近两周的已知问题)里看看有没有你这一版，再按下面的顺序排查。

**先排除一个最好改的原因：旧配置。** 不少老教程让你在 `~/.codex/config.toml`（Windows 是 `%USERPROFILE%\.codex\config.toml`）里写 `approval_policy = "untrusted"`。官方配置文档现在写明这个值已经不支持，新版本读到它会报：

```text
approval_policy = "untrusted" is no longer supported; remove this setting
```

桌面版里你可能只看到 `Unable to load organization settings`（中文界面是「无法加载组织设置」，下面一行「应用已暂停，等待安全加载组织设置后才能恢复。网络连接、代理或防火墙可能导致无法访问……」），或者一直转圈。[#48324](https://github.com/openai/codex/issues/48324) 里 10 月 15 日有用户就是删掉这一行后好的。打开 `config.toml`，删掉 `approval_policy = "untrusted"`；想让 Codex 跑命令前先问你，改成 `approval_policy = "on-request"`（`on-failure` 也已经不推荐了）。保存后完全退出再打开。项目设置里的 `trust_level = "untrusted"` 是另一回事，官方说仍然支持，不用删。

**Windows 上更新到 26.1021 之后才弹「无法加载组织设置」的，先查 `CODEX_CLI_PATH`。** 10 月 24 日起不少人在 26.1021.31345（商店包 26.1021.2667.0、26.1021.3705.0）上遇到：一打开就是这个框，点重试、退出登录都回到这一页，修复、重置、重装、把 `config.toml` 挪走都没用。三位用户查到同一个原因（[#56620](https://github.com/openai/codex/issues/56620#issuecomment-6382621764)、[#48324](https://github.com/openai/codex/issues/48324#issuecomment-6381489469)、[#57051](https://github.com/openai/codex/issues/57051#issuecomment-6416185957)，没有官方回复）：电脑里有一个用户级的环境变量 `CODEX_CLI_PATH`，指着早先自己装的、或者复制出来的旧 `codex.exe`（有人那份是 0.154.0）。桌面版启动时会优先用这个变量指的程序，旧程序和新版桌面版对不上，读组织设置这一步就失败了。日志（`%LOCALAPPDATA%\Packages\OpenAI.Codex_2p2nqsd0c76g0\LocalCache\Local\Codex\Logs`）里先是一行带 `source=override` 的，接着是 `Desktop startup policy read failed … phase=policy-read reason=protocol`。官方安装脚本不会设这个变量，多半是以前照着某个教程或工具设的。

1. PowerShell 里查一下：`[Environment]::GetEnvironmentVariable("CODEX_CLI_PATH", "User")`。有输出（一个 `codex.exe` 的路径）就是这一种；没输出就不是，往下看别的原因。
2. 完全退出 ChatGPT（托盘图标也退掉），删掉这个变量：`[Environment]::SetEnvironmentVariable("CODEX_CLI_PATH", $null, "User")`。也可以在「设置 → 系统 → 系统信息 → 高级系统设置 → 环境变量」里删。
3. 重启电脑，再打开 ChatGPT。报告的人都是这样恢复的，不用重装，也不丢数据。还有一位删掉变量后应用能打开了，浏览器控制还不正常，退出登录再登录一次才好（[#48324](https://github.com/openai/codex/issues/48324#issuecomment-6410725332)）。

后面[无法加载你的账户](#无法加载你的账户we-couldnt-load-your-account)第 3 步、[Windows 沙盒](#windows-沙盒setup-refresh-had-errors-os-error-32)最后一条说的也是这个变量。也有人删了变量照样打不开，日志里是 `bundled_executable_relocation_failed` 加 `EPERM`，这一种还没有确认可用的办法（[#56716](https://github.com/openai/codex/issues/56716)）。

**Windows 上 Agent 设成在 WSL 里运行的，是另一个原因。** 10 月 14 日桌面版自动更新内核之后，Agent 环境选了 WSL 的（配置里是 `runCodexInWindowsSubsystemForLinux = true`），一打开就弹 `Unable to load organization settings`，关不掉，26.1013.4894.0 上还是这样。到日志里搜 `bundled_executable_relocation_failed`，同一行带 `codex-resources\bwrap` 的就是这一种（[#54478](https://github.com/openai/codex/issues/54478)、[#54300](https://github.com/openai/codex/issues/54300)，没有官方回复）。几位用户来回切换试过：设回 Windows 就能打开，切回 WSL 又弹。弹窗挡着进不了设置，就改文件：完全退出应用，打开 `%USERPROFILE%\.codex\config.toml`，把 `[desktop]` 下面的 `runCodexInWindowsSubsystemForLinux` 改成 `false`，再打开。这段时间命令就在 Windows 本机跑。#54300 的报告人来回切换之后有一部分旧对话的记录对不上了，改之前可以先把 `%USERPROFILE%\.codex` 整个复制一份。

开着 WSL、走代理的，还有一种情况可以先排除（[LINUX DO](https://linux.do/t/topic/3041933) 上一位用户 10 月 18 日查到并解决的）：WSL 模式下，桌面版会在 WSL 里起一个后台服务，启动时要先向 OpenAI 读一次组织策略，日志（`%LOCALAPPDATA%\Packages\OpenAI.Codex_2p2nqsd0c76g0\LocalCache\Local\Codex\Logs`）里读失败是 `Desktop startup policy read failed … phase=policy-read`。这个服务用的是 WSL 里 `~/.codex/.env` 的代理，地址指向 Windows 那边的代理端口。他换了一个 Wi-Fi，Windows 把新网络当成「公用网络」，防火墙就把 WSL 发到代理端口的连接全拦了：WSL 里 `ping` 得通，`curl` 代理端口却一直超时。把这个网络改回「专用」就好了：设置 → 网络和 Internet → Wi-Fi → 点当前的网络 →「网络配置文件类型」选「专用网络」。换网络后才开始弹这个框的，先看这一条。

还不行，按顺序试：

1. 彻底退出再打开：Windows 在任务管理器里结束 ChatGPT（旧版叫 Codex）的进程，后台残留的 `codex.exe` 也一起结束（[#48333](https://github.com/openai/codex/issues/48333) 的报告人就是结束它之后才进去的）；Mac 按 Cmd + Q。**Windows 上每次自动更新后都打不开的，再看「详细信息」里有没有 `codex-windows-sandbox-service.exe`**，它是桌面版装的一个系统服务（服务名 `CodexSandboxService.OpenAI.Codex`），也结束掉再打开。一位用户说近一个月每次更新后都要这样做才进得去；他的 Windows 安装日志里，10 月 14 日到 21 日新版本好几次没装上，报错是 `0x80073CF9`（`apps need to be closed`，有程序没关）（[#56532](https://github.com/openai/codex/issues/56532)，10 月 23 日，没有官方回复）。
2. 重启电脑。
3. 检查更新。已知问题一般靠新版本修，Windows 可以在 Microsoft Store 里看有没有更新。
4. Windows：设置 → 应用 → 已安装的应用 → ChatGPT → 高级选项，先点「修复」（不删数据）；不行再点「重置」（会清掉应用自己的数据，要重新登录）。
5. 到 [openai/codex 的 issue](https://github.com/openai/codex/issues) 里搜你的版本号和现象，看是不是已知问题。

> issue 里常有人贴「临时办法」，比如设一个 `CODEX_CLI_PATH` 把桌面版指到别的 CLI 上、从第三方镜像降级。这些官方都没认可（上面说的是删掉一个旧的 `CODEX_CLI_PATH`，方向正好相反），[#40752](https://github.com/openai/codex/issues/40752) 里就有人反馈改完之后历史会话打不开。能等就等官方修。

### 无法加载工作空间设置（Couldn't load workspace settings）

```text
无法加载工作空间设置
请重试以继续聊天
```

英文界面是 `Couldn't load workspace settings` / `Retry to continue chatting`。新建对话时输入框上方出现这条提示，发送按钮是灰的，点「重试」没用；已有的对话还能接着发，同一个账号在网页版也正常。有时一起出现的还有「无法检查锁定模式」（`Couldn't check Lockdown mode`）。

10 月 7 日桌面版更新之后开始出现。openai/codex 里已经有二十多个 issue 在报，到 10 月 24 日都没有官方回复。26.1013.40934 上 macOS（10 月 17–19 日，实体机、虚拟机都有）和 Windows（10 月 20 日）都还有人遇到（[#51917](https://github.com/openai/codex/issues/51917)、[#51667](https://github.com/openai/codex/issues/51667)、[#52342](https://github.com/openai/codex/issues/52342)）。10 月 22 日的 26.1016.61520 有好有坏：一位 Mac 用户说更新后好了，关掉了 #52342；另一位 Mac 用户同一天在同一版上照样被拦，日志里是下面第一种（[#51917](https://github.com/openai/codex/issues/51917)）。所以先更新，更新完还报再往下看。日志里也不全是 Cloudflare 的 403：有人在 26.1007.71326 上是 HTTP 503，有人是 `devicecheck_registration_network_failed` 加 `ERR_CONNECTION_CLOSED`，还有人是 `432 Workspace routing is unavailable`（[#55489](https://github.com/openai/codex/issues/55489)）。

**不是你的工作空间或项目设置坏了。** 桌面版新建对话前，要先向 chatgpt.com 取一次账号设置（日志里是 `/settings/user`、`/accounts/check`），这一步失败就不让发消息。个人账号也会遇到，和有没有加入团队工作空间没关系。用户从日志里查到三种情况：

| 日志里的错误 | 出在哪 | 状态 |
| --- | --- | --- |
| `cloudflare_challenge`，HTTP 403 | Windows 居多，macOS 也有。这个请求被 Cloudflare 的人机验证拦下了；同一时间网页版能正常打开 | 没修（[#51917](https://github.com/openai/codex/issues/51917)、[#51691](https://github.com/openai/codex/issues/51691)、[#52790](https://github.com/openai/codex/issues/52790)、[#52874](https://github.com/openai/codex/issues/52874)） |
| `devicecheck_token_generation_failed` | macOS。应用向苹果的 DeviceCheck 要设备凭证失败。虚拟机里的 macOS 反馈最多，实体的 M1、M2 机器也有人遇到 | 没修（[#52001](https://github.com/openai/codex/issues/52001)、[#52342](https://github.com/openai/codex/issues/52342)、[#52470](https://github.com/openai/codex/issues/52470)、[#51667](https://github.com/openai/codex/issues/51667)） |
| 没有别的提示，就是发送按钮一直灰着 | 10 月 7 日的 26.1002.51308 | **已修**。这一版有[官方事故记录](https://status.openai.com/incidents/01M4ADHMMBK7K92NJFJ0DXRYJM)，当天发了修复版，更新就好 |

想确认是哪一种，到日志里搜 `cloudflare_challenge` 或 `devicecheck`：Mac 的日志在 `~/Library/Logs/com.openai.codex/`，Windows 在 `%LOCALAPPDATA%\Packages\OpenAI.Codex_2p2nqsd0c76g0\LocalCache\Local\Codex\Logs`。

下面的办法都来自 issue 里的用户，不是官方方案；几乎每一条都既有人说管用、也有人说没用。从省事的开始试：

1. **更新到最新版，完全退出再打开。** 10 月 7 日那一版的问题这样就能解决。[#52437](https://github.com/openai/codex/issues/52437) 里有人完全重启应用后恢复；[#51917](https://github.com/openai/codex/issues/51917) 里有人什么都没动，等了半小时自己好了。
2. **新对话先选一个项目。** [#51917](https://github.com/openai/codex/issues/51917) 里有人发现，被拦的只有「这台电脑、不选项目」的新对话，选一个已有的项目就能发。10 月 20 日一位 Windows 用户在 26.1013.40934 上选了项目照样发不出去，所以不是人人管用。
3. **退出登录再登录。** [#52790](https://github.com/openai/codex/issues/52790) 的报告人这样能恢复，但过一阵又会出现。同一个 issue 里 10 月 20 日另一位 Windows 用户的办法是：点「重试」，趁它在重试的那几秒里连着点发送，消息就发出去了（[他的回复](https://github.com/openai/codex/issues/52790#issuecomment-6287495626)，只有他一个人试过）。
4. **从旧对话里新开一个。** 打开一个还能发消息的旧对话，让 Codex 自己建：「用内置的 create_thread 工具，在这个项目里新建一个本地对话，先打个招呼。」Mac 上有用户确认可行（[#52470](https://github.com/openai/codex/issues/52470#issuecomment-6105237946)、[#52001](https://github.com/openai/codex/issues/52001)）。
5. **先用网页版或 CLI。** 发帖的人大多提到同一个账号在网页版正常，CLI 也不受影响（[#52342](https://github.com/openai/codex/issues/52342)）。
6. **退回上一个版本。** Mac 上有人退回 26.930.61225 后恢复（[#52001](https://github.com/openai/codex/issues/52001)），Windows 上有人退回 26.928 后能新建对话（[#51917](https://github.com/openai/codex/issues/51917)）。这只是权宜之计，官方修好后记得升回来。

走代理的用户多留意一点：`cloudflare_challenge` 说明 Cloudflare 对这次请求不放心，要先做人机验证。10 月 16 日有 Windows 用户对着日志发现，拦截正好是从他的出口 IP 变了那一刻开始的，后来什么都没改又自己好了（[#51917](https://github.com/openai/codex/issues/51917)）。所以开着代理的，别来回切节点，固定一个等一等。换节点本身不一定有用，[#52790](https://github.com/openai/codex/issues/52790)、[#51917](https://github.com/openai/codex/issues/51917) 里都有人换过节点和 IP，没解决。还有一位用户是在应用的内置浏览器里打开了一次 chatgpt.com，等验证页自己过去，再回来点「重试」后恢复的；他自己也说不确定是不是这一步起的作用（[#51691](https://github.com/openai/codex/issues/51691)）。

### 新对话发不出去：启动聊天时出错（Cannot serialize value）

```text
启动聊天时出错
Cannot serialize value: (couldn't stringify value)
```

英文界面是 `Error starting chat`。10 月 21 日桌面版自动更新到 26.1016.51333 之后，新建对话发第一条消息就报这一句，发一句「你好」也一样，Mac、Windows、Linux 都有。openai/codex 里报得最多的一条有 168 个 👍（[#55744](https://github.com/openai/codex/issues/55744)）。

几位用户对着日志查到范围（[#55827](https://github.com/openai/codex/issues/55827)、[#55744](https://github.com/openai/codex/issues/55744)）：只在输入框下面选了「工作树」、新建对话的时候出错，日志里是 `[Composer] submit failed`，带 `mode=worktree`；同一个项目选「本地模式」能正常开始，已经开着的旧对话也能接着发。请求还没发出去就在应用里失败了，所以和网络、额度、账号都没关系，换节点、重新登录、重装都不用。

**已修，更新应用就好。** 10 月 22 日 Mac 上的 26.1016.61520 推出后，几位用户确认好了，[#55744](https://github.com/openai/codex/issues/55744) 当天关闭；Windows 上也有人确认 61520 和之后的 26.1016.62342 正常。还在 51333 的：Mac 点顶部菜单 ChatGPT →「检查更新…」，Windows 到 Microsoft Store 里更新，然后完全退出再打开。一时更新不了，新对话先切到「本地模式」，等更新完再用工作树。

### 无法加载你的账户（We couldn't load your account）

```text
无法加载你的账户
请尝试重新加载，或退出登录后重新登录
```

英文界面是 `We couldn't load your account` / `Try reloading, or sign out and sign in again`，下面两个按钮是「重新加载」（Reload）和「退出登录」（Sign out）。登录成功以后窗口里只有这一页，点「重新加载」还是它；同一个账号在网页版往往是正常的。[#51548](https://github.com/openai/codex/issues/51548) 标题里写的 `Unable to load account` 是从韩文界面翻回英文的，说的是同一页。

9 月下旬起 openai/codex 里陆续有人报，Windows 和 macOS 都有。10 月 12 日本仓库 [Issue #1](https://github.com/Avarce/codex-china-guide/issues/1) 里有读者在 macOS 26.1007 上遇到：同一台电脑，老账号正常，新账号一登录就是这一页。这一节引用的 6 个 issue 都没有 OpenAI 的人回复。

**这一页是兜底提示，原因不止一种。** 桌面版登录后要先向 chatgpt.com 查一遍：这次登录对应哪个账号，这个账号能不能用。哪一步查不出来，显示的都是这一页（对过 macOS 26.1007.21159 的安装包）。所以先到日志里看卡在哪一步：

```bash
# macOS
grep -rh "chatgpt-account-lookup" ~/Library/Logs/com.openai.codex/ | sort | tail -n 5
```

Windows 的日志在 `%LOCALAPPDATA%\Packages\OpenAI.Codex_2p2nqsd0c76g0\LocalCache\Local\Codex\Logs`，搜同一个词。这一行不带 token、邮箱和账号 ID，可以直接贴出来问人。行首是 UTC 时间，加 8 小时是北京时间；找到出问题那次登录对应的一行，看 `failureType=`：

| `failureType=` | 意思 |
| --- | --- |
| `account_info_request_failed` | 取账号信息的请求失败了。[#52996](https://github.com/openai/codex/issues/52996)（Windows 26.1007）的日志是这一种，同时有一串 `Workspace routing is unavailable`、`status=432` |
| `accounts_request_failed` | 取账号列表的请求失败了。[#47969](https://github.com/openai/codex/issues/47969)（macOS）是这一种：`/accounts/check` 返回 500，同一个账号在网页版和 ChatGPT Classic 应用里都正常 |
| `account_info_account_id_missing` | 登录成功了，但没拿到这次登录对应的账号 ID |
| `account_info_token_unavailable` | 应用没拿到登录令牌 |
| `account_ordering_empty`、`current_account_entry_missing`、`current_account_mismatch` | 账号列表是空的，或者列表里找不到、对不上当前登录的账号 |
| `undefined`，同一行写着 `result=succeeded` | 账号查到了，卡在下一步：应用判断这个账号能不能用聊天和 Work。安装包里这一步不放行的情况有：应用里记着的当前账号和这次登录的不是同一个、没取到这个账号的套餐、账号已停用、这次登录无权访问该账号、工作空间设置取不到。这一行不写是哪一种。[Issue #1](https://github.com/Avarce/codex-china-guide/issues/1) 的读者就是这一行：这两天新注册的 Plus 账号，网页版正常，同一台 Mac 上老账号也正常，具体原因还在查 |

`failureType=` 看不出问题的，再看当天有哪些请求失败了。下面这条只输出接口路径和状态码，同样不带账号信息：

```bash
# macOS，统计今天（UTC 日期）失败的请求
grep -rh "sa_server_request_failed" ~/Library/Logs/com.openai.codex/ | grep "^$(date -u +%F)" | sed -nE 's/.*routePattern=([^ ]+) status=([^ ]+).*/\1 \2/p' | sort | uniq -c | sort -rn | head
```

正常使用时这里也会有零星几条 500，不用管。要看的是 `/accounts/check/{version}`、`/settings/user`、`/me` 这几个取账号信息的接口有没有成片失败，以及状态码是不是 401、403、432。

查得最细的一份报告在 [#52112](https://github.com/openai/codex/issues/52112) 和 [#48571](https://github.com/openai/codex/issues/48571)（macOS 26.1002.52244）：报告人解开 `~/.codex/auth.json` 里的令牌，发现里面没有 `chatgpt_account_id`，退出重新登录也拿不到。桌面版因此算不出请求该发到哪里，取账号信息的请求全部在本地失败，日志里是 `Workspace routing is unavailable`、状态码 432。同一个账号、同一台电脑，CLI 是正常的。#52112 在 10 月 10 日被关闭，没有说明修没修。

下面都是 issue 里用户的做法，不是官方方案。按顺序试：

1. **换一个账号登录试试。** 别的账号在同一台电脑上正常，就说明问题不在电脑、网络或安装上，重装、清缓存、换节点可以省了。[#51548](https://github.com/openai/codex/issues/51548) 的报告人重装、重置应用、换网络、重建 `auth.json`、用 CLI 重新登录都试过，没有用。
2. **在浏览器里用这个账号登录 chatgpt.com。** 网页版正常，就是桌面版这一侧的问题，接着往下看。网页版也登不上，或者提示账号被停用，那是账号本身的事，要找 OpenAI 客服。
3. **看有没有设过 `CODEX_CLI_PATH`。** 终端里跑 `echo $CODEX_CLI_PATH`（Mac 再跑一次 `launchctl getenv CODEX_CLI_PATH`；Windows PowerShell 是 `$env:CODEX_CLI_PATH`）。有输出，说明桌面版被指到了你自己装的那份 CLI 上，去掉这个环境变量再重启应用。[#46382](https://github.com/openai/codex/issues/46382) 里两位 Windows 用户去掉之后 432 就没了；同一个帖子里也有人去掉之后没变化。
4. **更新到最新版，完全退出再打开，退出登录再登录。** 省事，但别抱太大希望：#47969 的报告人更新后好了，三天后又坏了。
5. **先用网页版或 CLI。** #52112 里同一个账号的 CLI 正常，#51548 里 CLI 也能登录成功。
6. **别急着把 `CODEX_CLI_PATH` 指向旧版。** #48571 里有人把它指到 0.154.0 之后账号能加载了，但用量、额度这些功能都没了。

## 6. 报错逐条处理

### 按状态码查：400、403、404、429、500、502、503

报错里只看得到一个数字的，先按这张表对一下。同一个数字在不同情况下意思不一样，以冒号后面的那句话为准。

| 状态码 | 在 Codex 里多半是 | 看这里 |
| --- | --- | --- |
| `400 Bad Request` | 后面跟着 `model is not supported when using Codex with a ChatGPT account`：模型名、旧设置或套餐的问题。只有 `{"detail":"Bad Request"}`，而且只在某一个很长的旧会话里出现、别的会话正常：这个会话的历史太大（图片多），新开一个会话 | [模型不支持](#模型不支持model-is-not-supported)、[断线重连](#断线重连stream-disconnected-before-completion) |
| `401 Unauthorized` | 本机登录失效；带 `Incorrect API key provided: sk-svcac…` 的是官方故障 | [401](#登录失效或官方故障401-unauthorized) |
| `402 Payment Required` | 用 ChatGPT 账号登录的：保存的登录还指着一个已经停用的工作空间，日志里是 `{"detail":{"code":"deactivated_workspace"}}`，常见于 Team、Business 席位到期或被移出。Windows 桌面版可能一直停在启动画面。退出登录再登录，选个人账号或还在用的工作空间；桌面版进不去，就完全退出，把 `%USERPROFILE%\.codex\auth.json` 改名成 `auth.json.bak` 再打开，它会让你重新登录（[#19075](https://github.com/openai/codex/issues/19075)，好几位用户确认管用，没有官方修复）。接了中转站的，多半是余额用完 | [接第三方时的报错](#接第三方时的报错) |
| `403 Forbidden` | 带 `unsupported_country_region_territory`：节点所在地区不支持。日志里是 `cloudflare_challenge`：请求被 Cloudflare 的人机验证拦下了，桌面版会表现为「无法加载工作空间设置」 | [地区不支持](#地区不支持unsupported_country_region_territory)、[无法加载工作空间设置](#无法加载工作空间设置couldnt-load-workspace-settings) |
| `404 Not Found` | 接 DeepSeek 或第三方 API 时地址填错，或者对方不支持 Responses 协议 | [接第三方时的报错](#接第三方时的报错) |
| `429 Too Many Requests` | 官方故障，或者额度快用完、同时开的会话太多 | [429](#请求太多429-too-many-requests) |
| `500`、`502` | 用 ChatGPT 账号登录的，多半是 OpenAI 服务器那边出错；接了第三方 API 或中转站的，多半是对方的服务器或线路有问题 | [服务器处理失败](#服务器处理失败an-error-occurred-while-processing-your-request)、[接第三方时的报错](#接第三方时的报错) |
| `503 Service Unavailable` | 服务器满载，常和 `Selected model is at capacity` 一起出现 | [模型满载](#模型满载selected-model-is-at-capacity) |

### VS Code 插件打不开：Codex could not start

```text
Codex could not start
The extension could not start its user interface.
```

北京时间 10 月 24 日早上，VS Code 里的 Codex 插件（发布者 OpenAI、ID `openai.chatgpt`）自动更新到 26.1021.22027（预览版是 26.51021.22027）之后，很多人一打开侧边栏就只有上面两行，或者一直转圈、灰着。插件的日志里是这样一行：

```text
[CodexWebviewProvider] Webview renderer did not become ready elapsedMs=30438 extensionVersion=26.1021.22027 reason=renderer_ready_timeout receivedWebviewMessage=false role=sidebar stage=renderer timeoutMs=30000
```

意思是插件的后台已经启动了（前面有 `Initialize received`），侧边栏的界面却在 30 秒内没加载出来，插件就放弃了，换成这一页。Windows、Linux、Intel Mac 都有，本机、Remote-SSH、WSL 都有；报告的人大多同时用着 VS Code 1.143.1。10 月 24 日以来 openai/codex 里至少 16 个 issue 在报（[#56655](https://github.com/openai/codex/issues/56655)、[#56691](https://github.com/openai/codex/issues/56691)、[#56660](https://github.com/openai/codex/issues/56660)、[#56617](https://github.com/openai/codex/issues/56617)），没有官方回复，到 26 日插件市场也还没有新版本。

下面都是 issue 里用户的办法，不是官方方案。按顺序试：

1. **把插件退回 26.1007.21434。** 十几位用户确认这样就好了，Windows、Linux、Intel Mac 都有（[#56655](https://github.com/openai/codex/issues/56655#issuecomment-6383612539)、[#56691](https://github.com/openai/codex/issues/56691#issuecomment-6399118283)）；也有一两位退回后还是不行。在扩展面板里右键 Codex 插件，选「安装特定版本...」（`Install Another Version...`），挑 26.1007.21434；或者在终端里运行：

   ```bash
   code --install-extension openai.chatgpt@26.1007.21434 --force
   ```

   装好后重新加载窗口。**然后再右键这个插件，把「自动更新」的勾去掉**，不然它过一阵又会升回 26.1021.22027；官方出了修好的版本再打开。连着 Remote-SSH、WSL 的，远程那一侧装的插件也要这样退回。
2. **还不行，把 VS Code 退回 1.142.0。** 有三位用户这样好了（[#56691](https://github.com/openai/codex/issues/56691#issuecomment-6404564734)、[#56691](https://github.com/openai/codex/issues/56691#issuecomment-6410921840)、[#56655](https://github.com/openai/codex/issues/56655#issuecomment-6387613635)）。
3. **不想降级，可以先试一下挪位置。** 右键侧边栏上的 Codex 标签，选「移动到」→「面板」或「主侧边栏」，它会重新加载一次界面；之后挪回原处也能用（[#56655](https://github.com/openai/codex/issues/56655#issuecomment-6409931120)，两位用户确认）。
4. 急着用，先用桌面版或 CLI，同一个账号、同一份额度。

### 断线重连：stream disconnected before completion

```text
stream disconnected before completion: error sending request for url (https://chatgpt.com/backend-api/codex/responses)
Reconnecting... 2/5
```

中文界面显示「正在重新连接 1/5」，一路数到「5/5」后失败；也可能是「服务器繁忙，正在重新连接」。Codex 默认重试 5 次，都失败才把报错显示出来。

CLI 里还有一种是 `Reconnecting... waiting for network`，后面没有 1/5 这样的计数，会一直等下去。这是连接根本没建起来、请求发不出去时的样子：新版本不再数到 5 次就报错，而是隔一阵重试一次，间隔越拉越长，最长一分钟一次（对照 0.164.0 源码的 `responses_retry.rs`，这个行为默认开着）。网页能打开、Codex 却一直这样的，多半是终端没走代理，按下面的顺序查。

`stream disconnected before completion` 只是统称，意思是这一轮回复没能完成。**原因看冒号后面那半句**（对照 Codex 0.162.1 的源码和 issue 里的日志）：

| 冒号后面 | 是什么 | 怎么办 |
| --- | --- | --- |
| `error sending request for url (…)` | 请求没能发出去，多半是网络或代理 | 按下面的顺序试 |
| `An error occurred while processing your request. You can retry your request, or contact us through our help center at help.openai.com …`，多半带一个 request ID | OpenAI 服务器返回的错误。这句话不在 Codex 的代码里，是服务器发回来的 | 换节点没用，见[服务器处理失败](#服务器处理失败an-error-occurred-while-processing-your-request) |
| `stream closed before response.completed`、`websocket closed by server before response.completed` | 连接中途被断开，网络、代理、服务器都有可能 | 先按下面的顺序试；只有某个旧会话这样的，看本节最后一段 |
| `idle timeout waiting for SSE`、`idle timeout waiting for websocket` | 等了太久没收到数据 | 同上 |

**后半句是 `error sending request` 的，多半是网络问题。** openai/codex 的维护者排查过一批这类报告，结论是大多是网络连通性问题：网络不稳、VPN、代理、防火墙；也有网络把 WebSocket 挡了的（[#14209](https://github.com/openai/codex/issues/14209)、[#13245](https://github.com/openai/codex/issues/13245)）。

按顺序试：

1. 看 [status.openai.com](https://status.openai.com)。有故障记录就是官方的问题，等修好。
2. 终端里跑 `codex doctor`，`websocket` 不是 connected，就是网络没通。
3. 按[第 4 节](#4-国内网络终端要单独设代理)设好代理，或打开 TUN 模式。
4. 换一个稳定的节点，别用很多人共用、经常断的线路。
5. Mac 合盖睡眠醒来后才出现的，重开会话或重启 Codex。这个问题官方 issue [#3355](https://github.com/openai/codex/issues/3355) 还开着。
6. 还不行，在 Codex 里输入 `/feedback` 上传日志，拿到的会话 ID 可以贴进 GitHub issue。

**只有某一个旧会话一直重连，新开的会话没事？** 可能是这个会话的历史太大了。[LINUX DO](https://linux.do/t/topic/2832147) 上有人排查过：报错是 `failed to send websocket request: IO error … (os error 10054)` 或 `10053`，换机场、换节点、开 TUN 都没用；最后发现那个旧会话里攒了很多图片的工具输出，一次请求有 46 MB。

- 别在旧会话里硬撑，也别 Fork（Fork 会把历史一起带过去）。新开一个会话，只把旧会话的文字结论贴过去：目标、做完了什么、改了哪些文件、下一步。
- 平时少让它直接看原始大图，先缩小或裁剪；可以把这条写进 `AGENTS.md`。

### 服务器处理失败：An error occurred while processing your request

```text
stream disconnected before completion: An error occurred while processing your request. You can retry your request, or contact us through our help center at help.openai.com if the error persists. Please include the request ID xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx in your message.
```

中文界面先是「正在重新连接 1/5」数到「5/5」，然后显示上面这段。这段是服务器发回来的原文，中文界面里也是英文。压缩上下文时出现的写法是：

```text
Error running remote compact task: stream disconnected before completion: An error occurred while processing your request. …
```

日志里还可能是 `unexpected status 503 Service Unavailable: An error occurred while processing your request.`（[#52380](https://github.com/openai/codex/issues/52380)）。

**这不是你的网络问题。** 冒号后面这句话不在 Codex 的代码里（查过 0.162.1 的源码），是 OpenAI 服务器发回来的，request ID 也是服务器给这次请求的编号。也就是说，请求已经到了 OpenAI，是那边处理失败。几位用户翻过日志：服务器先返回 HTTP 200，1–3 秒后报这个错；会话日志里记的是 `codex_error_info: "other"`，不是满载时的 `server_overloaded`（[#52814](https://github.com/openai/codex/issues/52814)、[#53127](https://github.com/openai/codex/issues/53127)）。

**10 月 9 日起明显变多。** openai/codex 里提到这句原文的 issue，9 月 1 日到 10 月 8 日有 23 个，10 月 9–12 日四天新开了 10 个（10 月 12 日上午统计）。这 10 个都开着，没有官方回复，状态页这几天也没有对应的事故记录。Windows 和 macOS 桌面版 26.1007、VS Code 插件、CLI 0.162 都有；Plus、Pro 都有人遇到。

用户报告里反复出现的几点，都不是官方结论：

- **跟着会话走，不跟着账号和网络走。** 一位用户同一个账号同时开着 5 个长会话：70 分钟里，2 个发了 140 次请求没有一次失败，另外 3 个失败了 60%–75%，而且出问题的是哪几个，一天里还会变（[#52814](https://github.com/openai/codex/issues/52814) 的回复）。也有人同一台电脑、同一个网络，一个会话每条都失败，别的会话正常（[#53249](https://github.com/openai/codex/issues/53249)、[#53167](https://github.com/openai/codex/issues/53167)）。
- **出问题的多是用了很久、图片多的会话。** 有人量过失败的那次请求：36 MB，其中 16 张图片占了 35 MB（[#52731](https://github.com/openai/codex/issues/52731) 的回复）。另一位的两个会话，请求体一个是 18.8 MB，一个压缩后还有 39–54 MB（[#53127](https://github.com/openai/codex/issues/53127)）。
- **压缩也会失败**，就是上面 `Error running remote compact task` 那句（[#52919](https://github.com/openai/codex/issues/52919)、[#53203](https://github.com/openai/codex/issues/53203)）。

怎么办。下面都是用户试出来的，不保证有效：

1. **先别换节点、重装、重新登录。** issue 里试过没用的：重启电脑两次（[#52919](https://github.com/openai/codex/issues/52919)）、换手机热点（[#52731](https://github.com/openai/codex/issues/52731)）、更新应用（[#53127](https://github.com/openai/codex/issues/53127)）、调低推理强度（[#52814](https://github.com/openai/codex/issues/52814)）。
2. **新开一个会话，发一句很短的话。** 能正常回复，说明账号和网络没问题，问题在旧会话。
3. **旧会话先试一次 `/compact`。** 有人压缩成功后，同一个会话的下一条就正常了，历史里的图片数据从 35 MB 降到 0.25 MB（[#52731](https://github.com/openai/codex/issues/52731) 的回复，10 月 11 日）。他自己也说这只是恢复了一次，不能证明以后不再出现。压缩本身也报错的，别反复试，走下一步。
4. **换新会话接着做。** 只把文字结论贴过去：目标、做完了什么、改了哪些文件、下一步。别 Fork，Fork 会把历史一起带过去。新会话用久了、图片攒多了，断线还会再来（[#51889](https://github.com/openai/codex/issues/51889) 的回复），少让它一次看很多截图或整页的渲染图。
5. **记下 request ID 和出错时间。** 在 Codex 里输入 `/feedback` 上传日志，或者带着 request ID 到 [help.openai.com](https://help.openai.com) 找客服。这是报错里官方自己写的途径，客服要靠这个编号查。
6. **新会话里连一句短话都失败的，不是上面这种情况。** 先看[模型满载](#模型满载selected-model-is-at-capacity)，到会话日志里搜 `server_overloaded`。9 月还有一位用户统计过：同一台电脑、同一个网络，换一个账号，失败率差了几倍（[#45925](https://github.com/openai/codex/issues/45925)，没有官方回复）。

### 一直正在思考、卡住不动：Thinking

桌面版一直显示「正在思考」（Thinking），或者「已处理 …」（Working for …）后面的时间一直往上跳；CLI 停在这一行：

```text
Working (12m 30s • esc to interrupt)
```

「卡住」有好几种，处理办法不一样，先对一下是哪一种：

| 你看到的 | 多半是 | 怎么办 |
| --- | --- | --- |
| 侧边栏里这个聊天写着「等待批准」（Awaiting approval），或者对话里有一张等你点的卡片 | 它在等你批准一条命令，没有卡住 | 点进去批准或拒绝。官方排查「卡住」的第一步就是看它是不是在等批准 |
| 最后一步是「命令已运行 …」（Running command for …），时间一直在涨 | 那条命令自己没退出：`npm run dev` 这类开发服务器、会一直跑下去的进程，或者在等你输入的脚本 | 停掉这一轮，让它把这类命令放到后台跑，或者加上超时。CLI 0.163.0 修了其中一种：命令本身已经结束，它带起的子进程还占着输出，这一轮就一直不结束（[#52652](https://github.com/openai/codex/pull/52652)） |
| 时间在涨，隔一阵就多出一步 | 任务大或者推理强度高，模型就是在想 | 可以等，也可以停下来把任务拆小。有 Plus 用户说，同类任务在 GPT-6 Sol 上要 40 分钟以上（[#47656](https://github.com/openai/codex/issues/47656)，用户报告） |
| 很久没有任何新内容，最后跳成「正在重新连接 1/5」 | 连接断了 | 见[断线重连](#断线重连stream-disconnected-before-completion)；终端里跑 `codex doctor`，看 `websocket` 那一行 |
| 结果已经出来了，界面还显示在跑，下一条发不出去或者一直排队 | 界面状态没更新，这一轮其实已经结束 | VS Code 插件：命令面板运行 `Developer: Restart Extension Host` 或 `Developer: Reload Window`（[#50084](https://github.com/openai/codex/issues/50084)，用户报告）。桌面版：等别的聊天跑完，重启应用 |

处理顺序。前三步是官方 [Troubleshooting](https://learn.chatgpt.com/docs/reference/troubleshooting) 写的：

1. 看它是不是在等你批准。
2. 打开内置终端，跑一个简单命令，比如 `git status`，看终端本身有没有卡住。
3. 停掉这一轮（桌面版点「停止」，CLI 按 Esc），新开一个聊天，用范围更小、更具体的要求重来。
4. 停不下来的：10 月 14 日有 Windows 用户报告，一个聊天显示 `Working for 5h 23m 7s`，却找不到停止按钮（[#53838](https://github.com/openai/codex/issues/53838)，没有官方回复）。这时只能退出应用再打开。

### 登录后跳到 localhost 打不开：ERR_EMPTY_RESPONSE

```text
该网页无法正常运作
localhost 未发送任何数据。
ERR_EMPTY_RESPONSE

无法访问此网站
localhost 拒绝了我们的连接请求。
ERR_CONNECTION_REFUSED
```

英文浏览器里是 `This page isn’t working` / `localhost didn’t send any data.`，或 `This site can’t be reached` / `localhost refused to connect.`。CLI 0.158.0（9 月 28 日）起，回调地址里的 `localhost` 换成了 `127.0.0.1`，报错里的 `localhost` 也跟着变成 `127.0.0.1`，是同一回事。10 月 13 日查到的情况：Cursor 的插件市场给的还是 9 月 11 日的 26.5908.31748，自带 CLI 0.154，所以 Cursor 里报错写的是 `localhost`；VS Code 市场的 26.51007.21434 自带 0.162，写的是 `127.0.0.1`。

邮箱、验证码、选工作空间都过了，点最后一个「继续」，浏览器跳到 `localhost:1455`（新版是 `127.0.0.1:1455`），出现上面这一页。**账号这一段没有问题**，卡的是最后一步：「通过 ChatGPT 登录」登录完，浏览器要把凭证送到 `http://localhost:1455/auth/callback`，由正在等登录的那个 Codex 接收。`localhost` 和 `127.0.0.1` 都指浏览器所在的这台电脑。Codex 不在这台电脑上，或者 1455 端口上是别的程序，就接不到。

先认自己是哪种情况：

| 情况 | 怎么认 | 怎么办 |
| --- | --- | --- |
| 编辑器连着远程：Remote SSH、WSL、容器、Codespaces | 窗口标题里带 `[SSH: 主机名]` 这样的字样，左下角状态栏也会显示连着哪台主机 | 插件和 Codex 跑在远程那台机器上，等回调的也是它，浏览器却在你自己的电脑上（[#4265](https://github.com/openai/codex/issues/4265)、[#6403](https://github.com/openai/codex/issues/6403)）。用下面的设备码登录，这是官方文档给远程环境的首选办法 |
| 这台电脑的 1455 端口上是别的程序 | 同时开着桌面版、另一个 VS Code 或 Cursor 窗口、终端里的 `codex` | 把别的都关掉再登录。[#22048](https://github.com/openai/codex/issues/22048) 的发帖人是连着远程登录时，自己电脑的 1455 被另一个 VS Code 窗口或 Codex 占着，回调被别的进程收走，或者直接失败。旧版本这时命令行报 `Port 127.0.0.1:1455 is already in use`，[#3927](https://github.com/openai/codex/issues/3927) 里三位用户关掉开着 Codex 的 VS Code 或 Cursor 后解决。现在的版本会先请占着端口的那个 Codex 登录退出，不行就改用备用端口 1457 |
| Windows 上插件设成了在 WSL 里运行 | 设置里 `chatgpt.runCodexInWindowsSubsystemForLinux` 是开的（现在默认是关的）；浏览器等很久后显示 `localhost took too long to respond`（localhost 的响应时间过长） | [#12263](https://github.com/openai/codex/issues/12263) 发帖人的可用配置：把它和 `remote.openBrowserInRemote` 都设成 `false`，重启 VS Code |

| Windows 上一点登录就报 `failed to start login server`，后面是 `os error 10013`（以一种访问权限不允许的方式做了一个访问套接字的尝试），或者 `Port 127.0.0.1:1457 is already in use`，可查下来 1455、1457 上根本没有程序 | 管理员 PowerShell 里跑 `netsh interface ipv4 show excludedportrange protocol=tcp`，看 1455、1457 是不是落在某一段里（[#40344](https://github.com/openai/codex/issues/40344) 是 1447–1546，[#42994](https://github.com/openai/codex/issues/42994) 是 1438–1537） | Windows 把这段端口留给了 Hyper-V、WSL 2 或 Docker（Windows 自己的 WinNAT 服务），Codex 绑不上。最省事是用下面的设备码登录。想照常登录：管理员 PowerShell 里先 `net stop winnat`，登录完再 `net start winnat`（#42994 发帖人的做法）。停掉的那一会儿，WSL 和 Docker 连不了网 |

查 1455 被谁占着：macOS、Linux 用 `lsof -i :1455`，Windows 用 `netstat -ano | findstr :1455`。

两种报错的区别：`ERR_CONNECTION_REFUSED` 是连接被拒绝，通常是这台电脑的 1455 端口上没有程序在等；`ERR_EMPTY_RESPONSE` 是对方接了连接，没发任何数据就断开了。后一种在用户报告里出现在走 SSH 隧道的时候（[#2798](https://github.com/openai/codex/issues/2798) 的回复，没有官方解释）。两种的处理办法一样。

**用设备码登录，不经过 localhost：**

1. 浏览器打开 `chatgpt.com/settings/security`，往下找到 `Enable device code sign-in for Codex, Excel, PowerPoint, and Word`（中文界面：为 Codex、Excel、PowerPoint 和 Word 启用设备代码登录），打开。要在你用来登录 Codex 的那个账号里开；Business 这类工作空间由管理员在工作空间权限里开。
2. VS Code、Cursor 插件：登录页点 `More options`（更多登录选项）→ `Use device code`（使用设备码）。命令行：`codex login --device-auth`。
3. 出现一个对话框，写着 `To use a device code to log in, click the open browser button and paste the code below.`（如需使用设备码登录，请点击“打开浏览器”按钮，并粘贴下方验证码。），下面是 `Device code`（设备码）和一串代码。插件会自己打开浏览器，没打开就点 `Open browser`（打开浏览器）。
4. **浏览器先出现登录页是正常的，不是又失败了。** 命令行里的原话是先登录、再输入代码。登录有套餐的那个账号，然后把代码粘贴进去。代码 15 分钟内有效。
5. 回到编辑器或终端，等它自己变成已登录。

这几步里容易卡住的地方。我们帮人处理过一例，是 Cursor 连着 SSH 远程，前两条都遇到了：

- **Cursor 弹出 `Do you want Cursor to open the external website?`**（装了中文语言包是「是否要 Cursor 打开外部网站?」）。这是编辑器的链接保护，不是报错：Cursor 默认信任的域名里没有 `auth.openai.com`。点 `Open`（打开）就行。点 `Configure Trusted Domains`（配置受信任的域），再选 `Trust https://auth.openai.com`（信任 …），以后不再问，这一次也会打开。点 `Copy`（复制）或直接关掉对话框，网页不会打开。VS Code 1.140 默认信任这个域名，不会弹。
- **设备码登录本身不经过 localhost。** 我们处理的那一例里，点了 `Open browser`、登录之后，浏览器还是跳到了上面那一页，原因没有查实。可以这样试：关掉之前登录留下的标签页，回编辑器点 `Cancel`（取消）重新取一次代码，然后在浏览器地址栏直接打开 `https://auth.openai.com/codex/device`，登录后粘贴代码。还是跳走，就换无痕窗口打开这个地址。
- 代码过期了，点 `Cancel`（取消）再选一次设备码，会换一串新的。
- 设备码是 Codex 所在的那台机器去申请的。远程服务器在国内的，服务器自己也要能连上 OpenAI（见[第 4 节](#4-国内网络终端要单独设代理)）；[#8158](https://github.com/openai/codex/issues/8158) 里有人在远程服务器上得到的是 `device code request failed with status 403 Forbidden`。
- **代码别发给任何人。** 那个开关下面写着设备码可能被钓鱼；命令行里的提示是：这次登录不是你自己在 Codex 里发起的，就取消。

想在远程环境里照常用浏览器登录的，还有两个办法，都是官方文档列的备用办法：

- 转发端口，前提是你自己电脑上的 1455 没被占。VS Code 连着远程时，命令面板里选 `Forward a Port`，填 `1455`，再登录（[讨论 #4650](https://github.com/openai/codex/discussions/4650) 里一位用户确认可行）。用命令行的，官方的写法是先在自己电脑上运行 `ssh -L 1455:localhost:1455 <用户>@<主机>`，再在这个 SSH 会话里运行 `codex login`。
- 在有浏览器的电脑上登录好，把 `~/.codex/auth.json` 复制到远程机器的同一位置。这个文件等于账号钥匙。#8158 里的反馈不一：两位用户把 Windows 上的 `auth.json` 复制进 WSL 后正常；另两位复制到远程服务器后能登录，但提问之后一直停在思考。

这一节的依据（10 月 13 日核对）：浏览器报错的中英文取自 Chromium 源码；回调地址、备用端口 1457、设备码页面的地址和 15 分钟有效期，取自 openai/codex `rust-v0.162.1` 的 `codex-rs/login/src/server.rs` 和 `device_code_auth.rs`；插件登录页和设备码对话框的中英文、选了设备码后会自己打开浏览器，取自 VS Code 插件 26.51007.21434 的安装包，Cursor 市场上的 26.5908.31748 逐字相同；链接保护对话框的原文、各按钮的行为和默认信任的域名，取自 Cursor 3.24.12 和 VS Code 1.140 的安装包，中文取自 VS Code 简体中文语言包；远程环境用设备码、转发端口、复制 `auth.json` 三条见官方的 [Authentication](https://learn.chatgpt.com/docs/auth)。我们没有自己在远程环境里复现这两种报错。

### 登录失败：Token exchange failed

```text
Token exchange failed: error sending request for url (https://auth.openai.com/oauth/token)
Token exchange failed: token endpoint returned status 403 Forbidden
```

桌面版中文界面里是「登录失败：」后面跟着这句英文。浏览器那边登录成功了，Codex 拿凭证时连不上 OpenAI。维护者的判断是，剩下的这类情况大多和网络代理、VPN 有关（[#2414](https://github.com/openai/codex/issues/2414)）。

浏览器里看到的是一页 `Sign-in could not be completed`：这是 Codex 自己的登录回调页，原因写在下面那行，是 `Token exchange failed` 的就按这一节处理。只显示 `State mismatch` 的，是浏览器把一次旧的登录结果送了回来（比如点了之前留下的登录标签页）：关掉所有登录相关的标签页，重新运行一次 `codex login`，只用这一次打开的页面登录（对照 0.163.0 的 `codex-rs/login/src/server.rs`）。

1. 按[第 4 节](#4-国内网络终端要单独设代理)设代理或开 TUN，再 `codex login`。
2. 报 403 的，多半是出口节点的问题，换一个节点再试。
3. 还不行，改用设备码登录：`codex login --device-auth`（先在网页版安全设置里打开）。

### 登录过期：refresh token was already used

```text
Your access token could not be refreshed because your refresh token was already used. Please log out and sign in again.
```

中文界面里类似的提示是「此设备上的 ChatGPT 会话已过期。请重新登录后重试。」

登录凭证会自动续期，每次续期旧的就作废。维护者提到的一个常见原因是后台还开着一个旧版本的 Codex（CLI 或编辑器插件），它拿旧凭证去续期（[#9634](https://github.com/openai/codex/issues/9634)）。把同一份 `auth.json` 拷到几台电脑上用，也会互相顶掉。

1. 关掉所有 Codex 窗口、终端和编辑器，把 CLI、插件、桌面版都升级到最新。
2. `codex logout`，再 `codex login`。
3. 每台电脑各自登录，别共用同一份 `auth.json`。

### 地区不支持：unsupported_country_region_territory

```text
"code": "unsupported_country_region_territory",
"message": "Country, region, or territory not supported"
```

中文界面：「我们的服务在你所在的国家或地区不可用」或「我们无法确定你所在的国家/地区。请更换网络后重试」。

OpenAI 根据你的出口 IP 判断地区。中国大陆和香港都不在支持名单里，节点落在这两个地区就会这样。换到支持地区的固定节点；终端连不上的，检查终端是不是根本没走代理（[第 4 节](#4-国内网络终端要单独设代理)）。

### 模型满载：Selected model is at capacity

```text
Selected model is at capacity. Please try a different model.
```

同一句报错有两种原因：服务器真的满载（很多人同时遇到，等就行），或者**你的账号被临时限制**（只有你遇到，换模型、重试都没用；有人管这叫「被风控」「进小黑屋」）。OpenAI 9 月 14 日在官方社区确认，即使订阅有效，也可能因账号活动被临时限制部分模型，系统会自动重新评估。10 月 24 日 [LINUX DO](https://linux.do/t/topic/3068156) 上一位 Plus 用户贴出了 OpenAI 客服给他的回复，说法一样：`Access to certain models or features can remain temporarily limited while automated checks are completed`，也就是自动检查做完之前，部分模型或功能可能一直受限；客服不透露检查的细节，只能等系统重新评估（一位用户的邮件，仅供参考）。

最快的判断：看 [status.openai.com](https://status.openai.com)，再在同一台电脑上换一个账号试。别人的账号正常、你的不行，多半是账号被限。也有例外：维护者 9 月解释过一例，两个会话可能被分到了不同地区的机房，一个满载、一个不满（[#46103](https://github.com/openai/codex/issues/46103)）。这时别一直点重试，也别急着升级或开新号。完整判断表和处理步骤：[Selected model is at capacity 怎么办](https://aonir.com/guides/codex-selected-model-at-capacity/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_capacity)。

**之前的几波**（日期是北京时间，issue 里都没有官方回复）：

- **10 月 9–10 日**：发帖的人多在东八区，桌面版、CLI、网页版都有。一位用户翻了自己的会话日志，两天里八成多的对话失败，每条都是 `server_overloaded`，周额度才用了 7%，出问题的都在白天（[#52796](https://github.com/openai/codex/issues/52796)）。状态页这两天没有记录，所以**状态页一切正常，不代表不是服务器的问题**。
- **10 月 12–14 日**：每天又新开十几个，有人周额度还剩 94%、7 个模型全报这一句（[#53559](https://github.com/openai/codex/issues/53559)），换模型不太管用。13 日、14 日状态页各记了一次 Codex 性能下降（[13 日](https://status.openai.com/incidents/01M4T4ZCM54D3ZPTQXRSWFG0XS)、[14 日](https://status.openai.com/incidents/01M4WKPE1EY48ZB4RZJ80YH7ES)）。
- **10 月 15 日**：Tibo 说上了一批改动、加了算力，真碰上了 Codex 也会过一会儿自己接着跑（[原帖](https://x.com/thsottiaux/status/2110750421367287996)）。之后每天新开的相关 issue 从 13 日的 18 个降到 17 日的 0 个。
- **10 月 19–23 日**：又一波，每天新开 8–29 个（按标题或正文里有这句原文统计，UTC 日期），Tibo 没再提。报告的人周额度都还有很多，所有模型都报；有人自己的账号换到同事电脑上照样报，同事的账号却正常（[#55294](https://github.com/openai/codex/issues/55294)）；还有一位 Plus 用户发现，只有 `codex resume` 接着的旧会话一直报，新开的会话能用（[#55444](https://github.com/openai/codex/issues/55444)）。所以先**新开一个会话**试一句短话。

这一波里状态页只记了两段：北京时间 21 日 10:26–10:48 ChatGPT、Codex 和 API 性能下降；22 日 10:18–16:50 印度、日本、韩国的用户 ChatGPT 对话和 Responses API 报错增多，用日本、韩国节点的那段时间可能正好碰上。两条值得照着试的报告：

- **桌面版不行，先试 CLI。** 一位 Mac 上的 Pro 用户 10 月 23 日对照过：同一个账号、同一个模型、同一个网络，同一个时段里桌面版 21 轮全报这一句，CLI 大约七成能过；换美国、新加坡、日本、韩国等六个出口都一样（[#55260](https://github.com/openai/codex/issues/55260)，单个用户的数据）。
- **原始返回是 `server_is_overloaded`。** [#56346](https://github.com/openai/codex/issues/56346) 的报告人贴出了原文：HTTP 状态码是 200，错误写在返回的内容里，桌面版再把它显示成「Selected model is at capacity」。跟帖的一位 Windows 上的 Plus 用户统计，自己 10 月 21–23 日九成以上的请求都这样失败。请求已经到了 OpenAI、被那边拒了，所以换节点、重装都没用。

**24 日起少了，25 日起换成了 429。** 按同样的办法数，24 日 2 个、25 日 0 个、26 日到 UTC 15 点 6 个（重数时前几天的数字会差一两个，GitHub 的搜索索引在变）。但 25 日起另一种报告多了起来：周额度还剩八九成，却一直 `exceeded retry limit, last status: 429 Too Many Requests`，一位用户抓到的原始返回写的就是 `Codex is at capacity`，见 [429](#请求太多429-too-many-requests)。

碰上了，看到它在重试就先别手动打断；所有模型一直报、一连几天都这样，更要考虑是不是账号被限。

想确认自己是不是这种情况，到会话日志里搜 `server_overloaded`。日志在 `~/.codex/sessions`，Windows 是 `%USERPROFILE%\.codex\sessions`：

```bash
grep -rl "server_overloaded" ~/.codex/sessions | tail -5
```

搜得到，Usage 页面的额度又还有，就是服务器那边满载，不是你的额度、网络或设置的问题。issue 里用户试出来的几条，都不保证有效：

- 避开高峰。[#52796](https://github.com/openai/codex/issues/52796) 里出问题的都是北京时间的白天。
- 长任务拆短。[#52833](https://github.com/openai/codex/issues/52833)、[#52394](https://github.com/openai/codex/issues/52394) 都提到，步骤多、跑得久的任务更容易中途断掉，短任务能过。
- 桌面版一直报的，在同一台电脑上试试 CLI。[#52394](https://github.com/openai/codex/issues/52394) 里有人桌面版连新对话都报，CLI 用同一个模型却能正常回复。
- 重装、修复应用没用，同一个 issue 里有人试过。

### 被标记为网络安全风险：flagged for possible cybersecurity risk

```text
此聊天已被标记为可能存在网络安全风险
如果这看起来不对，请尝试换个说法，或提交 /feedback。要获得网络安全工作的授权，请加入 Trusted Access for Cyber 计划。
```

英文界面是 `This chat was flagged for possible cybersecurity risk` / `If this seems wrong, try rephrasing your request or submit /feedback. To get authorized for security work, join the Trusted Access for Cyber program.`。同一类的提示还有这几句（中英文都取自桌面版自带的语言包）：

| 你看到的 | 是什么 |
| --- | --- |
| 「你的对话中有多个可能的网络安全风险的标记」（`Your conversations have multiple flags for possible cybersecurity risk`），下面是「回复可能比平时慢，因为已开启额外安全检查。立即切换到 GPT-5.6 Terra 以获得更快回复」 | 被标记了不止一次，之后的回复会多做一道检查，所以变慢 |
| 「正在进一步审慎考虑此请求」（`Giving this request a little extra thought`），旁边有「重试」 | 这一次请求在做额外的安全检查。不想等，点「停止并重试」，它会换一个更快、但处理复杂任务弱一些的模型重发 |
| 「此内容无法显示」（`This content can’t be shown`），下面是「此内容因可能存在网络安全风险而被标记」 | 这一次的回复被拦下了 |
| CLI 里 `Warning: Your account was flagged for potentially high-risk cyber activity and this request was routed to gpt-5.2 as a fallback` | 见本节最后一段 |

**这是什么。** OpenAI 对网络安全相关的请求有一道单独的检查。按语言包里的说明，它重点看的是可能用于超出授权范围访问账户或系统、干扰系统运行或开发高风险网络工具、收集敏感数据或监控私人通信、绕过安全控制这几类的请求。官方的 [Cyber safety](https://learn.chatgpt.com/docs/cyber-safety) 文档写明，正常的安全工作、甚至和安全无关的操作也可能触发它；觉得是误判，在 Codex 里用 `/feedback` 反馈。openai/codex 里 10 月以来有不少误判的报告，比如普通的代码审查、迁移后的回归测试也被拦（[#56695](https://github.com/openai/codex/issues/56695)、[#56615](https://github.com/openai/codex/issues/56615)、[#50886](https://github.com/openai/codex/issues/50886)），都没有官方回复。

**这一条本身不是封号。** 被拦下、被放慢的是这一次请求，账号还能接着用。被标记多少次会怎样，官方没有说。账号被停用是另一回事，一般会收到 OpenAI 的通知邮件，申诉走邮件里的链接（OpenAI 帮助中心的 [Troubleshooting OpenAI account deactivation](https://help.openai.com/en/articles/10562188-troubleshooting-openai-account-deactivation)）。

怎么办：

1. **换个说法，把上下文交代清楚。** 比如说明这是你自己的项目、你要修的是什么问题。这是提示里官方自己给的第一个办法。
2. **觉得是误判，用 `/feedback` 反馈**，带上出错的时间。
3. **一直变慢的**，按提示切到 GPT-5.6 Terra，或者在「正在进一步审慎考虑此请求」那里点「停止并重试」。
4. **真在做授权的安全工作**（渗透测试、漏洞研究），可以按提示申请 Trusted Access for Cyber（产品里叫 Daybreak）。个人申请要有符合条件的付费套餐、开启 Advanced Account Security、至少一把实体的 FIDO2 硬件安全密钥，还要做身份验证；官方也写了 Daybreak 不是每个地区都提供（[Troubleshooting OpenAI Daybreak](https://help.openai.com/en/articles/20001259-troubleshooting-openai-daybreak)）。

**CLI 里那句 `Your account was flagged for potentially high-risk cyber activity`。** 完整的是 `… and this request was routed to gpt-5.2 as a fallback. To regain access to gpt-5.3-codex, apply for trusted access …`。对照 0.164.0 的源码（`codex-rs/core/src/session/mod.rs`），这是一句写死的提示：服务器实际用来回答的模型和你选的不一样时，CLI 就打出这一句，并把原因一律记成网络安全；里面的 gpt-5.2、gpt-5.3-codex 是旧的模型名，没有跟着更新。所以看到它，能确定的是这次请求换了一个模型回答，别照字面去找 gpt-5.3-codex。处理办法和上面一样。

### 模型不支持：model is not supported

```text
The 'gpt-6-astra' model is not supported when using Codex with a ChatGPT account.
```

常见原因有四种：

- **模型名写错了。** 命令行里要写完整 ID。官方 Models 页现在推荐的是 `gpt-6-astra`、`gpt-6.1-sol`、`gpt-6.1-terra`（10 月 26 日新出）、`gpt-6-luna`，另外 `gpt-6-sol` 和 GPT-5.6 的三款也还能选（10 月 26 日核对）。`gpt-6.1-terra` 要 CLI 0.159.0 以上。写 `--model astra` 这种简称，CLI 能启动，发消息时才报这个错（[#46410](https://github.com/openai/codex/issues/46410)）。
- **旧设置还指着下线的模型。** `~/.codex/config.toml` 里的 `model = "..."`、脚本、定时任务写死了旧模型。**GPT-5.5 在 2026 年 10 月 14 日从 ChatGPT 和 Codex 下线**，API 不受影响。官方 Models 页给的替换：Plus、Pro、Business、Enterprise、Edu 改成 `gpt-6-sol`，Free 和 Go 改成 `gpt-6-luna`。
- **新模型还在分批开放，或者你的套餐、客户端没有。** GPT-6.1 Sol 9 月 29 日发布，官方写的是能不能用取决于套餐、客户端和工作区设置；CLI 是 0.159.1 才把它加进自带的模型列表的，旧版本先升级。9 月有好几个 Pro 账号反映 Astra、Sol 被拒，官方 issue 到 10 月 3 日还开着（[#46304](https://github.com/openai/codex/issues/46304)、[#47333](https://github.com/openai/codex/issues/47333)）。
- **刚升级或换了套餐，登录凭证里还是旧套餐。** 10 月 11 日有用户在苹果 App Store 里换了套餐，之后 Codex 拿到的登录凭证里套餐一栏写的是免费版，所有模型都报这句，等到这个凭证过期、换了新的才恢复（[#53020](https://github.com/openai/codex/issues/53020)，还开着，另有人说遇到同样的情况）。刚充值或升级就报这个错的，先退出登录再重新登录（CLI 是 `codex logout` 再 `codex login`），让它重新拿一次凭证。

处理：在会话里输入 `/model`，从列表里选一个；删掉 config.toml 里写死的 `model`；把 Codex 升级到最新。这个报错的完整返回里是 `"status":400` 和 `invalid_request_error`，按状态码搜到这里的也是这一种。列表里选了还报错，多半是开放范围的问题，用 `/feedback` 反馈后等。

### 请求太多：429 Too Many Requests

```text
exceeded retry limit, last status: 429 Too Many Requests
```

先看 [status.openai.com](https://status.openai.com)。6 月 3 日那次 429，维护者的回复是官方事故、不是客户端问题，修好就恢复（[#26034](https://github.com/openai/codex/issues/26034)）。没有事故的话，看看是不是额度快用完了（[第 7 节](#7-额度用完了)），或者同时开的会话太多。

**10 月 25 日起：周额度还剩八九成，也一直 429。** 报告的人说 25 日开始遇到，26 日 openai/codex 里新开了 5 个带这句原文的 issue，报的多是 GPT-6.1 Sol，也有人 Astra 一样；桌面版 26.1021.31345、VS Code 插件、CLI 0.164.0 都有，Plus、Pro 都有（[#57072](https://github.com/openai/codex/issues/57072)、[#57073](https://github.com/openai/codex/issues/57073)、[#57084](https://github.com/openai/codex/issues/57084)）。桌面版里看着是一直「正在思考」，几分钟后才动。一位 Mac 上的 Plus 用户另外抓了一次原始返回（[#57073](https://github.com/openai/codex/issues/57073)）：

```text
HTTP 429
Retry-After: 205
{"error":{"type":"rate_limit_error","code":"turn_rate_limit_exceeded","message":"Codex is at capacity, please try again later"}}
```

CLI 里只显示 `exceeded retry limit, last status: 429 Too Many Requests`。也就是说，它和前几天的[模型满载](#模型满载selected-model-is-at-capacity)是一类：请求已经到了 OpenAI，被那边以满载为由拒了，和你的周额度、网络都没关系。还有人发现好几个会话的下一次重试都落在同一个时刻，像是服务器让整个账号一起等几分钟（[#57072](https://github.com/openai/codex/issues/57072)）。没有官方回复，状态页也没有记录。

下面是单个用户的经验，不保证有效：停下这一轮、重新发一次，有时比等它自己重试来得快（#57073 的报告人）；一位用户说 Sol 先被拦，Luna 和 Astra 当天下午只各碰到一次（[#57072 的回复](https://github.com/openai/codex/issues/57072#issuecomment-6418431046)）。

### 登录失效或官方故障：401 Unauthorized

```text
unexpected status 401 Unauthorized: Incorrect API key provided: sk-svcac***…. You can find your API key at https://platform.openai.com/account/api-keys., url: https://chatgpt.com/backend-api/codex/responses
```

401 有两种，处理办法相反，先分清：

- **报错里带 `Incorrect API key provided: sk-svcac…`，而你明明是用 ChatGPT 登录的，没填过 API Key。** 这是 OpenAI 服务器那边的故障，不是你的账号问题。北京时间 9 月 26 日 06:58–07:54 出过一次，CLI、桌面版、插件、网页版同时受影响，维护者回复是大范围故障，不用重复报告（[#48237](https://github.com/openai/codex/issues/48237)，[官方事故记录](https://status.openai.com/incidents/01M3DCNWMW57HYK8FJ5FBFPA39)）。这时**重新登录没用**，看 [status.openai.com](https://status.openai.com)，等恢复。当时压缩上下文也会报同一个错（`Error running remote compact task`）。
- **只有 `401 Unauthorized`，状态页一切正常，别人也没事。** 多半是你本机的登录失效了：`codex logout`，再 `codex login`。反复出现的，看[登录过期](#登录过期refresh-token-was-already-used)那一节。

### 后台服务（daemon）报错

```text
Error: start the Windows daemon from a non-elevated terminal; shared clients must not inherit administrator privileges
Error: host Job Object prevents daemon detachment; start from a host that allows breakaway
To work without the background server, rerun the same command with --no-daemon (including resume or fork and its arguments).
```

CLI 0.157.0（9 月 25 日）起，运行 `codex` 会自动启动一个常驻的后台服务（daemon），所有终端窗口共用它；窗口关了它还在，还会自己更新。维护者的解释是，这样多个会话之间能互相看到谁在跑（[#48043](https://github.com/openai/codex/issues/48043)）。它带来了一批新报错，大多出在 Windows 上：

| 你看到的 | 原因 | 怎么办 |
| --- | --- | --- |
| `start the Windows daemon from a non-elevated terminal` | 你用管理员身份开的终端。后台服务只允许用普通权限启动，不然普通会话就能借它执行管理员命令 | 用普通终端打开；必须用管理员终端的，加 `--no-daemon`。官方在考虑让管理员终端自动这样处理，还没定 |
| `host Job Object prevents daemon detachment` | 0.157.0 的问题 | 升级，0.157.1 已修（[#48016](https://github.com/openai/codex/issues/48016)） |
| 每发一次请求，终端窗口闪几下 | 0.157.0–0.159.1 的问题 | 升级，0.159.2 已修（[#48074](https://github.com/openai/codex/issues/48074)） |
| `Server is draining; retry after reconnecting (code -32600)` | 后台服务在自己更新，把正在跑的会话断开了。报告的人说自己什么都没改（[#51763](https://github.com/openai/codex/issues/51763)，10 月 7 日，未修） | 等几秒重新连接；不行就 `codex app-server daemon restart`，再 `codex resume` 接着干 |
| `failed to open daemon process`、`Access is denied. (os error 5)` | Codex 记下的后台服务进程号过期了，这个号后来被 Windows 分给了别的进程。issue 里有人查到是分给了 Windows Defender，Codex 去检查它时被系统拒绝（[#48043](https://github.com/openai/codex/issues/48043)） | 先关掉所有 Codex（桌面版、VS Code 插件也算），到 `%USERPROFILE%\.codex` 里把 `app-server-control`、`app-server-daemon` 两个文件夹改名或删掉，再启动。不要去结束那个进程号对应的进程。步骤来自 [LINUX DO](https://linux.do/t/topic/2981182) |
| `account/read failed during TUI bootstrap: … workspace routing discovery failed (code -32603)` | CLI 升级了，后台服务还是旧版（帖子里是 CLI 0.162.0 配 0.161.0 的后台服务），或者后台服务还记着旧的代理设置。发帖人断网重启后开始出现（[OpenAI 社区](https://community.openai.com/t/1404666)） | `codex app-server daemon version` 看两边版本，`codex app-server daemon restart` 重启，不会删配置、登录和对话记录。顺便核对代理端口：HTTP 端口和 SOCKS 端口通常不是同一个。同一句报错后半段也可能是 `failed to load workspace requirements (code -32603)`：10 月 10 日 macOS 上有人升到 0.162.1 后遇到，后台服务还停在 0.161.0，他用 `codex -c log_dir="$HOME/.codex/log"` 能正常启动（[#52775](https://github.com/openai/codex/issues/52775)，单个用户的报告） |
| `Background server has incompatible feature settings` | 后台服务更新后，功能开关和这个会话对不上 | 按提示重启后台服务。它会列出重启后用哪些设置，其他开着的会话会被断开，先让它们停在安全的地方 |
| 升级了，问题还在；Windows 上终端窗口一直弹、抢焦点 | 后台服务还是旧版本。CLI 和后台服务的版本是分开的：10 月 20 日 OpenAI 的人看过一位用户的诊断，CLI 是 0.163.0，后台服务还停在 0.159.0，弹窗口就是旧版的问题（[#55522](https://github.com/openai/codex/issues/55522)） | 会话里输入 `/status` 看后台服务的版本；输入 `/daemon`，选 **Install latest public stable** 让它马上更新。更新时会打断正在跑的任务。不进会话也行：`codex app-server daemon update`；想让后台服务直接用你现在这份 CLI，加 `--from-cli --yes`，然后 `codex app-server daemon restart`。10 月 24 日一位 Windows 用户的 CLI 是 0.164.0、后台服务还停在 0.157.1，这样更新、重启后窗口就不再弹了（[#48043](https://github.com/openai/codex/issues/48043#issuecomment-6378955743)） |

常用命令：

```bash
codex --no-daemon                          # 这一次不用后台服务，每个窗口各跑各的（和以前一样）
codex app-server daemon version            # 看后台服务的版本
codex app-server daemon stop               # 停掉后台服务
codex app-server daemon restart            # 重启后台服务
codex app-server daemon update             # 更新后台服务（会打断正在跑的任务）
codex features disable daemon_auto_start   # 以后都不自动启动
```

最后一条是 issue 里用户的做法（[#48195](https://github.com/openai/codex/issues/48195)），维护者在排查时用的是对应的 `codex features enable daemon_auto_start`。关掉以后，「CLI 退出后任务继续在后台跑」这类依赖后台服务的功能就没有了。

### 权限：不能联网、写不了文件、完全访问权限是灰的

桌面版、CLI、插件在你电脑上跑命令时，默认都在沙盒（sandbox，也有人叫沙箱）里：能读文件、能改当前项目里的文件、能跑普通命令，**但命令默认不能联网**；要改项目外面的文件、要联网，会先停下来问你（官方文档 [Sandbox](https://learn.chatgpt.com/docs/sandboxing)、[Agent approvals & security](https://learn.chatgpt.com/docs/agent-approvals-security)）。所以下面几种多半不是坏了：

- `npm install`、`pip install`、`git clone`、`curl` 失败，或者它停下来问要不要允许网络访问。
- 它能搜网页，跑的命令却连不上网。网页搜索和命令联网是两个开关，官方文档写明可以只开网页搜索、不给命令联网。
- 改 `~/.zshrc` 这类项目外的文件时，停下来等你批准。
- Mac 上弹窗问能不能访问「桌面」「下载」「音乐」这些文件夹。这是 macOS 自己的权限提示，官方 Troubleshooting 里有这一条。

权限设置在哪：桌面版是输入框下面的权限菜单（请求批准、帮我批准、完全访问权限），默认值在 设置 → 配置 里；CLI 里输入 `/permissions` 切换，`/status` 的 Permissions 那一行能看到现在是什么权限。

**想让命令能联网，又不想开完全访问。** 这样沙盒的其他限制都还在：

- 桌面版：设置 → 配置（Configuration）→ 打开「允许网络访问」（Allow network access）。它的说明是「当沙盒设为工作空间写入时允许网络访问」，所以同一页的「沙盒设置」要是「工作空间写入」（Workspace write）。提示「此项目使用内联沙盒表。请直接编辑 config.toml 修改网络访问权限」的，按下一条改文件。
- 配置文件 `~/.codex/config.toml`（Windows 是 `%USERPROFILE%\.codex\config.toml`）里加上：

```toml
[sandbox_workspace_write]
network_access = true
```

- CLI 只想这一次能联网：`codex -c 'sandbox_workspace_write.network_access=true'`。

新的权限配置（`default_permissions` 加 `[permissions.名字]`，官方标着 Beta）和上面这套写法不能混用：配置里出现了 `sandbox_mode`，就按上面这套走；用新写法的，联网写在那个配置的 `[permissions.名字.network]` 下面，`enabled = true`（官方 [Permissions](https://learn.chatgpt.com/docs/permissions)）。

**写不了文件。** 权限是「只读」（Read only）时什么都改不了，换回「请求批准」（Ask for approval）。要改项目外面的某个目录，每次都会问你；常用的目录可以加进可写目录，路径写完整的：

```toml
[sandbox_workspace_write]
writable_roots = ["/Users/你的用户名/other-project"]
```

Windows 的路径用单引号，免得反斜杠要转义：`writable_roots = ['D:\code\other-project']`。`network_access` 和 `writable_roots` 都要写在同一个 `[sandbox_workspace_write]` 下面，这个表头在文件里只能出现一次，写两遍 Codex 会读不了配置。官方更推荐另开一个项目或 worktree，而不是把可写范围越放越大（Sandbox 文档）。

**完全访问权限是灰的，写着「已被 requirements.toml 禁用」**（`Disabled by requirements.toml`）。这是管理员策略，本机的 `config.toml` 改不动它；官方的说法是组织或设备策略可以限制这些选项，本地设置覆盖不了（Troubleshooting 里「Approve for me is missing or disabled」一条）。策略可能来自（官方 [Managed configuration](https://learn.chatgpt.com/docs/enterprise/managed-configuration)）：

- 这台电脑上的 `/etc/codex/requirements.toml`（macOS、Linux）或 `%ProgramData%\OpenAI\Codex\requirements.toml`（Windows）；
- Business、Enterprise 工作空间的管理员下发的策略；
- 公司 Mac 上的 MDM 描述文件。

用的是公司或别人的工作空间，找工作空间管理员。自己的电脑、个人账号也是灰的，看看上面那个文件在不在、是谁放的，它要管理员权限才能改。「由组织管理」（Managed）、「该值由管理员策略管理」、「沙盒模式受此次安装的限制」是同一类。写的是「已在 config.toml 中禁用」（Disabled in config.toml）的不一样，是你自己的配置文件挡住了。

**完全访问要不要开。** 完全访问（Full access）就是不用沙盒、也不再问你（`sandbox_mode = "danger-full-access"` 加 `approval_policy = "never"`），文件和网络的边界都没有了，官方说只在你确实想给它这么大权限时用。只是想让命令联网，开上面的「允许网络访问」就够了。

**弹出 `command failed; retry without sandbox?`。** 不是报错，是在问你：这条命令在沙盒里失败了（多半是要联网或写项目外的文件），要不要在沙盒外面再跑一次（0.163.0 源码里这是沙盒拒绝后的批准提示）。看清楚是什么命令再批准；同一类命令总被拦，按上面的办法开「允许网络访问」或加可写目录，比每次都批准省事。

**开着完全访问，命令还是被拒：`rejected: blocked by policy`。** 报错长这样，多见于删文件、清理目录这类命令，Windows 桌面版最多：

```text
exec_command failed: CreateProcess {
  message: "Rejected(\"<被拒的那条命令> rejected: blocked by policy\")"
}
```

OpenAI 的人 10 月 16 日看过一位用户的日志后回复（[#54476](https://github.com/openai/codex/issues/54476#issuecomment-6207845526)）：完全访问是「从不请求批准」，某条命令被标记时没有人可问，就直接拒掉，也不经过自动审核，所以 `/approve` 里看不到它。他给的办法：CLI 里输入 `/permissions`，选 **Approve for me**（桌面版权限菜单里是「帮我批准」），用 `/status` 确认改过来了，再让它重试。这样被标记的命令会交给自动审核判断，而不是直接拒绝。之前已经被拒的不会自动重来。他们也承认这句报错没写清楚原因。10 月 10 日以来报这一句的 issue 有三十多个。

**「帮我批准」也会拦：`自动审核超时`、`命令被自动审核拒绝`。** 「帮我批准」（Approve for me）是让 ChatGPT 先替你审一遍要跑的命令。它拒了，界面上是「命令被自动审核拒绝：…」，或者整轮停下来「本轮操作已被自动审查终止」；审核本身没按时完成，是「自动审核超时」。确定这条命令没问题的，在 CLI 里输入 `/approve`，它会批准最近一次被自动审核拒掉的操作重试一次（0.163.0 里这条命令的说明是 approve one retry of a recent auto-review denial）。总是超时的，这一轮先切回「请求批准」，自己点。

Windows 上发送按钮提示「设置智能体沙盒以继续」（Set up Agent sandbox to continue）的，是 Windows 沙盒还没装好，见 [Windows 设置未完成](#windows-设置未完成helper_failed)。

### Windows 沙盒：setup refresh had errors (os error 32)

```text
helper_unknown_error: setup refresh had errors
trusted Node process exited unexpectedly
```

10 月 7 日 Windows 桌面版更新到 26.1002 之后，很多人的所有命令都在执行前失败，连 `Get-Location` 都跑不了。沙盒日志里是 Windows 的 `os error 32`（文件正被占用）：沙盒启动时要检查自己运行目录的权限，里面的 `node_repl.exe` 却正被 Codex 自己占着（[#51601](https://github.com/openai/codex/issues/51601)）。issue 里的人试过重启电脑、修复安装，都没用。

| 你用的是 | 现在的状态 | 怎么办 |
| --- | --- | --- |
| Windows 桌面版 | 10 月 9–10 日多位用户确认 **26.1007.21434** 已修。这是「关于」里显示的版本号，Microsoft Store 里的包版本写的是 26.1007.2314.0，是同一版 | 在 Microsoft Store 更新后重启。10 月 10 日还在报这个错的，看版本大多停在 26.1002。更新之前，有人退回 26.930.61225 能用 |
| 单独安装的 CLI（npm） | 0.161.0、0.162.0、0.162.1 都有人复现（[#51634](https://github.com/openai/codex/issues/51634)）。北京时间 10 月 16 日发布的 **0.163.0** 带上了修复：修沙盒权限时，只在确实要改的时候、用更小的访问权限去打开文件，正在运行的 `node_repl.exe` 就不会再被占用冲突挡住（[#51822](https://github.com/openai/codex/pull/51822)；0.162.1 里没有这个改动）。当天有一位用户确认 0.163.0 上不再报这个错，不过他用浏览器控制时还有 `trusted Node process exited unexpectedly`，他判断是另一个问题（[#51590](https://github.com/openai/codex/issues/51590)） | 先 `codex update` 到 0.163.0。还报的，报告的人在同一台机器上用 0.160.1 正常，可以先装回去 |

```bash
npm install -g @openai/codex@0.160.1 --registry=https://registry.npmmirror.com
```

不想降级的，issue 里还有两个用户自己试出来的办法：

- **换成不需要管理员权限的沙盒。** 在 `%USERPROFILE%\.codex\config.toml` 里，把 `[windows]` 下面的 `sandbox = "elevated"` 改成 `sandbox = "unelevated"`，保存后完全退出再打开 Codex。[#52488](https://github.com/openai/codex/issues/52488)、[#51601](https://github.com/openai/codex/issues/51601) 里好几个人这样恢复了；也有人说不管用，[#51590](https://github.com/openai/codex/issues/51590) 里 10 月 8 日一位改了照样报错，另一位绕过去之后又被另一个检查 `cannot enforce split writable root sets` 挡住。它的隔离比默认的弱一些（见 [Windows 设置未完成](#windows-设置未完成helper_failed)的第 3 步），官方修好后改回来。
- **只用 CLI 的。** 占着文件的 `node_repl.exe`，是桌面版写进 `config.toml` 的 `[mcp_servers.node_repl]` 带起来的。10 月 11 日有人退出桌面版后把这几段注释掉，CLI 0.162.1 就能跑命令了；要改哪几行见[他的回复](https://github.com/openai/codex/issues/52179#issuecomment-6107852303)。桌面版下次启动多半会把它们加回来。

- **CLI 已经是 0.163.0，浏览器控制、Computer Use 还报这一句。** [#54635](https://github.com/openai/codex/issues/54635) 的报告人查到，`config.toml` 和 Computer Use 插件的 `.mcp.json` 里各写着一个 `CODEX_CLI_PATH`，更新后还指着旧版 CLI，跑的是旧版自带的沙盒程序，就还有这个问题。把这两处改成现在的 `codex.exe` 路径（或者删掉这一项）后好了。上面「无法加载你的账户」一节第 3 步说的也是这个变量，顺手一起查。

issue 里有人提醒：别为了这个去手动改文件夹权限，退回旧版或者等更新更稳。

### Windows 桌面版登录后几十秒自己退出

没有报错窗口，也不卡，登录后 10–35 秒整个应用直接消失，所有 ChatGPT 进程一起没了；不登录就一直正常。10 月 7 日桌面版更新到 26.1002.7124.0 之后有人遇到，重装、换新的用户配置都没用（[OpenAI 社区](https://community.openai.com/t/1404464)，帖子已标记为解决）。

发帖人查到的原因：登录后应用会在后台下载并安装一个约 528 MB 的运行时组件（`OpenAI.CodexPrimaryRuntime`），安装这一步把主进程带崩了。因为是账号开启的功能触发的，所以重装没用。

怎么确认是不是这个问题，看这两处：

- `%LOCALAPPDATA%\Packages\OpenAI.Codex_2p2nqsd0c76g0\LocalCache` 里有没有一堆 `codex-windows-runtime-framework-*` 文件夹，每个里面一个约 528 MB 的 `.msix`，每启动一次多一个。
- 同一目录下 `Local\Codex\Logs` 的日志，崩溃前最后一行是不是 `primary_runtime_install_started`。

处理顺序：

1. 先在 Microsoft Store 更新到最新版再试。更新不一定管用：[LINUX DO](https://linux.do/t/topic/2996108) 上有人在 26.1007.641.0 上仍然闪退。
2. 还闪退，就手动装这个运行时组件：应用启动时发现组件已经装好，会跳过会崩溃的那一步。LINUX DO 那位用户的做法最简单：完全退出 Codex，打开上面的 `codex-windows-runtime-framework-*` 文件夹，双击里面的 `OpenAI.CodexPrimaryRuntime…msix` 安装，再打开 Codex。想稳妥一点，按 OpenAI 社区帖子里的 PowerShell 命令做，其中有一步是确认安装包的签名是 OpenAI 的。
3. 那堆重复下载的文件夹很占硬盘，问题解决后可以删掉旧的。

这是用户自己查出来的办法，不是官方方案。

### WSL：Failed to create unified exec process (os error 2)

```text
Failed to create unified exec process: No such file or directory (os error 2)
```

Windows 桌面版打开 Run agent in WSL（配置里是 `runCodexInWindowsSubsystemForLinux = true`）之后，每条命令都报这个错。从 9 月 30 日的 26.928 开始，10 月 11 日还有人在 26.1007.21434 上遇到（[#49731](https://github.com/openai/codex/issues/49731)）。10 月 12 日 OpenAI 的人在 issue 里回复「会修」并关闭了它，但 10 月 14 日有人在新版 26.1007.71326 上仍遇到，另开了 [#54063](https://github.com/openai/codex/issues/54063)。10 月 16 日有一位用户确认新版 **26.1013.31256** 修好了：命令和文件操作都正常，重启 WSL 和 Windows 之后也正常，临时目录换成了 `tmp/arg0-linux`，和官方的修复 [#53412](https://github.com/openai/codex/pull/53412)（按系统分开存放这些临时目录）对得上（[#49731](https://github.com/openai/codex/issues/49731)）。所以先更新到 26.1013.31256 试；还报的，再用下面的办法。10 月 10 日一位用户查到了原因：WSL 里的 Codex 和 Windows 这边的 Codex 共用同一个 `.codex` 目录，临时文件都放在 `tmp/arg0`（WSL 里看是 `/mnt/c/Users/<用户名>/.codex/tmp/arg0`）。两边启动时都会清掉里面「没人在用」的目录，可 Linux 和 Windows 的文件锁互相看不见，后启动的一方就把对方正在用的删了，`codex-linux-sandbox` 跟着没了。所以常见的情况是刚打开还能用，过一会儿所有命令都失败。

两个办法：

1. 不是非用 WSL 不可的，先在设置里关掉 Run agent in WSL。
2. 要继续用的，issue 里一位用户给了临时办法：把这个目录挂到 Linux 自己的文件系统上。在 WSL 里执行，把 `<用户名>` 换成你自己的：

```bash
mkdir -p ~/codex-arg0-local
sudo mount --bind ~/codex-arg0-local /mnt/c/Users/<用户名>/.codex/tmp/arg0
findmnt /mnt/c/Users/<用户名>/.codex/tmp/arg0     # 确认挂上了
```

挂好后要让 Codex 重新认一次：完全退出桌面版（托盘图标也退掉），在 PowerShell 里执行 `wsl --shutdown`，再打开。只挂目录不重启的话，已经在跑的 Codex 还指着被删掉的旧路径，等于没挂。

上面这样挂的，WSL 重启后会失效。想让它每次自动挂上，按[查到原因的那位用户的写法](https://github.com/openai/codex/issues/49731#issuecomment-6100089583)加进 `/etc/fstab`。先备份；路径要写全，不能用 `~` 或 `$HOME`：

```bash
sudo cp /etc/fstab /etc/fstab.bak
echo "/home/<Linux 用户名>/codex-arg0-local /mnt/c/Users/<用户名>/.codex/tmp/arg0 none bind,nofail 0 0" | sudo tee -a /etc/fstab
```

10 月 11 日另一位用户在 26.1007 上照做后确认可用。想撤销，删掉 fstab 里这一行，再对同一个路径执行 `sudo umount`。别为了这个把权限改成完全访问，那只是绕开了沙盒。这些是用户自己试出来的办法，不是官方方案，官方修好以后就不需要了。

### 网页版：Unable to determine project root for task

```text
Error submitting message
Unable to determine project root for task
```

在 Codex 网页版选好已经发布的云端环境，发第一条消息就报这个错，页面停在输入框不动。9 月 30 日起有人反馈，Windows 和 Mac 的浏览器都有，强制刷新、重新选环境都没用；到 10 月 10 日还没有官方回复（[#49497](https://github.com/openai/codex/issues/49497)）。

多半不是你的环境配错了：反馈的人里，有人的环境已经能跑通 25 个测试，照样发不出去。目前没有确认可用的解决办法。着急的话先用桌面版或 CLI 在本地跑；想跟进展，可以去 issue 里点个 👍。

**Safari、iPad 上：`Can't find variable: DisposableStack`。**

```text
Error submitting message
Can't find variable: DisposableStack
```

10 月 19 日起，在 Mac 的 Safari（26.5、27）里打开 Codex 网页版，一发消息就报这一句；iPad 上 Safari 和 Chrome 都一样，iPhone、iPad 的 ChatGPT 里是 `Codex server returned an error`。同一个浏览器里普通的 ChatGPT 对话正常，清网站数据、开无痕窗口都没用（[#55271](https://github.com/openai/codex/issues/55271)、[#55361](https://github.com/openai/codex/issues/55361)）。北京时间 10 月 24 日早上，这两个 issue 被 OpenAI 的工单机器人标成已完成，没写说明，也还没有用户回来确认好了；遇到的先刷新页面再试一次。`DisposableStack` 是一项比较新的 JavaScript 功能，报错的意思是浏览器里没有它；iPad 上的 Chrome 用的也是 Safari 的内核，所以一样报。在 Mac 或 Windows 上，先换 Chrome、Edge 打开 chatgpt.com/codex 试试，这是我们按报错推的，issue 里还没人在电脑版 Chrome 上确认过；手边只有 iPad 的，只能等官方修。

### 终端里复制不了文字

CLI 0.157.0 起默认用全屏界面：Codex 自己接管整个终端窗口，和 vim 一样，不再往终端的滚动记录里写。好处是长对话滚动快、输入框固定在底部；代价是在 Mac 自带的终端（Terminal.app）里，选中文字按 Cmd + C 没反应。维护者的答复是 Terminal.app 在全屏模式下不会把 Cmd + C 传给 Codex，这个不会改（[#48122](https://github.com/openai/codex/issues/48122)）。三个办法：

1. 输入 `/tui`，切回原来的滚动模式；或者启动时加 `--no-alt-screen`（`codex --no-alt-screen`）。全屏模式的那些功能就没有了。
2. 用选中即复制（copy-on-select）或右键复制，不按 Cmd + C。0.158.0 起这两项可以自己开关。
3. 换一个终端，比如 Ghostty、Kitty，它们没有这个限制。

全屏模式的官方介绍：[Codex CLI goes fullscreen](https://github.com/openai/codex/discussions/49129)。

### 远程控制：无法启用远程控制（Couldn't enable remote control）

```text
无法启用远程控制。请重试
无法启用远程控制。请确保仅有一个 ChatGPT 实例在运行
无法更新远程控制状态
无法开始设备配对
```

英文界面依次是 `Couldn’t enable remote control. Try again`、`Could not enable remote control`、`Couldn’t update remote control availability`、`Couldn’t start device pairing`（第二句中文界面多了半句）。用手机上 ChatGPT 的 Codex（旧版叫 Remote）控制电脑，要先在电脑的桌面版里打开：设置 → 连接（Connections）→「允许发现并控制此设备」，有的版本写「允许连接」。打不开，就是上面这几句。

先按官方 [Remote connections](https://learn.chatgpt.com/docs/remote-connections) 的排查顺序：

1. 电脑和手机登录的是同一个 ChatGPT 账号、**同一个工作空间**，两边的 App 都更新到最新。
2. 打开开关、点「添加」之后报错的：重启电脑上的 ChatGPT 桌面版再试。
3. 用的是 Business、Enterprise 工作空间：要管理员先开通 Remote Control。
4. 退出过登录的：退出登录会关掉远程控制，配对记录还在，重新登录后把开关再打开一次。
5. 6 月 8 日以后没用过的旧连接：两边都更新，再重新配对。

下面是用户在 openai/codex 里查到的几种情况，都没有官方回复：

- **只开了一个 ChatGPT，还是提示「请确保仅有一个实例在运行」。** 日志里是 `409 Conflict` 和 `Remote app server already online`，意思是服务器认为这台电脑已经有一个 Codex 在线（[#51071](https://github.com/openai/codex/issues/51071)）。有用户查到，占着的是另外装的 CLI 的后台服务：确认 CLI 里没有在跑的任务后执行 `codex app-server daemon stop`，手机就连上了（[#52675](https://github.com/openai/codex/issues/52675)）。也有人停了后台服务、重新配对都没用（[#52099](https://github.com/openai/codex/issues/52099)）。这条命令会停掉 CLI 的后台服务，CLI 里正在跑的任务会被打断。
- **Mac 是用「迁移助理」从旧 Mac 整机搬过来的。** 两台 Mac 的 `~/.codex/installation_id` 一模一样，服务器当成同一台机器，同样报 409（[#36295](https://github.com/openai/codex/issues/36295)，几位用户复现过）。issue 里用户的修法要改 `~/.codex` 里的身份文件，不是官方办法；真要动，先把整个 `~/.codex` 备份一份。
- **用的是系统代理，没开 TUN。** 有用户查到，桌面版的后台服务连远程控制时不走系统代理，日志里一直是 `failed to lookup address information`（[#52393](https://github.com/openai/codex/issues/52393)），和[第 4 节](#4-国内网络终端要单独设代理)说的 CLI 默认不读系统代理是一回事。10 月 20 日 Windows 上也有人查到同样的事（[#55557](https://github.com/openai/codex/issues/55557)，26.1013.4894.0，Clash 只开了系统代理）：把 `HTTP_PROXY`、`HTTPS_PROXY` 设成 Windows 用户变量、重启应用后，「允许连接」能打开、也能出二维码了；可桌面版还有一条连 `wss://codex-cloud-backend.chatgpt.com/` 的长连接是写死的，系统代理和这两个变量都不认，照样直连，手机可能还是连不上。另一位用户确认了。他还试过 `features.respect_system_proxy=true`，结果直接报 `Remote control is enabled on <电脑名> but the connection is errored.`，别开这个。所以国内用远程控制，**打开代理软件的 TUN 模式**最省事，这一点我们没有实测，报告的人也没试。

手机上一直回到 `Authorize this phone` 的，见[最近两周的已知问题](#最近两周的已知问题)表下面折叠的那一行。

### 插件和 MCP 连不上：OAuth state not found

**连接插件，回到应用报 `OAuth state not found`。** 在桌面版里连 GitHub、Figma、Firecrawl 或者自己加的 MCP 服务器，浏览器里授权完、跳回应用，就弹这一句，插件还是没连上。10 月 16 日起有人报，Windows 26.1013 上最多，没有官方回复（[#54626](https://github.com/openai/codex/issues/54626)、[#54924](https://github.com/openai/codex/issues/54924)、[#55314](https://github.com/openai/codex/issues/55314)）。几位用户的办法一样：**改到网页版连**。浏览器打开 chatgpt.com，用同一个账号把这个插件连上，回到桌面版就能用了（[#54626 的回复](https://github.com/openai/codex/issues/54626#issuecomment-6237513591)）。#55314 的报告人只是在浏览器里登录了一次 ChatGPT 网页版，也好了。

**MCP 服务器启动超时。** CLI 或桌面版提示：

```text
MCP client for `xxx` timed out after 30 seconds. Add or adjust `startup_timeout_sec` in your config.toml:
[mcp_servers.xxx]
startup_timeout_sec = XX
```

意思是这个 MCP 服务器 30 秒内没启动完（0.163.0 的默认值）。用 `npx`、`uvx` 启动的，第一次要先下载依赖，国内网络下常常超过 30 秒。照提示在 `~/.codex/config.toml` 里给它单独放宽，比如 `startup_timeout_sec = 120`；`xxx` 换成报错里的名字。放宽了还是超时，先在终端里手动跑一遍 `command` 那一行，看它自己能不能起来、是不是卡在下载上。下载慢的，给 npm 配国内镜像，或者先把包装好再让 Codex 启动。


### 上下文满了：ran out of room in the model's context window

```text
Codex ran out of room in the model's context window. Start a new thread or clear earlier history before retrying.
```

这不是额度用完，是**这一个会话**装的内容太多了：聊得太久、读了大文件、工具一次返回了很大的结果（比如浏览器截图）。Codex 平时会自动压缩上下文，但单次塞进来的东西太大时也会顶满（[#4926](https://github.com/openai/codex/issues/4926)）。10 月 12 日起，GPT-6.1 Sol 在 Codex 和 Work 里的默认上下文从 272k 提到 500k（Codex 负责人 Tibo [10 月 12 日的说明](https://x.com/thsottiaux/status/2109430343346581527)），用这个模型会少顶满几次。

1. 输入 `/compact`，把前面的对话压缩成摘要再继续。桌面版也能在输入框里输 `/compact`（中文界面显示「压缩此聊天的上下文」）。想随时看到上下文用了多少，在设置 → 常规里打开「在编辑器中显示上下文窗口使用情况」。
2. 压缩后还不够，`/new` 开一个新会话，把要点和下一步重新交代一遍。
3. 大任务拆小，一个会话只做一件事。别一次让它读整个大日志或大 JSON，先让它用命令过滤出需要的部分。
4. 升级到最新版。维护者 6 月说过，压缩逻辑已经改到本地执行，之前那类「压缩失败」的报错少了很多（[#9046](https://github.com/openai/codex/issues/9046)）。

`/status` 可以随时看当前会话用了多少上下文。

### 证书错误（公司网络、抓包软件）

公司网络会做 TLS 拦截，或者本机开着 Charles、Fiddler 这类抓包工具时，Codex 会报证书相关的错误。官方的办法是把公司或抓包工具的根证书指给 Codex：

```bash
export CODEX_CA_CERTIFICATE=/path/to/your-root-ca.pem
codex login
```

没设 `CODEX_CA_CERTIFICATE` 时，Codex 会退回读 `SSL_CERT_FILE`。登录、普通请求和 WebSocket 都用这一份证书。

## 7. 额度用完了

```text
You've hit your usage limit. Upgrade your plan to continue, or try again at …
```

中文界面：「你已达到使用上限。升级套餐以继续，或在 … 后重试。」后半句也可能是「升级套餐或充值额度以继续」「请充值后继续」。还有一种是「你已达到 GPT-6 Astra 的使用限额。请在 … 后重试，或与其他模型新建对话。」后一种说明是**这个模型**的限额用完了，换一个模型往往还能继续用。

先看还剩多少：CLI 里输入 `/status`；桌面版点个人菜单里的「使用情况」；网页看 [chatgpt.com/codex/settings/usage](https://chatgpt.com/codex/settings/usage)。然后按情况选：

| 情况 | 怎么办 |
| --- | --- |
| 5 小时额度用完 | 等报错里写的时间，一般几小时内就回来。Plus 有 5 小时限制；Pro 目前没有，只算每周额度（官方定价页，10 月 18 日核对） |
| 每周额度用完 | 等周额度重置；急用就换个消耗低的模型，或者买额外额度 |
| 当前模型用完了 | 按提示换一个模型开新会话 |
| 手上有重置机会 | 中文界面叫「使用已储备的重置机会」，点「使用重置」。至少有一项用量剩余不到 80% 才用得了，见下面的[重置机会用不了](#重置机会用不了你当前的用量暂时无需重置)。Codex 负责人 Tibo 经常在新模型上线、用户数破纪录这类时候给大家发重置，最近几乎隔一两天一次：最近一次是北京时间 10 月 26 日 13:21，他说「今天是重置」，没说几点到（[原帖](https://x.com/thsottiaux/status/2114588174249071102)）；25 日 11:39 宣布、14:06 说已经全部到账（[宣布](https://x.com/thsottiaux/status/2114200152701575656)、[到账](https://x.com/thsottiaux/status/2114237083652882932)）；再往前是 22 日、23 日接连宣布的两次，都说「明天太平洋时间上午 10 点前」到，也就是北京时间 23 日、24 日凌晨 1 点前后（原帖：[22 日](https://x.com/thsottiaux/status/2113162277688070445)、[23 日](https://x.com/thsottiaux/status/2113492187770175603)），以及 10 月 20 日 14:33（[原帖](https://x.com/thsottiaux/status/2112432076767289678)）；最近一张重置卡是 10 月 8 日凌晨发的（GPT-6 进普通 Chat 那天）。重置卡有具体的到期时间，过期不能恢复：网页版 Settings → Usage 里把鼠标停在到期日期上，能看到具体时间和时区（OpenAI_Support 10 月 6 日在[官方社区](https://community.openai.com/t/1403334)的答复）。[每一次的时间和原推在这里](https://aonir.com/guides/codex-usage-reset/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_resets) |
| 经常用完，影响干活 | 考虑升级。9 月 29 日起 Pro 分三档：Pro 100、Pro 200、Pro 500（$100 / $200 / $500），Codex 额度分别约为 Plus 的 5、10、25 倍 |

Plus 每 5 小时大概能发多少条（官方定价页的估算，本地任务，10 月 26 日核对）：

| 模型 | Plus 每 5 小时 |
| --- | --- |
| GPT-6 Astra | 5–45 条 |
| GPT-6.1 Sol | 15–160 条 |
| GPT-6.1 Terra（10 月 26 日新出） | 35–285 条 |
| GPT-6 Sol | 15–150 条 |
| GPT-6 Luna | 350–3,000 条 |

这是估算，不是固定次数，任务越大越耗，另外还有每周上限。同一份额度，用 Sol 能跑的条数大约是 Astra 的 3 倍，Terra 的估算条数又比 Sol 多。日常写代码先用 GPT-6.1 Sol，难活再换 Astra；看截图、操作网页和桌面应用、写文案，可以试 GPT-6.1 Terra（官方的定位是快、省，擅长看图、操作电脑和写作，普通对话里没有，Free 和 Go 不包括）。

> 中文界面里有个容易看混的地方：**「额度」有两种意思。**「每周使用限额」「5 小时使用限额」是套餐自带、会自动恢复的；「添加额度」「额度余额」对应英文的 credits，是用完后另外花钱买的点数。官方说明 Plus 和 Pro 用完限额后可以买 credits 继续用，不一定要升级。

**周额度突然掉了一大截。** 北京时间 10 月 17 日凌晨起，openai/codex 里好几个人报周额度几十秒内大幅下降：一位从 77% 掉到 10%，用了 38 秒，本地日志里这段时间只多了 38 万 token，九成多是缓存；另一位 8 秒内从 58% 掉到 0%（[#54677](https://github.com/openai/codex/issues/54677)、[#54718](https://github.com/openai/codex/issues/54718)）。

**10 月 21 日官方认了。** 北京时间凌晨 01:10，Tibo 在 X 上说：周末有人反映额度掉得比预期快，他们查到约 4 万个账号实际用的额度上限和所选套餐对不上，已经修正，正在逐个联系这些用户，受影响的会补 credits，并道歉（[原帖](https://x.com/thsottiaux/status/2112592264686149749)）。上面两位发帖人当天都收到了邮件，标题是「Correcting your usage limit」，按他们贴出的原文：从 Pro 200 换到更便宜的套餐之后，系统出错，一直按原来 Pro 200 的周额度算；修正的时候，这一周已经用掉的量按新套餐的上限重新算，所以一下子掉了一大截。补偿是 $500 的 credits（12,500），和周额度分开算，2027 年 1 月 20 日到期；周额度本身没有退回。另有一位说收到的是 25,000 credits（[#55049](https://github.com/openai/codex/issues/55049)）。issue 里也有人说自己从没换过套餐，是不是同一个原因还不清楚。

怎么办：先看邮箱里有没有这封邮件，credits 在 Usage 页面能看到。没收到、又觉得自己被多扣了的，截下 Usage 页面，带着时间去找 OpenAI 客服；也可以在 Codex 里输入 `/feedback` 提交并记下反馈 ID。这之后 Tibo 又宣布了几次重置（北京时间 10 月 20 日、23 日、24 日、25 日、26 日，见上表），额度掉了的先刷新看看现在还剩多少。

10 月 17 日 Tibo 还说，Codex 和 Work 的实际可用量提高了：子 agent 之间复用上下文、上下文缓存留得更久、请求路由改进让缓存命中更多；具体能多用多少，他没说（[原帖](https://x.com/thsottiaux/status/2111244596131111253)）。

会员到期变回 Free 再重新订阅，周额度不会刷新。[LINUX DO](https://linux.do/t/topic/2995861) 上有人试过：Plus 到期时周额度还剩 82%，马上重新订阅，还是 82%。这是一位用户的情况，仅供参考。

各套餐多少钱、Astra / Sol / Luna 哪个更省额度：[Codex 多少钱？Plus 和 Pro 怎么选](https://aonir.com/guides/codex-pricing/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_pricing)；哪个套餐在哪个入口能用哪个模型：[ChatGPT 各套餐模型对照](https://aonir.com/guides/chatgpt-models-by-plan/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_models)。

### 重置机会用不了：你当前的用量暂时无需重置

```text
你当前的用量暂时无需重置
```

英文界面是 `Your usage does not need a reset right now`，有时候是 `Your usage limits are at 100%`。北京时间 10 月 23 日凌晨那次全体重置之后，好几个人点「使用重置」都是这一句，可 Usage 页面上的周额度明明只剩八成多（[#56262](https://github.com/openai/codex/issues/56262)、[#56258](https://github.com/openai/codex/issues/56258)）。

**这是规则。** OpenAI 帮助中心的 [How resets work in Codex](https://help.openai.com/en/articles/20001498)（原来叫 How banked Codex resets work；10 月 24 日读过，26 日重读）现在写明：

- **至少有一项用量剩余不到 80% 才能用。** 重置覆盖的几项用量都还剩 80% 或以上，就会出现这一句。这次没用掉的重置不会被扣，到期前还能用。[#56262](https://github.com/openai/codex/issues/56262) 里有人翻出这篇文章 9 月 6 日的存档，当时还没有这条门槛。
- **重置补的是哪几项，按套餐不同。** Plus 和 Business Standard 补 5 小时和每周两项；Pro 100、Pro 200、Pro 500 只补每周那一项；Go 补每月那一项。补的是把低于 100% 的那几项拉回 100%，不是在现有基础上再加一份。生图、上传文件、普通聊天语音这些单独的限额不受影响。
- **在哪用。** 桌面版：设置 →「用量和账单」（英文 Usage & billing），点写着有重置可用的那一栏；网页版在 Usage 设置里；CLI 里输入 `/usage`，选 Redeem reset（版本旧的没有这一项）。手机 App 里用不了。
- **让 ChatGPT 替你用。** 桌面版设置里有一项「允许助手使用用量重置次数」，说明写的是：只有你在对话里明确让它重置，而且当前限额剩余不超过 10% 时，它才会用，系统不会自动用。手上的卡莫名少了一张，先看看是不是开了这一项、对话里是不是让它重置过。
- **全体重置不会扣你手上的重置卡。** 刚用完一张重置卡又碰上全体重置，用掉的那张不退，两次不会叠加成 200%；没用的卡照旧留着。「全体重置」也不是每个账号、每一项限额都有，以每次公告写的为准。
- **过期、用错了，客服不补。** 文章写明客服不处理免费重置的资格、不延期、不补发，也不给补偿。所以手上有卡的，记得看到期时间（把鼠标停在到期日期上能看到时区）。
- **重置会改下一次刷新的时间。** 用重置卡、全体重置、付费重置，只要让某一项用量重新开始算，下一次刷新就从那时起重新排。文章的例子是，原定周五刷新的周额度在周二被重置了，下一次刷新就是下周二，原来的周五不再算。所以下次几点刷新，以 Usage 页面上显示的为准，别按自己点重置、或者看到公告的时间去推；新周期开始前，页面上的时间可能只是估计，还会变。只是日期变了，不代表又用掉了一张卡。
- **用了却好像没变，或者弹「无法重置用量。请重试。」「无法加载用量限制重置信息」「用量详情不可用」。** 用量要过一会儿才更新：刷新或者重新打开 Usage 页面，确认看的是对的账号和工作空间。**别为了试试看再用一张、或者再买一次。** 重置卡只有真的补回了至少一项才算用掉，一项都没补的话，卡还留着。
- **用过的记录。** 桌面版 设置 →「用量和账单」→「使用限额重置」里有「历史记录」（History），能看到最近 30 天重置卡的进出：「已获得重置次数」（Reset received）只是收到了一张卡，不代表额度已经刷新；「已使用重置次数」（Reset used）是用掉了一张；还能用的在「可用」那一栏，带到期时间。全体重置和付费重置不进这份记录，所以记录里没有，不代表你错过了全体重置。重置卡也可能来自邀请活动，规则以那次活动的说明为准。

**还有一种是花钱买的。** 设置 →「用量和账单」里的「购买即时重置」，或者周额度用完后弹出的「支付 {价格} 重置」，是付费的即时重置，只对部分 Plus、Pro 个人账号开放，同一个套餐的人看到的也可能不一样。按 OpenAI 的 [Paid weekly Work and Codex rate limit resets](https://help.openai.com/en/articles/20001507)（10 月 26 日重读，Free、Go、Business、Enterprise、Edu 都买不了，能不能买还和账单所在的国家有关）：付款后 5 小时和每周额度马上恢复，存不起来，也不会等你把剩下的额度用完；**之后的每周刷新时间会改**，购买页上的原话是「下次周限额重置时间将调整为你下次在 Codex 或 ChatGPT Work 中发送消息后 7 天」；一般不退款。官方的说法是，它把你正常的每周额度提前拿来用，不是额外多给一份，也不进 credits 余额。

## 8. 怎么设置中文

**桌面版自带简体中文，不用装汉化包。**

- **桌面版**：按 Cmd + 逗号（Windows 是 Ctrl + 逗号）打开设置，在「常规」里找到「语言」（英文界面叫 Language），选「中文（中国）」，也就是简体中文。列表很长，可以在搜索框里输入「中文」或 Chinese。默认是「自动检测」，跟随系统语言。
- **VS Code / Cursor 插件**：跟着编辑器的显示语言走。只想让插件显示中文，在设置里把 `chatgpt.localeOverride` 设成 `zh-CN`。
- **命令行 CLI**：没有中文界面，官方配置里也没有这个选项。想让它用中文回复，在 `~/.codex/AGENTS.md` 里写一句「用简体中文回复」（见[第 9 节](#9-agentsmd-中文模板)）。

**设置里没有「语言」这一项？** 这个选项由 OpenAI 后台的开关控制。我们看了桌面版 26.915 安装包里的代码，「语言」这一项和中文文字包都受同一个开关控制，没对你的账号打开时就不显示。国内用户先看下面[选了中文，界面还是英文](#选了中文界面还是英文)：多半是启动那一刻没连上 OpenAI，这个开关没拿到。开着代理重启还是没有，再更新到最新版；仍然没有，就只能等官方开放。

**不建议装第三方汉化包。** 这类工具要么解包修改安装文件（app.asar），再改掉程序的完整性校验；要么通过调试端口往运行中的应用里注入脚本。官方一更新就容易失效，甚至打不开。有的还捆绑了跳过官方登录、多账号切换这类功能。

详细步骤和官方示意图：[Codex 怎么设置中文](https://aonir.com/guides/codex-chinese-settings/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_chinese)。

### 选了中文，界面还是英文

「语言」里明明选了中文（中国），`config.toml` 里也写着 `[desktop] localeOverride = "zh-CN"`，重启后界面还是英文，有时还变回了旧版界面、少了新功能。

原因是桌面版每次启动都要先问一次 OpenAI 的服务器，要不要打开中文界面（一个叫 `enable_i18n` 的后台开关），中文文字包其实已经在安装包里了。启动那一刻连不上 chatgpt.com，就按英文、按旧界面显示；有用户查到上次联网时拿到的结果没能存下来，所以下次没网照样是英文。Windows、macOS 都有人来回试过：开着代理启动是中文，关了代理启动是英文（[#51980](https://github.com/openai/codex/issues/51980)、[#52471](https://github.com/openai/codex/issues/52471)，没有官方回复）。

1. 先打开代理软件的系统代理或 TUN 模式。桌面版不读终端里设的代理变量。
2. 从托盘图标（Mac 是 Cmd + Q）彻底退出，不是只关窗口。
3. 再打开。中文出来以后，中途关掉代理，这一次打开的窗口也会一直是中文。

macOS 上也有一位开着代理照样是英文（#52471），这种只能等官方修。

### Windows 上中文乱码

Codex 读出来的中文是一串乱码，或者它改完文件、文件里的中文坏了，还有的直接报 `apply_patch verification failed: Failed to find expected lines`（它按乱码去找原文，当然找不到）。

原因：中文 Windows 默认用 GBK（代码页 936），Windows 自带的 PowerShell 5.1 读文件时也按 GBK，Codex 却按 UTF-8 去解。官方还没修，issue 从 5 月开到现在（[#23044](https://github.com/openai/codex/issues/23044)）。下面都是用户试出来的办法，不是官方方案：

- **只用 CLI 的：装 PowerShell 7，先切到 UTF-8 再启动。** #23044 里一位用户测过四种组合，只有「PowerShell 7 + 代码页 65001」读出来是对的；PowerShell 5.1 切了 65001 照样乱，只装 PowerShell 7 不切也乱。所以先装 PowerShell 7（他提醒用微软官网的 MSI 安装包，`winget` 装的商店版在 Codex 里有问题），然后每次在命令提示符里先 `chcp 65001` 再运行 `codex`；嫌麻烦就写一个 `.bat`：

  ```bat
  @echo off
  chcp 65001 >nul
  codex %*
  ```

- **桌面版也想解决：把系统换成 UTF-8。** 设置 → 时间和语言 → 语言和区域 → 管理语言设置 → 更改系统区域设置，勾上「Beta 版：使用 Unicode UTF-8 提供全球语言支持」，重启电脑。这样 PowerShell 5.1 默认也按 UTF-8 读。代价是一些老的中文软件可能反过来显示乱码，用不惯就改回来。这一条我们没有在 Codex 上实测。

### 中文界面和英文教程对不上

界面切成中文以后，还有一个问题：官方文档、英文教程和视频、GitHub issue、Tibo 的推文，还有终端里的 CLI，都是英文。照着英文教程找按钮时，常常对不上。最常用的几个：

| 英文 | 中文界面 |
| --- | --- |
| Local / Worktree / Cloud | 本地 / 工作树 / 云端 |
| Hand off | 移交 |
| Review | 审查（有些地方译成「审核」） |
| Stage / Revert | 暂存 / 还原 |
| Approve for me / Full access | 帮我批准 / 完全访问权限（设置里叫「完整访问权限」） |
| Steer / Queue | 引导 / 排队（26.915 及更早叫「调整方向 / 加入队列」） |
| Compact | 压缩 |
| Usage limits / Credits | 用量限制 / 额度（见上一节，两个都可能叫「额度」） |

完整对照 117 条，按界面区域分组：[UI-GLOSSARY.md](UI-GLOSSARY.md)。中文取自 ChatGPT 桌面版自带的简体中文界面，英文取自同一版本的原文，10 月 3 日对着 26.930 逐条复核过，10 月 11–12 日按 26.1007 补了 7 条报错提示。

## 9. AGENTS.md 中文模板

`AGENTS.md` 是写给 Codex 看的项目说明书。官方文档的说法是，Codex 在动手之前会先读它，所以「怎么装依赖、怎么跑测试、哪些文件别碰」写在这里，就不用每次在对话里重复。

它从三个地方读，后读到的优先：

1. 全局：`~/.codex/AGENTS.md`，所有项目都生效。适合放个人习惯，比如「用简体中文回复」。
2. 项目根目录的 `AGENTS.md`。
3. 当前目录一路往上的子目录里的 `AGENTS.md`，越靠近你工作的目录，优先级越高。

合起来默认最多读 32 KiB，超出的部分会被截掉，所以写短一点、写具体一点。

模板在这里，复制到项目根目录再改：[templates/AGENTS.md](templates/AGENTS.md)。全局那份很短：

```md
# ~/.codex/AGENTS.md
- 用简体中文回复，代码、命令、报错保留英文原文。
- 改代码前先说明要改哪些文件、为什么；一次只做一件事。
- 装新依赖、删文件、改数据库结构前先问我。
- 改完跑一遍项目的测试，贴出结果；没跑就说没跑。
```

写好后验证 Codex 读到了没有：

```bash
codex --ask-for-approval never "总结一下你当前加载了哪些说明"
```

想让 Codex 按一套固定做法干活，比如先问清需求、测试先行、画架构图、做 PPT，装 skill 比全写进 `AGENTS.md` 合适：`AGENTS.md` 每次都整份读，skill 平时只带一句说明，用到才读正文。值得装哪些、怎么确认装上了、装多了有什么影响，见我们另一个仓库 [Codex Skills 中文精选](https://github.com/Avarce/codex-skills-cn)。

## 10. 接入 DeepSeek 和第三方 API

Codex 不是只能用 ChatGPT 账号。在配置文件里加一个「模型提供方」（model provider），就能改用别家的 API：用那一家的 key，按那一家的价格付费，不走 ChatGPT 套餐的额度。CLI、桌面版、VS Code 插件读的是同一份配置：`~/.codex/config.toml`，Windows 是 `%USERPROFILE%\.codex\config.toml`。

反过来不行：ChatGPT 套餐里的 Codex 额度只能用 ChatGPT 登录来用，它不是 API key，填不到别的工具里。

### 先看能不能接：对方要支持 Responses API

Codex 现在只认一种接口协议，`wire_api = "responses"`。老教程里的 `wire_api = "chat"`（Chat Completions）已经删掉了，写了会直接报错。官方配置文档的说法是 `responses` 是唯一支持的值。

很多写着「兼容 OpenAI」的服务，兼容的只是 Chat Completions，这种直接填地址是接不上的。动手之前，先到那一家的文档里找有没有 Codex 或 Responses API 的接入说明。

### DeepSeek

DeepSeek 的 API 支持 Responses 格式，官方有一篇[接入 Codex 的文档](https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/codex)。模型名是 `deepseek-flash`（能看图）和 `deepseek-v4-pro`，key 在 [DeepSeek 开放平台](https://platform.deepseek.com/api_keys)创建。下面两种做法选一种。

**做法一：照官方文档，把 DeepSeek 设成默认。** 桌面版、插件、CLI 之后都走 DeepSeek。官方给了一键脚本，会先把你的 `config.toml` 备份到 `~/.codex/backup-deepseek/`，再写入配置和模型目录 `~/.codex/models.json`；再跑一次、选菜单第 9 项可以恢复。

```bash
# macOS / Linux（命令来自 DeepSeek 官方文档，10 月 11 日核对）
bash <(curl -fsSL https://cdn.deepseek.com/api-docs/codex-deepseek-setup.sh)
```

```powershell
# Windows PowerShell
irm https://cdn.deepseek.com/api-docs/codex-deepseek-setup.ps1 | iex
```

> **原来用 ChatGPT 登录的，先看这一条。** 官方配置里有一行 `forced_login_method = "api"`。Codex 读到它，发现你当前是用 ChatGPT 登录的，会直接把登录退掉，并提示 `API key login is required, but ChatGPT is currently being used. Logging out.`。这不是出错，再运行一次就能用 DeepSeek 了。但以后想换回 ChatGPT 套餐，光恢复配置不够，还得重新 `codex login`。

**做法二：只在 CLI 里按需切换，不动原来的登录。** 把 DeepSeek 的设置单独放进一个 profile 文件，要用的时候加 `--profile deepseek`，不加就还是原来的 ChatGPT 账号。

1. 把官方文档「方式二」里的 `models.json` 完整复制下来，存成 `~/.codex/models.json`。它告诉 Codex 这两个模型的上下文长度、能不能看图、有哪些推理档位。
2. 新建 `~/.codex/deepseek.config.toml`（文件名里的 `deepseek` 就是 profile 的名字）：

   ```toml
   model = "deepseek-flash"          # 或 deepseek-v4-pro
   model_provider = "deepseek"
   web_search = "disabled"           # 官方文档要求：DeepSeek 模型下关掉内置联网搜索
   model_catalog_json = "~/.codex/models.json"

   [model_providers.deepseek]
   name = "deepseek"
   base_url = "https://api.deepseek.com/"
   wire_api = "responses"
   env_key = "DEEPSEEK_API_KEY"      # 从环境变量读 key，不把 key 写进文件
   ```

3. 设好环境变量再启动：

   ```bash
   export DEEPSEEK_API_KEY="sk-你的key"      # Windows PowerShell：$env:DEEPSEEK_API_KEY="sk-你的key"
   codex --profile deepseek
   ```

这份配置比官方的少了 `forced_login_method`，所以不会退掉 ChatGPT 登录。profile 是命令行的参数，桌面版和插件怎么选 profile，官方文档没有写；想在桌面版里用 DeepSeek，按做法一来。

怎么确认生效了：在项目目录里运行 `codex exec --profile deepseek "hi"`，启动信息里有 `model: deepseek-flash` 和 `provider: deepseek` 两行。DeepSeek 文档的说法是，桌面版的模型选择器会显示「自定义」或模型名。

### 接别家，以及 CC Switch、中转站

别家的写法一样，改三处：`[model_providers.名字]` 里的名字（不能用 `openai`、`ollama`、`lmstudio`，这三个是内置的）、`base_url`、`env_key`。Codex 实际请求的地址是 `base_url` 后面直接拼 `/responses`，所以 `base_url` 要不要带 `/v1`，以对方文档给的地址为准。

**CC Switch 是什么。** [farion1231/cc-switch](https://github.com/farion1231/cc-switch) 是一个开源的桌面工具（MIT 协议），用图形界面替你改这些配置文件，在几家供应商之间一键切换。按它 README 的说法，它的「路由」模式会在本机起一个转发服务，在 Chat Completions、Responses 等几种格式之间做转换，所以只支持 Chat Completions 的服务也能经它接到 Codex 上（10 月 11 日，v4.0.8，我们没有实测）。用之前知道两点：它切换时会改写你的 `config.toml`，自己先备份一份；它的 README 里有大量中转站的赞助广告，那不代表这些中转站可靠。

**用 CC Switch 换了 ChatGPT 账号，CLI 里还是原来那个。** [V2EX](https://www.v2ex.com/t/1251444) 上一位用户 10 月 24 日的说法：CC Switch 只改了账号文件，常驻的后台服务还拿着旧的登录状态；他用 `codex --no-daemon` 启动就换过来了。网上流传的 `codex --disable tui_app_server` 现在不起作用，我们对过源码，这个开关在 0.163.0 里就已经标成移除、读到就跳过。按后台服务的原理，换完账号跑一次 `codex app-server daemon restart` 应该也行，这一条我们没试。

**中转站。** 就是第三方转卖的 API 入口，配置方法同上，一个 `base_url` 加一个 key。区别在风险：你的代码和对话都要经过它的服务器；给你用的是不是你选的那个模型，你没法验证；充进去的钱没有官方保障。公司的代码别走中转。

### 接第三方时的报错

| 你看到的 | 原因 | 怎么办 |
| --- | --- | --- |
| `` `wire_api = "chat"` is no longer supported `` | 配置是照旧教程写的，Chat Completions 协议已经删了 | 改成 `wire_api = "responses"`。对方不支持 Responses API 的，直接接不了 |
| `unexpected status 404 Not Found … url: https://…/responses` | 地址填得不对，或者对方根本没有 `/responses` 这个接口 | 报错最后的 `url` 就是 Codex 实际请求的地址，拿它对照对方文档，看 `base_url` 是多了还是少了 `/v1` |
| ``Missing environment variable: `DEEPSEEK_API_KEY`.`` | `env_key` 指的环境变量，在启动 Codex 的这个终端里没有 | 先 `export` 再启动；想长期生效，写进 `~/.zshrc` 或 Windows 的用户环境变量 |
| `API key login is required, but ChatGPT is currently being used. Logging out.` | 配置里有 `forced_login_method = "api"`，Codex 把 ChatGPT 登录退掉了 | 见上面那条提醒。不想被退登录，用做法二 |
| ``Model metadata for `deepseek-flash` not found. Defaulting to fallback metadata`` | 没有模型目录，Codex 不知道这个模型的上下文长度和能力，按保守的默认值来 | 放好 `models.json`，并在配置里写 `model_catalog_json` |
| `model_catalog_json path … must contain at least one model` | `models.json` 是空的，或者没复制完整 | 从官方文档重新复制 |
| `Codex is ignoring 1 unrecognized configuration setting`，下面一行是 `` `preferred_auth_method` is ignored `` | DeepSeek 官方配置里的这一项，新版 Codex 已经不认了 | 不影响使用，删掉这一行就不再提示 |
| `model_providers contains reserved built-in provider IDs` | 提供方的名字用了内置的 `openai`、`ollama` 或 `lmstudio` | 换个名字，比如 `openai-custom` |
| ``--profile `xxx` cannot be used while … contains legacy … `[profiles.xxx]` `` | `config.toml` 里还留着旧写法的 `[profiles.xxx]`。0.134.0 起 `--profile` 不再读它 | 把那一段挪到 `~/.codex/xxx.config.toml`，键写在最外层，再删掉旧的 |
| `401 Unauthorized: Missing bearer or basic authentication in header, url: https://api.openai.com/v1/responses` | 请求没有走到你配的提供方，去了默认的 OpenAI，而你又没登录。常见的是忘了加 `--profile` | 看启动信息里 `provider:` 那一行是不是你配的名字 |
| 项目里的 `.codex/config.toml` 写了提供方，不生效 | 官方文档：项目级配置里的 `model_provider`、`model_providers` 会被忽略 | 写到用户级的 `~/.codex/config.toml` 或 profile 文件里 |
| 切换之后，以前的对话不见了 | 没删。DeepSeek 文档的说明是，Codex 把 ChatGPT 账号的对话和第三方 API 的对话分开显示 | 换回原来的配置，重启客户端就回来了 |
| 桌面版用自己配的提供方，输入框却灰了 | ChatGPT 套餐额度用完后，桌面版把输入框整个禁用了，哪怕模型走的是别家（[#48816](https://github.com/openai/codex/issues/48816)，未修） | 等套餐额度恢复，或者先用 CLI |
| 没登 ChatGPT 账号、只用 API Key，桌面版每条消息都「提交消息时出错」（`Error submitting message`），日志里是 `Work browser access requires an authenticated account`；Work 模式一直停在「Loading models…」 | 26.1013.40934 起，`config.toml` 的 `[desktop]` 下面是 `conversationDetailMode = "STEPS_PROSE"`（Work 那种段落式的显示）时，桌面版要先确认 ChatGPT 账号身份，用 API Key 取不到，于是所有消息都发不出去。真正的报错只在桌面版日志里（Windows 是 `%LOCALAPPDATA%\Packages\OpenAI.Codex_2p2nqsd0c76g0\LocalCache\Local\Codex\Logs`） | 完全退出桌面版，把这一行改成 `conversationDetailMode = "STEPS_COMMANDS"`（默认值），或者删掉，再打开。代价是不再用段落式显示。办法来自 [#54841](https://github.com/openai/codex/issues/54841#issuecomment-6260844879) 里一位中文用户，另有两位确认管用；没有官方回复。用 CC Switch 接第三方的，还有一个办法：CC Switch → Codex，把连接方式从「直连」换成「路由」，确认路由服务开着、选的还是原来的提供方，再重开桌面版。10 月 23 日 [#54841](https://github.com/openai/codex/issues/54841) 里一位用户这样恢复了，另一位中文用户确认「直连」「聚合」都不行、「路由」可以。只是一直「加载模型」的，LINUX DO 上有人从 Work 模式切回 Codex 模式就好了 |
| 换了中转站或提供方之后，新会话正常，旧会话报 `invalid_encrypted_content`（有的中转站写成 `encrypted content hydration failed`） | Codex 每一轮都会把之前的推理内容原样带上，这部分是加密的，只有当初生成它的那一家能解开，换一家就被拒（[#53713](https://github.com/openai/codex/issues/53713)，还开着） | 旧会话换回原来那家接着用；要用新的一家，就新开会话，把旧会话的文字结论贴过去 |
| 开了 CC Switch 的「路由」模式，日志里有 `websocket_not_supported`、`CC Switch local routing does not support Responses over WebSocket; use HTTP` | CC Switch 的路由模式只转发 HTTP，Codex 先试 WebSocket，被它回了 426。CC Switch 维护者 10 月 16 日的说明：这是有意的，让 Codex 自己退回 HTTP（[cc-switch#8404](https://github.com/farion1231/cc-switch/issues/8404)） | 能正常回复就不用管。一直卡着不回的：`config.toml` 里这个提供方下面的 `supports_websockets` 改成 `false`（Codex 0.164.0 的源码里，自定义提供方不写这一项时默认就是 `false`）。我们没有实测 |
| 中转站或第三方 API 报 `402 Payment Required` | 多半是对方账户余额用完了 | 去对方后台看余额。用 ChatGPT 账号登录也报 402 的，是另一回事，见[按状态码查](#按状态码查400403404429500502503) |

这一节的依据：Codex 一侧的行为，包括请求发到哪、带的是哪个 key、每条报错的原文、哪种写法会退掉 ChatGPT 登录，是我们 10 月 11 日用桌面版自带的 Codex 0.162.0-alpha.17.2，在一个隔离的配置目录里对着本机的模拟接口测出来的（`codex exec` 和交互界面都测了，包括后台服务已经在跑的情况），并对照了 0.162.1 的源码。DeepSeek 一侧的说法，包括支持 Responses 格式、模型名、桌面版和插件共用配置，来自它的官方文档。我们没有用真实的 DeepSeek key 跑过完整对话。

## 常见问题

**Codex 要单独买吗？**
不用。Codex 包含在 ChatGPT 的 Free、Go、Plus、Pro、Business 等套餐里，Free 也能用，只是额度少。用 API Key 登录的话按 API 用量另外计费。

**桌面版、CLI、插件额度是分开的吗？**
不分开。用同一个 ChatGPT 账号登录，额度都从这个账号里扣，ChatGPT Work 也共用同一份额度。

**CLI 里能用的功能，桌面版里没有？**
两边带的 Codex 版本不一定一样，新功能常常先到 CLI。查 CLI 版本用 `codex --version`；Mac 上查桌面版自带的 Codex 版本：

```bash
/Applications/ChatGPT.app/Contents/Resources/codex-cli/bin/codex --version
```

10 月 11 日我们的电脑上，桌面版 26.1007.21159 自带的是 0.162.0-alpha.17.2，npm 上最新的是 0.162.1。VS Code 插件也自带一份，同样可能比你自己装的旧。

**日志在哪？**
Mac 桌面版日志在 `~/Library/Logs/com.openai.codex/`；会话记录在 `~/.codex/sessions`。发给别人前先看一眼，别把 token 带出去。

**归档是什么意思？归档的对话在哪里、怎么恢复、怎么删除？**
归档（繁体中文界面叫「封存」）不是删除：聊天从侧边栏里拿走，内容还在，随时能取消归档。归档的聊天要先取消归档才能接着聊，界面上会提示「此聊天已归档。要继续，请先取消归档。」

- **怎么归档**：在侧边栏里右键这个聊天，选「归档」（Archive）；或者在这个聊天的输入框里输入 `/archive`；快捷键 Cmd + Shift + A（Windows 是 Ctrl + Shift + A）。
- **归档的在哪、怎么恢复**：设置 →「已归档的聊天」（Archived chats），在那里取消归档，聊天会回到侧边栏原来的位置（官方 Troubleshooting）。侧边栏里找不到某个聊天，也先来这里看。
- **怎么删除**：桌面版在 Codex 模式下，侧边栏里右键一个聊天，菜单里有「永久删除」（Permanently delete）。点了会先弹确认框「要删除任务？」（`Delete task?`），说明是「这将永久删除该任务及其子智能体任务。此操作无法撤销。」不确定以后还要不要的，用归档就好。10 月 16 日在 macOS 桌面版 26.1007.21159 上实际看过：Codex 模式下，右键菜单最底下是「归档」和「永久删除」；切到 ChatGPT 模式，同一个菜单里只有归档，没有删除。安装包里还能看到有几种聊天不显示这一项，具体是哪几种没有查清。CLI 里也能删：

```bash
codex archive <会话 ID 或会话名>     # 归档
codex unarchive <会话 ID 或会话名>   # 取消归档
codex delete <会话 ID 或会话名>      # 永久删除，删之前会让你确认
```

会话 ID 在 CLI 里输入 `/status`，看 Session 那一行。`codex delete --force` 不再确认，但只认会话 ID，不认会话名。会话记录在 `~/.codex/sessions`，归档的在 `~/.codex/archived_sessions`。

**打开旧聊天，提示「恢复对话失败」？**
后面会跟着具体原因（英文界面是 `Failed to resume chat`）。常见的两种，提示里已经写了怎么办：

- 「此任务正在其他位置运行。请先在那里关闭会话，然后在此处重试。」（`This task is active elsewhere. Close it there, then retry here.`）同一个聊天在别处开着：另一个窗口、CLI，或者手机远程。关掉那边再试，和[第 5 节](#5-桌面版打不开)的「已在另一个应用中打开」是一类。
- 「ChatGPT 无法加载 config.toml，因此此对话串无法继续。请修复 …」：配置文件写错了，提示里有文件位置和出错的地方。改好保存后，重新打开这个聊天。

**`codex resume` 里看不到以前的会话？**
它默认只列当前项目的会话，也就是你启动 CLI 时所在的那个目录。想看所有项目的，用 `codex resume --all`。这是 OpenAI 的人 10 月 26 日在 [#57002](https://github.com/openai/codex/issues/57002#issuecomment-6409329156) 里的回答；0.164.0 源码里 `--all` 的说明也是这样写的：显示所有会话，不按当前目录过滤，并多显示一列会话所在的目录。

**Codex 能用中国手机号验证吗？**
一般不行。OpenAI 帮助中心的支持名单里没有中国大陆、香港、澳门，验证码也只发短信（12 个国家可以用 WhatsApp）。号码绑定后不能更换，要用一个以后还收得到短信的境外号码，见[第 3 节](#3-要验证手机号)。

**Codex 为什么这么慢？**
常见几种：选的模型和推理档太重（Astra 和高推理档本来就更慢，日常写代码用 GPT-6.1 Sol 的中等推理一般就够）；服务器正忙，10 月 9–14 日、19 日以后都有几波满载，那几天又慢又报错；会话很长，上下文越大越慢，可以新开一个会话或者 `/compact`。想要快，官方有 Fast 模式：CLI 里输入 `/fast` 打开，GPT-6.1 Sol、Astra、GPT-6 Sol、Luna 都支持，代价是按标准速度的 2.5 倍扣套餐额度；更快的 Ultrafast 只有 Pro 500 和部分企业版有（官方 [Speed](https://learn.chatgpt.com/docs/agent-configuration/speed) 文档，10 月 18 日核对）。

如果卡的是桌面版本身（打字、切换对话都卡顿，整台电脑跟着变慢、鼠标都卡），是另一回事。Mac 上打开「活动监视器」，看 `syspolicyd`、`trustd` 是不是在 ChatGPT 打开后占满 CPU、内存涨到几 GB：这是 6 月就有人报的问题，399 个 👍，到现在还开着，没有官方回复，用户试过的办法效果不一（[#25719](https://github.com/openai/codex/issues/25719)）；顺便按下面「磁盘空间莫名少了」那一条查一下崩溃报告目录。Windows 上 OpenAI 的人 8 月说 26.825.32147 修了其中一个原因（沙盒里跑的命令让 `dwm.exe` 的资源越积越多），之后仍有人在新版上卡（[#20214](https://github.com/openai/codex/issues/20214)）；先更新到最新版、重启应用。

**开了两步验证（2FA），还要验证手机号吗？**
要。OpenAI 帮助中心写明，邮箱和验证器 App 都代替不了 Codex 的手机验证。

**`codex exec` 报 `Not inside a trusted directory and --skip-git-repo-check was not specified.`？**
`codex exec`（非交互、写脚本时用的那个）默认只在 Git 仓库里跑，当前目录不在任何仓库里就直接退出（0.163.0 的 `codex-rs/exec/src/lib.rs`）。在项目目录里 `git init` 一下，或者命令里加 `--skip-git-repo-check`。加了 `--yolo` 的也会跳过这个检查，但那是连沙盒和批准一起关掉，别为这个用。

**桌面版字太小，怎么调？**
设置（Cmd + 逗号，Windows 是 Ctrl + 逗号）→「外观」里有三项：「界面字号」（调整 ChatGPT 的基础字号）、「代码字体大小」（聊天和差异视图里的代码）、「内容字体大小」。找不到就在设置的搜索框里输「字号」。字体本身在同一页的「代码字体」里换，官方 Troubleshooting 说字显示不对时就改这一项。

**Mac 的磁盘空间莫名少了几十上百 GB？**
先查一下桌面版的崩溃报告目录。openai/codex 里 6 月就有人报（[#25921](https://github.com/openai/codex/issues/25921)，到 10 月 24 日还开着，没有官方回复）：桌面版的崩溃上报卡住之后，会在后台不停往磁盘写转储文件，应用照常能用，什么提示都没有。报告人一天涨了 4.9 GB；10 月 5 日有人清出 114 GB，10 月 17 日有人在 26.1007.71326 上两个目录加起来 323 GB、四百多万个文件。

```bash
du -sh ~/Library/Application\ Support/Codex/Crashpad ~/Library/Application\ Support/com.openai.codex/web/Crashpad 2>/dev/null
```

只有几十 MB 就不是这个问题。大的话：先完全退出 ChatGPT（Cmd + Q），在「活动监视器」里搜 `crashpad`，确认没有残留的进程；然后在 Finder 里按 Shift + Cmd + G，粘贴 `~/Library/Application Support/Codex/Crashpad`，把里面的 `pending` 文件夹拖进废纸篓，另一个目录同样处理。几乎所有文件都在 `pending` 里，都是本地的崩溃报告，删了不影响使用和对话记录（对话记录在 `~/.codex`）；文件上百万个时，整个文件夹拖进废纸篓比用命令逐个删快得多。官方修好之前还可能再长，隔一阵查一次。这个办法来自 issue 里几位用户（10 月 18 日有人汇总过），不是官方步骤。

**Computer Use 插件不可用、应用内浏览器不可用？**
先按官方 [Computer Use](https://learn.chatgpt.com/docs/computer-use) 文档把三件事过一遍：插件装了没有（Codex 或 Work 里打开「插件」→ Computer Use，提示安装就装，提示启用就启用，里面的服务器和技能两个开关都打开）；Mac 上「系统设置 → 隐私与安全性」里，「屏幕录制」和「辅助功能」有没有给 `Codex Computer Use`；Windows 上要操作的那个应用是不是开在当前桌面上。公司账号的，管理员可以在 `requirements.toml` 里用 `[features].computer_use = false` 整个关掉，这时只能找管理员。设置里浏览器插件写着「已被你的组织停用，或在你所在地区不可用」的，就是这两种原因之一：官方文档对 Computer Use 只写了「在支持的地区」提供，没列名单；云端浏览器写明 Free 和 Go 用不了。走代理的人，地区是按什么判断的官方没说。三件事都对、还是不可用的，是还没修的问题（[#18258](https://github.com/openai/codex/issues/18258)，开了半年，没有官方回复）。

**`/feedback` 提示「未能验证反馈权限。请重试」（`Couldn't verify feedback permissions`）？**
这是反馈功能自己出错了，10 月以来 openai/codex 里有二十多个 issue 报这一句，10 月 16 日以后还有 4 个，没有官方回复。重试几次不行就别耗着：要反馈额度、报错这类问题，截图并记下出错时间，直接到 [help.openai.com](https://help.openai.com) 找客服，或者到 [openai/codex 的 issue](https://github.com/openai/codex/issues) 里搜同样的报错。写的是「你所在的组织已关闭反馈功能」或「反馈功能已在你的配置中关闭」的不一样，是管理员或你自己的配置关掉的。

## 国内怎么开通

没有海外信用卡，常见三条路：

1. **自己用礼品卡订阅**：见我们的两篇实测教程：[ChatGPT Plus 国内充值 / Apple 礼品卡实测](https://github.com/Avarce/chatgpt-plus-china-guide)、[OpenAI Gift Card 购买与兑换](https://github.com/Avarce/openai-gift-card-guide)。
2. **先把几种方法比一遍**：[没有海外信用卡怎么开通 ChatGPT Plus：5 种方法对比](https://aonir.com/guides/chatgpt-plus-without-foreign-card/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_no_card)。
3. **微信充值到本人账号**：AONIR 提供 [ChatGPT Plus ¥168 / 月](https://aonir.com/chatgpt-plus/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=cta_plus)、[ChatGPT Pro 100（5×）¥730 / 月](https://aonir.com/chatgpt-pro/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=cta_pro_5x)、[Pro 200（10×）¥1400 / 月](https://aonir.com/chatgpt-pro-10x/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=cta_pro_10x)，都能微信自助充值；全天高强度用的还有 [Pro 500 ¥3500 / 月](https://aonir.com/chatgpt-pro-500/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=cta_pro_500)（客服开通）。都是充到你自己的账号，Codex 额度跟着套餐走。

有海外卡的，直接在官方订阅最划算。

## 投稿一条报错

遇到这页没收的报错，或者文中的办法你试了不管用，最省事的是到[集中反馈帖](https://github.com/Avarce/codex-china-guide/issues/1)下面留一句；也可以[单独开一个 Issue](../../issues/new?template=error-report.md)。贴上报错原文、你用的是桌面版 / CLI / 插件、版本号、系统，以及你怎么解决的（没解决也可以发）。

贴之前把 `auth.json` 的内容、token、邮箱和手机号删掉。收到后我们核对，补进这页，并记在[更新日志](CHANGELOG.md)里。

## 官方来源

- ChatGPT Learn：[Authentication](https://learn.chatgpt.com/docs/auth) · [Troubleshooting](https://learn.chatgpt.com/docs/reference/troubleshooting) · [Environment variables](https://learn.chatgpt.com/docs/config-file/environment-variables) · [Configuration Reference](https://learn.chatgpt.com/docs/config-file/config-reference)
- ChatGPT Learn：[Windows sandbox](https://learn.chatgpt.com/docs/windows/windows-sandbox) · [Settings](https://learn.chatgpt.com/docs/reference/settings) · [Codex IDE extension](https://learn.chatgpt.com/docs/codex/ide)
- ChatGPT Learn：[Sandbox](https://learn.chatgpt.com/docs/sandboxing) · [Agent approvals & security](https://learn.chatgpt.com/docs/agent-approvals-security) · [Permissions](https://learn.chatgpt.com/docs/permissions) · [Managed configuration](https://learn.chatgpt.com/docs/enterprise/managed-configuration) · [Remote connections](https://learn.chatgpt.com/docs/remote-connections)
- ChatGPT Learn：[Codex CLI](https://learn.chatgpt.com/docs/codex/cli) · [ChatGPT desktop app](https://learn.chatgpt.com/docs/app) · [Custom instructions with AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md) · [Pricing](https://learn.chatgpt.com/docs/pricing) · [Models](https://learn.chatgpt.com/docs/models)
- OpenAI Help Center：[What does phone verification look like?](https://help.openai.com/en/articles/8983040-what-does-phone-verification-look-like) · [哪些号码不能用](https://help.openai.com/en/articles/8983024) · [WhatsApp 验证支持的国家](https://help.openai.com/en/articles/8983038-which-countries-do-you-support-for-whatsapp-phone-verification) · [不支持改号](https://help.openai.com/en/articles/9135134-how-to-change-the-phone-number-associated-with-your-account) · [支持的国家和地区](https://help.openai.com/en/articles/7947663-chatgpt-supported-countries)
- OpenAI Help Center：[Troubleshooting Model Feature Access Issues](https://help.openai.com/en/articles/10258669-troubleshooting-model-feature-access-issues) · [How resets work in Codex](https://help.openai.com/en/articles/20001498-how-resets-work-in-codex) · [Paid weekly Work and Codex rate limit resets](https://help.openai.com/en/articles/20001507-paid-weekly-work-and-codex-rate-limit-resets)
- ChatGPT Learn：[Computer Use](https://learn.chatgpt.com/docs/computer-use) · [Browser](https://learn.chatgpt.com/docs/browser) · [Cyber safety](https://learn.chatgpt.com/docs/cyber-safety)
- OpenAI Help Center：[Troubleshooting OpenAI Daybreak](https://help.openai.com/en/articles/20001259-troubleshooting-openai-daybreak) · [Troubleshooting OpenAI account deactivation](https://help.openai.com/en/articles/10562188-troubleshooting-openai-account-deactivation)
- ChatGPT Learn：[Changelog](https://learn.chatgpt.com/docs/changelog)（版本和模型的发布日期）
- GitHub：[openai/codex](https://github.com/openai/codex)，文中引用的 issue 都附了编号链接；各版本改了什么见 [Releases](https://github.com/openai/codex/releases)
- [OpenAI 状态页](https://status.openai.com)
- 第三方模型：ChatGPT Learn 的 [Advanced Configuration](https://learn.chatgpt.com/docs/config-file/config-advanced)（Custom model providers、Profiles）· DeepSeek 官方文档 [接入 Codex](https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/codex) · [CC Switch](https://github.com/farion1231/cc-switch)
- 安装、更新和卸载的位置：openai/codex 仓库 `rust-v0.162.1` 标签下的 `scripts/install/` 和 `codex-rs/tui/src/update_action.rs`；安装脚本先走 `releases.openai.com`、不通再走 GitHub 的逻辑，对照的是 0.164.0 正式版附带的 `install.sh`（和 `chatgpt.com/codex/install.sh` 下载到的逐字节相同）

中文界面的文字取自 ChatGPT 桌面版（macOS）自带的简体中文语言包，CLI 的报错和命令对照 openai/codex 对应版本的源码和帮助输出；每一条取自哪一版，见[更新日志](CHANGELOG.md#出处中文界面文字和命令原文取自哪一版)最后的「出处」。「最近两周的已知问题」里的状态，取自各 issue 在 10 月 26 日的开关状态和 OpenAI 员工的回复，官方事故以状态页的记录为准；标了「用户报告」「社区办法」的没有经过官方确认。

## 转载与许可

本页内容采用 [CC BY 4.0](LICENSE) 许可：可以转载、翻译、改编，注明出处并附上本仓库链接即可。Codex、ChatGPT、OpenAI 是 OpenAI 的商标，本仓库与 OpenAI 没有隶属关系。
