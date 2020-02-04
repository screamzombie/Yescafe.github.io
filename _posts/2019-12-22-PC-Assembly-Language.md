---
layout: post
title: PC Assembly Language - 6.828 Assembly Guide Book
categories: Assembly
description: MIT 6.828 指定的汇编参考书
keywords: Assembly, 6.828
---

csapp, 还有以后可能还会读一些汇编的书, 所以专门开了一个汇编 category. 而且还放在 6.828 的分类下面.    
Lab1 真的挺麻烦的, 看不懂代码, 需要的前置知识有点小多, 汇编就是一个部分.  
于是, 啃这个 828 的指定汇编书.   

因为内容不算太多, 所以就放在一篇里面了. 新的这个博客模板也挺方便的, 旁边会自动生成目录.  

## 第1章 - 导论
最开始的导论部分说了一些基础知识.    

### 1.1 数值系统  
内存不会直接去存十进制数, 因为还是存二进制数简单, 这就不用多解释了.  

#### 1.1.1 十进制数  
十进制数, 就是每一位乘上一个以10为底数的幂后求和, 具体是什么举个例子:    
$234 = 2 \times 10^2 + 3 \times 10^1 + 4 \times 10^0$  

#### 1.1.2 二进制数
二进制位之可能是 1 或者 0, 表示方法类似十进制, 举个例子:  
$11001_2\\
= 1 \times 2^4 + 1 \times 2^3 + 0 \times 2^2 + 0 \times 2^1 + 1 \times 2^0 \\
= 16 + 8 + 1 \\
= 25$    
这个过程就是进制转换成十进制的方法.   
然后写了一些关于二进制加法, 乘法, 除法余数的内容, 内容比较多但是比较简单就不复制了.  

#### 1.1.3 十六进制数  
十六进制也好说, 表示一个位的数有十六个, A表示十进制 10, B 表示十进制 11, 以此类推. 举例:  
$2BD_{16} \\
= 2 \times 16^2 + 11 \times 16^1 + 13 \times 16^0 \\
= 512 + 176 + 13
= 701
$  
后面说了一下怎么用 8421 码转换二进制为十六进制, 很简单, 这辈子也都不会忘, 所以略过.  

### 1.2 计算机结构  
#### 1.2.1 Memory (内存)
在没有弄清楚 memory 这个词的实际意义之前, 先不在这里把翻译标成"内存", 即使十有八九都是对的.  

存储空间 (memory) 有着一套特殊的计量单位体系, 最小的单位是 1 字节(1 byte), 注意不是 1 个位(1 bit). 一台有 32 megabytes(32 MiB) 存储空间(原文依旧是 memory)可以保存 32 兆字节(32 million bytes)的数据信息. *看到这个兆字节我就想到了 Android O 开始中文语言环境下存储空间单位也变成汉字(类似"兆字节""吉字节")这种睿智的改动.* 在内存 (memory) 中的每一个字节, 都被标上了一个独一无二的标签, 这就是**地址 (address)**.  

内存在通常情况下都是一大块一大块得使用的(原文是 is used in lager chunks than single bytes). 在 PC 架构中, 这些较大的块 (sections) 经常是以 2 字节的*字* (word), 4 字节的*双字* (double word), 8 字节的*四字* (quad word) 和 16 字节的*节* (paragraph) 出现的, 除了最后一个, 其他的东西也基本都在 csapp 里见过了.   

内存里的数据都是以数值 (numeric) 的方式存储的(这也是必然, 毕竟二进制不管是转十六进制还是十进制都比较容易). 字符的话, 需要一套合适的*字符码 (character code)*, 来提供到数字的映射. 比较常见的, 美国标准信息交换码, 也就是俗称的 ASCII . 后来出来一个升级版的叫 Unicode, ASCII 表示一个字符要一个字节, Unicode 表示一个字符要一个字. 这样就能表示更多的字符了.  

#### 1.2.2 CPU
总是说处理器频率, 算力什么的, 到底这只是一个执行指令的物理设备. 中央处理器处理的通常都是一些最简单的指令, 这些指令通常需要在 CPU 自己内部的一些*特殊存储 (special storage)* 里的数据, 这些*特殊存储*就是**寄存器 (registers)** 了. 这些寄存器的访问速度比内存要快上太多. *在 csapp 里提到的存储分级金字塔中, 寄存器是 L0, 内存是 L4, 中间还夹着三级高速缓存呢, 这怎么比.* 但是因为寄存器的数量有限(造价昂贵?), 程序员在编写代码的时候需要注意怎么分配使用寄存器了.  

在CPU里跑的是最基础的**机器语言 (machine language)**, 这些机器语言的指令是纯数字, 而且还是二进制, 或者可以表示成十六进制, 但是还是太难看了. 于是人们就发明了一些更方便用文本字符形式描述的语言, 但是这些语言都必须要通过一些手段翻译 (deciphered) 成机器码, 这些用于翻译的程序被叫做**编译器 (compiler)**程序.   
每种类型的CPU的机器码都是独特的, 所以mac上写的机器码不能在IBM机子上跑.  

计算机会使用**时钟 (clock)** 来*同步指令执行 (to synchronize the execution of the instructions)*. 这个时钟会以固定的频率发出脉冲(这个固定频率一般被叫做**时钟速度 (clock speed)**). 比如你买了个 1.5GHz 的电脑, 那它的时钟发脉冲的频率就是 1.5GHz. 这个单位挺有意思, Hz 好像不常识中的按每分钟计的, 而是以一个常数比率进行节拍 (beats). CPU 的各个部分会根据这个节拍(通常被称为**时钟周期 (cycle)**) 让他们的操作正确的进行, 就像节拍器能帮助曲子以正确的旋律演奏一样. *真可爱, 这是原文的比喻哦~* 一个指令需要多少个这样的节拍执行, 是跟 CPU 代数和型号有关系的. *这意思就是不能光看频率了? 频率相同性能也可能不同?* 

#### 1.2.3 CPU 80x86 系列
IBM 型 PC 有一块来自 80x86 家族的 CPU, 这些 CPU 只是代数或者型号不同, 都是一个妈生的, 所以*基础机器语言*是一样的, 但是每一代都有增强 (enhance). 下面是原文:  
> **8088,8086:** These CPU’s from the programming standpoint are identical. They were the CPU’s used in the earliest PC’s. They provide several 16-bit registers: AX, BX, CX, DX, SI, DI, BP, SP, CS, DS, SS, ES, IP, FLAGS. They only support up to one megabyte of memory and only operate in *real mode*. In this mode, a program may access any memory address, even the memory of other programs! This makes debugging and security very difficult! Also, program memory has to be divided into *segments*. Each segment can not be larger than 64K.  
> **80286:** This CPU was used in AT class PC’s. It adds some new instructions to the base machine language of the 8088/86. However, its main new feature is 16-bit protected mode. In this mode, it can access up to 16 megabytes and protect programs from accessing each other’s memory.  
> However, programs are still divided into segments that could not be bigger than 64K.  
> **80386:** This CPU greatly enhanced the 80286. First, it extends many of the registers to hold 32-bits (EAX, EBX, ECX, EDX, ESI, EDI, EBP, ESP, EIP) and adds two new 16-bit registers FS and GS. It also adds a new 32-bit protected mode. In this mode, it can access up to 4 gigabytes. Programs are again divided into segments, but now each segment can also be up to 4 gigabytes in size!  
> **80486/Pentium/Pentium Pro:** These members of the 80x86 family add very few new features. They mainly speed up the execution of the instructions.  
> **Pentium MMX:** This processor adds the MMX (MultiMedia eXentions) instructions to the Pentium. These instructions can speed up common graphics operations.  
> **Pentium II:** This is the Pentium Pro processor with the MMX instructions added. (The Pentium III is essentially just a faster Pentium II.)   

太多了不翻译了, 也挺好读的. 强调两个我关注到的点:  
1. 初代处理器提供了 14 个 16 位寄存器, 并且只支持 1MiB 的内存, 而且是在*实模式 (real mode)* 下操作的. 并且程序内存被分成了*段 (segment)*, 每个段最大只能有 64KiB. *这就是在以前的游戏机卡带只有那么小的原因?*  
2. 在 80386 那一代, 已经进入了 32 位处理器时代, 很多以前 16 位的寄存器被升级成了 32 位的, 新加入了两个 16 位寄存器, 引入了全新的 32 位保护模式.  

仅从这两点就可以看出, 这升级也是有够明显的.  

#### 1.2.4 8086 16 位寄存器
8086CPU 提供了四个一般用途 (general purpose) 的寄存器, 或者叫通用寄存器吧, AX, BX, CX 和 DX. 这四个寄存器都能被分解成 2 个 8 位寄存器, 比如 AX 可以被分成 AH 和 AL. *8 位? 8 位也就是一个字节, 于是自然这就是高 8 位和低 8 位了, AH 的 H 表示 HIGH, AL 的 L 表示 LOW.* 虽然可以拆, 但不代表他们独立, 修改了 AX 的值, AH 和 AL 的值也会随之改变, 并且? *vis versa*? 搜了一下好像是"反之亦然"的意思.

SI 和 DI 通常作为指针使用, 也可以当通用寄存器, 但是不能被分解成两个 8 位的.   

BP 和 SP 通常用于指向机器语言栈中的数据.  

CS, DS, SS 和 ES被叫作*段寄存器 (segment registers)*, CS 是 Code segment, DS 是 Data segment, SS 是 Stack segment, ES 是 Extra segment. 

IP是 *Instruction Pointer*, 这个和 CS 已经在 Lab1 里遇到过了, 毕竟是回头再来看汇编的. IP 与 CS 配合使用来跟踪下一个指令的地址. 一般情况下, CPU 每处理一个指令 IP 就会指向下一个指令地址.  

FLAGS 挺特殊的, 写 C 的时候也经常用到 flag, 道理都差不多, 就是存上一个指令的执行信息的(如果有这样的信息的话).

#### 1.2.5 80386 32 位寄存器  
刚才也提到过了, 80386 开始 16 位寄存器都被扩展成 32 位了. 但是为了保证对下的兼容性, AX还是原来的那个 16 位寄存器, EAX 表示新扩展出来的 32 位寄存器. 于是 AX 就变成了 EAX 的低 16 位, AL 就变成了 EAX 的低 8 位. **注意!** 没有可以**直接访问** EAX 的高 16 位的方法.  

#### 1.2.6 实模式
实模式中的内存被限制到 1MiB($2^{20}$ 字节), 合法的地址范围是十六进制 00000~FFFFF. 显然这个 20 位数无法放到 8086 的 16 位寄存器中. 因特尔解决了这个问题, 他使用了两个 16 位数推导一个地址, 第一个 16 位数被叫做*selector*, 第二个被叫做*offset*. 物理地址需要一个 32 位*selector*引用, 于是这里有一个计算公式:   
$$16*selector+offset$$    
乘以 16 非常简单, 只需要在十六进制最右边加一个 0 就行了. 比如推导物理地址引用 047C:0048,   
$$047C0+0048=04808$$  
实际上, selector值就是一个节数(16 字节).   

但是实模式这种切割地址的方式有一些缺点:  
1. 单个 selector 只能引用 64K 的内存, 如果程序代码超过 64K 怎么办?  
2. 每一个字节内存的索引方式不唯一. 比如刚才的 04808 可以被 047C:0048, 047D:0038,
047E:0028 或者是 047B:0058 引用. 这样会对比较地址造成一定的影响.  

#### 1.2.7 16 位保护模式
在 80286 的 16 位保护模式中, selector 值翻译成物理地址的方式与实模式完全不同. 在保护模式中, selector 值来自*描述符表 (descriptor table)* 中的一个*索引 (index)* 值. 在两个模式中, 程序都是被分块 (segements) 的. 在实模式中, 程序块的地址都是固定的, 并用 selector 值表示一个块开始位置的节的值; 但是在保护模式中, 这些块的地址不是固定的, 甚至都不用全部放入内存.  

保护模式使用了一种技术叫做**虚拟内存 (virtual memory)**, 虚拟内存最基本的用途是让正在使用的数据和代码在内存中, 其他的数据和代码临时存放在 disk 中, segments 能按需在内存和 disk 中切换. 这些切换操作对操作系统是完全透明的, 程序不需要针对虚拟内存这种设计专门编写.  

在保护模式中, 每一个代码块都会在描述附表中被声明成一个个条目 (entry). 这个条目包含了操作系统需要的信息, 比如这个块是否在内存中, 在内存的哪里, 访问权限. 这些描述符表中条目的索引, 存放在*段寄存器*中.   

16 位保护模式的最大缺点还是 offset 只有 16 位, 因此块大小还是被限制在了 64K, 大序列 (array) 的索引还是一个问题.  

#### 1.2.8 32 位保护模式
80386 引入了 32 位保护模式. 80286 的 16 位保护模式和 80386 的 32 位保护模式有两个主要区别:  
1. offsets 被扩展到了 32 位. 这就允许 offset 能够将最大范围扩大到四十亿字节. 从而, 块被扩展到了 4 gigabytes(4 GiB).  
2. 块能被分成更小的 4K 单位, 被称为**页面 (pages)**. 虚拟内存的工作对象从原来的块变成了 pages, 这意味只有块中的某个部分会被放到内存中工作. 毕竟最大已经可以扩展到 4GiB 了, 整块再放进去不合实际.  

> In Windows 3.x, standard mode referred to 286 16-bit protected mode
and enhanced mode referred to 32-bit mode. Windows 9X, Windows NT/2000/XP,
OS/2 and Linux all run in paged 32-bit protected mode.  

上面这段不翻译了.  

#### 1.2.9 中断

有时候程序必须要暂停去处理一些需要相应的重要事件. 计算机硬件提供了一种叫做**中断 (interrupts)** 的机制去处理 (handle) 这些事件. 比如鼠标移动了, 计算机需要暂停程序运行, 去移动屏幕上的光标. 这时终止请求会被的递交给*interrupt handler (中断控制器)*, 中断控制器会处理中断. 每种不同类型的中断用不同的整数来表示. 在物理内存的开始处, 有一个*中断向量表 (table of interrupt vectors)*, 其中包含了中断控制器的分块地址. 刚才说的用来表示中断类型的整数是表中的必不可少的索引值.  

*下面这段中出现的"发起", 都翻译自 raised from 这个词, 感觉有点像 raise exceptions 的那个意思.*  
CPU 外还可以发起*外部中断 (external interrupts)*, 其实鼠标属于这种外部发起的. 很多的 I/O 设备都会发起外部中断.  
相对的, 内部中断在 CPU 内部被发起, 无论是 CPU 内部错误还是直接发起的中断指令, 其中错误中断被叫做 *traps*. 产生中断的中断指令被叫做*软件中断 (software interrupts)*. 比如 DOS 提供了各种各样的中断用的 API, 现代的操作系统可以直接调用 C 提供的接口.  

很多中断控制器会在引起本次中断的程序运行完毕后返回. 它们会恢复中断前的所有寄存器值. 所以, 中断控制器发生前后看起来就好像跟什么都没发生过一样, 除了损失了一些时钟周期. *traps* 在通常情况下不会返回, 他们通常会中断程序运行. 

### 1.3 汇编语言
#### 1.3.1 机器语言
每一个指令都有它自己特定的**操作码 (*operation code*, or *opcode* for short)**, opcode 总是在一条指令的开头. 很多指令会包含数据 (data). 机器语言用二进制表示(或者是十六进制).   

#### 1.3.2 汇编语言
汇编语言用文本存储. 比如举个例子:  
```asm  
add eax, ebx
```  
汇编指令的通用格式为:
```asm
mnemonic operand(s)
```
mnemonic 查了一下是助记符号的意思, operand 操作数. 
这里提到了一个名词, *assembler(汇编器)*, 之前还遇到过一个叫 disassembler 的软件, 那个是反汇编器. 干吗用的, 就跟高级语言的编译器一样, 将汇编代码转换成机器码. 但是考虑到汇编指令转换机器码, 就只是映射关系, 所以比高级语言编译器要简单的多.  

#### 1.3.3 指令操作数
每一个指令自己有一个运算数定值(0到3), 并且分别对应下面四种运算数类型:  
> **register:** These operands refer directly to the contents of the CPU’s registers.  
> **memory:** These refer to data in memory. The address of the data may be a constant hardcoded into the instruction or may be computed using values of registers. Address are always offsets from the beginning of a segment.  
> **immediate:** These are fixed values that are listed in the instruction itself. They are stored in the instruction itself (in the code segment), not in the data segment.  
> **implied:** There operands are not explicitly shown. For example, the increment instruction adds one to a register or memory. The one is implied.  

寄存器, 内存, 立即数和蕴含数(?).  

#### 1.3.4 基础指令
最基础的就是 mov 了.   
```asm
mov dest, src
```
数据从 src 中拷贝到 dest . 
注意两点:  
1. dest 和 src都不能是内存操作数.  
2. 两个操作数必须是同一规格的.  

例子:  
```asm  
mov eax, 3 ; store 3 into EAX register (3 is immediate operand)
mov bx, ax ; store the value of AX into the BX register
```    

add 指令用来加整数:    
```asm  
add eax, 4 ; eax = eax + 4
add al, ah ; al = al + ah
```   

sub 指令用来减:  
```asm   
sub bx, 10 ; bx = bx - 10
sub ebx, edi ; ebx = ebx - edi
```   

inc 和 dec 用来使值增减1. 因为这里的增量 1 是隐含的操作数, 所以 inc 和 dec 的机器码要比执行类似操作的add 和 sub 要短.  
```asm
inc ecx  ; ecx++
dev dl   ; dl--
```  

#### 1.3.5 Directives(指示符)  
指示符是汇编器的而不是 CPU 的 artifact(something observed in a scientific investigation or experiment that is not naturally present but occurs as a result of the preparative or investigative procedure). 它们被用于指导汇编器去做什么事或者是通知汇编器什么. **它们不会被翻译成机器码.** 指示符的常用法:  
- 定义常量
- 定义存储数据的内存
- 组织内存为 segment
- 视情况包含源代码
- 包含其它文件

NASM 代码(之前没介绍, 这是本书用的汇编)使用像 C 一样的预处理器来实现指令. NASM 的预处理器指令以 % 开头(C 中用的是 #).  

##### 1.3.5.1 ```equ``` 指示符
用于定义一个符号 (symbol). 格式:  
```asm  
[symbol] equ [value]
```    
符号不能被二次定义.   

##### 1.3.5.2 ```%define``` 指示符
用法类似于 C 语言的宏:  
```asm
%define SIZE 100
move   eax, SIZE
```  

##### 1.3.5.3 数据指示符
这里说到了数据指示符有两种手段去保留内存, 一个是定义一个内存空间, 另一个是定义空间并初始化值.  
第一种可以使用 RES*X* 指令, X是一个字母, 用来表示对象的规格. 比如 B 表示字节, W 表示字, D 表示双字, Q 表示四字, T 表示十字节. 
第二种使用 D*X*, X 同上. 可以给这些内存空间标上一个 label (从后文来看这些标签不一定是L加上数字的组合), 举一些例子:  
```asm
L1 db   0       ; byte labeled L1 with initial value 0
L2 dw   1000    ; word labeled L2 with initial value 1000
L3 db   110101b ; byte initialized to binary 110101 (53 in decimal)
L4 db   12h     ; byte initialized to hex 12 (18 in decimal)
L5 db   17o     ; byte initialized to octal 17 (15 in decimal)
L6 dd   1A92h   ; double word initialized to hex 1A92
L7 resb 1       ; 1 uninitialized byte
L8 db   "A"     ; byte initialized to ASCII code for A (65)
```  
单引号和双引号的作用一样. 以上的这些数据被线性安排在内存里, 比如 L2 就在 L1 的后面.  
可以定义内存序列:  
```asm  
L9  db 0, 1, 2, 3        ; defines 4 bytes
L10 db "w", "o", "r", ’d’, 0 ; defines a C string = "word"
L11 db ’word’, 0         ; same as L10
```  
对于较大的内存序列, NASM 有一个TIMES 指示符:  
```asm   
L12 times 100 db 0 ; equivalent to 100 (db 0)’s
L13 resw 100       ; reserves room for 100 words
```  
要注意的是这些标签, 同 C 的指针类似, 它们是表示的是内存空间的地址, 取值需要有类似"解引用"的操作:  
```asm
mov al, [L1]   ; copy byte at L1 into AL
mov eax, L1    ; EAX = address of byte at L1
mov [L1], ah   ; copy AH into byte at L1
mov eax, [L6]  ; copy double word at L6 into EAX
add eax, [L6]  ; EAX = EAX + double word at L6
add [L6], eax  ; double word at L6 += EAX
mov al, [L6]   ; copy first byte of double word at L6 into AL
```  
最后一行, 我们看到的是 L6 只有第一个字节被拷贝进了 al 寄存器中, 这是 NASM 的一个重要属性--不会持续追踪 label 的数据规格. 不同于 C, C 是可以通过指针类型来判断指针指向内存空间的规格的. 

考虑下面这个指令:   
```asm
mov [L6], 1     ; store a 1 at L6
```  
上面说了不追踪 label 的规格, 所以这里编译器并不知道 L6 的规格, 于是需要手动指定, 加上一个*规格指定器 (size specifier)*:  
```asm
mov dword [L6], 1   ; store a 1 at L6
```  
这告诉汇编器以双字 1 存储到 L6 位置.   
其它的规格指定器还有 BYTE, WORD, QWORD 和 TWORD.  

#### 1.3.6 输入和输出
输入输出行为依赖于系统, 像 C 一样的高级语言有标准库提供输入输出, 而汇编没有, 于是必须直接访问硬件(这是在保护模式中的特权操作), 抑或是使用操作系统提供的较为低级的例行程序 (routine) 去完成输入输出的操作.  
汇编可以和 C 对接, 所以汇编里也可以直接使用 C 标准库. 但是因为这种交互比较麻烦, 所以作者 (the author, 说的是这书的作者?)开发了一套自己的 routine, 比 C 的用起来更简单. 下面简单介绍一下:  
- `print_int`: 打印 `EAX` 中存储整数.
- `print_char`: 打印 `AL` 中存储整数 ASCII 码对应的字符.
- `print_string`: 打印 `EAX` 中存储的地址所存储的字符串, 这个字符串必须是 C 风格的(比如 null 表示结尾).
- `print_nl`: 打印一个 '\n'.
- `read_int`: 从键盘输入读取一个整数存到 `EAX` 中.
- `read_char`: 从键盘输入读取单个字符, 将其ASCII码存到 `AL` 中.

接着提供了 include 这些 routines 的方法. 同样类似于 C 语言的预处理器:  
```asm
%include "asm_io.inc"
```

想使用这些 routines 可以使用 `CALL` 指令. 

#### 1.3.7 调试
然后作者说在 asm_io.inc 加入了一套调试用的程序:  
- `dump_regs`: 打印寄存器值到标准输出的宏. 接受一个参数. 这个参数用来区分不同的 dump_regs 指令. 
- `dump_mem`: 打印一块内存的值. 接受三个参数. 第一个参数同 dump_reg 的单参, 第二个是显示的地址(可以是标签), 第三个是用来指定想要显示第二个参数(地址)开始的 16 字节的节的数目.
- `dump_stack`: 顾名思义, 打印 CPU 栈中的值. 三个参数, 第一个同, 第二个参数指定将要打印栈指针 `EBP` 地址以降 (below) 的双字数目, 第三个参数指定同第二个参数中地址以升 (above) 的双字数目. 
- `dump_math`: 打印数字处理器 (math coprocessor) 的寄存器值. 单参同 dump_regs.

### 1.4 创建一个程序
#### 代码
```asm
; file: first.asm

%include "asm_io.inc"

; 初始化的数据放到.data段中
segment .data
int main()
{
    int ret_status;
    ret_status = asm_main();
    return ret_status;
}
outmsg2 db " and ", 0
outmsg3 db ", the sum of these is ", 0

; 未初始化的数据方到.bss段中
input1 resd 1
input2 resd 1

; 代码放到.text段中
segment .text
    global   asm_main
asm_main:
    enter    0,0          ; 开始执行
    pusha

    mov      eax, prompt1
    call     print_string

    call     read_int
    mov      [input1], eax

    mov      eax, prompt2
    call     print_string

    call     read_int
    mov      [input2], eax

    mov      eax, [input1]
    add      eax, [input2]
    mov      ebx, eax

    dump_regs 1
    dump_mem  2, outmsg1, 1

    mov      eax, outmsg1
    call     print_string
    mov      eax, [input1]
    call     print_int
    mov      eax, outmsg2
    call     print_string
    mov      eax, [input2]
    call     print_int
    mov      eax, outmsg3
    call     print_string
    mov      eax, ebx
    call     print_int
    call     print_nl
    
    popa
    mov      eax, 0
    leave
    ret
```  
```c
// file: driver.c
int main()
{
    int ret_status;
    ret_status = asm_main();
    return ret_status;
}
```   
```makefile
# Makefile
base    := asm_io.o driver.o
req     := $(base) asm_io.inc
cc      := gcc
ccflags := -m32
na      := nasm
naflags := -f elf
asmsobj := first.o

first: first.o $(req)
    $(cc) $(ccflags) first.o $(base) -o first

$(asmsobj): %.o: %.asm
    $(na) $(naflags) $<
asm_io.o: asm_io.asm asm_io.inc
    $(na) $(naflags) -d ELF_TYPE asm_io.asm

driver.o: driver.c
    $(cc) $(ccflags) -c $< -o $@

.PHONY:
clean:
    rm -f *.o
```  
```bash
# propmt of `make --just-print`
nasm -f elf first.asm
nasm -f elf -d ELF_TYPE asm_io.asm
gcc -m32 -c driver.c -o driver.o
gcc -m32 first.o asm_io.o driver.o -o first
```
```bash
# Dependencies
# dnf
sudo dnf install glibc-devel.i686
# apt 
sudo apt install gcc-multilib
```
```
Result:
Enter a number: 1
Enter another number: 2
Register Dump # 1
EAX = 00000003 EBX = 00000003 ECX = 6337CF5E EDX = FFDD4004
ESI = F7F80E24 EDI = 00000000 EBP = FFDD3FB8 ESP = FFDD3F98
EIP = 080491E7 FLAGS = 0206        PF   
Memory Dump # 2 Address = 0804C04C
0804C040 65 72 20 6E 75 6D 62 65 72 3A 20 00 59 6F 75 20 "er number: ?You "
0804C050 65 6E 74 65 72 65 64 20 00 20 61 6E 64 20 00 2C "entered ? and ?,"
You entered 1 and 2, the sum of these is 3
```

#### 解释
第一块是 first.asm 汇编代码, 也是这个程序的主要逻辑部分. 需要以来本书作者编写的 asm_io.inc 库, 原书的下载地址已经失效, 到这个地方下: [http://pacman128.github.io/pcasm](http://pacman128.github.io/pcasm)  
第二块是汇编的驱动程序 driver.c, 编译时需要用 32 位, 且需将单单其编译成目标文件(```-c```)  
第三块是一个 Makefile.  
第四块是依赖库, 不装 32 位程序好像跑不起来.  
第五块是运行结果.  

他这里稍微提了大端和小端表示法的问题, csapp 上有说, 不提了.  

好了, 因为找到了这书的中文版, 所以之后就没有英文标题了.  

## 第2章 - 基本汇编语言
### 2.1 整型工作方式
#### 2.1.1 整型表示法
这里主要是要介绍负整数在内存中的表示.   
##### 原码 (Signed magnitude)
原码是最简单的, 一个符号位加上一串原码, 比如用 $\underline{0}1111111$ 表示 +127, $\underline{1}1111111$ 表示 -127, 符号位用下划线表示了. 但是这样表示会带来一些麻烦 -- 0 的表示和运算的问题, 具体的不提了, csapp 上都有.  

##### 反码 (One’s complement)
一个数的反码可以通过将这个数的每一位求反得. 比如 $\underline{0}0111000 (+56)$, 反过来就是 $\underline{1}1000111(-56)$. 这样表示虽然可以解决一部分数运算的问题, 但是仍然会有 0 的问题. 

##### 补码 (Two’s complement)
怎么找到一个数的补码, 求反再加 1 就可以了. 比如 $\underline{0}0111000 (+56)$ 的补码是 $\underline{1}1001000(-56)$. 重点是我们算一下 0 的, 0 的补码还是它本身, 这样就解决了在原码和反码中出现的正负 0 的问题. 补码就是计算机内存中存储整数的方式, 这样存储的好处, csapp 上都有提, 在这里就不多赘述了.  

#### 2.1.2 正负号延伸
##### 减小数据的大小
减小数据的大小就是截去高位, 关键在于不是所有数都可以截. 为了能转换正确, 对于无符号数要求截去的数都为 0; 对于有符号数, 要求移除的位都是 1 或者 0. 否则, 这个数的数值就会和之前不同.   

##### 增大数据的大小
扩大数据大小, 对于无符号只需要在前面添加 0 就可以了, 对于有符号数, 需要在前面添加多个和符号位相同的位. 具体的证明过程 csapp 上有. 在汇编中, 使用 mov 指令就可以轻松把 AL 中的无符号字节扩展到 AX 中:  
```asm
mov    ah, 0
```
但是难办的是 AX 扩展到 EAX. 前文有提到 AX 的高 8 位是 AH, 低 8 位是 AL, 而 EAX 的高 16 位无法表示. 80386 给我们提供了如下方式扩展 AX 中的无符号字节:  
```asm
movzx  eax, ax
movzx  eax, al
movzx  ax, al
movzx  ebx, ax
```
对于有符号的, 8086 提供了几条指令来扩展: CBW(Convert Byte to Word) 将 AL 扩展到 AX, CWD(Convert Word to Double word) 可以将 AX 扩展成 DX:AX, 注意不是 EAX. 80386 又加入了 CWDE(Convert Word to Double word Extended), 可以直接将 AX 扩展到 EAX. CDQ(Convert Double word to Quad word)甚至可以将 EAX 扩展到 EDX:EAX (64 位). 最后, 还有个 MOVSX, 用法和 MOVZX 一样, 只是对于有符号作用.  

##### C语言中的应用
直接来看一个C代码:   
```c
unsigned char uchar = 0xFF;
signed char schar = 0xFF;
int a = (int) uchar;    /∗ a = 255 (0x000000FF) ∗/
int b = (int) schar;    /∗ b = −1 (0xFFFFFFFF) ∗/
```
不看书上是怎么说的了. 考虑到已经学习过 csapp, 所以直接看汇编好了:  
```c
int main(void) {
    volatile unsigned char uchar = 0xff;
    volatile signed   char schar = 0xff;
    volatile int a = (int) uchar;
    volatile int b = (int) schar;
    return 0;
}
```
```asm
	.file	"integer_extend_test.c"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	movb	$-1, -1(%rbp)
	movb	$-1, -2(%rbp)
	movzbl	-1(%rbp), %eax
	movzbl	%al, %eax
	movl	%eax, -8(%rbp)
	movzbl	-2(%rbp), %eax
	movsbl	%al, %eax
	movl	%eax, -12(%rbp)
	movl	$0, %eax
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (GNU) 9.2.1 20190827 (Red Hat 9.2.1-1)"
	.section	.note.GNU-stack,"",@progbits
```
可以很容易盯上这两句:  
```asm
movzbl	%al, %eax
movsbl	%al, %eax
```
gcc 生成的汇编代码和我们这里写的汇编 mov 方向是反的, 所以应该是 al 扩展成 eax, 但是注意他们的指令是不一样的, 一个是 movzbl, 一个 movsbl. 

然后书中还提到了一个标准库函数 `fgetc` 的 bug, 这个函数虽然是读取字符的, 但是它却返回一个 `int` 值. 在对于 EOF 和字符 0xFF 处理上, 这个函数显得束手无策. 书中推荐用 int 型来接收 `fgetc` 的返回值. 

#### 2.1.3 补码运算
FLAGS 寄存器为 add 和 sub 提供了两种状态, overflow 和 carry flag. 如果操作的正确结果太大了以致于不匹配有符号数运算的目的操作数(简单说就是目的操作数溢出了), 溢出标志位被置位. 如果在加法中的最高有效位有一个进位或在减法中的最高有效位有一个借位, 进位标志位将被置位.(2.1.5 节会说) 所以 FLAGS 可以用来检查无符号数运算的溢出情况. 补码运算的优势, 在于它把整数运算构成环状(见 csapp 和群论, 因为接触过环论不能确定这里说的"环"跟环论的环有没有关系). 根据 csapp, 无符号有符号的加减法指令各只需要一个.   
乘法和除法不同, 有提供给无符号的 MUL 和 DIV, 提供给有符号的 IMUL 和 IDIV. 
```asm
mul    source
```
mul 支持这种比较落后的乘法, 根据 source 的大小, 判断到底是把它跟 AL 乘放 AX 里, 还是把它跟 AX 乘放DX:AX 里, 还是把它跟 EAX 乘放 EDX:EAX 里. 而 imul 提供了更多的格式:  
```asm
imul   dest, src1       ; dest *= src1
imul   dest, src1, src2     ; dest  = src1 * src2
```
相对应的, div 和 idiv也一样. 但是要注意, 8/16/32 位除法对应的结果, 分别放在 AL/AX/EAX 中, 余数放在了 AH/DX/EDX 中. 

NEG 指令通过计算补码来获取单个操作数的相反数, 可以是 8 位, 16 位, 32 位寄存器或着内存区域.  

#### 2.1.4 程序例子
```asm
%include "asm_io.inc"

segment .data
prompt db "Enter a number: ", 0
square_msg db "Square of input is ", 0
cube_msg db "Cube of input is ", 0
cube25_msg db "Cube of input times 25 is ", 0
quot_msg db "Quotient of cube/100 is ", 0
rem_msg db "Remainer of cube/100 is ", 0
neg_msg db "The negation of the remainder is ", 0

segment .bss
input resd 1

segment .text
    global asm_main
asm_main:
    enter 0, 0
    pusha

    mov eax, prompt
    call print_string

    call read_int
    mov [input], eax
    
    imul eax           ; edx:eax = eax * eax
    mov ebx, eax
    mov eax, square_msg
    call print_string
    mov eax, ebx
    call print_int
    call print_nl

    mov ebx, eax
    imul ebx, [input]      ; ebx *= [input]
    mov eax, cube_msg
    call print_string
    mov eax, ebx
    call print_int
    call print_nl

    imul ecx, ebx, 25      ; ecx = ebx * 25
    mov eax, cube25_msg
    call print_string
    mov eax, ecx
    call print_int
    call print_nl

    mov eax, ebx
    cdq            ; use cdq to extend eax to edx:eax, initialize edx
    mov ecx, 100       ; CAN'T divided by immediate
    idiv ecx           ; edx:eax /= ecx
    mov ecx, eax
    mov eax, quot_msg
    call print_string
    mov eax, ecx
    call print_int
    call print_nl
    mov eax, rem_msg
    call print_string
    mov eax, edx
    call print_int
    call print_nl

    neg edx
    mov eax, neg_msg
    call print_string
    mov eax, edx
    call print_int
    call print_nl

    popa
    mov eax, 0
    leave
    ret
```
自己手敲了一遍, 东西挺简单的, 但是由于是汇编, 所以变得异常复杂.   
运行结果看下, 程序不解释:  
```
$ ./math
Enter a number: 4
Square of input is 16
Cube of input is 64
Cube of input times 25 is 1600
Quotient of cube/100 is 0
Remainer of cube/100 is 64
The negation of the remainder is -64
```

#### 2.1.5 扩充精度运算
```ADC: operand1 = operand1 + carry flag + operand2```  
```SBB: operand1 = operand1 - carry flag - operand2```  

用法很简单, 举例将 EDX:EAX 和 EBX:ECX 中存储的 64 位整数求和, 那么就是:  
```asm
add  eax, ecx
adc  edx, ebx
```
相减就是:  
```asm
sub  eax, ecx
sbb  edx, ebx
```

### 2.2 控制结构
#### 2.2.1 比较
比较结果存在 FLAGS 里.   
对于无符号, FLAGS 有两个标志位非常重要: 零标志位 (Zero flag, ZF) 和进位标志位 (carry flag, CF), 如果比较结果为 0, 零标志位将会被置 1.   
```asm
cmp  vleft, vright
```
这个比较会计算 vleft - vright 的值:   
- 如果 vleft = vright, 那么 ZF 就被置 1 了.  
- 如果 vleft > vright, 那么 ZF 就不被置位, CF 也不会.  
- 如果 vleft < vright, 那么 ZF 不会, 但是 CF 会被置位 (算作借位)

对于有符号, FLAGS 有三个标志位: ZF, 溢出标志位 (Overflow flag, OF) 和符号标志位 (Sign flag, SF). 如果一个操作结果上溢或者下溢 OF 就会被置位; 如果一个操作的结果为负数, 则 SF 会被置位:  
- 如果 vleft = vright, ZF 被置位
- 如果 vleft > vright, SF $=$ OF
- 如果 vleft < vright, SF $\neq$ OF

#### 2.2.2 分支指令
JMP 类似于 C 的goto 语句, 即无条件分支, 它的唯一参数是一个代码标号, 简单说就是指向某一个代码位置的指针名, 会被汇编器和连接器翻译成地址.  
下面这段话照搬了, 因为暂时还不知道怎么用:  
> **SHORT** 这个跳转类型局限在一小范围内。它仅仅可以在内存中向上或向
下移动 128 字节。这个类型的好处是相对于其它的,它使用较少的内
存。它使用一个有符号字节来储存跳转的位移。位移表示向前或向后
移动的字节数(位移须加上 EIP)。为了指定一个短跳转,需在 JMP 指令
里的变量之前使用关键字 SHORT。  
> **NEAR** 这个跳转类型是无条件和有条件分支的缺省类型,它可以用来跳
到一段中的任意地方。事实上, 80386 支持两种类型的近跳转。其中
一个的位移使用两个字节。它就允许你向上或向下移动 32,000 个字
节。另一种类型的位移使用四个字节,当然它就允许你移动到代码段
中的任意位置。四字节类型是 386 保护模式的缺省类型。两个字节类
型可以通过在 JMP 指令里的变量之前放置关键字 WORD 来指定。  
> **FAR** 这个跳转类型允许控制转移到另一个代码段。在 386 保护模式下,这
种事情是非常鲜见的。

有关代码标号的问题, 将会体现在之后的汇编代码里. 简单说它类似于 C 中 goto 的标号, 而且之前一直在出现的 asm_main 也就是一个标号.  

JMP 还有其他版本的有条件分支指令, 下面列举了:  


| 指令 | 功能 |
| -- | -- |
| JZ | JNP 如果 ZF 被置位了, 就分支 |
| JNZ | 如果 ZF 没有被置位, 就分支 |
| JO | 如果 OF 被置位了, 就分支 |
| JNO | 如果 OF 没有被置位, 就分支 |
| JS | 如果 SF 被置位了, 就分支 |
| JNS | 如果 SF 没有被置位, 就分支 |
| JC | 如果 CF 被置位了, 就分支 |
| JNC | 如果 CF 没有被置位, 就分支 |
| JP | 如果 PF 被置位了, 就分支 |
| JNP | 如果 PF 没有被置位, 就分支 |



用法都跟 JMP 一样. 其中 PF 是奇偶被追陈述 (Parity flag), 用来表示结果中的低 8 位 1 的位数值为奇数个或偶数个, 暂时不知道有什么用.  

以上的这些指令用于判断两个量的相等或者大小还是很容易的, 但是仿佛不容易表示大于等于和小于等于, 具体不举例子了. 80x86 提供了额外的分支指令:  
有符号:


| 指令 | 功能 |
| -- | -- |
| JE | 如果 vleft = vright, 则分支 |
| JNE | 如果 vleft != vright, 则分支 |
| JL, JNGE | 如果 vleft < vright, 则分支 |
| JLE, JNG | 如果 vleft ≤ vright, 则分支 |
| JG, JNLE | 如果 vleft > vright, 则分支 |
| JGE, JNL | 如果 vleft ≥ vright, 则分支 |


无符号:


| 指令 | 功能 |
| -- | -- |
| JE | 如果 vleft = vright, 则分支 |
| JNE | 如果 vleft != vright, 则分支 |
| JB, JNAE | 如果 vleft < vright, 则分支 |
| JBE, JNA | 如果 vleft ≤ vright, 则分支 |
| JA, JNBE | 如果 vleft > vright, 则分支 |
| JAE, JNB | 如果 vleft ≥ vright, 则分支 |



*这个表格真心不好整理...*
这些指令有点顾名思义了, N 是 not, G 是 greater, L 是 less, E 是 equal, A 和 B我猜应该是 after 和 before.  

嫖一段代码:  
```c
if (EAX >= 5)
    EBX = 1;
else
    EBX = 2;
```  
```asm
    cmp eax, 5
    jge thenblock
    mov ebx, 2
    jmp next
thenblock:
    mov ebx, 1
next:
```  

#### 2.2.3 循环指令
**LOOP** ECX 自减,如果 ECX $\neq$ 0, 分支到代码标号指向的地址  
**LOOPE, LOOPZ** ECX 自减(FLAGS寄存器没有被修改), 如果 ECX $\neq$ 0 而且 ZF = 1, 则分支  
**LOOPNE, LOOPNZ** ECX 自减(FLAGS没有改变), 如果 ECX $\neq$ 0 而且 ZF = 0, 则分支  

再嫖一段:  
```c
int sum = 0;
for (int i = 10; i > 0; i--)
    sum += i;
```  
```asm
    mov eax, 0      ; sum
    mov ecx, 10     ; i
loop_start:
    add eax, ecx
    loop loop_start
```  

### 2.3 翻译标准的控制结构
#### 2.3.1 if 语句
```c
if (condition)
    then_block;
else
    else_block:
```  
```asm
    ; set FLAGS
    jxx else_block  ; select xx: if condition == false, exec else_block
    ; then_block
    jmp endif
else_block:
    ; else_block
endif:
```  

#### 2.3.2 while 循环
```c
while (condition)
    loop_block;
```  
```asm  
while:
    ; set FLAGS
    jxx endwhile  ; select xx: if condition == false, exec else_block
    ; loop_block
    jmp while
endwhile:
```

#### 2.3.3 do...while 循环
```c
do {
    loop_block;
} while (condition);
```
```asm
do:
    ; loop_block
    ; set FLAGS
    jxx do        ; select xx: if condition == false, exec else_block
```

### 2.4 例子: 查找素数
下面先给出一个 C 语言的版本:  
```c
#include <stdio.h>

int main(void)
{
    unsigned guess;    // 当前对素数的猜测
    unsigned factor;   // 猜测数的可能的因子
    unsigned limit;    // 查找这个值以下的素数

    printf("Find primes up to: ");
    scanf("%u", &limit);
    printf("2\n");     // 把头两个素数作为特殊情况处理
    printf("3\n");
    guess = 5;         // 初始化猜测数
    while (guess <= limit) {
        factor = 3;
        while (factor * factor > guess &&
               guess % factor != 0)
            factor += 2;
        if (guess % factor != 0)
            printf("%d\n", guess);
        guess += 2;    // 只考虑奇数
    }
}
```
然后下面是汇编的版本:
```asm
%include "asm_io.inc"

segment .data
Message db  "Find primes up to: ", 0

segment .bss
Limit resd 1
Guess resd 1

segment .text
    global asm_main
asm_main:
    enter 0, 0
    pusha

    mov eax, Message
    call print_string
    call read_int                ; scanf("%u", &limit);
    mov [Limit], eax

    mov eax, 2                   ; printf("2\n");
    call print_int
    call print_nl
    mov eax, 3                   ; printf("3\n");
    call print_int
    call print_nl

    mov dword [Guess], 5         ; Guess = 5;
while_limit:                     ; while (Guess <= Limit)
    mov eax, [Guess]
    cmp eax, [Limit]
    jnbe end_while_limit         ; use jnbe since numbers are unsigned. 我寻思用ja应该也可以

    mov ebx, 3                   ; `ebx means` factor = 3;
while_factor:
    mov eax, ebx
    mul eax                      ; edx:eax = eax * eax
    jo end_while_factor          ; if answer won’t fit in eax alone
    cmp eax, [Guess]
    jnb end_while_factor         ; if !(factor * factor < guess)
    mov eax, [Guess]
    mov edx, 0
    div ebx                      ; edx = edx:eax % ebx
    cmp edx, 0
    je end_while_factor          ; if !(guess % factor != 0)

    add ebx, 2                   ; factor += 2;
    jmp while_factor
end_while_factor:
    je end_if                    ; if !(guess % factor != 0)
    mov eax, [Guess]
    call print_int
    call print_nl
end_if:
    add dword [Guess], 2         ; guess += 2;
    jmp while_limit
end_while_limit:

    popa 
    mov eax, 0
    leave
    ret
```


## 第3章 - 位操作
### 3.1 移位运算
#### 3.1.1 逻辑移位
逻辑移位很简单, 多了就去掉, 缺了就加 0.   
SHL 和 SHR 指令分别表示逻辑左移和逻辑右移. 接收两个操作数, 第一个是要移位的对象, 第二个要不是常数, 要不可以是 CL 寄存器的值. 

#### 3.1.2 移位的应用
快速乘除就不说了.  
逻辑移位只能用在无符号数上.  

#### 3.1.3 算术移位
算术左移 SAL(Shift Arithmetic Left) 会被翻译成与 SHL 一样的机器码, 即这两个指令的作用是相同的.  
算术右移 SAR(Shift Arithmetic Right) 大体上与 SHR 相同, 但是它不会无脑在左边空位上补 0, 而是会复制符号位.   
具体的不在这里过多解释了, csapp 上都看过了.  

#### 3.1.4 循环移位
已经够顾名思义的了, 向左向右分别是 ROL 和 ROR. 还有个带上 CF 的版本, RCL 和 RCR, 比如想要移位 AX, 那么总共参与运算的位就是 17 位 -- AX 和 CF, 最终结果存放在 AX 里. 

#### 3.1.5 简单应用
这个代码还蛮有意思的, 摘抄一下:  
```asm
    mov bl, 0          ; bl: the number of `on` in eax
    mov ecx, 32        ; loop counter
count_loop:
    rol eax, 1
    jnc skip_inc       ; if CF == 0, then goto skip_inc
    inc bl
skip_inc:
    loop count_loop
```
这个代码的用处是统计 32 位整数 eax 的为 `1` 的位的个数. 这里的 rol 使用的恰到好处, 循环 32 次后, eax 会被还原成原来的样子.  

### 3.2 布尔按位运算
#### 3.2.1 AND运算符
略

#### 3.2.2 OR运算符
略略

#### 3.2.3 XOR运算符
略略略

#### 3.2.4 NOT运算符
略略略略, 等等, NOT 运算符可以直接或者一个数的反码(这里翻译版写的是补码, 个人感觉有点奇怪, 就去看了一下原版, 是 one’s complement 反码. 翻译出错). 然后想着写了个代码验证以下:  
```c
#include <stdio.h>

int main(void)
{
    int a = 10;
    asm("notl    -4(%rbp)");
    printf("NOT a = %d\n", a);
    return 0;
}
```
```
$ ./test_asm    
NOT a = -11
```

#### 3.2.5 TEST指令
TEST 指令执行一次 AND 运算, 然后基于可能的结果对 FLAGS 进行设置. 比如结果为 0, ZF 会被置位.  

#### 3.2.6 位操作的应用
直接使用书上的例子:  
```asm
mov ax, 0C123H
or  ax, 8             ; 开启位 3,  ax = C12BH
and ax, 0FFDFH        ; 关闭位 5,  ax = C10BH
xor ax, 8000H         ; 求反位 31, ax = 410BH
```
上面的操作, 不过就是下面三种行为的一种:  
- 开启位 $i$, 与 $2^i$ 进行 OR 运算
- 关闭位 $i$, 与只有位 $i$ 为 off 的二进制数进行 AND 运算, 这个操作数通常被称为掩码 (mask)
- 求反位 $i$, 与 $2^i$ 进行 XOR 运算

AND 可以用来计算除以 2 的几次方后的余数, 将它与 $2^i-1$ 的掩码做 AND 运算, 就可以得到除以 $2^i$ 的余数. 这个原理很简单的, 就不演示了.   

下面这个比较有意思, 它可以开启或者关闭任意的比特位:   
开启 eax 的位 cl:  
```asm
mov cl, bh
mov ebx, 1
shl ebx, cl
or eax, ebx
```
关闭 eax 的位 cl:
```asm
mov cl, bh
mov ebx, 1
shl ebx, cl
not ebx
and eax, ebx
```
还有个求反任何一个比特位的书象征性地留作了作业, 其实也很简单:  
求反 eax 的位 cl:
```asm
mov cl, bh
mov ebx, 1
shl ebx, cl
xor eax, ebx
```

然后这里有个非常重要的东西, 就是 80x86 程序中会经常出现:  
```asm
xor eax, eax      ; eax = 0
```
这样用 XOR 运算的方式赋 eax 为 0, 要比同样功能的 `mov eax, 0` 的机器代码的指令要少(机器码的长度).   

### 3.3 避免使用条件分支
现代处理器的*预测执行* (利用 CPU 的并行能力同时执行多条指令)可能会与条件分支发生冲突, 浪费了处理器的时间, 所以需要避免使用条件分支.  
比如之前在 3.1.5 中的代码, 使用了一个 skip_inc 条件分支来跳过 inc bl 的过程, 但是这是完全不需要的, 可以通过下面的方式改写:  
```asm
    mov bl, 0
    mov ecx, 32
count_loop:
    shl eax, 1
    adc bl, 0
    loop count_loop
```

接着是一个求两数最大值的程序, 正常情况下应该使用一个 cmp, 然后把较大的那个数输出, 但是下面的例子避免了使用条件分支:  
```asm
; file: max.asm
%include "asm_io.inc"

segment .data
message1 db "Enter a number: ", 0
message2 db "Enter another number: ", 0
message3 db "The larger number is: ", 0

segment .bss
input1 resd 1       ; store the first number

segment .text
    global asm_main
asm_main:
    enter 0, 0
    pusha

    mov eax, message1
    call print_string
    call read_int          ; input the first number
    mov [input1], eax

    mov eax, message2
    call print_string
    call read_int          ; input the second number (in eax)

    xor ebx, ebx           ; ebx = 0
    cmp eax, [input1]      ; compare the first with the second
    setg bl                ; ebx = (input2 > input1) ? 1: 0
    neg ebx                ; ebx = (input2 > input1) ? 0xFFFFFFFF : 0
    mov ecx, ebx           ; ecx = (input2 > input1) ? 0xFFFFFFFF : 0
    and ecx, eax           ; ecx = (input2 > input1) ? input2 : 0
    not ebx                ; ebx = (input2 > input1) ? 0 : 0xFFFFFFFF
    and ebx, [input1]      ; ebx = (input2 > input1) ? 0 : input1
    or ecx, ebx            ; ecx = (input2 > input1) ? input2 : input1

    mov eax, message3
    call print_string
    mov eax, ecx
    call print_int
    call print_nl

    popa
    mov eax, 0
    leave
    ret
```
两点:  
1. setg 的用法注释已经写得很清楚了
2. neg 可以用 dec 代替, 但是值会反过来

### 3.4 在 C 中进行位操作
#### 3.4.1 C 中的按位运算
先吹一波 C 语言 nb, 很多高级语言都不支持这样的功能.  
对 C 语言位操作简单说明:  
- AND 运算符使用二元运算符 & 来描述
- OR 运算符使用二元运算符 | 来描述
- XOR 运算符使用二元运算符 ^ 来描述
- NOT 运算符使用二元运算符 ~ 来描述
- SHL/SAL 运算符使用二元运算符 << 来描述
- SHR/SAR 运算符使用二元运算符 >> 来描述
例子就不举了.  

#### 3.4.2 在C中使用按位运算
第一段给提示说, 一个好的C编译器会自动把 x *= 2 这种使用移位来运算.  
举例 POSIX 的 API 位三种不同类型的用户保留了文件权限: user, group 和 others, 然后每一个类型的用户都可以被授予读, 写和/或执行. 其实也就是我们在 Linux 中比较常见的 chmod 指令, POSIX 也有一个系统函数叫 chmod. 然后来看下代码:  
```c
chmod("foo", S_IRUSR | S_IWUSR | S_IRGRP);
```
然后它还提供了一个结构体类型 stat, 用来存储某一个文件的权限位. 同名的 stat 函数可以将一个文件的权限放入到一个 struct stat 变量中. 
> 下面是一个移除文件的others用户的写权限和增加owner用户的读权限的例子: 
> ```c
> struct stat file_stats;  /* stat()使用的结构体 */
> stat("foo", &file_stats); /* 读文件信息file stats.st mode中有权限位 */
> chmod("foo", (file_stats.st_mode & ~S_IWOTH) | S_IRUSR);
> ```

### 3.5 Big 和 Little Endian 表示法
之前有提到过大端小端的, csapp 上有, 我也不太想提. 但是既然原文说了, 这里就稍微提一点:  
对于双字 $12345678_{16}$, 如果是 big endian 表示法, 就会是 12 34 56 78; 如果是 little endian 表示法, 就会是 78 56 34 12. 原文提到一个正常人是无法接受 little endian 表示法的, 或者 Intel 公司的工程师会不会是抖 S? 但是广大的程序员群体接受了. 不管是 big 还是 little, 区别只在于 human being, 而不在于 CPU 的电路, 电路是无序的, 对于 CPU 而言它才不管你到底是哪个在先哪个在后. 同样的, 对于比特位, 在 CPU 中的排列顺序也是无从得知的, 因为 CPU 或内存中没有对单个比特位进行编址, 所以我们无从得知它们的排列. 所以还要纠结那么多干嘛呢?  

下面摘抄的C语言代码, 其实非常简单, 可以看出当前的处理器到底是 big 还是 little endian 表示. 啊, 算了, 原书的代码有点恶心, 我用我喜欢的方式重写了一下:    
```cpp
#include <cstdio>
#include <cstddef>

template<typename T, std::size_t SIZE>
union Convert {
    T src;
    char dest[SIZE];
};

int main()
{
    unsigned int a = 0x12345678;
    Convert<decltype(a), sizeof(a)> convert;
    convert.src = a;
    for (auto c: convert.dest) {
        printf("%x ", c);
    }
    putchar(012);
    return 0;
}
```
```
$ ./endian_test 
78 56 34 12
```
因为我的电脑是因特尔处理器, 所以理所当然, 用的是 little endian 表示法. 

#### 3.5.1 什么时候需在乎这种东西?
算是一个小插曲吧, 多的不说了, 就是两台计算机在交换信息的时候. TCP/IP消息头都会以 big endian 格式来存储整型. 然后巴拉巴拉讲了一堆网络的东西. 最后说了486处理器提供了BSWAP指令来交换32位寄存器中的字节, 也就是大小端转换, 然后XCHG指令可以, 有点抽象, 举个例子:  
```asm
xchg ah, al       ; 交换ax中的字节
```

### 3.6 计算位数