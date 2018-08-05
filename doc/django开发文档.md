### 开发过程记录

创建项目目录，进入目录创建虚拟环境目录，打开虚拟环境，安装django

1. `mkdir djangogirls`
2. `cd djgnaogirls`
3. `python -m venv myvenv`
4. `myvenv\Scripts\activate`
5. `pip install django==1.8`


创建一个项目，并设置基本设置，添加时区，加载文件路径，数据库

6. `django-admin startproject mysite`
7. `cd mysite`

	settings.py
	TIME_ZONE = 'Asia/Shanghai'  # 添加时区
	STATIC_ROOT = os.path.join(BASE_DIR, 'static'})  # 添加静态文件路径

	DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    	}
	}  # 设置数据库

8. `python manage.py migrate` 

	python manage.py runserver  # 运行项目，看看效果。 应该在有manage.py的文件夹中运行该命令
	在浏览器 输入127.0.0.1：8000  # 看效果


创建应用，

9. `python manage.py startapp blog`

	settings.py
	INSTALLED_APPS = [
		'blog',  # 在设置中添加应用信息，让django能够加载该应用的数据
	]

10. `cd blog`

```
	models.py
	from django.db import models  # 导入模型类
	from django.utils import timezone  # 导入时间模块

	class Post(models.Model):  # 继承模型
		author = models.ForeignKey('auth.User', on_delete=models.CASCADE)  # 指向auth.User模型
		title = models.CharField(max_length=200)  # 标题字段，最大200字符
		text = models.TextField()  # 文档字段
		created_time = models.DateTimeField(default=timezone.now)  # 创建时间 默认为当前时间
		published_time = models.DateTimeField(blank=True, null=True)  # 发布时间，可以为空，可以为None

		def publish(self):
			"""
			设置发布时间可以为当前时间的快捷设置
			"""
			self.published_time = timezone.now()
			self.save()

		def __str__(self):
			"""
			定义调用结果集是显示的内容为当前文章标题
			"""
			return self.title

```

让django知道数据库模型变更，并让django准备数据库的迁移文件

11. `python manage.py makemigrations blog`
12. `python manage.py migrate blog`

管理后台，创建后台模板，添加用户，添加文章

```	
	blog/admin.py
	from django.contrib import admin
	from .models import Post

	admin.site.register(Post)

	$python manage.py runserver # 运行测试
	127.0.0.1:8000\admin   # 打开浏览器查看后台
```

13. `python manage.py createsuperuser`

14. `127.0.0.1:8000\admin`

