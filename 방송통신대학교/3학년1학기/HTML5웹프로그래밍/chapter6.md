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
  - text-align : justify 로 지정된 요소에서만 동작

##### text-indent

- 텍스트 블록에서 첫 번째 줄의 들여쓰기 지정
  - 값
    - 길이
    - %, 부모 요소의 폭에 대한 백분율

##### text-transform

- 텍스트의 영어 알파벳 표기 방식 지정
- 값
  - none
  - capitalize
  - uppercase
  - lowercase

##### text-decoration-line

- 텍스트 장식과 관련된 선의 종류 지정
  - 값
    - none
    - underline
    - overline
    - line-through
  - 하나 이상의 속성값을 동시에 사용 가능

##### text-decoration-style

- 선의 스타일 지정
- 값
  - solid
  - double
  - dotted
  - dashed
  - wavy

##### text-decoration-thickness

- 선의 두께 지정

##### text-decoration

- line의 기본값이 none이기 때문에 line의 값은 필수로 지정해주어야 한다.

##### text-shadow

- 텍스트에 그림자 효과 지정
- 값
  - none
  - 수평위치 수직위치 흐림정도 색상
  - 여러 그림자 생성 가능, 각 그림자는 콤마로 구분

##### text-overflow

- 요소 박스 영역을 벗어난 텍스트의 표시 방식 지정
- 값
  - clip
  - ellipsis
- 아래 두 속성을 함께 사용해야 함
  - white-space
    - 요소 내의 공백문자의 처리 방식
    - 값
      - nowrap
        - 연속된 공백은 하나의 공백으로 처리
        - 줄바꿈문자를 만나기 전까지는 한 줄로 처리
  - overflow
    - 값
      - hidden
      - scroll
      - auto

##### word-wrap

- 단어가 길어서 요소 폭을 넘을 때 단어를 분리해서 줄바꿈을 수행할지 여부 지정
- 값
  - normal
  - break-word

##### vertical-align

- 요소의 수직 정렬 방식 지정
- 값
  - 길이
    - 음수 가능, %
    - 고정값은 책 확인

### 리스트

항목 마커 설정 가능

##### list-style-type

- 항목 마커의 종류 지정
- ul
  - 값
    - none
    - disc
    - circle
    - square
- ol
  - 값
    - decimal 등등

##### list-style-position

- 항목 마커의 위치 지정
- 값
  - inside
  - outside

##### list-style-image

- 이미지를 항목 마커로 지정
- 값
  - none
  - url(이미지파일)

##### list-style

- 일괄 지정

##### ul 원하는 마커로 바꾸기

```css
ul {
    list-style: none;
    padding: 0;
    margin: 0;
}
li {
    padding-left: 16px;
}
li::before {
    content: '※';
}
```

### 테이블

##### border

- table, td, th 요소에 대한 테두리 지정

##### table-layout

- 셀 안의 내용의 크기에 따른 셀의 크기 변화 여부 지정
- 값
  - auto
    - 셀의 폭이 셀의 내용 크기 따라 자동으로 결정
  - fixed
    - 셀의 내용 크기와 상관없이 테이블/셀의 폭에 의해서만 결정

##### border-collapse

- 테이블 테두리와 셀 테두리를 하나로 합칠지 여부 지정
- 값
  - separate
  - collapse
  - border-spacing 속성과 empty-cells 속성은 무시됨

##### border-spacing

- 인접한 셀 테두리 사이의 간격 지정
- 값
  - 수평거리 수직거리
  - 하나의 값만 지정되면 수평거리와 수직거리는 동일

##### caption-side

- 테이블 캡션의 위치
- 값
  - top
  - bottom

##### empty-cells

