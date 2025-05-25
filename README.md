# 1.Subnet参数配置
## 1.0. 文档示例(测试环境)
```bash
$ avalanche blockchain create segchain

OUTPUT
    ✔ Subnet-EVM
    ✔ Proof Of Authority
    ✔ Get address from an existing stored key
    ✔ subnet_segchain_airdrop
    ✓ Validator Manager Contract owner address 0x8Eba...
    ✔ I want to use defaults for a test environment
    ✔ Chain ID: 7708
    Token Symbol: SEG
    ...
    ✓ Successfully created blockchain configuration
```

使用CLI生成预设测试配置文件如下
```text
{
    .avalanche-cli/
    └── subnets/
        └── segchain/
            ├── chain.json
            ├── genesis.json
            └── sidecar.json
}
```

## 1.1. genesis.json 配置

```json
{
  "config": {
    "chainId": 7708,            // 链 ID
    "feeConfig": {              // 费用配置
      "gasLimit": 12000000,     // 区块 gas 限制
      "targetBlockRate": 2,     // 目标出块时间(秒)
      "minBaseFee": 25000000000,//最小基础费用
      "targetGas": 60000000,    //目标 gas 使用量
      "baseFeeChangeDenominator": 36, //基础费用变化分母
      "minBlockGasCost": 0,     //出块最低 Gas 量
      "maxBlockGasCost": 1000000,//出块最高 Gas 量
      "blockGasCostStep": 200000 // 区块 gas 成本步长
    "warpConfig": {             // Warp 配置
      "blockTimestamp": 1748151525,
      "quorumNumerator": 67,    //共识阈值
      "requirePrimaryNetworkSigners": true
      //是否需要主网签名
    }
  },
  "alloc": {                    // 初始账户分配
    // 预分配账户配置
  }
 }
}
```

### 备注

#### config
- **chainId**
  - 自定义值范围: 1-4294967295
  - 注意: 确保不与其他链冲突

#### feeConfig
- **blockGasCostStep**: 区块 gas 成本步长：根据自上一个区块以来经过的时间量确定增加/减少区块 gas 成本的量。这里设置为一个非常大的数字，实际上就要求区块生产速度不能快于目标区块生成速率：2


#### alloc
- 在 `alloc` 中添加预分配账户
- 格式: `"地址": { "balance": "金额" }`

## 1.2. sidecar.json 配置

```json
{
  "Name": "segchain",           // 子网名称
  "VM": "Subnet-EVM",          // 虚拟机类型
  "TokenName": "SEG Token",     // 代币名称
  "TokenSymbol": "SEG",        // 代币符号
  "ValidatorManagement": "Proof Of Authority",  
  // 验证者管理方式
  "ValidatorManagerOwner": "0x8Eba0021c60931EADED540b32bd8bB96D0E9d335",  
  // 验证者管理合约所有者
  "Sovereign": true,           // 是否主权链
  "UseACP99": true            // 是否使用 ACP99
}
```

## 1.3. chain.json 配置
```json
{
  "database-type": "leveldb",    // 数据库类型
  "log-level": "debug",          // 日志级别
  "warp-api-enabled": true,      // 默认启用 Warp API
  "eth-apis": [                  // 启用的以太坊 API
    "eth",
    "eth-filter",
    "net",
    "admin",
    "web3",
    "internal-eth",
    "internal-blockchain",
    "internal-transaction",
    "internal-debug",
    "internal-account",
    "internal-personal",
    "debug",
    "debug-tracer",
    "debug-file-tracer",
    "debug-handler"
  ]
}
```

### 备注
- **日志级别**
  - 可选值: "debug" | "info" | "error"
  - 建议: 测试环境默认使用"debug, "生产环境使用 "info" 或 "error"

- **API 配置**
  - 可添加/删除 `eth-apis` 中的 API
  - 建议: 仅启用必要的 API 以提高安全性

- **Warp API**
  - `warp-api-enabled`: true/false
  - 建议: 如果需要跨链功能则启用


# 2.网络节点配置
