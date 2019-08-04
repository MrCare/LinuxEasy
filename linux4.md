# User 配置文件

## 1. 用户配置文件

- 用户信息文件：`/etc/passwd`

- 影子文件： `/etc/shadow`

- 组信息文件：`/etc/group`；组密码文件：`/etc/gshadow`

`man 5 passwd`    #配置文件帮助文档

### /etc/passwd

- 权限：644

- 密码：`/etc/shadow` 

```bash
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
avahi-autoipd:x:170:170:Avahi IPv4LL Stack:/var/lib/avahi-autoipd:/sbin/nologin
systemd-bus-proxy:x:999:997:systemd Bus Proxy:/:/sbin/nologin
systemd-network:x:998:996:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:997:995:User for polkitd:/:/sbin/nologin
abrt:x:173:173::/etc/abrt:/sbin/nologin
libstoragemgmt:x:996:994:daemon account for libstoragemgmt:/var/run/lsm:/sbin/nologin
tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon:/dev/null:/sbin/nologin
ntp:x:38:38::/etc/ntp:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
chrony:x:995:993::/var/lib/chrony:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
syslog:x:994:992::/home/syslog:/bin/false
www:x:1000:1000::/home/www:/sbin/nologin
mysql:x:1001:1001::/home/mysql:/sbin/nologin
jupyter:x:1002:1002::/home/jupyter:/bin/bash
```

`wc` + 文件    #统计文件的行数，字符数，字节数

| 字段  | 说明                                                                       |
| --- | ------------------------------------------------------------------------ |
| 1   | 用户名称                                                                     |
| 2   | 密码标志                                                                     |
| 3   | UID<br/>`0`超级用户：系统识别为root用户的唯一标识<br/>`1-499`系统用户：伪用户<br/>`500-65535`普通用户 |
| 4   | GID：用户初始组ID                                                              |
| 5   | 用户说明                                                                     |
| 6   | 家目录<br/>普通用户：`/home/用户名`<br/>超级用户：`/root`                                |
| 7   | 登录之后的Shell                                                               |

初始组：

- 用户一建立，立刻拥有其相应组

- 每个用户必须有一个初始组，只能有一个初始组

附加组：

- 用户可以加入多个其他的用户组，并拥有这些组的权限，附加组可以有多个

Shell：

- 是Linux的命令解释器（可以更改）

### /etc/shadow

```bash
root:$6$qa.ysfoASlhY8XzN$hT7LYUemoQgHWMe3/p9h7MadW77vjCYulOLVbOckQGYMOy2fHcRCdJs1CF.w.LT7apD..GcXXm8qi3.6F9ksT1:18069:0:99999:7:::
bin:*:17246:0:99999:7:::
daemon:*:17246:0:99999:7:::
adm:*:17246:0:99999:7:::
lp:*:17246:0:99999:7:::
sync:*:17246:0:99999:7:::
shutdown:*:17246:0:99999:7:::
halt:*:17246:0:99999:7:::
mail:*:17246:0:99999:7:::
uucp:*:17246:0:99999:7:::
operator:*:17246:0:99999:7:::
games:*:17246:0:99999:7:::
gopher:*:17246:0:99999:7:::
ftp:*:17246:0:99999:7:::
nobody:*:17246:0:99999:7:::
dbus:!!:18069::::::
rpc:!!:18069:0:99999:7:::
vcsa:!!:18069::::::
abrt:!!:18069::::::
rpcuser:!!:18069::::::
nfsnobody:!!:18069::::::
haldaemon:!!:18069::::::
ntp:!!:18069::::::
saslauth:!!:18069::::::
postfix:!!:18069::::::
sshd:!!:18069::::::
tcpdump:!!:18069::::::
oprofile:!!:18069::::::
apache:!!:18069::::::
```

| 字段  | 说明                                                                                     |
|:--- | -------------------------------------------------------------------------------------- |
| 1   | 用户名                                                                                    |
| 2   | 加密密码<br/>加密算法升级为SHA512散列加密算法<br/>如果密码位是“!!”或“*”代表没有密码，不能登录<br/>可以在用户密码字段前加“!”临时禁用用户的密码 |
| 3   | 密码最后一次修改的日期<br/>使用1970年1月1日作为标准时间，每过一天，时间戳+1                                           |
| 4   | 两次密码的修改间隔（如果是0则随时都可以修改，如果是n则n天之后才可以修改）                                                 |
| 5   | 密码有效期，在最后一次修改密码之后密码的有效期限                                                               |
| 6   | 密码达到有效期前的警告天数（和第五字段相比）                                                                 |
| 7   | 密码过期后的宽限天数（和第五字段相比）<br/>0：代表密码过期立即失效 <br/>-1：代表密码永远不会失效                                |
| 8   | 帐号失效时间：用时间戳表示（强制执行）                                                                    |
| 9   | 保留                                                                                     |

### /etc/group

```bash
root:x:0:
bin:x:1:bin,daemon
daemon:x:2:bin,daemon
sys:x:3:bin,adm
adm:x:4:adm,daemon
tty:x:5:
disk:x:6:
lp:x:7:daemon
mem:x:8:
kmem:x:9:
wheel:x:10:
mail:x:12:mail,postfix
uucp:x:14:
man:x:15:
games:x:20:
gopher:x:30:
video:x:39:
dip:x:40:
ftp:x:50:
lock:x:54:
audio:x:63:
nobody:x:99:
users:x:100:
dbus:x:81:
rpc:x:32:
utmp:x:22:
utempter:x:35:
floppy:x:19:
vcsa:x:69:
abrt:x:173:
cdrom:x:11:
tape:x:33:
```

| 字段  | 说明      |
| --- | ------- |
| 1   | 组名      |
| 2   | 组密码标志   |
| 3   | GID     |
| 4   | 组中的附加用户 |

### /etc/gshadow

| 字段  | 说明      |
| --- | ------- |
| 1   | 组名      |
| 2   | 组密码     |
| 3   | 组管理员用户名 |
| 4   | 组中附加用户  |

- 看组的初始用户要去`/etc/passwd` 中看GID字段

- 手工更改4个文件一样可以添加过修改用户

## 2. 用户管理的相关文件

用户的家目录

- 普通用户：`/home/用户名`，所有者和所属组都是此用户，权限700 $

- root用户：`/root` 所有者和所属组都是root用户，权限是500#

用户的邮箱

- `/var/spool/mail/用户名`

用户模板目录

- 新建用户的家目录里会存在此目录下的文件

- `/etc/skel`

## 3. 用户的管理命令

### useradd

`useradd` + 选项 + 用户名

| 选项           | 作用                          |
| ------------ | --------------------------- |
| `-u` + UID   | UID 手工指定用户的UID号             |
| `-d` + 家目录   | 手工指定用户家目录                   |
| `-c` + 用户说明  | 备注信息                        |
| `-g` + 组名    | 指定用户初始组                     |
| `-G` + 组名    | 指定用户附加组                     |
| `-s` + shell | 指定用户登录的shell，默认`/bin/shell` |

配置文件

- `/etc/default/useradd`

```bash
# useradd defaults file
GROUP=100    #用户默认组

HOME=/home    #用户家目录

INACTIVE=-1    #密码过期宽限天数（shadow文件7字段）

EXPIRE=    #密码失效时期

SHELL=/bin/bash    #默认shell

SKEL=/etc/skel    #模板目录

CREATE_MAIL_SPOOL=yes    #是否建立邮箱
```

- `/etc/login.defs`

```bash
#
# Please note that the parameters in this configuration file control the
# behavior of the tools from the shadow-utils component. None of these
# tools uses the PAM mechanism, and the utilities that use PAM (such as the
# passwd command) should therefore be configured elsewhere. Refer to
# /etc/pam.d/system-auth for more information.
#

# *REQUIRED*
#   Directory where mailboxes reside, _or_ name of file, relative to the
#   home directory.  If you _do_ define both, MAIL_DIR takes precedence.
#   QMAIL_DIR is for Qmail
#
#QMAIL_DIR      Maildir
MAIL_DIR        /var/spool/mail
#MAIL_FILE      .mail

# Password aging controls:
#
#       PASS_MAX_DAYS   Maximum number of days a password may be used.
#       PASS_MIN_DAYS   Minimum number of days allowed between password changes.
#       PASS_MIN_LEN    Minimum acceptable password length.
#       PASS_WARN_AGE   Number of days warning given before a password expires.
#
PASS_MAX_DAYS   99999    #密码有效期(5)

PASS_MIN_DAYS   0    #密码修改间隔(4)

PASS_MIN_LEN    5    #密码最小5位(PAM)

PASS_WARN_AGE   7    #密码到期警告(6)


#
# Min/max values for automatic uid selection in useradd
#
# encrypt 加密
```

### passwd

`passwd` + 选项 + 用户名

| 选项       | 作用                 |
| -------- | ------------------ |
| `-S`     | 查询用户密状态，仅root可用    |
| `-l`     | 暂时锁定用户，仅root可用     |
| `-u`     | 解锁用户，仅root可用       |
| `-stdin` | 可以通过管道符输出的数据作为用户密码 |

```bash
echo "密码" | passwd --stdin 用户    #批量设置密码，编程时可用
```

`whoami`    #查询当前用户

### usermod 修改用户信息

`usermod` + 选项 + 用户名

| 选项          | 作用          |
| ----------- | ----------- |
| `-u` + UID  | 修改用户的 UID 号 |
| `-c` + 用户说明 | 用户说明        |
| `-G` + 组名   | 修改用户附加组     |
| `-L` + 用户名  | 临时锁定用户      |
| `-U` + 用户名  | 解锁用户        |

### chage 修改用户密码状态

`chage` + 选项 + 用户名

| 选项   | 作用                        |
| ---- | ------------------------- |
| `-l` | 列出用户的详细密码状态               |
| `-d` | 修改密码组后一次更改日期 (shadow 3字段) |
| `-m` | 两次密码修改时间间隔(4)             |
| `-M` | 密码有效期(5)                  |
| `-W` | 过期前警告天速(6)                |
| `-I` | 过期后宽限天数(7)                |
| `-E` | 帐号失效时间(8)                 |

```bash
chage -d 0 用户    #相当于把密码修改日期归零，用户一登录就要修改密码(用于保证系统安全性)
```

### userdel

`userdel -r` + 用户名    #删除用户的同时删除用户家目录

手工删除用户：要删除6个文件

```bash
/etc/passwd/
/etc/shadow/
/etc/group/
/etc/gshadow/
/var/spool/mail/用户
/home/用户
```

### id & su

`id` + 用户名    #查询用户UID，GID，所属组ID

`su` + 选项 + 用户名    #用户身份切换

| 选项        | 作用                   |
| --------- | -------------------- |
| `-`       | 代表连带用户的环境变量一起切换，不可省略 |
| `-c` + 命令 | 仅执行一次命令，而不切换用户身份     |

```bash
su - root -c "useradd usernew"
```

## 4. 用户组管理命令

### groupadd

`groupadd` + 选项 + 组名

| 选项         | 作用    |
| ---------- | ----- |
| `-g` + GID | 指定组ID |

### groupmod

`groupmod` + 选项 + 组名

| 选项         | 作用    |
| ---------- | ----- |
| `-g` + GID | 修改组ID |
| `-n` + 新组名 | 修改组名  |

### groupdel

`groupdel` + 组名

- 组如果是初始组，要删除必须先删除组中用户

### gpasswd

`gpasswd` + 选项 + 组名    #把用户添加入组或从组中删除

| 选项         | 作用       |
| ---------- | -------- |
| `-a` + 用户名 | 把用户加入组   |
| `-d` + 用户名 | 把用户从组中删除 |


