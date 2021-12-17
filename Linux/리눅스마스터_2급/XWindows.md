# X-Windows

# 1. X-Window 개념 및 사용법

# 1-1. X윈도우의 특징과 구성 요소

- 개념과 특징

네트워크 프로토콜 기반의 클라이언트/서버 시스템

오픈 데스크톱 환경으로 KDE, GNOME, XFCE 등이 있음



- 구성요소

XProtocol



Xlib

윈도우 창 생성, 이벤트 처리, 창 조회, 키보드 처리와 같은 라이브러리를 제공

XCB : xlib를 대체하기 위해 등장, 향상된 쓰레드 기능 지원, 확장성 우수



Xtoolkit

고급 레벨의 GUI 생성을 위해 사용

Xt Intrinsic과 Widget 포함

XView, Xaw, Motfi, Qt, GTK, KTK 등이 있음



XFree86

인텔 x86 계열의 유닉스 운영체계에서 동작하는 X 서버

XF86Config



# 1-2. 용어설명

- 데스크톱 환경 : GUI 환경을 이용하기 위해 사용자에게 제공되는 인터페이스 스타일(파일관리자, 아이콘, 창 도구 모음, 배경화면 등이 포함)

KDE : KDE에서는 윈도우 매니저로 Kwin을 사용한다.



GNOME : GNU에서 만든 공개형 데스크톱으로 소스 공개 자유 소프트웨어이다.

데스크톱 부분과 라이브러리 부분은 LGPL, 응용 프로그램은 GPL 라이선스 정책을 따른다.



LXDE, XFCE



- 윈도우 매니저 : X 윈도우 상에서 윈도우의 배치와 표현을 담당하는 시스템 프로그램

| 이름         | 설명                                                         |
| ------------ | ------------------------------------------------------------ |
| fvwm         | fvwm2는 virtual window manager 버전 2.x 약어로 fvwm에 기능을 추가시킨 것이다. |
| Window Maker | 현재 GNOME과 KDE에 통합되었다.                               |
| Blackbox     | 넥스트스텝의 인터페이스를 기반으로 한 윈도우 매니저이다.     |
| kwm          | KDE 1.x의 기본 윈도우 매니저이다.                            |



- 유저 인터페이스 : 사람들이 컴퓨터와 상호 작용하는 시스템
- 디스플레이 매니저 : X 윈도우 구성요소 중 사용자 로그인 및 세션 관리 역할 수행 프로그램, XDM, GDM(CentOS의 디스플레이 매니저), KDM 등이 있음

- X 프로토콜 : X 윈도우 시스템에서 서버는 디스플레이를 전담하는 기능을 하며, X 클라이언트의 요구에 따라서 화면 출력과 사용자의 입력 처리를 담당



# 1-3. 기타

KDE : QT라이브러리 기반

GNOME : LGPL을 따르는 GTK+ 라이브러리 기반



# 1-4. X윈도우 설정과 실행

- 파일 /etc/inittab : 리눅스 부팅모드 설정

`이름 : 런레벨 : 옵션 : process --옵션`

3 : 텍스트 모드에 의한 다중 사용자 모드

5 : 그래픽 모드에 의한 다중 사용자 모드

- X 윈도우 실행 명령어 : startx

- X 윈도우 강제 종료(컨트롤 + 알트 + 백스페이스)
- xinit에 전달하는 옵션 : '--'
- 환경변수 display는 현재 X-윈도우 디스플레이 위치를 지정할 수 있다.

형식 : export DISPLAY= "IP주소:디스플레이번호,스크린번호" (번호는 0부터 시작)



# 1-5. KDE, GNOME, GRUB

- 데스크톱 환경 : KDE, GNOME, Xface
- GNU 프로젝트 부트로더 : GRUB
- Windows maker : GNOME과 KDE에 통합



# 1-6. GNOME

- GNOME : nautilus 파일 관리자로 사용
- GNOME 2 : metacity 파일 관리자
- GNOME 3 : M     

# 1-7. 참고

- Ismod : 커널에 로드되어 있는 모듈 확인
- modprobe : 리눅스 커널에 모듈을 추가하거나 제거하는데 사용
- 명령어 ip addr add는 지정된 인터페이스(eth1)에 IP 주소를 지정할 경우에 사용한다.
- 명령어 system-config-network GUI 환경에서 네트워크 설정 명령어이다.
- IP 주소 지정 형식은 `ifconfig [인터페이스명][IP주소][netmask 서브넷마스크]`[upidown] 이다.
- /etc/services : 인터넷 서비스 관련 포트 번호를 확인할 때 사용
- /etc/protocols : 서비스 가능한 프로토콜 목록이 정의
- 네트워크 설정 파일 /etc/sysconfig/network-scipts/ifcfg-eth(n)을 이용하여 인터페이스의 IO 주소를 지정할 수 있다. (번호는 0부터 시작)
- /etc/sysconfig/network-scripts : 네트워크 인터페이스 환경설정과 관련된 파일이 저장
- /etc/init.d/network <startlstop> : 네트워크 서비스 시작 / 정지
- X 컨소시엄에 의해서 X11버전이 처음으로 개정되어 X01R02에서 X01R6까지 발표되었다.



# 2. X-윈도우 활용

# 2-1. xhost

- X 서버에 접속할 수 있는 클라이언트를 지정하거나 해제
- 형식 : xhost `[+|-][IP|도메인명]`



# 2-2. xauth

X윈도우를 실행하면 $HOME/.Xauthority 파일이 생성됨

이 파일은 키 값을 갖고 있는데 이 키 값을 가지고 X서버로 접근하면 해당 사용자로 인증하여 사용 가능하도록 한다.

인증키를 설치하는 명령은 xauth add $DISPLAY이다.

- MMC 방식의 인증 방식을 사용하기 위한 필수 유틸리티
- xauth list $DISPLAY

지정된 호스트 표시창의 쿠키값을 확인하는 명령



# 2-3. X윈도우 응용 프로그램

- 오피스 : LibreOffice, gedit, kwrite
- 그래픽 : GIMP(이미지 편집), ImageMagick, eog(이미지 뷰어), kolourpaint, gThumb, gwenview
- 멀티미디어 : Totem(동영상 재생), PHYTHMBOX CHEESE
- 파일관리 : nautilus, konqueror(KDE에서 사용하는 웹브라우저 / 파일관리 프로그램)
- 개발 : ECLIPSE
- 기타 : KSanpshot(스크린샷 프로그램)

