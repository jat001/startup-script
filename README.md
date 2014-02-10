Linux Init Scripts
====================

Some simple but useful init scripts in `/etc/init.d`.

You need to add them to system services using `chkconfig`, `sysv-rc-conf` or `update-rc.d`.

Don't forget change the file permissions to 755 (`-rwxr-xr-x`), change the owner to root and change the group to root.

Maybe you also need to modify the program path, the configuration file path, the log file path or the pid file path.

In Some Linux distributions (such as Debian, Ubuntu, Fedora, etc), they use tmpfs to mount `/run` temporarily. You must change the folder permissions to 777 (`drwxrwxrwx`) at system startup to make the programs have permissions to create a pid file in `/var/run`.

For example, add `chmod 777 /run` in `/etc/init/mounted-run.conf`:

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

### See also

[The Guidelines for SysV-style Initscripts](https://en.opensuse.org/openSUSE:Packaging_init_scripts)


Linux 启动脚本
=============

一些简单但有用的启动脚本，放在 `/etc/init.d` 中。

你需要使用 `chkconfig`, `sysv-rc-conf` 或 `update-rc.d` 添加它们到系统服务中。

不要忘了更改文件权限为 755（`-rwxr-xr-x`）、所有者为 root、群组为 root。

你也可能需要更改程序路径、配置文件路径、日志文件路径或 pid 文件路径。

在一些 Linux 发行版中（如 Debian、Ubuntu 和 Fedora 等）使用 tmpfs 临时挂载 /run，必须在系统启动时把它的权限改为 777（`drwxrwxrwx`）才能使程序有权限在 `/var/run` 中创建 pid 文件。

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

### 参见

[制作 SysV 风格启动脚本的指南](https://zh.opensuse.org/openSUSE:Packaging_init_scripts)
