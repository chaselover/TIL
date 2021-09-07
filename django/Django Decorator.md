# Django Decorator

* 장고는 다양한 HTTP기능을 지우너하기 위해 view에 적용할 수 있는 여러 데코레이터를 제공한다.
* `HttpResponseNotAllowed`클래스의 요건이 맞지 않으면 반환된다.['GET','POST']형식의 매개변수.



## HTTP 메서드

* **require_http_methods**

```python
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET", "POST"])
def my_view(request):
    # I can assume now that only GET or POST requests make it this far
    # ...
    pass
```



* require_GET
* require_POST
* require_safe



## 클래스 꾸미기

```python
decorators = [never_cache, login_required]

@method_decorator(decorators, name='dispatch')
class ProtectedView(TemplateView):
    template_name = 'secret.html'

@method_decorator(never_cache, name='dispatch')
@method_decorator(login_required, name='dispatch')
class ProtectedView(TemplateView):
    template_name = 'secret.html'
```

위와 아래는 동일하다.

