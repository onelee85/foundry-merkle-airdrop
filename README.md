# Foundry Merkle Airdrop

## 项目概述

基于Foundry框架开发的Merkle树空投系统，支持ERC20代币的安全分发。该实现结合Merkle证明和EIP-712签名验证，确保空投过程的安全性和可验证性。

核心功能：
- 基于Merkle树的空投验证
- EIP-712标准的签名验证
- 防重放攻击机制
- 与OpenZeppelin合约库兼容
- ZkSync链支持

## 技术栈
- Solidity 0.8.24
- Foundry开发环境
- OpenZeppelin合约库
- Murky Merkle树工具

## 快速开始

### 前置要求
- [Foundry](https://book.getfoundry.sh/getting-started/installation) 开发环境
- Git
- Node.js (可选，用于辅助脚本)

### 安装

```bash
# 克隆仓库
cd foundry-merkle-airdrop

# 安装子模块
forge install
```

### 编译合约

```bash
forge build
```

### 运行测试

```bash
forge test
# 详细测试输出
forge test -vvv
# 仅运行特定测试
forge test --match-test testUsersCanClaim
```

### 部署合约

```bash
# 本地部署（Anvil）
forge script script/DeployMerkleAirdrop.s.sol --fork-url http://localhost:8545 --broadcast

# 测试网部署（示例：Sepolia）
forge script script/DeployMerkleAirdrop.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast
```

## 合约架构

### 核心合约

#### MerkleAirdrop.sol
<mcfile name="MerkleAirdrop.sol" path="src/MerkleAirdrop.sol"></mcfile>

实现空投分发逻辑，主要功能：
- 验证Merkle证明
- 验证EIP-712签名
- 处理代币分发
- 防止重复领取

关键方法：
<mcsymbol name="claim" filename="MerkleAirdrop.sol" path="src/MerkleAirdrop.sol" startline="32" type="function"></mcsymbol> - 领取空投入口
<mcsymbol name="getMessageHash" filename="MerkleAirdrop.sol" path="src/MerkleAirdrop.sol" startline="52" type="function"></mcsymbol> - 生成EIP-712消息哈希

#### BagelToken.sol
<mcfile name="BagelToken.sol" path="src/BagelToken.sol"></mcfile>

示例ERC20代币合约，用于空投演示。支持铸造和转账功能。

### 部署脚本

<mcfile name="DeployMerkleAirdrop.s.sol" path="script/DeployMerkleAirdrop.s.sol"></mcfile>
自动化部署脚本，负责：
- 部署代币合约
- 部署空投合约
- 配置代币分发

## 使用指南

### 生成Merkle树

```bash
forge script script/MakeMerkle.s.sol --broadcast
```

### 生成空投输入文件

```bash
forge script script/GenerateInput.s.sol --broadcast
```

### 领取空投

```solidity
// 示例代码：用户领取空投
(address user, uint256 userPrivKey) = makeAddrAndKey("user");
(uint8 v, bytes32 r, bytes32 s) = signMessage(userPrivKey, user);
airdrop.claim(user, amountToCollect, proof, v, r, s);
```

## 测试

测试覆盖主要功能场景：
- 验证空投领取流程
- 防止重复领取
- 验证签名机制
- ZkSync链兼容性

查看测试实现：<mcfile name="MerkleAirdrop.t.sol" path="test/MerkleAirdrop.t.sol"></mcfile>

## 安全考虑
- 使用SafeERC20进行安全转账
- 实现重入保护
- 双重验证机制（Merkle证明+签名）
- 遵循Checks-Effects-Interactions模式

## 许可证
本项目采用MIT许可证 - 详见LICENSE文件

## 引用
- [OpenZeppelin Contracts](https://github.com/OpenZeppelin/openzeppelin-contracts)
- [Foundry](https://github.com/foundry-rs/foundry)
- [Murky](https://github.com/dmfxyz/murky)