移动应用
===

Ionic 2
---

Login
---

###Json Web Tokens

```
pip install djangorestframework-jwt
```

```
urlpatterns = patterns(
    '',
    # ...

    url(r'^api-token-auth/', 'rest_framework_jwt.views.obtain_jwt_token'),
)
```

TODO
---

