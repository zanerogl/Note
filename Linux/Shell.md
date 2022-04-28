# Shell

### 案例一

打印**Hello World!**

> - 创建**.sh**文件
>
>   ```shell
>   vim Hello.sh
>   ```
>
> - 编写**.sh**文件
>
>   ```shell
>   #告诉系统用的是bash
>   #!/bin/bash
>   echo "Hello World!"
>   ```
>
> - 运行
>
>   ```shell
>   sh Hello.sh
>   ```
>
>   ```shell
>   ./ Hello.sh #需要保证有执行权限
>   ```

-----

### 案例二

**变量**

> - **代码**
>
>   ```shell
>   #!/bin/bash
>   #定义变量A并输出变量A
>   A=100
>   echo A=$A
>   echo "A=$A"
>       
>   #销毁变量A
>   unset A
>   echo A=$A
>       
>   #声明静态变量B=2，静态变量不能unset
>   readonly B=2 
>   echo "B=$2"
>       
>   #将指令返回的结果赋给变量
>   C=`date`
>   D=$(date)
>   echo "C=$C"
>   echo D=$D
>       
>   #多行注释
>   :<<!
>   echo $PATH
>   !
>   ```
>
>   **注意：**变量定义时**=**两侧不能有空格；可以有字母、数字、下划线，但是不能以数字开头

### 案例三

**位置参数**

> - **基本语法**
>   **$n** (功能描述：为数字，**$0**代表命令本身，**$1-$9**代表第一到第九个参数，十以上的参数，十以上的参数需要用大括号包含，如**${10})**
>   **$***	(功能描述：这个变量代表命令行中所有的参数，$*把所有的参数看成一个整体)
>   **$@**	(功能描述：这个变量也代表命令行中所有的参数，不过$@把每个参数区分对待)
>   **$#**	(功能猫述：这个变量代表命令行中所有参数的个数)
>
> - **代码**
>
>   ```shell
>   #!/bin/bash
>   echo "0=$0 1=$1 2=$2"
>   echo "所有的参数=$*"
>   echo "$@"
>   ```
>
> - 运行结果
>
>   ```shell
>   0=./LP.sh 1=10 2=30
>   所有的参数=10 30
>   10 30
>   ```

### 案例四

**预定义变量**

> - **基本语法**
>   **$$** (功能描述：当前进程的进程号(PID))
>   **$!** (功能猫述：后台运行的最后一个进程的进程号(PID))
>   **$?** (功能描述：最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；
>   如果这个变量的值为非0（具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了。)
>
> - **代码**
>
>   ```shell
>   #!/bin/bash
>   echo "当前执行的进程ID=$$"
>   #以后台的方式运行一个脚本，并获取其进程号
>   /home/zane/Project/Shell/Hello.sh &
>   echo "最后一个后台方式运行的进程ID=$!"
>   echo "执行的结果是=$!"
>   ```

### 案例五

**运算符**

> - 基本语法
>   1.“$(运算式)”或“$[运算式]”或者expr m+n/expression表达式
>   2.注意eXp运算符间要有空格，如果希望将eXpr的结果赋给某个变量，使用
>   3.expr m-n
>   4.
>
>   ```shell
>   #expr \* , / , %  乘 除 取余
>   ```
>
>   
>
> - 代码
>
>   ```shell
>   #!/bin/bash
>   #计算(2+3)*4的值
>   #方法一
>   RES1=$(((2+3)*4))
>   echo res1=$RES1
>   #方法二
>   RES2=$[(2+3)*4]
>   echo "res2=$RES2"
>   #方法三
>   TEMP=`expr 2 + 3`
>   RES3=`expr $TEMP \* 4`
>   echo "temp=$TEMP"
>   echo "res3=RES3"
>       
>   #求出预定义参数二和参数三的和
>   SUM=$[$1+$2]
>   echo "sum=$SUM"
>   ```
>

### 案例六

**条件判断**

> - **判断语句**
>
>   ```shell
>   -lt #小于
>   -le #小于等于
>   -eq #等于
>   -gt #大于
>   -ge #大于等于
>   -ne #不等于
>   -r #读权限
>   -w #写权限
>   -x #执行权限
>   -f #文件存在并是一个常规文件
>   -e #文件存在
>   -d #文件存在并是一个目录
>   ```
>
> - **代码**
>
>   ```shell
>   #!/bin/bash
>   #判断语句 = 
>   if [ "ok" = "ok" ]
>   then
>           echo "equal"
>   fi
>   #判断传入数字大小
>   if [ $1 -ge 60 ]
>   then
>           echo "Passed"
>   elif [ $1 -lt 60 ]
>   then
>           echo "Failed"
>   fi
>   #判断文件是否存在
>   if [ -f /home/zane/Project/Shell/var.sh ]
>   then
>           echo "Yes, it exist."
>   fi
>   #[ ]内非空即为真
>   if [ aa ]
>   then
>           echo "Yes"
>   fi
>   ```
>
> - **注意：**[ ]直接要有空格

### 案例七

**case语句**

> - 代码
>
>   ```shell
>   #!/bin/bash
>   #当参数是 1 时输出 a , 2 时输出 b , 其他时输出 other
>   case $1 in
>        "1")
>                echo "a"
>                ;;
>        "2")
>                echo "b"
>                ;;
>        *)
>                echo "others"
>                ;;
>   esac
>   ```
>
>   

### 案例八

**for语句**

> - 代码
>
>   ```shell
>   #!/bin/bash
>   #求1+到100的和
>   #for
>   SUM=0
>   for(( i=1;i<=$1;i++ ))
>   do
>           SUM=$[$SUM+$i]
>   done
>   echo "By For: The sum = $SUM"
>   
>   #Aalgorithm
>   TEMP=$(((1+$1)*$1))
>   RES=`expr $TEMP / 2`
>   
>   echo "By Aalgorithm: The sum =$RES"
>   ```
>
>   
>

### 案例九

**while语句**

> - 代码
>
>   ```shell
>   #!/bin/bash
>   #求1+..+n的值
>   SUM=0
>   i=0
>   while [ $i -le $1 ]
>   do
>           SUM=$[$SUM+$i]
>           i=$[$i+1]
>   done
>   echo "SUM=$SUM"
>   ```

### 案例十

**read语句**

> - 基本语法
>
>   read + 选项 + 参数
>
>   **选项：**
>   **p**:指定读取值时的提示符；
>   **t**:指定读取值时等待的时间（秒），如果没有在指定的时间内输入，就不再等待了
>   **参数：**
>   变量：指定读取值的变量名
>
> - 代码
>
>   ```shell
>   #!/bin/bash
>   #读取控制台输入的一个NUM1值
>   read -p "Please enter a number : " NUM1
>   echo "Res = $NUM1"
>   #读取控制台输入的一个值，等待10s
>   read -t 10 -p "Please enter a var :" VAR
>   echo "Res = $VAR"
>   ```