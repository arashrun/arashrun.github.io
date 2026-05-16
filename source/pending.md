
#### udev

[Writing udev rules](https://www.reactivated.net/writing_udev_rules.html)
[udev - ArchWiki - Arch Linux 教程](https://wiki.archlinux.org.cn/title/Udev)


*udev* 规则由管理员编写，存放在 `/etc/udev/rules.d/` 中，文件名必须以 _.rules_ 结尾。由各种软件包提供的 *udev* 规则位于 `/usr/lib/udev/rules.d/`。如果 `/usr/lib` 和 `/etc` 中存在同名文件，则 `/etc` 中的文件具有更高的优先级。

