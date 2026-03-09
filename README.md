# OpenClaw Desktop

高性能跨平台 AI 助手管理工具，基于 **Tauri 2.0 + React + TypeScript + Rust** 构建。

![Platform](https://img.shields.io/badge/platform-macOS%20|%20Windows%20|%20Linux-blue)
![Tauri](https://img.shields.io/badge/Tauri-2.0-orange)
![React](https://img.shields.io/badge/React-18-61DAFB)
![Rust](https://img.shields.io/badge/Rust-1.70+-red)

## 项目介绍

OpenClaw Desktop 是一款普通人入手私人助理的高性能跨平台 AI 助手管理工具。

**优点**：
***1. 一键运行完成安装。安装私人 AI 助理就像安装应用一样简单。
***2. 自动更新。软件支持自动更新功能，当 OpenClaw 发布新版本时，应用会自动检测并下载安装更新

## 核心架构

本项目采用 **Tauri + 独立后端服务** 架构：

```
┌─────────────────────────────────────────────────────────┐
│                   Tauri 桌面应用                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │              React 前端界面                     │   │
│  └─────────────────────────────────────────────────┘   │
│                         │                               │
│                    Tauri IPC                           │
│                         │                               │
│  ┌─────────────────────────────────────────────────┐   │
│  │     Rust 后端 (进程管理)                          │   │
│  │     - start_service()                            │   │
│  │     - stop_service()                             │   │
│  │     - get_service_status()                       │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                          │
                     端口检测 (18789)
                          │
┌─────────────────────────────────────────────────────────┐
│              Python 后端服务 (独立进程)                   │
│              http://localhost:18789                     │
└─────────────────────────────────────────────────────────┘
```

### 架构优势

1. **后端独立部署** - 后端作为独立可执行文件，不需要打包在 Tauri 内部
2. **端口检测状态** - 通过检查端口是否被占用来判断服务状态
3. **前端直连后端** - 前端通过 HTTP API 与后端通信，无需复杂 IPC
4. **简单可维护** - 路径处理简单，架构清晰

## 功能特性

| 模块 | 功能 |
|------|------|
| 📊 **仪表盘** | 实时服务状态监控、进程内存统计、一键启动/停止/重启 |
| 🤖 **AI 配置** | 14+ AI 提供商、自定义 API 地址、模型快速切换 |
| 📱 **消息渠道** | Telegram、Discord、Slack、飞书、微信、iMessage、钉钉 |
| ⚡ **服务管理** | 后台服务控制、实时日志、开机自启 |
| 🧪 **测试诊断** | 系统环境检查、AI 连接测试、渠道连通性测试 |
| 🔄 **自动更新** | 软件随着 OpenClaw 更新，自动下载并安装最新版本 |

### 自动更新

软件支持自动更新功能，当 OpenClaw 发布新版本时，应用会自动检测并下载安装更新：

- **自动检测**：启动应用时自动检查最新版本
- **静默下载**：后台下载更新包，不影响正常使用
- **一键安装**：下载完成后提示用户安装更新
- **无缝升级**：安装更新后自动重启应用

更新流程：
1. 应用启动时连接更新服务器
2. 检测到新版本后提示用户
3. 用户确认后开始下载更新
4. 下载完成自动安装并重启

## 快速开始

### 环境要求

- **Node.js** >= 18.0
- **Rust** >= 1.70
- **pnpm** (推荐) 或 npm

### macOS 额外依赖

```bash
xcode-select --install
```

### Windows 额外依赖

- [Microsoft C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
- [WebView2](https://developer.microsoft.com/en-us/microsoft-edge/webview2/)

### Linux 额外依赖

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install libwebkit2gtk-4.1-dev build-essential curl wget file libxdo-dev libssl-dev libayatana-appindicator3-dev librsvg2-dev

# Fedora
sudo dnf install webkit2gtk4.1-devel openssl-devel curl wget file libxdo-devel
```

### 安装与运行

```bash
# 克隆项目
git clone https://github.com/kylinstar2019/OpenClaw-Desktop.git
cd OpenClaw-Desktop

# 安装依赖
npm install

# 开发模式运行
npm run tauri:dev

# 构建发布版本
npm run tauri:build
```

## 项目结构

```
OpenClaw-Desktop/
├── src-tauri/                 # Rust 后端
│   ├── src/
│   │   ├── main.rs            # 入口
│   │   ├── commands/          # Tauri Commands
│   │   │   ├── service.rs     # 服务管理
│   │   │   ├── config.rs      # 配置管理
│   │   │   ├── process.rs     # 进程管理
│   │   │   └── diagnostics.rs # 诊断功能
│   │   ├── models/            # 数据模型
│   │   └── utils/             # 工具函数
│   ├── Cargo.toml
│   └── tauri.conf.json
│
├── src/                       # React 前端
│   ├── App.tsx
│   ├── components/
│   │   ├── Layout/            # 布局组件
│   │   ├── Dashboard/         # 仪表盘
│   │   ├── AIConfig/          # AI 配置
│   │   ├── Channels/          # 渠道配置
│   │   ├── Service/           # 服务管理
│   │   ├── Testing/           # 测试诊断
│   │   └── Settings/          # 设置
│   └── styles/
│       └── globals.css
│
├── package.json
├── vite.config.ts
└── tailwind.config.js
```

## 技术栈

| 层级 | 技术 | 说明 |
|------|------|------|
| 前端框架 | React 18 | 用户界面 |
| 状态管理 | Zustand | 轻量级状态管理 |
| 样式 | TailwindCSS | 原子化 CSS |
| 动画 | Framer Motion | 流畅动画 |
| 图标 | Lucide React | 精美图标 |
| 后端 | Rust | 高性能系统调用 |
| 跨平台 | Tauri 2.0 | 原生应用封装 |

## 构建产物

运行 `npm run tauri:build` 后，会在 `src-tauri/target/release/bundle/` 生成：

| 平台 | 格式 |
|------|------|
| macOS | `.dmg`, `.app` |
| Windows | `.msi`, `.exe` |
| Linux | `.deb`, `.AppImage` |

## 设计理念

- **暗色主题**：护眼舒适，适合长时间使用
- **现代 UI**：毛玻璃效果、流畅动画
- **响应式**：适配不同屏幕尺寸
- **高性能**：Rust 后端，极低内存占用

## 开发命令

```bash
# 开发模式（热重载）
npm run tauri:dev

# 仅运行前端
npm run dev

# 构建前端
npm run build

# 构建完整应用
npm run tauri:build

# 检查 Rust 代码
cd src-tauri && cargo check

# 运行 Rust 测试
cd src-tauri && cargo test
```

## 配置说明

### Tauri 配置 (tauri.conf.json)

- `app.windows` - 窗口配置
- `bundle` - 打包配置
- `plugins.shell.scope` - Shell 命令白名单
- `plugins.fs.scope` - 文件访问白名单

### 环境变量

应用会读取 `~/.openclaw/env` 中的环境变量配置。

## macOS 常见问题

### "已损坏，无法打开" 错误

macOS 的 Gatekeeper 安全机制可能会阻止运行未签名的应用。解决方法：

**方法一：移除隔离属性（推荐）**

```bash
# 对 .app 文件执行
xattr -cr /Applications/OpenClaw\ Desktop.app

# 或者对 .dmg 文件执行（安装前）
xattr -cr ~/Downloads/OpenClaw-Desktop.dmg
```

**方法二：通过系统偏好设置允许**

1. 打开 **系统偏好设置** > **隐私与安全性**
2. 在 "安全性" 部分找到被阻止的应用
3. 点击 **仍要打开**

**方法三：临时禁用 Gatekeeper（不推荐）**

```bash
# 禁用（需要管理员密码）
sudo spctl --master-disable

# 安装完成后重新启用
sudo spctl --master-enable
```

### 权限问题

如果应用无法正常访问文件或执行操作：

**授予完全磁盘访问权限**

1. 打开 **系统偏好设置** > **隐私与安全性** > **完全磁盘访问权限**
2. 点击锁图标解锁，添加 **OpenClaw Desktop**

**重置权限**

```bash
# 重置辅助功能权限数据库
sudo tccutil reset Accessibility

# 重置完全磁盘访问权限
sudo tccutil reset SystemPolicyAllFiles
```

## 许可证

MIT License - 详见 [LICENSE](LICENSE)

---

**Made with ❤️ by OpenClaw Team**
