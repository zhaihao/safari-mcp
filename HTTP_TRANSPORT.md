# Safari MCP HTTP Transport Support

## Overview

Safari MCP 现在支持通过 HTTP URL 连接，不仅限于 stdio 传输。

## Installation

### 从 GitHub Fork 安装

```bash
# 使用 pnpm 全局安装（推荐）
pnpm add -g git+https://github.com/zhaihao/safari-mcp.git#ai

# 或使用 npm 全局安装
npm install -g git+https://github.com/zhaihao/safari-mcp.git#ai
```

### 从本地目录安装

```bash
# 克隆仓库
git clone https://github.com/zhaihao/safari-mcp.git -b ai
cd safari-mcp

# 使用 pnpm 全局安装
pnpm install -g .

# 或使用 npm 全局安装
npm install -g .
```

### 验证安装

```bash
# 检查安装
which safari-mcp

# 测试运行（stdio 模式）
echo '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-03-26","capabilities":{},"clientInfo":{"name":"test","version":"1.0"}}}' | safari-mcp
```

## Usage

### 启动 HTTP 模式

```bash
# 方式 1: 使用环境变量
MCP_TRANSPORT=http MCP_HTTP_PORT=9225 npx safari-mcp

# 方式 2: 使用 npm script（如果你已克隆仓库）
npm run mcp:http
```

### 环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `MCP_TRANSPORT` | `stdio` | 传输方式：`stdio` 或 `http` |
| `MCP_HTTP_PORT` | `9225` | HTTP 传输端口 |
| `MCP_HTTP_HOST` | `127.0.0.1` | HTTP 服务器地址 |

### 配置 Claude Code

在 `~/.mcp.json` 中添加：

```json
{
  "mcpServers": {
    "safari-http": {
      "url": "http://127.0.0.1:9225/mcp"
    }
  }
}
```

### 端口分配

| 用途 | 端口 |
|------|------|
| MCP stdio 传输 | 无（标准输入/输出）|
| MCP HTTP 传输 | 9225（可配置）|
| Safari 扩展通信 | 9224（固定）|
| WebSocket（Chrome 扩展）| 9223（固定）|

## 技术实现

- 使用 `@modelcontextprotocol/sdk` 的 `StreamableHTTPServerTransport`
- 支持 Server-Sent Events (SSE) 流式传输
- 会话管理通过随机 UUID 实现
- 完全兼容现有的 stdio 传输模式

## 测试

```bash
# 测试语法
node --check index.js

# 测试 HTTP 模式启动
MCP_TRANSPORT=http MCP_HTTP_PORT=9925 node index.js

# 测试 HTTP 端点
curl http://127.0.0.1:9925/mcp
```
