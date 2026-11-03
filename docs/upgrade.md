# 保留数据升级

先阅读目标 Release 的实例格式、平台验收、已知问题和回退说明。保持原数据目录与项目目录，不通过清空实例解决普通升级问题。

## macOS GUI

1. 核对原启动器或 WebUI 当前使用的数据目录。
2. 等待正在执行的任务结束，停止服务；仅关闭窗口不会停止服务。
3. 备份原数据目录、外部项目目录和旧应用。运行期 socket 不作为备份文件。
4. 解压新 ZIP 并替换应用，通过「打开已有助手…」选择原数据目录。
5. 打开工作台，按提示完成版本检查或解锁，核对项目、会话、计划和凭据状态。

## macOS / Linux CLI

把变量设为当前旧程序与原数据目录。以下命令不会自动取消未完成任务：

```sh
SB_CLI='/实际路径/旧版/shining-buddy'
SB_DATA='/实际路径/instance'
"$SB_CLI" service status --data-dir "$SB_DATA" --json --no-input
"$SB_CLI" service stop --data-dir "$SB_DATA" --json --no-input
"$SB_CLI" service status --data-dir "$SB_DATA" --json --no-input
```

确认服务已停止后备份程序、数据与外部项目。将新版解压到新位置，保留旧程序用于回退：

```sh
SB_CLI='/实际路径/新版/shining-buddy'
chmod +x "$SB_CLI"
"$SB_CLI" --version --json
"$SB_CLI" service start --data-dir "$SB_DATA" --json --no-input
"$SB_CLI" web open --data-dir "$SB_DATA" --json --no-input
"$SB_CLI" service status --data-dir "$SB_DATA" --json --no-input
```

已有系统自启动登记的实例，还应核对登记指向的新程序路径；移动或改名后按 `service --help` 的命令更新登记。不要让旧、新程序同时服务同一实例。

## Windows

停止服务后备份实例、外部项目与旧程序目录。将新版 ZIP 完整解压到新目录，用新版启动器关联原实例。自启动实例需要核对登记的程序路径。

## 验收与回退

核对运行版本、项目、会话、计划与最近运行。凭据解锁后检查可用性，任务结果和通知送达分别核对。

升级失败时保留旧程序、原数据和错误，先读[故障排查](troubleshooting.md)。只有目标版本支持原格式时才直接回退程序；格式升级后的回退按对应版本说明恢复备份。不要自动重跑结果未知的任务。
