# Axios

Axios로 서버와 통신할 때는 아래와 같은 이유로 일반적으로 JSON 형식의 데이터를 사용한다. 

- JSON는 특정 언어에 종속되지 않는다.
- XML보다 가볍기 때문에 최소한의 용량으로 데이터 전송이 가능하다.
- 구조 정의의 편리와 가독성이 뛰어나다.

## JSON

### 개요

> 직렬화는 컴퓨터 과학의 테이터 스토리지 문맥에서 데이터 구조나 오브젝트 상태를 동일하거나 다른 컴퓨터 환경에 저장하고 나중에 재구성할 수 있는 포맷으로 변환하는 과정이다.

JavaScript Object Notation (JSON) 은 데이터 교환 형식의 일종으로 여러 데이터 유형들을 **직렬화**하기 위해 사용된다. JSON은 숫자, 부울, 문자열, null, 배열 및 이러한 값으로 구성된 객체를 나타낼 수 있다. JSON은 기본적으로 함수, 정규식, 날짜 등과 같은 더 복잡한 데이터 유형은 나타내지 않는다.

JavaScript 구문에 기반을 두고 있지만 분명한 차이점을 가지고 있다.

- 객체
  - 객체 속성의 이름은 반드시 큰 따옴표로 표시된 문자열이어야 한다. 
  - 후행 쉼표는 허용하지 않는다.
- 숫자
  - 선행 0은 허용하지 않는다.
  - 소숫점 뒤에는 적어도 한 자릿수가 뒤따라야 한다.
  - NaN과 infinity 미지원

- 기타
  - 문자열의 작은 따옴표 금지
  - 주석 및 undefined 미지원

### Axios와 Express 서버의 JSON 통신

예시를 통해 실제 JSON 통신을 살펴보자.

```js
// Frontend - axios.js

const instance = axios.create({
  baseURL: API_HOST,
  headers: {
    'Content-Type': 'application/json; charset=UTF-8',
  },
})

function loginUser(userData) {
  return instance.post('/login', userData)
}
```

위 코드는 클라이언트 측에서 구현한 로그인 함수이다. 확장성을 고려해 baseURL과 헤더의 일부 부분이 미리 정의된 axios 객체를 만들고, 그 객체로 post 요청을 보내고 있다.  

```js
//Backend - server.js

import express from 'express'
import morgan from 'morgan'
import cors from 'cors'

const app = express()

app.use(morgan('dev'))
app.use(cors())
app.use(express.json())

export default appjs
```

```js
//Backend - userController.js

const postLogin = async (req, res) => {
  const { email, password } = req.body
  try {
    const dbUserData = await UserModel.findOne({ email })
    const passwordCheck = await bcrypt.compare(password, dbUserData.password)
    if (dbUserData) {
      if (passwordCheck) {
        const token = generateAccessToken(dbUserData.email)
        return res.json({
          token,
          user: {
            email: dbUserData.email,
            username: dbUserData.username,
          },
          result: 'success',
        })
      }
    }
  } catch (e) {
    console.log('error', e)
    return res.send({
      result: 'failed',
    })
  }
}
```

/login 으로 post 요청이 이루어지면 실행되는 postLogin 함수이다. 로직을 간단하게 정리하자면 req.body에서 받아온 사용자 정보를 DB에서 찾고, 만약 해당 정보가 있을 경우에는 여러 데이터들을 객체에 담아 다시 클라이언트측으로 보내고 있다.

```js
// Frontend - Login.js

let userData = {
    email,
    password,
  }  
const handleSubmit = async (e) => {
    e.preventDefault()
    const { data } = await loginUser(userData)
    saveTokenToCookie(data.token)
    saveUserToCookie(JSON.stringify(data.user))
    if (getTokenFromCookie() && getUserFromCookie()) {
      setIsLogin(true)
      navigate('/')
    }
  }
```

다시 클라이언트 측의 함수이다. 사용자 정보를 입력하고 로그인 버튼을 누르면 작동하는데, 해당 정보를 미리 정의된 userData 객체에 할당해서 위에서 살펴본 loginUser의 인수로 넘기고 있다. 그리고 반환받은 정보들을 쿠키에 저장하고, 저장이 완료되면 login 상태를 갱신시키는 로직이다.

로직이 정상적으로 작동하자 안도감이 먼저 들었지만, 다시 보니 ' 어...왜? 작동하지...?' 라는 생각이 들었다. 이런 생각이 들었던 이유는 내가 JSON 형식으로 데이터를 제대로 변환시키고 있지 않았기 때문이다. 그래서 axios나 서버를 만들기 위해 사용한 Express에서 뭔가 자동으로 처리해주는 부분이 있겠다는 생각이 들었다.

#### POST JSON with Axios

> If you pass a JavaScript object as the 2nd parameter to the `axios.post()` function, Axios will automatically serialize the object to JSON for you. Axios will also set the `Content-Type` header to `'application/json'`, so web frameworks like Express can automatically parse it.

```js
// Frontend - axios.js

function loginUser(userData) {
  return instance.post('/login', userData)
}
```

위에서 살펴본 loginUser 함수의 일부분이다. 2번째 인자로 사용자 정보를 넘겨주고 있는데, 실제로 나는 JSON 형식이 아닌 일반 JS 객체를 넘기고 있다. 다행히도 Axios는 post 메서드의 두 번째 인자에 들어온 JS 객체를 JSON 형태로 변환시켜준다고 한다. 심지어 header에 content-type도 자동으로 `application/json`으로 설정해준다. 사실 header도 내가 정의할 필요가 없었던 것이다.

#### Express json()

그럼 이제 post 요청이 넘어왔을 때의 상황을 살펴보자.

```js
//Backend - userController.js

const { email, password } = req.body
```

userController.js를 다시 확인해보면 parse, 그러니까 다시 원래의 JS 객체로 변환시키는 작업없이 바로 비구조화 할당을 통해 객체의 속성들에 접근하고 있는 걸 확인할 수 있다. 생각해보면 굉장히 부자연스러운 상황이다. 분명 req.body에 담긴 데이터는 JSON 형태의 데이터일텐데 바로 객체의 속성에 접근할 수 있다니?

```js
//Backend - server.js

app.use(express.json())
```

위에서도 잠깐 언급했지만 서버를 만들기 위해 Express 프레임워크를 사용했고, json() 이라는 Express의 내장 미들웨어를 사용했다.

> This is a built-in middleware function in Express. It parses incoming requests with JSON payloads and is based on body-parser.

json()은 JSON 데이터와 함께 온 요청들을 parse한다. 그러니까 userController에서 req.body에 접근할 때는 이미 JS 객체로 변환된 상태인 것이다.

#### Express res.json()

> Sends a JSON response. This method sends a response (with the correct content-type) that is the parameter converted to a JSON string using JSON.stringify().

```js
//Backend - userController.js

return res.json({
    token,
    user: {
    email: dbUserData.email,
    username: dbUserData.username,
    },
    result: 'success',
})
```

response 객체를 보낼 때도 마찬가지로 나는 JSON이 아닌 일반 JS 객체를 보내고 있다. 그럼에도 Express는 너그럽게 모든 걸 받아주고 있다. Express response 객체의 json 메서드는 response 객체를 올바른 content-type과 함께 JSON 형태로 변환해서 보내준다. json 메서드만 사용한다면 header와 데이터 유형에는 구애받지 않아도 되는 것이다.

#### Get the HTTP Response Body with Axios

> Axios parses the response based on the HTTP response's `Content-Type` header. When the response's content type is `application/json`, Axios will automatically try to parse the response into a JavaScript object.

```js
// Frontend - Login.js

const handleSubmit = async (e) => {
    e.preventDefault()
    const { data } = await loginUser(userData)
```

자, 이제 마지막 부분이다. 클라이언트 측에서 response를 받을 때, Axios는 response 헤더의 content-type이 `application/json`일 경우 자동으로 JSON을 JS 객체로 변환시킨다.  정말 수많은 실수들을 했지만, Axios와 Express의 도움으로 로직이 아무런 문제없이 작동하게 된 것이다. 

# 😎마무리

이전에 CS50이라는 하버드대학의 컴퓨터과학 강의를 들었는데, 교수님이 계속 강조하던 부분이 '컴퓨터는 마법이 아니다'라는 것이었다. 나도 맨 처음에 위에서 이 로직이 작동할 때는 진짜 신기하고 마법같았지만 이렇게 하나씩 살펴보니 결국 모든 게 다 구현되어 있기 때문에 가능한 것이라는 걸 알게 되었다. 앞으로도 로직이 작동된다고 그냥 넘어가기보다는 그 아래에서 무슨 일들이 일어나는지 꼭 살펴봐야겠다.

# :books:참고자료

https://developer.mozilla.org/ko/docs/Glossary/JSON

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/JSON#javascript%EC%99%80_json%EC%9D%98_%EC%B0%A8%EC%9D%B4

https://masteringjs.io/tutorials/axios/response-body

https://masteringjs.io/tutorials/axios/post-json

https://www.digitalocean.com/community/tutorials/nodejs-req-object-in-expressjs