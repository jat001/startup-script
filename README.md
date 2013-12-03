Linux Startup Script
====================

Some simple but useful startup script in `/etc/init.d`.

You need to add them to system services using `chkconfig`, `sysv-rc-conf` or `update-rc.d`.

Don't forget modify file permission to 755 (`-rwxr-xr-x`), modify owner to root and modify group to root.

Maybe you also need to modify the application path, the configuration file path, the log file path or the pid file path.

Linux 启动脚本
=============

一些简单但有用的启动脚本，放在 `/etc/init.d` 中。

你需要使用 `chkconfig`, `sysv-rc-conf` 或 `update-rc.d` 添加它们到系统服务中。

不要忘了更改文件权限为 755（`-rwxr-xr-x`）、所有者为 root、群组为 root。

你也可能需要更改程序路径、配置文件路径、日志文件路径或 pid 文件路径。