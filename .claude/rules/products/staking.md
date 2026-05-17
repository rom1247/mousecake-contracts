---
paths:
  - src/staking/**
  - src/**/*Staking*.sol
  - src/**/*Stake*.sol
---

# Staking Pool

## 适用范围

单池或单资产质押与奖励分发。多池权重与 `allocPoint` 见 `products/masterchef.md`。

## 设计检查清单

- `stake` / `withdraw` / `claim` 遵循 CEI；奖励计提前更新状态
- `rewardPerToken` 或指数记账：精度、时间基准、首笔质押边界
- 锁定期、冷却期、提前退出罚金；紧急提款路径
- 奖励代币枯竭、暂停充值、仅提现模式
- 质押资产与奖励代币相同时的会计分离
- 批量操作 Gas 上限；避免对 unbounded 用户列表循环结算

## 权限与资金

- 费率、奖励率、奖励源地址变更受权限控制，优先 timelock
- 紧急 `pause` 不导致用户本金不可恢复（除非明确设计）

## 外部依赖

- 奖励/质押 ERC20 见 `security.md`；定价相关见 `defi.md`

## 测试要点

- 首质押者、零质押、满池、重复 claim、双花 claim
- 时间推进下奖励累计与 fuzz 边界
