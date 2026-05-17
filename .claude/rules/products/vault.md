---
paths:
  - src/vault/**
  - src/**/*Vault*.sol
  - src/**/*Strategy*.sol
---

# Vault / Strategy

## 适用范围

金库份额、底层资产、外部策略委托。

## 设计检查清单

- 份额与资产换算；首存/空库/inflation 攻击（虚拟 shares、最小首存等）
- `deposit` / `withdraw` / `redeem` 上限、暂停、费用
- Strategy 权限：仅可调用白名单、不可任意转走库内无关资产
- `harvest`、亏损计提、绩效费；舍入方向（见 `defi.md`）
- 策略替换、迁移：存量用户可退出或等价迁移
- 紧急退出：暂停存款、允许提款、策略 unwind 假设

## 权限与资金

- 策略添加/移除、费率、cap 变更优先 timelock
- 底层资产与份额代币会计恒等式可测

## 外部依赖

- 定价、滑点、预言机见 `defi.md`；ERC20 见 `security.md`

## 测试要点

- 首存攻击、大额存取、策略 mock 亏损/收益
- 暂停下 deposit revert、withdraw 可用（若设计如此）
