# ✅모듈

## 1. 자바스크립트와 모듈

 자바스크립트는 웹페이지에 있어서 보조적인 기능을 수행하기 위해 한정적인 용도로 만들어진 태생적 한계로 다른 언어에 비해 부족한 부분이 있다. 대표적인 것이 모듈 기능이 없다는 것이다.

 브라우저 상에서 동작하는 JavaScript는 스크립트 태그로 로드하며 복수의 JavaScript 파일을 로드할 경우 하나의 파일로 합쳐지며 동일한 유효범위를 갖게 된다. 그래서 전역 변수의 중복 등의 문제가 발생할 수 있다. 이것으로는 모듈을 구현할 수 없다.

> ES6에서는 클라이언트 사이드 JavaScript에서도 동작하는 모듈 기능을 추가하였다.

 JavaScript를 클라이언트 사이드에 국한하지 않고 범용적으로 사용하고자 하는 움직임이 생기면서 모듈 기능은 반드시 해결해야하는 핵심 과제가 되었고 이런 상황에서 제안된 것이 [CommonJS](http://www.commonjs.org/)와 [AMD(Asynchronous Module Definition)](https://github.com/amdjs/amdjs-api/wiki/AMD)이다.

 Node.js는 사실상 모듈 시스템의 사실상 표준인 [CommonJS](http://www.commonjs.org/)를 채택하였고 현재는 독자적인 진화를 거쳐 CommonJS과 100% 동일하지는 않지만 기본적으로 CommonJS 방식을 따르고 있다. 따라서 Node.js 환경에서는 파일별로 독립적인 파일 스코프를 갖는다.

## 🚀 Further Study

[CommonJS, AMD, 그리고 UMD란?](../Javascript/Javascript_CommonJS_AMD_UMD.md)

## 2. export 키워드

 모듈은 독자적인 모듈 스코프를 갖는다. 따라서 모듈 내부에서 선언한 모두 식별자는 기본적으로 해당 모듈 내부에서만 참조할 수 있다. 이 식별자를 외부에 공개하여 다른 모듈들이 재사용할 수 있게 하려면 export 키워드를 사용한다.

 export 키워드는 선언문 앞에 사용한다. 이로써 변수, 함수, 클래스 등 모든 식별자를 export 할 수 있다. 아래는 express를 사용해 만든 서버의 일부이다.

```javascript
// userRouter.js

import express from "express";
import { edit, remove } from "../controllers/userController";

const userRouter = express.Router();

userRouter.get("/edit", edit);
userRouter.get("/remove", remove);

export default userRouter;
```

 마지막 줄을 보면 export default라는 부분을 확인할 수 있다. 이는 하나의 값만 export한다는 의미로 여기서는 userRouter이라는 변수만 export하겠다는 의미라고 보면 된다.

```javascript
// userController.js

export const edit = (req, res) => res.send("User Edit");
export const remove = (req, res) => res.send("User Remove");
```

여기서는 edit과 remove 함수를 모두 export하기 위해 default를 사용하지 않았다.

### 3. import 키워드

 다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드하려면 import 키워드를 사용한다. 위의 코드의 일부를 다시 살펴보자.

```javascript
import express from "express";
import { edit, remove } from "../controllers/userController";
```

 import를 할 때는 default로 export한 경우와 그렇지 않은 경우로 나누어 봐야 한다. default export한 경우에는 하나의 값만을 지정해서 내보낸 것이기 때문에 어떤 이름으로 import하더라도 문제되지 않는다. 하지만 그냥 export한 경우에는 {}와 함께 export할 식별자 이름 그대로 import해야만 한다.

# :books:참고자료

이웅모, 모던 자바스크립트 Deep Dive, 위키북스, 2020

노마드코더 유튜브 클론코딩 강의
