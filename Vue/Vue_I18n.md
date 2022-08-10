# Vue

## I18n

Vue I18n은 Vue.js의 국제화 플러그인이다. 현지화 기능을 Vue.js 애플리케이션에 쉽게 통합할 수 있다.



## 회사 코드 파헤쳐보기

```js
import Vue from 'vue'
import VueI18n from 'vue-i18n'

import en from '../assets/locales/en-us.json'
import ko from '../assets/locales/ko.json'

const FALLBACK = 'en-us'
const currentLocale = (window.applicationFramework) ? window.applicationFramework.util.getLanguage() : FALLBACK

function loadLocaleMessage (locale, cb = () => {}) {
  const url = './locales/' + locale + '.json'
  const fileReader = new XMLHttpRequest()
  fileReader.open('GET', url, true)
  fileReader.onload = () => {
    const message = JSON.parse(fileReader.responseText)
    i18n.setLocaleMessage(locale, message)
    i18n.locale = locale
    if (cb) cb()
  }
  fileReader.onerror = () => {
    console.log('[obigo-js-ui] ' + locale + '.json is not exit')
    i18n.locale = FALLBACK
  }
  fileReader.send(null)
}

const i18n = new VueI18n({
  locale: FALLBACK,
  fallbackLocale: FALLBACK,
  messages: {
    'en-us': en,
    en: en,
    ko: ko
  }
})

loadLocaleMessage(currentLocale)

i18n.loadLocaleMessage = loadLocaleMessage

Vue.use(VueI18n)

export default i18n
```











By calling `app.use(i18n)`, By default, we can access the VueI18n instance from each component with `this.$i18n`, which can be referenced from the `global` property of i18n instance that created with `createI18n`