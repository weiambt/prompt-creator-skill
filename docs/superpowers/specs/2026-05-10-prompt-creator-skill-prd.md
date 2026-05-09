# Prompt Creator Skill — PRD

> **Generated:** 2026-05-10

## 1. Concept & Vision

一个用于快速生成高质量、结构化提示词（Prompt）的 Claude Code Skill。当用户提出一个提示词创建需求时，Skill 自动分析需求完整性，通过追问补全关键信息，动态选择合适的框架要素组合，最终生成 N 个不同版本的定制化提示词供用户选择使用。

设计理念：**框架是参考，不是枷锁**。不机械套用固定框架，而是提取多个框架的核心要素后动态合成最适合当前场景的定制化架构。

## 2. Core Modes

### 2.1 Normal Mode（默认）

用户仅要求创建提示词，不涉及评测。

**流程：**
```
用户描述需求
    ↓
Skill 分析需求完整性
    ↓ [信息不足]
追问 1-3 个问题（不超过 3 个）
    ↓ [信息完整]
动态框架合成 + 生成 N 个不同版本 prompt
    ↓
直接输出给用户
```

**输出内容：**
- N 个完整的 prompt 正文（可直接复制使用）
- 每个 prompt 对应的框架参考来源说明
- 适用场景简述

### 2.2 Evaluation Mode（未来扩展，MVP 暂不考虑）

当用户要求创建并评测提示词时触发。此模式下会生成多个 prompt 候选 → 评测计划 → 逐轮评测 → 输出最优 M 个 + 评测报告。

> **MVP 里程碑不包含评测功能。**

## 3. Framework Reference（内置 6 种框架）

Skill 内置以下框架作为动态合成的要素来源：

| 框架名 | 核心要素 | 适用场景 |
|--------|----------|----------|
| **RTF** | Role + Task + Format | 通用任务、简单任务 |
| **ICIO** | Instruction + Context + Input + Output | 需要上下文理解、多阶段推理 |
| **CRISPE** | Capacity + Role + Insight + Statement + Personality + Experiment | 学术/科研、内容创作 |
| **CO-STAR** | Context + Objective + Scope + Tone + Audience + Response | 复杂结构化输出任务 |
| **TIDD-EC** | Task + Instructions + Do + Don't + Example + Content | 需要明确边界的任务（教育培训、法律咨询、技术支持）|
| **BROKE** | Background + Role + Objective + Key Result + Evolution | 目标导向、持续改进型任务 |

**动态合成原则：**
- 根据用户需求的场景类型，从上述框架中选取相关要素
- 最终输出的架构是"杂交"产物，不拘泥于单一框架
- 交付时附带说明"本 prompt 参考了哪些框架的哪些要素"

## 4. Configuration

配置文件路径：`~/.prompt-creator/config.json`

```json
{
  "defaultPromptCount": 3,
  "maxQuestions": 3
}
```

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `defaultPromptCount` | number | 3 | 正常模式下默认生成的 prompt 数量 |
| `maxQuestions` | number | 3 | 需求信息不足时，最多追问的问题数 |

## 5. Interaction Flow

### 5.1 需求分析

当用户提出需求后，Skill 自动提取并分析以下关键要素：

| 关键要素 | 说明 |
|----------|------|
| **任务类型** | 翻译、写作、分析、代码生成、问答... |
| **目标受众** | 谁将使用这个 prompt |
| **输出格式** | 期望的输出形式（表格、段落、代码块...）|
| **风格语气** | 正式/轻松/专业/口语化... |
| **场景上下文** | 使用环境的额外背景 |

### 5.2 追问策略

- 如果缺失关键信息，**最多追问 3 个问题**
- 问题逐个发出，不要一次性堆叠
- 优先问**任务类型**和**目标受众**，其次是输出格式

### 5.3 Prompt 生成策略（多样性保障）

生成 N 个 prompt 时，通过以下两个维度保证多样性：

1. **框架维度**：选择最适合当前场景的 2-3 个框架，分别应用其核心要素
2. **自由组合维度**：在框架要素之外，基于需求自由调整措辞、细节程度、表述风格

### 5.4 输出结构

```
=== Prompt 1/3 ===
[prompt 正文]

> 框架参考: RTF (角色+任务) + CO-STAR (语气+受众)
> 适用场景: xxx

===

=== Prompt 2/3 ===
[prompt 正文]

> 框架参考: BROKE (背景+目标+关键结果)
> 适用场景: xxx

===
...
```

## 6. Delivery Metadata（每个 Prompt 的元数据）

| 字段 | 说明 |
|------|------|
| `prompt` | 完整的提示词正文 |
| `frameworkSources` | 参考的框架列表（字符串数组）|
| `applicableScenario` | 适用场景简述 |
| `suggestions` | 可选的使用建议 |

## 7. Constraints & Non-Goals

### MVP 范围内
- 纯生成，不含评测
- 单次对话内完成所有生成
- 支持简体中文交互

### MVP 范围外（未来版本）
- 评测模式（多轮迭代 + 评测报告）
- 多轮对话式 Prompt 优化
- Prompt 性能追踪
- 英文等其他语言支持
- Prompt 持久化存储与管理

## 8. Success Criteria

- [ ] 用户输入一个简单的提示词需求（如"写一个翻译 prompt"），Skill 能正确追问关键问题
- [ ] 用户提供完整信息后，能在单次回复中输出 N 个质量可用的不同版本 prompt
- [ ] 每个 prompt 都清晰标注了框架参考来源
- [ ] 配置文件可读可写，生成数量可自定义
- [ ] Skill 代码结构清晰，易于维护和扩展