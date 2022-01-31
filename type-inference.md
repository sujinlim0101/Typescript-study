### 타입 추론

타입스크립트를 처음 접한 개발자가 자바스크립트 코드를 포매팅할때 먼저 하는 일은  타입 구문을 넣는 것입니다. 타입스크립트는 타입을 위한 언어이기 때문에, 변수를 선언할 때마다 타입을 명시해야한다고 생각하기 때문입니다. 그러나 타입스크립트의 많은 타입 구문은 사실 불필요합니다. 다음과 같은 코드에 변수 타입을 선언한느것은 비생산적이며 형편없는 스타일로 여겨집니다.

```jsx
let x: number = 12;
```

이렇게만 해도 충분합니다.

```jsx
let x = 12;
```

```jsx
const person: {
  name: string;
  born: {
    where: string;
    when: string;
  };
  died: {
    where: string;
    when: string;
  }
} = {
  name: 'Sojourner Truth',
  born: {
    where: 'Swartekill, NY',
    when: 'c.1797',
  },
  died: {
    where: 'Battle Creek, MI',
    when: 'Nov. 26, 1883'
  }
};
```

위 같은 코드는 아래와 같은 코드로 충분합니다.

```jsx
const person = {
  name: 'Sojourner Truth',
  born: {
    where: 'Swartekill, NY',
    when: 'c.1797',
  },
  died: {
    where: 'Battle Creek, MI',
    when: 'Nov. 26, 1883'
  }
};
```

axis 변수를 string으로 예상하기 쉽지만 다음 예제처럼 'y'가 더 정확한 타입입니다. 이처럼 타입스크립트 추론은 우리 예상보다 더 정확할수 있습니다.

```jsx
const axis1: string = 'x';  // Type is string
const axis2 = 'y';  // Type is "y"
```

만약 아래와 같은 코드에서 id의 타입이 달라졌다고 해봅시다. 만약 logProduct내의 명시적 타입 구문이 없다며 코드는 아무런 수정 없이 타입 체커를 통과 했을 겁니다. 

```jsx
interface Product {
  id: number;
  name: string;
  price: number;
}

function logProduct(product: Product) {
  const id: number = product.id;
  const name: string = product.name;
  const price: number = product.price;
  console.log(id, name, price);
}
```

이럴 때는 비구조 할당문을 사용해 구현하는게 낫습니다.

```jsx
interface Product {
  id: string;
  name: string;
  price: number;
}
function logProduct(product: Product) {
  const {id, name, price} = product;
  console.log(id, name, price);
}
```

기본값이 있는 경우에도 타입 구문을 생략합니다.

```jsx
function parseNumber(str: string, base=10) {
  // ...
}
```

### 예외) 객체 리터럴, 함수 반환

**추론될수 있는 경우라도 객체 리터럴과 함수 반환에는 타입 명시를 고려하면 좋다. 내부 구현의 오류가 사용자 코드 위치에 나타내는 것을 방지한다.**

아래와 같은 정의에 타입을 명시하면 잉여 속성 체크가 동작합니다. 잉여 속성 체크는 특히 선택적 속성이 있는 타입의 오타같은 오류를 잡는데 효과적입니다. 그리고 변수가 사용되는 순간이 아닌 할당하는 시점에 오류가 표시되도록합니다.

### #객체 리터럴

```jsx
const elmo: Product = {
  name: 'Tickle Me Elmo',
  id: '048188 627152',
  price: 28.99,
};
```

반면에, 타입 구문 체크를 없새면 잉여 속성 체크가 되지 않고, 사용되는 시점에 표시됩니다.

```jsx
const furby = {
  name: 'Furby',
  id: 630509430963,
  price: 35,
};
```

📍 사용되는 시점에 에러남

```jsx
interface Product {
  id: string;
  name: string;
  price: number;
}

function logProduct(product: Product) {
  const id: number = product.id;
     // ~~ Type 'string' is not assignable to type 'number'
  const name: string = product.name;
  const price: number = product.price;
  console.log(id, name, price);
}
const furby = {
  name: 'Furby',
  id: 630509430963,
  price: 35,
};
logProduct(furby);
        // ~~~~~ Argument .. is not assignable to parameter of type 'Product'
        //         Types of property 'id' are incompatible
        //         Type 'number' is not assignable to type 'string'
```

그러나 타입 구문을 제대로 명시하면 실제로 실수가 발생한 부분에 오류를 표시해줍니다.

```jsx
interface Product {
  id: string;
  name: string;
  price: number;
}

function logProduct(product: Product) {
  const id: number = product.id;
     // ~~ Type 'string' is not assignable to type 'number'
  const name: string = product.name;
  const price: number = product.price;
  console.log(id, name, price);
}
 const furby: Product = {
   name: 'Furby',
   id: 630509430963,
// ~~ Type 'number' is not assignable to type 'string'
   price: 35,
 };
 logProduct(furby);
```

### #함수

캐시가 있으면 반환하고 없으면 fetch 하는 함수입니다.

```jsx
const cache: {[ticker: string]: number} = {};
function getQuote(ticker: string) {
  if (ticker in cache) {
    return cache[ticker];
  }
  return fetch(`https://quotes.example.com/?q=${ticker}`)
      .then(response => response.json())
      .then(quote => {
        cache[ticker] = quote;
        return quote;
      });
}
function considerBuying(x: any) {}
```

if 문에서 반환하는 타입이 number일수도 있으니 아래와 같이 에러가 발생합니다.

```jsx
const cache: {[ticker: string]: number} = {};
function getQuote(ticker: string) {
  if (ticker in cache) {
    return cache[ticker];
  }
  return fetch(`https://quotes.example.com/?q=${ticker}`)
      .then(response => response.json())
      .then(quote => {
        cache[ticker] = quote;
        return quote;
      });
}
function considerBuying(x: any) {}
getQuote('MSFT').then(considerBuying);
              // ~~~~ Property 'then' does not exist on type
              //        'number | Promise<any>'
              //      Property 'then' does not exist on type 'number'
```

이때 의도된 반환타입을 명시하면 정확한 위치에서 오류가 납니다.

```jsx
const cache: {[ticker: string]: number} = {};
function getQuote(ticker: string): Promise<number> {
  if (ticker in cache) {
    return cache[ticker];
        // ~~~~~~~~~~~~~ Type 'number' is not assignable to 'Promise<number>'
  }
  // COMPRESS
  return Promise.resolve(0);
  // END
}
```
참고

* 이펙티브 타입스크립트 책
