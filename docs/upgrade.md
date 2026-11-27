# 保留数据升级

先阅读目标 Release 的实例格式、平台验收、已知问题和回退说明。保持原数据目录与项目目录，不通过清空实例解决普通升级问题。

## macOS GUI

1. 核对原启动器或 WebUI 当前使用的数据目录。
2. 等待正在执行的任务结束，停止服务；仅关闭窗口不会停止服务。
3. 备份原数据目录、外部项目目录和旧应用。运行期 socket 不作为备份文件。
4. 解压新 ZIP 并替换应用，通过「打开已有助手…」选择原数据目录。
5. 打开工作台，按提示完成版本检查或解锁，核对项目、会话、计划和凭据状态。

## macOS / Linux CLI

自 v0.9.9-rc.7 起，可将发行附件 `stop.sh` 和 `start.sh` 放在安装目录根部，与 `bin/` 和 `instance/` 同级。脚本需要 Python 3。默认先找 `bin/shining-buddy`，也支持安装目录根部的 `shining-buddy`。脚本所在位置决定默认路径，执行时可以在任意目录。

```sh
sh /安装目录/stop.sh
# 确认已停止，备份原程序与实例，然后替换 CLI。
sh /安装目录/start.sh
```

非默认布局用 `--cli` 和 `--data-dir` 指定。停服默认等在途任务完成，明确需要取消时使用 `stop.sh --cancel-active`。启停脚本适用于手动管理的实例。已启用 Shining Buddy 内置自启动的实例继续使用 `service` 命令管理。

已有自定义 macOS LaunchAgent 保活时，可为**两份脚本**同时传入 `--keepalive-plist` 和 `--keepalive-state`，也可设置 `SB_KEEPALIVE_PLIST` 和 `SB_KEEPALIVE_STATE`。保活程序需要遵守状态目录中的 `off` 标记。脚本读取 plist 的实际 Label，停服前暂停，确认新版本的管理服务可用后恢复。普通手动实例无需设置保活。

也可使用以下 CLI 命令自行启停。

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

从 v0.9.4 起，启动可能先进入受限维护页面：可查看进度、诊断与安全停止；`ready: true` 仅表示管理可达。维护完成、凭据就绪与升级放行后，业务才恢复。

维护协议提升到 3 后，旧 CLI 拒绝继续写入；不要删除维护回执或锁来绕过检查。中断恢复需使用原操作与匹配备份，已有新写入时不能自动回退覆盖。

核对运行版本、项目、会话、计划与最近运行。凭据解锁后检查可用性，任务结果和通知送达分别核对。

升级失败时保留旧程序、原数据和错误，先读[故障排查](troubleshooting.md)。只有目标版本支持原格式时才直接回退程序；格式升级后的回退按对应版本说明恢复备份。不要自动重跑结果未知的任务。
