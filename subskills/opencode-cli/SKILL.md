---
name: opencode-cli
description: OpenCode CLI 调用指南。提供所有 CLI 命令、标志、环境变量的速查参考，用于在终端中编程式调用 OpenCode。当用户需要通过 shell 脚本、CI/CD、自动化流水线或非交互模式使用 OpenCode 时触发。触发词：opencode cli、opencode run、opencode serve、非交互模式、自动化调用、脚本调用 opencode。
---

# OpenCode CLI Skill

以编程方式调用 OpenCode CLI，覆盖非交互运行、服务部署、会话管理等场景。

## 核心命令速查

### 非交互运行（推荐 serve + attach 模式）

> **已知问题**: `opencode run` 单独运行会报 "Session not found"（TUI 实例占用数据库锁）。
> **解决方案**: 先 `opencode serve` 启动后台服务，再用 `opencode run --attach` 连接。

```bash
# 步骤 1: 启动后台服务（一次性）
opencode serve --port 4100 &

# 步骤 2: 通过 attach 发送请求（可重复调用）
opencode run --attach http://localhost:4100 "Explain closures in JS"
opencode run --attach http://localhost:4100 -m anthropic/claude-sonnet-4 "Write a test"
opencode run --attach http://localhost:4100 -f src/main.py "Review this file"
opencode run --attach http://localhost:4100 --format json "Return raw JSON events"
```

**关键标志**: `-m model`, `-f file`, `--attach url`, `--format json|default`, `--share`, `--agent name`, `--dir path`, `-c` (continue), `-s sessionID`, `--fork`

**注意**: `--format json` 输出流式事件（`step_start`, `text`, `step_finish`），文本回复可能只渲染在 ANSI 终端中。通过查询 SQLite DB 可获取完整回复内容：`sqlite3 ~/.local/share/opencode/opencode.db "SELECT substr(data,1,300) FROM part WHERE session_id='<id>' ORDER BY time_created;"`

### TUI 启动

```bash
opencode                    # 默认启动 TUI
opencode -c                 # 继续上次会话
opencode -s <sessionID>     # 指定会话
opencode -m anthropic/claude-sonnet-4  # 指定模型
opencode --prompt "initial prompt"     # 初始提示词
```

### 服务模式

```bash
opencode serve --port 4096 --hostname 0.0.0.0   # 无头 API 服务器
opencode web --port 4096                          # Web 界面
opencode attach http://host:4096                  # 附加 TUI 到远程后端
```

**认证**: 设置 `OPENCODE_SERVER_PASSWORD` 启用 HTTP 基本认证（用户名默认 `opencode`）

### 会话管理

```bash
opencode session list                     # 列出所有会话
opencode session list -n 10 --format json # 最近 10 个，JSON 格式
opencode export [sessionID]               # 导出会话 JSON
opencode import session.json              # 导入会话
opencode import https://opncd.ai/s/abc123 # 从分享链接导入
```

### 认证与模型

```bash
opencode auth login           # 交互式登录（配置 API Key）
opencode auth list            # 查看已认证提供商
opencode auth logout          # 清除凭据
opencode models               # 列出所有可用模型
opencode models anthropic     # 按提供商筛选
opencode models --refresh     # 刷新模型缓存
opencode models --verbose     # 详细信息（含费用）
```

### MCP 管理

```bash
opencode mcp add              # 添加 MCP 服务器
opencode mcp list             # 列出服务器状态
opencode mcp auth [name]      # OAuth 认证
opencode mcp auth list        # 查看 OAuth 状态
opencode mcp logout [name]    # 移除 OAuth 凭据
opencode mcp debug <name>     # 调试连接
```

### 代理管理

```bash
opencode agent list           # 列出所有代理
opencode agent create         # 创建自定义代理
```

### 统计与维护

```bash
opencode stats                          # Token 用量和费用
opencode stats --days 7 --models 5     # 最近 7 天，前 5 模型
opencode upgrade                       # 升级到最新版
opencode upgrade v0.1.48               # 升级到指定版本
opencode uninstall --dry-run           # 预览卸载（不实际删除）
```

## 全局标志

| 标志 | 简写 | 用途 |
|------|------|------|
| `--help` | `-h` | 帮助 |
| `--version` | `-v` | 版本号 |
| `--print-logs` | | 日志输出到 stderr |
| `--log-level` | | DEBUG/INFO/WARN/ERROR |

## 常用环境变量

| 变量 | 用途 |
|------|------|
| `OPENCODE_SERVER_PASSWORD` | serve/web 模式认证密码 |
| `OPENCODE_CONFIG` | 自定义配置文件路径 |
| `OPENCODE_CONFIG_CONTENT` | 内联 JSON 配置 |
| `OPENCODE_PERMISSION` | 内联 JSON 权限配置 |
| `OPENCODE_DISABLE_AUTOUPDATE` | 禁用自动更新 |
| `OPENCODE_DISABLE_AUTOCOMPACT` | 禁用自动上下文压缩 |
| `OPENCODE_ENABLE_EXPERIMENTAL_MODELS` | 启用实验性模型 |

完整环境变量列表见 `references/env-vars.md`。

## 排障速查

| 问题 | 原因 | 解决方案 |
|------|------|----------|
| `Session not found` | TUI 实例占用 DB 锁 | 用 `opencode serve` + `opencode run --attach` |
| `--format json` 输出为空 | 终端 ANSI 渲染吞掉内容 | 查 SQLite DB 获取完整回复 |
| MCP 冷启动慢 | 每次单独 run 需重新加载 | serve 模式保持 MCP 常驻 |

## 自动化脚本模式

详见 `references/patterns.md`：
- serve + attach 推荐模式（生产可用）
- CI/CD 集成模式
- 批处理脚本模式
- JSON 输出解析模式
