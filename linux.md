### 端口

```shell
#显示端口占用情况
netstat -ano
    -a	显示所有连接all
    -n	以数字形式显示地址和端口号
    -o	显示拥有的与每个连接关联的进程id

#windows下
netstat -ano | findstr 端口号
#linux下
netstat -ano | grep 端口号


```



### 文件搜索

#### 文件名搜索-find

find搜索在分区和硬盘中查找，较为耗资源

```shell
	find [搜索范围] [匹配条件]
    #在目录etc下搜索名称为init的文件
    find /etc -name init 
    #不区分文件名大小写搜索文件
    find /etc -iname init 

```

#### 文件搜索-find

```shell
#搜索大于100m的文件，这里是以数据块为单位的，一个数据块为512字节,0.5k，+号代表大于，-号表示小于
find / -size +204800
#根据所有者来查找
find /home -user hdt
#根据时间属性来查找,在etc目录下，5分钟内被修改的文件，-5表示五分钟以内，+5表示超过5分钟
find /etc -cmin -5
	-amin	访问时间被改变
	-cmin	文件属性被改变
	-mmin	文件内容被改变
#表示两个条件同时满足，这里-a表示并且的意思，大于80m且小于100m
find /etc -size +16384 -a -size -204800
#找出etc目录下名称为init开头的文件，如果-type后为d则找出来的都是文件夹
find /etc -name init* -a -type f
	-type	根据文件类型查找
		f	文件
		d	目录
		l	软链接文件
		
#删除当前目录下inum值为1123的文件，有时文件名称很奇怪可以通过inum来获取值来删除
#可通过ls -i查看inum值
find . -inum 1123 -exec rm {} \;
	-inum	根据i节点查找
	
	
#在etc目录下找到名称为init的文件，并使用ls显示出所有信息，最后面的{} \;属于固定格式
find /etc -name init -exec ls -l {} \;
#在etc目录下找到名称为init的文件，并使用rm删除，在删除的时候-ok命令会提示确认
find /etc -name init -ok rm  {} \;

```

#### 文件搜索-local

local搜索，在文件资料库文件``/var/lib/mlocate/mlocate.db``中维护了文件信息，搜索较快。如果文件信息没有被收录到文件中，tmp目录下的信息没有被收录到数据库文件，则搜索失败；

```shell
#更新资料库文件
updatedb
#搜索名称为init
local init
#搜索名称为init,不区分文件名大小写
local -i init
```

#### 文件搜索-which

查找命令所在的目录

```shell
which cp
which rm
```

#### 文件搜索-whereis

查找命令所在的目录，并且显示相关命令的帮助文档路径，配置文件所在路径

```shell
whereis cp
whereis rm
```

#### 文件搜索-grep

查找命令所在的目录，并且显示相关命令的帮助文档路径

```shell
#在demo.py文件中查找包含字符串mysql的行
grep mysql /etc/demo.py
#在demo.py文件中查找包含字符串mysql的行，-i命令不区分mysql的大小写
grep -i mysql /etc/demo.py
#在demo.py文件中查找不包含字符串mysql的行
grep -v mysql /etc/inittab
```

#### 总结

whereis:	查找命令所在的目录，并且显示相关命令的帮助文档路径，配置文件所在路径

which:查找命令所在目录

### 帮助命令

#### 帮助命令-man

```shell
#帮助命令,获取ls命令的操作手册
man ls
#查看配置文件的帮助,man+配置文件名称
man services
#1代表命令的帮助文件
man 1 passwd
#5代表命令的配置文件帮助
man 5 passwd

```

#### 帮助命令-whatis

```shell
#简短的显示命令的信息
whatis ls 
```

#### 帮助命令-apropos

```shell
#查看配置文件的简短信息
apropos 配置文件名称
```

#### 帮助命令-help

```shell
#查看touch的命令帮助
touch --help 
#查看内置命令的命令帮助，内置命令cd无法用man来查看帮助
help cd
```

### Linux常用命令

#### useradd、passwd

```shell
#添加用户
useradd 用户名
#修改用户名密码
passwd 用户名

```

#### who、w命令

```shell
#查看谁在登录主机，信息简单
who 
#较为多的信息显示登陆主机信息，
#idle字段：表示用户登陆后的系统空闲时间，未执行任何操作
#pcpu：累计占用的cpu时间
#what：指当前执行了什么操作
w
```

### 压缩解压缩命令

#### gzip

gzip只能压缩文件，压缩后不保留源文件，格式为.gz

```shell
#压缩文件.gz
gzip 文件名称
#解压缩文件.gz，或者使用gzip -d 文件名称    解压缩
gunzip 压缩文件名称
```

#### tar 

```shell
#1.打包
tar -cvf demo.tar 打包的目录
#2.压缩
gzip demo.tar

#打包压缩一步到位
tar -zcvf demo.tar.gz 压缩目录
#打包压缩生成.bz2文件
tar -cjf demo.tar.bz2 压缩目录

#解压缩.tar.bz2文件
tar -xjf demo.tar.bz2
#解压缩
tar -zxvf demo.tar.gz
	-z	解压缩
	-x	解包
	-v	显示详细信息
	-f	指定解压文件
```

#### zip

压缩文件为zip，目录，.bz2

```shell
#打包文件为demo.zip
zip demo.zip 文件名称
#压缩目录
zip -r demo.zip 目录名称

```

#### bzip2

```shell
#-k选项表示压缩后保留源文件
bzip2 -k 文件名称

#解压缩
bunzip2 -k demo.bz2
```

### 网络命令

#### write

```shell
#给用户发信息，使用ctrl+D保存
write 用户名
```

#### wall

```shell
#给所有用户发送广播消息，write all 取了 w all
wall 发送内容
eg: wall this is a message
```

#### ping

```shell
#测试网络是否连通，会一直ping显示状态
ping ip地址

#测试网络是否连通，-c 3 表示ping3次
ping -c 3 ip地址

```

#### ifconfig

```shell
#查看信息,eth0表示本地网卡，
ifconfig
```

#### mail

```shell
#查看发送电子邮件
maill 用户名
```

#### last

```shell
#显示计算机所有用户登录的信息
last
```

#### lastlog

```shell
#显示用户的登录信息
lastlog
#只查看某一个用户的登录信息
lastlog -u 用户id
```

#### traceroute

```shell
#显示数据包到主机之间的路径
traceroute
```

#### netstat

```shell
#显示网络信息
netstat -ano
	-t	TCP协议
	-u	UDP协议
	-l	监听
	-r	路由
	-n	显示ip地址和端口号
#查看本机监听端口
netstat -tlun
#查看本机所有的网络连接
netstat -an
#查看本机路由表
netstat -rn
```

#### setup

```shell
#配置网络
setup
```

#### mount

```shell
#挂载命令
#创建挂载点
mkdir /mnt/cdrom
#挂载  /dec/sr0为设备文件名称    /mnt/cdrom为挂载点
mount /dec/sr0  /mnt/cdrom
#去挂载点查看挂载的文件
cd /mnt/cdrom

#卸载设备，umount后面跟设备文件名称或挂载点都行
umount /dec/sr0
```

### 关机重启

#### shutdown

关机前会保存任务，建议使用shutdown

```shell
#关机，当天20:30关机
shutdown -h 20:30
#现在关机
shutdown -h now 
	-c	取消前一个关机命令
	-h	关机
	-r	重启

#取消前一个关机命令
shutdown -c 
```

#### halt			  关机

#### poweroff	关机

#### init 0			关机

#### restart

```shell
#重启
restart 
```

#### init

```shell
#系统运行级别
init 0
	0	关机
	1	单用户
	2	不完全多用户，不含NFS服务
	3	完全多用户
	4	未分配
	5	图形界面
	6	重启
```

#### runlevel

```shell
#查询系统运行级别
runlevel
```

#### logout

```shell
#退出登录
logout
```

### Vim

```shell
#插入模式
i a o
#命令模式下执行
a	在光标所在的字符后面插入
A	在光标所在的字符行尾插入
i	在光标所在的字符前插入
I	在光标所在的行首插入
o	在光标下插入新行
O	在光标上插入新行

#在命令模式下执行
:set nu		设置行号
:set nonu	取消行号
gg			到第一行
G			到最后一行
nG			到第n行，其中n为数字，eg，3G，跳到第三行
:n			到第n行
$			移至行尾
0			移至行首（数字0）

#删除命令
x			删除光标所在的字符
nx			删除光标所在处后n个字符，eg:6x	删除光标后面的6个字符
dd			删除光标所在行，ndd删除n行，eg：12dd删除12行
dG			删除光标所在行到文件末尾内容
D			删除光标所在处到行尾内容
:n1,n2d		删除指定范围行，eg：11,14d	删除11到14行

#复制和剪切命令
yy			复制当前行
nyy			复制当前行以下n行
dd			剪切当前行
ndd			剪切当前行以下n行
p、P			粘贴在当前光标所在行下  行上

#替换和取消命令
r			取代光标所在处字符
R			从光标所在处开始替换字符
u			取消上一步操作

#搜索和搜索替换命令，返回上一步
n			搜索指定字符串的下一个出现位置
:set ic 	执行这个之后，搜索不区分大小写
/demo		搜文中的demo字符串
:%s/hello/demo/g	%s代表全文替换，全文将hello替换为demo，g表示不询问
:13,15s/hello/demo/c	将13行到15行中的hello替换成demo，c表示询问

#保存退出
:q!			不保存退出
:w			保存
:wq			保存并退出
:w new.py	保存到另一个文件new.py中
ZZ			快捷键，不保存退出

#小技巧
:r /etc/demo.py	将demo.py文件下的内容导入到当前文件中
:! ls 			!后面可以接命令，操作命令行
：r !date 		将命令的执行结果导入到当前文件，光标位置为导入开始位置
：map 快捷键 操作  设置快捷键
:n1,n2s/^/#/g	连续行注释
:n1,n2s/^#//g	连续行注释

:ab mymail huangd@123 	ab命令可以设置常用的字符替换，当输入mymail空格或回车，会自动变为huangd@123

#永久生效的命令，比如set nu等，一般是写放在/home/.vimrc内，如果是普通用户将其放在/home/hdt/.vimrc文件内，root用户放在/root/.vimrc下。该文件内的内容会自动生效，以下命令则会自动生效
set nu
ab mymail huangd@123
```

### linux软件安装

#### RPM安装

具有很多依赖，安装包之前需要提前安装依赖包

```shell
#安装
rpm -ivh 全名
	-i			安装
	-v			显示详细信息
	-h			显示进度
	--nodeps	不检测依赖性（不推荐）
#卸载
rpm -e 包名
#查询是否已经安装
rpm -q 包名
#查询所有已经安装的rpm包
rpm -qa 包名
#已安装过的包信息
rpm -qi 包名
	-i	查询软件信息
	-q	查询未安装包信息
#查询未安装的包
rpm -pl 包名
	-l	列表
	-p	查询未安装包信息
#查询系统文件属于哪个RPM包
rpm -qf 系统文件名
#查询软件包的依赖性
rpm -pR 包名
#RPM包校验，查看包文件进行了哪些修改
rpm -V 已安装包名

#rpm包中提取文件
#rpm2cpio
rpm2cpio 包名 | \
cpio -idv .文件绝对路径 
	-i	还原
	-d	还原时自动新建目录
	-v	显示还原过程
```

#### yum在线管理（RPM包管理）

##### IP地址配置和网络yum源

如果是redhat可以通过setup命令进入网络配置

![image-20210418090359102](C:\Users\18270\AppData\Roaming\Typora\typora-user-images\image-20210418090359102.png)

上面网络配置完成后，重启网络服务；有的系统默认网卡是没有启动的，需要查看一下是否启动，如果没有自启动需要配置一下

```shell
#将下面文件中的ONBOOT=no 改为ONBOOT=yes
vi /etc/syconfig/network-scripts/ifcfg-eth0

#重启网络服务
service network restart
```

查看网络yum源配置，自行百度修改配置源

```shell
vi /etc/yum.repos.d/Centos-Base.repo
```

yum命令

```shell
#查看可用的软件包列表
yum list
#查看包含关键字的软件包
yum search 关键字
#安装
yum -y install 包名
	-y	自动回答yes
#安装gcc
yum -y install gcc 
#升级，不加包名，则更新所有的软件包，包含linux内核，不建议升级所有包名，会导致宕机
yum -y update 包名
#卸载，少用yum卸载，由于有依赖包，一个包有可能同时被其他的包依赖，yum卸载有可能会把其他同时依赖的包给一起删掉
yum -u remove 包名
#查看软件组
yum grouplist
#软件包组安装
yum groupinstall
#软件包组卸载
yum groupremove

```

光盘yum源配置

```sh
#挂载光盘,下面要用
mount /dev/sr0 /mnt/cdrom/
#让网络yum源失效
cd /etc/yum.repos.d
#将repo文件的配置源修改为其他的名称后缀即可
mv CentOS-Base.repo \CentOS-Base.repo.bak
mv CentOS-Debuginfo.repo \CentOS-Debuginfo.repo.bak
mv CentOS-Vault.repo \CentOS-Vault.repo.bak
#修改光盘yum源
vim CentOS-Media.repo
    name=CentOS$releasever - Media
    #下面的地址为上面自己光盘的挂载地址
    baseurl=file///mnt/cdrom
    #注释下面两个地址，本地没有这两个地址的时候会一直报错找不到目录
    # file///media/cdrom
    # file///media/cdrecorder/
	gpgcheck=1
	#将enabled=1，让这个yum源配置文件生效
	enabled=1
	gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
#注意，给配置文件加注释的时候不要在行尾加，直接在行首加注释，不然会遇到格式问题的错误

```

##### RPM包安装位置

默认位置

``` 
/etc/				配置文件安装目录
/usr/bin/			可执行的命令安装目录
/usr/lib/			程序所使用的函数库保存位置
/usr/share/doc/		基本的软件使用手册保存位置
/usr/share/man/		保住文件保存位置
```

##### 脚本安装包

Webmin（web端管理）安装案例

```sh
#安装网址：https://sourceforge.net/projects/webadmin/files/

```

### 用户管理

#### 用户配置

```sh
#查看配置文件的帮助
man 5 passwd
#查看用户信息
cat /etc/passwd
#加密后的密码存储在shadow里面
cat /etc/shadow
cat /etc/gshadow
#shadow文件中的字段介绍：
#第一个字段，用户名
#第二个字段，密码，*号代表没有密码
#第三个字段，代表创建时间
#第四个字段，两次密码修改时间间隔（和第3字段相比）
#第五个字段，密码有效期（和第3字段相比）
#第六个字段，密码修改到期前警告天数（和第5字段相比）
#第七个字段，密码过期后的宽限天数
#第八个字段，账号失效时间
#第九个字段，保留
root:*:18632:0:99999:7:::
```

# Shell

```sh
test='test'
#单引号不得有转义字符和变量名，但可以拼接
str='hello world'
str='hello'${test}' world'
#双引号可接受转义字符和变量名
str="\"${test}"
str="hello world"
#加井显示字符串长度
echo ${#str}
#从字符串第二个字符开始截取4个字符
echo ${str:1:4}
#数组，空格分割
array_name=(1 2 3 4)
array_name=(
1
2
3
4
)
#读取数组
echo ${array_name[1]}
#读取所有元素
echo ${array_name[@]}
#获取数组个数
length=${#array_name[@]}
length=${#array_name[*]}
#获取某个元素的长度
length=${#array_name[1]}

#关系运算
-eq 相等
-ne 不等
-gt 左大于右
-lt 右大于左
-ge 左大于右
-le 右大于左

#布尔运算符
! 非运算
-o 或运算
-a 与运算
#逻辑运算符
&& 逻辑的AND
|| 逻辑的OR
#字符串运算符
= 相等
!= 不等
-z 长度为0
-n 长度不为0
$ 为空，不为空返回true





```





























































