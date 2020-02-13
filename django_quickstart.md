## Django速查手册
### 开始项目
1. `django-admin startproject mysite`
2. 进入项目目录并创建app
   ```
   cd mysite
   chmod +x manage.py
   ./manage.py startapp myapp
   ```
3. 设置mysite/settings.py
   将myapp添加到INSERT_APPS里面
4. 写入SQL表以启用admin等内置app
   `./manage.py migrate`
5. 创建静态文件目录
   ```
   mkdir -p static/myapp
   mkdir -p templates/myapp
   ```
6. 创建超级用户
   `./manage.py createsuperuser`

7. 运行测试服务器
   `./manage.py runserver`

### Model
- 声明model
  ```python
  from django import models
  class MyModel(models.Model):
       myfield = models.Field()
  ```
- 保存模型
  `manage.py makemigrations`
  `manage.py migrate`
- model操作
  - `Model.objects.get(condition)`
  - `model.save()`
### 使用admin
在admin.py中注册模型：
`admin.site.register(YourModel)`
