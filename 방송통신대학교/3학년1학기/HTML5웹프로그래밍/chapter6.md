# HTML5웹프로그래밍

## 5강. CSS: 개요, 선택자, 색상

### 글꼴

#### 글꼴 관련 속성

##### font-family

- 글자체 지정
  - 값
    - 폰트패밀리
    - 일반 폰트명
    - 값을 작성할 때, 중간에 공백이 있을 때만 따옴표 사용
    - 가장 앞에 있는 값으로 지정됨

##### font-size

- 글자 크기 지정
  - 값
    - 절대크기 키워드
    - 상대크기 키워드
    - 절대크기 단위
      - em
        - 부모 요소의 글자 크기가 1em
    - 상대크기 단위

##### font-style, font-variant

- 글자 모양 지정
  - 값
    - normal
    - italic
    - oblique
- 작은 크기의 대문자로의 변환 여부
  - 값
    - normal
    - small-caps

##### font-weight

- 글자의 굵기 지정
  - 값
    - normal(=400)
    - bold(=700)
    - bolder
    - lighter
    - 100...

##### font

- 글꼴 관련 속성을 한꺼번에 지정하는 속성
- 사용 방법은 책 참고
  - size, family는 필수
  - line-height 값은 앞에 '/' 작성 필요

##### @font-face 규칙

- 사용자 정의 글꼴을 사용하기 위한 것
  - 사용 가능한 글꼴 종류
    - TTF
    - OTF
    - WOFF
  - 등록방법은 책 참고

### 텍스트

#### 텍스트 관련 속성

##### letter-spacing, word-spacing

- 문자 사이의 간격
- 단어 사이의 간격
- 값
  - normal
  - 길이
  - 음수 가능, 서로 겹치게 할 수 있음

##### line-height

- 문장의 줄 간격 지정
  - 값
    - normal
    - 길이
    - 숫자
      - 현재 글꼴 크기의 상수배
    - %
      - 현재 글꼴 크기에 대한 백분율

##### text-align

- 텍스트의 수평 정렬 방식 지정
  - 값
    - left
    - right
    - center
    - justify
      - 양쪽 정렬

##### text-align-last

- 텍스트의 마지막 줄의 정렬 방식 지정
  - 값
    - auto
    - right
    - center
    - justify
    - start
    - end
  - text-align : justify 로 지정된 요소에서만 동

### 리스트

### 테이블
