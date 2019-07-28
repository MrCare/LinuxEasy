# Shell 编程

## 1. 基础正则表达式

### 1.1 正则表达式与通配符

正则表达式：

- 用来在文件中匹配符合条件的字符串

- 正则是**包含匹配**，grep, awk, sed 等命令可以支持正则表达式

通配符：

- 用来匹配符合条件的文件名，通配符是**完全匹配**

- ls, find, cp, 这些命令不支持正则表达式，只能用shell自己的通配符进行匹配

基础正则表达式：

| 元字符       | 作用                                                             |
|:--------- |:-------------------------------------------------------------- |
| *         | 前一个字符匹配0次或任意多次                                                 |
| .         | 匹配除了换行符外的任意一个字符                                                |
| ^         | 匹配行首，如^#会匹配以#开头的行                                              |
| $         | 匹配行尾                                                           |
| []        | 匹配中括号中指定的任意一个字符，只匹配一个字符                                        |
| [^]       | 匹配除中括号以外的任意一个字符                                                |
| \         | 转义符                                                            |
| \\{n\\}   | 表示其前面字符恰好出现n次，如[0-9]\\{n\\} 匹配4位数字，[1][3-8][0-9]\\{9\\} 匹配手机号码 |
| \\{n,\\}  | 表示其前面的字符出现不小于n次                                                |
| \\{n,m\\} | 表示其前面字符出现至少n次，至多m次                                             |

- “*”前一个字符匹配0次，或任意多次
  
  - grep “a*” file.txt  #匹配所有内容
  
  - grep “aa*” file.txt  #匹配至少包含一个a的行

- “.”匹配除了换行符外任意一个字符
  
  - grep "s..d" file.txt  #匹配在s和d这两个字母之间一定有两个字符的单词
  
  - grep “s.*d” file.txt  #匹配在s和d字母之间有任意字符

- "^"匹配行首，"$"匹配行尾
  
  - grep “^M” file.txt  #匹配以M开头的行
  
  - grep “n$“ file.txt  #匹配以n结尾的行
  
  - grep -n "^$" file.txt  #匹配空白行，并显示空白行行号

- 正则的包含匹配让 \\{2\\} 与 \\{2,\\} 几乎没有区别

## 2. 字符截取命令

### 2.1 cut 字段提取命令

`cut` + 选项 + 文件名

* -f + n   #n为第几列

* -d + ”分隔符“    #按分隔符提取列
- eg：cat /etc/passwd | grep /bin/bash | grep -v root | cut -d ":" -f 1    #提取除 root 以外的所有用户

- 局限：
  
  - 不能很好的识别以空格为分隔符的字段
  
  - 解决方案：awk

### 2.2 printf 命令

`printf` + ‘输出类型输出格式’ + 输出内容    #不能加文件名或接收管道符

| 输出类型  | 作用                                          |
| ----- | ------------------------------------------- |
| %ns：  | 输出字符串，n是数字指代输出几个字符                          |
| %ni   | 输出整数，n是数字指代输出几个数字                           |
| %m.nf | 输出浮点数，m是整数位数，n是小数位数，如 %8.2f，代表8位整数，两位小数的浮点数 |

| 输出格式 | 作用                 |
| ---- | ------------------ |
| \a   | 输出警告声音             |
| \b   | 输出退格键，也就是Backspace |
| \f   | 清除屏幕               |
| \n   | 换行                 |
| \r   | 回车                 |
| \t   | 水平输出 Tab 键         |
| \v   | 竖直输出 Tab 键         |

### 2.3 awk 命令

`awk` + '条件1{动作1}条件2{动作2}...' + 文件名

- 条件：一般使用关系表达式作为条件
  
  - x > 10    #判断变量 x 是否大于 10

- 动作：（Action）：
  
  - 格式化输出
  
  - 流程控制语句

- eg：awk ‘{printf $2 "\t"}’ file.txt    #输出 file.txt 的第 2 列

- awk 默认识别的分隔符为：制表符与空格

**条件**：BEGIN 在所有的数据处理之前执行

FS 内置变量：制定分隔符

- awk 'BEGIN{FS=":"}{print $1 "\t"}'

- awk 'BEGIN{FS=":"}{printf $1 "\t" "\n"}'

- linux 中只有printf，而awk中可支持print与printf，区别在于print会在行尾自动加"\n"

- 如果不加 BEGIN awk会先执行后面的处理，结果是，数据第一行不会按分隔符正确分割

**条件**：END 在所有的数据处理之后

### 2.4 sed 命令

sed 命令：

- sed 是一种几乎包括在所有 UNIX 平台（包括 Linux）的轻量级流编辑器。sed 主要是用来将数据进行选取，替换，删除，新增的命令

- 与 vim 相比的优势：可以接收管道符输入

`sed` + 选项 + ‘动作’ + 文件名

- 选项：

| 选项  | 作用                                              |
|:--- |:----------------------------------------------- |
| -n  | 一般sed命令会把所有数据输出到屏幕，如果加入此选项，则只会把经过sed命令处理的行输出到屏幕 |
| -e  | 允许对输入数据应用多条sed命令编辑                              |
| -i  | 用 sed 的修改结果直接修改读取数据的文件，而不是有屏幕输出                 |

- 动作：

| 动作  | 作用                                                    |
| --- | ----------------------------------------------------- |
| a\  | 追加，在当前行后添加一行或多行，行末尾“\”代表数据未完结                         |
| c\  | 行替换，用c后面的字符串替换原数据行，行末尾“\”代表数据未完结                      |
| i\  | 插入，在当前行插入一行或多行，行末尾“\”代表数据未完结                          |
| d   | 删除，删除指定行                                              |
| p   | 打印，输出指定行                                              |
| s   | 字串替换，用一个字符串替换另一个字符串，格式为“行范围s/旧字串/新字串/g”（与vim中的替换格式类似） |

eg：

- sed '2p' file.txt    #输出 file.txt 的全部行

- sed -n '2p' file.txt    #只输出 file.txt 的第二行

- sed '2,4d' file.txt    #删除第二行到第四行但不修改文件本身

- sed ‘2a hello’ file.txt    #在第二行之后追加 hello

- sed ‘2a hello \ world’ file.txt    #在第二行前插入两行数据

## 3. 字符处理命令

排序命令 sort：

- `sort` + 选项 + 文件名

| 选项     | 文件名                              |
| ------ | -------------------------------- |
| -f     | 忽略大小写                            |
| -n     | 以数值型进行排序（默认是字符串型）                |
| -r     | 反向排序                             |
| -t     | 指定分隔符，默认是分隔符是制表符                 |
| -k n,m | 按照指定的字段范围排序，从第n字段开始，m字段结束（默认到行尾） |

- eg：
  
  - sort /etc/passwd    #排序用户信息文件
  
  - sort -r /etc/passwd    #反向排序
  
  - sort -n -t ":" -k 3,3 /etc/passwd    #按照第3字段进行数值型排序

统计命令 wc：

- `wc` + 选项 + 文件名

| 选项  | 作用     |
| --- | ------ |
| -l  | 只统计行数  |
| -w  | 只统计单词数 |
| -m  | 只统计字符数 |

## 4. 条件判断

按照文件类型进行判断：

- 两种使用方式
  
  - test -e file.txt
  
  - [ -e file.txt ]

- 输入上述命令后还要用 echo $? 若输出 0 则上条命令正确执行，文件存在

eg：

- [ -e file.txt ] && echo "yes || echo "no"

| 测试选项              | 作用                      |
| ----------------- | ----------------------- |
| **-d** + file.txt | 判断文件是否存在，且是否为目录文件，是目录为真 |
| **-e**            | 判断是否存在，存在为真             |
| **-f**            | 判断是否存在，且是否为普通文件，是普通文件为真 |
| -L                | 判断是否存在，且是否为符号链接文件       |
| -p                | 判断是否存在，且是否为管道文件         |
| -s                | 判断是否存在，且是否为非空           |
| -S                | 判断是否存在，且是否为套接字文件        |
| -b                | 判断是否存在，且是否为块设备文件        |
| -c                | 判断是否存在，且是否为字符设备文件       |

按照文件权限进行判断：

| 测试选项              | 作用                    |
| ----------------- | --------------------- |
| **-r** + file.txt | 判断文件是否存在并是否有读权限       |
| **-w**            | 判断文件是否存在并是否有写权限       |
| **-x**            | 判断文件是否存在并是否有执行权限      |
| -u                | 是否存在并有 SUID 权限（灵魂附体）  |
| -g                | 是否存在并有 SGID 权限（灵魂附体）  |
| -k                | 是否存在并有 SBit 权限 （天网恢恢） |

两个文件间的比较：

| 测试选项                   | 作用                                                      |
| ---------------------- | ------------------------------------------------------- |
| file1.txt -nt file.txt | 判断 file1.txt 的修改时间是否比 file2.txt 的新                      |
| -ot                    | 判断 1 的修改时间是否比 2 的旧                                      |
| -ef                    | 判断 1 与 2 的 Inode 号是否一致，可以理解为两个文件是否为同一个文件，这个是用来判断硬链接的好方法 |

两个整数之间的比较：

| 测试选项          | 作用               |
| ------------- | ---------------- |
| int1 -eq int2 | int1 与 int2 是否相等 |
| -ne           | 是否不等             |
| -gt           | int1 是否大于 Int2   |
| -lt           | 是否小于             |
| -ge           | 是否大于等于           |
| -le           | 是否小于等于           |

两个字符串之间的比较：

| 测试选项         | 作用              |
| ------------ | --------------- |
| -z 字符串       | 判断字符串是否为空       |
| -n 字符串       | 是否为非空           |
| str1 == str2 | str1 是否等于 str2  |
| str1 != str2 | str1 是否不等于 str2 |

eg：

- [ -z "$str" ] && echo yes || echo no    #判断 str 变量是否为空

多重条件判断：

| 测试选项       | 作用                      |
| ---------- | ----------------------- |
| 判断1 -a 判断2 | 逻辑与，判断1与判断2都成立，最终的结果才为真 |
| 判断1 -o 判断2 | 逻辑或，有一个成立，最终的结果就为真      |
| ！判断        | 逻辑非，使原始判断式取反            |

eg：

- [ -z "$int1" -a "\$int1" -gt “$int2” ] && echo yes || echo no    #判断 int1 存在且大于 int2 这个命题是真是假

## 5. 流程控制

### 5.1 if 语句

**单分支if条件语句**

```bash
if [条件判断式];then
    程序
fi
```

或者

```bash
if [条件判断式]
    then
        程序
fi
```

- 注意：
  
  - [ 条件判断式 ]就是使用 test 命令判断，中括号和条件判断式之间必须有空格

- eg：判断分区使用率脚本

```bash
#!/bin/bash
#Author: Mr.Car

rate=$(df -h | grep "/dev/sda3" | awk '{print $5}' | cut -d "%" -f1) #把根分区使用率作为变量值赋予变量rate

if [ $rate -ge 80 ];then
    echo "Warning! /dev/sda3 is full"
fi
```

**双分支 if 条件语句**

```bash
if [ 条件判断式 ]
    then
        条件成立时执行的程序
    else
        条件不成立时执行的程序
fi
```

- eg1：备份数据库

```bash
#!/bin/bash
#备份mysql数据库

ntpdate asia.pool.ntp.org &> /edv/null  #同步系统时间
date=$(date +%y%m%d)  #把当前系统时间按照“年月日”格式赋予变量date
size=$(du -sh /var/lib/mysql)  #统计数据库的大小，并把大小赋予size变量

if [ -d /tmp/dbbak ]
    then
        echo "Date : $date!" > /tmp/dbbak/dbinfo.txt  #覆盖
        echo "Data size : $size" >> /tmp/dbbak/dbinfo.txt  #追加
        cd /tmp/dbbak
        tar -zcf mysql-lib-$date.tar.gz /var/lib/mysql dbinfo.txt &> /dev/nul  #把mysql与dbinfo.txt打包；输出丢入回收站
        rm -rf /tmp/dbbak/dbinfo.txt
    else
        mkdir /tmp/dbbak
        echo "Dare : $date!" > /tmp/dbbak/dbinfo.txt
        cd /tmp/dbbak
        tar -zcf mysql-lib-$date.tar.gz /var/lib/mysql dbinfo.txt &> /dev/null
        rm -rf /tmp/dbbak/dbinfo.txt #其实可以把重复的语句拿出来
fi
```

- eg2：判断 apache 是否启动
  - `nmap` -sT 127.0.0.1   #远程扫描开启的服务

```bash
#!/bin/bash
#判断 apache 是否启动

port=$(nmap -sT 127.0.0.1 | grep tcp | grep http | awk '{print $2}')    #使用 nmap 命令扫描服务器，并截取 apache 服务状态，赋予变量 port

if [ "$port" == "open" ]
    then
        echo "$(date) httpd is ok!" >> /tmp/autostart-acc.log
    else
        /etc/rc.d/init.d/httpd start &> /dev/null
        echo "$(date) restart httpd !!" >> /tmp/autostart-err.log
fi
```

**多分支 if 语句**

```bash
if [ 条件判断式1 ]
    then
        程序1
elif
    then
        程序2
else
    程序n
fi
```

- eg：判断用户输入是什么文件

```bash
!#/bin/bash
#判断用户输入什么文件

read -p "Please input a filename:" file  #接收键盘输入并赋予变量file

if [ -z "$file" ]  #判断是否为空
    then
        echo "please input a filename"
        exit 1  #报错跳出并返回1，可以确定跳出位置
elif [ ! -e "$file" ]  #判断是否不存在
    then
        echo "file not exit"
        exit 2
elif [ -f "$file" ]  #判断file的值是否为普通文件
    then
        echo "regular file"
elif [ -d "$file" ]  #判断file的值是否为目录文件
    then
        echo "directory file"
else
        echo "other file"
fi
```

### 5.2 case 语句

多分支 case 条件语句

- case 与 if 的区别是：case 只能判断一种条件关系，而 if 语句可以判断多种条件关系

```bash
case $variable in
    "value1")
        program1
        ;;
    "value2")
        program2
        ;;
    *)  #注意*不能加""
        program3
        ;;
esac
```

### 5.3 for 循环

语法1：

```bash
for variable in value1 value2 value3...
    do
        program
    done
```

- eg：批量解压缩脚本

```bash
#!/bin/bash

cd /lamp
ls *.tar.gz > ls.log
for i in $(cat ls.log)
    do
        tar -zxf $i &> /dev/null
    done
rm -rf /lamp/ls.log
```

语法2：

```bash
for ((init;condition;change))
    do
        program
    done
```

- eg：从1加到100

```bash
#!/bin/bash
# add 1,2..100

s = 0
for ((i=1;i<=100;i=i+1))
    do
        s=$(($s+$i))  #数值相加
    done
echo "$s"
```

- eg：批量添加用户

```bash
#!/bin/bash
#批量添加用户

read -p "please input user name:" -t 30 name  #等待30s
read -p "please input the number of users:" -t 30 num
read -p "please input the password of users:" -t 30 pass
if [ ! -z "$name" -a ! -z "$num" -a ! -z "$pass" ]
    then
        y=$(echo $num | sed 's/^[0-9]*$'//g)  #用正则替换所有以数字开头的字符串
        if [ -z "$y" ]  #如果所有都被替换，则说明输入只有数字
            then
                for((i=1;i<=$num;i=i+1))
                    do
                        useradd $name$i &> /dev/null
                        echo $pass | /usr/bin/passwd --stdin "$name$i" &> /dev/null  #此处必须用引号括起，否则会报错，系统会把$name$i当作另一个用户
                    done
        fi
fi
```



### 5.4 while 循环 & until 循环

while 循环：

```bash
while [ condition ]
    do
        program
    done
```

- eg：从1加到100

```bash
#!/bin/bash
#add 1,2,...100

i=1
s=0
while [ "$i" -le 100 ]
    do
        s=$(($s+$i))
        i=$(($i+1))
    done
echo "$s"
```

until 循环

```bash
until [ condition ]
    do
        program
    done
```

- eg：从1加到100

```bash
#!/bin/bash
#add 1,2,...100

i=1
s=0
while [ "$i" -gt 100 ]
    do
        s=$(($s+$i))
        i=$(($i+1))
    done
echo "$s"
```




