

# 标准IO

## 导学

> hardware --驱动---api--应用
>
> IO操作（文件操作）（input/output）
>
> 进程创建与回收 
>
> 多线程编程技术
>
> 进程间通信技术（同步  互斥）
>
> API：全称Application Programming Interface，即应用程序编程接口。
>
> API是一些预先定义函数，目的是用来提供应用程序与开发人员基于某软件或者某硬件得以访问一组例程的能力，并且无需访问源码或无需理解内部工作机制细节。
>
> API就是操作系统给应用程序的调用接口，应用程序通过调用操作系统的 API而使操作系统去执行应用程序的命令（动作）。在 Windows 中，系统API是以函数调用的方式提供的。

## 介绍

> 文件的概念和类型
>
> r常规文件/d目录文件/c字符设备文件/b块设备文件  /p管道文件/s套接字文件/l符号连接文件 
>
> 白字      /蓝字        /黄字 键盘	     /黄字 磁盘 	/ 进程间通讯 / 网络编程 /
>
> 应用代码  c库函数   系统调用
>
> 字符设备文件: 键盘
>
> 块设备文件:硬盘 
>
> 系统调用-->内核： 实现了硬件软件的隔离
>
> 如何理解==标准IO==
>
> ==ANSI C 标准定义==
>
> c库  是标配
>
> 标准I/O缓冲机制（减少调用）  cpu--内存--磁盘
>
> 流（file）的含义
>
> FILE，又叫stream（文本流，二进制流）
>
> 流的缓冲类型
>
> 全缓冲（缓冲区无数据或者无空间 才执行I/O）
>
> 行缓冲（eg：遇到换行符再执行i/o）
>
> 无缓冲（数据直接写入，流不进行缓冲）
>
> stdin 输入流 0 键盘  行缓冲
>
> stdout 输出流 1 屏幕  行缓冲
>
> stderr 错误流 2 出错提示   无缓冲
>
> ![img](F:\Typora\clipboard.png)
>
> 1024 次 之后输出字符

## 打开

> 函数  FILE *fopen(const char *path,constchar*mode)
>
> (打开路径和文件名)(打开的模式)
>
> r 只读方式     r+读写方式    替换式写入   r+b=r
>
> w只写方式    w+ 读写方式  清空后写入	w+b=w
>
> a只写方式     a+读写方式     尾部追加写入	a+b=a
>
> (b是bin的意思, 01 数据流, linux系统不区分IO流和文本流)
>
> ![img](F:\Typora\clipboard-1625215559383.png)
>
> 
>
> ![](F:\Typora\clipboard-1625215559386.png)
>
> 

## 关闭

> 标准IO 关闭流
>
> 函数int fclose(FILE *stream);
>
> fclose()调用成功返回0，失败返回EOF，并设置errno。
>
> 流关闭的时候自动刷新缓冲区中的数据并释放缓冲区
>
> 当一个程序正常终止时，所有流都会被关闭
>
> 流一但关闭后就不能执行任何操作

## 错误信息处理

> 标准IO处理错误信息 函数
>
> extern int errno;
>
> void perror(const char *s);
>
> char*strerror(int errno);
>
> errno 存放错误信号，由系统生成
>
> perror先输出字符串s，再输出错误号对应的错误信息
>
> strerror 根据错误号返回对应的错误信息

一个程序能打开的流的数量最多为1021+stdin+stdout_stderr   一共1024个

## **字符输入和输出**

getc putc fgetc fputc

## **行输入和行输出**

gets puts fgets fputs

## **对象的输入和输出**

#include<stdio.h>

size_t fread(void*ptr, size_t size, size_t n, FILE *fp);

​					to 			大小		个数		from

size_t fwrite(const void *ptr,size_t size,size_t n,FILE *fp);

​					from				大小			个数		to

成功返回读写对象的个数,出错时返回EOF,即可以读写文本文件,也可以读写数据文件,效率高

## **流的刷新和定位**

> 刷新流
>
> 
>
> #include<stdio.h>
>
> int fflush(FILE *fp);
>
> 成功时返回0;出错时返回EOF;
>
> 将流缓冲区中的数据写入实际的文件
>
> linux下智能刷新输出缓冲区,输入缓冲区丢弃

> 定位流
>
> ```c
> #include<stdio.h>
> 
> long ftell(FILE *stream);  //显示流指针当前位置
> 
> long fseek(FILE *stream,long offset, int whence); // 以参考点调节流指针的位置
> 
> 		//流指针		  偏移量(左减右加)   参考点
> 
> void rewind(FILE *stream);//将流指针重新 定位到文件开始位置
> ```
>
> 
>
> ftell()成功时返回流的当前读写位置,出错时返回EOF;
>
> fseek()定位一个流,成功时返回0;出错时返回EOF;
>
> whence 参数: SEEK_SET(文件头)/SEEK_CUR(文件当前位置)/SEEK_END(文件尾)
>
> 



> 判断流是否出错和结束
>
> ```c
> #include <stdio.h>
> 
> int ferror(FILE *stream);//返回1时表示出错,否则返回0
> 
> int feof(FILE *stream);	//返回1时表示文件已到末尾,否则返回0
> ```
>
> 



## **格式化输入和输出**

sprintf  /  fprintf

#include<stdio.h>

int printf(const char *fmt,...);		//格式化输出 

int fprintf(FILE *stream, const char *fmt,...);  //输出到文件

int sprintf(char* s, const char * fmt,...);	//输出到字符串

成功时返回输出的字符个数;出错时返回EOF;





# 文件IO

## 文件IO简单了解

### 介绍

posix(可移植操作系统接口)定义的一组函数

文件描述符 0 1 2 

open/close/read/write

open 

```c
#include<fcntl.h>

int open(const char *pathname,int flags,mode_t mode);
//     包括路径的文件名	  打开方式	被打开文件的存取权限8进制表示法
```

​				

### 打开

```c
#include<fcntl.h>

int fd;

if((fd = open("1.txt", O_RDWR|O_CREAT|O_EXCL,0666)<0)
{
		if(errno ==EEXIST)
		{
			perror("exist error");	
		}
		else
		{
			perror("other error");
		}
}
```



### 关闭

close函数用来关闭一个打开的文件:

```c
#include<unistd.h>
int close(int fd);
```

成功时返回0;出错时返回EOF

程序结束时自动关闭所有已打开的文件

文件关闭后,文件描述符不再代表文件

### 读取文件

read

```c
#include <unistd.h>
ssize_t read(int fd,void *buf,size_t count);
//		文件描述符	读缓冲区首地址		读的大小
```

成功时返回实际读取的字节数,出错时返回EOF

读到文件末尾时返回0

count不应超过buf大小

### 写入文件

write 

```c
#include<unistd.h>
ssize write(int fd,void *buf,size_t count);
```

成功时返回实际写入的字节数,出错时返回EOF

buf是发送数据的缓冲区

count不应超过buf大小



### 定位文件

lseek

```c
#include <unistd.h>
off_t lseek(int fd,off_t offset,intt whence);
```

成功时返回当前的文件读写位置,出错时返回EOF

参数offset和参数whence同fseek完全一样

## 文件IO进阶操作

### 操作目录

#### 读取目录

> opendir函数
>
> ```c
> #include<dirent.h>
> DIR *opendir(const char *name);
> ```
>
> DIR使用来描述一个打开的目录文件的结构体类型
>
> 成功时返回目录流指针,出错时返回NULL

#### 访问目录

> readdir函数用来读取目录流中的内容
>
> ```c
> #include<dirent.h>
> struct dirent *readdir(DIR *dirp);
> ```
>
> struct dirent 使用来描述目录流中一个目录项的结构体类型:内容如下图
>
> ![image-20210719200326918](F:/Typora/image-20210719200326918.png)
>
> 包含成员 char d_name[256];
>
> 成功时返回目录流dirp中下一个目录项;
>
> 出错或到末尾时返回NULL;
>
> eg:
>
> ![image-20210720102642329](F:/Typora/image-20210720102642329.png)

#### 关闭目录

> closedir 关闭一个目录文件
>
> ```c
> #include<dirent.h>
> int closedir(DIR *dirp);
> ```
>
> 成功时返回0;出错时返回EOF;

### 修改文件访问权限

> chmod  /  fchmod
>
> #include<sys/stat.h>
>
> int chmod(const char*path, mode_t mode);
>
> int fchmod(int fd,mode_t mode);
>
> 成功时返回0,出错时返回EOF;
>
> root和文件所有者能修改文件的访问权限
>
> eg:
>
> chmod("test.txt",0666);

### 获取文件属性

> stat/lstat/fstat 函数用来获取文件属性
>
> ```c
> #include<sys/stat.h>
> int stat(const char *path, struct stat*buf);//当指定一个链接文件时, 查看的是链接的目标文件的属性
> int lstat(const char *path,struct stat*buf);//当指定一个链接文件时,查看的是链接文件本身的属性
> //				文件名,		结构体
> int fstat(int fd, struct stat *buf);
> ```
>
> 成功时返回0;出错时返回EOF
>
> 如果path是符号链接stat获取的是目标文件的属性,而lstat获取的是链接文件的属性
>
> > struct stat 是存放文件属性的结构体类型: 内容如下
> >
> > 1. mode_t	 st_mode	类型和访问权限
> > 2. uid_t 	st_uid	所有者id
> > 3. uid_t	st_gid	用户者id
> > 4. off_t	st_size	文件大小
> > 5. time_t	st_mtime	最后修改时间

### 程序库概念和静态库制作

/lib  或者 usr/lib中存放库文件.(通常是已经写好的可调用的函数)

> 静态库
>
> 1.特点:
>
> ​	编译(链接)时把静态库中相关代码复制到可执行文件中
>
> ​	程序中已包含代码,运行时不再需要静态库
>
> ​	程序运行时无需加载库,运行速度快
>
> ​	占用更多磁盘和内存空间
>
> ​	静态库升级后,程序需要重新编译链接
>
> 2.创建:  (2~7)
>
> ​	确定库中函数的功能和接口
>
> ​	编写库源码hello.c
>
> ```c
> #include<stdio.h>
> void hello(void){
>  	printf("hello world\n");
>  return;
> }
> ```
>
> 3.编译生成目标文件
>
> ​	$gcc -c hello.c -Wall
>
> 4.创建静态库
>
> ​	$ ar crs  libhello.a
>
> ​	hello.o
>
> 5.查看库中符号信息
>
> ​	$nm libhello.a
>
> ​	hello.o:
>
> ​	0000000 T hello
>
> ​					U puts
>
> 6.编写应用程序test.c
>
> ```c
> #include <stdio.h>
> void hello(void);
> int main()
> {
> 	hello();
> 	return 0;
> }
> ```
>
> 7.编译test.c并连接静态库libhello.a
>
> ​	$ gcc -o test test.c -L.  -lhello
>
> ​	$./test
>
> ​	hello world



### 动态库的制作和使用

> 又叫共享库
>
> 1.特点:
>
> ​	编译(链接)时仅记录哪个共享库的哪个符号,不复制共享库中相关代码
>
> ​	程序不包含库中代码,尺寸 小
>
> ​	多个程序可共享同一个库
>
> ​	程序运行时需要加载库
>
> ​	库升级方便,无需重新编译程序
>
> ​	使用更加广泛
>
> 2.创建:(2~7)
>
> ​	确定库中函数的功能,接口
>
> 3.编写库源码hello.c  bye.c
>
> ```c
> #include<stdio.h>
> void hello(void)
> {
> 	printf("hello world\n");
> 	return 0;
> }
> ```
>
> 4.编译生成目标文件
>
> ​		$gcc  -c  ==-fPIC==  hello.c  bye.c  -Wall
>
> ​				位置无关代码
>
> 5.创建共享库common
>
> ​	$gcc -shared -o libcommon.so.1  hello.o    //1  是版本号
>
> ​	bye.o
>
> 6.为共享库文件创建链接文件
>
> ​	$ln -s libcommon.so.1  libcommon.so
>
> 7.符号链接文件命名规则
>
> ​	lib<库名>.so
>
> 8.如何找到共享库
>
> ​	为了让系统能找到共享库,一共由三种办法
>
> ​	a.把库拷贝到/usr/lib  或者/lib目录下 (不推荐)
>
> ​	b.在LD_LIBRARY_PATH环境变量中添加库所在路径
>
> ​	c.添加/etc/ld.so.conf.d/*.conf文件,执行ldconfig刷新
>
>  

# 作业

## D1


打开一个不存在的文件，分别使用perror和strerror打印错误信息

## D2



使用标准IO写2个学生的结构体数据到文件

## D3

遍历一个文件夹下所有文件，并打印文件大小和日期

