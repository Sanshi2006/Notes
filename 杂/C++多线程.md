# 一. 进程与线程

* 进程是正在运行的程序的实例，线程是进程的实际运作单位

区别：

* 一个程序有且只有一个进程，但可以拥有至少一个的线程。
* 不同进程拥有不同的地址空间，互不相关，而不同线程共同拥有相同进程的地址空间。

![image-20260412153335263](https://raw.githubusercontent.com/Sanshi2006/ImagesResources/main/img/20260412153635306.png)



# 二. `std::thread`

用于执行多线程，既让一个程序同时做多件事

例：

```cpp
#include <thread>
#include <iostream>
#include <chrono>   // 用于 sleep

void my_task() {                    // 这就是新线程要执行的函数
    std::cout << "我是新线程，正在工作...\n";
    std::this_thread::sleep_for(std::chrono::seconds(2));  // 模拟干2秒活
    std::cout << "新线程工作完成！\n";
}

int main() {
    std::cout << "主线程：我要雇一个工人\n";
    
    std::thread t(my_task);         // ← 这一行就创建并立刻启动了一个新线程
    
    std::cout << "主线程：工人已经上岗了，我继续干我的事\n";
    
    // ... 这里主线程可以继续执行其他代码 ...
    
    return 0;
}
```

运行这个程序时，会发生：

* 主线程打印 “我要雇一个工人“
* 立刻创建一个新线程，开始执行 `my_task()`
* 主线程继续往下走，打印 “工人已经上岗了...”，同时新线程也在进行
* `main()` 函数结束 → 整个程序直接退出

结果会发现，新线程中有可能只打印了第一句话

这是因为，主线程结束，整个进程就结束了，新线程被强制终止了

那么怎么保证新线程一定能完整运行完呢，这时候就引入了`join()`



## 2.1 `.join()`

作用是让主线程停下来，等待子线程执行完毕后，再继续执行

例：

```cpp
#include<bits/stdc++.h>

void my_task() {                  
    std::cout << "我是新线程，正在工作...\n";
    std::this_thread::sleep_for(std::chrono::seconds(2));  // 模拟干2秒活
    std::cout << "新线程工作完成！\n";
}
int main() {
    std::cout << "主线程：我要雇工人\n";
    
    std::thread t(my_task);
    
    std::cout << "主线程：我现在要等工人干完\n";
    
    t.join();
    
    std::cout << "主线程：工人已经彻底干完，我现在安全退出\n";
    
    return 0;
}
```

此时发现会输出：

![image-20260412161702273](https://raw.githubusercontent.com/Sanshi2006/ImagesResources/main/img/20260412161702377.png)

新线程完整地运行完成



## 2.2 不使用`.join()`的后果

C++ 标准规定了非常严格的行为：

- 如果一个 `std::thread` 对象**被销毁**（比如离开作用域），而它既没有 `join`，也没有 `detach`，程序会直接调用` std::terminate()`，导致整个程序崩溃（`abort`）。
- 即使不崩溃，子线程也可能在运行到一半时被强行杀死，导致资源没有释放、文件没有关闭、内存泄漏等问题。



```cpp
#include <thread>
#include <iostream>
#include <chrono>   // 用于 sleep

void my_task() {                    // 这就是新线程要执行的函数
    std::cout << "我是新线程，正在工作...\n";
    std::this_thread::sleep_for(std::chrono::seconds(2));  // 模拟干2秒活
    std::cout << "新线程工作完成！\n";
}

int main() {
    std::cout << "主线程：我要雇一个工人\n";
    
    std::thread t(my_task);         // ← 这一行就创建并立刻启动了一个新线程
    
    std::cout << "主线程：工人已经上岗了，我继续干我的事\n";
    
    // ... 这里主线程可以继续执行其他代码 ...
    
    return 0;
}
```

比如刚开始这个版本下会发生:

![image-20260412162432624](https://raw.githubusercontent.com/Sanshi2006/ImagesResources/main/img/20260412162432734.png)



此外还应该注意线程对象作用域和生命周期问题：

```cpp
#include<bits/stdc++.h>

void my_task() {                  
    std::cout << "我是新线程，正在工作...\n";
    std::this_thread::sleep_for(std::chrono::seconds(2));  // 模拟干2秒活
    std::cout << "新线程工作完成！\n";
}
void start_thread() {
    std::thread t(my_task);
    // 函数结束，t 被销毁 → 崩溃！
}

int main() {
    start_thread();
    return 0;
}
```

比如这样，在`start_thread()`函数中新线程被创建，但是有可能新线程还未执行完毕，但是`start_thread()`已经执行完毕，对象t被销毁，此时的t处于既没有 `join`，也没有 `detach`的状态，`std::thread` 的析构函数就会直接调用 `std::terminate()`，导致整个程序异常终止（崩溃）。



## 2.3 `.detach()`

`.detach()`的作用是：告诉系统，我不再关心这个线程了，你让它自己跑吧，主线程结束时也别杀它。

特点：

①主线程结束后，子线程还能继续运行（直到它自己结束）。

②你无法再和这个线程交互（不能 join，不能知道它什么时候结束）。

③资源回收由操作系统负责



# 三. 互斥量`std::mutex`

首先来看一个例子：

```cpp
#include<bits/stdc++.h>

int cnt = 0;
void work(){
    for(int i = 0; i < 100000; i++){
        cnt++;
    }
}
int main() {

    std::thread t1(work);
    std::thread t2(work);

    t1.join();
    t2.join();
    std::cout << cnt << "\n";
    return 0;
}
```

进行两次测试，发现输出答案依次是100746，164591，而预期值是200000

这是由于两个线程同时进行，并且对同一变量进行了操作，导致了错误

为避免这种情况，我们引入了`std::mutex`

* `mutex`的作用是当多个线程要对同一变量进行操作时，对变量上锁，保证每次只能有一个线程对共享数据进行操作



## 3.1 基础用法

```cpp
#include<bits/stdc++.h>

int cnt = 0;

std::mutex mtx; //定义了互斥量
void work(){
    for(int i = 0; i < 100000; i++){
        mtx.lock();//上锁
        cnt++;
        mtx.unlock();//解锁
    }
}
int main() {

    std::thread t1(work);
    std::thread t2(work);

    t1.join();
    t2.join();
    std::cout << cnt << "\n";
    return 0;
}
```

此时进行多次测试，发现答案始终为200000，说明每次只有一个线程对共享数据进行了操作

但是这种写法有一个问题，如果没有`unlock`，那么后续线程都会被阻塞，程序卡死

于是引入了自动解锁写法



## 3.2 `std::lock_guard`

特点：在锁生命周期结束后自动解锁（即使函数中间 throw 异常、return，也会解锁）

```cpp
#include<bits/stdc++.h>

int cnt = 0;

std::mutex mtx; //定义互斥量
void work(){
    for(int i = 0; i < 100000; i++){
        std::lock_guard<std::mutex>lock(mtx);
        cnt++;
    }
}
int main() {

    std::thread t1(work);
    std::thread t2(work);

    t1.join();
    t2.join();
    std::cout << cnt << "\n";
    return 0;
}
```



* 要注意的是`std::mutex`不支持递归上锁（即允许同一线程对同一互斥量多次上锁），如果要递归上锁的话要用`std::recursive_mutex`



## 3.3 `std::unique_lock`

`std::unique_lock`是比`std::lock_guard`更灵活、更强大的智能锁，它同样是RAII（自动解锁），但是它允许在持有锁的过程中手动解锁和重新上锁

常用构造方式：

```cpp
std::unique_lock<std::mutex> lock1(mtx);                    // 立即上锁（最常用）

std::unique_lock<std::mutex> lock2(mtx, std::defer_lock);   // 创建时不上锁，后面手动 lock()

std::unique_lock<std::mutex> lock3(mtx, std::try_to_lock);  // 尝试上锁，不阻塞

std::unique_lock<std::mutex> lock4(mtx, std::adopt_lock);   // 假设已经上锁了，接管它
```



# 四. 条件变量`std::condition_variable`

条件变量用于阻塞一个或多个线程，直到某个条件完成，`condition_variable`通知其余阻塞线程。从而使得已阻塞的线程可以继续处理后续的操作。



```cpp
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>
#include <iostream>
#include <chrono>

std::mutex mtx;                          // 门锁
std::condition_variable cv;              // 铃铛
std::queue<int> tasks;                   // 共享任务队列
bool finished = false;                   // 结束标志

// ==================== 生产者（厨师） ====================
void producer() {
    for (int i = 0; i < 10; ++i) {
        {
            std::lock_guard<std::mutex> lock(mtx);   // 1. 上锁（进厨房）
            tasks.push(i);                            // 2. 放入任务（做好菜）
            std::cout << "生产者：放入任务 " << i << "\n";
        } // 3. lock_guard 在这里自动解锁（出门）

        cv.notify_one();                              // 4. 摇铃铛！通知一个服务员
        std::this_thread::sleep_for(std::chrono::milliseconds(300));
    }

    // 告诉大家收工
    {
        std::lock_guard<std::mutex> lock(mtx);
        finished = true;
    }
    cv.notify_all();                                  // 通知所有服务员：今天结束了
}

// ==================== 消费者（服务员） ====================
void consumer(int id) {
    while (true) {
        std::unique_lock<std::mutex> lock(mtx);       // 必须用 unique_lock！

        // 5. 最重要的一行：睡觉 + 等待条件满足
        cv.wait(lock, []() { 
            return !tasks.empty() || finished;   // 条件：有任务 或者 已经结束
        });

        // 6. 被唤醒后，lock 已经自动重新上锁了
        if (finished && tasks.empty()) {
            std::cout << "消费者" << id << " 收工了\n";
            break;
        }

        int task = tasks.front();
        tasks.pop();
        // lock 可以在这里提前解锁（推荐）
        lock.unlock();                                // 7. 立刻释放锁，提高并发

        std::cout << "消费者" << id << " 处理任务 " << task << "\n";
        std::this_thread::sleep_for(std::chrono::milliseconds(500));  // 模拟处理时间
    }
}

int main() {
    std::thread prod(producer);
    std::thread cons1(consumer, 1);
    std::thread cons2(consumer, 2);

    prod.join();
    cons1.join();
    cons2.join();

    std::cout << "所有线程结束\n";
}
```



# 五. `epoll`

`epoll` 是 Linux 提供的一种高性能 I/O 多路复用机制，它可以同时监听大量文件描述符（file descriptor），并且只在“有事件发生”时通知你



## 5.1 三个核心函数

在头文件`sys/epoll.h`下

### ①`epoll_create1`

定义方法：`int epfd = epoll_create1(EPOLL_CLOEXEC);`

创建一个`epoll`实例，返回一个`epoll`文件操作符

（文件描述符（File Descriptor，简称 fd） 是 Linux/Unix 系统用来标识一个打开的文件或资源的非负整数。）



当调用 `fork()` 创建子进程时，父进程的所有文件描述符（包括这个 `epfd`）默认会被子进程继承。

而参数`EPOLL_CLOEXEC`的作用是在执行 `exec()`族函数（比如 `execl()`、``execve()`` 等，用于替换当前进程映像）时， 自动关闭这个 epoll 文件描述符。



### ②`epoll_ctl`

作用是向 epoll 实例中添加、修改或删除一个文件描述符（fd），并告诉 epoll 你关心这个 fd 的哪些事件。



函数原型：

```cpp
int epoll_ctl(int epfd, 
              int op, 
              int fd, 
              struct epoll_event *event);
```

| 参数    | 类型                   | 含义                                               | 是否必须传入                       |
| ------- | ---------------------- | -------------------------------------------------- | ---------------------------------- |
| `epfd`  | `int`                  | 用 `epoll_create1()` 创建的 epoll 实例的文件描述符 | 是                                 |
| `op`    | `int`                  | 操作类型：添加 / 修改 / 删除                       | 是                                 |
| `fd`    | `int`                  | 要监听的那个文件描述符（socket、管道等）           | 是                                 |
| `event` | `struct epoll_event *` | 事件设置（告诉 epoll 你关心什么事件）              | 添加/修改时必须，删除时可以为 NULL |



参数op的三种形式：

| 操作宏          | 值   | 含义                           | 什么时候用             |
| --------------- | ---- | ------------------------------ | ---------------------- |
| `EPOLL_CTL_ADD` | 1    | **添加**一个 fd 到 epoll 中    | 新连接到来时（最常用） |
| `EPOLL_CTL_MOD` | 3    | **修改**一个已存在的 fd 的事件 | 需要改变监听事件时     |
| `EPOLL_CTL_DEL` | 2    | 从 epoll 中**删除**一个 fd     | 连接关闭时             |



* `struct epoll_event` 结构体

```cpp
struct epoll_event {
    uint32_t     events;      // 事件掩码
    epoll_data_t data;        // 用户自定义数据（用来找回 fd 或其他信息）
};
```

`events`常用事件标志：

| 事件标志       | 含义                              | 说明                             |
| -------------- | --------------------------------- | -------------------------------- |
| `EPOLLIN`      | 可读（有数据到达）                | 最常用                           |
| `EPOLLOUT`     | 可写（发送缓冲区有空间）          | 通常配合非阻塞使用               |
| `EPOLLET`      | **边缘触发**（Edge Triggered）    | 高性能服务器必用                 |
| `EPOLLLT`      | 水平触发（Level Triggered，默认） | 简单但性能较差                   |
| `EPOLLONESHOT` | 触发一次后自动失效                | 防止同一个 fd 被多个线程同时处理 |
| `EPOLLRDHUP`   | 对端关闭连接或半关闭              | 常用                             |



`epoll_data_t `是一个 联合体，它的定义是：

```cpp
typedef union epoll_data {
    void        *ptr;      // ① 最常用：存储指针
    int          fd;       // ② 存储文件描述符
    uint32_t     u32;      // ③ 存储 32位无符号整数
    uint64_t     u64;      // ④ 存储 64位无符号整数
} epoll_data_t;
```



### ③ `epoll_wait`

作用是让当前线程阻塞等待，直到 epoll 监控的文件描述符上有事件发生，然后把已经就绪的事件返回给你。



函数原型：

```cpp
int epoll_wait(int epfd, 
               struct epoll_event *events, 
               int maxevents, 
               int timeout);
```

参数：

| 参数        | 类型                   | 含义                                                   | 常用值 / 注意事项 |
| ----------- | ---------------------- | ------------------------------------------------------ | ----------------- |
| `epfd`      | `int`                  | 你之前用 `epoll_create1()` 创建的 epoll 实例           | 必须是有效的 epfd |
| `events`    | `struct epoll_event *` | **输出参数**：内核会把就绪的事件**填充**到这个数组里   | 必须是数组首地址  |
| `maxevents` | `int`                  | 数组能容纳的最大事件个数（告诉内核最多返回多少个事件） | 通常设为数组大小  |
| `timeout`   | `int`                  | 超时时间（毫秒）                                       | -1 = 无限等待     |



返回值及其含义：

①$\gt 0$：成功，返回本次就绪的事件个数（可能是 $1~maxevents$）

②$0$：超时返回（只有 $timeout \ge 0$ 时才可能发生）

③$-1$：出错（比如被信号中断），需检查 $errno$



# 六. LT模式与ET模式

| 模式   | 全称            | 中文名   | 触发时机                                     |
| ------ | --------------- | -------- | -------------------------------------------- |
| **LT** | Level Triggered | 水平触发 | 只要条件满足（缓冲区有数据），就**一直触发** |
| **ET** | Edge Triggered  | 边缘触发 | **只在状态发生变化的瞬间**触发一次           |
