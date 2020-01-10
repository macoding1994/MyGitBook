# 线程

> 线程：就是在程序运行过程中，执行程序代码的一个分支，每个运行的程序至少都有一个线程，是CPU调度的基本单位
>
> 相关概念：
>
> - 线程锁 ： 互斥锁对共享数据进行锁定，保证同一时刻只能有一个线程去操作

## 原生threading.Thread

### 导包

```python
#导入线程模块
import threading
```

### 创建

> 参数说明：
>
> * target : 执行任务的函数引用，不能写成`func()`
> * args : 元祖传参
> * kwargs ：字典传参

```python
# 创建一个线程
thread = threading.Thread(target=func,args,kwargs)
```

### 启动

```python
# 创建一个线程
thread = threading.Thread(target=func,args,kwargs)
thread.start()
```

### 互斥锁
> 使用：
> - 如果这个锁之前是没有上锁的，那么`acquire`不会堵塞
> - 如果在调用`acquire`对这个锁上锁之前 它已经被 其他线程上了锁，那么此时`acquire`会堵塞，直到这个锁被解锁为止
>
> 目的：
>
> * 能够保证多个线程访问共享数据不会出现资源竞争及数据错误

```	python
# 创建锁
mutex = threading.Lock()

# 锁定
mutex.acquire()

# 释放
mutex.release()

# 解决死锁问题 使用上下文管理器管理LOCK
with mutex:
    pass
```
### 常用API

> 查看当前线程 ： `threading.current_thread().name`
>
> 活动线程的列表 ：`threading.enumerate()`

### demo

#### 1.多线程共享全局变量的代码

```python
import threading
import time


# 定义全局变量
my_list = list()

# 写入数据任务
def write_data():
    for i in range(5):
        my_list.append(i)
        time.sleep(0.1)
    print("write_data:", my_list)


# 读取数据任务
def read_data():
    print("read_data:", my_list)


if __name__ == '__main__':
    # 创建写入数据的线程
    write_thread = threading.Thread(target=write_data)
    # 创建读取数据的线程
    read_thread = threading.Thread(target=read_data)

    write_thread.start()
    # 延时
    # time.sleep(1)
    # 主线程等待写入线程执行完成以后代码在继续往下执行
    write_thread.join()
    print("开始读取数据啦")
    read_thread.start()
#--------------------------------------------------------------------------------------
write_data: [0, 1, 2, 3, 4]
开始读取数据啦
read_data: [0, 1, 2, 3, 4]
```

#### 2.使用互斥锁完成2个线程对同一个全局变量各加100万次的操作

```python
import threading


# 定义全局变量
g_num = 0

# 创建全局互斥锁
lock = threading.Lock()


# 循环一次给全局变量加1
def sum_num1():
    # 上锁
    lock.acquire()
    for i in range(1000000):
        global g_num
        g_num += 1

    print("sum1:", g_num)
    # 释放锁
    lock.release()


# 循环一次给全局变量加1
def sum_num2():
    # 上锁
    lock.acquire()
    for i in range(1000000):
        global g_num
        g_num += 1
    print("sum2:", g_num)
    # 释放锁
    lock.release()


if __name__ == '__main__':
    # 创建两个线程
    first_thread = threading.Thread(target=sum_num1)
    second_thread = threading.Thread(target=sum_num2)
    # 启动线程
    first_thread.start()
    second_thread.start()

    # 提示：加上互斥锁，那个线程抢到这个锁我们决定不了，那线程抢到锁那个线程先执行，没有抢到的线程需要等待
    # 加上互斥锁多任务瞬间变成单任务，性能会下降，也就是说同一时刻只能有一个线程去执行
#--------------------------------------------------------------------------------------
sum1: 1000000
sum2: 2000000
```

#### 3.自定义线程类

> - 自定义线程不能指定target，因为自定义线程里面的任务都统一在run方法里面执行
> - 启动线程统一调用start方法，不要直接调用run方法, 因为这样不是使用子线程去执行任务

```python
import threading

# 自定义线程类
class MyThread(threading.Thread):
    # 通过构造方法取接收任务的参数
    def __init__(self, info1, info2):
        # 调用父类的构造方法
        super(MyThread, self).__init__()
        self.info1 = info1
        self.info2 = info2

    # 定义自定义线程相关的任务
    def show1(self):
        print(self.info1)

    def show2(self):
        print(self.info2)

    # 通过run方法执行相关任务
    def run(self):
        self.show1()
        self.show2()

# 创建自定义线程
my_thread = MyThread("测试1", "测试2")
# 启动
my_thread.start()
```

---

## concurrent.futures线程池

> 为什么需要线程池：
>
> * 创建了20个线程，而同时只允许3个线程在运行，但是20个线程都需要创建和销毁，线程的创建是需要消耗系统资源
>
> 线程池思想：
>
> * 与进程池思想差不多，每个线程各分配一个任务，剩下的任务排队等待，当某个线程完成了任务的时候，排队任务就可以安排给这个线程继续执行，自动去调度线程
> * 与markdown一般，不需要去花大量心思去排版（互斥锁），只需要注重你的文字（代码编写）
>
> 线程池优点：
>
> * 主线程可以获取某一个线程（或者任务的）的状态，以及返回值
> * 当一个线程完成的时候，主线程能够立即知道
> * 让多线程和多进程的编码接口一致

### 导包

```python
# 标准库concurrent.futures模块 不需要pip安装
from concurrent.futures import ThreadPoolExecutor
```

### 创建

```python
# 使用上下文来创建线程池
with ThreadPoolExecutor(max_workers=2) as pool:
    pool.submit(func,args)

# 正常创建线程池
pool = ThreadPoolExecutor(max_workers=2)
pool.submit(func,args)
pool.shutdown()
```

### 线程池API

> submit(func,args):
>
> > `submit`函数来提交线程需要执行的任务（函数名和参数）到线程池中,并且返回一个future对象
>
> * func -- 函数引用
> * args -- 元祖传参
>
> cancel()：
>
> > `cancel`函数用来取消提交的任务，前提是这个任务未在线程池中执行，返回Ture/False
>
> result():
>
> > `result`函数用于返回任务完成后的返回值，该函数阻碍主线程
>
> as_completed():
>
> > `as_completed()`方法是一个生成器，在没有任务完成的时候，会阻塞，在有某个任务完成的时候，会`yield`这个任务，就能执行for循环下面的语句，然后继续阻塞住，循环到所有的任务结束
> >
> > ```python
> > from concurrent.futures import ThreadPoolExecutor, as_completed
> > import time
> > 
> > # 参数times用来模拟网络请求的时间
> > def get_html(times):
> >     time.sleep(times)
> >     print("get page {}s finished".format(times))
> >     return times
> > 
> > executor = ThreadPoolExecutor(max_workers=2)
> > urls = [3, 2, 4] # 并不是真的url
> > all_task = [executor.submit(get_html, (url)) for url in urls]
> > 
> > for future in as_completed(all_task):
> >     data = future.result()
> >     print("in main: get page {}s success".format(data))
> > 
> > # 执行结果
> > # get page 2s finished
> > # in main: get page 2s success
> > # get page 3s finished
> > # in main: get page 3s success
> > # get page 4s finished
> > # in main: get page 4s success
> > ```
>
> map():
>
> > `map`函数与python中的`map`一样，会顺便执行，但是本质上是与`submit`和`result`结合
> >
> > ```python
> > from concurrent.futures import ThreadPoolExecutor
> > import time
> > 
> > # 参数times用来模拟网络请求的时间
> > def get_html(times):
> >     time.sleep(times)
> >     print("get page {}s finished".format(times))
> >     return times
> > 
> > executor = ThreadPoolExecutor(max_workers=2)
> > urls = [3, 2, 4] # 并不是真的url
> > 
> > for data in executor.map(get_html, urls):
> >     print("in main: get page {}s success".format(data))
> > # 执行结果
> > # get page 2s finished
> > # get page 3s finished
> > # in main: get page 3s success
> > # in main: get page 2s success
> > # get page 4s finished
> > # in main: get page 4s success
> > ```
> add_done_callback（func）：推荐
>
> > `add_done_callback`函数是future对象的回调函数，当一个future对象完成任务后会回调`func`函数，此方法是异步返回结果，与`result`不同，并不会阻塞主线程
> >
> > ```python
> > from concurrent.futures import ThreadPoolExecutor
> > import time
> > 
> > # 参数times用来模拟网络请求的时间
> > def get_html(times):
> >     time.sleep(times)
> >     print("get page {}s finished".format(times))
> >     return times
> > 
> > def print_res(future):
> >     print(future.result())
> > 
> > urls = [3, 2, 4] # 并不是真的url
> > 
> > with ThreadPoolExecutor(max_workers=2) as pool:
> >     for url in urls:
> >         future = pool.submit(get_html,url)
> >         future.add_done_callback(print_res)
> > # get page 2s finished
> > # 2
> > # get page 3s finished
> > # 3
> > # get page 4s finished
> > # 4
> > ```



