## 文件I/O

大多数 UNIX 文件I/O只需用到5个函数：open，read, write, lseek以及close. 这几个函数通常被称为不带缓存的I/O（unbuffered I/O）， —— 这里不带缓存的指的是每个 read 和 write 都调用内核中的一个系统调用。

#### 文件描述符

进程访问文件结构： 

文件描述符 --> 文件表 --> v 节点表。

进程通过读写文件描述符对文件进行访问。文件表里存有文件位置信息（读写的当前位置）以及引用计数信息。 注意子进程fork和open在对文件访问时的区别：

- fork 出的子进程继承其父进程的打开文件，文件描述符的内容与父进程的相同，指向文件表中的同一个表项。
- 通过 open 生成的文件描述符，指向文件表中的不同表项。

（CSAPP 中系统I/O章节有清晰的图例说明）

#### 共享

只要涉及在多个进程间共享资源，原子操作的概念就变得非常重要。

close 关闭一个文件时，也释放该进程加在该文件上的所有记录锁。