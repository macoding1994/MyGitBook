# 进程

>进程：
>
>* 操作系统资源分配的基本单位
>* 一个程序至少有一个进程，一个进程至少有一个线程
>* 线程是依附在进程里面的，没有进程就没有线程
>
>进程的状态：
>
>![](images\进程状态.png)
>
>特点：
>
>* 多进程之间不共享全局变量，需要`multiprocessing.queue`

## 原生multiprocessing.Process

### 导包

```python
import multiprocessing
```

### 创建

> 参数说明：
>
> - target : 执行任务的函数引用，不能写成`func()`
> - args : 元祖传参
> - kwargs ：字典传参
>
> 具体使用与线程差不多

```python
process = multiprocessing.Process(target=func,args=(),kwargs={})
```

### 启动

```python
process = multiprocessing.Process(target=func,args=(),kwargs={})
process.start()
```

### 守护进程

```python
import multiprocessing
import time

# 测试子进程是否执行完成以后主进程才能退出
def work():
    for i in range(10):
        print("工作中...")
        time.sleep(0.2)

if __name__ == '__main__':
    # 创建子进程
    work_process = multiprocessing.Process(target=work)
    # 设置守护主进程，主进程退出后子进程直接销毁，不再执行子进程中的代码
    # work_process.daemon = True
    work_process.start()

    # 让主进程等待1秒钟
    time.sleep(1)
    print("主进程执行完成了啦")
    # 让子进程直接销毁，表示终止执行， 主进程退出之前，把所有的子进程直接销毁就可以了
    work_process.terminate()
    # 总结： 主进程会等待所有的子进程执行完成以后程序再退出
```