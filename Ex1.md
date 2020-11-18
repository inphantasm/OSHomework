写在最前：打补丁用
$ diff -uNr lab1 lab2 >lab12.patch
$ patch -p0 lab1 <lab12.patch
他给我跳出来File lab1 is not a regular file，并拒绝我的patch请求，那么文件夹patch要用什么呢？我并不了解。
所以我手动合并的。

为什么lab2和lab2_result中这个玩意的代码不一样，但是效果感觉差不多？这是我不能理解的
附注：
````C++
// le = list_next(&free_list);
// while (le != &free_list) {
//     p = le2page(le, page_link);
//     if (base + base->property <= p) {
//         assert(base + base->property != p);
//         break;
//     }
//     le = list_next(le);
// }
// ?
````
这段代码完全没用。甚至问了助教，助教也觉得没用，真好。
占用块，lab2是先删再放，result是先放再删，个人认为是因为先删完了可能突然断电然后就数据丢失了，助教也支持，开心
pmm.c那#if 0属实给我整傻了，我还想为啥要if0，结果看了半天没看见里面按步骤的操作，老瞎子了呜呜呜
还有那堆pte_t啥的开始看得我好懵，结果一个一个看引用发现就是一uint，合着还是面向对象了，要不然看都看不懂，感恩作者（