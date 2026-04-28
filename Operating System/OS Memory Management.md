1. Logical Address: source code/compile/run时看到的，也是程序眼里操作的地址； Physical Address: 内存硬件上真实内存的地址，是内存实际寻址时的地址。
2. 地址写入有三种情况：
<ol>
<li> Compile: 编译时期写死地址，这种情况都是知道程序将来一定被加载到哪个位置，常见于嵌入式。</li>
<li> Load: 加载时才知道地址是多少，Loader载入内存时进行重定位。这个一般由programmer控制，如dlopen函数动态加载一个库（其实加载的时候ldd可以看到使用的库。）早期DOS，内核LASLR </li>
<li> Execute: 程序到了运行时再进行动态硬件映射，常见于动态加载的库如libc。</li>
</ol>
3. 编译时和加载时绑定地址，生成的$LA==PA$。运行时进行地址绑定，程序给出的地址经过MMU转换才是物理地址，此时LA也称做虚拟地址。
4. 动态链接的库在载入内存时就知道要链接哪些库，ld-linux.so会把一次性把这些库映射到进程地址空间，但是动态加载是程序运行过程中主动加载一个外部独立的文件，在程序执行到dlopen等的时候再映射。注意，是把整个库都映射到地址空间，但是不是真的加载进内存，OS背后使用虚拟内存和缺页等机制实现调用时再载入内存。
5. 有了执行时写入地址和MMU技术才能实现page out/in等操作，因为重新调入内存后他们的地址是不固定的。
6. 带版本管理，多版本共存的动态库系统专门被叫做共享库系统。
7. kernel OS代码绑定在编译时/加载时，kernel编译时就定好内核虚拟基地址（Linux：0xffff ffff 8000 0000），属于编译时预先定好内核虚拟布局+内核自举阶段早期重定位/加载时修正。内核要最早初始化MMU，页表，内存管理，自己不能依赖还没建好的用户态虚拟内存机制，所以提前固化/早期加载重定位。现在内核也支持KASLR地址空间随机化，在开机加载再重定位，不是写死，加载时微调绑定，安全防攻击。
8. MMU放置的时PTBR/CR3，实际的页表在内存中。但是：
<ol>
<li>多级遍历时MMU专用硬件电路流水执行，延迟远低于软件实现</li>
<li>Cache（L1/L2/L3），大概率都是片内Cache命中 </li>
<li>Locality，时间和空间都有局部性，TLB本身就不易miss </li>
</ol>
9. 动态加载由OS提供库，但是不是必须的，dlopen这类函数也可以手动实现，OS理论上可以对动态加载不提供特别支持。
## Coreutils

1. ldd
2. LD_DEBUG=all
3. strace
4. gdb: info sharedlibrary, disassemble \__libc_start_main
5. readelf -a , -d, -s, -r
6. pmap

## The implement of Dynamic Linking

1. call \*\*\*     \# \<printf@plt>
2. jmp \*printf\@got.plt
3. 第一次调用时，GOT里放的是下一条指令的地址，`push $0x00`, 0x00是index
4. jmp .plt 进入通用解析函数，参数为0x00
5. 触发**ld-linux.so**
6. 找到实际的`printf`地址
7. 写回 `printf@got.plt`
8. 以后调用printf，jmp \*got 直接跳转到libc中的真实printf位置
总体上，executable挖了两个hole实现了Dynamic Linking：
+ PLT： 一段跳板代码
+ GOT： 一个存放真实地址的变量
链接时链接器ld只填写了PLT代码，没有填写真实地址，加载时加载器ld-linux.so第一次第调用时才去查libc，填到GOT中

```C
#include <stdio.h> int main() { printf("hello\n"); return 0; }
```
``` asm
0000000000401136 <main>: 
401136: push %rbp 
401137: mov %rsp,%rbp 
40113a: lea 0xe93(%rip),%rdi # 402034 <_IO_stdin_used+0x4> 
401141: call 401030 <printf@plt> 
401146: mov $0x0,%eax 
40114b: pop %rbp 
40114c: ret

...

0000000000401030 <printf@plt>: 
401030: jmp *0x2f9a(%rip) # 404030 <printf@got.plt> 
401036: push $0x0 
40103b: jmp 401020 <.plt>

```
ld会给动态链接的库函数一个编号index：
```plaintext
重定位条目 0 → 对应符号 printf 
重定位条目 1 → 对应符号 scanf 
重定位条目 2 → 对应符号 malloc ...
```
.plt：
``` asm
printf@plt: 
	jmp *GOT 
	push 0 
	jmp 解析器 
scanf@plt: 
	jmp *GOT 
	push 1 
	jmp 解析器 
malloc@plt: 
	jmp *GOT 
	push 2 
	jmp 解析器
	
# At first: GOT
GOT[printf] = 0x40106
```

## Dynamic Memory Allocation

1. `FF/BF/NF` 用在库的allocator实现，如dlmalloc, ptmalloc(glibc **default**), jemalloc, tcmalloc, 包括First Fit, Next Fit, Best Fit, 一般不适用Worst Fit
2. OS内部管理动态内存分配不能使用\*malloc因为OS自包含，不依赖任何外部库，现代OS全部使用**虚拟内存+分页**，kernel中有专门的内存分配器：Linux：SLAB/SLUB(Linux **DEFAULT**)/SLOB(Small Embedding)，Windows：Kernel Pool/NonPaged Pool，他们在page之上再封装一层“小对象分配器”，把page切成很多小块。
3. 分级页表，哈希页表，倒置页表。现代OS：x86 Linux/Windows/macOS均使用四级页表PGD -> PUD -> PMD -> PTE，ARM64: Android/iOS/macOS使用三级或四级页表。哈希页表理论上适合**超大极稀疏空间**，但是硬件不原生支持，全靠软件查表，倒置页表大小之和物理内存大小有关，是**理论最优结构**但是工程上麻烦，硬件不配合，共享难受，除了老式IBM没有OS使用
4. 对于二进制执行文件的请求调页，有些系统试图限制交换空间的用量。这些文件的请求调页是从文件系统中直接读取的。然后，当需要页面置换时，这些帧可以简单的覆盖，当再次需要时，从文件系统中再次直接读入。为什么？因为二进制文件只读数据段不会被修改，swap内的和disk中的一样，完全没必要浪费swap space，而且大多数程序运行后热点代码就保留再内存内，不常用的被换出，即使demanding page到了这部分也影响不大。