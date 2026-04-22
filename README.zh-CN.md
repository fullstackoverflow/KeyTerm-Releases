<div align="center">
  <h1>⌨️ KeyTerm</h1>
  <p><strong>一个支持原生 YubiKey PIV 的现代 SSH 客户端</strong></p>

  <p>
    <img alt="Platform" src="https://img.shields.io/badge/platform-Windows%20%7C%20macOS-blue" />
    <img alt="Tauri" src="https://img.shields.io/badge/Tauri-2.x-24C8DB?logo=tauri" />
    <img alt="Rust" src="https://img.shields.io/badge/Rust-stable-orange?logo=rust" />
    <img alt="License" src="https://img.shields.io/badge/license-MIT-green" />
  </p>
</div>

---

## 功能特性

- 🔑 **YubiKey PIV 认证**：支持完整的 slot 管理、密钥生成、PIN / Touch 策略配置和序列号绑定
- 🔐 **多种认证方式**：支持 YubiKey PIV、SSH Key 文件、密码（系统钥匙串）和无认证
- 💻 **完整终端模拟**：基于 xterm.js，支持多标签、动态尺寸调整和 5000 行滚动缓冲
- 🪟 **分屏工作区**：可在单个标签页里左右或上下分屏，并支持拖拽分隔条
- 📁 **集成 SFTP 面板**：按会话浏览远程文件、上传下载并显示传输通知
- 🗂️ **连接管理器**：支持分组、标签和全文搜索
- 📝 **命令片段**：支持基于标签的片段库和 `Ctrl+K` 快速执行
- 🎥 **按键展示层**：支持 `Ctrl+Shift+K` / `Cmd+Shift+K` 切换
- 🔒 **安全凭据存储**：SSH 密码和存储密钥使用原生钥匙串保存
- 🛡️ **主机指纹校验**：支持 TOFU 与 `~/.ssh/known_hosts`，并可检测 MITM 风险
- 🎨 **主题切换**：内置深色 / 浅色界面主题
- 🗄️ **本地优先同步存储**：数据始终以本地为主，云端后端（S3/WebDAV）仅用于同步
- 🌍 **国际化支持**：内置英文 / 简体中文界面切换

### 计划中

- 🔗 **SSH agent forwarding**
- 📅 **定时任务 / 自动化钩子**

## 预览

![KeyTerm Demo](assets/preview.gif)

## 安装

请从公开发布仓库下载最新版本，例如：

```text
https://github.com/fullstackoverflow/KeyTerm-Releases/releases
```

| 平台 | 安装包 |
|----------|-----------|
| Windows  | `KeyTerm_x.x.x_x64-setup.exe` (NSIS) 或 `.msi` |
| macOS    | `KeyTerm_x.x.x_x64.dmg` |

### macOS 说明

当前 macOS 版本没有签名证书。如果你使用 macOS 包，请先自行解压应用后再手动启动。

如果 macOS 在解压后仍然阻止启动，可以执行：

```bash
sudo xattr -rd com.apple.quarantine /Applications/KeyTerm.app
```

## 认证方式

| 方式 | 说明 |
|--------|-------------|
| **YubiKey PIV** | 基于硬件的 PIV slot 认证，支持 PIN 和 Touch 策略 |
| **SSH Key File** | 支持 RSA、ECDSA、Ed25519 私钥，并自动选择合适的 RSA SHA-2 算法 |
| **Password** | SSH 密码认证，可选择保存到系统钥匙串 |
| **None** | 适用于允许无认证访问的服务器 |

## YubiKey 支持

KeyTerm 提供完整的 YubiKey PIV 支持：

- 检测已插入的 YubiKey（序列号、名称、固件版本）
- 管理所有 PIV slot（`9a`、`9c`、`9d`、`9e`、`82`–`95`）
- 直接在设备上生成 P-256 ECDSA 密钥
- 配置每个 slot 的 **PIN policy**（`never` / `once` / `always`）和 **touch policy**（`never` / `always` / `cached`）
- 将连接绑定到指定 YubiKey 序列号，避免误用错误设备
- 在认证过程中实时提示 PIN 和 Touch

## 技术栈

| 层 | 技术 |
|-------|-----------|
| UI Framework | React 19 + TypeScript |
| Desktop Runtime | Tauri 2 |
| Terminal | xterm.js 6 |
| SSH Client | russh（纯 Rust，异步） |
| SFTP | russh-sftp |
| YubiKey | yubikey-piv crate |
| Cryptography | p256、sha2、der |
| Storage | 可插拔存储驱动（Local + S3 + WebDAV） |
| Storage Abstraction | Apache OpenDAL |
| Vault 安全 | Argon2id + AES-256-GCM（内存解锁会话） |
| Build Tool | Vite 7 |

## 快捷键

| 快捷键 | 功能 |
|----------|--------|
| `Ctrl+K` / `Cmd+K` | 打开或关闭片段面板 |
| `Ctrl+Shift+K` / `Cmd+Shift+K` | 打开或关闭按键展示层 |
| `Ctrl+Shift+F` / `Cmd+Shift+F` | 打开或关闭 SFTP 侧边面板 |
| `Ctrl+Alt+Right` / `Cmd+Alt+Right` | 将当前面板向右分屏 |
| `Ctrl+Alt+Down` / `Cmd+Alt+Down` | 将当前面板向下分屏 |
| `Ctrl+W` / `Cmd+W` | 关闭当前分屏 |
| `Escape` | 关闭弹窗 / 抽屉 |
| `↑` / `↓` | 浏览片段列表 |
| `Enter` | 执行选中的片段 |
| 双击标签 | 重命名标签 |

## 右键菜单策略

- 系统右键菜单只在设计好的区域可用：
  - 标签项
  - 终端面板区域
- 其它 UI 区域默认禁用系统右键菜单，以避免误触弹出

## 许可证

MIT
