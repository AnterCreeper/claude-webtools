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
./skills/setup/setup           # 完整初始化
./skills/setup/setup --check   # 仅检查服务状态
./skills/setup/setup --memory  # 仅设置记忆文件链接
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

## 故障排除

**子模块拉取失败**
```bash
git submodule update --init --recursive --force
```

**服务未启动**
根据 CLAUDE.md 部署 GUILessBingSearch 和 qt-web-extractor
