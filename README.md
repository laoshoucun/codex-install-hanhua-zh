# Codex/ChatGPT 安装教程与使用指南官网下载

> **官方入口：[www.sz003.com/anzhuang](https://www.sz003.com/anzhuang)**
> Windows / macOS 都能装 · 桌面端、命令行、编辑器三种形态分开讲

## 仓库介绍

本仓库内容整理自 [Codex中文站](https://www.sz003.com/)，已收录 58 篇，每篇都保留官网原文链接：可以在这里直接读，也可以 `git clone` 到本地检索。

Codex 是 OpenAI 推出的 AI 编程智能体，有三种形态：装在电脑上的桌面客户端、在终端里跑的 Codex CLI、以及挂在 IDE 里的插件。三种形态的安装方式、能碰的文件范围、适用的活都不一样，混着讲最容易装错。本仓库按「怎么装、怎么用、怎么排查」三块整理，收录 Codex 中文站已经发布过的安装与使用类文章，每篇都能点回官网看原文。

## 视频演示

[![安装演示（8 分钟 · 从下载到界面变中文）](cover.jpg)](https://www.sz003.com/anzhuang)

点上面这张封面图，会打开官网页面，视频就在页面里直接播，不用下载。

> 视频和安装包都放在官方 CDN 上、不进仓库，这样官方更新之后你看到的一直是当前版本，不会拿到过期的旧包。

## 安装入口

所有安装包统一放在官网安装页，按自己的系统和芯片选，一个链接直达：

### ➡️ [点击进入 Codex 安装下载页（www.sz003.com/anzhuang）](https://www.sz003.com/anzhuang)

| 系统 | 装法 | 说明 |
| --- | --- | --- |
| Windows | 桌面客户端 | 微软商店版与传统安装包两种，装不上先分清自己装的是哪种 |
| macOS | 桌面客户端 | Intel 芯片与 Apple 芯片分开，按芯片下载 |
| Windows / macOS | Codex CLI | 终端命令行工具，四种官方安装方式，npm 那条路已不再推荐 |
| VS Code | IDE 插件 | 在编辑器里开对话，适合不想切窗口的人 |

装之前先确认一件事：桌面客户端和 CLI 是两条独立的路，装一个不等于装了另一个。CLI 装完要在终端里敲 `codex` 能起来才算通；桌面端装完要能看到登录界面。两个都装也不会冲突，但配置分开存，别混着改。

安装步骤、登录、以及装完之后要改哪几项配置，仓库里 58 篇按顺序排好了，从最新往下看即可。

## 主要特点

- 三种形态分开讲：桌面客户端、CLI 命令行、IDE 插件，各讲各的装法和适用场景。
- Windows 和 macOS 都覆盖，微软商店版与传统安装包的区别单独交代。
- 装完之后的配置逐项写：config.toml 常用项、AGENTS.md 怎么写、权限三档怎么选。
- Skills、MCP、Hooks、Plugins 四个扩展机制各有专文，标清彼此区别。
- 报错按现象归类：连不上、装不上、跑不动、一直重连，给出排查顺序。
- 每篇保留官网原文链接，可在线阅读，也可 git clone 到本地检索。

## 常见问题

**装完敲 codex 提示找不到命令？** 九成是 PATH 没生效。关掉终端重开一次；还是找不到，说明装到了用户目录之外，回去看安装那一步的默认路径有没有改过。

**微软商店版装不上怎么办？** 商店版对系统版本和区域有要求，报错码不同原因不同。仓库里《Codex 常见报错》那篇按报错顺序列了排查路径，先看它再动手重装。

**桌面端和 CLI 要都装吗？** 不必须。桌面端适合长任务和看结果，CLI 适合在项目目录里直接改文件。只用一种也能干活，两个都装不冲突。

**装完能直接改我的项目文件吗？** 默认有沙盒与审批机制，要放行才会动文件。权限三档怎么选，仓库里有专文。

## 文章目录（58 篇）

- 2026-09-29 · [自动化：让 Codex 按点自己干活](https://www.sz003.com/jiaocheng/2643) · [仓库内阅读](docs/2643-自动化：让Codex按点自己干活.md)
- 2026-09-28 · [Codex Cloud 怎么用：不装客户端，让它在云端干仓库的活](https://www.sz003.com/jiaocheng/2642) · [仓库内阅读](docs/2642-CodexCloud怎么用：不装客户端，让它在云端干仓库的活.md)
- 2026-09-28 · [在 VS Code 里用 Codex：装插件、开对话、用 @ 点文件](https://www.sz003.com/jiaocheng/2641) · [仓库内阅读](docs/2641-在VSCode里用Codex：装插件、开对话、用@点文件.md)
- 2026-09-28 · [Codex 沙盒与审批（下）：审批弹窗怎么答，被拒绝怎么查](https://www.sz003.com/jiaocheng/2676) · [仓库内阅读](docs/2676-Codex沙盒与审批（下）：审批弹窗怎么答，被拒绝怎么查.md)
- 2026-09-28 · [Codex 从零做网站（下）：管理后台、招商 PPT、宣传视频](https://www.sz003.com/jiaocheng/2674) · [仓库内阅读](docs/2674-Codex从零做网站（下）：管理后台、招商PPT、宣传视频.md)
- 2026-09-28 · [Codex 接 DeepSeek：配置文件怎么写，国内大模型怎么连](https://www.sz003.com/jiaocheng/2675) · [仓库内阅读](docs/2675-Codex接DeepSeek：配置文件怎么写，国内大模型怎么连.md)
- 2026-09-27 · [Codex CLI 命令速查：参数、斜杠命令和报错对照](https://www.sz003.com/jiaocheng/2649) · [仓库内阅读](docs/2649-CodexCLI命令速查：参数、斜杠命令和报错对照.md)
- 2026-09-27 · [Codex 常见报错：连不上、装不上、跑不动，按这个顺序查](https://www.sz003.com/jiaocheng/2648) · [仓库内阅读](docs/2648-Codex常见报错：连不上、装不上、跑不动，按这个顺序查.md)
- 2026-09-27 · [Codex 沙盒与审批（上）：什么时候放行，什么时候必须拦](https://www.sz003.com/jiaocheng/2647) · [仓库内阅读](docs/2647-Codex沙盒与审批（上）：什么时候放行，什么时候必须拦.md)
- 2026-09-25 · [Codex 从零做网站（上）：一句话搭出宠物零食前端页面](https://www.sz003.com/jiaocheng/2673) · [仓库内阅读](docs/2673-Codex从零做网站（上）：一句话搭出宠物零食前端页面.md)
- 2026-09-25 · [Codex 手机远程操控：安卓扫码连上，人不在电脑前也能盯](https://www.sz003.com/jiaocheng/2672) · [仓库内阅读](docs/2672-Codex手机远程操控：安卓扫码连上，人不在电脑前也能盯.md)
- 2026-09-25 · [Codex 做文献综述：把医学问题整理成能复核的证据表](https://www.sz003.com/jiaocheng/2671) · [仓库内阅读](docs/2671-Codex做文献综述：把医学问题整理成能复核的证据表.md)
- 2026-09-24 · [Codex 自动修 CI：接上 GitHub Actions，挂了它自己改](https://www.sz003.com/jiaocheng/2670) · [仓库内阅读](docs/2670-Codex自动修CI：接上GitHubActions，挂了它自己改.md)
- 2026-09-24 · [Codex 直接控制浏览器：接上 Chrome 就能替你去点](https://www.sz003.com/jiaocheng/2669) · [仓库内阅读](docs/2669-Codex直接控制浏览器：接上Chrome就能替你去点.md)
- 2026-09-24 · [Codex 远程修服务器 Bug：从定位到验证走一遍](https://www.sz003.com/jiaocheng/2668) · [仓库内阅读](docs/2668-Codex远程修服务器Bug：从定位到验证走一遍.md)
- 2026-09-23 · [Codex 读懂设计稿：Figma 和 Notion MCP 一起接](https://www.sz003.com/jiaocheng/2667) · [仓库内阅读](docs/2667-Codex读懂设计稿：Figma和NotionMCP一起接.md)
- 2026-09-23 · [Codex 搭 AI 知识库：LLM Wiki 加 Obsidian 一套流程](https://www.sz003.com/jiaocheng/2666) · [仓库内阅读](docs/2666-Codex搭AI知识库：LLMWiki加Obsidian一套流程.md)
- 2026-09-23 · [Codex 一句话处理飞书数据：接上飞书 CLI 就行](https://www.sz003.com/jiaocheng/2665) · [仓库内阅读](docs/2665-Codex一句话处理飞书数据：接上飞书CLI就行.md)
- 2026-09-22 · [Codex 写完文章自动配图：Obsidian 里一套流程搞定](https://www.sz003.com/jiaocheng/2664) · [仓库内阅读](docs/2664-Codex写完文章自动配图：Obsidian里一套流程搞定.md)
- 2026-09-22 · [Codex 自己开浏览器查资料：接上 Playwright MCP 就行](https://www.sz003.com/jiaocheng/2663) · [仓库内阅读](docs/2663-Codex自己开浏览器查资料：接上PlaywrightMCP就行.md)
- 2026-09-22 · [Codex 画架构图：接上 Draw.io MCP，AI 自己出图](https://www.sz003.com/jiaocheng/2662) · [仓库内阅读](docs/2662-Codex画架构图：接上Draw.ioMCP，AI自己出图.md)
- 2026-09-21 · [Codex 一句话生成 PPT：装上这个 Skill 就能用](https://www.sz003.com/jiaocheng/2661) · [仓库内阅读](docs/2661-Codex一句话生成PPT：装上这个Skill就能用.md)
- 2026-09-21 · [Codex 怎么带进团队用？AGENTS.md 管规则，PR 模板管留痕](https://www.sz003.com/jiaocheng/2660) · [仓库内阅读](docs/2660-Codex怎么带进团队用？AGENTS.md管规则，PR模板管留痕.md)
- 2026-09-21 · [Codex 从需求到交付（下）：提交、写 PR、复盘一条链收口](https://www.sz003.com/jiaocheng/2659) · [仓库内阅读](docs/2659-Codex从需求到交付（下）：提交、写PR、复盘一条链收口.md)
- 2026-09-20 · [Codex 从需求到交付（中）：小步改、认真验，改动不失控](https://www.sz003.com/jiaocheng/2658) · [仓库内阅读](docs/2658-Codex从需求到交付（中）：小步改、认真验，改动不失控.md)
- 2026-09-20 · [Codex 从需求到交付（上）：需求怎么拆，它才不跑偏](https://www.sz003.com/jiaocheng/2657) · [仓库内阅读](docs/2657-Codex从需求到交付（上）：需求怎么拆，它才不跑偏.md)
- 2026-09-20 · [Codex 哪些活该丢云上跑？云端和本地怎么分工](https://www.sz003.com/jiaocheng/2656) · [仓库内阅读](docs/2656-Codex哪些活该丢云上跑？云端和本地怎么分工.md)
- 2026-09-19 · [Codex 记不住项目？记忆系统怎么配，不用每次重讲](https://www.sz003.com/jiaocheng/2655) · [仓库内阅读](docs/2655-Codex记不住项目？记忆系统怎么配，不用每次重讲.md)
- 2026-09-19 · [Codex 怎么同时推多条任务线？线程管理和工作树讲清](https://www.sz003.com/jiaocheng/2654) · [仓库内阅读](docs/2654-Codex怎么同时推多条任务线？线程管理和工作树讲清.md)
- 2026-09-19 · [Codex Hooks（下）：怎么写、怎么排查，进阶用法举例](https://www.sz003.com/jiaocheng/2653) · [仓库内阅读](docs/2653-CodexHooks（下）：怎么写、怎么排查，进阶用法举例.md)
- 2026-09-18 · [Codex Hooks（上）：钩子是什么，能在哪些环节自动干活](https://www.sz003.com/jiaocheng/2652) · [仓库内阅读](docs/2652-CodexHooks（上）：钩子是什么，能在哪些环节自动干活.md)
- 2026-09-18 · [Codex 插件 Plugins 是什么？和 Skill、MCP 到底什么区别](https://www.sz003.com/jiaocheng/2651) · [仓库内阅读](docs/2651-Codex插件Plugins是什么？和Skill、MCP到底什么区别.md)
- 2026-09-18 · [Codex Skills 是什么？怎么装、怎么写、装哪些最有用](https://www.sz003.com/jiaocheng/2650) · [仓库内阅读](docs/2650-CodexSkills是什么？怎么装、怎么写、装哪些最有用.md)
- 2026-09-17 · [Codex 权限怎么设才安全？它能碰什么，三档怎么选](https://www.sz003.com/jiaocheng/2646) · [仓库内阅读](docs/2646-Codex权限怎么设才安全？它能碰什么，三档怎么选.md)
- 2026-09-17 · [Codex config.toml 怎么配？常用配置项一次说清](https://www.sz003.com/jiaocheng/2645) · [仓库内阅读](docs/2645-Codexconfig.toml怎么配？常用配置项一次说清.md)
- 2026-09-17 · [AGENTS.md 怎么写？让 Codex 第一次就懂你的项目](https://www.sz003.com/jiaocheng/2644) · [仓库内阅读](docs/2644-AGENTS.md怎么写？让Codex第一次就懂你的项目.md)
- 2026-09-17 · [Codex 可用技能榜第10期：前十名七个在做视频和图](https://www.sz003.com/chajian/2698) · [仓库内阅读](docs/2698-Codex可用技能榜第10期：前十名七个在做视频和图.md)
- 2026-09-16 · [Codex 基本组成：对话、项目和设置面板分别管什么](https://www.sz003.com/jiaocheng/2628) · [仓库内阅读](docs/2628-Codex基本组成：对话、项目和设置面板分别管什么.md)
- 2026-09-16 · [Codex 桌面端安装：macOS 与 Windows 怎么装，装不上怎么办](https://www.sz003.com/jiaocheng/2627) · [仓库内阅读](docs/2627-Codex桌面端安装：macOS与Windows怎么装，装不上怎么办.md)
- 2026-09-15 · [用手机远程操控 Codex：离开电脑也能盯住任务](https://www.sz003.com/jiaocheng/2629) · [仓库内阅读](docs/2629-用手机远程操控Codex：离开电脑也能盯住任务.md)
- 2026-09-15 · [第一次让 Codex 改代码：任务怎么挑，流程怎么走](https://www.sz003.com/jiaocheng/2632) · [仓库内阅读](docs/2632-第一次让Codex改代码：任务怎么挑，流程怎么走.md)
- 2026-09-15 · [怎么判断 Codex 真做完了，而不是看起来做完了](https://www.sz003.com/jiaocheng/2631) · [仓库内阅读](docs/2631-怎么判断Codex真做完了，而不是看起来做完了.md)
- 2026-09-15 · [任务怎么写，Codex 才干得对](https://www.sz003.com/jiaocheng/2630) · [仓库内阅读](docs/2630-任务怎么写，Codex才干得对.md)
- 2026-09-13 · [Codex 能做什么？哪些活不该交给它](https://www.sz003.com/jiaocheng/2626) · [仓库内阅读](docs/2626-Codex能做什么？哪些活不该交给它.md)
- 2026-09-13 · [Codex 安装和登录：四条入口怎么选，装完怎么确认](https://www.sz003.com/jiaocheng/2678) · [仓库内阅读](docs/2678-Codex安装和登录：四条入口怎么选，装完怎么确认.md)
- 2026-09-13 · [从 Copilot 到 Codex：AI 编程工具的四次进化](https://www.sz003.com/jiaocheng/2625) · [仓库内阅读](docs/2625-从Copilot到Codex：AI编程工具的四次进化.md)
- 2026-09-13 · [Codex 画 PPT 实测：一上午干完一天的活，两条路怎么选](https://www.sz003.com/jiaocheng/2624) · [仓库内阅读](docs/2624-Codex画PPT实测：一上午干完一天的活，两条路怎么选.md)
- 2026-09-12 · [Codex 一直重连 1/5？一句话让它自己修好代理](https://www.sz003.com/jiaocheng/2623) · [仓库内阅读](docs/2623-Codex一直重连15？一句话让它自己修好代理.md)
- 2026-09-12 · [用不上最先进的 Codex？先别急着说自己不行](https://www.sz003.com/jiaocheng/2622) · [仓库内阅读](docs/2622-用不上最先进的Codex？先别急着说自己不行.md)
- 2026-09-12 · [Codex 学习提问模板：学新东西先搭阶梯再啃 20% 重点](https://www.sz003.com/jiaocheng/2621) · [仓库内阅读](docs/2621-Codex学习提问模板：学新东西先搭阶梯再啃20重点.md)
- 2026-09-09 · [Codex 该装哪些 Skill：新手第一批按基础/提效/业务三层挑](https://www.sz003.com/jiaocheng/2619) · [仓库内阅读](docs/2619-Codex该装哪些Skill：新手第一批按基础提效业务三层挑.md)
- 2026-09-07 · [Codex Token 消耗太快？改 4 个设置，能省下 80%](https://www.sz003.com/jiaocheng/2617) · [仓库内阅读](docs/2617-CodexToken消耗太快？改4个设置，能省下80.md)
- 2026-09-03 · [Codex 和 DeepSeek harness 怎么选](https://www.sz003.com/jiaocheng/2614) · [仓库内阅读](docs/2614-Codex和DeepSeekharness怎么选.md)
- 2026-09-02 · [Codex 做游戏，这 10 个 Agent Skill 够你搭一支开发团队](https://www.sz003.com/chajian/2607) · [仓库内阅读](docs/2607-Codex做游戏，这10个AgentSkill够你搭一支开发团队.md)
- 2026-08-25 · [Codex桌面应用国内下载难？开源镜像15分钟同步官方包，还带增量更新](https://www.sz003.com/chajian/2559) · [仓库内阅读](docs/2559-Codex桌面应用国内下载难？开源镜像15分钟同步官方包，还带增量更新.md)
- 2026-08-25 · [AGENTS.md是什么？6万开源项目在用的AI编程说明书，Codex官方支持](https://www.sz003.com/jiaocheng/2557) · [仓库内阅读](docs/2557-AGENTS.md是什么？6万开源项目在用的AI编程说明书，Codex官方支持.md)
- 2026-08-25 · [给Codex装一套学术研究技能：9.2k星开源套件，文献综述论文评审全包了](https://www.sz003.com/chajian/2558) · [仓库内阅读](docs/2558-给Codex装一套学术研究技能：9.2k星开源套件，文献综述论文评审全包了.md)
- 2026-08-25 · [Codex CLI怎么安装？四种官方安装方式保姆级教程，Windows用户别再只装npm了](https://www.sz003.com/jiaocheng/2556) · [仓库内阅读](docs/2556-CodexCLI怎么安装？四种官方安装方式保姆级教程，Windows用户别再只装.md)

---

**官方入口：[Codex安装](https://www.sz003.com/anzhuang)**

© [Codex中文站](https://www.sz003.com/) · 本仓库为官网内容镜像，文章版权归原作者所有。
