# CSS GRID

grid도 flexbox와 마찬가지로 부모 요소에서 적용된다.

display: grid 라고 설정하면 됨

flexbox랑 거의 비슷

부모 요소에서 작업하게 됨

grid-template-colums column을 생성하고 px로 그 column의 너비를 설정

row는 가로, row의 height 높이를 px로 설정

설정한 크기 개수에 따라 column 수가 결정됨

column-gap으로 세로 간격 조정 가능

row-gap으로 가로 간격 조정 가능



CSS grid가 가진 함수

repeat로 모든 칼럼과 로우의 길이를 나열할 필요없이 한 번만 작성해주면 된다.

ex) repeat(4, 200px)



grid template areas

각 요소의 grid-area에 있는 값과 

gird template areas에 적는 값이 같아야 한다.

클래스 이름과는 상관없음

grid area의 값은 String이 아니어야 한다.

변수처럼 ""이 없음



grid-column start

grid-column end

줄을 기준으로 한다

그래서 1번부터 2번까지해도 달라지는 건없다

1번부터 3번으로 하면 맨 처음에 생각했던 결과가 나온다.

 area를 전부다 작성하는 것보다는 편한 방법



  grid-column-start: 1;

  grid-column-end: 5;

=

grid-column: 1/5;

더 간결하게 작성하는 방법



1부터 시작해서 5에서 끝난다고 하는 것보다는

시작부터 끝이라고 작성하는 게 더 간편

-1이 끝의 의미를 가지고 있음

grid-column: 1/-1;

-2,-3

끝에서부터 한 칸씩 전진



span 속성값

몇 칸의 cell을 차지하는지 작성

이경우에는 4개

수직, 수평 모두 작용



섞어서도 사용가능

가령 grid/column: 1/span2;

첫 번째 줄에서 시작해서 두 칸 차지



라인 네이밍

grid-template-columns: [first-line] 100px [second-line] 100px [third-line] 100px [fourth-line] 100px;

  grid-template-rows: repeat(4, 100px [sexy-line]);

모두 같은 라인으로 이름 붙일 경우에는 뒤의 숫자로 구분

이건 결국 숫자로 구분하는 거랑 큰 차이가 없는듯



fraction

사용가능한 공간을 의미

그리드 상에서의 공간을 뜻한다

grid-template-columns: repeat(4, 1fr);

이렇게 하면 설정된 그리드의 너비를 4공간으로 나눈다는 의미

너비를 설정하지 않으면 바디의 넓이를 따라간다.

grid-template-columns: 4fr 1fr 1fr 1fr;

이런 식으로 나누면 비율로 적용되어 첫 번째 칸이 나머지 칸들보다 4배의 공간을 더 갖게 됨

하지만 row를 설정하려면 그 전에 꼭 height를 설정해야 한다.



다른 방법보다 fr을 사용하는 걸 추천!



grid-template

슈퍼 지름길

지름길 중 가장 위

  grid-template:

  "header header header header" 1fr

  "content content content nav" 2fr

  "footer footer footer footer" 1fr / 1fr 1fr 1fr 1fr;



뒤의 길이는 높이 먼저

grid 템플릿에서는 repeat 함수가 적용되지 않는다.

어떻게 만들든 화면의 비율이 같다

라인 네이밍도 적용 가능



마지막에 슬래쉬 이후 각각의 column의 너비를 설정한다.



● justify-items
● align-items
● place-items: (수직) (수평);

▷ stretch : grid를 늘려서 grid를 채우게 한다.
▷ start : item을 cell 시작에 배치한다.
▷ center : item을 cell 중앙에 배치한다.
▷ end : item을 cell 끝에 배치한다.



[place items – justify-items, align-items]
\- Justify-items와 align-items로 그리드 내부에 아이템이 얼만큼 채워질지 제어할 수 있다.
\- 기본 값은 stretch로 그리드를 꽉 채우는 것으로 한다.
\- Place-items를 쓰면 justify-items와 align-items를 통합
 Place-items: stretch center; /*첫번째가 수직, 두번째가 수평*/