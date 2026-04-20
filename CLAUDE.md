# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

`article-taster` 是一个**文章品鉴师**技能，用于多维度评估文章质量、检测AI生成内容、识别原创内容，并提供阅读建议。

## 常用命令

```bash
# 安装依赖
pip install -r requirements.txt

# 分析单篇文章（Markdown输出）
python -m scripts.main analyze --text "文章内容..."

# 分析文章（JSON输出）
python -m scripts.main analyze --text "文章内容..." --output json

# 快速评分
python -m scripts.main quick --text "文章内容..."

# 从文件分析
python -m scripts.main analyze --file article.txt --title "文章标题"
```

## 架构设计

### 核心工作流 (4阶段)

```
M1: ArticleClassifier        # 类型识别
     ↓
M2: Type-specific Analyzer   # 类型专业分析
     ├─ TechAnalyzer (技术文章)
     ├─ CreativeAnalyzer (散文/情感)
     └─ NovelAnalyzer (小说,无剧透)
     ↓
M3: AIDetector               # AI生成检测
     ↓
M4: QualityScorer            # 综合评分
     ↓
ReportGenerator              # 报告生成
```

### 评分体系

- **技术文章**: 技术深度(30%)、结构清晰度(25%)、实用性(20%)、原创性(15%)、可读性(10%)
- **散文/小说**: 情感表达(30%)、文笔水平(25%)、叙事结构(20%)、创意性(15%)、共鸣度(10%)
- **AI检测**: 文本统计、困惑度、词汇丰富度、风格一致性、段落一致性、模板化程度
- **综合评分**: `最终评分 = 加权得分 × 类型匹配度 × (1 - AI概率 × 0.3)`

### 等级划分

| 等级 | 分数 | 建议 |
|------|------|------|
| A+ | 90-100 | 极力推荐 |
| A | 80-89 | 强烈推荐 |
| B+ | 70-79 | 推荐阅读 |
| B | 60-69 | 碎片时间可看 |
| C | 40-59 | 选择性阅读 |
| D | <40 | 不推荐 |

## 关键模块

| 文件 | 职责 |
|------|------|
| `main.py` | ArticleTaster 主类， orchestrate 全流程 |
| `article_classifier.py` | M1 类型识别 (技术文章/散文/小说/其他) |
| `tech_analyzer.py` | M2-T 技术文章分析 |
| `creative_analyzer.py` | M2-C 情感散文分析 |
| `novel_analyzer.py` | M2-N 小说分析 (无剧透) |
| `ai_detector.py` | M3 AI生成检测与原创识别 |
| `scorer.py` | M4 综合评分计算 |
| `report_generator.py` | JSON/Markdown 报告生成 |

## 配置说明

`config/` 目录包含评分权重和检测规则:
- `scoring_weights.json` - 评分权重配置
- `ai_patterns.json` - AI特征模式
- `exemption_rules.json` - 原创内容豁免规则 (古诗词等)
