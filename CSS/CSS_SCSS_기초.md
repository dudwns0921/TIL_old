# SCSS 기초

## CSS Preprocessor

프로젝트 규모가 커지면 CSS는 불가피하게 가독성이 떨어지는 등 유지보수의 어려움을 주는 요소가 된다. 코드의 재활용성을 올리고, 가독성을 올리는 등 CSS에서 보이던 단점을 보완하고, 개발의 효율을 올리기 위해 등장한 개념이 바로 CSS 전처리기이다. 

css 전처리기는 특정한 문법을 활용해 작성된 코드를 다시금 순수 CSS 코드로 바꾸어주는 역할을 한다. 

## SCSS

css 전처리기 중 아마 가장 많이 들어본 것이 바로 SCSS일 것이다. 그런데 종종 Sass와 혼용되는 것을 봤을 거라고 생각한다. 둘의 차이는 무엇일까?

Sass의 3버전에서 새롭게 등장한 SCSS는 CSS 구문과 완전히 호환되도록 새로운 구문을 도입해 만든 Sass의 모든 기능을 지원하는 CSS의 상위집합이다. 결국 SCSS 안에 Sass가 포함된다고 생각하면 된다.

## Variables

변수를 작성하기 위한 _variables.scss 파일을 만들어준다. 사실 어떤 이름이라도 상관은 없다. 다만 앞의 밑줄은 반드시 들어가야만 한다. 밑줄의 의미는 순수 CSS로 변경하지 않을 파일을 의미한다.

```scss
/*_variables.scss */

$bg: red;
```

변수를 만드는 방법은 간단하다. 달러 표시에 변수명을 작성해주고 콜론 뒤에 해당 값을 적어주면 된다.

## Nesting

Nesting은 중첩이란 뜻인데, 간단한 예시를 통해 중첩이 어떻게 적용되는지 살펴보자.

```html
<div class="container">
  <h1>제목</h1>
  <h2>부제목</h2>
</div>
```

위와 같은 구조가 있을 때, 제목과 부제목을 각각 다른 색으로 설정해야 한다고 가정하겠다.

```css
/* 일반 CSS */

.container h1 {
    color: red;
}

.container h2 {
    color: blue;
}
```

```scss
/* SCSS */

.container {
  h1 {
    color: red;
  }
  h2 {
    color: blue;
  }
}
```

둘의 차이가 확실하게 느껴질 거라고 생각한다. 들여쓰기를 통해 container 안에 h1요소와 h2요소가 포함되었음을 직관적으로 알 수 있다.

### Ampersand(상위 선택자 참조)

중첩 안에서 `&` 키워드는 상위(부모) 선택자를 참조하여 치환한다.

```scss
.list {
  li {
    &:last-child {
      margin-right: 0;
    }
  }
}
```

간단한 예로 여러 개의 리스트 요소들이 있다고 해보자. 다음과 같이 &키워드를 사용해서 마지막에 있는 요소에만 특정 속성을 적용시킬 수 있다. 

## Mixins

SCSS Mixins는 스타일 시트 전체에서 **재사용 할 CSS 선언 그룹** 을 정의하는 기능이다. Mixin은 두 가지만 기억하면 된다. 선언하기(`@mixin`)와 포함하기(`@include`) 이다. 만들어서(선언), 사용(포함)하는 것이다.

```scss
@mixin flexWrapper {
    display: flex;
    justify-content: center;
    align-items: center;
}

/*-----------------------------------------------------------------*/

.container {
  @include flexWrapper;
}
```

위와 같이 @mixin을 사용해 flexWrapper를 위한 mixin을 만들고, @include를 통해 container 클래스를 가지고 있는 div 요소에 flexWrapper mixin을 포함시켰다.

### 인수

Mixin은 함수처럼 인수를 가질 수 있다. 하나의 Mixin으로 다양한 결과를 만들 수 있다. 가령 같은 요소들이 여러 개 있을 때 각각을 다른 색상으로 설정해야 한다고 해보자.

```scss
@mixin link($color){
    text-decoration: none;
    color: $color;
}

/*-----------------------------------------------------------------*/

a:nth-child(odd) {
    @include link(blue);
}


a:nth-child(even) {
    @include link(red);
}
```

다음과 같이 홀수 요소들에는 파란색을, 짝수 요소들에는 빨간색을 설정했다. 주의할 점은 변수를 사용할 때는 꼭 $ 키워드를 함께 사용해주어야 한다는 것이다.

### if-else

mixin에서는 if-else 구문도 활용할 수 있다. 위의 예시를 한 번 바꿔보겠다.

```scss
@mixin link($index) {
  text-decoration: none;
  @if ($index== 'odd') {
    color: blue;
  } @else {
    color: red;
  }
}

/*-----------------------------------------------------------------*/

a:nth-child(odd) {
  @include link('odd');
}

a:nth-child(even) {
  @include link('even');
}
```

## Extends

SCSS 에는 Extends라는 기능도 있다. CSS 구문을 재사용한다는 점에서 Mixins과 크게 다르지 않다. 다만 여러 부작용이 발생할 가능성이 있어 사용을 권장하지 않는다고 한다.

[Sass Guidelines — Korean translation](https://sass-guidelin.es/ko/#extend)

# :books:참고자료

노마드코더 CSS Layout 마스터 클래스 강의

[Sass(SCSS) 완전 정복! | HEROPY](https://heropy.blog/2018/01/31/sass/)
