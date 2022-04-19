# 모던 자바스크립트

## 챕터 : 문서 수정하기

### 시계 만들기 과제

```html
<div id="clock">
    <span class="hour">hh</span>
    :<span class="min">mm</span>
    :<span class="sec">ss</span>
</div>
<button class="startBtn">시작</button>
<button class="stopBtn">정지</button>
```

```javascript
let hour = document.querySelector('.hour');
let min = document.querySelector('.min');
let sec = document.querySelector('.sec');

const startBtn = document.querySelector('.startBtn');
const stopBtn = document.querySelector('.stopBtn');

let timeId;

// setInterval의 반환값을 할당하기 위한 변수
// clearInterval을 사용할 때 필요하다.

function getClock() {
  const date = new Date();

  let hours = date.getHours();
  hours = hours > 9 ? hours : `0${hours}`;
  hour.innerHTML = hours;

  let minutes = date.getMinutes();
  minutes = minutes > 9 ? minutes : `0${minutes}`;
  min.innerHTML = minutes;

  let seconds = date.getSeconds();
  seconds = seconds > 9 ? seconds : `0${seconds}`;
  sec.innerHTML = seconds;
}

// 조건 연산자로 각 값이 9보다 작을 경우 0을 붙여서 출력하도록 했다.

function start() {
  timeId = setInterval(getClock, 1000);
  getClock();
}

// setInterval 함수를 통해 매 초마다 getClock 함수를 실행시킨다.
// setInterval 함수의 반환값인 해당 작업의 ID 를 timeId에다 할당한다.

function stop() {
  clearInterval(timeId);
}

// timeId와 clearInterval 함수를 통해 setInterval 함수를 취소한다.

startBtn.addEventListener('click', start);
stopBtn.addEventListener('click', stop);
```

start 버튼을 누르면 시계가 동작하고, stop 버튼을 누르면 시계가 멈추는 로직이다.

### 달력 만들기 과제

#### table 태그

table 태그는 데이터를 포함하는 셀들의 행과 열로 구성된 2차원 테이블을 정의할 때 사용한다. 이러한 테이블은 `<table>` 요소와 자식 요소인 하나 이상의 `<tr>`,`<th>`,`<td>` 요소들로 구성된다. `<tr>`요소는 각 행(row)를 정의하고, `<th>` 요소는 각 열의 제목을 정의한다. 또한 `<td>` 요소는 하나의 테이블 셀을 정의한다.

```html
<div id="calendar"></div>
```

```javascript
function createCalendar(elem, year, month) {

    let mon = month - 1; // 자바스크립트에선 월을 1월~12월이 아닌 0월~11월로 취급한.
    let d = new Date(year, mon);

    let table = '<table><tr><th>MO</th><th>TU</th><th>WE</th><th>TH</th><th>FR</th><th>SA</th><th>SU</th></tr><tr>';

    // 첫번째 행을 만드는 부분
    // 월요일부터 월의 첫 번째 날까지는 공백으로 채워야 한다.

    for (let i = 0; i < getDay(d); i++) {
      table += '<td></td>';
    }

    // 실제 날짜가 들어있는 <td>
    while (d.getMonth() == mon) {
    // while문의 조건으로 해당 월일 
      table += '<td>' + d.getDate() + '</td>';

      if (getDay(d) % 7 == 6) { // 주의 마지막 요일인 일요일을 만나면 개행한.
        table += '</tr><tr>';
      }

      d.setDate(d.getDate() + 1);
    // while문의 조건을 업데이트하기 위한 코드이다.
    // date 객체에 하루씩 더해서 해당 월의 마지막날을 넘어가게 되면
    // while문의 조건이 false가 되어 반복이 멈춘다.
    }
    // 달력의 마지막 행에 빈칸을 추가한다.
    if (getDay(d) != 0) {
      for (let i = getDay(d); i < 7; i++) {
        table += '<td></td>';
      }
    }

    table += '</tr></table>';

    elem.innerHTML = table;
  }

  function getDay(date) { // 월요일부터 일요일을 0부터 6의 숫자로 얻는 부분
    let day = date.getDay();
    if (day == 0) day = 7; // 일요일에 해당하는 번호(0)를 7로 고치는 부분
    return day - 1;
  }

  createCalendar(calendar, 2012, 9);
```
