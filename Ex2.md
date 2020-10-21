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