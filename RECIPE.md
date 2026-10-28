# OpenClaw 配方（提取中）

```toml
format = "recipkg/1"

[package]
name = "openclaw"
version = "0.1.2"
kind = "software"
description = "多渠道个人 Agent 与 Gateway 的重建配方草案。"
entry = "BUILD_PROMPT.md"

[dependencies.quality]
git = "https://github.com/recipkg/recipkg.git"
rev = "41e5b55c6612a9a5a10e499b5ace6a1a3fed0f2b"
subdir = "recipes/software-quality"

[dependencies.design]
git = "https://github.com/recipkg/design-directions.git"
rev = "6fdba350d1effa2d50dc0185095bb91ca7a699ee"
```

上游 [openclaw/openclaw](https://github.com/openclaw/openclaw/tree/51420782b3810b4aa1230aa9621aec78f7dbbd3c)，观察提交 `51420782b3810b4aa1230aa9621aec78f7dbbd3c`，上游声明 MIT。此草案尚未完成重建合同或运行验收，详见 `COVERAGE.md`。
