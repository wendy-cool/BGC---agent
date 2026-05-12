# 🕵️ Beijing Agent — 大客户销售背调助手

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

大客户销售拜访前的公司背调 Agent。输入目标公司名称，自动完成公司画像、经营分析、行业定位，生成结构化的会客策略报告。

## 两种模式

| 模式 | 命令 | 内容 | 耗时 |
|------|------|------|------|
| 快速 | `/beijing 公司名` | 公司画像、经营分析、竞争定位、会客策略 | ~5 min |
| 深度 | `/beijing --deep 公司名` | 快速模式 + 组织架构、关键人物地图、决策人画像、需求推断 | ~20 min |
| 产品匹配 | `/beijing 公司名 --product "你的产品"` | 深度模式 + 4 维度产品匹配度评估 + 竞品替代策略 | ~25 min |

## 快速开始

### 1. 安装

将 `SKILL.md` 放入 Claude Code 的 skills 目录：

```bash
mkdir -p ~/.claude/skills/beijing
cp SKILL.md ~/.claude/skills/beijing/
```

### 2. 使用

```
/beijing 字节跳动               # 快速背调，生成 5 分钟速览报告
/beijing --deep 帆软            # 深度背调，含组织架构和决策人画像
/beijing 阿里云 --product "CRM"  # 指定产品，含匹配度评估和切入策略
```

### 3. 输出

报告自动保存至桌面：
- 快速模式 → `~/Desktop/{公司名}_背调报告_快速_{日期}.md`
- 深度模式 → `~/Desktop/{公司名}_背调报告_深度_{日期}.md`

### 4. 打印报告

将 .md 报告转为打印优化的 HTML，自动在浏览器中打开，直接 Cmd+P 即可打印：

```bash
cd ~/projects/beijing-agent
npm install
node scripts/to-print.js ~/Desktop/帆软_背调报告_深度_20260512.md
```

打印版特点：A4 纸张适配、中文排版优化、表格防断页、顶部一键打印按钮

## 示例输出

| 公司 | 模式 | 报告 |
|------|------|------|
| 帆软软件 | 快速 | [帆软_背调报告_快速_20260512.md](examples/fanruan-deep-report-20260512.md) |

## 核心设计

### 销售视角翻译（最重要）

不是写公司百科，而是帮销售找到沟通切入点。每个事实都回答："这条信息对销售有什么用？"

### 并行搜索架构

```
第 1 轮：快速画像（5 个搜索并行）
  └── 公司画像、主营产品、最新新闻、融资财报、招聘规模

第 2 轮：行业竞争（3 个搜索并行）
  └── 市场格局、合作伙伴、行业趋势

第 3 轮：深度信息（4 个搜索并行，仅 --deep）
  └── 组织架构、创始人战略、新业务动态、风险排查
```

### 信息可信度标记

🔴 官方 | 🟡 第三方 | 🟠 推测 | ⚪ 待核实

### 产品匹配度框架

指定 `--product` 后，从 4 个维度评估匹配度：需求、预算、时机、关系。如检测到竞品，额外输出替代策略。

## 项目结构

```
beijing-agent/
├── LICENSE
├── README.md
├── CHANGELOG.md
├── SKILL.md              # 核心 Agent prompt
├── package.json
├── scripts/
│   └── to-print.js       # 报告转打印 HTML
└── examples/
    └── fanruan-deep-report-20260512.md
```

## 版本

当前版本：**v2.1** — 参见 [CHANGELOG.md](CHANGELOG.md)
