1. 从CPU加电后执行的第一条指令开始，单步追踪BIOS的执行。
    藉由：
        set architecture i8086          // 将结构设置为i8086
        target remote :1234             // 使用1234端口在gdb和qemu间建立联系
    和make debug命令即可看到从0x0000fff0开始的指令。如果想看到具体的汇编指令，可以使用
        x /2i $pc                       // x查看内存单元
                                        // 2表示显示的内存单元个数，i表示指令地址格式
                                        // $pc表示显示pc现在所处的位置
    然后在此时，用si可以一步一步地让gdb执行下一条指令，每一条都可以反汇编。
2. 在初始化位置0x7c00设置实地址断点,测试断点正常。
    设置断点：
    b *0x7c00
    c
    x /10i $pc
    效果：
    0x7c00      cli
    0x7c01      cld
    0x7c02      xor...
3. 从0x7c00开始跟踪代码运行,将单步跟踪反汇编得到的代码与bootasm.S和 bootblock.asm进行比较。
    基本一模一样。xor替换为xorw，mov替换为movw，in替换为inb，test替换为testb，mov替换为movb。
4. 随便找个地方放了嘿。
    0xff00大成功的说。直接进行一个屏的刷。
// make lab1-mon 运行结果
+ cc kern/init/init.c
+ cc kern/libs/stdio.c
+ cc kern/libs/readline.c
+ cc kern/debug/panic.c
kern/debug/panic.c: In function ‘__panic’:
kern/debug/panic.c:27:5: warning: implicit declaration of function ‘print_stackframe’; did you mean ‘print_trapframe’?  -Wimplicit-function-declaration]
   27 |     print_stackframe();
      |     ^~~~~~~~~~~~~~~~
      |     print_trapframe
+ cc kern/debug/kdebug.c
+ cc kern/debug/kmonitor.c
+ cc kern/driver/clock.c
+ cc kern/driver/console.c
+ cc kern/driver/picirq.c
+ cc kern/driver/intr.c
+ cc kern/trap/trap.c
kern/trap/trap.c: In function ‘print_trapframe’:
kern/trap/trap.c:109:16: warning: taking address of packed member of ‘struct trapframe’ may result in an unaligned pointer value [-Waddress-of-packed-member]
  109 |     print_regs(&tf->tf_regs);
      |                ^~~~~~~~~~~~
+ cc kern/trap/vectors.S
+ cc kern/trap/trapentry.S
+ cc kern/mm/pmm.c
+ cc libs/string.c
+ cc libs/printfmt.c
+ ld bin/kernel
+ cc boot/bootasm.S
+ cc boot/bootmain.c
+ cc tools/sign.c
+ ld bin/bootblock
'obj/bootblock.out' size: 500 bytes
build 512 bytes boot sector: 'bin/bootblock' success!
10000+0 records in
10000+0 records out
5120000 bytes (5.1 MB, 4.9 MiB) copied, 0.0750435 s, 68.2 MB/s
1+0 records in
1+0 records out
512 bytes copied, 0.0019912 s, 257 kB/s
154+1 records in
154+1 records out
78960 bytes (79 kB, 77 KiB) copied, 0.0027499 s, 28.7 MB/s
/bin/sh: 1: gnome-terminal: not found
make: *** [Makefile:202: lab1-mon] Error 127
# ERROR. Install gnome-terminal: Failed.

# 装了Xming后成功了。
# 按照report.md中做法将语句加入gdbinit，在c/continue一行报错，没有解决，使用make lab1-mon规避了这个问题。

inphantasm@DESKTOP-79F93QP:~/ucore_lab_os/labcodes/lab1$ qemu -d in_asm -D q.log
Unable to init server: Could not connect: Connection refused
gtk initialization failed
# 没开Xming，我是脑瘫

"便**可以**在q.log中读到"call bootmain"前执行的命令"
# 没读到。

https://blog.csdn.net/moonsheep_liu/article/details/39099969
