# Linux 常用命令

## 1. 文件处理命令

### ls

`ls` + 选项 + 文件或目录名

`ll` 是 `ls -ld` 的别名

| 选项   | 英文原意     | 解释                 |
| ---- | -------- | ------------------ |
| `-a` | all      | 完整选项               |
| `-l` | long     | 详细信息               |
| `-h` | human    | 人性化显示文件大小 MB       |
| `-d` | direct   | 显示目录的纤细信息，但不显示下包文件 |
| `-i` | identity | 查询文件id (Inode)     |

- 输出说明：

![权限](img/权限.png)

### mkdir

`mkdir` + 选项 + 目录名

| 选项   | 作用                         |
| ---- | -------------------------- |
| `-p` | 递归创建目录，连续创建多条，也可空格分隔创建多条目录 |

### cd & pwd

`cd` + 目录名    #come into directory

`pwd`    #print work directory

- `.`    #present directory

- `..`    #last directory

### cp

`cp` + 选项 + 原文件or目录 + 目标目录

| 选项    | 作用                       |
| ----- | ------------------------ |
| `-r`  | 复制目录（递归复制）               |
| `-p`  | 保留文件属性（在文件复制时保持文件修改时间不变） |
| `-rp` | 最常用的复制命令组合选项             |

### rm & mv

`rm` + 选项 + 文件名or目录名

| 选项    | 作用                      |
| ----- | ----------------------- |
| `-r`  | 删除目录（递归删除，会将目录下的文件一并删除） |
| `-f`  | 不提示反悔操作                 |
| `-rf` | 最常用的删除目录命令              |

`rmdir` + 目录名    #移除空目录

`mv` + 原文件名 + 新文件名    #剪切移动or更名

### ln

软链接：

- 最常见的 777 权限文件（lrwxrwxrwx）

- 相当于快捷方式

- `ln -s` + 原文件名 + 新文件名

硬链接：

- `ln` + 原文件名 + 新文件名

- 相当于`cp -p` + 实时更新，硬链接的两个文件的inode一样

### touch

创建新文件：不建议文件名带有空格

- `touch` + 文件名

### cat & more & less

`cat -n` + 文件名    #查看文件，并显示行号

`tac` + 文件名    #倒着显示文件

`more` + 文件名    #查看文件，分页显示，只能下翻

| `more`交互按钮 | 作用  |
| ---------- | --- |
| 空格 or `f`  | 翻页  |
| 回车         | 换行  |
| `q`        | 退出  |

`less` + 文件名    #查看文件，分页显示，可以随意翻

| `less` 多于 `more`的交互按钮 | 作用          |
| --------------------- | ----------- |
| PgUp                  | 向上翻页        |
| 上箭头                   | 向上换行        |
| `/` + 搜索内容            | `n` next下一条 |

### head & tail

`head` + 文件名

- 默认查看文件前10行

- `-n7`    #查看前7行

`tail` + 文件名

- `-n7`    #查看后7行 

- `-f`     #动态显示文件内容（用于监控日志）

## 2. 权限管理命令

### chmod

change the permissions of a file

- `chmod` + `[{ugoa}{+-=}{rwx}]` + 文件or目录
  
  - 同时为多种用户授权时 用`,`分隔

- 权限的数字表示：

| 权限       | 十进制 | 二进制 |
| -------- | --- | --- |
| r        | 4   | 100 |
| w        | 2   | 010 |
| x        | 1   | 001 |
| rwx      | 7   | 111 |
| rw-      | 6   | 110 |
| r-x      | 5   | 101 |
| -wx 几乎没有 | 3   | 011 |

- `chmod 777` + 目录or文件

- `chmod -R` + 目录    #递归修改目录及包含所有的文件权限 

- 对权限的理解：
  
  - file：
    
    - r：cat/more/less/head/tail
    
    - w：vim
    
    - x：script command
  
  - directory：
    
    - r：ls
    
    - w：touch/mkdir/rmdir/rm    #删除文件需要对目录有w权限而不是对文件
    
    - x：cd 

### chown

change file ownership    #only root

- `chown` + 用户 + 文件or目录    #改变文件or目录所有者

- `chgrp` + 组 + 文件or目录    #改变文件or目录所属组

### umask

新建目录或文件的缺省权限

- `umask -S`：查看缺省权限（文件不能创建x权限，只能赋予x权限

- `umask 002` 所对应的文件和目录创建缺省权限分别为`664(666-2)`和`775(777-2)`

## 3. 文件搜索命令

### find

`find` + 搜索范围 + 匹配条件 + 匹配内容

- 匹配内容：`*`匹配任意字符，`?`匹配单个字符

| 匹配条件                 | 作用          | 备注                                          |
| -------------------- | ----------- | ------------------------------------------- |
| `-name`              | 根据名称搜索      |                                             |
| `-iname`             | 不区分大小写      |                                             |
| `-size`              | 大小          | `+`大于 `-`小于 单位为数据块，1块数据块为 512 Byte 或 0.5 KB |
| `-user`              | 所有者         |                                             |
| `-group`             | 所属组         |                                             |
| `-amin`              | 访问时间        | access                                      |
| `-cmin`              | 文件属性        | change                                      |
| `-mmin`              | 文件内容        | modify                                      |
| `-o`                 | or          | 连接两个条件                                      |
| `-a`                 | and         | 连接两个条件                                      |
| `-type`              | 文件类型        |                                             |
| `-inum`              | 文件inode     |                                             |
| `-exec` + 命令 + `{}\` | 对搜索结果执行‘命令’ | 与其他条件直接连用，没有确认环节                            |
| `-ok` + 命令 + `{}\`   | 对搜索结果执行‘命令’ | 与其他条件直接连用，有确认环节                             |

eg：

```bash
find /etc -name init    #精确查找 /etc 下 name 包含 init 的文件
find /etc -size +20480    #大小超过 10G 的文件
find /etc -user Car    #所有者为 Car 的文件
find /etc -group Car    #所属组为 Car 的文件
find /etc -cmin -5    #在5min之内属性被更改的文件
find /etc -size +40960 -a -size -204800    #大小处于20G与100G之间的文件
find /etc -name inittab -exec ls -l {}\    #找到inittab文件并执行 ls -l inittab
```

### locate

秒搜：在`/var/lib/mlocate/mlocate.db`处维护了一个哈希表

- `locate` + 查找内容

- find 是遍历搜索，耗费系统资源

- 无法寻找 `/tmp` 下的文件

- 使用前最好 `updatedb` 更新哈希表，`locate` 才可以查到最新资料

- `locate -i` 不区分大小写查找

### which & whereis

`which` + 命令    #搜索命令所在目录及别名信息（查看命令有无别名）

- eg：`rm` = `rm -i`    #默认remove文件要inquiry

`whereis` + 命令    #搜索命令所在目录及帮助文档所在目录

### grep

`grep` + 选项 + 搜索字串 + 文件名

| 选项   | 作用      |
| ---- | ------- |
| `-i` | 不区分大小写  |
| `-v` | 排除制定字符串 |

eg

- grep mysql /root/install.log    #在install.log中寻找mysql所在行，并打出

- grep -v ^# /etc/initab    #在initab中打印除了以#开头的内容

## 4. 帮助命令

### man & whatis

manual 手册（手工的）

- `man` + 命令    #查看命令的帮助文件，可以用`/ 字串` 查询

- `man 5` + 配置文件    #查看配置文件的帮助文档

eg：

- passwd 既有命令又有配置文件，若查询配置文件需要用 `man 5`

whatis 简短型帮助文件

- `whatis` + 命令

apropos 查看配置文件帮助信息

- `apropos` + 配置文件

### info & help

`info`与`man`相似并通用

命令 + `--help` #会列出所有选项

`help`    #有些命令是shell内置的，如cd,pwd,umask等，help可以查到这些，其实man和info也可以

## 5. 用户管理命令

### useradd & passwd

捆绑使用：

- `useradd` + 用户名    #增加新用户

- `passwd` + 用户名    #给用户设置密码

### who & w

`who`    #查看用户登录情况

- tty：本地登录

- pts：远程登录

`w`    #查看更多细节，与`uptime` 和 `top`命令结果第一行显示内容相近

| IDLE   | JCPU      | PCPU          |
| ------ | --------- | ------------- |
| 用户空闲时间 | 累积占用CPU时间 | 当前执行操作占用CPU时间 |

## 6. 压缩和解压命令

压缩：节省空间，不易被病毒侵染

linux支持格式：

- `.gz .zip .tar`    #zip linux与win通用

### zip & unzip & gzip & gunzip

`zip` + 选项 + 压缩后的文件名 + 目录或文件

| 选项   | 作用                 |
| ---- | ------------------ |
| 无参数  | 压缩文件               |
| `-r` | 压缩目录 recurrence 递归 |

`unzip` + 文件    #解压缩

`gzip` + 选项 + 文件名    #不可压缩目录，仅可压缩文件，不保留原文件

| 选项   | 作用  |
| ---- | --- |
| 无参数  | 压缩  |
| `-d` | 解压缩 |

`gunzip` + 文件名    #解压缩

### tar

`tar` + 选项 + 压缩后的文件名 + 待压缩的目录

| 选项     | 作用                    |
| ------ | --------------------- |
| `-c`   | 打包 create 创建新的 tar 文件 |
| `-v`   | 显示详细信息 verbose        |
| `-f`   | 指定压缩后的文件名 file        |
| `-z`   | 打包时同时压缩 zip           |
| `-x`   | 解包 extract            |
| `-zcf` | 最常用的打包压缩组合选项          |
| `-zxf` | 最常用的解压组合命令            |

### bzip2

`bzip2` + 选项 + 文件    #压缩比惊人

| 选项   | 作用    |
| ---- | ----- |
| `-k` | 保留原文件 |

### 压缩小结

| 文件格式       | 命令      | 压缩      | 解压缩                     | 限制              |
| ---------- | ------- | ------- | ----------------------- | --------------- |
| `.gz`      | `gzip`  | `gzip`  | `gunzip` or `(gzip -d)` | 不能压缩目录，不保留原文件   |
| `.tar`     | `tar`   | `-cf`   | `-xf`                   | 打包不压缩（对目录）      |
| `.tar.gz`  | `tar`   | `-zcf`  | `-zxf`                  | 打包压缩（对目录），压缩比惊人 |
| `.zip`     | `zip`   | `-r`    | `unzip`                 | 可压目录            |
| `.bz2`     | `bzip2` | `bzip2` | `bunzip2`               | 可保留原文件，不可压缩目录   |
| `.tar.bz2` | `tar`   | `-cjf`  | `-xjf`                  | 打包压缩 bz2 压缩比惊人  |

## 7. 网络命令

### write & wall

`write` + 用户   #给登录服务器的其他用户发消息

- 若输错退格需 `ctrl + Backspace`

- `ctrl + D` 保存结束

- 需 `w` 查看用户在不在线

`wall`    #等效于 `write all`

- `wall` + message    #发广播信息

### ping & ifconfig

`ping` + IP地址

- 会一直 ping 下去，直至 `ctrl + c` 停止

- `ping -c 3`    #只 ping 3 次，终止

- 关注的是 packet lost 丢包率，丢包率约高，说明网络情况越差

`ifconfig` + ethn + ip地址

- `ifconfig eth0 192.168.8.250`    #给以太网第一块网卡设置ip地址（Ethernet）

### mail

`mail` + 用户名    #发邮件

| 交互命令      | 作用    |
| --------- | ----- |
| `help`    | 看信息命令 |
| `h`       | 看列表   |
| `序列号`     | 查看邮件  |
| `d` + 序列号 | 删除    |
| `q`       | 退出    |

### last & traceroute

`last`    #统计所有用户全部时候的登录信息

- `lastlog`    #统计用户最后一次登录时间

- `lastlog -u` + UID    #某个用户最后一次访问时间

`traceroute` + 地址    #查看计算机访问另一台主机走的是什么路径

### netstat

`netstat` + 选项

| 选项   | 含义               |
| ---- | ---------------- |
| `-t` | TCP 协议，安全，三次握手   |
| `-u` | UDP 协议，快，连接可靠性不好 |
| `-l` | 监听               |
| `-r` | 路由               |
| `-n` | 显示 ip 地址和端口号     |

eg：

```bash
netstat -tlun    #TCP有监听
netstat -an    #查看所有监听信息
netstat -rn    #查询路由列表，最后面是网关
```

### setup

`setup`    #redhat专有命令，图形界面（永久生效）

- `service network restart`    #全新启动

### mount & umount

挂载

- `mount` + 设备文件 + 挂载点    #mount 登上骑上，手动挂载，分配盘符

卸载

- `umount` + 设备文件 + 挂载点    #注意不能在挂载目录下卸载

## 8. 关机与重启

### shutdown

服务器只允许重启而不能关机

`shutdown` + 选项 + now or 时间

| 选项   | 作用           |
| ---- | ------------ |
| `-h` | 立即关机 or 定时关机 |
| `-r` | 立即重启 or 定时重启 |
| `-c` | 取消前一个命令      |

类似命令有：

- `halt`    #停止

- `poweroff`

- `init 0`

### reboot & init & logout

`reboot`    #重启

`init` + 系统运行级别

| 系统运行级别 | 说明                                       |
| ------ | ---------------------------------------- |
| 0      | **关机**                                   |
| 1      | 单用户                                      |
| 2      | 不完全多用户，不含 NFS 服务(net file system 网络文件系统) |
| 3      | 完全多用户                                    |
| 4      | 未分配                                      |
| 5      | 图形界面                                     |
| 6      | **重启**                                   |

- init 命令中运行级别的查询
  - `runlevel`    #3 5 原来是3，当前是5  

`logout`    #退出登录界面
