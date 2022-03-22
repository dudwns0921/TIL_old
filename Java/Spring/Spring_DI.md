# Spring-의존성 주입(Dependency Injection, DI)

대부분의 애플리케이션은 애플리케이션 전체 기능 중 일부를 담당하는 많은 컴포넌트로 구성된다.

각 컴포넌트는 다른 애플리케이션 구성 요소와 협력해서 작업을 처리한다.

그리고 애플리케이션이 실행될 때는 각 컴포넌트가 어떻게든 생성되어야 하고 상호 간에 알 수 있어야 한다.

스프링은 스프링 애플리케이션 컨텍스트라는 컨테이너를 제공하는데, 이것은 애플리케이션 컴포넌트를 생성하고 관리한다.

그리고 애플리케이션 컴포넌트, 또는 빈들은 애플리케이션 컨텍스트 내부에서 서로 연결되어 완전한 애플리케이션을 만든다.

빈의 상호 연결은 의존성 주입이라고 알려진 패턴을 기반으로 수행된다.

애플리케이션 컴포넌트에서 의존하는 다른 빈의 생성과 관리를 자체적으로 하는 대신 컨테이너가 해주며,

이 개체에서는 모든 컴포넌트를 생성, 관리하고 해당 컴포넌트를 필요로 하는 빈에 주입한다.

일반적으로 이러한 과정은 생성자 인자 또는 속성의 접근자 메서드를 통해 처리된다.

이를 통해 서로 강하게 결합되어 있는 두 클래스를 분리하고, 두 객체 간의 관계를 결정해줌으로써 결합도를 낮추고 유연성을 확보하고자 했다.

예시를 통해 의존성 주입에 대해서 좀 더 확실하게 알아보자.

```java
public class Pencil {

}


public class Store {

    private Pencil pencil;

    public Store() {
        this.pencil = new Pencil();
    }

}
```

위의 예시와 같이 Store 객체가 Pencil 객체를 사용하고 있는 경우에 Store객체가 Pencil 객체에 의존성이 있다고 표현한다.

하지만 이런 경우는 두 가지 문제가 발생한다.

- 두 클래스가 강하게 결합되어 있음

위의 예시는 Store 클래스와 Pencil 클래스가 강하게 결합되어 있다.

그래서 가령 연필 외에 다른 상품을 판매하고자 한다면 Store 클래스의 생성자를 수정해야만 한다.

이에 대한 해결책으로 상속을 떠올릴 수 있지만, 상속 또한 제약이 많고 확장성이 떨어지는 문제가 있다.

- 객체들이 아닌 클래스 간의 관계가 맺어지고 있음

올바른 객체지향적 설계라면 객체들 간의 관계가 맺어져야 한다.

객체들 간에 관계가 맺어졌다면 다른 객체의 구체적인 클래스(Pencil, Food)를 알지 못하더라도 인터페이스의 타입(Product)으로 사용할 수 있다.

위의 문제를 해결하기 위해서는 먼저 다형성이 필요하다.

Pencil, Food 등 여러 제품들을 하나로 표현하기 위해서는 Product라는 인터페이스가 필요하다.

그리고 Store와 Pencil이 강하게 결합되어 있는 부분을 느슨하게 해주기 위해 외부에서 상품을 주입받아야 한다.

```java
public interface Product {

}

public class Pencil implements Product {

}

Public class Store {

    private Product product;

    public Store(Product product) {
        this.product = product;
    }

}
```

여기서 스프링이 애플리케이션 컨텍스트, 컨테이너를 필요로 하는 이유를 알 수 있다.

예시와 같이 Pencil이라는 객체를 만들고, 그 객체를 Store로 주입시키는 역할을 위해 컨테이너가 필요하게 된 것이다.

```java
public class Container {

    public void store() {

        Product pencil = new Pencil();
        // 의존성 주입
        Store store = new Store(pencil);
    }

}
```

그리고 이러한 개념은 제어의 역전(Inversion of Control, IOC)라고 불리기도 한다.

스프링에서 어떤 컴포넌트를 사용할지에 대한 책임이 컨테이너로 넘어갔고, 자신은 수동적으로 주입받는 컴포넌트를 사용하기 때문이다.

# :books:참고자료

https://mangkyu.tistory.com/150

크레이그 월즈, 스프링 인 액션, 제이펍, 2020