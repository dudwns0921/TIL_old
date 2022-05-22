# Axios 인터셉터

이전에 토큰 기반 인증 시스템에 대해서 다뤄본 적이 있다.

토큰 기반 시스템의 일반적인 흐름은 다음과 같다.

1. 유저가 아이디와 비밀번호로 **로그인**을 한다
2. 서버측에서 해당 **계정정보를 검증**한다.
3. 계정정보가 정확하다면, 서버측에서 유저에게 **토큰을 발급**한다.
4. 클라이언트 측에서 전달받은 **토큰을 저장**해두고, 서버에 요청을 할 때 마다, 해당 **토큰을 함께 서버에 전달**한다.
5. 서버는 **토큰을 검증**하고, **요청에 응답**한다.

```js
import axios from 'axios';
import store from '@/store/index';

const axiosService = axios.create({
  baseURL: process.env.VUE_APP_API_URL,
    headers: {
        Authorization: store.state.token,
    }
});
```

Vue 프로젝트에서는 일반적으로 아래 과정을 통해 토큰 기반 인증 시스템을 구현한다.

1. vuex를 통해 만든 store에 token state를 생성한다.

2. 로그인 후 받아온 토큰 값을 commit 메서드를 통해 token state에 할당한다.

3. 해당 값을 Authorization 속성에 실어 서버 측에 요청한다.

위 코드는 Authorization 속성에 store에 있는 token state 값을 할당한 부분이다. 딱히 문제가 없어보이지만, 실제로는 토큰 값이 비어있게 된다. 

자바스크립트에는 데이터가 변경되었을 경우 이를 자동으로 갱신해주는 기능이 없다. 그러니까 header에 실은 토큰값은 서버로부터 토큰값을 발급받기 전의 토큰값으로 빈 문자열인 것이다. 사용자가 서버로부터 응답을 받기 위해서는 요청을 보낼 때마다 header의 토큰값을 할당해주어야 하는 것이다. 이를 위해서 사용하는 것이 바로 axios의 인터셉터이다.

## Axios 인터셉터 사용하기

```js
// src/api/index.js

import axios from 'axios';
import { setInterceptors } from './common/interceptors';

function createAxiosService() {
    const axiosService = axios.create({
      baseURL: process.env.VUE_APP_API_URL,
    });

  return setInterceptors(axiosService);
}

const axiosService = createAxiosService();

function loginUser(userData) {
  return axiosService.post('login', userData);
}
...

export { loginUser };
```

```js
//src/api/common/interceptors.js

import store from '@/store/index';

export function setInterceptors(axiosService) {
    axiosService.interceptors.request.use(
        function (config) {
          // 요청을 보내기 전에 어떤 처리를 할 수 있다.
          config.headers.Authorization = store.state.token;
          return config;
        },
        function (error) {
          // 요청이 잘못되었을 때 에러가 컴포넌트 단으로 오기 전에 어떤 처리를 할 수 있다.
          return Promise.reject(error);
        }
  );

  axiosService.interceptors.response.use(
    function (response) {
        // 서버에 요청을 보내고 나서 응답을 받기 전에 어떤 처리를 할 수 있다.
          return response;
        },
      function (error) {
        // 응답이 에러인 경우에 미리 전처리할 수 있다.
          return Promise.reject(error);
        }
    );

    return axiosService;
}
```

위와 같이 코드를 짜면 인터셉터가 요청을 보내기 전에 store에서 token 값을 가져와 header에 싣게 된다.

# :books:참고자료

인프런 - Vue.js 끝장내기 강의 내용
