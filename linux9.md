# Linux 服务管理

## 1. 服务简介与分类

服务分类

![Linux 服务](img/服务.png)

启动与自启动

- 服务启动：在当前系统中让服务运行，并提供功能

- 服务自启动：服务随着系统启动而自动启动

查询已安装服务

- `chkconfig --list`    #查看服务自启动状态，可以看到所有RPM包安装的服务

- 在 `/usr/local` 下查询，一般源码包会安装在这里，如 Python3

- `ps aux` 查看进程，看到所有目前已经启动的服务

- `service httpd start`     #service 会搜索 etc/rc.d/init.d/ 并启动，如果想用 service 启动服务可以在此目录下建立软链接

## 2. RPM 包安装服务的管理

### 2.1 独立的服务管理

重要路径：

- `/etc/init.d/` 独立的服务启动脚本位置

- `/etc/sysconfig/` 初始化环境配置文件位置

- `/etc/` 配置文件位置

- `/etc/xinetd.config` xinetd 配置文件

- `/etc/xinetd.d/` 基于 xinetd 服务的启动脚本

- `/var/lib/`服务产生的数据放在这里

- `/var/log/`日志

独立服务的启动方法

- `/etc/init.d/name start` (stop, status, restart)

- `service name start` (stop, status, restart)

- `service --status-all` 查看正在运行的 rpm 包安装的服务状态

独立服务的自启动方法

- `chkconfig --level 2345 http on`     #2345 运行级别；on开启，off关闭

- 修改`/etc/rc.d/rc.local` 文件，将服务的正确启动命令写入该文件，服务就会开机自启

- 使用 ntsysv 命令管理自启动，有图形界面（Red hat 专有命令）

### 2.2 基于 xinetd 服务管理

xinetd 与 telnet

- 名：超级守护进程

- 由于不安全已经被淘汰，目前已经被 SSH（Secure Shell）取代

## 3. 源码包安装服务的管理

源码包安装服务的启动

- 绝对路径，调用启动脚本，可以查看 install 脚本查询启动脚本

- `/usr/local/apache2/bin/apachectl start` (stop)

自启动

- 修改`/etc/rc.d/rc.local` 文件，将服务的正确启动命令写入该文件

利用 service 启动服务

- service 会搜索 `etc/rc.d/init.d/`并启动，如果想用 service 启动服务可以在此目录下建立软链接

利用 chkconfig 与 ntsysv 命令管理

```bash
vim /etc/init.d/apache
#chkconfig:35 86 76
#制定 httpd 脚本可以被 chkconfig 命令管理，格式是 chkconfig：运行级别 启动顺序(不能与系统现有顺序重复) 关闭顺序(不能与系统现有顺序重复)

chkconfig --add apache
chkconfig --list | grep apache    #查看list
```
