### Workflow
execve() -> load ld-linux.so -> 链接器初始化 -> 链接器加载libc.so等动态链接库 -> 链接器跳转到libc \_start -> \_start初始化栈/寄存器 -> \_\_libc_start_main -> libc全局初始化（malloc/stdio/atexit/pthread) -> 调用main(argc, argv, envp) -> main return -> \_\_libc_start_main进行清理 -> exit(main()) -> process terminate

### Attention
1. OS Kernel调用syscall execve来创建进程地址空间，解析ELF文件，如果是Dynamic Linking才跳转到ld-linux.so，静态链接程序的\_start入口在ELF内，内核负责把argc, argv[], envp[]放在程序栈顶，这是整个流程唯一的参数来源。
2. ld-linux.so动态链接器，这是加载的第一个用户态代码，它加载并初始化libc之后跳转到\_start符号处，自此由lib接管程序执行。
3. sysdeps/x86_64/start.S
``` asm
.global _start 
	_start: 
	# 栈顶此时是：argc (内核放的) 
	# 栈+8: argv[0] 
	# 栈+16: envp[0] 
	mov %rsp, %rdi # 把栈指针传给 __libc_start_main 
	xor %ebp, %ebp # 清空ebp，标记栈底（防止栈回溯出错） 
	call __libc_start_main # 调用C语言初始化函数 
	# 永不执行到这里：__libc_start_main 内部会调用exit 
	hlt
```
4. \_\_libc_start_main
```C
int __libc_start_main ( 
	int (*main) (int, char **, char **), // 用户main函数 
	int argc,                            // 参数个数 
	char **argv,                         // 参数数组 
	void (*init) (void),                 // 构造函数初始化 
	void (*fini) (void),                 // 析构函数清理 
	void (*rtld_fini) (void),            // 链接器清理 
	void *stack_end,                     // 栈顶地址 
	struct tcbhead *tcb                  // 线程控制块 
) __attribute__ ((noreturn));
```

Flow: 
```C
// _start 传递的栈数据
ARGC = argc;
ARGV = argv;
__environ = envp = argv + argc + 1 # global __environ

// 初始化进程全局标识，供后续系统调用使用
__libc_pid = getpid(); // store PID
__libc_uid = getuid(); // store UID 
__libc_gid = getgid(); // store GID

// 线程库 / 栈初始化
__pthread_initialize_minimal(); // initialize thread library
__stack_end = stack_end;        // store the top of stack

// atexit函数会把清理函数加入调用链表,进程退出时逆序执行这些函数
atexit(rtld_fini); 
atexit(fini);

// 执行所有 __attribute__((constructor)) 函数, 执行C++全局对象构造函数
init(); 

// main(argc, argv, envp)
exit(main(argc, argv, envp));
```

exit() 是libc的标准退出函数，完成所有清理工作并调用kernel
```C
// stdlib/exit.c

void exit(int statuc) {
	// all threads can seen this 
	__exitcode = status;
	
	// atexit registered func
	__run_exit_handlers();
	
	// Close all IO stream(flush)
	_IO_cleanup();
	
	// syscall (kill process)
	_exit(status);
}

void __run_exit_handlers(void) {
	while (exit_function_list) {
		void (*func)() = exit_function_list->func;
		exit_function_list = exit_function_list->next;
		func();
	}
}

// _exit(status)
// syscall(SYS_exit, status)
// release all resources (fd, memory, signal ...)
// send SIGCHILD signal to parent process
// parent process can `status` by wait()
```

5. main()调用完成后不能直接结束，因为libc必须执行**清理工作**（刷新缓冲区、执行析构函数、关闭文件），否则会数据丢失 / 资源泄漏。

