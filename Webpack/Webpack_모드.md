# Webpack 모드

웹팩 모드에는 다음과 같이 3가지 모드가 존재한다.

| mode        | 설명              |
| ----------- | --------------- |
| development | 개발 모드           |
| production  | 배포 모드(기본 값)     |
| none        | 기본 최적화 옵션 설정 해제 |

> "production" 모드는 Webpack 모듈 번들링 과정에서 자체적으로 코드를 최적화하여 용량을 줄인다. 

## 비교

각 모드의 번들링 결과를 살펴보자. 모드에 따라 파일 크기가 달라진다. 

### development 모드

개발 모드로 번들링 할 경우 출력된 결과물의 파일 크기는 **40KB**이다.

![](https://968663149-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MTbexhuB2p5ev8QaGgg%2F-MTiMvSVbjQTZ-lP-_ya%2F-MTiNfG-G4dScgzJBsk9%2Fimage.png?alt=media&token=8a41a3b6-9e80-4637-97a2-71d34ffad7a9) 

### none 모드

설정 없음 모드로 번들링 할 경우, 출력된 결과물의 파일 크기는 **35KB**이다.

![](https://968663149-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MTbexhuB2p5ev8QaGgg%2F-MTiMvSVbjQTZ-lP-_ya%2F-MTiODAa3e2tZ5vb1FrR%2Fimage.png?alt=media&token=d552ca96-8792-4578-8568-68a1734925da) 

### production 모드

배포 모드로 번들링 할 경우 출력된 결과물의 파일 크기는 **12KB**이다.

![](https://968663149-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-MTbexhuB2p5ev8QaGgg%2F-MTiMvSVbjQTZ-lP-_ya%2F-MTiO3hohx8ZDdo8MvXc%2Fimage.png?alt=media&token=75b89a9e-84c4-4fc3-ab8c-4fdb81fa6516)

## 참고

```javascript
if (process.env.NODE_ENV === "production") {
  module.exports.devtool = "#source-map";
  // http://vue-loader.vuejs.org/en/workflow/production.html
  module.exports.plugins = (module.exports.plugins || []).concat([
    new webpack.DefinePlugin({
      "process.env": {
        NODE_ENV: '"production"',
      },
    }),
    new webpack.optimize.UglifyJsPlugin({
      sourceMap: true,
      compress: {
        warnings: false,
      },
    }),
    new webpack.LoaderOptionsPlugin({
      minimize: true,
    }),
  ]);
}
```

위 코드는 vue-cli를 통해 생성한 프로젝트의 webpack.config.js의 일부이다. 웹팩 버전 4 이전에는 위와 같이 각 모드에 알맞게 플러그인을 사용해주어야 했다. 하지만 웹팩 버전 4 이후 mode  속성이 생겨 간단하게 값을 설정해주는 것만으로도 위의 설정을 한 번에 처리할 수 있게 되었다. 

이런 모드 속성이 생긴 데에는 Parcel의 등장과 관련이 있다. Parcel은 Webpack에 비해 매우 늦게 나왔지만 Webpack 보다 빠르고 설정이 필요 없는 부분에서 장점이 있다. 이러한 Parcel과 경쟁하기 위해 Webpack에서 mode 속성이 추가된 버전 업을 좀 빠르게 진행했다고 한다.



# :books:참고자료

[Webpack 모드 - Webpack 러닝 가이드](https://yamoo9.gitbook.io/webpack/webpack/config-webpack-dev-environment/webpack-mode)

인프런 - 프론트엔드 개발자를 위한 웹팩 강의 내용


