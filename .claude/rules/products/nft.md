---
paths:
  - src/nft/**
  - src/**/*NFT*.sol
  - src/**/*Membership*.sol
---

# NFT / Membership

## 适用范围

ERC721 / ERC1155、会员权益、可转让或 soulbound。

## 设计检查清单

- 供应上限、公售/预留/团队 mint 配额；`safeMint` 重入
- 版税 EIP-2981 或自定义：接收方、分润上限
- `tokenURI` / 元数据可变性及信任假设
- Soulbound：禁止/限制 transfer 的实现与 bypass 面
- 会员权益绑定 tokenId：销毁/转让对权益的影响
- 签名 mint：nonce、过期、域分隔、取消列表
- 质押 NFT 或锁定：与 `products/staking.md` 衔接

## 权限与资金

- `setBaseURI`、暂停 mint、提款权限最小化
- 盲盒/随机：链上随机源可信度（VRF 等）需说明

## 外部依赖

- ERC721/1155 回调见 `security.md`

## 测试要点

- 超限 mint、未授权 mint、transfer 限制
- 签名 mint 重放与过期
