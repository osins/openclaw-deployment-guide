# macOS 集成指南

本指南介绍了如何在 macOS 环境下最大化 OpenClaw 的能力，包括系统集成、Apple 生态系统的利用以及其他 macOS 专用功能。

## 目录

- [系统级集成](#系统级集成)
- [Apple 生态系统集成](#apple-生态系统集成)
- [通知与交互](#通知与交互)
- [媒体与通讯](#媒体与通讯)
- [开发者工具](#开发者工具)
- [安全与隐私](#安全与隐私)
- [配置示例](#配置示例)

## 系统级集成

### 文件系统监控
利用 macOS 的文件系统事件监控能力：

```bash
# 监控特定目录的变化
fswatch -o /path/to/monitor | xargs -n1 -I{} openclaw trigger file-change-event
```

### 系统控制
使用 macOS 内建命令控制系统功能：

```applescript
# 音量控制
osascript -e "set volume output volume 50"

# 电源管理
sudo pmset sleepnow

# 亮度控制（需安装第三方工具）
brightness 0.7
```

### 应用程序控制
通过 AppleScript 控制 macOS 应用程序：

```applescript
# 控制 Safari
tell application "Safari"
    activate
    make new document with properties {URL:"https://example.com"}
end tell
```

## Apple 生态系统集成

### AppleScript/Automation
利用 AppleScript 与 macOS 原生应用深度集成：

```json
{
  "tools": {
    "macos_app_control": {
      "type": "exec",
      "command": "osascript -e '{{script}}'",
      "parameters": {
        "script": {
          "type": "string",
          "description": "要执行的 AppleScript"
        }
      }
    }
  }
}
```

### iCloud 同步
配置与 iCloud 的数据同步：

```bash
# 同步到 iCloud Drive
cp /path/to/data ~/Library/Mobile\ Documents/com~apple~CloudDocs/
```

### 快捷指令集成
与 macOS 的快捷指令应用集成：

```applescript
# 执行快捷指令
osascript -e 'tell application "Shortcuts" to run shortcut "{{shortcut_name}}"'
```

## 通知与交互

### 系统通知
使用 macOS 通知中心：

```applescript
osascript -e 'display notification "{{message}}" with title "{{title}}" subtitle "{{subtitle}}"'
```

### 语音反馈
利用 macOS 的文本转语音功能：

```bash
say "Hello from OpenClaw" -v Samantha
```

## 媒体与通讯

### 媒体控制
控制音乐和视频播放：

```bash
# 控制 iTunes/Apple Music
osascript -e 'tell application "Music" to play track "{{track_name}}"'

# 屏幕捕捉
screencapture -x ~/Desktop/screenshot.png
```

### 通讯集成
与 Messages、Mail 等应用集成：

```applescript
# 发送 iMessage
osascript -e 'tell application "Messages" to send "{{message}}" to buddy "{{contact}}"'
```

## 开发者工具

### Xcode 集成
自动化开发流程：

```bash
# 构建项目
xcodebuild -project MyApp.xcodeproj -scheme MyApp build

# 运行测试
xcodebuild -project MyApp.xcodeproj -scheme MyApp test
```

### 终端增强
与 iTerm2 等终端应用协作：

```json
{
  "tools": {
    "terminal_automation": {
      "type": "exec",
      "command": "osascript -e 'tell application \"iTerm\" to create window with default profile'",
      "description": "创建新的终端窗口"
    }
  }
}
```

## 安全与隐私

### 权限管理
确保 OpenClaw 有必要的系统权限：

1. 打开“系统偏好设置” > “安全性与隐私” > “隐私”标签
2. 在左侧列表中选择“辅助功能”、“自动化”等
3. 确保 OpenClaw 或其运行的终端应用在允许列表中

### 数据保护
使用钥匙串存储敏感信息：

```bash
# 存储凭证到钥匙串
security add-internet-password -s example.com -a username -w password

# 读取凭证
security find-internet-password -s example.com -w
```

## 配置示例

### macOS 优化配置
```json
{
  "tools": {
    "system": {
      "exec": {
        "enabled": true,
        "allowlist": [
          "osascript",
          "afplay",
          "say",
          "pmset",
          "networksetup",
          "defaults",
          "caffeinate",
          "screencapture",
          "ffmpeg",
          "ioreg",
          "system_profiler",
          "diskutil",
          "hdiutil",
          "brightness"  // 如果已安装
        ]
      }
    }
  },
  "hooks": {
    "macos_startup": {
      "enabled": true,
      "script": "osascript -e 'display notification \"OpenClaw 已启动\" with title \"OpenClaw\"'"
    }
  }
}
```

## 安装和配置

1. 确保已安装必要的命令行工具：
   ```bash
   xcode-select --install
   ```

2. （可选）安装亮度控制工具：
   ```bash
   brew install kfix/brightness/brightness
   ```

3. 配置 OpenClaw 以使用上述工具集

4. 根据需要调整权限设置

## 故障排除

### 权限问题
如果遇到权限错误，请检查：
- 系统偏好设置中的隐私设置
- 应用是否有辅助功能权限
- 命令是否在 allowlist 中

### 脚本执行失败
- 验证 AppleScript 语法
- 确认目标应用是否正在运行
- 检查参数是否正确传递

## 贡献

欢迎提交 Pull Request 来补充更多 macOS 特定的集成技巧和解决方案。