# HTML5웹프로그래밍

## 8강. CSS: 배경, 그라데이션, 변형

### 배경

#### 배경 관련 속성

- background-color
  - 기본값은 transparent
- background-image
  - 기본값은 none
  - 콤마로 구분해서 여러 개의 이미지 지정이 가능
  - 순서대로 계층을 형성한다.
- background-repeat
  - 기본값은 repeat
  - space
    - 배경을 채우는 영역의 경계에서 이미지가 잘려 보이지 않도록 반복 이미지 간의 공백 조정
  - round
    - 배경을 채우는 영역의 경계에서 이미지가 잘려 보이지 않도록 이미지의 크기를 재설정
- background-attatchment
  - 배경 이미지를 콘텐츠 영역과 함께 스크롤 되도록 할 것인지 지정
- background-position
  - 배경 이미지의 시작 위치 지정
    - 기본 위치
      - 요소의 왼쪽 상단 모서리
      - xpos, ypos 에서 하나의 값만 지정되면 다른 하나는 center(50%)로 취급
      - %로 지정하면, 배경 이미지의 x축, y축 %대로 기준점이 이동한다.
      - 원래는 왼쪽 상단 모서리가 기준점
- background-origin
  - 배경 이미지가 시작하는 기준 위치 지정
    - padding-box
      - 패딩을 기준으로 왼쪽 모서리 위
    - border-box
      - 테두리를 기준으로 왼쪽 모서리 위
    - content-box
      - 콘텐츠를 기준으로 왼쪽 모서리 위
  - background-attatchment 속성이 fixed로 지정되면 동작하지 않음
- background-size
  - 값
    - auto
    - cover
      - 이미지의 폭과 높이 중에서 길이가 짧은 것을 기준으로 영역을 채움
      - 원래 크기 비율 무시
    - contain
      - 이미지의 폭과 높이 중에서 길이가 긴 것을 기준으로 영역을 채움
        - 원래 크기 비율 유지
    - 폭, 높이
    - 폭 높이중 하나만 설정되면 나머지 하나는 auto로 설정
- background-clip
  - 배경 속성이 적용되는 영역 지정
    - 값
      - border-box
      - padding-box
      - content-box
      - background-origin의 기본값이 border-box이기 때문에 border-box로 background-clip을 지정하면 border에 있는 이미지가 잘림
- background
  - background 속성의 일괄 지정
  - 각 속성은 생략 가능
  - background-size 속성값을 사용할 때는 반드시 '/' 사용
  - 이미지 파일을 여러 개 사용하면서 배경색을 지정하는 경우에는 background-color 속성값을 리스트의 맨 뒤에 위치시킴
  - 한 번에 여러 개의 이미지를 지정하는 경우에는 콤마로 구분

### 그라데이션

- 두 개 이상의 색상 사이에서 색상의 점진적인 변화

  - 선형
    - linear-gradient(진행방향, 색상1, 색상2)
    - 여러 색을 지정하면 균일하게 변화
    - 퍼센트를 사용해 각 색의 시작점을 지정할 수 있음
    - repeating을 앞에 붙여서 반복하게 할 수 있음
  - 방사형
    - radial-gradient
      - shape size **at** postion, 색상1, 색상2
      - shape
        - ellipse
        - circle
      - size
        - 중심을 기준으로 했을 때 size 값이 정해짐
        - closest-side
        - farthest-side
        - closest-corner
        - farthest-corner - 기본값
      - position
        - 중심의 위치
  - 원뿔형
    - conic-gradient
    - from 시작각도 at 중심위치, 색상1, 색상2

  ### 변형

- 요소의 형태, 크기, 위치를 변경시키는 효과

  - 이동
    - translate
      - 요소의 중앙점을 기준으로 좌표축을 따라 지정한 거리만큼 이동
      - 
  - 회전
    - rotate
  - 크기 변경
    - scale
      - 요소의 중앙점을 기준으로 확대 또는 축소
  - 기울임
    - skew

- transform
  - 요소에 대한 변형 지정
  - 여러 개의 변형 함수 사용 가능
    - 앞에서 살펴본 것들이 변형 함수
    - 이를 한꺼번에 적용시키는 함수가 matrix

- transform-origin
  - 요소 박스에 대한 변형 기준점 지정

- perspective
  - 3차원 공간에서 해당 요소와 관측점과의 거리 지정
    - 부모 요소에서 적용해 자식 요소에 원근감 부여
- perspective-origin
  - 기준점 지정

- backface-visibility
  - 요소 뒷면의 표시 여부 지정
