# paper-reading · 科研论文精读与笔记 Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Skill](https://img.shields.io/badge/type-agent--skill-green.svg)]()
[![语言](https://img.shields.io/badge/语言-中文-important.svg)]()

一个教你 AI 助手「怎么读论文」的 agent skill。装好后，丢给它一篇论文（PDF / arXiv 链接 / 贴文本），它会按**三遍读论文法**精读，产出一份结构化 Markdown 笔记，并自动维护你的文献笔记库索引。

> 核心理念：笔记不是摘要的改写，而是回答三个问题——**它做了什么、它对我意味着什么、我能拿走什么**。

## 它解决什么问题

- **读完就忘**：普通的 AI 论文总结是「摘要的复述」，看完和没看一样。这个 skill 的笔记结构强制包含「亮点与启发」「局限与质疑」「与我的研究的相关性」——写下来的东西将来真的用得上。
- **没有批判性**：skill 内置审稿人视角的第三遍阅读，专门挑实验设计、隐含假设、泛化性的毛病。
- **文献散落各处**：笔记统一落在 `literature-notes/` 目录，自动更新 `index.md` 索引，读多篇时主动生成对比表，组会汇报和写 related work 直接从索引出发。

## 效果示例

完整示例见 [examples/vaswani-2017-attention.md](examples/vaswani-2017-attention.md)（Transformer 论文的精读笔记），节选：

> - **一句话总结**：完全抛弃 RNN 和 CNN、只靠自注意力与前馈网络堆叠的 Transformer 架构，在机器翻译上同时拿到当时的最高精度和快一个量级的训练速度……
>
> ## 局限与质疑
> - 注意力计算量随序列长度**平方**增长，长上下文的成本瓶颈……

## 安装

把仓库克隆到你的用户级 skills 目录（ZCode 及任何支持 `.agents/skills/` 约定的 agent 工具都适用）：

```bash
# macOS / Linux / Windows Git Bash
git clone https://github.com/paratrooper-vincent/paper-reading.git ~/.agents/skills/paper-reading
```

```powershell
# Windows PowerShell
git clone https://github.com/paratrooper-vincent/paper-reading.git "$HOME\.agents\skills\paper-reading"
```

克隆完重启 / 新开会话即可生效，无需其他配置。

<details>
<summary>装到单个项目里（仅该项目可用）</summary>

```bash
cd 你的项目
git clone https://github.com/paratrooper-vincent/paper-reading.git .agents/skills/paper-reading
```
</details>

## 使用

装好后正常说话即可触发，比如：

| 你说 | 它做 |
|---|---|
| 「精读这篇论文并做笔记 `D:/papers/xxx.pdf`」 | 三遍精读，笔记存到 `literature-notes/`，更新索引 |
| 「帮我看看这篇值不值得读 https://arxiv.org/abs/1706.03762」 | 五分钟速读，给出 读 / 跳过 / 以后读 建议 |
| 「筛一下这批文献，只留值得精读的」 | 逐篇第一遍，输出筛选清单 |
| 「把 literature-notes 里的高效注意力笔记做个对比表」 | 生成多维对比表，追加到 `index.md` |
| 「组会要汇报这篇，帮我准备 15 分钟的讲法」 | 基于笔记给出汇报结构 |

## 它是如何工作的

skill 教给模型的是一套完整工作流：

1. **第一遍（5 分钟）**——只读标题、摘要、引言、结论，回答「类型 / 背景 / 正确性 / 贡献 / 清晰度」，先判断值不值得花时间。
2. **第二遍（理解）**——通读正文、吃透图表，目标是能向同行三分钟讲清这篇论文。
3. **第三遍（审稿人视角）**——质疑实验设置、消融归因、隐含假设、泛化性。

产出按固定结构落盘（元信息 / 一句话总结 / 研究问题 / 方法 / 实验 / 亮点 / 局限 / 与我何干 / 术语金句），文件名 `第一作者姓氏-年份-关键词.md`。

## 目录结构

```
paper-reading/
├── SKILL.md                        # skill 本体：工作流 + 笔记规范
├── assets/
│   └── paper-note-template.md      # 空白笔记模板
├── examples/
│   └── vaswani-2017-attention.md   # 真实成品示例（Transformer 论文）
└── references/
    └── reading-guide.md            # 深入指南：五类论文策略 / 常见坑 / 对比阅读
```

## 自定义

- **改笔记结构**：编辑 [assets/paper-note-template.md](assets/paper-note-template.md) 和 `SKILL.md` 中的「笔记格式」一节（两处保持一致）。
- **改笔记目录约定**：默认 `literature-notes/`，在 `SKILL.md` 的「笔记文件的组织」一节修改。
- **适配英文输出**：把 `SKILL.md` 的「笔记格式」换成英文标题即可，工作流不用动。

## 致谢

- 阅读方法论来自 S. Keshav 的经典短文 *"How to Read a Paper"* (SIGCOMM CCR, 2007)。
- 笔记结构设计参考了众多科研工作者的文献笔记实践。

## 许可

[MIT](LICENSE) © 2026 paratrooper-vincent

---

## English

**paper-reading** is an agent skill that teaches your AI assistant *how* to read academic papers. Give it a paper (PDF / arXiv link / pasted text) and it performs a three-pass reading (based on S. Keshav's classic *How to Read a Paper*): a 5-minute triage, a full comprehension pass, and a critical reviewer-style pass — then produces a structured Markdown note (motivation / method / experiments / insights / limitations / relevance-to-you) in a `literature-notes/` folder, keeping an `index.md` and comparison tables up to date.

**Install** (works with ZCode and any agent that discovers skills in `.agents/skills/`):

```bash
git clone https://github.com/paratrooper-vincent/paper-reading.git ~/.agents/skills/paper-reading
```

**Example** — see [examples/vaswani-2017-attention.md](examples/vaswani-2017-attention.md). The skill instructions are written in Chinese; the note structure is trivially portable to English (see 自定义 above).

**License** — [MIT](LICENSE) © 2026 paratrooper-vincent
