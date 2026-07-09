---
aliases: [场景设计, 环境一致性, 场景母版]
tags: [核心步骤, step3]
step: 3
icon: 🏠
sidebar_label: 环境不乱变
parent: "[[../02_角色设计/README|角色设计]]"
---

# 03 · 场景设计 — 让环境"不乱变"

> 第 3 镜的窗户在左边，第 5 镜跑到了右边——观众可能说不清哪里不对，但他们的大脑已经标记了"这不对"。这就是场景不一致的代价。

## 🔍 这是什么？

**场景设计**，就是把你剧本里每个地点"拍一张身份证"—锁定它的标志物、光源、色调、空间结构。之后生成这个场景的任何镜头时，都严格用同一套参数，确保背景不乱变。

这个概念来自影视工业的"场景平面图"和"美术概念设计"：一个场景的墙壁颜色、窗户位置、灯光方向在拍摄期间不会被随意改动。

> **一句话总结**：场景设计 = 选定场景外观 → 固化为"母代码" → 在所有分镜中锚定这个环境。

## 💡 为什么场景是第二大翻车点？

角色不一致观众马上会说"这谁？"，场景不一致虽然观众不一定能说清楚，但**会让他们感到"不舒服"、"出戏"、"有点假"**。这就是潜意识的穿帮。

### 不做场景设计的典型翻车现场

- 第一镜卧室床头柜是白色，第三镜变成棕色 → 弹幕「这个床头柜会变色」
- 第一镜窗户在左边，第五镜窗户跑到了右边 → 空间感混乱，观众不知道自己在哪
- 咖啡馆灯光从暖黄变成冷白 → 氛围断裂，情绪不连贯
- 地板纹理每镜都在变 → 视觉疲劳，AI味最重的地方

> 场景设计的目的 = **锁死背景变量**，让观众的大脑把注意力放在你的角色和故事上，而不是在潜意识里处理"环境为什么一直在变"。

## 🛠️ 怎么做？— 场景母代码法

### 核心概念

**场景母代码** = 一段写死后永远不修改的文字描述，包含这个场景的所有关键信息。之后生成这个场景的任何镜头时，**原封不动**粘贴进去。

### 场景母代码必须包含的四要素

| 要素 | 通俗理解 | 必须写到什么程度 | 示例 |
|------|---------|----------------|------|
| **标志性物体** | 这个场景最显眼的东西 | 具体到颜色+材质+位置 | `Red vintage cash register on the wooden counter` |
| **光源与时间** | 什么时候、什么光 | 时间+光源方向+质感 | `Afternoon sunlight, 45-degree from left window, soft shadows` |
| **色调风格** | 整体配色 | 主色+辅色+风格词 | `Warm amber palette, brick red + cream + dark wood` |
| **空间结构** | 窗户/门/天花板在哪 | 关键建筑元素的方位 | `High ceiling, large window on left wall, door on right` |

### 实操步骤

#### 第 1 步：列出剧本里所有场景

一般一部 1-2 分钟的 AI 漫剧，需要的场景不超过 **2-3 个**：
- 卧室
- 咖啡馆
- 街道
- 教室
- ……

#### 第 2 步：给每个场景写母代码

```
[场景名称] + [标志物] + [光源时间] + [色调] + [空间结构]

示例：
温馨咖啡馆，复古工业风，裸露红砖墙面，吧台上有一台黄铜色复古收银机，
午后阳光从左墙大落地窗45度照入，暖琥珀色调，深棕色木地板，
高天花板，左侧大窗，右侧吧台，深处通往后厨的门。
```

#### 第 3 步：生成场景母版图

1. 打开即梦 AI → 图片生成
2. 输入场景母代码 → 多生成几张 → 挑选最满意的一张
3. 保存为 `场景名_母版.jpg`

#### 第 4 步：在所有分镜中复用

```
每个分镜提示词 = [景别+角色+动作] + [场景母代码（原封不动粘贴）]
```

| 镜头 | 提示词结构 |
|------|-----------|
| 女主喝咖啡（特写） | `Silver hair girl drinking coffee, close up, + [场景母代码]` |
| 男主推门（远景） | `Man entering door, wide shot, + [场景母代码]` |
| 两人对话（中景） | `Two people talking at the counter, medium shot, + [场景母代码]` |

结果：三个镜头的背景**完全一致**——砖墙一样的红色、暖光一样的温度、窗户一直在左边。

---

## 📐 进阶：场景多维控制

### 种子（Seed）控制技巧

> ⚠️ **常见误区：固定 Seed 值 ≠ 锁定一切。** 当提示词为了配合剧情而微调时，固定种子也会产生"特征偏移"。

**正确做法**：
1. 生成满意的第一张场景图后，**记录其 Seed 值**
2. 在生成其他镜头时使用相同 Seed，**只改变角色描述和镜头角度**
3. 如果你用的工具不支持 Seed（如即梦免费版），用**每次复制粘贴相同的场景母代码**来替代

### 多视角基础底图库

对每个核心场景，预生成 3-5 组不同视角的底图：

| 类型 | 用途 | 提示词变化 |
|------|------|-----------|
| **全景**（Long Shot） | 交代空间全貌 | `wide shot, establishing shot` |
| **中景**（Medium Shot） | 角色与环境的交互 | `medium shot` |
| **细节**（Detail Shot） | 锁定材质参考 | `close up, texture detail` |

### ControlNet 深度图锁定（进阶）

如果你用 ComfyUI / Stable Diffusion：
1. 生成场景母版图
2. 用 ControlNet 提取 Depth Map（深度图）
3. 后续生成时，同一场景都加载这个深度图——这样 AI 生成的每一帧共享同一个物理空间结构

---

## 📊 光影情绪对照表

| 想传达的情绪 | 光源描述 | 色调 | 氛围关键词 |
|-------------|---------|------|-----------|
| **温馨 / 治愈** | `warm afternoon sunlight, soft shadows, golden hour` | 暖琥珀、米黄 | cozy, warm, nostalgic |
| **悬疑 / 紧张** | `single overhead light, harsh shadows, low-key` | 暗绿、冷蓝 | eerie, tense, mysterious |
| **浪漫 / 爱情** | `golden hour backlight, soft rim light, candlelight` | 暖橙、粉金 | romantic, dreamy, intimate |
| **孤独 / 悲伤** | `overcast, diffused light, cool blue hour` | 低饱和、冷灰 | lonely, melancholic, quiet |
| **史诗 / 霸气** | `dramatic side light, strong contrast, low angle sun` | 金橙、深蓝 | powerful, majestic, epic |
| **科技 / 未来** | `neon light, cool white, blue-purple ambient` | 冷蓝、霓虹紫 | futuristic, sleek, cyberpunk |
| **恐怖 / 惊悚** | `flashlight beam, pitch black shadows, single source` | 暗红、墨绿 | terrifying, claustrophobic |
| **日常 / 生活** | `natural daylight, even soft shadows, white balance` | 自然色、清爽 | ordinary, peaceful, real |

## 🌐 三大光源铁律

1. **主光永远有方向** — 禁用平光（平光仅用于资产设定集）。写清楚光从哪边来。
2. **角色边缘必须有 1-2 像素的轮廓光** — 逆光勾边，把人从背景中"拉"出来。
3. **环境光与场景色调一致** — 冷蓝场景补冷蓝光，暖黄场景补暖黄光。

---

## 🐛 做坏了怎么办？

| 问题 | 症状 | 诊断 | 解决方案 |
|------|------|------|---------|
| **背景穿帮** | 床头柜颜色变了、墙壁纹理变了 | 每次生成没用母代码 | 写场景母代码 → 每次复制粘贴，不手打 |
| **窗户位置乱跑** | 第一镜左窗，第三镜右窗 | 空间结构没写死 | 在母代码明确写 `large window on left wall` |
| **光影不统一** | 同一场景忽冷忽暖 | 光源时间描述不一致 | 锁定 `Afternoon sunlight, 45-degree from left` 不变 |
| **色调跳跃** | 一个场景三个颜色 | 色彩缺少锚定 | 末尾固定色彩词 `warm amber tone` |
| **角色像P上去的** | 角色光影和环境不合 | 角色和场景分开生成 | 用图生图+场景底图作为参考，让AI自动融合光影 |
| **空间感崩塌** | 天花板一会儿高一米低 | 缺空间尺寸描述 | 加 `high ceiling, standard 2.8m room height` |

---

## 🌍 六大世界观场景完整提示词库

> 每种世界观有独特的**核心关键词**和**通用模板**，直接套用即可产出高质量场景图。

### 🏯 古风仙侠

**核心关键词**：`国漫古风、敦煌壁画风、飞檐翘角、亭台楼阁、竹林云雾、灵石、宗门道袍、江南烟雨、雪山之巅、悬浮宫殿、龙纹纹饰、水墨晕染`

| 场景 | 提示词 |
|------|--------|
| 仙侠洞府 | 国漫仙侠风，昏暗的洞府石壁，深夜，零星灵石发出微光，缠绕绷带的石床，周围漂浮紊乱的灵气光点，电影级侧逆光，浅景深，氛围凝重，高细节 |
| 江南烟雨 | 水墨风国漫，江南古街，清晨，细雨绵绵，青石板路湿润反光，两侧白墙黛瓦阁楼，挂着油纸伞的商铺，雾气缭绕，低角度平拍，柔和光影，温馨静谧 |
| 云间仙宫 | 敦煌壁画风，悬浮在云端的宫殿，正午，晴空万里，周围环绕龙凤祥云，宫殿梁柱雕刻龙纹，金色琉璃瓦，阳光穿透云层形成光柱，广角仰拍，宏伟庄严 |

### 🤖 赛博朋克/科幻

**核心关键词**：`霓虹灯光、全息投影、悬浮车、高楼霓虹、雨夜湿滑街道、蒸汽管道、齿轮机械、赛博格改造、蓝紫色调、高对比度`

**完整场景Prompt**：日式赛博朋克风格，垂直都市街景，霓虹汉字招牌+蒸汽管道+义体商人摊位+穿梭浮空车。夸张广角鱼眼透视强化拥挤纵深。高饱和蓝紫洋红霓虹主导，环境光反射在潮湿地面，疏离喧嚣不夜城，8K电影级渲染

### 🧝 奇幻/魔幻

**核心关键词**：`魔法森林、精灵城堡、巨龙、魔法光晕、发光藤蔓、星空草原、古堡、火焰魔法、水晶矿洞、独角兽、彩虹瀑布`

| 场景 | 提示词 |
|------|--------|
| 精灵森林 | 奇幻风，发光精灵森林，夜晚，满月，荧光藤蔓缠绕古树，蘑菇发出蓝绿色微光，精灵飞舞轨迹光，林间雾气，低角度仰拍，柔和月光，梦幻氛围，动态粒子效果，高细节 |
| 水晶矿洞 | 魔幻风，地下水晶矿洞，无自然光，彩色水晶发出璀璨光芒，地面散落水晶碎片，矿道两侧发光矿石，侧逆光勾勒水晶轮廓，浅景深，神秘梦幻 |

### ⚙️ 蒸汽朋克

**核心关键词**：`齿轮、铜管道、飞艇、钟楼、黄铜色、蒸汽烟雾、机械装置、复古钟表、Victorian风格建筑、机械臂、煤烟、铁轨`

**完整场景Prompt**：蒸汽朋克风，Victorian风格钟楼城市，清晨薄雾，空中飞行蒸汽飞艇，街道齿轮驱动马车，建筑布满铜管道和齿轮，蒸汽从管道喷出，低角度仰拍，黄铜色调，电影级光影，高细节机械纹理，动态蒸汽效果

### 💀 废土/末日

**核心关键词**：`后末日、废墟美学、科技废墟、橙黄色调、荒芜、破碎玻璃摩天楼、废弃车辆、生锈金属、沙尘暴`

**完整场景Prompt**：`A post-apocalyptic sci-fi city, broken glass skyscraper, abandoned cars, neon billboards flickering, overcast sky with patches of blue, orange-brown dust storm, rusted metal structures, hyper-detailed, cinematic lighting, 8k --ar 21:9 --style raw`

### 🏫 现代日常/校园

**核心关键词**：`二次元卡通、校园操场、教室、便利店、雨夜街头、天台、咖啡厅、樱花树、晚自习、霓虹招牌、车流光影、温馨日常`

**完整场景Prompt**：日系二次元风格，高中教学楼天台，黄昏金色时刻，远处城市天际线，锈迹铁网围栏，地面有涂鸦痕迹，天空橙红渐变紫色，柔光逆光，青春感氛围，新海诚风格光影，高细节

---

## 💡 灯光公式大全（中英文对照）

> 灯光是场景的**灵魂**。同一场景不同灯光 = 完全不同的情绪。

### 🎯 方向性灯光 (Directional Lighting)

| 英文 Prompt | 中文 | 适用场景/情绪 |
|------------|------|-------------|
| **Front light** | 正面光 | 纯净柔和、无阴影肖像 |
| **Side light / Split lighting** | 侧面光/分光照明 | 突出立体感、戏剧性对比 |
| **Backlight / Rim light** | 逆光/轮廓光 | 剪影、边缘发光、神秘感 |
| **Top light** | 顶光 | 宗教感、悬疑、压抑 |
| **Bottom light** | 底光 | 恐怖、诡异、奇幻 |
| **Half-rear lighting** | 半背光 | 柔和层次、人像过渡 |

### 🎨 质感灯光 (Quality of Light)

| 英文 Prompt | 中文 | 效果特点 |
|------------|------|---------|
| **Hard light** | 硬光 | 阴影锐利、高对比、纹理突出 |
| **Soft light / Diffused light** | 柔光/漫射光 | 阴影柔和、过渡平滑、温馨 |
| **Harsh midday sun** | 正午刺眼阳光 | 强阴影、明亮、活力感 |
| **Overcast light** | 阴天光 | 低饱和、无影、忧郁宁静 |
| **Studio light** | 摄影棚光 | 均匀、专业人像质感 |

### 🌅 自然光与时段的魔法

| 英文 Prompt | 中文 | 氛围特点 |
|------------|------|---------|
| **Golden hour light** | 黄金时刻光 | 温暖橙金色、浪漫希望 |
| **Sunset glow** | 日落余晖 | 暖调、治愈、孤独感 |
| **Morning light** | 晨光 | 清新、柔亮、新始感 |
| **Crepuscular rays / God rays** | 丁达尔光/耶稣光 | 神圣感、梦幻、大面积光束 |
| **Blue hour** | 蓝调时刻 | 冷蓝天空、都市夜景过渡 |
| **Moonlight** | 月光 | 冷淡蓝色、夜境静谧 |
| **Dappled light** | 斑驳光（林间光斑） | 透过树叶、自然治愈 |
| **Twilight** | 暮光 | 日落后蓝紫调、安静神秘 |

### 🏮 人工照明 & 特效光

| 英文 Prompt | 中文 | 氛围特点 |
|------------|------|---------|
| **Volumetric lighting** | 体积光/层次光 | 可见光束、电影感 |
| **Cinematic lighting** | 电影灯光 | 戏剧性、强对比 |
| **Neon lights / Neon glow** | 霓虹灯/霓虹辉光 | 赛博朋克、都市雨夜 |
| **Rembrandt light** | 伦勃朗光 | 单侧45°光、三角高光、古典 |
| **Candlelight** | 烛光 | 温暖、亲密、怀旧 |
| **Mood lighting** | 情绪照明 | 暗调、情感浓烈 |
| **Warm indoor lighting** | 暖色室内光 | 台灯、壁炉、咖啡馆 |
| **Fluorescent lighting** | 荧光灯 | 冷白、医院感、科技疏离 |
| **Spotlight** | 聚光灯 | 舞台感、焦点突出 |

### 🎬 氛围营造万能组合公式

| 目标氛围 | 推荐灯光组合 |
|---------|-------------|
| **温馨治愈** | `Soft light + Golden hour + Warm indoor lighting + Soft shadows` |
| **神秘悬疑** | `Top light + Low-key lighting + Dark moody lighting + Strong shadows` |
| **赛博科幻** | `Neon lights + Cyberpunk light + Volumetric lighting + Rain reflections` |
| **古典油画** | `Rembrandt light + Candlelight + Chiaroscuro + Warm tones` |
| **恐怖诡异** | `Bottom light + Dim light + Flickering light + Cold blue tones` |
| **浪漫梦幻** | `Fairy light + Twilight + Bioluminescent + Soft bokeh` |
| **史诗宏大** | `Crepuscular rays + Backlight + Golden hour + Atmospheric lighting` |
| **极简冷淡** | `Cool fluorescent lighting + Overcast light + Minimal shadows` |

---

## 🎥 构图与镜头语言速查

> 相机角度和构图决定画面的**叙事张力**。AI默认会把主体放画面正中——所以你必须明确指定。

### 📷 景别类型 (Shot Types)

| 景别 | 英文提示词 | 适合什么 |
|------|-----------|---------|
| **大远景** | `extreme wide shot, establishing shot` | 开头/结尾交代大环境 |
| **全景** | `full shot, wide shot` | 角色全身+环境 |
| **中景** | `medium shot` | 对话/互动（最常用） |
| **近景** | `medium close-up` | 情绪表达 |
| **特写** | `close-up shot` | 面部情绪/关键道具 |
| **大特写** | `extreme close-up` | 眼睛/手指/细节 |
| **过肩镜头** | `over-the-shoulder shot` | 对话、分享视角 |
| **主观视角** | `POV shot, first-person view` | 沉浸感、代入感 |

### 🎬 相机角度与情绪

| 角度 | 英文提示词 | 效果 |
|------|-----------|------|
| **平视** | `eye-level shot` | 连接感、对等、自然 |
| **仰拍** | `low angle shot` | 权力感、英雄气概、压迫 |
| **俯拍** | `high angle shot` | 脆弱、渺小、孤独 |
| **鸟瞰** | `bird's eye view, aerial view` | 全景、疏离、上帝视角 |
| **虫视** | `worm's-eye view` | 敬畏、宏伟、超现实 |
| **荷兰角** | `Dutch angle, tilted frame` | 紧张、不安、心理失衡 |

### 🔭 镜头与焦段效果

| 镜头 | 英文提示词 | 效果 |
|------|-----------|------|
| **广角** | `wide-angle lens, 24mm` | 空间感、夸张纵深 |
| **长焦** | `telephoto lens, 200mm` | 压缩感、背景虚化 |
| **鱼眼** | `fisheye lens` | 超现实扭曲、赛博朋克 |
| **浅景深** | `shallow depth of field, f/1.8` | 主体突出、背景虚化 |
| **深景深** | `deep depth of field, f/16` | 全景清晰、大场景 |

> ⚠️ **构图铁律**：① 相机词放在Prompt**开头**（AI对前面词的权重更高）② 不要混用冲突角度（如同时写bird's eye + eye-level）③ 每次只选**一个明确角度**，勿堆砌

---

## 🌦️ 天气效果与氛围营造

> 天气不只是背景——它是**情绪的放大器**。雨天=孤独，雪天=宁静，雾天=神秘。

**天气万能公式**：`[场景主体] + [天气现象] + [强度等级] + [细节表现] + [氛围基调]`

| 天气 | 英文提示词 | Prompt示例 |
|------|-----------|-----------|
| **晴天** | `clear sky, bright sunlight, sun-kissed` | 晴朗蓝天+葱郁草地+金灿灿阳光线条环绕+温暖治愈 |
| **雨天** | `rain, wet pavement, rain reflections, puddles` | 濛濛雨幕+湿润街道+透明雨滴线条+地面水花溅起 |
| **雪天** | `snowfall, snow-covered, ice crystals, frost` | 漫天飘雪+屋顶厚雪+树枝挂冰棱+暖黄路灯映雪 |
| **雾天** | `fog, mist, haze, low visibility` | 浓雾笼罩+雾气晕染效果+能见度低+神秘氛围 |
| **雷阵雨** | `thunderstorm, lightning strike, dark clouds` | 乌云密布+闪电划过天际+雷光闪烁+暴雨倾盆 |
| **暴风雪** | `blizzard, howling wind, whiteout, snowdrift` | 狂风暴雪+白茫茫+积雪堆+极寒氛围 |
| **沙尘暴** | `sandstorm, dust storm, orange haze` | 漫天黄沙+低能见度+橙黄色调+废土末日后 |
| **樱花雨** | `cherry blossom petals falling, spring breeze` | 樱花飘落+春日微风+粉色花瓣+治愈浪漫 |

---

## 🔧 道具与材质提示词公式

**道具设计核心公式**：`目标物体 + 核心材质 + 质感基调 + 纹理细节 + 色彩`

| 材质类型 | 英文提示词 | Prompt示例 |
|---------|-----------|-----------|
| **皮革** | `leather texture, grain, stitching` | 深棕色头层牛皮，略带磨损粗糙质感，十字车缝线 |
| **金属** | `metal, brushed, polished, reflective` | 银色金属，光滑反光质感，金属拉丝纹理，轻微做旧 |
| **木质** | `wood grain, natural texture, hand-polished` | 原木材质，温暖色调，木质年轮纹理清晰 |
| **布料** | `fabric texture, cotton linen, soft folds` | 棉麻材质，柔软蓬松质感，织物纹理，自然褶皱 |
| **玻璃** | `glass, transparent, refraction, edge reflection` | 透明玻璃，光滑细腻质感，轻微反光，边缘折射光 |
| **宝石** | `gemstone, crystalline, faceted, sparkling` | 红宝石，晶莹剔透，切割面反射光芒，内部火焰纹 |
| **陶瓷** | `ceramic, glazed surface, crackle glaze` | 青瓷，釉面光滑，冰裂纹纹理，温润如玉 |

---

## 🚫 反向提示词体系 — 避免穿帮

> 反向提示词（Negative Prompt）告诉AI**"不要画什么"**，是避免畸形、穿帮、画风污染的关键防线。

### 通用万能反向提示词

**SD/MJ/即梦通用**：
```
(worst quality:1.4), (low quality:1.4), lowres, blurry, bad anatomy, bad hands, mutated hands, extra limbs, extra fingers, (poorly drawn face:1.1), (mutation:1.2), (deformed:1.2), jpeg artifacts, signature, watermark, username, text, error
```

**二次元漫画专用**（额外禁止写实污染）：
```
low quality, worst quality, blurry, bad anatomy, bad hands, missing fingers, extra digits, fewer digits, (extra limbs:1.3), (malformed limbs:1.3), (disfigured, deformed, mutated:1.2), realistic, photorealistic, 3d render, sketch, oil painting, watermark, text, signature
```

**古风/新中式专用**：
```
low res, blurry, pixelated, 3d render, cartoon, modern text, watermark, deformed face, bad anatomy, messy hair, extra ornaments, clutter background, bright neon colors, modern buildings, Western architecture
```

### AI漫画最常见崩坏类型速查

| 类型 | 典型表现 | 针对性反向词 |
|------|---------|-------------|
| **肢体异常** | 多手指、手指粘连、畸形脚 | `extra fingers, bad hands, mutated hands` |
| **关节扭曲** | 手臂反向弯曲、手腕角度不自然 | `bad anatomy, deformed, malformed limbs` |
| **面部失调** | 眼睛不对称、嘴巴歪斜 | `poorly drawn face, asymmetrical eyes` |
| **画风污染** | 二次元中出现写实皮肤纹理 | `realistic, photorealistic, 3d render` |
| **多余元素** | 水印、文字、签名 | `watermark, text, signature, username` |
| **背景杂讯** | 现代建筑出现在古风场景 | `modern buildings, Western architecture` |

> ⚠️ **反向词三大原则**：① 不堆砌重复词（blurry+fuzzy选一个）② 按类型分组用括号加权 `(bad hands:1.4)` ③ 漫画风必须加 `realistic, photorealistic` 防止写实污染

---

## 🔄 转场与场景衔接技巧

> 转场是AI漫剧最容易被忽略的环节。好的转场让观众**感觉不到场景切换**，差的转场让人**出戏**。

### 三种基础转场方式

| 转场类型 | 视觉呈现 | AI实现方式 | 适合场景 |
|---------|---------|-----------|---------|
| **动作匹配** | 角色挥手→下一镜同方向挥手 | 两镜头用相同动作描述词 | 动作衔接 |
| **形似转场** | 圆形钟表→圆形月亮 | 图像处理叠加渐变 | 诗意/文艺 |
| **光影转场** | 暗幕→亮幕（或反向） | 前镜末尾加fade to black | 时间推移 |

**场景锚点转场法（防穿帮核心）**：同一场景所有镜头的Prompt末尾固定粘贴场景母代码，这是最简单有效的防穿帮方法。

> ✅ **转场规则**：一部1-2分钟AI漫剧建议只用2-3个场景，场景间切换时用黑屏0.5秒+音效过渡。

---

## 🧰 场景设计工具链与资源库

### 🎨 AI生图工具对比

| 工具 | 费用 | 场景能力 | 特色 |
|------|------|---------|------|
| **Midjourney V7** | $10-60/月 | ⭐⭐⭐⭐⭐ | 审美天花板，--sref风格锁定，--cref角色参考 |
| **SD + ComfyUI** | 免费(需显卡) | ⭐⭐⭐⭐⭐ | ControlNet精确控制构图，IP-Adapter锁定一致性 |
| **即梦AI** | 免费/月68元 | ⭐⭐⭐⭐ | 中文语义强，东方美学/二次元/国风擅长 |
| **DALL·E 3** | ChatGPT Plus | ⭐⭐⭐⭐ | 文字理解最强，精确遵循复杂指令 |
| **堆友 d.design** | 免费 | ⭐⭐⭐ | 阿里出品，日漫/场景插画/赛博朋克等多风格 |
| **Leonardo.AI** | 免费/付费 | ⭐⭐⭐⭐ | 游戏/奇幻场景，支持自定义模型训练 |

### 🔧 ComfyUI 场景一致性工作流（进阶）

| 工具 | 作用 | 关键参数 |
|------|------|---------|
| **IP-Adapter** | 用参考图锁定场景风格/角色外貌 | weight 0.5–0.8，FaceID模式 |
| **ControlNet Canny** | 锁定线稿结构，场景布局不变 | Control Weight 0.7–1.2 |
| **ControlNet Depth** | 锁定空间透视和深度关系 | 配合Midas预处理器 |
| **ControlNet OpenPose** | 锁定角色姿势骨架 | DWpose预处理器 |

推荐大模型：Anything V5 / Counterfeit V3 / MeinaMix · VAE: kl-f8-anime2.ckpt · Sampler: DPM++ 2M Karras, Steps 25-30, CFG 7-8

### 📚 在线资源与参考网站

| 资源 | 网址 | 用途 |
|------|------|------|
| **Civitai (C站)** | [civitai.com](https://civitai.com) | 海量动漫场景LoRA/Checkpoint模型下载 |
| **堆友 d.design** | [d.design](https://d.design) | 阿里出品免费AI场景生成 |
| **LiblibAI** | [liblib.art](https://www.liblib.art) | 国内最大SD模型站，海量场景LoRA |
| **PromptBase** | [promptbase.com](https://promptbase.com) | 付费/免费提示词市场 |
| **Public Prompts** | [toolai.io](https://www.toolai.io/zh/ai/public-prompts) | 免费社区驱动提示词与模型资源 |
| **film-grab.com** | [film-grab.com](https://film-grab.com) | 按电影查看色调光影参考 |

### 💻 GitHub 开源项目

| 项目 | 链接 | 亮点 |
|------|------|------|
| **AI-Prompts-for-Comic-Book-Art** | [monju252](https://github.com/monju252/AI-Prompts-for-Comic-Book-Art) | 10套完整漫画提示词模板 |
| **comic-cli** | [augmentedmike](https://github.com/augmentedmike/comic-cli) | Gemini驱动的命令行漫画生成流水线 |
| **LumenX (阿里)** | [alibaba/lumenx](https://github.com/alibaba/lumenx) | 剧本→角色→分镜→视频一站式平台 |
| **DiffSensei (北大)** | [jianzongwu](https://github.com/jianzongwu/DiffSensei) | CVPR 2025，4.3万页漫画数据集MangaZero |
| **prompt-to-comic** | [mettafore](https://github.com/mettafore/prompt-to-comic) | LangGraph+GPT-4o+DALL·E全自动漫画生成 |
| **Make Comics** | [Nutlope](https://github.com/Nutlope/make-comics) | Next.js+Google Flash Image生成漫画 |

### 🎓 推荐学习路径

1. **B站搜索**「秋叶 ComfyUI 工作流」— 2025最新节点搭建教程
2. **B站搜索**「AI漫剧 场景提示词」— 大量国漫实战案例
3. **CSDN博客** 搜「愚公系列 AI漫剧创作一本通」— 系统化场景设计原则
4. **什么值得买** 搜「AI漫剧」— 大量个人创作者实战分享
5. **RunComfy** ([runcomfy.com](https://www.runcomfy.com)) — Consistent Character Creator 3.0 工作流

> 🎯 **场景设计核心心法**：场景设计不是画背景，是**在空间中讲故事**。光有方向、色有情绪、物有位置、空间有结构——四者锁死，场景就稳了。最好的场景参考库不是Pinterest，是你手机相册里**真实世界**的照片。

---

## 📖 延伸资源

- [[../02_角色设计/README|上一步：角色设计]]
- [[../04_分镜脚本/README|下一步：分镜脚本]]
- [[../10_参考资料/README|完整工具清单]]
- Pinterest 搜 `environment concept art` — 全球顶级场景设计参考
- film-grab.com — 按电影看每个镜头的色调和光影
- [[01_场景母代码库|场景母代码库]] — 6大类型的完整模板
- [[02_各题材场景提示词|各题材场景提示词大全]] — 直接复制使用
