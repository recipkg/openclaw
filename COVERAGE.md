# 覆盖与证据

- [openclaw/openclaw](https://github.com/openclaw/openclaw/tree/51420782b3810b4aa1230aa9621aec78f7dbbd3c)，固定提交 `51420782b3810b4aa1230aa9621aec78f7dbbd3c`；上游 LICENSE 和 README 声明 MIT。
- 已阅读 README、`docs/channels/pairing.md`、`src/pairing/pairing-store-model.ts`、`src/pairing/pairing-challenge.ts`、`docs/tools/skills.md` 及 `docs/gateway/sandboxing/modes-scope-and-backend.md` 的相关部分。来源观察不等于实测或完整安全分析。
- 固定提交 `51420782...` 已以源码模式局部运行：在模拟模型下验证就绪、两轮对话与正常关闭；第一次重启等待就绪超时，后续以更长等待通过同一会话的历史恢复样本，见 `GATEWAY-SOURCE-OBSERVED.md`。尚未独立重建。另以 npm 发布包 `2026.10.8` 在临时目录进行 Gateway 与模拟模型对照；其版本字符串短哈希 `dc2523e` 与发布 tag 对应提交前缀一致，但没有验证发布构建的完整溯源，与固定提交不同。具体流程、模拟条件和安全边界见 `GATEWAY-RELEASE-OBSERVED.md`；不能将其标为固定提交验收。
- 在固定源码直跑的隔离启动中确认旧 `agents.defaults.heartbeat` 字段被拒绝，移除后通过该项校验；Doctor 迁移合同目前只有文档/源码证据，见 `HEARTBEAT-MIGRATION-SOURCE.md`，尚未实际迁移。
- 未连接真实消息服务或真实模型，未验证实际沙箱、渠道权限或完整源码测试。多渠道、工作区/工具授权、网络协议、事件、插件、异常恢复、实际隔离与依赖尚未完整提取。
