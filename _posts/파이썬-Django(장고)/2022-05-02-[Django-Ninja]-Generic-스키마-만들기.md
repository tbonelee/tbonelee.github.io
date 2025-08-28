---
layout: post
title: "[Django-Ninja] Generic 스키마 만들기"
date: "2022-05-02 12:36:23 +0900"
categories:
  - 파이썬-Django(장고)
---
(cf) 퓨어한 장고 프레임워크가 아니라 Django
 Ninja프레임워크를 사용할 때 적용할 수 있는 내용입니다.
 


# Djnago\-Ninja


# 제네릭 스키마를 만들고 싶은 경우


json응답을 다음과 같이 감싸고 싶은 경우가 있다.



```False
{
    "status": "success",
    "data": ...
}
```


`data`프로퍼티 안에는 장고 모델이나 특정 스키마가
 nested된 형태다.
 



 반복되는 코드를 줄이기 위해 다음과 같이 베이스 스키마
 클래스를 선언해주었다.
 



```False
from typing import TypeVar, Genric, Optional
from pydantic.generics import GenericModel

T = TypeVar("T")

class ApiResponseSchemaBase(GenericModel, Generic[T]):
    status: str
    data: Optional[T]
```


 그 후 원하는 스키마 클래스를 타입으로 지정하여 사용하면
 된다.
 


ex1\)



```False
def FooSchema(Schema):
    foo: str

foo_data = {"foo": "bar"}

@router.get('/foo', response=ApiResponseSchemaBase[FooSchema])
def get_foo(request):
    return {"status": "success", "data": foo_data} 
```


```False
// 응답 예시
{
  "status": "success",
  "data": {
    "foo": "bar"
  }
}
```

ex2\)



```False
def FooSchema(Schema):
    foo: str

foo_list = [
    {"foo": "bar1"},
    {"foo": "bar2"}
]

@router.get("/foo-list", response=ApiResponseSchemaBase[List[FooSchema]])
def get_foos(request):
    return {"status": "success", "data": foo_list}
```


```False
// 응답 예시
{
  "status": "success",
  "data": [
    {"foo": "bar1"},
    {"foo": "bar2"}
  ]
}
```
