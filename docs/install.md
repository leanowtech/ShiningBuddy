# 安装与首次启动

先在 [Releases](https://github.com/leanowtech/ShiningBuddy/releases) 选择系统与 CPU 架构对应的附件，阅读该版本的已知问题和验证状态。

## macOS GUI

1. 解压 `Shining-Buddy-v<版本>-macos-arm64.zip`，得到 `Shining Buddy.app`，放到固定目录后打开。
2. 若 macOS 阻止打开，先核对下载摘要与发行说明的签名状态，再按「隐私与安全性」提示操作。
3. 新用户选择「创建新助手…」；已有实例选择「打开已有助手…」，选中原数据目录。
4. 点击「启动并打开工作台」，在 WebUI 完成密码、模型与通道设置。后续重启若提示解锁，使用原主密码在 WebUI 解锁。

关闭启动器窗口不等于停止服务；需要停止时使用「停止服务」或 CLI。

## macOS / Linux CLI

把下面的文件名替换为实际下载的 CLI。示例使用自己的用户目录，不需要 root：

```sh
SB_CLI="$PWD/shining-buddy-v<版本>-<目标>"
SB_DATA="$HOME/shining-buddy/instance"
chmod +x "$SB_CLI"
"$SB_CLI" --version
"$SB_CLI" service start --data-dir "$SB_DATA" --json --no-input
"$SB_CLI" web open --data-dir "$SB_DATA" --json --no-input
```

使用返回的完整浏览器入口完成设置。默认 WebUI 端口为 `18790`，以启动输出为准。远程服务器若需监听其他网卡，可在首次启动时加 `--bind-address 0.0.0.0`，按自己的网络访问策略配置入口。

服务器没有浏览器时，将返回的入口交给可访问该服务器的浏览器，保留路径与参数。打不开时核对监听地址、端口和防火墙。

## Windows

完整解压对应架构的 GUI ZIP，保留其中所有文件，打开 `Shining Buddy.exe`，选择实例并打开 WebUI。CLI 的 PowerShell 示例：

```powershell
$SbCli = (Resolve-Path '.\shining-buddy-v<版本>-windows-x64.exe').Path
$SbData = Join-Path $env:USERPROFILE 'shining-buddy\instance'
& $SbCli --version
& $SbCli service start --data-dir $SbData --json --no-input
& $SbCli web open --data-dir $SbData --json --no-input
```

## 数据与凭据

CLI 日常启动选择同一个 `data-dir` 即可，实例设置由其中的 `config.toml` 管理。实例外的项目目录需要另行备份。GUI 会保存实例书签，系统自启动登记位于操作系统规定的位置；这些入口不替代实例设置。

在 WebUI 凭据安全页面配置模型或任务凭据，由随包 kvault 管理。不要在聊天、Issue 或截图中提交密码和密钥。
