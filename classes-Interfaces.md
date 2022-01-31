Object-oriented Programming(OOP)가 뭔가?

실제 생활의  Entitiy를 쓰는 것과 같다.

**Objects**

- 코드에서 작업하는 것들이며 class들의 instances이다.

**Class**

- object를 위한 Blueprint이다.
- object가 어떻게 생겼는지, 어떤 method와 properties를 가지고 있는 지 정의한다.

```typescript
class Department {
	name: string;
	construnctor(n: string) {
		this.name = n;
	}
}
const accounting = new Department('Accounting');
```
자바스크립트로 컴파일하기
```typescript
"use strict";
class Department {
  constructor(n) {
    this.name = n;
  }
}
```

### Constructor Functions & The “this” keyword

```jsx
class Department {
	name: string;
	constructor(n: string) {
		this.name = n;
	}
	describe() {
		console.log('Department: ' + this.name);
	}
}
const accounting = new Department('Accounting')
accounting.describe()
// console.log -> Department: Accounting

```

name에 바로 접근할수 없고, this로 접근해야한다.

```jsx
const accountingCopy = { describe: accounting.describe };
accountingCopy.describe();
// console.log -> Department: undefined
```

그 인스턴스가 아니고 더미 오브젝트기 때문에 undefined가 나온다.

그래서 describe에 this를 넣어보자.

```jsx
class Department {
	name: string;
	constructor(n: string) {
		this.name = n;
	}
	describe(this: Department) {
		console.log('Department: ' + this.name);
	}
}
const accounting = new Department('Accounting')
accounting.describe()
// console.log -> Department: Accounting
```

```jsx
const accountingCopy = { name: 'Programming', describe: accounting.describe };
accountingCopy.describe();
// console.log -> Department: Programming
```
### private and public Access Modifiers

```jsx
class Department {
	name: string;
	employees: string[] = [];

	constructor() {
		this.name: = n;
	}
	describe(this: Department) {
		console.log()
	}
	addEmployee(employee: string) {
		this.employees.push(employ)
	}
	printEmployeeInformation() {
		console.log(this.employees.length);
		console.log(this.employees);
	}
}

const accounting = new Department('Accounting')
accounting.addEmployee('Max')
accounting.addEmployee('Tim')
```

여기서 문제는 우리는 addEmployee 메소드를 이용하는 방법 말고도 employees property에 직접 접근해서 사람을 추가하는게 가능하다는 것이다.

```jsx
**📌 문제**
accounting.employees[2] = 'Kim'
```

큰 팀에서 일하기 위해서 한가지 방법으로만 사용하는게 좋다. 그렇기 때문에, private가 필요해진다.

```jsx
class Department {
	name: string;
	private employees: string[] = []; // private 추가

	constructor() {
		this.name: = n;
	}
	describe(this: Department) {
		console.log()
	}
	addEmployee(employee: string) {
		this.employees.push(employ)
	}
	printEmployeeInformation() {
		console.log(this.employees.length);
		console.log(this.employees);
	}
}

const accounting = new Department('Accounting')
accounting.addEmployee('Max')
accounting.addEmployee('Tim')
accounting.employees[2] = 'Kim' // **📌 에러**
```

✅  Vanilla javascript는 private 필드 syntax는 ‘private’와 ‘public’ 키워드를 사용하지 않는다.

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Public_class_fields](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Public_class_fields)

### Shortanand Initalization & readonly properties

constructor에서 initalize와 동시에 property를 정의할수 있다.

```jsx
class Department {
  // private id: string;
  // private name: string;
  private employees: string[] = [];

  constructor(private id: string, public name: string) {
    // this.id = id;
    // this.name = n;
  }

  describe(this: Department) {
    console.log(`Department (${this.id}): ${this.name}`);
  }

  addEmployee(employee: string) {
    // validation etc
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}

const accounting = new Department('d1', 'Accounting');

accounting.addEmployee('Max');
accounting.addEmployee('Manu');

// accounting.employees[2] = 'Anna';

accounting.describe();
accounting.name = 'NEW NAME';
accounting.printEmployeeInformation();

// const accountingCopy = { name: 'DUMMY', describe: accounting.describe };

// accountingCopy.describe();
```

readonly는 자바스크립트에 없는 타입스크립트의 기능이다.

```jsx
class Department {
  // private readonly id: string;
  // private name: string;
  private employees: string[] = [];

  constructor(private readonly id: string, public name: string) {
    // this.id = id;
    // this.name = n;
  }

  describe(this: Department) {
    console.log(`Department (${this.id}): ${this.name}`);
  }

  addEmployee(employee: string) {
    // validation etc
    // this.id = 'd2';
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}

const accounting = new Department('d1', 'Accounting');

accounting.addEmployee('Max');
accounting.addEmployee('Manu');

// accounting.employees[2] = 'Anna';

accounting.describe();
accounting.name = 'NEW NAME';
accounting.printEmployeeInformation();

// const accountingCopy = { name: 'DUMMY', describe: accounting.describe };

// accountingCopy.describe();
```
### Inheritance
상속받은 클래스에서 constructor를 사용하지 않으면 부모 클래스의 생성자를 그대로 사용한다. 하지만 constructor를 사용하면 super() 메서드를 사용하여 부모 클래스를 초기화해줘야한다.

```jsx
class Department {
  // private readonly id: string;
  // private name: string;
  private employees: string[] = [];

  constructor(private readonly id: string, public name: string) {
    // this.id = id;
    // this.name = n;
  }

  describe(this: Department) {
    console.log(`Department (${this.id}): ${this.name}`);
  }

  addEmployee(employee: string) {
    // validation etc
    // this.id = 'd2';
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}

class ITDepartment extends Department {
  admins: string[];
  constructor(id: string, admins: string[]) {
    super(id, 'IT');
    this.admins = admins;
  }
}

class AccountingDepartment extends Department {
  constructor(id: string, private reports: string[]) {
    super(id, 'Accounting');
  }

  addReport(text: string) {
    this.reports.push(text);
  }

  printReports() {
    console.log(this.reports);
  }
}

const it = new ITDepartment('d1', ['Max']);

it.addEmployee('Max');
it.addEmployee('Manu');

// it.employees[2] = 'Anna';

it.describe();
it.name = 'NEW NAME';
it.printEmployeeInformation();

console.log(it);

const accounting = new AccountingDepartment('d2', []);

accounting.addReport('Something went wrong...');

accounting.printReports();

// const accountingCopy = { name: 'DUMMY', describe: accounting.describe };

// accountingCopy.describe();
```
### Overriding Properties & The “protected” Modifier
private는 그 클래스 안에서만 사용이 가능하다. 그래서 상속한 클래스는 사용이 불가능한데, protected를 사용하면 가능하다.

```jsx
class Department {
  // private readonly id: string;
  // private name: string;
  protected employees: string[] = [];

  constructor(private readonly id: string, public name: string) {
    // this.id = id;
    // this.name = n;
  }

  describe(this: Department) {
    console.log(`Department (${this.id}): ${this.name}`);
  }

  addEmployee(employee: string) {
    // validation etc
    // this.id = 'd2';
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}

class ITDepartment extends Department {
  admins: string[];
  constructor(id: string, admins: string[]) {
    super(id, 'IT');
    this.admins = admins;
  }
}

class AccountingDepartment extends Department {
  constructor(id: string, private reports: string[]) {
    super(id, 'Accounting');
  }

  addEmployee(name: string) {
    if (name === 'Max') {
      return;
    }
    this.employees.push(name);
  }

  addReport(text: string) {
    this.reports.push(text);
  }

  printReports() {
    console.log(this.reports);
  }
}

const it = new ITDepartment('d1', ['Max']);

it.addEmployee('Max');
it.addEmployee('Manu');

// it.employees[2] = 'Anna';

it.describe();
it.name = 'NEW NAME';
it.printEmployeeInformation();

console.log(it);

const accounting = new AccountingDepartment('d2', []);

accounting.addReport('Something went wrong...');

accounting.addEmployee('Max');
accounting.addEmployee('Manu');

accounting.printReports();
accounting.printEmployeeInformation();

// const accountingCopy = { name: 'DUMMY', describe: accounting.describe };

// accountingCopy.describe();
```
### Getters & Setters
getter은 기본적으로 property이며 값을 얻기 위해메서드를 실행하는 프로퍼티이다. private한 필드를 encapsulate하는 용도로 사용하곤 한다.
```typescript
class Department {
  // private readonly id: string;
  // private name: string;
  protected employees: string[] = [];

  constructor(private readonly id: string, public name: string) {
    // this.id = id;
    // this.name = n;
  }

  describe(this: Department) {
    console.log(`Department (${this.id}): ${this.name}`);
  }

  addEmployee(employee: string) {
    // validation etc
    // this.id = 'd2';
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}

class ITDepartment extends Department {
  admins: string[];
  constructor(id: string, admins: string[]) {
    super(id, 'IT');
    this.admins = admins;
  }
}

class AccountingDepartment extends Department {
  private lastReport: string;

  get mostRecentReport() {
    if (this.lastReport) {
      return this.lastReport;
    }
    throw new Error('No report found.');
  }

  set mostRecentReport(value: string) {
    if (!value) {
      throw new Error('Please pass in a valid value!');
    }
    this.addReport(value);
  }

  constructor(id: string, private reports: string[]) {
    super(id, 'Accounting');
    this.lastReport = reports[0];
  }

  addEmployee(name: string) {
    if (name === 'Max') {
      return;
    }
    this.employees.push(name);
  }

  addReport(text: string) {
    this.reports.push(text);
    this.lastReport = text;
  }

  printReports() {
    console.log(this.reports);
  }
}

const it = new ITDepartment('d1', ['Max']);

it.addEmployee('Max');
it.addEmployee('Manu');

// it.employees[2] = 'Anna';

it.describe();
it.name = 'NEW NAME';
it.printEmployeeInformation();

console.log(it);

const accounting = new AccountingDepartment('d2', []);

accounting.mostRecentReport = 'Year End Report';
accounting.addReport('Something went wrong...');
console.log(accounting.mostRecentReport);

accounting.addEmployee('Max');
accounting.addEmployee('Manu');

accounting.printReports();
accounting.printEmployeeInformation();

// const accountingCopy = { name: 'DUMMY', describe: accounting.describe };

// accountingCopy.describe();
```
### Abstract class


상속:  **extends로 표현되며** 부모에서 선언된 메소드 혹은 변수를 자식은 모두 상속 받아 그대로 사용 할 수 있다.

오버라이드: **implements** 는 부모의 메소드나 변수를 그대로 가져다 쓰는게 아닌 반드시 **오버라이드** 해서 사용 해야 한다.

abstract 클래스로 인스턴스를 만들수 없고, 상속받는 클래스로 만들수 있다. 이 abstract 클래스는 describe처럼 공통의 메소드를 다르게 구현하고 싶을 때 이용하면 좋다.

```typescript

abstract class Department {
  static fiscalYear = 2020;
  // private readonly id: string;
  // private name: string;
  protected employees: string[] = [];

  constructor(protected readonly id: string, public name: string) {
    // this.id = id;
    // this.name = n;
    // console.log(Department.fiscalYear);
  }

  static createEmployee(name: string) {
    return { name: name };
  }

  abstract describe(this: Department): void; // 상속받는 클래스는 무조건 구현해야한다.

  addEmployee(employee: string) {
    // validation etc
    // this.id = 'd2';
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}

class ITDepartment extends Department {
  admins: string[];
  constructor(id: string, admins: string[]) {
    super(id, 'IT');
    this.admins = admins;
  }

  describe() {
    console.log('IT Department - ID: ' + this.id);
  }
}

class AccountingDepartment extends Department {
  private lastReport: string;

  get mostRecentReport() {
    if (this.lastReport) {
      return this.lastReport;
    }
    throw new Error('No report found.');
  }

  set mostRecentReport(value: string) {
    if (!value) {
      throw new Error('Please pass in a valid value!');
    }
    this.addReport(value);
  }

  constructor(id: string, private reports: string[]) {
    super(id, 'Accounting');
    this.lastReport = reports[0];
  }

  describe() {
    console.log('Accounting Department - ID: ' + this.id);
  }

  addEmployee(name: string) {
    if (name === 'Max') {
      return;
    }
    this.employees.push(name);
  }

  addReport(text: string) {
    this.reports.push(text);
    this.lastReport = text;
  }

  printReports() {
    console.log(this.reports);
  }
}

const employee1 = Department.createEmployee('Max');
console.log(employee1, Department.fiscalYear);

const it = new ITDepartment('d1', ['Max']);

it.addEmployee('Max');
it.addEmployee('Manu');

// it.employees[2] = 'Anna';

it.describe();
it.name = 'NEW NAME';
it.printEmployeeInformation();

console.log(it);

const accounting = new AccountingDepartment('d2', []);

accounting.mostRecentReport = 'Year End Report';
accounting.addReport('Something went wrong...');
console.log(accounting.mostRecentReport);

accounting.addEmployee('Max');
accounting.addEmployee('Manu');

// accounting.printReports();
// accounting.printEmployeeInformation();
accounting.describe();

// const accountingCopy = { name: 'DUMMY', describe: accounting.describe };

// accountingCopy.describe();
```

### Singleton & Private Constructs

클래스가 하나의 인스턴스만 가지고 싶다면 singleton 활용하면 된다.. 클래스 내부에서 instance를 private static으로 정의한다.
```typescript
class AccountingDepartment extends Department {
  private lastReport: string;
  private static instance: AccountingDepartment; // private니까 직접 접근 불가.

  get mostRecentReport() {
    if (this.lastReport) {
      return this.lastReport;
    }
    throw new Error('No report found.');
  }

  set mostRecentReport(value: string) {
    if (!value) {
      throw new Error('Please pass in a valid value!');
    }
    this.addReport(value);
  }

  private constructor(id: string, private reports: string[]) { // constructor를 
    // private로 둠. 외부에서 쓰지 못함
    super(id, 'Accounting');
    this.lastReport = reports[0];
  }

  static getInstance() { // getInstance로 instance에 접근 가능
    if (AccountingDepartment.instance) {
      return this.instance;
    }
    this.instance = new AccountingDepartment('d2', []);
    return this.instance;
  }

  describe() {
    console.log('Accounting Department - ID: ' + this.id);
  }

  addEmployee(name: string) {
    if (name === 'Max') {
      return;
    }
    this.employees.push(name);
  }

  addReport(text: string) {
    this.reports.push(text);
    this.lastReport = text;
  }

  printReports() {
    console.log(this.reports);
  }
}

const accounting = AccountingDepartment.getInstance();
const accounting2 = AccountingDepartment.getInstance();

console.log(accounting, accounting2);

accounting.mostRecentReport = 'Year End Report';
accounting.addReport('Something went wrong...');
console.log(accounting.mostRecentReport);

accounting.addEmployee('Max');
accounting.addEmployee('Manu');
```