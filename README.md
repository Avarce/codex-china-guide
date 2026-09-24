# Codex 国内使用与报错速查（2026）：安装、登录、手机号、代理、额度

> **最后核对：2026 年 9 月 24 日**（北京时间）· Codex CLI 0.156.1 · ChatGPT 桌面版 26.915
> 遇到报错，直接 Ctrl + F 搜报错里的几个词。终端、英文界面、中文界面三种写法都收了。
> 觉得有用就点右上角 ⭐ Star，下次报错不用再到处搜。

![Codex 国内使用与报错速查：按报错原文查原因和处理步骤](images/cover.webp)

国内用 Codex，卡住的地方基本就那几处：装不上、打不开、登录跳不回来、要验证手机号、终端连不上、用着用着断线、额度用完。这页按「你看到的报错」来排，每条写清楚多半是什么原因、按什么顺序处理，并附上官方文档或 openai/codex 仓库里维护者回复的出处，可以自己点进去核对。

由 [AONIR](https://aonir.com/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=brand_home) 整理维护。我们提供 ChatGPT / Claude 会员充值服务，和 OpenAI 没有隶属关系；这页只讲 Codex 本身，充值相关只放在[最后一节](#国内怎么开通)。

## 目录

- [按报错找](#按报错找)
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
| `stream disconnected before completion`、`Reconnecting... 1/5`、正在重新连接…、服务器繁忙，正在重新连接 | 网络或代理 | [断线重连](#断线重连stream-disconnected-before-completion) |
| `error sending request for url (https://chatgpt.com/backend-api/codex/...)` | 终端没走代理 | [断线重连](#断线重连stream-disconnected-before-completion) |
| 浏览器显示登录成功，终端一直在等 | 本机回调被挡 | [2. 登录](#2-登录) |
| `Token exchange failed` | 登录时网络不通 | [登录失败](#登录失败token-exchange-failed) |
| `refresh token was already used`、此设备上的 ChatGPT 会话已过期 | 登录凭证失效 | [登录过期](#登录过期refresh-token-was-already-used) |
| `Verify your phone number`、`Phone number required` | 手机号验证 | [3. 要验证手机号](#3-要验证手机号) |
| `unsupported_country_region_territory`、我们的服务在你所在的国家或地区不可用 | 节点所在地区 | [地区不支持](#地区不支持unsupported_country_region_territory) |
| `Selected model is at capacity` | 服务器满载，或账号被临时限制 | [模型满载](#模型满载selected-model-is-at-capacity) |
| `The '...' model is not supported when using Codex with a ChatGPT account` | 模型名写错、旧设置或还没开放 | [模型不支持](#模型不支持model-is-not-supported) |
| `exceeded retry limit, last status: 429 Too Many Requests` | 官方故障或请求太多 | [429](#请求太多429-too-many-requests) |
| `ran out of room in the model's context window` | 这个会话的上下文满了 | [上下文满了](#上下文满了ran-out-of-room-in-the-models-context-window) |
| 证书错误、`certificate` | 公司网络或抓包软件 | [证书错误](#证书错误公司网络抓包软件) |
| `You've hit your usage limit`、你已达到使用上限 | 额度用完 | [7. 额度用完了](#7-额度用完了) |
| Windows 安装未完成 · `helper_failed`、完成 Windows 设置以继续 | Windows 沙盒没装好 | [Windows 安装未完成](#windows-安装未完成helper_failed) |
| 已在另一个应用中打开 | 这个会话在别处开着 | [5. 桌面版打不开](#5-桌面版打不开) |
| 更新后打不开、只有启动动画、窗口空白 | 多半是新版本的问题 | [更新后打不开](#更新后打不开只有启动动画或白屏) |
| 界面是英文，想切成中文 | 设置里就能改 | [8. 怎么设置中文](#8-怎么设置中文) |

不知道是哪一类，先在终端跑一次 `codex doctor`。它会检查安装、配置、登录和网络，每一项打 ✓ 或 ⚠，最后一栏 Connectivity 就是网络情况。

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
# npm 国内镜像（9 月 24 日核对：镜像版本和 npm 官方一致，都是 0.156.1）
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

OpenAI 帮助中心 9 月 23 日更新的说法：

- 在桌面版里使用 Codex 可能要求验证手机号。官方社区里不少人说 CLI 和 VS Code 插件登录也会被带到同一个验证页。
- 验证码只发短信，部分国家可以发 WhatsApp。**邮箱和验证器 App（2FA）都代替不了。**
- 座机、Google Voice 这类网络电话（VoIP）不行。中国大陆和香港不在支持名单里，国内手机号一般用不了。
- **号码绑定后不能更换。** 好在正常情况下验证一次就够了，不会反复要求。

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

还有一条：**节点尽量固定，别频繁切换地区。** OpenAI 帮助中心把「从陌生的地点登录」列为账号被临时限制的安全原因之一。

## 5. 桌面版打不开

先看是哪一种：

| 你看到的 | 多半是 | 怎么办 |
| --- | --- | --- |
| Windows 安装未完成 · `helper_failed`、完成 Windows 设置以继续 | Windows 沙盒没装好 | [看下面](#windows-安装未完成helper_failed) |
| 已在另一个应用中打开 | 这个会话还在终端、编辑器或另一个窗口里开着 | 在那边关掉这个会话，回来点「重试」 |
| 更新后双击没反应、只有启动动画、窗口一片空白 | 多半是新版本的问题 | [看下面](#更新后打不开只有启动动画或白屏) |
| 能打开，但登录页出不来、一直转圈 | 网络 | [第 4 节](#4-国内网络终端要单独设代理) |

### Windows 安装未完成（helper_failed）

Windows 版的 Codex 要在你电脑上改代码、跑命令，先得建一个沙盒（隔离环境），这一步需要一次管理员授权。界面会提示「完成 Windows 设置以继续」，失败时显示「Windows 安装未完成」和一个错误码，比如 `helper_failed`。

官方文档列的常见原因：弹出「用户账户控制」时点了「否」；电脑不允许创建本地用户和组、不允许改防火墙；公司的管理策略挡住了其中一步。

按顺序试：

1. 点「重试 Windows 设置」，弹出「用户账户控制」时点「是」。
2. 公司电脑：问 IT 是否允许这类管理员授权的设置（创建本地用户和组、改防火墙规则、给沙盒用户登录权限）。
3. 急着用，可以换成官方的备用沙盒。它的隔离比默认的弱一些，但能继续改代码、跑命令。在 `%USERPROFILE%\.codex\config.toml` 里加：

   ```toml
   [windows]
   sandbox = "unelevated"
   ```

   也可以先点「继续使用受限访问」，这样只能聊天，不能建文件、改代码。
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
- 9 月的 26.915 版，Mac 上有白屏报告，issue 还开着（[#46641](https://github.com/openai/codex/issues/46641)）。

按顺序试：

1. 彻底退出再打开：Windows 在任务管理器里结束 ChatGPT（旧版叫 Codex）的进程；Mac 按 Cmd + Q。
2. 重启电脑。
3. 检查更新。已知问题一般靠新版本修，Windows 可以在 Microsoft Store 里看有没有更新。
4. Windows：设置 → 应用 → 已安装的应用 → ChatGPT → 高级选项，先点「修复」（不删数据）；不行再点「重置」（会清掉应用自己的数据，要重新登录）。
5. 到 [openai/codex 的 issue](https://github.com/openai/codex/issues) 里搜你的版本号和现象，看是不是已知问题。

> issue 里常有人贴「临时办法」，比如改 `CODEX_CLI_PATH`、从第三方镜像降级。这些官方都没认可，[#40752](https://github.com/openai/codex/issues/40752) 里就有人反馈改完之后历史会话打不开。能等就等官方修。

## 6. 报错逐条处理

### 断线重连：stream disconnected before completion

```text
stream disconnected before completion: error sending request for url (https://chatgpt.com/backend-api/codex/responses)
Reconnecting... 2/5
```

中文界面显示「正在重新连接…」或「服务器繁忙，正在重新连接」。

**多半是网络问题。** openai/codex 的维护者排查过一批这类报告，结论是大多是网络连通性问题：网络不稳、VPN、代理、防火墙；也有网络把 WebSocket 挡了的（[#14209](https://github.com/openai/codex/issues/14209)、[#13245](https://github.com/openai/codex/issues/13245)）。

按顺序试：

1. 看 [status.openai.com](https://status.openai.com)。有故障记录就是官方的问题，等修好。
2. 终端里跑 `codex doctor`，`websocket` 不是 connected，就是网络没通。
3. 按[第 4 节](#4-国内网络终端要单独设代理)设好代理，或打开 TUN 模式。
4. 换一个稳定的节点，别用很多人共用、经常断的线路。
5. Mac 合盖睡眠醒来后才出现的，重开会话或重启 Codex。这个问题官方 issue [#3355](https://github.com/openai/codex/issues/3355) 还开着。
6. 还不行，在 Codex 里输入 `/feedback` 上传日志，拿到的会话 ID 可以贴进 GitHub issue。

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

最快的判断：看 [status.openai.com](https://status.openai.com)，再在同一台电脑上换一个账号试。别人的账号正常、你的不行，就是账号被限。这时别一直点重试，也别急着升级或开新号。完整判断表和处理步骤：[Selected model is at capacity 怎么办](https://aonir.com/guides/codex-selected-model-at-capacity/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_capacity)。

### 模型不支持：model is not supported

```text
The 'gpt-6-astra' model is not supported when using Codex with a ChatGPT account.
```

常见原因有三种：

- **模型名写错了。** 命令行里要写完整 ID，比如 `gpt-6-astra`。写 `--model astra` 这种简称，CLI 能启动，发消息时才报这个错（[#46410](https://github.com/openai/codex/issues/46410)）。
- **旧设置还指着下线的模型。** `~/.codex/config.toml` 里的 `model = "..."`、脚本、定时任务写死了旧模型。GPT-5.5 在 2026 年 10 月 14 日从 ChatGPT 和 Codex 下线，官方建议改成 `gpt-5.6-sol`。
- **新模型还在分批开放，或者你的套餐没有。** 9 月有好几个 Pro 账号反映 Astra、Sol 被拒，官方 issue 还开着（[#46304](https://github.com/openai/codex/issues/46304)、[#47333](https://github.com/openai/codex/issues/47333)）。

处理：在会话里输入 `/model`，从列表里选一个；删掉 config.toml 里写死的 `model`；把 Codex 升级到最新。列表里选了还报错，多半是开放范围的问题，用 `/feedback` 反馈后等。

### 请求太多：429 Too Many Requests

```text
exceeded retry limit, last status: 429 Too Many Requests
```

先看 [status.openai.com](https://status.openai.com)。6 月 3 日那次 429，维护者的回复是官方事故、不是客户端问题，修好就恢复（[#26034](https://github.com/openai/codex/issues/26034)）。没有事故的话，看看是不是额度快用完了（[第 6 节](#7-额度用完了)），或者同时开的会话太多。

`last status: 401 Unauthorized` 是另一回事：登录失效了，`codex logout` 后重新登录。

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

中文界面：「你已达到使用上限。升级套餐以继续，或在 … 再试。」也可能是「你已达到 GPT-6 Astra 的使用限额。请在 … 后重试，或与其他模型新建对话。」后一种说明是**这个模型**的限额用完了，换一个模型往往还能继续用。

先看还剩多少：CLI 里输入 `/status`；桌面版点个人菜单里的「使用情况」；网页看 [chatgpt.com/codex/settings/usage](https://chatgpt.com/codex/settings/usage)。然后按情况选：

| 情况 | 怎么办 |
| --- | --- |
| 5 小时额度用完 | 等报错里写的时间，一般几小时内就回来。Plus 有 5 小时限制；Pro 目前没有，只算每周额度（Codex 负责人 Tibo 8 月 25 日说明） |
| 每周额度用完 | 等周额度重置；急用就换个消耗低的模型，或者买额外额度 |
| 当前模型用完了 | 按提示换一个模型开新会话 |
| 手上有重置机会 | 中文界面叫「使用已储备的重置机会」。Tibo 经常在新模型上线、用户数破纪录这类时候给大家发重置，[这里有时间线](https://aonir.com/guides/codex-pricing/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_resets#resets) |
| 经常用完，影响干活 | 考虑升级：Plus → Pro 5×，Codex 用量是 Plus 的 5 倍 |

> 中文界面里有个容易看混的地方：**「额度」有两种意思。**「每周使用限额」「5 小时使用限额」是套餐自带、会自动恢复的；「添加额度」「额度余额」对应英文的 credits，是用完后另外花钱买的点数。官方说明 Plus 和 Pro 用完限额后可以买 credits 继续用，不一定要升级。

各套餐多少钱、每 5 小时大概能发多少条、Astra / Sol / Luna 哪个更省额度：[Codex 多少钱？Plus 和 Pro 怎么选](https://aonir.com/guides/codex-pricing/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_pricing)，或者看我们的 [ChatGPT / Claude 速查表](https://github.com/Avarce/chatgpt-claude-cheatsheet)。

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
| Steer / Queue | 调整方向 / 加入队列 |
| Compact | 压缩 |
| Usage limits / Credits | 用量限制 / 额度（见上一节，两个都可能叫「额度」） |

完整对照 100 多条，按界面区域分组：[UI-GLOSSARY.md](UI-GLOSSARY.md)。中文取自 ChatGPT 桌面版 26.915 自带的简体中文界面，英文取自同一版本的原文。

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
/Applications/Codex.app/Contents/Resources/codex --version
```

**日志在哪？**
Mac 桌面版日志在 `~/Library/Logs/com.openai.codex/`；会话记录在 `~/.codex/sessions`。发给别人前先看一眼，别把 token 带出去。

**开了两步验证（2FA），还要验证手机号吗？**
要。OpenAI 帮助中心写明，邮箱和验证器 App 都代替不了 Codex 的手机验证。

## 国内怎么开通

没有海外信用卡，常见三条路：

1. **自己用礼品卡订阅**：见我们的两篇实测教程：[ChatGPT Plus 国内充值 / Apple 礼品卡实测](https://github.com/Avarce/chatgpt-plus-china-guide)、[OpenAI Gift Card 购买与兑换](https://github.com/Avarce/openai-gift-card-guide)。
2. **先把几种方法比一遍**：[没有海外信用卡怎么开通 ChatGPT Plus：5 种方法对比](https://aonir.com/guides/chatgpt-plus-without-foreign-card/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=guide_no_card)。
3. **微信充值到本人账号**：AONIR 提供 [ChatGPT Plus ¥168 / 月](https://aonir.com/chatgpt-plus/?utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=cta_plus)、[ChatGPT Pro 5× ¥730 / 月](https://aonir.com/chatgpt-pro/?plan=5x&utm_source=github&utm_medium=referral&utm_campaign=codex_guide&utm_content=cta_pro_5x)，充到你自己的账号，不需要密码，Codex 额度跟着套餐走。

有海外卡的，直接在官方订阅最划算。

## 投稿一条报错

遇到这页没收的报错，欢迎[开一个 Issue](../../issues/new?template=error-report.md)：贴上报错原文、你用的是桌面版 / CLI / 插件、版本号、系统，以及你怎么解决的（没解决也可以发）。

贴之前把 `auth.json` 的内容、token、邮箱和手机号删掉。收到后我们核对，补进这页，并记在[更新日志](CHANGELOG.md)里。

## 官方来源

- ChatGPT Learn：[Authentication](https://learn.chatgpt.com/docs/auth) · [Troubleshooting](https://learn.chatgpt.com/docs/reference/troubleshooting) · [Environment variables](https://learn.chatgpt.com/docs/config-file/environment-variables) · [Configuration Reference](https://learn.chatgpt.com/docs/config-file/config-reference)
- ChatGPT Learn：[Windows sandbox](https://learn.chatgpt.com/docs/windows/windows-sandbox) · [Settings](https://learn.chatgpt.com/docs/reference/settings) · [Codex IDE extension](https://learn.chatgpt.com/docs/codex/ide)
- ChatGPT Learn：[Codex CLI](https://learn.chatgpt.com/docs/codex/cli) · [ChatGPT desktop app](https://learn.chatgpt.com/docs/app) · [Custom instructions with AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md) · [Pricing](https://learn.chatgpt.com/docs/pricing) · [Models](https://learn.chatgpt.com/docs/models)
- OpenAI Help Center：[What does phone verification look like?](https://help.openai.com/en/articles/8983040-what-does-phone-verification-look-like) · [Troubleshooting Model Feature Access Issues](https://help.openai.com/en/articles/10258669-troubleshooting-model-feature-access-issues) · [How banked Codex resets work](https://help.openai.com/en/articles/20001498-how-banked-codex-resets-work)
- GitHub：[openai/codex](https://github.com/openai/codex)，文中引用的 issue 都附了编号链接
- [OpenAI 状态页](https://status.openai.com)

中文界面的文字取自 ChatGPT 桌面版 26.915.31945（macOS）自带的简体中文语言包；`codex doctor` 输出来自我们编辑电脑上的实际运行结果（CLI 0.153.4）。

## 转载与许可

本页内容采用 [CC BY 4.0](LICENSE) 许可：可以转载、翻译、改编，注明出处并附上本仓库链接即可。Codex、ChatGPT、OpenAI 是 OpenAI 的商标，本仓库与 OpenAI 没有隶属关系。
