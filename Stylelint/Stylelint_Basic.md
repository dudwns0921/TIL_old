# Stylelint

## 기초

> ***공식문서***
>
> *A mighty, modern linter that helps you avoid errors and enforce conventions in your styles.*

### 기본 설정

####  `npm`으로 `stylelint`와 [standard configuration](https://www.npmjs.com/package/stylelint-config-standard)를 설치

```shell
npm install --save-dev stylelint stylelint-config-standard
```

#### `.stylelintrc.json` 설정 파일을 프로젝트 루트 경로에다가 만들고, 파일에 아래와 같이 작성

```json
{
  "extends": "stylelint-config-standard"
}
```

___

### ➕추가 작업

#### `.stylelintignore`파일에 `stylelint`가 무시할 파일들 작성

```
/reset.css
```

___

#### 일반적으로는 빌드 전에 아래와 같이 명령어를 통해 프로젝트 전체를 lint한다.

```bash
npx stylelint "**/*.css"
```

하지만 현재 회사에서는 `VSC extension`으로 설치한 `stylelint`를 통해 수정하는 파일에 대해서만 `lint`를 적용해 `autofix` 되도록 적용했다. 팀과 협의가 된 상태가 아니며 프로젝트 전체에 `lint`를 적용할 경우 어떤 오류가 발생할지 모르기 때문이다.

#### VSC 설정 파일 수정

```json
// settings.json

{
  "editor.codeActionsOnSave": {
    "source.fixAll.stylelint": true
  },
  "stylelint.validate":[
    "css"
  ]
}
```

위와 같이` VSC 설정 파일`을 수정했다. 앞에서 이야기했지만, 이 방법은 `stylelint`가 `VSC extension`으로 설치되어있어야 한다는 점을 기억하자.

# :books:참고자료

https://stylelint.io/