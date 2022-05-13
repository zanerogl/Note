## open()  read()  write()

#### 样例：将旧文件复制为新文件

```c
/** 
	复制一个文件 
				 **/
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
 
 //步骤：打开目标文件->读取目标文件->创建新文件->写入新文件
 
// 执行：./copy [filename0] [filename1]

int main(int argc, char **argv)		//argc为长度，argv为内容
{
	int fd_old, fd_new;	//新文件和旧文件的变量
	
	//read()函数要用到的参数
	char buf[1024];		//缓冲区大小
	int len;			//读取的文件的长度
	
	//错误输入
	if(argc != 3){		//命令参数不为3则退出
		printf("Usage: %s <old-file> <new-file>\n", argv[0]);
		return -1;
	}
	
	//打开目标文件
	fd_old = open(argv[1] , O_RDWR);	//用可读可写的方式打开目标文件（rdonly）
	if(fd_old == -1){
		printf("can not open the file %s\n", argv[1]);
		return -1;
	}
	
	//读取目标文件
	len = read(fd_old, buf, 1024);
	if(len == -1){
		printf("can not read the file %s\n", argv[1]);
		return -1;
	}
	
 	//创建新文件
	fd_new = open(argv[2], O_RDWR | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR | S_IRGRP | S_IWGRP | S_IROTH | S_IWOTH);
	
	/*
	O_RDWD：可读写
	O_CREAT：创建文件
	O_TRUNC：将原文件内容丢弃大小置为0
	参数3：用户、组、其他的权限设置
	*/
	
	//写入新文件
	len = write(fd_new, buf, len); 
	if(len == -1){
		printf("can not write the file %s\n", argv[1]);
		return -1;
	}
	return 0;
}
```

**进阶**

*通过内存映射的方式读取文件并写入*

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>
#include <sys/mman.h>

/*
	执行：./copy_mmap [filename0] [filename1]

*/

int main(int argc, char **argv)
{
	int fd_old, fd_new;
	int len = 0;
	char *buf;
	struct stat stat;
	
	//输出错误
	if(argc != 3){
		printf("Usage: %s [filename0] [filename1]\n", argv[0]);
		return -1;
	}
	
	fd_old = open(argv[1], O_RDWR);	//返回值为一个文件描述符，Linux的文件描述符是一个非负整数
	if(fd_old == -1){
		printf("Can not open %s\n", argv[1]);
		return -1;
	}
	
	//通过确定文件状态的方式来排除太大的文件？
	if(fstat(fd_old, &stat) == -1){
		printf("Can not get stat of file %s\n", argv[1]);
		return -1;
	}
	//通过内存映射的方式将fd_old的内容存入缓冲变量buf
	//buf = mmap(NULL, stat.st_size, PROT_READ, MAP_SHARED, fd_old, 0);//标准写法
	buf = mmap(NULL, 1024, PROT_READ, MAP_SHARED, fd_old, 0);
	//创建新文件
	fd_new = open(argv[2], O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR | S_IRGRP | S_IWGRP | S_IROTH | S_IWOTH);
	
	//将缓冲变量写入新文件
	//len = write(fd_new, buf, stat.st_size);//标准写法
	len = write(fd_new, buf, 1024);
	
	return 0;
}
```

