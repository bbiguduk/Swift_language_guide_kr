# 기본 연산자 (Basic Operators)

할당, 산술, 비교와 같은 동작을 수행합니다.

*연산자(operator)*는 값을 확인, 변경, 결합하기 위해
사용하는 기호나 구 입니다.
예를 들어 덧셈 연산자(addition operator)(`+`)는 `let i = 1 + 2`에서처럼
두 숫자를 더하고,
논리 AND 연산자(`&&`)는 `if enteredDoorCode && passedRetinaScan`에서처럼
두 Boolean 값을 결합합니다.

Swift는 C와 같은 언어에서 알고 있는 연산자를 지원하고,
일반적인 코딩 오류를 방지하기 위해 여러 기능을 개선했습니다.
할당 연산자(assignment operator)(`=`)는
등가 연산자(`==`)를 의도했을 때
실수로 사용되지 않도록 값을 반환하지 않습니다.
산술 연산자(`+`, `-`, `*`, `/`, `%` 등)는
타입이 수용할 수 있는 값의 범위보다
크거나 작은 숫자로 작업을 수행하려할 때 예기치 않은 결과를 피하기 위해
값 오버플로우(overflow)를 감지하고 허용하지 않습니다.
Swift의 오버플로우 연산자를 사용하여
값 오버플로우 동작을 선택적으로 활성화할 수 있으며,
이에 대한 자세한 내용은 [오버플로우 연산자 (Overflow Operators)](./advanced-operators.md#오버플로우-연산자-overflow-operators)에서 확인할 수 있습니다.

Swift는 C에서는 없는
`a..<b` 및 `a...b`와 같은 값의 범위를 나타내는
범위 연산자를 제공합니다.

이 챕터에서는 Swift의 기본 연산자에 대해 설명합니다.
[고급 연산자 (Advanced Operators)](./advanced-operators.md)에서는 Swift의 고급 연산자에 대해 다루고
커스텀 연산자를 어떻게 정의하고
커스텀 타입을 위한 표준 연산자의 구현은 어떻게 하는지 설명합니다.

## 용어 (Terminology)

연산자는 단항(unary), 이항(binary), 삼항(ternary)으로 구분됩니다:

- *단항(Unary)* 연산자는 단일 항목에 동작합니다(예: `-a`).
  단항 *접두* 연산자는 항목 바로 직전에 위치하고(예: `!b`),
  단항 *접미* 연산자는 항목 바로 다음에 위치합니다(예: `c!`).
- *이항(Binary)* 연산자는 두 항목에 동작하고(예: `2 + 3`)
  두 항목 사이에 위치하므로 위치는 *중위* 연산자라고 합니다.
- *삼항(Ternary)* 연산자는 세 항목에 동작합니다.
  C처럼 Swift는 하나의 삼항 연산자만 있으며,
  삼항 조건 연산자입니다(`a ? b : c`).

연산자가 작용하는 값을 *피연산자*라고 합니다.
`1 + 2` 표현식에서 `+`기호는 중위 연산자(infix operator)이고
값 `1`과 `2`는 피연산자 입니다.

## 할당 연산자 (Assignment Operator)

*할당 연산자(assignment operator)*(`a = b`)는
`a`가 `b`로 초기화 되거나 업데이트 됩니다:

```swift
let b = 10
var a = 5
a = b
// a is now equal to 10
```

<!--
  - test: `assignmentOperator`

  ```swifttest
  -> let b = 10
  -> var a = 5
  -> a = b
  /> a is now equal to \(a)
  </ a is now equal to 10
  ```
-->

할당의 우항이 여러개의 값이 있는 튜플이라면,
튜플의 요소는 여러개의 상수나 변수로 한 번에 분해될 수 있습니다:

```swift
let (x, y) = (1, 2)
// x is equal to 1, and y is equal to 2
```

<!--
  - test: `assignmentOperator`

  ```swifttest
  -> let (x, y) = (1, 2)
  /> x is equal to \(x), and y is equal to \(y)
  </ x is equal to 1, and y is equal to 2
  ```
-->

<!--
  - test: `tuple-unwrapping-with-var`

  ```swifttest
  >> var (x, y) = (1, 2)
  ```
-->

<!--
  This still allows assignment to variables,
  even though var patterns have been removed,
  because it's parsed as a variable-declaration,
  using the first alternative where (x, y) is a pattern,
  but `var` comes from the variable-declaration-head
  rather than from the pattern.
-->

C와 Objective-C에서의 할당 연산자와 다르게
Swift의 할당 연산자는 값을 반환하지 않습니다.
아래 예문은 유효하지 않습니다:

```swift
if x = y {
    // This is not valid, because x = y does not return a value.
}
```

<!--
  - test: `assignmentOperatorInvalid`

  ```swifttest
  -> if x = y {
        // This isn't valid, because x = y doesn't return a value.
     }
  !$ error: cannot find 'x' in scope
  !! if x = y {
  !!    ^
  !$ error: cannot find 'y' in scope
  !! if x = y {
  !!        ^
  ```
-->

이 특징은 등가 연산자(`==`)가 실제로 사용되어야 할 때,
실수로 할당 연산자(`=`)가 사용되는 것을 방지합니다.
`if x = y`가 유효하지 않는 것처럼,
Swift는 코드에서 이런 종류의 오류를 피할 수 있도록 도와줍니다.

<!--
  TODO: Should we mention that x = y = z is also not valid?
  If so, is there a convincing argument as to why this is a good thing?
-->

## 산술 연산자 (Arithmetic Operators)

Swift는 모든 숫자 타입에 대해 4개의 기본 *산술 연산자(arithmetic operators)*를 제공합니다:

- 덧셈(`+`)
- 뺄셈(`-`)
- 곱셈(`*`)
- 나눗셈(`/`)

```swift
1 + 2       // equals 3
5 - 3       // equals 2
2 * 3       // equals 6
10.0 / 2.5  // equals 4.0
```

<!--
  - test: `arithmeticOperators`

  ```swifttest
  >> let r0 =
  -> 1 + 2       // equals 3
  >> assert(r0 == 3)
  >> let r1 =
  -> 5 - 3       // equals 2
  >> assert(r1 == 2)
  >> let r2 =
  -> 2 * 3       // equals 6
  >> assert(r2 == 6)
  >> let r3 =
  -> 10.0 / 2.5  // equals 4.0
  >> assert(r3 == 4.0)
  ```
-->

C와 Objective-C에서의 산술 연산자와 다르게
Swift 산술 연산자는 기본적으로 값의 오버플로우를 허용하지 않습니다.
Swift의 오버플로우 연산자(예: `a &+ b`)를 사용하여 값 오버플로우 동작을 선택적으로 활성화할 수 있습니다.
자세한 내용은 [오버플로우 연산자 (Overflow Operators)](./advanced-operators.md#오버플로우-연산자-overflow-operators)를 참고바랍니다.

덧셈 연산자는 `String` 연결도 지원합니다:

```swift
"hello, " + "world"  // equals "hello, world"
```

<!--
  - test: `arithmeticOperators`

  ```swifttest
  >> let r4 =
  -> "hello, " + "world"  // equals "hello, world"
  >> assert(r4 == "hello, world")
  ```
-->

### 나머지 연산자 (Remainder Operator)

*나머지 연산자(remainder operator)*(`a % b`)는
`a` 안에 들어갈 `b`의 배수가 몇 번 들어갈 수 있는지 계산하고,
남는 값을 반환합니다
(이것을 *나머지*라고 합니다).

> Note: 나머지 연산자(`%`)는 다른 언어에서
> *모듈로 연산자(modulo operator)*라고 합니다.
> 그러나 음수에 대한 Swift의 동작은
> 모듈로 연산이 아닌 나머지 연산 입니다.

<!--
  - test: `percentOperatorIsRemainderNotModulo`

  ```swifttest
  -> for i in -5...0 {
        print(i % 4)
     }
  << -1
  << 0
  << -3
  << -2
  << -1
  << 0
  ```
-->

여기 나머지 연산자가 어떻게 동작하는지 살펴봅시다.
`9 % 4`를 계산하기위해, `9` 안에 얼마나 많은 `4`가 들어가는지 알아야 합니다:

![](../.gitbook/assets/remainderInteger_2x~dark.png)

`9`에 `4`가 2번 들어가고 `1`이 남는 것을 알 수 있습니다(오렌지 색).

Swift에서 이것을 작성해보면:

```swift
9 % 4    // equals 1
```

<!--
  - test: `arithmeticOperators`

  ```swifttest
  >> let r5 =
  -> 9 % 4    // equals 1
  >> assert(r5 == 1)
  ```
-->

`a % b`의 결과를 구하기 위해
`%` 연산자는 아래의 방정식을 계산하고
출력으로 `remainder`를 반환합니다:

`a` = (`b` x `some multiplier`) + `remainder`

여기서 `some multiplier`는 `a`에 들어갈
`b`의 최대 배수 입니다.

이 방정식에 `9`와 `4`를 넣고 계산하면:

`9` = (`4` x `2`) + `1`

`a`에 음수를 이용하여 나머지를 계산할 때도 같은 메서드로 적용합니다:

```swift
-9 % 4   // equals -1
```

<!--
  - test: `arithmeticOperators`

  ```swifttest
  >> let r6 =
  -> -9 % 4   // equals -1
  >> assert(r6 == -1)
  ```
-->

방정식에 `-9`와 `4`를 넣고 계산하면:

`-9` = (`4` x `-2`) + `-1`

`-1`의 나머지를 얻습니다.

`b`의 음수 값에 대해서는 `b`의 부호가 무시됩니다.
이것은 `a % b`와 `a % -b`는 항상 같은 결과를 반환한다는 의미입니다.

### 단항 뺄셈 연산자 (Unary Minus Operator)

숫자 값의 부호는 `-` 접미사를 사용하여 변경할 수 있으며,
이것을 *단항 뺄셈 연산자(unary minus operator)*라고 합니다:

```swift
let three = 3
let minusThree = -three       // minusThree equals -3
let plusThree = -minusThree   // plusThree equals 3, or "minus minus three"
```

<!--
  - test: `arithmeticOperators`

  ```swifttest
  -> let three = 3
  -> let minusThree = -three       // minusThree equals -3
  -> let plusThree = -minusThree   // plusThree equals 3, or "minus minus three"
  ```
-->

단항 빼기 연산자(`-`)는
공백없이 작동하는 값 바로 앞에 추가합니다.

### 단항 덧셈 연산자 (Unary Plus Operator)

*단항 덧셈 연산자*(`+`)는 어떠한 변경없이
그 값을 그대로 반환합니다:

```swift
let minusSix = -6
let alsoMinusSix = +minusSix  // alsoMinusSix equals -6
```

<!--
  - test: `arithmeticOperators`

  ```swifttest
  -> let minusSix = -6
  -> let alsoMinusSix = +minusSix  // alsoMinusSix equals -6
  >> assert(alsoMinusSix == minusSix)
  ```
-->

단항 덧셈 연산자는 실제로 아무런 동작을 하지 않지만,
음수에 단항 뺄셈 연산자를 사용할 때
양수에 대칭을 위해 사용할 수 있습니다.

## 복합 할당 연산자 (Compound Assignment Operators)

C처럼, Swift는 할당(`=`)과 다른 연산자를 결합한 *복합 할당 연산자(compound assignment operators)*를 제공합니다.
아래 예시는 *덧셈 할당 연산자*(`+=`)입니다:

```swift
var a = 1
a += 2
// a is now equal to 3
```

<!--
  - test: `compoundAssignment`

  ```swifttest
  -> var a = 1
  -> a += 2
  /> a is now equal to \(a)
  </ a is now equal to 3
  ```
-->

표현식 `a += 2`는 `a = a + 2`의 짧은 표현입니다.
효과적으로 덧셈과 할당은 동시에 수행되고
하나의 연산자로 결합됩니다.

> Note: 복합 할당 연산자는 값을 반환하지 않습니다.
> 예를 들어 `let b = a += 2`로 작성할 수 없습니다.

Swift 표준 라이브러리에서 제공하는 연산자에 대한 내용은
[연산자 선언(Operator Declarations)](https://developer.apple.com/documentation/swift/operator_declarations)을 참고바랍니다.

## 비교 연산자 (Comparison Operators)

Swift는 아래의 비교 연산자(comparison operators)를 제공합니다:

- 같음(`a == b`)
- 다름(`a != b`)
- 보다 큼(`a > b`)
- 보다 작음(`a < b`)
- 보다 크거나 같음(`a >= b`)
- 보다 작거나 같음(`a <= b`)

> Note: Swift는 두 객체 참조가 동일한 객체 인스턴스를 참조하는지 판별하는
> *식별 연산자(identity operators)*(`===`와 `!==`)를 제공합니다.
> 자세한 내용은 [동일성 연산자 (Identity Operators)](./structures-and-classes.md#동일성-연산자-identity-operators)를 참고바랍니다.

각 비교 연산자는 구문이 참인지 아닌지 판단하기 위해 `Bool` 값을 반환합니다:

```swift
1 == 1   // true because 1 is equal to 1
2 != 1   // true because 2 is not equal to 1
2 > 1    // true because 2 is greater than 1
1 < 2    // true because 1 is less than 2
1 >= 1   // true because 1 is greater than or equal to 1
2 <= 1   // false because 2 is not less than or equal to 1
```

<!--
  - test: `comparisonOperators`

  ```swifttest
  >> assert(
  -> 1 == 1   // true because 1 is equal to 1
  >> )
  >> assert(
  -> 2 != 1   // true because 2 isn't equal to 1
  >> )
  >> assert(
  -> 2 > 1    // true because 2 is greater than 1
  >> )
  >> assert(
  -> 1 < 2    // true because 1 is less than 2
  >> )
  >> assert(
  -> 1 >= 1   // true because 1 is greater than or equal to 1
  >> )
  >> assert( !(
  -> 2 <= 1   // false because 2 isn't less than or equal to 1
  >> ) )
  ```
-->

비교 연산자는 `if` 구문과 같은
조건 구문에서 사용되기도 합니다:

```swift
let name = "world"
if name == "world" {
    print("hello, world")
} else {
    print("I'm sorry \(name), but I don't recognize you")
}
// Prints "hello, world", because name is indeed equal to "world".
```

<!--
  - test: `comparisonOperators`

  ```swifttest
  -> let name = "world"
  -> if name == "world" {
        print("hello, world")
     } else {
        print("I'm sorry \(name), but I don't recognize you")
     }
  << hello, world
  // Prints "hello, world", because name is indeed equal to "world".
  ```
-->

`if` 구문에 대한 자세한 내용은 [제어 흐름 (ControlFlow)](./control-flow.md)을 참고바랍니다.

같은 타입과 같은 갯수의 값을 가지고 있는 튜플은
비교할 수 있습니다.
튜플은 값이 다를 때까지
왼쪽에서 오른쪽으로
한 번에 하나씩
비교합니다.
두 값이 비교되고
해당 비교 결과에 따라
튜플 비교의 전체 결과가 결정됩니다.
모든 요소가 동일하면,
튜플은 같습니다.
예를 들어:

```swift
(1, "zebra") < (2, "apple")   // true because 1 is less than 2; "zebra" and "apple" are not compared
(3, "apple") < (3, "bird")    // true because 3 is equal to 3, and "apple" is less than "bird"
(4, "dog") == (4, "dog")      // true because 4 is equal to 4, and "dog" is equal to "dog"
```

<!--
  - test: `tuple-comparison-operators`

  ```swifttest
  >> let a =
  -> (1, "zebra") < (2, "apple")   // true because 1 is less than 2; "zebra" and "apple" aren't compared
  >> let b =
  -> (3, "apple") < (3, "bird")    // true because 3 is equal to 3, and "apple" is less than "bird"
  >> let c =
  -> (4, "dog") == (4, "dog")      // true because 4 is equal to 4, and "dog" is equal to "dog"
  >> print(a, b, c)
  << true true true
  ```
-->

위 예시의
첫번째 줄에서 왼쪽에서 오른쪽으로 비교하는 동작을 확인할 수 있습니다.
튜플의 다른 어떤 값과 상관없이
`1`이 `2`보다 작기 때문에,
`(1, "zebra")`는 `(2, "apple")`보다 작습니다.
튜플의 첫번째 요소에 의해 비교를 이미 완료했기 때문에
`"zebra"`가 `"apple"`보다 더 작은지 여부는 아무런 상관이 없습니다.
그러나
튜플의 첫번째 요소가 같을 때는
두번째 요소를 *비교합니다* ---
위 예시에서 두 번째와 세 번째 줄의 결과를 살펴보면 알 수 있습니다.

튜플은 해당 튜플의 각 값에 연산자를 적용할 수 있을 때에만
주어진 연산자로 비교할 수 있습니다. 예를 들어
아래의 코드에서
`String`과 `Int` 값은
`<` 연산자를 사용하여 비교가 가능하므로
타입 `(String, Int)`의 두 튜플은 비교할 수 있습니다. 반대로
`<` 연산자는 `Bool` 값에 적용할 수 없기 때문에
타입 `(String, Bool)`의 두 튜플은 `<` 연산자로
비교할 수 없습니다.

```swift
("blue", -1) < ("purple", 1)        // OK, evaluates to true
("blue", false) < ("purple", true)  // Error because < can't compare Boolean values
```

<!--
  - test: `tuple-comparison-operators-err`

  ```swifttest
  >> _ =
  -> ("blue", -1) < ("purple", 1)        // OK, evaluates to true
  >> _ =
  -> ("blue", false) < ("purple", true)  // Error because < can't compare Boolean values
  !$ error: type '(String, Bool)' cannot conform to 'Comparable'
  !! ("blue", false) < ("purple", true)  // Error because < can't compare Boolean values
  !!                 ^
  !$ note: only concrete types such as structs, enums and classes can conform to protocols
  !! ("blue", false) < ("purple", true)  // Error because < can't compare Boolean values
  !!                 ^
  !$ note: required by referencing operator function '<' on 'Comparable' where 'Self' = '(String, Bool)'
  !! ("blue", false) < ("purple", true)  // Error because < can't compare Boolean values
  !!                 ^
  ```
-->

<!--
  - test: `tuple-comparison-operators-ok`

  ```swifttest
  >> let x = ("blue", -1) < ("purple", 1)        // OK, evaluates to true
  >> print(x)
  << true
  ```
-->

> Note: Swift 표준 라이브러리는 7개 미만의 요소를 가지고 있는 튜플에 대해
> 튜플 비교 연산자를 제공합니다.
> 7개 이상의 요소의 튜플을 비교하려면
> 비교 연산자를 직접 구현해야 합니다.

<!--
  TODO: which types do these operate on by default?
  How do they work with strings?
  How about with your own types?
-->

## 삼항 조건 연산자 (Ternary Conditional Operator)

*삼항 조건 연산자(ternary conditional operator)*는 `question ? answer1 : answer2`형태의
세 부분으로 이루어진 특별한 연산자입니다.
`question`이 참이나 거짓인지에 따라
두 표현식 중 하나를 나타내는 식입니다.
`question`이 참이면 `answer1`을 반환하고;
반대면 `answer2`를 반환합니다.

삼항 조건 연산자는 아래의 코드를 줄여서 표현한 것입니다:

```swift
if question {
    answer1
} else {
    answer2
}
```

<!--
  - test: `ternaryConditionalOperatorOutline`

  ```swifttest
  >> let question = true
  >> let answer1 = true
  >> let answer2 = true
  -> if question {
        answer1
     } else {
        answer2
     }
  !! /tmp/swifttest.swift:5:4: warning: expression of type 'Bool' is unused
  !! answer1
  !! ^~~~~~~
  !! /tmp/swifttest.swift:7:4: warning: expression of type 'Bool' is unused
  !! answer2
  !! ^~~~~~~
  ```
-->

<!--
  FIXME This example has too much hand waving.
  Swift doesn't have 'if' expressions.
-->

테이블 열의 높이를 계산하는 예시가 있습니다.
테이블 열은 헤더를 가지고 있다면 콘텐츠 높이보다 50 포인트 더 커야 하고
헤더가 없다면 20 포인트 더 커야 합니다:

```swift
let contentHeight = 40
let hasHeader = true
let rowHeight = contentHeight + (hasHeader ? 50 : 20)
// rowHeight is equal to 90
```

<!--
  - test: `ternaryConditionalOperatorPart1`

  ```swifttest
  -> let contentHeight = 40
  -> let hasHeader = true
  -> let rowHeight = contentHeight + (hasHeader ? 50 : 20)
  /> rowHeight is equal to \(rowHeight)
  </ rowHeight is equal to 90
  ```
-->

위의 예시는 아래의 코드를 짧게 표현한 것입니다:

```swift
let contentHeight = 40
let hasHeader = true
let rowHeight: Int
if hasHeader {
    rowHeight = contentHeight + 50
} else {
    rowHeight = contentHeight + 20
}
// rowHeight is equal to 90
```

<!--
  - test: `ternaryConditionalOperatorPart2`

  ```swifttest
  -> let contentHeight = 40
  -> let hasHeader = true
  -> let rowHeight: Int
  -> if hasHeader {
        rowHeight = contentHeight + 50
     } else {
        rowHeight = contentHeight + 20
     }
  /> rowHeight is equal to \(rowHeight)
  </ rowHeight is equal to 90
  ```
-->

첫번째 예에서 삼항 조건 연산자를 사용하면
`rowHeight`를 한줄의 코드로 올바른 값을 설정할 수 있으며,
두번째 예에서 사용된 코드보다 간결합니다.

삼항 조건 연산자는
두 표현식 중 어느 것을 선택할지 결정하는 효율적인 축약형을 제공합니다.
그러나 삼항 조건 연산자 사용시에는 주의가 필요합니다.
너무 남용하면 읽기 어려운 코드가 될 수 있습니다.
여러 삼항 조건 연산자를 하나의 복합 구문으로 결합하는 것을 피해야 합니다.

## Nil-병합 연산자 (Nil-Coalescing Operator)

*nil-병합 연산자(nil-coalescing operator)*(`a ?? b`)는
옵셔널 `a`에 값이 있으면 `a`를 풀거나,
`a`가 `nil`이면 기본값 `b`를 반환합니다.
표현식 `a`는 항상 옵셔널 타입 입니다.
표현식 `b`는 `a`에 저장된 타입과 같아야 합니다.

nil-병합 연산자는 아래 코드를 짧게 표현할 수 있습니다:

```swift
a != nil ? a! : b
```

<!--
  - test: `nilCoalescingOperatorOutline`

  ```swifttest
  >> var a: Int?
  >> let b = 42
  >> let c =
  -> a != nil ? a! : b
  >> print(c)
  << 42
  ```
-->

위 코드는 삼항 조건 연산자를 사용하고
`a`가 `nil`이 아닐경우 `a` 값을 접근하기 위해
강제 언래핑(`a!`)하며 `a`가 `nil` 일 경우 `b`를 반환합니다.
nil-병합 연산자는 조건 검사 및 언래핑을
간결하고 읽기 쉬운 형태로 캡슐화 합니다.

> Note: `a`의 값이 `nil`이 아닐경우
> `b`는 절대 반환되지 않습니다.
> 이러한 경우를 *단락 평가(short-circuit evaluation)*라 합니다.

아래 예시는 기본 컬러 이름과 사용자가 정의한 옵셔널 컬러 이름 중 선택하기 위해
nil-병합 연산자를 사용합니다:

```swift
let defaultColorName = "red"
var userDefinedColorName: String?   // defaults to nil

var colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName is nil, so colorNameToUse is set to the default of "red"
```

<!--
  - test: `nilCoalescingOperator`

  ```swifttest
  -> let defaultColorName = "red"
  -> var userDefinedColorName: String?   // defaults to nil

  -> var colorNameToUse = userDefinedColorName ?? defaultColorName
  /> userDefinedColorName is nil, so colorNameToUse is set to the default of \"\(colorNameToUse)\"
  </ userDefinedColorName is nil, so colorNameToUse is set to the default of "red"
  ```
-->

`userDefinedColorName` 변수는 기본값이 `nil` 인
옵셔널 `String`으로 정의됩니다.
`userDefinedColorName`은 옵셔널 타입이기 때문에
값을 선택하기 위해 nil-병합 연산자를 사용할 수 있습니다.
위의 예시에서 연산자는
`colorNameToUse`라는 `String` 변수의 초기값을 결정하는데 사용됩니다.
`userDefinedColorName`이 `nil`이기 때문에,
표현식 `userDefinedColorName ?? defaultColorName`은
`defaultColorName`나 `"red"`의 값을 반환합니다.

`userDefinedColorName`에 `nil`이 아닌 값을 대입하고
nil-병합 연산자로 검사를 다시 수행하면
기본값 대신에 `userDefinedColorName`에 래핑된 값을 사용합니다:

```swift
userDefinedColorName = "green"
colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName is not nil, so colorNameToUse is set to "green"
```

<!--
  - test: `nilCoalescingOperator`

  ```swifttest
  -> userDefinedColorName = "green"
  -> colorNameToUse = userDefinedColorName ?? defaultColorName
  /> userDefinedColorName isn't nil, so colorNameToUse is set to \"\(colorNameToUse)\"
  </ userDefinedColorName isn't nil, so colorNameToUse is set to "green"
  ```
-->

## 범위 연산자 (Range Operators)

Swift는 값의 범위를 짧게 표현하는
*범위 연산자(range operators)*가 있습니다.

### 닫힌 범위 연산자 (Closed Range Operator)

*닫힌 범위 연산자(closed range operator)*(`a...b`)는
값 `a`와 `b`가 포함된
`a`부터 `b`까지의 범위를 정의합니다.
`a`의 값은 `b`보다 클 수 없습니다.

<!--
  - test: `closedRangeStartCanBeLessThanEnd`

  ```swifttest
  -> let range = 1...2
  >> print(type(of: range))
  << ClosedRange<Int>
  ```
-->

<!--
  - test: `closedRangeStartCanBeTheSameAsEnd`

  ```swifttest
  -> let range = 1...1
  ```
-->

<!--
  - test: `closedRangeStartCannotBeGreaterThanEnd`

  ```swifttest
  -> let range = 1...0
  xx assertion
  ```
-->

닫힌 범위 연산자는
`for`-`in` 루프와 같이
모든 값의 범위를 반복할 때 유용합니다:

```swift
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```

<!--
  - test: `rangeOperators`

  ```swifttest
  -> for index in 1...5 {
        print("\(index) times 5 is \(index * 5)")
     }
  </ 1 times 5 is 5
  </ 2 times 5 is 10
  </ 3 times 5 is 15
  </ 4 times 5 is 20
  </ 5 times 5 is 25
  ```
-->

`for`-`in` 루프에 대한 자세한 설명은 [제어 흐름 (ControlFlow)](./control-flow.md)을 참고바랍니다.

### 반-열림 범위 연산자 (Half-Open Range Operator)

*반-열림 범위 연산자(half-open range operator)*(`a..<b`)는
`b`가 포함되지 않은
`a`부터 `b`까지의 범위 실행을 정의합니다.
이것은 마지막 값은 포함되지 않지만 처음 값은 포함되므로
*반-열림(half-open)*이라 합니다.
닫힌 범위 연산자와 같이
`a`의 값은 `b`보다 클 수 없습니다.
`a`의 값이 `b`와 같다면
결과 범위는 비어있을 것입니다.

<!--
  - test: `halfOpenRangeStartCanBeLessThanEnd`

  ```swifttest
  -> let range = 1..<2
  >> print(type(of: range))
  << Range<Int>
  ```
-->

<!--
  - test: `halfOpenRangeStartCanBeTheSameAsEnd`

  ```swifttest
  -> let range = 1..<1
  ```
-->

<!--
  - test: `halfOpenRangeStartCannotBeGreaterThanEnd`

  ```swifttest
  -> let range = 1..<0
  xx assertion
  ```
-->

반-열림 범위는 리스트의 길이까지 순차적으로 작업할 수 있는
배열과 같은 0부터 시작하는 리스트를
다룰 때 유용합니다:

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
    print("Person \(i + 1) is called \(names[i])")
}
// Person 1 is called Anna
// Person 2 is called Alex
// Person 3 is called Brian
// Person 4 is called Jack
```

<!--
  - test: `rangeOperators`

  ```swifttest
  -> let names = ["Anna", "Alex", "Brian", "Jack"]
  -> let count = names.count
  >> assert(count == 4)
  -> for i in 0..<count {
        print("Person \(i + 1) is called \(names[i])")
     }
  </ Person 1 is called Anna
  </ Person 2 is called Alex
  </ Person 3 is called Brian
  </ Person 4 is called Jack
  ```
-->

배열은 4개의 아이템을 가지고 있지만
`0..<count`는 반-열림 범위이기 때문에 오직 `3`
(배열의 마지막 아이템의 인덱스)까지
카운트 합니다.
배열에 대한 자세한 설명은 [배열 (Arrays)](./collection-types.md#배열-arrays)을 참고바랍니다.

### 단-방향 범위 (One-Sided Ranges)

닫힌 범위 연산자는
한 방향으로 계속되는 범위에 대한
대체 형식입니다 ---
인덱스 2에서
배열 끝까지 배열의 모든 요소를 포함하는 범위가
그 예입니다.
이러한 경우 범위 연산자의
한 쪽을 생략할 수 있습니다.
이러한 범위의 종류는 연산자가 오직 한쪽의 값만 가지고 있으므로
*단-방향 범위(one-sided range)*라고 합니다.
예를 들어:

```swift
for name in names[2...] {
    print(name)
}
// Brian
// Jack

for name in names[...2] {
    print(name)
}
// Anna
// Alex
// Brian
```

<!--
  - test: `rangeOperators`

  ```swifttest
  -> for name in names[2...] {
         print(name)
     }
  </ Brian
  </ Jack

  -> for name in names[...2] {
         print(name)
     }
  </ Anna
  </ Alex
  </ Brian
  ```
-->

반-열림 연산자는
마지막 값만 기입하여
단-방향 형식을 가질 수 있습니다.
보통 형식과 같이 마지막 값은
범위에 포함되지 않습니다.
예를 들어:

```swift
for name in names[..<2] {
    print(name)
}
// Anna
// Alex
```

<!--
  - test: `rangeOperators`

  ```swifttest
  -> for name in names[..<2] {
         print(name)
     }
  </ Anna
  </ Alex
  ```
-->

단-방향 범위는 서브스크립트 뿐만 아니라
다른 상황에서도 사용할 수 있습니다.
첫 번째 값을 생략한 단-방향 범위는
어디서부터 반복을 시작해야 할지 명확하지 않기 때문에
반복할 수 없습니다.
반복문에서 단-방향 범위의 마지막 값은 생략할 수 있습니다;
그러나 범위가 무한정 계속되므로
루프에 대해 명시적으로 종료 조건을 추가해야 합니다.
아래 코드와 같이
단-방향 범위에 특정 값이 포함되어 있는지 체크할 수 있습니다.

```swift
let range = ...5
range.contains(7)   // false
range.contains(4)   // true
range.contains(-1)  // true
```

<!--
  - test: `rangeOperators`

  ```swifttest
  -> let range = ...5
  >> print(type(of: range))
  << PartialRangeThrough<Int>
  >> let a =
  -> range.contains(7)   // false
  >> let b =
  -> range.contains(4)   // true
  >> let c =
  -> range.contains(-1)  // true
  >> print(a, b, c)
  << false true true
  ```
-->

## 논리 연산자 (Logical Operators)

*논리 연산자(Logical operator)*는 Boolean 로직 값을
`true`와 `false`로 수정하거나 결합합니다.
Swift는 C-기반 언어에서 볼 수 있는 세 가지 표준 논리 연산자를 제공합니다:

- 논리적 NOT(`!a`)
- 논리적 AND(`a && b`)
- 논리적 OR(`a || b`)

### 논리적 NOT 연산자 (Logical NOT Operator)

*논리적 NOT 연산자(logical NOT operator)*(`!a`)는 Boolean 값에 대해
`true`를 `false`로 `false`를 `true`와 같이 반대로 만듭니다.

논리적 NOT 연산자는 접두사 연산자이며,
동작할 값 바로 앞에
공백없이 위치합니다.
이것은 아래 예시와 같이 "`a`가 아니다"라고 읽을 수 있습니다:

```swift
let allowedEntry = false
if !allowedEntry {
    print("ACCESS DENIED")
}
// Prints "ACCESS DENIED"
```

<!--
  - test: `logicalOperators`

  ```swifttest
  -> let allowedEntry = false
  -> if !allowedEntry {
        print("ACCESS DENIED")
     }
  <- ACCESS DENIED
  ```
-->

`if !allowedEntry` 구문은 "엔트리가 허락되지 않는다면"이라고 읽을 수 있습니다.
다음 라인은 "엔트리가 허락되지 않음"이 참일 때;
이 말은 `allowedEntry`가 `false` 일 경우에만 실행된다는 의미입니다.

이 예시와 같이
Boolean 상수와 변수 이름을 신중하게 선택하면
중복 부정이나 논리적 상태의 혼동을 피하기 위해
읽기 쉽고 간결함을 유지할 수 있습니다.

### 논리적 AND 연산자 (Logical AND Operator)

*논리적 AND 연산자(logical AND operator)*(`a && b`)는
두 값이 모두 `true`여야 `true`를 표현하는 논리적 표현식을 만듭니다.

두 값중 하나라도 `false`이면,
결과는 `false`입니다.
사실 *첫 번째* 값이 `false`이면,
`true`의 결과를 얻을 수 없기 때문에
두번째 값은 살펴보지 않습니다.
이러한 경우를 *단락 평가(short-circuit evaluation)*라 합니다.

이 예시는 두 `Bool` 값이
`true` 일 경우에만 접근을 허용합니다:

```swift
let enteredDoorCode = true
let passedRetinaScan = false
if enteredDoorCode && passedRetinaScan {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "ACCESS DENIED"
```

<!--
  - test: `logicalOperators`

  ```swifttest
  -> let enteredDoorCode = true
  -> let passedRetinaScan = false
  -> if enteredDoorCode && passedRetinaScan {
        print("Welcome!")
     } else {
        print("ACCESS DENIED")
     }
  <- ACCESS DENIED
  ```
-->

### 논리적 OR 연산자 (Logical OR Operator)

*논리적 OR 연산자(logical OR operator)*(`a || b`)는
두 인접한 파이프 문자(|)로 만들어진 중위 연산자(infix operator)입니다.
이것을 사용하여
두 값 중 *하나*라도 `true`이면
표현식이 `true`가 되는 논리적 표현식을 만듭니다.

위에서 논리적 AND 연산자와 마찬가지로
논리적 OR 연산자는 단락 평가(short-circuit evaluation)를 사용하여 표현식을 고려합니다.
논리적 OR 연산자의 좌변이 `true`이면,
표현식의 결과는 바뀌지 않으므로
우변은 고려하지 않습니다.

아래 예시에서
첫번째 `Bool` 값(`hasDoorKey`)은 `false`이지만,
두번째 값(`knowsOverridePassword`)은 `true`입니다.
하나의 값이 `true`이기 때문에,
표현식은 `true`가 되고
접근이 허용됩니다:

```swift
let hasDoorKey = false
let knowsOverridePassword = true
if hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "Welcome!"
```

<!--
  - test: `logicalOperators`

  ```swifttest
  -> let hasDoorKey = false
  -> let knowsOverridePassword = true
  -> if hasDoorKey || knowsOverridePassword {
        print("Welcome!")
     } else {
        print("ACCESS DENIED")
     }
  <- Welcome!
  ```
-->

### 논리적 연산자 결합 (Combining Logical Operators)

긴 복합 표현식을 생성하기 위해 여러 개의 논리적 연산자를 결합할 수 있습니다:

```swift
if enteredDoorCode && passedRetinaScan || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "Welcome!"
```

<!--
  - test: `logicalOperators`

  ```swifttest
  -> if enteredDoorCode && passedRetinaScan || hasDoorKey || knowsOverridePassword {
        print("Welcome!")
     } else {
        print("ACCESS DENIED")
     }
  <- Welcome!
  ```
-->

이 예시는 긴 복합 표현식을 생성하기 위해 여러 개의 `&&`와 `||` 연산자를 사용하였습니다.
그러나 `&&`와 `||` 연산자는 오직 두 값에서만 동작하므로,
이것은 세 개의 작은 표현식이 함께 엮어있는 형태입니다.
이 예시는 아래와 같이 읽을 수 있습니다:

올바른 출입문 코드를 입력하고 망막 스캔을 통과했거나
유효한 키를 가지고 있거나
긴급 비밀번호를 알고 있을 경우에
접근이 허용됩니다.

`enteredDoorCode`, `passedRetinaScan`, `hasDoorKey`의 값을 기반으로
첫 번째 두 표현식은 `false`입니다.
그러나 긴급 비밀번호는 알고 있으므로
이 표현식의 결과는 `true`입니다.

> Note: Swift 논리적 연산자 `&&`와 `||`은 왼쪽 우선 결합(left-associative)입니다
> 그 의미는 여러 개의 논리적 연산자로 이루어진 복합 표현식은
> 가장 왼쪽부터 판단한다는 뜻입니다.

### 명시적 소괄호 (Explicit Parentheses)

소괄호를 포함하는 것이 영향이 없을 때 복합 표현식을 읽기 쉽게 하기위해
소괄호를 포함하는 것은 때때로 유용합니다.
위의 출입문 접근 예시에서
의도를 명확하게 하기위해
복합 표현식의 첫 번째 부분을 소괄호를 추가하면 유용합니다:

```swift
if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "Welcome!"
```

<!--
  - test: `logicalOperators`

  ```swifttest
  -> if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
        print("Welcome!")
     } else {
        print("ACCESS DENIED")
     }
  <- Welcome!
  ```
-->

소괄호는 처음 두 값을
전체 논리에서 별도의 부분으로 고려되도록 해줍니다.
복합 표현식의 결과는 바뀌지 않지만
전체적인 의도는 명확해 집니다.
가독성은 항상 간결성보다 선호됩니다;
의도를 명확하게 하는데 도움이 되는 곳에 소괄호를 사용해야 합니다.

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->