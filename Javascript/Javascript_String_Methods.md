# Useful String Methods

ES6부터 유용한 문자열 메소드들이 추가되었다.



## includes()

**`includes()`** 메서드는 하나의 문자열이 다른 문자열에 포함되어 있는지를 판별하고, 결과를 `true` 또는 `false` 로 반환한다.

```javascript
const str = 'To be, or not to be, that is the question.';

console.log(str.includes('To be'));       // true
console.log(str.includes('nonexistent')); // false
console.log(str.includes('To be', 1));    // false : 문자열을 찾기 시작할 위치를 설정할 수 있다. 기본값은 0이다.
console.log(str.includes('TO BE'));       // false : 대소문자를 구별한다.
```



## repeat()

**`repeat()`** 메서드는 문자열을 주어진 횟수만큼 반복해 붙인 새로운 문자열을 반환한다.

```javascript
'abc'.repeat(-1);   // RangeError : 반복 횟수는 양의 정수여야 함.
'abc'.repeat(0);    // ''
'abc'.repeat(1);    // 'abc'
'abc'.repeat(2);    // 'abcabc'
'abc'.repeat(3.5);  // 'abcabcabc' (count will be converted to integer)
'abc'.repeat(1/0);  // RangeError : 반복 횟수는 무한대보다 작아야 하며, 최대 문자열 크기를 넘어선 안됨.
```



## startsWith()

**`startsWith()`** 메소드는 어떤 문자열이 특정 문자로 시작하는지 확인하여 결과를 `true` 혹은 `false`로 반환한다.

```javascript
const str = 'To be, or not to be, that is the question.';

console.log(str.startsWith('To be'));         // true
console.log(str.startsWith('not to be'));     // false
console.log(str.startsWith('not to be', 10)); // true 찾고자 하는 문자열 길이값을 넣어 특정 위치에서 어떤 문자열로 시작하는지 확인할 수 있다.
```



## endsWith()

The **`endsWith()`** 메서드를 사용하여 어떤 문자열에서 특정 문자열로 끝나는지를 확인할 수 있으며, 그 결과를 `true` 혹은 `false`로 반환한다. 

```javascript
const str = 'To be, or not to be, that is the question.';

console.log(str.endsWith('question.')); // true
console.log(str.endsWith('to be'));     // false
console.log(str.endsWith('to be', 19)); // true 찾고자 하는 문자열 길이값을 넣어 특정 위치에서 어떤 문자열로 끝나는지 확인할 수 있다.
```



# :books:참고자료

노마드코더 강의

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/includes

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/repeat

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith