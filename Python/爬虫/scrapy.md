# Scrapy

> Scrapy框架优点:
>
> * 提供整套项目的打包方案，类似Django创建一个项目，开发速度快
> * 使用scrapy框架开发的项目，稳定性极高
> * scrapy框架的底层实现优秀，性能优越
> * 使用scrapy框架实现分布式爬虫简单

## scrapy的流程

![](D:\python\Python-for-GitBook\Python\爬虫\images\scrapy工作流程.png)

> 各个功能模块之间的通讯，必须通过引擎：
>
> 1. 起始的url，调度器会把url等信息封装成请求对象
> 2. 调度器会把请求对象返回给引擎，引擎会把请求对象给下载器，发送请求获取响应
> 3. 下载器吧响应内容给引擎，引擎再将内容给爬虫，解析数据
> 4. 爬虫解析完数据给引擎，引擎判断是否将数据存入数据队列 或者 再次由调度器重新封装请求对象
>
> 一般编写的代码为：
>
> 1. spiders中的解析数据
> 2. 保存数据item
> 3. 一些中间件（比如Downloader middle 反反爬）

## scrapy创建

> 创建一个项目工程： `scrapy startproject <name>`
>
> 

