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

