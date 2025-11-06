{% hint style="info" %}
📢 **공지**  
이 문서는 더 이상 업데이트되지 않습니다.  
아래 새로운 URL로 업데이트 되었으니, 새로운 URL로 접속해 주세요.  
[Swift Book Korean](https://bbiguduk.github.io/swift-book-korean/documentation/tsplk/)
{% endhint %}

# 패턴 (Patterns)

값을 일치시키고 분해합니다.

*패턴(pattern)*은 단일 값이나
복합값의 구조를 나타냅니다.
예를 들어 튜플 `(1, 2)`의 구조는 콤마로 구분된 두 요소의 목록입니다.
패턴은 특정값이 아닌 값의 구조를 나타내기 때문에
다양한 값과 일치시킬 수 있습니다.
예를 들어 패턴 `(x, y)`는 튜플 `(1, 2)`와 서로 다른 두 요소를 가진 튜플과 일치합니다.
패턴을 값과 일치시키는 것 외에도
복합값의 일부나 전체를 추출하고
각 부분을 상수나 변수 이름으로 바인딩할 수 있습니다.

Swift에는 두 가지 기본 패턴이 있습니다:
하나는 모든 종류의 값과 일치하는 패턴이고
다른 하나는 런타임에 지정된 값과 일치하는데 실패할 수 있는 패턴입니다.

패턴의 첫 번째 종류는 단순 변수, 상수, 옵셔널 바인딩에서
값을 분해하는데 사용됩니다.
여기에는 와일드카드 패턴, 식별자 패턴,
이를 포함하는 모든 값 바인딩이나 튜플 패턴이 포함됩니다.
이러한 패턴에 타입 주석을 지정하여
특정 타입의 값만 일치하도록 제한할 수 있습니다.

패턴의 두 번째 종류는 전체 패턴 일치에 사용되며
일치시키려는 값이 런타임에 없을 수 있습니다.
여기에는 열거형 케이스 패턴, 옵셔널 패턴, 표현식 패턴,
타입-캐스팅 패턴을 포함합니다. 이러한 패턴은 `switch` 구문의 케이스 레이블,
`do` 구문의 `catch` 절,
`if`, `while`, `guard`, `for`-`in` 구문의
케이스 조건에서 사용합니다.

> Grammar of a pattern:
>
> *pattern* → *wildcard-pattern* *type-annotation*_?_ \
> *pattern* → *identifier-pattern* *type-annotation*_?_ \
> *pattern* → *value-binding-pattern* \
> *pattern* → *tuple-pattern* *type-annotation*_?_ \
> *pattern* → *enum-case-pattern* \
> *pattern* → *optional-pattern* \
> *pattern* → *type-casting-pattern* \
> *pattern* → *expression-pattern*

## 와일드카드 패턴 (Wildcard Pattern)

*와일드카드 패턴(wildcard pattern)*은 어떤 값이든 일치시키고 무시하며 언더바(`_`)로 구성됩니다.
일치하는 값에 대해 신경쓰지 않을 경우에 와일드카드 패턴을 사용합니다.
예를 들어 다음의 코드는 닫힌 범위 `1...3`을 반복하고,
각 반복마다 범위의 현재값을 무시합니다:

```swift
for _ in 1...3 {
    // Do something three times.
}
```

<!--
  - test: `wildcard-pattern`

  ```swifttest
  -> for _ in 1...3 {
        // Do something three times.
     }
  ```
-->

> Grammar of a wildcard pattern:
>
> *wildcard-pattern* → **`_`**

## 식별자 패턴 (Identifier Pattern)

*식별자 패턴(identifier pattern)*은 모든 값과 일치하고 일치하는 값을
변수나 상수 이름으로 바인딩합니다.
예를 들어 다음의 상수 선언에서 `someValue`는
`Int` 타입의 `42` 값이 일치하는 식별자 패턴입니다:

```swift
let someValue = 42
```

<!--
  - test: `identifier-pattern`

  ```swifttest
  -> let someValue = 42
  ```
-->

일치가 성공하면 값 `42`는 상수 이름 `someValue`에
바인딩(할당)됩니다.

변수나 상수 선언의 왼쪽 패턴이
식별자 패턴이면
식별자 패턴은 암시적으로 값-바인딩 패턴(value-binding pattern)의 하위 패턴입니다.

> Grammar of an identifier pattern:
>
> *identifier-pattern* → *identifier*

## 값-바인딩 패턴 (Value-Binding Pattern)

*값-바인딩 패턴(value-binding pattern)*은 일치한 값을 변수나 상수 이름에 바인딩합니다.
상수의 이름에 일치되는 값을 바인딩하는 값-바인딩 패턴은
`let` 키워드로 시작합니다; 변수의 이름에 바인딩하면
`var` 키워드로 시작합니다.

값-바인딩 패턴 내의 식별자 패턴은
일치하는 값을 새로운 명명된 변수나 상수로 바인딩합니다.
예를 들어 튜플의 요소를 분해하고 각 요소의 값을
해당 식별자 패턴에 바인딩할 수 있습니다.

```swift
let point = (3, 2)
switch point {
    // Bind x and y to the elements of point.
case let (x, y):
    print("The point is at (\(x), \(y)).")
}
// Prints "The point is at (3, 2)."
```

<!--
  - test: `value-binding-pattern`

  ```swifttest
  -> let point = (3, 2)
  -> switch point {
        // Bind x and y to the elements of point.
        case let (x, y):
           print("The point is at (\(x), \(y)).")
     }
  <- The point is at (3, 2).
  ```
-->

위의 예시에서 `let`은 튜플 패턴 `(x, y)`에서 각 식별자 패턴에 적용합니다.
이 동작으로 인해 `switch` 케이스 `case let (x, y):`와 `case (let x, let y):`은
동일한 값에 일치됩니다.

> Grammar of a value-binding pattern:
>
> *value-binding-pattern* → **`var`** *pattern* | **`let`** *pattern*

<!--
  NOTE: We chose to call this "value-binding pattern"
  instead of "variable pattern",
  because it's a pattern that binds values to either variables or constants,
  not a pattern that varies.
  "Variable pattern" is ambiguous between those two meanings.
-->

## 튜플 패턴 (Tuple Pattern)

*튜플 패턴(tuple pattern)*은 소괄호로 묶인 0개 이상의 패턴의 콤마로 구분된 목록입니다.
튜플 패턴은 해당 튜플 타입의 값에 일치됩니다.

타입 주석을 사용하여
튜플 패턴이 특정 종류의 튜플 타입에만 일치하도록 제한할 수 있습니다.
예를 들어 상수 선언 `let (x, y): (Int, Int) = (1, 2)`에서 튜플 패턴 `(x, y): (Int, Int)`은
두 요소 모두 `Int` 타입의
튜플 타입만 일치됩니다.

튜플 패턴이 `for`-`in` 구문이나
변수나 상수 선언에서 패턴으로 사용되면, 와일드카드 패턴,
식별자 패턴, 옵셔널 패턴, 이를 포함하는 다른 튜플 패턴만 포함할 수 있습니다.
예를 들어
다음 코드는 튜플 패턴 `(x, 0)`에서 요소 `0`은 표현식 패턴이므로
유효하지 않습니다:

```swift
let points = [(0, 0), (1, 0), (1, 1), (2, 0), (2, 1)]
// This code isn't valid.
for (x, 0) in points {
    /* ... */
}
```

<!--
  - test: `tuple-pattern`

  ```swifttest
  -> let points = [(0, 0), (1, 0), (1, 1), (2, 0), (2, 1)]
  -> // This code isn't valid.
  -> for (x, 0) in points {
  >>    _ = x
        /* ... */
     }
  !$ error: expected pattern
  !! for (x, 0) in points {
  !!         ^
  ```
-->

단일 요소를 포함하는 튜플 패턴을 둘러싼 소괄호는 아무런 효과가 없습니다.
이 패턴은 단일 요소의 타입 값에 일치됩니다.
예를 들어 다음은 동일합니다:

<!--
  This test needs to be compiled.
  The error message in the REPL is unpredictable as of
  Swift version 1.1 (swift-600.0.54.20)
-->

```swift
let a = 2        // a: Int = 2
let (a) = 2      // a: Int = 2
let (a): Int = 2 // a: Int = 2
```

<!--
  - test: `single-element-tuple-pattern`

  ```swifttest
  -> let a = 2        // a: Int = 2
  -> let (a) = 2      // a: Int = 2
  -> let (a): Int = 2 // a: Int = 2
  !$ error: invalid redeclaration of 'a'
  !! let (a) = 2      // a: Int = 2
  !! ^
  !$ note: 'a' previously declared here
  !! let a = 2        // a: Int = 2
  !! ^
  !$ error: invalid redeclaration of 'a'
  !! let (a): Int = 2 // a: Int = 2
  !! ^
  !$ note: 'a' previously declared here
  !! let a = 2        // a: Int = 2
  !! ^
  ```
-->

> Grammar of a tuple pattern:
>
> *tuple-pattern* → **`(`** *tuple-pattern-element-list*_?_ **`)`** \
> *tuple-pattern-element-list* → *tuple-pattern-element* | *tuple-pattern-element* **`,`** *tuple-pattern-element-list* \
> *tuple-pattern-element* → *pattern* | *identifier* **`:`** *pattern*

## 열거형 케이스 패턴 (Enumeration Case Pattern)

*열거형 케이스 패턴(enumeration case pattern)*은 기존 열거형 타입의 케이스와 일치합니다.
열거형 케이스 패턴은 `switch` 구문 케이스 레이블과
`if`, `while`, `guard`, `for`-`in` 구문의 케이스 조건에
나타납니다.

일치시키려는 열거형 케이스에 연관값이 있는 경우,
해당 열거형 케이스 패턴은 각 연관값에 대한 요소를 포함하는 튜플 패턴을 지정해야 합니다.
연관값을 포함하는 열거형 케이스를 일치시키기 위해
`switch` 구문을 사용하는 예시는
[연관값 (Associated Values)](./enumerations.md#연관값-associated-values)을 참고바랍니다.

열거형 케이스 패턴은
옵셔널로 래핑된 케이스의 값과도 일치합니다.
이 간략한 구문으로 옵셔널 패턴을 생략할 수 있습니다.
`Optional`은 열거형으로 구현되므로
`.none`과 `.some`은
열거형 타입의 케이스와 동일한 스위치에
나타날 수 있습니다.

```swift
enum SomeEnum { case left, right }
let x: SomeEnum? = .left
switch x {
case .left:
    print("Turn left")
case .right:
    print("Turn right")
case nil:
    print("Keep going straight")
}
// Prints "Turn left"
```

<!--
  - test: `enum-pattern-matching-optional`

  ```swifttest
  -> enum SomeEnum { case left, right }
  -> let x: SomeEnum? = .left
  -> switch x {
     case .left:
         print("Turn left")
     case .right:
         print("Turn right")
     case nil:
         print("Keep going straight")
     }
  <- Turn left
  ```
-->

> Grammar of an enumeration case pattern:
>
> *enum-case-pattern* → *type-identifier*_?_ **`.`** *enum-case-name* *tuple-pattern*_?_

## 옵셔널 패턴 (Optional Pattern)

*옵셔널 패턴(optional pattern)*은 `Optional<Wrapped>` 열거형의
`some(Wrapped)` 케이스에 래핑된 값과 일치합니다.
옵셔널 패턴은 식별자 패턴 바로 뒤에 물음표가 오는 것으로 구성되며
열거형 케이스 패턴과 동일한 위치에 나타납니다.

옵셔널 패턴은 `Optional` 열거형 케이스 패턴에
대한 편의 문법이므로
다음은 동일합니다:

```swift
let someOptional: Int? = 42
// Match using an enumeration case pattern.
if case .some(let x) = someOptional {
    print(x)
}

// Match using an optional pattern.
if case let x? = someOptional {
    print(x)
}
```

<!--
  - test: `optional-pattern`

  ```swifttest
  -> let someOptional: Int? = 42
  -> // Match using an enumeration case pattern.
  -> if case .some(let x) = someOptional {
        print(x)
     }
  << 42

  -> // Match using an optional pattern.
  -> if case let x? = someOptional {
        print(x)
     }
  << 42
  ```
-->

옵셔널 패턴은 `for`-`in` 구문에서 옵셔널 값의 배열을 반복하는
편리한 방법을 제공하여,
`nil`이 아닌 요소에 대해서만 루프의 본문을 실행합니다.

```swift
let arrayOfOptionalInts: [Int?] = [nil, 2, 3, nil, 5]
// Match only non-nil values.
for case let number? in arrayOfOptionalInts {
    print("Found a \(number)")
}
// Found a 2
// Found a 3
// Found a 5
```

<!--
  - test: `optional-pattern-for-in`

  ```swifttest
  -> let arrayOfOptionalInts: [Int?] = [nil, 2, 3, nil, 5]
  -> // Match only non-nil values.
  -> for case let number? in arrayOfOptionalInts {
        print("Found a \(number)")
     }
  </ Found a 2
  </ Found a 3
  </ Found a 5
  ```
-->

> Grammar of an optional pattern:
>
> *optional-pattern* → *identifier-pattern* **`?`**

## 타입-캐스팅 패턴 (Type-Casting Patterns)

타입-캐스팅 패턴(type-casting pattern)은 두 가지가 있으며, `is` 패턴과 `as` 패턴이 있습니다.
`is` 패턴은 `switch` 구문 케이스 레이블에서만 나타납니다.
`is`와 `as` 패턴은 다음의 형식을 가집니다:

```swift
is <#type#>
<#pattern#> as <#type#>
```

`is` 패턴은 런타임에 해당 값의 타입이
`is` 패턴의 오른편에 지정한 타입과 일치하거나 해당 타입의 하위 클래스가 일치하면 값과 일치합니다.
`is` 패턴은 타입 캐스팅을 동작하지만
반환된 타입을 버린다는 것에서 `is` 연산자와 유사하게 동작합니다.

`as` 패턴은 런타임에 해당 값의 타입이
`as` 패턴의 오른편에 지정한 타입과 일치하거나 해당 타입의 하위 클래스가 일치하면 값과 일치합니다.
일치가 성공하면,
일치된 값의 타입은 `as` 패턴의 오른편에 지정한 *패턴(pattern)*으로
캐스팅됩니다.

`is`와 `as` 패턴으로 값을 일치시키기 위해
`switch` 구문을 사용하는 예시는
[Any와 AnyObject에 대한 타입 캐스팅 (Type Casting for Any and AnyObject)](../language-guide-1/type-casting.md#any와-anyobject에-대한-타입-캐스팅-type-casting-for-any-and-anyobject)을 참고바랍니다.

> Grammar of a type casting pattern:
>
> *type-casting-pattern* → *is-pattern* | *as-pattern* \
> *is-pattern* → **`is`** *type* \
> *as-pattern* → *pattern* **`as`** *type*

## 표현식 패턴 (Expression Pattern)

*표현식 패턴(expression pattern)*은 표현식의 값을 나타냅니다.
표현식 패턴은 `switch` 구문 케이스 레이블에서만
나타납니다.

표현식 패턴으로 표현되는 표현식은
Swift 표준 라이브러리의 패턴 일치 연산자(`~=`)를 사용하여
입력 표현식의 값과 비교합니다.
일치가 성공하면 `~=` 연산자는 `true`를 반환합니다.
기본적으로 `~=` 연산자는
`==` 연산자를 사용하여 동일한 타입의 두 값을 비교합니다.
다음 예시에서 보듯이
범위 내에 값이 포함되는지 검사하기 위해
값의 범위로 값을 일치시킬 수도 있습니다.

```swift
let point = (1, 2)
switch point {
case (0, 0):
    print("(0, 0) is at the origin.")
case (-2...2, -2...2):
    print("(\(point.0), \(point.1)) is near the origin.")
default:
    print("The point is at (\(point.0), \(point.1)).")
}
// Prints "(1, 2) is near the origin."
```

<!--
  - test: `expression-pattern`

  ```swifttest
  -> let point = (1, 2)
  -> switch point {
        case (0, 0):
           print("(0, 0) is at the origin.")
        case (-2...2, -2...2):
           print("(\(point.0), \(point.1)) is near the origin.")
        default:
           print("The point is at (\(point.0), \(point.1)).")
     }
  <- (1, 2) is near the origin.
  ```
-->

`~=` 연산자를 오버로드하여 커스텀 표현식 일치 동작을 제공할 수 있습니다.
예를 들어 위의 예시에서 `point` 표현식을 문자열로 비교하기 위해
다시 작성할 수 있습니다.

```swift
// Overload the ~= operator to match a string with an integer.
func ~= (pattern: String, value: Int) -> Bool {
    return pattern == "\(value)"
}
switch point {
case ("0", "0"):
    print("(0, 0) is at the origin.")
default:
    print("The point is at (\(point.0), \(point.1)).")
}
// Prints "The point is at (1, 2)."
```

<!--
  - test: `expression-pattern`

  ```swifttest
  -> // Overload the ~= operator to match a string with an integer.
  -> func ~= (pattern: String, value: Int) -> Bool {
        return pattern == "\(value)"
     }
  -> switch point {
        case ("0", "0"):
           print("(0, 0) is at the origin.")
        default:
           print("The point is at (\(point.0), \(point.1)).")
     }
  <- The point is at (1, 2).
  ```
-->

> Grammar of an expression pattern:
>
> *expression-pattern* → *expression*

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->
