# OpenCode CLI 环境变量完整列表

## 核心配置

| 变量 | 类型 | 描述 |
|------|------|------|
| `OPENCODE_AUTO_SHARE` | bool | 自动分享会话 |
| `OPENCODE_CONFIG` | string | 配置文件路径 |
| `OPENCODE_TUI_CONFIG` | string | TUI 配置文件路径 |
| `OPENCODE_CONFIG_DIR` | string | 配置目录路径 |
| `OPENCODE_CONFIG_CONTENT` | string | 内联 JSON 配置内容 |
| `OPENCODE_PERMISSION` | string | 内联 JSON 权限配置 |

## 服务器与网络

| 变量 | 类型 | 描述 |
|------|------|------|
| `OPENCODE_SERVER_PASSWORD` | string | serve/web HTTP 基本认证密码 |
| `OPENCODE_SERVER_USERNAME` | string | 认证用户名（默认 opencode） |
| `OPENCODE_MODELS_URL` | string | 自定义模型配置 URL |

## 功能开关

| 变量 | 类型 | 描述 |
|------|------|------|
| `OPENCODE_DISABLE_AUTOUPDATE` | bool | 禁用自动更新 |
| `OPENCODE_DISABLE_PRUNE` | bool | 禁用旧数据清理 |
| `OPENCODE_DISABLE_TERMINAL_TITLE` | bool | 禁用终端标题更新 |
| `OPENCODE_DISABLE_DEFAULT_PLUGINS` | bool | 禁用默认插件 |
| `OPENCODE_DISABLE_LSP_DOWNLOAD` | bool | 禁用 LSP 自动下载 |
| `OPENCODE_DISABLE_AUTOCOMPACT` | bool | 禁用自动上下文压缩 |
| `OPENCODE_DISABLE_MODELS_FETCH` | bool | 禁用远程模型获取 |
| `OPENCODE_ENABLE_EXPERIMENTAL_MODELS` | bool | 启用实验性模型 |
| `OPENCODE_ENABLE_EXA` | bool | 启用 Exa 搜索工具 |

## Claude Code 兼容

| 变量 | 类型 | 描述 |
|------|------|------|
| `OPENCODE_DISABLE_CLAUDE_CODE` | bool | 禁用 .claude 读取 |
| `OPENCODE_DISABLE_CLAUDE_CODE_PROMPT` | bool | 禁用 CLAUDE.md |
| `OPENCODE_DISABLE_CLAUDE_CODE_SKILLS` | bool | 禁用 skills 加载 |

## 其他

| 变量 | 类型 | 描述 |
|------|------|------|
| `OPENCODE_GIT_BASH_PATH` | string | Windows Git Bash 路径 |
| `OPENCODE_FAKE_VCS` | string | 模拟 VCS（测试用） |
| `OPENCODE_CLIENT` | string | 客户端标识（默认 cli） |

## 实验性环境变量

| 变量 | 类型 | 描述 |
|------|------|------|
| `OPENCODE_EXPERIMENTAL` | bool | 启用所有实验性功能 |
| `OPENCODE_EXPERIMENTAL_BASH_DEFAULT_TIMEOUT_MS` | number | bash 默认超时(ms) |
| `OPENCODE_EXPERIMENTAL_OUTPUT_TOKEN_MAX` | number | 最大输出 token |
| `OPENCODE_EXPERIMENTAL_FILEWATCHER` | bool | 启用目录文件监听 |
| `OPENCODE_EXPERIMENTAL_DISABLE_FILEWATCHER` | bool | 禁用文件监听 |
| `OPENCODE_EXPERIMENTAL_PLAN_MODE` | bool | 启用计划模式 |
| `OPENCODE_EXPERIMENTAL_MARKDOWN` | bool | 启用实验性 Markdown |
| `OPENCODE_EXPERIMENTAL_LSP_TOOL` | bool | 启用实验性 LSP 工具 |
| `OPENCODE_EXPERIMENTAL_LSP_TY` | bool | Python TY LSP |
| `OPENCODE_EXPERIMENTAL_OXFMT` | bool | oxfmt 格式化器 |
| `OPENCODE_EXPERIMENTAL_ICON_DISCOVERY` | bool | 图标发现 |
| `OPENCODE_EXPERIMENTAL_DISABLE_COPY_ON_SELECT` | bool | 禁用选中复制 |
| `OPENCODE_EXPERIMENTAL_EXA` | bool | 实验性 Exa 功能 |
