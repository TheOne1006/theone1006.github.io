---
layout: default
title: Lucky Contract Address
description: 合约地址生成器，帮助你在多个区块链网络上部署具有特定地址
page_url: https://github.com/TheOne1006/notion-mind
---

## App At vercel

[lucky-contract-address](https://lucky-contract-address.vercel.app/)

## 功能特点

- **多链支持**: 支持在不同的区块链网络上部署合约
- **幸运地址生成**: 可以生成满足特定模式的合约地址
  - 支持16进制格式匹配 (例如: 88888888...)
  - 支持正则表达式匹配
- **智能合约编译**: 内置Solidity编译器，支持多个版本
- **构造参数配置**: 支持动态配置合约构造函数参数
- **并行计算**: 多进程并行计算，提高地址匹配效率
- **计算数据管理**: 支持随时暂停/恢复进度

## 概率计算

合约地址数量为`2^160`，每个地址有`2^160`种可能。
表现为 40 个 16 进制的字符,
其中连续 8 个 8 相同的字符，概率为：

**近似估计**：

- 每个位置起始的8字符连续相同的概率为 $\frac{1}{16^7}$，共33个位置。
- 近似概率为 $\frac{33}{16^7} \approx 1.229 \times 10^{-7}$（泊松近似）。

### 1. **概率分析**

- 连续 8 个相同字符的概率为：

$$
p = \frac{33}{16^7} \approx 1.229 \times 10^{-7}
$$

---

### 2. **期望尝试次数**

- 在概率为 $p$ 的情况下，获得一次成功所需的期望尝试次数为：

$$
E = \frac{1}{p} = \frac{16^7}{33} \approx 8.138 \times 10^6
$$

---

### 3. **计算时间**

- 计算速度为 100,000 ops（每秒 10,000 次尝试）。
- 所需时间（秒）为：
  $$
  \text{Time} = \frac{E}{\text{ops}} = \frac{8.138 \times 10^6}{100,000} = 81.38 \text{s}
  $$
- 转换为更直观的单位：
  - 分钟：$\frac{81.38}{60} \approx 1.356 \text{min}$

---

## 突破桎梏

纯 js 版本的速度较慢，使用 WebAssembly (WASM) 等其他方案：

| 版本                                   | 每个 worker 的 ops |
| -------------------------------------- | ------------------ |
| js版本（ethereum-cryptography/keccak） | 30,000             |
| WebAssembly 版本                       | 50,000 ~ 80,000    |

## 工作原理

该工具使用CREATE2操作码来确定性地生成合约地址。通过以下公式计算地址:

```solidity
addr = address(uint160(uint(keccak256(abi.encodePacked(
        bytes1(0xff),
        address(this),
        bytes32(salt),
        keccak256(bytecode)
    )))));
```

其中:

- factoryAddress: 部署工厂合约的地址
- salt: 用于生成不同地址的随机数
- bytecode: 合约的字节码

## 使用方法

1. **编写/编译合约**

   - 直接输入Solidity代码进行编译
   - 或直接输入合约字节码

2. **配置幸运数字**

   - 设置期望的地址模式（16进制或正则表达式）
   - 配置构造函数参数（如果需要）
   - 选择工作进程数量
     > 注意: 工作进程数量越多，搜索速度越快，但也会占用更多的系统资源

3. **开始搜索**

   - 点击"Check"按钮开始搜索匹配地址
   - 系统会并行计算直到找到符合条件的地址

4. **部署合约**
   - 选择找到的幸运地址
   - 确认gas费用
   - 执行部署交易

### 推荐正则

正则匹配网站: [regex101](https://regex101.com/)

> tips: 地址会转换成小写toLowerCase

1. `/([\0-9a-f])\1{13}/`: 连续13个相同数字
2. `/^0x[0-9a-f]*(?:.*8){13}[0-9a-f]*$/`: 至少包含13个8（可不连续）
   - 概率为: $P(X \geq 13) = \sum_{k=13}^{40} \binom{40}{k} \left(\frac{1}{16}\right)^k \left(\frac{15}{16}\right)^{40-k}.$
   - $\approx 1.23 \times 10^{-10}$
   - 120,000 ops/s 情况下 1 分钟 1 个
   - example: `0xbde8c0cc98d6008988e8880863813385576858ea`
3. `/^0x(?:[0-35-9a-f]*8){10}[0-35-9a-f]*$/`: 至少包含10个8且不包含4

## 技术栈

- 前端框架: Next.js + React
- 区块链交互: wagmi + viem
- 多语言支持: i18n
- 并行计算: Web Workers + WebAssembly
- 智能合约: Solidity

## 开发

```bash
# 安装依赖
pnpm install

# 启动开发服务器
pnpm dev

# 构建生产版本
pnpm build
```

## 贡献

欢迎提交Issue和Pull Request来帮助改进这个项目。
