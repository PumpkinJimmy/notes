## Django速查手册
### Model
- 声明model
    ```python
    from django import models
    class MyModel(models.Model):
   	 myfield = models.Field()
    ```
- 保存模型
   manage.py makemigrations
   manage.py migrate
- model操作
   Model.objects.get(condition)
   model.save()
