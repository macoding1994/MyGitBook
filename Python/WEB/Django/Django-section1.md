# Model

[TOC]



## ORM

### 什么是ORM

ORM (Object-Relationl Mapping)是关系型数据库与和对象之间的映射

### 为什么有ORM

主要是将业务逻辑和数据逻辑分离，将MVT中MV层解耦合；在一定程度上可以防止SQL注入;避免写一些复制的SQL语句，但性能会有所下降

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



打印ORM转换过程中的SQL，需修改settings.py，在最后一行添加

``` python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console':{
            'level':'DEBUG',
            'class':'logging.StreamHandler',
        },
    },
    'loggers': {
        'django.db.backends': {
            'handlers': ['console'],
            'propagate': True,
            'level':'DEBUG',
        },
    }
}
```



### 怎么用

* 链式调用接口
  
  > 支持链式调用的接口，返回的均为QuerySet对象
  
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

### 实例

先在创建一个测试Models

```python
from django.db import models

#定义图书模型类BookInfo
class BookInfo(models.Model):
    btitle = models.CharField(max_length=20, verbose_name='名称')
    bpub_date = models.DateField(verbose_name='发布日期')
    bread = models.IntegerField(default=0, verbose_name='阅读量')
    bcomment = models.IntegerField(default=0, verbose_name='评论量')
    is_delete = models.BooleanField(default=False, verbose_name='逻辑删除')

    class Meta:
        db_table = 'tb_books'  # 指明数据库表名
        verbose_name = '图书'  # 在admin站点中显示的名称
        verbose_name_plural = verbose_name  # 显示的复数名称

    def __str__(self):
        """定义每个数据对象的显示信息"""
        return self.btitle

#定义英雄模型类HeroInfo
class HeroInfo(models.Model):
    GENDER_CHOICES = (
        (0, 'male'),
        (1, 'female')
    )
    hname = models.CharField(max_length=20, verbose_name='名称')
    hgender = models.SmallIntegerField(choices=GENDER_CHOICES, default=0, verbose_name='性别')  
    hcomment = models.CharField(max_length=200, null=True, verbose_name='描述信息')
    hbook = models.ForeignKey(BookInfo, on_delete=models.CASCADE, verbose_name='图书')  # 外键
    is_delete = models.BooleanField(default=False, verbose_name='逻辑删除')

    class Meta:
        db_table = 'tb_heros'
        verbose_name = '英雄'
        verbose_name_plural = verbose_name

    def __str__(self):
        return self.hname
```

接着在控制台输入`	python manage.py makemigrations` `python manage.py migrate`,输入查询数据

```
insert into tb_books(btitle,bpub_date,bread,bcomment,is_delete) values
('射雕英雄传','1980-5-1',12,34,0),
('天龙八部','1986-7-24',36,40,0),
('笑傲江湖','1995-12-24',20,80,0),
('雪山飞狐','1987-11-11',58,24,0);
```

```
insert into tb_heros(hname,hgender,hbook_id,hcomment,is_delete) values
('郭靖',0,1,'降龙十八掌',0),
('黄蓉',1,1,'打狗棍法',0),
('黄药师',0,1,'弹指神通',0),
('欧阳锋',0,1,'蛤蟆功',0),
('梅超风',1,1,'九阴白骨爪',0),
('乔峰',0,2,'降龙十八掌',0),
('段誉',0,2,'六脉神剑',0),
('虚竹',0,2,'天山六阳掌',0),
('王语嫣',1,2,'神仙姐姐',0),
('令狐冲',0,3,'独孤九剑',0),
('任盈盈',1,3,'弹琴',0),
('岳不群',0,3,'华山剑法',0),
('东方不败',1,3,'葵花宝典',0),
('胡斐',0,4,'胡家刀法',0),
('苗若兰',1,4,'黄衣',0),
('程灵素',1,4,'医术',0),
('袁紫衣',1,4,'六合拳',0);
```





#### select_related

常用于Django中ORM的 **N+1** 问题，用表内联（INNER JOIN）的方式优化查询，降低SQL查询次数,提高系统性能,N+1应发查询次数**（K*N + 1）**，K为外键数，N为Modal对象数量

```python
# N+1 问题

heros = HeroInfo.objects.all()[:5]
print(heros.count()) # 5
for hero in heros:
    print(hero.hbook.id)
# --------------------   console  ----------------------
(0.002) SELECT COUNT(*) FROM (SELECT "tb_heros"."id" AS Col1 FROM "tb_heros"  LIMIT 5) subquery; args=()
(0.000) SELECT "tb_heros"."id", "tb_heros"."hname", "tb_heros"."hgender", "tb_heros"."hcomment", "tb_heros"."hbook_id", "tb_heros"."is_delete" FROM "tb_heros"  LIMIT 5; args=()
(0.001) SELECT "tb_books"."id", "tb_books"."btitle", "tb_books"."bpub_date", "tb_books"."bread", "tb_books"."bcomment", "tb_books"."is_delete" FROM "tb_books" WHERE "tb_books"."id" = 1; args=(1,)
(0.001) SELECT "tb_books"."id", "tb_books"."btitle", "tb_books"."bpub_date", "tb_books"."bread", "tb_books"."bcomment", "tb_books"."is_delete" FROM "tb_books" WHERE "tb_books"."id" = 1; args=(1,)
(0.001) SELECT "tb_books"."id", "tb_books"."btitle", "tb_books"."bpub_date", "tb_books"."bread", "tb_books"."bcomment", "tb_books"."is_delete" FROM "tb_books" WHERE "tb_books"."id" = 1; args=(1,)
(0.000) SELECT "tb_books"."id", "tb_books"."btitle", "tb_books"."bpub_date", "tb_books"."bread", "tb_books"."bcomment", "tb_books"."is_delete" FROM "tb_books" WHERE "tb_books"."id" = 1; args=(1,)
(0.000) SELECT "tb_books"."id", "tb_books"."btitle", "tb_books"."bpub_date", "tb_books"."bread", "tb_books"."bcomment", "tb_books"."is_delete" FROM "tb_books" WHERE "tb_books"."id" = 1; args=(1,)
```

```python
# 解决 N+1 问题

heros = HeroInfo.objects.all().select_related[:5]
print(heros.count()) # 5
for hero in heros:
    print(hero.hbook.id)
# --------------------   console  ----------------------
(0.001) SELECT COUNT(*) FROM (SELECT "tb_heros"."id" AS Col1 FROM "tb_heros"  LIMIT 5) subquery; args=()
(0.000) SELECT "tb_heros"."id", "tb_heros"."hname", "tb_heros"."hgender", "tb_heros"."hcomment", "tb_heros"."hbook_id", "tb_heros"."is_delete", "tb_books"."id", "tb_books"."btitle", "tb_books"."bpub_date", "tb_books"."bread", "tb_books"."bcomment", "tb_books"."is_delete" FROM "tb_heros" INNER JOIN "tb_books" ON ("tb_heros"."hbook_id" = "tb_books"."id")  LIMIT 5; args=()
```



#### **prefetch_related**

