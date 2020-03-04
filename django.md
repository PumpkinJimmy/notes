## Django笔记
### 模板继承
提供一个骨架模板，里面包含若干block
在一个子模板中使用extend，然后提供若干个block的具体内容，就可以用子模板中的内容替换骨架模板中对应block的内容，如
```
base.html:
<body>
{% block main  %}
{% endblock %}
</body>

detail.html:
{% extends "base.html" %}
{% block main %}
Hello, Django!
{% endblock %}
```

### 文件上传
1. 使用表单属性methdo="POST", enctype="multipart/form-data"（也就是不对表单数据转义）
2. 使用<input type="file" name="filename" />
3. 上传的文件放在request.FILES["filename"]里边，取数据要read（其实上传文件对象是仿文件接口的）

### Field.null vs. Field.blank
1. field.null指示数据库行为：当字段为空时是否使用null作为空值。
   注意字符类的字段（如CharField）不要使用null=True，由于这些字段默认空值为空串
2. field.blank指示表单验证行为：字段可不可以为空（默认是不可，即blank=False）


### ForeignKey.on_delete
在Django2中on_delet是ForeignKey必须的参数，用以指示当外键被删除时，对象的行为，models.CASCADE表示外键删除时相应删除，models.SET_NULL表示对象的外键字段设为null（前提是该字段null=True），完整列表详见Doc
### DateField.auto_now & DateField.auto_now_add
DateField.auto_now 指示一个日期字段每次执行save时自动使用当天日期，适用于last-modified一类的字段
DateField.auto_now_add指示一个日期字段自动使用对象创建时的日期，适用于create-date一类的字段
### 使用static
在产品环境中，静态文件请求肯定是交给反向代理（Nginx）直接处理。
但在开发阶段，可以使用Django提供的静态文件机制来使用静态文件。
使用方法：
1. 在需要使用静态文件的**html文件开头**添加`{% load static %}`加载static标签
2. 在引用静态文件的位置使用`{% static path %}`来引用，其中path指代文件在STATIC_ROOT下的相对路径。
### 使用session
session基于cookie，但只在客户端保存sessionkey，具体的会话内容保存在服务端，从而提高安全性。
session通过自带的一个Django自带的app实现，默认开启，直接用就可以了。
session可以看作是存放当前用户对应的数据的容器。
基本语法：把`request.session`当作字典来存取。
### 重定向
- 基本方法是使视图返回`django.http.HttpResponseRedirect(url)`
- 快捷方法是使用使用`django.shortcuts.redirect`，传递参数可以是url，也可以是配置的url名称加上适当的参数（其实就是省了自己用`reverse`）
### 使用auth
Django自带的用户验证系统。
三个重要的Model: User, Group, Permission.
- 位于`django.contrib.auth`中，下面简写为`auth`
- `auth.authenticate(username= , password= )`给定用户名及密码验证是否通过，通过则返回相应的User对象，否则返回None
- `auth.models`含三个重要Model即`User` `Group` `Permission`，其中`Group` `Permission`都是`User`的`ManyToMany`字段成员
- 密码使用加盐散列算法处理过，故不可直接操作
- 创建用户：`user = User.objects.create_user; user.save()`
- 改密码：`user.set_password`
- 检查权限：`user.has_perm`
- 检查是否当前已登录：`request.user.is_authenticated`
- 登录：`auth.login(request, user)`；登出：`auth.logout(request)`
- `login_required(login_url= )`装饰器可用于必须登录才能访问的视图，提供参数为登录页面的url，若不提供参数，则使用`settings.LOGIN_URL`的配置