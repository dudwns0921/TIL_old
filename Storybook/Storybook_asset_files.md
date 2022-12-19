# Storybook

## 정적 파일 사용하기

스토리북에서 컴포넌트를 렌더링할 때, images, videos, fonts 등의 asset 파일이 필요한 경우가 있다. 스토리북에서 asset 파일을 사용하는 데는 크게 두 가지 방법이 있다.

1. 각 스토리 파일에서 asset을 import 하는 방법
2. 설정 파일에서 staticDirs 속성에 asset 파일 경로 정의

2번의 경우 한 번 정의해놓으면 asset 파일에 대해 신경쓰지 않아도 되기 때문에 1번보다는 2번을 사용하기를 추천한다. 

```js
// .storybook/main.js

module.exports = {
  stories: [],
  addons: [],
  staticDirs: ['../public'],
};
```

아래와 같이 여러 개의 경로를 전달하는 것도 가능하다.

```js
// .storybook/main.js

module.exports = {
  stories: [],
  addons: [],
  staticDirs: ['../public', '../static'],
};
```

# :books:참고자료

https://storybook.js.org/docs/react/configure/images-and-assets