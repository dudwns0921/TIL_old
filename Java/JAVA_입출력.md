# JAVA IO 기반 입출력

자바에서 데이터는 스트림을 통해 입출력된다. **스트림**은 단일 방향으로 연속적으로 흘러가는 것을 의미한다.

프로그램이 출발지냐 또는 도착지냐에 따라서 스트림의 종류가 결정되는데, 

프로그램이 데이터를 입력받을 때에는 **입력 스트림(InputStream)**,

프로그램이 데이터를 보낼 때에는 **출력 스트림(OutputStream)**이라고 부른다.

처음에 이야기했듯이 스트림은 단방향 특성을 갖고 있기 때문에 데이터 교환을 위해서는 두 스트림이 모두 필요하다.



자바의 기본적인 데이터 입출력 API는 java.io 패키지에서 제공하고 있다.

| 주요 클래스                                                  | 설명                                                  |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| File                                                         | 파일 시스템의 파일 정보를 얻기 위한 클래스            |
| Console                                                      | 콘솔로부터 문자를 입출력하기 위한 클래스              |
| InputStream / OutputStream                                   | 바이트 단위 입출력을 위한 최상위 입출력 스트림 클래스 |
| FileInputStream / FileOutputStream<br />DataInputStream / DataOutputStream<br />ObjectInputStream / ObjectOutputStream<br />PrintStream<br />BufferedInputStream / BufferedOutputStream | 바이트 단위 입출력을 위한 하위 스트림 클래스          |
| Reader / Writer                                              | 문자 단위 입출력을 위한 최상위 입출력 클래스          |
| FileReader / FileWriter<br />InputStreamReader / OutputStreamWriter<br />PrintWriter<br />BufferedReader / BufferedWirter | 문자 단위 입출력을 위한 하위 스트림 클래스            |

지금까지의 경험으로는 다른 데이터들보다도 텍스트 형태의 데이터를 입출력하는 경우가 제일 많다고 생각한다. 

따라서 File, FileReader, FileWriter, BufferedReader, BufferedWriter 클래스를 중심으로 텍스트를 읽고 쓰는 경우를 살펴보자.



## 1. 텍스트 읽기

### 1) File 객체를 생성

```java
File file = new File("./data/Abc1115.csv");
```

파일이 있는 경로가 상대경로로 작성되었다. 필자는 자바 프로젝트 폴더에 data 폴더를 생성하고 그 안에다가 읽어와야 할 csv 파일을 넣어두었다.

### 2) File 객체를 파라미터로 하는 FileReader 객체 생성

```java
// 방법 1
FileReader fr = new FileReader("./data/Abc1115.csv");

// 방법 2
File file = new File("./data/Abc1115.csv");
FileReader fr = new FileReader(file);
```

위와 같이 두 가지 방법으로 FileReader 객체를 생성할 수 있다. 

하지만 읽어야 할 파일이 File 객체로 생성되어 있다면 두 번째 방법으로 좀 더 편하게 객체를 생성할 수 있다.

FileReader 객체가 생성될 때 파일과 직접 연결이 되는데, 이 때 파일이 존재하지 않으면 FileNotFoundException을 발생시키므로 예외 처리를 해야한다. 

### 3) FileReader 객체를 파라미터로 하는 BufferedReader 생성

BufferedReader는 문자 입력 스트림에 연결되어 버퍼를 제공해주는 보조 스트림이다. 

BufferedReader는 입력 소스로부터 자신의 내부 버퍼 크기만큼 데이터를 미리 읽고 저장해준다.

프로그램은 외부의 입력 소스로부터 직접 읽는 대신 버퍼로부터 읽음으로써 읽기 성능이 향상된다.

```java
File file = new File("./data/Abc1115.csv");
FileReader fr = new FileReader(file);
BufferedReader br = new BufferedReader(fr);
// 문자 입력 스트림인 FileReader에 연결
```

### 4) BufferedReader에서 읽은 텍스트를 임시로 저장하는 String 타입의 변수 생성

### 5) While문과 readLine() 메소드를 통해 텍스트 파일을 한 줄씩 읽음

```java
	public void BufferedReaderExample(){
		File file = new File("./data/Abc1115.csv");
			FileReader fr = new FileReader(file);
			BufferedReader br = new BufferedReader(fr);
            // 3번 : 문자 입력 스트림인 FileReader에 연결
			String line = null;
            // 4번
			while((line=br.readLine())!=null) {
            // 5번 : readLine() 메소드를 통해 한 줄씩 받아오고 출력, 받아올 데이터가 없으면 반복문 종료
				System.out.println(line);
			}
		} catch (IOException e) {
			e.printStackTrace();
		}		
	}
```

readLine() 메소드는 "\ n" 문자를 포함하는 파일의 전체 라인을 읽는 데 사용된다.

쉽게 말하면 줄바꿈이 있기 전까지의 데이터를 모두 읽어오는 것이다.



그리고 여기서 예외 처리 코드를 통해 IOException을 예외처리한 것을 볼 수 있다.

IOException는 입출력 처리의 실패, 또는 인터럽트의 발생으로 인해 발생한다.

여기서는 readLine() 메소드때문에 작성되었는데, readLine의 입력스트림이 null일 경우 IOException을 throw하게 되어있다.

다시 말하면, 읽어올 자원이 없는데 강제로 읽게 하면 프로그램에 장애가 일어나기 때문에 미리 예외처리를 하는 것이다.



아까 FileReader 객체가 생성될 때 FileNotFoundException를 예외처리해주어야 한다고 했는데,

IOException이 File~Exception보다 상위 예외 클래스이기 때문에 IOException만 예외 처리해주어도 무방하다.

### 6) BufferedReader을 close()

이전에는 입출력 스트림을 사용할 때는 불필요한 자원을 낭비하지 않도록 명시적으로 close() 메소드를 사용해 스트림을 닫아주어야 했다.

하지만 자바 7 이후로 try-with-resources 구문을 지원해 자동으로 close() 메소드를 호출할 수 있게 되었다.

try 블록에서 호출된 객체가 AutoCloseable을 구현했다면 자바는 try 블록의 코드가 끝나는 시점에 close() 메소드를 호출한다.

#### :bulb:Tip!

https://docs.oracle.com/javase/8/docs/api/java/lang/AutoCloseable.html

위 링크에서 AutoColseable을 구현한 객체들의 명단을 확인할 수 있다.



## 2. 텍스트 쓰기

### 1) File 객체를 생성

### 2) File 객체를 파라미터로 하는 FileWriter 객체 생성

### 3) FileWriter 객체를 파라미터로 하는 BufferedWriter 객체 생성

### 4) write() 메소드로 내용을 작성하고 flush() 메소드로 버퍼에 있는 내용을 전송

텍스트 읽기 과정과 거의 유사하다. 

```java
	public void writer() {
		File file = new File("./filePractice/example.txt");
        // 1번
		try {
			if(file.exists()==false) {
				file.createNewFile();
			} else {
				System.out.println("텍스트 파일이 이미 생성되었습니다.");
			}
			FileWriter fw = new FileWriter(file);
			BufferedWriter bw = new BufferedWriter(fw);
			bw.write("나는 유정이를"+"\r\n");
			bw.write("세상에서 제일로 사랑한다.");
			bw.flush();
			
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
```

try 블록 안에 파일이 제대로 생성되었는지 확인하는 부분이 있다.

File 객체를 생성했다고 해서 파일이나 디렉토리가 생성되는 것이 아니다.

생성자 매개값으로 주어진 경로가 유효하지 않더라도 컴파일 에러나 예외가 발생하지 않는다.

따라서 실제로 파일이 생성되었는지 확인하기 위해 exists() 메소드를 사용했다. 

파일이 존재한다면 true를 리턴하고, 그렇지 않다면 false를 리턴한다.



여기서도 BufferedWriter을 사용했는데, 그 이유는 BufferedReader를 사용했을 때와 같다.

다시 설명하자면, BufferedWriter는 프로그램에서 전송한 데이터를 내부 버퍼에 쌓아두었다가 버퍼가 꽉 차면 모든 데이터를 한꺼번에 보낸다.

프로그램이 직접 데이터를 보내는 것이 아니라 메모리 버퍼로 데이터를 고속 전송하기 때문에 실행 성능이 향상된다.



추가적으로 BufferedWriter에는 8192 문자가 저장될 수 있다.

8192문자가 다 채워지면 버퍼에서 자동으로 데이터를 전송하지만 그렇지 않다면 flush() 메소드를 사용해 버퍼에 있는 내용들을 파일로 전송해주어야 한다.



BufferedWriter도 AutoCloseble 객체를 구현하고 있기 때문에 try 블록 안에만 넣어준다면 자동으로 close()가 된다.
