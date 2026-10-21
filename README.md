# Intelligent Photo Post-Processing Skill

内容感知型专业摄影后期 Skill。

核心思想：**先理解照片，再选择风格。**

本仓库不是滤镜集合，也不是固定流水线。风格库只是候选方案。

## 工作流

`Analyze → Classify → Match → Decide Strength → Dodge & Burn → Grade → QC`

### Analyze
分析主体、环境、天气、季节、时间、光线、色彩、空间和叙事。

### Classify
判断自然风光、城市建筑、城市街拍、旅行人像、动物、美食等类型。

### Match
从 `styles/` 中选择最匹配的风格。

### Decide Strength
根据原图状态决定轻度、中度或强风格。

### Dodge & Burn
仅在有必要时调用局部明度塑光。

### Grade
执行色彩、曝光、对比度、细节、颗粒等调整。

### QC
检查结构、光线、色彩、材质、风格和 AI 痕迹。

## 重要原则

1. 风格不是必选项。
2. 禁止硬套风格。
3. 原图结构优先。
4. 真实光线优先。
5. 风格服务于照片内容。
6. 原图优秀时只做轻度后期。
7. 不适合任何风格时执行自然原生后期。
8. 禁止凭空增加照片中不存在的元素。

## 当前风格库

- Canon LS 自然通透胶片
- 高山雪景冷白空气感
- 银翼杀手2049｜砂金硬光冷青底
- 城市街拍｜高级局部明度塑光
- 野生动物纪实自然光
- 旅行人像｜自然纪实电影感
- 美食商业摄影｜真实食物质感
- 人物特写｜伦勃朗硬光低饱和纪实
- 复古电影｜自然光胶片体积感
- 侘寂深灰｜低曝光暖色克制纪实

后续新增风格统一采用：`适用场景 → 核心视觉 → 执行逻辑 → 禁止事项`。

## GitHub 使用

```text
photo-post-processing-skill/
├── SKILL.md
├── README.md
├── styles/
│   ├── canon_ls.md
│   ├── snow_cold_air.md
│   ├── blade_runner_2049.md
│   ├── city_street_dodge_burn.md
│   ├── wildlife_documentary.md
│   ├── travel_portrait_documentary.md
│   ├── food_commercial.md
│   ├── portrait_rembrandt_documentary.md
│   ├── retro_film_volume_documentary.md
│   └── wabi_sabi_deep_gray.md
└── docs/
    ├── style-selection.md
    └── negative-prompts.md
```

`SKILL.md` 为主入口，其余文件为风格和规则模块。
