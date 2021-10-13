# CSS

flex의 세계에는 두 가지가 있다.

\1. row(행): 가로를 의미한다.
\2. column(열): 세로를 의미한다.

flex container의 flex-direction 기본 값은 row다. 이때 해당 row에 있는 item의 위치를 변경시키기 위해서는 justify-content를 사용하는데 수평 축에 있는 flex children의 위치를 변경한다. (flex container에서는 부모가 자식의 위치를 변경한다는 것이 저번 수업의 핵심 내용이었다.)

이제 수평 축에 main-axis라는 멋드러진 이름을 붙여주자. 이 이름을 적용시켜 위 예시를 다시 생각해보면 다음과 같다.

flex-direction의 기본 값이 row일 때 수평축이 곧 main-axis다. 다시 말해 가로가 곧 main-axis인 것이다.
이때 main-axis에서 justify-content를 사용하면 item을 움직일 수 있는 것인데 main-axis가 수평축, 가로이기 때문에 가로로 item이 움직인다.

다른 axis로는 cross-axis가 존재한다. flex container가 row을 가질 때 cross-axis는 단어 그대로 가로지르기 때문에 수직(vertical)이다.
이때 cross-axis에서 item을 움직이기 위해서는 align-content를 사용한다. 수직, 세로로 움직이는 것이다.

이는 차후 direction을 배울 때 매우 중요하므로 main-axis와 cross-axis 용어 및 개념을 잊지 말자.

align-center를 사용할 때는 반드시 flex container(부모)의 높이가 어느 정도 되는지 반드시 확인하자. 이미 item과 높이가 동일하여 위치를 변환시킬 수 없는 경우가 많기 때문이다.



flex direction 의 기본방향은 row (가로)
row방향일때 : main axis =가로(justify-content) / Cross axis=세로(align-items)
column방향일때 : main axis=세로(justify-content) / Cross axis=가로(align-items)



부모가 아닌 자식 아이템의 위치(position)를 직접 변경하고 싶을 때는 align-self와 order를 사용한다. 이때 유의할 점은 부모의 높이(heigth)가 설정되어 있어야 한다.

align-self는 align-item과 같이 동작한다. 다시 말해 cross axis 방향에 있는 item의 위치를 바꾸게 된다.

order의 경우 단어 그대로 순서를 바꾼다. 이때 기본 값(default)는 0이라 order를 1로 줄 경우 order를 주지 않은 것보다 뒤에 위치하게 된다. HTML을 통해 순서를 바꾸기 힘들 때 사용하면 좋다.



1.5 wrap, nowrap, reverse, align-content
\- father, child 모두 flex로 해주면 child에 있던 글자가 중앙에 위치하게 됨

\1) flex-wrap
\- flexbox는 width보다도, '같은 줄'에 있도록 하는 것을 우선시함
-> flex-wrap: wrap; (child의 사이즈를 유지하라고 하는 것)
-> nowrap; (child를 모두 같은 줄에 정렬함, 이때 width가 줄어들 수 있음, 기본값)

\2) reverse
\- flex-direction: row-reverse; (오른쪽에서부터 1이 시작)

방향이 반대로

\- column-reverse;
\- flex-wrap: wrap-reverse; (한 줄이 되지 않아도 아래에서 위로 정렬되게)

\* wrap으로 정렬 시 (여러 줄으로, 각 item의 width를 유지하면서)
각 줄(기본: row) 간의 간격이 생기는데, 이것을 'align-content'라는 property로 조절 가능

\3) align-content (line을 변경, line은 cross-axis에 있는 상태 / 만약 flex-direction이 기본값인 row이라면)

`align-content`를 사용하여 여러 줄 사이의 간격을 지정할 수 있습니다. 이 속성은 다음의 값들을 인자로 받습니다:

- `flex-start`: 여러 줄들을 컨테이너의 꼭대기에 정렬합니다.
- `flex-end`: 여러 줄들을 컨테이너의 바닥에 정렬합니다.
- `center`: 여러 줄들을 세로선 상의 가운데에 정렬합니다.
- `space-between`: 여러 줄들 사이에 동일한 간격을 둡니다.
- `space-around`: 여러 줄들 주위에 동일한 간격을 둡니다.
- `stretch`: 여러 줄들을 컨테이너에 맞도록 늘립니다.

flexbox는 화면 안에 오직 한 줄에 있도록 유지한다

너비가 바뀌더라도



\1) flex-shrink: flexbox가 쥐어짤 때, element의 행동의 정의함
ex) flex-wrap: nowrap일때, 화면이 작아지면 width가 설정되어있어도 줄어들지.
\- flex-shrink: 1; - flex-shrink: n(정수); --> 여러 개 element 중 특정 element만 덜 줄어들거나, 더 줄어들게 할 수 있음!!

상대적으로 작용

가령 어떤 박스의 flex-shrink가 2라면 다른 박스에 비해 2배 줄어들게 됨



\2) flex-grow: shrink와 반대, 화면이 늘어남에 따라 box 크기가 얼마나 늘어날까?
\- flex-grow: 1; - flex-grow: 0; - 남아있는 공간을 가져옴 (space를 없애고)
-> 즉, 남아있는 공간, 여백이 있을 때만 grow 가능

=> 화면이 커질 때, element도 함께 커지길 원할 때 사용할 수 있음
=> flex-grow property가 0인 상태거나, 따로 정의되지 않았다면, 화면이 커져도 각 element 크기가 커지지 않음
(여백만 늘어나게 됨)

상대적으로 작용





반응형 디자인을 할 때 유용하다



flex basis

width와 같다고도 말할 수 있음

element에 처음 크기를 줌 찌그러지거나 늘어나기 전에

실제 크기가 아님

flex-grow와 shrink 때문에



flex-direction에 따라 작용하는 게 달라짐

row일 때는 width로 작용

column일 때는 height로 작용

main axis 쪽 크기

