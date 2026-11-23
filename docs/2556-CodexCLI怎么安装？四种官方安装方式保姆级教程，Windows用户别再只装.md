# Codex CLI怎么安装？四种官方安装方式保姆级教程，Windows用户别再只装npm了

> 原文链接：[https://www.sz003.com/jiaocheng/2556](https://www.sz003.com/jiaocheng/2556)

我同事前几天装 Codex CLI，照着搜来的教程敲了 npm install -g @openai/codex，装完一跑，版本报错，更新失灵，回头问我："是不是我电脑的问题。"电脑无辜，教程有责，全网搜得着的安装指南清一色押在 npm 上，而官方 README 里早铺了条更顺的路，只是少有人翻到那一页。

结论先搁这儿：现在装 Codex CLI，首选官方独立安装器，一条命令的事。npm 不过是四种方式之一，还常常不是最省心的那个。

四条路，各自认领

Mac 和 Linux 用户，终端里敲这条：

curl -fsSL https://chatgpt.com/codex/install.sh | sh

Windows 这边对应 PowerShell：

powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"

惯用包管理器的走另两条：npm 全局安装（npm install -g @openai/codex），或 Mac 上 brew install --cask codex。装完之后随便进个目录敲 codex，交互界面就起来了。

还有个细节值得记：独立安装器默认从 releases.openai.com 取版本元数据和安装包，取不到或校验不过关，会自行退回 GitHub Releases。想强制走 GitHub，设上环境变量 CODEX_INSTALLER_USE_RELEASES_OPENAI_COM=false 就行。下载慢成蜗牛的时候，这个开关真能救命。

装完只算第一步：登录与三种形态

第一次跑 codex，登录方式选 Sign in with ChatGPT。官方建议直接用 ChatGPT 账号走订阅额度，Plus，Pro，Business，Edu，Enterprise 套餐全含 Codex 使用量。API Key 也行，只是配置多几步，新手先绕开。

登进来你会看到 Codex 摆出三张面孔：终端里直接跑的，是 CLI。要桌面体验，一条 codex app 拉起，或者去 Codex App 页面下客户端。不想在本机装东西，还有云端的 Codex Web（chatgpt.com/codex）。编辑器党也有份：VS Code，Cursor，Windsurf 各有对应扩展。

两个坑，踩过才疼

头一个坑：别去 GitHub Releases 手动翻二进制，除非你点名要某个旧版本。一个 Release 塞了十几个平台的压缩包，Mac 用户得在 aarch64 和 x86_64 里挑对架构，解压完还得自己改名。官方安装器一条命令的事，手动能折腾掉半小时。

第二个坑：Windows 用户看仔细。官方对 Win11 的支持走 PowerShell 安装器这条路，系统版本太老，先升级再装，硬来没有好结果。

如今的安装体验比早年友好太多。当年被 npm 那套劝退过的，值得拿官方脚本重装一次。装完再去教程区翻翻进阶配置，基本一回就通。

---

原文链接：[https://www.sz003.com/jiaocheng/2556](https://www.sz003.com/jiaocheng/2556)
