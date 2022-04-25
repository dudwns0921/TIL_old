# 실무 환경을 위한 프로젝트 설정 2

좀 더 효율적이고 안전한 작업을 위해 API 설정과 env 파일에 대해서 알아보자.

## API 설정 공통화

여기서는 HTTP 비동기 통신 라이브러리인 Axios를 예시로 들겠다.기본적으로 axios를 사용하는 방법은 HTTP 요청이 필요한 vue 컴포넌트에 직접 axios를 임포트해서 사용하는 방법이 있다. 하지만 이렇게 되면 요청이 필요한 모든 vue 컴포넌트에 각각 임포트를 해주어야 하기 때문에 굉장히 비효율적이다. 그래서 API 함수 구조화를 통해 독립적인 파일에 API 관련 설정들을 정의해주면 좋다.

```javascript
import axios from 'axios';

function registerUser(userData) {
  const url = 'http://localhost:3000/signup';
  return axios.post(url, userData);
}

const instance = axios.create({
  baseURL: 'http://localhost:3000/',

function registerUser2(userData) {
  return instance.post('signup', userData);
}

export { registerUser, registerUser2 };
```

`axios.create()` 라고 하는 axios 메서드를 사용해 사용자 정의 구성을 사용하는 axios 인스턴스를 생성할 수 있다.

registerUser2가 axios 인스턴스를 생성해 만든 커스텀 메서드이다. baseURL을 설정해놨기 때문에 위와 같이 좀 더 깔끔한 코드를 작성할 수 있다.

## env 파일

웹,앱 개발을 하다보면 포트, DB관련 정보, API_KEY등.. 개발자 혼자서 또는 팀만 알아야 하는 값들이 있다. 이 때 환경변수 파일을 외부에 만들고, 보안이 필요한 값들을 정의해 소스코드 내에 하드코딩하지 않고 사용할수 있다.

### env 파일 규칙들

- 프로젝트 최상위 루트 파일에 파일을 생성해야 한다.

- 해당 파일은 키 = 값 형태로 데이터들을 정의할 수 있는 파일이다.

- VUE_APP 접두사를 붙이게 되면 그 환경 변수는 자동으로 로드되어 전역에서 사용 가능하다.

```
.env 파일

VUE_APP_API_URL=http://localhost:3000/
```

```javascript
const instance = axios.create({
  baseURL: process.env.VUE_APP_API_URL,
});
```

위의 코드를 환경변수를 적용해 수정해보았다.

___

- 개발 모드와 배포 모드의 url이 다르기 때문에 각 환경마다 파일을 생성해주어야 한다.

- `.env.development` 파일은 개발 모드에서만 동작한다.

- `.env.production` 파일은 배포 모드에서만 동작한다.

- `.env` 파일은 우선순위가 낮다. 그래서 개발 모드에서는 `.env.development` 파일이 우선순위가 높아서 이 파일에 있는 환경 변수를 가져온다. 이 파일에 값이 없으면 `.env` 파일에 있는 값을 찾는다.

- `.env` 파일은 `.env.development` 와 `.env.production` 파일에 없는 공통의 값을 지정하기 위해 사용한다.

```
.env.development

VUE_APP_API_URL=http://localhost:3000/
```

```
.env.production

VUE_APP_API_URL=http://realurl.com/
```

이런 식으로 작성하면 같은 환경변수로 환경에 알맞는 url에 접근이 가능하다.

# :books:참고자료

https://jess2.xyz/vue/vue-tip/#3-axios-%ED%86%B5%EC%8B%A0

인프런 - Vue.js 끝장내기 강의 내용
