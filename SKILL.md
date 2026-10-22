---
name: intelligent-photo-post-processing
description: 内容感知型专业摄影后期 Skill。先分析照片内容、主体、环境、季节、天气、时间、光线、色彩和叙事，再从风格库选择最匹配风格；风格是候选方案，禁止硬套。不适合时执行自然原生专业后期。严格保留原图结构、构图、透视、地貌、建筑、人物、动物、车辆、物体数量及位置。
---
# Intelligent Photo Post-Processing Skill

## 0. System identity

本 Skill 是“智能摄影后期决策系统”，不是固定滤镜库。

核心原则：
- 内容锁定，影像优化：原图是唯一内容事实来源。
- 风格服务于照片内容，技术服务于真实光影。
- 规则与执行流程分离。
- 执行顺序固定：先分析 → 再选择 → 兼容性判断 → 组合 → 真实性审查。
- 没有合适风格时，使用 `natural_native_professional`（自然原生专业后期）兜底，不得为了套风格而强行风格化。

## 1. Single Source of Truth 与模块分工

`rules/photography-rules.md` 是本 Skill 的唯一总规则源，定义不可突破的事实、真实性和后期边界；任何模块与其冲突时，以该文件为准。

| 模块 | 职责 | 不负责 |
|---|---|---|
| `rules/photography-rules.md` | 唯一总规则源：事实保护、真实性、后期边界 | — |
| `styles/` | 风格候选定义 | 最高优先级的事实保护规则 |
| `techniques/` | 可复用摄影后期技术（如 Dodge & Burn） | 风格定义；技术不是风格 |
| `engine/` | 分析、决策与 QC 模块（见第 2 节） | 总规则 |
| `docs/` | 决策、评分和质量控制流程说明；实施路线 | 总规则 |

维护规则：
- 修改总规则：只改 `rules/photography-rules.md`。
- 修改执行流程：改本文件或对应 `docs/`、`engine/` 文件。
- 禁止在其他模块复制或改写总规则。

## 2. System architecture

本 Skill 采用四层架构：`Rules → Analysis → Decision → Execution/QC`

| 层 | 文件 | 职责 |
|---|---|---|
| Rules | `rules/photography-rules.md` | 定义不可突破的事实、真实性和后期边界 |
| Analysis | `engine/photo-analysis.md` + `schemas/photo-analysis.json` | 把照片转成标准化分析画像（Photo Analysis Profile） |
| Decision | `engine/style-matching.md`、`engine/technique-matching.md`、`engine/compatibility.md`、`engine/strength-control.md` | 选择风格、技术、组合和强度 |
| Execution/QC | `engine/quality-control.md` | 按第 3 节处理顺序执行，并审查结果 |

决策链：
`Input → Photo Analysis Profile → Style Candidates → Technique Candidates → Compatibility → Strength → Execution → QC → Correct/Accept`

完整实施路线见 `docs/implementation-roadmap.md`。

## 3. Execution pipeline

收到照片后，按以下顺序严格执行，不得跳步。

1. **INPUT LOCK**
   - 确认原图是唯一内容事实来源。
   - 锁定构图、机位、透视、主体位置、物体数量、空间关系和身份。

2. **CONTENT ANALYSIS**
   - 判定主体类别：风光、雪景、建筑、城市、人文、旅行人像、肖像、野生动物、美食、静物等。
   - 判定主体权重、环境权重、叙事关系。

3. **LIGHT ANALYSIS**
   - 判定主光方向、光质、反差、光位（逆光/侧光/顶光/平光）、阴影方向、环境亮度层级。
   - 判定是否允许调用光影重塑类技术：原图不存在对应光线条件时不允许。

4. **IMAGE CONDITION ANALYSIS**
   - 评估曝光、动态范围、白平衡、色偏、饱和度、局部反差、噪声、细节、原始景深。

5. **STYLE MATCHING**（评分规则见第 4 节）
   - 对 `styles/` 中的候选风格逐一评分；风格不是强制项。
   - 多个风格达到同一匹配带时，选择对原始事实改变最少且最能服务主体叙事的方案。
   - 用户未指定风格且无候选达到 3.2：使用 `natural_native_professional`。

6. **TECHNIQUE MATCHING**（选择规则见第 5 节）
   - 从 `techniques/` 中按需调用 0 个或多个技术。
   - 调用前先判定技术与原始光线、镜头、运动和场景是否兼容。
   - 不允许为了使用技术而制造原图不存在的摄影条件。

7. **COMPOSITION**
   - 仅当用户明确要求裁切、构图调整、去除内容时才改变结构；否则保持原构图。

8. **PROCESSING**
   - 固定顺序：曝光 → 白平衡 → 高光/阴影 → 色彩 → 局部明暗 → 纹理/细节 → 噪点/颗粒 → 轻微镜头校正。
   - 先解决照片问题，再叠加风格。

9. **STRENGTH CONTROL**（强度规则见第 7 节）
   - 每个风格/技术都必须根据原图质量和匹配度单独确定强度。

10. **QC**
    - 检查真实性、身份、地貌、建筑、光线、颜色、纹理和 AI 痕迹。
    - 风格/技术效果与事实保护冲突时：先降低强度；仍冲突则撤销该风格/技术。

## 4. Style selection scoring

每个候选风格按六个维度评分，每维 0–5 分：

| 维度 | 含义 | 权重 |
|---|---|---|
| Subject | 主体匹配 | 0.25 |
| Light | 光线匹配 | 0.20 |
| Color | 色彩匹配 | 0.15 |
| Narrative | 叙事匹配 | 0.15 |
| Texture | 材质/纹理匹配 | 0.10 |
| Preservation | 对原始事实的保护能力 | 0.15 |

总分：
`0.25*Subject + 0.20*Light + 0.15*Color + 0.15*Narrative + 0.10*Texture + 0.15*Preservation`

总分带：
- `>= 4.0`：强匹配，可正常调用。
- `3.2–3.9`：中匹配，降低强度调用。
- `2.5–3.1`：弱匹配，仅在用户明确指定该风格时考虑。
- `< 2.5`：拒绝调用，改用更合适的风格或 `natural_native_professional`。

兜底：用户未指定风格且无候选达到 3.2 时，使用 `natural_native_professional`。

## 5. Technique selection

技术按“必要性 + 兼容性 + 风险”三项判定：

- 必要性高、兼容性高、风险低 → 调用。
- 必要性低，或原图已具备该效果 → 不调用。
- 与原始光线、镜头、运动或场景不兼容 → 不调用。

组合规则：
- 技术可以组合，但不得让两个技术重复完成同一任务。
- 功能重叠时，由针对性更强的技术承担主要任务。例如局部明度塑造（Local Luminance Shaping）与人物光影重塑（Portrait Light Sculpting）高度重叠时，由后者承担主要任务，避免双重压暗/提亮。

## 6. Style × Technique composition

允许的组合示例（风格 + 技术）：
- Retro Film + Portrait Light Sculpting
- Travel Documentary + Dodge & Burn
- Blade Runner 2049 + Slow Shutter Street
- Wabi-sabi + Local Luminance Shaping
- Natural Landscape + Atmospheric Perspective

禁止：
- 强制把所有照片套用同一风格。
- 把技术名称（如 Dodge & Burn）当作风格标签。
- 同时使用多个互相冲突的高强度风格。
- 用风格制造原图不存在的光源、天气、建筑、人物或反射。

## 7. Strength model

风格强度与技术强度均为 0–100。

默认起点：

| 原图条件 | 强度 |
|---|---|
| 原图优秀 | 10–25 |
| 普通但光线良好 | 25–45 |
| 明显需要风格化 | 45–60 |
| 用户明确要求，或原图条件非常匹配 | 60+（仅限此情况） |

调整规则：
- 默认克制；原图越接近目标风格，改动越小。
- 强度随原图质量、风格匹配度（第 4 节总分带）和真实性风险动态调整；强度不是越高越好。

## 8. User intent override

**用户明确指定风格**：先执行兼容性检查。
- 兼容：使用指定风格，并按原图条件调整强度。
- 不兼容：不得机械执行。说明最关键的冲突，并采用低强度版本；低强度版本仍会破坏真实性时，优先保护原图事实。

**用户只上传图片、未说明需求**：自动完成分析、选风格、选技术、调强度和 QC，不要求用户先选模板。

## 9. Failure fallback

任何环节无法证明更强的处理有益时，回退到更自然的方案，不得继续叠加效果；最终兜底为 `natural_native_professional`。

## 10. Output behavior

图像编辑完成后直接输出结果。除非用户要求解释，否则不复述内部决策过程。
