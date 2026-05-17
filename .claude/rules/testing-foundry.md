---
paths:
  - test/**/*
  - script/**/*
  - foundry.toml
---

# 测试、Foundry 与脚本规则

## 测试

只要改动影响逻辑，就默认补充或更新测试。测试优先使用 `forge test` 体系。

测试要求：

- 新增功能必须至少有 1 个正向测试
- 关键分支必须有失败路径或 revert 测试
- 对参数空间较大的逻辑优先补 fuzz 测试
- 对权限、边界值、重复调用、零地址、零数量、上限值进行覆盖
- 修复 bug 时，优先先写能复现问题的测试，再修复实现

测试命名建议：

- `test_FunctionName()`：基础行为
- `test_RevertWhen_Condition()`：失败路径
- `testFuzz_FunctionName(...)`：模糊测试

如果改动涉及以下场景，优先考虑补充更强测试：

- 权限控制
- 资金结算
- 签名验证
- 升级初始化
- 会计累计值
- 状态机切换
- DeFi 集成时的 fork test
- 核心协议的不变量测试

## Foundry 命令

必要时可使用更有针对性的命令：

```bash
forge test --match-test <TEST_NAME>
forge test --match-contract <CONTRACT_NAME>
forge test -vvvv
forge coverage
forge snapshot
```

规则如下：

- 改完合约后，至少运行相关测试或构建检查
- 仅在必要时增加 verbosity，不制造噪音输出
- 如果测试失败，先定位根因，再决定修实现还是修测试
- 不为了让测试通过而弱化断言或删除关键覆盖

## 脚本与部署

- 部署与交互脚本放在 `script/`，沿用 `.s.sol` 命名习惯
- 私钥、RPC URL、API Key 必须来自环境变量，不写入源码
- 使用 `vm.startBroadcast()` / `vm.stopBroadcast()` 时，尽量缩小广播范围
- 部署脚本中记录关键部署顺序、构造参数和依赖关系
- 对生产部署脚本，优先保证可重复执行、可读、可审查
