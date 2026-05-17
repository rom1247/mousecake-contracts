---
paths:
  - src/launchpad/**
  - src/**/*Launchpad*.sol
  - src/**/*IDO*.sol
---

# Launchpad / IDO

## 适用范围

公募、私募、配售、认购、退款与解锁。

## 设计检查清单

- 软硬顶、单人上限、总募集上限；超额认购与比例分配规则
- 白名单：Merkle / 签名 / 链上映射；防重放、过期、域分隔
- 定价币种 fee-on-transfer：用余额差确认实收
- 未达标退款、未售代币去向；募集结束状态机
- Vesting：cliff、线性/分段释放；`claim` CEI
- 抢跑：开启区块、commit-reveal（若采用）可说明
- 募集款与项目方提币权限、时间锁

## 权限与资金

- 暂停不应永久锁死用户已购份额或可退款额
- 管理员提取仅限约定比例与时机

## 外部依赖

- 签名与 ERC20 见 `security.md`；代币见 `products/token.md`；滑点/deadline 见 `defi.md`

## 测试要点

- 未达标全额退款、达标不可再购、重复认购 revert
- Merkle/签名错误 proof、过期签名
- Vesting 时间点与超额 claim revert
