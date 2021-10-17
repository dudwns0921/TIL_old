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



● align-self
● justify-self
● place-self: (수직) (수평);
▷ child에만 적용돠는 property이다.



emmet abbrevation

.item*20>{$}

20개의 item이라는 클래스를 가지는 div를 만들고 그 안을 연속되는 숫자로 채움



설정한 그리드보다 더 많은 요소를 가져오게 될 수도 있다.

그렇게 되면 설정된 row가 없기 때문에 그리드는 알아서 새로운 row들을 만든다. 이 때 설정된 값이 없으므로 요소들의 내용만큼의 크기로 생성됨



그래서 사용하는 것이 아래

더많은 콘텐츠가 있으면 우리가 row를 지정해주지 않아도 자동으로  생성

초과하는 콘텐츠에 대해서 적용됨

만약 row를 정의하지 않는다면 모든  row에 적용됨

얼마나 많은 요소들을 갖는지 상관없을 때



● grid-auto-rows: (크기);
▷ 만들어놓은 row보다 더 많은 content가 있으면, 자동으로 row를 만들어라.

● grid-auto-flow: (방향); [기본값: row]
▷ flex-direction과 비슷하다.
▷ row가 끝날 때 새로운 row를 만들지, 새로운 column을 만들지 결정한다.

● grid-auto-columns: (크기);
▷ grid-auto-flow: column;일때 작동한다.



[minmax]
\- Grid-template-columns: repeat(10, minmax(100px, 1fr)); //최대 1fr로 하되 최소 100px너비

요소가 가능한한 가장 크기를 바라지만 동시에 엄청 작게 되지 않기를 바랄 때 사용 가능.



auto-fill은 화면상에서 가능한한 많은 column을 갖게 해줌

column이 비게 되더라도

auto-fit

현재 요소들이 빈 공간없이 채워지는 것



새로운 요소가 추가됐을 때

auto fill은

column중 빈 공간을 요소에게 줌

더 정확 

auto fit은 row안에 맞추기 위해 다른 요소들의 크기가 조금 줄어들게 됨

더 유동적



[auto-fill, auto-fit]
\- Grid-template-columns: repeat(auto-fill, minmax(100px, 1fr)); //창 너비가 늘어나면 빈 column들로 row를 채움
\- Grid-template-columns: repeat(auto-fit, minmax(100px, 1fr)); // 창 너비가 늘어나면 element를 늘려서 row에 맞게 해줌

● max-content
▷ content의 크기만큼 cell의 크기를 늘린다.
● min-content
▷ content의 크기를 최대한 줄여 cell의 크기를 줄인다.

※ 어떻게 content가 보여야 하는지 디자인하는 것이다.
※ repeat(), minmax와 결합하여 반응형 디자인을 만들 수 있다.CSS GRID

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



● align-self
● justify-self
● place-self: (수직) (수평);
▷ child에만 적용돠는 property이다.



emmet abbrevation

.item*20>{$}

20개의 item이라는 클래스를 가지는 div를 만들고 그 안을 연속되는 숫자로 채움



설정한 그리드보다 더 많은 요소를 가져오게 될 수도 있다.

그렇게 되면 설정된 row가 없기 때문에 그리드는 알아서 새로운 row들을 만든다. 이 때 설정된 값이 없으므로 요소들의 내용만큼의 크기로 생성됨



그래서 사용하는 것이 아래

더많은 콘텐츠가 있으면 우리가 row를 지정해주지 않아도 자동으로  생성

초과하는 콘텐츠에 대해서 적용됨

만약 row를 정의하지 않는다면 모든  row에 적용됨

얼마나 많은 요소들을 갖는지 상관없을 때



● grid-auto-rows: (크기);
▷ 만들어놓은 row보다 더 많은 content가 있으면, 자동으로 row를 만들어라.

● grid-auto-flow: (방향); [기본값: row]
▷ flex-direction과 비슷하다.
▷ row가 끝날 때 새로운 row를 만들지, 새로운 column을 만들지 결정한다.

● grid-auto-columns: (크기);
▷ grid-auto-flow: column;일때 작동한다.



[minmax]
\- Grid-template-columns: repeat(10, minmax(100px, 1fr)); //최대 1fr로 하되 최소 100px너비

요소가 가능한한 가장 크기를 바라지만 동시에 엄청 작게 되지 않기를 바랄 때 사용 가능.



auto-fill은 화면상에서 가능한한 많은 column을 갖게 해줌

column이 비게 되더라도

auto-fit

현재 요소들이 빈 공간없이 채워지는 것



새로운 요소가 추가됐을 때

auto fill은

column중 빈 공간을 요소에게 줌

더 정확 

auto fit은 row안에 맞추기 위해 다른 요소들의 크기가 조금 줄어들게 됨

더 유동적



[auto-fill, auto-fit]
\- Grid-template-columns: repeat(auto-fill, minmax(100px, 1fr)); //창 너비가 늘어나면 빈 column들로 row를 채움
\- Grid-template-columns: repeat(auto-fit, minmax(100px, 1fr)); // 창 너비가 늘어나면 element를 늘려서 row에 맞게 해줌

● max-content
▷ content의 크기만큼 cell의 크기를 늘린다.
● min-content
▷ content의 크기를 최대한 줄여 cell의 크기를 줄인다.

※ 어떻게 content가 보여야 하는지 디자인하는 것이다.
※ repeat(), minmax와 결합하여 반응형 디자인을 만들 수 있다.