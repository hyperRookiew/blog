---
title: 高性能服务器：Linux下的信号与后台守护进程
categories: [技术]
tags:
  - linux
  - 高性能服务器
date: 2021-11-20 19:47:47
---

> 有兴趣搞一搞高性能流媒体服务器了，这些得了解，学习过程中记录一下，方便未来回顾。

### 一、Linux 下的几个重要信号
- SIGPIPE：管道终止信号，当写入无人读取的管道时产生该信号，默认终止进程，需要我们去处理
    - 网络程序必须要处理，否则Client端断开连接之后，会导致Server Crash
- SIGCHLD：子进程结束或者停止发送时候
    - 容易产生僵尸进程（一个早已死亡的进程，但是在进程表中还存在）
    - 子进程结束的时候，他并没有完全销毁，因为父进程还需要使用它的消息。
    - 父进程没有处理SIGCHLD信号，或者没有调用wait/waitpid等待子进程结束，就会出现僵尸进程。
- SIGALRM: 定时信号，秒为单位，默认会终止进程，所以需要我们去处理（捕获，然后忽略）
- SIGINT: 键盘输入的退出信号
- SIGQUIT： 键盘输入的退出信号
- SIGHUP：控制终端挂起信号

<!--more-->

### 二、信号的发送和处理
- 硬件方式
    - ctrl+c，ctrl+\

### 三、安装信号
##### signal(int sig, void (*func)(int));
```c++
#include <iostream>
#include <signal.h>
#include <unistd.h>

void sighandle(int sig)
{
    std::cout << "receive signal:" << sig << std::endl;
}

int main(int argc, char *argv[])
{
    signal(SIGINT, sighandle); // 捕获SIGINT信号
    signal(SIGQUIT, sighandle);
    signal(SIGHUP, sighandle);
    pause();
}
```

##### 通过 sigaction方法
- sigaction
```c++
struct sigaction
{
    void (*sa_handler)(int sig); // 捕获到sign的处理函数，需要设置
    void (*sa_sigaction)(int, siginfo_t *, void *); // 与上类似，一般不用
    sigset_tsa_mask; // 掩码，需要设置
    int sa_flags; //根据SA_SIGINFO标记是选择sa_handler还是sigaction进行处理，需要设置
};
```
- Demo

```c++
#include <iostream>
#include <signal.h>
#include <unistd.h>
void sig_handler(int sig)   // 处理函数
{
    std::cout << "receive signal:" << sig << std::endl;
}
int main(int argc, char *argcs[])
{
    struct sigaction act, oact;
    act.sa_handler = sig_handler;
    sigfillset(&act.sa_mask); // 设置掩码
    act.sa_flags = 0;   // 选择使用sighandler来处理
    sigaction(SIGINT, &act, &oact);
    sigaction(SIGQUIT, &act, &oact);
    pause();
    return 0;
}
```

### 四、后台进程
##### fork方式
- 四个步骤
    - fork 一个子进程，父进程退出了，那么子进程将成为孤儿进程，被init进程接管
    - 调用setsid建立新的进程会话
    - 将当前工作目录切换到更目录（因为父亲进程已经没了，init进程的工作目录在根目录）
    - 将标准输出，输入，出错重定向到 /dev/null
- Demo 代码

```c++
#include <iostream>
#include <unistd.h> // fork
#include <stdlib.h> // for session
#include <fcntl.h>  // for open
void daemonize()
{
    /*step1: fork一个子进程*/
    int fd;       // file descriptionor
    pid_t pid;    // 进程id
    pid = fork(); // 当前进程创建一个子进程，pid为子进程的id
    if (pid < 0)
    {
        std::cout << "can't create suprocess!" << std::endl;
        exit(-1);
    }
    else
    {
        if (pid != 0) // 判断子进程是否创建成功，为0则创建成功，非0则为父进程
        {
            exit(0); // 父进程退出，子进程成为孤儿进程，被init进程接管
        }
    }
    /*strp2: 建立新的进程会话*/
    setsid(); //调用setsid来创建新的进程会话。这使得daemon进程成为会话首进程，脱离和terminal的关联。
    /*step3： 切换工作目录到根目录*/
    if (chdir("/") < 0) // 将当前工作目录切换到根目录。父进程继承过来的当前目录可能mount在一个文件系统上
    {
        std::cout << "can't change dir!" << std::endl;
        exit(-1);
    }
    // 将标准输入、输出、错位重定向到根目录
    fd = open("/dev/null", O_RDWR); // O_WRWR，可读可写
    dup2(fd, STDIN_FILENO);         // 重定向方法，dup2()
    dup2(fd, STDOUT_FILENO);
    dup2(fd, STDERR_FILENO);
    return;
}
int main(int argc, char *argcs[])
{
    daemonize(); // 调取这个程序之后，就将程序切换到后台
    while (1)
    {
        sleep(1);
    }
    return 0;
}
```

##### 调用系统的daemon API
- linux的系统函数，其实实际上也是走上边的四个步骤,推荐优先使用
    - unistd.h中的daemon() api
-demo

```c++
#include <iostream>
#include <unistd.h>
#include <stdlib.h>
int main(int argc, char *argcs[])
{
    /* Put the program in the background, and dissociate from the controlling
    terminal.  If NOCHDIR is zero, do `chdir ("/")'.  If NOCLOSE is zero,
    redirects stdin, stdout, and stderr to /dev/null.  */
    if (daemon(0, 0) == -1)
    {
        std::cout << "error" << std::endl;
        exit(-1);
    }
    // 以上代码便可以实现将进程切换到后台运行，之后再执行其他语句
    while (1)
    {
        sleep(1);
    }
    return 0;
}
```

### 总结
似乎对linux又友好一点了