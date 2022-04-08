# Vuex

React에서 상태 관리를 위해 주로 Redux를 사용한다면, Vue에서는 Vuex를 사용한다. 공식 상태 관리 라이브러리가 Pinia로 변경되었다는데, 새롭게 Vue 프로젝트를 시작하는 것이라면 해당 라이브러리를 사용해보는 게 좋다고 생각한다.

## Pinia 공식문서

> https://pinia.vuejs.org/

## 설치

> npm i vuex@next --save

## 사용방법

가장 기초적인 사용방법을 알아보자.

```javascript
// src/store/index.js

import { createStore } from 'vuex';

const store = createStore({
  state() {
    return {
      username: '',
    };
  },
  getters: {
    isLogin(state) {
      return state.username !== '';
    },
  },
  mutations: {
    setUsername(state, username) {
      state.username = username;
    },
    clearUsername(state) {
      state.username = '';
    },
  },
});

export default store;
```

```javascript
// src/main.js

import { createApp } from 'vue';
import App from './App.vue';
import store from '@/store/index';

createApp(App).use(store);
```

공식문서에서는 루트 컴포넌트와 store를 동일한 파일에다가 만들었지만, 좀 더 용이한 관리를 위해서는 독립적인 파일을 만들어주는 것이 좋다고 생각한다. 

store는 상태, state를 저장하는 저장소이다. store을 생성하기 위해서는 vuex의 createStore 메서드를 사용해주면 되고, 그 안에 객체로 관련 설정들을 정의할 수 있다. 

vuex의 사용을 위해서는 기본적으로 3가지를 정의해주어야 한다.

1. **state**

2. **mutations**

3. **getters**

mutations은 state의 변경을 위한 메서드들이 정의되는 객체이다. state는 어떤 경우에도 직접적인 변경은 금지된다. getters는 말 그대로 state를 가져오기 위한 메서드들이 정의되는 객체이다.

위 예시는 실제로 프로젝트에서 로그인 여부를 체크하기 위해 만든 것이다. 프로젝트에서 어떻게 사용되었는지 좀 더 자세히 알아보자.

## 프로젝트 예시

```javascript
 //LoginForm.vue

methods: {
  async submitForm() {
    try {
      const userData = {
        username: this.username,
        password: this.password,
      };
      const { data } = await loginUser(userData);
      this.$store.commit('setUsername', data.user.username);
      this.$router.push('/main');
    } 
```

위 코드의 전체적인 과정을 보자면,

1. input에 사용자가 입력한 아이디와 패스워드를 각각 userData의 객체 안에 username과 password로 정의한다.

2. loginUser 메서드를 통해 서버로 사용자 정보를 전송하고, Promise가 이행 상태로 바뀌면 해당 Promise의 data 객체를 구조 분해 할당을 사용해 같은 이름의 변수에 할당한다.

3. commit 메서드로 store의 setUsername 메서드에 파라미터로 아이디 정보를 전달한다.

4. username state가 해당 값으로 바뀐다.

```javascript
 <template>
...
    <div class="right-area" v-if="isLogin">
      <span @click="logOut">로그아웃</span>
    </div>
  </header>
</template>
...
computed: {
  isLogin() {
    return this.$store.getters.isLogin;
  },
},
...
 methods: {
  logOut() {
    this.$store.commit('clearUsername');
    this.$router.push('/');
  }
...
```

위 코드의 전체적인 과정을 보자면,

1. computed 속성에 로그인 여부를 체크하는 isLogin을 정의한다.

2. isLogin은 getters 안의 isLogin 메서드를 반환하는데, 해당 메서드는 유저 아이디가 빈 값인지 아닌지를 체크해 boolean 값을 반환한다.

3. isLogin이 true이면 v-if 디렉티브에 따라 로그아웃 버튼이 나타난다.

4. 해당 버튼을 누르게 되면 logOut 메서드가 실행되고, commit 메서드를 통해 clearUsername 메서드를 사용해 username state를 초기화한다.

## Actions

지금까지 정말 기본적인 vuex의 사용 방법에 대해서 알아보았다. 다음으로는 mutations과 비슷하지만 약간은 다른 특징을 갖고 있는 actions에 대해 알아보자.

- state를 바꾸는 것이 아니라, action은 mutation을 commit한다.

좀 더 풀어서 설명하자면 actions는 state를 변경하는 메서드인 mutations를 실행시킨다는 의미이다. 

- actions에서는 비동기 작업을 할 수 있다.

actions는 기본적으로 context라는 객체를 파라미터로 받는데, 이 객체는 store 인스턴스에 있는 속성과 메서드를 동일하게 갖고 있다. 그래서 context를 통해 state나 mutations에 접근 가능하며, 심지어 actions에도 접근할 수도 있다. 그렇기 때문에 여러 가지 mutations나 비동기 작업이 필요한 경우 이를 한 데 묶어 actions에 정의해 좀 더 깔끔한 코드를 만들 수 있다. 실제 프로젝트에서 어떻게 사용되었는지 한 번 살펴보자.

### 사용해보기

```javascript
// LoginForm.vue

methods: {
    async submitForm() {
        try {
            ...
            const { data } = await loginUser(userData);
            this.$store.commit('setToken', data.token);
            this.$store.commit('setUsername', data.user.username);
            saveAuthToCookie(data.token);
            saveUserToCookie(data.user.username);
            this.$router.push('/main');
        } catch (error) {
            ...
```

여러 가지 mutations과 비동기 작업이 함께 사용되다보니 컴포넌트에 비즈니스 로직이 너무 길어지게 되었다. 물론 이렇게 둬도 아무런 문제가 없지만 항상 클린한 코드를 추구하는 것이 결국에는 업무의 효율성을 높여준다고 생각한다.

```javascript
// src/store/index.js

import { saveAuthToCookie, saveUserToCookie } from '@/utils/cookies';
import { loginUser } from '@/api/index';
...
actions: {
    async LOGIN({ commit }, userData) {
        const { data } = await loginUser(userData); // api 호출
        // context.commit('setToken', data.token); 구조 분해 할당 사용 전
        commit('setToken', data.token);
        commit('setUsername', data.user.username);
        saveAuthToCookie(data.token);
        saveUserToCookie(data.user.username);
        return data;
    }
}    }
  }
})
```

store가 정의된 파일로, 위의 모든 비즈니스 로직을 LOGIN 이라는 actions에 담았다. 구조 분해 할당을 사용해 좀 더 단순화된 코드로 바꿔보았다. 

```javascript
// LoginForm.vue

methods: {
    async submitForm() {
        try {
            ...
            await this.$store.dispatch('LOGIN', userData);
            this.$router.push('/main');
        } catch (error) {
            ...
```

actions를 사용할 때는 dispatch 메서드를 사용해주면 된다. 보이다시피 아까의 긴 코드가 간결하게 2줄로 정리되었다. 

# :books:참고자료

https://vuex.vuejs.org/

인프런 - Vue.js 끝장내기 수업 내용
