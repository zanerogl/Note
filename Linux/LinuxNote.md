# LinuxNote





### Linux内核（源码）保存在哪里？

路径：

```shell
/usr/src/
```

----

### 进程

- **进程号**

  > **范围：** 0~3267
  >
  > 进程号总是唯一的，但进程号可以重用。当一个进程终止后，其进程号就可以再次使用了。
  >
  > 进程号为0及1的进程由内核创建。
  > 进程号为0的进程通常是调度进程，常被称为交换进程(swapper)。
  > 进程号为l的进程通常是init进程，nit进程是所有进程的祖先。
  > 除调度进程外，在linux下面所有的进程都由进程nit进程直接或者间接创建

  ----

- **参数解释**

  > **PPID**：当前进程的父进程的进程号
  > **PID**：当前进程的进程号
  > **PGID**：当前进程所在的组的进程组ID
  > **COMMAND**：当前进程的名字

  ----

- **获取进程号函数**

  > - **代码：**
  >
  > ```c++
  > #include<iostream>
  > #include<sys/types.h>
  > #include<unistd.h>
  > 
  > using namespace std;
  > 
  > int main(){
  >     //查看当前进程的进程号
  > 	printf("pid = %d\n", getpid());
  > 
  > 	//查看当前进程的父进程号
  > 	printf("ppid = %d\n", getppid());
  > 
  > 	//查看当前进程所在进程组的id
  > 	printf("pgid = %d\n", getpgid(getpid()));
  >     
  > 	while(1){//方便查看进程id
  > 	
  > 	}
  > 	return 0;
  > }
  > ```
  >
  > **运行结果：**
  >
  > ```shell
  > pid = 11260
  > ppid = 7519
  > pgid = 11260
  > ```

  ---

- **fork()函数**

  > - **功能**
  >
  >   frok()函数用于从一个已存在的进程中创建一个新进程，新进程称为子进程，原进程称为父进程。
  >
  > - **注意**
  >
  >   使用frok()函数得到的子进程是父进程的一个复制品，它从父进程处继承了整个进程的地址空间。
  >
  >   父子进程是独立运行的
  >
  >   父子进程是来回交替执行的，谁先谁后是不确定的
  >
  > - **代码**
  >
  >   ```c++
  >   #include<iostream>
  >   #include<stdlib.h>
  >   #include<unistd.h>
  >   
  >   using namespace std;
  >   
  >   int main(){
  >   #if 0
  >   	//通过fork创建一个子进程
  >   	fork();
  >   	cout<<"Hello"<<endl;
  >   	while(1);
  >   #endif
  >   	//通过fork函数的返回值来区分父子进程的独立的代码区
  >   	pid_t pid;
  >   	pid = fork();
  >   	if(pid<0){	  //fork()创建进程失败时返回值为-1
  >   		perror("Fail to fork");
  >   		return -1;
  >   	}
  >   	else if(pid > 0){ //父进程
  >   		while(1){
  >   			cout<<"Partent process ppid: "<<getppid()<<"  pid: "<<getpid()<<endl;
  >   			sleep(1);
  >   		}
  >   	}
  >   	else{		  //子进程
  >   		while(1){
  >   			cout<<"Son process ppid: "<<getppid()<<" pid:  "<<getpid()<<endl;
  >   			sleep(1);
  >   		}
  >   	}
  >   	return 0;
  >   }
  >   ```
  >
  >   - **运行结果**
  >
  >     ```c++
  >     Partent process ppid:  7519  pid: 11649
  >     Son process ppid: 11649 pid: 11650
  >     Son process ppid: 11649 pid: 11650
  >     Partent process ppid:  7519 pid: 11649
  >     Son process ppid: 11649 pid: 11650
  >     Partent process ppid:  7519 pid: 11649
  >     Son process ppid: 11649 pid: 11650
  >     Partent process ppid: 7519 pid: 11649
  >     ```
  >
  >     

  ----

  

