# Style × Technique Compatibility Engine

> 本文件只负责判定风格（style）与技术（technique）、以及技术与技术之间能否组合。总规则以 `rules/photography-rules.md` 为唯一来源；与之冲突时以总规则为准。
>
> 术语约定：Retro Film、Travel Documentary、Blade Runner 2049、Wabi-sabi、Natural Landscape 为 **styles**；Portrait Light Sculpting、Dodge & Burn（D&B）、Slow Shutter Street、Local Luminance Shaping、Atmospheric Perspective 为 **techniques**。techniques 不参与风格六维评分，也不作为风格候选。

## 1. 适用时机

- 在风格已按六维评分选定后、叠加任何 technique 之前执行。
- 本判定不修改风格的六维评分和总分带结论，只决定 technique 能否叠加、以何种条件叠加。

## 2. 兼容等级

| 等级 | 判定条件 | 执行动作 |
|---|---|---|
| Compatible | 各判断维度均无冲突 | 直接组合 |
| Conditional | 存在冲突，但可通过限定作用区域、明确主次或降低强度消除 | 按写明的条件组合；条件无法满足时降为 Weak |
| Weak | 冲突只能部分缓解 | 优先放弃该 technique，只保留风格 |
| Reject | 冲突无法消除，或事实风险升高 | 禁止组合 |

## 3. 判断维度

逐项检查以下维度。组合的最终等级取所有维度中最低的等级。

| 维度 | 检查内容 | 冲突时的处理 |
|---|---|---|
| Subject compatibility | technique 是否适用于画面主体类型 | 不适用时降级 |
| Light compatibility | technique 是否与原片光向、光质一致 | 出现光向冲突时降级或拒绝 |
| Color compatibility | technique 带来的色彩变化是否与风格色彩方向一致 | 出现颜色冲突时降级或拒绝 |
| Narrative compatibility | technique 是否强化风格的叙事，而不是偏离 | 偏离时降级 |
| Operation overlap | technique 与风格或其他 technique 是否在同一区域重复塑造明暗或质感 | 出现重复塑造时明确主次，无法明确时降级或拒绝 |
| Authenticity risk | 组合是否引入原片不存在的元素或改变事实 | 事实风险升高时直接 Reject |

## 4. 典型组合

### 4.1 Style + Technique

| Style | Technique | 等级 | 条件 / 说明 |
|---|---|---|---|
| Retro Film | Portrait Light Sculpting | Conditional / Compatible | 取决于人物光线：原片人物光向清晰、与风格方向一致时为 Compatible，否则为 Conditional |
| Travel Documentary | Dodge & Burn | Compatible | — |
| Blade Runner 2049 | Slow Shutter Street | Compatible | 仅限原片中真实存在的城市运动元素；需要虚构运动元素时按 Authenticity risk 处理为 Reject |
| Wabi-sabi | Local Luminance Shaping | Compatible | — |
| Natural Landscape | Atmospheric Perspective | Compatible | — |

### 4.2 Technique + Technique

| Technique A | Technique B | 等级 | 条件 / 说明 |
|---|---|---|---|
| Portrait Light Sculpting | 强 Dodge & Burn | Conditional | 人物脸部以 Light Sculpting 为主，D&B 仅用于补充材质，不再参与脸部光影塑造 |

## 5. 未列出组合

按第 3 节逐维度判定，并按第 2 节执行。