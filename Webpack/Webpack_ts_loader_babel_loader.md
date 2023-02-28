# Webpack

## babel-loader와 ts-loader

타입스크립트 사용을 위해 웹팩을 설정할 때, 크게 두 가지 방법이 있다.

- ts-loader
- babel-loader(preset-typescript)

두 로더의 차이는 빌드 과정에 있다.

- babel-loader는 type과 관련된 구문을 무시(제거)한 채 빌드를 진행

- ts-loader는 모든 구문을 파악, 타입들을 체크해가면서 빌드를 진행

|        | babel-loader | ts-loader |
| ------ | ------------ | --------- |
| 안정성 | X            | O         |
| 속도   | O            | X         |

## :bulb:Tip - typescript의 트랜스파일링

나는 typescript가 typescript 코드를 javascript 코드로 변환하는 트랜스파일링을 수행한 후에도 babel을 통해 추가적인 트랜스파일링이 이루어져야 브라우저 호환성의 문제가 없을 것으로 생각했다.

하지만 typescript는 tsconfig의 target 속성의 값에 따라 트랜스파일링을 하고 있으며, target의 default 값이 es3로 되어있다. es3는 모든 브라우저에서 사용 가능하기 때문에 babel로 트랜스파일링 작업을 추가로 할 필요는 없다.

**트랜스파일링** : 한 언어로 작성된 소스 코드를 비슷한 수준의 추상화를 가진 다른 언어로 변환

**컴파일링** : 한 언어로 작성된 소스 코드를 다른 언어로 변환
