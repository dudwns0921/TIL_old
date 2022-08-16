# Node.js

## npm

### package.json

> You can add a package.json file to your package to make it easy for others to manage and install. Packages published to the registry must contain a `package.json` file.
>
> - lists the packages your project depends on
> - specifies versions of a package that your project can use using [semantic versioning rules](https://docs.npmjs.com/about-semantic-versioning)
> - makes your build reproducible, and therefore easier to share with other developers

개발자가 배포한 패키지에 대해, 다른 사람들이 관리하고 설치하기 쉽게 하기 위한 문서. npm에 패키지를 배포하고 npm registry에 올리기 위해서 반드시 필요한 문서파일이다.

- 자신의 프로젝트가 의존하는 패키지의 리스트
- 자신의 프로젝트의 버전을 명시
- 다른 환경에서도 빌드를 재생 가능하게 만들어, 다른 개발자가 쉽게 사용할 수 있도록 한다.

#### Properties

#### browser

`browser`프로퍼티는 아래의 정보를 담고 있다.

- 웹 애플리케이션이 타깃으로 하는 브라우저 정보
- 유지보수시에 사용해야 할 자바스크립트 런타임 환경인  Node의 버전

다른 라이브러리들이 브라우저에 대한 정보를 필요로 할 때 여기에 선언되어 있는 정보를 활용함.

```json
...
"browserslist": [
    "> 1%",
    "last 2 versions",
    "not dead"
  ]
...
```

위 예시는 `vue-cli`로 프로젝트 생성시 자동으로 등록되는 설정이다.

- 전세계 점유율이 1%이상인 브라우저
- 최근 2개 버전의 브라우저
- 지원이 중단된 브라우저는 제외

# :books:참고자료

https://hoya-kim.github.io/2021/09/14/package-json/