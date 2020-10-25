bootloader如何读取硬盘扇区的？

bootmain.c中的函数void readsect(void *dst, uint32_t secno)用来读取硬盘的第secno个扇区到dst位置

| IO地址 | 功能                                                         |
| ------ | ------------------------------------------------------------ |
| 0x1f0  | 读数据，当0x1f7不为忙状态时，可以读。                        |
| 0x1f2  | 要读写的扇区数，每次读写前，你需要标明你要读写几个扇区（一个以上）。 |
| 0x1f3  | LBA参数的0-7位/不知道是什么                                  |
| 0x1f4  | LBA参数的8-15位/不知道是什么                                 |
| 0x1f5  | LBA参数的16-23位/不知道是什么                                |
| 0x1f6  | 0-3位表示LBA参数的24-27位，第四位表示主盘（0）从盘（1）      |
| 0x1f7  | 状态/命令寄存器。先给命令再读取，如果不忙就从0x1f0读数据。   |

先设置读取扇区数目是1(0x1F2)，然后0x1F3-0x1F6联合制定扇区号。最后用0x1F7进行读取扇区的命令。

bootloader如何加载ELF文件到OS中的？

一个ELF文件被分为好几个段，有程序段，数据段等等。通过ELF文件头的phoff可以找到第一个段的地址，phnum提供了段的数量，所以需要通过一个遍历来找到所有段的信息。（ELF文件头有哪些参数可以看elf.h文件）

所以看bootmain函数，首先读取ELF文件头（这里通过ELF存的一个幻数来判断是不是一个合法的文件）
readseg((uintptr_t)ELFHDR, SECTSIZE * 8, 0);
然后读取到头地址和数量，进行一个循环遍历读取到所有内容。
程序头表（Program Header）的每个成员分别记录一个段的信息，包括offset，va。其中，每个段要放在什么位置由每个段的va字段来决定。
ph = (struct proghdr *)((uintptr_t)ELFHDR + ELFHDR->e_phoff);
eph = ph + ELFHDR->e_phnum;
for (; ph < eph; ph ++) {
	readseg(ph->p_va & 0xFFFFFF, ph->p_memsz, ph->p_offset);
}
最后根据ELF文件头储存的入口进入。
((void (*)(void))(ELFHDR->e_entry & 0xFFFFFF))();