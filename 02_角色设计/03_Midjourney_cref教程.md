---
aliases: [Midjourney cref, MJ角色参考, 角色一致性]
tags: [工具教程, Midjourney, 角色设计]
step: 2
parent: "[[02_角色设计/README|角色设计]]"
---

# 🎯 Midjourney --cref 完整教程

## 基本概念

Midjourney 的 `--cref`（Character Reference，角色参考）是 MJ 官方提供的角色一致性功能。你上传一张角色图片，MJ 就会在后续的生成中「照着这个人的样子画」。

## 核心参数

| 参数 | 完整写法 | 作用 | 取值范围 | 推荐 |
|------|---------|------|---------|------|
| `--cref` | Character Reference | 上传角色参考图的 URL | 1-3 张图 | 2 张（正面+侧面） |
| `--cw` | Character Weight | 参考图的「影响力」 | 0-100 | 50-80 |
| `--sref` | Style Reference | 锁定画风和色调的参考图 | 1 张 | 1 张 |
| `--seed` | Seed | 固定随机种子，同种子=同人物 | 任意整数 | 记录首张满意的值 |

## 操作步骤

### 1. 生成初始角色

```
a young female warrior, silver hair, red eyes, black armor,
front view, full body, character design sheet, white background --v 7.0 --ar 16:9
```

### 2. 用 --cref 生成不同角度

拿到初始角色的图片后，复制图片链接，放入后续提示词：

```
side view of the same character --cref [正面图URL] --cw 80 --v 7.0
back view of the same character --cref [正面图URL] --cw 80 --v 7.0
```

### 3. 用 --sref 锁定风格

```
the warrior standing in a bamboo forest, morning mist --cref [角色URL] --sref [风格参考图URL] --v 7.0
```

### 4. 记录 seed

首张满意角色图生成后：
1. 在 Discord 中右键点击生成的图片
2. 选择「添加反应」→ 搜索 ✉️（envelope）
3. Midjourney Bot 会私信你这条生成的所有参数，包含 seed 值
4. 记录下这个 seed 值，后续所有相同角色的生成都加上 `--seed [该值]`

## 参数调节技巧

| 场景 | 设置 | 效果 |
|------|------|------|
| 换场景不换角色 | `--cref [URL] --cw 80` | 角色外观不变，场景自由变化 |
| 换装不换脸 | `--cref [URL] --cw 0` | 只锁脸，服装可以完全不同 |
| 全套锁定 | `--cref [URL] --cw 100` | 脸+发型+服装全部锁定 |
| 多角色混合 | `--cref [URL1] --cref [URL2]` | 融合两个角色的特征 |

## V6 vs V7 的区别

| 功能 | V6 `--cref` | V7 `--oref` |
|------|-----------|-----------|
| 锁什么 | 角色外观（脸、头发、服装） | 角色 + 物体的身份（更全能） |
| 控制参数 | `--cw` 0-100 | `--ow` 1-1000（推荐 50-250） |
| 参考图数量 | 支持多张 | 每次只能 1 张 |
| 智能程度 | 成熟稳定 | 更聪明，处理复杂角度更好 |
| GPU 消耗 | 标准 | **2 倍** |

> **如果刚开始用 MJ → 用 V6 `--cref`，更稳定。如果追求极致 → 用 V7 `--oref`，效果更好但消耗更高。**
