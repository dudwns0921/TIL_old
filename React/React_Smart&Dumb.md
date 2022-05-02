# React

## Dumb Components & Smart Components

React를 배우기 시작하면 우리는 자연스럽게 컴포넌트와 마주하게 된다. 컴포넌트는 하나의 부품과 같은 역할로, 각각의 컴포넌트들이 모여 하나의 커다란 애플리케이션을 이루게 된다. 보기에는 간단한 개념같지만, 사실 컴포넌트는 조금 더 복잡하다.

컴포넌트에는 Dumb Components와 Smart Components, 총 두 가지 종류가 있다. 각각에 대해서 좀 더 자세히 알아보자.

### Dumb Components

Dumb Components는 'Presentational' components라고도 불리는데, 그 이유는 그들의 유일한 목적이 바로 DOM에 무언가를 보여주기 위함이기 때문이다.

이 컴포넌트는 render 메서드만을 가지고 있다. Dumb Components는 내부의 관리해야 할 state를 가지고 있지 않다. 다시 말하면, 그들이 보여주고 있는 데이터에 대해 변경을 요청받았을 때 어떻게 바꾸는지 알지 못한다.

```javascript
const Footer = (props) => {
  return(
  <div>
    <ul>
      <li>Footer Information</li>
    </ul>
  </div>
  )
}
```

위와 같은 컴포넌트가 가장 대표적인 Dumb Components의 예시라고 할 수 있다. 이 컴포넌트들은 작성된 대로 렌더링할 뿐이며 이 컴포넌트를 변경하고 싶다면 컴포넌트가 작성된 파일로만 가면 된다.

### Smart Components

이와 달리, Smart Components는 state를 추적하며 애플리케이션이 어떻게 작동하는지에 대해 상관한다.

Presentational & Container design pattern을 사용할 경우, Container component가 Smart Components의 대표적인 예시라고 할 수 있다.

## :bulb:Tip

## Presentational & Container 디자인 패턴이란?

로직을 수행하는 컴포넌트와 UI를 렌더링하는 컴포넌트가 분리된 패턴이다. 둘이 구분되어 있기 때문에 애플리케이션의 유지보수가 쉽고, 컴포넌트들의 재사용성이 높아진다.

Presentational components는 앞에서 살펴본 Dumb Components에 해당하며, Container components는 앞에서 살펴본 Smart Components에 해당한다.

___

Container Components는 로직을 수행하고, 그 데이터를 Presentational Components에 prop으로 전달해준다. Container Components는 주로 state를 갱신하고 prop으로 전달해주기 위한 다른 콜백 함수를 가지고 있다.

# :books:참고자료

https://medium.com/@thejasonfile/dumb-components-and-smart-components-e7b33a698d43
