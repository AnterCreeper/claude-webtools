---
name: websearch
description: 实时网页搜索（Bing via GUILessBingSearch），获取互联网最新信息
version: 1.0.0
author: Claude
---

# websearch — 实时网页搜索

通过本地 GUILessBingSearch HTTP API 服务执行实时 Bing 搜索，获取互联网最新信息。

## 前置要求

1. **已部署 GUILessBingSearch 服务**（用户已完成）
   - 服务运行在 http://127.0.0.1:8765
   - 可通过 `curl http://127.0.0.1:8765/health` 验证

## 用法

```bash
# 基本搜索
./skills/websearch/websearch "深度学习 congestion control"

# 指定结果数量
./skills/websearch/websearch "transformer architecture" -n 20

# JSON 格式输出
./skills/websearch/websearch "Python tutorial" --json

# 检查服务健康
./skills/websearch/websearch --health
```

## 配置

通过环境变量自定义：

```bash
# 自定义服务地址
export WEBSEARCH_URL=http://127.0.0.1:8765

# 如服务配置了 API Key
export WEBSEARCH_API_KEY=your_key
```

## Claude Code 集成

在 Claude Code 中使用此技能：

```bash
# 直接调用
! ./skills/websearch/websearch "你的搜索词"

# 或在对话中使用 /websearch 快捷命令（需配置）
```

## API 端点

服务提供以下 HTTP API：

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/health` | 健康检查 |
| POST | `/search` | 执行搜索 |

### POST /search 请求体

```json
{
  "query": "搜索关键词",
  "count": 10
}
```

### 响应格式

```json
[
  {
    "title": "结果标题",
    "link": "https://example.com",
    "snippet": "结果摘要..."
  }
]
```

## 输出示例

```
找到 10 条结果（'深度学习 congestion control'）：

[1] On the Design of High-throughput Congestion Control
    https://example.com/paper1
    深度学习方法在拥塞控制中的应用...

[2] Learning-based Congestion Control Survey
    https://example.com/paper2
    本文综述了基于学习的拥塞控制算法...
```

## 故障排除

**服务未启动**
```
错误: 无法连接到搜索服务 (http://127.0.0.1:8765): [Errno 111] Connection refused
```
解决方案：
1. 确认 GUILessBingSearch 服务已启动
2. 检查端口是否被占用
3. 验证 `WEBSEARCH_URL` 环境变量

**搜索结果为空**
- 检查网络连接
- 确认 Bing 可访问（可能需要配置 `_U` Cookie）
- 查看 GUILessBingSearch 服务日志

## 与其他功能配合

搜索结果可用于：
- 发现新的研究论文和 arXiv 链接
- 获取最新技术动态
- 验证本地知识库的信息完整性
- 为文献综述补充最新资料
