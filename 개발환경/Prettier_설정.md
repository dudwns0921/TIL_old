# Prettier 설정

## Prettier

프리티어(Prettier)는 코드 스타일을 정리해주는 도구이다. VSCode(Visual Studio Code), Atom, Sublime 등 대중적인 텍스트 편집기에서 이미 플러그인 형태로 지원하고 있으며 VSCode에서는 아래와 같이 확장 플러그인으로 설치할 수 있다.

### Prettier 적용하기

먼저 플러그인 형태로 Prettier를 적용하는 방법에 대해서 알아보겠다. 참고로 VScode기준임을 미리 밝힌다. 먼저 Extensions에 들어가 Prettier을 검색하고 설치해준다.

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode"
  "editor.formatOnSave": true,
  "editor.formatOnType": true,
}
```

그 다음에 사용자 설정 파일인 settings.json 파일에다가 위 코드를 작성해준다. 

먼저 기본 포매터를 Prettier로 설정해준다. 그 다음에 코드를 작성하고 저장 버튼을 눌렀을 때 자동으로 코드를 정리하도록 두 속성을 정의해준다.

![](C:\Users\220307\AppData\Roaming\marktext\images\2022-03-31-10-59-56-image.png)

사용자 설정 파일은 윈도우 기준으로 Ctrl+, 을 눌러 Settings 창을 띄운 후 오른쪽 맨 위에 보이는 문서 모양 아이콘을 선택해서 들어갈 수 있다.

그리고 프로젝트 안에다가 .prettierrc 파일을 만들고 그 안에다가 본인이 설정하고 싶은 옵션들을 정의해준다. 

```json
{
  "singleQuote": true,
  "semi": true,
  "useTabs": false,
  "tabWidth": 2,
  "trailingComma": "all",
  "printWidth": 80
}
```

이렇게 플러그인으로 간단하게 코드를 정리할 수도 있지만 많은 개발자분들이 ESLint와 결합하여 프로젝트 설정 파일로 관리하는 것을 추천한다. 왜냐하면 팀원들이 모두 동일한 텍스트 편집기와 설정을 사용한다는 보장이 없기 때문이다.

https://joshua1988.github.io/web-development/vuejs/boost-productivity/



preset : plugin들의 집합

dedicated config files

각각 도구 파일 생성됨

vue 3점대 후반부터 eslint 오류 화면 덮는 현상 발생

eslint는 올바른 코드를 작성하는 걸 도와주는 도구일뿐 / eslint의 오류는 프로젝트의 실행과는 관련이 없음

vue.config.js에서 devServer 객체의 overlay 속성을 false로 두는 것으로 오버레이 문제를 해결할 수 있다.

## ESLint 소개

린트(ESLint)는 잘못된 코드 스타일로 인해 에러가 나지 않게 코드 문법을 잡아주는 문법 검사기입니다. 문장 뒤에 자동으로 세미콜론, 콤마를 붙여주기도 하고 의미 없는 변수, API 사용에 대해 경고해주는 등 여러 문법 오류에 대해서 미리 알려주죠. 가급적 덜 에러가 나는 코드를 작성하면 자연스럽게 버그도 줄어들기 때문에 서비스 품질을 높이는데도 도움이 됩니다.

no-console

개발할 때는 상관없음

배포 모드일 때는 error

개발 모드일 때는 off

설정파일들을 바꿨을 때는 서버를 재실행해야 한다.

프리티어랑 같이 써야하는 이유

vue cli로 프로젝트를 생성할 때 eslint+prettier 세팅으로 만들었기 때문에

프리티어 설정 파일을 개별적으로 만들어서 설정할 경우 충돌이 일어날 수 있음

eslintrc 파일이 우선시되어야 하므로 prettier 설정들은 해당 파일에 들어가야함

프리티어 속성

off, warn error

eslint 로그 레벨

프리티어에 걸리는 건 모두 error로 설정

프리티어 플러그인

format on save

꺼야지 원하는 방식대로 포매팅 가능해짐
