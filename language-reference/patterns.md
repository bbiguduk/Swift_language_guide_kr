# 패턴 (Patterns)

값을 일치시키고 분리합니다.

_패턴 (pattern)_ 은 단일 값 또는 복합 값의 구조를 나타냅니다. 예를 들어 튜플 `(1, 2)` 의 구조는 콤마로 구분된 두 요소의 리스트입니다. 패턴은 특정값이 아닌 값의 구조를 나타내기 때문에 다양한 값과 일치 시킬 수 있습니다. 예를 들어 패턴 `(x, y)` 은 튜플 `(1, 2)` 와 다른 두 요소 튜플과 일치합니다. 패턴을 값과 일치시키는 것 외에도 복합 값의 일부 또는 전체를 추출하고 각 부분을 상수 또는 변수 이름으로 바인드 할 수 있습니다.

Swift 에는 두가지의 기본 패턴이 있습니다: 모든 종류의 값과 일치하는 패턴과 런타임에 지정된 값과 일치하지 않을 수 있는 패턴이 있습니다.

패턴의 첫번째 종류는 단순 변수, 상수, 그리고 옵셔널 바인딩에서 값을 구조화 하는데 사용됩니다. 와일드 카드 패턴, 식별자 패턴, 그리고 이를 포함하는 모든 값 바인딩 또는 튜플 패턴이 포함됩니다. 이러한 패턴에 타입 주석을 지정하여 특정 타입의 값만 일치되도록 제한할 수 있습니다.

패턴의 두번째 종류는 전체 패턴 일치에 사용되며 일치하려는 값이 런타임 때 없을 수 있습니다. 열거형 케이스 패턴, 옵셔널 패턴, 표현식 패턴, 그리고 타입-캐스팅 패턴을 포함합니다. `switch` 구문의 케이스 라벨, `do` 구문의 `catch` 절, 또는 `if`, `while`, `guard`, 또는 `for`-`in` 구문의 케이스 조건에서 이 패턴을 사용합니다.

> Grammar of a pattern:
>
> *pattern* → *wildcard-pattern* *type-annotation*_?_
>
> *pattern* → *identifier-pattern* *type-annotation*_?_
>
> *pattern* → *value-binding-pattern*
>
> *pattern* → *tuple-pattern* *type-annotation*_?_
>
> *pattern* → *enum-case-pattern*
>
> *pattern* → *optional-pattern*
>
> *pattern* → *type-casting-pattern*
>
> *pattern* → *expression-pattern*

## 와일드 카드 패턴 (Wildcard Pattern)

_와일드 카드 패턴 (wildcard pattern)_ 은 모든 값과 일치하고 무시되며 언더바 (`_`) 로 구성됩니다. 일치하는 값에 대해 신경쓰지 않을 경우에 와일드 카드 패턴을 사용합니다. 예를 들어 다음의 코드는 닫힌 범위 `1...3` 을 반복하고 루프가 반복할 때마다 범위의 현재값을 무시합니다:

```swift
for _ in 1...3 {
    // Do something three times.
}
```

> Grammar of a wildcard pattern:
>
> *wildcard-pattern* → **`_`**

## 식별자 패턴 (Identifier Pattern)

_식별자 패턴 (identifier pattern)_ 은 모든 값과 일치하고 일치하는 값을 변수 또는 상수 이름으로 바인드 합니다. 예를 들어 다음의 상수 선언에서 `someValue` 는 타입 `Int` 의 `42` 값이 일치하는 식별자 패턴입니다:

```swift
let someValue = 42
```

일치가 성공하면 값 `42` 는 상수 이름 `someValue` 에 바인드 (할당) 됩니다.

변수 또는 상수 선언의 왼쪽의 패턴이 식별자 패턴일 때 식별자 패턴은 암시적으로 값-바인딩 패턴 (value-binding pattern) 의 하위 패턴입니다.

> Grammar of an identifier pattern:
>
> *identifier-pattern* → *identifier*

## 값-바인딩 패턴 (Value-Binding Pattern)

_값-바인딩 패턴 (value-binding pattern)_ 은 변수 또는 상수 이름에 일치되는 값으로 바인드 합니다. 상수의 이름에 일치되는 값을 바인드 하는 값-바인딩 패턴은 `let` 키워드로 시작합니다; 변수의 이름에 바인드 하면 `var` 키워드로 시작합니다.

값-바인딩 패턴 내에서 식별자 패턴은 일치하는 값으로 새로운 명명된 변수 또는 상수로 바인드 됩니다. 예를 들어 튜플의 요소를 분해하고 각 요소의 값을 해당 식별자 패턴에 바인드 할 수 있습니다.

```swift
let point = (3, 2)
switch point {
    // Bind x and y to the elements of point.
case let (x, y):
    print("The point is at (\(x), \(y)).")
}
// Prints "The point is at (3, 2)."
```

위의 예제에서 `let` 은 튜플 패턴 `(x, y)` 에서 각 식별자 패턴에 배포합니다. 이 동작으로 인해 `switch` 케이스 `case let (x, y):` 와 `case (let x, let y):` 은 동일합니다.

> Grammar of a value-binding pattern:
>
> *value-binding-pattern* → **`var`** *pattern* | **`let`** *pattern*

## 튜플 패턴 (Tuple Pattern)

_튜플 패턴 (tuple pattern)_ 은 소괄호로 묶인 콤마로 구분된 0개 이상의 패턴의 리스트입니다. 튜플 패턴은 해당 튜플 타입의 값과 일치합니다.

타입 주석을 사용하여 튜플 타입의 특정 종류와 일치하도록 하기 위해 튜플 패턴을 제한할 수 있습니다. 예를 들어 상수 선언 `let (x, y): (Int, Int) = (1, 2)` 에서 튜플 패턴 `(x, y): (Int, Int)` 은 두 요소 모두 타입 `Int` 의 튜플 타입만 일치합니다.

튜플 패턴이 `for`-`in` 구문 또는 변수 또는 상수 선언에서 패턴으로 사용되면 와일드 카드 패턴, 식별자 패턴, 옵셔널 패턴, 또는 이를 포함하는 다른 튜플 패턴만 포함할 수 있습니다. 예를 들어 튜플 패턴 `(x, 0)` 에서 요소 `0` 은 표현식 패턴이므로 다음의 코드는 유효하지 않습니다:

```swift
let points = [(0, 0), (1, 0), (1, 1), (2, 0), (2, 1)]
// This code isn't valid.
for (x, 0) in points {
    /* ... */
}
```

단일 요소를 포함하는 튜플 패턴 주변의 소괄호는 아무런 효과가 없습니다. 패턴은 단일 요소의 타입의 값과 일치합니다. 예를 들어 다음은 동일합니다:

```swift
let a = 2        // a: Int = 2
let (a) = 2      // a: Int = 2
let (a): Int = 2 // a: Int = 2
```

> Grammar of a tuple pattern:
>
> *tuple-pattern* → **`(`** *tuple-pattern-element-list*_?_ **`)`**
>
> *tuple-pattern-element-list* → *tuple-pattern-element* | *tuple-pattern-element* **`,`** *tuple-pattern-element-list*
>
> *tuple-pattern-element* → *pattern* | *identifier* **`:`** *pattern*

## 열거형 케이스 패턴 (Enumeration Case Pattern)

_열거형 케이스 패턴 (enumeration case pattern)_ 은 존재하는 열거형 타입의 케이스와 일치합니다. 열거형 케이스 패턴은 `switch` 구문 케이스 라벨과 `if`, `while`, `guard`, 그리고 `for`-`in` 구문의 케이스 조건에서 나타납니다.

일치 시키려는 열거형 케이스에 연관된 값이 있는 경우 해당 열거형 케이스 패턴은 각 연관된 값에 대한 하나의 요소를 포함하는 튜플 패턴을 지정해야 합니다. 연관된 값을 포함하는 열거형 케이스를 일치 시키기 위해 `switch` 구문을 사용하는 예제는 [연관된 값 (Associated Values)](../language-guide-1/enumerations.md#associated-values) 을 참고 바랍니다.

열거형 케이스 패턴은 옵셔널로 래핑된 케이스의 값과도 일치합니다. 이 간략한 구문으로 옵셔널 패턴을 생략할 수 있습니다. `Optional` 은 열거형으로 구현되므로 `.none` 과 `.some` 은 열거형 타입의 케이스로 동일한 switch 에 나타날 수 있습니다.

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

> Grammar of an enumeration case pattern:
>
> *enum-case-pattern* → *type-identifier*_?_ **`.`** *enum-case-name* *tuple-pattern*_?_

## 옵셔널 패턴 (Optional Pattern)

_옵셔널 패턴 (optional pattern)_ 은 `Optional<Wrapped>` 열거형의 `some(Wrapped)` 케이스에 래핑된 값과 일치합니다. 옵셔널 패턴은 식별자 패턴과 물음표 바로 뒤에 오는 것으로 구성되며 열거형 케이스 패턴과 동일한 위치에 나타납니다.

옵셔널 패턴은 `Optional` 열거형 케이스 패턴에 대한 구문 설탕 이므로 다음은 동일합니다:

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

옵셔널 패턴은 `for`-`in` 구문에서 옵셔널 값의 배열을 반복하는 편리한 방법을 제공하여 `nil` 이 아닌 요소에 대해서만 루프의 본문을 실행합니다.

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

> Grammar of an optional pattern:
>
> *optional-pattern* → *identifier-pattern* **`?`**

## 타입-캐스팅 패턴 (Type-Casting Patterns)

타입-캐스팅 패턴 (type-casting pattern) 은 `is` 패턴과 `as` 패턴 두가지가 있습니다. `is` 패턴은 `switch` 구문 케이스 라벨에서만 나타납니다. `is` 와 `as` 패턴은 다음의 형식을 가집니다:

```swift
is <#type#>
<#pattern#> as <#type#>
```

`is` 패턴은 런타임 시 해당 값의 타입이 `is` 패턴의 오른편에 지정한 타입과 일치하거나 해당 타입의 하위 클래스가 일치하면 값으로 일치됩니다. `is` 패턴은 타입 캐스트를 동작하지만 반환된 타입을 버린다는 것에서 `is` 연산자와 유사하게 동작합니다.

`as` 패턴은 런타임 시 해당 값의 타입이 `as` 패턴의 오른편에 지정한 타입과 일치하거나 해당 타입의 하위 클래스가 일치하면 값으로 일치합니다. 일치가 성공하면 일치된 값의 타입은 `as` 패턴의 오른편에 지정한 _패턴 (pattern)_ 으로 캐스팅 됩니다.

`is` 와 `as` 패턴으로 값을 일치 시키기 위해 `switch` 구문을 사용하는 예제는 [Any 와 AnyObject 에 대한 타입 캐스팅 (Type Casting for Any and AnyObject)](../language-guide-1/type-casting.md#any-anyobject-type-casting-for-any-and-anyobject) 을 참고 바랍니다.

> Grammar of a type casting pattern:
>
> *type-casting-pattern* → *is-pattern* | *as-pattern*
>
> *is-pattern* → **`is`** *type*
>
> *as-pattern* → *pattern* **`as`** *type*

## 표현식 패턴 (Expression Pattern)

_표현식 패턴 (expression pattern)_ 은 표현식의 값을 표현합니다. 표현식 패턴은 `switch` 구문 케이스 라벨에서만 나타납니다.

표현식 패턴에 의해 표현된 표현식은 Swift 표준 라이브러리 `~=` 연산자를 사용하여 입력 표현식의 값과 비교합니다. `~=` 연산자가 `true` 를 반환하면 일치는 성공합니다. 기본적으로 `~=` 연산자는 `==` 연산자를 사용하여 동일한 타입의 두 값을 비교합니다. 다음 예제에서 보듯이 범위내에 값이 포함되는지 검사하기 위해 값의 범위로 값을 일치시킬 수도 있습니다.

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

사용자 정의 표현식 일치 동작을 제공하기 위해 `~=` 연산자를 오버로드 할 수 있습니다. 예를 들어 포인트의 문자열 표현으로 `point` 표현식을 비교하기 위해 위의 예제를 다시 작성할 수 있습니다.

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

> Grammar of an expression pattern:
>
> *expression-pattern* → *expression*
