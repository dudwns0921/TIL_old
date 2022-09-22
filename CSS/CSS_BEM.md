# CSS

## BEM 방법론

**BEM**은 `Block, Element, Modifier`의 약자이며, 여러 구성요소들을 그룹화하기 위한 간단하면서 매우 효과적인 방법이다. 예시를 통해 `Block, Element, Modifier`가 어떤 뜻을 갖고 있는지 알아보자.

![image-20220922154509002](md-images/image-20220922154509002.png)	

### Block

`Block`은 재사용 가능하며, 기능적으로 독립적인 컴포넌트이다. 위와 같은 `header` 전체가 `Block`에 해당한다고 보면  된다. 

### Element

`Element`는 `Block`을 구성하는 단위로, 위 header에서는 각 로고, 페이지 이동 텍스트 등이 이에 해당한다. `Block`은 독립적이라면, `Element`는 `Block`에 의존적인 형태를 띈다고 할 수 있다.

### Modifier

`Modifier`는 `Element`의 속성을 나타낸다.

## 사용해보기

위에서 예시로 든 `header`를 마크업하고 `class`명을 **BEM 방법론**에 따라 작성해보도록 하겠다. **BEM 방법론**의 작성법과 특징은 아래와 같다.

> **{Block}__{Element}--{Modifier}**

- class만을 사용
- 목적에 따른 네이밍
- 각 부분이 복합어가 되는 경우에 하이픈 하나만 써서 연결

```HTML
  <div class="header">
    <div class="header__logo"></div>
    <div class="header__navigation--navi-text"></div>
    <div class="header__navigation--navi-text"></div>
    <div class="header__navigation--navi-text"></div>
  </div>
```

보다시피 로고를 제외한 각 요소는 페이지 이동을 위한 목적을 가지고 있기 때문에 `logo`를 제외한 나머지 `Element`는 `navigation`이라고 작성해주었다. 그리고 텍스트라는 속성을 가지기에 `Modifier`을 `navi-text`로 작성했으며, 복합어이기 때문에 하이픈 하나를 사용해 연결했다.

# :books:참고자료

https://www.integralist.co.uk/posts/bem/

https://nykim.work/15





