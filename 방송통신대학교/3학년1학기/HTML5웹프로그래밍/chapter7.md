# HTML5웹프로그래밍

## 7강. CSS: 박스 모델, 테두리

### 박스 모델

#### CSS 박스 모델

- 모든 HTML 요소를 사격형 형태의 박스로 취급
  - CSS를 통해 각 박스의 위치, 크기, 색상 등을 지정
    - 웹 페이지의 레이아웃을 구성하는 중요한 개념

요소

- 내용
- 패딩
  - 요소의 내용과 테두리 사이의 여백 지정
  - 설정하는 건 마진과 동
- 테두리
- 마진
  - 박스의 외부 여백 지정
  - 모든 방향에 대해서 일괄 지정
  - % 는 요소의 폭에 대한 백분율
  - 속성값의 개수에 따른 마진의 적용 방향이 달라짐
  - 인접한 두 요소의 수직 여백은 통합됨
    - bottom
    - top
    - 더 큰 값으로 합쳐짐
- 위 4가지 요소로 구성되며 이들을 모두 합친 너비와 세로를 각각 width, height라고 한다.

#### 요소의 크기: width, height 속성

- 요소의 콘텐츠가 표시되는 영역의 폭과 높이 지정
  - 값
    - 길이
    - %
    - auto
  - 하나의 속성만 지정되면 나머지 하나는 자동으로 크기 결정

#### 크기 제한 속성

- min-width
- min-height
- max-width
- max-height
- 콘텐츠 영역의 최소 폭, 최소 높이, 최대 폭, 최대 높이 지정

#### 요소의 위치 : position 속성

- 요소 배치의 기준이 되는 위치(요소의 위치 설정 방식) 지정
  - 값
  - static
    - 페이지의 정상적인 흐름에 따라 현재의 위치가 요소의 위치가 됨
    - 별도의 위치 지정/변경 불가
    - top, bottom 등의 속성값 무시
  - absolute
    - 브라우저의 왼쪽 상단 모서리를 기준으로 지정한 위치만큼 이동해 요소를 배치
  - relative
    - 해당 요소의 현재 위치를 (0,0) 으로 정하고 이를 기준으로 지정한 위치만큼 이동해서 요소를 배치
  - fixed
    - 현재 윈도우를 기준으로 지정한 위치만큼 이동해 지정
    - 스크롤해도 움직이지 않음

#### 요소의 위치 : top, bottom, left, right 속성

- 요소를 포함하는 박스의 각 변으로부터 해당 요소가 떨어져 있는 거리 지정
  - 값
    - auto
    - 길이
    - %
  - relative
    - 상위 요소가 없을시 static인 경우의 위치에서 거리 지

#### 요소의 위치 : z-index 속성

- 요소 간의 겹쳐지는 순서 지정
  - 값
    - auto
    - 정수
  - position relative, absolute, fixed인 경우에만 적용

#### 화면 표시 : display 속성

- 요소를 어떻게 표시할 지를 지정
  - 값
    - none
    - inline
    - block
    - inline-block
      - `width`와 `height` 속성 지정 및 `margin`과 `padding` 속성의 상하 간격 지정이 가능
    - list-item

#### 화면 표시 : overflow 속성

- 콘텐츠가 요소 박스를 벗어나는 경우의 처리 방법 지정
- height 속성이 지정된 블록 요소에 대해서만 적용 가능
- 값
  - visible
  - hidden
  - clip
  - scroll
  - auto
- overflow-x, overflow-y 를 통해 수평, 수직 방향으로 구분해 오버플로의 처리 방법을 지정할 수 있음

#### 화면 표시 : visibility 속성

- 값
  - visible
  - hidden
    - 화면에 표시하지 않지만, 페이지에서 원래 공간은 그대로 차지
  - collapse
    - hidden과 동일
    - 단, 테이블 요소의 경우에는 행/열 제거

#### 플로팅 : float 속성

- 콘텐츠의 일반적인 흐름을 벗어나서 부모 요소 영역을 기준으로 해당 요소를 왼쪽/오른쪽에 배치하도록 지정

#### 플로팅 : clear 속성

- float 속성의 영향을 받는 요소의 흐름을 해제하여 바로 아래쪽에 요소를 배치하도록 지정

#### 박스의 크기 : box-sizing

- width / height 속성을 사용하여 요소의 폭과 높이를 지정할 때 패딩과 테두리를 포함시킬지의 여부 지정
- 값
  - content-box
  - border-box

#### 박스의 크기 : resize 속성

- 사용자가 요소 박스의 크기를 조절할 수 있도록 지정
- 값
  - none
  - both
  - horizontal
  - vertical
- overflow
  - hidden, scroll, auto 속성과 함께 사용해야 함.

### 테두리

#### boder 관련 속성

- border-width
- border-style
- border-color
- 속성값의 개수에 따라서 마진과 패딩처럼 적용된다.

#### border-radius

- 요소 박스의 각 모서리를 둥글게 지정

#### box-shadow

- 박스 요소에 그림자 스타일 지정
