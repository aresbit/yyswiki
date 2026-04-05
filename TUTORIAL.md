# LLM Wiki 知识库完整使用教程

## 概述

本教程将指导你如何使用基于Andrej Karpathy"LLM Wiki"模式构建的个人知识库系统。通过本教程，你将学会如何初始化知识库、录入资料、查询知识、维护wiki，以及集成各种工具。

## 目录

1. [系统架构](#系统架构)
2. [环境准备](#环境准备)
3. [快速开始](#快速开始)
4. [核心操作详解](#核心操作详解)
   - [资料录入](#资料录入)
   - [知识查询](#知识查询)
   - [维护检查](#维护检查)
5. [实际工作流程示例](#实际工作流程示例)
6. [工具集成指南](#工具集成指南)
   - [Obsidian集成](#obsidian集成)
   - [搜索工具配置](#搜索工具配置)
   - [Git版本控制](#git版本控制)
7. [最佳实践](#最佳实践)
8. [故障排除](#故障排除)
9. [进阶技巧](#进阶技巧)

## 系统架构

### 三层结构
```
原始资料层 (raw_sources/)    # 你的原始文档（不可变）
       ↓
   Wiki层 (wiki/)           # LLM生成的Markdown页面（LLM维护）
       ↓
   模式层 (CLAUDE.md)       # 操作指南和规范
```

### 目录结构详解
```
~/yyswiki/
├── CLAUDE.md              # 核心模式文件，指导LLM操作
├── README.md              # 项目说明
├── TUTORIAL.md            # 本教程文件
├── raw_sources/          # 原始资料库
│   ├── articles/         # 文章（.md, .pdf, .txt等）
│   ├── papers/           # 学术论文
│   ├── images/           # 图片资源
│   └── notes/            # 个人笔记
└── wiki/                 # LLM生成的Wiki
    ├── index.md          # 所有页面的索引目录
    ├── log.md            # 操作时间线日志
    ├── entities/         # 实体：人物、地点、组织等
    ├── concepts/         # 概念：理论、方法、框架等
    ├── summaries/        # 原始资料摘要
    └── comparisons/      # 比较分析页面
```

## 环境准备

### 1. 确保目录结构完整
```bash
# 进入yyswiki目录
cd ~/yyswiki

# 检查目录结构
ls -la
```

如果目录缺失，可以手动创建：
```bash
mkdir -p raw_sources/{articles,papers,images,notes}
mkdir -p wiki/{entities,concepts,summaries,comparisons}
```

### 2. 准备LLM代理
你可以使用以下任一LLM代理：
- **Claude Code**（推荐）：`claude code`或通过IDE扩展
- **OpenAI Codex**：通过API调用
- **其他支持文件操作的LLM代理**

### 3. 可选工具安装
- **Obsidian**：Wiki可视化查看器
- **qmd**：本地搜索工具（大型wiki时推荐）
- **Git**：版本控制

## 快速开始

### 第一步：了解核心文件
1. 阅读`CLAUDE.md` - 了解操作规范
2. 阅读`README.md` - 了解项目概述
3. 浏览`wiki/index.md` - 查看当前知识库状态

### 第二步：添加第一个资料
1. 将你的文档复制到`raw_sources/`相应目录
2. 启动LLM代理（如Claude Code）
3. 向LLM发出指令："请根据CLAUDE.md的指南，处理raw_sources/articles/中的新资料"

### 第三步：查看结果
1. 检查`wiki/summaries/` - 查看生成的摘要
2. 检查`wiki/index.md` - 查看更新的索引
3. 检查`wiki/log.md` - 查看操作记录

## 核心操作详解

### 资料录入 (Ingest)

#### 基本流程
1. **LLM读取原始资料**
   ```markdown
   请读取并分析 raw_sources/articles/机器学习简介.md
   ```

2. **创建摘要页面**
   - 位置：`wiki/summaries/机器学习简介.md`
   - 内容：关键点总结、重要概念、引用

3. **识别和更新实体/概念**
   - 识别资料中提到的实体（如"深度学习"）
   - 更新或创建相关页面

4. **更新索引和日志**
   - 更新`wiki/index.md`
   - 在`wiki/log.md`中添加记录

#### 完整指令示例
```markdown
请执行资料录入操作：
1. 处理 raw_sources/articles/机器学习简介.md
2. 按照CLAUDE.md中的录入流程
3. 创建摘要页面
4. 识别相关实体和概念并更新相应页面
5. 更新index.md和log.md
```

### 知识查询 (Query)

#### 基本流程
1. **LLM查阅索引**
   - 首先读取`wiki/index.md`找到相关页面

2. **综合回答**
   - 读取相关页面内容
   - 综合信息生成回答

3. **可选：创建新页面**
   - 如果回答内容有价值，可保存为新wiki页面

#### 查询类型示例
1. **简单查询**
   ```markdown
   什么是机器学习的主要类型？
   ```

2. **综合查询**
   ```markdown
   比较监督学习和无监督学习的异同点，并举例说明。
   ```

3. **探索性查询**
   ```markdown
   基于当前wiki内容，机器学习领域还有哪些重要概念需要进一步了解？
   ```

#### 完整指令示例
```markdown
请回答以下问题：
1. 使用wiki/index.md查找相关信息
2. 综合相关页面内容
3. 如果需要，可以创建新的比较分析页面
4. 在log.md中记录此次查询

问题：机器学习中的过拟合和欠拟合有什么区别？如何避免？
```

### 维护检查 (Lint)

#### 检查内容
1. **一致性检查**
   - 查找矛盾或不一致的内容
   - 检查过时的声明

2. **完整性检查**
   - 查找孤立页面（无入链）
   - 识别重要但缺失的概念

3. **结构优化**
   - 建议新的分类或标签
   - 优化页面组织结构

#### 完整指令示例
```markdown
请执行wiki维护检查：
1. 检查所有页面的一致性和完整性
2. 查找矛盾或过时的内容
3. 识别孤立页面和缺失的重要概念
4. 在log.md中记录检查结果和建议
```

## 实际工作流程示例

### 场景一：学习新主题

假设你想学习"深度学习"：

1. **收集资料**
   ```bash
   # 将相关资料放入raw_sources
   cp ~/Downloads/深度学习论文.pdf ~/yyswiki/raw_sources/papers/
   cp ~/Downloads/深度学习教程.md ~/yyswiki/raw_sources/articles/
   ```

2. **批量录入**
   ```markdown
   请处理raw_sources/papers/和raw_sources/articles/中所有关于深度学习的资料，
   按照CLAUDE.md的录入流程创建wiki页面。
   ```

3. **探索查询**
   ```markdown
   基于wiki中的深度学习内容，回答：
   1. 深度学习与传统机器学习的核心区别是什么？
   2. 列出深度学习的三个主要应用领域
   3. 创建"深度学习应用比较"页面到wiki/comparisons/
   ```

4. **知识扩展**
   ```markdown
   请检查深度学习相关的wiki页面，
   建议还需要了解哪些相关概念，并创建学习路线图。
   ```

### 场景二：项目管理

假设你在管理一个软件开发项目：

1. **录入项目文档**
   ```markdown
   请处理以下项目资料：
   - raw_sources/notes/需求文档.md
   - raw_sources/notes/技术方案.md
   - raw_sources/notes/会议记录.md
   
   创建项目相关的实体页面（如功能模块、技术选型等）。
   ```

2. **查询项目状态**
   ```markdown
   基于项目wiki，回答：
   1. 项目的关键需求有哪些？
   2. 技术方案中的主要决策是什么？
   3. 有哪些待解决的问题？
   ```

3. **维护项目wiki**
   ```markdown
   每周执行一次项目wiki维护检查，
   确保所有信息都是最新的。
   ```

### 场景三：读书笔记

假设你在读《人类简史》：

1. **逐章录入**
   ```markdown
   我刚刚读完《人类简史》第一章，
   请根据我的笔记raw_sources/notes/人类简史_第一章.md
   创建相关的wiki页面：
   - 摘要页面
   - 关键概念页面（认知革命、八卦理论等）
   - 更新索引和日志
   ```

2. **建立连接**
   ```markdown
   请连接《人类简史》的概念与我之前录入的
   《枪炮、病菌与钢铁》的相关概念。
   ```

3. **深度分析**
   ```markdown
   基于整本书的wiki内容，
   分析"认知革命"对人类历史的影响，
   并创建深度分析页面。
   ```

## 工具集成指南

### Obsidian集成

#### 1. 设置Obsidian Vault
```bash
# 方法一：直接打开wiki目录
obsidian ~/yyswiki/wiki

# 方法二：创建符号链接（如果Obsidian需要特定位置）
ln -s ~/yyswiki/wiki ~/ObsidianVaults/LLM_Wiki
```

#### 2. 推荐插件
1. **Dataview**：查询frontmatter数据
   ```markdown
   ```dataview
   TABLE created, updated, tags
   FROM "concepts"
   WHERE contains(type, "concept")
   SORT updated DESC
   ```
   ```

2. **Graph View**：可视化知识连接
3. **Marp**：从wiki生成演示文稿
4. **Templater**：创建页面模板

#### 3. 工作流优化
- 左侧：Obsidian查看wiki
- 右侧：LLM代理操作wiki
- 实时同步查看更新

### 搜索工具配置

#### 小型wiki（<100页面）
使用内置工具：
```bash
# 搜索关键词
grep -r "机器学习" ~/yyswiki/wiki/

# 查找最近更新的页面
grep "updated:" ~/yyswiki/wiki/**/*.md | sort -r

# 使用index.md导航
cat ~/yyswiki/wiki/index.md | grep -A2 -B2 "实体"
```

#### 大型wiki（>100页面）
安装qmd：
```bash
# 安装qmd
cargo install --git https://github.com/tobi/qmd

# 初始化索引
qmd index ~/yyswiki/wiki/

# 搜索
qmd search "机器学习"

# LLM集成
# 在CLAUDE.md中添加qmd搜索指令
```

### Git版本控制

#### 1. 初始化Git仓库
```bash
cd ~/yyswiki
git init
git add .
git commit -m "初始提交：LLM Wiki知识库"
```

#### 2. 创建.gitignore
```markdown
# 临时文件
*.tmp
*.swp

# 大型文件
raw_sources/**/*.pdf
raw_sources/**/*.mp4

# 系统文件
.DS_Store
Thumbs.db
```

#### 3. 自动化提交
创建提交脚本：
```bash
#!/bin/bash
cd ~/yyswiki
git add .
git commit -m "自动提交: $(date '+%Y-%m-%d %H:%M')"
```

## 最佳实践

### 1. 命名规范
- **文件命名**：使用小写字母和下划线（`machine_learning.md`）
- **页面标题**：使用清晰的中文描述
- **标签系统**：保持一致性，避免同义词

### 2. 质量控制
- **逐步录入**：每次处理少量资料，确保质量
- **定期审查**：每周检查wiki健康状况
- **版本备份**：重要变更前创建Git分支

### 3. 效率技巧
- **批量操作**：相似资料可以批量处理
- **模板使用**：创建常用页面模板
- **自动化脚本**：常用操作编写脚本

### 4. 知识组织
- **层级结构**：避免过深的目录嵌套
- **交叉链接**：积极创建页面间链接
- **分类合理**：定期调整分类系统

## 故障排除

### 常见问题

#### 1. LLM不遵循CLAUDE.md
**症状**：LLM忽略操作指南
**解决**：
```markdown
请严格按照CLAUDE.md中的指南操作，
特别是第3节的工作流程和第4节的页面格式。
```

#### 2. 索引更新不及时
**症状**：新页面未出现在index.md中
**解决**：
```markdown
请手动更新wiki/index.md，
确保所有新页面都被列出。
```

#### 3. 页面孤立
**症状**：页面没有入链或出链
**解决**：
```markdown
请执行lint操作，
查找孤立页面并为其添加相关链接。
```

#### 4. 原始资料格式问题
**症状**：LLM无法正确解析某些文件
**解决**：
- 转换为纯文本或Markdown格式
- 提供文件格式说明给LLM
- 分段处理大型文件

### 调试步骤
1. 检查`wiki/log.md`最近操作
2. 验证目录结构和文件权限
3. 简化指令测试基本功能
4. 逐步增加复杂度

## 进阶技巧

### 1. 自定义页面模板
在`CLAUDE.md`中添加模板：
```markdown
### 实体页面模板
---
title: {{实体名称}}
type: entity
created: {{日期}}
updated: {{日期}}
sources: []
tags: []
---

# {{实体名称}}

## 概述

## 详细属性

## 相关关系

## 引用来源
```

### 2. 自动化工作流
创建自动化脚本：
```bash
#!/bin/bash
# auto_ingest.sh
# 自动处理新资料

NEW_FILES=$(find ~/yyswiki/raw_sources -name "*.md" -mtime -1)

for file in $NEW_FILES; do
    echo "处理: $file"
    # 调用LLM API处理文件
done
```

### 3. 多领域知识库
创建多个wiki实例：
```
~/knowledge/
├── tech_wiki/      # 技术知识库
├── research_wiki/  # 研究知识库
└── personal_wiki/  # 个人知识库
```

### 4. 团队协作
- 使用Git分支管理不同贡献
- 设立审核流程（人类审核LLM更新）
- 创建团队专属的CLAUDE.md变体

### 5. 集成外部工具
- **文献管理**：Zotero + Markdown导出
- **笔记应用**：Logseq/Obsidian双向链接
- **任务管理**：集成待办事项到wiki

## 持续学习与改进

### 1. 迭代CLAUDE.md
随着使用经验积累，不断优化CLAUDE.md：
- 添加新的工作流程
- 优化页面格式
- 调整分类系统

### 2. 社区分享
- 分享你的CLAUDE.md变体
- 贡献新的工具集成方案
- 交流最佳实践

### 3. 跟踪进展
定期回顾：
- 知识库增长统计
- 使用频率分析
- 价值评估

## 总结

LLM Wiki知识库是一个强大的个人知识管理系统，它将LLM的自动化能力与人类的方向指导相结合。通过本教程，你应该能够：

1. ✅ 理解LLM Wiki的架构和工作原理
2. ✅ 掌握资料录入、查询和维护的核心操作
3. ✅ 集成Obsidian、搜索工具等外部工具
4. ✅ 应用最佳实践提高知识库质量
5. ✅ 解决常见问题并应用进阶技巧

### 下一步行动建议

1. **立即开始**：添加你的第一个资料并执行录入
2. **定期使用**：将LLM Wiki融入日常工作流
3. **持续优化**：根据使用经验调整CLAUDE.md
4. **分享反馈**：贡献你的改进建议

### 资源链接

- [原始LLM Wiki概念](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- [Obsidian官网](https://obsidian.md)
- [qmd搜索工具](https://github.com/tobi/qmd)
- [示例项目仓库](https://github.com/yourusername/llm-wiki-template)

---

*祝你构建出属于自己的强大知识库！*

**更新记录**：
- 2026-04-05：创建初始教程
- 更多更新请查看`wiki/log.md`

**反馈建议**：欢迎通过GitHub Issues或直接编辑本文件提供改进建议。