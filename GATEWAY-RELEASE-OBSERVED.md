# Gateway 局部运行观察：npm 发布包

本页是补充证据，不是固定源码提交 `51420782b3810b4aa1230aa9621aec78f7dbbd3c` 的运行结果。测试使用 npm `openclaw@2026.10.8`，程序版本字符串为 `2026.10.8 (dc2523e)`；上游发布 tag `v2026.10.8` 指向提交 `dc2523e5a5c7a309517fd7fa834add938231d526`。短哈希一致不能单独证明 npm 包的完整构建来源。

在 Linux、Node 26.12.1、一次性目录及独立状态目录下运行该发布包。npm 安装禁用生命周期脚本并省略可选依赖，启动警告 fs-safe 原生支持不可用，技能监听退回轮询；这不代表完整正常安装或其他平台行为。Gateway 仅绑定回环并使用临时 token，关闭消息渠道、心跳、定时任务、浏览器和 mDNS；模型是回环 HTTP fixture，返回固定的 OpenAI 兼容流式文本，不调用真实模型、不要求工具调用，也不连接真实 IM。

## 观察到的流程

1. `gateway call health --json` 返回成功状态，样例中 `channels` 为空。
2. 连续两次 `agent --session-key agent:main:recipkg-fixture --message ... --json` 返回 `status: ok`，可见答复均为 fixture 文本。模型第二次收到的消息包含第一轮用户消息、第一轮答复和第二轮用户消息。
3. `gateway call chat.history --params '{"sessionKey":"agent:main:recipkg-fixture"}' --json` 返回同一会话 ID 和按用户、助手、用户、助手排列的消息。正常停止 Gateway 并使用同一状态目录重启后，历史的会话 ID、顺序和这两轮文本仍存在。

独立验证仓库的 `test_openclaw.py` 可以用 `RECIPKG_TEST_OPENCLAW` 指向待测程序重放这个局部样例。测试使用临时 HOME、配置、工作区、状态与凭据，结束后关闭进程并清理；它不是网络或系统调用沙箱。本次 Gateway 日志出现远程模型目录请求被程序自己的安全检查阻止的记录，不能把关闭渠道和 mDNS当作已阻断所有外网访问。

固定提交的协议文档描述 WebSocket JSON 请求、响应、事件、握手和权限范围，但不同版本的行为需按其自己的协议验证。未测试真实渠道、工具执行、审批、恶意输入、并发、异常崩溃恢复、权限完整性、真实 UI 或独立重建；这一个正常重启样例不能推广为完整恢复保证。
