# Filtering


> “ 由Manager提供的根QuerySet描述了数据库表中的所有对象。通常，可是你只需要选择完整对象的一个子集而已。
> 
—— Django文档
>”
 
 REST framework列表视图的默认行为是返回一个model的全部queryset。通常你却想要你的API来限制queryset返回的数据。

最简单的删选一个`GenericAPIView`子视图的queryset的方法就是复写他的`.get_queryset()`方法。

重写这个方法允许你使用很多不同的方式来定制视图返回的queryset。


# API Guide

## DjangoFilterBackend

`django-filter`库包含一个为REST framework提供高度可定制字段过滤的`DjangoFilterBackend`类。

要使用`DjangoFilterBackend`，首先要先安装`django-filter`。

```
pip install django-filter
```

现在，你需要将filter backend 添加到你django project的settings中：

```
REST_FRAMEWORK = {
    'DEFAULT_FILTER_BACKENDS': ('django_filters.rest_framework.DjangoFilterBackend',)
}
```

或者你也可以将filter backend添加到一个单独的view或viewSet中：

```
from django_filters.rest_framework import DjangoFilterBackend

class UserListView(generics.ListAPIView):
    ...
    filter_backends = (DjangoFilterBackend,)
```

如果你正在使用 browsable API或 admin API，你还需要安装`django-crispy-forms`，通过使用Bootstarp3渲染来提高filter form在浏览器中的展示效果。


```
pip install django-crispy-forms
```

安装完成后，将`crispy-forms`添加到你Django project的`INSTALLED_APPS`中，browsable API将为`DjangoFilterBackend`提供一个像下面这样的filter control：
![django filter](/assets/django-filter.png)

## 指定筛选字段（Specifying filter fields）

如果你的需求都是些简单相等类型的筛选，那么你可以在你的view或viewSet里面设置一个`filter_fields`属性，列出所有你想依靠筛选的字段集合。

```
class ProductList(generics.ListAPIView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    filter_backends = (filters.DjangoFilterBackend,)
    filter_fields = ('category', 'in_stock')
```








