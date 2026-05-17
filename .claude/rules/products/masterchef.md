---
paths:
  - src/masterchef/**
  - src/**/*MasterChef*.sol
  - src/**/*Masterchef*.sol
---

# MasterChef

## 适用范围

多池流动性挖矿、`poolId`、`allocPoint`、待领取与复投。

## 设计检查清单

- 新增/调整池与 `allocPoint` 对存量待领取的影响；`massUpdatePools` 顺序
- `deposit` / `withdraw` / `harvest` / `emergencyWithdraw` CEI 与事件
- 存款/取款/ harvest 手续费；LP 代币非标准（fee-on-transfer）处理
- 迁移池、弃用池：存量用户可撤出或 claim
- 复投路径的外部调用与滑点（见 `defi.md`）
- Owner 调参：alloc、费率、实现地址；优先 timelock

## 权限与资金

- 奖励源余额可支撑排放；增发权限边界清晰
- 紧急提取仅影响奖励或明确放弃奖励，不静默吞本金

## 外部依赖

- LP 与奖励代币见 `security.md`；swap/预言机见 `defi.md`
- 单池质押逻辑可参考 `products/staking.md`

## 测试要点

- 多池 alloc 变更前后待领取一致
- 紧急提取、空池、重复 harvest
