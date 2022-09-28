# ESLint

## Configuration

### plugins

> ***공식 문서***
>
> *`plugins`  속성 값은 사용할 `plugin`이름의 문자열인데, 이 때 사용할 ` plugin` 이름의 `eslint-plugin` 접두사를 생략할 수 있다. 그리고 적용할 `plugin`이 여러 개라면 `plugins` 의 값은 여러 개의 문자열로 이루어진 배열이 된다.* 
>
> ```json
> {
>   // eslint-plugin-vue의 접두사 생략
>   "plugins": ["vue"]
> }
> ```
>
> ***+ 추가 설명***
>
> *`plugin`은 `ESLint`에 다양한 `extension`을 추가할 수 있는 `npm package`이다. 플러그인은 새로운 규칙을 추가하거나, 공유 가능한 설정을 `export` 하는 등의 다양한 기능을 수행할 수 있다.*

쉽게 말하면 `plugin`은 `ESLint`에서 기본적으로 제공하는 규칙들 외에도 부가적인 규칙들을 사용할 수 있도록 만들어준다고 생각하면 된다.

주의할 점은 `plugins` 속성에 `plugin`만 추가한다고 해서 관련 규칙들이 바로 활성화되는 것이 아니라는 점이다. 이는 단순히 `plugin`이 포함하고 있는 규칙들을 설정이 가능한 상태로 만들어줄 뿐이다. 그래서 `plugin`의 규칙들을 활성화하고 싶다면 `extends`나 `rules`속성에서 추가로 설정을 해주어야 한다. 

### extends

> ***공식 문서***
> *`extends` 속성 값은 특정 설정을 명시하는 문자열이다. 이 때 해당 문자열은 설정 파일에 대한 경로일 수도 있고, 공유 가능한 설정의 이름일 수도 있다. 이 때, 적용할 설정이 여러 개라면 `extends` 의 값은 여러 개의 문자열로 이루어진 배열이 된다.*
>
> ```json
> {
>     "extends": [
>         "./node_modules/coding-standard/eslintDefaults.js",
>         "./node_modules/coding-standard/.eslintrc-es6",
>         "./node_modules/coding-standard/.eslintrc-jsx"
>     ]
> }
> ```
>
> ```json
> {
>     "extends": [
>         "eslint:recommended",
>     ]
> }
> ```

다시 말하면, extends는 다른 사람들이 미리 등록해놓은 설정을 사용하게끔 해주는 속성이라고 보면 된다.

___

앞에서 `plugin`에서 규칙들을 추가하고, 해당 규칙들을 사용하기 위해서는 `extends`와 `rules`에서 추가 설정을 해주어야 한다고 했다. 하지만 `extends`에서 불러오는 설정에 `extends`, `plugins`, `rules`가 정의되어 있는 경우도 있기 때문에 단순히 설정 파일을 사용하는 사람 입장에서는 `plugin -> extends, rules` 의 흐름이 항상 적용되는 건 아니라는 점을 기억해두자.

```json
{
  "extends": ["plugin:prettier/recommended"]
}
// 위와 아래는 동일한 설정
{
  "plugins": ["prettier"],  
  "extends": ["prettier"],
  "rules": {
    "prettier/prettier": "error",
    "arrow-body-style": "off",
    "prefer-arrow-callback": "off"
  }
} 
```

### overrides

 `overrides` 속성은 프로젝트 내에서 일부 파일에 대해서만 살짝 다른 설정을 적용해줘야 할 때 사용한다. 예를 들어, 프로젝트에 자바스크립트 파일과 타입스크립트 파일이 공존한다면 자바스크립트 파일을 기준으로 기본 설정을 하고, 타입스크립트 파일을 위한 설정은 `overrides` 속성에 명시할 수 있다. 타입스크립트 확장자를 가진 파일에 대해서는 타입스크립트 용 파서와 플러그인과 추천 설정을 사용하도록 세팅해주고 있다.

```json
{
  "overrides": [
    {
      "files": "**/*.+(ts|tsx)",
      "parser": "@typescript-eslint/parser",
      "plugins": ["@typescript-eslint"],
      "extends": ["plugin:@typescript-eslint/recommended"]
    }
  ]
}
```

# :books:참고자료

https://eslint.org/

https://www.daleseo.com/eslint-config/