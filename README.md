# 개발 가이드

## React

### 초기 세팅

-   create-react-app 으로 프로젝트 생성
    -   현재는 초기 세팅 완료 상태. git pull 후 yarn install 만 실행
-   프로젝트 생성 후 webpack 등 커스터마이징을 위한 eject 지양
-   <em>jQuery 사용 X (Vanilla JS로 작성)</em>
-   디렉토리 구조는 도메인 방식 (현 프로젝트 구조) 지향

### 디렉토리 구조 (부분 변경 가능성 있음)

```
+-- [public] - React 기본 구조
+-- [src]
+----- [api] - api
+--------- api.IMCS.js
+--------- api.MIMS.js
+--------- index.js - 폴더내 api 파일은 index 에서 통합 관리 및 사용 (Home.js - import 참조)
+----- [app] - 앱 전반에 관련
+--------- constants.js - 상수
+--------- History.js - history
+--------- index.js - app 내 js import
+--------- Navigation.js - 네비게이션 모듈
+--------- router.js - 라우터
+--------- slice.js - 글로벌 slice (Redux)
+--------- store.js - Redux(reduxjs/toolkit 기반)
+----- [assets] - 정적 리소스
+--------- [css] - css
+--------- [fonts] - 폰트
+--------- [images] - 이미지
+----- [components] - 공통 컴포넌트
+--------- Toast.js - 공통 모달 예시
+--------- Modal.js - 공통 팝업 예시
+----- [features] - 각 페이지별 컴포넌트
+--------- [home] - 홈 컴포넌트 (각 페이지별 분리 개발)
+------------- Home.js
+------------- HomeAside.js
+------------- HomeContents.js
+------------- HomeHeader.js
+------------- homeSlice.js
+--------- [launcher] - 런처 컴포넌트
+--------- [my] - 마이 컴포넌트
+--------- [setting] - 설정 컴포넌트
+--------- [title] - VOD 상세 컴포넌트
+----- [native] - 네이티브 인터페이스
+--------- nativeReceive.js - 네이티브에서 전달
+--------- nativeSend.js - 네이티브로 전달
+----- [utils] - 유틸
+--------- utilUi.js
+--------- index.js - 폴더내 util 파일은 index 에서 통합 관리 및 사용 (Home.js - import 참조)
+----- App.js - React 기본 구조
+----- index.js - React 기본 구조
+----- serviceWorker.js - React 기본 구조
+----- setupProxy.js - CORS 우회를 위한 Proxy 설정 (서버에서 Access-Control-Allow-Origin 처리 및 크롬 플러그인으로 대체 가능)
+-- .env - 환경변수
+-- package.json - package.json
```

### 개발

-   React Hooks 기반 함수형 컴포넌트로 개발 (기존 class 기반 X)
-   [Navigation 가이드](src/app/Navigation.md) 참조

```
import React, {useEffect, useState} from 'react';
import Navigation from "../../app/Navigation";

const Home = () => {
    const [state, setState] = useState('');

    //초기 init 및 Navigation 설정
    useEffect(() => {
        Navigation.set({...});

        example();

        return () => {
            //component unmount 시 호출, 클린업 함수
        }
    }, []);

    const example = () => {
        setState("Hello World");
    }

    return (
        <div>{state}</div>
    )
}

export default Home;
```

#### Redux

-   Redux는 "redux-toolkit" 및 hooks(useDispatch, useSelector) 기반으로 개발
    -   /app/store.js 참조
    -   App.js useDispatch 예제 참조, Home.js useSelector 예제 참조
-   Redux 파일(slice)은 각 페이지(features) 별 위치 - deviceInfo 및 userInfo 같은 글로벌 상태는 /app/slice.js 에 위치
    <br />

## Coding conventions

### 스크립트 버전

-   스크립트 버전은 ES6(ECMA2015) 이상 사용한다.
-   <em>jQuery 사용 X (Vanilla JS로 작성)</em>

### Syntax

-   개발시 IDE 간격은 4spaces.
-   var는 사용하지 않는다 (let / const 사용).
-   변수명은 카멜케이스(camelCase)를 사용한다.
-   Class 및 Component 는 파스칼케이스(PascalCase)를 사용한다
-   상수의 경우 모든 문자를 대문자로 사용하며 단어 사이에 밑줄을 넣어준다.

```
let variableLikeThis = '';
const CONSTANTS_LIKE_THIS = 0;
```

-   함수명은 카멜케이스(camelCase)를 사용한다.
-   소문자로 시작하며 새로운 단어의 첫 번째 문자는 대문자로 사용한다.

```
const getName = () => {

};
```

-   객체의 값이 함수일 경우 단축 또는 arrow function 으로 선언한다. (this 바인딩 필요 유무에 따라 판단)

```
let object = {
    key: 'value',
    normalFunction: function() {
        X
    }
    normalFunction() {
        O
    },
    arrowFunction: () => {
        O
    }
}
```

-   String 문자열은 Single Quotes(')을 사용한다.
-   보간 기능이 필요한 경우 백틱(`)을 사용한다.

```
let name = '홍길동';
<div>`내 이름은 ${name}입니다.`</div>
```

-   HTML 속성값과 JSON Data의 key, value는 Double Quotes(")을 사용한다.

```
<div class="container"></div>
```

```
{
    "key": "value"
}
```

-   괄호와 키워드 및 콜론(:) 간 간격 준수.

```
if (A) {

} else if (B) {

} else {

}

switch (state) {
    case 'A':
        break;
    default:
        break;
}

const object = {
    a: '',
    b: ''
}
```

<br />

## 웹컨테이너를 위한 React 세팅

#### (웹컨테이너 미사용시 적용 X)

### 패키지 설치

```
yarn add react-app-rewired run-script-os zip-webpack-plugin
```

-   react-app-rewired
    -   CRA(create-react-app) 환경에서 webpack 추가 설정
        -   <em>eject 후 webpack 수정 지양</em>
-   run-script-os
    -   OS별 명령어 실행 (copy 등)
-   zip-webpack-plugin
    -   zip 파일 생성을 위한 플러그인

### 환경변수 파일 추가

-   .env.development
-   .env.production

### config-overrides.js 파일 추가

-   r eact-app-rewired 가 참조하는 웹팩 추가 설정 파일
    -   zip 파일 생성을 위한 세팅 추가

### package.json scripts 수정 및 추가

-   start: "cp .env.development ./.env && react-app-rewired start"
-   build: "cp .env.production ./.env && react-app-rewired build"
    -   CRA 환경변수 참조 방식: .env.development 파일 참조 후 .env 파일 생성
-   zip ~ clean:buildZip:darwin:linux 까지 프로세스 추가
    -   zip 파일 생성 및 빌드 후 webcontainer 폴더로 복사

### 노드 바이너리 경로 충돌 대응

-   npm 경로 충돌 에러 시 .npmrc 파일 추가
    -   scripts-prepend-node-path=true
