---
paths:
  - src/bridge/**
  - src/**/*Bridge*.sol
---

# Bridge / Cross-chain

## 适用范围

跨链消息、铸烧、锁定解锁、中继验证。

## 设计检查清单

- 链 ID / domain separator；目标链与源链配置不可混淆
- 消息 nonce、唯一 id、重放防护；已处理映射
- 限额：单笔/日累计；暂停与恢复
- 信任模型：轻客户端、多签中继、oracle；明确 finality 假设
- 铸烧 vs 锁定：供应一致性；失败消息重试或退款
- 升级与密钥轮换流程

## 权限与资金

- 中继/validator 集合变更走 governance 或 timelock
- 紧急暂停时用户资产可证明赎回或消息延迟执行

## 外部依赖

- 签名与 oracle 见 `security.md`、`defi.md`

## 测试要点

- 重复处理同一 messageId revert
- 错误 chainId、过期 nonce、超限额 revert
