# Codex Skills 是什么？怎么装、怎么写、装哪些最有用

> 原文链接：[https://www.sz003.com/jiaocheng/2650](https://www.sz003.com/jiaocheng/2650)

Codex 每次都能读代码，改代码，跑命令。但它不知道你写 README 的习惯，也不知道你们团队做代码审查的规矩。同一套要求重复交代，早晚会漏掉几条。

Skill 就是把这类重复要求固定下来的东西。它是一份写给 Codex 看的流程说明，落在文件里，需要的时候叫出来用。装得再多也不会一次性占满上下文，Codex 只在你真的用得上时才把完整内容读进来。

Skill 和 AGENTS.md 不是一回事

概念
解决什么问题
更像什么

AGENTS.md
这个项目里所有任务都要遵守什么规则
项目工作守则

Skill
某一类重复任务应该怎么做
专项操作手册

PR 审查，生成 PPT，整理文档这几类活如果经常重复，就适合沉淀成 Skill。整个项目都要遵守的通用规则放 AGENTS.md，一次性的聊天偏好留在提示词里就够了。

Plugin 是比 Skill 更大一层的安装和分发单元，那部分留给下一篇。

一个 Skill 至少要有哪些东西

Agent Skills 是一个开放格式标准，Codex 支持它。最小的 Skill 就是一个文件夹里放一份 SKILL.md，元数据写在 YAML front matter 里，执行说明用 Markdown 写。

字段
是否必需
作用

name
是
Skill 的稳定名称，习惯用小写字母，数字和连字符

description
是
告诉 agent 这个 Skill 做什么，什么时候该用

name 要和父目录名保持一致，目录叫 pdf-processing，里面的 name 也得是这个。

决定它会不会被用上的是 description。写得好的描述会同时交代能力和触发场景，比如把 PDF 转成带公式和来源的学习笔记。只写帮你处理文档这种，Codex 判断不出什么时候该调用它。

入口写最短，材料放旁边

目录
适合放什么
例子

scripts/
可执行脚本
提取内容，批量格式化，校验

references/
按需读取的长文档
API 规则，写作规范，公司流程

assets/
模板和图片
PPT 模板，表格模板，示例图

只有 SKILL.md 是必需的，其余目录按需添加。不确定要不要写的东西，先别塞进入口文件。写成超长提示词仓库，加载和匹配都会变差。

Codex 怎么找到它，又怎么用它

和其他 agent 一样，Codex 用的是渐进式加载。启动时只把每个 Skill 的名字，描述和路径放进上下文，选中之后才读完整的 SKILL.md。流程里提到脚本时，它再按需去读。

启用方式有两种。显式调用是在提示词里写一个 $ 加技能名，或者用斜杠打开菜单选一个。隐式匹配靠 description，任务描述跟某个 Skill 对得上，Codex 自己就会选它。想让某个 Skill 只在你点名时出现，可以在扩展配置里关掉隐式调用。

放在哪个目录

范围
常见位置
适合场景

项目级
仓库根目录下的 .agents/skills
团队共享，这个仓库所有人用

子目录级
当前或父目录下的 .agents/skills
monorepo 里按模块放局部 Skill

用户级
$HOME/.agents/skills
个人常用，跨项目复用

管理员级
/etc/codex/skills
机器或容器里的默认 Skill

系统级
Codex 内置
官方随 Codex 提供

.agents 偏通用规范，写进仓库可以跨工具复用。.codex 偏 Codex 客户端自己的本地管理和插件缓存。两个位置 Codex 都会读，想让别的 agent 也认，优先选 .agents。

如果团队 Skill 和个人 Skill 撞了同一个 name，别指望它们自动合并。给名字加项目前缀更稳，比如 acme-doc-review。

什么时候值得做成 Skill

值得沉淀的任务有三个共同点：重复出现，步骤稳定，需要专业判断。

文档类里最常见，按固定风格改写文章，生成课程笔记，每次的步骤都一样。代码类的典型是按团队标准做 PR 审查，或者把修 CI 的过程固化下来。生成 PPT 和导出 PDF 属于同一性质的交付活。按固定流程去查 GitHub 或者公司内部系统，也适合做成 Skill。

反过来，一次性的聊天偏好不必写成 Skill，整个项目的通用规则也不该塞进来，那是 AGENTS.md 的位置。

起草一个 Skill

内置的 skill-creator 可以带着你把一套重复流程整理成 Skill。它先问清用途和触发场景，再确认输出格式和限制条件，之后才规划目录结构并生成文件。

它一般会提醒控制篇幅，别写成百科。同时要给 Codex 留出判断空间。验证方式还要可靠，能看出 Skill 有没有按预期工作。

手动写也不难，在 .agents/skills 下建一个目录，里面放一份 SKILL.md。改完之后重开一个 thread 才能确认生效。

最省事的起点还是先用现成的。CLI 里打一个斜杠或者一个 $，就能看到当前有哪些 Skill。也可以在任务里点名，例如让 readme-skill 根据当前项目生成 README。

密钥和 token 不要写进 Skill。脚本如果会联网或者写文件，要在说明里讲清风险和审批边界。团队共享的 Skill 不要写个人路径，也不要带只在某台电脑上存在的脚本。

---

原文链接：[https://www.sz003.com/jiaocheng/2650](https://www.sz003.com/jiaocheng/2650)
