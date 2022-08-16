# Vue

## Vuetify

- Vue 기반 UI 프레임워크
- 구글의 Material Design을 기반으로 설계됨

### 장점

- 생산성이 높다.
- 일관성이 있어 유지보수가 용이하다.
- 지식이 부족해도 퀄리티있게 만들 수 있다.
- 코드에 대한 재사용성이 높다.

### 설치하기

> The current version of Vuetify does not support Vue 3. Support for Vue 3 will come with the release of [Vuetify v3](https://vuetifyjs.com/en/introduction/roadmap/#v30-titan). When creating a new project, please ensure you selected Vue 2 from the Vue CLI prompts, or that you are installing to an existing Vue 2 project.

`Vue 3`는 아직 `Vuetify`를 지원하지 않는다는 점을 주의하자.

#### Vue-cli 활용

```bash
vue add vuetify
```

12column 시스템



### [#](https://vuetifyjs.com/en/components/grids/#v-container)v-container

`v-container` provides the ability to center and horizontally pad your site’s contents

너비값이 반응형

화면에 따라 길이가 달라짐

### [#](https://vuetifyjs.com/en/components/grids/#v-col)v-col

v-col` is a content holder that must be a direct child of `v-row

cols

Sets the default number of columns the component extends. Available options are **1 -> 12** and **auto**.



### [#](https://vuetifyjs.com/en/components/grids/#v-row)v-row



`v-row` is a wrapper component for `v-col`. It utilizes flex properties to control the layout and flow of its inner columns

uses a standard gutter of **24px**

no-gutters로 gutter 없애기 가능

### [#](https://vuetifyjs.com/en/components/grids/#v-spacer)v-spacer

컴포넌트 사이 간격 주기 위한 컴포넌트

`v-spacer` is a basic yet versatile spacing component used to distribute remaining width in-between a parents child components



#### [#](https://vuetifyjs.com/en/components/grids/#align)Align



Change the vertical alignment of flex items and their parents using the **align** and **align-self** properties.



- start
- center
- end



Aligh-self

본인 자신만 정렬

[#](https://vuetifyjs.com/en/components/grids/#breakpoint-sizing)Breakpoint sizing

반응형 디자인을 쉽게 할 수 있음

#### [#](https://vuetifyjs.com/en/components/grids/#justify)Justify



Change the horizontal alignment of flex items using the **justify** property.

- start
- end
- center
- space-between
- space-around



v-app

최상단에 선언해야 하는 루트 컴포넌트

id가 app인 div로 치환이 됨



v-main

레이아웃을 제외한 컨텐츠 영역

main 태그로 치환됨





v-app-bar, v-footer

app 속성 추가해서

기본 레이아웃 컴포넌트로 사용가능



v-main 컴포넌트

vue DOM 마운트 될 때

앱바나 내비게이션 등의 레이아웃 컴포넌트들의

높이와 너비를 프레임워크 코어에 등록

v-main은 레이아웃 높이와 너비를 취하여 콘텐츠의 영역을 조절





breakpoint

  {{ *$vuetify**.**breakpoint**.*name }}

뷰포트

애플리케이션 화면 크기



| Device      | Code   | Type                   | Range              |
| :---------- | :----- | :--------------------- | :----------------- |
| Extra small | **xs** | Small to large phone   | < 600px            |
| Small       | **sm** | Small to medium tablet | 600px > < 960px    |
| Medium      | **md** | Large tablet to laptop | 960px > < 1264px*  |
| Large       | **lg** | Desktop                | 1264px > < 1904px* |
| Extra large | **xl** | 4k and ultra-wide      | > 1904px*          |

```
<template>
  <v-container fluid>
    <v-card-title>
      $vuetify.breakpoint.name : {{ $vuetify.breakpoint.name }}</v-card-title
    >
    <v-card outlined :height="height"></v-card>
  </v-container>
</template>

<script>
export default {
  computed: {
    height() {
      switch (this.$vuetify.breakpoint.name) {
        case 'xs':
          return 100
        case 'sm':
          return 200
        case 'md':
          return 300
        case 'lg':
          return 400
        case 'xl':
          return 500
        default:
          return 500
      }
    },
  },
}
</script>

<style></style>

```

$vuetify.breakpoint.mobile

mobile 너비 이하일 경우에는 true

이상은 false 반환