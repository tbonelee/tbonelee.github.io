---
layout: post
title: "blur 이벤트 발생지가 window내부인지 외부인지 구분하기"
date: "2022-06-13 15:59:24 +0900"
categories:
  - 자바스크립트, 타입스크립트
---
# 상황



 리액트에서 직접 검색할 수 있는 드랍다운을 커스텀으로 구현.
 



 onBlur 속성에 핸들러를 달아서 인풋 바깥을 클릭했을 때
 드랍다운이 닫히도록 하고자 했음
 



```False
const onBlur = useCallback(() => {
  // 드랍다운 닫는 행동
}, [/* dependencies */]);

return (
  <>
  // pre-input elements...
  <input onBlur={onBlur} {...otherAttributes}/>
  // after-input elements...
  </>
);
```

# 문제점



 드랍다운에서 선택(클릭 또는 리턴 키 입력)을 하지 않고
 blur이벤트가 발생하는 경우 입력하던 input칸이 초기화되도록
 해놓은 상태.
 



 유저는 input에서 중간까지 입력하고 나머지 입력할 것이
 생각나지 않아서 다른 창으로 넘어갔다가 오는 경우가
 있는데(ex. alt\+tab) 이 때도 blur이벤트 발생해서 입력하던
 것이 초기화되었음.
 


# 적용한 방법



 window내부에서 다른 곳으로 focus가 이동한 경우
 document.activeElement가 바뀌지만 창 전환하는 경우 바뀌지
 않는 점을 이용.
 



```False
const onBlur = useCallback((e) => {
  if (document.activeElement === e.currentTarget) {
    return;
  }
  // 드랍다운 닫는 행동
}, [/* dependencies */]);

return (
  <>
  // pre-input elements...
  <input onBlur={onBlur} {...otherAttributes}/>
  // after-input elements...
  </>
);
```


 window내부에서 input 요소 외의 다른 요소를 클릭한 경우
 `document.activeElement`는 클릭한 요소가 되었고,
 blur이벤트는 input에서 발생했으므로
 `e.currentTarget`은 input요소가 되었음. 따라서 이
 때는 if문에 걸려서 return.
 



 창 전환한 경우 `document.activeElement`가
 input요소로 유지되어서 if문에 들어오게 됨으로 드랍다운을
 닫는 행동을 하지 않게 되었음.
