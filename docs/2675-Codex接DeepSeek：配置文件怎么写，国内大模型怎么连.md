# Codex 接 DeepSeek：配置文件怎么写，国内大模型怎么连

> 原文链接：[https://www.sz003.com/jiaocheng/2675](https://www.sz003.com/jiaocheng/2675)

把 Codex 接到 DeepSeek 这类国内大模型，改的还是同一个 config.toml 文件。但它比接中转站要多一道工序，原因在协议上。最新版的 Codex 只认 Responses 协议，国内大模型多数只提供 Chat 协议，两边对不上。

中间要加一层做协议转换。这一层不装，配置文件写得再对也会报错。整个流程可以拆成四步：拿 API Key，装并启动那一层转换，改配置文件，最后验证。

中间那层协议转换

案例里用的转发中间件叫 codex-relay，依赖 Python 环境。另一个可选项是 Moon Bridge，依赖 Node.js，在 GitHub 上能找到，DeepSeek 的说明仓库里还配了一份中文教程。

选哪个看本机已经有什么环境。Python 现成就用 codex-relay，Node 现成就用 Moon Bridge。

先拿到 DeepSeek 的 API Key

进 DeepSeek 的开放平台注册账号，然后充值，先充 10 块钱就够试。进 API Keys 页面点创建，填一个名称，确认之后 key 就出来了。

key 只在创建那一次能复制，之后回到这个页面也看不到完整的一串。创建完先粘到别处临时存一下。真忘了也不麻烦，把旧 key 删掉重建一个就可以。

拿 key 的同时把 codex-relay 装上。在 PowerShell 里运行 pip install codex-relay。本机要先有 Python 环境，跑完没报错就算装好了。

启动脚本写好再运行

启动 codex-relay 要用一段 PowerShell 脚本。脚本里要设三样东西：上游地址指向 DeepSeek 的接口，API Key 换成你刚建的那一串，端口固定用 4446。脚本开头清掉旧的环境变量，末尾打印三行确认信息，顺便检查 codex-relay 命令找不找得到。

把这段内容粘进一个文本文件，后缀名改成 ps1，右键选 PowerShell 运行。控制台出现正在监听 127.0.0.1:4446 并把请求转发到 DeepSeek 地址的提示，就说明这一层通了。

这个窗口不能关。关了等于关掉转发服务，Codex 那边立刻用不了。想长期用就让它一直开着。

config.toml 里要改的几行

配置文件在用户目录下的 .codex 文件夹里，文件名是 config.toml。装 Codex 的时候它会自动创建。如果找不到，就手动建一个文本文件，再把名称和后缀改成 config.toml。用记事本打开，把下面这些项加到文件最前面。文件原本有内容不影响，只要这段在顶部。

配置项
写什么

model
deepseek-chat

model_provider
deepseek_relay

approval_policy
never

sandbox_mode
danger-full-access

model_providers 里的 name
DeepSeek via Relay

base_url
本机 4446 端口上的地址

wire_api
responses

requires_openai_auth
false

supports_websockets
false

request_max_retries 和 stream_max_retries
0

stream_idle_timeout_ms
300000

关键几行各自管一件事。model 选 deepseek-chat，model_provider 指向下面定义的 deepseek_relay。base_url 写成本机上 4446 端口那个地址，让所有请求先走本地这一层。wire_api 保持 responses，让 Codex 看到的是它认的协议。

还有三处必须关掉。requires_openai_auth 设成 false，鉴权交给本地代理来做。supports_websockets 设成 false，DeepSeek 不支持 WebSocket。features 里的 responses_websockets_v2 也设成 false，版本对不上的功能先别开。

重试次数设成 0，避免同一个问题被回复好几次。超时时间拉长到 300000 毫秒，给 DeepSeek 留出思考的余地。

多智能体那几项也一起开。max_threads 设 4，max_depth 设 1，单任务运行上限 1800 秒。

配置里的 approval_policy 和 sandbox_mode 是示例给的放开权限写法。真要长期用，建议先在测试目录里跑顺再说。

验证和排错

配置写完，打开 Codex 试一轮。能正常回话就说明闭环成了。请求先到本地的 4446 端口，再由代理转给 DeepSeek。base_url 和启动脚本里的监听端口必须是同一个，两边不一致就断在中间。

用不起来的常见原因就那么几个。那个 PowerShell 窗口被关了，转发服务跟着停。config.toml 里还留着示例 Key，没换成自己的，或者用的是已经作废的 key。上游地址和端口写错一位。websockets 那几处没关。报错里出现协议不兼容的提示，基本就是代理这一层没起来。

另一条路是用面板工具切换

不想手工改文件，还有一个办法。CC Switch 是一个第三方开源桌面工具，用来统一管理不同的 Agent 工具。以前要手动改 Claude Code，Codex，Gemini CLI 各自的配置文件，现在它把这些收进一个可视化面板。

用途
说的是什么

Provider 切换
从官方 API 切到中转 API 或者另一个模型服务

MCP 统一管理
不用分别给 Claude Code，Codex，Gemini 配 MCP

Skills 管理
从 GitHub 或压缩包装 Skill，同步到不同 AI 编程工具

用法不复杂。在官网下载装上，先去 DeepSeek 官网建一个 API Key。打开 CC Switch，添加模型，把 key 粘进去。再开启本地路由映射，然后点添加。进设置把路由全部打开，点启用。如果它的路由，模型服务和 Codex 侧配置都兼容，再打开 Codex 就有可能通过这套路由用上 DeepSeek。

这条路不属于 OpenAI 官方功能。能不能正常使用，模型能力，上下文长度和工具调用兼容性都要以 CC Switch 和模型服务商为准，费用和隐私规则也一样。重要项目先用测试仓库验证，别直接在生产项目上试。

两条路都指向同一个结果。区别在配置这条路要自己看懂每一行，面板那条省事但多了一层第三方。

---

原文链接：[https://www.sz003.com/jiaocheng/2675](https://www.sz003.com/jiaocheng/2675)
