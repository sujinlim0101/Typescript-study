- TSConfig setup하기

```jsx
tsc logging.ts -w
tsc --init // tsconfig.json 가 생김
tsc -w // tsconfig가 있는 프로젝트의 타입스크립트가 모두 자바스크립트로 변환
tsc // 자바스크립트로 변환
```

- 프로젝트 구조 정리하기

```jsx
// tsconfig.json
"compilerOptions": {
	"outDir": "./build", // build 할 폴더 지정
	"rootDir": "./src", // 만약 src 디렉토리 외에 다른 곳에 ts 가 있으면 에러
},
"exclude": ["./src/dev.ts"] // 제외할 것 명시
"include": ["./src/ddd.ts"] // 포함할 것만 명시 
```

다른 디렉토리에서 타입스크립트 파일이 각각있다면 디렉토리 구조가 유지되면서 변환된다.

- 컴파일러 옵션들 파헤치기
    
    -tsconfig 옵션 살펴보기
    
    ```jsx
    "incremental": true, // 수정된 사항이 없다면 컴파일 x, 디스크에 보관하고 있다가 수정된 것만 o
    "target": "ES6", // 필요한 버전으로 컴파일
    "module": "ES6", // 모듈을 import 인지 require로 구현할 것인지
    "lib": [], // 라이브러리를 결정 대부분 설정 안함
    "allowJs": true, // 자바스크립트와 타입스크립트 모두 사용할 것인지
    "checkJs": true, // 잘못 작성된 부분에 에러를 표시
    "jsx": // 리액트 관련
    "sourceMap": true, // 디버깅 관련
    "outFile": "" // 자바스크립트 파일을 하나로 만듬
    "composite": true,
    "tsBuildInfoFile": "./", // incremental option이 true이면 관련된 정보들을 담을 파일을 지정
    "removeComments": true, // 코멘트 지우기
    "noEmit": true // 컴파일 에러가 있는지 없는지만 확인하고 실제로 컴파일 하지 않는다.
    ```
    
- 디버깅 하는 방법
    
    sourceMap을 true로 설정해서 디버깅을 편하게 할수 있다! .map 파일을 생성한다는 뜻이다.
    
    extension에서 Debugger for Chrome이 있음.