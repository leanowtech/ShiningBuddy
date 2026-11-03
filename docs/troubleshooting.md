# 故障排查与反馈

反馈时记录版本、系统、CPU 架构、入口和可重复步骤。只提交脱敏错误，不上传完整配置、数据库、凭据或业务文件。

macOS / Linux 的只读状态命令：

```sh
SB_CLI='/实际路径/shining-buddy'
SB_DATA='/实际路径/instance'
"$SB_CLI" --version --json
"$SB_CLI" service status --data-dir "$SB_DATA" --json --no-input
"$SB_CLI" service logs --data-dir "$SB_DATA" --lines 60 --json --no-input
```

- 页面打不开：先检查服务状态和最新日志，再核对 `web open` 返回的完整入口、监听地址、端口与网络。
- 凭据未就绪：在凭据安全页检查状态并用原主密码解锁。密码错误、会话竞争与服务异常分别处理，不为修复普通异常清空 vault。
- 任务或通知缺失：分别查看计划、运行、成果与通知回执。平台接受不等于手机实收，结果未知时不要反复重跑。
- 升级失败：核对 GUI、CLI、服务实际版本是否一致，保留旧程序和备份。不要删除实例锁、数据库或修改内部记录来强行启动。
