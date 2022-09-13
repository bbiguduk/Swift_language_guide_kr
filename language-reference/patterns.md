# 패턴 (Patterns)

<!--
A pattern represents the structure of a single value or a composite value. For example, the structure of a tuple (1, 2) is a comma-separated list of two elements. Because patterns represent the structure of a value rather than any one particular value, you can match them with a variety of values. For instance, the pattern (x, y) matches the tuple (1, 2) and any other two-element tuple. In addition to matching a pattern with a value, you can extract part or all of a composite value and bind each part to a constant or variable name.

In Swift, there are two basic kinds of patterns: those that successfully match any kind of value, and those that may fail to match a specified value at runtime.

The first kind of pattern is used for destructuring values in simple variable, constant, and optional bindings. These include wildcard patterns, identifier patterns, and any value binding or tuple patterns containing them. You can specify a type annotation for these patterns to constrain them to match only values of a certain type.

The second kind of pattern is used for full pattern matching, where the values you’re trying to match against may not be there at runtime. These include enumeration case patterns, optional patterns, expression patterns, and type-casting patterns. You use these patterns in a case label of a switch statement, a catch clause of a do statement, or in the case condition of an if, while, guard, or for-in statement.
-->

_패턴 (pattern)_ 은 단일 값 또는 복합 값의 구조를 나타냅니다. 예를 들어 튜플 `(1, 2)` 의 구조는 콤마로 구분된 두 요소의 목록입니다. 패턴은 특정값이 아닌 값의 구조를 나타내기 때문에 다양한 값과 일치 시킬 수 있습니다. 예를 들어 패턴 `(x, y)` 은 튜플 `(1, 2)` 와 다른 두 요소 튜플과 일치합니다. 패턴을 값과 일치시키는 것 외에도 복합 값의 일부 또는 전체를 추출하고 각 부분을 상수 또는 변수 이름으로 바인드 할 수 있습니다.

Swift 에는 두가지의 기본 패턴이 있습니다: 모든 종류의 값과 일치하는 패턴과 런타임에 지정된 값과 일치하지 않을 수 있는 패턴이 있습니다.

패턴의 첫번째 종류는 단순 변수, 상수, 그리고 옵셔널 바인딩에서 값을 구조화 하는데 사용됩니다. 와일드 카드 패턴, 식별자 패턴, 그리고 이를 포함하는 모든 값 바인딩 또는 튜플 패턴이 포함됩니다. 이러한 패턴에 타입 주석을 지정하여 특정 타입의 값만 일치되도록 제한할 수 있습니다.

패턴의 두번째 종류는 전체 패턴 일치에 사용되며 일치하려는 값이 런타임 때 없을 수 있습니다. 열거형 케이스 패턴, 옵셔널 패턴, 표현식 패턴, 그리고 타입-캐스팅 패턴을 포함합니다. `switch` 구문의 케이스 라벨, `do` 구문의 `catch` 절, 또는 `if`, `while`, `guard`, 또는 `for`-`in` 구문의 케이스 조건에서 이 패턴을 사용합니다.

> GRAMMAR OF A PATTERN\
> pattern → [wildcard-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_wildcard-pattern)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation) $$_{opt}$$ \
> pattern → [identifier-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_identifier-pattern)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation) $$_{opt}$$ \
> pattern → [value-binding-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_value-binding-pattern)\
> pattern → [tuple-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_tuple-pattern)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation) $$_{opt}$$ \
> pattern → [enum-case-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_enum-case-pattern)\
> pattern → [optional-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_optional-pattern)\
> pattern → [type-casting-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_type-casting-pattern)\
> pattern → [expression-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_expression-pattern)

## 와일드 카드 패턴 (Wildcard Pattern)

<!--
A wildcard pattern matches and ignores any value and consists of an underscore (_). Use a wildcard pattern when you don’t care about the values being matched against. For example, the following code iterates through the closed range 1...3, ignoring the current value of the range on each iteration of the loop:
-->

_와일드 카드 패턴 (wildcard pattern)_ 은 모든 값과 일치하고 무시되며 언더바 (`_`) 로 구성됩니다. 일치하는 값에 대해 신경쓰지 않을 경우에 와일드 카드 패턴을 사용합니다. 예를 들어 다음의 코드는 닫긴 범위 `1...3` 을 반복하고 루프의 각 반복의 범위의 현재값을 무시합니다:

```swift
for _ in 1...3 {
    // Do something three times.
}
```

> GRAMMAR OF A WILDCARD PATTERN\
> wildcard-pattern → `_`

## 식별자 패턴 (Identifier Pattern)

<!--
An identifier pattern matches any value and binds the matched value to a variable or constant name. For example, in the following constant declaration, someValue is an identifier pattern that matches the value 42 of type Int:
-->

_식별자 패턴 (identifier pattern)_ 은 모든 값과 일치하고 일치하는 값을 변수 또는 상수 이름으로 바인드 합니다. 예를 들어 다음의 상수 선언에서 `someValue` 는 타입 `Int` 의 `42` 값이 일치하는 식별자 패턴입니다:

```swift
let someValue = 42
```

<!--
When the match succeeds, the value 42 is bound (assigned) to the constant name someValue.

When the pattern on the left-hand side of a variable or constant declaration is an identifier pattern, the identifier pattern is implicitly a subpattern of a value-binding pattern.
-->

일치가 성공하면 값 `42` 는 상수 이름 `someValue` 에 바인드 (할당) 됩니다.

변수 또는 상수 선언의 왼쪽의 패턴이 식별자 패턴일 때 식별자 패턴은 암시적으로 값-바인딩 패턴 (value-binding pattern) 의 하위 패턴입니다.

> GRAMMAR OF AN IDENTIFIER PATTERN\
> identifier-pattern → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)

## 값-바인딩 패턴 (Value-Binding Pattern)

<!--
A value-binding pattern binds matched values to variable or constant names. Value-binding patterns that bind a matched value to the name of a constant begin with the let keyword; those that bind to the name of variable begin with the var keyword.

Identifiers patterns within a value-binding pattern bind new named variables or constants to their matching values. For example, you can decompose the elements of a tuple and bind the value of each element to a corresponding identifier pattern.
-->

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

<!--
In the example above, let distributes to each identifier pattern in the tuple pattern (x, y). Because of this behavior, the switch cases case let (x, y): and case (let x, let y): match the same values.
-->

위의 예제에서 `let` 은 튜플 패턴 `(x, y)` 에서 각 식별자 패턴에 배포합니다. 이 동작으로 인해 `switch` 케이스 `case let (x, y):` 와 `case (let x, let y):` 은 동일한 값과 일치합니다.

> GRAMMAR OF A VALUE-BINDING PATTERN\
> value-binding-pattern → `var` [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern) |  `let` [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern)

## 튜플 패턴 (Tuple Pattern)

<!--
A tuple pattern is a comma-separated list of zero or more patterns, enclosed in parentheses. Tuple patterns match values of corresponding tuple types.

You can constrain a tuple pattern to match certain kinds of tuple types by using type annotations. For example, the tuple pattern (x, y): (Int, Int) in the constant declaration let (x, y): (Int, Int) = (1, 2) matches only tuple types in which both elements are of type Int.

When a tuple pattern is used as the pattern in a for-in statement or in a variable or constant declaration, it can contain only wildcard patterns, identifier patterns, optional patterns, or other tuple patterns that contain those. For example, the following code isn’t valid because the element 0 in the tuple pattern (x, 0) is an expression pattern:
-->

_튜플 패턴 (tuple pattern)_ 은 소괄호로 묶인 콤마로 구분된 0개 이상의 패턴의 목록입니다. 튜플 패턴은 해당 튜플 타입의 값과 일치합니다.

타입 주석을 사용하여 튜플 타입의 특정 종류와 일치하도록 하기 위해 튜플 패턴을 제한할 수 있습니다. 예를 들어 상수 선언 `let (x, y): (Int, Int) = (1, 2)` 에서 튜플 패턴 `(x, y): (Int, Int)` 은 두 요소 모두 타입 `Int` 의 튜플 타입만 일치합니다.

튜플 패턴이 `for`-`in` 구문 또는 변수 또는 상수 선언에서 패턴으로 사용되면 와일드 카드 패턴, 식별자 패턴, 옵셔널 패턴, 또는 이를 포함하는 다른 튜플 패턴만 포함할 수 있습니다. 예를 들어 튜플 패턴 `(x, 0)` 에서 요소 `0` 은 표현식 패턴이므로 다음의 코드는 유효하지 않습니다:

```swift
let points = [(0, 0), (1, 0), (1, 1), (2, 0), (2, 1)]
// This code isn't valid.
for (x, 0) in points {
    /* ... */
}
```

<!--
The parentheses around a tuple pattern that contains a single element have no effect. The pattern matches values of that single element’s type. For example, the following are equivalent:
-->

단일 요소를 포함하는 튜플 패턴 주변의 소괄호는 아무런 효과가 없습니다. 패턴은 단일 요소의 타입의 값과 일치합니다. 예를 들어 다음은 동일합니다:

```swift
let a = 2        // a: Int = 2
let (a) = 2      // a: Int = 2
let (a): Int = 2 // a: Int = 2
```

> GRAMMAR OF A TUPLE PATTERN\
> tuple-pattern → `(` [tuple-pattern-element-list](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_tuple-pattern-element-list) $$_{opt}$$ `)` \
> tuple-pattern-element-list → [tuple-pattern-element](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_tuple-pattern-element) |  [tuple-pattern-element](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_tuple-pattern-element)  `,` [tuple-pattern-element-list](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_tuple-pattern-element-list)\
> tuple-pattern-element → [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern) |  [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)  `:` [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern)

## 열거형 케이스 패턴 (Enumeration Case Pattern)

<!--
An enumeration case pattern matches a case of an existing enumeration type. Enumeration case patterns appear in switch statement case labels and in the case conditions of if, while, guard, and for-in statements.

If the enumeration case you’re trying to match has any associated values, the corresponding enumeration case pattern must specify a tuple pattern that contains one element for each associated value. For an example that uses a switch statement to match enumeration cases containing associated values, see Associated Values.

An enumeration case pattern also matches values of that case wrapped in an optional. This simplified syntax lets you omit an optional pattern. Note that, because Optional is implemented as an enumeration, .none and .some can appear in the same switch as the cases of the enumeration type.
-->

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

> GRAMMAR OF AN ENUMERATION CASE PATTERN\
> enum-case-pattern → [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-identifier) $$_{opt}$$ `.` [enum-case-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_enum-case-name)  [tuple-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_tuple-pattern) $$_{opt}$$&#x20;

## 옵셔널 패턴 (Optional Pattern)

<!--
An optional pattern matches values wrapped in a some(Wrapped) case of an Optional<Wrapped> enumeration. Optional patterns consist of an identifier pattern followed immediately by a question mark and appear in the same places as enumeration case patterns.

Because optional patterns are syntactic sugar for Optional enumeration case patterns, the following are equivalent:
-->

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

<!--
The optional pattern provides a convenient way to iterate over an array of optional values in a for-in statement, executing the body of the loop only for non-nil elements.
-->

옵셔널 패턴은 `for`-`in` 구문에서 옵셔널 값의 배열을 반복하는 편리한 방법을 제공하여 `nil` 이 아닌 요소에 대해서만 루프의 바디를 실행합니다.

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

> GRAMMAR OF AN OPTIONAL PATTERN\
> optional-pattern → [identifier-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_identifier-pattern)  `?`

## 타입-캐스팅 패턴 (Type-Casting Patterns)

<!--
There are two type-casting patterns, the is pattern and the as pattern. The is pattern appears only in switch statement case labels. The is and as patterns have the following form:
-->

타입-캐스팅 패턴 (type-casting pattern) 은 `is` 패턴과 `as` 패턴 두가지가 있습니다. `is` 패턴은 `switch` 구문 케이스 라벨에서만 나타납니다. `is` 와 `as` 패턴은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 3.39.18.png>)

<!--
The is pattern matches a value if the type of that value at runtime is the same as the type specified in the right-hand side of the is pattern—or a subclass of that type. The is pattern behaves like the is operator in that they both perform a type cast but discard the returned type.

The as pattern matches a value if the type of that value at runtime is the same as the type specified in the right-hand side of the as pattern—or a subclass of that type. If the match succeeds, the type of the matched value is cast to the pattern specified in the right-hand side of the as pattern.

For an example that uses a switch statement to match values with is and as patterns, see Type Casting for Any and AnyObject.
-->

`is` 패턴은 런타임 시 해당 값의 타입이 `is` 패턴의 오른편에 지정한 타입과 일치하거나 해당 타입의 하위클래스가 일치하면 값으로 일치됩니다. `is` 패턴은 타입 캐스트를 동작하지만 반환된 타입을 버린다는 것에서 `is` 연산자와 유사하게 동작합니다.

`as` 패턴은 런타임 시 해당 값의 타입이 `as` 패턴의 오른편에 지정한 타입과 일치하거나 해당 타입의 하위클래스가 일치하면 값으로 일치합니다. 일치가 성공하면 일치된 값의 타입은 `as` 패턴의 오른편에 지정한 _패턴 (pattern)_ 으로 캐스팅 됩니다.

`is` 와 `as` 패턴으로 값을 일치 시키기 위해 `switch` 구문을 사용하는 예제는 [Any 와 AnyObject 에 대한 타입 캐스팅 (Type Casting for Any and AnyObject)](../language-guide-1/type-casting.md#any-anyobject-type-casting-for-any-and-anyobject) 을 참고 바랍니다.

> GRAMMAR OF A TYPE CASTING PATTERN\
> type-casting-pattern → [is-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_is-pattern) |  [as-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_as-pattern)\
> is-pattern → `is` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type)\
> as-pattern → [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern)  `as` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type)

## 표현식 패턴 (Expression Pattern)

<!--
An expression pattern represents the value of an expression. Expression patterns appear only in switch statement case labels.

The expression represented by the expression pattern is compared with the value of an input expression using the Swift standard library ~= operator. The matches succeeds if the ~= operator returns true. By default, the ~= operator compares two values of the same type using the == operator. It can also match a value with a range of values, by checking whether the value is contained within the range, as the following example shows.
-->

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

<!--
You can overload the ~= operator to provide custom expression matching behavior. For example, you can rewrite the above example to compare the point expression with a string representations of points.
-->

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

> GRAMMAR OF AN EXPRESSION PATTERN\
> expression-pattern → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression)
