# LLM Wiki 知识库 - 操作指南

本文档定义了LLM Wiki知识库的结构、约定和工作流程。请遵循这些指南来维护wiki。

## 概述

这是一个基于LLM的个人知识库，采用三层架构：
1. **原始资料** (`raw_sources/`) - 原始文档、文章、图片等（不可变）
2. **Wiki** (`wiki/`) - LLM生成的Markdown文件（LLM全权维护）
3. **模式** (`CLAUDE.md`) - 本文件，定义wiki结构和操作规范

## 目录结构

```
~/yyswiki/
├── CLAUDE.md              # 模式文件（本文件）
├── raw_sources/          # 原始资料目录
│   ├── articles/         # 文章
│   ├── papers/          # 论文
│   ├── images/          # 图片
│   └── notes/           # 个人笔记
├── wiki/                # Wiki页面目录
│   ├── index.md         # 索引页面（所有页面的目录）
│   ├── log.md           # 操作日志（时间线）
│   ├── entities/        # 实体页面（人物、地点、概念等）
│   ├── concepts/        # 概念页面
│   ├── summaries/       # 摘要页面
│   └── comparisons/     # 比较分析页面
└── README.md            # 项目说明
```

## Wiki页面规范

### 页面类型

1. **实体页面** (`wiki/entities/`) - 具体的人、地点、事物、组织等
   - 文件名：`entity_name.md`（使用小写和下划线）
   - 包含：定义、属性、相关事件、与其他实体的关系

2. **概念页面** (`wiki/concepts/`) - 抽象概念、理论、方法等
   - 文件名：`concept_name.md`
   - 包含：定义、解释、应用场景、相关概念

3. **摘要页面** (`wiki/summaries/`) - 原始资料的摘要
   - 文件名：`source_title.md`（基于原始资料标题）
   - 包含：摘要、关键点、引用、相关实体/概念链接

4. **比较页面** (`wiki/comparisons/`) - 比较分析
   - 文件名：`topic_comparison.md`
   - 包含：比较维度、分析、结论

### 页面格式

每个Wiki页面应包含：

```markdown
---
title: 页面标题
type: [entity|concept|summary|comparison]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [相关原始资料]
tags: [标签1, 标签2]
---

# 页面标题

## 概述
简要介绍。

## 详细内容
结构化内容。

## 关系
- 链接到相关页面：[[entity_name]]
- 链接到概念：[[concept_name]]

## 引用
- 来自原始资料：[source_title](raw_sources/path/to/source.md)
```

## 工作流程

### 1. 资料录入 (Ingest)

当有新资料添加到`raw_sources/`时：
1. 读取并理解原始资料内容
2. 创建摘要页面到`wiki/summaries/`
3. 识别并更新相关实体和概念页面
4. 更新`wiki/index.md`
5. 在`wiki/log.md`中添加录入记录

录入记录格式：
```markdown
## [YYYY-MM-DD] ingest | 资料标题
- 摘要：创建了摘要页面`summary_name.md`
- 更新：更新了实体页面`entity1.md`、`entity2.md`
- 链接：添加了X个内部链接
```

### 2. 查询回答 (Query)

当用户提出问题时：
1. 首先查阅`wiki/index.md`找到相关页面
2. 阅读相关页面内容
3. 综合信息生成回答
4. 如果回答内容有价值，可创建新的Wiki页面
5. 在`wiki/log.md`中添加查询记录

查询记录格式：
```markdown
## [YYYY-MM-DD] query | 查询主题
- 查阅了X个页面：[[page1]]、[[page2]]
- 生成了回答/创建了新页面：`new_page.md`
```

### 3. 维护检查 (Lint)

定期检查Wiki健康状态：
1. 查找矛盾或不一致的内容
2. 检查过时声明
3. 查找孤立页面（无入链）
4. 识别重要但缺失的概念
5. 建议新的探索方向

## 索引和日志

### index.md
- 所有Wiki页面的目录
- 按类别组织（实体、概念、摘要、比较）
- 每个条目包含：链接、一句话摘要、最后更新日期
- LLM在每次录入时更新

### log.md
- 按时间顺序记录所有操作
- 使用统一前缀：`## [YYYY-MM-DD] 操作类型 | 描述`
- 便于使用grep查询：`grep "^## \[" log.md | tail -10`

## 工具和技巧

### 搜索
- 小型wiki：使用`index.md`和grep
- 大型wiki：考虑集成本地搜索工具如[qmd](https://github.com/tobi/qmd)

### Obsidian集成
- 使用Obsidian作为Wiki查看器
- 利用图视图查看连接关系
- 使用Dataview插件查询frontmatter

### 图片处理
- 将图片下载到`raw_sources/images/`
- 在Markdown中使用相对路径引用

## 开始使用

1. **初始化**：确保目录结构完整
2. **首次录入**：添加第一个原始资料并执行录入流程
3. **探索查询**：基于已有内容提出问题
4. **定期维护**：执行lint检查保持wiki健康

## 版本控制

Wiki目录是一个Git仓库，提供：
- 版本历史
- 变更追踪
- 协作支持

---

*本模式基于Andrej Karpathy的"LLM Wiki"概念设计。Wiki由LLM维护，人类负责指导方向、提出问题、思考意义。*