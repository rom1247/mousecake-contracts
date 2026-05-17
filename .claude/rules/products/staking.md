---
paths:
  - src/staking/**
  - src/domain/staking/**
  - src/modules/staking/**
  - src/**/*Staking*.sol
  - src/**/*Stake*.sol
---

# Staking Pool

## 适用范围

单池或单资产质押与奖励分发。多池权重与 `allocPoint` 见 `products/masterchef.md`。

## 目录约定

- `src/domain/staking/`：锁仓事实（时长、金额、解锁时间等）优先放此处，供其他模块只读。
- `src/modules/staking/` 或 `src/staking/`：用户入口（`stake` / `withdraw` / `claim`）。
- `src/domain/tier/` 若存在：积分逻辑见 `products/launchpad.md`；本规则适用于 tier 对锁仓状态的读取，不在 staking 模块重复实现 IDO 额度公式。

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
- 模块间仅通过 `interfaces` / registry 引用，禁止 import 其他 `modules/*` 实现

## 测试要点

- 首质押者、零质押、满池、重复 claim、双花 claim
- 时间推进下奖励累计与 fuzz 边界
