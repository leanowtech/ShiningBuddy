# Shining Buddy

Shining Buddy 是运行在个人电脑或独立服务器上的个人助理，提供 WebUI、微信 iLink 对话、项目与会话、定时任务、Skill、记忆和成果文件管理。

本仓库提供安装包、发行说明、使用手册和问题反馈入口。核心源码不在本仓库发布。安装包随附运行所需的 JavaScript 和脚本，仍可被提取或逆向。

- [下载安装包](https://github.com/leanowtech/ShiningBuddy/releases)
- [安装与首次启动](docs/install.md)
- [保留数据升级](docs/upgrade.md)
- [故障排查](docs/troubleshooting.md)
- [反馈问题](https://github.com/leanowtech/ShiningBuddy/issues/new/choose)

## 选择安装包

| 系统 | 程序 | 文件名中的目标 |
| --- | --- | --- |
| Apple Silicon Mac | 菜单栏 GUI 或单文件 CLI | `macos-arm64` |
| Linux ARM64 | 单文件 CLI | `linux-arm64` |
| Linux x86_64 | 单文件 CLI | `linux-x64` |
| Windows ARM64 / x64 | GUI ZIP 或单文件 CLI | `windows-arm64` / `windows-x64` |

以具体 Release 的附件和平台验证状态为准。`rc` 是预发布版本；交叉构建成功不表示目标机器已完成验收。签名、公证状态见对应发行说明。

GUI 负责选择实例、启动服务和打开 WebUI；模型配置、首次密码设置与凭据解锁在 WebUI 完成。CLI 使用显式 `--data-dir` 选择实例，实例设置保存在该目录的 `config.toml`。

## 下载后核对

Release 附带 `SHA256SUMS` 和 `release-manifest.json`。下载清单列出的全部文件后，在同一目录执行：

```sh
# macOS
shasum -a 256 -c SHA256SUMS
# Linux
sha256sum -c SHA256SUMS
```

只下载一个附件时，可将该文件的 SHA256 与清单对应行比较。GitHub 自动显示的 `Source code (zip/tar.gz)` 仅包含本发行仓库文档，请从 Assets 选择安装包。

普通发行包不附带核心开发工程。核心代码开发需要显式提供匹配版本的源码；日常记忆、Skill、任务、指令与权限配置不依赖该开发工程。

本仓库公开不表示核心程序采用开源许可，详见[许可说明](LICENSE.md)。第三方组件保留各自的许可证。
