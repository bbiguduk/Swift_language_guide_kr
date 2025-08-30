# 확장 (Extensions)

기존 타입에 기능을 추가합니다.

*확장(Extensions)*은 기존의 클래스, 구조체, 열거형, 프로토콜 타입에
새로운 기능을 추가합니다.
이것은 기존 소스 코드에 접근 권한이 없는
타입을 확장하는 기능이 포함됩니다
(*소급 모델링(retroactive modeling)*이라고 함).
확장은 Objective-C의 카테고리와 유사합니다.
(Objective-C 카테고리와 다르게 Swift 확장은 이름이 없습니다.)

Swift에서 확장은 다음을 수행할 수 있습니다:

- 인스턴스 연산 프로퍼티와 타입 연산 프로퍼티 추가
- 인스턴스 메서드와 타입 메서드 정의
- 새로운 이니셜라이저 제공
- 서브스크립트 정의
- 새로운 중첩 타입 정의와 사용
- 기존 타입이 프로토콜을 준수하도록 함

Swift에서
프로토콜을 확장하여 프로토콜 요구사항 구현을 제공하거나
준수하는 타입의 기능을 추가할 수도 있습니다.
더 자세한 내용은 [프로토콜 확장 (Protocol Extensions)](./protocols.md#프로토콜-확장-protocol-extensions)을 참고바랍니다.

> Note: 확장은 타입에 새로운 기능을 추가할 수 있지만,
> 기존 기능을 재정의할 수는 없습니다.

<!--
  - test: `extensionsCannotOverrideExistingBehavior`

  ```swifttest
  -> class C {
        var x = 0
        func foo() {}
     }
  -> extension C {
        override var x: Int {
           didSet {
              print("new x is \(x)")
           }
        }
        override func foo() {
           print("called overridden foo")
        }
     }
  !$ error: property does not override any property from its superclass
  !! override var x: Int {
  !! ~~~~~~~~     ^
  !$ error: ambiguous use of 'x'
  !! print("new x is \(x)")
  !!            ^
  !$ note: found this candidate
  !! var x = 0
  !!     ^
  !$ note: found this candidate
  !! override var x: Int {
  !!              ^
  !$ error: invalid redeclaration of 'x'
  !! override var x: Int {
  !!              ^
  !$ note: 'x' previously declared here
  !! var x = 0
  !!     ^
  !$ error: method does not override any method from its superclass
  !! override func foo() {
  !! ~~~~~~~~      ^
  !$ error: invalid redeclaration of 'foo()'
  !! override func foo() {
  !!               ^
  !$ note: 'foo()' previously declared here
  !! func foo() {}
  !!      ^
  ```
-->

## 확장 문법 (Extension Syntax)

`extension` 키워드를 사용하여 확장을 선언합니다:

```swift
extension SomeType {
    // new functionality to add to SomeType goes here
}
```

<!--
  - test: `extensionSyntax`

  ```swifttest
  >> struct SomeType {}
  -> extension SomeType {
        // new functionality to add to SomeType goes here
     }
  ```
-->

확장은 하나 이상의 프로토콜을 채택하여 기존 타입을 확장할 수 있습니다.
프로토콜 준수를 추가할 때,
클래스나 구조체를 작성하는 것과 동일한 방법으로
프로토콜 이름을 작성합니다:

```swift
extension SomeType: SomeProtocol, AnotherProtocol {
    // implementation of protocol requirements goes here
}
```

<!--
  - test: `extensionSyntax`

  ```swifttest
  >> protocol SomeProtocol {}
  >> protocol AnotherProtocol {}
  -> extension SomeType: SomeProtocol, AnotherProtocol {
        // implementation of protocol requirements goes here
     }
  ```
-->

이러한 방법으로 프로토콜 준수를 추가하는 것에 대한 자세한 설명은
[확장으로 프로토콜 준수 추가 (Adding Protocol Conformance with an Extension)](./protocols.md#확장으로-프로토콜-준수-추가-adding-protocol-conformance-with-an-extension)를 참고바랍니다.

확장은 [제네릭 타입 확장 (Extending a Generic Type)](./generics.md#제네릭-타입-확장-extending-a-generic-type)에 설명된대로
기존 제네릭 타입을 확장하기 위해 사용할 수 있습니다.
[제네릭 Where 절이 있는 확장 (Extensions with a Generic Where Clause)](./generics.md#제네릭-where-절이-있는-확장-extensions-with-a-generic-where-clause)에 설명된대로
제네릭 타입을 확장하여 조건부로 기능을 추가할 수도 있습니다.

> Note: 기존 타입에 새로운 기능을 추가하기 위해 확장을 정의한다면,
> 새로운 기능은 확장이 정의되기 전에 생성된
> 기존 인스턴스에서도 사용 가능합니다.

## 연산 프로퍼티 (Computed Properties)

확장은 기존 타입에 인스턴스 연산 프로퍼티와 타입 연산 프로퍼티를 추가할 수 있습니다.
이 예시는 거리 단위 작업에 필요한
Swift `Double` 타입의 다섯 개의 인스턴스 연산 프로퍼티를 추가합니다:

```swift
extension Double {
    var km: Double { return self * 1_000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1_000.0 }
    var ft: Double { return self / 3.28084 }
}
let oneInch = 25.4.mm
print("One inch is \(oneInch) meters")
// Prints "One inch is 0.0254 meters"
let threeFeet = 3.ft
print("Three feet is \(threeFeet) meters")
// Prints "Three feet is 0.914399970739201 meters"
```

<!--
  - test: `extensionsComputedProperties`

  ```swifttest
  -> extension Double {
        var km: Double { return self * 1_000.0 }
        var m: Double { return self }
        var cm: Double { return self / 100.0 }
        var mm: Double { return self / 1_000.0 }
        var ft: Double { return self / 3.28084 }
     }
  -> let oneInch = 25.4.mm
  -> print("One inch is \(oneInch) meters")
  <- One inch is 0.0254 meters
  -> let threeFeet = 3.ft
  -> print("Three feet is \(threeFeet) meters")
  <- Three feet is 0.914399970739201 meters
  ```
-->

이 연산 프로퍼티는 특정 길이의 단위를
`Double` 값으로 표현합니다.
연산 프로퍼티로 구현 되었지만,
이러한 프로퍼티의 이름은
거리 변환을 수행하기 위해 해당 리터럴 값을 사용하는 방법으로
부동소수점 값에 점 문법을 사용할 수 있습니다.

이 예시에서 `1.0`의 `Double` 값은 "1 미터"로 표시됩니다.
표현식 `1.m`은 `1.0`의 `Double` 값을 계산한 것으로 간주하므로
`m` 연산 프로퍼티가 `self`를 반환하는 이유입니다.

다른 단위는 미터로 계산된 값으로 표현되기 위해 변환을 요구합니다.
1 킬로미터는 1,000 미터와 같으므로
`km` 연산 프로퍼티는 미터로 표현된 숫자로 변환하기 위해
`1_000.00`의 값을 곱합니다.
유사하게 3.28084 피트는 1 미터이므로
`ft` 연산 프로퍼티는 피트를 미터로 변환하기 위해
`Double` 값을 `3.28084`로 나눕니다.

이 프로퍼티는 읽기 전용 연산 프로퍼티이므로,
간결함을 위해 `get` 키워드 없이 표현합니다.
반환값은 `Double` 타입의 값이고,
`Double`이 허용되는 수학적 계산 내에서는 사용될 수 있습니다:

```swift
let aMarathon = 42.km + 195.m
print("A marathon is \(aMarathon) meters long")
// Prints "A marathon is 42195.0 meters long"
```

<!--
  - test: `extensionsComputedProperties`

  ```swifttest
  -> let aMarathon = 42.km + 195.m
  -> print("A marathon is \(aMarathon) meters long")
  <- A marathon is 42195.0 meters long
  ```
-->

> Note: 확장은 새로운 연산 프로퍼티를 추가할 수 있지만 저장 프로퍼티나
> 기존 프로퍼티에 프로퍼티 관찰자를 추가할 수 없습니다.

<!--
  - test: `extensionsCannotAddStoredProperties`

  ```swifttest
  -> class C {}
  -> extension C { var x = 0 }
  !$ error: extensions must not contain stored properties
  !! extension C { var x = 0 }
  !!                   ^
  ```
-->

<!--
  TODO: change this example to something more advisable / less contentious.
-->

## 이니셜라이저 (Initializers)

확장은 기존 타입에 새로운 이니셜라이저를 추가할 수 있습니다.
이것은 이니셜라이저 매개변수로 고유한 커스텀 타입을
받아들이기 위해 다른 타입을 확장하거나
타입의 기존 구현의 부분으로 포함되지 않는
추가 초기화 옵션을 제공할 수 있습니다.

확장은 클래스에 새로운 편의 이니셜라이저를 추가할 수 있지만
새로운 지정 이니셜라이저나 디이니셜라이저는 클래스에 추가할 수 없습니다.
지정 이니셜라이저와 디이니셜라이저는
항상 기존 클래스 구현에 의해 제공되어야 합니다.

모든 저장 프로퍼티에 기본값을 제공하고
모든 커스텀 이니셜라이저를 정의하지 않은
값 타입으로 이니셜라이저를 추가하기 위해 확장을 사용하면
확장의 이니셜라이저 내에서
값 타입에 대해 기본 이니셜라이저와 멤버별 이니셜라이저를 호출할 수 있습니다.
[값 타입을 위한 이니셜라이저 위임 (Initializer Delegation for Value Types)](./initialization.md#값-타입을-위한-이니셜라이저-위임-initializer-delegation-for-value-types)에서 설명한대로
값 타입의 기존 구현의 부분으로
이니셜라이저를 작성한 경우에는 해당되지 않습니다.

다른 모듈에서 선언된
구조체에 이니셜라이저를 추가하기 위해 확장을 사용한다면
새로운 이니셜라이저는 정의한 모듈에서
이니셜라이저를 호출할 때까지 `self`를 접근할 수 없습니다.

아래의 예시는 사각형을 나타내기 위해 커스텀 `Rect` 구조체를 정의합니다.
이 예시는 모든 프로퍼티에 `0.0`의 기본값을 제공하는
`Size`와 `Point`라는 두 구조체도 정의합니다:

```swift
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
}
```

<!--
  - test: `extensionsInitializers`

  ```swifttest
  -> struct Size {
        var width = 0.0, height = 0.0
     }
  -> struct Point {
        var x = 0.0, y = 0.0
     }
  -> struct Rect {
        var origin = Point()
        var size = Size()
     }
  ```
-->

`Rect` 구조체는 모든 프로퍼티에 대해 기본값을 제공하므로
[기본 이니셜라이저 (Default Initializers)](./initialization.md#기본-이니셜라이저-default-initializers)에서 설명한대로
자동으로 기본 이니셜라이저와 멤버별 이니셜라이저를 받습니다.
이 이니셜라이저는 새로운 `Rect` 인스턴스를 생성하기 위해 사용할 수 있습니다:

```swift
let defaultRect = Rect()
let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
   size: Size(width: 5.0, height: 5.0))
```

<!--
  - test: `extensionsInitializers`

  ```swifttest
  -> let defaultRect = Rect()
  -> let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
        size: Size(width: 5.0, height: 5.0))
  ```
-->

특정 중심점과 크기를 가지는
이니셜라이저를 제공하기 위해 `Rect` 구조체를 확장할 수 있습니다:

```swift
extension Rect {
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```

<!--
  - test: `extensionsInitializers`

  ```swifttest
  -> extension Rect {
        init(center: Point, size: Size) {
           let originX = center.x - (size.width / 2)
           let originY = center.y - (size.height / 2)
           self.init(origin: Point(x: originX, y: originY), size: size)
        }
     }
  ```
-->

이 새로운 이니셜라이저는 제공된 `center` 점과 `size` 값을
기반으로 적절한 원점을 계산하는 것으로 시작합니다.
그러면 이 이니셜라이저는 적절한 프로퍼티에
새로운 원점과 크기 값을 저장하는
구조체의 자동 멤버별 이니셜라이저 `init(origin:size:)`를 호출합니다:

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                      size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
```

<!--
  - test: `extensionsInitializers`

  ```swifttest
  -> let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
        size: Size(width: 3.0, height: 3.0))
  /> centerRect's origin is (\(centerRect.origin.x), \(centerRect.origin.y)) and its size is (\(centerRect.size.width), \(centerRect.size.height))
  </ centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
  ```
-->

> Note: 확장으로 새로운 이니셜라이저를 제공하면,
> 이니셜라이저가 완료되면
> 각 인스턴스가 완전히 초기화 되었는지 확인해야 합니다.

## 메서드 (Methods)

확장은 기존 타입에 새로운 인스턴스 메서드와 타입 메서드를 추가할 수 있습니다.
아래 예시는 `Int` 타입으로 `repetitions`라는 새로운 인스턴스 메서드를 추가합니다:

```swift
extension Int {
    func repetitions(task: () -> Void) {
        for _ in 0..<self {
            task()
        }
    }
}
```

<!--
  - test: `extensionsInstanceMethods`

  ```swifttest
  -> extension Int {
        func repetitions(task: () -> Void) {
           for _ in 0..<self {
              task()
           }
        }
     }
  ```
-->

`repetitions(task:)` 메서드는 매개변수가 없고 반환값이 없는 함수를 나타내는
`() -> Void` 타입의 인자를 가집니다.

이 확장을 정의한 후에,
여러 번 작업을 수행하기 위해
모든 정수로 `repetitions(task:)` 메서드를 호출할 수 있습니다:

```swift
3.repetitions {
    print("Hello!")
}
// Hello!
// Hello!
// Hello!
```

<!--
  - test: `extensionsInstanceMethods`

  ```swifttest
  -> 3.repetitions {
        print("Hello!")
     }
  </ Hello!
  </ Hello!
  </ Hello!
  ```
-->

### 인스턴스 메서드 변경 (Mutating Instance Methods)

확장에 추가된 인스턴스 메서드는 인스턴스 자체로 수정 또는 변경할 수 있습니다.
`self`나 프로퍼티를 수정하는 구조체와 열거형 메서드는
기존 구현의 mutating 메서드와 같이
`mutating`으로 인스턴스 메서드를 표시해야 합니다.

아래 예시는 원래 값을 제곱하는
Swift의 `Int` 타입의 `square`라는 새로운 mutating 메서드를 추가합니다:

```swift
extension Int {
    mutating func square() {
        self = self * self
    }
}
var someInt = 3
someInt.square()
// someInt is now 9
```

<!--
  - test: `extensionsInstanceMethods`

  ```swifttest
  -> extension Int {
        mutating func square() {
           self = self * self
        }
     }
  -> var someInt = 3
  -> someInt.square()
  /> someInt is now \(someInt)
  </ someInt is now 9
  ```
-->

## 서브스크립트 (Subscripts)

확장은 기존 타입에 새로운 서브스크립트를 추가할 수 있습니다.
이 예시는 Swift의 `Int` 타입에 정수 서브스크립트를 추가합니다.
이 서브스크립트 `[n]`은 숫자의 오른쪽 부터
`n`에 위치하는 자리의 숫자를 반환합니다:

- `123456789[0]`은 `9`를 반환
- `123456789[1]`은 `8`을 반환

등등:

```swift
extension Int {
    subscript(digitIndex: Int) -> Int {
        var decimalBase = 1
        for _ in 0..<digitIndex {
            decimalBase *= 10
        }
        return (self / decimalBase) % 10
    }
}
746381295[0]
// returns 5
746381295[1]
// returns 9
746381295[2]
// returns 2
746381295[8]
// returns 7
```

<!--
  - test: `extensionsSubscripts`

  ```swifttest
  -> extension Int {
        subscript(digitIndex: Int) -> Int {
           var decimalBase = 1
           for _ in 0..<digitIndex {
              decimalBase *= 10
           }
           return (self / decimalBase) % 10
        }
     }
  >> let r0 =
  -> 746381295[0]
  /> returns \(r0)
  </ returns 5
  >> let r1 =
  -> 746381295[1]
  /> returns \(r1)
  </ returns 9
  >> let r2 =
  -> 746381295[2]
  /> returns \(r2)
  </ returns 2
  >> let r3 =
  -> 746381295[8]
  /> returns \(r3)
  </ returns 7
  ```
-->

<!--
  Rewrite the above to avoid bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

<!--
  TODO: Replace the for loop above with an exponent,
  if/when integer exponents land in the stdlib.
  Darwin's pow() function is only for floating point.
-->

`Int` 값이 요청된 인덱스에 대해 충분한 자리를 가지고 있지 않다면,
숫자 왼쪽에 0이 채워진 것처럼
서브스크립트 구현은 `0`을 반환합니다:

```swift
746381295[9]
// returns 0, as if you had requested:
0746381295[9]
```

<!--
  - test: `extensionsSubscripts`

  ```swifttest
  >> let r4 =
  -> 746381295[9]
  /> returns \(r4), as if you had requested:
  </ returns 0, as if you had requested:
  >> let r5 =
  -> 0746381295[9]
  ```
-->

<!--
  TODO: provide an explanation of this example
-->

<!--
  Rewrite the above to avoid bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

## 중첩 타입 (Nested Types)

확장은 기존 클래스, 구조체, 열거형에 새로운 중첩 타입을 추가할 수 있습니다:

```swift
extension Int {
    enum Kind {
        case negative, zero, positive
    }
    var kind: Kind {
        switch self {
        case 0:
            return .zero
        case let x where x > 0:
            return .positive
        default:
            return .negative
        }
    }
}
```

<!--
  - test: `extensionsNestedTypes`

  ```swifttest
  -> extension Int {
        enum Kind {
           case negative, zero, positive
        }
        var kind: Kind {
           switch self {
              case 0:
                 return .zero
              case let x where x > 0:
                 return .positive
              default:
                 return .negative
           }
        }
     }
  ```
-->

이 예시는 `Int`에 새로운 중첩된 열거형을 추가합니다.
`Kind`라는 열거형은
특정 정수 표현에 대한 숫자의 종류를 나타냅니다.
명확하게 숫자가
음수, 0, 양수 인지 표현합니다.

이 예시는 정수에 대해 적절한 `Kind` 열거형 케이스를 반환하는
`kind`라는
새로운 인스턴스 연산 프로퍼티를 `Int`에 추가합니다.

이 중첩 열거형은 모든 `Int` 값에서 사용할 수 있습니다:

```swift
func printIntegerKinds(_ numbers: [Int]) {
    for number in numbers {
        switch number.kind {
        case .negative:
            print("- ", terminator: "")
        case .zero:
            print("0 ", terminator: "")
        case .positive:
            print("+ ", terminator: "")
        }
    }
    print("")
}
printIntegerKinds([3, 19, -27, 0, -6, 0, 7])
// Prints "+ + - 0 - 0 + "
```

<!--
  - test: `extensionsNestedTypes`

  ```swifttest
  -> func printIntegerKinds(_ numbers: [Int]) {
        for number in numbers {
           switch number.kind {
              case .negative:
                 print("- ", terminator: "")
              case .zero:
                 print("0 ", terminator: "")
              case .positive:
                 print("+ ", terminator: "")
           }
        }
        print("")
     }
  -> printIntegerKinds([3, 19, -27, 0, -6, 0, 7])
  << + + - 0 - 0 +
  // Prints "+ + - 0 - 0 + "
  ```
-->

<!--
  Workaround for rdar://26016325
-->

`printIntegerKinds(_:)` 함수는
`Int` 값의 배열을 입력 받고 반복합니다.
배열의 각 정수에 대해
함수는 해당 정수의 `kind` 연산 프로퍼티를 고려하고
적절한 설명을 출력합니다.

> Note: `number.kind`는 `Int.Kind` 타입으로 이미 알고 있습니다.
> 이러한 점 때문에 모든 `Int.Kind` 케이스 값은
> `switch` 구문 내에서
> `Int.Kind.negative`가 아닌 `.negative`로 짧게 작성할 수 있습니다.

> Beta Software:
>
> This documentation contains preliminary information about an API or technology in development. This information is subject to change, and software implemented according to this documentation should be tested with final operating system software.
>
> Learn more about using [Apple's beta software](https://developer.apple.com/support/beta-software/).

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->