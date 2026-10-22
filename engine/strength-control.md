# Strength Control Engine

> 本文件定义风格应用强度的计算与分配方式。评分口径、总分带与规则红线以 `rules/photography-rules.md` 为唯一总规则源，本文件不另立口径。

## 1. 基础强度（Base）

按原图状态选择一档作为 Base（0–100 刻度）：

| 原图状态 | Base 区间 |
|---|---|
| 原图优秀，且已接近目标效果 | 10–25 |
| 原图普通，但光线良好 | 25–45 |
| 原图明显需要风格化 | 45–60 |
| 仅限用户明确要求，或原图条件与目标风格高度匹配 | 60+ |

判定要求：

- 相邻档位边界值重叠时（如 25、45），取较保守的一档。
- 未满足 60+ 档的前置条件时，Base 不得超过 60。

## 2. 动态修正

```
Strength = Base × Match × Need × QualityGuard × AuthenticityGuard
```

- Base：取自第 1 节，使用 0–100 刻度。
- Match、Need、QualityGuard、AuthenticityGuard：各自归一化到 0–1。
- 最终 Strength 限制在 0–100。

Match 与风格匹配总分带的对应关系：

- 中匹配（3.2–3.9）须降强度。
- 弱匹配（2.5–3.1）仅在用户明确指定该风格时才进入计算。
- 拒绝（<2.5）的风格不进入强度计算。
- 用户未指定风格且无候选达到 3.2 时，走 `natural_native_professional` 自然原生兜底。

## 3. 局部控制

不得只用一个全局强度覆盖整张图。当画面不同区域的处理需求不一致时，按以下维度拆分强度：

- **Subject emphasis**：主体强调
- **Background retreat**：背景退让
- **Shadow density**：阴影密度
- **Highlight rolloff**：高光过渡
- **Warm/cool separation**：冷暖分离
- **Skin protection**：肤色保护
- **Texture/detail**：质感与细节

## 4. 保护项优先级

Skin protection、Architecture protection、Terrain protection 等保护项的优先级高于风格强度。

执行规则：当风格强度与保护项冲突时，先满足保护项，再在相应区域局部降低风格强度；不得为维持风格强度而突破保护项。