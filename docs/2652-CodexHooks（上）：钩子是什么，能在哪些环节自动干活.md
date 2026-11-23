# Codex Hooks（上）：钩子是什么，能在哪些环节自动干活

> 原文链接：[https://www.sz003.com/jiaocheng/2652](https://www.sz003.com/jiaocheng/2652)

Hooks 是 Codex 的生命周期扩展。它允许你在固定时机运行一段外部脚本，比如提示词提交之前，工具调用完之后，或者一次对话即将结束的时候。

它做的是确定性的检查，不替 Codex 推理。脚本怎么写就怎么执行，它的价值在这里，风险也在这里。这是相当进阶的用法，一般建议先从社区里找现成的 Hook 用，或者让 Codex 帮你写一份。

它常被用来做这几件事

把对话或者工具调用的记录送到团队日志系统

检查用户提示词里有没有误贴 API key 或者 token

在会话开始时加载本地说明和项目约定

在工具调用之前拦下高风险命令，比如删除文件，发布和部署

在工具调用之后检查输出，提醒 Codex 继续验证

在对话停止之前要求它再跑一次测试，或者补一份交付说明

这些事情的共同点是判断标准明确。该拦就拦，该记就记。

默认就开着

Codex 默认启用 Hooks。想在本机关掉，在 config.toml 里把 features 表下的 hooks 设为 false 就行。官方推荐的 feature key 是 hooks，旧的 codex_hooks 还能用，但已经标记为弃用。

团队或者企业管理员可以通过 requirements.toml 统一管理这个开关。个人用户一般只需要管好本机和项目里的两份配置。

配置放在哪四个位置

Codex 会在当前激活的配置层旁边找 Hooks，常见的位置有四个。

位置
说明

用户目录下的 .codex/hooks.json
用户级，单独写一份 Hook 配置文件

用户目录下的 .codex/config.toml
用户级，写在 hooks 表里

仓库下的 .codex/hooks.json
项目级，跟着仓库走

仓库下的 .codex/config.toml
项目级，写在 hooks 表里

多处都配上时 Codex 会全部加载。为了不混，同一层里建议只选一种写法，要么用 hooks.json，要么写在 config.toml 里。

项目内的 Hook 只有在这个项目的 .codex 配置层被信任之后才会加载。没信任的项目里，用户级和系统级的 Hook 照常生效。

一次对话里，它们按什么顺序出现

Hook 绑在固定的生命周期节点上，不是随机触发的。会话启动触发 SessionStart。用户提交提示词触发 UserPromptSubmit。Codex 准备调用工具时是 PreToolUse，如果这个工具需要额外权限，接着来一次 PermissionRequest。工具执行完走 PostToolUse。当前这一轮准备收尾的时候是 Stop。

中途如果发生上下文压缩，会触发 PreCompact 和 PostCompact。启动了子代理，则有 SubagentStart 和 SubagentStop。

常用事件各自能干什么

事件
触发时机
常见用途

SessionStart
会话启动，恢复或者压缩后重新开始
加载本地上下文，提示项目约定

UserPromptSubmit
用户提示词即将提交
检查敏感信息，提示补充复现步骤

PreToolUse
Codex 调用工具之前
拦截危险命令

PermissionRequest
准备请求权限时
放行低风险请求，拒绝高风险请求

PostToolUse
工具调用完成之后
检查命令输出，要求继续验证

PreCompact 和 PostCompact
上下文压缩前后
保存摘要，补充压缩后的上下文

SubagentStart 和 SubagentStop
子代理开始或停止
给子代理补充规则

Stop
当前一轮即将结束
要求再跑测试，补总结

不同事件的权限不一样

事件
能不能改变后续行为

PreToolUse
可以拒绝部分工具调用，也可以给 Codex 补充上下文

PermissionRequest
可以放行或者拒绝权限请求

PostToolUse
工具已经跑完，副作用撤不回来，只能改它接下来看到的反馈

UserPromptSubmit
可以阻止提示词提交，或者补充上下文

Stop
可以要求 Codex 继续一轮

SessionStart
主要用来补充会话开始时的上下文

所以想拦危险命令，得挂在 PreToolUse。想审查执行结果，用 PostToolUse。怕它结束得太早，就靠 Stop。挂错事件点，效果会完全不一样。

作用范围分三种

官方把事件按作用范围分了层。围绕一轮用户请求触发的叫 turn scope，比如 PreToolUse，PostToolUse，UserPromptSubmit 和 Stop。围绕会话启动的是 thread scope，SessionStart 属于这一类。围绕子代理起停的是 subagent scope，对应 SubagentStart 和 SubagentStop。

入门先掌握 turn scope 就够了。检查提示词，拦工具调用，看工具输出，这些最常用的都在这层里。结束前补一次验证也属于它。

---

原文链接：[https://www.sz003.com/jiaocheng/2652](https://www.sz003.com/jiaocheng/2652)
