# JWT

## jsonwebtoken

> https://github.com/auth0/node-jsonwebtoken#readme

간단하게 정리하자면, JWT를 좀 더 쉽게 만들고 검증할 수 있도록 해주는 라이브러리이다. 더 자세한 내용은 위 링크를 참조하자.

## 토큰 만들기

```bash
npm i jsonwebtoken
```

jsonwebtoken으로 토큰을 만들 때 필요한 데이터는 다음과 같다.

- 비밀키
- 해시를 위해 사용되는 데이터(특정 사용자를 구별할 수 있는 user ID와 같은 데이터)
- 토큰 만료 시간

실제 JWT를 구성하는 데이터와는 조금 다르긴 하지만 결국 세 가지 데이터를 조합한 후 비밀키를 통해 암호화하고 검증한다는 점에서 개념적인 차이는 없다.

### 비밀키

비밀키를 만드는 한 가지 방법 중 하나는 Node.js의 내장 라이브러리인 `crypto`를 사용하는 것이다. 

```js
import crypto from 'crypto'
crypto.randomBytes(64).toString('hex')
```

사실 한 번 만든 후에 그 값을 계속 활용하는 것이기 때문에, 위 코드는 일회성이라고 생각하면 된다.

#### .env

이렇게 만든 비밀키는 보안을 위해 `.env `파일에 저장하는 것이 좋다. 솔직히 말하면 토이 프로젝트를 만들 때 굳이 이런 작업까지 해야 하나 싶지만, 실제 서비스에서 이런 작업들이 이루어진다는 정도는 알아두면 좋다고 생각한다.

```
// .env
SECRET_KEY=5fcc64b236328b2...
```

env 파일에 비밀키를 정의했으면, 이제 프로젝트에서 직접 쓰기 위해 로드할 필요가 있다. Vue나 React에서는 특정 접두사를 붙이는 것만으로 쉽게 로드가 가능했지만, 프레임워크가 없는 상태에서는 직접 로드를 해줄 필요가 있다. 이를 위해 사용하는 것이 `dotenv`이다.

#### dotenv

`dotenv`는 `.env` 파일에 저장된 환경변수를 `process.env`로 로드하는 라이브러리이다. 사용방법은 아래와 같다.

```js
import dotenv from 'dotenv'

// get config vars
dotenv.config();

// access config var
process.env.TOKEN_SECRET;
```

그러면 이제 실제로 토큰을 만들어보자.

```js
const generateAccessToken = (userEmail) => {
  return jwt.sign({ userEmail }, process.env.SECRET_KEY, {
    expiresIn: process.env.EXPIRATION_DATE,
  })
}
```

실제로 만들어본 토큰 생성 함수이다. 해쉬를 위한 데이터로는 userEmail을 사용했고, 비밀키와 만료 시간은 모두 `.env` 파일에서 불러오고 있다.

## 토큰 검증하기

```js
const authenticateToken = (req, res, next) => {
  const headerAuth = req.headers.authorization
  const token = headerAuth && headerAuth.split(' ')[1]
  if (token === null) return res.status(400).send()
  jwt.verify(token, process.env.SECRET_KEY, (e) => {
    if (e) {
      console.log(e)
      return res.status(403)
    }
    next()
  })
}
```

토큰 검증을 위한 함수를 실제로 만들어봤다. 토큰 검증을 하는 여러 가지 방법이 있겠지만 한 가지 방법은 Express.js의 미들웨어 기능을 활용하는 것이다. 위 함수를 제대로 이해하기 위해서는 클라이언트 사이드에서 요청 헤더가 어떻게 구성되어있는지 확인할 필요가 있다.

```js
  headers: {
    'Content-Type': 'application/json; charset=UTF-8',
    Authorization: `Bearer ${token}`,
  },
```

클라이언트 사이드에서 보내는 요청의 헤더 부분만을 가져왔다. 실제로는 토큰을 받아온 후에 데이터가 삽입되어야 하므로 위 코드처럼 하드코딩하기보다는 axios의 interceptor 기능을 사용하는 것이 일반적이다. 추가적으로 여기서 Bearer은 인증 타입을 나타내는 키워드로 'JWT 혹은 OAuth에 대한 토큰을 사용한다.' 는 의미를 갖고 있다.

자, 여기서 다시 한 번 토큰 검증 방식의 과정을 상기시켜보자.

1. 유저가 아이디와 비밀번호로 **로그인**을 한다
2. 서버측에서 해당 **계정정보를 검증**한다.
3. 계정정보가 정확하다면, 서버측에서 유저에게 **토큰을 발급**한다.
4. 클라이언트 측에서 전달받은 **토큰을 저장**해두고, 서버에 요청을 할 때 마다, 해당 **토큰을 함께 서버에 전달**한다.
5. 서버는 **토큰을 검증**하고, **요청에 응답**한다.

그러니까 이미 인증을 통해 클라이언트 사이드에서 토큰값을 받은 것이며, 인증이 필요한 api 요청의 헤더에 토큰값을 담아 전달하고 있는 상황인 것이다. 

```js
  const headerAuth = req.headers.authorization
  const token = headerAuth && headerAuth.split(' ')[1]
```

다시 백엔드로 돌아가서 코드를 분석해보겠다. 

1. 프론트에서 온 요청 헤더에서 토큰값이 담긴 속성인 authorization의 값을 headerAuth 변수에 담는다.
2. headerAuth의 값이 있을 경우 split 함수를 통해 만들어진 배열의 두 번째 값을 가져온다. 설명을 덧붙이자면 인증 타입 키워드로 Bearer을 썼기 때문에 토큰값만을 가져오기 위한 절차라고 생각하면 된다.

```js
  if (token === null) return res.status(400).send()
  jwt.verify(token, process.env.SECRET_KEY, (e) => {
    if (e) {
      console.log(e)
      return res.status(403)
    }
    next()
  })
```

4. 토큰값이 없으면 잘못된 요청이라는 400 status를 보낸다.

5. 토큰값이 있는 경우, jsonwebtoken의 verify 메서드를 통해 토큰을 검증한다.

6. 검증할 때 에러가 발생할 경우 권한에 문제가 있다는 403 status를 반환하고, 그렇지 않으면 다음 함수로 제어를 넘기는 next() 메서드를 실행시킨다.

# :books:참고자료

https://github.com/auth0/node-jsonwebtoken#readme

https://www.digitalocean.com/community/tutorials/nodejs-jwt-expressjs