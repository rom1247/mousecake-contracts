---
paths:
  - src/factory/**
  - src/**/*Factory*.sol
  - src/**/*Deployer*.sol
---

# Factory / Deployer

## 适用范围

配对创建、池子部署、CREATE2 确定性地址。

## 设计检查清单

- CREATE vs CREATE2：salt、init code hash、地址预测文档
- 部署后 ownership / admin 移交；禁止遗留 deployer 单点
- 模板与实现分离；克隆最小化与初始化一次
- 注册表：重复部署、弃用标记、索引事件
- 构造/初始化参数不可变项在事件中完整记录
- 若部署可升级实例，遵循 `upgradeability.md`

## 权限与资金

- 仅授权方可创建新实例或升级模板
- 工厂暂停不影响已部署实例正常运行（除非明确）

## 外部依赖

- 新 token/pool 见对应 `products/`；安全见 `security.md`

## 测试要点

- 预测地址与实际部署一致（CREATE2）
- 重复注册、零地址参数 revert
