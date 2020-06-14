# 创建django项目

当我们使用`django-admin startproject name`时，Django内部会发生什么？

1. 使用Pycharm调试django

   >`django-admin startproject name`

   ![](.\startproject.jpg)

2. 函数入口

   >调用**core.management**文件中的**execute_from_command_line()**
   
   ![命令入口](.\命令入口.png)
   
   >**execute_from_command_line()**中实例一个**class ManagementUtility**对象调用**execute（）**
   
   ![management对象](.\management对象.png)
   
   >
   
   
   ![BaseCommand](.\BaseCommand.png)



   

