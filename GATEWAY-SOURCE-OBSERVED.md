# Gateway 固定源码运行观察（有限样本通过）

在 Linux amd64 使用固定源码 [OpenClaw `51420782b3810b4aa1230aa9621aec78f7dbbd3c`](https://github.com/openclaw/openclaw/tree/51420782b3810b4aa1230aa9621aec78f7dbbd3c)，Node.js 26.13.0 和 pnpm 12.15.0；依锁文件安装主程序及 `packages/` 工作区，安装脚本未执行。以 `node --import scripts/tsx.mjs src/entry.ts` 直接运行 TypeScript 源码，`--version` 输出 `2026.10.8 (5142078)`；没有构建正式发行包或前端。仅使用一次性 HOME、状态、会话、工作区及虚构 token，Gateway 和 OpenAI 兼容模拟模型监听回环；关闭配置中的 cron、浏览器、mDNS 和渠道。环境不是操作系统或网络沙箱，关闭某项调度配置也不证明所有后台维护均停止。

首次只安装主程序依赖时，Gateway 因缺少工作区 `markdown-core` 的 `micromark` 依赖而未能启动；补充安装工作区包后该错误消失。旧发布包夹具设置 `agents.defaults.heartbeat.every` 在此提交被配置校验拒绝；移除字段后才继续启动，见 `HEARTBEAT-MIGRATION-SOURCE.md`。一次以端口监听作为就绪的健康检查失败；在后续临时探针中改为等待 `/readyz`，该接口先返回 HTTP 503，再返回带 `ready: true` 的 200，随后 Gateway 健康调用通过，`channels` 为空。

在重启前，临时探针完成两轮 `agent --session-key agent:main:recipkg-fixture`：两次结果为 `ok`，回答均来自模拟模型；第二次模型请求含第一轮问题、回答与第二轮问题。`chat.history` 返回同一会话的 user、assistant、user、assistant 顺序。Gateway 收到 SIGTERM 后日志记录正常关闭及 agent 数据库 clean-close receipt。

使用同一临时配置和状态重启后，探针在有界就绪等待内超时，**未读取到重启后的历史**；整项测试因此失败。运行在共享、内存紧张的机器上，直接源码启动及模块加载耗时较长；这不是性能基准，也不能据此判断产品在正常发行构建中的启动或恢复行为。该样本未运行真实模型、渠道、工具、UI、权限或独立重建；`/readyz` 的等待与更长超时属于临时探针，不能把发布包测试的通过结果当作固定源码重启通过。

之后使用标准库验证仓库更新后的同一测试夹具，选择 `current` 配置、`/readyz` 就绪检查、600 秒单次启动上限、240 秒 CLI 上限和 120 秒 Agent 上限；在相同的固定源码与临时数据条件下重新运行，1 项样本在 484.181 秒内通过。测试覆盖健康调用、两轮模拟对话、重启前历史、不强杀的正常关闭、重启后就绪，以及同一 session ID、角色顺序和消息内容恢复。此前等待超时的结果仍然成立；两次结果均仅代表各自运行条件，不能据此推断正式发行包的启动时间或完整兼容性。
