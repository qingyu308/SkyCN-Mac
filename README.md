# SkyCN-Mac

在 Apple Silicon Mac 上，通过 **Sikarugir + Wine** 启动网易国服 Windows PC 版《光·遇》的非官方兼容环境。安装后，从 macOS 启动台点击“光·遇”，即可打开网易官方启动器，再按官方流程登录、下载、更新和进入游戏。

本项目免费，不需要 Steam、虚拟机或付费兼容层。它与网易、thatgamecompany 和 Sikarugir 官方均无隶属关系。

> **已验证设备**：MacBook Air M1、16 GB 内存、macOS 27.2。该机器上已实际验证启动器下载、游戏画面、声音、键盘、鼠标，以及从启动台打开 App。其他 Mac 和后续游戏版本尚未逐一验证。

## 下载安装包

从 [v1.1.0 发布页](https://github.com/qingyu308/SkyCN-Mac/releases/tag/v1.1.0)下载 **[SkyCN-Mac-v1.1.0-no-game.dmg](https://github.com/qingyu308/SkyCN-Mac/releases/download/v1.1.0/SkyCN-Mac-v1.1.0-no-game.dmg)**。这是可安装的镜像；GitHub 自动生成的 `Source code.zip` 仅包含仓库文件，不能代替 DMG。

公开版 **不包含游戏本体**。游戏必须在安装后通过网易官方启动器下载，以保留正常的登录、更新和校验流程。

| 项目 | 内容 |
| --- | --- |
| 安装镜像 | 约 1.08 GB；展开后的 App 与环境约 2.2 GiB |
| 已包含 | macOS 启动 App、独立 SkyCN Wrapper、Sikarugir Wine Engine、网易启动器，以及兼容性修复 |
| 未包含 | 游戏本体、网易账号与密码、Cookies、用户缓存、Rosetta 2、Sikarugir Creator 图形管理程序 |
| 游戏来源 | 安装后由网易官方启动器下载 |

## 安装与使用

1. 在 Apple Silicon Mac 上打开 DMG，把 **“光·遇.app”** 拖到 **“应用程序”**。
2. 首次打开“光·遇”。App 会把独立环境安装到 `~/Games/SkyCN`，然后打开网易 FeverGames 启动器。第一次准备环境需要一些时间。
3. 如果 macOS 提示安装 Rosetta 2，请按 Apple 的系统提示完成；如遇到 App 安全提示，只针对这个 App 在“系统设置 → 隐私与安全性”中确认。无需关闭 SIP 或全局 Gatekeeper。
4. 在网易启动器中自行登录，选择《光·遇》并点击下载。下载完成后，继续从官方启动器进入游戏。
5. 以后直接从启动台点击 **“光·遇”**，无需打开 Terminal。

首次安装会复用已经存在且完整的 `~/Games/SkyCN` 目录，不会覆盖它。如果该目录存在但不完整，App 会提示处理，不会直接删除数据。建议为环境与游戏下载预留至少 15 GB 空间；实际需求会随游戏更新变化。

## 解决过哪些兼容问题

这些配置只作用于 SkyCN 独立环境，不修改网易游戏文件，也不降低 macOS 全局安全设置：

- **启动器无法下载**：仅对 SkyCN Wine 进程的本机 loopback socket 调整发送缓冲区，解决网易启动器本地 IPC 初始化卡住、下载按钮无响应。网易原版 `IPCPlugin.dll`、ZeroMQ DLL 和下载程序保持不变。
- **Vulkan 图形链路**：游戏通过 Windows Vulkan → WineVulkan → Wrapper 自带 MoltenVK → Apple Metal 渲染。兼容 Layer 只对 `Sky.exe` 报告一组已验证的显卡身份（NVIDIA GeForce GT 1030）；真正执行渲染的仍是 Apple M1，并未伪造 Vulkan 功能支持。
- **异常颜色**：保留已验证的 MoltenVK 设置，处理曾出现的大面积黄紫色画面。
- **中文方块字**：在独立 Wine Prefix 中设置中文环境和字体映射，让启动器与提示文字正常显示。
- **Retina 分辨率**：启用 Wine 的 Retina 模式，按当前 Mac 屏幕动态报告分辨率。游戏最终显示面积仍可能受游戏自身设置影响。
- **环境隔离**：使用独立的 64 位 Windows 10 Prefix；不影响其他 Wrapper 或 Windows 软件。

安装包保留了这些修复所需的运行文件与部分源码，例如 `Patches/skycn_loopback_buffer.c` 及其 `.dylib`，以及 `Diagnostics/gpu-spoof/` 中的 Vulkan Layer。Sikarugir Wine 核心和网易文件未被修改。

## 适用范围和已知限制

- 目前只在上述 M1 Mac 上完成真实游戏测试。其他 Apple Silicon 机型、macOS 版本或未来的网易客户端更新可能需要重新验证。
- 这是兼容环境；游戏或启动器升级后，Wine、Vulkan 或游戏安全组件的兼容性可能变化。请始终使用官方启动器，不绕过登录、更新或安全流程。
- App 使用 ad-hoc 代码签名，**没有 Apple 公证**；其他 Mac 首次运行可能需要针对这个 App 单独确认。
- Retina 模式不会保证游戏画面自动铺满每台 Mac 的缩放桌面。M1 MacBook Air 可先用中等画质、接近 1080p、最高 60 FPS；如果持续发热或降频，再降低帧率。

## 文件位置与卸载

| 用途 | 位置 |
| --- | --- |
| 启动台 App | `/Applications/光·遇.app` |
| 独立项目 | `~/Games/SkyCN/` |
| Wine Prefix | `~/Games/SkyCN/Wrapper/SkyCN.app/Contents/SharedSupport/prefix/` |
| 网易启动器 | Prefix 内的 `C:\Program Files\FeverGames\FeverGamesLauncher.exe` |
| 游戏默认目录 | Prefix 内的 `C:\FeverApps\sky` |

卸载时先退出游戏和启动器，再移除 `/Applications/光·遇.app`。如果还想删除这个项目的**全部游戏数据和配置**，检查后再移除 `~/Games/SkyCN/`。无需卸载 Rosetta 2、Homebrew 或其他 Wine Wrapper。

## 校验与来源

`v1.1.0` 公开版 DMG：**1,083,389,091 字节**。SHA-256：

```text
2d2ebdeb5def60c8c3e89ccf1037b4769720432e6d16dbaec29e829d389e21f6
```

本项目使用 [Sikarugir 官方项目](https://github.com/Sikarugir-App/Sikarugir)、Wine 和 MoltenVK 的运行组件。网易启动器与《光·遇》的权利归各自权利人所有；本仓库不代表这些项目的官方支持，也不另行授予第三方组件的使用许可。


## macOS 提示“Apple 无法验证光·遇.app”时

本项目目前只有 ad-hoc 签名，没有 Apple Developer ID 签名和公证，因此从网络下载后，macOS 可能阻止第一次打开。请先确认 DMG 来自本仓库的正式发布页，并核对该版本发布说明中的 SHA-256；不要对来源不明的 App 执行以下操作。

1. 打开 DMG，把“光·遇.app”拖入“应用程序”，等待复制完成。不要直接从 DMG 运行。
2. 在“应用程序”或启动台点击一次“光·遇”。若出现“Apple 无法验证……”的提示，点击“完成”或关闭提示。
3. 打开“系统设置”→“隐私与安全”，向下找到“已阻止‘光·遇.app’以保护 Mac”，点击旁边的“仍要打开”。
4. 按 macOS 提示，用本机密码或触控 ID 确认，然后在随后出现的对话框中点击“打开”。首次启动会复制 SkyCN 环境，显示进度，并打开网易官方启动器。
5. 这个允许只对当前 Mac 上的这个 App 生效；其他 Mac 第一次安装时需要各自确认。若更新或替换 App，macOS 可能再次要求确认。

这是 [Apple 官方提供的单个 App 允许流程](https://support.apple.com/guide/mac-help/open-a-mac-app-from-an-unknown-developer-mh40616/mac)。**无需关闭 SIP、全局 Gatekeeper 或 FileVault，也不要使用 `xattr` 命令批量解除隔离。**在没有 Developer ID 签名和 Apple 公证前，无法保证所有 Mac 首次打开时免提示。
