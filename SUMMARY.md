# Summary

* [简介](README.md)
* Python
    * [1.基础语法](Python/基础语法/basis.md)
    * [2.高级语法](Python/高级语法/advanced.md)
        * [2.1多任务](Python/高级语法/多任务/introduction.md)
            * [2.1.1线程](Python/高级语法/多任务/线程/thread.md)
            * [2.1.2进程](Python\高级语法\多任务\进程\process.md)
            * [2.1.3协程](Python\高级语法\多任务\协程\asyncio.md)
            * 2.1.4比较
    * 3.GUI
        * PyQt
        * [Kivy](Python/GUI/Kivy/Kivy.md)
    * 4.WEB
        * [4.1Django](Python/WEB/Django/Django.md)
          * [4.1.1Django-section1](Python/WEB/Django/Django-section1.md)
          * [4.1.2Django-section2](Python/WEB/Django/Django-section2.md)
          * [4.1.3Django-section3](Python/WEB/Django/Django-section3.md)
          * [4.1.4Django-section4](Python/WEB/Django/Django-section4.md)
          * [4.1.5Django-section5](Python/WEB/Django/Django-section.md)
        * 4.2 Flask
          * 4.2.1 Flask-celery
        * 4.3 Django源码解析
          * [4.3.1 源码](Python/WEB/Django源码解析/section1.md)
    * [5.Spiders](Python/爬虫/introduction.md)
        * [5.1requests](Python/爬虫/request.md)
        * 5.2selenium
        * [5.3scrapy](Python\爬虫\scrapy.md)
    * 6.AI
    * 7.DEMO

------

TODOLIST：

* 5月份：
  * 5.1-5.5
    * MACODINGBLOGS 完善与部署
    * Django企业开发实战（markdown）-- 3D
  * 5.6-5.24
    
    * MACIDUBFBLOGS 完善
    
    * Django-DRF -- 8D
    
      > 解耦硬编码 ： 在逻辑运算中存在无意义的数字，通过定义更加语义化的变量来取代毫无意义的数字
      >
      > 开闭原则：修改代码时无需全局更改
  
* 6月份：

* 7月份：

  * 7.10 阿里云`flask-celery-example`项目老是发生celery挂掉

  * 原因：可能是worker的并发是prefork（多进程）,并且可能是由于死锁原因造成！

  * 解决：

    * 那么可以使用 CELERYD_FORCE = True ，这样可以有效防止死锁。即使不是这个原因造成的，也尽量加上。

    * 第二种解决方式适用于大多数进程hanging的情况，可以使用time_limit参数，设定任务的执行超时时间，当超过这个时间的话，就先生成新的进程，并通过信号将hanging的进程杀死。
另外，如果配置中使用了act_late的参数数，需要配合broker_transport_options = {'visiblity_timeout': 10 *60 }使用，这样，在task经过超时时间之后如果还没被ack, 就会被发送到其他worker去执行。如果没设置ack_late，代表对执行结果并不关心，这个参数也就没必要设置了

* 8月份：

* 9月份：

* 10月份：

* 11月份：

* 12月份：