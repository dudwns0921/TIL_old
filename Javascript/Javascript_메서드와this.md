# 모던자바스크립트

## 챕터 : 메서드와 this

객체는 사용자(user), 주문(order) 등과 같이 실제 존재하는 개체(entity)를 표현하고자 할 때 생성된다. 사용자는 현실에서 장바구니에서 물건 선택하기, 로그인하기, 로그아웃하기 등의 행동을 한다. 이와 마찬가지로 사용자를 나타내는 객체 user도 특정한 행동을 할 수 있다.

자바스크립트에서는 객체의 프로퍼티에 함수를 할당해 객체에게 행동할 수 있는 능력을 부여해준다. 이 때 이 함수를 메서드라고 부른다.

메서드는 대부분 객체 프로퍼티의 값을 활용한다. 그렇기 때문에 객체 내부에 접근할 수 있어야 하는데, 그런 경우에 this 키워드를 사용한다. 여기서 this는 객체를 나타내는데, 구체적으로 메서드를 호출할 때 사용된 객체를 의미한다.

### 자바스크립트의 this

자바스크립트의 this는 다른 언어와 동작 방식이 다르다. 자바스크립트에서는 모든 함수에 this를 사용할 수 있는데, 그 이유는 this의 값이 런타임에 결정되며 컨텍스트에 따라 달라지기 때문이다. 동일한 함수라도 다른 객체에서 호출했다면 this가 참조하는 값이 달라지는데, 예시를 통해 알아보자.

```javascript
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// 아래 코드는 f 라는 프로퍼티에 sayHi라는 함수를 할당한 것이다.
user.f = sayHi;
admin.f = sayHi;

// 'this'는 '점(.) 앞의' 객체를 참조하기 때문에
// this 값이 달라짐
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)
```

### this가 없는 화살표 함수

화살표 함수는 일반 함수와는 달리 ‘고유한’ `this`를 가지지 않는다. 화살표 함수에서 `this`를 참조하면, 화살표 함수가 아닌 ‘평범한’ 외부 함수에서 `this` 값을 가져온다.

```javascript
let user = {
    firstName: "Elio",
    sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
}
};

user.sayHi(); // Elio
```

## 과제1

## 계산기 만들기

```javascript
let calculator = {
  x: '',
  y: '',
  read() {
    this.x = Number(prompt('첫 번째 값을 입력하세요'));
    this.y = Number(prompt('두 번째 값을 입력하세요'));

    //this.x = +prompt('첫 번째 값을 입력하세요');
    //this.y = +prompt('두 번째 값을 입력하세요');
  },
  add(x, y) {
    return x+y;
  },
  multiple(x, y) {
    return x*y;
  }
}

calculator.read();
alert(this.add(this.x, this.y));
alert(this.multiple(this.x, this.y));
```

해답에서는 각주 처리한 부분처럼 코드를 작성했다. 그리고 위에서 따로 x,y에 대한 선언을 하지 않았는데, 그 이유는 다음과 같다.

1. 먼저 각주처럼 코드를 작성하면 두 변수가 초기화됨과 동시에 값이 할당된다.

2. 산술연산자가 있기 때문에 묵시적 형변환이 일어나 Number 타입의 값이 할당된다.

조금 깊게 생각하면 코드를 훨씬 간결하게 작성할 수 있는 것 같다.

# :books:참고자료

https://ko.javascript.info/object-methods
