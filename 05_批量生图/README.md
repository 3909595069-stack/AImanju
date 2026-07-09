---
aliases: [批量生图, 分镜画面, AI绘画, 提示词工程]
tags: [核心步骤, step5]
step: 5
icon: 🖼️
sidebar_label: 文字变成画面
parent: "[[../04_分镜脚本/README|分镜脚本]]"
---

# 批量生图：把文字变成画面

> 先出图，再生视频——这是 AI 短片工作流里最容易被跳过、也最不该被跳过的一步。

## 🔍 这是啥？

**批量生图**，就是根据 [[../04_分镜脚本/README|分镜脚本]] 中的每个镜头描述，逐张生成对应的静态画面。

这一步的产物是：一组和你的分镜一一对应的关键帧图像。它们是下一步 [[../06_图生视频/README|图生视频]] 的输入。

> 核心逻辑：先确保每一帧的画面是对的，再让它们动起来。跳过这一步直接文生视频 = 每一秒都在掷骰子。

## 💡 为啥做？

**为什么先出图再生视频——而非直接文生视频？**

1. **精确度**：文生视频的不可控性远大于文生图。先出图可以逐张验收画面是否对，不对就重来——成本极低
2. **一致性**：同一个角色的定妆照可以在所有生图中作为参考图锚定。文生视频没有这个锚点
3. **首尾帧控制**：图生视频可以指定首帧和尾帧，实现精确的镜头衔接。文生视频做不到
4. **试错成本**：一张图生成 4 次选最优 ≈ 10 秒；一段视频生成 4 次选最优 ≈ 20 分钟

> 一句话：生图是你对最终画面的最后一次精确控制。

---

## 🎨 生图提示词深度工程

### 公式一：七要素全能公式（通用版）

```
[主体描述] + [动作/状态] + [环境/场景] + [风格/媒介] + [构图/视角] + [光影/色彩] + [画质/细节]
```

**每个模块按重要性从左到右排列——AI 对越靠前的词越敏感。**

### 公式二：六要素精简版（即梦/国产工具优化版）

```
[画风关键词] + [场景母代码] + [角色外貌] + [动作/表情] + [景别/角度] + [光影/色调]
```

### 公式三：三阶提示词链（专业版 · Prompt Chaining）

这是2025年业界验证的最佳实践，将一条长 prompt 拆成三步迭代：

**第一阶段 — 角色定妆**：
```
[风格] + [角色外貌细节] + [全身/半身] + [中性背景] + [角色参考图锚定]
```
> 示例：`日式动画电影风格，赛璐珞渲染，25岁女性，黑色长直发齐刘海，白色衬衫深蓝半裙，全身站立，中性灰背景，character sheet`

**第二阶段 — 场景注入**：
```
[第一阶段最优图作为参考] + [场景母代码] + [动作描述] + [光影氛围]
```
> 示例：`[参考图1] + 深夜便利店内部，L型空间，左侧3排货架，门口红色立式冰柜 + 站在收银台后面，低头看手机 + 冷白荧光灯顶光，门口暖黄灯光渗入`

**第三阶段 — 镜头精调**：
```
[第二阶段最优图作为参考] + [景别调整] + [镜头角度] + [特效/氛围强化]
```
> 示例：`[参考图2] + 中景 + 平视，固定机位 + 冷白+暖黄+红点缀色调，轻微film grain`

> **为什么用提示词链？** 一条超长 prompt 容易让 AI "迷失重点"。分步迭代每步只让 AI 解决一个问题，最终效果远优于一步到位。

### 提示词权重控制语法

不同工具支持不同的权重语法：

| 工具 | 加强权重 | 减弱权重 | 交替融合 |
|:--|:--|:--|:--|
| **Stable Diffusion** | `(关键词:1.3)` | `(关键词:0.7)` | `[选项A\|选项B]` |
| **Midjourney** | `关键词::2` | `关键词::-0.5` | `{选项A, 选项B}` |
| **即梦/豆包** | 靠位置（越靠前越重要） | 放后面 | 不支持 |
| **ComfyUI** | Conditioning节点权重 | Conditioning节点权重 | 双CLIP编码器 |

### 2025年前沿：AI自动写Prompt

腾讯混元开源的 **PromptEnhancer** 框架（2025），通过24维度对齐人类意图 + 思维链CoT + 强化学习GRPO训练，能将简单指令自动扩展为专业提示词：

```
输入：穿宇航服的汤姆猫
输出（自动）：masterpiece, best quality, 3D rendered, Pixar-style, an anthropomorphic 
grey tabby cat wearing a detailed white NASA spacesuit with reflective visor, 
standing on lunar surface, Earth visible in starry background, soft cinematic 
lighting, subsurface scattering on fur, shallow depth of field, 8K, highly detailed
```

> 漫画「属性绑定」「否定指令」「复杂空间关系」场景准确率提升15%-17%。

---

## 🤖 主流生图工具全维度对比

### 五大工具矩阵（2025年版）

| 维度 | **即梦 4.0** | **Midjourney V7** | **SD+ComfyUI** | **Flux.1** | **DALL-E 3** |
|:--|:--|:--|:--|:--|:--|
| **中文理解** | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐（需翻译） | ⭐⭐ | ⭐⭐⭐ |
| **角色一致性** | ⭐⭐⭐⭐（参考图） | ⭐⭐⭐⭐⭐（--oref） | ⭐⭐⭐⭐⭐（LoRA） | ⭐⭐⭐⭐ | ⭐⭐ |
| **风格控制** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **批量效率** | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| **上手难度** | ⭐（极低） | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐ |
| **价格** | 免费/68元/月 | $30/月起 | 免费（需GPU） | 免费（需GPU） | 含ChatGPT |
| **国内可用** | ✅ 是 | ❌ 需翻墙 | ✅ 是 | ✅ 是 | ❌ 需翻墙 |
| **漫画最适用** | 日常/治愈/甜宠 | 电影感/写实 | 专业流程/连载 | 插画品质 | 快速概念 |
| **特色功能** | 角色特征锁定 | Omni-Reference | ControlNet/LoRA | 文本渲染强 | 原生多语言 |

### 新手推荐路径

```
入门（0元）：即梦4.0 免费额度 → 豆包生图
进阶（68元/月）：即梦4.0 会员 → 解锁全部功能
专业（本地GPU）：ComfyUI + SDXL/Flux → 完全自由的批量管线
高端（$30/月+翻墙）：Midjourney V7 → 电影级品质
```

### Midjourney 角色一致性黑科技（2025版）

| 参数 | 版本 | 用途 | 权重范围 |
|:--|:--|:--|:--|
| `--cref [URL]` | V6.1/Niji | 锁定角色面部+发型+服装 | `--cw 0-100` |
| `--oref [URL]` | **V7（2025.5发布）** | 全能参考——角色+物体+道具 | `--ow 0-1000` |
| `--sref [URL]` | V6/V7 | 锁定艺术风格（不影响角色） | `--sw 0-1000` |

**关键用法**：
- `--cw 100`：锁定面部+发型+服装（默认）
- `--cw 0`：只锁面部，可换装/换发型
- `--oref + --sref` 同时使用：角色是谁 + 什么画风 = 完美锁定
- 一句 prompt 同时用两个 `--oref` 可以让两个角色出现在同一画面

---

## 👤 角色一致性终极指南

这是AI漫剧制作中最核心的技术难题。以下是2025年经过验证的三层方案：

### 第一层：即梦「参考图」模式（零门槛）

```
步骤：
1. 生成一张满意的角色正面定妆照
2. 进入「角色特征」模式，上传参考图
3. 参考强度选择：70-80%（同场景换动作）/ 30-50%（换装/换背景）
4. 用新场景描述生成→ 角色保持一致性
```

| 参考强度 | 效果 | 适用场景 |
|:--|:--|:--|
| 80-100% | 面部+发型+服装完全一致 | 同场景换动作 |
| 50-70% | 保留面部+发型 | 换装/微调 |
| 30-50% | 仅保留面部特征 | 大幅换装/换背景 |
| <30% | 仅保留面部大致轮廓 | 不同年龄段/风格化 |

**三要素完全不变**（锁定角色核心）：
- 艺术风格关键词
- 主体形象（五官、发型）
- 服装配饰关键词

**可变化的**（推动剧情）：
- 环境场景描述
- 镜头构图/景别
- 人物动作/表情

### 第二层：Stable Diffusion LoRA 角色训练（专业级）

训练一个专属 LoRA 模型，是保持原创角色一致性的终极方案。

**训练数据集要求**：
- **15-30张高质量参考图**，必须包含不同角度、表情、光照
- 质量 > 数量：宁可15张全对，不要50张掺杂崩图
- 包含正面/侧面/半侧面/背面等多种角度

**训练参数参考（Kohya_ss）**：

| 参数 | 推荐值 | 说明 |
|:--|:--|:--|
| Rank | 4-16 | 角色LoRA用rank 16足够 |
| 学习率 | 1e-4 (0.0001) | 范围0.0001-0.0005 |
| 步数 | 800-1800（SDXL） | 观察loss曲线防过拟合 |
| 分辨率 | 768×768 或 1024×1024 | 取决于底模 |
| Batch Size | 1 + 梯度累积4-8 | 显存不够时的策略 |
| 触发词 | 唯一标识，如 `sarah_chara` | 生图时写这个就能召唤角色 |

**常见训练翻车**：
- ❌ 只用了正面照 → LoRA 不会画侧面
- ❌ 只有一种表情 → 换表情就崩
- ❌ 训练太久（>2000步） → 过拟合，只会画训练集里的姿势

### 第三层：IP-Adapter + ControlNet（免训练方案）

不想训练 LoRA？用 ControlNet 锁定结构和构图 + IP-Adapter 注入面部特征：

```
IP-Adapter FaceID Plus v2 (SDXL) + OpenPose ControlNet
→ 面部特征从参考图提取（权重0.4-0.7）
→ 姿势/结构从姿态骨架锁定
→ 综合一致性可达80%+，无需训练
```

### 一致性技术选型指南

| 方案 | 时间投入 | 一致性水平 | 最适合 |
|:--|:--|:--|:--|
| 即梦参考图 | 即时 | 70-85% | 新手/快速出片 |
| IP-Adapter + ControlNet | 20-60分钟设置 | 70-80% | 快速测试/单发 |
| LoRA训练（单角色） | 2-6小时训练 | 85-95% | 连载/系列化作品 |
| LoRA + IP-Adapter + ControlNet | 完整工作流 | 90%+ | 专业级制作 |

---

## 🎭 画风关键词完全库

### 12种漫画风格 × 中英文关键词 × 推荐工具

| 风格方向 | 正向提示词组合 | 推荐模型/平台 | 关键参数 |
|:--|:--|:--|:--|
| **日式动画** | `Japanese animation style, cel shading, Ghibli-inspired, clean lines, soft gradients, Studio Ghibli aesthetic` | 即梦4.0 / MJ Niji / SD+Anything V5 | Niji `--style expressive` |
| **韩式网漫** | `Korean webtoon style, manhwa art, digital painting, smooth rendering, romantic palette, soft shading, clean lineart` | 即梦4.0 / MJ Niji / SD+Pony | `--s 600-800` |
| **3D卡通** | `3D rendered, Pixar-style, soft lighting, round shapes, subsurface scattering, C4D, octane render, vibrant colors` | 即梦 / MJ / ComfyUI+SDXL | `--s 400-600` |
| **古风水墨** | `Chinese ink wash painting, gufeng, watercolor texture, misty atmosphere, scroll painting, sumi-e, minimalist brushwork` | 即梦 / MJ / SD+专用LoRA | `--s 300-500` |
| **写实电影** | `photorealistic, cinematic lighting, 35mm film grain, anamorphic lens, shallow depth of field, 8K, RAW photo` | MJ V7 / SD+Realistic模型 | `--s 100-300` |
| **赛博朋克** | `cyberpunk, neon lights, rain-slicked streets, holographic displays, Blade Runner aesthetic, dystopian future, chromatic aberration` | MJ / SDXL / ComfyUI | `--s 600-1000` |
| **美式漫画** | `American comic book style, bold ink lines, Ben-Day dots, halftone patterns, primary colors, Roy Lichtenstein style` | MJ / SD+Comic LoRA | `--s 500-700` |
| **黑白线稿** | `black and white manga, ink drawing, screentone, crosshatching, stark contrast, clean linework, manga panel style` | SD+线稿LoRA / MJ | `--no color` |
| **水彩插画** | `watercolor painting, soft washes, bleeding edges, paper texture, pastel tones, artistic illustration, dreamy atmosphere` | MJ / SD+水彩LoRA | `--s 400-600` |
| **90年代复古动画** | `90s anime aesthetic, retro anime, cel animation, grainy film, VHS artifacts, vintage color palette, hand-drawn feel` | MJ / Niji / SD+复古LoRA | `--s 300-500` |
| **浮世绘风格** | `ukiyo-e style, Japanese woodblock print, flat colors, bold outlines, Hokusai inspired, decorative patterns` | MJ / SD+专用LoRA | `--s 400-600` |
| **厚涂油画** | `oil painting style, thick brushstrokes, impasto technique, rich textures, palette knife marks, dramatic lighting, classical composition` | MJ / SD+油画LoRA | `--s 500-800` |

### 风格锁定防跑偏技巧

在提示词中**重复核心风格词 + 限制色板**：
```
[核心风格词重复2-3次] + [主色调: 鹅黄、靛青、檀木棕] + 
[排除: --no realistic, photorealism, 写实风格]
```

---

## 💡 光影色调情绪引擎

> 光影是画面的灵魂——它决定了观众在0.1秒内的情绪反应。

### 五大光型速查

| 光型 | 英文关键词 | 效果 | 情绪 |
|:--|:--|:--|:--|
| **正面光** | `front lighting, flat lighting` | 细节清晰、减少阴影 | 坦诚、明亮、中性 |
| **侧面光** | `side lighting, split lighting` | 强烈立体感、半明半暗 | 张力、冲突、戏剧性 |
| **逆光/轮廓光** | `back lighting, rim light, silhouette` | 勾勒轮廓、梦幻光晕 | 浪漫、神秘、神圣 |
| **顶光** | `overhead lighting, top-down light` | 面部阴影、戏剧性反差 | 压抑、悬疑、宗教感 |
| **底光** | `bottom lighting, underlighting` | 颠覆正常光影 | 恐怖、诡异、奇幻 |

### 六种氛围色调速查

| 氛围 | 关键提示词 | 画面效果 |
|:--|:--|:--|
| 🌅 **黄金时刻** | `golden hour light, warm sunset glow, long shadows, amber tones` | 温暖迷人、夕阳宁静 |
| 🌅 **蓝调时刻** | `blue hour, twilight, cool tones, soft diffused light, dusk atmosphere` | 清冷、宁静、过渡感 |
| ☀️ **晨光希望** | `morning light, soft sunlight through window, gentle rays, fresh atmosphere` | 宁静清新、充满希望 |
| 🌧️ **阴郁暗调** | `moody darkness, overcast, misty foggy, cold light, desaturated tones` | 忧郁、神秘、压抑 |
| 🌆 **霓虹夜晚** | `neon lights, night city, rain-slicked pavement, colorful reflections, ambient glow` | 赛博朋克、迷幻、都市孤独 |
| 🔥 **戏剧暖光** | `chiaroscuro, dramatic warm spotlight, Rembrandt lighting, deep shadows` | 厚重、戏剧感、古典 |

### 经典打光技法

| 打光法 | 英文关键词 | 特征 | 适合 |
|:--|:--|:--|:--|
| **伦勃朗光** | `Rembrandt lighting, triangle of light under eye` | 一侧脸三角形光斑 | 人物肖像、厚重感 |
| **电影光** | `cinematic lighting, motivated lighting, atmospheric` | 画面如电影截图 | 剧情向、情感表达 |
| **霓虹光** | `neon lights, colored gels, cyberpunk lighting` | 彩色光源、都市感 | 赛博朋克、科幻 |
| **体积光** | `volumetric lighting, god rays, crepuscular rays, Tyndall effect` | 可见光束穿透空间 | 神圣、梦幻、森林 |
| **蝴蝶光** | `butterfly lighting, paramount lighting` | 鼻下蝴蝶形阴影 | 美妆、女性角色 |

### 光影万能提示模板

```
[灯光类型] + [光源方向] + [阴影质感] + [色温] + [特殊效果]

示例：
cinematic side lighting from a window, soft shadows with ambient fill,
warm tungsten color temperature (3200K), subtle lens flare,
dust particles floating in light beam
```

### 色温标准术语表

| 术语 | 色温范围 | 画面印象 |
|:--|:--|:--|
| `candlelight` | 1800-2000K | 极暖、浪漫、古老 |
| `tungsten light` | 2800-3200K | 温暖、室内、舒适 |
| `warm white` | 3500-4000K | 温和、家居、咖啡厅 |
| `neutral white` | 5000-5500K | 自然、还原真实颜色 |
| `daylight` | 5500-6500K | 正午阳光、清晰、冷静 |
| `overcast / cool white` | 6500-7500K | 阴天、清冷、科技感 |
| `moonlight` | 8000-10000K | 极冷、神秘、幽静 |

---

## 🏭 工业化批量生图工作流

### 项目文件夹结构（标准化模板）

```
📁 项目名_AI漫剧/
├── 📁 01_脚本素材/
│   ├── 剧本_终稿.md
│   ├── 分镜脚本_终稿.md
│   └── 提示词库_按镜头编号.xlsx
├── 📁 02_角色资产/
│   ├── 主角_三视图/
│   │   ├── 主角_正面_定妆.png
│   │   ├── 主角_侧面_定妆.png
│   │   ├── 主角_背面_定妆.png
│   │   └── 主角_表情集_喜怒哀惊.png
│   ├── 配角A_定妆.png
│   └── 角色设定卡_汇总.md
├── 📁 03_场景资产/
│   ├── 场景01_咖啡馆/
│   │   ├── 咖啡馆_主视角.png
│   │   ├── 咖啡馆_反打.png
│   │   └── 咖啡馆_细节_收银台.png
│   └── 场景02_街道/
├── 📁 04_生图输出/
│   ├── S01_全景_咖啡馆外观_定稿.png
│   ├── S01_全景_咖啡馆外观_v1_reject.png
│   ├── S02_中景_主角进门_定稿.png
│   └── ...
├── 📁 05_后期精修/
│   ├── S01_放大超清.png
│   ├── S02_手指修复.png
│   └── ...
├── 📁 06_视频输出/
└── 📁 07_参考资料/
```

### 生图进度追踪表（CSV/Excel模板）

| 镜号 | 景别 | 描述摘要 | 角色 | 场景 | 状态 | 定稿图 | 尝试次数 | 问题备注 |
|:--|:--|:--|:--|:--|:--|:--|:--|:--|
| S01 | 全景 | 咖啡馆外观，夜雨 | 无 | 场景01 | ✅ 完成 | `S01_定稿.png` | 3 | 第1、2次光影偏暗 |
| S02 | 中景 | 主角推门而入 | 主角 | 场景01 | ✅ 完成 | `S02_定稿.png` | 1 | 一次过 |
| S03 | 特写 | 手拿咖啡杯 | 主角 | 场景01 | ❌ 待重做 | — | 4 | 手指扭曲，需策略一 |

**状态标记**：✅ 完成 | 🔄 待审核 | ❌ 重做 | ⏭️ 跳过 | 📌 备用

### 批量生图质量检查清单

**生图前（Pre-Flight）**：
- [ ] 角色三视图已确认并锁定关键词
- [ ] 场景背景母版图已建立（每场景6-9张备选）
- [ ] 每条提示词包含完整要素（画风+景别+角色+动作+场景+光影）
- [ ] 图片比例统一设定（9:16竖屏或3:4）
- [ ] 负面提示词已统一准备
- [ ] 文件命名规范已确定

**生图中（In-Flight）**：
- [ ] 同角色服装/发型/瞳色全程一致
- [ ] 同场景光影色调一致（粘贴场景母代码）
- [ ] 每镜至少生成2-4张备选
- [ ] 崩图及时标记/删除，不混入素材库
- [ ] 进度表实时更新

**生图后（Post-Flight）**：
- [ ] 所有镜头逐一对照分镜脚本检查
- [ ] 角色一致性抽查（同一角色3-5镜面部对比）
- [ ] 场景连续性检查（同一场景所有镜头背景一致）
- [ ] 手指/面部特写重点检查
- [ ] 放大/锐化处理（分辨率≥1080P）
- [ ] 定稿图已按镜号统一命名

---

## 🔧 ComfyUI 漫画专用工作流

> ComfyUI 是目前最灵活的 AI 图像生成管线工具。它的节点式界面让你可以构建可复用的、精确控制的批量生图流程。

### 漫画专用节点工作流逻辑

```
[参考图加载] → [CLIP文本编码器] → [KSampler(底模)] → [VAE解码] →
    ↓                                                    ↓
[ControlNet(Canny/OpenPose)]                    [面部细化(Adetailer)]
    ↓                                                    ↓
[IP-Adapter FaceID]                            [放大(Upscale 4×)] →
                                                       ↓
                                              [保存输出 PNG]
```

### 推荐工作流模板

| 模板名称 | 核心功能 | 适用 |
|:--|:--|:--|
| **Consistent Character Creator 3.0** | 从单张参考图生成多角度、多表情角色集 | 角色资产建设 |
| **VNCCS 3.0** | 角色创建 → 换装 → 表情批量 → 自动去背景 | 连载漫剧 |
| **ACE++ Character Consistency** | 免训练指令式角色一致性（Flux Fill） | 快速出片 |
| **Endless Consistent Characters** | 多视角 → 面部放大 → 表情 → 光照 → 角色合集 | 专业角色设计 |

### ComfyUI 关键节点说明

| 节点名称 | 作用 | 关键参数 |
|:--|:--|:--|
| **Load Checkpoint** | 加载底模 | 漫画推荐：Pony Diffusion V6 / Animagine XL |
| **CLIP Text Encode** | 正/负面提示词编码 | 正面词权重靠前、负面词统一 |
| **KSampler** | 核心采样器 | 漫画推荐：Euler A / DPM++ 2M Karras, CFG 5-7, Steps 25-35 |
| **ControlNet (Canny)** | 边缘检测锁定构图 | 权重 0.6-0.8 |
| **ControlNet (OpenPose)** | 人体骨骼锁定姿势 | 权重 0.7-0.9 |
| **IP-Adapter FaceID** | 面部特征注入 | 权重 0.4-0.7 |
| **Upscale 4× Ultrasharp** | 4倍放大+锐化 | 漫画专用模型 |
| **Adetailer / Face Detailer** | 面部自动检测+修复 | 检测置信度 0.5 |

### ComfyUI 批量生图脚本思路

通过 ComfyUI 的 `Queue Prompt` 功能配合 CSV/JSON 输入，可以实现一键批量生成：
1. 准备一个 JSON 文件，包含所有镜头的提示词
2. 使用 `Load Prompt From File` 节点读取
3. 使用 `KSampler` 批量处理，每镜生成4张备选
4. 使用 `Save Image` 按镜号自动命名

---

## 🔍 图片质量控制与后期处理

### 放大算法选择指南

| 放大模型 | 最适合 | 倍数 | 特点 |
|:--|:--|:--|:--|
| **4× Ultrasharp** | 漫画/二次元 | 4× | 线条锐利、细节增强强 |
| **2x-Manga-Ora** | 黑白漫画/线稿 | 2× | 保留手绘感、去除色彩 |
| **ESRGAN 4×** | 通用 | 4× | 细节还原好、速度适中 |
| **foolhardy_Remacri** | 漫画/动画纹理 | 4× | 纹理细节突出 |
| **Real-ESRGAN** | 写实照片/混合 | 4× | 去模糊+降噪 |

### 后期统一调色参数

AI 不同批次生成的图常有色调偏差——需要统一调色来"糊成一部剧"：

| 参数 | 建议 | 作用 |
|:--|:--|:--|
| **色温** | 暖色 +5~+10 / 冷色 -5~-10 | 统一冷暖基调 |
| **饱和度** | -5~-10 | AI图通常太鲜艳，降一点更耐看 |
| **对比度** | +5~+10 | 增加立体感和层次 |
| **锐化** | +10~+20 | 弥补AI生图柔感 |
| **曝光** | ±0.2 | 轻微修正过曝/欠曝 |

> **关键技巧**：同一场景的所有镜头，在 Photoshop/剪映中**复制粘贴调色参数**——这是保持画面一致的最快方法。

### 局部重绘修复

当 AI 画崩了某个局部（手指/表情/道具）时：

| 修复方案 | 操作 | 适用 |
|:--|:--|:--|
| **即梦AI「编辑」功能** | 圈选问题区域 → 输入修正词 | 面部、手指小范围 |
| **ComfyUI Inpaint** | Mask 问题区域 → 降低 Denoising 0.4-0.6 重新生成 | 局部修复不改风格 |
| **Photoshop 手动修** | 液化+修补+克隆工具 | 精度要求高时 |
| **美图秀秀/醒图** | AI消除/AI扩图 | 快速、零操作门槛 |

---

## 🚫 生图翻车大全与排错

### 八大经典翻车问题

| # | 问题 | 典型表现 | 根因 | 标准解法 |
|:--|:--|:--|:--|:--|
| 1 | 🖐️ **手指扭曲** | 6根手指、手指粘连、手指错位 | AI模型的共同弱点 | 藏手/构图规避/负面词/多重生成/局部重绘 |
| 2 | 👤 **角色"变脸"** | 同一角色不同镜头像两个人 | 未使用参考图/风格词漂移 | 锁定参考图+固定风格关键词+LoRA触发词 |
| 3 | 👁️ **眼睛不对称** | 左右眼大小不一、方向不同 | 侧脸角度AI理解错误 | 负面词加`asymmetrical eyes`，正面词加`symmetrical face` |
| 4 | 🏠 **场景背景漂移** | 同一场景不同镜头环境不一致 | 未使用场景母代码 | 每个镜头末尾固定粘贴场景母代码 |
| 5 | 🎨 **风格跑偏** | 同一集画风突变 | 风格关键词不一致/模型版本不同 | 固定风格关键词+固定模型版本号+相同seed范围 |
| 6 | 📐 **构图崩塌** | 主体变形、比例失调、透视错误 | 提示词要素冲突 | 简化提示词/使用ControlNet锁定构图/添加`anatomically correct` |
| 7 | 🌫️ **模糊发虚** | 画面不够锐利、细节朦胧 | 分辨率不足/采样步数少 | 提升分辨率到1024+/Steps增至30+/加`sharp focus, highly detailed` |
| 8 | 🐌 **生成速度慢** | 一张图等几分钟 | GPU性能/工具限制 | 降低分辨率先出小图再放大/使用LCM加速LoRA/换工具 |

### 手指问题终极排错流程

```
Step 1: 构图规避（成功率90%）→ 不用手部特写景别
Step 2: 藏手设计（成功率100%）→ 手背身后/插兜/被遮挡
Step 3: 负面提示词强化（成功率60%）→ 加满手指相关负面词
Step 4: 多重生成选最优（成功率70%/8张）→ 同一prompt生成8张
Step 5: 局部重绘（成功率85%）→ 只对有问题区域重绘
Step 6: 后期PS修图（成功率100%，最费时）→ 手动精修
```

### 角色一致性崩溃诊断

```
检查清单：
□ 是否使用了参考图功能（即梦的「角色特征」/MJ的--cref或--oref）？
□ 风格关键词在新提示词中是否完全一致？
□ 服装/发型/瞳色关键词是否被新描述覆盖？
□ 是否换了工具/模型版本？
□ 是否加入了和角色设定冲突的新元素（如"现代服装"覆盖了"古装"）？
□ 同一角色的不同镜头是否用了同一条基础prompt链？
```

---

## 🧰 生图工具链与学习资源

### 专业工具推荐

| 工具 | 类型 | 费用 | 推荐理由 |
|:--|:--|:--|:--|
| **即梦 4.0** | 在线生图 | 免费/68元 | 中文理解最好、角色一致性最强 |
| **豆包** | 在线生图 | 免费 | 字节系、和即梦互补 |
| **Midjourney V7** | 在线生图 | $30/月 | 电影级画质、--oref角色锁定 |
| **ComfyUI** | 本地管线 | 免费 | 节点化批量管线、无限DIY |
| **AUTOMATIC1111 WebUI** | 本地界面 | 免费 | 入门首选、插件生态丰富 |
| **Kohya_ss** | LoRA训练 | 免费 | 业界标准LoRA训练工具 |
| **OneTrainer** | 模型训练 | 免费 | 比Kohya_ss更现代的训练UI |
| **剪映** | 后期修图 | 免费/会员 | AI消除/AI扩图/统一调色 |

### 学习资源推荐

**📚 书籍**：
- 《AIGC商业插画应用技巧》（2024年底）——AI绘画在商业场景的系统方法论
- 《Stable Diffusion AI绘画全面贯通》（2024年底）——从生成参数到模型训练的完整教程
- 《AI绘画：从入门到精通》——中文社区最全面的 SD 入门书

**📺 B站UP主/教程**：
- 搜索「即梦AI教程 漫画」——大量免费实战教程
- 搜索「ComfyUI 漫画工作流」——节点管线搭建手把手
- 搜索「Midjourney 角色一致性」——MJ进阶技巧
- 搜索「Stable Diffusion LoRA训练」——角色LoRA训练保姆级教程

**📄 论文（进阶）**：
- PromptEnhancer — 腾讯混元24维度提示词增强框架（2025）
- PRISM: Automated Black-box Prompt Engineering for T2I（arXiv 2403.19103）
- OminiControl Comic LoRA — 基于Flux的漫画角色一致性（KSE 2025）

**💻 GitHub仓库**：
- `github.com/Hunyuan-PromptEnhancer/PromptEnhancer` — 腾讯提示词增强框架
- `github.com/cubiq/ComfyUI_IPAdapter_plus` — ComfyUI IP-Adapter 节点
- `github.com/AHEKOT/ComfyUI_VNCCS` — 视觉小说/漫画专用ComfyUI工作流
- `github.com/Justin21523/charaforge-T2I-Lab` — 大规模动漫角色微调实验室

---

## 📖 延伸资源

- [[../04_分镜脚本/README|分镜脚本——生图的上游输入]]
- [[../06_图生视频/README|图生视频——生图的下游工序]]
- [[../10_参考资料/README|工具清单——即梦/MJ/SD 怎么选]]
- B站搜索「即梦提示词」——大量 prompt 实战教程
- B站搜索「AI漫剧 批量生图 工作流」——最新实战分享
