# LLM Wiki 知识库

基于Andrej Karpathy的["LLM Wiki"模式](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)构建的个人知识库系统。

## 核心理念

与传统的RAG（检索增强生成）不同，LLM Wiki采用**持续构建和维护**的方式：
- LLM**增量构建和维护**一个持久化的wiki
- 知识被**编译一次并保持更新**，而不是每次查询重新推导
- Wiki是一个**持久化、复合增长的产物**

## 架构

```
.
├── CLAUDE.md          # 模式文件（操作指南）
├── raw_sources/      # 原始资料（不可变）
├── wiki/             # LLM生成的Markdown页面
│   ├── index.md      # 索引页面
│   ├── log.md        # 操作日志
│   ├── entities/     # 实体页面
│   ├── concepts/     # 概念页面
│   ├── summaries/    # 摘要页面
│   └── comparisons/  # 比较分析页面
└── README.md         # 本文件
```

## 快速开始

### 1. 初始化
确保目录结构完整。如果缺失，运行：
```bash
mkdir -p raw_sources/{articles,papers,images,notes}
mkdir -p wiki/{entities,concepts,summaries,comparisons}
```

### 2. 添加第一个资料
1. 将原始文档放入`raw_sources/`相应目录
2. 启动LLM代理（如Claude Code）
3. 要求LLM执行"录入"操作

### 3. 查询知识库
1. 向LLM提出问题
2. LLM会查阅wiki并综合回答
3. 有价值的回答可保存为新的wiki页面

### 4. 维护
定期要求LLM执行"lint"操作，检查wiki健康状态。

## 工作流程

### 录入 (Ingest)
- LLM读取新资料
- 创建摘要页面
- 更新相关实体/概念页面
- 更新索引和日志

### 查询 (Query)
- LLM通过索引查找相关页面
- 综合信息生成回答
- 可选：将回答保存为新页面

### 维护 (Lint)
- 查找矛盾或不一致
- 检查过时内容
- 识别缺失的概念
- 建议新方向

## 工具集成

### Obsidian
- 使用Obsidian打开`wiki/`目录作为vault
- 利用图视图查看知识连接
- 安装Dataview插件查询frontmatter

### 搜索工具
- 小型wiki：使用`grep`和`index.md`
- 大型wiki：集成[qmd](https://github.com/tobi/qmd)进行混合搜索

### Git
整个目录是一个Git仓库，提供版本控制和协作支持。

## 使用场景

- **个人知识管理**：记录学习笔记、健康数据、目标追踪
- **研究项目**：深入专题研究，整理文献和思路
- **读书笔记**：逐章构建书籍wiki，记录人物、情节、主题
- **团队协作**：维护团队内部wiki，集成会议记录、项目文档
- **竞争分析**：跟踪竞争对手信息，构建动态知识库

## 优势

- **复合增长**：每次操作都增加wiki价值
- **维护成本低**：LLM自动处理繁琐的整理工作
- **结构清晰**：信息高度组织化，便于检索
- **可扩展**：可根据需求调整结构和流程

## 开始你的LLM Wiki之旅

1. 阅读`CLAUDE.md`了解详细操作指南
2. 添加你的第一个原始资料
3. 与LLM协作构建你的个人知识库
4. 定期使用和维护，让知识持续增长

---

*灵感来源：Andrej Karpathy的"LLM Wiki"模式。人类负责思考方向，LLM负责知识整理。*