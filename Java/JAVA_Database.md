# JAVA JDBC 사용

JDBC는 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API이다. JDBC는 데이터베이스에서 자료를 쿼리하거나 업데이트하는 방법을 제공한다.

 

## JDBC 연결 순서

1. 드라이버 로드

   데이터베이스와 의사소통하기 위한 정보들이 드라이버에 있기 때문에 드라이버 먼저 로딩

2. Connection DB 연결

3. Statement

   쿼리 실행을 위해 statement 객체 생성

4. 결과값 처리

   쿼리 실행으로 나온 결과값을 변수에 할당하고 처리

5. 연결 해제



프로젝트 오른쪽 클릭

build path

Libraries

Add External JARs





-. jdbc:oracle:thin은 사용하는 JDBC드라이버가 thin 타입을 의미한다. 자바용 오라클 JDBC드라이버는 크게 두가지가 있는데 하나는 Java JDBC THIN 드라이버고, 다른 하나는 OCI기반의 드라이버라고 한다.

-. username/password은 option이다. [ ]안에 있는 정보는 반드시 명기할 필요는 없다는 뜻이다.

-. :port 번호도 option이다. 다만 Oracle의 listener port인 1521을 사용하지 않을 경우는 이 값을 명기해 줘야 된다. 예를 들어서 jdbc:oracle:thin:hr/hr@//localhost:1522

-. localhost는 Oracle DB가 설치되어 있는 서버의 IP인데 위 경우는 로컬에 설치되어 있다는 뜻이다.

-. 1521 은 오라클 listener의 포트번호이다.

-. /XE는 Oracle database client의 고유한 service name이다. 디폴트로 XE를 사용하므로 이 정보도 option이다. 이에 대한 설정 정보는 Oracle이 설치된 폴더 아래의 app\oracle\product\11.2.0\server\network\ADMIN\listener.ora 파일에 다음과 같이 표시되어 있다.

DEFAULT_SERVICE_LISTENER = (XE)


출처: https://developer-joe.tistory.com/82 [코드 조각-Android, Java, Spring, JavaScript, C#, C, C++, PHP, HTML, CSS, Delphi]



```java
public void insertData() throws ClassNotFoundException, SQLException {
	//studentTBL에 Abc1115.csv 삽입
	String sql = "insert into studentTBL values(990001, 'addx', 17, 29, 16, 19, 43, 154, 'C', 'A', 'C')";
	Connection con = this.makeConnection();
	Statement stmt = con.createStatement();
	int affectedCount = stmt.executeUpdate(sql);
	
	if(affectedCount>0) {
		System.out.println(affectedCount+"행을 삽입했습니다.");
	}
	stmt.close();
	// connection 객체를 기반으로 statement 객체가 만들어졌으므로 먼저 닫는다.
	con.close();
}
```



preparedStatement 부분 작성하기