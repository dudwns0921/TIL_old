# Utils

프로젝트 당시 썼던 유용한 코드들을 모아보았다.

## Email Validation

```javascript
const validateEmail = email => {
  return String(email)
    .toLowerCase()
    .match(
      /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/,
    );
};
```

위 함수는 정규표현식을 활용해 인자로 넘긴 값이 이메일 형식인지 확인하는 함수이다. 

## Cookie

```javascript
function saveAuthToCookie(value) {
  document.cookie = `til_auth=${value}`;
}
// 쿠키의 저장할 키와 
function saveUserToCookie(value) {
  document.cookie = `til_user=${value}`;
}

function getAuthFromCookie() {
  return document.cookie.replace(
    /(?:(?:^|.*;\s*)til_auth\s*=\s*([^;]*).*$)|^.*$/,
    '$1',
  );
}

function getUserFromCookie() {
  return document.cookie.replace(
    /(?:(?:^|.*;\s*)til_user\s*=\s*([^;]*).*$)|^.*$/,
    '$1',
  );
}

function deleteCookie(value) {
  document.cookie = `${value}=; expires=Thu, 01 Jan 1970 00:00:01 GMT;`;
}

export {
  saveAuthToCookie,
  saveUserToCookie,
  getAuthFromCookie,
  getUserFromCookie,
  deleteCookie,
};
```

인증을 위한 토큰값과 username을 쿠키에 저장하는 코드이다. 쿠키에는 키와 값의 형태로 데이터가 저장되므로 til_auth를 본인이 설정한 키 값으로 바꾸어주면 된다.

## Filters

```javascript
export function formatDate(value) {
  const date = new Date(value);
  const year = date.getFullYear();
  let month = date.getMonth();
  month = month > 9 ? month : `0${month}`;
  const day = date.getDay();
  let hours = date.getHours();
  hours = hours > 9 ? hours : `0${hours}`;
  const minutes = date.getMinutes();

  return `${year}년 ${month}월 ${day}일 ${hours}시 ${minutes}분`;
}
```

vue 3.x부터는 필터 기능이 없어지긴 했지만, computed나 methods 속성을 활용할 수 있다. 위 함수는 날짜 포매팅 함수로 return에 본인이 원하는 형식의 날짜를 작성해주면 된다.
