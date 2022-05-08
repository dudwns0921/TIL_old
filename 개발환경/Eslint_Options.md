# ESLint

## ESLint 옵션

최근 이전에 했던 React 프로젝트 리팩토링을 진행하며 ESLint를 적용시켰다. 설정 파일은 다음과 같다.

```json
{
    "env": {
        "browser": true,
        "es2021": true,
        "node": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended",
        "plugin:prettier/recommended",
        "plugin:react/jsx-runtime"
    ],
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": 2017,
        "sourceType": "module"
    },
    "plugins": [
        "react"
    ],
    "rules": {
        "react/prop-types": 1,
        "no-unused-vars": 1,
        "react/jsx-key": 1
    }
}

```

npm init @eslint/config를 통해 설정 파일을 자동으로 생성했다. 여기서 개인적으로 추가한 부분들에 대해서 설명해보겠다.

### env

env는 사전 정의된 전역 변수 사용을 정의한다. 해당 옵션에 `"node" : true`를 추가했는데, 그 이유는 process 전역 변수를 사용하기 위함이었다.

리팩토링한 프로젝트는 영화 정보 api를 사용하고 있는데, api key가 소스코드 내에 하드코딩되어있어 보안이 취약하다고 판단해 이를 해결하고자 .env 파일을 활용했다. env 파일은 간단하게 보안이 필요한 값들을 저장하는 파일이라고 생각하면 된다. .env 파일에 접근하기 위해서는 process 전역 변수가 필요했고, 이를 위해  `node: true` 옵션을 추가한 것이다. 

### extends

extends는 추가한 플러그인에서 사용할 규칙들을 설정한다.

eslint는 3rd party 플러그인 사용을 지원한다. 플러그인은 일종의 규칙들의 집합이라고 할 수 있고, 플러그인을 추가한다고 해서 규칙이 적용되는 것은 아니다. 규칙을 사용하기 위해서는 추가한 플러그인 중, 사용할 규칙을 extends에 설정해주어야 한다.

내가 추가로 설정한 규칙들은 아래와 같다.

- #### plugin:prettier/recommended

1. Prettier와 충돌하는 ESLint의 규칙들 비활성화
2. Prettier 플러그인 등록
3. ESLint에서 Prettier 플러그인이 제공하는 규칙들을 추가해 해당 규칙들에 어긋날 경우 에러 메시지 띄움
4. Prettier 플러그인과 문제가 발생하는 ESLint의 두 가지 규칙을 비활성화

- #### plugin:react/jsx-runtime

https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/react-in-jsx-scope.md

위 링크의 내용을 정리하자면 React17에서 새롭게 등장한 JSX를 사용한다면, JSX를 사용할 때 무조건 react를 import할 필요가 없다. 따라서  JSX를 사용할 경우 react를 import하는 구문이 있어야 한다는 ESLint의 규칙을 위 규칙을 추가함으로써 비활성화하라는 내용이다.

### parserOptions

parserOptions은 ESLint 사용을 위해 지원하려는 Javascript 언어 옵션을 지정할 수 있다. 

처음에 자동으로 설정된 옵션은 ` "ecmaVersion": "latest"`였는데, 아래와 같은 에러가 발생했다.

> Parsing error: ImportDeclaration should appear when the mode is ES6 and in the module context

그러니까 Import 구문은 mode가 ES6이고, ESM이 지원되는 환경 속에서 사용되어야 한다는 의미이다. 해결 방법은 간단했다. 그냥 ecmaVersion을 2021로 바꿨더니 에러가 더 이상 발생하지 않았다. 확실하지는 않지만 개인적으로는 최신 버전이라 안정성이 보장되지 않는 탓에 저런 에러가 발생한 게 아닌가 싶다. 그래서 버전을 낮추니 문제가 해결된 것이라고 생각한다.

### rules

ESLint에는 프로젝트에서 사용하는 규칙을 수정할 수 있다. 규칙을 변경하는 경우, 다음과 같은 방법으로 설정해야 한다.

- `"off"` 또는 `0`: 규칙을 사용하지 않음
- `"warn"` 또는 `1`: 규칙을 경고로 사용
- `"error"` 또는 `2`: 규칙을 오류로 사용

위 3개 규칙에 어긋나는 코드가 너무 많아서 error가 아닌 warn으로 설정해 프로젝트 실행에는 문제가 없도록 처리했다.

## 마무리

솔직히 처음에 ESLint를 설정하고 사용한 적은 많았어도, 이렇게 도중에 사용해본 적은 처음이었다. ESLint를 적용하니 거의 모든 파일을 수정해야 했기 때문에, 처음에 설정을 잘해야겠다는 생각이 정말 많이 들었다. 그래도 이번 과정을 통해 ESLint에 대해 좀 더 자세히 알게 된 거 같아 다행이라고 생각한다.

# :books:참고자료

https://eslint.org/

https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/react-in-jsx-scope.md