# Codex 远程修服务器 Bug：从定位到验证走一遍

> 原文链接：[https://www.sz003.com/jiaocheng/2668](https://www.sz003.com/jiaocheng/2668)

平时用 Codex CLI，默认都在本地项目里跑：代码在你机器上，它就在这台机器上改。现实里更常见的一种情况是代码根本不在你电脑上。项目部署在云服务器的 Docker 容器里，手上只有 SSH 权限，本地没有源码。线上出了 Bug，又必须马上定位。

Codex 的远程模式就是为这个场景准备的。你在本地敲命令，它在远程服务器上读代码，改代码，跑测试。自己的 Mac 上不需要克隆任何仓库。分工很清楚：本地负责表达需求和审查结果，远程负责代码、依赖和运行环境，AI 在中间执行可以验证的步骤。

这次要修的是一个字段名

演示环境是云服务器上一个叫 codex-demo 的 Docker 容器，里面放着一个小项目 docker-health-demo，用途是生成容器健康检查报告。

项目结构不长。containers.json 是模拟的容器状态数据，container_health.py 是健康检查脚本，也是这次要修的那个，test_container_health.py 是配套的 pytest 测试文件。

直接运行脚本会报错，提示 KeyError，缺的是 status 字段。跑测试也一样挂，输出 FAILED，跟着同一个 KeyError。

原因一看就明白：代码里读了一个叫 status 的字段，数据文件里这个字段实际叫 state。一个名字写错而已。对人来说三十秒的事，真正要看的是 Codex 怎么远程把这个闭环走完。

远程那一侧：先把 app-server 起起来

先 SSH 登录云服务器，进入容器，切到项目目录，再启动 Codex 的 app-server：

codex app-server &#8211;listen ws://0.0.0.0:9001

看到 listening on 那一行，还有 readyz 和 healthz 两个探活地址，说明启动成功了。

这里有个地方要注意，不要加 ws-auth 参数。走 SSH 隧道连接时，安全性由 SSH 本身保证，不需要再叠一层 WebSocket 认证。

本地这一侧：把 9001 端口转出来

在本地新开一个终端窗口，把远程的 9001 端口映射到本地：

ssh -N -L 9001:127.0.0.1:9001 root@你的服务器IP

输入密码之后没有任何输出是正常的，它就是一条安静的隧道，挂在后台别关。

想确认隧道通不通，用 curl 打一下 healthz 探活地址，返回 HTTP 1.1 的 200 OK 就说明没问题。

没登录过的机器要先登录一次

这台机器如果从来没登录过 Codex CLI，需要先完成一次登录。输入 codex auth login，它会在浏览器打开 OpenAI 的登录页面，登录你的 ChatGPT 账号即可，终端显示 Successfully logged in 就算完成。这一步只有第一次需要，之后登录凭证会缓存在本地。

把本地 Codex 接到远程项目

再开一个终端窗口，用下面这条命令接入：

codex &#8211;remote ws://127.0.0.1:9001

顺利的话会看到 Codex 的 TUI 界面。注意目录那一行，显示的是远程容器里的路径，也就是 /workspace/docker-health-demo。模型是 gpt-5.5。到这一步远程连接就通了，你本地的 Codex 已经站在远程服务器的项目目录里。

最后一条提示词，它自己走完修复

在输入框里给它一条英文提示词，意思是让它找出 container_health.py 里的 Bug 并修好，再用 pytest 验证。

它会自己完成一串动作。先把 container_health.py 和 containers.json 读一遍，发现字段名不匹配，代码里用 status，数据里是 state。接着改代码，再跑一次 pytest 验证结果。

修复后的输出是这样：脚本报出 total 为 5，running 为 3，exited 为 2，不健康的容器是 redis-cache 和 old-api。pytest 输出 1 passed。一个远程 Bug 修复的闭环到这里就结束了。

什么情况下值得用

整个流程拆开就是三件事：远程启动 app-server，本地做 SSH 隧道转发端口，本地用 codex &#8211;remote 接入。连通之后体验和本地使用 Codex 没有区别，读文件，改代码，跑测试全在远程执行。

这几类情况都能用上它：线上服务出 Bug 要快速定位，开发环境放在远程 GPU 服务器上，多人协作时想直接在共享服务器上改代码，还有本地机器性能不够或者依赖装不全。

需要提前知道的是，远程模式目前还在 Alpha 阶段，有些地方比较粗糙，比如 WebSocket 连接偶尔会降级成 HTTPS。核心功能已经可用，稳定性还在迭代。

---

原文链接：[https://www.sz003.com/jiaocheng/2668](https://www.sz003.com/jiaocheng/2668)
