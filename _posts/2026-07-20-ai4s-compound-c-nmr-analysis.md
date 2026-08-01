---
title: "AI4S 结构解析：化合物 C 的 NMR & ESI-MS 综合谱学分析"
date: 2026-07-20
tags: [AI4S, NMR, 化学波谱]
---

> 这是「AI4S 交叉实践课程」（张韶光老师）的化合物 C 结构确证汇报整理，忠实记录汇报内容与所有工具链接。

![封面]({{ '/assets/images/ai4s-compound-c/slide-00.png' | relative_url }})

---

## 汇报提纲

01 反应分析与初始候选结构
02 数据处理及谱图质量评估
03 关键 NMR 证据与峰归属
04 AI 工具：输入、输出与人工审核
05 ESI-MS 转化产物及其与 C 的关系
06 最终结构假设、局限性与后续实验

![汇报提纲]({{ '/assets/images/ai4s-compound-c/slide-01.png' | relative_url }})

---

## 01 反应分析与初始候选结构

由反应可能的杂质和溶剂：**C₆D₆、二乙胺、二乙胺合硼烷、二碘丙烷、硼烷、THF**。

![反应分析]({{ '/assets/images/ai4s-compound-c/slide-02.png' | relative_url }})

---

## 02 数据处理及谱图质量评估

选择化合物峰，谱图瞬间干净。

![谱图质量评估]({{ '/assets/images/ai4s-compound-c/slide-03.png' | relative_url }})

### 杂质判断（AI 输出，非实验结论）

- ChatGPT 认为杂质出峰：11.3 δ，48.5 δ → **二乙胺合硼烷**（ChatGPT 参考检索的文献）
- NMRium 预测二乙胺合硼烷：38.36 δ，14.85 δ
- ChemDraw 预测二乙胺合硼烷：40 δ，10.9 δ
- NMRium 预测二乙胺：15.7 δ，44.50 δ
- ChemDraw 预测二乙胺：44.1 δ，15.1 δ
- 1.3 δ：常见幽灵峰，极易从硅胶、实验器材中引入（DeepSeek）

![杂质判断]({{ '/assets/images/ai4s-compound-c/slide-04.png' | relative_url }})

---

## 03 关键 NMR 证据与峰归属

### P 连接的 C（预测谱图差异点）

| 工具 | P 连接 C 预测 (δ) |
|------|------------------|
| ChemDraw | 1.3 / 1.4 |
| NMRium | 3.99 / 1.91 |
| DeepSeek 推断 | 1.8–2.2 / 1.0–1.6 |
| 实际谱图归属 | 2.13 / 2.32 |

![关键NMR证据]({{ '/assets/images/ai4s-compound-c/slide-05.png' | relative_url }})

### 耦合裂分情况

- ChemDraw 预测、NMRium 预测、原始谱图对比
- 裂分比例 1:6:15:20:15:6:1 → **裂分情况符合**
- 耦合常数：ChemDraw 6.92 Hz / NMRium 6.8 Hz / 实测 7 Hz（0.014 ppm × 500 MHz）→ **耦合情况符合**

![耦合裂分]({{ '/assets/images/ai4s-compound-c/slide-06.png' | relative_url }})

---

## 04 AI 工具：输入、输出与人工审核

### 使用的 AI

- 大模型：DeepSeek（识图模式，深度思考）、Gemini（3.5 flash）、ChatGPT（pro）
- 专用小模型：SpecXMaster

![AI工具总览]({{ '/assets/images/ai4s-compound-c/slide-07.png' | relative_url }})

### 通用大模型的表现

> 能力一般的大模型难以独立完成整个任务，提供的信息多而杂，需要使用者有一定的判别能力提取出正确信息。
> —— DeepSeek、Gemini（底物识别有问题）

![通用大模型]({{ '/assets/images/ai4s-compound-c/slide-08.png' | relative_url }})

> 思维不够发散，遇到难以解释的问题也很少去反思结构的原因，更倾向于强行解释。
> —— Gemini，需要使用者的判断

![Gemini局限]({{ '/assets/images/ai4s-compound-c/slide-09.png' | relative_url }})

> 大模型的作用：可以提供一些正确的线索。
> —— Gemini

![大模型作用]({{ '/assets/images/ai4s-compound-c/slide-10.png' | relative_url }})

### ChatGPT pro

> 更强的大模型：可以独立完成复杂任务。

![ChatGPT pro]({{ '/assets/images/ai4s-compound-c/slide-11.png' | relative_url }})

> 对图像多次裁剪识别，准确度更高；搜索相关文献能力更强，对反应理解更好；搜索了类似结构的 NMR，自己进行验证。
> **是否可重复？**

![ChatGPT pro细节]({{ '/assets/images/ai4s-compound-c/slide-12.png' | relative_url }})

### 专用小模型

> 需要精确的输入数据，更多起交叉验证的作用。
> —— SpecXMaster

三种输入方式的对比：只提供元素组成 / 提供元素组成和含有 Dipp 基团 / 提供元素组成和分子式。

![SpecXMaster]({{ '/assets/images/ai4s-compound-c/slide-13.png' | relative_url }})

---

## 05 ESI-MS 转化产物及其与 C 的关系

DeepSeek（识图）：只提供谱图和预测结构的情况下，DeepSeek 已经指出了可能的氧化情况。

$$234 + 16 + 23 = 273$$

确实符合得很好，我们认为理由比较充分，认为是合理的推断。

![ESI-MS 转化产物]({{ '/assets/images/ai4s-compound-c/slide-14.png' | relative_url }})

### 精确质量计算

| 离子 | DeepSeek 计算 | 实际所得 |
|------|--------------|---------|
| m/z 273 | 273.1379 | 273.1926 |
| m/z 280 | 280.1463 | 280.2651 |
| m/z 263 | 263.1564 | 263.1912 |

- DeepSeek：m/z 263 = 234 + 29，非常符合 +29 加合峰（乙基加合或乙醛加合）的特征
- 其他峰归类尝试

> 我们认为精确计算的结果较为充分地佐证了结构。

![精确质量计算]({{ '/assets/images/ai4s-compound-c/slide-15.png' | relative_url }})

---

## 06 最终结构假设、局限性与后续实验

局限性：

1. 有关 P 连接的 C 仍然不是很明确
2. 对质谱的峰只有少量有所分析
3. GPT pro 的工作具有普遍性吗？

![最终结构假设]({{ '/assets/images/ai4s-compound-c/slide-16.png' | relative_url }})

---

## 附：工具使用情况

### DeepSeek / NMRium / ChemDraw / MestReNova / 千问

- DeepSeek：质谱结果分析、核磁生成结构、结构预测核磁、预测乙二胺合硼烷、反应分析
- NMRium：NMRium demo – Predict 预测乙二胺、乙二胺硼烷、候选结构 C/H—NMR
- ChemDraw：预测乙二胺、乙二胺硼烷、候选结构 C/H—NMR
- MestReNova：提供核磁谱图，去杂质溶剂，源文件获得 500 Hz 信息
- 千问：反应产物可能结构分析（相对失败）

![工具使用 DeepSeek]({{ '/assets/images/ai4s-compound-c/slide-17.png' | relative_url }})

### Gemini / ChatGPT / SpecXMaster

- Gemini：全流程尝试、质谱分析
- ChatGPT：无质谱全流程分析、有质谱全流程分析
- SpecXMaster：核磁生成结构（多组输入）

![工具使用 Gemini]({{ '/assets/images/ai4s-compound-c/slide-18.png' | relative_url }})

---

![结束]({{ '/assets/images/ai4s-compound-c/slide-19.png' | relative_url }})

**谢谢，欢迎提问与讨论** 🔬
