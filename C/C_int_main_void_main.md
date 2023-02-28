# C

## int main() vs void main()

방송통신대학의 C 프로그래밍 수업에서 실습중 아래 소스 코드를 컴파일하는 과정에서 오류가 났다.

```c
#include <stdio.h>

void main() {
	printf("hello world");
}
```

typescript를 사용하며 리턴값이 없는 경우는 늘 void를 사용했었기에 왜 컴파일 오류가 나는지 궁금해졌다. 결론부터 말하자면, int main()이 C/C++ 표준에 맞는 표기이며, void main()은 잘못된 표기라고 한다.

```c
int main() {
	return 0;
}
```

위의 코드에서 return 0은 main() 함수가 정상적으로 종료되었다는 뜻의 exit(0) 을 호출하는 역할을 한다. exit 의 인수는 컴파일러마다 약간씩 다르게 정의되어 있으나, 일반적으로 0이면 정상종료를 의미하고, 그 외의 숫자들 사전에 정의된 에러 타입을 의미한다. 

따라서 void main() 은 return 0을 통해 함수를 정상적으로 종료시키지 않는 것이기에 프로세스상에서 올바르지 않은 종료조건(0)을 운영체제 리턴하고 자신을 종료할 가능성이 있다.

# :books:참고자료

https://soyoja.com/100
