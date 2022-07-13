# 함수형 프로그래밍

## 들어가기 앞서...

먼저 함수형 프로그래밍이란 하루 만에 배울 수 있는 게 아니다. 코드를 쓰고 설계하는 새로운 관점을 의미한다. 따라서 아래의 내용을 통해 함수형 프로그래밍을 완벽하게 이해할 수 있다고는 생각하지 말자.

## 함수형 프로그래밍의 특징

### 선언형 프로그래밍

함수형 프로그래밍은 선언형 프로그래밍을 사용한다. 다음 예시를 살펴보자.

#### 이름의 길이가 5 이상인 과일들만 배열에 저장하기 

#### 명령형 프로그래밍:

```
var fruits = ["apple","banana","watermellon","pear","peach"]
fruitesOverFive = [];
for(var i = 0; i< fruits.length; i++){
	if(fruits[i].length > 5){
    	fruitsOverFive.push(fruits[i]);
    }
}
return fruitsOverFive;
```

#### 선언형 프로그래밍:

```
var fruitsOverFive = fruits.filter((fruit) => fruit.length>5)
```

먼저 명령형 프로그래밍은 요구사항의 구현이 개발자인 우리에게 달려있기 때문에 오류가 발생할 가능성이 높다. 또한 결과를 얻기 위한 과정이 로직의 중심이 되기 때문에 그 과정들을 모두 이해해야 한다는 단점이 있다.

그에 반면에 선언형 프로그래밍은 결과에 집중한다. 선언형 프로그래밍의 예시를 보면 훨씬 간단하며, filter라는 함수를 알고 있다면 직관적인 이해가 가능하다.

# :books:참고자료

https://blog.naver.com/whdgml1996/221987818879