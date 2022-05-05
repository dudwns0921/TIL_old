# CRA

## 빌드 설정 커스터마이징

> Create React App is an officially supported way to create single-page React applications. It offers a modern build setup with no configuration.
>
> CRA는 SPA를 만들기 위해 공식적으로 지원되는 방법입니다. CRA는 어떠한 설정 없이도 최신의 빌드 환경을 제공합니다.

아마 많은 사람들이 처음 리액트 애플리케이션을 만들 때 CRA를 사용했을 거라고 생각한다. 위에서 말한 것처럼 최신의 빌드 환경을 제공해주기 때문에 우리가 해야할 것은 UI에 대한 코드를 작성하는 것뿐이다. 하지만 빌드 환경에 대한 커스터마이징이 필요하게 되면 어떻게 해야할까?

### react-scripts eject

첫 번째로 해볼 수 있는 방법은 eject 명령어를 사용하는 것이다. CRA는 설정파일을 숨기기 때문에 eject 명령어를 통해서 설정파일들을 추출해야 설정을 변경할 수 있다. eject 명령어는 영구적이기 때문에 실행하면 설정 파일들을 다시 숨길 수는 없다.

자, 이제 커스터마이징을 하면 된다. 하지만 이렇게 되면 애초에 CRA를 쓴 목적이 퇴색된다고 생각한다. 복잡한 설정을 피하기 위해 CRA를 썼는데 다시 그 복잡한 설정을 하나하나 확인해가며 커스터마이징을 해야한다니... 여기서 이번에 알아볼 react-app-rewired가 등장한다. 기존의 설정에 추가된 설정을 덮어쓰는 일종의 꼼수(?)라고 생각하면 된다.

### react-app-rewired

