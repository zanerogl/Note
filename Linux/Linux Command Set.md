# Linux Command Set

## A

#### at 

- 定时执行任务命令，60秒检测一次任务列表，一般只执行一次

> ```shell
> at 5pm + 2 days #两天后的五点执行，把date重定向写入text中
> at> date >> /home/zane/text.txt
> ```
>
> ```shell
> atq #查看已有的任务
> ```
>
> ```shell
> atrm 6 #删除编号为6的任务
> ```

## B

## C

### cat

> ```shell
> cat hello.cpp #查看当前目录下的hello.cpp文件的内容
> cat -n hello.cpp  #查看当前目录下的hello.cpp文件的内容，并加上行号
> cat -n hello.cpp | more  #查看当前目录下的hello.cpp文件的内容，并加上行号，more是用来翻页的
> cat pj/C++/hello.cpp | grep "cout" #只显示含有cout的行
> ```

### cal

> ```shell
> cal #显示这个月的日历
> cal 2022 #显示2022年每个月的日历
> ```

### cd

> ```shell
> cd ~  #回到当前用户的目录主目录
> cd .. #返回当前目录的上一级目录；同理，cd../.. 返回上上级目录
> ```

### chgrp

> ```shell
> chgrp TemporaryUser hello.c #将hello.c文件所属于的组改为TemporaryUser
> chgrp -R zane text/ #修改目录组
> ```

### chmod

>  修改文件权限，方法一：
>  ```shell
>  chmod u=rwx,g=rx,o=rw text.txt #修改text.txt文件的权限，user=rwx，group=r-x，others=rw-
>  chmod u-x text.txt #取消text.txt文件user的x（可执行）权限
>  chmod o+x text.txt #添加text.txt文件others的x（可执行）权限 
>  ```
>
>  修改文件权限，方法二：
>
>  - 各权限对应的数字：r=4   w=2   x=1
>
>  ```shell
>  chmod 752 text.txt #修改text.txt的权限，user=rwx，group=r-x，others=-w-
>  ```
>
>  注意：u,g,o三部分的权限范围只有：1~7

### chown

> ```shell
> chown zane file/ #更改file的所有者
> chown -R zane text/ #更改目录的所有者
> ```

### cp

>```shell
>cp TEXT/temp.txt text/ #拷贝TEXT目录下的temp.txt文件到text/目录下
>cp -r text/ TEXT/ #递归拷贝text/目录到TEXT/目录下
>\cp -r text/ TEXT/ #递归拷贝 text/目录 并强制覆盖到TEXT/目录下
>```
>

### crontab

- 概述

  让系统按时间执行脚本

- 所在目录

  ```shell
  /etc/crontab
  ```

- 指令

> ```shell
> crontab -e #编辑crontab定时任务
> crontab -l #查询crontab任务
> crontab -r #删除当前用户所有的crontab任务
> ```

- 占位符 * * * * *

  | 项目      | 含义                 | 范围                  |
  | --------- | -------------------- | --------------------- |
  | 第一个“*” | 一小时当中的第几分钟 | 0-59                  |
  | 第二个“*” | 一天当中的第几小时   | 0-23                  |
  | 第三个“*” | 一个月当中的第几天   | 1-31                  |
  | 第四个“*” | 一年当中的第几月     | 1-12                  |
  | 第五个“*” | 一周当中的星期几     | 0-7(0和7都代表星期日) |

- 特殊符号

  | 特殊符号 | 含义                                                         |
  | -------- | ------------------------------------------------------------ |
  | *        | 代表任何时间。比如第一个“*”就代表一小时中每分钟都执行一次的意思。 |
  | ,        | 代表不连续的时间。比如"08,12,16***命令”,就代表在每天的8点0分,12点0分,16点0分都执行一次命令 |
  | -        | 代表连续的时间范国。比如“"05*1-6命令”,代表在周一到周六的凌晨5点0分执行命令 |
  | */n      | 代表每隔多久执行一次。比如“'/10****命令”,代表每隔10分钟就执行一遍命令 |

- 特定时间执行任务案例

  | 时间               | 含义                                                         |
  | ------------------ | ------------------------------------------------------------ |
  | 45 22 * * * 命令   | 在22点45分执行命令                                           |
  | 0 17 * * 1 命令    | 每周一的17点0分执行命令                                      |
  | 0 5 1,15 * * 命令  | 每月1号和15号的凌晨5点0分执行命令                            |
  | 40 4  * * 1-5 命令 | 每周一到周五的凌晨4点40分执行命令                            |
  | */10 4 * * * 命令  | 每天的凌晨4点,每隔10分钟执行一次命令                         |
  | 0 0 1,15 * 1 命令  | 每月1号和15号,每周1的0点0分都会执行命令。注意:星期几和几号最好不要同时出现,因为他们定义的都是天 |

#### 实例

- 每隔1分钟，就将当前的日期信息，追加到文件中

  ```shell
  crontab -e
  ```

  ```shell
  */1 * * * * date >> /home/zane/text.txt
  ```

- 每隔1分钟,将当前日期和日历都追加到 /home/zane/text.txt 文件中

  创建.sh文件(脚本)

  ```shell
  vim my.sh
  ```

  在my.sh中输入

  ```shell
  date >> /home/zane/text.txt
  ```

  ```
  crontab -e
  ```

  ```shell
  */1 * * * * /home/zane/my.sh
  ```

## D

### date

> ```shell
> date #显示当前时间
> date "+%Y" #显示当前年份
> date "+%Y-%m-%d-%H-%M-%S" #显示年-月-日-时-分-秒
> date -s "2022-2-14 15:02:10" #设置系统时间
> ```

### dpkg

> ```shell
> dpkg -l | grep firefox #查看已安装的软包firefox的信息
> ```

## E

### echo

> ```shell
> echo $PATH #输出环境变量
> echo $HOSTNAME #输出主机名
> echo "Hello Linux" #输出Hello Linux
> ```

## F

### find

> ```shell
> find -name hello.cpp #在当前目录下查找hello.cpp文件
> find -user zane #在当前目录下查找zane用户的文件
> find C++/ -size -500k #在C++/目录下查找小于500k的文件（+500大于，500等于，k,M,G）
> ```

## G

### grep

> - 用于查找文件里符合条件的字符串
>
> ```shell
> grep "cout<<" pj/C++/hello.cpp #查找并显示含有cout的行
> grep -n "cout<<" pj/C++/hello.cpp #查找并显示含有cout的行，并显示行号
> grep -i "cout<<" pj/C++/hello.cpp #查找并显示含有cout的行，忽略大小写
> cat pj/C++/hello.cpp | grep "cout" #只显示含有cout的行
> cat pj/C++/hello.cpp | grep -n "cout" #只显示含有cout的行，并显示行号
> ```

### gzip

> ```shell
> gzip text.txt #压缩为后缀为gz格式的包
> gunzip text.txt.gz #解压
> ```
>
> 



## H

```shell
halt #关机
```

### head

> ```shell
> head Project/C++/hello.cpp #显示文件的头部内容，默认前10行
> head -n5 Project/C++/hello.cpp #显示文件前5行
> ```

### history

> ```shell
> history #显示所有的历史命令
> history 10 #显示最近使用过的10个命令
> !10 #再次执行在history里编号为10的命令
> ```

## I

### id

> ```shell
> id zane #查看用户信息
> ```

## J

### journalctl

> ```shell
> journalctl | grep sshd #查看关于sshd的内存日志
> ```

## K

### kill

> - 用于杀死进程
>
> ```shell
> kill 888 #结束PID为888的进程
> killall gedit #删除所有进程名为gedit的进程（包括其子进程）
> kill -9 2225 #强制删除PID为2225的进程
> ```

## L

### less

> ```shell
> less Project/C++/hello.cpp #查看文件
> ```
>
> | 操作     | 功能说明                                           |
> | -------- | -------------------------------------------------- |
> | Space    | 向下翻动一页                                       |
> | PageDown | 向下翻动一页                                       |
> | PageUp   | 向上翻动一页                                       |
> | /字符    | 向下搜寻『字串』的功能；n：向下查找；N：向上查找； |
> | ?字符    | 向上搜寻『字串』的功能；n：向上查找；N：向下查找； |
> | q        | 退出less                                           |

### ln

> ```shell
> ln -s Project/ pj #在当前目录创建软链接，pj相当于Project的快捷方式，可cd pj
> ```

### locate

> ```shell
> updatedb #先执行完这个再执行locate
> locate hello.cpp #直接查找hello.cpp文件
> ```

### ls

> ```shell
> ls -a #显示当前目录所在的文件和目录，包括隐藏的
> ls -l #以列表的方式显示信息
> ls -lh #显示当前目录下的文件大小
> ls -ahl #显示当前目录下的文件的所有者和其所在的组
> ```

### ll

> ```shell
> ll #查看当前目录下所有的信息
> ```
>
> #### 扩展
>
> ```shell
> #权限信息  文件数/子目录数  文件所属用户  文件所属组  文件大小(k)  文件最后修改时间  名
> lrwxrwxrwx  1  zane  zane     8  2月 14 14:47  pj -> Project//
> drwxrwxr-x  3  zane  zane  4096  2月 14 14:15  Project/
> -rwxrwxr-x  1  zane  zane 17368  2月 14 14:15  hello*
> -rw-rw-r--  1  zane  zane   194  2月 14 15:41  hello.cpp
> ```
>
> ##### 权限信息 
>
> | 位   | 说明                                                         |
> | ---- | ------------------------------------------------------------ |
> | 0    | **d,-,l,c,b**  分别表示：目录，文件，快捷方式，字符设备(鼠标)，块设备(硬盘) |
> | 1~3  | **r,w,x**  该文件所有者对该文件的权限  User                  |
> | 4~6  | **r,w,x**  该文件所在组对该文件的权限  Group                 |
> | 7~9  | **r,w,x**  其他用户对该文件的权限         Others             |
>
> ##### 权限说明
>
> - **r：可读     w：可写可删除     x： 可执行可进入目录**
>
> 注意：只有获得文件所在目录的**w**权限才能删除文件
>
> 

## M

```shell
man ls #获取ls命令的帮助信息
```

### mkdir

> ```shell
> mkdir Project #在当前目录下创建Project目录
> mkdir -p Project/C++ #创建多级目录
> ```

### more

> ```shell
> more hello.cpp #查看文件
> ```
>
> | 操作   | 功能说明                        |
> | ------ | ------------------------------- |
> | space  | 向下翻一页                      |
> | Enter  | 向下翻一行                      |
> | q      | 立刻离开more,不再显示该文件内容 |
> | Ctrl+F | 向下滚动一屏                    |
> | Ctrl+B | 返回上一屏                      |
> | =      | 输出当前行的行号                |
> | :f     | 输出文件名和当前行的行号        |

### mv

>```shell
>mv text text0 #重命名当前目录下的text/目录
>mv text0/ TEXT/ #移动目录
>```
>

## N

### netstat

> - 用于监控网络状态
>
> ```shell
> netstat -an | more #显示系统网络情况
> netstat -anp | more #显示系统网络情况，包括PID和进程名（要在root用户下才能显示完全）
> ```
>
> - 显示信息解释
>
> ```shell
> #协议					本地地址				外部地址			状态			PID和进程名
> Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name 
> tcp        0      0 192.168.60.128:44360    34.117.237.239:443      ESTABLISHED 4714/firefox        
> tcp        0      0 192.168.60.128:22       192.168.60.1:57105      ESTABLISHED 12465/sshd: zane [p 
> ```
>
> - Windows与Linux通过sshd通信原理图
>
> ```mermaid
> graph LR
> Windows-xshell___ --192.168.60.1:57105 ____-->Linux_22__
> Linux_22__--192.168.60.128:22 ____--> Windows-xshell___
> ```
>



## O





## P

```shell
pwd #显示当前的工作目录的绝对路径
```

### ps

> - 查看系统正在执行的进程（运行中的程序）
>
> ```shell
> ps -a #显示当前终端的所有进程信息
> ps -u #以用户的格式显示进程信息
> ps -x #显示后台进程运行的参数
> ps -aux #组合显示
> ps -aux | grep sshd #结合grep筛选显示出进程名为sshd内容
> ```
>
> - 内容解释
>
> ```shell
> #用户ID  进程id CPU占比 物理内存占比 虚拟，物理内存占比   终端名称   状态   启动时间 运行时间   运行指令
> USER     PID   %CPU    %MEM          VSZ       RSS      TTY     STAT    START   TIME     COMMAND
> root     1     0.0     0.4        102148      9108        ?       Ss    20:25   0:01     /sbin/init splash
> root     2     0.0     0.0             0         0        ?        S    20:25   0:00     [kthreadd]
> root     3     0.0     0.0             0         0        ?       I<    20:25   0:00     [rcu_gp]
> ```
>
> - 树状图显示
>
> ```shell
> pstree #以树状图显示进程
> pstree -p #同时显示PID
> pstree -u #同时显示用户名
> ```

## Q



## R

```shell
reboot #现在重新启动计算机
```

### rmdir

>```shell
>rmdir project/ #删除project目录（空）
>rmdir -p TEXT/text/ #删除多级目录 前提text是空目录
>```

### rm

> ```shell
> rm hello.cpp #删除当前目录下的文件
> rm -r TEXT/text/ #删除TEXT/下的text/目录
> rm -f TEXT/text.txt #强制删除TEXT/目录下的text.txt文件，不会有提示
> rm -rf TEXT/ #递归强制删除TEXT目录，其下级目录也跟着删除
> ```

## S

### shutdown

> ```shell
> shutdown-h now #立该进行关机
> shudown-h 1 #1分钟后关机
> shutdown-r now #现在重新启动计算机
> ```

```shell
sync #把内存的数据同步到磁盘
```

### systemctl

> - 系统管理指令，位于/usr/lib/systemd/system下
>
> ```shell
> systemctl list-unit-files | grep sshd #筛选查看sshd服务状态
> systemctl status sshd #查看sshd的状态
> systemctl stop sshd #关闭sshd
> systemctl start sshd #开启sshd 
> systemctl enable ssh #启动ssh
> systemctl disenable ssh #关闭ssh
> systemctl is-active sshd #查看sshd服务是否活跃
> systemctl is-enabled sshd #查看sshd是否开启
> ```

## T

### tail

> ```shell
> tail Project/C++/hello.cpp #显示文件尾部内容，默认是最后10行
> tail -n 5  Project/C++/hello.cpp #显示文件最后5行、
> tail -f Project/C++/hello.cpp #实时监控文件最后
> ```

### tar

> ```shell
> tar -zcvf text.tar text.txt #压缩当前目录下的text.txt文件
> tar -zcvf text.tar.gz text.txt #同上
> tar -zxvf text.tar #将text.tar解压到当前目录
> tar -zxvf text.tar -C temp/ #将text.tar解压到temp/目录
> ```
>
> | 选项 | 功能               |
> | ---- | ------------------ |
> | -c   | 产生.tar打包文件   |
> | -v   | 显示详细信忠       |
> | -f   | 指定压缩后的文件名 |
> | -z   | 打包同时压缩       |
> | -x   | 解包.tar文件       |
>
> 
>

### touch

> ```shell
> touch text.txt #当前目录下创建txt文件
> touch text/text.txt #在已有的目录text/下创建txt文件
> ```

### top

> ```
> top #动态显示进程情况
> top #
> ```
>
> 交互操作
>
> | 操作 | 功能                                |
> | ---- | ----------------------------------- |
> | P    | 以CPU使用率排序，默认就是此项       |
> | M    | 以内存的使用率排序                  |
> | N    | 以PID排序                           |
> | u    | 以指定user查看；输入u，再输入用户名 |
> | k    | 删除指定PID进程；输入k，再输入PID   |
> | q    | 退出top                             |
>
> 

### tree

> ```shell
> tree Project/ #以树状的形式列出Project/目录下的情况
> ```
>
> 如下：
>
> ```shell
> Project/
> └── C++
>     ├── hello
>     └── hello.cpp
> ```



## U

### useradd

> ```shell
> useradd user1 #添加新用户user1，可能不会创建家目录，同时创建user1组
> useradd -m user1 #添加新用户user1，并创建家目录user1/ 
> ```

### userdel

> ```shell
> userdel -r user1 #递归删除用户，包括删除其家目录
> ```

### usermod

> ```shell
> usermod -g TemporaryUser user1 #将user1的主要用户组修改为TemporaryUser
> ```


## V

### vi / vim

**vi** 文本编辑命令

**vim** vi的增强版，具有较强的程序编辑能力

- 正常模式 

  > 按下 esc 后的模式
  >
  > 以vim打开一个档案就直接进入一般模式了(这是默认的模式)。在这个模式中,你可以使用『上下
  > 左右』按键来移动光标,你可以使用『除字符』就『删除整行』来处理档案内容,也可以使用
  > 『复制、粘贴』来处理你的文件数据

- 插入模式 

  > 按下 i 后的模式
  >
  > 按下i,IO,O,a,A,r,R等任何一个宇母之后オ会进入编損模式一般来说用 i 即可

- 命令行模式

  > 按下 esc 再按下 : 后的模式
  >
  > 在这个模式当中,可以提供你相关指令,完成读取、存盘、替换、离开vim、显示行号等的动作
  > 则是在此模式中达成的!
  >
  > **指令** 
  >
  > ```shell
  >  :q	#退出
  >  :q!	#强制退出，不保存
  >  :wq	#保持退出
  >  ```
  >   

**快捷键**

- yy 拷贝当前行,拷贝当前行向下的5行5yy,并粘贴(输入p)。
- dd 删除当前行 ，5dd 删除当前行向下的5行 
- u 撤销上一次的 行输入
- /关键字 在文件中查找，输入n就是查找下ー个
- : set nu 设置文件的行号   
- : set nonu 取消文件的行号
- 行号 + shift + g 将光标移动到相应行
- gg 定位到首行
- G 定位到末尾行

**实例: **在终端输出 Hello Linux!

```shell
vim hello.cpp #用vim创建hello.cpp文件并进入
```

```c++
#include <iostream> 
using namespace std;

int main (){
    cout<<"Hello Linux!"<<endl;
    return 0;
}
```

```shell
g++ hello.cpp -o hello #编译
./hello	#运行
```



## W

## X

## Y

## Z

### zip

> ```shell
> zip -r Project.zip Project/ #把Project目录压缩名为Project.zip的压缩包
> unzip -d temp/ Project.zip #把Project.zip解压到temp/目录下
> ```

## >/>>

### >

> ```shell
> echo "#include<iostream>" > C++/hello.cpp #将#include<iostream>重定向输入到hello.cpp中，并将原内容覆盖
> ```
>
> ```shell
> ls -l /home > /home/info.txt #将home/信息输入到其目录下的info.txt文件下
> ```
>

### >>

> ```shell
> echo "//input" >> C++/hello.cpp #将//input重定向追加输入到hello.cpp的最后一行
> ```
>
> ```shell
> cal >> /home/mycal.txt #将这个月的日历追加到mycal.txt中
> ```

## |

> ```shell
> ls -l Project/ | grep "^d" #列出Project/目录下的目录
> ls -l /opt | grep "^-" | wc -l #列出/opt目录下的文件个数
> ls -lR Project/ | grep "^-" | wc -l #列出目录下的文件个数并包括子文件夹里的
> ls -lR Project/ | grep "^d" | wc -l #列出目录下的目录个数并包括子文件夹里的
> ```
>
> 
