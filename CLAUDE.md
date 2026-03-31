# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 提供在本仓库中处理代码时的指导。

## 项目概述

本仓库为 Claude Code 提供**网页搜索和内容提取技能**。它集成了两个外部服务：

- **[GUILessBingSearch](https://github.com/wszqkzqk/GUILessBingSearch)**（端口 8765）—— Bing 网页搜索 API
- **[qt-web-extractor](https://github.com/wszqkzqk/qt-web-extractor)**（端口 8766）—— 支持 JS 渲染的网页内容提取

两者分别作为 git 子模块包含在 `GUILessBingSearch/` 和 `qt-web-extractor/` 中。

## 仓库结构

```
.claude/skills/          # Claude Code 技能定义
├── setup/               # 初始化技能
│   ├── SKILL.md         # 技能文档
│   └── setup            # Python CLI 脚本
├── websearch/           # Bing 搜索技能
│   ├── SKILL.md         # 技能文档
│   └── websearch        # Python CLI 脚本
└── webextract/          # 网页提取技能
    ├── SKILL.md         # 技能文档
    └── webextract       # Python CLI 脚本

skills -> .claude/skills # 便捷符号链接
.claude/settings.local.json  # Claude Code 权限配置
```

## 环境要求

使用这些技能需要在本地运行外部服务：

### 1. 部署 GUILessBingSearch（用于 websearch 技能）

```bash
cd GUILessBingSearch
# 按照服务的部署说明操作
# 默认端点：http://127.0.0.1:8765
```

验证：`curl http://127.0.0.1:8765/health`

### 2. 部署 qt-web-extractor（用于 webextract 技能）

```bash
cd qt-web-extractor
# 按照服务的部署说明操作
# 默认端点：http://127.0.0.1:8766
```

验证：`curl http://127.0.0.1:8766/health`

### 3. 初始化子模块（如果为空）

```bash
git submodule update --init --recursive
```

## 使用技能

### 网页搜索

```bash
# 通过技能脚本搜索
./skills/websearch/websearch "搜索关键词"
./skills/websearch/websearch "搜索关键词" -n 20    # 20 条结果
./skills/websearch/websearch --health              # 检查服务
```

### 网页提取

```bash
# 提取单个页面
./skills/webextract/webextract https://example.com

# 批量提取多个页面
./skills/webextract/webextract https://site1.com https://site2.com

# 强制 PDF 模式
./skills/webextract/webextract https://example.com/doc.pdf --pdf
```

## 环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `WEBSEARCH_URL` | `http://127.0.0.1:8765` | GUILessBingSearch 端点 |
| `WEBSEARCH_API_KEY` | （空） | 搜索服务的 API 密钥 |
| `WEBEXTRACT_URL` | `http://127.0.0.1:8766` | qt-web-extractor 端点 |
| `WEBEXTRACT_API_KEY` | （空） | 提取服务的 API 密钥 |

## 架构

两个技能都是 Python CLI 脚本，其工作流程如下：

1. 从环境变量读取配置
2. 向各自的服务发送 HTTP POST 请求
3. 解析 JSON 响应并格式化为人可读输出
4. 支持 `--json` 标志以输出原始 JSON

这些技能遵循 Claude Code 的技能规范（在 `SKILL.md` 文件中使用 YAML 前置元数据记录）。

### websearch 脚本流程

- `search()` → POST `/search`，携带 `{"query": "...", "count": N}`
- 返回 `{title, link, snippet}` 结果列表

### webextract 脚本流程

- 单 URL：`extract()` → POST `/extract`，携带 `{"url": "...", "pdf": bool}`
- 批量 URL：`extract_batch()` → POST `/`，携带 `{"urls": [...]}`
- 返回 `{url, title, text, html, error}`

## 故障排除

**"连接被拒绝"错误**：外部服务未运行。请启动 GUILessBingSearch 和/或 qt-web-extractor。

**空子模块**：运行 `git submodule update --init --recursive`。

**权限被拒绝**：确保脚本可执行：`chmod +x skills/websearch/websearch skills/webextract/webextract`

---

## 快速开始 (Setup)

使用内置的 `setup` 技能一键初始化仓库：

```bash
# 完整初始化
./skills/setup/setup

# 仅检查服务状态
./skills/setup/setup --check

# 仅设置记忆文件
./skills/setup/setup --memory
```

setup 技能会完成：
1. 初始化 git 子模块（GUILessBingSearch、qt-web-extractor）
2. 设置脚本可执行权限
3. 检查服务运行状态
4. 创建 Claude Code 记忆文件（包含行为准则）
