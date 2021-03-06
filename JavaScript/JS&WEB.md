# css 다루기  
Cascading Style Sheet  
* HTML 문서의 요소가 어떻게 표시될지 정의  
  
## 문법  
[selector] { [property_name]:[value]; ...}  
* selector : tag 이름, id, calss로 작성 가능  
- tag 이름 :태그 이름 그대로 사용 (h1, h2, p, img, ...)  
- id :#을 붙이고 id 사용(#name, #id, ...)  
- class : .을 붙이고 class 이름 사용 (.lyric, ...)  
* property_name : selector에 해당하는 엘리먼트의 style 속성  
* valule : 앞에 정의한 style property의 값  
  
  
## 적용하기  
1. head 태그 안에 <link> 태그 사용  
  <link rel = "stylesheet" href = "[css file]">  
2. head 태그 안에 <style> 태그를 사용해 직접 정의  
3. html element 안에 직접 속성으로 정의  
  <p style = "color:red"> .... </p>  
  
## CSS 적용 우선 순위  
1. html element 안에서 적용한 스타일  
2. style tag 안에서 적용한 스타일  
3. link를 통해 외부 파일에서 적용한 스타일  
  
  