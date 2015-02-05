Linux Init Scripts
====================

Some simple but useful init scripts in `/etc/init.d`.

You need to add them to system services using `chkconfig`, `sysv-rc-conf`, `update-rc.d` or `insserv`.

`sysv-rc-conf` will ignores `Required-Start`, `Required-Stop` and `Should-Start`, `Should-Stop`, and led startup sequences will be incorrect. Debian and Ubuntu users can use `update-rc.d` or `insserv` to set startup scripts. Please run `[sudo] ln -s /usr/lib/insserv/insserv /sbin/insserv` if can't find `insserv`.

Please remove `/etc/init.d/.legacy-bootordering` for closing legacy mode, startup sequences may be incorrect too, if legacy mode enabled.

Don't forget change the file permissions to 755 (`-rwxr-xr-x`), change the owner to root and change the group to root.

Maybe you also need to modify the program path, the configuration file path, the log file path or the pid file path.

In some Linux distributions (such as Debian, Ubuntu, Fedora, etc), they use tmpfs to mount `/run` temporarily. You must change the directory permissions to 777 (`drwxrwxrwx`) at system startup, to make the program doesn't run as root, has permissions to create a pid file in `/var/run`.

For example, add `chmod 777 /run` to `/etc/init/mounted-run.conf`:

```shell
# mounted-run - Populate and link to /run filesystem
#
# Populates the /run filesystem and adds compatibility links to it

description "Populate and link to /run filesystem"

start on mounted MOUNTPOINT=/run TYPE=tmpfs

task

script
    chmod 777 /run

    : > "/run/utmp"
    chmod 664 "/run/utmp"
    chgrp utmp "/run/utmp"

    # compatibility; should go away soon
    [ -d /dev/.initramfs/varrun ] && cp -a /dev/.initramfs/varrun/* /run/ || true

    mkdir -p /run/sendsigs.omit.d

    # Background the initial motd seeding
    #[ -d "/etc/update-motd.d" ] && run-parts --lsbsysinit /etc/update-motd.d > /run/motd &
end script
```

### WARNING

Some init scripts may in conflict with the software own scripts. Such as rsync-server and rsync, munin-server and munin munin-node. **Do NOT use them together**！

### See also

[The Guidelines for SysV-style Initscripts](https://en.opensuse.org/openSUSE:Packaging_init_scripts)


Linux 启动脚本
=============

一些简单但有用的启动脚本，放在 `/etc/init.d` 中。

你需要使用 `chkconfig`、`sysv-rc-conf`、`update-rc.d` 或 `insserv` 添加它们到系统服务中。

`sysv-rc-conf` 会忽略 `Required-Start`、`Required-Stop` 和 `Should-Start`、`Should-Stop`，从而导致启动顺序不正确，Debian 和 Ubuntu 用户可以使用 `update-rc.d` 或 `insserv` 设置启动项。如果找不到 `insserv`，请运行 `ln -s /usr/lib/insserv/insserv /sbin/insserv`。

请删除 `/etc/init.d/.legacy-bootordering` 以关闭 legacy mode，启用 legacy mode 也可能导致启动顺序不正确。

不要忘了更改文件权限为 755（`-rwxr-xr-x`）、所有者为 root、群组为 root。

你也可能需要更改程序路径、配置文件路径、日志文件路径或 pid 文件路径。

在一些 Linux 发行版中（如 Debian、Ubuntu 和 Fedora 等）使用 tmpfs 临时挂载 `/run`，必须在系统启动时把该目录的权限改为 777（`drwxrwxrwx`），才能使不以 root 身份运行的程序有权限在 `/var/run` 中创建 pid 文件。

比如在 `/etc/init/mounted-run.conf` 中加入 `chmod 777 /run`：

```shell
# mounted-run - Populate and link to /run filesystem
#
# Populates the /run filesystem and adds compatibility links to it

description "Populate and link to /run filesystem"

start on mounted MOUNTPOINT=/run TYPE=tmpfs

task

script
    chmod 777 /run

    : > "/run/utmp"
    chmod 664 "/run/utmp"
    chgrp utmp "/run/utmp"

    # compatibility; should go away soon
    [ -d /dev/.initramfs/varrun ] && cp -a /dev/.initramfs/varrun/* /run/ || true

    mkdir -p /run/sendsigs.omit.d

    # Background the initial motd seeding
    #[ -d "/etc/update-motd.d" ] && run-parts --lsbsysinit /etc/update-motd.d > /run/motd &
end script
```

### 注意

部分启动脚本可能与软件自带脚本冲突，如 rsync-server 和 rsync，munin-server 和 munin、munin-node，**请勿同时使用**！

### 参见

[制作 SysV 风格启动脚本的指南](https://zh.opensuse.org/openSUSE:Packaging_init_scripts)
