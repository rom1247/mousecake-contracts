---
paths:
  - src/**/*.sol
---

# Solidity 编码规则

## 基本规范

- 使用清晰、稳定、可审计的实现，避免炫技式写法
- 保持现有代码风格一致，包括命名、导入顺序、文件组织方式
- 能用 `custom errors` 时，不使用冗长 `require(..., "string")`
- 重要状态变更应配套事件
- 优先显式表达业务意图，不依赖隐式副作用
- 避免 magic numbers；必要时提取为 `constant` 或 `immutable`

## 函数与可见性

- 合约内部优先按 `errors -> events -> types -> constants -> immutables -> state -> modifiers -> constructor/initializer -> external -> public -> internal -> private` 排列
- 函数可见性必须最小化：能 `external` 就不要 `public`，能 `internal` 就不要 `public`
- 只读参数优先使用 `calldata`
- 对外部可调用函数，必须关注访问控制、输入校验、重入风险和事件记录
- 非必要不暴露管理员能力；管理员能力必须清晰、可测试、可追踪

## 状态与存储

- 明确区分 `storage`、`memory`、`calldata`
- 读写存储前先思考是否存在冗余 SLOAD/SSTORE
- 仅在逻辑严格安全且有明确收益时使用 `unchecked`
- 若采用升级架构，遵循 `upgradeability.md`，严禁随意变更存储布局

## 可读性与注释

- 对外部接口、复杂逻辑、权限边界、资金路径补充必要注释
- 注释解释“为什么”，不要重复“代码做了什么”
- 对审计关键路径，优先使用简洁、直接、可推导的代码

## 内联汇编

如确需使用 `assembly`，必须满足：

- 高频路径确有明确收益，且普通 Solidity 难以实现
- 注释说明内存布局、边界条件和安全前提
- 补充针对性的测试
