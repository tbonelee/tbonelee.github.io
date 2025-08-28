---
layout: post
title: "[JS]Execution Context 기본 개념"
date: "2021-10-30 17:08:04 +0900"
categories:
  - 자바스크립트, 타입스크립트
---
# JavaScript execution contexts


(참고한 것)



[In depth: Microtasks and the JavaScript runtime
 environment \- Web APIs \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide/In_depth)




[Understanding Execution Context and Execution Stack in
 Javascript](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0)





---



 자바스크립트의 코드는 하나의
 **execution context**내부에서 작동한다.
 



 다음 세 종류의 코드는 각각 새로운 execution context를
 생성한다.
 


- global context :
	- 코드의 메인 바디를 실행하기 위해 생성됨.
	- 어떠한 함수 안에 있지 않은 코드를 실행하기 위한
	 컨텍스트
- local context:
	- 각 함수를 실행하기 위한 컨텍스트
- eval function execution context:
	- eval 함수 내에서 실행되는 코드를 위한 컨텍스트


각 컨텍스트는 코드 내에서 스코프의 레벨로 볼 수 있다.



 코드의 특정 부분이 실행되어야 할 때 해당 부분을 위한 실행
 컨텍스트가 생성되고, 해당 부분이 종료되면 컨텍스트가
 파괴된다.
 



 각 컨텍스트가 생성될 때마다
 **execution context stack**에 쌓이게 된다.
 종료되면 스택에서 제거. (콜 스택 비슷한 느낌?)
 


다음의 예시를 보자.



```False
let outputElem = document.getElementById("output");

let userLanguages = {
  "Mike": "en",
  "Teresa": "es"
};

function greetUser(user) {
  function localGreeting(user) {
    let greeting;
    let language = userLanguages[user];

    switch(language) {
      case "es":
        greeting = `¡Hola, ${user}!`;
        break;
      case "en":
      default:
        greeting = `Hello, ${user}!`;
        break;
    }
    return greeting;
  }
  outputElem.innerHTML += localGreeting(user) + "<br>\r";
}

greetUser("Mike");
greetUser("Teresa");
greetUser("Veronica");
```

- 프로그램이 시작할 때 global context가 생성.
	- `greetUser("Mike")`에 도달했을 때
	 `greetUser()`함수를 위한 컨텍스트가
	 생성되고 컨텍스트 스택에 쌓인다.
		- `greetUser()`가
		 `localGreeting()`을 호출하면 또 다른
		 컨텍스트가 생성. `localGreeting()`이
		 리턴하면 스택에서 제거되고 파괴된다. 그러면
		 스택에서 남은 컨텍스트 중 가장 위에 있는
		 컨텍스트를 resume한다(여기서는
		 `greetUser()`가 된다).
		- `greetUse()`가 리턴하고 컨텍스트는
		 스택에서 제거된 다음 파괴된다.
	- `greetUser("Teresa")`에서 위와
	 비슷한 과정 반복
		- `greetUser()`에서 반복
		- `greetUser()`반환 후 같은 과정 반복
	- `greetUser("Veronica")`에서 과정
	 반복
		- `greetUser()`에서 반복
		- `greetUser()`반환 후 같은 과정 반복
- 메인 프로그램이 종료하고 실행 컨텍스트가 스택에서 제거됨.
 스택에 남은 컨텍스트가 없으므로 프로그램 실행이 끝남.



 각 실행 컨텍스트는 이런 방식으로 각각의 변수와 객체들을
 관리할 수 있고, 실행되어야 하는 코드를 효과적으로 관리할 수
 있다.
 



 재귀 함수는 컨텍스트 스택이 계속 쌓이므로 메모리를 많이
 차지할 수 있기 때문에 주의하자.
