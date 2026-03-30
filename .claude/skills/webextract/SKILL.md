---
name: webextract
description: 网页内容提取（qt-web-extractor），将网页转换为 Markdown 文本
version: 1.0.0
author: Claude
---

# webextract — 网页内容提取

通过本地 qt-web-extractor HTTP API 服务提取网页内容，支持 JavaScript 渲染、SPA、PDF 等，输出干净的 Markdown 格式文本。

## 前置要求

1. **已部署 qt-web-extractor 服务**（用户已完成）
   - 服务运行在 http://127.0.0.1:8766
   - 可通过 `curl http://127.0.0.1:8766/health` 验证

## 用法

```bash
# 提取单个网页
./skills/webextract/webextract https://example.com

# 强制 PDF 模式
./skills/webextract/webextract https://example.com/doc.pdf --pdf

# 批量提取多个网页
./skills/webextract/webextract https://site1.com https://site2.com

# JSON 格式输出
./skills/webextract/webextract https://example.com --json

# 检查服务健康
./skills/webextract/webextract --health
```

## 配置

通过环境变量自定义：

```bash
# 自定义服务地址
export WEBEXTRACT_URL=http://127.0.0.1:8766

# 如服务配置了 API Key
export WEBEXTRACT_API_KEY=your_key
```

## Claude Code 集成

在 Claude Code 中使用此技能：

```bash
# 直接调用
! ./skills/webextract/webextract https://example.com

# 提取后分析
! ./skills/webextract/webextract https://news.ycombinator.com --json
```

## API 端点

服务提供以下 HTTP API：

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/health` | 健康检查 |
| POST | `/extract` | 单 URL 提取 |
| POST | `/` | 批量提取（Open WebUI 格式）|

### POST /extract 请求体

```json
{
  "url": "https://example.com",
  "pdf": false
}
```

### 响应格式

```json
{
  "url": "https://example.com",
  "title": "页面标题",
  "text": "Markdown 格式的正文内容...",
  "html": "<html>...</html>",
  "error": ""
}
```

## 输出示例

```
标题: Example Domain
URL: https://example.com

内容:
----------------------------------------
This domain is for use in illustrative examples in documents.

## More information

You may use this domain in literature without prior coordination...
```

## 功能特性

- **JavaScript 渲染** — 支持 React/Vue/Angular 等 SPA
- **Cookie & 登录** — 可访问需要登录的页面
- **PDF 提取** — 自动检测或强制 PDF 模式
- **Markdown 输出** — 干净的文本格式，适合 LLM 处理
- **批量处理** — 一次提取多个页面

## 故障排除

**服务未启动**
```
错误: 无法连接到提取服务 (http://127.0.0.1:8766): [Errno 111] Connection refused
```
解决方案：
1. 确认 qt-web-extractor 服务已启动
2. 检查端口是否被占用
3. 验证 `WEBEXTRACT_URL` 环境变量

**提取超时**
- 某些页面可能需要更长的加载时间
- 检查服务配置的 `TIMEOUT_MS` 设置

**Cloudflare 拦截**
- 部分受 Cloudflare 保护的页面可能无法提取
- 这是所有 headless 浏览器的已知限制

## 与其他功能配合

提取结果可用于：
- 网页内容分析和总结
- 从文档中提取关键信息
- 监控网页变化
- 为 RAG 系统获取最新内容
- 阅读 PDF 论文和报告
