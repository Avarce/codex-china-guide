# Codex 国内使用与报错速查（2026）：安装、登录、手机号、代理、额度

> **最后核对：2026 年 10 月 11 日**（北京时间）· Codex CLI 0.162.1 · 桌面版 26.1007
> 遇到报错，直接 Ctrl + F 搜报错里的几个词。终端、英文界面、中文界面三种写法都收了。
> 觉得有用就点右上角 ⭐ Star，下次报错不用再到处搜。

![Codex 国内使用与报错速查：按报错原文查原因和处理步骤](images/cover.webp)

国内用 Codex，卡住的地方基本就那几处：装不上、打不开、登录跳不回来、要验证手机号、终端连不上、用着用着断线、额度用完。这页按「你看到的报错」来排，每条写清楚多半是什么原因、按什么顺序处理，并附上官方文档或 openai/codex 仓库里维护者回复的出处，可以自己点进去核对。

由 [AONIR](https://aonir.com/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=brand_home) 整理维护。我们提供 ChatGPT / Claude 会员充值服务，和 OpenAI 没有隶属关系；这页只讲 Codex 本身，充值相关只放在[最后一节](#国内怎么开通)。

**这几天问得最多的（10 月 11 日）：**

- 桌面版新对话发不出去，提示「无法加载工作空间设置」，还没修：[看这里](#无法加载工作空间设置couldnt-load-workspace-settings)
- 10 月 9 日起每条消息都是 `Selected model is at capacity`，额度明明还有：[看这里](#模型满载selected-model-is-at-capacity)
- Windows 上所有命令都跑不了，`setup refresh had errors`，桌面版已修、CLI 没修：[看这里](#windows-沙盒setup-refresh-had-errors-os-error-32)
- GPT-5.5 在 10 月 14 日下线，设置里写死 `gpt-5.5` 的要改：[看这里](#模型不支持model-is-not-supported)

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
- [常见问题](#常见问题)
- [国内怎么开通](#国内怎么开通)
- [官方来源](#官方来源) · [更新日志](CHANGELOG.md) · [投稿一条报错](#投稿一条报错)

## 按报错找

| 你看到的 | 多半是 | 看这里 |
| --- | --- | --- |
| `stream disconnected before completion`、`Reconnecting... 1/5`、正在重新连接 1/5（重连 5 次后失败）、一直重新连接、服务器繁忙，正在重新连接 | 网络或代理 | [断线重连](#断线重连stream-disconnected-before-completion) |
| `error sending request for url (https://chatgpt.com/backend-api/codex/...)` | 终端没走代理 | [断线重连](#断线重连stream-disconnected-before-completion) |
| 浏览器显示登录成功，终端一直在等 | 本机回调被挡 | [2. 登录](#2-登录) |
| `Token exchange failed` | 登录时网络不通 | [登录失败](#登录失败token-exchange-failed) |
| `refresh token was already used`、此设备上的 ChatGPT 会话已过期 | 登录凭证失效 | [登录过期](#登录过期refresh-token-was-already-used) |
| `Verify your phone number`、`Phone number required` | 手机号验证 | [3. 要验证手机号](#3-要验证手机号) |
| `unsupported_country_region_territory`、我们的服务在你所在的国家或地区不可用 | 节点所在地区 | [地区不支持](#地区不支持unsupported_country_region_territory) |
| `Selected model is at capacity`，会话日志里是 `server_overloaded` | 服务器满载（10 月 9–10 日有一波），或账号被临时限制 | [模型满载](#模型满载selected-model-is-at-capacity) |
| `The '...' model is not supported when using Codex with a ChatGPT account` | 模型名写错、旧设置或还没开放 | [模型不支持](#模型不支持model-is-not-supported) |
| `exceeded retry limit, last status: 429 Too Many Requests` | 官方故障或请求太多 | [429](#请求太多429-too-many-requests) |
| `unexpected status 401 Unauthorized`、`Incorrect API key provided: sk-svcac…` | 官方故障，或登录失效 | [401](#登录失效或官方故障401-unauthorized) |
| `start the Windows daemon from a non-elevated terminal`、`host Job Object prevents daemon detachment` | 0.157 起新增的后台服务 | [后台服务报错](#后台服务daemon报错) |
| Windows 上终端窗口一闪一闪 | 旧版后台服务的问题，已修 | [后台服务报错](#后台服务daemon报错) |
| 改了代理，终端里还是连不上 | 后台服务还在用旧的代理设置 | [4. 国内网络](#4-国内网络终端要单独设代理) |
| Mac 终端里 Cmd + C 复制不了 | CLI 改成了全屏界面 | [复制不了文字](#终端里复制不了文字) |
| `ran out of room in the model's context window` | 这个会话的上下文满了 | [上下文满了](#上下文满了ran-out-of-room-in-the-models-context-window) |
| 证书错误、`certificate` | 公司网络或抓包软件 | [证书错误](#证书错误公司网络抓包软件) |
| `You've hit your usage limit`、你已达到使用上限 | 额度用完 | [7. 额度用完了](#7-额度用完了) |
| Windows 设置未完成（旧版本写的是「Windows 安装未完成」）、设置无法完成、`helper_failed`、完成 Windows 设置以继续 | Windows 沙盒没装好 | [Windows 设置未完成](#windows-设置未完成helper_failed) |
| 已在另一个应用中打开 | 这个会话在别处开着 | [5. 桌面版打不开](#5-桌面版打不开) |
| 更新后打不开、只有启动动画、窗口空白 | 多半是新版本的问题 | [更新后打不开](#更新后打不开只有启动动画或白屏) |
| `Unable to load organization settings`、一直停在 `Starting your task`（正在启动你的任务） | 桌面版 26.924 的问题 | [更新后打不开](#更新后打不开只有启动动画或白屏) |
| 无法加载工作空间设置、请重试以继续聊天、`Couldn't load workspace settings`，新对话的发送按钮是灰的 | 桌面版 10 月 7 日那一版起的问题，还没修完 | [无法加载工作空间设置](#无法加载工作空间设置couldnt-load-workspace-settings) |
| 无法检查锁定模式、`Couldn't check Lockdown mode`，日志里 `cloudflare_challenge` 或 `devicecheck_token_generation_failed` | 同上，桌面版取账号设置的请求失败了 | [无法加载工作空间设置](#无法加载工作空间设置couldnt-load-workspace-settings) |
| `helper_unknown_error: setup refresh had errors`、`trusted Node process exited unexpectedly`、日志里 `os error 32` | Windows 沙盒的问题，10 月 7 日那一版开始 | [Windows 沙盒](#windows-沙盒setup-refresh-had-errors-os-error-32) |
| `Failed to create unified exec process: No such file or directory (os error 2)` | Windows 桌面版开了 Run agent in WSL | [WSL](#wslfailed-to-create-unified-exec-process-os-error-2) |
| `Error submitting message`、`Unable to determine project root for task` | Codex 网页版云端任务的问题，还没修 | [网页版](#网页版unable-to-determine-project-root-for-task) |
| `Server is draining; retry after reconnecting`、`Background server has incompatible feature settings` | 后台服务自己更新了 | [后台服务报错](#后台服务daemon报错) |
| VS Code 里语音输入一闪就停，显示 `Dictation stopped`，日志里 `HTTP 403 Forbidden` | 插件的语音接口被拒，还没修 | [最近的已知问题](#最近两周的已知问题) |
| `failed to open daemon process`、`Access is denied. (os error 5)` | 后台服务留下的进程号过期了 | [后台服务报错](#后台服务daemon报错) |
| `failed to send websocket request`、`os error 10054` / `10053`，只有某个旧会话一直重连 | 这个会话的历史里图片太多，请求太大 | [断线重连](#断线重连stream-disconnected-before-completion) |
| `untrusted-process-ancestry`，内置浏览器或浏览器控制用不了 | macOS 上的问题，还没修 | [最近的已知问题](#最近两周的已知问题) |
| `account/read failed during TUI bootstrap`、`workspace routing discovery failed`、`failed to load workspace requirements` | 后台服务版本旧了，或者还记着旧的代理端口 | [后台服务报错](#后台服务daemon报错) |
| Windows 桌面版登录后几十秒自己退出，没有任何提示 | 10 月 7 日那一版装运行时组件时崩溃 | [登录后自己退出](#windows-桌面版登录后几十秒自己退出) |
| VS Code 日志里 `Failed to release queued message send lock`、`"undefined" is not valid JSON` | 插件 10 月 1 日那一版的问题，已修 | [最近的已知问题](#最近两周的已知问题) |
| `You have reached your Codex usage limits for code reviews` | GitHub 代码审查有单独的额度 | [最近的已知问题](#最近两周的已知问题) |
| `mountinfo path is not absolute` | Linux 桌面版 9 月下旬几个版本的沙盒问题 | [最近的已知问题](#最近两周的已知问题) |
| 界面是英文，想切成中文 | 设置里就能改 | [8. 怎么设置中文](#8-怎么设置中文) |

不知道是哪一类，先在终端跑一次 `codex doctor`。它会检查安装、配置、登录和网络，每一项打 ✓ 或 ⚠，最后一栏 Connectivity 就是网络情况。

## 最近两周的已知问题

9 月 25 日的 CLI 0.157.0 改动很大：默认启动一个后台服务（daemon），界面改成全屏。9 月 24 日到 10 月 3 日，openai/codex 新开的 issue 里有 280 个提到 daemon。下面是 9 月 24 日以来反馈最多的问题，状态核对到 10 月 11 日，10 月新出的排在前面。标着「没修」的，10 月 11 日都回到原 issue 逐条复核过，仍然开着。先看你的问题是不是已经修了，升级就能解决的不用再折腾。

| 现象 | 出在哪个版本 | 现在的状态 | 怎么办 |
| --- | --- | --- | --- |
| 桌面版新对话发不出去：「无法加载工作空间设置 / 请重试以继续聊天」（`Couldn't load workspace settings`），旧对话和网页版正常 | Windows、macOS 桌面版 26.1002 起（10 月 7 日） | 没修。10 月 11 日 Windows 26.1007.21434、macOS 26.1007.21159 上仍有人遇到，至少 15 个 issue，没有官方回复（[#51917](https://github.com/openai/codex/issues/51917)、[#52470](https://github.com/openai/codex/issues/52470)） | 见[无法加载工作空间设置](#无法加载工作空间设置couldnt-load-workspace-settings) |
| 每条消息都报 `Selected model is at capacity`，换模型没用，周额度几乎没动；会话日志里是 `server_overloaded` | 10 月 9–10 日，发帖的人大多在东八区；桌面版、CLI、网页版都有 | 没有官方回复，状态页这两天也没有对应的事故记录；10 月 11 日相关 issue 都还开着（[#52796](https://github.com/openai/codex/issues/52796)、[#52394](https://github.com/openai/codex/issues/52394)） | 见[模型满载](#模型满载selected-model-is-at-capacity) |
| Windows 桌面版登录后 10–35 秒自己退出，没有报错窗口 | 桌面版 26.1002.7124.0（10 月 7 日） | 一位用户查到原因并给出办法，帖子已标记为解决（[OpenAI 社区](https://community.openai.com/t/1404464)，10 月 8 日） | 见[登录后自己退出](#windows-桌面版登录后几十秒自己退出) |
| CLI 启动就退出：`account/read failed during TUI bootstrap … workspace routing discovery failed (code -32603)` | CLI 0.162.0，走本地代理 | 发帖人确认重启后台服务后解决（[OpenAI 社区](https://community.openai.com/t/1404666)，10 月 9 日） | `codex app-server daemon restart`，见[后台服务报错](#后台服务daemon报错) |
| 重置卡到了到期那天，没到晚上就不见了 | 所有版本 | **官方答复**（OpenAI_Support，10 月 6 日，[OpenAI 社区](https://community.openai.com/t/1403334)）：每张卡有具体的到期时间，过期不能恢复 | 网页版 Settings → Usage 里，把鼠标悬停在到期日期上能看到具体时间和时区。没到时间就消失的，找客服查 |
| GitHub 上 `@codex review` 被拒：`You have reached your Codex usage limits for code reviews`，Usage 页面的周额度却还有 | 9 月 29 日起有人反馈 | 没有官方回复（[OpenAI 社区](https://community.openai.com/t/1402074)）。代码审查的用量是单独算的 | 帖子里用户的办法：账号里有 credits 的，在 Code review 设置里打开 use credits。这个办法不是都管用：10 月 9 日有人开着这个设置、credits 余额也是正的，照样被拒（[#52549](https://github.com/openai/codex/issues/52549)） |
| Windows 上所有命令都在执行前失败：`helper_unknown_error: setup refresh had errors`，日志里是 `os error 32` | Windows 桌面版 26.1002（10 月 7 日）；CLI 0.162.0、0.162.1 | 桌面版：10 月 9–10 日多位用户确认 26.1007.21434 **已修**（[#51601](https://github.com/openai/codex/issues/51601)，120 条回复）。CLI：10 月 10 日仍有人在 0.162.1 上复现（[#51634](https://github.com/openai/codex/issues/51634)） | 桌面版更新到 26.1007.21434；CLI 见 [Windows 沙盒](#windows-沙盒setup-refresh-had-errors-os-error-32) |
| Windows 桌面版开了 Run agent in WSL，每条命令都是 `Failed to create unified exec process: No such file or directory (os error 2)` | 桌面版 26.928 起 | 没修，10 月 11 日有人在 26.1007.21434 上仍遇到（[#49731](https://github.com/openai/codex/issues/49731)）。10 月 10 日一位用户查到了原因 | 用户给了挂载目录的办法，10 月 10 日补了重启后不失效的写法，见 [WSL](#wslfailed-to-create-unified-exec-process-os-error-2) |
| Codex 网页版第一条消息发不出去：`Unable to determine project root for task` | 9 月 30 日起，云端环境 | 没修，还没有官方回复（[#49497](https://github.com/openai/codex/issues/49497)） | 见[网页版](#网页版unable-to-determine-project-root-for-task) |
| 会话突然断开：`Server is draining; retry after reconnecting`，恢复时提示 `Background server has incompatible feature settings` | 后台服务自动更新的时候 | 没修（[#51763](https://github.com/openai/codex/issues/51763)，10 月 7 日） | 按提示重启后台服务，见[后台服务报错](#后台服务daemon报错) |
| VS Code 插件语音输入显示 `Dictation stopped`，日志里 `/transcribe` 返回 `HTTP 403 Forbidden` | 插件 26.917.62051 起 | 官方没修（[#49351](https://github.com/openai/codex/issues/49351)），10 月 9 日有用户确认了一个办法 | VS Code 用户设置里加 `"http.electronFetch": true`，重新加载窗口。不行的话，语音先用桌面版 |
| Linux 桌面版更新后命令都跑不了，沙盒报 `mountinfo path is not absolute` | Linux 桌面版 26.917.51856 起（9 月 22 日） | issue 9 月 24 日关闭，但到 9 月 28 日仍有人说新版还有；10 月 4 日有用户确认 26.930.41038 正常（[#47345](https://github.com/openai/codex/issues/47345)，51 条回复） | 更新到 26.930 以上；更新前只能退回旧版，旧版没有 GPT-6 Sol |
| Tibo 说重置已全部到账，自己的额度却没变 | 10 月 2 日那次全体重置 | 没有官方回复（[#50451](https://github.com/openai/codex/issues/50451)）。Pro 500、Business 都有人反馈，Tibo 当晚说 Pro 500 已补上 | 先刷新 Usage 页面、确认账号和工作区；过一两天还没有，找 OpenAI 客服并附 Usage 截图 |
| 套餐额度用完后，桌面版输入框直接变灰发不出去，即使模型走的是自己配置的 provider | 桌面版 26.924 起 | 没修（[#48816](https://github.com/openai/codex/issues/48816)，另有 [#48650](https://github.com/openai/codex/issues/48650)、[#48369](https://github.com/openai/codex/issues/48369)） | 等套餐额度恢复，或等官方修复 |
| 内置浏览器或浏览器控制用不了，报 `untrusted-process-ancestry` | macOS，9 月 27 日起 | 没修（[#48786](https://github.com/openai/codex/issues/48786)、[#49301](https://github.com/openai/codex/issues/49301)）；10 月 10 日 [LINUX DO](https://linux.do/t/topic/3002787) 上也有人问，没有人给出办法 | 暂时没有办法，等修复 |
| dot 建不了本地任务，提示 `not attached to an active Aeon`，或者在已保存的项目里没法接着做 | Pro 的 dots，9 月 30 日起 | 没修（[#51730](https://github.com/openai/codex/issues/51730)、[#49729](https://github.com/openai/codex/issues/49729)） | 先在 Codex 里自己建任务 |
| Windows 上每发一次请求，终端窗口就闪几下 | CLI 0.157.0 起 | **已修**，0.159.2（[#48074](https://github.com/openai/codex/issues/48074)，这段时间反馈最多的一条） | `codex update`；升级后还闪，在会话里输入 `/daemon` 把后台服务也更新 |
| `host Job Object prevents daemon detachment`，CLI 启动不了 | CLI 0.157.0，Windows | **已修**，0.157.1（[#48016](https://github.com/openai/codex/issues/48016)） | `codex update` |
| `start the Windows daemon from a non-elevated terminal` | CLI 0.157.0 起，Windows 管理员终端 | 没修，10 月 7 日还有人在问（[#48043](https://github.com/openai/codex/issues/48043)） | 用普通（非管理员）终端打开，或者 `codex --no-daemon`。报的是 `failed to open daemon process` 的，见[后台服务报错](#后台服务daemon报错) |
| `401 Unauthorized: Incorrect API key provided: sk-svcac…`，明明是用 ChatGPT 登录的 | 所有版本 | 官方故障，北京时间 9 月 26 日 06:58–07:54，**已恢复**（[#48237](https://github.com/openai/codex/issues/48237)） | 不用重新登录，见 [401](#登录失效或官方故障401-unauthorized) |
| Mac 自带终端里 Cmd + C 复制不了 | CLI 0.157.0 起 | 官方不改，是 Terminal.app 的限制（[#48122](https://github.com/openai/codex/issues/48122)） | 见[复制不了文字](#终端里复制不了文字) |
| VS Code 插件按回车后消息没了，Codex 也不回；日志里是 `Failed to release queued message send lock`、`"undefined" is not valid JSON` | 插件 26.928.31416（10 月 1 日更新） | **已修**，10 月 8 日有用户确认新版正常，issue 已关闭（[#49988](https://github.com/openai/codex/issues/49988)）。另一个「上一轮结束后新消息一直排队」的问题还开着（[#50118](https://github.com/openai/codex/issues/50118)） | 更新插件；还卡住就重启 VS Code |
| VS Code 插件的模型列表里没有 GPT-6.1 Sol | 插件 26.917.62051（自带的 CLI 太旧） | 10 月 1 日已关闭（[#49464](https://github.com/openai/codex/issues/49464)） | 新版插件自带 CLI 0.159.2，有这个模型，但有上一行的问题；留在旧版的，可以按 issue 里的办法把 `chatgpt.cliExecutable` 指向自己装的新版 CLI |
| Linux 桌面版任务一直停在 `Starting your task` | 桌面版 26.924 | **已修**，官方 9 月 29 日回复更新即可（[#48189](https://github.com/openai/codex/issues/48189)） | 更新到最新版 |
| Windows 桌面版 `Unable to load organization settings`，或更新后一直转圈 | 桌面版 26.924 | 没修，10 月 6 日有人在 26.930.6422.0 上仍复现（[#48324](https://github.com/openai/codex/issues/48324)、[#48463](https://github.com/openai/codex/issues/48463)） | 见[更新后打不开](#更新后打不开只有启动动画或白屏) |
| Mac 桌面版白屏 | 桌面版 26.915 | 没修，9 月 20 日以后 issue 没有新进展（[#46641](https://github.com/openai/codex/issues/46641)） | 同上 |
| 安卓手机连电脑（Codex Remote），一直回到 `Authorize this phone` | ChatGPT 安卓版 1.2026.265 | 没确认修好：10 月 4 日有人说安卓新版已经好了，也有人更新后还是不行（[#48774](https://github.com/openai/codex/issues/48774)、[#48555](https://github.com/openai/codex/issues/48555)） | 先把安卓版 ChatGPT 更新到最新试试。#48555 是电脑端换过 ChatGPT 账号之后出现的 |

还有一件有日期的事：**GPT-5.5 在 10 月 14 日从 ChatGPT 和 Codex 下线**，设置或脚本里写死了 `gpt-5.5` 的要提前改，见[模型不支持](#模型不支持model-is-not-supported)。

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

CLI 选一种装法就行。官方安装脚本要从 chatgpt.com 下载，国内经常卡住，**用 npm 国内镜像最省事**（需要 Node.js 16+）：

```bash
# npm 国内镜像（10 月 11 日核对：镜像版本和 npm 官方一致，都是 0.162.1；Windows 上 0.162 有沙盒问题，见「Windows 沙盒」一节）
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

## 2. 登录

第一次运行 `codex`，或者第一次打开桌面版、插件，都会让你选登录方式：

- **Sign in with ChatGPT**（中文界面：通过 ChatGPT 登录）：用你 ChatGPT 套餐里的 Codex 额度。大多数人选这个。
- **API Key**：按 API 用量另外计费，不走套餐额度；Codex 云端任务必须用 ChatGPT 登录。

登录会打开浏览器，你登录完，浏览器再把凭证交回本机的 Codex（默认走 `localhost:1455`）。**浏览器显示成功、终端却一直在等**，通常是这个本机回调被网络设置挡住了，或者你是在远程服务器上装的。这时改用设备码登录：

1. 先在 ChatGPT 网页版的安全设置里打开设备码登录（Device code），不开这一步会失败。
2. 运行 `codex login --device-auth`，或在登录界面选 Sign in with Device Code。
3. 打开它给的链接，登录后输入一次性代码。

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

还有一条：**节点尽量固定，别频繁切换地区。** OpenAI 帮助中心把「从陌生的地点登录」列为账号被临时限制的安全原因之一。

## 5. 桌面版打不开

先看是哪一种：

| 你看到的 | 多半是 | 怎么办 |
| --- | --- | --- |
| Windows 设置未完成（旧版本写的是「Windows 安装未完成」）、设置无法完成、`helper_failed`、完成 Windows 设置以继续 | Windows 沙盒没装好 | [看下面](#windows-设置未完成helper_failed) |
| 已在另一个应用中打开 | 这个会话还在终端、编辑器或另一个窗口里开着 | 在那边关掉这个会话，回来点「重试」 |
| 更新后双击没反应、只有启动动画、窗口一片空白 | 多半是新版本的问题 | [看下面](#更新后打不开只有启动动画或白屏) |
| `Unable to load organization settings`，点重试没用 | Windows 桌面版 26.924 的问题，官方还没修 | [看下面](#更新后打不开只有启动动画或白屏) |
| 能打开，但新对话发不出去：无法加载工作空间设置（`Couldn't load workspace settings`） | 桌面版 10 月 7 日那一版起的问题，还没修完 | [看下面](#无法加载工作空间设置couldnt-load-workspace-settings) |
| 任务一直停在 `Starting your task`（中文界面：正在启动你的任务） | Linux 桌面版 26.924 的问题，已修 | 更新到最新版 |
| 能打开，但登录页出不来、一直转圈 | 网络 | [第 4 节](#4-国内网络终端要单独设代理) |

### Windows 设置未完成（helper_failed）

Windows 版的 Codex 要在你电脑上改代码、跑命令，先得建一个沙盒（隔离环境），这一步需要一次管理员授权。界面会提示「完成 Windows 设置以继续」，失败时显示「Windows 设置未完成」（旧版本写的是「Windows 安装未完成」），下面一行小字是原因：「设置无法完成」对应错误码 `helper_failed`，「设置超时」对应 `setup_timeout`，另外还有「审批已取消」「启动设置未成功」。

官方文档列的常见原因：弹出「用户账户控制」时点了「否」；电脑不允许创建本地用户和组、不允许改防火墙；公司的管理策略挡住了其中一步。

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

这类问题大多出在新版本本身，而且往往很多人同时遇到：

- 8 月 26 日，Windows 版更新到 26.820 后不少人打不开，报 `Unable to locate Codex CLI`，官方回复正在加急修（[#40752](https://github.com/openai/codex/issues/40752)、[#40700](https://github.com/openai/codex/issues/40700)）。
- 6 月的 26.609 版也出现过更新后打不开，官方按高优先级处理，已经修复（[#27979](https://github.com/openai/codex/issues/27979)）。
- 9 月的 26.915 版，Mac 上有白屏报告，issue 到 10 月 3 日还开着（[#46641](https://github.com/openai/codex/issues/46641)）。
- 9 月 25 日前后的 26.924 版：Linux 上任务一直停在 `Starting your task`，官方 9 月 29 日回复已修复，更新即可（[#48189](https://github.com/openai/codex/issues/48189)）。Windows 上有人一打开 Codex 就弹 `Unable to load organization settings`（[#48324](https://github.com/openai/codex/issues/48324)），也有人更新后一直转圈（[#48463](https://github.com/openai/codex/issues/48463)、[#48333](https://github.com/openai/codex/issues/48333)），这几条官方还没回复。

按顺序试：

1. 彻底退出再打开：Windows 在任务管理器里结束 ChatGPT（旧版叫 Codex）的进程，后台残留的 `codex.exe` 也一起结束（#48333 的报告人就是结束它之后才进去的）；Mac 按 Cmd + Q。
2. 重启电脑。
3. 检查更新。已知问题一般靠新版本修，Windows 可以在 Microsoft Store 里看有没有更新。
4. Windows：设置 → 应用 → 已安装的应用 → ChatGPT → 高级选项，先点「修复」（不删数据）；不行再点「重置」（会清掉应用自己的数据，要重新登录）。
5. 到 [openai/codex 的 issue](https://github.com/openai/codex/issues) 里搜你的版本号和现象，看是不是已知问题。

> issue 里常有人贴「临时办法」，比如改 `CODEX_CLI_PATH`、从第三方镜像降级。这些官方都没认可，[#40752](https://github.com/openai/codex/issues/40752) 里就有人反馈改完之后历史会话打不开。能等就等官方修。

### 无法加载工作空间设置（Couldn't load workspace settings）

```text
无法加载工作空间设置
请重试以继续聊天
```

英文界面是 `Couldn't load workspace settings` / `Retry to continue chatting`。新建对话时输入框上方出现这条提示，发送按钮是灰的，点「重试」没用；已有的对话还能接着发，同一个账号在网页版也正常。有时一起出现的还有「无法检查锁定模式」（`Couldn't check Lockdown mode`）。

10 月 7 日桌面版更新之后开始出现。到 10 月 11 日，openai/codex 里至少有 15 个 issue 在报，都没有官方回复；Windows 26.1007.21434、macOS 26.1007.21159 这两个最新版上仍有人遇到。

**不是你的工作空间或项目设置坏了。** 桌面版新建对话前，要先向 chatgpt.com 取一次账号设置（日志里是 `/settings/user`、`/accounts/check`），这一步失败就不让发消息。个人账号也会遇到，和有没有加入团队工作空间没关系。用户从日志里查到三种情况：

| 日志里的错误 | 出在哪 | 状态 |
| --- | --- | --- |
| `cloudflare_challenge`，HTTP 403 | Windows 居多，macOS 也有。这个请求被 Cloudflare 的人机验证拦下了；同一时间网页版能正常打开 | 没修（[#51917](https://github.com/openai/codex/issues/51917)、[#51691](https://github.com/openai/codex/issues/51691)、[#52790](https://github.com/openai/codex/issues/52790)、[#52874](https://github.com/openai/codex/issues/52874)） |
| `devicecheck_token_generation_failed` | macOS。应用向苹果的 DeviceCheck 要设备凭证失败。虚拟机里的 macOS 反馈最多，实体的 M1、M2 机器也有人遇到 | 没修（[#52001](https://github.com/openai/codex/issues/52001)、[#52342](https://github.com/openai/codex/issues/52342)、[#52470](https://github.com/openai/codex/issues/52470)、[#51667](https://github.com/openai/codex/issues/51667)） |
| 没有别的提示，就是发送按钮一直灰着 | 10 月 7 日的 26.1002.51308 | **已修**。这一版有[官方事故记录](https://status.openai.com/incidents/01M4ADHMMBK7K92NJFJ0DXRYJM)，当天发了修复版，更新就好 |

想确认是哪一种，到日志里搜 `cloudflare_challenge` 或 `devicecheck`：Mac 的日志在 `~/Library/Logs/com.openai.codex/`，Windows 在 `%LOCALAPPDATA%\Packages\OpenAI.Codex_2p2nqsd0c76g0\LocalCache\Local\Codex\Logs`。

下面的办法都来自 issue 里的用户，不是官方方案；几乎每一条都既有人说管用、也有人说没用。从省事的开始试：

1. **更新到最新版，完全退出再打开。** 10 月 7 日那一版的问题这样就能解决。[#52437](https://github.com/openai/codex/issues/52437) 里有人完全重启应用后恢复；[#51917](https://github.com/openai/codex/issues/51917) 里有人什么都没动，等了半小时自己好了。
2. **新对话先选一个项目。** [#51917](https://github.com/openai/codex/issues/51917) 里有人发现，被拦的只有「这台电脑、不选项目」的新对话，选一个已有的项目就能发。
3. **退出登录再登录。** [#52790](https://github.com/openai/codex/issues/52790) 的报告人这样能恢复，但过一阵又会出现。
4. **从旧对话里新开一个。** 打开一个还能发消息的旧对话，让 Codex 自己建：「用内置的 create_thread 工具，在这个项目里新建一个本地对话，先打个招呼。」Mac 上有用户确认可行（[#52470](https://github.com/openai/codex/issues/52470#issuecomment-6105237946)、[#52001](https://github.com/openai/codex/issues/52001)）。
5. **先用网页版或 CLI。** 发帖的人大多提到同一个账号在网页版正常，CLI 也不受影响（[#52342](https://github.com/openai/codex/issues/52342)）。
6. **退回上一个版本。** Mac 上有人退回 26.930.61225 后恢复（[#52001](https://github.com/openai/codex/issues/52001)），Windows 上有人退回 26.928 后能新建对话（[#51917](https://github.com/openai/codex/issues/51917)）。这只是权宜之计，官方修好后记得升回来。

走代理的用户多留意一点：`cloudflare_challenge` 说明 Cloudflare 对这次请求不放心，要先做人机验证。换一个固定的节点可以试，但别指望一定有用，[#52790](https://github.com/openai/codex/issues/52790)、[#51917](https://github.com/openai/codex/issues/51917) 里都有人换过节点和 IP，没解决。还有一位用户是在应用的内置浏览器里打开了一次 chatgpt.com，等验证页自己过去，再回来点「重试」后恢复的；他自己也说不确定是不是这一步起的作用（[#51691](https://github.com/openai/codex/issues/51691)）。

## 6. 报错逐条处理

### 断线重连：stream disconnected before completion

```text
stream disconnected before completion: error sending request for url (https://chatgpt.com/backend-api/codex/responses)
Reconnecting... 2/5
```

中文界面显示「正在重新连接 1/5」，一路数到「5/5」后失败；也可能是「服务器繁忙，正在重新连接」。

**多半是网络问题。** openai/codex 的维护者排查过一批这类报告，结论是大多是网络连通性问题：网络不稳、VPN、代理、防火墙；也有网络把 WebSocket 挡了的（[#14209](https://github.com/openai/codex/issues/14209)、[#13245](https://github.com/openai/codex/issues/13245)）。

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

### 登录失败：Token exchange failed

```text
Token exchange failed: error sending request for url (https://auth.openai.com/oauth/token)
Token exchange failed: token endpoint returned status 403 Forbidden
```

浏览器那边登录成功了，Codex 拿凭证时连不上 OpenAI。维护者的判断是，剩下的这类情况大多和网络代理、VPN 有关（[#2414](https://github.com/openai/codex/issues/2414)）。

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

同一句报错有两种原因：服务器真的满载（很多人同时遇到，等就行），或者**你的账号被临时限制**（只有你遇到，换模型、重试都没用）。OpenAI 9 月 14 日在官方社区确认，即使订阅有效，也可能因账号活动被临时限制部分模型，系统会自动重新评估。

最快的判断：看 [status.openai.com](https://status.openai.com)，再在同一台电脑上换一个账号试。别人的账号正常、你的不行，多半是账号被限。也有例外：维护者 9 月解释过一例，两个会话可能被分到了不同地区的机房，一个满载、一个不满（[#46103](https://github.com/openai/codex/issues/46103)）。这时别一直点重试，也别急着升级或开新号。完整判断表和处理步骤：[Selected model is at capacity 怎么办](https://aonir.com/guides/codex-selected-model-at-capacity/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_capacity)。

**10 月 9–10 日这一波。** 北京时间 10 月 9 日上午起，很多人每条消息都报这一句，换模型也没用。不少发帖的人在东八区，桌面版、CLI、网页版都有。一位用户翻了自己的会话日志（[#52796](https://github.com/openai/codex/issues/52796)）：10 月 9 日 82 轮对话失败 66 轮，10 月 10 日上午 53 轮失败 45 轮；出问题的时段是 9 日 11:00–19:00、10 日 9:00–14:00，早上 9 点前正常。每条失败记录都是 `codex_error_info: "server_overloaded"`，没有一条是额度用完，当时他的周额度只用了 7%。同一时间新开的 issue 有十多个（[#52394](https://github.com/openai/codex/issues/52394)、[#52465](https://github.com/openai/codex/issues/52465)、[#52812](https://github.com/openai/codex/issues/52812)、[#52833](https://github.com/openai/codex/issues/52833)），到 10 月 11 日都没有官方回复，状态页这两天也没有对应的事故记录。所以**状态页一切正常，不代表不是服务器的问题**。

想确认自己是不是这种情况，到会话日志里搜 `server_overloaded`。日志在 `~/.codex/sessions`，Windows 是 `%USERPROFILE%\.codex\sessions`：

```bash
grep -rl "server_overloaded" ~/.codex/sessions | tail -5
```

搜得到，Usage 页面的额度又还有，就是服务器那边满载，不是你的额度、网络或设置的问题。issue 里用户试出来的几条，都不保证有效：

- 避开高峰。[#52796](https://github.com/openai/codex/issues/52796) 里出问题的都是北京时间的白天。
- 长任务拆短。[#52833](https://github.com/openai/codex/issues/52833)、[#52394](https://github.com/openai/codex/issues/52394) 都提到，步骤多、跑得久的任务更容易中途断掉，短任务能过。
- 桌面版一直报的，在同一台电脑上试试 CLI。[#52394](https://github.com/openai/codex/issues/52394) 里有人桌面版连新对话都报，CLI 用同一个模型却能正常回复。
- 重装、修复应用没用，同一个 issue 里有人试过。

### 模型不支持：model is not supported

```text
The 'gpt-6-astra' model is not supported when using Codex with a ChatGPT account.
```

常见原因有三种：

- **模型名写错了。** 命令行里要写完整 ID。官方 Models 页现在推荐的三个是 `gpt-6-astra`、`gpt-6.1-sol`、`gpt-6-luna`，另外 `gpt-6-sol` 和 GPT-5.6 的三款也还能选（10 月 3 日核对）。写 `--model astra` 这种简称，CLI 能启动，发消息时才报这个错（[#46410](https://github.com/openai/codex/issues/46410)）。
- **旧设置还指着下线的模型。** `~/.codex/config.toml` 里的 `model = "..."`、脚本、定时任务写死了旧模型。**GPT-5.5 在 2026 年 10 月 14 日从 ChatGPT 和 Codex 下线**，API 不受影响。官方 Models 页给的替换：Plus、Pro、Business、Enterprise、Edu 改成 `gpt-6-sol`，Free 和 Go 改成 `gpt-6-luna`。
- **新模型还在分批开放，或者你的套餐、客户端没有。** GPT-6.1 Sol 9 月 29 日发布，官方写的是能不能用取决于套餐、客户端和工作区设置；CLI 是 0.159.1 才把它加进自带的模型列表的，旧版本先升级。9 月有好几个 Pro 账号反映 Astra、Sol 被拒，官方 issue 到 10 月 3 日还开着（[#46304](https://github.com/openai/codex/issues/46304)、[#47333](https://github.com/openai/codex/issues/47333)）。

处理：在会话里输入 `/model`，从列表里选一个；删掉 config.toml 里写死的 `model`；把 Codex 升级到最新。列表里选了还报错，多半是开放范围的问题，用 `/feedback` 反馈后等。

### 请求太多：429 Too Many Requests

```text
exceeded retry limit, last status: 429 Too Many Requests
```

先看 [status.openai.com](https://status.openai.com)。6 月 3 日那次 429，维护者的回复是官方事故、不是客户端问题，修好就恢复（[#26034](https://github.com/openai/codex/issues/26034)）。没有事故的话，看看是不是额度快用完了（[第 7 节](#7-额度用完了)），或者同时开的会话太多。

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
| 升级了，问题还在 | 后台服务还是旧版本。CLI 和后台服务的版本是分开的 | 会话里输入 `/status` 看后台服务的版本，输入 `/daemon` 让它马上更新 |

常用命令：

```bash
codex --no-daemon                          # 这一次不用后台服务，每个窗口各跑各的（和以前一样）
codex app-server daemon version            # 看后台服务的版本
codex app-server daemon stop               # 停掉后台服务
codex app-server daemon restart            # 重启后台服务
codex features disable daemon_auto_start   # 以后都不自动启动
```

最后一条是 issue 里用户的做法（[#48195](https://github.com/openai/codex/issues/48195)），维护者在排查时用的是对应的 `codex features enable daemon_auto_start`。关掉以后，「CLI 退出后任务继续在后台跑」这类依赖后台服务的功能就没有了。

### Windows 沙盒：setup refresh had errors (os error 32)

```text
helper_unknown_error: setup refresh had errors
trusted Node process exited unexpectedly
```

10 月 7 日 Windows 桌面版更新到 26.1002 之后，很多人的所有命令都在执行前失败，连 `Get-Location` 都跑不了。沙盒日志里是 Windows 的 `os error 32`（文件正被占用）：沙盒启动时要检查自己运行目录的权限，里面的 `node_repl.exe` 却正被 Codex 自己占着（[#51601](https://github.com/openai/codex/issues/51601)）。issue 里的人试过重启电脑、修复安装，都没用。

| 你用的是 | 现在的状态 | 怎么办 |
| --- | --- | --- |
| Windows 桌面版 | 10 月 9–10 日多位用户确认 **26.1007.21434** 已修。这是「关于」里显示的版本号，Microsoft Store 里的包版本写的是 26.1007.2314.0，是同一版 | 在 Microsoft Store 更新后重启。10 月 10 日还在报这个错的，看版本大多停在 26.1002。更新之前，有人退回 26.930.61225 能用 |
| 单独安装的 CLI（npm） | 0.161.0、0.162.0、0.162.1 都有人复现，10 月 10 日还没修（[#51634](https://github.com/openai/codex/issues/51634)） | 报告的人在同一台机器上用 0.160.1 正常，可以先装回去 |

```bash
npm install -g @openai/codex@0.160.1 --registry=https://registry.npmmirror.com
```

不想降级的，issue 里还有两个用户自己试出来的办法：

- **换成不需要管理员权限的沙盒。** 在 `%USERPROFILE%\.codex\config.toml` 里，把 `[windows]` 下面的 `sandbox = "elevated"` 改成 `sandbox = "unelevated"`，保存后完全退出再打开 Codex。[#52488](https://github.com/openai/codex/issues/52488)、[#51601](https://github.com/openai/codex/issues/51601) 里好几个人这样恢复了。它的隔离比默认的弱一些（见 [Windows 设置未完成](#windows-设置未完成helper_failed)的第 3 步），官方修好后改回来。
- **只用 CLI 的。** 占着文件的 `node_repl.exe`，是桌面版写进 `config.toml` 的 `[mcp_servers.node_repl]` 带起来的。10 月 11 日有人退出桌面版后把这几段注释掉，CLI 0.162.1 就能跑命令了；要改哪几行见[他的回复](https://github.com/openai/codex/issues/52179#issuecomment-6107852303)。桌面版下次启动多半会把它们加回来。

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

Windows 桌面版打开 Run agent in WSL（配置里是 `runCodexInWindowsSubsystemForLinux = true`）之后，每条命令都报这个错。从 9 月 30 日的 26.928 开始，10 月 11 日还有人在 26.1007.21434 上遇到（[#49731](https://github.com/openai/codex/issues/49731)）。10 月 10 日一位用户查到了原因：WSL 里的 Codex 和 Windows 这边的 Codex 共用同一个 `.codex` 目录，临时文件都放在 `tmp/arg0`（WSL 里看是 `/mnt/c/Users/<用户名>/.codex/tmp/arg0`）。两边启动时都会清掉里面「没人在用」的目录，可 Linux 和 Windows 的文件锁互相看不见，后启动的一方就把对方正在用的删了，`codex-linux-sandbox` 跟着没了。所以常见的情况是刚打开还能用，过一会儿所有命令都失败。

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

### 终端里复制不了文字

CLI 0.157.0 起默认用全屏界面：Codex 自己接管整个终端窗口，和 vim 一样，不再往终端的滚动记录里写。好处是长对话滚动快、输入框固定在底部；代价是在 Mac 自带的终端（Terminal.app）里，选中文字按 Cmd + C 没反应。维护者的答复是 Terminal.app 在全屏模式下不会把 Cmd + C 传给 Codex，这个不会改（[#48122](https://github.com/openai/codex/issues/48122)）。三个办法：

1. 输入 `/tui`，切回原来的滚动模式；或者启动时加 `--no-alt-screen`（`codex --no-alt-screen`）。全屏模式的那些功能就没有了。
2. 用选中即复制（copy-on-select）或右键复制，不按 Cmd + C。0.158.0 起这两项可以自己开关。
3. 换一个终端，比如 Ghostty、Kitty，它们没有这个限制。

全屏模式的官方介绍：[Codex CLI goes fullscreen](https://github.com/openai/codex/discussions/49129)。

### 上下文满了：ran out of room in the model's context window

```text
Codex ran out of room in the model's context window. Start a new thread or clear earlier history before retrying.
```

这不是额度用完，是**这一个会话**装的内容太多了：聊得太久、读了大文件、工具一次返回了很大的结果（比如浏览器截图）。Codex 平时会自动压缩上下文，但单次塞进来的东西太大时也会顶满（[#4926](https://github.com/openai/codex/issues/4926)）。

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
| 5 小时额度用完 | 等报错里写的时间，一般几小时内就回来。Plus 有 5 小时限制；Pro 目前没有，只算每周额度（官方定价页，10 月 3 日核对） |
| 每周额度用完 | 等周额度重置；急用就换个消耗低的模型，或者买额外额度 |
| 当前模型用完了 | 按提示换一个模型开新会话 |
| 手上有重置机会 | 中文界面叫「使用已储备的重置机会」。Codex 负责人 Tibo 经常在新模型上线、用户数破纪录这类时候给大家发重置，最近一张重置卡是北京时间 9 月 30 日凌晨 DevDay 现场发的。[每一次的时间和原推在这里](https://aonir.com/guides/codex-usage-reset/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_resets) |
| 经常用完，影响干活 | 考虑升级。9 月 29 日起 Pro 分三档：Pro 100、Pro 200、Pro 500（$100 / $200 / $500），Codex 额度分别是 Plus 的 5、10、25 倍 |

Plus 每 5 小时大概能发多少条（官方定价页的估算，本地任务，10 月 3 日核对）：

| 模型 | Plus 每 5 小时 |
| --- | --- |
| GPT-6 Astra | 5–45 条 |
| GPT-6.1 Sol | 15–160 条 |
| GPT-6 Sol | 15–150 条 |
| GPT-6 Luna | 350–3,000 条 |

这是估算，不是固定次数，任务越大越耗，另外还有每周上限。同一份额度，用 Sol 能跑的条数大约是 Astra 的 3 倍，日常写代码先用 GPT-6.1 Sol，难活再换 Astra。

> 中文界面里有个容易看混的地方：**「额度」有两种意思。**「每周使用限额」「5 小时使用限额」是套餐自带、会自动恢复的；「添加额度」「额度余额」对应英文的 credits，是用完后另外花钱买的点数。官方说明 Plus 和 Pro 用完限额后可以买 credits 继续用，不一定要升级。

会员到期变回 Free 再重新订阅，周额度不会刷新。[LINUX DO](https://linux.do/t/topic/2995861) 上有人试过：Plus 到期时周额度还剩 82%，马上重新订阅，还是 82%。这是一位用户的情况，仅供参考。

各套餐多少钱、Astra / Sol / Luna 哪个更省额度：[Codex 多少钱？Plus 和 Pro 怎么选](https://aonir.com/guides/codex-pricing/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_pricing)；哪个套餐在哪个入口能用哪个模型：[ChatGPT 各套餐模型对照](https://aonir.com/guides/chatgpt-models-by-plan/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_models)。

## 8. 怎么设置中文

**桌面版自带简体中文，不用装汉化包。**

- **桌面版**：按 Cmd + 逗号（Windows 是 Ctrl + 逗号）打开设置，在「常规」里找到「语言」（英文界面叫 Language），选「中文（中国）」，也就是简体中文。列表很长，可以在搜索框里输入「中文」或 Chinese。默认是「自动检测」，跟随系统语言。
- **VS Code / Cursor 插件**：跟着编辑器的显示语言走。只想让插件显示中文，在设置里把 `chatgpt.localeOverride` 设成 `zh-CN`。
- **命令行 CLI**：没有中文界面，官方配置里也没有这个选项。想让它用中文回复，在 `~/.codex/AGENTS.md` 里写一句「用简体中文回复」（见[第 9 节](#9-agentsmd-中文模板)）。

**设置里没有「语言」这一项？** 这个选项由 OpenAI 后台的开关控制。我们看了桌面版 26.915 安装包里的代码，「语言」这一项和中文文字包都受同一个开关控制，没对你的账号打开时就不显示。先更新到最新版并重启；还是没有，就只能等官方开放。

**不建议装第三方汉化包。** 这类工具要么解包修改安装文件（app.asar），再改掉程序的完整性校验；要么通过调试端口往运行中的应用里注入脚本。官方一更新就容易失效，甚至打不开。有的还捆绑了跳过官方登录、多账号切换这类功能。

详细步骤和官方示意图：[Codex 怎么设置中文](https://aonir.com/guides/codex-chinese-settings/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_chinese)。

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

完整对照 115 条，按界面区域分组：[UI-GLOSSARY.md](UI-GLOSSARY.md)。中文取自 ChatGPT 桌面版自带的简体中文界面，英文取自同一版本的原文，10 月 3 日对着 26.930 逐条复核过，10 月 11 日按 26.1007 补了 5 条报错提示。

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

**开了两步验证（2FA），还要验证手机号吗？**
要。OpenAI 帮助中心写明，邮箱和验证器 App 都代替不了 Codex 的手机验证。

## 国内怎么开通

没有海外信用卡，常见三条路：

1. **自己用礼品卡订阅**：见我们的两篇实测教程：[ChatGPT Plus 国内充值 / Apple 礼品卡实测](https://github.com/Avarce/chatgpt-plus-china-guide)、[OpenAI Gift Card 购买与兑换](https://github.com/Avarce/openai-gift-card-guide)。
2. **先把几种方法比一遍**：[没有海外信用卡怎么开通 ChatGPT Plus：5 种方法对比](https://aonir.com/guides/chatgpt-plus-without-foreign-card/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_no_card)。
3. **微信充值到本人账号**：AONIR 提供 [ChatGPT Plus ¥168 / 月](https://aonir.com/chatgpt-plus/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=cta_plus)、[ChatGPT Pro 5× ¥730 / 月](https://aonir.com/chatgpt-pro/?plan=5x&utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=cta_pro_5x)（官方的 Pro 100），5 倍还不够的有 [Pro 10× ¥1350 / 月](https://aonir.com/chatgpt-pro/?plan=20x&utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=cta_pro_10x)（官方的 Pro 200，联系客服充值）。都是充到你自己的账号，不需要密码，Codex 额度跟着套餐走。

有海外卡的，直接在官方订阅最划算。

## 投稿一条报错

遇到这页没收的报错，欢迎[开一个 Issue](../../issues/new?template=error-report.md)：贴上报错原文、你用的是桌面版 / CLI / 插件、版本号、系统，以及你怎么解决的（没解决也可以发）。

贴之前把 `auth.json` 的内容、token、邮箱和手机号删掉。收到后我们核对，补进这页，并记在[更新日志](CHANGELOG.md)里。

## 官方来源

- ChatGPT Learn：[Authentication](https://learn.chatgpt.com/docs/auth) · [Troubleshooting](https://learn.chatgpt.com/docs/reference/troubleshooting) · [Environment variables](https://learn.chatgpt.com/docs/config-file/environment-variables) · [Configuration Reference](https://learn.chatgpt.com/docs/config-file/config-reference)
- ChatGPT Learn：[Windows sandbox](https://learn.chatgpt.com/docs/windows/windows-sandbox) · [Settings](https://learn.chatgpt.com/docs/reference/settings) · [Codex IDE extension](https://learn.chatgpt.com/docs/codex/ide)
- ChatGPT Learn：[Codex CLI](https://learn.chatgpt.com/docs/codex/cli) · [ChatGPT desktop app](https://learn.chatgpt.com/docs/app) · [Custom instructions with AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md) · [Pricing](https://learn.chatgpt.com/docs/pricing) · [Models](https://learn.chatgpt.com/docs/models)
- OpenAI Help Center：[What does phone verification look like?](https://help.openai.com/en/articles/8983040-what-does-phone-verification-look-like) · [哪些号码不能用](https://help.openai.com/en/articles/8983024) · [WhatsApp 验证支持的国家](https://help.openai.com/en/articles/8983038-which-countries-do-you-support-for-whatsapp-phone-verification) · [不支持改号](https://help.openai.com/en/articles/9135134-how-to-change-the-phone-number-associated-with-your-account) · [支持的国家和地区](https://help.openai.com/en/articles/7947663-chatgpt-supported-countries)
- OpenAI Help Center：[Troubleshooting Model Feature Access Issues](https://help.openai.com/en/articles/10258669-troubleshooting-model-feature-access-issues) · [How banked Codex resets work](https://help.openai.com/en/articles/20001498-how-banked-codex-resets-work)
- ChatGPT Learn：[Changelog](https://learn.chatgpt.com/docs/changelog)（版本和模型的发布日期）
- GitHub：[openai/codex](https://github.com/openai/codex)，文中引用的 issue 都附了编号链接；各版本改了什么见 [Releases](https://github.com/openai/codex/releases)
- [OpenAI 状态页](https://status.openai.com)

中文界面的文字取自 ChatGPT 桌面版（macOS）自带的简体中文语言包，9 月 24 日按 26.915.31945 整理，10 月 3 日按 26.930.21537 复核，10 月 11 日新增的几条提示取自 26.1007.21159；`codex doctor` 输出来自我们编辑电脑上的实际运行结果（CLI 0.153.4）；`--no-daemon`、`--no-alt-screen`、`codex app-server daemon …`、`codex features …` 对照了桌面版 26.930 自带的 Codex（0.159.0-alpha.12.1）的帮助输出。「最近两周的已知问题」里的状态，取自各 issue 在 10 月 11 日的开关状态和 OpenAI 员工的回复，官方事故以状态页的记录为准；标了「用户报告」「社区办法」的没有经过官方确认。

## 转载与许可

本页内容采用 [CC BY 4.0](LICENSE) 许可：可以转载、翻译、改编，注明出处并附上本仓库链接即可。Codex、ChatGPT、OpenAI 是 OpenAI 的商标，本仓库与 OpenAI 没有隶属关系。
