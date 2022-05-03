# Vue

## 스타일 가이드 : 우선순위 B 규칙

### 싱글 파일 컴포넌트 사용

싱글 파일 컴포넌트를 사용하면 다음과 같은 장점이 있다.

- 완전한 구문 강조

- CommonJS 모듈

- 컴포넌트에만 제한된 CSS

#### 싱글 파일 컴포넌트 이름 규칙

PascalCase 혹은 Kebab-case를 사용해야 한다.

**Pascal case examples**

- OutOfMemoryException
- DateFormat
- DatabaseConnection
- LinkedList
- EventHandler

Pascal case와 유사한 Camel case가 있는데, 둘의 차이점은 시작할 때 대문자를 사용하는지의 여부이다. Pascal case는 시작할 때 대문자를 사용한다.

**kebab-case examples**

- out-of-memory-exception
- date-format
- database-connection
- linked-list
- event-handler

### 베이스 컴포넌트 이름

애플리케이션에 특화된 스타일링이나 규칙을 적용하는**Base components (a.k.a. presentational, dumb, or pure components)** 은 반드시 특정한 접두사와 함께 사용되어야 한다.

이 컴포넌트들은 아래 요소만을 가질 수 있다.

- HTML 요소

- 다른 base components

- 3rd-party UI components

하지만 베이스 컴포넌트들은 전역 state를 가질 수 없다.

베이스 컴포넌트들의 목적에 해당하는 요소가 있으면, 이들의 이름은 대부분의 경우 그들이 감싸고 있는 요소들의 이름을 포함한다.(e.g. BaseButton, BaseTable) 좀 더 구체적인 상황을 위한 비슷한 컴포넌트를 만들 때, 이 컴포넌트들은 항상 베이스 컴포넌트를 사용한다.(e.g. BaseButton 컴포넌트는 ButtonSubmit 컴포넌트에서 사용될 수 있다.) 

### 싱글 인스턴스 컴포넌트 이름

오직 하나의 인스턴스만을 가지고 있는 컴포넌트는 'The' 접두사를 사용해야 한다. 여기서 오직 하나란 컴포넌트가 한 페이지에서만 사용되어야 한다는 것이 아니라 한 페이지당 한 번씩 사용된다는 걸 의미한다. 이 컴포넌트들은 그 어떤 prop도 받지 않는다. 

```
예시

components/
|- TheHeading.vue
|- TheSidebar.vue
```

### 강한 연관성을 가진 컴포넌트 이름

부모 컴포넌트와 강한 연관성을 가지고 있는 자식 컴포넌트는 부모 컴포넌트의 이름을 접두사로 사용해야 한다.

### 컴포넌트 이름의 단어 순서 정렬

컴포넌트 이름은 가장 높은 계층의 단어로 시작해 기술적인 단어로 끝나야 한다.

```
예시


components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue
```

### 셀프 클로징 컴포넌트

내용이 없는 컴포넌트는 DOM 템플릿을 제외한 싱글 파일 컴포넌트, string templates, 그리고 JSX에서는 셀프 클로징 형태로 작성되어야 한다.

### 템플릿에서 컴포넌트 이름 규칙 지정

대부분의 프로젝트에서 싱글 컴포넌트 내에서 컴포넌트의 이름은 항상 PascalCase를 사용하고, DOM 템플릿에서는 kebab-case를 사용한다

### JS/JSX에서 컴포넌트 이름 규칙 지정

JS나 JSX에서 컴포넌트의 이름은 항상 PascalCase가 되어야 한다. 자바스크립트에서 클래스와 프로토타입 생성자같이 구별되는 인스턴스를 가질 수 있는 것들에 대해 PascalCase를 사용한다. Vue 컴포넌트 또한 인스턴스를 가지므로 PascalCase를 사용하는 것이 합당하다. 더불어, JSX를 PascalCase로 사용하는 것은 읽는 사람들로 하여금 컴포넌트와 HTML 요소를 좀 더 쉽게 구별할 수 있도록 한다.

```js
예시

import MyComponent from './MyComponent.vue'

export default {
  name: 'MyComponent',
  // ...
}
```

### 전체 이름 컴포넌트 이름

컴포넌트 이름은 축약하지 말아야 한다.

```
예시


components/
|- GameCardH.vue

components/
|- GameCardHorizontal.vue
```

실제로 축약해서 컴포넌트명을 만든 경험이 있어 기억에 남는 규칙이다. 특히 협업할 때 팀원들이 컴포넌트의 이름을 명확하게 인식할 수 있어야 하기에 중요하다.

### prop 이름 규칙 지정

prop을 선언할 때는 늘 CamelCase를 사용해야 하고, 템플릿이나 JSX에서는 kebab-case를 사용해야 한다.

```js
예시

props: {
  greetingText: String
}

<WelcomeMessage greeting-text="hi"/>
```

### 다중 속성 엘리먼트

여러 개의 속성을 가진 요소는 한 줄에 하나의 속성만을 작성해야 한다.

```html
예시

<MyComponent
  foo="a"
  bar="b"
  baz="c"
/>
```

### 템플릿에서 단순한 표현식

템플릿에서는 단순한 표현식만을 사용해야 한다. 복잡한 표현식일 경우 computed 속성이나 메소드를 활용해야 한다. 템플릿에 복잡한 표현식을 그대로 사용할 경우 Vue의 핵심 특징인 선언적 렌더링이 의미가 없어진다.

```js
예시

<!-- In a template -->
{{ normalizedFullName }}

// The complex expression has been moved to a computed property
computed: {
  normalizedFullName: function () {
    return this.fullName.split(' ').map(function (word) {
      return word[0].toUpperCase() + word.slice(1)
    }).join(' ')
  }
}
```

### 단순한 computed 속성

복잡한 computed 속성은 가능한한 여러 개의 단순한 속성으로 나누어져야 한다.

### 속성 값에 따옴표

```html
예시

<input type="text">
<AppSidebar :style="{ width: sidebarWidth + 'px' }">
```

### 축약형 디렉티브

축약형 디렉티브,

- : for v-bind:

- @ for v-on

- `#` for v-slot

들은 디렉티브와 혼용해서 사용하면 안된다.

# :books:참고자료

[Style Guide — Vue.js](https://kr.vuejs.org/v2/style-guide/#%EC%85%80%ED%94%84-%ED%81%B4%EB%A1%9C%EC%A7%95-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EB%A7%A4%EC%9A%B0-%EC%B6%94%EC%B2%9C%ED%95%A8)
