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
