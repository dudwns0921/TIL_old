# JWT

## 개요

이전에 토큰 기반 인증 방식에 대해서 정리한 적이 있다.

> [Pre 토큰 기반 인증](./토큰_기반_인증.md)

토큰 기반 인증 방식을 공부하다보니 자연스럽게 JWT를 알게 되었는데, JWT에 대해 좀 더 자세히 알아보고자 한다.

> JSON Web Token (JWT) is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the **HMAC** algorithm) or a public/private key pair using **RSA** or **ECDSA**.

JWT는 당사자들간에 JSON 객체를 통해 정보를 전송하는 간단하고 완벽한 방법을 제공하는 개방형 표준이다. 이 정보는 전자 서명이 이루어지기 때문에 검증할 수 있고, 신뢰도가 높다. JWT는 RSA 혹은 ECDSA를 이용한 공개키, 개인키 혹은 HMAC 알고리즘을 사용하는 비밀키를 통해 서명될 수 있다.

JWT를 유용하게 사용할 수 있는 상황은 위에서도 언급했듯이 당사자들간에 정보를 전달하거나, 혹은 사용자에 대한 인증이 필요한 상황이다.

## 구조

JWT의 구조는 아래와 같다.

- Header
- Payload
- Signature

### Header

Header는 일반적으로 두 부분으로 나뉜다.

```json
{
	"alg": "HS256",
	"typ": "JWT"
}
```

alg은 알고리즘의 약자로 서명 방식에 어떤 알고리즘이 사용되었는지를 정의한다. typ은 토큰의 유형, 여기서는 JWT를 의미한다.

### Payload

```json
{
  "sub": "1234567890",
  "name": "Jun",
  "admin": true
}
```

token의 두 번째 부분인 payload는 claim을 포함하고 있다. claim이란 개체(일반적으로 사용자를 의미)나 추가적인 데이터에 대한 설명이다. 이러한 claim에는 총 3가지 종류가 있다.

#### Registered claims

Registered claims는 미리 정의된 claim들의 집합으로 필수는 아니지만 유용하다. 몇 가지 예시로는 iss(발행자), exp(만기 시간) 등이 있다.

#### Public claims

Public claims는 JWT를 사용하는 사람들에 의해 마음대로 정의될 수 있다. 다만 충돌을 피하기 위해 [IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml)에서 정의하거나 uri의 형태로 정의해야 한다.

```json
{
    "https://xxx.com/xxx/xx" : true
}
```

#### Private claims

Private claims는 사용자 정의 claims로 Registered claims과 Public claims에 포함되지 않아야 하며, 정보를 주고 받는 당사자들이 사용하겠다는 합의가 이루어져야 한다.

Header와 Payload는 Base64Url로 인코딩되어 각각 JWT의 첫 번째, 그리고 두 번째 부분을 형성한다.

## Signature

JWT의 마지막 부분은 바로 signature이다. 이 서명은 헤더의 인코딩값과, 정보의 인코딩값을 합친후 주어진 비밀키로 해시를 해 생성한다. 서명 부분을 만드는 pseudo code의 구조는 다음과 같다.

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```