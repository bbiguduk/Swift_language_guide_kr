# 제네릭 매개변수와 인자 (Generic Parameters and Arguments)

선언을 일반화하여 구체적인 타입을 추상화합니다.

이 챕터에서 제네릭 타입, 함수, 이니셜라이저에 대한 매개변수와 인자를 설명합니다.
제네릭 타입, 함수, 서브스크립트, 이니셜라이저를 선언할 때
해당 제네릭 타입, 함수, 이니셜라이저가 동작할 수 있는 타입 매개변수를 지정합니다.
이러한 타입 매개변수는
제네릭 타입의 인스턴스가 생성되거나 제네릭 함수 또는 이니셜라이저가 호출될 때
실제 구체적인 타입 인자에 의해 대체되는 플레이스 홀더 역할을 합니다.

Swift의 제네릭에 대한 개요는 [제네릭 (Generics)](../language-guide-1/generics.md)을 참고바랍니다.

<!--
  NOTE: Generic types are sometimes referred to as :newTerm:`parameterized types`
  because they're declared with one or more type parameters.
-->

## 제네릭 매개변수 절 (Generic Parameter Clause)

*제네릭 매개변수 절(generic parameter clause)*은 제네릭 타입이나 함수의 타입 매개변수와
해당 매개변수에 대한 관련 제약조건과 요구사항을 지정합니다.
제네릭 매개변수 절은 꺾쇠 괄호(<>)로 둘러싸여 있고
다음의 형식을 가집니다:

```swift
<<#generic parameter list#>>
```

위 형식의 *제네릭 매개변수 목록(generic parameter list)*은 콤마로 구분된 제네릭 매개변수의 목록이고
각 매개변수는 다음의 형식을 가집니다:

```swift
<#type parameter#>: <#constraint#>
```

제네릭 매개변수는 *타입 매개변수(type parameter)*와 선택 사항인 *제약조건(constraint)*으로 구성됩니다.
위 형식의 *타입 매개변수(type parameter)*는 단순히 플레이스 홀더 타입의
이름입니다
(예를 들어 `T`, `U`, `V`, `Key`, `Value` 등).
함수나
이니셜라이저의 시그니처를 포함하여 타입, 함수, 이니셜라이저 선언의
나머지 부분에서 타입 매개변수와 모든 연관 타입에 접근할 수 있습니다.

위 형식의 *제약조건(constraint)*은 타입 매개변수가 특정 클래스를 상속하거나
프로토콜 또는 프로토콜 합성을 준수해야 함을 지정합니다.
예를 들어 아래 제네릭 함수에서 제네릭 매개변수 `T: Comparable`은
타입 매개변수 `T`를 대신하는 모든 타입 인자는 `Comparable` 프로토콜을
준수해야 함을 나타냅니다.

```swift
func simpleMax<T: Comparable>(_ x: T, _ y: T) -> T {
    if x < y {
        return y
    }
    return x
}
```

<!--
  - test: `generic-params`

  ```swifttest
  -> func simpleMax<T: Comparable>(_ x: T, _ y: T) -> T {
        if x < y {
           return y
        }
        return x
     }
  ```
-->

예를 들어 `Int`와 `Double`은 `Comparable` 프로토콜을 준수하므로
이 함수는 두 타입의 인자를 허용합니다. 제네릭 타입과 다르게
제네릭 함수나 이니셜라이저를 사용할 때는 제네릭 인자 절을 지정하지 않습니다.
대신 타입 인자는 함수나 이니셜라이저에
전달되는 인자의 타입으로부터 추론됩니다.

```swift
simpleMax(17, 42) // T is inferred to be Int
simpleMax(3.14159, 2.71828) // T is inferred to be Double
```

<!--
  - test: `generic-params`

  ```swifttest
  >> let r0 =
  -> simpleMax(17, 42) // T is inferred to be Int
  >> assert(r0 == 42)
  >> let r1 =
  -> simpleMax(3.14159, 2.71828) // T is inferred to be Double
  >> assert(r1 == 3.14159)
  ```
-->

<!--
  Rewrite the above to avoid bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

### 제네릭 Where 절 (Generic Where Clauses)

타입 매개변수와 연관 타입에 요구사항을 추가로 지정하려면
타입이나 함수의 본문의
시작 중괄호 직전에 제네릭 `where` 절을 포함할 수 있습니다.
제네릭 `where` 절은 `where` 키워드로 구성되고
다음에 콤마로 구분된 하나 이상의 *요구사항(requirements)*의 목록으로 구성됩니다.

```swift
where <#requirements#>
```

제네릭 `where` 절에서 *요구사항(requirements)*은 타입 매개변수가 클래스를 상속하거나
프로토콜 또는 프로토콜 합성을 준수해야 함을 지정합니다.
제네릭 `where` 절은 타입 매개변수에 대한 간단 제약조건
(예를 들어 `<T: Comparable>`은 `<T> where T: Comparable`과 동일)을
표현하는 편의 문법을 제공하지만,
타입 매개변수와 연관 타입에
더 복잡한 제약조건을 제공하기 위해 사용할 수 있습니다.
예를 들어 타입 매개변수의 연관 타입을 프로토콜을 준수하도록 제한할 수 있습니다.
예를 들어 `<S: Sequence> where S.Iterator.Element: Equatable`은
`S`가 `Sequence` 프로토콜을 준수하고
연관 타입 `S.Iterator.Element`는
`Equatable` 프로토콜을 준수하도록 지정합니다.
이 제약조건은 시퀀스의 각 요소가 비교 가능함을 보장합니다.

`==` 연산자를 사용하여
두 타입이 동일하다는 요구사항을 지정할 수도 있습니다.
예를 들어 `<S1: Sequence, S2: Sequence> where S1.Iterator.Element == S2.Iterator.Element`은
`S1`과 `S2`가 `Sequence` 프로토콜을 준수하고
두 시퀀스의 요소가 같은 타입이어야 한다는 제약조건을 나타냅니다.

타입 매개변수를 대체하는 모든 타입 인자는
타입 매개변수에 있는 모든 제약조건과 요구사항을 충족해야 합니다.

제네릭 `where` 절은
타입 매개변수를 포함하는 선언의 일부로 나타나거나
타입 매개변수를 포함하는 선언 내부에 중첩된 선언의
일부로 나타날 수 있습니다.
중첩된 선언에 대한 제네릭 `where` 절은
둘러싸는 선언의 타입 매개변수를 참조할 수 있습니다;
그러나
`where` 절의 요구사항은
작성된 선언에만 적용됩니다.

둘러싸는 선언도 `where` 절을 가지고 있으면,
두 절의 요구사항은 결합됩니다.
아래 예시에서 `startsWithZero()`는
`Element`가 `SomeProtocol`과 `Numeric` 모두 준수하는 경우에만 가능합니다.

```swift
extension Collection where Element: SomeProtocol {
    func startsWithZero() -> Bool where Element: Numeric {
        return first == .zero
    }
}
```

<!--
  - test: `contextual-where-clauses-combine`

  ```swifttest
  >> protocol SomeProtocol { }
  >> extension Int: SomeProtocol { }
  -> extension Collection where Element: SomeProtocol {
         func startsWithZero() -> Bool where Element: Numeric {
             return first == .zero
         }
     }
  >> print( [1, 2, 3].startsWithZero() )
  << false
  ```
-->

<!--
  - test: `contextual-where-clause-combine-err`

  ```swifttest
  >> protocol SomeProtocol { }
  >> extension Bool: SomeProtocol { }

  >> extension Collection where Element: SomeProtocol {
  >>     func returnTrue() -> Bool where Element == Bool {
  >>         return true
  >>     }
  >>     func returnTrue() -> Bool where Element == Int {
  >>         return true
  >>     }
  >> }
  !$ error: no type for 'Self.Element' can satisfy both 'Self.Element == Int' and 'Self.Element : SomeProtocol'
  !! func returnTrue() -> Bool where Element == Int {
  !!                                            ^
  ```
-->

타입 매개변수에 다른 제약조건, 요구사항 또는 둘 다 제공하여
제네릭 함수나 이니셜라이저를 오버로드할 수 있습니다.
오버로드된 제네릭 함수나 이니셜라이저를 호출할 때
컴파일러는 이러한 제약조건을 사용하여 호출할 오버로드된
함수나 이니셜라이저를 결정합니다.

제네릭 `where` 절에 대한 자세한 정보와
제네릭 함수 선언의 예시를 보려면
[제네릭 Where 절 (Generic Where Clauses)](../language-guide-1/generics.md#제네릭-where-절-generic-where-clauses)을 참고바랍니다.

> Grammar of a generic parameter clause:
>
> *generic-parameter-clause* → **`<`** *generic-parameter-list* **`>`** \
> *generic-parameter-list* → *generic-parameter* | *generic-parameter* **`,`** *generic-parameter-list* \
> *generic-parameter* → *type-name* \
> *generic-parameter* → *type-name* **`:`** *type-identifier* \
> *generic-parameter* → *type-name* **`:`** *protocol-composition-type*
>
> *generic-where-clause* → **`where`** *requirement-list* \
> *requirement-list* → *requirement* | *requirement* **`,`** *requirement-list* \
> *requirement* → *conformance-requirement* | *same-type-requirement*
>
> *conformance-requirement* → *type-identifier* **`:`** *type-identifier* \
> *conformance-requirement* → *type-identifier* **`:`** *protocol-composition-type* \
> *same-type-requirement* → *type-identifier* **`==`** *type*

<!--
  NOTE: A conformance requirement can only have one type after the colon,
  otherwise, you'd have a syntactic ambiguity
  (a comma-separated list types inside of a comma-separated list of requirements).
-->

## 제네릭 인자 절 (Generic Argument Clause)

*제네릭 인자 절(generic argument clause)*은 제네릭 타입의
타입 인자를 지정합니다.
제네릭 인자 절은 꺾쇠 괄호(<>)로 둘러싸여져 있고
다음의 형식을 가집니다:

```swift
<<#generic argument list#>>
```

위 형식의 *제네릭 인자 목록(generic argument list)*은 콤마로 구분된 타입 인자의 목록입니다.
*타입 인자(type argument)*는 제네릭 타입의 제네릭 매개변수 절에 있는
해당 타입 매개변수를 대체하는 실제 구체적인 타입의 이름입니다.
그 결과는 해당 제네릭 타입의 특수 버전이 됩니다.
아래 예시는 Swift 표준 라이브러리의
제네릭 딕셔너리 타입의 단순화한 버전을 보여줍니다.

```swift
struct Dictionary<Key: Hashable, Value>: Collection, ExpressibleByDictionaryLiteral {
    /* ... */
}
```

<!--
  TODO: How are we supposed to wrap code lines like the above?
-->

제네릭 `Dictionary` 타입의 특수한 버전인 `Dictionary<String, Int>`는
제네릭 매개변수 `Key: Hashable`과 `Value`를
구체적인 타입 인자 `String`과 `Int`로 대체하여 형성됩니다.
각 타입 인자는 제네릭 `where` 절에 지정된 추가 요구사항을 포함하여
자신이 대체하는 제네릭 매개변수의 모든 제약조건을 충족해야 합니다.
위의 예시에서 `Key` 타입 매개변수는 `Hashable` 프로토콜을 준수하도록 제약되므로
`String`도 `Hashable` 프로토콜을 준수해야 합니다.

타입 매개변수 자체를 제네릭 타입의 특수한 버전인
타입 인자로 대체할 수도 있습니다(적절한 제약조건과 요구사항을 충족하는 경우).
예를 들어 `Array<Element>`의 타입 매개변수 `Element`를
배열의 특수한 버전인 `Array<Int>`로 대체하여
요소가 정수인 배열을 형성할 수 있습니다.

```swift
let arrayOfArrays: Array<Array<Int>> = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```

<!--
  - test: `array-of-arrays`

  ```swifttest
  -> let arrayOfArrays: Array<Array<Int>> = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
  ```
-->

[제네릭 매개변수 절 (Generic Parameter Clause)](./generic-parameters-and-arguments.md#제네릭-매개변수-절-generic-parameter-clause)에서 언급했듯이
제네릭 함수나 이니셜라이저의
타입 인자로 지정하기 위해 제네릭 인자 절을 사용하지 않습니다.

> Grammar of a generic argument clause:
>
> *generic-argument-clause* → **`<`** *generic-argument-list* **`>`** \
> *generic-argument-list* → *generic-argument* | *generic-argument* **`,`** *generic-argument-list* \
> *generic-argument* → *type*

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->
