安装虚拟环境:

```python
python -m venv tutorial-env

启动运行虚拟环境：
tutorial-env\Scripts\activate.bat
```

安装Django

```
环境下:
python -m pip install Django
创建:
django-admin startproject 项目名
运行:
python manage.py runserver ip:端口
或：
python manage.py runserver
可以不写IP和端口，默认IP是127.0.0.1，默认端口为8000。
```

为子应用设置指定环境

```
File 》 setting》project》interpreter》的右边设置图标》选择该项目下的环境
```

模型迁移

```
先注册:
settings.py 中INSTALLED_APPS 中注册子应用
如:'home.apps.HomeConfig'


子应用中 的models.py 中设置模型
运行:
python manage.py makemigrations 子应用名

```

