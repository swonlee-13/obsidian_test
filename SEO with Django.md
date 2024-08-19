# 1. slug

#slug 는 URL에서 사용되는 특별한 형식의 문자열입니다. 주로 웹 개발에서 다음과 같은 특징을 가집니다:
 예시
```python
from django.urls import path

from . import views

urlpatterns = [
    path("articles/2003/", views.special_case_2003),
    path("articles/<int:year>/", views.year_archive),
    path("articles/<int:year>/<int:month>/", views.month_archive),
    path("articles/<int:year>/<int:month>/<slug:slug>/", views.article_detail),
]
```

1. 정의:
    - 일반적으로 제목이나 이름을 URL 친화적인 문자열로 변환한 것입니다.
2. 특징:
    - 소문자, 숫자, 하이픈(-)으로 구성됩니다.
    - 공백은 하이픈으로 대체됩니다.
    - 특수 문자는 대부분 제거됩니다.
3. 용도:
    - SEO(검색 엔진 최적화)에 유리합니다.
    - URL을 읽기 쉽고 이해하기 쉽게 만듭니다.
    - 리소스를 고유하게 식별하는 데 사용됩니다.
4. 예시:
    - 제목: "Building a Django Site"
    - Slug: "building-a-django-site"
5. Django에서의 사용:
    - `<slug:slug>` 패턴은 이러한 형식의 문자열을 매칭합니다.
    - Django의 모델에서 `SlugField`를 사용하여 자동으로 생성할 수 있습니다.

```python
path('articles/<slug:slug>/', views.article_detail)

# views.py 
def article_detail(request, slug):
    article =Article.objects.get(slug=slug)
    return render(request, 'article_detail.html', {'article': article})
```

여기서, 뷰 함수는 전달받은 slug를 사용하여 데이터베이스에서 해당 리소스를 조회할 수 있습니다.

주어진 예시에서 "building-a-django-site"는 아티클의 제목이나 식별자를 나타내는 slug입니다. 이를 통해 URL이 더 의미있고 읽기 쉬워집니다.