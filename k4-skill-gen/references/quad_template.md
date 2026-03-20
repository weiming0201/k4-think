# 四联档调度器模板

> 读此文件：生成四联档的调度器 SKILL.md 时

## 调度器 SKILL.md 结构

```yaml
---
name: {skill-name}
description: "{领域}K4参考手册。碱基功能表、子skill映射。触发由 BOOTSTRAP 控制，路由由子skill自路由。触发词：{领域关键词}。"
---
```

```markdown
# {Skill名} 参考手册

> 理论基础：`skills/k4-think/references/k4_meta.md`
> 触发：BOOTSTRAP §2（数不可逆信号）
> 路由：每个子skill输出末尾的路由指令
> 状态：`memory/k4_state.md`

## 功能表

|  | 看γ | 排π | 比α | 问υ |
|---|---|---|---|---|
| **{A} γ** | [init·1] {操作} | [init·2] {操作} | [init·3] {操作} | [init·4] {操作} |
| **{B} π** | [verify·2] {操作} | [verify·1] {操作} | [verify·4] {操作} | [verify·3] {操作} |
| **{C} α** | [search·3] {操作} | [search·4] {操作} | [search·1] {操作} | [search·2] {操作} |
| **{D} υ** | [converge·4] {操作} | [converge·3] {操作} | [converge·2] {操作} | [converge·1] {操作} |

## 子Skill映射

| 路径 | 文件 | 输入 | 输出 |
|---|---|---|---|
| init | `skills/{name}-init/SKILL.md` | 问题P | 四维度+验证+𝔇+路由指令 |
| search | `skills/{name}-search/SKILL.md` | init全输出 | 搜索结果+可能性+路由指令 |
| verify | `skills/{name}-verify/SKILL.md` | init+search全输出 | 矩阵+缺口+路由指令 |
| converge | `skills/{name}-converge/SKILL.md` | 前序全输出 | 𝔇+𝔎+行动方案+路由指令 |

## 执行协议

1. **入口**：BOOTSTRAP §2 决定是否用及深度
2. **首步**：永远从 init 开始
3. **路由**：每步输出末尾的路由指令决定下一步。agent 直接执行
4. **状态**：每步写 `memory/k4_state.md`，下步从此文件读上下文
5. **约束**：思行分离；轮数≤3

## 自适应深度

不预选模式。init 完看 𝔇 决定继续还是停：

init → 𝔇≥0.8 停 = "快速"
init → search → 停 = "标准"
init → search → verify → converge → 停 = "完整"

模式是轨迹的事后描述，不是预先选择。
```

## 子skill路由指令模板

每个子skill输出末尾必须包含标准路由指令块：

**init 路由指令：**
```
𝔇≥0.8 → 停止
𝔇 0.5-0.8 → 继续：读 skills/{name}-search/SKILL.md
𝔇<0.5 → 回退：重拆或换框架
```

**search 路由指令：**
```
高置信≥3 且覆盖均衡 → 停止
存在关键缺口 → 继续：读 skills/{name}-verify/SKILL.md
维度定义有问题 → 回退：读 skills/{name}-init/SKILL.md
```

**verify 路由指令：**
```
缺口<25% 且致命质疑=0 → 继续：读 skills/{name}-converge/SKILL.md
致命质疑≥1 → 回退：读 skills/{name}-init/SKILL.md
信息不足 → 回退：读 skills/{name}-search/SKILL.md
```

**converge 路由指令：**
```
𝔇≥0.8 且 𝔎≥0.7 → 停止（BOOTSTRAP 2+信号时 𝔇≥0.9）
𝔇<0.5 → 回退：读 skills/{name}-init/SKILL.md
𝔎<0.5 → 回退：读 skills/{name}-search/SKILL.md
轮数≥3 → 强制停止
```

## 生成注意事项

1. **{A}{B}{C}{D} 替换为实际维度名**
2. **{操作} 替换为领域特有的16格操作描述**（≤20字/格）
3. **后缀名可替换**：领域术语优先，在映射表注明对应关系
4. **路由阈值可调**：高风险领域提高门槛
5. **调度器是参考手册不是进程**：不含路由逻辑，路由在子skill里
