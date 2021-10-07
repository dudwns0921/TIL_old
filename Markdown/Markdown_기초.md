# 멀티캠퍼스 Git 특강 1일차 Markdown 기초



## 1. Heading

- '#'의 개수의 따라 대응되는 수준이 있으며, h~-h6까지 표현 가능

- 문서의 구조를 위해 작성되며 글자 크기를 조절하기 위해 사용되어서는 안됨

  # h1

  ## h2

  ### h3



##  2. List

- List는 순서가 있는 리스트(ol)과 순서가 없는 리스트(ul)로 구성

  - tip) Typora에서 tab으로 하위 항목, Shift+tab으로 상위 항목

  

## 3. Fenced Code Block

- 코드 블록은 backtick 기호 3개를 활용하여 작성

  - 숫자 1 옆에 있는 기호로 따옴표와 헷갈릴 수 있으니 주의

- 코드 블록에 특정 언어를 명시하면 Syntax Highlighting 적용 가능

  - 일부 환경에서는 적용이 되지 않을 수 있음

- 키워드가 아닌 일반 문자열을 쓰고 싶을 때 사용할 수 있음

  ```java
  system.out.println("안녕하세요!");
  ```

## 4. Inline Code Block

- 코드 블록은 backtick 기호 1개를 인라인에 활용하여 작성

- Fenced Code Block과 마찬가지로 일반 문자열로 쓰고 싶을 때 사용 가능

  

## 5. Link

- `[문자열](url)`을  통해 링크를 작성 가능

  - 특정 파일들 포함하여 연결 시킬 수도 있음

  [Naver](https://naver.com)

  

## 6. Image

- Link와 사용방법이 동일하다.

  - Typora에서는 드래그&드랍 방식으로 바로 이미지를 사용할 수 있도록 해준다.

  
  ![image_example](../md-images/image1.jpg)

## 7. Backquotes (인용문)

- `>`을 통해 인용문을 작성

  >지성을 소유하고 
  >
  >또 그렇다는 것을 아는 사람은 
  >
  >그렇지 못한 열 사람에게 언제나 승리한다  
  >
  >-버나드 쇼- 

  

## 8. Table (표)

- Typora에서는 ctrl+t로 생성가능

  | 이름  | 사번 | 부서   |
  | ----- | ---- | ------ |
  | James | 0001 | dept01 |
  | Jun   | 0002 | dept01 |
  | Kevin | 0003 | dept02 |

  

## 9. Text 강조

- `**script**, __script__`  **굵게**, __bold__

- `*script*, _script_` *이탤릭체*, _기울임_

- <span style="color:red">빨간 글씨</span>, `<span style="color:red">빨간 글씨</span>`

  마크다운 문법이라기보다는 html 태그를 사용한 것임을 알아두자. 
  
  color의 값은 RGB값을 사용해도 된다.
  
  

## 10. 수평선

- 3개 이상의 asterisks(***), dashes(---), underscores(___)

  ___

  