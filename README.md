# OpenClaw 部署指引

本仓库用于记录和分享 OpenClaw 的部署技巧及常见问题解决方案。

## 目录

- [常见问题](#常见问题)
- [部署技巧](#部署技巧)
- [故障排除](#故障排除)

## 常见问题

### Telegram 连接问题

#### 问题描述
在对接 Telegram 时，如果不设置代理或轮询模式，无法实现 Telegram 和 OpenClaw 的消息互通。

#### 解决方案
当遇到 Telegram 机器人无法与 OpenClaw 正常通信时，需要配置代理或轮询模式来解决网络连接问题。以下是几种可能的解决方案：

1. **使用代理服务器**
   在 OpenClaw 配置中添加代理设置，确保请求能够正确路由到 Telegram 服务器。

2. **使用轮询模式**
   修改 OpenClaw 的 Telegram 通道配置，使用轮询模式而非默认的 Webhook 模式。

3. **使用隧道工具**
   如果服务器无法直接访问 Telegram API，可以使用隧道工具（如 ngrok）将本地端口暴露到公网。

#### 配置示例

**方法一：使用代理**
```json
{
  "channels": {
    "telegram": {
      "enabled": true,
      "dmPolicy": "pairing",
      "botToken": "YOUR_BOT_TOKEN",
      "groupPolicy": "allowlist",
      "streamMode": "partial",
      "proxy": "http://127.0.0.1:30808"
    }
  }
}
```

**方法二：使用轮询模式**
```json
{
  "channels": {
    "telegram": {
      "enabled": true,
      "dmPolicy": "pairing",
      "botToken": "YOUR_BOT_TOKEN",
      "groupPolicy": "allowlist",
      "streamMode": "partial",
      "mode": "polling"
    }
  }
}
```

### Node.js 版本兼容性问题

#### 问题描述
如果 Node.js 版本不是 22，则 OpenClaw gateway 无法正常启动。

#### 解决方案
确保安装并使用 Node.js v22 或兼容的版本。可以使用 Node 版本管理器来切换版本。

#### 检查和安装方法

1. 检查当前 Node.js 版本：
   ```bash
   node --version
   ```

2. 推荐使用 nvm（Node Version Manager）来管理 Node.js 版本：
   ```bash
   # 安装 Node.js v22
   nvm install 22
   nvm use 22
   ```

3. 验证版本：
   ```bash
   node --version  # 应该显示 v22.x.x
   ```

## 部署技巧

## 故障排除

### 诊断步骤

1. 检查网关状态：
   ```bash
   openclaw gateway status
   ```

2. 检查配置：
   ```bash
   openclaw config get
   ```

3. 查看日志：
   ```bash
   tail -f /tmp/openclaw/openclaw-*.log
   ```

### 使用 Doctor 工具进行自动诊断和修复

OpenClaw 提供了一个强大的 `doctor` 工具来进行健康检查和快速修复：

```bash
# 查看 doctor 命令的帮助信息
openclaw doctor --help

# 运行健康检查
openclaw doctor

# 自动应用推荐的修复（谨慎使用）
openclaw doctor --repair

# 强制应用激进修复（覆盖自定义服务配置）
openclaw doctor --force

# 深度扫描系统服务以查找额外的 gateway 安装
openclaw doctor --deep
```

Doctor 工具可以自动检测和修复多种问题，包括：
- 网关服务配置问题
- 通道连接问题
- 系统服务冲突
- 配置文件损坏
- 其他常见运行时问题

## 贡献

欢迎提交 Pull Request 来补充更多部署技巧和解决方案。