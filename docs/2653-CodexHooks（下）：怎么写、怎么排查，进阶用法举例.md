# Codex Hooks（下）：怎么写、怎么排查，进阶用法举例

> 原文链接：[https://www.sz003.com/jiaocheng/2653](https://www.sz003.com/jiaocheng/2653)

知道 Hook 能在哪些时机触发之后，剩下的事是把它写出来。一份配置只有三层，真正干活的是 handler 里的命令。什么时候触发由事件决定，匹配哪些场景交给 matcher，这两层看一遍就会，麻烦在第三层。

脚本从标准输入拿到一段 JSON，用退出码和输出告诉 Codex 该不该继续。返回格式写错，或者挂错事件，都会让它静默失效。社区里能找到不少现成可用的 Hook，先读懂一份再改，比从空白开始省事得多。这一篇把写法，进阶用法和排错放在一起，重点是容易踩空的地方。

一份配置长什么样

最外层是事件名，下面挂一个数组。每一项带 matcher，用正则说明什么情况算匹配。命中之后，这一项里的 hooks 再列出要执行的动作。每个动作声明 type 是 command，写上要执行的命令，还可以配一句 statusMessage 显示当前进度。

目前会真正执行的 handler 类型只有 command 一种。prompt 和 agent 会被解析，但不会执行。

matcher 是个正则

matcher 用来过滤这个 Hook 什么时候触发。PreToolUse 和 PostToolUse 匹配的是工具名，可以只匹配 Bash，也可以匹配 apply_patch 或者 MCP 工具名。

不同事件的 matcher 含义不一样。SessionStart 匹配启动来源，可取 startup，resume，clear 和 compact。PreCompact 与 PostCompact 匹配压缩触发方式，Stop 也类似。UserPromptSubmit 不使用 matcher，配了会被忽略。

脚本收到什么，又该返回什么

字段
含义

session_id
当前 Codex 会话 id

transcript_path
会话记录路径，可能为空

cwd
当前工作目录

hook_event_name
当前 Hook 事件名

model
当前模型 slug

permission_mode
当前权限模式，部分事件才提供

影响 Codex 有两条路。正常退出并在标准输出里写一段 JSON，可以补充上下文，拦截工具，或者要求继续一轮。以退出码 2 结束并把原因写进标准错误，表示阻止，具体效果看事件。

会话记录不宜当成稳定接口。官方说它只是方便读取的记录，格式以后可能变。

三种典型返回

在 PreToolUse 里拦下一次工具调用，返回体里写清事件名是 PreToolUse，permissionDecision 设为 deny，理由放进 permissionDecisionReason。旧格式用 decision 等于 block 加 reason 也认。

PermissionRequest 处理权限请求，decision 里写 behavior 是 allow 或者 deny，拒绝时可以带一句 message。同一个事件有多个 Hook 匹配时，只要有一个返回 deny 就会被拒。有任意一个返回 allow，普通审批提示就可以跳过。都没有表态，就走正常审批。

Stop 事件返回 block，不是否定本轮结果，而是让 Codex 再继续一轮，reason 成为新的继续提示词。

PreToolUse 只是护栏，不是安全边界

官方明确说过，它不会拦截所有 shell 调用，也不拦 WebSearch 这类既不属于 shell 也不属于 MCP 的工具。真正高风险的项目，还是要靠沙盒，审批策略和人工 review 兜底。

删除文件这类动作，最好在 AGENTS.md 里就写清必须先征得明确允许。Hook 负责执行，AGENTS.md 负责让 Codex 理解规则。

审查与信任

非托管的 command Hook 在运行之前需要被审查和信任。Codex 按 Hook 当前的定义算一个 hash，改过之后会重新回到待审查状态，信任之前会被跳过。

CLI 里打一个 /hooks 就能看到 Hook 来自哪里，审查新增或者变化过的条目，决定信任还是禁用。已经在 Codex 外面审查过来源的一次性自动化场景，可以用 codex &#8211;dangerously-bypass-hook-trust 跳过持久化信任检查。参数名里的 dangerously 已经说明风险，不适合日常用。

多个 Hook 会一起跑

多个配置层都有匹配的 Hook 时，Codex 会全部运行，高优先级层不会覆盖低优先级层。

同一个事件下的多个 command hook 是并发启动的。所以一个 Hook 没法靠跑得快来阻止另一个匹配项。写的时候记住两点，Hook 之间不要依赖先后顺序，多个 Hook 都可能做决定时按官方规则理解冲突，比如 PermissionRequest 里 deny 优先。

一个能用的例子

Hook 命令默认跟平台绑定。要兼容 Windows，可以给 command handler 加一条 Windows 专用命令。仓库内的 Hook 建议从 Git 根目录解析脚本路径，别用 .codex/hooks 这种相对路径，Codex 有可能从子目录启动。

举个教学用的例子。在 PreToolUse 上挂一个只匹配 Bash 的 Hook，脚本从输入里取出 command 字段，用正则检查有没有删除类操作。命中 rm -rf 或者 Remove-Item 这类写法，就返回 deny，并让 Codex 先向用户确认。

这个脚本只做一件事，教学价值大于实用。正则覆盖不了所有危险命令，真实项目还得靠沙盒和人工确认。

写 Hook 的几条纪律

把大量业务逻辑塞进 Hook

用 Hook 替代测试，CI 和权限审批

在 Hook 里自动执行发布和数据库迁移

运行不可审查的远程脚本

依赖会话记录的私有格式做长期集成

脚本要短，要能被审查。路径用绝对路径或者基于 Git 根目录。配置里不要写 token 和密码，也不要下载远程脚本就直接执行。团队项目的 hooks.json 应该跟着代码一起进 review。

---

原文链接：[https://www.sz003.com/jiaocheng/2653](https://www.sz003.com/jiaocheng/2653)
