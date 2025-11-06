{% hint style="info" %}
📢 **공지**  
이 문서는 더 이상 업데이트되지 않습니다.  
아래 새로운 URL로 업데이트 되었으니, 새로운 URL로 접속해 주세요.  
[Swift Book Korean](https://bbiguduk.github.io/swift-book-korean/documentation/tsplk/)
{% endhint %}

# 프로토콜 (Protocols)

타입이 구현해야 하는 요구사항을 정의합니다.

*프로토콜(protocol)*은 특정 작업이나 기능에 적합한
메서드, 프로퍼티, 그리고 다른 요구사항의
청사진을 정의합니다.
프로토콜은 클래스, 구조체, 열거형이 *채택*하여
이 요구사항의 구현을 제공할 수 있습니다.
프로토콜의 요구사항을 만족하는 모든 타입은
프로토콜을 *준수(conform)*한다고 합니다.

프로토콜을 준수하는 타입이 구현해야 하는 요구사항을 명시하는 것 외에도,
프로토콜을 확장하여 이 요구사항 중 일부를 구현하거나
준수하는 타입이 활용할 수 있는 추가 기능을 구현할 수도 있습니다.

<!--
  FIXME: Protocols should also be able to support initializers,
  and indeed you can currently write them,
  but they don't work due to
  <rdar://problem/13695680> Constructor requirements in protocols (needed for NSCoding).
  I'll need to write about them once this is fixed.
  UPDATE: actually, they *can* be used right now,
  but only in a generic function, and not more generally with the protocol type.
  I'm not sure I should mention them in this chapter until they work more generally.
-->

## 프로토콜 문법 (Protocol Syntax)

프로토콜은 클래스, 구조체, 열거형과 유사한 방법으로 정의합니다:

```swift
protocol SomeProtocol {
    // protocol definition goes here
}
```

<!--
  - test: `protocolSyntax`

  ```swifttest
  -> protocol SomeProtocol {
        // protocol definition goes here
     }
  ```
-->

커스텀 타입이 특정 프로토콜을 채택한다고 나타내려면
타입 이름 뒤에 콜론과 함께
프로토콜 이름을 나열합니다.
여러 프로토콜을 채택하려면 콤마로 구분하여 나열할 수 있습니다:

```swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
    // structure definition goes here
}
```

<!--
  - test: `protocolSyntax`

  ```swifttest
  >> protocol FirstProtocol {}
  >> protocol AnotherProtocol {}
  -> struct SomeStructure: FirstProtocol, AnotherProtocol {
        // structure definition goes here
     }
  ```
-->

클래스에 상위 클래스가 존재한다면,
상위 클래스 다음에 콤마로 구분하여 채택하는 모든 프로토콜을 나열합니다:

```swift
class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
    // class definition goes here
}
```

<!--
  - test: `protocolSyntax`

  ```swifttest
  >> class SomeSuperclass {}
  -> class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
        // class definition goes here
     }
  ```
-->

> Note: 프로토콜은 타입이기 때문에
> Swift에서 다른 타입의 이름처럼
> (`Int`, `String`, `Double` 등)
> 대문자로 시작합니다
> (`FullyNamed`, `RandomNumberGenerator` 등).

## 프로퍼티 요구사항 (Property Requirements)

프로토콜은 이를 준수하는 타입이
특정 이름과 타입을 가진 인스턴스 프로퍼티나 타입 프로퍼티를 제공하도록 요구할 수 있습니다.
프로토콜은 프로퍼티가
저장 프로퍼티나 연산 프로퍼티인지 지정하지 않습니다 ---
필요한 프로퍼티 이름과 타입만 지정합니다.
프로토콜은 각 프로퍼티가 읽기만 가능한지
읽기와 쓰기가 모두 가능한지도 지정합니다.

프로토콜이 프로퍼티의 읽기와 쓰기를 모두 요구하는 경우,
상수 저장 프로퍼티나 읽기 전용 계산 프로퍼티는
해당 요구사항을 충족할 수 없습니다.
프로토콜이 프로퍼티의 읽기만 요구하는 경우,
모든 종류의 프로퍼티로 요구사항을 충족할 수 있고
필요한 경우
쓰기가 가능하게 구현해도 무방합니다.

프로퍼티 요구사항은 항상 `var` 키워드로
변수 프로퍼티로 선언됩니다.
읽기와 쓰기 가능한 프로퍼티는
타입 선언 뒤에 `{ get set }`으로 작성하여 나타내고,
읽기만 가능한 프로퍼티는 `{ get }`으로 작성하여 나타냅니다.

```swift
protocol SomeProtocol {
    var mustBeSettable: Int { get set }
    var doesNotNeedToBeSettable: Int { get }
}
```

<!--
  - test: `instanceProperties`

  ```swifttest
  -> protocol SomeProtocol {
        var mustBeSettable: Int { get set }
        var doesNotNeedToBeSettable: Int { get }
     }
  ```
-->

프로토콜에 타입 프로퍼티 요구사항을 정의하려면
`static` 키워드를 붙여야 합니다.
클래스에 타입 프로퍼티 요구사항을 구현할 때는 `class`나 `static` 키워드를 사용할 수 있지만,
프로토콜에서는 `static` 키워드만 사용해야 합니다:

```swift
protocol AnotherProtocol {
    static var someTypeProperty: Int { get set }
}
```

<!--
  - test: `instanceProperties`

  ```swifttest
  -> protocol AnotherProtocol {
        static var someTypeProperty: Int { get set }
     }
  ```
-->

다음은 단일 인스턴스 프로퍼티 요구사항을 가지는 프로토콜 예시입니다:

```swift
protocol FullyNamed {
    var fullName: String { get }
}
```

<!--
  - test: `instanceProperties`

  ```swifttest
  -> protocol FullyNamed {
        var fullName: String { get }
     }
  ```
-->

`FullyNamed` 프로토콜은 이를 준수하는 타입이 완벽한 이름을 제공하도록 요구합니다.
이 프로토콜은 준수 타입의 성격에 대해 다른 것은 지정하지 않으며 ---
해당 타입이 전체 이름을 제공해야 된다고만 요구합니다.
이것은 `FullyNamed` 타입은
`String` 타입의 `fullName`이라는 읽기만 가능한 인스턴스 프로퍼티를 가져야 합니다.

다음은 `FullyNamed` 프로토콜을
채택하고 준수하는 구조체의 예시입니다:

```swift
struct Person: FullyNamed {
    var fullName: String
}
let john = Person(fullName: "John Appleseed")
// john.fullName is "John Appleseed"
```

<!--
  - test: `instanceProperties`

  ```swifttest
  -> struct Person: FullyNamed {
        var fullName: String
     }
  -> let john = Person(fullName: "John Appleseed")
  /> john.fullName is \"\(john.fullName)\"
  </ john.fullName is "John Appleseed"
  ```
-->

이 예시는 특정 이름을 가진 사람을 나타내는
`Person`이라는 구조체를 정의합니다.
정의의 첫 번째 줄에
`FullyNamed` 프로토콜을 채택합니다.

`Person`의 각 인스턴스는 `String` 타입의
`fullName`이라는 단일 저장 프로퍼티를 가집니다.
이것은 `FullyNamed` 프로토콜의 단일 요구사항과 일치하고
`Person`은 프로토콜을 올바르게 준수하고 있다는 의미입니다.
(Swift는 프로토콜 요구사항이 충족되지 않으면 컴파일 시 오류가 발생합니다.)

다음은 `FullyNamed` 프로토콜을 채택하고 준수하는 더 복잡한 클래스입니다:

```swift
class Starship: FullyNamed {
    var prefix: String?
    var name: String
    init(name: String, prefix: String? = nil) {
        self.name = name
        self.prefix = prefix
    }
    var fullName: String {
        return (prefix != nil ? prefix! + " " : "") + name
    }
}
var ncc1701 = Starship(name: "Enterprise", prefix: "USS")
// ncc1701.fullName is "USS Enterprise"
```

<!--
  - test: `instanceProperties`

  ```swifttest
  -> class Starship: FullyNamed {
        var prefix: String?
        var name: String
        init(name: String, prefix: String? = nil) {
           self.name = name
           self.prefix = prefix
        }
        var fullName: String {
           return (prefix != nil ? prefix! + " " : "") + name
        }
     }
  -> var ncc1701 = Starship(name: "Enterprise", prefix: "USS")
  /> ncc1701.fullName is \"\(ncc1701.fullName)\"
  </ ncc1701.fullName is "USS Enterprise"
  ```
-->

이 클래스는 스타십에 대해 읽기 전용 연산 프로퍼티로
`fullName` 프로퍼티 요구사항을 구현합니다.
각 `Starship` 클래스 인스턴스는 필수 `name`과 옵셔널 `prefix`를 저장합니다.
`fullName` 프로퍼티는 `prefix` 값이 존재하면 사용하여
`name`의 앞에 추가해 스타십의 전체 이름을 생성합니다.

<!--
  TODO: add some advice on how protocols should be named
-->

## 메서드 요구사항 (Method Requirements)

프로토콜은 이를 준수하는 타입이
특정 인스턴스 메서드와 타입 메서드를 구현하도록 요구할 수 있습니다.
이 메서드는 일반 인스턴스 메서드와 타입 메서드를 정의하는 방식과 동일하게
프로토콜의 정의에 작성되지만
중괄호나 메서드 본문 없이 작성합니다.
가변 매개변수(variadic parameter)도 일반 메서드와 같은 규칙에 따라 사용할 수 있습니다.
그러나 프로토콜의 정의 내에서 메서드 매개변수에 대해 기본값은 지정할 수 없습니다.

타입 프로퍼티 요구사항과 마찬가지로
프로토콜에 타입 메서드 요구사항을 정의할 때
`static` 키워드를 항상 표기합니다.
클래스에 타입 메서드 요구사항을 구현할 때는 `class`나 `static` 키워드를 사용할 수 있지만,
프로토콜에서는 `static` 키워드만 사용해야 합니다:

```swift
protocol SomeProtocol {
    static func someTypeMethod()
}
```

<!--
  - test: `typeMethods`

  ```swifttest
  -> protocol SomeProtocol {
        static func someTypeMethod()
     }
  ```
-->

다음 예시는 단일 인스턴스 메서드 요구사항을 가지는 프로토콜을 정의합니다:

```swift
protocol RandomNumberGenerator {
    func random() -> Double
}
```

<!--
  - test: `protocols`

  ```swifttest
  -> protocol RandomNumberGenerator {
        func random() -> Double
     }
  ```
-->

`RandomNumberGenerator` 프로토콜은
`random` 이라는 인스턴스 메서드 요구사항을 가지며
호출할 때마다 `Double` 값을 반환합니다.
프로토콜에 지정하지 않았지만,
이 값은
`0.0`부터 `1.0`미만의 숫자라고 가정합니다.

`RandomNumberGenerator` 프로토콜은 각 난수가 생성되는 방법에 대해
어떠한 것도 가정하지 않습니다 ---
이것은 새로운 난수를 생성하는
표준 방법을 제공할 것을 요구합니다.

다음은 `RandomNumberGenerator` 프로토콜을
채택하고 준수하는 클래스의 구현입니다.
이 클래스는 *선형 합동 생성기(linear congruential generator)*로 알려진
의사 난수(pseudorandom number) 생성 알고리즘을 구현합니다:

```swift
class LinearCongruentialGenerator: RandomNumberGenerator {
    var lastRandom = 42.0
    let m = 139968.0
    let a = 3877.0
    let c = 29573.0
    func random() -> Double {
        lastRandom = ((lastRandom * a + c)
            .truncatingRemainder(dividingBy:m))
        return lastRandom / m
    }
}
let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// Prints "Here's a random number: 0.3746499199817101"
print("And another one: \(generator.random())")
// Prints "And another one: 0.729023776863283"
```

<!--
  - test: `protocols`

  ```swifttest
  -> class LinearCongruentialGenerator: RandomNumberGenerator {
        var lastRandom = 42.0
        let m = 139968.0
        let a = 3877.0
        let c = 29573.0
        func random() -> Double {
           lastRandom = ((lastRandom * a + c)
               .truncatingRemainder(dividingBy:m))
           return lastRandom / m
        }
     }
  -> let generator = LinearCongruentialGenerator()
  -> print("Here's a random number: \(generator.random())")
  <- Here's a random number: 0.3746499199817101
  -> print("And another one: \(generator.random())")
  <- And another one: 0.729023776863283
  ```
-->

## Mutating 메서드 요구사항 (Mutating Method Requirements)

메서드가 속한 인스턴스를 수정이나 *변경*해야 하는 경우가 있습니다.
값 타입(구조체와 열거형)의 인스턴스 메서드의 경우
메서드의 `func` 키워드 앞에 `mutating` 키워드를 작성하여
메서드가 속한 인스턴스와
인스턴스의 모든 프로퍼티를 수정할 수 있음을 나타냅니다.
이 프로세스는 [인스턴스 메서드 내부에서 값 타입 수정 (Modifying Value Types from Within Instance Methods)](./methods.md#인스턴스-메서드-내부에서-값-타입-수정-modifying-value-types-from-within-instance-methods)에 설명되어 있습니다.

프로토콜 인스턴스 메서드 요구사항이
프로토콜을 채택하는 모든 타입의 인스턴스도 변경할 수 있도록 정의하는 경우
프로토콜의 정의에서
해당 메서드에 `mutating` 키워드를 붙여야 합니다.
이를 통해 구조체와 열거형도 프로토콜을 채택하고
메서드 요구사항을 충족할 수 있습니다.

> Note: `mutating`으로 프로토콜 인스턴스 메서드 요구사항을 표시하면,
> 클래스에 대한 해당 메서드의 구현을 작성할 때
> `mutating` 키워드를 작성할 필요가 없습니다.
> `mutating` 키워드는 구조체와 열거형에서만 사용합니다.

아래의 예시는 `toggle`이라는 단일 인스턴스 메서드 요구사항을 정의하는
`Togglable`이라는 프로토콜을 정의합니다.
이름에서 알 수 있듯이, `toggle()` 메서드는
해당 타입의 프로퍼티를 수정하여
프로토콜을 준수하는 타입의 상태를 전환하거나 반전하기 위한 것입니다.

`toggle()` 메서드는
`Togglable` 프로토콜 정의에서 `mutating` 키워드로 표시되고,
이 메서드가 호출될 때
준수하는 인스턴스의 상태를 변경한다고 나타냅니다:

```swift
protocol Togglable {
    mutating func toggle()
}
```

<!--
  - test: `mutatingRequirements`

  ```swifttest
  -> protocol Togglable {
        mutating func toggle()
     }
  ```
-->

구조체나 열거형에 `Togglable` 프로토콜을 구현하면,
해당 구조체나 열거형은 `mutating`으로 표시된
`toggle()` 메서드의 구현을 제공하여
프로토콜을 준수할 수 있습니다.

아래의 예시는 `OnOffSwitch`라는 열거형을 정의합니다.
이 열거형은 열거형 케이스 인 `on`과 `off`를 나타내기 위해
두 가지 상태를 가집니다.
이 열거형의 `toggle` 구현은
`Togglable` 프로토콜의 요구사항에 맞게 `mutating`으로 표시됩니다:

```swift
enum OnOffSwitch: Togglable {
    case off, on
    mutating func toggle() {
        switch self {
        case .off:
            self = .on
        case .on:
            self = .off
        }
    }
}
var lightSwitch = OnOffSwitch.off
lightSwitch.toggle()
// lightSwitch is now equal to .on
```

<!--
  - test: `mutatingRequirements`

  ```swifttest
  -> enum OnOffSwitch: Togglable {
        case off, on
        mutating func toggle() {
           switch self {
              case .off:
                 self = .on
              case .on:
                 self = .off
           }
        }
     }
  -> var lightSwitch = OnOffSwitch.off
  -> lightSwitch.toggle()
  // lightSwitch is now equal to .on
  ```
-->

## 이니셜라이저 요구사항 (Initializer Requirements)

프로토콜은 이를 준수하는 타입이
특정 이니셜라이저를 구현하도록 요구할 수 있습니다.
이 이니셜라이저는 일반 이니셜라이저를 정의하는 방식과 동일하게
프로토콜의 정의에 작성되지만
중괄호나 이니셜라이저 본문 없이 작성합니다.

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}
```

<!--
  - test: `initializers`

  ```swifttest
  -> protocol SomeProtocol {
        init(someParameter: Int)
     }
  ```
-->

### 프로토콜 이니셜라이저 요구사항의 클래스 구현 (Class Implementations of Protocol Initializer Requirements)

프로토콜의 이니셜라이저 요구사항은
준수하는 클래스에서 지정 이니셜라이저나 편의 이니셜라이저로 구현할 수 있습니다.
이 경우 모두
`required` 수정자를 이니셜라이저 구현에 표시해야 합니다:

```swift
class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // initializer implementation goes here
    }
}
```

<!--
  - test: `initializers`

  ```swifttest
  -> class SomeClass: SomeProtocol {
        required init(someParameter: Int) {
           // initializer implementation goes here
        }
     }
  ```
-->

<!--
  - test: `protocolInitializerRequirementsCanBeImplementedAsDesignatedOrConvenience`

  ```swifttest
  -> protocol P {
        init(x: Int)
     }
  -> class C1: P {
        required init(x: Int) {}
     }
  -> class C2: P {
        init() {}
        required convenience init(x: Int) {
           self.init()
        }
     }
  ```
-->

`required` 수정자를 사용하면
프로토콜을 준수하는
클래스의 모든 하위 클래스가
이니셜라이저 요구사항을 명시적으로 구현하거나 상속받도록 보장합니다.

더 자세한 정보는
[필수 이니셜라이저 (Required Initializers)](./initialization.md#필수-이니셜라이저-required-initializers)을 참고바랍니다.

<!--
  - test: `protocolInitializerRequirementsRequireTheRequiredModifierOnTheImplementingClass`

  ```swifttest
  -> protocol P {
        init(s: String)
     }
  -> class C1: P {
        required init(s: String) {}
     }
  -> class C2: P {
        init(s: String) {}
     }
  !$ error: initializer requirement 'init(s:)' can only be satisfied by a 'required' initializer in non-final class 'C2'
  !! init(s: String) {}
  !! ^
  !! required
  ```
-->

<!--
  - test: `protocolInitializerRequirementsRequireTheRequiredModifierOnSubclasses`

  ```swifttest
  -> protocol P {
        init(s: String)
     }
  -> class C: P {
        required init(s: String) {}
     }
  -> class D1: C {
        required init(s: String) { super.init(s: s) }
     }
  -> class D2: C {
        init(s: String) { super.init(s: s) }
     }
  !$ error: 'required' modifier must be present on all overrides of a required initializer
  !! init(s: String) { super.init(s: s) }
  !! ^
  !! required
  !$ note: overridden required initializer is here
  !! required init(s: String) {}
  !! ^
  ```
-->

> Note: `final` 수정자로 표시한 클래스는
> 하위 클래스가 될 수 없으므로
> 프로토콜 이니셜라이저 구현에 `required` 수정자를 표시할 필요가 없습니다.
> `final` 수정자에 대한 자세한 내용은 [재정의 방지 (Preventing Overrides)](./inheritance.md#재정의-방지-preventing-overrides)를 참고바랍니다.

<!--
  - test: `finalClassesDoNotNeedTheRequiredModifierForProtocolInitializerRequirements`

  ```swifttest
  -> protocol P {
        init(s: String)
     }
  -> final class C1: P {
        required init(s: String) {}
     }
  -> final class C2: P {
        init(s: String) {}
     }
  ```
-->

하위 클래스가 상위 클래스의 지정 이니셜라이저를 재정의 하고
프로토콜의 동일한 이니셜라이저 요구사항도 구현해야 하면,
`required`와 `override` 수정자 둘 다 이니셜라이저 구현에 표시해야 합니다:

```swift
protocol SomeProtocol {
    init()
}

class SomeSuperClass {
    init() {
        // initializer implementation goes here
    }
}

class SomeSubClass: SomeSuperClass, SomeProtocol {
    // "required" from SomeProtocol conformance; "override" from SomeSuperClass
    required override init() {
        // initializer implementation goes here
    }
}
```

<!--
  - test: `requiredOverrideInitializers`

  ```swifttest
  -> protocol SomeProtocol {
        init()
     }

  -> class SomeSuperClass {
        init() {
           // initializer implementation goes here
        }
     }

  -> class SomeSubClass: SomeSuperClass, SomeProtocol {
        // "required" from SomeProtocol conformance; "override" from SomeSuperClass
        required override init() {
           // initializer implementation goes here
        }
     }
  ```
-->

### 실패 가능한 이니셜라이저 요구사항 (Failable Initializer Requirements)

프로토콜은 [실패 가능한 이니셜라이저 (Failable Initializers)](./initialization.md#실패-가능한-이니셜라이저-failable-initializers)에 정의한대로
준수하는 타입에 대해 실패 가능한 이니셜라이저 요구사항을 정의할 수 있습니다.

실패 가능한 이니셜라이저 요구사항은
준수하는 타입에서 실패 가능한 이니셜라이저나 실패 불가능한 이니셜라이저로 충족될 수 있습니다.
실패 불가능한 이니셜라이저 요구사항은
실패 불가능한 이니셜라이저나 암시적 언래핑 된 실패 가능한 이니셜라이저로 충족될 수 있습니다.

<!--
  - test: `failableRequirementCanBeSatisfiedByFailableInitializer`

  ```swifttest
  -> protocol P { init?(i: Int) }
  -> class C: P { required init?(i: Int) {} }
  -> struct S: P { init?(i: Int) {} }
  ```
-->

<!--
  - test: `failableRequirementCanBeSatisfiedByIUOInitializer`

  ```swifttest
  -> protocol P { init?(i: Int) }
  -> class C: P { required init!(i: Int) {} }
  -> struct S: P { init!(i: Int) {} }
  ```
-->

<!--
  - test: `iuoRequirementCanBeSatisfiedByFailableInitializer`

  ```swifttest
  -> protocol P { init!(i: Int) }
  -> class C: P { required init?(i: Int) {} }
  -> struct S: P { init?(i: Int) {} }
  ```
-->

<!--
  - test: `iuoRequirementCanBeSatisfiedByIUOInitializer`

  ```swifttest
  -> protocol P { init!(i: Int) }
  -> class C: P { required init!(i: Int) {} }
  -> struct S: P { init!(i: Int) {} }
  ```
-->

<!--
  - test: `failableRequirementCanBeSatisfiedByNonFailableInitializer`

  ```swifttest
  -> protocol P { init?(i: Int) }
  -> class C: P { required init(i: Int) {} }
  -> struct S: P { init(i: Int) {} }
  ```
-->

<!--
  - test: `iuoRequirementCanBeSatisfiedByNonFailableInitializer`

  ```swifttest
  -> protocol P { init!(i: Int) }
  -> class C: P { required init(i: Int) {} }
  -> struct S: P { init(i: Int) {} }
  ```
-->

<!--
  - test: `nonFailableRequirementCanBeSatisfiedByNonFailableInitializer`

  ```swifttest
  -> protocol P { init(i: Int) }
  -> class C: P { required init(i: Int) {} }
  -> struct S: P { init(i: Int) {} }
  ```
-->

<!--
  - test: `nonFailableRequirementCanBeSatisfiedByIUOInitializer`

  ```swifttest
  -> protocol P { init(i: Int) }
  -> class C: P { required init!(i: Int) {} }
  -> struct S: P { init!(i: Int) {} }
  ```
-->

## 의미론적 요구사항만 있는 프로토콜 (Protocols that Have Only Semantic Requirements)

위의 모든 프로토콜 예시는 일부 메서드나 프로퍼티를 요구하지만
프로토콜 선언에 요구사항을 포함하지 않아도 됩니다.
프로토콜을 사용해 *의미론적(semantic)* 요구사항을 기술할 수도 있습니다 ---
이것은 해당 타입의 값이 어떻게 동작하고
어떤 연산을 지원하는지에 대한 요구사항을 의미합니다.
<!--
Avoiding the term "marker protocol",
which more specifically refers to @_marker on a protocol.
-->
Swift 표준 라이브러리는 여러 프로토콜을 정의하며
이것은 어떠한 메서드나 프로퍼티를 요구하지 않습니다:

- [`Sendable`][]: [Sendable 타입 (Sendable Types)](./concurrency.md#sendable-타입-sendable-types)에서 설명한대로
  동시성 도메인 간 공유될 수 있는 값을 위한 프로토콜입니다.
- [`Copyable`][]: [Borrowing 매개변수와 Consuming 매개변수 (Borrowing and Consuming Parameters)](../language-reference/declarations.md#borrowing-매개변수와-consuming-매개변수-borrowing-and-consuming-parameters)에서 설명한대로
  함수에 전달될 때
  Swift가 복사할 수 있는 값을 위한 프로토콜입니다.
- [`BitwiseCopyable`][]: 비트 단위로 복사될 수 있는 값을 위한 프로토콜입니다.

[`BitwiseCopyable`]: https://developer.apple.com/documentation/swift/bitwisecopyable
[`Copyable`]: https://developer.apple.com/documentation/swift/copyable
[`Sendable`]: https://developer.apple.com/documentation/swift/sendable

<!--
These link definitions are also used in the section below,
Implicit Conformance to a Protocol.
-->

이러한 프로토콜의 요구사항에 대한 자세한 내용은
이 문서의 개요를 참고바랍니다.

다른 프로토콜을 채택할 때와
동일한 문법을 사용하여 이러한 프로토콜을 채택합니다.
유일한 차이점은
프로토콜의 요구사항을 구현하는 메서드나 프로퍼티 선언이 포함되어 있지 않습니다.
예를 들어:

```swift
struct MyStruct: Copyable {
    var counter = 12
}

extension MyStruct: BitwiseCopyable { }
```

위 코드는 새로운 구조체를 정의합니다.
`Copyable`은 의미론적 요구사항만 가지므로
구조체 선언에 프로토콜을 채택하기 위한 코드가 없습니다.
유사하게 `BitwiseCopyable`은 의미론적 요구사항만 가지므로
프로토콜을 채택하는 확장은 빈 본문을 가집니다.

보통 이러한 프로토콜의 준수를 작성할 필요가 없습니다 ---
대신 [프로토콜 암시적 준수 (Implicit Conformance to a Protocol)](#프로토콜-암시적-준수-implicit-conformance-to-a-protocol)에서 설명한대로
Swift는 암시적으로 준수를 추가합니다.

<!-- TODO: Mention why you might define your own empty protocols. -->

## 프로토콜을 타입으로 사용 (Protocols as Types)

프로토콜은 실제로 어떤 기능도 구현하지 않습니다.
그럼에도 불구하고 코드에서 프로토콜을 타입으로 사용할 수 있습니다.

프로토콜을 타입으로 사용하는 가장 일반적인 방법은
제네릭 제약조건(generic constraint)으로 프로토콜을 사용하는 것입니다.
제네릭 제약조건이 있는 코드는
프로토콜을 준수하는 어떠한 타입에서 동작할 수 있고
구체적인 타입은 API를 사용하는 코드에 의해 선택됩니다.
예를 들어
제네릭 타입의 인자를 받는
함수를 호출할 때,
호출자는 타입을 선택합니다.

불투명 타입(opaque type)을 사용하는 코드는
프로토콜을 준수하는 일부 타입에서 동작합니다.
실제 타입은 컴파일 시간에 알 수 있으며,
API 구현에서 해당 타입을 선택하지만,
해당 타입의 식별자는 API의 클라이언트로 부터 숨겨집니다.
불투명 타입을 사용하면 API 의 자세한 구현이
추상 계층을 통해 노출되는 것을 방지할 수 있습니다 ---
예를 들어, 함수의 반환 타입을 숨기고
값이 특정 프로토콜을 준수한다는 것만 보장할 수 있습니다.

박싱 프로토콜 타입(boxed protocol type)을 사용하는 코드는
런타임 때 선택된 프로토콜을 준수하는 모든 타입에서 동작할 수 있습니다.
이런 런타임 유연성을 지원하기 위해,
Swift는 필요할 때
성능 비용을 발생하는
*박스(box)*라는 간접 계층을 추가합니다.
이러한 유연성 때문에
Swift는 컴파일 시에 실제 타입을 알 수 없으므로,
프로토콜에 의해 요구되는
멤버만 접근할 수 있습니다.
실제 타입의 다른 API에 접근하려면
런타임에서 캐스팅이 필요합니다.

프로토콜을 제네릭 제약조건으로 사용하는 것에 대한 자세한 내용은
[제네릭 (Generics)](./generics.md)을 참고바랍니다.
불투명 타입과 박싱 프로토콜 타입에 대한 자세한 내용은
[불투명 타입 (Opaque Types)](./opaque-types.md)을 참고바랍니다.

<!--
Performance impact from SE-0335:

Existential types are also significantly more expensive than using concrete types.
Because they can store any value whose type conforms to the protocol,
and the type of value stored can change dynamically,
existential types require dynamic memory
unless the value is small enough to fit within an inline 3-word buffer.
In addition to heap allocation and reference counting,
code using existential types incurs pointer indirection and dynamic method dispatch
that cannot be optimized away.
-->

## 위임 (Delegation)

*위임(Delegation)*은 클래스나 구조체가
일부 책임을 다른 타입의 인스턴스에
넘길 수 있도록 하는 디자인 패턴입니다.
이 디자인 패턴은 위임할 책임을
캡슐화한 프로토콜을 정의해 구현하며,
이를 준수하는 타입(위임자)은
위임된 기능을 반드시 구현해야 합니다.
위임은 특정 동작에 응답하거나,
해당 소스의 구체적인 타입을 알 필요 없이
외부 소스에서 데이터를 가져올 때 사용할 수 있습니다.

아래 예시는 주사위 게임과
게임의 진행 사항을 관찰하는
위임에 대한 중첩된 프로토콜을 정의합니다:

```swift
class DiceGame {
    let sides: Int
    let generator = LinearCongruentialGenerator()
    weak var delegate: Delegate?

    init(sides: Int) {
        self.sides = sides
    }

    func roll() -> Int {
        return Int(generator.random() * Double(sides)) + 1
    }

    func play(rounds: Int) {
        delegate?.gameDidStart(self)
        for round in 1...rounds {
            let player1 = roll()
            let player2 = roll()
            if player1 == player2 {
                delegate?.game(self, didEndRound: round, winner: nil)
            } else if player1 > player2 {
                delegate?.game(self, didEndRound: round, winner: 1)
            } else {
                delegate?.game(self, didEndRound: round, winner: 2)
            }
        }
        delegate?.gameDidEnd(self)
    }

    protocol Delegate: AnyObject {
        func gameDidStart(_ game: DiceGame)
        func game(_ game: DiceGame, didEndRound round: Int, winner: Int?)
        func gameDidEnd(_ game: DiceGame)
    }
}
```

`DiceGame` 클래스는 각 플레이어가 주사위를 굴리고,
주사위를 굴려 가장 높은 숫자를 얻는 플레이어가 승리하는
게임을 구현합니다.
이전 챕터의 예시에서
사용한 선형 합동 생성기를 사용하여
주사위 굴림에 대한 난수를 생성합니다.

`DiceGame.Delegate` 프로토콜은
주사위 게임의 진행 사항을 추적하기 위해 채택할 수 있습니다.
`DiceGame.Delegate` 프로토콜은
주사위 게임의 내에서 사용되기 때문에,
`DiceGame` 클래스 내에 중첩되어 있습니다.
프로토콜은
외부 선언이 제네릭하지 않으면,
구조체와 클래스와 같은 타입 선언 내에 중첩될 수 있습니다.
중첩 타입에 대한 자세한 내용은 [중첩 타입 (Nested Types)](./nested-types.md)을 참고바랍니다.

강한 순환 참조를 방지하기 위해,
위임자(delegate)는 약한 참조로 선언됩니다.
약한 참조에 대한 자세한 내용은
[클래스 인스턴스 간의 강한 순환 참조 (Strong Reference Cycles Between Class Instances)](./automatic-reference-counting.md#클래스-인스턴스-간의-강한-순환-참조-strong-reference-cycles-between-class-instances)를 참고바랍니다.
프로토콜을 클래스 전용 프로토콜로 표시하면
`DiceGame` 클래스는
위임자가 약한 참조로 사용되어야 한다고 선언할 수 있습니다.
[클래스 전용 프로토콜 (Class Only Protocols)](#클래스-전용-프로토콜-class-only-protocols)에서 설명했듯이
클래스 전용 프로토콜은
`AnyObject`의 상속받음으로 표시합니다.

`DiceGame.Delegate`는 게임의 진행 사항을 추적하기 위해 세 개의 메서드를 제공합니다.
이 세 개의 메서드는 위의 `play(rounds:)` 메서드에서
게임 로직으로 사용됩니다.
`DiceGame` 클래스는 새로운 게임이 시작하거나, 새로운 차례가 시작하거나, 게임이 끝날 때
해당 위임 메서드를 호출합니다.

`delegate` 프로퍼티는 *옵셔널* `DiceGame.Delegate`이므로,
`play(rounds:)` 메서드는 [옵셔널 체이닝 (Optional Chaining)](./optional-chaining.md)에서 설명한 대로
위임에 대한 메서드를 호출할 때마다 옵셔널 체이닝을 사용합니다.
`delegate` 프로퍼티가 `nil`이면
이 위임자 호출은 무시됩니다.
`delegate` 프로퍼티가 `nil`이 아니면
이 위임자 메서드가 호출되고,
매개변수로 `DiceGame` 인스턴스를 전달합니다.

다음 예시는 `DiceGame.Delegate` 프로토콜을 채택하는
`DiceGameTracker`라는 클래스는 나타냅니다:

```swift
class DiceGameTracker: DiceGame.Delegate {
    var playerScore1 = 0
    var playerScore2 = 0
    func gameDidStart(_ game: DiceGame) {
        print("Started a new game")
        playerScore1 = 0
        playerScore2 = 0
    }
    func game(_ game: DiceGame, didEndRound round: Int, winner: Int?) {
        switch winner {
            case 1:
                playerScore1 += 1
                print("Player 1 won round \(round)")
            case 2: playerScore2 += 1
                print("Player 2 won round \(round)")
            default:
                print("The round was a draw")
        }
    }
    func gameDidEnd(_ game: DiceGame) {
        if playerScore1 == playerScore2 {
            print("The game ended in a draw.")
        } else if playerScore1 > playerScore2 {
            print("Player 1 won!")
        } else {
            print("Player 2 won!")
        }
    }
}
```

`DiceGameTracker` 클래스는 `DiceGame.Delegate` 프로토콜에 의해 요구되는
세 가지 메서드를 모두 구현합니다.
이 메서드는
새로운 게임이 시작되면 플레이어의 점수를 모두 0으로 초기화하고,
각 라운드가 끝날 때마다 점수를 업데이트하고,
게임이 끝나면 승자를 발표합니다.

`DiceGame`과 `DiceGameTracker`의 실제 동작은 다음과 같습니다:

```swift
let tracker = DiceGameTracker()
let game = DiceGame(sides: 6)
game.delegate = tracker
game.play(rounds: 3)
// Started a new game
// Player 2 won round 1
// Player 2 won round 2
// Player 1 won round 3
// Player 2 won!
```

## 확장으로 프로토콜 준수 추가 (Adding Protocol Conformance with an Extension)

기존 타입의 소스 코드에 접근할 수 없더라도
새로운 프로토콜을 채택하고 준수하도록 확장할 수 있습니다.
확장은 기존 타입에 새로운 프로퍼티, 메서드, 서브스크립트를 추가할 수 있으므로,
프로토콜이 요구하는 모든 요구사항을 추가할 수 있습니다.
자세한 내용은 [확장 (Extensions)](./extensions.md)을 참고바랍니다.

> Note: 확장을 통해 타입에 프로토콜 준수를 추가하면,
> 기존 인스턴스도 자동으로 해당 프로토콜을 채택하고 준수합니다.

예를 들어 `TextRepresentable`이라는 프로토콜은
텍스트로 표현할 수 있는 모든 타입이 구현할 수 있습니다.
이것은 자신에 대한 설명이거나 현재 상태의 텍스트 버전일 수 있습니다:

```swift
protocol TextRepresentable {
    var textualDescription: String { get }
}
```

<!--
  - test: `protocols`

  ```swifttest
  -> protocol TextRepresentable {
        var textualDescription: String { get }
     }
  ```
-->

위에서 `Dice` 클래스는 확장을 통해 `TextRepresentable`을 채택하고 준수할 수 있습니다:

<!--
  No "from above" xref because
  even though Dice isn't defined in the section immediately previous
  it's part of a running example and Dice is used in that section.
-->

```swift
extension Dice: TextRepresentable {
    var textualDescription: String {
        return "A \(sides)-sided dice"
    }
}
```

<!--
  - test: `protocols`

  ```swifttest
  -> extension Dice: TextRepresentable {
        var textualDescription: String {
           return "A \(sides)-sided dice"
        }
     }
  ```
-->

이 확장은 `Dice`가 원래 구현에서 프로토콜을 채택한 것과 동일하게
새로운 프로토콜을 채택합니다.
프로토콜 이름은 콜론으로 구분된 타입 이름 뒤에 제공되고,
프로토콜의 모든 요구사항은
확장의 중괄호 내에 구현합니다.

이제 모든 `Dice` 인스턴스는 `TextRepresentable`로 취급될 수 있습니다:

```swift
let d12 = Dice(sides: 12, generator: LinearCongruentialGenerator())
print(d12.textualDescription)
// Prints "A 12-sided dice"
```

<!--
  - test: `protocols`

  ```swifttest
  -> let d12 = Dice(sides: 12, generator: LinearCongruentialGenerator())
  -> print(d12.textualDescription)
  <- A 12-sided dice
  ```
-->

마찬가지로 `SnakesAndLadders` 게임 클래스도 확장을 통해
`TextRepresentable` 프로토콜을 채택하고 준수할 수 있습니다:

```swift
extension SnakesAndLadders: TextRepresentable {
    var textualDescription: String {
        return "A game of Snakes and Ladders with \(finalSquare) squares"
    }
}
print(game.textualDescription)
// Prints "A game of Snakes and Ladders with 25 squares"
```

<!--
  - test: `protocols`

  ```swifttest
  -> extension SnakesAndLadders: TextRepresentable {
        var textualDescription: String {
           return "A game of Snakes and Ladders with \(finalSquare) squares"
        }
     }
  -> print(game.textualDescription)
  <- A game of Snakes and Ladders with 25 squares
  ```
-->

### 조건부 프로토콜 준수 (Conditionally Conforming to a Protocol)

제네릭 타입은 타입의 제네릭 매개변수가 프로토콜을 준수하는 경우와 같이
특정 조건에서만
프로토콜의 요구사항을 충족시킬 수 있습니다.
확장에서 제약조건을 명시하여
제네릭 타입이 조건부로 프로토콜을 준수하도록 만들 수 있습니다.
제네릭 `where` 절을 작성하여
채택할 프로토콜 이름 뒤에 제약조건을 명시합니다.
제네릭 `where` 절에 대한 자세한 내용은 [제네릭 Where 절 (Generic Where Clauses)](./generics.md#제네릭-where-절-generic-where-clauses)을 참고바랍니다.

다음 확장은
`Array` 인스턴스가 저장하는 항목이 `TextRepresentable`을 준수할 때에만
`TextRepresentable` 프로토콜을 준수하도록 합니다.

```swift
extension Array: TextRepresentable where Element: TextRepresentable {
    var textualDescription: String {
        let itemsAsText = self.map { $0.textualDescription }
        return "[" + itemsAsText.joined(separator: ", ") + "]"
    }
}
let myDice = [d6, d12]
print(myDice.textualDescription)
// Prints "[A 6-sided dice, A 12-sided dice]"
```

<!--
  - test: `protocols`

  ```swifttest
  -> extension Array: TextRepresentable where Element: TextRepresentable {
        var textualDescription: String {
           let itemsAsText = self.map { $0.textualDescription }
           return "[" + itemsAsText.joined(separator: ", ") + "]"
        }
     }
     let myDice = [d6, d12]
  -> print(myDice.textualDescription)
  <- [A 6-sided dice, A 12-sided dice]
  ```
-->

### 확장으로 프로토콜 채택 선언 (Declaring Protocol Adoption with an Extension)

타입이 이미 프로토콜의 모든 요구사항을 준수하지만,
해당 프로토콜을 채택한다고 아직 명시하지 않은 경우,
빈 확장을 사용하여 프로토콜을 채택하도록 만들 수 있습니다:

```swift
struct Hamster {
    var name: String
    var textualDescription: String {
        return "A hamster named \(name)"
    }
}
extension Hamster: TextRepresentable {}
```

<!--
  - test: `protocols`

  ```swifttest
  -> struct Hamster {
        var name: String
        var textualDescription: String {
           return "A hamster named \(name)"
        }
     }
  -> extension Hamster: TextRepresentable {}
  ```
-->

`Hamster`의 인스턴스는 `TextRepresentable`이 요구되는 타입 어디에서나 사용할 수 있습니다:

```swift
let simonTheHamster = Hamster(name: "Simon")
let somethingTextRepresentable: TextRepresentable = simonTheHamster
print(somethingTextRepresentable.textualDescription)
// Prints "A hamster named Simon"
```

<!--
  - test: `protocols`

  ```swifttest
  -> let simonTheHamster = Hamster(name: "Simon")
  -> let somethingTextRepresentable: TextRepresentable = simonTheHamster
  -> print(somethingTextRepresentable.textualDescription)
  <- A hamster named Simon
  ```
-->

> Note: 타입은 요구사항이 충족된다고 해서 프로토콜을 자동으로 채택하지 않습니다.
> 항상 프로토콜 채택을 명시적으로 선언해야 합니다.

## 자동 생성 구현을 사용하여 프로토콜 채택 (Adopting a Protocol Using a Synthesized Implementation)

Swift는 많은 경우에
`Equatable`, `Hashable`, `Comparable`에 대해
프로토콜 준수성을 자동으로 제공할 수 있습니다.
이렇게 합성 구현을 사용하면
프로토콜 요구사항 구현을 위해
반복적인 상용구 코드를 작성할 필요가 없습니다.

<!--
  Linking directly to a section of an article like the URLs below do
  is expected to be stable --
  as long as the section stays around, that topic ID will be there too.

  Conforming to the Equatable Protocol
  https://developer.apple.com/documentation/swift/equatable#2847780

  Conforming to the Hashable Protocol
  https://developer.apple.com/documentation/swift/hashable#2849490

  Conforming to the Comparable Protocol
  https://developer.apple.com/documentation/swift/comparable#2845320

  ^-- Need to add discussion of synthesized implementation
  to the reference for Comparable, since that's new

  Some of the information in the type references above
  is also repeated in the "Conform Automatically to Equatable and Hashable" section
  of the article "Adopting Common Protocols".
  https://developer.apple.com/documentation/swift/adopting_common_protocols#2991123
-->

Swift는 다음과 같은 커스텀 타입에 대해
`Equatable`의 합성 구현을 제공합니다:

- `Equatable` 프로토콜을 준수하는 저장 프로퍼티만 있는 구조체
- `Equatable` 프로토콜을 준수하는 연관 타입만 있는 열거형
- 연관 타입이 없는 열거형

`==`의 합성 구현을 받기 위해선,
`==` 연산자를 직접 구현하지 않고
원래 선언을 포함한 파일에서
`Equatable`을 채택한다고 선언합니다.
`Equatable` 프로토콜은 `!=`의 기본 구현도 제공합니다.

아래의 예시는 `Vector2D` 구조체와 유사한
3차원의 벡터 `(x, y, z)`에 대한
`Vector3D` 구조체를 정의합니다.
`x`, `y`, `z` 프로퍼티는 모두 `Equatable` 타입이므로,
`Vector3D`는 등가 연산자의
합성 구현을 받습니다.

```swift
struct Vector3D: Equatable {
    var x = 0.0, y = 0.0, z = 0.0
}

let twoThreeFour = Vector3D(x: 2.0, y: 3.0, z: 4.0)
let anotherTwoThreeFour = Vector3D(x: 2.0, y: 3.0, z: 4.0)
if twoThreeFour == anotherTwoThreeFour {
    print("These two vectors are also equivalent.")
}
// Prints "These two vectors are also equivalent."
```

<!--
  - test: `equatable_synthesis`

  ```swifttest
  -> struct Vector3D: Equatable {
        var x = 0.0, y = 0.0, z = 0.0
     }

  -> let twoThreeFour = Vector3D(x: 2.0, y: 3.0, z: 4.0)
  -> let anotherTwoThreeFour = Vector3D(x: 2.0, y: 3.0, z: 4.0)
  -> if twoThreeFour == anotherTwoThreeFour {
         print("These two vectors are also equivalent.")
     }
  <- These two vectors are also equivalent.
  ```
-->

<!--
  Need to cross reference here from "Adopting Common Protocols"
  https://developer.apple.com/documentation/swift/adopting_common_protocols

  Discussion in the article calls out that
  enums without associated values are Equatable & Hashable
  even if you don't declare the protocol conformance.
-->

Swift는 다음과 같은 커스텀 타입에 대해
`Hashable`의 합성 구현을 제공합니다:

- `Hashable` 프로토콜을 준수하는 저장 프로퍼티만 가지는 구조체
- `Hashable` 프로토콜을 준수하는 연관 타입만 가지는 열거형
- 연관 타입이 없는 열거형

`hash(into:)`의 합성 구현을 받기 위해선,
`hash(into:)` 메서드를 직접 구현하지 않고
원래 선언을 포함한 파일에서
`Hashable`을 채택한다고 선언합니다.

Swift는 원시값이 없는 열거형에 대해
`Comparable`의 합성 구현을 제공합니다.
열거형이 연관 타입을 가지고 있다면,
모두 `Comparable` 프로토콜을 준수해야 합니다.
`<`의 합성 구현을 받기 위해선,
`<` 연산자를 직접 구현하지 않고
원래 열거형 선언을 포함한 파일에서
`Comparable`을 채택한다고 선언합니다.
`<=`, `>`, `>=`의 `Comparable` 프로토콜의 기본 구현도
자동으로 제공합니다.

아래의 예시는 초보자, 중급자, 전문가의 케이스를 가진
`SkillLevel` 열거형을 정의합니다.
전문가는 가진 별의 숫자에 따라 추가적으로 순위가 매겨집니다.

```swift
enum SkillLevel: Comparable {
    case beginner
    case intermediate
    case expert(stars: Int)
}
var levels = [SkillLevel.intermediate, SkillLevel.beginner,
              SkillLevel.expert(stars: 5), SkillLevel.expert(stars: 3)]
for level in levels.sorted() {
    print(level)
}
// Prints "beginner"
// Prints "intermediate"
// Prints "expert(stars: 3)"
// Prints "expert(stars: 5)"
```

<!--
  - test: `comparable-enum-synthesis`

  ```swifttest
  -> enum SkillLevel: Comparable {
         case beginner
         case intermediate
         case expert(stars: Int)
     }
  -> var levels = [SkillLevel.intermediate, SkillLevel.beginner,
                   SkillLevel.expert(stars: 5), SkillLevel.expert(stars: 3)]
  -> for level in levels.sorted() {
         print(level)
     }
  <- beginner
  <- intermediate
  <- expert(stars: 3)
  <- expert(stars: 5)
  ```
-->

<!--
  The example above iterates and prints instead of printing the whole array
  because printing an array gives you the debug description of each element,
  which looks like temp123908.SkillLevel.expert(5) -- not nice to read.
-->

<!--
  - test: `no-synthesized-comparable-for-raw-value-enum`

  ```swifttest
  >> enum E: Int, Comparable {
  >>     case ten = 10
  >>     case twelve = 12
  >> }
  !$ error: type 'E' does not conform to protocol 'Comparable'
  !! enum E: Int, Comparable {
  !!      ^
  !$ note: enum declares raw type 'Int', preventing synthesized conformance of 'E' to 'Comparable'
  !! enum E: Int, Comparable {
  !!         ^
  !$ note: candidate would match if 'E' conformed to 'FloatingPoint'
  !! public static func < (lhs: Self, rhs: Self) -> Bool
  !!                        ^
  !$ note: candidate has non-matching type '<Self, Other> (Self, Other) -> Bool'
  !! public static func < <Other>(lhs: Self, rhs: Other) -> Bool where Other : BinaryInteger
  !!                        ^
  !$ note: candidate would match if 'E' conformed to '_Pointer'
  !! public static func < (lhs: Self, rhs: Self) -> Bool
  !!                        ^
  !$ note: candidate would match if 'E' conformed to '_Pointer'
  !! @inlinable public static func < <Other>(lhs: Self, rhs: Other) -> Bool where Other : _Pointer
  !!                                   ^
  !$ note: candidate has non-matching type '<Self> (Self, Self) -> Bool'
  !! @inlinable public static func < (x: Self, y: Self) -> Bool
  !!                                   ^
  !$ note: candidate would match if 'E' conformed to 'StringProtocol'
  !! @inlinable public static func < <RHS>(lhs: Self, rhs: RHS) -> Bool where RHS : StringProtocol
  !!                                   ^
  !$ note: protocol requires function '<' with type '(E, E) -> Bool'
  !! static func < (lhs: Self, rhs: Self) -> Bool
  !!                 ^
  ```
-->

## 프로토콜 암시적 준수 (Implicit Conformance to a Protocol)

일부 프로토콜은 너무 흔해서
새로운 타입을 선언할 때 거의 항상 작성하게 됩니다.
다음의 프로토콜의 경우
프로토콜의 요구사항을 구현하는 타입을 정의할 때
Swift가 자동으로 준수를 추론하므로
직접 작성할 필요가 없습니다:

- [`Copyable`][]
- [`Sendable`][]
- [`BitwiseCopyable`][]

<!--
The definitions for the links in this list
are in the section above, Protocols That Have Semantic Requirements.
-->

명시적으로 준수를 작성할 수 있지만
코드의 동작을 변경하지 않습니다.
암시적 준수를 억제하려면
준수 목록에서 프로토콜 이름 앞에 물결표(`~`)를 작성합니다:

```swift
struct FileDescriptor: ~Sendable {
    let rawValue: Int
}
```

<!--
The example above is based on a Swift System API.
https://github.com/apple/swift-system/blob/main/Sources/System/FileDescriptor.swift

See also this PR that adds Sendable conformance to FileDescriptor:
https://github.com/apple/swift-system/pull/112
-->

위 코드는 POSIX 파일 디스크립터를 감싸는 래퍼의 일부를 보여줍니다.
`FileDescriptor` 구조체는
`Sendable` 프로토콜의 요구사항을 모두 충족하여
일반적으로 Sendable합니다.
그러나
`~Sendable` 작성하는 것은 암시적 준수를 억제합니다.
파일 디스크립터는 열린 파일을 식별하고 상호작용하기 위해
정수를 사용하고
정수값은 Sendable함에도 불구하고
이를 Sendable하지 않게 만드는 것은 특정 버그를 피하는데 도움을 줄 수 있습니다.

암시적 준수를 억제하는 다른 방법은
비가용성으로 표시된 확장을 사용하는 것입니다:

```swift
@available(*, unavailable)
extension FileDescriptor: Sendable { }
```

<!--
  - test: `suppressing-implied-sendable-conformance`
  
  -> struct FileDescriptor {
  ->     let rawValue: CInt
  -> }

  -> @available(*, unavailable)
  -> extension FileDescriptor: Sendable { }
  >> let nonsendable: Sendable = FileDescriptor(rawValue: 10)
  !$ warning: conformance of 'FileDescriptor' to 'Sendable' is unavailable; this is an error in Swift 6
  !! let nonsendable: Sendable = FileDescriptor(rawValue: 10)
  !! ^
  !$ note: conformance of 'FileDescriptor' to 'Sendable' has been explicitly marked unavailable here
  !! extension FileDescriptor: Sendable { }
  !! ^
-->

이전 예시처럼
코드의 한 곳에 `~Sendable`을 작성하면,
프로그램의 다른 곳에 있는 코드가
`FileDescriptor` 타입에 `Sendable` 준수를 추가하여 확장할 수 있습니다.
반면에
이 예시의 비가용성 확장은
`Sendable`의 암시적 준수를 억제할 뿐만 아니라
코드의 다른 곳에 있는 어떤 확장도
해당 타입에 `Sendable` 준수를 추가하는 것을 방지합니다.

> Note:
> 위에 언급한 프로토콜 외에도
> 분산 액터(distributed actors)는 [`Codable`][] 프로토콜을 암시적으로 준수합니다.

[`Codable`]: https://developer.apple.com/documentation/swift/codable

## 프로토콜 타입의 컬렉션 (Collections of Protocol Types)

프로토콜은 [프로토콜을 타입으로 사용 (Protocols as Types)](#프로토콜을-타입으로-사용-protocols-as-types)에서 언급했듯이
배열이나 딕셔너리와 같은 컬렉션의
타입으로 사용할 수 있습니다.
이 예시는 `TextRepresentable`에 대한 배열을 생성합니다:

```swift
let things: [TextRepresentable] = [game, d12, simonTheHamster]
```

<!--
  - test: `protocols`

  ```swifttest
  -> let things: [TextRepresentable] = [game, d12, simonTheHamster]
  ```
-->

이제 배열에 항목을 반복할 수 있고,
각 항목의 설명을 출력할 수 있습니다:

```swift
for thing in things {
    print(thing.textualDescription)
}
// A game of Snakes and Ladders with 25 squares
// A 12-sided dice
// A hamster named Simon
```

<!--
  - test: `protocols`

  ```swifttest
  -> for thing in things {
        print(thing.textualDescription)
     }
  </ A game of Snakes and Ladders with 25 squares
  </ A 12-sided dice
  </ A hamster named Simon
  ```
-->

`thing` 상수는 `TextRepresentable` 타입입니다.
실제 인스턴스가 `Dice`, `DiceGame`, `Hamster` 중
하나 이지만 이 타입은 아닙니다.
그럼에도 불구하고 `TextRepresentable` 타입이므로,
`TextRepresentable`은 `textualDescription` 프로퍼티를 가지고 있다는 것을 알고 있으므로
루프를 통해 `thing.textualDescription`에 안전하게 접근할 수 있습니다.

## 프로토콜 상속 (Protocol Inheritance)

프로토콜은 하나 이상의 다른 프로토콜을 *상속*할 수 있고,
상속받은 요구사항에 새로운 요구사항을 추가할 수 있습니다.
프로토콜 상속의 문법은 클래스 상속 문법과 유사하지만,
여러 프로토콜을 콤마로 구분하여 나열할 수 있습니다:

```swift
protocol InheritingProtocol: SomeProtocol, AnotherProtocol {
    // protocol definition goes here
}
```

<!--
  - test: `protocols`

  ```swifttest
  >> protocol SomeProtocol {}
  >> protocol AnotherProtocol {}
  -> protocol InheritingProtocol: SomeProtocol, AnotherProtocol {
        // protocol definition goes here
     }
  ```
-->

다음은 위의 `TextRepresentable` 프로토콜을
상속하는 프로토콜의 예입니다:

```swift
protocol PrettyTextRepresentable: TextRepresentable {
    var prettyTextualDescription: String { get }
}
```

<!--
  - test: `protocols`

  ```swifttest
  -> protocol PrettyTextRepresentable: TextRepresentable {
        var prettyTextualDescription: String { get }
     }
  ```
-->

이 예시는 `TextRepresentable`을 상속하는
`PrettyTextRepresentable`이라는 새로운 프로토콜을 정의합니다.
`PrettyTextRepresentable`을 채택하는 타입은
`TextRepresentable`의 모든 요구사항을 만족해야 하며,
*추가*로 `PrettyTextRepresentable`의 모든 요구사항도 만족해야 합니다.
이 예시에서 `PrettyTextRepresentable`은
`String`을 반환하는 `prettyTextualDescription`이라는 읽기 전용 프로퍼티 요구사항을 추가합니다.

`SnakesAndLadders` 클래스는 확장을 통해 `PrettyTextRepresentable`을 채택하고 준수할 수 있습니다:

```swift
extension SnakesAndLadders: PrettyTextRepresentable {
    var prettyTextualDescription: String {
        var output = textualDescription + ":\n"
        for index in 1...finalSquare {
            switch board[index] {
            case let ladder where ladder > 0:
                output += "▲ "
            case let snake where snake < 0:
                output += "▼ "
            default:
                output += "○ "
            }
        }
        return output
    }
}
```

<!--
  - test: `protocols`

  ```swifttest
  -> extension SnakesAndLadders: PrettyTextRepresentable {
        var prettyTextualDescription: String {
           var output = textualDescription + ":\n"
           for index in 1...finalSquare {
              switch board[index] {
                 case let ladder where ladder > 0:
                    output += "▲ "
                 case let snake where snake < 0:
                    output += "▼ "
                 default:
                    output += "○ "
              }
           }
           return output
        }
     }
  ```
-->

이 확장은 `PrettyTextRepresentable` 프로토콜을 채택하고
`SnakesAndLadders` 타입에 대해
`prettyTextualDescription` 프로퍼티의 구현을 제공한다고 나타냅니다.
`PrettyTextRepresentable`을 채택한 타입은 `TextRepresentable`도 채택해야 하므로,
`prettyTextualDescription`의 구현은
출력 문자열을 위해 `TextRepresentable` 프로토콜에서
`textualDescription` 프로퍼티를 접근하는 것으로 시작합니다.
콜론과 줄 바꿈을 추가하고
정리된 텍스트 표현을 시작으로 사용합니다.
그런 다음 보드 사각형의 배열을 통해 반복하고,
각 사각형의 내용을 나타내기 위해 기호를 추가합니다:

- 사각형의 값이 `0`보다 크면, 사다리의 밑부분이 되고
  `▲`로 표시합니다.
- 사각형의 값이 `0`보다 작으면,
  뱀의 머리이고 `▼`으로 표시합니다.
- 사각형의 값이 `0`이고 "자유" 정사각형이면,
  `○`으로 표시합니다.

`prettyTextualDescription` 프로퍼티는 이제 모든 `SnakesAndLadders` 인스턴스의
텍스트 설명을 출력하기 위해 사용할 수 있습니다:

```swift
print(game.prettyTextualDescription)
// A game of Snakes and Ladders with 25 squares:
// ○ ○ ▲ ○ ○ ▲ ○ ○ ▲ ▲ ○ ○ ○ ▼ ○ ○ ○ ○ ▼ ○ ○ ▼ ○ ▼ ○
```

<!--
  - test: `protocols`

  ```swifttest
  -> print(game.prettyTextualDescription)
  </ A game of Snakes and Ladders with 25 squares:
  </ ○ ○ ▲ ○ ○ ▲ ○ ○ ▲ ▲ ○ ○ ○ ▼ ○ ○ ○ ○ ▼ ○ ○ ▼ ○ ▼ ○
  ```
-->

## 클래스 전용 프로토콜 (Class-Only Protocols)

프로토콜 상속 목록에 `AnyObject` 프로토콜을 추가여
해당 프로토콜을 클래스 타입(구조체나 열거형이 아닌)에서만 채택할 수 있게 제한할 수 있습니다.

```swift
protocol SomeClassOnlyProtocol: AnyObject, SomeInheritedProtocol {
    // class-only protocol definition goes here
}
```

<!--
  - test: `classOnlyProtocols`

  ```swifttest
  >> protocol SomeInheritedProtocol {}
  -> protocol SomeClassOnlyProtocol: AnyObject, SomeInheritedProtocol {
        // class-only protocol definition goes here
     }
  ```
-->

위의 예시에서 `SomeClassOnlyProtocol`은 클래스 타입서에만 채택할 수 있습니다.
`SomeClassOnlyProtocol`을
구조체나 열거형에 채택하면 컴파일 오류가 발생합니다.

> Note: 프로토콜 요구사항이
> 참조 타입의 특성을 필요로 하거나 기대하는 경우
> 클래스 전용 프로토콜을 사용합니다.
> 참조 타입과 값 타입에 대한 자세한 내용은
> [구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)](./structures-and-classes.md#구조체와-열거형은-값-타입-structures-and-enumerations-are-value-types)과
> [클래스는 참조 타입 (Classes Are Reference Types)](./structures-and-classes.md#클래스는-참조-타입-classes-are-reference-types)을 참고바랍니다.

<!--
  - test: `anyobject-doesn't-have-to-be-first`

  ```swifttest
  >> protocol SomeInheritedProtocol {}
  -> protocol SomeClassOnlyProtocol: SomeInheritedProtocol, AnyObject {
        // class-only protocol definition goes here
     }
  ```
-->

<!--
  TODO: a Cacheable protocol might make a good example here?
-->

## 프로토콜 합성 (Protocol Composition)

동시에 여러 프로토콜을 준수하는 타입을 요구하는 것이 유용할 수 있습니다.
*프로토콜 합성(protocol composition)*을 사용하여
여러 프로토콜을 단일 요구사항으로 결합할 수 있습니다.
프로토콜 합성은
모든 프로토콜의 요구사항을 가진
임시 로컬 프로토콜을 정의한 것처럼 동작합니다.
프로토콜 합성은 새로운 프로토콜 타입을 정의하지 않습니다.

프로토콜 합성은 `SomeProtocol & AnotherProtocol` 형태입니다.
앰퍼샌드(`&`)로 구분하여
많은 프로토콜을 나열할 수 있습니다.
프로토콜 목록 외에도
프로토콜 합성은 클래스 타입 하나를 포함시켜
필수 상위 클래스를 지정할 수도 있습니다.

다음은 `Named`와 `Aged`라는 두 프로토콜을
함수 매개변수에 단일 요구사항으로 결합한 예입니다:

```swift
protocol Named {
    var name: String { get }
}
protocol Aged {
    var age: Int { get }
}
struct Person: Named, Aged {
    var name: String
    var age: Int
}
func wishHappyBirthday(to celebrator: Named & Aged) {
    print("Happy birthday, \(celebrator.name), you're \(celebrator.age)!")
}
let birthdayPerson = Person(name: "Malcolm", age: 21)
wishHappyBirthday(to: birthdayPerson)
// Prints "Happy birthday, Malcolm, you're 21!"
```

<!--
  - test: `protocolComposition`

  ```swifttest
  -> protocol Named {
        var name: String { get }
     }
  -> protocol Aged {
        var age: Int { get }
     }
  -> struct Person: Named, Aged {
        var name: String
        var age: Int
     }
  -> func wishHappyBirthday(to celebrator: Named & Aged) {
        print("Happy birthday, \(celebrator.name), you're \(celebrator.age)!")
     }
  -> let birthdayPerson = Person(name: "Malcolm", age: 21)
  -> wishHappyBirthday(to: birthdayPerson)
  <- Happy birthday, Malcolm, you're 21!
  ```
-->

이 예시에서
`Named` 프로토콜은
`name`이라는 읽기 전용 `String` 프로퍼티인 단일 요구사항을 가지고 있습니다.
`Aged` 프로토콜은
`age`라는 읽기 전용 `Int` 프로퍼티인 단일 요구사항을 가지고 있습니다.
두 프로토콜 모두 `Person` 구조체에서 채택합니다.

예시는 `wishHappyBirthday(to:)` 함수도 정의합니다.
`celebrator` 매개변수의 타입은 "`Named`와 `Aged` 프로토콜 모두 준수하는 타입"이라는 의미인
`Named & Aged`입니다.
요구된 프로토콜 모두 준수하는 한
함수에 전달하는 특정 타입은 중요하지 않습니다.

그런 다음 `birthdayPerson`이라는 새로운 `Person` 인스턴스를 생성하고,
`wishHappyBirthday(to:)` 함수에 새로운 인스턴스를 전달합니다.
`Person`은 프로토콜을 모두 준수하기 때문에 이 호출은 유효하고
`wishHappyBirthday(to:)` 함수는 생일 메세지를 출력할 수 있습니다.

다음은
`Location` 클래스와
이전 예시에서의 `Named` 프로토콜을 결합한 예입니다:

```swift
class Location {
    var latitude: Double
    var longitude: Double
    init(latitude: Double, longitude: Double) {
        self.latitude = latitude
        self.longitude = longitude
    }
}
class City: Location, Named {
    var name: String
    init(name: String, latitude: Double, longitude: Double) {
        self.name = name
        super.init(latitude: latitude, longitude: longitude)
    }
}
func beginConcert(in location: Location & Named) {
    print("Hello, \(location.name)!")
}

let seattle = City(name: "Seattle", latitude: 47.6, longitude: -122.3)
beginConcert(in: seattle)
// Prints "Hello, Seattle!"
```

<!--
  - test: `protocolComposition`

  ```swifttest
  -> class Location {
         var latitude: Double
         var longitude: Double
         init(latitude: Double, longitude: Double) {
             self.latitude = latitude
             self.longitude = longitude
         }
     }
  -> class City: Location, Named {
         var name: String
         init(name: String, latitude: Double, longitude: Double) {
             self.name = name
             super.init(latitude: latitude, longitude: longitude)
         }
     }
  -> func beginConcert(in location: Location & Named) {
         print("Hello, \(location.name)!")
     }

  -> let seattle = City(name: "Seattle", latitude: 47.6, longitude: -122.3)
  -> beginConcert(in: seattle)
  <- Hello, Seattle!
  ```
-->

`beginConcert(in:)` 함수는
"`Location`의 하위 클래스이면서
`Named` 프로토콜을 준수하는 모든 타입"이라는 의미의
`Location & Named` 타입의 매개변수를 가집니다.
이 경우 `City`는 이 요구사항에 충족합니다.

`Person`은 `Location`의 하위 클래스가 아니므로,
`beginConcert(in:)` 함수로 `birthdayPerson` 전달은 유효하지 않습니다.
마찬가지로
`Named` 프로토콜을 준수하지 않고
`Location`의 하위 클래스를 만들어
타입의 인스턴스로 `beginConcert(in:)`을 호출하는 것도
유효하지 않습니다.

## 프로토콜 준수 여부 검사 (Checking for Protocol Conformance)

프로토콜 준수 여부를 확인하거나 특정 프로토콜로 캐스팅 하기 위해
[타입 캐스팅 (Type Casting)](./type-casting.md)에서 설명한 `is`와 `as` 연산자를 사용할 수 있습니다.
프로토콜을 확인하고 캐스팅하는 것은
타입을 확인하고 캐스팅하는 것과 정확하게 같은 문법을 따릅니다:

- `is` 연산자는 인스턴스가 프로토콜을 준수한다면 `true`를 반환하고,
  그렇지 않으면 `false`를 반환합니다.
- 다운캐스트 연산자의 `as?` 버전은
  프로토콜의 타입의 옵셔널 값을 반환하고
  인스턴스가 프로토콜을 준수하지 않으면 `nil`을 반환합니다.
- 다운캐스트 연산자의 `as!` 버전은 프로토콜 타입으로 강제로 다운캐스트하고
  다운캐스트가 성공하지 못하면 런타임 오류가 발생합니다.

아래 예시는 `area`라는 읽기 전용 `Double` 프로퍼티의 단일 프로퍼티 요구사항을 가지는
`HasArea`라는 프로토콜을 정의합니다:

```swift
protocol HasArea {
    var area: Double { get }
}
```

<!--
  - test: `protocolConformance`

  ```swifttest
  -> protocol HasArea {
        var area: Double { get }
     }
  ```
-->

다음은 `HasArea` 프로토콜을 준수하는
`Circle`과 `Country`인 두 개의 클래스 입니다:

```swift
class Circle: HasArea {
    let pi = 3.1415927
    var radius: Double
    var area: Double { return pi * radius * radius }
    init(radius: Double) { self.radius = radius }
}
class Country: HasArea {
    var area: Double
    init(area: Double) { self.area = area }
}
```

<!--
  - test: `protocolConformance`

  ```swifttest
  -> class Circle: HasArea {
        let pi = 3.1415927
        var radius: Double
        var area: Double { return pi * radius * radius }
        init(radius: Double) { self.radius = radius }
     }
  -> class Country: HasArea {
        var area: Double
        init(area: Double) { self.area = area }
     }
  ```
-->

`Circle` 클래스는 저장 프로퍼티 `radius`를 기반으로
연산 프로퍼티 `area`를 구현합니다.
`Country` 클래스는 저장 프로퍼티로 직접 `area` 요구사항을 구현합니다.
두 클래스 모두 `HasArea` 프로토콜을 준수합니다.

다음은 `HasArea` 프로토콜을 준수하지 않는 `Animal`이라는 클래스 입니다:

```swift
class Animal {
    var legs: Int
    init(legs: Int) { self.legs = legs }
}
```

<!--
  - test: `protocolConformance`

  ```swifttest
  -> class Animal {
        var legs: Int
        init(legs: Int) { self.legs = legs }
     }
  ```
-->

`Circle`, `Country`, `Animal` 클래스는 공통 상위 클래스가 없습니다.
그럼에도 불구하고 모두 클래스이므로 이 세 가지 타입의 인스턴스는
타입 `AnyObject`의 값을 저장하는 배열에 저장할 수 있습니다:

```swift
let objects: [AnyObject] = [
    Circle(radius: 2.0),
    Country(area: 243_610),
    Animal(legs: 4)
]
```

<!--
  - test: `protocolConformance`

  ```swifttest
  -> let objects: [AnyObject] = [
        Circle(radius: 2.0),
        Country(area: 243_610),
        Animal(legs: 4)
     ]
  ```
-->

`objects` 배열은
2의 반지름을 가진 `Circle` 인스턴스,
제곱 킬로미터 단위로 영국의 면적으로 초기화 된
`Country` 인스턴스,
네 개의 다리를 가진 `Animal` 인스턴스를 포함하는 배열 리터럴로 초기화 됩니다.

`objects` 배열은 이제 반복될 수 있고,
배열의 각 객체는
`HasArea` 프로토콜을 준수하는지 확인할 수 있습니다:

```swift
for object in objects {
    if let objectWithArea = object as? HasArea {
        print("Area is \(objectWithArea.area)")
    } else {
        print("Something that doesn't have an area")
    }
}
// Area is 12.5663708
// Area is 243610.0
// Something that doesn't have an area
```

<!--
  - test: `protocolConformance`

  ```swifttest
  -> for object in objects {
        if let objectWithArea = object as? HasArea {
           print("Area is \(objectWithArea.area)")
        } else {
           print("Something that doesn't have an area")
        }
     }
  </ Area is 12.5663708
  </ Area is 243610.0
  </ Something that doesn't have an area
  ```
-->

배열의 객체가 `HasArea` 프로토콜을 준수하면,
`as?` 연산자가 반환하는 옵셔널 값은 `objectWithArea`라는 상수에
옵셔널 바인딩으로 언래핑합니다.
`objectWithArea` 상수는 `HasArea` 타입으로 알고 있으므로,
`area` 프로퍼티는 접근 가능하고 안전하게 출력할 수 있습니다.

기본 객체는 캐스팅 과정에 의해 변경되지 않습니다.
그것은 계속해서 `Circle`, `Country`, `Animal`입니다.
그러나 `objectWithArea` 상수에 저장될 때,
`HasArea` 타입으로만 알고 있으므로,
`area` 프로퍼티만 접근 가능합니다.

<!--
  TODO: This is an *extremely* contrived example.
  Also, it's not particularly useful to be able to get the area of these two objects,
  because there's no shared unit system.
  Also also, I'd say that a circle should probably be a structure, not a class.
  Plus, I'm having to write lots of boilerplate initializers,
  which make the example far less focused than I'd like.
  The problem is, I can't use strings within an @objc protocol
  without also having to import Foundation, so it's numbers or bust, I'm afraid.
-->

<!--
  TODO: Since the restrictions on @objc of the previous TODO are now lifted,
  Should the previous examples be revisited?
-->

## 옵셔널 프로토콜 요구사항 (Optional Protocol Requirements)

<!--
  TODO: split this section into several subsections as per [Contributor 7746]'s feedback,
  and cover the missing alternative approaches that he mentioned.
-->

<!--
  TODO: you can specify optional subscripts,
  and the way you check for them / work with them is a bit esoteric.
  You have to try and access a value from the subscript,
  and see if the value you get back (which will be an optional)
  has a value or is nil.
-->

프로토콜에 *옵셔널 요구사항(optional requirements)*을 정의할 수 있습니다.
이 요구사항은 프로토콜을 준수하는 타입이 구현할 필요가 없습니다.
옵셔널 요구사항은 프로토콜의 정의에
`optional` 수정자를 앞에 붙입니다.
옵셔널 요구사항은 Objective-C와 상호 운용되는
코드를 작성할 수 있습니다.
프로토콜과 옵셔널 요구사항 모두
`@objc` 속성으로 표시되어야 합니다.
`@objc` 프로토콜은 구조체나 열거형에서 채택할 수 없고
클래스에만 채택할 수 있습니다.

옵셔널 요구사항의 메서드나 프로퍼티를 사용할 때,
그 타입은 자동으로 옵셔널이 됩니다.
예를 들어
`(Int) -> String` 타입의 메서드는 `((Int) -> String)?`이 됩니다.
전체 함수 타입은
메서드의 반환값이 아니라
옵셔널로 래핑됩니다.

옵셔널 프로토콜 요구사항은 프로토콜을 준수하는 타입에
요구사항이 구현되어 있지 않을 수 있으므로,
옵셔널 체이닝으로 호출할 수 있습니다.
호출할 때 `someOptionalMethod?(someArgument)`와 같이
메서드의 이름 뒤에 물음표를 작성하여
옵셔널 메서드를 호출합니다.
옵셔널 체이닝에 대한 자세한 내용은 [옵셔널 체이닝 (Optional Chaining)](./optional-chaining.md)에서 확인할 수 있습니다.

다음 예시는 증가하는 값을 제공하기 위해 외부 데이터 소스를 사용하는
`Counter`라는 정수 카운팅 클래스를 정의합니다.
이 데이터 소스는 두 개의 옵셔널 요구사항을 가진
`CounterDataSource` 프로토콜로 정의됩니다:

```swift
@objc protocol CounterDataSource {
    @objc optional func increment(forCount count: Int) -> Int
    @objc optional var fixedIncrement: Int { get }
}
```

<!--
  - test: `protocolConformance`

  ```swifttest
  >> import Foundation
  -> @objc protocol CounterDataSource {
  ->    @objc optional func increment(forCount count: Int) -> Int
  ->    @objc optional var fixedIncrement: Int { get }
  -> }
  ```
-->

`CounterDataSource` 프로토콜은
`increment(forCount:)`라는 옵셔널 메서드 요구사항과
`fixedIncrement`라는 옵셔널 프로퍼티 요구사항을 정의합니다.
이 요구사항은 `Counter` 인스턴스에 대해 적절한 증가 값을 제공하기 위한
두 가지 방법을 정의합니다.

> Note: 엄밀히 말하면 프로토콜 요구사항을 구현하지 않고도
> `CounterDataSource`를 준수하는
> 커스텀 클래스를 작성할 수 있습니다.
> 둘 다 옵셔널 입니다.
> 기술적으로는 가능하지만 좋은 데이터 소스로는 적합합지 않습니다.

아래 정의된 `Counter` 클래스는
`CounterDataSource?` 타입의 옵셔널 `dataSource` 프로퍼티를 가지고 있습니다:

```swift
class Counter {
    var count = 0
    var dataSource: CounterDataSource?
    func increment() {
        if let amount = dataSource?.increment?(forCount: count) {
            count += amount
        } else if let amount = dataSource?.fixedIncrement {
            count += amount
        }
    }
}
```

<!--
  - test: `protocolConformance`

  ```swifttest
  -> class Counter {
        var count = 0
        var dataSource: CounterDataSource?
        func increment() {
           if let amount = dataSource?.increment?(forCount: count) {
              count += amount
           } else if let amount = dataSource?.fixedIncrement {
              count += amount
           }
        }
     }
  ```
-->

`Counter` 클래스는 `count`라는 변수 프로퍼티에 현재값을 저장합니다.
`Counter` 클래스는 메서드가 호출될 때마다 `count` 프로퍼티를 증가시키는
`increment`라는 메서드도 정의합니다.

`increment()` 메서드는 먼저 데이터 소스의 `increment(forCount:)` 메서드를 통해
증가 값을 조회하려고 합니다.
`increment()` 메서드는 `increment(forCount:)` 호출에 대해 옵셔널 체이닝을 사용하고
메서드의 단일 인자로 현재 `count` 값을 전달합니다.

여기서 두 단계 옵셔널 체이닝을 사용합니다.
먼저 `dataSource`는 `nil`일 수 있으므로,
`dataSource`가 `nil`이 아닌 경우에만 `increment(forCount:)` 호출해야 된다는 것을 나타내기 위해
`dataSource` 이름 뒤에 물음표를 붙입니다.
두 번째로 옵셔널 요구사항 이므로,
`dataSource`가 존재하더라도
`increment(forCount:)`가 구현되어 있다고 보장하지 않습니다.
여기서 `increment(forCount:)`가 구현되어 있지 않다는 가능성을
옵셔널 체이닝으로 처리합니다.
`increment(forCount:)`가 존재할 경우에만
`increment(forCount:)` 호출이 이뤄지고
존재하지 않으면 `nil`입니다.
이것이 `increment(forCount:)` 이름 뒤에 물음표가 붙는 이유입니다.

`increment(forCount:)` 호출은 위의 2가지 이유로 실패할 수 있으므로,
이 호출은 항상 *옵셔널* `Int` 값을 반환합니다.
`increment(forCount:)`가 `CounterDataSource`의 정의에서 옵셔널이 아닌 `Int` 값으로
반환하더라도 마찬가지입니다.
두 개의 옵셔널 체이닝 연산자가
차례로 있지만
결과는 여전히 단일 옵셔널로 래핑됩니다.
여러 개 옵셔널 체이닝 연산자 사용에 대한 자세한 내용은
[여러 단계의 체이닝 연결 (Linking Multiple Levels of Chaining)](./optional-chaining.md#여러-단계의-체이닝-연결-linking-multiple-levels-of-chaining)을 참고바랍니다.

`increment(forCount:)` 호출 후에 반환한 옵셔널 `Int`는
옵셔널 바인딩을 사용하여 `amount`라는 상수에 언래핑됩니다.
옵셔널 `Int`에 값이 포함되어 있다면 —--
이것은 위임자와 메서드가 모두 존재하고
메서드가 값을 반환 한 경우 —--
언래핑된 `amount`는 `count` 저장 프로퍼티에 추가하고
증가를 완료합니다.

`dataSource`가 `nil`이거나
데이터 소스가 `increment(forCount:)`를 구현하지 않아
`increment(forCount:)` 메서드로 부터 값을 조회할 수 *없는* 경우에는
`increment()` 메서드는
데이터 소스의 `fixedIncrement` 프로퍼티를 대신 조회하려고 합니다.
`fixedIncrement` 프로퍼티도 옵셔널 요구사항이므로,
그 값은 `fixedIncrement`가 `CounterDataSource` 프로토콜 정의에
옵셔널이 아닌 `Int` 프로퍼티로 정의되었어도
옵셔널 `Int` 값입니다.

다음은 데이터 소스를 매번 조회할 때마다 `3`의 상수 값을 반환하는
간단한 `CounterDataSource` 구현 입니다.
옵셔널 `fixedIncrement` 프로퍼티 요구사항을 구현하여 이것을 수행합니다:

```swift
class ThreeSource: NSObject, CounterDataSource {
    let fixedIncrement = 3
}
```

<!--
  - test: `protocolConformance`

  ```swifttest
  -> class ThreeSource: NSObject, CounterDataSource {
        let fixedIncrement = 3
     }
  ```
-->

새로운 `Counter` 인스턴스의 데이터 소스로 `ThreeSource` 인스턴스를 사용할 수 있습니다:

```swift
var counter = Counter()
counter.dataSource = ThreeSource()
for _ in 1...4 {
    counter.increment()
    print(counter.count)
}
// 3
// 6
// 9
// 12
```

<!--
  - test: `protocolConformance`

  ```swifttest
  -> var counter = Counter()
  -> counter.dataSource = ThreeSource()
  -> for _ in 1...4 {
        counter.increment()
        print(counter.count)
     }
  </ 3
  </ 6
  </ 9
  </ 12
  ```
-->

위의 코드는 새로운 `Counter` 인스턴스를 생성하고
이 데이터 소스를 새로운 `ThreeSource` 인스턴스로 설정하고
카운터의 `increment()` 메서드를 네 번 호출합니다.
예상대로 카운터의 `count` 프로퍼티는
`increment()`가 호출될 때마다 3씩 증가합니다.

다음은 `Counter` 인스턴스를 현재 `count` 값에서
0으로 증가나 감소시키는
`TowardsZeroSource`라는 더 복잡한 데이터 소스입니다:

```swift
class TowardsZeroSource: NSObject, CounterDataSource {
    func increment(forCount count: Int) -> Int {
        if count == 0 {
            return 0
        } else if count < 0 {
            return 1
        } else {
            return -1
        }
    }
}
```

<!--
  - test: `protocolConformance`

  ```swifttest
  -> class TowardsZeroSource: NSObject, CounterDataSource {
        func increment(forCount count: Int) -> Int {
           if count == 0 {
              return 0
           } else if count < 0 {
              return 1
           } else {
              return -1
           }
        }
     }
  ```
-->

`TowardsZeroSource` 클래스는
`CounterDataSource` 프로토콜의 옵셔널 `increment(forCount:)` 메서드를 구현하고
카운트 방향을 정하기 위해 `count` 인자값을 사용합니다.
`count`가 0이면 이 메서드는 더이상 카운트 작업을 진행하지 않음을 나타내기 위해
`0`을 반환합니다.

`-4`부터 0까지 카운트 하기 위해
기존 `Counter` 인스턴스에 `TowardsZeroSource`의 인스턴스를 사용할 수 있습니다.
카운터가 0에 도달하면 더이상 카운팅이 동작하지 않습니다:

```swift
counter.count = -4
counter.dataSource = TowardsZeroSource()
for _ in 1...5 {
    counter.increment()
    print(counter.count)
}
// -3
// -2
// -1
// 0
// 0
```

<!--
  - test: `protocolConformance`

  ```swifttest
  -> counter.count = -4
  -> counter.dataSource = TowardsZeroSource()
  -> for _ in 1...5 {
        counter.increment()
        print(counter.count)
     }
  </ -3
  </ -2
  </ -1
  </ 0
  </ 0
  ```
-->

## 프로토콜 확장 (Protocol Extensions)

프로토콜은 확장을 통해
메서드, 이니셜라이저, 서브스크립트, 연산 프로퍼티 구현을
준수 타입에 제공할 수 있습니다.
이를 통해 각 타입의 개별 구현이나
전역 함수가 아닌 프로토콜 자체에 동작을 정의할 수 있습니다.

예를 들어 `RandomNumberGenerator` 프로토콜은 확장을 통해
임의의 `Bool` 값을 반환하기 위해
필수 `random()` 메서드의 결과를 사용하는
`randomBool()` 메서드를 제공할 수 있습니다:

```swift
extension RandomNumberGenerator {
    func randomBool() -> Bool {
        return random() > 0.5
    }
}
```

<!--
  - test: `protocols`

  ```swifttest
  -> extension RandomNumberGenerator {
        func randomBool() -> Bool {
           return random() > 0.5
        }
     }
  ```
-->

프로토콜 확장을 생성함으로써
모든 준수 타입은
추가 수정없이 이 메서드를 사용할 수 있습니다.

```swift
let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// Prints "Here's a random number: 0.3746499199817101"
print("And here's a random Boolean: \(generator.randomBool())")
// Prints "And here's a random Boolean: true"
```

<!--
  - test: `protocols`

  ```swifttest
  >> do {
  -> let generator = LinearCongruentialGenerator()
  -> print("Here's a random number: \(generator.random())")
  <- Here's a random number: 0.3746499199817101
  -> print("And here's a random Boolean: \(generator.randomBool())")
  <- And here's a random Boolean: true
  >> }
  ```
-->

<!--
  The extra scope in the above test code allows this 'generator' variable to shadow
  the variable that already exists from a previous testcode block.
-->

프로토콜 확장은 준수 타입에 구현을 추가할 수 있지만
다른 프로토콜을 확장하거나 다른 프로토콜을 상속할 수 없습니다.
프로토콜 상속은 항상 프로토콜 선언 자체에서만 지정할 수 있습니다.

### 기본 구현 제공 (Providing Default Implementations)

프로토콜의 메서드나 연산 프로퍼티 요구사항에
기본 구현을 제공하기 위해 프로토콜 확장을 사용할 수 있습니다.
준수 타입이 필수 메서드나 프로퍼티의 자체 구현을 제공하면,
그 구현은 확장에서 제공하는 기본 구현 대신 사용됩니다.

> Note: 확장으로 제공된 기본 구현을 가진 프로토콜 요구사항은
> 옵셔널 프로토콜 요구사항과 다릅니다.
> 준수 타입이 자체 구현을 제공하지 않아도 되지만,
> 기본 구현을 가지므로 옵셔널 체이닝 없이 호출할 수 있습니다.

예를 들어 `TextRepresentable` 프로토콜을 상속하는
`PrettyTextRepresentable` 프로토콜은
`textualDescription` 프로퍼티 접근의 결과를 반환하기 위해
필요한 `prettyTextualDescription` 프로퍼티의 기본 구현을 제공할 수 있습니다:

```swift
extension PrettyTextRepresentable  {
    var prettyTextualDescription: String {
        return textualDescription
    }
}
```

<!--
  - test: `protocols`

  ```swifttest
  -> extension PrettyTextRepresentable  {
        var prettyTextualDescription: String {
           return textualDescription
        }
     }
  ```
-->

<!--
  TODO <rdar://problem/32211512> TSPL: Explain when you can/can't override a protocol default implementation
-->

<!--
  If something is a protocol requirement,
  types that conform to the protocol can override the default implementation.
-->

<!--
  If something isn't a requirement,
  you get wonky behavior when you try to override the default implementation.
-->

<!--
  If the static type is the conforming type,
  your override is used.
-->

<!--
  If the static type is the protocol type,
  the default implementation is used.
-->

<!--
  You can't write ``final`` on a default implementation
  to prevent someone from overriding it in a conforming type.
-->

### 프로토콜 확장에 제약조건 추가 (Adding Constraints to Protocol Extensions)

프로토콜 확장을 정의할 때,
확장의 메서드와 프로퍼티를 사용할 수 있기 전에
준수 타입이 만족해야 하는 제약조건을 지정할 수 있습니다.
제네릭 `where` 절을 작성하여
확장하는 프로토콜의 이름 뒤에 제약조건을 작성합니다.
제네릭 `where` 절의 자세한 내용은 [제네릭 Where 절 (Generic Where Clauses)](./generics.md#제네릭-where-절-generic-where-clauses)을 참고바랍니다.

예를 들어
`Equatable` 프로토콜을
준수하는 항목의 모든 컬렉션에 적용하는
`Collection` 프로토콜의 확장을 정의할 수 있습니다.
컬렉션의 요소를 Swift 표준 라이브러리의
`Equatable` 프로토콜로 제한하면
두 요소 간의 같음과 다름에 대한 확인을 위해 `==`와 `!=` 연산자를 사용할 수 있습니다.

```swift
extension Collection where Element: Equatable {
    func allEqual() -> Bool {
        for element in self {
            if element != self.first {
                return false
            }
        }
        return true
    }
}
```

<!--
  - test: `protocols`

  ```swifttest
  -> extension Collection where Element: Equatable {
         func allEqual() -> Bool {
             for element in self {
                 if element != self.first {
                     return false
                 }
             }
             return true
         }
     }
  ```
-->

`allEqual()` 메서드는 컬렉션에 모든 요소가 같을 때만
`true`를 반환합니다.

모든 요소가 같고
하나만 다른
정수의 두 배열을 생각해 봅시다:

```swift
let equalNumbers = [100, 100, 100, 100, 100]
let differentNumbers = [100, 100, 200, 100, 200]
```

<!--
  - test: `protocols`

  ```swifttest
  -> let equalNumbers = [100, 100, 100, 100, 100]
  -> let differentNumbers = [100, 100, 200, 100, 200]
  ```
-->

배열은 `Collection`을 준수하고
정수는 `Equatable`을 준수하므로,
`equalNumbers`와 `differentNumbers`는 `allEqual()` 메서드를 사용할 수 있습니다:

```swift
print(equalNumbers.allEqual())
// Prints "true"
print(differentNumbers.allEqual())
// Prints "false"
```

<!--
  - test: `protocols`

  ```swifttest
  -> print(equalNumbers.allEqual())
  <- true
  -> print(differentNumbers.allEqual())
  <- false
  ```
-->

> Note: 준수 타입이 동일한 메서드나 프로퍼티에 대한 구현을 제공하는
> 여러 제약조건의 확장에 대한 요구사항을 충족한다면
> Swift는 가장 특화된 제약조건의 구현을 사용합니다.

<!--
  TODO: It would be great to pull this out of a note,
  but we should wait until we have a better narrative that shows how this
  works with some examples.
-->

<!--
  TODO: Other things to be included
  ---------------------------------
  Class-only protocols
  Protocols marked @objc
  Standard-library protocols such as Sequence, Equatable etc.?
  Show how to make a custom type conform to Boolean or some other protocol
  Show a protocol being used by an enumeration
  accessing protocol methods, properties etc.  through a constant or variable that's *just* of protocol type
  Protocols can't be nested, but nested types can implement protocols
  Protocol requirements can be marked as @unavailable, but this currently only works if they're also marked as @objc.
  Checking for (and calling) optional implementations via optional binding and closures
-->

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->