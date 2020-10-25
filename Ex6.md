中断向量表一个表项占用8个字节，其中2-3字节是段选择子，0-1字节和6-7字节拼成位移。

https://img-blog.csdnimg.cn/20190425163612355.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ2OTcxNg==,size_16,color_FFFFFF,t_70


在这个表项中，由8到12位用于表示门类型，门类型有三种，包括中断门，陷阱门，任务门。通过对XXX的不同取值，达到访问不同门的效果
从上图可以看出，其中的第16位到第31位为中断历程的段选择子，第0到15位以及第48到63位分别代表偏移量的低位和高位。这三个数据通过组合决定了中断处理代码的入口地址。

# 来源（https://blog.csdn.net/weixin_42469716/article/details/89519905）

依次对所有中断入口进行初始化。
可以看到在vector.S中预置了256个中断。
所以第一步就是需要一个uintptr_t __vectors[]来维护中断处理函数的入口。
第二步就是用SETGATE宏来设置中断服务程序（ISR）的入口。
第三步，用lidt命令让CPU知道IDT被加载到哪里了。

extern uintptr_t __vectors[]; //开一个vector，用setgate宏把idt放进去
for (int i = 0; i < sizeof(idt) / sizeof(struct gatedesc); i++)
{
    // SETGATE的参数是gate, istrap, sel, off, dpl
    SETGATE(idt[i], 0, GD_KTEXT, __vectors, DPL_KERNEL);
}
    SETGATE(idt[T_SWITCH_TOK], 0, GD_KTEXT, __vectors[T_SWITCH_TOK], DPL_USER);
    lidt(&idt_pd);


处理时钟中断，使得每过100次时钟中断后，打印一次"100 ticks"

case IRQ_OFFSET + IRQ_TIMER:// 硬件的 偏移量+计时器
    ticks++;// clock.c中的全局变量，用来记录进行了多少次时钟中断
    if(ticks % TICK_NUM == 0)// 每一百个cycle打印一次
    {
        print_ticks();
    }
    break;

其中print_ticks()是
static void print_ticks() {
    cprintf("%d ticks\n",TICK_NUM);
}