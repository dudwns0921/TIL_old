# 개발자 도구들

## Webpack Dev Server

웹팩 데브 서버는 웹 애플리케이션을 개발하는 과정에서 유용하게 쓰이는 도구이다. 웹팩의 빌드 대상 파일이 변경 되었을 때 매번 웹팩 명령어를 실행하지 않아도 코드만 변경하고 저장하면 웹팩으로 빌드한 후 브라우저를 새로고침 해준다.

매번 명령어를 치는 시간과 브라우저를 새로 고침하는 시간 뿐만 아니라 웹팩 빌드 시간 또한 줄여주기 때문에 웹팩 기반의 웹 애플리케이션 개발에 필수로 사용된다.

### 설치하기

```bash
npm install --save-dev webpack-dev-server
```

### 웹팩 데브 서버의 특징

```javascript
"scripts": {
  "dev": "webpack serve",
  "build": "webpack"
}
```

웹팩 데브 서버를 실행하여 웹팩 빌드를 하는 경우에는 빌드한 결과물이 파일 탐색기나 프로젝트 폴더에서 보이지 않는다. 좀 더 구체적으로 얘기하자면 웹팩 데브 서버로 빌드한 결과물은 메모리에 저장되고 파일로 생성하지는 않기 때문에 컴퓨터 내부적으로는 접근할 수 있지만 사람이 직접 눈으로 보고 파일을 조작할 순 없다.

따라서, 웹팩 데브 서버는 개발할 때만 사용하다가 개발이 완료되면 웹팩 명령어를 이용해 결과물을 파일로 생성해야 한다.

컴퓨터 구조 관점에서 파일 입출력보다 메모리 입출력이 더 빠르고 컴퓨터 자원이 덜 소모된기 때문에 개발 과정에서는 데브 서버를 통해 결과물을 메모리에만 저장하는 것이 개발 생산성을 향상시킬 수 있다.

## 소스 맵

소스 맵(Source Map)이란 배포용으로 빌드한 파일과 원본 파일을 서로 연결시켜주는 기능이다. 보통 서버에 배포를 할 때 성능 최적화를 위해 HTML, CSS, JS와 같은 웹 자원들을 압축한다. 그런데 만약 압축하여 배포한 파일에서 에러가 난다면 어떻게 디버깅을 할 수 있을까?

정답은 바로 소스 맵을 이용해 배포용 파일의 특정 부분이 원본 소스의 어떤 부분인지 확인하는 것이다. 이러한 편의성을 제공하는 것이 소스 맵이다.

### 소스 맵 설정하기

웹팩에서 소스 맵을 설정하는 방법은 아래와 같다.

```javascript
// webpack.config.js
module.exports = {
  devtool: 'cheap-eval-source-map'
}
```

`devtool` 속성을 추가하고 소스 맵 설정 옵션 중 하나를 선택해 지정해주면 된다.

# :books: 참고자료

[Webpack Dev Server | 웹팩 핸드북](https://joshua1988.github.io/webpack-guide/devtools/webpack-dev-server.html)
