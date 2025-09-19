# 타입 (Types)

내장된 명명 타입(named type)과 복합 타입(compound type)을 사용합니다.

Swift에는 두 가지 종류의 타입이 있습니다: 명명 타입과 복합 타입입니다.
*명명 타입(named type)*은 정의될 때 특정 이름을 부여할 수 있는 타입입니다.
명명 타입은 클래스, 구조체, 열거형, 프로토콜을 포함합니다.
예를 들어
`MyClass`라는 커스텀 클래스의 인스턴스는 `MyClass` 타입을 가집니다.
커스텀 명명 타입 외에도
Swift 표준 라이브러리는 배열, 딕셔너리, 옵셔널 값을 나타내는 타입을 포함하여
자주 사용하는 명명 타입이 정의되어 있습니다.

다른 언어에서 일반적으로 기본 타입이나 원시 타입으로 간주되는 데이터 타입(Data types) —--
숫자, 문자, 문자열과 같은 타입도 —--
Swift 표준 라이브러리에서는 구조체를 사용하여 정의되고 구현된
명명 타입입니다.
명명 타입이므로,
필요에 따라 프로그램에 맞게 동작을
[확장 (Extensions)][확장 (Extensions)](../language-guide-1/extensions.md))과 [확장 선언 (Extension Declaration)](./declarations.md#확장-선언-extension-declaration)에서 설명한대로
확장 선언을 사용하여 확장할 수 있습니다.

*복합 타입(compound type)*은 Swift 언어 자체에 정의된 이름이 없는 타입입니다.
복합 타입은 두 가지 종류의 타입이 있습니다: 함수 타입과 튜플 타입입니다.
복합 타입은 명명 타입과 다른 복합 타입을 포함할 수 있습니다.
예를 들어 튜플 타입 `(Int, (Int, Int))`는 두 요소를 포함합니다:
첫 번째는 명명 타입인 `Int`,
두 번째는 다른 복합 타입인 `(Int, Int)`입니다.

명명 타입이나 복합 타입을 묶을 소괄호를 넣을 수 있습니다.
그러나 타입을 묶은 소괄호는 아무런 영향을 주지 않습니다.
예를 들어 `(Int)`는 `Int`와 같습니다.

이 챕터에서는 Swift 언어 자체에 정의된 타입과
Swift의 타입 추론에 대해 설명합니다.

> Grammar of a type:
>
> *type* → *function-type* \
> *type* → *array-type* \
> *type* → *dictionary-type* \
> *type* → *type-identifier* \
> *type* → *tuple-type* \
> *type* → *optional-type* \
> *type* → *implicitly-unwrapped-optional-type* \
> *type* → *protocol-composition-type* \
> *type* → *opaque-type* \
> *type* → *boxed-protocol-type* \
> *type* → *metatype-type* \
> *type* → *any-type* \
> *type* → *self-type* \
> *type* → **`(`** *type* **`)`**

## 타입 주석 (Type Annotation)

*타입 주석(type annotation)*은 변수 타입이나 표현식 타입을 명시적으로 지정합니다.
타입 주석은 아래 예시에서 보여주듯이
콜론(`:`)으로 시작하고 타입으로 끝납니다:

```swift
let someTuple: (Double, Double) = (3.14159, 2.71828)
func someFunction(a: Int) { /* ... */ }
```

<!--
  - test: `type-annotation`

  ```swifttest
  -> let someTuple: (Double, Double) = (3.14159, 2.71828)
  -> func someFunction(a: Int) { /* ... */ }
  ```
-->

첫 번째 예시에서
표현식 `someTuple`은 튜플 타입 `(Double, Double)`을 갖도록 지정합니다.
두 번째 예시는
함수 `someFunction`의 매개변수 `a`는 타입 `Int`를 갖도록 지정합니다.

타입 주석은 타입 앞에 타입 속성을 포함할 수 있습니다.

> Grammar of a type annotation:
>
> *type-annotation* → **`:`** *attributes*_?_ *type*

## 타입 식별자 (Type Identifier)

*타입 식별자(type identifier)*는 명명 타입이나
명명 타입 또는 복합 타입의 타입 별칭(type alias)을 참조합니다.

대부분의 경우 타입 식별자는 같은 이름의
명명 타입을 직접 참조합니다.
예를 들어 `Int`는 명명 타입 `Int`를 직접 참조하는 타입 식별자이고,
타입 식별자 `Dictionary<String, Int>`는
명명 타입 `Dictionary<String, Int>`를 직접 참조합니다.

타입 식별자가 동일한 이름의 타입을 참조하지 않는 두 가지 경우가 있습니다.
첫 번째 경우는 타입 식별자가 명명 타입이나 복합 타입의 타입 별칭을 참조하는 경우입니다.
예를 들어 아래 예시에서
타입 주석에서 `Point`는 튜플 타입 `(Int, Int)`를 나타냅니다.

```swift
typealias Point = (Int, Int)
let origin: Point = (0, 0)
```

<!--
  - test: `type-identifier`

  ```swifttest
  -> typealias Point = (Int, Int)
  -> let origin: Point = (0, 0)
  ```
-->

두 번째 경우는 타입 식별자가 다른 모듈에서 선언되거나 다른 타입 내에 중첩된
명명 타입을 참조하기 위해 점(`.`) 구문을 사용하는 경우입니다.
예를 들어 다음 코드의 타입 식별자는 `ExampleModule` 모듈에 선언된
명명 타입 `MyType`을 참조합니다.

```swift
var someValue: ExampleModule.MyType
```

<!--
  - test: `type-identifier-dot`

  ```swifttest
  -> var someValue: ExampleModule.MyType
  !$ error: cannot find type 'ExampleModule' in scope
  !! var someValue: ExampleModule.MyType
  !!                ^~~~~~~~~~~~~
  ```
-->

> Grammar of a type identifier:
>
> *type-identifier* → *type-name* *generic-argument-clause*_?_ | *type-name* *generic-argument-clause*_?_ **`.`** *type-identifier* \
> *type-name* → *identifier*

## 튜플 타입 (Tuple Type)

*튜플 타입(tuple type)*은 소괄호 안에 콤마로 구분된 타입의 목록입니다.

튜플 타입을 함수의 반환 타입으로 사용하여
함수가 여러 값을 포함하는 단일 튜플을 반환하도록 할 수 있습니다.
튜플 타입의 요소에 이름을 지정하고 해당 이름을 사용하여
개별 요소의 값을 참조할 수도 있습니다. 요소 이름은
바로 뒤에 콜론(`:`)이 오는 식별자로 지정합니다. 이러한 기능을 보여주는 예시는
[여러 개의 반환값이 있는 함수 (Functions with Multiple Return Values)](../language-guide-1/functions.md#여러-개의-반환값이-있는-함수-functions-with-multiple-return-values)를 참고바랍니다.

튜플 타입의 요소가 이름을 가지는 경우
이름은 타입의 부분입니다.

```swift
var someTuple = (top: 10, bottom: 12)  // someTuple is of type (top: Int, bottom: Int)
someTuple = (top: 4, bottom: 42) // OK: names match
someTuple = (9, 99)              // OK: names are inferred
someTuple = (left: 5, right: 5)  // Error: names don't match
```

<!--
  - test: `tuple-type-names`

  ```swifttest
  -> var someTuple = (top: 10, bottom: 12)  // someTuple is of type (top: Int, bottom: Int)
  -> someTuple = (top: 4, bottom: 42) // OK: names match
  -> someTuple = (9, 99)              // OK: names are inferred
  -> someTuple = (left: 5, right: 5)  // Error: names don't match
  !$ error: cannot assign value of type '(left: Int, right: Int)' to type '(top: Int, bottom: Int)'
  !! someTuple = (left: 5, right: 5)  // Error: names don't match
  !!             ^~~~~~~~~~~~~~~~~~~
  ```
-->

모든 튜플 타입은 두 개 이상의 타입을 포함하고,
단, `Void`는 예외이며, 빈 튜플 타입 인 `()`에 대한 타입 별칭입니다.

> Grammar of a tuple type:
>
> *tuple-type* → **`(`** **`)`** | **`(`** *tuple-type-element* **`,`** *tuple-type-element-list* **`)`** \
> *tuple-type-element-list* → *tuple-type-element* | *tuple-type-element* **`,`** *tuple-type-element-list* \
> *tuple-type-element* → *element-name* *type-annotation* | *type* \
> *element-name* → *identifier*

## 함수 타입 (Function Type)

*함수 타입(function type)*은 함수, 메서드, 클로저의 타입을 나타내고
화살표(`->`)로 매개변수와 반환 타입을 구분하여 구성합니다:

```swift
(<#parameter type#>) -> <#return type#>
```

*매개변수 타입(parameter type)*은 콤마로 구분된 타입의 목록입니다.
*반환 타입(return type)*은 튜플 타입일 수 있으므로,
함수 타입은 여러 값을 반환하는
함수와 메서드를 지원합니다.

함수 타입이 `() -> T`일 경우
(`T`는 모든 타입)
해당 매개변수는 `autoclosure` 속성을 적용하여
호출 시점에 암시적으로 클로저를 생성할 수 있습니다.
이것은 함수를 호출할 때
명시적으로 클로저를 작성할 필요없이
표현식의 평가를 연기하는
구문상 편리함을 제공합니다.
자동 클로저 함수 타입 매개변수의 예시는
[자동 클로저 (Autoclosures)](../language-guide-1/closures.md#자동-클로저-autoclosures)를 참고바랍니다.

함수 타입의 *매개변수 타입*은 가변 매개변수(variadic parameters)를 가질 수 있습니다.
문법적으로
가변 매개변수는 `Int...`와 같이
기본 타입 이름 뒤에 점 3개(`...`)로 구성합니다. 가변 매개변수는 기본 타입 이름의
요소를 포함하는 배열로 처리합니다. 예를 들어 가변 매개변수 `Int...`는
`[Int]`로 처리합니다. 가변 매개변수를 사용하는 예시는
[가변 매개변수 (Variadic Parameters)](../language-guide-1/functions.md#가변-매개변수-variadic-parameters)를 참고바랍니다.

in-out 매개변수(in-out parameter)를 지정하려면 `inout` 키워드를 매개변수 타입 앞에 붙여야 합니다.
가변 매개변수나 반환 타입에는 `inout` 키워드를 사용할 수 없습니다.
In-out 매개변수는 [In-Out 매개변수 (In-Out Parameters)](../language-guide-1/functions.md#in-out-매개변수-in-out-parameters)에 설명되어 있습니다.

함수 타입이 하나의 매개변수만 가지고 있고
매개변수 타입이 튜플 타입인 경우
함수 타입을 작성할 때 튜플 타입을 괄호로 묶어야 합니다.
예를 들어
`((Int, Int)) -> Void`는
튜플 타입 `(Int, Int)`의
단일 매개변수를 가지고
값을 반환하지 않는 함수 타입입니다.
반대로 괄호가 없으면
`(Int, Int) -> Void`는
두 개의 `Int` 매개변수를 가지고
값을 반환하지 않는 함수 타입입니다.
마찬가지로 `Void`는 `()`의 타입 별칭이므로
함수 타입 `(Void) -> Void`는
`(()) -> ()`와 같습니다 —--
이것은 빈 튜플인 단일 인자를 가지는 함수와 같습니다.
이 타입은 `() -> ()`와 같지 않습니다 —--
이것은 인자를 가지지 않는 함수입니다.

함수와 메서드의 인자 이름은
해당 함수 타입의 일부가 아닙니다.
예를 들어:

<!--
  - test: `argument-names`

  ```swifttest
  -> func someFunction(left: Int, right: Int) {}
  -> func anotherFunction(left: Int, right: Int) {}
  -> func functionWithDifferentLabels(top: Int, bottom: Int) {}

  -> var f = someFunction // The type of f is (Int, Int) -> Void, not (left: Int, right: Int) -> Void.
  >> print(type(of: f))
  << (Int, Int) -> ()
  -> f = anotherFunction              // OK
  -> f = functionWithDifferentLabels  // OK
  ```
-->

```swift
func someFunction(left: Int, right: Int) {}
func anotherFunction(left: Int, right: Int) {}
func functionWithDifferentLabels(top: Int, bottom: Int) {}

var f = someFunction // The type of f is (Int, Int) -> Void, not (left: Int, right: Int) -> Void.
f = anotherFunction              // OK
f = functionWithDifferentLabels  // OK

func functionWithDifferentArgumentTypes(left: Int, right: String) {}
f = functionWithDifferentArgumentTypes     // Error

func functionWithDifferentNumberOfArguments(left: Int, right: Int, top: Int) {}
f = functionWithDifferentNumberOfArguments // Error
```

<!--
  - test: `argument-names-err`

  ```swifttest
  -> func someFunction(left: Int, right: Int) {}
  -> func anotherFunction(left: Int, right: Int) {}
  -> func functionWithDifferentLabels(top: Int, bottom: Int) {}

  -> var f = someFunction // The type of f is (Int, Int) -> Void, not (left: Int, right: Int) -> Void.
  -> f = anotherFunction              // OK
  -> f = functionWithDifferentLabels  // OK

  -> func functionWithDifferentArgumentTypes(left: Int, right: String) {}
  -> f = functionWithDifferentArgumentTypes     // Error
  !$ error: cannot assign value of type '(Int, String) -> ()' to type '(Int, Int) -> ()'
  !! f = functionWithDifferentArgumentTypes     // Error
  !! ^

  -> func functionWithDifferentNumberOfArguments(left: Int, right: Int, top: Int) {}
  -> f = functionWithDifferentNumberOfArguments // Error
  !$ error: type of expression is ambiguous without more context
  !! f = functionWithDifferentNumberOfArguments // Error
  !! ~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  ```
-->

인자 레이블은 함수 타입의 일부분이 아니므로,
함수 타입을 작성할 때 생략합니다.

```swift
var operation: (lhs: Int, rhs: Int) -> Int     // Error
var operation: (_ lhs: Int, _ rhs: Int) -> Int // OK
var operation: (Int, Int) -> Int               // OK
```

<!--
  - test: `omit-argument-names-in-function-type`

  ```swifttest
  -> var operation: (lhs: Int, rhs: Int) -> Int     // Error
  !$ error: function types cannot have argument labels; use '_' before 'lhs'
  !!    var operation: (lhs: Int, rhs: Int) -> Int     // Error
  !!                    ^
  !!                    _
  !$ error: function types cannot have argument labels; use '_' before 'rhs'
  !!    var operation: (lhs: Int, rhs: Int) -> Int     // Error
  !!                              ^
  !!                              _
  !$ error: invalid redeclaration of 'operation'
  !! var operation: (_ lhs: Int, _ rhs: Int) -> Int // OK
  !!     ^
  !$ note: 'operation' previously declared here
  !! var operation: (lhs: Int, rhs: Int) -> Int     // Error
  !!     ^
  !$ error: invalid redeclaration of 'operation'
  !! var operation: (Int, Int) -> Int               // OK
  !!     ^
  !$ note: 'operation' previously declared here
  !! var operation: (lhs: Int, rhs: Int) -> Int     // Error
  !!     ^
  -> var operation: (_ lhs: Int, _ rhs: Int) -> Int // OK
  -> var operation: (Int, Int) -> Int               // OK
  ```
-->

함수 타입에 하나 이상의 화살표(`->`)를 포함하는 경우,
함수 타입은 오른쪽에서 왼쪽으로 그룹화됩니다.
예를 들어
함수 타입 `(Int) -> (Int) -> Int`는 `(Int) -> ((Int) -> Int)`로 해석됩니다 —--
즉, 이 함수는 `Int`를 받아
`Int`를 받고 반환하는 또 다른 함수를 반환합니다.

오류를 던지거나 다시 던질 수 있는 함수 타입은
`throws` 키워드가 포함되어야 합니다.
오류의 타입을 지정하기 위해서
`throws` 뒤 괄호안에 타입을 포함할 수 있습니다.
오류 타입은 `Error` 프로토콜을 준수해야 합니다.
특정 타입 없이 `throws`를 작성하는 것은
`throws(any Error)`로 작성한 것과 같습니다.
`throws`를 생략하는 것은 `throws(Never)`로 작성하는 것과 같습니다.
함수가 던지는 오류 타입은
제네릭 타입, 박싱 프로토콜 타입, 불투명 타입을 포함하여
`Error`를 준수하는 모든 타입일 수 있습니다.

함수가 던지는 오류의 타입은 함수 타입의 일부이고,
오류 타입 간의 하위 타입 관계는
해당 함수 타입 간에도 하위 타입 관계가 성립한다는 의미입니다.
예를 들어, 임의로 정의한 `MyError` 타입을 선언하면,
상위 타입에서 하위 타입으로의
함수 타입 간의 관계는 다음과 같습니다:

1. 모든 오류를 던지는 함수는 `throws(any Error)`로 표시합니다.
2. 특정 오류를 던지는 함수는 `throws(MyError)`로 표시합니다.
3. 던지지 않는 함수는 `throws(Never)`로 표시합니다.

이 하위 타입 관계의 결과는 다음과 같습니다:

- 오류를 던지지 않는 함수는
  오류를 던지는 함수가 필요한 곳에서 사용할 수 있습니다.
- 구체적인 오류 타입을 던지는 함수는
  일반적인 던지는 함수가 필요한 곳에 사용할 수 있습니다.
- 더 구체적인 오류 타입을 던지는 함수는
  더 일반적인 오류 타입을 던지는 함수가 필요한 곳에 사용할 수 있습니다.

함수 타입에서 오류 타입으로
연관 타입(associated type)이나 제네릭 타입 매개변수(generic type parameter)를 사용하면,
연관 타입이나 제네릭 타입 매개변수는
암시적으로 `Error` 프로토콜을 준수해야 합니다.

던지는 함수와 다시 던지는 함수는
[오류를 던지는 함수와 메서드 (Throwing Functions and Methods)](./declarations.md#오류를-던지는-함수와-메서드-throwing-functions-and-methods)와
[오류를 다시 던지는 함수와 메서드 (Rethrowing Functions and Methods)](./declarations.md#오류를-다시-던지는-함수와-메서드-rethrowing-functions-and-methods)에 설명되어 있습니다.

비동기 함수의 함수 타입은
`async` 키워드로 표시되어야 합니다.
`async` 키워드는 함수의 타입의 부분이며,
동기 함수는 비동기 함수의 하위 타입(subtypes)입니다.
결과적으로 비동기 함수와 같은 위치에서
동기 함수를 사용할 수 있습니다.
비동기 함수에 대한 더 자세한 설명은
[비동기 함수와 메서드 (Asynchronous Functions and Methods)](./declarations.md#비동기-함수와-메서드-asynchronous-functions-and-methods)를 참고바랍니다.

<!--
  - test: `function-arrow-is-right-associative`

  ```swifttest
  >> func f(i: Int) -> (Int) -> Int {
  >>     func g(j: Int) -> Int {
  >>         return i + j
  >>     }
  >>     return g
  >> }

  >> let a: (Int) -> (Int) -> Int = f
  >> let r0 = a(3)(5)
  >> assert(r0 == 8)

  >> let b: (Int) -> ((Int) -> Int) = f
  >> let r1 = b(3)(5)
  >> assert(r1 == 8)
  ```
-->

### 비탈출 클로저의 제약 (Restrictions for Nonescaping Closures)

비탈출 함수(nonescaping function) 타입의 매개변수는
프로퍼티, 변수, 상수에 저장할 수 없습니다.
그렇게 하면 해당 값이 탈출될 수 있기 때문입니다.

<!--
  - test: `cant-store-nonescaping-as-Any`

  ```swifttest
  -> func f(g: ()->Void) { let x: Any = g }
  !$ error: converting non-escaping value to 'Any' may allow it to escape
  !! func f(g: ()->Void) { let x: Any = g }
  !!                                    ^
  ```
-->

비탈출 함수 타입의 매개변수는
다른 비탈출 함수 매개변수 인자로 전달할 수도 없습니다.
이 제약은 Swift가
런타임이 아닌 컴파일 시
메모리 충돌 접근에 대한 검사를 더 많이 수행하는데 도움이 됩니다.
예를 들어:

```swift
let external: (() -> Void) -> Void = { _ in () }
func takesTwoFunctions(first: (() -> Void) -> Void, second: (() -> Void) -> Void) {
    first { first {} }       // Error
    second { second {}  }    // Error

    first { second {} }      // Error
    second { first {} }      // Error

    first { external {} }    // OK
    external { first {} }    // OK
}
```

<!--
  - test: `memory-nonescaping-functions`

  ```swifttest
  -> let external: (() -> Void) -> Void = { _ in () }
  -> func takesTwoFunctions(first: (() -> Void) -> Void, second: (() -> Void) -> Void) {
         first { first {} }       // Error
         second { second {}  }    // Error

         first { second {} }      // Error
         second { first {} }      // Error

         first { external {} }    // OK
         external { first {} }    // OK
     }
  !$ error: passing a closure which captures a non-escaping function parameter 'first' to a call to a non-escaping function parameter can allow re-entrant modification of a variable
  !! first { first {} }       // Error
  !! ^
  !$ error: passing a closure which captures a non-escaping function parameter 'second' to a call to a non-escaping function parameter can allow re-entrant modification of a variable
  !! second { second {}  }    // Error
  !! ^
  !$ error: passing a closure which captures a non-escaping function parameter 'second' to a call to a non-escaping function parameter can allow re-entrant modification of a variable
  !! first { second {} }      // Error
  !! ^
  !$ error: passing a closure which captures a non-escaping function parameter 'first' to a call to a non-escaping function parameter can allow re-entrant modification of a variable
  !! second { first {} }      // Error
  !! ^
  ```
-->

위의 코드에서
`takesTwoFunctions(first:second:)`의 두 매개변수는 모두 함수입니다.
두 매개변수 모두 `@escaping`으로 표시되지 않으므로
결과적으로 둘 다 비탈출입니다.

위의 예시에서 "Error"로 표시된 네 개의 함수 호출은
컴파일러 오류를 일으킵니다.
`first`와 `second` 매개변수는
비탈출 함수이므로,
다른 비탈출 함수 매개변수 인자로 전달할 수 없습니다.
반대로
"OK"로 표시된 두 개의 함수 호출은 컴파일러 오류를 발생시키지 않습니다.
이 함수 호출은
`external`이 `takesTwoFunctions(first:second:)`의 매개변수가 아니므로 제약에 위배되지 않습니다.

제약을 피해야 하는 경우 매개변수 중 하나를 탈출로 표시하거나,
`withoutActuallyEscaping(_:do:)` 함수를 이용하여
탈출 함수로 비탈출 함수 매개변수 중 하나를 임시로 변경해야 합니다.
메모리 충돌 접근을 피하는 것에 대한 자세한 내용은
[메모리 안전성 (Memory Safety)](../language-guide-1/memory-safety.md)을 참고바랍니다.

> Grammar of a function type:
>
> *function-type* → *attributes*_?_ *function-type-argument-clause* **`async`**_?_ *throws-clause*_?_ **`->`** *type*
>
> *function-type-argument-clause* → **`(`** **`)`** \
> *function-type-argument-clause* → **`(`** *function-type-argument-list* **`...`**_?_ **`)`**
>
> *function-type-argument-list* → *function-type-argument* | *function-type-argument* **`,`** *function-type-argument-list* \
> *function-type-argument* → *attributes*_?_ *parameter-modifier*_?_ *type* | *argument-label* *type-annotation* \
> *argument-label* → *identifier*
>
> *throws-clause* → **`throws`** | **`throws`** **`(`** *type* **`)`**

<!--
  NOTE: Functions are first-class citizens in Swift,
  except for generic functions, i.e., parametric polymorphic functions.
  This means that monomorphic functions can be assigned to variables
  and can be passed as arguments to other functions.
  As an example, the following three lines of code are OK::

      func polymorphicF<T>(a: Int) -> T { return a }
      func monomorphicF(a: Int) -> Int { return a }
      var myMonomorphicF = monomorphicF

  But, the following is NOT allowed::

      var myPolymorphicF = polymorphicF
-->

## 배열 타입 (Array Type)

Swift 언어는 Swift 표준 라이브러리의 `Array<Element>` 타입에 대해
다음과 같은 문법을 제공합니다:

```swift
[<#type#>]
```

다른 표현으로 다음의 두 선언도 동일합니다:

```swift
let someArray: Array<String> = ["Alex", "Brian", "Dave"]
let someArray: [String] = ["Alex", "Brian", "Dave"]
```

<!--
  - test: `array-literal`

  ```swifttest
  >> let someArray1: Array<String> = ["Alex", "Brian", "Dave"]
  >> let someArray2: [String] = ["Alex", "Brian", "Dave"]
  >> assert(someArray1 == someArray2)
  ```
-->

두 경우 모두 상수 `someArray`는
문자열 배열로 선언됩니다. 배열의 요소는
대괄호에 유효한 인덱스 값을 지정하여 서브스크립트를 통해 접근할 수 있습니다:
`someArray[0]`는 `"Alex"`인 인덱스 0번째 요소를 나타냅니다.

대괄호 쌍을 중첩하여 다차원 배열을 만들 수 있습니다.
여기서 요소의 기본 타입 이름은 가장 안쪽
대괄호 쌍에 포함됩니다.
예를 들어
대괄호 세 쌍을 이용하여 정수의 3차원 배열을 생성할 수 있습니다:

```swift
var array3D: [[[Int]]] = [[[1, 2], [3, 4]], [[5, 6], [7, 8]]]
```

<!--
  - test: `array-3d`

  ```swifttest
  -> var array3D: [[[Int]]] = [[[1, 2], [3, 4]], [[5, 6], [7, 8]]]
  ```
-->

다차원 배열의 요소에 접근할 때,
가장 왼쪽에 있는 서브스크립트 인덱스는 배열의 가장 바깥 쪽에 있는 요소를
참조합니다. 오른쪽에 있는 다음 서브스크립트 인덱스는
한 차원 더 들어가는 중첩된 배열의 요소를 참조합니다. 이것은
위의 예시에서 `array3D[0]`은 `[[1, 2], [3, 4]]`를 참조하고
`array3D[0][1]`은 `[3, 4]`, `array3D[0][1][1]`은 값 4를 참조합니다.

Swift 표준 라이브러리 `Array` 타입에 대한 자세한 설명은
[배열 (Arrays)](../language-guide-1/collection-types.md#배열-arrays)을 참고바랍니다.

> Grammar of an array type:
>
> *array-type* → **`[`** *type* **`]`**

## 딕셔너리 타입 (Dictionary Type)

Swift 언어는 Swift 표준 라이브러리 `Dictionary<Key, Value>` 타입에 대해
아래와 같은 문법을 제공합니다:

```swift
[<#key type#>: <#value type#>]
```

다른 표현으로 다음의 두 선언도 동일합니다:

```swift
let someDictionary: [String: Int] = ["Alex": 31, "Paul": 39]
let someDictionary: Dictionary<String, Int> = ["Alex": 31, "Paul": 39]
```

<!--
  - test: `dictionary-literal`

  ```swifttest
  >> let someDictionary1: [String: Int] = ["Alex": 31, "Paul": 39]
  >> let someDictionary2: Dictionary<String, Int> = ["Alex": 31, "Paul": 39]
  >> assert(someDictionary1 == someDictionary2)
  ```
-->

두 경우 모두 상수 `someDictionary`는
키로 문자열과 값으로 정수를 가지는 딕셔너리가 선언됩니다.

딕셔너리의 값은 대괄호에
해당 키를 지정하여
접근할 수 있습니다: `someDictionary["Alex"]`는
키 `"Alex"`와 연관값을 참조합니다.
서브스크립트는 딕셔너리의 값 타입의 옵셔널 값을 반환합니다.
지정한 키가 딕셔너리에 포함되지 않은 경우
서브스크립트는 `nil`을 반환합니다.

딕셔너리의 키 타입은 Swift 표준 라이브러리 `Hashable` 프로토콜을 준수해야 합니다.

<!--
  Used to have an xref to :ref:`CollectionTypes_HashValuesForSetTypes` here.
  But it doesn't really work now that the Hashable content moved from Dictionary to Set.
-->

Swift 표준 라이브러리 `Dictionary` 타입의 자세한 설명은
[딕셔너리 (Dictionaries)](../language-guide-1/collection-types.md#딕셔너리-dictionaries)를 참고바랍니다.

> Grammar of a dictionary type:
>
> *dictionary-type* → **`[`** *type* **`:`** *type* **`]`**

## 옵셔널 타입 (Optional Type)

Swift 언어는 접미사 `?`를
Swift 표준 라이브러리에 정의된 명명 타입 `Optional<Wrapped>`의 문법적 편의로 정의합니다.
다른 표현으로 다음의 두 선언은 동일합니다:

```swift
var optionalInteger: Int?
var optionalInteger: Optional<Int>
```

<!--
  - test: `optional-literal`

  ```swifttest
  >> var optionalInteger1: Int?
  >> var optionalInteger2: Optional<Int>
  ```
-->

<!--
  We can't test the code listing above,
  because of the redeclaration of optionalInteger,
  so we at least test that the syntax shown in it compiles.
-->

두 경우 모두 변수 `optionalInteger`는
옵셔널 정수의 타입을 가지도록 선언됩니다.
타입과 `?`사이에는 공백이 없어야 합니다.

타입 `Optional<Wrapped>`는 열거형으로 정의되어 있고,
값이 존재하거나 존재하지 않을 수 있는 것을 나타내는데 `none`과 `some(Wrapped)`의 두 가지 케이스를 가집니다.
모든 타입은 명시적으로 선언되거나 옵셔널 타입으로 암시적으로 변환될 수 있습니다.
선언할 때 초기값을 제공하지 않으면
옵셔널 변수나 프로퍼티는 자동으로 `nil`을 설정합니다.

<!--
  TODO Add a link to the Optional Enum Reference page.
  For more information about the Optional type, see ...
-->

옵셔널 타입의 인스턴스는 값을 포함할 경우,
아래에서 보듯이 접시마 연산자 `!`를 사용하여 값에 접근할 수 있습니다:

```swift
optionalInteger = 42
optionalInteger! // 42
```

<!--
  - test: `optional-type`

  ```swifttest
  >> var optionalInteger: Int?
  -> optionalInteger = 42
  >> let r0 =
  -> optionalInteger! // 42
  >> assert(r0 == 42)
  ```
-->

<!--
  Refactor the above if possible to avoid using bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

`nil`의 값을 가지는 옵셔널을
언래핑 하기위해 `!` 연산자를 사용하면 런타임 오류가 발생합니다.

옵셔널 체이닝과 옵셔널 바인딩을 사용하여
옵셔널 표현식에 대해 조건적으로 작업을 수행할 수도 있습니다. 값이 `nil`인 경우에
작업은 더이상 수행되지 않으므로 런타임 오류가 발생하지 않습니다.

더 자세한 정보와 옵셔널 타입 사용에 대한 예시를 보려면
[옵셔널 (Optionals)](../language-guide-1/the-basics.md#옵셔널-optionals)을 참고바랍니다.

> Grammar of an optional type:
>
> *optional-type* → *type* **`?`**

## 암시적 언래핑 옵셔널 타입 (Implicitly Unwrapped Optional Type)

Swift 언어는 접미사 `!`를
Swift 표준 라이브러리에 정의된 명명 타입 `Optional<Wrapped>`의 문법적 편의로 정의하고,
옵셔널 타입에 접근할 때
자동으로 언래핑된 동작을 추가합니다.
`nil`의 값을 가지는 옵셔널에 암시적 언래핑을 사용하려고 하면
런타임 오류가 발생합니다.
암시적 언래핑 동작을 제외하고
다음의 두 선언은 동일합니다:

```swift
var implicitlyUnwrappedString: String!
var explicitlyUnwrappedString: Optional<String>
```

타입과 `!` 사이에 공백이 포함되지 않습니다.

암시적 언래핑은
해당 타입을 포함하는 선언의 의미를 변경하기 때문에
튜플 타입이나 제너릭 타입
--- 딕셔너리나 배열의 요소 타입 ---
내에 중첩된 옵셔널 타입은 암시적 언래핑으로 표시할 수 없습니다.
예를 들어:

```swift
let tupleOfImplicitlyUnwrappedElements: (Int!, Int!)  // Error
let implicitlyUnwrappedTuple: (Int, Int)!             // OK

let arrayOfImplicitlyUnwrappedElements: [Int!]        // Error
let implicitlyUnwrappedArray: [Int]!                  // OK
```

암시적 언래핑 옵셔널은
옵셔널 값과
동일한 `Optional<Wrapped>` 타입을 가지므로
코드에서 옵셔널을 사용할 수 있는
모든 곳에서 사용할 수 있습니다.
예를 들어 옵셔널의 변수, 상수, 프로퍼티에
암시적 언래핑 옵셔널의 값을 할당할 수 있고 그 반대도 가능합니다.

옵셔널과 마찬가지로 암시적 언래핑 옵셔널 변수나 프로퍼티를 선언할 때
초기값을 제공하지 않으면
자동으로 `nil`을 기본값으로 설정합니다.

암시적 언래핑 옵셔널 표현식에 조건적으로 동작을
수행하려면 옵셔널 체이닝을 사용합니다.
값이 `nil`이라면
더 이상 작업이 수행되지 않으며 런타임 오류가 발생하지 않습니다.

암시적 언래핑 옵셔널 타입에 대한 자세한 정보는
[암시적 언래핑 옵셔널 (Implicitly Unwrapped Optionals)](../language-guide-1/the-basics.md#암시적-언래핑-옵셔널-implicitly-unwrapped-optionals)을 참고바랍니다.

> Grammar of an implicitly unwrapped optional type:
>
> *implicitly-unwrapped-optional-type* → *type* **`!`**

## 프로토콜 합성 타입 (Protocol Composition Type)

*프로토콜 합성 타입(protocol composition type)*은 지정된 프로토콜의 목록에서
각 프로토콜을 준수해야 하는 타입을 정의하거나,
특정 클래스를 상속하면서
동시에 지정된 프로토콜의 목록애서 각 프로토콜을 준수해야 하는 타입을 정의할 수도 있습니다.
프로토콜 합성 타입은
타입 주석,
제네릭 매개변수 절,
제네릭 `where` 절을 지정할 때만 사용할 수 있습니다.

<!--
  In places where a comma-separated list of types is allowed,
  the P&Q syntax isn't allowed.
-->

프로토콜 합성 타입은 다음의 형식을 가집니다:

```swift
<#Protocol 1#> & <#Protocol 2#>
```

프로토콜 합성 타입을 사용하면 타입이 준수하려는 각 프로토콜을 상속하는
새롭운 프로토콜을 명시적으로 정의하지 않아도
타입이 여러 프로토콜의 요구사항을 준수하는 값을 지정할 수 있습니다.
예를 들어
`ProtocolA`, `ProtocolB`, `ProtocolC`를 상속하는
새로운 프로토콜을 선언하는 대신
프로토콜 합성 타입 `ProtocolA & ProtocolB & ProtocolC`를 사용할 수 있습니다.
마찬가지로 `SuperClass`의 하위 클래스와 `ProtocolA`를 준수하는
새로운 프로토콜을 선언하는 대신에
`SuperClass & ProtocolA`를 사용할 수 있습니다.

프로토콜 합성 목록의 각 항목은 다음 중 하나입니다;
이 목록은 최대 하나의 클래스를 포함할 수 있습니다:

- 클래스의 이름
- 프로토콜의 이름
- 기본 타입이 프로토콜 합성 타입, 프로토콜, 클래스인
  타입 별칭

프로토콜 합성 타입이 타입 별칭을 포함할 때,
동일한 프로토콜이 정의에
중복해서 나타날 수 있으며 ---
중복은 무시됩니다.
예를 들어
아래 코드에서 `PQR`의 정의는
`P & Q & R`과 동일합니다.

```swift
typealias PQ = P & Q
typealias PQR = PQ & Q & R
```

<!--
  - test: `protocol-composition-can-have-repeats`

  ```swifttest
  >> protocol P {}
  >> protocol Q {}
  >> protocol R {}
  -> typealias PQ = P & Q
  -> typealias PQR = PQ & Q & R
  ```
-->

> Grammar of a protocol composition type:
>
> *protocol-composition-type* → *type-identifier* **`&`** *protocol-composition-continuation* \
> *protocol-composition-continuation* → *type-identifier* | *protocol-composition-type*

## 불투명 타입 (Opaque Type)

*불투명 타입(opaque type)*은
구체적인 타입 지정없이
프로토콜이나 프로토콜 합성을 준수하는 타입을 정의합니다.

불투명 타입은 함수나 서브스크립트의 반환 타입,
함수, 서브스크립트, 이니셜라이저 매개변수의 타입,
또는 프로퍼티의 타입으로 나타납니다.
불투명 타입은 배열의 요소 타입이나 옵셔널의 래핑된 타입과 같은
튜플 타입 또는 제네릭 타입의 부분으로 나타날 수 없습니다.

불투명 타입은 다음의 형식을 가집니다:

```swift
some <#constraint#>
```

위 형식의 *제약조건(constraint)*은 클래스 타입,
프로토콜 타입,
프로토콜 합성 타입,
`Any`입니다.
어떤 값이 불투명 타입의 인스턴스로 사용되려면
그 값은 지정된 프로토콜이나
프로토콜 합성을 준수하거나
나열된 클래스를 상속해야 합니다.
불투명 값과 상호작용하는 코드는
*제약조건(constraint)*에 의해 정의된 인터페이스 범위 내에서만
값으로 사용할 수 있습니다.

<!--
  The wording above intentionally follows generic constraints
  because the meaning here and there is the same,
  and the compiler uses the same machinery for both under the hood.
-->

컴파일 시간에
불투명 타입을 가진 값은 구체적인 타입을 가지며,
Swift는 이 내부 타입을 사용해 최적화를 수행할 수 있습니다.
그러나
불투명 타입은 하나의 경계를 형성하며
이 경계를 통해 내부 타입에 대한 정보가 외부로 노출될 수 없습니다.

프로토콜 선언은 불투명 타입을 포함할 수 없습니다.
클래스는 final이 아닌 메서드(nonfinal method)의 반환 타입으로 불투명 타입을 사용할 수 없습니다.

불투명 타입을 반환 타입으로 사용하는 함수는
항상 하나의 동일한 내부 타입을 반환해야 합니다.
반환 타입은
함수의 제네릭 타입 매개변수도 포함할 수 있습니다.
예를 들어 함수 `someFunction<T>()`는
타입 `T`나 `Dictionary<String, T>`의 값을 반환할 수 있습니다.

매개변수에 불투명 타입을 작성하는 것은
제네릭 타입에 대한 편의 문법(syntactic sugar)이며
이것은 제네릭 타입 매개변수의 이름을 지정하지 않는 방식입니다.
암시적 제네릭 타입 매개변수는
불투명 타입에 명시된 프로토콜을 준수하도록 요구하는 제약조건을 가집니다.
여러 불투명 타입을 작성한 경우에는
각각 자체 제네릭 타입 매개변수를 생성합니다.
예를 들어 다음의 선언은 동일합니다:

```swift
func someFunction(x: some MyProtocol, y: some MyProtocol) { }
func someFunction<T1: MyProtocol, T2: MyProtocol>(x: T1, y: T2) { }
```

두 번째 선언에서
제네릭 타입 매개변수 `T1`과 `T2`는 이름을 가지므로
코드의 다른 곳에서 이 타입을 참조할 수 있습니다.
반면에
첫 번째 선언의 제네릭 타입 매개변수는
이름이 없으므로 다른 코드에서 참조할 수 없습니다.

가변 매개변수(variadic parameter)의 타입에는 불투명 타입을 사용할 수 없습니다.

반환되는 함수 타입의 매개변수나
함수 타입인 매개변수 타입의 매개변수로
불투명 타입을 사용할 수 없습니다.
이러한 위치에서는
함수를 호출하는 쪽에서 알 수 없는 타입의
값을 구성해야 하기 때문입니다.

```swift
protocol MyProtocol { }
func badFunction() -> (some MyProtocol) -> Void { }  // Error
func anotherBadFunction(callback: (some MyProtocol) -> Void) { }  // Error
```

> Grammar of an opaque type:
>
> *opaque-type* → **`some`** *type*

## 박싱 프로토콜 타입 (Boxed Protocol Type)

박싱 프로토콜 타입(Boxed Protocol Type)은
프로토콜이나 프로토콜 합성을 준수하는 타입을 정의하고
프로그램 실행 중에도
준수하는 타입이 변경될 수 있습니다.

박싱 프로토콜 타입은 다음과 같은 형식을 가집니다:

```swift
any <#constraint#>
```

위 형식의 *제약조건(constraint)*은 프로토콜 타입,
프로토콜 합성 타입,
프로토콜 타입의 메타타입,
프로토콜 합성 타입의 메타타입입니다.

런타임에
박싱 프로토콜 타입의 인스턴스는
*제약조건*에 충족하는 모든 타입의 값을 포함할 수 있습니다.
이런 동작은 컴파일 시간에 구체적인 타입이 고정되는
불투명 타입(opaque type) 동작과 대조됩니다.
박싱 프로토콜 타입으로 사용되는 추가 간접 수준을
*박싱(boxing)*이라 합니다.
박싱(Boxing)은 일반적으로 별도의 메모리 공간을 할당하고
해당 값에 접근할 때 추가적인 간접 수준을 필요로 하므로
런타임 성능에 영향을 줄 수 있습니다.

`any`를 `Any`나 `AnyObject` 타입에 적용해도
이미 박싱 프로토콜 타입이므로
아무런 효과가 없습니다.

<!--
  - test: `any-any-does-nothing`

   >> var x: any Any = 12
   >> var y: Any = 12
   >> print(type(of: x))
   << Int
   >> print(type(of: y))
   << Int
   >> print(type(of: x) == type(of: y))
   << true
-->

<!--
  - test: `any-anyobject-does-nothing`

   >> import Foundation
   >> var x: any AnyObject = NSNumber(value: 12)
   >> var y: AnyObject = NSNumber(value: 12)
   >> print(type(of: x))
   << __NSCFNumber
   >> print(type(of: y))
   << __NSCFNumber
   >> print(type(of: x) == type(of: y))
   << true
-->

<!--
Contrast P.Type with (any P.Type) and (any P).Type
https://github.com/swiftlang/swift-evolution/blob/main/proposals/0335-existential-any.md#metatypes
-->

> Grammar of a boxed protocol type
>
> *boxed-protocol-type* --> **``any``** *type*

## 메타타입 타입 (Metatype Type)

*메타타입 타입(metatype type)*은
클래스 타입, 구조체 타입, 열거형 타입, 프로토콜 타입을 포함하여 모든 타입의 타입 자체를 나타냅니다.

클래스, 구조체, 열거형 타입의 메타타입은
해당 타입의 이름 다음에 `.Type`을 붙입니다.
런타임 시 해당 프로토콜을 준수하는
구체적인 타입이 아닌 ---
프로토콜 타입의 메타타입은 프로토콜의 이름 다음에 `.Protocol`을 붙입니다.
예를 들어 클래스 타입 `SomeClass`의 메타타입은 `SomeClass.Type`이고
프로토콜 `SomeProtocol`의 메타타입은 `SomeProtocol.Protocol`입니다.

접미사 `self` 표현식을 사용하여 타입을 값으로 접근할 수 있습니다.
예를 들어 `SomeClass.self`는 `SomeClass`의 인스턴스가 아닌
`SomeClass` 자체를 반환합니다.
그리고 `SomeProtocol.self`는 런타임 시 `SomeProtocol`을 준수하는 타입의 인스턴스가 아닌
`SomeProtocol` 자체를 반환합니다.
다음 예시와 같이
타입의 인스턴스와 함께 `type(of:)` 함수를 호출하여
해당 인스턴스의 런타임 시 타입을 값으로 접근할 수 있습니다:

```swift
class SomeBaseClass {
    class func printClassName() {
        print("SomeBaseClass")
    }
}
class SomeSubClass: SomeBaseClass {
    override class func printClassName() {
        print("SomeSubClass")
    }
}
let someInstance: SomeBaseClass = SomeSubClass()
// The compile-time type of someInstance is SomeBaseClass,
// and the runtime type of someInstance is SomeSubClass
type(of: someInstance).printClassName()
// Prints "SomeSubClass"
```

<!--
  - test: `metatype-type`

  ```swifttest
  -> class SomeBaseClass {
         class func printClassName() {
             print("SomeBaseClass")
         }
     }
  -> class SomeSubClass: SomeBaseClass {
         override class func printClassName() {
             print("SomeSubClass")
         }
     }
  -> let someInstance: SomeBaseClass = SomeSubClass()
  -> // The compile-time type of someInstance is SomeBaseClass,
  -> // and the runtime type of someInstance is SomeSubClass
  -> type(of: someInstance).printClassName()
  <- SomeSubClass
  ```
-->

자세한 내용은
Swift 표준 라이브러리의
[`type(of:)`](https://developer.apple.com/documentation/swift/2885064-type)을 참고바랍니다.

메타타입 값을 사용하여
해당 타입의 인스턴스를 생성하려면 이니셜라이저 표현식을 사용할 수 있습니다.
클래스 인스턴스의 경우
호출되는 이니셜라이저는 `required` 키워드로 표시하거나
`final` 키워드로 전체 클래스를 표시해야 합니다.

```swift
class AnotherSubClass: SomeBaseClass {
    let string: String
    required init(string: String) {
        self.string = string
    }
    override class func printClassName() {
        print("AnotherSubClass")
    }
}
let metatype: AnotherSubClass.Type = AnotherSubClass.self
let anotherInstance = metatype.init(string: "some string")
```

<!--
  - test: `metatype-type`

  ```swifttest
  -> class AnotherSubClass: SomeBaseClass {
        let string: String
        required init(string: String) {
           self.string = string
        }
        override class func printClassName() {
           print("AnotherSubClass")
        }
     }
  -> let metatype: AnotherSubClass.Type = AnotherSubClass.self
  -> let anotherInstance = metatype.init(string: "some string")
  ```
-->

> Grammar of a metatype type:
>
> *metatype-type* → *type* **`.`** **`Type`** | *type* **`.`** **`Protocol`**

## Any 타입 (Any Type)

`Any` 타입은 다른 모든 타입의 값을 포함할 수 있습니다.
`Any`는 다음 타입의 인스턴스에 대해
구체적인 타입으로 사용할 수 있습니다:

- 클래스, 구조체, 열거형
- `Int.self`와 같은 메타타입
- 다양한 타입으로 구성된 튜플
- 클로저 또는 함수 타입

```swift
let mixed: [Any] = ["one", 2, true, (4, 5.3), { () -> Int in return 6 }]
```

<!--
  - test: `any-type`

  ```swifttest
  -> let mixed: [Any] = ["one", 2, true, (4, 5.3), { () -> Int in return 6 }]
  ```
-->

인스턴스에 대해 구체적인 타입으로 `Any`를 사용할 때,
해당 프로퍼티나 메서드에 접근하려면
먼저 알려진 타입으로 인스턴스를 캐스팅해야 합니다.
`Any`의 구체적인 타입인 인스턴스는
본래 동적 타입을 유지하고
`as`, `as?`, `as!`와 같은
타입 캐스팅 연산자(type-cast operators) 중 하나를 사용하여 타입을 캐스팅할 수 있습니다.
예를 들어
`as?`을 사용하여 배열의 첫 번째 객체를 `String`으로
다운캐스트합니다:

```swift
if let first = mixed.first as? String {
    print("The first item, '\(first)', is a string.")
}
// Prints "The first item, 'one', is a string."
```

<!--
  - test: `any-type`

  ```swifttest
  -> if let first = mixed.first as? String {
         print("The first item, '\(first)', is a string.")
     }
  <- The first item, 'one', is a string.
  ```
-->

캐스팅에 대한 자세한 내용은 [타입 캐스팅 (Type Casting)](../language-guide-1/type-casting.md)을 참고바랍니다.

`AnyObject` 프로토콜은 `Any` 타입과 유사합니다.
모든 클래스는 암시적으로 `AnyObject`를 준수합니다.
언어에 의해 정의되는
`Any`와 달리
`AnyObject`는 Swift 표준 라이브러리에 의해 정의됩니다.
더 자세한 내용은
[클래스 전용 프로토콜 (Class Only Protocols)](../language-guide-1/protocols.md#클래스-전용-프로토콜-class-only-protocols)과
[`AnyObject`](https://developer.apple.com/documentation/swift/anyobject)를 참고바랍니다.

> Grammar of an Any type:
>
> *any-type* → **`Any`**

## Self 타입 (Self Type)

`Self` 타입은 특정 타입이 아니라
현재 타입을 반복해서 쓰거나 그 이름을 알 필요 없이
편리하게 참조할 수 있습니다.

프로토콜 선언이나 프로토콜 멤버 선언에서
`Self` 타입은 프로토콜을 채택하는 최종 타입을 나타냅니다.

구조체, 클래스, 열거형 선언에서
`Self` 타입은 선언에 의해 도입된 타입을 참조합니다.
타입의 멤버 선언 내에서
`Self` 타입은 해당 타입을 참조합니다.
클래스 선언의 멤버에서
`Self` 타입은 다음의 경우에만 사용할 수 있습니다:

- 메서드의 반환 타입
- 읽기 전용 서브스크립트의 반환 타입
- 읽기 전용 연산 프로퍼티의 타입
- 메서드의 본문

예를 들어
아래의 코드는 반환 타입이 `Self`인
인스턴스 메서드 `f`를 보여줍니다.

<!--
  - test: `self-in-class-cant-be-a-parameter-type`

  ```swifttest
  -> class C { func f(c: Self) { } }
  !$ error: covariant 'Self' or 'Self?' can only appear as the type of a property, subscript or method result; did you mean 'C'?
  !! class C { func f(c: Self) { } }
  !!                     ^~~~
  !!                     C
  ```
-->

<!--
  - test: `self-in-class-can-be-a-subscript-param`

  ```swifttest
  >> class C { subscript(s: Int) -> Self { return self } }
  >> let c = C()
  >> _ = c[12]
  ```
-->

<!--
  - test: `self-in-class-can-be-a-computed-property-type`

  ```swifttest
  >> class C { var s: Self { return self } }
  >> let c = C()
  >> _ = c.s
  ```
-->

```swift
class Superclass {
    func f() -> Self { return self }
}
let x = Superclass()
print(type(of: x.f()))
// Prints "Superclass"

class Subclass: Superclass { }
let y = Subclass()
print(type(of: y.f()))
// Prints "Subclass"

let z: Superclass = Subclass()
print(type(of: z.f()))
// Prints "Subclass"
```

<!--
  - test: `self-gives-dynamic-type`

  ```swifttest
  -> class Superclass {
         func f() -> Self { return self }
     }
  -> let x = Superclass()
  -> print(type(of: x.f()))
  <- Superclass

  -> class Subclass: Superclass { }
  -> let y = Subclass()
  -> print(type(of: y.f()))
  <- Subclass

  -> let z: Superclass = Subclass()
  -> print(type(of: z.f()))
  <- Subclass
  ```
-->

위의 예시의 마지막 부분은
`Self`가 변수 `z` 자체의 컴파일 시 타입(compile-time type)인 `Superclass`가 아닌
`z` 값의 런타임 타입(runtime type)인 `Subclass`를 참조하는 것을 보여줍니다.

<!--
  TODO: Using Self as the return type from a subscript or property doesn't
  currently work.  The compiler allows it, but you get the wrong type back,
  and the compiler doesn't enforce that the subscript/property must be
  read-only.  See https://bugs.swift.org/browse/SR-10326
-->

중첩된 타입 선언 내에서
`Self` 타입은 가장 안쪽 타입 선언에 의해 도입된
타입을 참조합니다.

`Self` 타입은
Swift 표준 라이브러리에서
[`type(of:)`](https://developer.apple.com/documentation/swift/2885064-type) 함수와 동일한 타입을 참조합니다.
현재 타입의 멤버를 접근하기 위해 `Self.someStaticMember`라고 작성하는 것은
`type(of:self).someStaticMember`로 작성하는 것과 동일합니다.

> Grammar of a Self type:
>
> *self-type* → **`Self`**

## 타입 상속 절 (Type Inheritance Clause)

*타입 상속 절(type inheritance clause)*은 명명 타입이 상속하는 클래스와
명명 타입이 준수하는 프로토콜을 지정하기 위해 사용됩니다.
타입 상속 절은 콜론(`:`)으로 시작하고,
그 뒤에 타입 식별자의 목록이 옵니다.

클래스 타입은 하나의 상위 클래스를 상속할 수 있고 여러 개의 프로토콜을 준수할 수 있습니다.
클래스를 정의할 때,
상위 클래스의 이름은 타입 식별자의 목록에서 첫 번째로 나타나야 하고,
다음으로 준수하는 여러 개의 프로토콜을 작성합니다.
클래스가 다른 클래스를 상속하지 않으면
해당 목록은 프로토콜로 시작할 수 있습니다.
클래스 상속에 대한 자세한 설명과 예시는
[상속 (Inheritance)](../language-guide-1/inheritance.md)을 참고바랍니다.

클래스 타입 외 명명 타입은 프로토콜 목록만 상속하거나 준수할 수 있습니다.
프로토콜 타입은 여러 다른 프로토콜을 상속할 수 있습니다.
프로토콜 타입이 다른 프로토콜을 상속할 때,
그 상위 프로토콜의 요구사항이 통합되고
해당 프로토콜을 상속하는 모든 타입은 그 요구사항을 모두 준수해야 합니다.

열거형 정의의 타입 상속 절은 프로토콜 목록이거나
케이스의 원시값(raw values)을 지정하는 열거형인 경우
해당 원시값의 타입을 지정하는 단일 명명 타입일 수 있습니다.
타입 상속 절을 사용하여
원시값의 타입을 지정하는 열거형 정의의 예시는 [원시값 (Raw Values)](../language-guide-1/enumerations.md#원시값-raw-values)을 참고바랍니다.

> Grammar of a type inheritance clause:
>
> *type-inheritance-clause* → **`:`** *type-inheritance-list* \
> *type-inheritance-list* → *attributes*_?_ *type-identifier* | *attributes*_?_ *type-identifier* **`,`** *type-inheritance-list*

## 타입 추론 (Type Inference)

Swift는 광범위하게 *타입 추론(type inference)*을 사용하므로
코드에서 많은 변수와 표현식의 타입 전체나 타입 일부를 생략할 수 있습니다.
예를 들어
`var x: Int = 0`으로 작성하는 대신에 `var x = 0`으로
타입을 완벽하게 생략하고 작성할 수 있습니다 —--
컴파일러는 `x`를 타입 `Int`의 값으로 추론합니다.
유사하게 컨텍스트에서 전체 타입이 추론될 수 있을 때 타입을 생략할 수 있습니다.
예를 들어 `let dict: Dictionary = ["A": 1]`을 작성하면,
컴파일러는 `dict`이 타입 `Dictionary<String, Int>`이라고 추론합니다.

위의 두 예시에서
타입 정보는 표현식 트리의 잎에서 루트까지 전달됩니다.
즉,
`var x: Int = 0`에서 `x`의 타입은 먼저 `0`의 타입을 확인한 다음에
이 타입 정보를 루트(변수 `x`)까지 전달하여 추론합니다.

Swift에서 타입 정보는 루트에서 잎까지 반대로 흐를 수도 있습니다.
예를 들어 다음 예시에서
상수 `eFloat`의 명시적 타입 주석(`: Float`)은
숫자 리터럴 `2.71828`이 `Double`이 아닌 `Float` 타입으로 추론합니다.

```swift
let e = 2.71828 // The type of e is inferred to be Double.
let eFloat: Float = 2.71828 // The type of eFloat is Float.
```

<!--
  - test: `type-inference`

  ```swifttest
  -> let e = 2.71828 // The type of e is inferred to be Double.
  -> let eFloat: Float = 2.71828 // The type of eFloat is Float.
  ```
-->

Swift의 타입 추론은 단일 표현식이나 구문 수준에서 동작합니다.
이것은 표현식에서 생략된 타입이나 타입의 일부를 추론하는데 필요한 모든 정보는
표현식이나 하위 표현식 중 하나를
타입 검사(type-checking)하여 접근할 수 있습니다.

<!--
  TODO: Email Doug for a list of rules or situations describing when type-inference
  is allowed and when types must be fully typed.
-->

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->