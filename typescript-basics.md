## Using Types
### Primitive Type: string,number, boolean
* string은 "Hello, world"와 같은 문자열 값을 나타냅니다
* number은 42와 같은 숫자를 나타냅니다. JavaScript는 정수를 위한 런타임 값을 별도로 가지지 않으므로, int 또는 float과 같은 것은 존재하지 않습니다. 모든 수는 단순히 number입니다
* boolean은 true와 false라는 두 가지 값만을 가집니다

### 배열
[1, 2, 3]과 같은 배열의 타입을 지정할 때 number[] 구문을 사용할 수 있습니다.

### any
TypeScript는 또한 any라고 불리는 특별한 타입을 가지고 있으며, 특정 값으로 인하여 타입 검사 오류가 발생하는 것을 원하지 않을 때 사용할 수 있습니다.

### Enums

* enum Role {ADMIN, READ_ONLY} 로 사용하고, 따로 값 지정이 없으면 ADMIN은 0이 되고 READ_ONLY는 1 값이 된다.

### 함수
함수는 JavaScript에서 데이터를 주고 받는 주요 수단입니다. TypeScript에서는 함수의 입력 및 출력 타입을 지정할 수 있습니다.
매개변수 타입 표기
함수를 선언할 때, 함수가 허용할 매개변수 타입을 선언하기 위하여 각 매개변수 뒤에 타입을 표기할 수 있습니다. 매개변수 타입은 매개변수 이름 뒤에 표기합니다.

- 매개변수 타입 표기
```typescript
function greet(name: string) {
  console.log("Hello, " + name.toUpperCase() + "!!");
}
```
- 반환 타입 표기
반환 타입 또한 표기할 수 있습니다. 반환 타입은 매개변수 목록 뒤에 표기합니다.

```typescript
function getFavoriteNumber(): number {
  return 26;
}

```
### 객체 타입
원시 타입을 제외하고 가장 많이 마주치는 타입은 _객체 타입_입니다. 객체는 프로퍼티를 가지는 JavaScript 값을 말하는데, 대부분의 경우가 이에 해당하죠! 객체 타입을 정의하려면, 해당 객체의 프로퍼티들과 각 프로퍼티의 타입들을 나열하기만 하면 됩니다.

예를 들어, 아래 함수는 좌표로 보이는 객체를 인자로 받고 있습니다.
```typescript
// 매개 변수의 타입은 객체로 표기되고 있습니다.
function printCoord(pt: { x: number; y: number }) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });
```

### 유니언 타입


* 유니언 타입 정의하기

타입을 조합하는 첫 번째 방법은 유니언 타입을 사용하는 것입니다. 유니언 타입은 서로 다른 두 개 이상의 타입들을 사용하여 만드는 것으로 | 를 사용하여 만듭니다.

### Type alias

지금까지는 객체 타입과 유니언 타입을 사용할 때 직접 해당 타입을 표기하였습니다. 이는 편리하지만, 똑같은 타입을 한 번 이상 재사용하거나 또 다른 이름으로 부르고 싶은 경우도 존재합니다.
```typescript
type Point = {
  x: number;
  y: number;
};
 
// 앞서 사용한 예제와 동일한 코드입니다
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
 
printCoord({ x: 100, y: 100 });
```
### 인터페이스
인터페이스
인터페이스 선언은 객체 타입을 만드는 또 다른 방법입니다.

```typescript
interface Point {
  x: number;
  y: number;
}
 
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
 
printCoord({ x: 100, y: 100 });
```

### Type alias과 인터페이스의 차이점
타입 별칭과 인터페이스는 매우 유사하며, 대부분의 경우 둘 중 하나를 자유롭게 선택하여 사용할 수 있습니다. interface가 가지는 대부분의 기능은 type에서도 동일하게 사용 가능합니다. 이 둘의 가장 핵심적인 차이는, 타입은 새 프로퍼티를 추가하도록 개방될 수 없는 반면, 인터페이스의 경우 항상 확장될 수 있다는 점입니다.

* 확장하기 비교

인터페이스 확장하기

```typescript
interface Animal {
  name: string
}

interface Bear extends Animal {
  honey: boolean
}

const bear = getBear()
bear.name
bear.honey
```
교집합을 통하여 타입 확장하기


```typescript
type Animal = {
  name: string
}

type Bear = Animal & {
  honey: Boolean
}

const bear = getBear();
bear.name;
bear.honey;
```
기존의 인터페이스에 새 필드를 추가하기
```typescript
interface Window {
  title: string
}

interface Window {
  ts: TypeScriptAPI
}

const src = 'const a = "Hello World"';
window.ts.transpileModule(src, {});
```
타입은 생성된 뒤에는 달라질 수 없다
```typescript
type Window = {
  title: string
}

type Window = {
  ts: TypeScriptAPI
}

 // Error: Duplicate identifier 'Window'.
```
### 타입 단언

때로는 TypeScript보다 당신이 어떤 값의 타입에 대한 정보를 더 잘 아는 경우도 존재합니다.

```typescript
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;

```

### 리터럴 타입

string과 number와 같은 일반적인 타입 이외에도, 구체적인 문자열과 숫자 값을 타입 위치에서 지정할 수 있습니다.

```typescript
const constantString = "Hello World";
```

### null과 undefined

JavaScript에는 빈 값 또는 초기화되지 않은 값을 가리키는 두 가지 원시값이 존재합니다. 바로 null과 undefined입니다.

TypeScript에는 각 값에 대응하는 동일한 이름의 두 가지 타입이 존재합니다. 각 타입의 동작 방식은 strictNullChecks 옵션의 설정 여부에 따라 달라집니다.
* strictNullChecks 설정되었을 때

```typescript
function doSomething(x: string | undefined) {
  if (x === undefined) {
    // 아무 것도 하지 않는다
  } else {
    console.log("Hello, " + x.toUpperCase());
  }
}
```

참고
https://www.typescriptlang.org/ko/docs/handbook/2/everyday-types.html