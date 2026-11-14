# 更新日志

Codex 改了安装方式、登录流程、报错或额度规则，这里记一笔。日期为北京时间。

## 2026-11-14

- 新增：Windows 上 Agent 设成在 WSL 里运行的，更新到商店包 26.1106.6628.0、26.1111.2228.0 后一打开就是「无法加载组织设置」（#60974，11 月 12 日起，报告人之外 8 位用户确认，没有官方回复）。报告人查到原因：安装包里给 WSL 用的 Linux 版 `codex` 在 WSL 里没有执行权限（`444`），启动检查一运行就失败。办法：先把 Agent 改回在 Windows 上运行；一定要用 WSL 的，三位用户复制同一版本的 `codex`、`codex-code-mode-host` 并 `chmod 755`、只对这次启动设 `CODEX_CLI_PATH` 后能用（用户的临时办法，每次更新要重做）。页首、按报错找、已知问题表第一行、「更新后打不开」一节一起改（提醒别把 `CODEX_CLI_PATH` 存成用户环境变量）。「更新后打不开」另加一种：点「退出登录」也回到这一页的，一位 Windows 11 用户把 `auth.json` 改名、重新登录后好了（#48324）。
- 换了封面和链接预览图：原来那张写着 9 月 24 日、CLI 0.156.1、15 类常见报错，早就对不上了。新的一张画的是这页怎么用：Ctrl + F 搜报错原文，「按报错找」表里那一行被标出来；不写日期和版本号。
- 「最近两周的已知问题」每行只留现在的状态：16 格长的（多的近千字）缩成一两句加出处，挪走前的全文放在本文件最后的「[已知问题表的详细经过](#已知问题表的详细经过)」。报错原文都还在 README 里，Ctrl + F 照样搜得到；README 从 283 KB 减到 271 KB。
- 页首「这几天问得最多的」从 8 条减到 5 条：已经修好的 No projects、VS Code 插件、SSH 自动连接和「模型满载」挪回已知问题表，留下 WSL「无法加载组织设置」、11 月 13 日的重置卡、12 月共用额度、网站被拦、网络安全误判。
- 重置：北京时间 11 月 13 日 05:59 Tibo 说一张重置卡已经启用，会陆续进到每个人的账号；页首和第 7 节加上。
- 按报错找加一行：「ChatGPT 遇到了问题」（`ChatGPT hit a snag`）、「ChatGPT 无法加载你的配置」，只写应用自己给的办法；补英文原文 `Server is busy, reconnecting`（服务器繁忙，正在重新连接）、`This is open in another app`（已在另一个应用中打开）。
- 已知问题：No projects 在 26.1111 上又一位 Windows 用户报（#61294）；`Failed to resume chat`（`AbsolutePathBuf …`）在 26.1106.51428 上仍有，北京时间 12 日、14 日各一位；VS Code 插件：之前更新后仍打不开的那位 Intel Mac 用户 13 日说装上 26.1111.21308 后好了；定时任务被拒：12–13 日又有四位报，一位发现更早建的旧对话里还能建；更新后报 `0x80070020`：13 日 26.1111.2228.0 上又一位，重启电脑后能打开；网站被拦 14 日、网络安全误判 13 日还有新报告；「模型满载」UTC 12 日 3 个（其中一个说的是回答质量）、13 日 0 个，没有回升。
- 第 3 节手机号：11 月 14 日重读帮助中心。号码类型、WhatsApp 国家两篇并进了手机验证那一篇（旧链接跳转过去）；加上帮助中心写明的两句：验证页让换号码时，可以在同一个账号里换另一个自己的手机号，按报错里的等待时间来；支持的国家的号码一直验证不了，带报错原文找客服。改号、支持的国家两篇没变。
- 版本：页首和安装一节改成 CLI 0.169.0（11 月 13 日，同一天还有 0.168.0；npm 国内镜像一致）；桌面版仍以 26.1106 为准，12 日起 issue 里已见到 26.1111（官方更新日志还没有）；CLI 0.168.0 起碰上 429 时重连提示写成 `Server is busy, reconnecting 2/5`（#59253），429 一行和一节补上；VS Code 插件 13 日有了预览版 26.51111.21308。常见问题「先把 CLI 升到 0.167.0」改成「0.167.0 或更新的版本」。已知问题的状态核对到 11 月 14 日；12 月共用额度 14 日重读帮助中心，没变。

## 2026-11-12

- VS Code 插件打不开（`Codex could not start`）：OpenAI 11 月 11 日 14:50 在 #56655 说修好的版本已发布（正式版 26.1106.51428、预览版 26.51106.51428），并关闭了这个 issue。处理步骤改成先更新、重新加载窗口，退回 26.1007.21434 留作更新后还不行时的办法；到 12 日下午更新后三位 Linux 用户说好了，一位 Intel Mac 用户说还是打不开，另有两位说还不行或偶尔不行。
- 项目列表空了（`No projects`）：11 日起推送的桌面版 26.1106.51428 修好了，Mac、Windows 上三位用户确认，一位 Windows 用户说还没恢复；#59895 还开着，OpenAI 没再回复。页首、按报错找、已知问题表和第 6 节一起改。
- 重启后 SSH 主机不自动连上：更新到 26.1106.51428 后两位用户说好了，其中一位 Mac（#59931、#60047），设置 → 连接里重新勾选留作还不行时的办法。Chrome `timeoutMs` 那一条：11 日又一位用户更新到 26.1106.51428 后好了（#59276）。
- `Selected model is at capacity`：7 日起少了以后没再多起来（UTC 10 日 0 个、11 日 2 个）。重置：到 12 日没有新的全体重置或重置卡，Tibo 11 日只说周四也许是放一张存着的重置卡的好时机。12 月共用额度：12 日重读帮助中心，没变。
- 更新后打不开、报 `0x80070020`：11 日又有两位在更新到商店包 26.1106.6628.0 前后遇到，12 日一位说卸载重装也没用（#56788）。
- 页首核对日期改为 11 月 12 日；现在的版本：桌面版 26.1106、CLI 0.167.2、VS Code 插件 26.1106.51428。
- 按报错找加一行、401 一节加一条：桌面版对话里 `workspace routing discovery unauthorized (401)`、「正在重新连接 5/5」。一位 Mac 用户 12 日发来截图，更新桌面版后就好了（用户报告）；更新到 26.1106 还报的，再退出登录、重新登录（#49443 里一位用户重新登录后恢复）。

## 2026-11-10

- 新增「项目列表空了：没有项目（No projects）」：11 月 6 日起桌面版 26.1103 上，退出登录或切换账号后左边栏的项目全没了，聊天只在「最近」里。OpenAI 的人北京时间 11 月 10 日凌晨在 #59895 回复，修复在「周二的版本」里发；用户的办法（完全退出重开；改 `~/.codex/.codex-global-state.json` 合并回原账号的项目列表，六位用户成功，非官方，先备份）。
- 新增「SSH 远程项目：重启应用后不自动连上」（26.1103，Windows、macOS；设置 → 连接里取消勾选再勾上「连接」，至少六位用户确认）、「让 Codex 打开某个网站被拦：Site safety policy blocked this URL」（6 月起就有人报，11 月 5–8 日又新开 7 个；微信公众号文章、拼多多商家后台等，「始终允许」也没用，没有官方回复）。
- 已知问题表加「让 Codex 定时盯一个任务被拒」（`Do not create an automation to track a specific job …`，11 月 5–9 日 5 个 issue；一位用户自己手动建定时任务成功）和「Linux 桌面版 26.1103.32239 回车、发送没反应」（两位用户退回 26.1027.51404 后好了）。
- 「被标记为网络安全风险」加三句同类提示（8–9 月起就有，11 月 5–9 日又有十几个人报误判）：让你申请 Daybreak 的「此内容无法显示」、「Daybreak 不适用于 Astra」、`This request was blocked by our safety systems`（`misalignmentPolicyViolation`）。
- VS Code 插件打不开：OpenAI 的人北京时间 11 月 10 日 14:26 第一次回复，已复现、打算第二天发修好的版本；加了两个各只有一位用户试过的办法（最后再打开 Codex 侧边栏；Linux 上 `code --disable-gpu`）。到 10 日下午插件市场还没有新版本。
- 远程控制加一条：「无法更新远程控制状态」「远程控制授权失败」（`Failed to authorize remote control`）、日志里 `Multi-factor authentication required` 的，两位用户开了 ChatGPT 账号的 MFA 就好了（其中一位以为开着，网页版安全设置里其实没开）。
- 「更新后打不开」加两种：Windows 更新后报 `0x80070020`，用 GPG 签名提交的，`gpgconf --kill scdaemon`、`gpgconf --kill gpg-agent` 后马上能开（一位用户）；WSL 模式下弹「无法加载组织设置」、日志 `policy-read reason=protocol` 的，一位用户查到是 BIOS 没开硬件虚拟化。
- 常见问题：「恢复对话失败」加 `AbsolutePathBuf deserialized without a base path`（Windows 26.1103，云端管理的聊天，先去网页版）；「`codex resume` 里看不到以前的会话」加 CLI 0.167.0 的改动（换了提供方，保存的聊天仍然显示）；「Codex 为什么这么慢」加 OpenAI 11 月 4 日说已修好对话越长内存越涨的问题。
- 第 7 节：12 月共用额度 11 月 10 日重读帮助中心没变；加一位 V2EX 用户说自己的账号像是提前合并了（个人观察）。
- 状态核对到 11 月 10 日：「模型满载」7 日起少了（UTC 5 日、6 日各 6 个，7、8 日周末 0 个，9 日 2 个、同一个人），还要再看；429 那句提示 0.167.0 里没提；Postman 11 月 5 日说撤回、issue 已关，之后几版更新说明是空的；Chrome 浏览器控制 11 月 5–9 日又新开十几个，多在单独的 CLI 里。
- 「最近两周的已知问题」：开头的各版本推送记录换成一段「现在的版本」（桌面版 26.1103、CLI 0.167.0、VS Code 插件 26.1021.22027）；新加 7 行（项目列表、SSH、Linux 发送没反应、定时任务被拒、网站被拦、恢复对话失败、网络安全新提示）；已经修好、两周没有新报告或一直没进展的 12 行挪进折叠区（额度小圆环、SYSTEMROOT、workspace routing 404、DisposableStack、API Key 的 Work 浏览器、周额度掉一大截、OAuth state not found、WSL 组织设置、服务器处理失败、无法加载你的账户、无法加载工作空间设置、Server is draining），每一条的报错原文都还在「按报错找」里（SYSTEMROOT 那一行是新补的）。按报错找加 7 行、改 4 行。页首「这几天问得最多的」换成 11 月 10 日的 8 条。
- 版本：页首和安装一节改成 CLI 0.167.0（11 月 6 日，npm 国内镜像一致）、桌面版 26.1103。

## 2026-11-05

- 额度规则：12 月起 Plus、Pro、Business 的 ChatGPT 和 Codex 共用一份额度，按 token 计，模型越高级、任务越复杂用得越多，Instant 聊天不算；合并后的剩余额度和刷新时间在 设置 → Usage 看。具体日期、每档额度多大，官方还没写。页首新加一条，第 7 节加一段。依据：OpenAI 帮助中心 Understanding ChatGPT and Codex usage limits（11 月 5 日读到）；11 月 1 日起用户在 V2EX 贴出的 OpenAI 邮件。
- 新增 Postman 一段：10 月 24 日前后起，Postman 桌面版（报告人用的是 12.33.4）会自己往 Codex 里装插件和启动钩子，表现是 `MCP server startup failed server_name="postman"`（`Auth required`）、Codex 总先调 Postman 的技能；Postman 11 月 5 日说撤回这次改动。删法写在第 6 节「插件和 MCP 连不上」（命令取自 `codex plugin --help`，我们没在装了它的电脑上跑过），按报错找加了一行。
- 新增「让 Codex 操作 Chrome 失败」：`The browser access host is unavailable`、`Cannot read properties of undefined (reading 'timeoutMs')`，11 月 2 日起变多，没有官方回复；一位 Linux 用户更新桌面版到 26.1029.61003 后好了。状态表第一行、按报错找各加一行。
- 版本：页首桌面版改成 26.1029（11 月 4 日起 issue 里出现 26.1029.61003、Windows 商店包 26.1029.7803.0，5 日有 Mac 用户贴出 26.1030.10813）；CLI 正式版还是 0.166.0（0.167.0、0.168.0 只有 alpha）；VS Code 插件还是 26.1021.22027。
- 报错状态回 openai/codex 核对到 11 月 5 日：「模型满载」还没退，UTC 11 月 2 日 12 个（重数后）、3 日 8 个、4 日 13 个，issue 里仍没有官方回复，加了用户转述的客服建议（新开一个短一点的对话）和「新对话照样报」的反例；429 一段加 OpenAI 的人 11 月 4 日的回复（改了提示的写法，之后的版本里出，没说等待会不会变）；VS Code 插件到 11 月 5 日仍没有新版本，第 6 节加「换一个全新的 VS Code 配置目录试试」（一位用户）；额度小圆环加「像是分批开放」（一位用户查到要服务器那边打开开关，V2EX 上也有人说自己的还在）；`DisposableStack` 加一位 Windows 用户确认退回插件管用；WSL 那一种「无法加载组织设置」报告人 11 月 3 日说更新后好了；「无法加载工作空间设置」又一位 Mac 用户说不再出现；`0x80070020` 5 日还有人重装后照样报。
- 常见问题「`codex resume` 里看不到以前的会话」加一段：接过中转站、DeepSeek 再换回官方（「转正」）后旧会话不见了，记录还在本地；CLI 0.166.0 源码里这个列表按当前 `model_provider` 过滤，`--all` 不去掉这一条，临时看用 `codex resume --all -c model_provider="旧名字"`。来源：LINUX DO 上的提问和 openai/codex 源码。「无法加载工作空间设置」一行加 LINUX DO 上一位要开 TUN 才拉得到模型配置的报告。
- 常见问题「Computer Use 插件不可用」：按官方文档现在的写法改了装法（设置 → Computer use → `Any App`）。文末「官方来源」加上帮助中心 20001625，状态的核对日期改成 11 月 5 日。

## 2026-11-03

- 重置：Tibo 的 28 天计划 11 月 2 日结束。最后两天各发了一张重置卡（11 月 1 日 08:43「Banked 1/2」、11 月 2 日 07:27「Day 28/ Banked 2/2」，第 26 天他提前预告过），10 月 28 日之后没有再全体重置。页首「重置」一行和第 7 节「手上有重置机会」一格随之更新。依据：Tibo 的原帖。
- 版本：页首桌面版改成 26.1027（10 月 31 日以后 openai/codex 新开的 issue 里报的是 26.1027.61615、26.1027.30824 等，Windows 商店包 26.1027.3384.0）；CLI 仍是 0.166.0（npm），VS Code 插件仍是 26.1021.22027（插件市场）。
- 报错状态回 openai/codex 核对到 11 月 3 日：「模型满载」UTC 11 月 2 日又多起来（当天 10 个 issue，26.1027 / 0.166.0，Luna、Sol、Astra 都报，没有官方回复），页首、状态表和第 6 节「模型满载」的历次波次都加了一条；VS Code 插件到 11 月 3 日还是没有修好的新版本，新 issue 少了，又有两位 Windows 用户确认退回 26.1007.21434 管用；额度小圆环在 26.1027 上仍不显示；429 一段加 11 月 2 日两位 Pro 用户的报告；「无法加载组织设置」加 26.1027 上的确认和一个原因不同的新 issue；Windows `0x80070020` 仍有（11 月 2–3 日 3 个新 issue，其中一位写明 26.1027）；`DisposableStack` 加 Windows 本机 VS Code 插件里的报告。
- 压缩后忘了任务（#57608）：报告人北京时间 11 月 1 日自己关了 issue，跟帖的人查到和实验功能 `context_management` 有关（0.166.0 源码里默认关；`codex features list` 可以看自己开没开，开着的可以 `codex features disable context_management`），写明是用户的办法。

## 2026-10-30

- Pro 额度翻倍（美国时间 10 月 29 日）：Pro 100、Pro 500 的 Work / Codex 额度翻倍；Pro 200 原定下调老订阅额度的计划取消，较低的那档翻倍到和较高的一样。三档约为 Plus 的 10、20、50 倍（50 倍是 Codex 负责人 Tibo 回复网友时确认的）；Chat 里 GPT-6 Pro 的每周额度 Pro 100、Pro 500 不变，Plus 不变。页首套餐说明、第 7 节「经常用完」一行和文末开通方式随之更新。依据：OpenAI 帮助中心 About ChatGPT Pro tiers（10 月 29 日更新）、Tibo 当天的原帖。
- Pro 200 的 AONIR 链接换成新地址 `/chatgpt-pro-200/`（旧地址自动跳转，查询参数保留）。
- 版本：CLI 28 日发 0.165.0、30 日发 0.166.0（npm 国内镜像、Homebrew 都已跟上）；桌面版 27 日起是 26.1023（26.1023.50603、26.1023.51421）；VS Code 插件还是 26.1021.22027，没有修好的新版本。
- 新增「额度小圆环不见了」：桌面版左下角头像上面的 5 小时 / 每周圆环 25 日起陆续消失，28 日更新到 26.1023 后报的人最多（十几位用户，没有官方回复）。额度没变，到 设置 →「使用情况和计费」看，或者输入框里 `/` 选「状态」。第 7 节「先看还剩多少」改成新位置。
- 上下文一节补全：「正在压缩上下文 · 可能需要几分钟」是正常提示；压缩完丢了任务（#57608，30 日还有人在 26.1023.51421 上遇到）；提前压缩，0.166.0 修了恢复、分叉会话后推理 token 计数出错；`model_context_window`、`model_auto_compact_token_limit`、`tool_output_token_limit` 三项设置（官方配置参考）。
- 第 4 节：CLI 0.166.0 的 `codex doctor --network-only`，只查配置和网络（我们用 0.166.0 跑过）。
- 第 1 节：Windows 应用内「安装更新」装不上（`0x80073CF9` / `0x800700E9`，到 Microsoft Store 点「重试」）；Homebrew 装的 CLI 升到 0.165 后 `killed`（`brew reinstall --cask codex`）。
- 状态：MXC 缺 `SYSTEMROOT` 的 issue 28 日关闭，一位用户在 26.1023 上没再遇到，修复 #56468 进了 CLI 0.166.0、没进 0.165.0；「无法加载工作空间设置」#51917 的报告人说 26.1023.51421 上好了；429 一波 27 日起新开的 issue 少了，但 28 日还有两位 Pro 用户在报；`at capacity` 27–29 日每天 4–5 个；Tibo 28 日又一次全体重置。
- 桌面版设置里的「用量和账单」在 26.1023 里叫「使用情况和计费」，第 7 节几处一起改了。
- 更正：Pro 三档「约为 Plus 的 10、20、50 倍」写明是 Codex 负责人 Tibo 的说法（OpenAI 帮助中心只写了额度翻倍，没写倍数），页首和第 7 节都改了；第 10 节 CC Switch 一句措辞调整。

## 2026-10-27

- 新模型 GPT-6.1 Terra（美国时间 10 月 26 日）：只在 Work 和 Codex，Plus、Pro、Business、Enterprise、Edu 可用，普通对话里没有，Free 和 Go 不包括。Plus 每 5 小时约 35–285 条（GPT-6.1 Sol 是 15–160），CLI 要 0.159.0 以上，`codex -m gpt-6.1-terra`。页首套餐说明、第 6 节「模型不支持」的推荐模型、第 7 节估算表随之更新。依据：ChatGPT Learn 更新日志、Models、Pricing。

## 2026-10-26

- 版本：桌面版 24 日起大多是 26.1021.31345（build 21828，Windows 商店包 26.1021.3705.0）；26.1021.22027 既是 23 日前后的一版桌面版，也是 VS Code 插件 24 日自动更新到的版本，别弄混。CLI 还是 0.164.0。
- 新增「VS Code 插件打不开：Codex could not start」：插件 26.1021.22027 之后侧边栏只有 `Codex could not start`、日志 `renderer_ready_timeout`，至少 16 个 issue，没有官方回复。退回 26.1007.21434 并关掉自动更新（十几位用户确认），其次退回 VS Code 1.142.0，或把 Codex 视图挪个位置。
- 新增「被标记为网络安全风险」：「此聊天已被标记为可能存在网络安全风险」「正在进一步审慎考虑此请求」「此内容无法显示」是网络安全检查拦下或放慢了这次请求，这一条本身不是封号；官方 Cyber safety 文档说正常操作也可能触发，误判用 `/feedback`。CLI 的 `Your account was flagged for potentially high-risk cyber activity` 是一句写死的提示，模型名是旧的。
- 429：10 月 25 日起周额度还剩八九成也一直 `429 Too Many Requests`，多是 GPT-6.1 Sol；用户抓到的原始返回是 `turn_rate_limit_exceeded` / `Codex is at capacity`，和「模型满载」是一类。`Selected model is at capacity` 本身 24 日起少了。
- 「无法加载组织设置」：Windows 26.1021 上新出的一种，原因是旧的 `CODEX_CLI_PATH` 用户环境变量（三位用户确认，删掉后重启）。
- 官方安装脚本 404：OpenAI 26 日关掉 issue，说是临时的路由问题，已恢复。
- Windows 自定义权限导致 MXC 缺 `SYSTEMROOT`：不只是 `deny` 规则，桌面版里所有自定义权限配置都会；修复 #56468 已合并，还没进任何版本。
- 重置：按帮助中心（26 日重读）的说法，重置卡、全体重置、付费重置都可能改下一次刷新时间；设置里的「历史记录」能看 30 天重置卡记录；别为了测试再用一张。第 7 节补上 25 日、26 日两次全体重置。
- 精简：已知问题表里已修好的 4 行（启动聊天时出错、安装脚本 404、Windows 沙盒 `os error 32`、WSL `os error 2`）收进表下面的折叠区；折叠区里 9 月下旬已经修好的 10 行删掉，「按报错找」里对应的几行直接写上答案；「模型满载」里 10 月 9–23 日的逐波记录压成每波一行；「更新后打不开」开头 6–9 月的旧版本列表删掉；文末「中文界面文字取自哪一版」的长段挪到本页最后的「出处」。所有报错原文都还在「按报错找」里。
- 其他：后台服务 `codex app-server daemon update`；`codex resume --all`（OpenAI 的人的回答）；`Reconnecting... waiting for network`；「无法生成提交信息」和「提交消息时出错」不是一回事；按报错找补了请求超时、令牌交换失败、电脑操控、被限流 / 被风控、重置卡用不了、续费会不会刷新额度、卡顿、10053 / 10054 的中文原文等写法。

## 2026-10-24

- 版本：CLI 0.164.0 正式版 10 月 23 日发布（npm 国内镜像、Homebrew 都已跟上）；桌面版 10 月 21–22 日连推 26.1016.51333、61520、62342，24 日起有人收到 26.1021.22027。
- 新增「启动聊天时出错 / Cannot serialize value: (couldn't stringify value)」：26.1016.51333 只在「工作树」模式新建对话时出错，26.1016.61520 起已修；更新不了先切「本地模式」。
- 新增「官方安装脚本 404」：`chatgpt.com/codex/install.sh` 10 月 23–24 日断断续续 404，`codex update` 失败了还显示更新成功。改用 GitHub Releases 上的同一份脚本（逐字节核对过）或 npm 镜像。
- 新增「重置机会用不了：你当前的用量暂时无需重置」：OpenAI 帮助中心写明至少一项用量剩余不到 80% 才能用；各套餐补哪几项、在哪用（含 CLI `/usage`）、「允许助手使用用量重置次数」、全体重置和重置卡的关系、客服不补；付费的「购买即时重置」会改每周刷新时间。第 7 节补上 10 月 23 日、24 日两次重置。
- `Selected model is at capacity`：21–23 日每天还有 18–29 个新 issue，没有官方回复；状态页 22 日记了印度、日本、韩国用户报错增多；用户对照数据：同一账号同一分钟桌面版全失败、CLI 七成能过，原始返回是 HTTP 200 里的 `server_is_overloaded`。
- 新增：Windows「无法遍历该路径，因为它包含不受信任的装入点」（开着阿里云盘，或应用装在 C 盘以外）；Mac 磁盘被崩溃报告目录吃掉几十上百 GB（`Crashpad/pending`）；Computer Use、应用内浏览器不可用时按官方文档查的三件事；桌面版本身卡顿时看 `syspolicyd` / `trustd`。
- 补上中文界面的「提交消息时出错」（`Error submitting message`）：「提交消息时出错 + Work browser access requires an authenticated account」在 Bing 上 3 个月有 293 次展示，以前只收了英文。
- Windows 每次更新后打不开：任务管理器里再结束 `codex-windows-sandbox-service.exe`（一位用户的办法，他的安装日志里新版本因为有程序没关而装不上）。
- `Work browser access requires an authenticated account`：用 CC Switch 的把「直连」换成「路由」也行（两位用户确认）；路由模式下日志里的 `websocket_not_supported` 是 CC Switch 有意回的 426，能回复就不用管。CC Switch 换了 ChatGPT 账号 CLI 不生效：`--no-daemon`；`--disable tui_app_server` 早已无效。
- 已知问题表：新增 4 行（启动聊天时出错、安装脚本 404、重置门槛、Windows 自定义权限里的 `deny` 规则导致 MXC 启动失败）；更新 `/codex/models` 404（报告人说约 10 小时后自己好了）、Safari `DisposableStack`（issue 被标成已完成，未见用户确认）、「无法加载工作空间设置」（26.1016.61520 上有好有坏）、`0x80070020`（26.1016 上仍有）。

## 2026-10-21

- 周额度骤降有了官方说法：10 月 21 日凌晨 Tibo 说约 4 万个账号用的额度上限和所选套餐对不上，已修正，会逐个联系并补 credits。发帖人贴出的邮件「Correcting your usage limit」：从 Pro 200 换到更便宜的套餐后一直按旧额度算，修正时把这周已用的量按新上限重算；补 $500（12,500 credits，2027-01-20 到期），周额度没有退回。
- `Selected model is at capacity` 10 月 19 日起又一波（20 日新开 19 个 issue），状态页没有记录，没有官方回复；有用户发现只有 `codex resume` 的旧会话报，新会话正常。
- 新增 `rejected: blocked by policy`：开着完全访问时被标记的命令直接被拒，OpenAI 的人回复改成「帮我批准」（`/permissions` → Approve for me）。同一节补「自动审核超时」「命令被自动审核拒绝」和 `/approve`。
- 新增：只用 API Key 时桌面版 `Error submitting message` / `Work browser access requires an authenticated account`，把 `conversationDetailMode` 改回 `STEPS_COMMANDS`（用户办法，三人确认）。
- 新增「插件和 MCP 连不上」：`OAuth state not found` 改到网页版连接；MCP 启动超时（默认 30 秒）用 `startup_timeout_sec` 放宽。
- 新增：Windows 登录 `os error 10013` / 1457 被占却没有程序：端口被 Windows 保留，用设备码登录或临时停 WinNAT。登录页 `Sign-in could not be completed`、`State mismatch`。
- 新增：`402 Payment Required` / `deactivated_workspace`（Team 席位到期后登录还指着旧工作空间，Windows 桌面版可能卡在启动画面）。
- 第 4 节加「办法三」：`~/.codex/.env` 写代理（CLI 会读）；桌面版 `workspace routing discovery timed out` 要设 Windows 用户变量；`timed out` 一类报错的去处。
- 第 8 节新增「选了中文，界面还是英文」（启动时连不上 OpenAI 就回落英文）和「Windows 上中文乱码」（GBK；PowerShell 7 + `chcp 65001`，或系统 UTF-8）。
- Windows：CLI 的「无法设置管理员沙盒」原文；`prefer_mxc` 导致 `HRESULT = 0x80070020` 的提醒；Agent 设在 WSL 时 `Unable to load organization settings`（改回 Windows）；中文用户名有进程没窗口；`CODEX_CLI_PATH` 指着旧版时的 `os error 32`；Microsoft Store 下载不了。
- Codex 网页版在 Safari、iPad 上 `Can't find variable: DisposableStack`。
- 后台服务：OpenAI 的人确认 CLI 新、后台服务旧会导致 Windows 弹窗口，`/daemon` → Install latest public stable。
- 「无法加载工作空间设置」：Windows 26.1013.40934 上也有了，二十多个 issue；改正一处：日志是 503 的那位用的是 26.1007.71326，不是 26.1013.40934。
- 远程控制：Windows 上有一条长连接不认系统代理和 `HTTP_PROXY`，开 TUN 更省事；别开 `respect_system_proxy`。
- 常见问题：桌面版字号怎么调；`/feedback` 报「未能验证反馈权限」；`codex exec` 报 `Not inside a trusted directory`（`git init` 或 `--skip-git-repo-check`）。
- 补上中文界面的「无法加载组织设置 / 应用已暂停，等待安全加载组织设置后才能恢复」；WSL 模式下换了 Wi-Fi 被当成「公用网络」、防火墙拦住 WSL 到代理端口的连接，改回「专用网络」（LINUX DO 上用户查到的）。
- 新增：换中转站后旧会话 `invalid_encrypted_content`（推理内容只有原来那一家能解）；`command failed; retry without sandbox?` 是批准提示，不是报错。
- 页首的开通方式从四行价格表改成一行文字，链接和价格不变；开头改成「各套餐能用的 Codex 不一样」。删掉了「CLI 要 Plus 起步」：官方定价页只是把 CLI、编辑器插件列在 Plus 的说明里，没写 Free、Go 用不了；补上官方写明的「云端任务不含 Free、Go」。
- 「最近两周的已知问题」：新增 8 行，9 月下旬到现在没有进展的 9 行收进表下面的折叠区；第 7 节 Pro 倍数改为「约为」，最近一次重置改为 10 月 20 日。

## 2026-10-18

- 新增「按状态码查」：400、401、403、404、429、500/502、503 各自在 Codex 里多半是什么情况，链到对应的小节。核对过的：模型不支持的完整返回是 `"status":400`；地区不支持是 403 加 `unsupported_country_region_territory`；at capacity 常带 503。
- 新增：`approval_policy = "untrusted" is no longer supported; remove this setting`。官方配置文档写明这个值已经不支持（`on-failure` 也不推荐了），老教程里的配置会让新版本报错，桌面版可能只显示 `Unable to load organization settings`；10 月 15 日 #48324 里有用户删掉这一行后恢复。
- 新增：周额度几十秒内掉一大截（10 月 17 日凌晨起，#54677、#54718），没有官方回复；10 月 18 日 05:12 有一次全体重置。
- 「模型不支持」加第四种原因：刚升级或换了套餐，登录凭证里还是旧套餐（#53020），先退出登录再登录。
- 「模型满载」：10 月 15 日 Tibo 说上了一批改动、加了算力，报错会大幅减少，碰上也会自己接着跑；之后每天新开的相关 issue 从 18 个降到 0 个。
- 「无法加载工作空间设置」：26.1013.40934 上仍有人遇到；一位 Windows 用户发现拦截是从出口 IP 变化那一刻开始的，开着代理的别来回切节点。
- 「Windows 设置未完成」：补官方文档里新的 MXC 沙盒，不需要管理员授权，桌面版在支持的电脑上会自动用，CLI 用 `[features] prefer_mxc = true` 打开。
- Windows `os error 32`：10 月 16 日又有一位用户确认 CLI 0.163.0 修好了。
- 第 7 节：最近一次全体重置改为 10 月 18 日；补重置卡的到期时间怎么看；补 10 月 17 日 Tibo 说的 Codex 和 Work 实际可用量提高。
- 常见问题加两条：能不能用中国手机号验证、为什么这么慢（Fast 模式按 2.5 倍扣额度）。
- 「最近两周的已知问题」：状态复核到 10 月 18 日；9 月下旬那批已修好或已过去的 13 条收进表下面的折叠区，表里只留还没解决和刚修好的。
- 页首核对日期改为 10 月 18 日，桌面版写 26.1013。

## 2026-10-16

- 新增「一直正在思考、卡住不动」：桌面版「正在思考」「已处理 …」、CLI `Working (… • esc to interrupt)` 一直不结束时，分五种情况认：在等批准、命令没退出、任务本身慢、连接断了、界面没刷新。处理顺序的前三步来自官方 Troubleshooting；CLI 0.163.0 修了「命令结束了、子进程还占着输出，这一轮就不结束」的情况。
- 新增「权限：不能联网、写不了文件、完全访问权限是灰的」：沙盒默认不让命令联网；不开完全访问也能联网的三种开法（设置里的「允许网络访问」、`config.toml`、CLI 一次性参数）；可写目录怎么加；「已被 requirements.toml 禁用」是管理员策略，策略文件在哪、本机改不动。依据是官方 Sandbox、Agent approvals & security、Permissions、Managed configuration 几篇。
- 新增「远程控制：无法启用远程控制」：四句报错的中英文，官方 Remote connections 的五步排查，以及用户查到的三种情况（另一个 Codex 后台服务占着、迁移助理复制了 Mac 的身份文件、系统代理），都注明没有官方回复。
- 常见问题加两条：归档是什么意思、归档的对话在哪里、怎么恢复和删除（归档不是删除；桌面版归档的三种办法，侧边栏右键里的「永久删除」；CLI 的 `codex archive` / `unarchive` / `delete`）；「恢复对话失败」的两种原因。
- Codex CLI 0.163.0 已发布（北京时间 10 月 16 日，npm 与 npmmirror 一致）。它带上了 Windows 沙盒 `os error 32` 的修复（0.162.1 里没有），当天有一位用户确认好了；「最近两周的已知问题」和「Windows 沙盒」两处同步。「换成 unelevated 沙盒」的办法补上有人说不管用。
- 桌面版 10 月 16 日出了 26.1013.31256：WSL `Failed to create unified exec process (os error 2)` 有一位用户确认在这一版上修好了（对应官方修复 #53412），表和正文同步；「无法加载工作空间设置」在这一版上仍有人遇到（macOS 虚拟机）。
- 「按报错找」加 5 行；中英界面对照补 12 条，共 129 条。

## 2026-10-15

- GPT-5.5 已在 10 月 14 日从 ChatGPT 和 Codex 下线：页首和「最近两周的已知问题」里「要提前改」的说法改成已发生，设置里还写着 `gpt-5.5` 的会报模型不支持。
- 「模型满载」补 10 月 12–14 日这一波：12 日、13 日 openai/codex 每天又新开十几个相关 issue，有人额度剩 94% 七个模型全报、有人换模型照样报。和上一波不同，13 日、14 日状态页记了两次 Codex 性能下降，都已标记解决，附事故记录链接。
- VS Code 插件语音输入 `Dictation stopped` / `HTTP 403`：OpenAI 10 月 14 日回复已在服务器端修好，不用更新插件，表里改为已修。
- WSL `Failed to create unified exec process (os error 2)`：OpenAI 10 月 12 日回复「会修」并关闭了 #49731，10 月 14 日有人在新版 26.1007.71326 上仍遇到（#54063），写进表和正文。
- 「无法加载工作空间设置」：新版 26.1007.71326 上仍有人遇到，状态仍是没修。
- 「最近两周的已知问题」各行回到原 issue 复核到 10 月 15 日；「上下文满了」补一句：10 月 12 日起 GPT-6.1 Sol 在 Codex 里默认上下文从 272k 提到 500k；「额度用完了」里最近一次全体重置改为 10 月 13 日，定价页估算和 Pro 不限 5 小时复核未变。

## 2026-10-13

- 新增「登录后跳到 localhost 打不开：ERR_EMPTY_RESPONSE」：邮箱、验证码、工作空间都过了，最后浏览器显示「localhost 未发送任何数据」或「localhost 拒绝了我们的连接请求」（CLI 0.158.0 起是 `127.0.0.1`）。来自我们处理的一例（Cursor 连着 SSH 远程）。写了怎么认三种情况（编辑器连着远程、本机 1455 上是别的程序、Windows 插件跑在 WSL 里），以及设备码登录的 5 步和 5 个容易卡住的地方：Cursor 弹出的 `Do you want Cursor to open the external website?` 是链接保护不是报错，浏览器先出现登录页是正常的。那一例里设备码登录后浏览器仍跳到 localhost，原因没有查实，写了可以试的办法并注明。
- 文字和行为对过源码和安装包：Chromium（浏览器报错的中英文）、openai/codex `rust-v0.162.1`（回调地址、备用端口 1457、设备码页面地址、15 分钟有效期）、VS Code 插件 26.51007.21434（`More options` → `Use device code`，中文「更多登录选项」→「使用设备码」）、Cursor 3.24.12 和 VS Code 1.140（链接保护对话框；Cursor 默认不信任 `auth.openai.com`，VS Code 信任）。
- 「2. 登录」的设备码三步写具体：开关的中英文全名、插件里的入口、先登录再输代码。
- 「按报错找」加两行。
- 当天再补：Cursor 的插件市场给的是 9 月 11 日的 26.5908.31748（自带 CLI 0.154），所以 Cursor 里报错写 `localhost`，VS Code 里写 `127.0.0.1`。这个旧版的登录页和设备码对话框与新版逐字相同，上面的步骤对 Cursor 同样适用。

## 2026-10-12

- 新增「服务器处理失败：An error occurred while processing your request」：重连 5 次后显示 `stream disconnected before completion: An error occurred while processing your request`，带一串 request ID。这句是 OpenAI 服务器发回来的，不是网络问题（对过 0.162.1 的源码）。10 月 9–12 日 openai/codex 新开 10 个相关 issue，没有官方回复，状态页没有事故记录。写了用户报告里的规律（跟着会话走、多是图片多的长会话、压缩也会失败）和 6 步处理办法，都注明是用户试出来的。
- 「断线重连」加一张表：`stream disconnected before completion` 冒号后面的四种写法各是什么原因。原来整节写的「多半是网络问题」只适用于 `error sending request` 这一种，已改。
- 「按报错找」「最近两周的已知问题」「这几天问得最多的」各加一行。
- 新增「无法加载你的账户（We couldn't load your account）」：桌面版登录后只有这一页，点「重新加载」没用。来自 [Issue #1](https://github.com/Avarce/codex-china-guide/issues/1) 的读者反馈（macOS 26.1007，同一台电脑老账号正常、新账号不行）。这一页是兜底提示，原因不止一种：写了怎么从日志的 `failureType=` 看卡在哪一步（对过 26.1007.21159 的安装包），以及 openai/codex 里几份报告查到的情况和用户试过的 6 个办法。引用的 issue 都没有官方回复。
- 上面这条在「按报错找」「最近两周的已知问题」「桌面版打不开」各加一行；中英界面对照补 2 条，共 117 条。
- 同一节当天再补：读者贴回的日志是 `failureType=undefined result=succeeded`（账号查到了，卡在下一步的权限判断），把这一行的说明按安装包写具体；加第二条日志命令，统计当天失败的请求；第一条命令补上按时间排序。

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
- 「手上有重置机会」一行更新到最新：最近一次全体重置是 10 月 11 日，最近一张重置卡是 10 月 8 日。

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

## 已知问题表的详细经过

11 月 14 日从 README「最近两周的已知问题」挪来：表里每行只留现在的状态，下面是挪走前那一格的全文（状态核对到 11 月 14 日），按原来的顺序。

### 桌面版左边栏的项目全不见了，只显示「没有项目」（No projects），以前的聊天只在「最近」里

**桌面版 26.1106.51428 修好了**：11 月 11 日起 Mac、Windows 上已有三位用户确认（[Mac](https://github.com/openai/codex/issues/59895#issuecomment-6791143085)、[Windows](https://github.com/openai/codex/issues/59895#issuecomment-6792640692)、[Windows](https://github.com/openai/codex/issues/59895#issuecomment-6811371703)），一位 Windows 用户说更新后原来的项目结构还是没回来（[回复](https://github.com/openai/codex/issues/59895#issuecomment-6806945456)）。11 月 13 日又一位 Windows 用户在 26.1111（商店包 26.1111.2228.0）上报：更新后自己加的本地项目又不见了，聊天还在（[#61294](https://github.com/openai/codex/issues/61294)）。之前：北京时间 11 月 10 日凌晨 OpenAI 的人说已经知道，修复在「周二的版本」里发（[#59895](https://github.com/openai/codex/issues/59895#issuecomment-6746247810)）；10 日下午 26.1103.60933 上还有人遇到。11 月 6–10 日新开了十几个 issue

### 让 Codex 定时盯一个任务（比如「每 15 分钟看一下训练跑完没有，跑完或失败告诉我」），它拒绝，引用一条内置规则：Do not create an aut

没有官方回复（[#59546](https://github.com/openai/codex/issues/59546)，21 人点了表情、10 条回复；11 月 12–13 日又有四位用户报，其中一位发现更早建的旧对话里还能建，新对话、从旧对话分出来的对话都会拒：[回复](https://github.com/openai/codex/issues/59546#issuecomment-6834228340)）。报告的人说这和[官方文档](https://learn.chatgpt.com/docs/automations#schedule-a-task-inside-a-chat)里「在聊天里定时检查长任务、直到跑完」的用法矛盾：定时功能本身能用，是内置提示词管得太宽

### 让 Codex 操作 Chrome（官方的 ChatGPT Chrome 扩展）时报 The browser access host is unavailabl

没有官方回复。带 `The browser access host is unavailable` 原文的 issue，UTC 11 月 2–5 日新开了 10 个（[#58856](https://github.com/openai/codex/issues/58856)、[#59300](https://github.com/openai/codex/issues/59300)）；带 `timeoutMs` 那句的 4–5 日新开了 6 个。有人是同一台电脑上桌面版能用、单独装的 CLI 不行（#59300）。一位 Linux 用户把桌面版从 26.1013.31256 更新到 26.1029.61003 后好了（[#59276](https://github.com/openai/codex/issues/59276)，只有他一位）。11 月 5–9 日又新开了十几个，桌面版 26.1103、CLI 0.167.0 上也有，多是在单独装的 CLI 或第三方客户端（比如 T3 Code）里报，同一台电脑上桌面版里正常；还是没有官方回复。11 月 11 日又一位用户把桌面版更新到 26.1106.51428 后好了，读页面、截图、下载都正常（[#59276](https://github.com/openai/codex/issues/59276#issuecomment-6803789577)）

### VS Code 插件打不开：侧边栏只有 Codex could not start / The extension could not start its us

**已修**：北京时间 11 月 11 日 14:50，OpenAI 的人说修好的版本已发布，正式版 26.1106.51428、预览版 26.51106.51428，更新后重新加载窗口即可，同时关闭了 #56655（[回复](https://github.com/openai/codex/issues/56655#issuecomment-6789168064)）。到 12 日下午，更新后三位 Linux 用户说好了；一位 Intel Mac 用户说正式版和预览版都还是打不开（[回复](https://github.com/openai/codex/issues/56655#issuecomment-6797222340)），另有两位说还不行或偶尔不行（其中一位改过插件的超时设置）。13 日傍晚，那位 Intel Mac 用户又说装上 26.1111.21308 之后好了（[回复](https://github.com/openai/codex/issues/56655#issuecomment-6849321132)）。之前：北京时间 11 月 10 日 14:26 说已经复现，打算「明天」发修好的版本（[回复](https://github.com/openai/codex/issues/56655#issuecomment-6760672154)），到 10 日下午插件市场还没有新版本；11 月 5–10 日又新开了 10 个。10 月 24 日以来至少 30 个 issue（[#56655](https://github.com/openai/codex/issues/56655)、[#56691](https://github.com/openai/codex/issues/56691)、[#56660](https://github.com/openai/codex/issues/56660)、[#56617](https://github.com/openai/codex/issues/56617)），27–29 日每天还新开四到六个，30 日起少了（按日志里 `renderer_ready_timeout` 这句数，UTC 30 日 2 个、31 日 1 个、11 月 1 日 0 个、2 日 1 个、3 日 1 个、4 日 2 个）；到 11 月 5 日插件市场还没有新版本，4 日还有人说公司三台 Windows 电脑一周多了都用不了（[#56691](https://github.com/openai/codex/issues/56691#issuecomment-6633941415)）。退回 26.1007.21434 十几位用户确认管用，27 日、29 日、30 日和 11 月 1 日又各有一位确认（后两位在 Windows 上：[30 日](https://github.com/openai/codex/issues/56691#issuecomment-6512360066)、[11 月 1 日](https://github.com/openai/codex/issues/56691#issuecomment-6547173116)）；也有几位说退回了还是不行，其中一位 Intel Mac 用户换一个全新的 VS Code 配置目录就能用（[#56655](https://github.com/openai/codex/issues/56655#issuecomment-6609163448)）

### 周额度还剩八九成，却一直报 exceeded retry limit, last status: 429 Too Many Requests，或者「正在思考」好

状态页没有记录。北京时间 11 月 4 日 07:04，OpenAI 的人在 [#58883](https://github.com/openai/codex/issues/58883#issuecomment-6617728562) 里回复：已经改了这句提示的写法，让它说得更清楚，会在之后的版本里出；只说了提示，没说等待本身会不会变。报告的人说 25 日开始遇到，26 日 openai/codex 里新开了 5 个带这句原文的 issue（[#57072](https://github.com/openai/codex/issues/57072)、[#57073](https://github.com/openai/codex/issues/57073)、[#57084](https://github.com/openai/codex/issues/57084)）。一位用户抓到了原始返回：HTTP 429，`turn_rate_limit_exceeded`，`Codex is at capacity, please try again later`，让 205 秒后再试。**27 日起新开的 issue 少了**（按标题或正文里有这句原文数，27–29 日都是 0 个），但 28 日 [#57084](https://github.com/openai/codex/issues/57084) 里还有两位 Pro 用户在报，其中一位 12 轮对话里碰上 14 次。11 月 2 日又有两位 Mac 上的 Pro 用户报（桌面版 26.1027 和 CLI 0.166.0 各一位）：429 之后一等就是几分钟，其中一位记到 419 秒（[#58906](https://github.com/openai/codex/issues/58906)、[#58883](https://github.com/openai/codex/issues/58883)）。11 月 6 日的 CLI 0.167.0 更新说明里没提这句提示，11 月 13 日的 0.168.0 改了 CLI 里的写法：碰上 429 时，重连提示从 `Reconnecting... 2/5` 变成 `Server is busy, reconnecting 2/5`（[#59253](https://github.com/openai/codex/pull/59253)，更新说明里有这一条）；桌面版中文界面是「服务器繁忙，正在重新连接」；11 月 5–9 日只新开了 1 个带这句原文的 issue

### Windows 桌面版更新后，一打开就是「无法加载组织设置」（Unable to load organization settings），点重试、退出登录都回到

没有官方回复。三位用户查到同一个原因：电脑里留着一个旧的用户环境变量 `CODEX_CLI_PATH`，指着早先自己装或复制的 `codex.exe`，删掉就能打开（[#48324](https://github.com/openai/codex/issues/48324#issuecomment-6381489469)、[#56620](https://github.com/openai/codex/issues/56620#issuecomment-6382621764)、[#57051](https://github.com/openai/codex/issues/57051#issuecomment-6416185957)）。也有人删了这个变量还是打不开，日志里是另一种错误（[#56716](https://github.com/openai/codex/issues/56716)），27 日那里有一位用户说在 cmd 里跑一次 `codex update` 就好了。10 月 31 日又一位 Windows 10 用户在 26.1027.3384.0 上确认删掉 `CODEX_CLI_PATH` 就好（[#56620](https://github.com/openai/codex/issues/56620#issuecomment-6541989796)）；11 月 2 日有一个新 issue 在 26.1027.3384.0 上报这一句，日志里是 `policy-read reason=protocol`，像是另一种原因（[#58846](https://github.com/openai/codex/issues/58846)）。11 月 9 日一位开着 WSL2 的用户（26.1103.4239.0）查到另一个原因：BIOS 里没开硬件虚拟化（[#58846](https://github.com/openai/codex/issues/58846#issuecomment-6734826297)）

### Windows 上所有命令都报 CreateProcessSecurityEnvironment failed (HRESULT = 0x80070020)，连

没有官方回复（[#55230](https://github.com/openai/codex/issues/55230)）。11 月 2–3 日又新开了 3 个，其中一位写明是 26.1027.30824、Windows 11 Home 26H2（[#58833](https://github.com/openai/codex/issues/58833)、[#58977](https://github.com/openai/codex/issues/58977)、[#59072](https://github.com/openai/codex/issues/59072)）；11 月 5 日 #59072 里又一位说重装 Codex 之后照样报。第二位报告人查到和新的 MXC 沙盒有关：CLI 加上 `prefer_mxc=true` 就复现，不加就正常。北京时间 10 月 24 日又有两位在 26.1016 上报类似的情况，其中一位连 `cmd.exe /c echo` 都一直挂着不返回；24–26 日又新开了 3 个（[#56770](https://github.com/openai/codex/issues/56770)、[#56788](https://github.com/openai/codex/issues/56788)、[#57069](https://github.com/openai/codex/issues/57069)）。11 月 6 日一位用户报的是 `0x8007000F`，查出来是一块外接硬盘出了故障，拔掉就好了（[#59941](https://github.com/openai/codex/issues/59941)，只有他一位）

### 每条消息都报 Selected model is at capacity，换模型没用，周额度几乎没动；会话日志里是 server_overloaded

9–10 日、12 日状态页都没有记录；13 日、14 日状态页记了两次 Codex 性能下降，都已标记解决。10 月 15 日 Codex 负责人 Tibo 说已经上了一批改动、加了算力，之后每天新开的相关 issue 从 13 日的 18 个降到 17 日的 0 个。**10 月 19 日起又多了**：19 日 8 个、20 日 19 个、21 日 29 个、22 日 22 个、23 日 18 个（按标题或正文里有这句原文统计，UTC 日期），issue 里没有官方回复，Tibo 也没再提。状态页这几天只记了两次：北京时间 21 日 10:26–10:48 ChatGPT、Codex 和 API 性能下降；22 日 10:18–16:50 印度、日本、韩国的用户 ChatGPT 对话和 Responses API 报错增多，用日本、韩国节点的可能碰上（[#55444](https://github.com/openai/codex/issues/55444)、[#55260](https://github.com/openai/codex/issues/55260)、[#55740](https://github.com/openai/codex/issues/55740)）。**24 日起少了**：24 日 2 个、25 日 0 个、26 日 6 个，27–29 日每天 4–5 个；不过 25 日起换了个样子，变成周额度还有、却一直报 429，见上面 429 那一行。状态页 27 日记了一次 GPT-6.1 Sol 性能下降（北京时间 27 日 23:02 到 28 日 00:20）。**11 月 2 日起又多起来**：UTC 10 月 31 日 2 个、11 月 1 日 3 个、2 日 12 个、3 日 8 个、4 日 13 个；桌面版 26.1027、26.1029，CLI 0.166.0，Windows、Mac，Plus、Pro 都有，Luna、Sol、Astra 都报（[#58824](https://github.com/openai/codex/issues/58824)、[#58858](https://github.com/openai/codex/issues/58858)、[#58882](https://github.com/openai/codex/issues/58882)、[#58911](https://github.com/openai/codex/issues/58911)、[#58917](https://github.com/openai/codex/issues/58917)、[#59059](https://github.com/openai/codex/issues/59059)）。到 11 月 5 日 issue 里没有官方回复，Tibo 也没提；状态页只在北京时间 5 日 06:38–08:32 记了一次「付费 ChatGPT 套餐发消息报错增多」，没写 Codex。一位用户 5 日转述了 OpenAI 客服给他的回复：不要把这句提示当成确定的原因，建议新开一个短一点的对话（[#56346](https://github.com/openai/codex/issues/56346#issuecomment-6653342651)）；也有人说全新的空对话只发一句「回复 OK」照样报（[#58858](https://github.com/openai/codex/issues/58858#issuecomment-6626435254)）。**11 月 7 日起少了**：UTC 5 日、6 日各 6 个，7 日、8 日（周末）0 个，9 日（周一）2 个、是同一个人开的；前两波都是周末少、周一又多；之后也没再多起来，10 日 0 个、11 日 2 个、12 日 3 个（其中一个说的是 Astra 回答质量）、13 日 0 个。到 11 月 14 日 issue 里仍没有官方回复

### 桌面版左下角头像上面的额度小圆环（5 小时、每周两圈）不见了；有的版本是头像菜单里不再显示剩余百分比和刷新时间

没有官方回复，不清楚是改版还是出错（[#57656](https://github.com/openai/codex/issues/57656)、[#56889](https://github.com/openai/codex/issues/56889)、[#57333](https://github.com/openai/codex/issues/57333)，加起来十几位用户）。到 11 月 3 日，Windows 上的 26.1027（26.1027.30824、26.1027.6604.0）仍不显示，Mac 26.1027.30824 上有人说它不再更新，Mac 26.1027.61615 上也有人说更新了几次都没回来，还是没有官方回复。一位用户查了自己装的这一版，说这个圆环要服务器那边打开一个开关才显示（[#57656](https://github.com/openai/codex/issues/57656#issuecomment-6596715554)，他的分析）；V2EX 上 11 月 2 日也有几位说自己的还在（[帖子](https://www.v2ex.com/t/1253672)），像是分批开放。额度本身没变，只是不显示了。11 月 5 日以后没有新报告，11 月 5 日还有两位 Windows 用户在 26.1029 上看得到圆环

### Windows 桌面版更新后，旧对话恢复不了、新对话里命令也跑不了，日志里是 failed to load AGENTS.md instructions for

**可能好了，还没坐实。** 10 月 28 日 [#56264](https://github.com/openai/codex/issues/56264) 被关闭；同一天一位用户在商店包 26.1023.6621.0（自带 CLI 0.165.0-alpha.1.2）上用自定义权限配置跑了几个新旧对话，这个错误没再出现（[他的回复](https://github.com/openai/codex/issues/56264#issuecomment-6472322184)，到 30 日只有他一个人回来确认）。修复 [#56468](https://github.com/openai/codex/pull/56468) 我们对过版本：进了 CLI 0.166.0，没进 0.165.0，所以桌面版这一版自带的 CLI 按代码看并不含它，他没再遇到的原因说不清。一开始以为只和 `deny` 规则有关；24 日一位用户逐项对照：桌面版里**所有**自定义权限配置都失败，默认权限和「请求批准」正常，同样的配置在单独装的 CLI 里正常。另一位对照了版本：商店包 26.1016.8120.0（桌面版 26.1016.61520）正常，26.1016.8622.0 和 26.1021.3705.0 都不行

### Codex 网页版发消息就报 Error submitting message、Can't find variable: DisposableStack；iPh

北京时间 10 月 24 日早上，两个 issue 都被 OpenAI 的工单机器人标成已完成（[#55271](https://github.com/openai/codex/issues/55271)、[#55361](https://github.com/openai/codex/issues/55361)），没有说明。到 26 日没有用户回来确认修好，24 日以后网页版也没有人再报这一句；27 日有人在浏览器里跑的 VS Code（Safari）上用 Codex 插件碰到同样的 `DisposableStack is undefined`（[#57370](https://github.com/openai/codex/issues/57370)），是另一处，30 日那里有一位说[更新浏览器后好了](https://github.com/openai/codex/issues/57370#issuecomment-6523840173)。31 日 #55271 下又有一位在 iPad 的 Safari 里（GitHub Codespaces 上的 Codex 插件 26.1021.22027）碰到：第一条能发，第二条报这一句（[回复](https://github.com/openai/codex/issues/55271#issuecomment-6540553990)）；11 月 2 日 Windows 上本机的 VS Code 也有人报第二条消息发不出（[#58817](https://github.com/openai/codex/issues/58817)），所以插件里的这一处不只是 Safari；11 月 4 日那里又一位 Windows 用户确认，把 VS Code 插件退回 26.1007.21434 就能连着发了（[回复](https://github.com/openai/codex/issues/58817#issuecomment-6622157999)）

### 周额度几十秒内掉了一大截：77% 变 10%、58% 变 0%，重置时间没变

**官方认了。** 北京时间 10 月 21 日 01:10，Tibo 说约 4 万个账号实际用的额度上限和所选套餐对不上，已经修正，会逐个联系受影响的人并补 credits（[原帖](https://x.com/thsottiaux/status/2112592264686149749)）。[#54677](https://github.com/openai/codex/issues/54677)、[#54718](https://github.com/openai/codex/issues/54718) 的发帖人收到了邮件，两位都是从 Pro 200 换到了更便宜的套餐，各补 $500（12,500 credits，2027 年 1 月 20 日到期）；周额度本身没有退回。issue 里也有人说自己没换过套餐，是不是同一个原因还不清楚

### Windows 上把 Agent 设成在 WSL 里运行，一打开就弹 Unable to load organization settings，关不掉

没有官方回复（[#54478](https://github.com/openai/codex/issues/54478)、[#54300](https://github.com/openai/codex/issues/54300)）。至少三位用户来回切换确认过：设回 Windows 就正常。**11 月 3 日 #54478 的报告人说 10 月 30–31 日那次更新之后好了，WSL 模式能正常打开，自己关掉了 issue**（[他的回复](https://github.com/openai/codex/issues/54478#issuecomment-6600162006)，只有他一位）。11 月 12 日起 26.1106、26.1111 上 WSL 模式又打不开，原因不同，见上表第一行

### 桌面版新对话发不出去：「无法加载工作空间设置 / 请重试以继续聊天」（Couldn't load workspace settings），旧对话和网页版正常

没修完。26.1013.40934 上 macOS（10 月 17–19 日，实体机和虚拟机都有）、Windows（10 月 20 日）都还有人遇到，加起来二十多个 issue，没有官方回复。26.1016.61520 上有好有坏：一位 Mac 用户说更新后好了，自己关掉了 issue（[#52342](https://github.com/openai/codex/issues/52342)）；另一位 Mac 用户 10 月 22 日在同一版上照样被拦，日志里是 `cloudflare_challenge` 的 403（[#51917](https://github.com/openai/codex/issues/51917)）；10 月 26 日又有 Mac 用户在 26.1021.31345 上遇到。**28 日 #51917 的报告人更新到 Windows 26.1023.51421 后说不再拦新对话了**（[他的回复](https://github.com/openai/codex/issues/51917#issuecomment-6485450352)）；11 月 5 日那里 26 日报过的 Mac 用户也说，之后几次更新下来已经不再出现（[回复](https://github.com/openai/codex/issues/51917#issuecomment-6648588949)）。还有一位 Windows 用户是账号信息和用量一直加载不出来、对话正常：他在 v2rayN 里打开全局代理就好了（[回复](https://github.com/openai/codex/issues/51917#issuecomment-6636496900)）；11 月 2 日 LINUX DO 上也有人说，桌面版新开的对话拉不到模型配置和用量，以前开系统代理就行，现在要开 TUN（[帖子](https://linux.do/t/topic/3110549)）。11 月 5 日 Mac 上的报告人说更新几版之后不再出现（[#51917](https://github.com/openai/codex/issues/51917#issuecomment-6648588949)）

### Windows 上所有命令都在执行前失败：helper_unknown_error: setup refresh had errors，日志里是 os erro

桌面版：10 月 9–10 日多位用户确认 26.1007.21434 **已修**（[#51601](https://github.com/openai/codex/issues/51601)，120 条回复）。CLI：0.162.1 上仍有人复现（[#51634](https://github.com/openai/codex/issues/51634)）；北京时间 10 月 16 日发布的 0.163.0 带上了修复（[#51822](https://github.com/openai/codex/pull/51822)），**当天先后有两位用户确认好了**（[#51590](https://github.com/openai/codex/issues/51590)），10 月 18 日又一位确认（[#51634](https://github.com/openai/codex/issues/51634)）。其中一位说内置浏览器控制还会报 `trusted Node process exited unexpectedly`，那是另一个问题

### Windows 桌面版开了 Run agent in WSL，每条命令都是 Failed to create unified exec process: No 

10 月 12 日 OpenAI 的人回复「会修」并关闭了 issue，但 10 月 14 日有人在新版 26.1007.71326 上仍遇到（[#49731](https://github.com/openai/codex/issues/49731)、[#54063](https://github.com/openai/codex/issues/54063)）。10 月 16 日一位用户确认 **26.1013.31256** 修好了，重启后也正常（[#49731](https://github.com/openai/codex/issues/49731)，对应的修复是 [#53412](https://github.com/openai/codex/pull/53412)）

## 出处：中文界面文字和命令原文取自哪一版

中文界面的文字取自 ChatGPT 桌面版（macOS）自带的简体中文语言包，9 月 24 日按 26.915.31945 整理，10 月 3 日按 26.930.21537 复核，10 月 11–12 日新增的几条提示、10 月 16 日新增的权限、远程控制、恢复对话和「正在思考」几处文字，10 月 21 日新增的「帮我批准」自动审核、反馈权限、字号几处文字，都取自 26.1007.21159；10 月 24 日新增的「启动聊天时出错」「本地模式」「工作树」、重置相关的「你当前的用量暂时无需重置」「允许助手使用用量重置次数」「购买即时重置」、浏览器插件「已被你的组织停用，或在你所在地区不可用」，取自 26.1016.62342；10 月 26 日新增的网络安全标记（「此聊天已被标记为可能存在网络安全风险」「正在进一步审慎考虑此请求」「此内容无法显示」）、重置「历史记录」「无法重置用量」、「无法生成提交信息」，也取自 26.1016.62342，VS Code 的「安装特定版本...」「自动更新」「移动到」取自 VS Code 官方简体中文语言包（microsoft/vscode-loc）；CLI 的 `Reconnecting... waiting for network`、`Your account was flagged for potentially high-risk cyber activity`、`codex resume --all`、`codex app-server daemon update` 对照的是 0.164.0 的源码；CLI 里的「无法设置管理员沙盒」选择框、登录页的 `Sign-in could not be completed` / `State mismatch`、端口被占时改用 1457 的逻辑、MCP 启动超时的提示和 30 秒默认值、`~/.codex/.env` 的读取、`/approve` 的说明，对照的是 0.163.0 的源码；`codex archive` / `unarchive` / `delete`、`codex app-server daemon stop` 对照了它自带的 Codex（0.162.0-alpha.17.2）的帮助输出和 0.163.0 的源码；CLI 卡住时的 `Working (… • esc to interrupt)` 取自 0.163.0 的源码；`codex doctor` 输出来自我们编辑电脑上的实际运行结果（CLI 0.153.4）；`--no-daemon`、`--no-alt-screen`、`codex app-server daemon …`、`codex features …` 对照了桌面版 26.930 自带的 Codex（0.159.0-alpha.12.1）的帮助输出。11 月 10 日新增的「没有项目」「最近」「连接」「继续聊天未成功」「远程控制授权失败」，以及「对于某些网络安全请求，我们会格外谨慎……」「Daybreak 不适用于 Astra……」两句的中文，取自桌面版 26.1103.32239 的语言包。11 月 14 日新增的「ChatGPT 遇到了问题」「ChatGPT 无法加载你的配置」和补上的英文 `Server is busy, reconnecting`、`This is open in another app`，取自 26.1106.51428。