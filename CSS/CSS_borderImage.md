# CSS

## border-image 활용하기

![](md-images/2022-04-29-16-27-14-image.png)

작업 도중 위와 같은 레이아웃을 구현해야 하는 일이 생겼다. border와 box-shadow가 모두 적용된 형태였는데, 특징은 다음과 같다.

1. border의 color를 linear-gradient로 처리

2. box-shadow를 오른쪽에만 적용

box-shadow를 오른쪽에만 적용하는 건 그렇게 어렵지 않았다.

```css
  box-shadow: 30px 0px 70px -70px #707070;
```

실제 코드는 위와 같다. 마지막 인수인 spread를 최소화하고 3번째 인수인 blur의 값을 최대로 높인다. 여기서 최소, 최대는 오른쪽 변을 벗어나지 않는 선에서의 최소, 최대이다. 그리고 x축의 위치를 조금씩 이동시켜주면 오른쪽에만 그림자가 퍼져나가는 것처럼 구현이 된다.

어려웠던 부분은 border의 color를 linear-gradient로 처리하는 부분이었다. 먼저 border에 color 속성에는 linear-gradient가 먹히지 않는다. 그래서 border-image-source와 border-slice 속성을 사용해야 하는데, 단축 속성인 border-image로 한 번에 해결할 수 있다. 

### border-image

border-image는 아래 속성들의 단축 속성이다. 

- border-image-source

요소의 테두리 이미지로 사용할 원본 이미지를 지정하는 속성이다.

- border-image-slice

border-image-source로 설정한 이미지를 여러 개의 영역으로 나눈다. 

- border-image-width

테두리 이미지의 너비를 결정한다.

- border-image-outset

요소의 border-box와 테두리 이미지의 거리를 설정한다.

- border-image-repeat

원본 이미지의 모서리 영역을 요소의 테두리 이미지 크기에 맞춰 조절할 때 사용할 방법을 지정한다.

생략한 속성은 초기값으로 설정되며 초기값은 다음과 같다.

```css
border-image-source: none
border-image-slice: 100%
border-image-width: 1
border-image-outset: 0
border-image-repeat: stretch
```

사용방법은 다음과 같다.

```css
  border-image:
    /* 원본 이미지 */
      url("https://mdn.mozillademos.org/files/4127/border.png")
    /* 슬라이스 */
      27 /
    /* 너비 */                    
      36px 28px 18px 8px /
    /* 거리 */
      18px 14px 9px 4px
    /* 반복 */
      round;
```

앞에서 이야기했지만 linear-gradient를 사용하기 위해서 border-image를 사용한 것이라서 source와 slice 속성만 사용했다.

```css
  border-image: linear-gradient(to bottom, transparent, #707070, transparent);
  border-image-slice: 1;
```

## 😎마무리

작업을 마치고 나니 항상 디자인을 완벽하게 구현하기란 어려운 일이라는 생각이 들었다. 그래서 디자이너와 협업하는 게 굉장히 중요하다는 걸 다시금 느끼게 되었다. 이번에는 디자인과 똑같이 구현할 수 있어서 다행이다.

# :books:참고자료

[border-image - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/ko/docs/Web/CSS/border-image)
