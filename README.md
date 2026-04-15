# 🧠 LLM Wiki — OpenClaw Agent Skill

> **为 AI Agent 设计的永久记忆系统**
> Permanent memory system for AI Agents, based on Karpathy LLM Wiki pattern.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-Compatible-00d4ff)](https://openclaw.ai)
[![Language](https://img.shields.io/badge/language-中文%20%2F%20EN-brightgreen)]()

---

## 🤔 为什么需要它 / Why

AI 每次对话都会失忆。

普通的 context window 撑不住几百页文档。RAG 向量检索对小团队来说太重了。

**LLM Wiki 的答案很简单**：把内容预先编译成结构化知识页面，用自然语言直接查询。

> "我们出的书第12章有什么经典语录？"
> → 龙虾秒级返回原文引用，不需要翻文件夹，不需要问老员工。

这是为 [OpenClaw](https://openclaw.ai) Agent 设计的 Skill，基于 [Andrej Karpathy 的 LLM Wiki 理念](https://x.com/karpathy)。

---

## ✨ 核心特性 / Features

- 📥 **多格式导入**：网页 URL / 飞书文档 / PDF / 纯文字，一句话存入
- 🗂️ **双层存储**：`raw/` 原文永久保留 + `wiki/` 结构化提炼，两者互不干扰
- 🔗 **交叉引用**：自动生成 `[[wikilinks]]`，知识页面互相关联
- 🔍 **自然语言查询**：不需要关键词，直接问问题，精准返回
- 🩺 **健康检查**：自动扫描死链、孤立页面、内容矛盾
- 📊 **操作日志**：完整 append-only 操作记录

---

## 🗂️ 存储结构 / Storage

```
{workspace}/.llm-wiki/
├── raw/          ← 原始内容（只写入一次，永不修改）
├── wiki/         ← 结构化知识页面（Agent 维护）
│   ├── overview.md
│   ├── source-<name>.md
│   ├── entity-<name>.md
│   ├── concept-<name>.md
│   └── synthesis-<topic>.md
├── index.md      ← 目录索引
└── log.md        ← 操作日志
```

**铁律：**
- `raw/` 内容只写入一次，永不修改 — 原始数据一字不丢
- 查询只读 `wiki/`，不直接读 `raw/` — 保证响应速度

---

## 🚀 快速开始 / Quick Start

### 1. 安装 Skill

把本仓库 clone 到你的 OpenClaw workspace 的 skills 目录：

```bash
cd ~/.openclaw/workspace/skills
git clone https://github.com/KKirikaze/llm-wiki-openclaw.git llm-wiki
```

### 2. 初始化知识库

在 OpenClaw 对话框中输入：

```
建立知识库
```

### 3. 存入内容

```
把这个存进知识库：https://example.com/article
把这段话记住：[你的内容]
帮我学习这篇飞书文档：https://xxx.feishu.cn/docx/xxx
```

### 4. 查询

```
从wiki里找找：AI落地有哪些常见坑？
用我的知识库回答：并购整合最难的是什么？
```

### 5. 其他命令

```
知识库现在有什么      # 查看状态
整理一下知识库        # 健康检查
查看知识库日志        # 操作记录
wiki怎么用            # 帮助说明
```

---

## ⚖️ LLM Wiki vs RAG 对比 / Comparison

| | LLM Wiki（本项目）| 企业级 RAG |
|--|--|--|
| **适用规模** | ≤ 40万字（~5-10万token）| 无上限 |
| **检索方式** | 结构化知识页面 + 语义理解 | 向量相似度 |
| **可读性** | 人可直接阅读和编辑 | 机器友好，人不友好 |
| **部署复杂度** | 零依赖，纯文件 | 需要向量库（Chroma等）|
| **知识积累** | 越用越强，交叉引用自动增长 | 静态索引，需重建 |
| **适合场景** | 个人/小团队知识沉淀 | 企业大规模语料检索 |

> 💡 两者不互斥：小团队用 LLM Wiki，数据量超过 40 万字后迁移到 RAG 架构。

---

## 📄 Wiki 页面规范 / Page Spec

所有 `wiki/` 页面必须包含 YAML frontmatter：

```yaml
---
type: source | entity | concept | synthesis
title: 页面标题
date: YYYY-MM-DD
tags: [标签1, 标签2]
source: raw/源文件名.md   # 仅 source 类型需要
---
```

页面分类：
- `source-<name>.md` — 源文件摘要
- `entity-<name>.md` — 实体（人、组织、产品等）
- `concept-<name>.md` — 概念（技术、方法、原理等）
- `synthesis-<topic>.md` — 综合分析（跨源洞察）

---

## 🗺️ Roadmap

- [x] 基础 CRUD（INIT / INGEST / QUERY / STATUS）
- [x] 健康检查（LINT）
- [x] 操作日志（LOG）
- [x] 支持飞书文档导入
- [x] 支持 PDF 导入
- [ ] 批量导入优化（大文件分块）
- [ ] 知识库导出（Markdown zip）
- [ ] 多知识库支持（按项目隔离）
- [ ] Web UI 预览

---

## 🤝 Contributing

欢迎 PR 和 Issue！

本项目由 [李佩琪 (Artemis)](https://github.com/KKirikaze) 与 OpenClaw AI 团队共同开发。

---

## 📜 License

MIT License — 自由使用，欢迎 star ⭐

---

## 🙏 致谢 / Credits

- [Andrej Karpathy](https://x.com/karpathy) — LLM Wiki 原始理念
- [OpenClaw](https://openclaw.ai) — Agent 运行环境
- 傅盛 × AI 团队 — 在真实企业场景中验证和打磨
