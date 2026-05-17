---
paths:
  - src/proxy/**
  - src/**/*Proxy*.sol
  - src/**/*Upgrade*.sol
  - script/**/*Upgrade*.s.sol
---

# Proxy / 可升级合约规则

产品形态：**Proxy / Upgradeable**。仅在需求明确采用升级架构时适用。

- 先确认是否真的需要 upgradeability；不默认把所有合约设计为可升级
- 若采用代理模式，必须说明所选模式（UUPS / Transparent / Beacon 等）及其信任假设
- 禁止随意变更既有状态变量顺序、类型或语义
- 必须检查初始化保护、重复初始化风险、实现合约锁定和存储兼容性
- 业务升级与紧急热修复须区分流程；关键升级走 timelock（见 `products/governance.md`）
- 如项目包含存储布局校验流程，优先使用 `forge inspect <Contract> storage-layout`
- 如使用 storage gap，需说明用途并保持一致扩展策略

## 测试要点

- 重复初始化、未初始化代理、实现未 `_disableInitializers` 的 revert 测试
- 升级前后存储槽与关键状态不变量（若有）测试
