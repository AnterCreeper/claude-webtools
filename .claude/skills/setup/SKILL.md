---
name: setup
description: 初始化 claude-webtools 仓库：子模块、脚本权限、服务检查
version: 1.0.0
author: Claude
---

# setup — 初始化 claude-webtools

一键初始化仓库，完成所有必要配置。

## 用法

```bash
./skills/setup/setup --memory-dir PATH           # 完整初始化（必须指定记忆目录）
./skills/setup/setup --check                     # 仅检查服务状态
./skills/setup/setup --memory --memory-dir PATH  # 仅设置记忆文件
```

## 执行内容

1. **初始化 git 子模块** — 拉取 GUILessBingSearch 和 qt-web-extractor
2. **设置脚本权限** — 确保 websearch、webextract 可执行
3. **检查服务状态** — 验证搜索和提取服务是否运行
4. **设置记忆文件** — 创建软链接到 Claude Code 记忆目录

## 前置要求

- Git 仓库已克隆
- Python 3 可用
- 有可写权限

## 选项说明

| 选项 | 必需 | 说明 |
|------|------|------|
| `--memory-dir PATH` | **是** | 指定 Claude Code 记忆目录路径 |
| `--check` | 否 | 仅检查服务运行状态（不需要 `--memory-dir`） |
| `--memory` | 否 | 仅创建记忆文件 |

**如何确定记忆目录路径**

在 Claude Code 中运行 `claude-memory` 或查看提示信息，记忆目录位于 `~/.claude/projects/{项目名}/memory/`。

## 故障排除

**子模块拉取失败**
```bash
git submodule update --init --recursive --force
```

**服务未启动**
根据 CLAUDE.md 部署 GUILessBingSearch 和 qt-web-extractor
