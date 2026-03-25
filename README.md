# K4-think

从问题本身长出正交维度，然后验证拆法对不对的思考框架。

别的框架给你固定的格子（SWOT、MECE）。K4 不预设格子——维度从问题的张力中生长，用数学方法验证正交且完备。

## 快速上手

```
问题来了 → 数不可逆信号
  0 → 不用K4，直觉就行
  1 → 分一下（decompose.md）
  2+ → 分→筛→搜→归，走完整流程
```

## 四步

| 步骤 | 做什么 | 旋转 |
|------|--------|------|
| **分** decompose | 拆成正交维度 + S₂/K3/CE验证 | 看→排→比→问 |
| **筛** validate | 16格填事实打吻合度，找盲点 | 排→看→问→比 |
| **搜** explore | 四信息源（学术/工业/亲历者/平行领域） | 比→问→看→排 |
| **归** converge | 内部一致性 + 压缩结论 + 𝔇 | 问→比→排→看 |

配对：分↔筛（AT对），搜↔归（CG对）。AT对完成才放CG对。大多数问题分完就够。

## 文件

| 文件 | 内容 |
|------|------|
| **meta.md** | 理论基础。K4 是什么、为什么是 4、证明 |
| **taiji.md** | 入口判断 + 𝔇门槛 + 核心规则 |
| **dispatcher.md** | 调度器。选碱基、选模型、终止条件 |
| **decompose.md** | 分的完整步骤和检验 |
| **explore.md** | 搜的四信息源和硬检验 |
| **validate.md** | 筛的16格矩阵和吻合度 |
| **converge.md** | 归的一致性检查和压缩 |
| **k4-agent.xsd** | XML Schema（agent间通信协议） |
| **de/** | 道。先贤结论 → AI设计门控 |

## 给 AI agent 用

```bash
# Claude Code
cp decompose.md explore.md validate.md converge.md your-project/.claude/agents/
cp dispatcher.md your-project/.claude/agents/
# 用 claude --agent dispatcher "问题P" 启动
```

适配 Claude Code、或任何支持自定义 prompt 的 LLM 工具。

## 给人用

1. 读 `taiji.md` 判断要不要用 K4
2. 读 `decompose.md` 开始拆
3. 拆完不确定 → 继续筛→搜→归
4. 想懂为什么 → 读 `meta.md`

## 什么时候不该用

- 问题是线性的（步骤1→2→3）
- 搞砸了能挽回
- 有成熟框架直接能用
- 需要秒级决策

## 来源

永杰（人）设计代数结构和理论证明。未名（AI）实现操作体系并实战验证。

理论锚定：Noether（对称/结构）、Shannon（信息/压缩）、Gödel（完备性极限）。

## License

MIT
