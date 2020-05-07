# Model

[TOC]



## ORM

### 什么是ORM

ORM (Object-Relationl Mapping)是关系型数据库与和对象之间的映射

### 为什么有ORM

主要是将业务逻辑和数据逻辑分离，将MVT中MV层解耦合；在一定程度上可以防止SQL注入避免写一些复制的SQL语句，但性能会有所下降

### 怎么用

* 数值型：

  * AutoField  自增长主键，Django Model 默认提供，可以被重写。完整定义为 id = models.AutoField(primary=Ture)  

  * BooleanField  布尔类型字段  

  * DecimalField  开发对数据精度要求较高的业务时考虑使用  

  * IntegerField  同AutoField一样，唯一的差距就是不自增  

  * PositiveIntegerField  同IntegerField，只包含正整数  

  * SmallIntegerField  小整数时一般会用到

* 字符型：

  * CharField  基础的varchar类型
  * URLField  继承自CharField,但是实现了对URL的特殊处理
  * EmailField  继承自CharField,但是实现了对E-mail的特殊处理
  * FileField  同URLField 一样，多了对文件的特殊处理。当你定义了该字段时，在admin部分展示时会自动生成一个上传文件的按钮
  * TextField  一般用来存放大量文本内容
  * ImageField  继承自FileField,用来处理图片相关的数据

* 日期型：

  * DateField
  * DateTimeField
  * TimeField

* 关系型：

  * Foreignkey
  * OneToOneField
  * ManyToManyField

* 配置：

  * null  如果为True，表示允许为空，默认值是False
  * choices  配置字段后，admin页面上就可以看到对应的可选展示
  * db_column  字段的名称，如果未指定，则使用属性的名称
  * db_index  若值为True, 则在表中会为此字段创建索引，默认值是False
  * default  默认值
  * editable 是否可编辑，默认时Ture
  * help_text  提示语
  * primary_key  主键，一个model钟只允许有一个
  * unique  唯一约束,当需要配置唯一值时设置为Ture

## QuerySet

### 什么是QuerySet

在Django中，Model有一个objects的属性来提供数据操作的接口，返回的对象就要QuerySet，也叫查询集。QuerySet具有**惰性执行**和**缓存**特性,在使用数据时，才会去DB中查询

```	python
qs = BookInfo.objects.all()  # 返回QuerySet对象
for book in qs:
    print(book.btitle)  # 执行查询语句
```

### 怎么用

* 链式调用接口
  * all
  * filter
  * exclude
  * reverse
  * distinct
  * none
* 非链式调用接口
  * get
  * create
  * get_or_create
  * update_or_create
  * count
  * latest
  * earliest
  * first
  * last
  * exists
  * bulk_create
  * in_bulk
  * update
  * delete
  * values
* 进阶接口（性能优化）
  * defer
  * only
  * select_related
  * prefetch_related
* 常用字段查询
* 进阶查询

