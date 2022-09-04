# Javascript

## Console

`console`, 아마 프로그래밍 세상에 입문했을 때 가장 먼저 마주치게 되는 객체가 아닐까 생각한다. 

```js
console.log("Hello World!") // Hello World!
```

위와 같이 `log` 메서드로 다들 한 번쯤 Hello World를 콘솔로 출력해본 경험이 있을 것이다. 개인적으로는 `debugger`보다도 많이 쓰는 메서드인데, `log` 메서드 말고도 다양한 메서드들이 존재한다고 해서 알아보고자 한다.

### warn, info, error

`log` 메서드의 형제 정도로 생각하면 된다. 콘솔에 인자를 출력한다는 점은 같지만, 디자인에서 조금씩 차이가 있다.

```
console.log('기본');
console.info('정보');
console.warn('경고');
console.error('에러');
```

![image-20220904093718153](C:\Users\User\Desktop\TIL\Javascript\md-images\image-20220904093718153.png)	

`warn`이나 `error`는 확실하게 디자인 차이가 나지만, `info`는 `log`와 디자인에서도 차이가 없다. `info`만의 특징은 아래와 같다.

- 정보성 메시지를 전달할 때 사용한다.
- `Firefox` 브라우저에서는 아래와 같이 메시지와 함께  `i`아이콘이 출력된다.

![image-20220904094456086](C:\Users\User\Desktop\TIL\Javascript\md-images\image-20220904094456086.png)	

### 객체 출력할 때 주의점

`log`메서드는 참조를 출력하기 때문에, 객체와 같이 내용물이 변할 수 있는 것들은 내용이 실시간으로 바뀐다.

```js
let obj = {};
console.log(obj);
obj.a = 1;
console.log(obj);
```

![image-20220904100024726](C:\Users\User\Desktop\TIL\Javascript\md-images\image-20220904100024726.png)	

위의 예시를 보면 `a`에 1을 할당하기 전에 출력했음에도 `a:1`이 객체 안에 들어있음을 확인할 수 있다. 코드가 이렇게 가까이 있을 때는 변경사항이 업데이트되었다고 생각할 수 있겠지만, 그렇지 않다면 굉장히 혼란스러울 수 있다.

이를 해결하기 위한 방법은 아래와 같다.

- 깊은 복사를 통한 출력
- 객체가 아닌 값을 출력

하지만 깊은 복사는 비용이 많이 들기 때문에 객체가 아닌 값을 출력하는 것이 좋다.

```js
let obj = {};
console.log(Object.keys(obj).length);
obj.a = 1;
console.log(Object.keys(obj).length);
```

![image-20220904100956798](C:\Users\User\Desktop\TIL\Javascript\md-images\image-20220904100956798.png)	

이처럼 객체가 아닌 값을 출력할 경우, 객체가 변경되는 것과 상관없이 객체와 관련된 정확한 값을 확인할 수 있다. 

### dir

`dir` 메서드는 인자로 전달된 JavaScript 객체의 속성 목록을 표시한다. 객체를 출력할 때는 `dir`을 쓰는 게 훨씬 유용하다. 예시를 통해 살펴보자.

```js
function f() { return true; } // 자바스크립트에서는 함수도 객체임을 기억하자.
console.log(f);
console.dir(f);
```

![image-20220904105248088](C:\Users\User\Desktop\TIL\Javascript\md-images\image-20220904105248088.png)	

###  Count

`count` 메서드는`count`메서드가 호출된 횟수를 기록한다. 아래 예시를 통해 살펴보자.

```js
function get() {
    // get 요청 이루어짐
    console.count('get');
}
get();
get();
get();
```

![image-20220904112226661](C:\Users\User\Desktop\TIL\Javascript\md-images\image-20220904112226661.png)	

예시에서 볼 수 있듯이 api 요청이나 함수 호출이 몇 번 이루어졌는지 등을 확인할 때 유용할 것이라고 생각한다.

### time, timeEnd

`time` 메서드는 작업에 걸리는 시간을 측정하는 데 사용하는 타이머를 시작한다.

`timeEnd`는 `time`을 호출해서 시작된 타이머를 중지한다.

정리하자면 두 메서드를 통해 코드 수행 시간을 확인할 수 있다. 이 때 `time`과 `timeEnd`에 같은 타이머 이름을 주어야 정상적으로 작동한다.

```js
console.time('timer');
let cnt = 0;
for (let i = 1; i <= 100; i++) cnt++;
console.log(cnt);
console.timeEnd('timer');
```

![image-20220904113445491](C:\Users\User\Desktop\TIL\Javascript\md-images\image-20220904113445491.png)	

# :books:참고자료

https://www.zerocho.com/category/JavaScript/post/5b2b45cf1350f9001b662ba6