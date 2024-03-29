## 7강. 함수와 기억클래스(1)

### 매개변수 사이의 자료 전달

- 값에 의한 자료 전달
  - 기본적인 자료 전달 방법
  - 실 매개변수와 형식 매개변수 사이에 값의 전달
  - 호출한 함수의 실행이 끝난 다음 전달받은 값을 되돌려 받지는 못한다.
- 참조에 의한 자료전달
  - 호출함수와 피 호출함수의 매개변수 값을 서로 교환할 수 있는 자료전달 방법
  - 값을 전달하는 것이 아니라 실 매개변수의 값이 들어있는 주소 값이 전달된다.
  - 실매개변수에는 & 기호가 사용됨, 주소 연산자를 의미
  - 형식 매개변수에는 *기호가 사용, 포인터 변수를 나타냄
- 차이점
  - 값에 의한 자료 전달 방법은 형식매개변수와 실 매개변수의 변화가 없음
  - 참조에 의한 자료전달 방법은 형식 매개변수의 값이 변하면 실매개변수의 값도 변한다

### 기억클래스

- 변수를 기억공간의 특정영역에 할당하는 방법
- 즉 각 변수의 유효범위와 존속기간을 설정
  - 변수의 사용위치에 따라
    - 지역변수
      - 특정 범위 내에서만 통용되는 변수
      - 선언된 블록이나 함수 내에서만 사용 가능
      - 함수에서 사용되는 매개 변수도 해당
    - 전역변수
      - 프로그램 전체에 걸쳐 사용될 수 있는 변수
      - 가급적 프로그램 선두에 위치하는 것이 좋음.
      - 전역 변수는 초기화 안 하면 0으로 자동 초기화
    - 지역변수와 전역변수 비교
      - 동일 범위 내에서는 지역 변수가 우선
      - 전역 변수의 선언은 프로그램 선두
      - 가급적 지역변수를 사용하는 것이 효율적
        - 함수의 독립성 향상
        - 디버깅 효율 향상
        - 기억 공간 절약

#### 기억클래스의 종류

- 변수의 기억클래스 종류
  - 변수의 초기화, 존속기간, 유효범위에 따라 구별
    - 자동 변수
    - 정적 변수
    - 외부 변수
    - 레지스터 변수
- 기억클래스 이용한 변수 선언
  - 형식
    - 기억 클래스 자료형 변수명
  - 기능
    - 기존의 변수 선언문에 기억클래스만 기입
    - 선언된 변수에 저장된 자료는 해당 기억영역에 놓이게 됨
- 자동변수
  - 함수 실행시 만들어지고, 실행이 끝나면 기억공간이 제거됨
  - 예약어 auto를 사용, 생략 가능
  - 통용 범위는 변수가 선언된 블록이나 함수 내로 한정
  - 지역 변수에 해당
  - 초기화가 필요
- 정적변수
  - 기억영역이 프로그램 끝날 때까지 유지
  - 예약어 static을 사용
  - 전역 변수에 해당
  - 변수의 값은 프로그램 실행 중 계속 유지
  - 초기화가 없으면 0으로 초기화됨
- 외부변수
  - 함수의 외부에서 선언
  - 예약어 extern을 사용
  - 전역 변수에 해당
  - 초기화가 없으면 0으로 초기화 됨
  - 다른 파일에서 외부 변수로 선언된 변수의 값을 참조할 수 있음
- 레지스터변수
  - CPU 내의 레지스터에 자료를 저장하고자 할 때
  - 예약어 register를 사용
  - 자동 변수와 동일한 속성
  - 프로그램의 실행속도 증가를 목적으로 사용
    - 주로 반복문에서 카운터 변수로 사용
