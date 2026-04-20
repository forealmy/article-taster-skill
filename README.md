# Article Taster - 文章品鉴师

> 帮助用户提前品尝文章可读性，过滤低质量内容，节省宝贵阅读时间

## 核心定位

**"文章含大质量检测"** —— 站在文章分析师的角度，多维度评估文章价值：

- 技术文章：衡量技术含量、学习价值
- AI生成检测：正确识别原创内容（古诗等不被误判）
- 情感散文：分析情感曲线、架构模式
- 小说：分析情节结构（但不剧透）

## 功能特性

### 智能类型识别

自动识别文章类型：技术文章、情感散文、小说或其他，精准匹配分析维度。

### 多维度质量评估

| 文章类型 | 评估维度 |
|----------|----------|
| 技术文章 | 技术深度、结构清晰度、实用性、原创性、可读性 |
| 散文/小说 | 情感表达、文笔水平、叙事结构、创意性、共鸣度 |
| 通用 | 内容深度、逻辑性、表达力、原创度 |

### AI味检测

识别常见AI生成特征：
- 段落长度高度一致（模板化）
- 废话率检测（空洞表达）
- 模板化程度（三段式、首先其次）
- 人类写作标记（personal voice）

**豁免规则**：古诗词、经典文学不受AI误判影响。

### 无剧透小说分析

分析小说情节结构、人物关系、阅读体验，但不会透露关键剧情。

## 快速开始

### 安装依赖

```bash
pip install -r requirements.txt
```

### 基本使用

```bash
# 分析单篇文章
python -m scripts.main analyze --text "文章内容..."

# 从文件分析
python -m scripts.main analyze --file article.txt --title "文章标题"

# 快速评分
python -m scripts.main quick --text "文章内容..."

# 指定文章类型
python -m scripts.main analyze --text "..." --type technical_article
```

### API 调用

```python
from scripts.main import ArticleTaster

taster = ArticleTaster()
report = taster.analyze(
    text="文章内容...",
    title="文章标题",
    output_format="markdown"  # 或 "json"
)

print(report["_markdown"])  # Markdown 格式
# 或
print(report)  # JSON 格式
```

## 输出示例

### Markdown 报告

```markdown
# 文章品鉴报告

## 基本信息
- **标题**: 技术架构设计原则
- **类型**: 技术文章 (置信度: 92%)
- **评分**: 85分 (A级)

## 综合评价
强烈推荐！内容扎实，适合认真阅读

**目标读者**: 初中级开发者
**预计阅读时间**: 10分钟

## 维度评分
| 维度 | 得分 | 权重 |
|------|------|------|
| 技术深度 | 88 | 30% |
| 结构清晰度 | 85 | 25% |
| 实用性 | 82 | 20% |
| 原创性 | 78 | 15% |
| 可读性 | 80 | 10% |

## AI检测结果
- **AI生成概率**: 15%
- **AI味评分**: 38 (轻度疑似AI)
- **结论**: 高度可信原创
```

## 评分等级

| 等级 | 分数 | 说明 |
|------|------|------|
| A+ | 90-100 | 极力推荐！值得反复研读 |
| A | 80-89 | 强烈推荐！内容扎实 |
| B+ | 70-79 | 推荐阅读，有价值 |
| B | 60-69 | 可读，碎片时间可看 |
| C | 40-59 | 一般，可选择性阅读 |
| D | <40 | 不推荐，浪费时间 |

## 项目结构

```
article-taster/
├── SKILL.md                    # 技能定义文件
├── CLAUDE.md                   # 开发指南
├── README.md                   # 本文件
├── requirements.txt            # 依赖
├── scripts/
│   ├── main.py                 # 主入口
│   ├── article_classifier.py   # M1: 类型识别
│   ├── tech_analyzer.py        # M2-T: 技术文章分析
│   ├── creative_analyzer.py    # M2-C: 情感散文分析
│   ├── novel_analyzer.py       # M2-N: 小说分析 (无剧透)
│   ├── ai_detector.py          # M3: AI生成检测
│   ├── scorer.py               # M4: 综合评分
│   └── report_generator.py     # 报告生成
└── config/
    ├── scoring_weights.json    # 评分权重
    ├── ai_patterns.json         # AI检测模式
    └── exemption_rules.json     # 原创豁免规则
```

## 依赖项

- Python 3.10+
- jieba (中文分词)
- scikit-learn (文本相似度)
- numpy (数值计算)

## License

MIT
