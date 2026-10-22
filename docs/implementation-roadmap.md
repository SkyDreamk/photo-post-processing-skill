# AI Intelligent Photo Post-Processing System — Implementation Roadmap

## 1. 总体目标

将当前 Skill 从“规则 + 风格库”升级为闭环智能后期系统，闭环链路为：

分析（Analysis）→ 决策（Decision）→ 执行（Execution）→ 质检（QC）→ 修正（Correction）

## 2. 核心原则

- 不先训练“万能修图模型”。先稳定决策链条，再逐步把需要机器视觉的节点替换为专用模块。
- 每一步决策都必须可追溯，即能说明系统为什么选择某种风格或技术。失败必须能定位到具体环节（分析 / 风格匹配 / 技术匹配 / 兼容性 / 强度 / 执行 / QC）。
- `rules/photography-rules.md` 是唯一总规则源（Single Source of Truth）。本路线图中的任何描述与其冲突时，以总规则源为准。
- styles 与 techniques 严格区分：
  - `styles/` 存放风格候选。
  - `techniques/` 存放可复用摄影技术，如 Dodge & Burn。
  - 技术条目不得作为风格参与风格评分。

## 3. 版本路线

### V1：规则层（已具备）

- `rules/photography-rules.md`：唯一总规则源
- `styles/`：风格候选库
- `techniques/`：可复用摄影技术库
- 基础决策、风格选择、QC 文档

### V2：决策层（当前重点）

本阶段的目标是稳定决策链条，不再无目的地新增风格。

#### Step 1 — Photo Understanding（照片理解）

**输入**：照片。

**输出**：Photo Analysis Profile。以下字段必须全部填写，无法判断的字段标注为“不确定”，并说明原因：

- 主体
- 场景
- 光线
- 色彩
- 景深
- 画质
- 构图
- 叙事
- 风险
- 不确定性

#### Step 2 — Style Matching（风格匹配）

**输入**：Photo Analysis Profile、`styles/` 中的全部风格。

**评分维度与权重**：

| 维度 | 权重 |
|---|---|
| Subject | 0.25 |
| Light | 0.20 |
| Color | 0.15 |
| Narrative | 0.15 |
| Texture | 0.10 |
| Preservation | 0.15 |

**总分带判定**：

| 总分 | 判定 | 处理 |
|---|---|---|
| >= 4.0 | 强匹配 | 可选用 |
| 3.2–3.9 | 中匹配 | 可选用，但须降低强度 |
| 2.5–3.1 | 弱匹配 | 仅在用户明确指定该风格时考虑 |
| < 2.5 | 拒绝 | 不选用 |

**兜底规则**：用户未指定风格，且没有任何候选达到 3.2 时，走 `natural_native_professional`（自然原生兜底）。

**输出**：各风格得分及所选风格（或兜底结果）。

#### Step 3 — Technique Matching（技术匹配）

**输入**：Photo Analysis Profile、已选风格、`techniques/` 中的全部技术。

**选择条件**：某项技术须同时满足以下 4 条，才可入选：

1. **问题存在**：照片中确实存在该技术要解决的问题。
2. **兼容**：与已选风格及其他已选技术兼容。
3. **风险可接受**：引入的风险在可接受范围内。
4. **不重复**：作用不与其他已选技术重复。

**输出**：0–N 项入选技术，每项附入选理由。

#### Step 4 — Compatibility（组合审查）

对 Style × Technique 组合逐一审查。出现以下任一情况，即拒绝该组合：

- 光向冲突
- 颜色冲突
- 重复塑造
- 高真实性风险

**输出**：通过审查的组合；被拒组合须注明拒绝原因。

#### Step 5 — Strength Control（强度控制）

**全局强度**：依据以下 4 个因素动态计算：

- 原图质量
- 匹配度（中匹配须降强度，见 Step 2）
- 处理必要性
- 真实性风险

**局部参数**：

- 主体强调
- 背景退让
- 阴影密度
- 暖冷分离
- 皮肤保护

**输出**：全局强度值与各局部参数值。

### V3：闭环执行

按以下顺序执行：

1. **Analysis**：生成 Photo Analysis Profile。
2. **Decision**：完成 V2 的 Step 2–5。
3. **Execution**：生成 Execution Prompt / Edit Instruction。
4. **Result**：产出处理结果。
5. **QC**：按 QC 文档逐项检查。
6. **修正**：QC 未通过时，自动降低强度，或撤销冲突模块，然后回到第 3 步重新执行。
7. **输出**：QC 通过后输出最终版本。

## 4. 最小可行闭环（MVP）

先实现一个最小闭环，不要求训练模型：

`Photo → Analysis Profile → Style Scores → Technique Scores → Compatibility → Strength → Execution → QC`

- 当前阶段：全流程由大模型依据照片完成。
- 后续阶段：将评分与检测模块逐步工具化。

## 5. 后续扩展

1. **元数据结构化**：将各 style / technique 的元数据结构化，降低纯自然语言匹配带来的不稳定性。
2. **测试照片集**：每类至少准备以下 3 种样本：
   - 正常样本
   - 边界样本
   - 拒绝样本
3. **决策评测集**：每次处理都记录以下内容，用于定位失败环节：
   - 选择了什么
   - 为什么选择
   - QC 哪些项失败
4. **专用视觉模块**：在决策链条稳定后，再引入以下模块替换对应节点：
   - 计算机视觉模型
   - 图像分割
   - 光照估计
   - 深度估计
