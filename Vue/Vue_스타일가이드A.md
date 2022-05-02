# Vue

## 스타일 가이드 : 우선순위 A 규칙

이 문서는 Vue 공식문서의 스타일 가이드를 참고해 작성되었다. 스타일 가이드는 총 4가지 기준으로 분류되며, 기준들은 아래와 같다.

- 우선순위 A : 필수

- 우선순위 B : 매우 추천함

- 우선순위 C : 추천함

- 우선순위 D : 주의 요함

먼저 우선순위 A 규칙에 대해서 자세히 알아보겠다.

### 컴포넌트 이름에 합성어 사용

**root 컴포넌트인 `App` 과 `<transition>`, `<component>`등 Vue에서 제공되는 내장 컴포넌트들을 제외하고 컴포넌트의 이름은 항상 합성어를 사용해야한다.**

모든 HTML 요소의 이름은 한 단어이기 때문에 합성어를 사용하는 것은 기존 그리고 향후 HTML 요소의 충돌을 방지해준다.

### 컴포넌트 데이터

**컴포넌트의 `data` 속성값은 반드시 객체를 반환하는 함수여야 한다.**

왜냐하면 data 속성값이 객체일 경우 컴포넌트를 재사용할 수 없기 때문이다. 아래와 같은 data를 가진 TodoList 컴포넌트를 상상해보자.

```js
data: {
  listTitle: '',
  todos: []
}
```

TodoList 컴포넌트의 모든 인스턴스는 동일한 data 객체를 참조하게 된다. 그렇기 때문에 만약 TodoList 컴포넌트가 애플리케이션의 여러 곳에서 사용되고 있는 상황에서, 한 컴포넌트에서만 listTitle의 값이 변경되더라도 다른 모든 컴포넌트의 listTitle도 변경된다.

결국 우리가 원하는 건 각 컴포넌트가 독립적인 스코프를 갖는 것이다. 이를 위해서는 각 컴포넌트의 인스턴스는 고유한 data 객체를 생성해야 하고, 자바스크립트에서는 이 문제를 함수 안에서 객체를 반환하는 방법으로 해결할 수 있다.

### Props 정의

```js
props: {
  status: String
}
```

위와 같이 prop은 적어도 타입은 명시되도록 가능한 상세하게 정의되어야 한다.

### v-for에 key 지정

간단한 예시로 그 이유를 알아보자.

```js
data: function () {
  return {
    todos: [
      {
        id: 1,
        text: 'Learn to use v-for'
      },
      {
        id: 2,
        text: 'Learn to use key'
      }
    ]
  }
}
```

그 다음 이 목록을 알파벳 순으로 정렬한다고 가정해보겠다. DOM이 업데이트될 때, Vue는 DOM 변경을 최소화하기 위해 렌더링을 최적화한다. 위 사례의 경우, 첫 번째 todo 요소를 지우고 리스트의 맨 끝에다가 추가하는 것이 최적의 방법일 것이다.

문제는, 몇몇 사례의 경우 요소가 DOM에 남아야 할 것을 예상해 특정 요소를 지우지 말아야 한다는 점이다. 리스트 정렬 애니메이션을 위해 `<transition-group>` 를 사용하거나 렌더링된 요소가 `<input>`인 경우 focus 상태를 유지하는 경우를 예시로 들 수 있겠다. 이런 경우에 요소가 유니크한 key 값을 갖고 있다면 Vue가 어떤 요소를 변경, 추가 또는 삭제할지 식별하는 것을 도울 수 있다.

### v-if와 v-for를 동시 사용 금지

일반적으로 아래의 경우에 v-if와 v-for를 함께 사용하려고 한다.

#### 리스트 필터링

```js
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

active인 user인 경우에만 이름을 표시하겠다는 의미로 사용한 것이겠지만, v-for 디렉티브의 우선순위가 v-if보다 높기 때문에 실제로는 아래와 같다.

```js
this.users.map(function (user) {
  if (user.isActive) {
    return user.name
  }
})
```

그러니까 렌더링이 일어날 때마다 active인 user들이 바뀌든 말든 상관없이 무조건 전체 리스트를 확인하게 되는 것이다. 이 문제를 해결하기 위해서는 computed 속성을 활용하면 된다.

```js
computed: {
  activeUsers: function () {
    return this.users.filter(function (user) {
      return user.isActive
    })
  }
}
```

```js
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

이렇게 바꾸면 아래와 같은 장점들이 있다.

- users 배열에 변화가 있을 때만 필터링이 이루어지기 때문에 로직 측면에서 효율적이다.

- v-for 디렉티브에서는 오직 active user만 렌더링이 이루어지기 때문에 더 효율적인 렌더링이 가능하다.

- 로직 계층과 표현 계층이 나뉘어 유지보수가 훨씬 쉬워진다.

### 컴포넌트 스타일 스코프

이 규칙은 오직 싱글 파일 컴포넌트인 경우에만 해당된다.

### Private property names

plugin, mixin 등에서 공용 API로 간주되지 말아야 할 것들에 대해 '$_'접두사를 항상 사용해야 한다.

# :books:참고자료

[Style Guide — Vue.js](https://v2.vuejs.org/v2/style-guide/?redirect=true#Private-property-names-essential)
