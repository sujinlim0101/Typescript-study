### Intersection Types

```tsx
type Admin = {
	name: string;
	privileges: string[];
}

type Employee = {
	name: string;
	startDate: Date;
}

type ElevatedEmployee = Admin & Employee; // 두 타입에 있는 모든 속성이 있어야한다.

const el: ElevatedEmployee = {
	name: 'Max'.
	privileges: ['sdf'],
	startDate: new Date()
}
```

Intersection type은 인터페이스의 상속과 관련이 깊다.

타입 대신 인터페이스로 바꿔보면 이렇다.

```tsx
interface Admin {
  name: string;
  privileges: string[];
};

interface Employee {
  name: string;
  startDate: Date;
};

interface ElevatedEmployee extends Employee, Admin {}

const e1: ElevatedEmployee = { // 마찬가지로 두 타입에 있는 모든 속성이 있어야한다.
  name: 'Max',
  privileges: ['create-server'],
  startDate: new Date()
};
```

Object 타입의 경우에는 간단하게도 이러한 오브젝트 프로퍼티들의 집합이 모두 있어야합니다. 하지만 유니온 타입의 경우는 다릅니다.

유니온타입의 경우에는 기본적으로 공통으로 가지고 있는 타입으로 지정됩니다.

```tsx
type Combinable = string | number;
type Numeric = number | boolean;

type Universal = Combinable & Numeric; // number type
```

### Type Guards

1. typeof 사용

```tsx
type Combinable = string | number;
type Numeric = number | boolean;

type Universal = Combinable & Numeric;

function add(a: Combinable, b: Combinable) {
	// typeof를 이용한 타입 가드
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
  return a + b;
}
```

1. property in object

```tsx
type UnknownEmployee = Employee | Admin;

function printEmployeeInformation(emp: UnknownEmployee) {
  console.log('Name: ' + emp.name);
	// emp.privileges --> error argument애 이 프로퍼티가 있을거라고 확신할수 없기 때문에
  if ('privileges' in emp) {
    console.log('Privileges: ' + emp.privileges);
  }
  if ('startDate' in emp) {
    console.log('Start Date: ' + emp.startDate);
  }
}

printEmployeeInformation({ name: 'Manu', startDate: new Date() }); // happy!
```

`emp.privileges`로 접근하면 에러가 나기 때문에 `'privileges' in emp` 를 써서 존재 여부를 확인하고, 로직을 작성한다.

1. 클래스의 경우 instanceof

```tsx
class Car {
  drive() {
    console.log('Driving...');
  }
}

class Truck {
  drive() {
    console.log('Driving a truck...');
  }

  loadCargo(amount: number) {
    console.log('Loading cargo ...' + amount);
  }
}

type Vehicle = Car | Truck;

const v1 = new Car();
const v2 = new Truck();

function useVehicle(vehicle: Vehicle) {
  vehicle.drive();
  if (vehicle instanceof Truck) {
    vehicle.loadCargo(1000);
  }
}
```

### Discriminated Unions

Discriminate Union이란 패턴인데, 유니통 타입과 타입 가드를 함께 쓸수 있도록 한 쉬운 방법이다.

번역하자면 구별된느 유니온 타입이다. 인터페이스들이 싱글턴 타입을 갖고 유니언 타입의 타입 가드를 사용하여  `switch` 에서 모든 경우의 수를 계산하고 각 인터페이스의 데이터까지 구별할수 있는 패턴이다.

```tsx
interface Bird {
  type: 'bird';
  flyingSpeed: number;
}

interface Horse {
  type: 'horse';
  runningSpeed: number;
}

type Animal = Bird | Horse;

function moveAnimal(animal: Animal) {
  let speed;
  switch (animal.type) {
    case 'bird': // IDE 에서 보여줘서 편하다.
      speed = animal.flyingSpeed;
      break;
    case 'horse':
      speed = animal.runningSpeed;
  }
  console.log('Moving at speed: ' + speed);
}
```

### Type casting

```tsx
// const userInputElement: HTMLElement | null
const userInputElement = document.getElementById('user-input');
if (userInputElement) {
  userInputElement.value = 'Hi there!'; // 에러 html이 input 타입인지 몰라서
}
```

두가지 방식으로 타입 캐스팅을 할수 있다.

- bracket을 이용해서 타입을 알려주는 방식

```tsx
*const userInputElement = <HTMLInputElement>document.getElementById('user-input')!;*
```

- as를 이용

```tsx
(userInputElement as HTMLInputElement).value = 'Hi there!';
```

### Index Properties

에러 메세지에 대한 타입을 만들건데, 어떤 필드가 쓰일지 작성 전에 모든 것을 알수 없다. `value` 타입에 관해서는 `string`인게 확실한데, `key`를 아직 알수 없는 상황이다.

```tsx
interface ErrorContainer { // { email: 'Not a valid email', username: 'Must start with a character!' }
  [prop: string]: string;
}

const errorBag: ErrorContainer = {
  email: 'Not a valid email!',
  username: 'Must start with a capital character!'
};
```

### Function Overloads

여러가지 함수를 정의하도록 하는 것이다. 

문제 상황: Combinable이 number | string 이라, 타입이 string 또는 number로 분명한 상황에서도 리턴타입이 Combinable로 예상된다.

```tsx
function add(a: Combinable, b: Combinable) { // 리턴타입이  Combinable
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
  return a + b; // 📌 문제! 둘다 숫자면 숫자가 리턴
}
const result = add('as', 'sdf');
result.split(' ') // 에러 string 타입으로 리턴됨에도 split 메소드 쓸수 없음

```

여러개의 함수를 정의해서 쓰면 된다.

```tsx

function add(a: string, b: string):number

function add(a: number, b: number):number

function add(a: Combinable, b: Combinable) {
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
  return a + b;
}
```

### Optional Chaning

**optional chaining** 연산자 (**`?.`**) 는 체인의 각 참조가 유효한지 명시적으로 검증하지 않고, 연결된 객체 체인 내에 깊숙이 위치한 속성 값을 읽을 수 있다.

```tsx
const adventurer = {
  name: 'Alice',
  cat: {
    name: 'Dinah'
  }
};

const dogName = adventurer.dog?.name;
console.log(dogName);
// expected output: undefined

console.log(adventurer.someNonExistentMethod?.());
// expected output: undefined
```

### Nullish Coleasing

**널 병합 연산자 (`??`)** 는 왼쪽 피연산자가 [null](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/null) 또는 [undefined](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/undefined)일 때 오른쪽 피연산자를 반환하고, 그렇지 않으면 왼쪽 피연산자를 반환하는 논리 연산자이다.

```tsx
const foo = null ?? 'default string';
console.log(foo);
// expected output: "default string"

const foo = 'a' ?? 'default string';
console.log(foo);
// expected output: "a"

const baz = 0 ?? 42;
console.log(baz);
// expected output: 0
```