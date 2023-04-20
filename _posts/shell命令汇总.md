# shell命令汇总

## 常用shell基础命令

### 一、文件、目录操作命令

命令大全：https://www.runoob.com/linux/linux-command-manual.html

##### **1、ls命令**

功能：显示文件和目录的信息

ls　以默认方式显示当前目录文件列表

ls -a 显示所有文件包括隐藏文件

ls -l 显示文件属性，包括大小，日期，符号连接，是否可读写及是否可执行

ls -lh 显示文件的大小，以容易理解的格式印出文件大小 (例如 1K 234M2G)

ls -lt 显示文件，按照修改时间排序

##### **2、cd命令**

功能：改名目录

cd dir　切换到当前目录下的dir目录

cd /　切换到根目录

cd ..　切换到到上一级目录

cd ../..　切换到上二级目录

cd ~　切换到用户目录，比如是root用户，则切换到/root下

根目录与家目录的区别：

根目录是系统的一级文件结构，家目录只是非root用户控制目录。相当于windows我的文档，非root用户只能完会控制家目录的文件，不能控制根目录下其它的文件。

根目录是设备的最顶层目录，用 / 表示
家目录是每个用户登录系统后所在的目录，通常在 /home 下，以用户名作为目录，可以用 ~ 表示。
cd / 进入根目录
cd ~/ 进入家目录
当然，也可以用 /home/someone 进入someone的家目录

##### **3、cp命令**

功能：copy文件

cp source target　将文件source复制为target

cp /root /source .　将/root下的文件source复制到当前目录

eg:cp /home/open_038_dev/external_files/test/test.sh .

cp –av soure_dir target_dir　将整个目录复制，两目录完全一样

##### **4、rm命令**

功能：删除文件或目录

rm file　删除某一个文件

rm -f file 删除时候不进行提示。可以于r参数配合使用

rm -rf dir　删除当前目录下叫dir的整个目录

##### **5、mv命令**

功能：将文件移动走，或者改名，在uinx下面没有改名的命令，如果想改名，可以使用该命令

mv source target　将文件source更名为target

命令参数：

-b ：若需覆盖文件，则覆盖前先行备份。 

-f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；

-i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！

-u ：若目标文件已经存在，且 source 比较新，才会更新(update)

-t  ： --target-directory=DIRECTORY move all SOURCE arguments into DIRECTORY，即指定mv的目标目录，该选项适用于移动多个源文件到一个目录的情况，此时目标目录在前，源文件在后。

实例一：文件改名

命令：

mv test.log test1.txt

实例二：移动文件

命令：

mv test1.txt test3

将文件log1.txt,log2.txt,log3.txt移动到目录test3中。 

mv log1.txt log2.txt log3.txt test3

将文件log1.txt log2.txt  log3.txt异动到/opt/soft/test/test4目录下

mv -t /opt/soft/test/test4/ log1.txt log2.txt  log3.txt 

移动当前文件夹下的所有文件到上一级目录

mv * ../

##### **6、diff**

功能：比较文件内容

diff dir1 dir2　比较目录1与目录2的文件列表是否相同，但不比较文件的实际内容，不同则列出

diff file1 file2　比较文件1与文件2的内容是否相同，如果是文本格式的文件，则将不相同的内容显示，如果是二进制代码则只表示两个文件是不同的

comm file1 file2　比较文件，显示两个文件不相同的内容

##### **7、ln命令**

功能：建立链接。windows的快捷方式就是根据链接的原理来做的

ln source_path target_path 硬连接

ln -s source_path target_path 软连接

ln是linux中又一个非常重要命令，它的功能是为某一个文件在另外一个位置建立一个同不的链接，这个命令最常用的参数是-s，具体用法是：ln –s 源文件 目标文件。
　　当我们需要在不同的目录，用到相同的文件时，我们不需要在每一个需要的目录下都放一个必须相同的文件，我们只要在某个固定的目录，放上该文件，然后在 其它的目录下用ln命令链接（link）它就可以，不必重复的占用磁盘空间。例如：ln –s /bin/less /usr/local/bin/less

http://www.cnblogs.com/joeblackzqq/archive/2011/03/20/1989625.html

##### **6、tree** 

树形结构 tree -L 1

### 二、查看文件内容命令

##### **1、cat命令**

显示文件的内容，和DOS的type相同

cat file　

##### **3、tail 命令**

功能：显示文件的最后几行

tail -n 100 aaa.txt 显示文件aaa.txt文件的最后100行

##### **4、vi和vim命令** 

vi file　编辑文件file

vi 原基本使用及命令：

输入命令的方式为先按[ESC]键，然后输入:w(写入文件),:w!(不询问方式写入文件）,:wq保存并退出,:q退出,q!不保存退出

输入i

##### **5、touch命令**

功能：创建一个空文件

touch aaa.txt  创建一个空文件，文件名为aaa.txt

### 三、基本系统命令

##### **1、man命令**

功能：查看某个命令的帮助，如果你不知道某个命令的用法不懂，可以问他，他知道就回告诉你

例如：

man ls 显示ls命令的帮助内容

##### **2、w命令**

功能：显示登录用户的详细信息

例如：

Sarge:~# w

22:06:51 up 43 min,  1 user,  load average: 0.00, 0.00, 0.00

USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT

zhoulj   pts/0    10.140.0.109     21:24    0.00s  0.85s  0.09s sshd: zhoulj [priv]

##### **7、uname命令**

功能：查看系统版本

uname -R　显示操作系统内核的version

例如：

Sarge:~# uname -a

Linux Sarge 2.6.8-2-386 #1 Tue Aug 16 12:46:35 UTC 2005 i686 GNU/Linux

##### **8、关闭和重新启动系统命令**

reboot　  重新启动计算机

shutdown -r now  重新启动计算机，停止服务后重新启动计算机

shutdown -h now  关闭计算机，停止服务后再关闭系统

halt   关闭计算机

一般用shutdown -r now,在重启系统是，关闭相关服务，shutdown -h now也是如此。

##### **9、su命令**

功能：切换用户

su -  切换到root用户

su - zhoulj 切换到zhoulj用户，

注意：- ，他很关键，使用-，将使用用户的环境变量

http://man.linuxde.net/su

### 四、监视系统状态命令

##### **1、top命令**/htop

功能：查看系统cpu、内存等使用情况

##### **2、free命令**

功能：查看内存和swap分区使用情况

##### **4、vmstat命令**

功能：监视虚拟内存使用情况

##### **5、ps命令**

功能：显示进程信息

ps ux 显示当前用户的进程

ps uxwww 显示当前用户的进程的详细信息

ps aux 显示所有用户的进程

ps ef 显示系统所有进程信息

##### **6、kill命令**

功能：干掉某个进程，进程号可以通过ps命令得到

nvidia-smi

kill -9 1001　将进程编号为1001的程序干掉

kill all -9 apache　将所有名字为apapche的程序杀死，kill不是万能的，对僵死的程序则无效。

##### **7、byobu**

后端运行

### 五、压缩命令

##### **1、gzip格式命令**

功能：压缩文件，gz格式的

注意：生成的文件会把源文件覆盖

gzip -v  压缩文件，并且显示进度

-d  解压缩

gunzip  -f  解压缩

例如：

gzip a.sh

gzip -d a.sh.gz

##### **2、zip格式命令**

功能：压缩和解压缩zip命令

zip   -r -0 不损耗文件压缩

unzip   

例如：

将/home/Blinux/html/这个目录下所有文件和文件夹打包为当前目录下的html.zip：

zip -q -r html.zip /home/Blinux/html
（-q：不显示指令执行过程     -r：递归处理，将指定目录下的所有文件和子目录一并处理）

zip a.sh.zip a.sh

unzip a.sh.zip

##### **4、tar命令**

功能：归档、压缩等，比较重要，会经常使用。

-cvf    压缩文件或目录

-xvf     解压缩文件或目录

-zcvf    压缩文件或，格式tar.gz

-zxvf    解压缩文件或，格式tar.gz

-zcvf     压缩文件或，格式tgz

-zxvf     解压缩文件或，格式tgz

举例:

tar cvf abc.tar *.sh

tar xvf abc.tar

tar czvf abc.tar.gz *.sh

tar xzvf abc.tar.gz

### 六、其他命令

##### **1、ssh命令**

功能：远程登陆到其他UNIX主机

ssh -l user1 192.168.1.2 使用用户名user1登陆到192.168.1.2

ssh 使用用户名user1登陆到192.168.1.2

##### **2、scp命令**

功能：安全copy

例如：

scp abc.tar.gz

:~ 将本地的abc.tar.gz 复制到 192.168.1.5的user1用户的根(/home/user1)下。

##### **3、telnet命令**

功能：登陆到远程主机

##### **4、su和sudo**

su 密码为root的密码，目的是进入root文件夹

sudo 密码为该账户的密码，目的是不进去root文件夹但以root的权限进行操作，需要先加入白名单才能使用



## 生产中遇到的shell命令

##### 一、jupyter-notebook中调用Shell变量

![img](https://img-blog.csdnimg.cn/20200229173322406.png)



##### 二、统计文件数量

```python
ls -l | wc -l
```

##### 三、其他shell命令

###### echo命令

Shell 的 echo 指令与 PHP 的 echo 指令类似，都是用于字符串的输出。命令格式：

```
echo string
```

您可以使用echo实现更复杂的输出格式控制。

1.显示普通字符串:

```
echo "It is a test"
```

这里的双引号完全可以省略，以下命令与上面实例效果一致：

```
echo It is a test
```

2.显示转义字符

```
echo "\"It is a test\""
```

结果将是:

```
"It is a test"
```

同样，双引号也可以省略

3.显示变量

read 命令从标准输入中读取一行,并把输入行的每个字段的值指定给 shell 变量

```
#!/bin/sh
read name 
echo "$name It is a test"
```

以上代码保存为 test.sh，name 接收标准输入的变量，结果将是:

```
[root@www ~]# sh test.sh
OK                     #标准输入
OK It is a test        #输出
```

4.显示换行

```
echo -e "OK! \n" # -e 开启转义
echo "It is a test"
```

输出结果：

```
OK!

It is a test
```

5.显示不换行

```
#!/bin/sh
echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test"
```

输出结果：

```
OK! It is a test
```

6.显示结果定向至文件

```
echo "It is a test" > myfile
```

7.原样输出字符串，不进行转义或取变量(用单引号)

```
echo '$name\"'
```

输出结果：

```
$name\"
```

8.显示命令执行结果

```
echo `date`
```

**注意：** 这里使用的是反引号 **`**, 而不是单引号 **'**。

结果将显示当前日期

```
Thu Jul 24 10:08:46 CST 2014
```

