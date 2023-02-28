# IT지식

## px vs dp, dpi

얼마 전에 화면 작업을 위한 Zeplin 파일을 받았는데, dp와 dpi 등 처음 보는 단위들이 있었다. 그래서 주로 쓰는 px과 어떤 차이가 있는지 찾아보았다.

### px

px은 화면을 구성하는 최소 단위이다. px 단위는 화면의 전체 크기와 상관없이 지정한 수치만큼 표시되는 절대적 표시 단위이므로 안드로이드에서는 px보다는 dp 단위를 사용하는 것이 좋다.

### dp(Density-independent Pixel)

우리말로 밀도, 독립화소라고 번역할 수 있다. 더 쉽게 말하면 디스플레이의 해상도(밀도)와 상관없이 다룰 수 있는 단위라는 뜻이다.

다만 px 단위는 스크린 안에서만 정확하지 현실 세계에서는 기기마다 표현되는 길이가 다르다.  따라서 dp단위를 사용하면 기기마다 차이를 고민하지 않고도 인터페이스를 디자인할 수 있어서 안드로이드에서 주로 사용되는 단위이다.

### dpi(dots per inch)

dpi는 dots per inch라는 뜻으로 1인치(2.54 센티미터)에 들어 있는 픽셀의 수를 말한다.

![1_Ii4mMCS3QjKDQOML8y7TqA](http://design.gabia.com/wordpress/wp-content/uploads/2019/03/1_Ii4mMCS3QjKDQOML8y7TqA-1024x590.png)

___

디자인 파일에서는 mdpi를 지원한다고 되어있었고, mdpi의 경우 1px=1dp이기 때문에 변환하지 않고 디자인 파일의 값을 그대로 px로 작성했다. 

# :books:참고자료

http://design.gabia.com/wordpress/?p=33289