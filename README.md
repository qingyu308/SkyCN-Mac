# SkyCN-Mac

在 Apple Silicon Mac 上，通过 **Sikarugir + Wine** 启动网易国服 Windows PC 版《光·遇》的非官方兼容环境。安装后，从 macOS 启动台点击“光·遇”，即可打开网易官方启动器，再按官方流程登录、下载、更新和进入游戏。

本项目免费，不需要 Steam、虚拟机或付费兼容层。它与网易、thatgamecompany 和 Sikarugir 官方均无隶属关系。

> **已验证设备**：MacBook Air M1、16 GB 内存、macOS 27.2。该机器上已实际验证启动器下载、游戏画面、声音、键盘、鼠标、Apple 自带拼音聊天与候选框，以及从启动台打开 App。其他 Mac、第三方输入法和后续游戏版本尚未逐一验证。

## V1.1.6 启动卡死修复

**完整安装包已构建，GitHub 发布草稿已保存，附件待上传，尚未发布。** 上传完成后会在 [Releases](https://github.com/qingyu308/SkyCN-Mac/releases) 提供 `SkyCN-Mac-v1.1.6-no-game.pkg`。新用户直接安装，已有 V1.1.4 / V1.1.5 用户可以升级，无需单独放置补丁。

本次定位到 macOS 会终止未声明后台运行、又从未显示窗口的旧启动入口及 Wine 子进程，导致残留的网易窗口无响应。V1.1.6（build 11）补上正确的后台 App 声明，保留原启动逻辑与已验证的下载、图形和输入法修复；没有更换 Wine 或修改游戏。修补后的本机正常启动已跨过原自动终止时间点；完整包六项检查通过，完整 1.1.6 安装流程与其他 Mac 的长期运行仍待验证。

升级前正常退出游戏、网易启动器与其他 Wine 程序。新安装器检测到运行中的进程会拒绝继续，原 App 会备份到 `~/Games/SkyCN/Backup/pre-v1.1.6/`；已有游戏、Prefix、Engine 和网易数据保留。本包不含游戏、实验性 FSR 或屏幕录制模块，也没有 Apple 公证。

V1.1.6：1,087,852,122 字节；SHA-256：`7d32b0cd9e3687996dcf6a713624cd0885e5b11ff48aa49cc43110e9fcee5598`。

## 下载安装包

当前版本是 **[V1.1.5（中文输入法修复）](https://github.com/qingyu308/SkyCN-Mac/releases/tag/V1.1.5)**。在发布页的 Assets 中下载 `SkyCN-Mac-v1.1.5-no-game.pkg`；GitHub 自动生成的 `Source code.zip` 不是安装包。需要回退时，可查看 [V1.1.4 发布页](https://github.com/qingyu308/SkyCN-Mac/releases/tag/V1.1.4)。

V1.1.5 使用 macOS 原生 PKG：系统安装器把直接启动的 App 放到 `/Applications/光·遇.app`，把独立兼容环境放到 `~/Games/SkyCN/`，安装后自动打开网易启动器。后续从启动台点击“光·遇”即可。安装结束到网易窗口响应之间会显示活动进度提示；不显示无法准确测量的百分比。

公开包 **不包含游戏本体**。游戏由网易官方启动器下载、更新和校验。

| 项目 | 内容 |
| --- | --- |
| V1.1.5 安装包 | 1,087,850,071 字节（约 1.09 GB）；不含游戏本体 |
| 已包含 | macOS 启动 App、独立 SkyCN Wrapper、Sikarugir Wine Engine、网易启动器与兼容性修复 |
| 未包含 | 游戏本体、网易账号与密码、Cookies、用户缓存、Rosetta 2、Sikarugir Creator 图形管理程序 |
| 游戏来源 | 安装后由网易官方启动器下载 |

## 安装与使用

1. 在 Apple Silicon Mac 上下载并打开 `SkyCN-Mac-v1.1.5-no-game.pkg`，按 macOS“安装器”提示安装。可能需要管理员密码或 Touch ID。
2. 如果 macOS 提示安装 Rosetta 2，请按 Apple 的系统提示完成。若系统阻止未公证安装包或 App，按下文的单个项目允许流程操作；无需关闭全局安全功能。
3. 安装完成后，网易 FeverGames 启动器会自动打开。首次启动期间会出现活动进度条；可关闭提示而不结束启动器。
4. 在网易启动器中自行登录，选择《光·遇》并下载。下载完成后，从官方启动器进入游戏。
5. 以后直接从启动台点击 **“光·遇”**；不需要 Terminal，也不会重复显示中间安装器。
6. 若要自动切换中文聊天，请在 macOS“系统设置 → 键盘 → 文本输入”启用 **Apple 自带拼音**和 **ABC（或美国英文）**。游戏操作时使用英文，打开聊天时切到拼音，离开聊天后恢复英文。

安装脚本会保留已经存在的 `~/Games/SkyCN`。如果这个目录存在但不完整，会报错并保留文件，不会直接删除游戏数据。建议为环境与游戏下载预留至少 15 GB 空间；实际需求会随游戏更新变化。

## 解决过哪些兼容问题

这些配置只作用于 SkyCN 独立环境，不修改网易游戏文件，也不降低 macOS 全局安全设置：

- **启动器无法下载**：仅对 SkyCN Wine 进程的本机 loopback socket 调整发送缓冲区，解决网易启动器本地 IPC 初始化卡住、下载按钮无响应。网易原版 `IPCPlugin.dll`、ZeroMQ DLL 和下载程序保持不变。
- **Vulkan 图形链路**：游戏通过 Windows Vulkan → WineVulkan → Wrapper 自带 MoltenVK → Apple Metal 渲染。兼容 Layer 只对 `Sky.exe` 报告一组已验证的显卡身份（NVIDIA GeForce GT 1030）；真正执行渲染的仍是 Apple M1，并未伪造 Vulkan 功能支持。
- **异常颜色**：保留已验证的 MoltenVK 设置，处理曾出现的大面积黄紫色画面。
- **中文方块字**：在独立 Wine Prefix 中设置中文环境和字体映射，让启动器与提示文字正常显示。
- **Retina 分辨率**：启用 Wine 的 Retina 模式，按当前 Mac 屏幕动态报告分辨率。游戏最终显示面积仍可能受游戏自身设置影响。
- **Apple 自带拼音与候选框**：V1.1.5 根据游戏已有的聊天输入焦点切换英文和自带拼音；修正窗口化候选位置与全屏候选层级，并在进入聊天时刷新输入上下文，处理偶尔没有候选框的问题。不读取聊天文字或按键。
- **环境隔离**：使用独立的 64 位 Windows 10 Prefix；不影响其他 Wrapper 或 Windows 软件。

安装包保留了这些修复所需的运行文件与部分源码，例如 `Patches/skycn_loopback_buffer.c` 及其 `.dylib`、输入法兼容组件，以及 `Diagnostics/gpu-spoof/` 中的 Vulkan Layer。只调整 SkyCN 专用 Wine 启动入口，Sikarugir Wine 核心二进制和网易文件未被修改。

## 适用范围和已知限制

- 当前完整包要求 **Apple Silicon Mac、macOS 27.0 或更新版本、Rosetta 2**；系统提示 Rosetta 时按 Apple 官方提示安装即可。
- 目前只在上述 M1 Mac 上完成真实游戏与本轮输入法测试。其他 Apple Silicon 机型、第三方输入法、长期运行或未来的网易客户端更新可能需要重新验证。V1.1.5 的完整升级安装流程尚未在另一台 Mac 验收。
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

V1.1.5 PKG：1,087,850,071 字节。SHA-256：

```text
d9e10b5bda501a157ed44aa2017d804e8059e438a452f9b42b42119c61386532
```

请以 [V1.1.5 发布说明](https://github.com/qingyu308/SkyCN-Mac/releases/tag/V1.1.5)中的附件和校验值为准。本项目使用 [Sikarugir 官方项目](https://github.com/Sikarugir-App/Sikarugir)、Wine 和 MoltenVK 的运行组件。网易启动器与《光·遇》的权利归各自权利人所有；本仓库不代表这些项目的官方支持，也不另行授予第三方组件的使用许可。

## macOS 提示无法验证安装包或“光·遇.app”时

V1.1.5 PKG 未使用 Apple Developer ID 签名，也未经过 Apple 公证；App 只有临时签名。macOS 可能阻止首次打开。先确认文件来自本仓库发行页并核对 SHA-256；不要对来源不明的文件执行以下操作。

1. 尝试打开 PKG。若系统阻止，关闭提示，打开“系统设置 → 隐私与安全性”，找到该安装包对应的提示并选择“仍要打开”，按系统提示确认。
2. 按 macOS 安装器完成安装。若首次启动“光·遇.app”时再次被阻止，可在同一设置页只允许这份 App，然后重试。
3. 管理员密码或 Touch ID 均在 macOS 系统界面中自行输入。此允许只作用于当前 Mac 上的相应项目；其他 Mac 可能需要各自确认。

这是 [Apple 官方提供的单个 App 允许流程](https://support.apple.com/guide/mac-help/open-a-mac-app-from-an-unknown-developer-mh40616/mac)。无需关闭 SIP、全局 Gatekeeper 或 FileVault，也不要用 `xattr` 批量解除隔离。在没有 Developer ID 签名和 Apple 公证前，无法保证所有 Mac 首次打开时免提示。
