# Kivy

Kivy是一款开源，可移植的PythonGUI框架，只需编写一套代码，便可运行于各大桌面及移动平台上（包括 Linux, Windows, OS X, Android, iOS, 以及 Raspberry Pi） Kivy 采用 Python 和 Cython  编写，在国外已经十分火爆

[Kivy官网](https://kivy.org/#home)是学习Kivy非常重要的一种途径，[Kivy官方教程](https://kivy.org/doc/stable/gettingstarted/intro.html)|[Kivy API](https://kivy.org/doc/stable/api-kivy.html)从中可以找到许多有用的demo,还有[Kivy Garden](https://github.com/kivy-garden)里面集成了许多非常有用的插件[garden.matplotlib](https://github.com/kivy-garden/garden.matplotlib)、[Mapview](https://github.com/kivy-garden/garden.mapview)等等

Kivy提供了一种专门针对简单和可伸缩的GUI设计的设计语言，KV语言。该语言使接口设计与应用程序逻辑分离变得简单，并遵循[关注点分离原则](http://en.wikipedia.org/wiki/Separation_of_concerns)，与PyQt中QT语言类似，将业务逻辑与界面逻辑分离

### 安装

在window下依次输入以下命令

```python
python -m pip install docutils pygments pypiwin32 kivy.deps.sdl2 kivy.deps.glew
python -m pip install kivy.deps.gstreamer
python -m pip install kivy
```

验证安装成功

```python
from kivy.app import App
from kivy.uix.button import Button

class TestApp(App):
    def build(self):
        return Button(text='Hello World')

TestApp().run()
```

### KV 与 Python

#### 怎么加载KV语言

* 通过名字加载

  Kivy查找与App类同名的Kv文件，如果它以‘App’结尾，则减去“App”：

  ```
  MyApp -> my.kv
  ```

  ![1589361426650](.\loadKV.png)

* Builder

  ```python
  Builder.load_file('./Test.kv')
  
  Builder.load_string(kv_string)
  ```

  

#### Python2kv

#### kv2Python