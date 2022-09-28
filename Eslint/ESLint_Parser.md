# Eslint

## Parser

> *위키백과*
>
> 컴퓨팅에서 **파서**(parser)는 [인터프리터](https://ko.wikipedia.org/wiki/인터프리터)나 [컴파일러](https://ko.wikipedia.org/wiki/컴파일러)의 구성 요소 가운데 하나로, 입력 토큰에 내재된 [자료 구조](https://ko.wikipedia.org/wiki/자료_구조)를 빌드하고 문법을 검사한다. 

설명이 좀 복잡하지만, 파서는 구문 분석을 하는 프로그램 정도로 정리할 수 있을 것 같다.

## ESLint의 Parser

ESlint는 [Espree](https://github.com/eslint/espree)를 기본 파서로 사용한다. 다른 파서를 선택해서 사용할 수도 있는데, 이 때 해당 파서는 아래 두 가지 조건을 만족해야 한다.

- 파서가 사용되는 설정 파일에서 불러올 수 있는 `Node module`이어야 한다.
- [parser interface](https://eslint.org/docs/latest/developer-guide/working-with-custom-parsers)를 따라야 한다.

다른 파서를 지정하기 위해서는 `ESLint` 정의 파일의 `parser` 옵션에 해당 파서를 정의하면 된다. 아래는 `Espree` 파서 대신에 `Esprima`를 사용하기 위한 설정 파일의 모습이다.

```json
{
    "parser": "esprima",
    "rules": {
        "semi": "error"
    }
}
```

##  Vue 3 프로젝트에서의 파싱 오류

`vue-cli`를 통해 `vue`프로젝트를 만들고 프로젝트에 맞게 `ESlint`설정 파일을 일부 수정했더니(`typescript` 및 `prettier plugin` 적용), `vue`파일의 `template`에서 아래와 같은 오류가 발생했다.

> Parsing error: '>' expected.eslint

위 오류는 `vue-eslint-parser`를 사용하는 것으로 위 문제를 해결할 수 있었다. `vue-cli`로 프로젝트를 생성하면 기본적으로 등록되는 파서로는 `vue`파일의 구문을 제대로 분석할 수 없어 `vue-eslint-parser`를 사용해야 하는 것 같다. 

# :books:참고자료

https://stackoverflow.com/questions/66597732/eslint-vue-3-parsing-error-expected-eslint

https://eslint.org/docs/latest/user-guide/configuring/plugins#specifying-parser

https://github.com/vuejs/vue-eslint-parser