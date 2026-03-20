# 四联档调度器模板

> 读此文件：生成四联档的调度器 SKILL.md 时

## 调度器 SKILL.md 结构

```yaml
---
name: {skill-name}
description: "{领域}K4调度器。管理{init/search/verify/converge}四路径。触发词：{领域关键词}。"
---
```

```markdown
# {Skill名} 调度器

> 理论基础：`skills/k4-think/references/k4_meta.md`

## 入口门控

P 是线性的？ → 不用四联，单文件够
P 一步能答？ → 不用四联
P 需要外部搜索+内部判断？ → 用四联

## 四种模式

| 模式 | 路径组合 | 适用 |
|---|---|---|
| 快速 | init | 只需四维度结构 |
| 内省 | init→converge | 纯内生 |
| 标准 | init→search | 需外部信息 |
| 完整 | init→search→verify→converge | 全路径 |

默认：标准

## 功能表

|  | 看γ | 排π | 比α | 问υ |
|---|---|---|---|---|
| **{A} γ** | [init·1] {具体操作} | [init·2] {具体操作} | [init·3] {具体操作} | [init·4] {具体操作} |
| **{B} π** | [verify·2] {具体操作} | [verify·1] {具体操作} | [verify·4] {具体操作} | [verify·3] {具体操作} |
| **{C} α** | [search·3] {具体操作} | [search·4] {具体操作} | [search·1] {具体操作} | [search·2] {具体操作} |
| **{D} υ** | [converge·4] {具体操作} | [converge·3] {具体操作} | [converge·2] {具体操作} | [converge·1] {具体操作} |

## 路由协议

init 完成 →
  ├─ 快速 → 停止
  ├─ 内省 → converge
  ├─ 标准 → search
  └─ 完整 → search

search 完成 →
  ├─ 标准 → 停止
  └─ 完整 → verify

verify 完成 → 反馈决策：
  ├─ 致命发现 → 强制 converge
  ├─ 缺口 < 25% → converge
  ├─ 框架问题 → 回 init
  └─ 信息不足 → 回 search

converge 完成 → 𝔇×𝔎 决策：
  ├─ 𝔇≥0.8, 𝔎≥0.7 → 停止
  ├─ 𝔇<0.5 → 回 init
  ├─ 𝔎<0.5 → 回 search
  └─ 轮数≥3 → 强制停止

## 子Skill映射

| 路径 | 文件 | 输入 | 输出 |
|---|---|---|---|
| init | `skills/{name}-init/SKILL.md` | 问题P | 四维度+验证+𝔇 |
| search | `skills/{name}-search/SKILL.md` | init全输出 | 搜索结果+可能性 |
| verify | `skills/{name}-verify/SKILL.md` | init+search全输出 | 矩阵+缺口+反馈 |
| converge | `skills/{name}-converge/SKILL.md` | 前序全输出 | 判断+𝔇+𝔎+决策 |

## 执行约束

1. 思行分离：四路径完成前不执行外部动作
2. 中间存档：每步写 session_scratch
3. 轮数限制：最多3轮
4. 传递约束：full row passing，不跳步
5. 模式锁定：选定后不切换
```

## 生成注意事项

1. **{A}{B}{C}{D} 替换为实际维度名**
2. **{具体操作} 替换为领域特有的16格操作描述**（≤20字/格）
3. **后缀名可替换**：如果领域有更自然的术语（如 `-scan/-analyze/-audit/-decide`），在映射表注明对应关系
4. **路由阈值可调**：𝔇/𝔎门槛根据领域风险级别调整（高风险领域提高门槛）
5. **模式默认值可调**：信息密集型领域默认"完整"，决策型领域默认"内省"
