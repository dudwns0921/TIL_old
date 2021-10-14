# Flexbox

## flexbox란?

flexbox는 뷰포트나 요소의 크기가 불명확하거나 동적으로 변할 때에도 효율적으로 요소를 배치, 정렬, 분산할 수 있는 방법을 제공하는 CSS3의 레이아웃 방식

flexbox의 장점을 한 마디로 표현하면 '복잡한 계산 없이 요소의 크기와 순서를 유연하게 배치할 수 있다'라고 할 수 있다. 



## flexbox의 구성

flexbox는 복수의 자식 요소인 flex item과 그 상위 부모 요소인 flex contatiner로 구성된다.

flexbox는 정렬하려는 요소의 부모 요소에 display: flex 속성을 선언하는 것을 통해 만들 수 있다.

![flexbox](./md-images/flexbox.png)	

그림에서와 같이 flex container에는 두 가지 축이 존재한다.

1. row : 행, 가로
2. cloumn : 열, 세로

flex item은 main axis에 따라 정렬되는데, main axis의 방향은 flex container의 flex-direction 속성으로 결정된다.

flex-direction의 기본값은 row으로 flex container에 flex 속성을 선언했을 때 요소들은 가로로 정렬된다.



## flexbox 속성

 flexbox에서 사용하는 속성은 부모 요소인 flex container에 정의하는 속성과 자식 요소인 flex item에 정의하는 속성으로 나누어진다.

전체적인 정렬이나 흐름에 관련된 속성은 flex container에 정의하고, 자식 요소의 크기나 순서에 관련된 속성은 flex item에 정의한다. 



| 부모 속성(flex container) | 자식 속성(flex item) |
| ------------------------- | -------------------- |
| flex-direction            | align-self           |
| justify-content           | order                |
| align-items               | flex-shrink          |
| flex-wrap                 | flex-grow            |
| align-content             | flex-basis           |

### justify-content

main axis에 따라 flex item들을 배치한다.



### align-items

cross axis에 따라 flex item들을 배치한다.

align-items를 사용할 때는 반드시 flex container의 높이가 어느 정도 되는지 반드시 확인하자.

이미 item과 높이가 동일하여 위치를 변환시킬 수 없는 경우가 많기 때문이다.



위에서도 말했지만 flex-direction 속성에 따라 main axis와 cross axis의 방향이 달라진다.

- #### flex-direction: row

main axis = 가로(justify-content) | cross axis = 세로(align-items)

- #### flex-direction: column

main axis = 세로(justify-content)  | cross axis = 가로(align-items)



### flex-wrap

flexbox는 기본적으로 flex item들이 같은 줄에 있도록 한다.

만약 뷰포트[^1]가 flex item들의 너비보다 작아진다면 flexbox는 flex item들을 한 줄에 배치하기 위해 이들의 너비를 줄인다.

이 때 flex-wrap 속성을 통해 이러한 flexbox의 기본적인 특성을 변경할 수 있다.

- #### flex-wrap: nowrap

기본값, flex item들을 한 줄에 배치

- #### flex-wrap: wrap

flex item의 크기를 유지하며 한 줄에서 벗어난다면 여러 줄로 나눠서 배치한다.



### align-content

flex-wrap을 사용했을 때와 같이 flex item들이 여러 줄에 배치되었을 때 그 줄들을 조정하는 속성이 바로 align-content이다.

이 속성은 한 줄로만 이루어진 flex container에는 아무런 효과가 없다.



___

flex container가 아닌 flex item의 위치나 크기를 직접 변경하고 싶을 때는 아래 속성들을 이용한다.



### align-self

align-items과 같이 cross axis에 따라 flex item의 위치를 설정할 수 있는 속성이다.



### order

단어 그대로 flex container 안의 flex item들간의 순서를 설정한다. 

기본값은 0이고, 상대적으로 적용된다.

HTML을 통해 순서를 바꾸기 힘들 때 사용하면 좋다.



### flex-shrink

위에서 말했던 것처럼 뷰포트의 크기가 작아져 flex item들이 작아질 때 그 요소들의 크기를 설정하는 속성

이 속성은 요소들에 대하여 상대적으로 적용된다.

가령 어떤 요소의 flex-shrink가 2라면 다른 요소에 비해 2배 줄어들게 된다.



### flex-grow

flex-grow: shrink와 반대로 뷰포트가 늘어나 빈 공간이 생길 때 요소들이 어떻게 늘어날지 정의한다.

이 속성도 요소들에 대하여 상대적으로 적용된다.

가령 어떤 요소의 flex-grow가 1이고 다른 요소의 flex-grow가 2라면,

남은 공간을 3으로 나눈 후 flex-grow가 1인 쪽에 여백의 3분의 1만큼 늘어나게 하고 flex-grow가 2인쪽에 3분의 2만큼 늘어나게 한다.



### flex-basis

뷰포트에 따라 크기가 변하기 전의 요소의 크기를 main axis에 따라 설정

flex-direction이 row일 때는 가로의 크기를, column일 때는 세로의 크기를 나타내게 된다.

사실 width가 대체할 수 있기 때문에 많이 사용하는 속성은 아니다.



# :bulb:Tip!

flexbox 사용법을 익히고 싶다면 아래 사이트를 이용해보자.

https://flexboxfroggy.com/#ko



# :books:참고자료

https://d2.naver.com/helloworld/8540176

노마드코더 css layout master class 수업 내용



# 각주

[^1]: 웹페이지가 브라우저 화면상에서 실제로 표시되는 영역
