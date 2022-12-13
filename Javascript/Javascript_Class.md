# Javascript

## Class

> ***"클래스는 객체 지향 프로그래밍에서 특정 객체를 생성하기 위해 변수와 메소드를 정의하는 일종의 틀로, 객체를 정의하기 위한 상태(멤버 변수)와 메서드(함수)로 구성된다."***
>
> ***위키백과***

ES6부터 Class 문법을 사용하면 객체 지향 프로그래밍에서 사용되는 다양한 기능을 자바스크립트에서도 사용할 수 있다.

### static

클래스 문법 안에서 static 키워드를 사용하면 정적 메서드, 정적 프로퍼티를 만들 수 있다. 메서드와 프로퍼티를 정적으로 선언하는 건 이들이 개별 인스턴스에 묶이지 않고 클래스 수준에서 사용할 수 있도록 하기 위함이다. 예시를 통해 살펴보자.

```js
class Singleton {
    private static instance

    private constructor() {
        // 초기화 진행
    }

    public static getInstance() {
        if (this.instance) {
            return this.instance
        }
        this.instance = new Singleton()
        return this.instance
    }
}
```

위의 예시는 싱글톤 패턴을 간단히 구현해본 예시이다. 싱글톤 패턴이란 하나의 인스턴스만을 생성해서 사용하는 디자인 패턴이다. 이를 구현하기 위해서는 아래의 두 가지 사항을 충족시켜야 한다.

1. 생성자 외부 접근 제한
2. 외부에서 접근 가능한 메서드로 인스턴스 반환

위의 사항들을 모두 충족하면 Singleton 클래스의 인스턴스를 얻는 방법은 오직 getInstance() 메서드를 사용하는 방법밖에는 없게 된다.

추가로 설명하자면, 생성자를 private으로 설정해 외부 접근을 막았기 때문에, 외부에서는 생성자를 통해 인스턴스를 생성할 수 없다. 그럼 이제 외부에서 접근이 가능한 메서드인 getInstance() 로 클래스의 인스턴스를 반환하면 된다. 그런데 인스턴스를 생성하지 않고 클래스 수준에서 메서드에 접근해야 하기 때문에 static 키워드를 사용해 정적 메서드로 만드는 것이다. 또한 getInstance의 내부에서 instance 프로퍼티에 접근하고 있기 때문에 instance 프로퍼티 또한 static으로 정의해주어야 한다.

```js
const singleton = Singleton.getInstance()
```

그럼 이제 위와 같이 getInstance() 메서드를 통해 인스턴스화 없이 Singleton 클래스의 인스턴스를 변수에 할당할 수 있게 된다.

# :books:참고문서

https://ko.javascript.info/class

https://100100e.tistory.com/339