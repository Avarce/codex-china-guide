# 更新日志

Codex 改了安装方式、登录流程、报错或额度规则，这里记一笔。日期为北京时间。

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

## 出处：中文界面文字和命令原文取自哪一版

中文界面的文字取自 ChatGPT 桌面版（macOS）自带的简体中文语言包，9 月 24 日按 26.915.31945 整理，10 月 3 日按 26.930.21537 复核，10 月 11–12 日新增的几条提示、10 月 16 日新增的权限、远程控制、恢复对话和「正在思考」几处文字，10 月 21 日新增的「帮我批准」自动审核、反馈权限、字号几处文字，都取自 26.1007.21159；10 月 24 日新增的「启动聊天时出错」「本地模式」「工作树」、重置相关的「你当前的用量暂时无需重置」「允许助手使用用量重置次数」「购买即时重置」、浏览器插件「已被你的组织停用，或在你所在地区不可用」，取自 26.1016.62342；10 月 26 日新增的网络安全标记（「此聊天已被标记为可能存在网络安全风险」「正在进一步审慎考虑此请求」「此内容无法显示」）、重置「历史记录」「无法重置用量」、「无法生成提交信息」，也取自 26.1016.62342，VS Code 的「安装特定版本...」「自动更新」「移动到」取自 VS Code 官方简体中文语言包（microsoft/vscode-loc）；CLI 的 `Reconnecting... waiting for network`、`Your account was flagged for potentially high-risk cyber activity`、`codex resume --all`、`codex app-server daemon update` 对照的是 0.164.0 的源码；CLI 里的「无法设置管理员沙盒」选择框、登录页的 `Sign-in could not be completed` / `State mismatch`、端口被占时改用 1457 的逻辑、MCP 启动超时的提示和 30 秒默认值、`~/.codex/.env` 的读取、`/approve` 的说明，对照的是 0.163.0 的源码；`codex archive` / `unarchive` / `delete`、`codex app-server daemon stop` 对照了它自带的 Codex（0.162.0-alpha.17.2）的帮助输出和 0.163.0 的源码；CLI 卡住时的 `Working (… • esc to interrupt)` 取自 0.163.0 的源码；`codex doctor` 输出来自我们编辑电脑上的实际运行结果（CLI 0.153.4）；`--no-daemon`、`--no-alt-screen`、`codex app-server daemon …`、`codex features …` 对照了桌面版 26.930 自带的 Codex（0.159.0-alpha.12.1）的帮助输出。
