# Spring-Model 객체

Controller에서 생성된 데이터를 담아 View로 전달할 때 사용하는 객체이다.

서블릿의 request 객체와 유사한 역할을 한다.

메서드에 Model 객체를 파라미터로 선언하면 애플리케이션 컨텍스트가 알아서 Model 객체를 만들어준다.



## 예시

```java
@GetMapping
public String showDesignForm(Model model) {
    List<Ingredient> ingredients = Arrays.asList(
        new Ingredient("FLTO", "Flour Tortilla", Type.WRAP),
        new Ingredient("COTO", "Corn Tortilla", Type.WRAP),
        new Ingredient("GRBF", "Ground Beef", Type.PROTEIN),
        new Ingredient("CARN", "Carnitas", Type.PROTEIN),
        new Ingredient("TMTO", "Diced Tomatoes", Type.VEGGIES),
        new Ingredient("LETC", "Lettuce", Type.VEGGIES),
        new Ingredient("CHED", "Cheddar", Type.CHEESE),
        new Ingredient("JACK", "Monterrey Jack", Type.CHEESE),
        new Ingredient("SLSA", "Salsa", Type.SAUCE),
        new Ingredient("SRCR", "Sour Cream", Type.SAUCE)
    );

    Type[] types = Ingredient.Type.values();
    for(Type type : types) {
        model.addAttribute(type.toString().toLowerCase(),
                           filterByType(ingredients, type));
    }

    model.addAttribute("taco", new Taco());

    return "design";
}
```

위의 코드는 스프링 인 액션의 타코 주문 웹 애플리케이션의 일부분이다.

아래를 보면 Taco 객체를 생성해 taco라는 이름으로 View 부분인 design이라는 이름을 가진 html 문서로 전달하고 있다.

```java
package tacos;

import java.util.List;

import lombok.Data;

@Data
public class Taco {
	private String name;
	private List<String> ingredients;
}
```

위에서 Taco 객체는 이름과 재료를 담고 있는 리스트를 멤버 변수로 가지고 있다.

```java
...
	<form method="POST" th:object="${taco}">
...
	<div class="content3 boxDesign">
		<h3>Name your taco creation!</h3>
		<input type="text" th:field="*{name}" />
		<button>Submit your taco</button>
	</div>
...
```

위의 코드는 design html 파일의 일부분이다.

결과만 보자면 input 태그에 적은 타코의 이름이 위에서 model 객체를 통해 전달한 Taco 객체의 이름으로 할당된다.

여기서 타임리프라는 자바 템플릿 엔진에 대해서 조금 알 필요가 있다.



th:object는 form을 통해 submit을 할 때 form의 데이터를 설정한 객체로 받아준다.

여기서는 Model을 통해 전달한 Taco 객체에 form의 데이터를 담는 것이다.

th:field는 th:object에 설정된 객체의 필드와 연결시켜준다.

그래서 Taco 객체의 name 필드와 연결되는 것이다.



# :books:참고자료

https://velog.io/@msriver/Spring-Model-%EA%B0%9D%EC%B2%B4

https://lopicit.tistory.com/224

크레이그 월즈, 스프링 인 액션, 제이펍, 2020