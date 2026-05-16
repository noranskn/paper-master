# 📄 Paper Master

> AI-powered academic paper reading assistant — extract structured notes from PDFs into an Excel master table.

**Paper Master** 是一个 Claude Code Skill，将 PDF 研究论文自动提取为结构化笔记，追加到 Excel 总表中。调用 PyMuPDF 进行 PDF 文本提取，Claude Agent 进行智能论文理解，openpyxl 进行表格写入。

---

## ✨ 提取字段 / Extracted Fields

每篇论文提取以下 10 个字段，按顺序写入 Excel 第一张表：

| # | 字段 | 说明 |
|---|------|------|
| 1 | **标题** | 英文标题 + 中文翻译（换行分隔） |
| 2 | **作者（机构/学校）** | 仅机构名，不写个人姓名 |
| 3 | **发表年份** | YYYY.MM 格式 |
| 4 | **期刊/会议** | 全名 + IF/分区/CCF 等级 |
| 5 | **摘要** | 问题背景 / 解决方法 / 达到目标 三要素 |
| 6 | **研究方法&内容** | 方法细节 + 达成目的 |
| 7 | **实验** | 工具 / 数据集 / 评价指标 |
| 8 | **相关工作** | 3~6 条，含差异/局限分析 |
| 9 | **自我思考** | 创新点 / 优势 / 局限 / 启发 |
| 10 | **有无代码** | 代码链接或标注无 |

---

## 🚀 快速开始 / Quick Start

### 1. 安装依赖

```bash
pip install PyMuPDF openpyxl
```

### 2. 在 Claude Code 中安装本 Skill

将本仓库克隆到本地，在 Claude Code 中加载为插件：

```
git clone https://github.com/noranskn/paper-master.git
```

### 3. 使用

在 Claude Code 对话中：

```
/paper-to-master-table 帮我把 reference/my-paper.pdf 论文信息总结到 E:\论文\SUMMARY.xlsx 第一张表
```

Claude Agent 会：
1. 用 PyMuPDF 提取 PDF 文本 → 保存同名 `.txt` 到 reference 目录
2. 阅读论文内容，按 10 个字段生成结构化笔记
3. 调用 `append_summary_row.py` 追加到 Excel 总表最后一行

---

## 🧠 工作原理 / How It Works

```
PDF 论文
  │
  ▼ PyMuPDF (fitz) 提取文本
.txt 文件
  │
  ▼ Claude Agent 阅读 & 理解
结构化 JSON 数据
  │
  ▼ append_summary_row.py (openpyxl)
Excel 总表 (SUMMARY.xlsx)
```

- **PyMuPDF**: 快速、可靠的 PDF 文本提取，处理中英文混排
- **Claude Agent**: 理解论文学术内容，区分摘要/方法/实验/相关工作
- **openpyxl**: 按字段匹配表头列，追加数据行

---

## 📁 目录结构 / Structure

```
paper-master/
├── README.md
├── LICENSE
├── requirements.txt
├── .claude-plugin/
│   ├── plugin.json          # 插件元数据
│   └── marketplace.json     # Marketplace 信息
├── skills/
│   └── paper-to-master-table/
│       ├── SKILL.md          # Skill 指令（Claude Agent 遵循的规范）
│       └── scripts/
│           ├── append_summary_row.py   # Excel 追加脚本
│           └── verify_last_row.py      # 验证工具
└── reference/                # （可选）存放待读 PDF 及提取的 txt
```

---

## 📦 依赖 / Dependencies

- Python ≥ 3.8
- [PyMuPDF](https://github.com/pymupdf/PyMuPDF) — AGPL 协议，快速 PDF 文本提取
- [openpyxl](https://openpyxl.readthedocs.io/) — MIT 协议，Excel 读写
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) — AI Agent 运行环境

---

## 📄 许可 / License

MIT License — 详见 [LICENSE](LICENSE)

---

## 🙏 致谢 / Acknowledgments

- [PyMuPDF](https://pymupdf.readthedocs.io/) 提供了极其好用的 PDF 文本提取
- [Claude Agent SDK](https://docs.anthropic.com/) 提供了强大的 AI 理解能力
