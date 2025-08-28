---
layout: post
title: "[TS] 'satisfies' 오퍼레이터"
date: "2024-01-09 23:58:04 +0900"
categories:
  - 자바스크립트, 타입스크립트
---
[Typescript 4\.9 릴리즈 노트](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-9.html#the-satisfies-operator)  
[관련 피쳐 이슈](https://github.com/microsoft/TypeScript/issues/47920)  
[관련 PR](https://github.com/microsoft/TypeScript/pull/46827)




 어떤 expression이 원하는 타입에 속하는지 밸리데이션을
 진행하면서 동시에 기존 타입을 유지할 수 있는 오퍼레이터이다
 


예시를 통해 보자.



```False
type SomeRequest = {
  // ...
  url: URL | string
}
```


 이러한 타입에 속하는 변수를 만들어서 사용하고 싶은 경우가
 있다.
 



```False
const someRequest = {
  // ...
  url: new URL("http://test.com")
}

// someRequest.url 의 타입은 URL로 추론됨
// someRequest.url.host 접근 가능
```


 하지만 위와 같이 사용하면 프로퍼티명에서 오타가 생겨도 쉽게
 놓칠 수 있다.
 



```False
const someRequest = {
  // ...
  urL: new URL("http://test.com")
}

// `urL` should be `url`
```


`SomeRequest` 타입을 타입 어노테이션으로 사용하면
 오타를 잡을 수 있지만 `url` 프로퍼티의 타입을
 보다 일반적인 `SomeRequest.url` 타입으로 다시
 확장시키는 효과를 갖게 된다.
 



```False
const someRequest: SomeRequest = {
  // ...
  urL: new URL("http://test.com")
  // 에러 메시지: Object literal may only specify known properties, but 'urL' does not exist in type 'SomeRequest'. Did you mean to write 'url'?
}
```


```False
const someRequest: SomeRequest = {
  // ...
  url: new URL("http://test.com")
}

// 에러 메시지: Property 'host' does not exist on type 'string | URL'.  
// 에러 메시지: Property 'host' does not exist on type 'string'.
someRequest.url.host 
```


 이 때 `satisfies` 오퍼레이터를 사용하면
 specific한 타입은 유지하면서 타입 밸리데이션도 할 수 있다.
 



```False
const someRequest = {
  // ...
  urL: new URL("http://test.com") // 에러 메시지: Object literal may only specify known properties, but 'urL' does not exist in type 'SomeRequest'. Did you mean to write 'url'?
}
```


```False
const someRequest = {
  url: new URL("http://test.com")
} satisfies SomeRequest

someRequest.url.host; // 타입 에러 발생 x
```
