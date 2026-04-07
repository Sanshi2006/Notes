1.`--tool=<name>`

默认是`memcheck`，还有`cachegrind`（缓存分析）、`helgrind`（多线程检测）、`massif`（堆栈分析）

2.`--leak-check=full`，告诉你泄露了多少内存，在哪一行申请的内存没有归还

3.`--show-leak-kinds=all`，展示所有类型的泄露

4.`--track-origins=yes`，追踪未初始化值在哪里定义

5.`--log-file=<filename>`，将错误日志输出到指定文件

6.`--xml=yes` / `--xml-file=<filename>`，输出为`xml`格式

7.`--error-exitcode=<number>`，如果发现了任何内存错误，让进程以指定的状态码退出



