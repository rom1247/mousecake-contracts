---
paths:
  - src/governance/**
  - src/**/*Governance*.sol
  - src/**/*Timelock*.sol
  - src/**/*Governor*.sol
---

# Governance / Timelock

## 适用范围

提案、投票、执行、时间锁延迟。

## 设计检查清单

- 提案阈值、quorum、投票期、执行 `eta`；快照区块或 checkpoint
- 提案 `targets` / `calldata` / `value` 可模拟、可审查；禁止任意 `delegatecall` 除非明确
- Timelock：`delay`、`gracePeriod`、取消与插队角色
- 重复 `execute`、已取消提案、过期提案 revert
- 投票权：质押衍生、ve 模型、委托；双重投票防护
- 与多签关系：谁可提交、谁可执行

## 权限与资金

- 治理金库支出与参数变更走同一或更严流程
- 紧急模块不得绕过 timelock 修改核心 immutables（除非文档化 break-glass）

## 外部依赖

- 签名见 `security.md`；升级执行见 `upgradeability.md`

## 测试要点

- 未达 quorum、过期的 execute、双重 execute
- timelock 延迟前后状态
