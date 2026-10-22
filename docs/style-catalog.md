# Style Catalog

> 本文件仅列出可选风格及其适用条件，不定义评分与选择规则。评分、阈值、兜底与红线一律以 `rules/photography-rules.md`（唯一总规则源）为准；本文件与其冲突时，以总规则源为准。

## 1. 风格清单

| Style | 适用主体 | 光线前提 | 典型技术（techniques） |
|---|---|---|---|
| Canon LS | 自然风光 | 自然日光 | Atmospheric Perspective / Local Luminance |
| High Mountain Snow | 雪山、雪林 | 冷光或冬季自然光 | Atmospheric Perspective |
| Blade Runner 2049 | 城市、工业、夜景 | 方向性光源或城市夜光 | Local Luminance / Slow Shutter Street |
| Wildlife Documentary | 动物 | 自然光 | Dodge & Burn / Local Luminance |
| Travel Portrait Documentary | 旅行人像 | 自然环境光 | Portrait Light Sculpting / Dodge & Burn |
| Portrait Rembrandt Documentary | 近距离肖像 | 明确的侧光或侧逆光 | Portrait Light Sculpting |
| Retro Film Volume Documentary | 风光、旅行、人文 | 真实的方向性自然光 | Atmospheric Perspective / Dodge & Burn |
| Wabi-sabi Deep Gray | 建筑、旧城、室内、静物 | 低调光或可控自然光 | Local Luminance / Dodge & Burn |
| Food Commercial | 美食 | 真实餐桌光或商业光 | Local Luminance / Dodge & Burn |
| Natural Native Professional（`natural_native_professional`） | 全部题材（仅作兜底，见第 2 节） | 无特定要求 | 曝光 / 白平衡 / 动态范围 / 局部明暗 |

判定要点：

- 「适用主体」与「光线前提」用于判断候选资格，两项均不满足的风格不进入评分候选。
- 进入候选后，按六维评分（Subject 0.25 / Light 0.20 / Color 0.15 / Narrative 0.15 / Texture 0.10 / Preservation 0.15）计算总分，并按总分带处理：
  - `>=4.0`：强匹配。
  - `3.2-3.9`：中匹配，降强度执行。
  - `2.5-3.1`：弱匹配，仅在用户明确指定该风格时考虑。
  - `<2.5`：拒绝。

## 2. 兜底规则

用户未指定风格，且没有任何候选风格达到 3.2 时，使用 `natural_native_professional`（自然原生兜底）。

该风格不做风格化处理，仅执行第 1 节表中列出的基础调整。

## 3. Style 与 Technique 的区分

- 第 1 节表中「Style」列为风格（styles），只有这些条目可作为风格选择结果。
- 「典型技术」列为技术（techniques），例如 Dodge & Burn、Slow Shutter Street、Portrait Light Sculpting、Atmospheric Perspective、Local Luminance。技术只能在已选定风格内调用，不得作为风格被选择或输出。
