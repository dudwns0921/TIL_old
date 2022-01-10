SCSS 

변수를 만들기 위해서는 먼저 _variables.scss 라는 파일을 만들어야 한다.

여기서 파일 이름은 중요하지 않고 앞의 언더스코어가 중요하다.

언더스코어의 의미는 이 파일의 내용을 css로 변환시키지 않고 오직 scss를 위해 사용하겠다는 것이다.



변수를 할당한 파일을 styles.scss에 import 해주면 그 안에서 변수들을 사용할 수 있다.





variables

$ + 변수이름 + : + 값

ex) $bg : red;



Nesting

부모 요소에 포함된 자식 요소들의 style을 바꾸고 싶을 경우,

일반적으로는 

.box input {

}

.box button {

}



이렇게 자식 요소 각각을 지정해서 값을 입력해야 한다.



하지만 Nesting을 사용하면

.box {

박스 클래스 관련 스타일

input {

}

button {

}

}

이렇게 세 번의 작업을 한 번에 처리할 수 있게 된다. 



상태일 경우에는,

&:hover 이렇게 적어주면 된다. 



네스팅 안에 네스팅을 또 다시 할 수도 있다. 

.box {

박스 클래스 관련 스타일

input {

&:hover

}

button {

}

}



Mixins

scss functionailty 를 재사용할 수 있도록 해줌

@ mixin_name {

style 작성

}



변수와 마찬가지로 style.scss에서 import 후 사용하면 된다.

@include 믹스인이름



mixin은 변수를 사용할 수도 있다.

@mixin($color) {

}

이후에 style을 작성할 때

@믹스인이름(적용하고 싶은 컬러 값)



scss 를 사용하면 프로그래밍하는 것처럼 css를 다룰 수 있게 된다. 



@if else도 사용 가능



믹스인은 상황에 따라 코딩을 다르게 하고 싶을 때

extend 코드를 확장하거나 재사용하고 싶을 때 사용



extend

%button {

  border-radius: 7px;

  font-size: 12px;

  text-transform: uppercase;

  padding : 5px 10px;

} 

선언 방법은 % 을 적어주면 된다.



@import "_buttons";



a {

 @extend %button;

}



사용방법은 다음과 같이 파일을 import하고 @extend %extend명을 적어주면 된다.



a {

 @extend %button;

 text-decoration: none;

}

button {

 @extend %button;

 border: none;

}



공통적인 것들을 extend로 공유하고 다른 부분만 따로 작성해주면 각 요소에 알맞게 css를 적용할 수 있다.







$minIphone: 500px;

$maxIphone: 690px;

$minTablet: $minIphone + 1;

$maxTablet: 1120px;



@mixin responsive($device) {

 @if $device == "iphone" {

  @media screen and (min-width: $minIphone) and (max-width: $maxIphone) {

   @content;

  }

 } @else if $device == "tablet" {

  @media screen and (min-width: $minTablet) and (max-width: $maxTablet) {

   @content;

  }

 } @else if $device == "iphone-l" {

  @media screen and (max-width: $minIphone) and (max-width: $maxIphone) and (orientation: landscape) {

   @content;

  }

 } @else if $device == "ipad-l" {

  @media screen and (min-width: $minTablet) and (max-width: $maxTablet) and (orientation: landscape) {

   @content;

  }

 }

}

미디어쿼리를 좀 더 쉽게 적용할 수 있다.



Bourbon, Sass MediaQueries, Animated SCSS

유용한 SCSS 라이브러리들

나중에 확인해보자
