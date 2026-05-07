# OpenCode CLI 自动化模式

## 模式 1: CI/CD 集成

```bash
# GitHub Actions 中使用 opencode run
opencode run --format json "Review the diff and suggest improvements" > review.json

# 带文件附加
opencode run -f changed_file.py "Check for security issues"

# 指定模型和代理
opencode run -m anthropic/claude-sonnet-4 --agent code-reviewer "Review PR"
```

## 模式 2: 批处理脚本

```bash
#!/bin/bash
for file in src/*.py; do
  opencode run -f "$file" "Add type hints to this file" > "out/$(basename $file).md"
done
```

## 模式 3: 远程服务器 + 客户端

```bash
# Terminal 1: 启动无头服务器（避免每次 MCP 冷启动）
opencode serve --port 4096 --hostname 0.0.0.0

# Terminal 2: 客户端附加
opencode run --attach http://localhost:4096 "Quick question"

# Terminal 3: 远程 TUI 附加
opencode attach http://10.20.30.40:4096 -u opencode -p mypassword
```

## 模式 4: JSON 输出解析

```bash
# 获取结构化输出用于后续处理
result=$(opencode run --format json "Analyze the codebase architecture")
echo "$result" | jq '.events[] | select(.type == "text") | .content'
```

## 模式 5: 会话延续工作流

```bash
# 开始新会话
opencode run --title "refactor-auth" "Plan the auth module refactor"

# 继续同一会话
opencode run -c "Now implement step 1"

# 从已知会话分叉
opencode run -s <sessionID> --fork "Try an alternative approach"
```

## 模式 6: 分享与协作

```bash
# 创建可分享的会话
opencode run --share "Explain the architecture"

# 导出会话供他人导入
opencode export <sessionID> > session.json

# 导入分享的会话
opencode import https://opncd.ai/s/abc123
```
