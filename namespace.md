# 네임스페이스 적용하기 (Namespacing)

더 많은 검사기를 추가하게 되면, 타입을 추적하고 다른 객체와 이름 충돌을 방지하기 위해 일종의 구조 체계가 필요합니다. 전역 네임스페이스에 다른 이름을 많이 넣는 대신, 객체들을 하나의 네임스페이스로 감싸겠습니다.

이 예에서는 모든 검사기 관련 개체를 `Validation`이라는 하나의 네임스페이스로 옮기겠습니다.여기서 인터페이스 및 클래스가 네임스페이스 외부에서도 접근 가능하도록 선언부에 `export`를 붙입니다.반면, 변수 `letterRegexp`와 `numberRegexp`는 구현 세부 사항이므로 외부로 내보내지 않아 네임스페이스 외부 코드에서 접근할 수 없습니다. 파일 하단의 테스트 코드에서, 네임스페이스 외부에서 사용될 때 타입의 이름을 검증해야 합니다 (예: `Validation.LetterOnlyValidator`).

### Namespaced Validators

```jsx
namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }
    const lettersRegexp = /^[A-Za-z]+$/;
    const numberRegexp = /^[0-9]+$/;
    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }
    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}
// 시도해 볼 샘플
let strings = ["Hello", "98052", "101"];
// 사용할 검사기
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();
// 각 문자열이 각 검사기를 통과했는지 표시
for (let s of strings) {
    for (let name in validators) {
        console.log(`"${ s }" - ${ validators[name].isAcceptable(s) ? "matches" : "does not match" } ${ name }`);
    }
}
```

### Splitting Across Files

애플리케이션 규모가 커지면, 코드를 여러 파일로 분할해야 유지 보수가 용이합니다.

여기서 `Validation` 네임스페이스를 여러 파일로 분할합니다. 파일이 분리되어 있어도 같은 네임스페이스에 기여할 수 있고 마치 한 곳에서 정의된 것처럼 사용할 수 있습니다. 파일 간 의존성이 존재하므로, 참조 태그를 추가하여 컴파일러에게 파일 간의 관계를 알립니다. 그 외에 테스트 코드는 변경되지 않았습니다.

- Validation.ts

```jsx
namespace Validation{
    export interface StringValidator{
        isAcceptable(s: string): boolean;
    }
}
```

- LettersOnlyValidators.ts

```jsx
/// <reference path="Validation.ts" />
namespace Validation {
    const lettersRegexp = /^[A-Za-z]+$/;
    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }
}
```

- Test.ts

```jsx
/// <reference path="Validation.ts" />
/// <reference path="LettersOnlyValidator.ts" />
/// <reference path="ZipCodeValidator.ts" />
// Some samples to try
let strings = ["Hello", "98052", "101"];
// Validators to use
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();
// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        console.log(`"${ s }" - ${ validators[name].isAcceptable(s) ? "matches" : "does not match" } ${ name }`);
    }
}
```

### 별칭 (Aliases)

```jsx
namespace Shapes {
    export namespace Polygons {
        export class Triangle { }
        export class Square { }
    }
}
import polygons = Shapes.Polygons;
let sq = new polygons.Square(); // 'new Shapes.Polygons.Square()'와 동일
```

참고
https://www.typescriptlang.org/ko/docs/handbook/namespaces.html