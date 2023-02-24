# 속성 (Attributes)

선언과 타입에 정보를 추가합니다.

Swift 에는 두 종류의 속성 (attributes) 이 있습니다 — 선언에 적용하는 속성과 타입에 적용하는 속성이 있습니다. 속성은 선언 또는 타입에 대한 추가적인 정보를 제공합니다. 예를 들어 함수 선언에 `discardableResult` 속성은 함수가 값을 반환하지만 반환값이 사용되지 않을 때 컴파일러는 경고를 생성하지 않는 것을 나타냅니다.

속성을 지정하려면 `@` 기호 다음에 속성의 이름과 속성이 허용하는 인수를 작성합니다:

```swift
@<#attribute name#>
@<#attribute name#>(<#attribute arguments#>)
```

어떤 선언 속성은 속성에 대한 추가 정보와 특정 선언에 어떻게 적용하는지 지정하는 인수를 허용합니다. 이러한 _속성 인수 (attribute arguments)_ 는 소괄호로 둘러싸이고 해당 형식이 속한 속성에 의해 정의됩니다.

## 선언 속성 (Declaration Attributes)

선언 속성 (declaration attribute) 은 선언에만 적용할 수 있습니다.

### available

이 속성을 적용하여 특정 Swift 언어 버전 또는 특정 플랫폼과 운영체제 버전과 관련된 선언의 라이프 사이클을 나타냅니다.

`available` 속성은 둘 이상의 콤마로 구분된 속성 인수의 리스트로 나타냅니다. 이러한 인수는 다음의 플랫폼 또는 언어 이름 중 하나로 시작합니다:

* `iOS`
* `iOSApplicationExtension`
* `macOS`
* `macOSApplicationExtension`
* `macCatalyst`
* `macCatalystApplicationExtension`
* `watchOS`
* `watchOSApplicationExtension`
* `tvOS`
* `tvOSApplicationExtension`
* `swift`

또한 별표 (`*`) 를 사용하여 위에 나열된 모든 플랫폼 이름에 대한 선언의 가용성을 나타낼 수도 있습니다. Swift 버전을 사용하여 지정한 가용 `available` 속성은 별표를 사용할 수 없습니다.

나머지 인수는 순서에 상관없이 나타날 수 있으며 중요한 이정표를 도입하여 선언의 라이프 사이클에 대한 추가 정보를 지정할 수 있습니다.

* `unavailable` 인수는 선언이 지정된 플랫폼에서 가능하지 않음을 나타냅니다. 이 인수는 Swift 버전 가용성을 지정할 때 사용될 수 없습니다.
* `introduced` 인수는 선언이 도입된 지정된 플랫폼 또는 언어의 첫번째 버전을 나타냅니다. 다음과 같은 형식을 가집니다:

```swift
introduced: <#version number#>
```

_버전 번호 (version number)_ 는 마침표로 구분된 하나에서 세개의 양수로 구성됩니다.

* `deprecated` 인수는 선언이 사용되지 않는 지정된 플랫폼 또는 언어의 첫번째 버전을 나타냅니다. 다음과 같은 형식을 가집니다:

```swift
deprecated: <#version number#>
```

옵셔널 _버전 번호 (version number)_ 는 마침표로 구분된 하나에서 세개의 양수로 구성됩니다. 버전 번호를 생략하면 지원 중단된 시기에 대한 정보를 제공하지 않고 선언이 현재 지원 중단됨을 나타냅니다. 버전 숫자를 생략하면 콜론 (`:`) 도 생략합니다.

* `obsoleted` 인수는 선언이 폐기된 지정된 플랫폼 또는 언어의 첫번째 버전을 나타냅니다. 선언이 폐기되면 지정된 플랫폼 또는 언어에서 제거되고 더이상 사용이 불가능합니다. 다음의 형식을 가집니다:

```swift
obsoleted: <#version number#>
```

_버전 번호 (version number)_ 는 마침표로 구분된 하나에서 세개의 양수로 구성됩니다.

* `message` 인수는 지원 중단되거나 폐기된 선언 사용에 대한 경고나 에러를 생성할 때 컴파일러가 표시하는 텍스트 메세지를 제공합니다. 다음의 형식을 가집니다:

```swift
message: <#message#>
```

_메세지 (message)_ 는 문자열 리터럴로 구성됩니다.

* `renamed` 인수는 이름이 변경된 새로운 선언을 나타내는 텍스트 메세지를 제공합니다. 컴파일러는 이름이 변경된 선언 사용에 대한 에러를 생성할 때 새로운 이름을 표시합니다. 다음의 형식을 가집니다:

```swift
renamed: <#new name#>
```

_새로운 이름 (new name)_ 은 문자열 리터럴로 구성됩니다.

프레임워크 또는 라이브러리의 릴리즈 간에 변경된 선언의 이름을 나타내기 위해 아래와 같이 타입 별칭 선언에 `renamed` 와 `unavailable` 인수로 `available` 속성을 적용할 수 있습니다. 이 조합으로 인해 컴파일 시 선언이 변경되었다는 에러가 발생합니다.

```swift
// First release
protocol MyProtocol {
    // protocol definition
}
```

```swift
// Subsequent release renames MyProtocol
protocol MyRenamedProtocol {
    // protocol definition
}

@available(*, unavailable, renamed: "MyRenamedProtocol")
typealias MyProtocol = MyRenamedProtocol
```

단일 선언에 여러개의 `available` 속성을 적용하여 다른 플랫폼과 다른 Swift 버전에서 선언의 가용성을 지정할 수 있습니다. `available` 속성이 적용된 선언은 현재 타겟이 지정한 플랫폼 또는 언어 버전이랑 일치하지 않으면 무시됩니다. 여러개의 `available` 속성을 사용하는 경우 효과적인 가용성은 플랫폼과 Swift 가용성의 조합입니다.

`available` 속성이 플랫폼 또는 언어 이름 인수 외에 `introduced` 인수를 지정하는 경우 아래와 같은 짧은 구문을 사용할 수 있습니다:

```swift
@available(<#platform name#> <#version number#>, *)
@available(swift <#version number#>)
```

`available` 속성에 대한 짧은 구문은 여러 플랫폼에 대한 가용성을 간결하게 표현합니다. 두 형식은 동일하지만 가능하면 짧은 형식을 선호합니다.

```swift
@available(iOS 10.0, macOS 10.12, *)
class MyClass {
    // class definition
}
```

Swift 버전 숫자를 사용하여 가용성을 지정하는 `available` 속성은 선언의 플랫폼 가용성을 추가로 지정할 수 없습니다. 대신에 Swift 버전 가용성과 하나 이상의 플랫폼 가용성을 지정하기 위해 분리된 `available` 속성을 사용합니다.

```swift
@available(swift 3.0.2)
@available(macOS 10.12, *)
struct MyStruct {
    // struct definition
}
```

### discardableResult

값을 반환하는 함수 또는 메서드의 결과를 사용하지 않고 호출할 때 컴파일러가 경고를 표시하지 않도록 함수 또는 메서드에 이 속성을 적용합니다.

### dynamicCallable

호출 가능한 함수 타입의 인스턴스로 처리하기 위해 클래스, 구조체, 열거형, 또는 프로토콜에 이 속성을 적용합니다. 이 타입은 `dynamicallyCall(withArguments:)` 메서드, `dynamicallyCall(withKeywordArguments:)` 메서드 또는 둘 다 구현해야 합니다.

여러 인수를 받는 함수 인 것처럼 동적으로 호출 가능한 타입의 인스턴스를 호출할 수 있습니다.

```swift
@dynamicCallable
struct TelephoneExchange {
    func dynamicallyCall(withArguments phoneNumber: [Int]) {
        if phoneNumber == [4, 1, 1] {
            print("Get Swift help on forums.swift.org")
        } else {
            print("Unrecognized number")
        }
    }
}

let dial = TelephoneExchange()

// Use a dynamic method call.
dial(4, 1, 1)
// Prints "Get Swift help on forums.swift.org"

dial(8, 6, 7, 5, 3, 0, 9)
// Prints "Unrecognized number"

// Call the underlying method directly.
dial.dynamicallyCall(withArguments: [4, 1, 1])
```

`dynamicallyCall(withArguments:)` 메서드의 선언은 위의 예제에서 `[Int]` 와 같이 [`ExpressibleByArrayLiteral`](https://developer.apple.com/documentation/swift/expressiblebyarrayliteral) 프로토콜을 준수하는 하나의 파라미터만 가져야 합니다. 반환 타입은 모든 타입이 가능합니다.

`dynamicallyCall(withKeywordArguments:)` 메서드를 구현하는 경우 동적 메서드 호출에 라벨을 포함할 수 있습니다.

```swift
@dynamicCallable
struct Repeater {
    func dynamicallyCall(withKeywordArguments pairs: KeyValuePairs<String, Int>) -> String {
        return pairs
            .map { label, count in
                repeatElement(label, count: count).joined(separator: " ")
            }
            .joined(separator: "\n")
    }
}

let repeatLabels = Repeater()
print(repeatLabels(a: 1, b: 2, c: 3, b: 2, a: 1))
// a
// b b
// c c c
// b b
// a
```

`dynamicallyCall(withKeywordArguments:)` 메서드의 선언은 [`ExpressibleByDictionaryLiteral`](https://developer.apple.com/documentation/swift/expressiblebydictionaryliteral) 프로토콜을 준수하는 단일 파라미터를 가지고 반환 타입은 모든 타입이 가능합니다. 파라미터의 [`Key`](https://developer.apple.com/documentation/swift/expressiblebydictionaryliteral/2294108-key) 는 [`ExpressibleByStringLiteral`](https://developer.apple.com/documentation/swift/expressiblebystringliteral) 여야 합니다. 이전 예제는 파라미터 타입으로 [`KeyValuePairs`](https://developer.apple.com/documentation/swift/keyvaluepairs) 를 사용하므로 호출자는 중복 파라미터 라벨을 포함할 수 있습니다 — `a` 와 `b` 는 `repeat` 을 호출하는데 여러번 나타납니다.

`dynamicallyCall` 메서드 모두 구현하면 `dynamicallyCall(withKeywordArguments:)` 는 메서드 호출이 키워드 인수를 포함할 때 호출됩니다. 다른 경우에는 `dynamicallyCall(withArguments:)` 가 호출됩니다.

`dynamicallyCall` 메서드 구현 중 하나에서 지정한 타입과 일치하는 인수와 반환값으로만 동적으로 호출 가능한 인스턴스를 호출할 수 있습니다. 다음 예제에서 호출은 `KeyValuePairs<String, String>` 을 가지는 `dynamicallyCall(withArguments:)` 의 구현이 없으므로 컴파일 되지 않습니다.

```swift
repeatLabels(a: "four") // Error
```

### dynamicMemberLookup

런타임 시 이름별로 멤버를 조회하기 위해 클래스, 구조체, 열거형, 또는 프로토콜에 이 속성을 적용합니다. 타입은 `subscript(dynamicMemberLookup:)` 으로 구현되어야 합니다.

명시적 멤버 표현식에서 명명된 멤버에 대한 해당 선언이 없는 경우 표현식은 타입의 `subscript(dynamicMemberLookup:)` 를 호출하는 것으로 이해되고 멤버에 대한 정보를 인수로 전달합니다. 서브 스크립트는 키 경로 또는 멤버 이름으로 파라미터를 사용할 수 있습니다; 두 서브 스크립트를 모두 구현하면 키 경로 인수를 가지는 서브 스크립트가 사용됩니다.

`subscript(dynamicMemberLookup:)` 의 구현은 [`KeyPath`](https://developer.apple.com/documentation/swift/keypath), [`WritableKeyPath`](https://developer.apple.com/documentation/swift/writablekeypath), 또는 [`ReferenceWritableKeyPath`](https://developer.apple.com/documentation/swift/referencewritablekeypath) 타입의 인수를 사용하여 키 경로를 허용할 수 있습니다. 대부분 `String` 인 [`ExpressibleByStringLiteral`](https://developer.apple.com/documentation/swift/expressiblebystringliteral) 프로토콜을 준수하는 타입의 인수를 사용하여 멤버 이름을 허용할 수 있습니다. 서브 스크립트의 반환 타입은 모든 타입이 가능합니다.

멤버 이름 별 동적 멤버 조회는 다른 언어의 데이터를 Swift 로 연결할 때와 같이 컴파일 시에 타입을 확인할 수 없는 데이터에 대한 래퍼 타입을 생성하기 위해 사용될 수 있습니다. 예를 들어:

```swift
@dynamicMemberLookup
struct DynamicStruct {
    let dictionary = ["someDynamicMember": 325,
                      "someOtherMember": 787]
    subscript(dynamicMember member: String) -> Int {
        return dictionary[member] ?? 1054
    }
}
let s = DynamicStruct()

// Use dynamic member lookup.
let dynamic = s.someDynamicMember
print(dynamic)
// Prints "325"

// Call the underlying subscript directly.
let equivalent = s[dynamicMember: "someDynamicMember"]
print(dynamic == equivalent)
// Prints "true"
```

키 경로 별 동적 멤버 조회는 컴파일 시 타입 검사를 제공하는 방법으로 래퍼 타입을 구성하는데 사용될 수 있습니다. 예를 들어:

```swift
struct Point { var x, y: Int }

@dynamicMemberLookup
struct PassthroughWrapper<Value> {
    var value: Value
    subscript<T>(dynamicMember member: KeyPath<Value, T>) -> T {
        get { return value[keyPath: member] }
    }
}

let point = Point(x: 381, y: 431)
let wrapper = PassthroughWrapper(value: point)
print(wrapper.x)
```

### frozen

타입에 변경사항을 제한하기 위해 구조체 또는 열거형 선언에 이 속성을 적용합니다. 이 속성은 라이브러리 진화 모드 (library evolution mode) 로 컴파일 될 때만 허용됩니다. 이후 버전의 라이브러리는 열거형의 케이스 또는 구조체의 저장된 인스턴스 프로퍼티를 추가, 제거, 또는 재정렬로 선언을 변경할 수 없습니다. 이러한 변경은 고정되지 않은 타입 (nonfrozen types) 에서 허용되지만 고정된 타입 (frozen types) 에 대해 ABI 호환성을 깨뜨립니다.

> NOTE\
> 컴파일러가 라이브러리 진화 모드로 있지 않으면 모든 구조체와 열거형은 암시적으로 고정 (frozen) 이고 이 속성은 무시됩니다.

라이브러리 진화 모드에서 고정되지 않은 구조체와 열거형의 멤버와 상호작용하는 코드는 향후 버전의 라이브러리에서 해당 타입의 멤버 중 일부를 추가, 제거, 또는 재정렬 하더라도 다시 컴파일되지 않고 계속 작업할 수 있는 방식으로 컴파일됩니다. 컴파일러는 정보 검색 및 간접 계층 추가와 같은 기술을 사용하여 이를 가능하게 합니다. 구조체 또는 열거형을 고정으로 표시하면 성능을 얻기위해 이러한 유연성을 포기합니다: 라이브러리의 향후 버전은 타입을 제한적으로 변경할 수 있지만 컴파일러는 타입의 멤버와 상호작용하는 코드에서 추가 최적화를 수행할 수 있습니다.

고정된 타입, 고정된 구조체의 저장된 프로퍼티와 고정된 열거형 케이스의 연관된 값의 타입은 public 이거나 `usableFromInline` 속성으로 표시되어야 합니다. 고정된 구조체의 프로퍼티는 프로퍼티 관찰자를 가질 수 없고 저장된 인스턴스 프로퍼티에 대해 초기값을 제공하는 표현식은 [inlinable](attributes.md#inlinable) 에서 설명한대로 인라인 가능한 함수와 동일한 제한사항을 따릅니다.

커맨드 라인에서 라이브러리 진화 모드를 활성화 하려면 `-enable-library-evolution` 옵션을 Swift 컴파일러에 전달해야 합니다. Xcode 에서 가능하게 하려면 [Xcode 도움말 (Xcode Help)](https://help.apple.com/xcode/mac/current/#/dev04b3a04ba) 에서 설명한대로 "Build Libraries for Distribution" 빌드 설정 (`BUILD_LIBRARY_FOR_DISTRIBUTION`) 을 Yes 로 설정해야 합니다.

고정된 열거형에 대한 switch 구문은 [향후 열거형 케이스 전환 (Switching Over Future Enumeration Cases)](statements.md#switching-over-future-enumeration-cases) 에서 설명한대로 `default` 케이스를 요구하지 않습니다. 고정된 열거형을 전환할 때 `default` 또는 `@unknown default` 를 포함하면 해당 코드는 실행되지 않기 때문에 경고가 생성됩니다.

### GKInspectable

사용자 정의 GameplayKit 구성요소 프로퍼티를 SpriteKit 에디터 UI 에 노출하기 위해 이 속성을 적용합니다. 이 속성을 적용하면 `objc` 속성을 의미합니다.

### inlinable

모듈의 public 인터페이스의 부분으로 선언의 구현을 노출하기 위해 함수, 메서드, 계산된 프로퍼티, 서브 스크립트, 편리한 초기화 구문, 또는 초기화 해제 구문 선언에 이 속성을 적용합니다. 컴파일러는 호출 부분에서 기호의 구현을 복사본으로 인라인 가능한 기호로 호출을 대체할 수 있습니다.

인라인 가능한 코드는 `public` 기호로 선언된 모든 모듈에서 상호작용 할 수 있고 `usableFromInline` 속성으로 표시된 동일한 모듈에서 선언된 `internal` 기호와 상호작용 할 수 있습니다. 인라인 가능한 코드는 `private` 또는 `fileprivate` 기호와 상호작용 할 수 없습니다.

이 속성은 중첩된 내부 함수 선언 또는 `fileprivate` 또는 `private` 선언에 적용될 수 없습니다. 인라인 가능한 함수 내부에 선언된 함수와 클로저는 이 속성으로 표시될 수 없더라도 암시적으로 인라인 입니다.

### main

프로그램 흐름에 대해 최상위 시작 지점을 포함하는 것을 나타내기 위해 구조체, 클래스 또는 열거형 선언에 이 속성을 적용할 수 있습니다. 타입은 인수가 없고 `Void` 를 반환하는 `main` 타입 함수를 제공해야 합니다. 예를 들어:

```swift
@main
struct MyTopLevel {
    static func main() {
        // Top-level code goes here
    }
}
```

`main` 속성의 요구사항을 설명하는 또다른 방법은 이 속성을 작성하는 타입이 다음의 가상의 프로토콜을 준수하는 타입과 동일한 요구사항을 충족해야 한다는 것입니다:

```swift
protocol ProvidesMain {
    static func main() throws
}
```

실행 가능하도록 만들기 위해 컴파일 한 Swift 코드는 [최상위-수준 코드 (Top-Level Code)](declarations.md#top-level-code) 에서 설명한대로 최상위 시작점을 포함해야 합니다.

### nonobjc

암시적으로 `objc` 속성을 억제하기 위해 메서드, 프로퍼티, 서브 스크립트, 또는 초기화 구문 선언에 이 속성을 적용합니다. `nonobjc` 속성은 Objective-C 에서 표현 가능하더라도 Objective-C 코드로 선언이 불가능 하도록 컴파일러에게 말합니다.

확장에 이 속성을 적용하는 것은 명시적으로 `objc` 속성으로 표시되지 않은 확장의 모든 멤버에 같은 영향을 미칩니다.

`objc` 속성으로 표시된 클래스의 브릿징 메서드에 대한 순환성을 확인하고 `objc` 속성으로 표시된 클래스의 메서드와 초기화 구문에 오버로딩을 허용하기 위해 `nonobjc` 속성을 사용합니다.

`nonobjc` 속성으로 표시된 메서드는 `objc` 속성으로 표시된 메서드로 재정의할 수 없습니다. 그러나 `objc` 속성으로 표시된 메서드는 `nonobjc` 속성으로 표시된 메서드로 재정의할 수 있습니다. 유사하게 `nonobjc` 속성으로 표시된 메서드는 `objc` 속성으로 표시된 메서드에 대한 프로토콜 요구사항을 충족할 수 없습니다.

### NSApplicationMain

애플리케이션 대리자를 나타내기 위해 클래스에 이 속성을 적용합니다. 이 속성을 사용하는 것은 `NSApplicationMain(_:_:)` 함수를 호출하는 것과 동일합니다.

이 속성을 사용하지 않으면 다음과 같이 `NSApplicationMain(_:_:)` 함수를 호출하는 최상위의 코드로 `main.swift` 파일을 적용해야 합니다:

```swift
import AppKit
NSApplicationMain(CommandLine.argc, CommandLine.unsafeArgv)
```

실행 가능하도록 만들기 위해 컴파일 한 Swift 코드는 [최상위-수준 코드 (Top-Level Code)](declarations.md#top-level-code) 에서 설명한대로 하나의 최상위 시작 지점을 포함해야 합니다.

### NSCopying

클래스에 저장된 변수 프로퍼티에 이 속성을 적용합니다. 이 속성을 사용하면 프로퍼티의 값 자체 대신에 `copyWithZone(_:)` 메서드에 의해 반환된 프로퍼티의 값의 _복사본 (copy)_ 으로 프로퍼티의 setter 가 합성됩니다. 프로퍼티의 타입은 `NSCopying` 프로토콜을 준수해야 합니다.

`NSCopying` 속성은 Objective-C `copy` 프로퍼티 속성와 유사하게 동작합니다.

### NSManaged

코어 데이터 (Core Data) 가 연관된 엔티티 설명 기반으로 런타임 시 동적으로 구현을 제공하는 것을 나타내기 위해 `NSManagedObject` 를 상속하는 클래스의 인스턴스 메서드나 저장된 변수 프로퍼티에 이 속성을 적용합니다. `NSManaged` 속성으로 표시된 프로퍼티의 경우 코어 데이터 (Core Data) 는 런타임에 스토리지 (storage) 도 제공합니다. 이 속성을 적용하면 `objc` 속성도 의미합니다.

### objc

Objective-C 로 표현될 수 있는 모든 선언에 이 속성을 적용합니다. 예를 들어 중첩되지 않은 클래스, 프로토콜, 제너릭이 아닌 열거형 (정수 원시값 타입으로 제한), 클래스의 프로퍼티, 그리고 메서드 (getter 와 setter 포함), 프로토콜과 프로토콜의 옵셔널 멤버, 초기화 구문, 그리고 서브 스크립트 입니다. `objc` 속성은 선언이 Objective-C 코드에서 사용 가능함을 컴파일러에게 말합니다.

확장에 이 속성을 적용하는 것은 암시적으로 `nonobjc` 속성으로 표시되지 않은 확장의 모든 멤버에 적용됩니다.

컴파일러는 암시적으로 Objective-C 에 정의된 모든 클래스의 하위 클래스에 `objc` 속성을 추가합니다. 그러나 하위 클래스는 제너릭이 아니어야 하며 제너릭 클래스를 상속해선 안됩니다. 아래에서 설명한대로 Objective-C 이름을 지정하기 위해 이러한 기준을 충족하는 하위 클래스에 `objc` 속성을 명시적으로 추가할 수 있습니다. `objc` 속성으로 표시된 프로토콜은 이 속성이 표시되지 않은 프로토콜을 상속할 수 없습니다.

`objc` 속성은 다음과 같은 경우에 암시적으로 추가됩니다:

* 선언이 하위 클래스의 재정의이고 하위 클래스의 선언이 `objc` 속성을 가지고 있습니다.
* 선언이 `objc` 속성을 가지는 프로토콜의 요구사항을 충족합니다.
* 선언이 `IBAction`, `IBSegueAction`, `IBOutlet`, `IBDesignable`, `IBInspectable`, `NSManaged`, 또는 `GKInspectable` 속성을 가지고 있습니다.

열거형에 objc 속성을 적용하면 각 열거형 케이스는 열거형 이름과 케이스 이름의 연결로 Objective-C 코드에 노출됩니다. 케이스 이름의 첫번째 문자는 대문자입니다. 예를 들어 Swift `Planet` 열거형에서 명명된 케이스 `venus` 는 명명된 케이스 `PlanetVenus` 로 Objective-C 코드로 노출됩니다.

`objc` 속성은 식별자로 구성된 단일 속성 인수를 선택적으로 허용합니다. 식별자는 `objc` 속성을 적용하는 엔티티에 대해 Objective-C 로 노출될 이름을 지정합니다. 클래스, 열거형, 열거형 케이스, 프로토콜, 메서드, getter, setter, 그리고 초기화 구문 이름으로 이 인수를 사용할 수 있습니다. 클래스, 프로토콜, 또는 열거형에 대해 Objective-C 이름을 지정하는 경우 [Objective-C 를 사용한 프로그래밍 (Programming with Objective-C)](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html#//apple\_ref/doc/uid/TP40011210) 에 [규칙 (Conventions)](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Conventions/Conventions.html#//apple\_ref/doc/uid/TP40011210-CH10-SW1) 에서 설명한대로 세글자의 접두사를 포함합니다. 아래 예제는 프로퍼티 자체의 이름이 아닌 `isEnabled` 로 Objective-C 에 `ExampleClass` 의 `enabled` 프로퍼티에 대한 getter 를 노출합니다.

```swift
class ExampleClass: NSObject {
    @objc var enabled: Bool {
        @objc(isEnabled) get {
            // Return the appropriate value
        }
    }
}
```

더 자세한 내용은 [Objective-C 에 Swift 가져오기 (Importing Swift into Objective-C)](https://developer.apple.com/documentation/swift/imported\_c\_and\_objective-c\_apis/importing\_swift\_into\_objective-c) 를 참고 바랍니다.

> NOTE\
> objc 속성에 인수는 해당 선언에 대한 런타임 이름도 변경할 수 있습니다. [`NSClassFromString(_:)`](https://developer.apple.com/documentation/foundation/1395135-nsclassfromstring) 과 같이 Objective-C 런타임과 상호작용하는 함수를 호출할 때와 앱의 info.plist 파일의 클래스 이름을 지정할 때 런타임 이름을 사용합니다. 인수를 전달하여 이름을 지정하면 해당 이름은 Objective-C 코드에서 이름과 런타임 이름으로 사용됩니다. 인수를 생략하면 Objective-C 코드에서 사용된 이름은 Swift 코드의 이름과 일치하고 런타임 이름은 일반 Swift 컴파일러 이름 변경 규칙을 따릅니다.

### objcMembers

클래스 선언에 이 속성을 적용하여 암시적으로 `objc` 속성을 클래스의 모든 Objective-C 호환 멤버, 확장, 하위 클래스, 그리고 모든 확장의 하위 클래스에 적용합니다.

대부분의 코드는 필요한 선언만 노출시키기 위해 `objc` 속성을 대신 사용합니다. 많은 선언의 노출이 필요하다면 `objc` 속성을 가지는 확장에 그룹화 할 수 있습니다. `objcMembers` 속성은 Objective-C 런타임의 내부 기능을 많이 사용하는 라이브러리에 대해 편리합니다. 필요하지 않은 곳에 `objc` 속성을 적용하면 바이너리 크기를 증가시키고 성능에 부정적인 영향을 미칠 수 있습니다.

### propertyWrapper

프로퍼티 래퍼 (protperty wrapper) 로 해당 타입을 사용하기 위해 클래스, 구조체, 또는 열거형 선언에 이 속성을 적용합니다. 타입에 이 속성을 적용하면 타입과 동일한 이름으로 사용자 정의 속성을 생성합니다. 래퍼 타입의 인스턴스로 프로퍼티에 대한 접근을 래핑하려면 클래스, 구조체, 또는 열거형의 프로퍼티에 새로운 속성을 적용해야 합니다. 지역과 전역 변수는 프로퍼티 래퍼를 사용할 수 없습니다.

래퍼는 `wrappedValue` 인스턴스 프로퍼티를 정의해야 합니다. 프로퍼티의 _래핑된 값 (wrapped value)_ 은 이 프로퍼티에 대해 getter 와 setter 를 노출하는 값입니다. 대부분의 경우 `wrappedValue` 는 계산된 값이지만 대신 저장된 값이 될 수 있습니다. 래퍼는 래핑된 값에 필요한 기본 저장소를 정의하고 관리합니다. 컴파일러는 래핑된 프로퍼티의 이름 앞에 언더바 (`_`) 로 래퍼 타입의 인스턴스에 대해 저장소를 합성합니다. 예를 들어 `someProperty` 에 대한 래퍼는 `_someProperty` 로 저장됩니다. 래퍼에 대한 합성된 저장소는 `private` 의 접근 제어 수준을 가집니다.

프로퍼티 래퍼를 가지는 프로퍼티는 `willSet` 과 `didSet` 블럭을 포함할 수 있지만 컴파일러가 합성한 `get` 또는 `set` 블럭을 재정의할 수 없습니다.

Swift 는 프로퍼티 래퍼의 초기화에 대해 두가지의 구문 설탕 (syntactic sugar) 을 제공합니다. 래핑된 값의 정의에 할당 구문을 사용하여 할당의 오른쪽에 있는 표현식을 프로퍼티 래퍼에 초기화 구문의 `wrappedValue` 파라미터에 대한 인수로 전달할 수 있습니다. 프로퍼티에 속성을 적용할 때 속성에 인수를 제공할 수도 있으며 이 인수는 프로퍼티 래퍼의 초기화 구문으로 전달됩니다. 예를 들어 아래 코드에서 `SomeStruct` 는 `SomeWrapper` 가 정의한 각 초기화 구문을 호출합니다.

```swift
@propertyWrapper
struct SomeWrapper {
    var wrappedValue: Int
    var someValue: Double
    init() {
        self.wrappedValue = 100
        self.someValue = 12.3
    }
    init(wrappedValue: Int) {
        self.wrappedValue = wrappedValue
        self.someValue = 45.6
    }
    init(wrappedValue value: Int, custom: Double) {
        self.wrappedValue = value
        self.someValue = custom
    }
}

struct SomeStruct {
    // Uses init()
    @SomeWrapper var a: Int

    // Uses init(wrappedValue:)
    @SomeWrapper var b = 10

    // Both use init(wrappedValue:custom:)
    @SomeWrapper(custom: 98.7) var c = 30
    @SomeWrapper(wrappedValue: 30, custom: 98.7) var d
}
```

래핑된 프로퍼티에 대한 _계획된 값 (projected value)_ 은 프로퍼티 래퍼가 추가 기능을 노출하기 위해 사용할 수 있는 두번째 값입니다. 프로퍼티 래퍼 타입의 작성자는 계획된 값의 의미를 결정하고 계획된 값이 노출하는 인터페이스를 정의할 책임이 있습니다. 프로퍼티 래퍼에서 값을 계획하려면 래퍼 타입에 `projectedValue` 인스턴스 프로퍼티를 정의합니다. 컴파일러는 래핑된 프로퍼티의 이름 앞에 달러 기호 (`$`) 로 계획된 값에 대한 식별자를 합성합니다. 예를 들어 `someProperty` 에 대한 계획된 값은 `$someProperty` 입니다. 계획된 값은 기존의 래핑된 프로퍼티와 동일한 접근 제어 수준을 가집니다.

```swift
@propertyWrapper
struct WrapperWithProjection {
    var wrappedValue: Int
    var projectedValue: SomeProjection {
        return SomeProjection(wrapper: self)
    }
}
struct SomeProjection {
    var wrapper: WrapperWithProjection
}

struct SomeStruct {
    @WrapperWithProjection var x = 123
}
let s = SomeStruct()
s.x           // Int value
s.$x          // SomeProjection value
s.$x.wrapper  // WrapperWithProjection value
```

### resultBuilder

결과 빌더 (result builder) 로 타입을 사용하기 위해 클래스, 구조체, 열거형에 이 속성을 적용합니다. _결과 빌더 (result builder)_ 는 데이터 구조체를 단계별로 빌드하는 타입입니다. 자연스럽고 선언적인 방법으로 중첩된 데이터 구조체를 생성하기 위해 도메인-특정 언어 (DSL) 를 구현하기 위해 결과 빌더를 사용합니다. `resultBuilder` 속성을 어떻게 사용하는지에 대한 예제는 [결과 빌더 (Result Builders)](../language-guide-1/advanced-operators.md#result-builders) 를 참고 바랍니다.

#### 결과-빌딩 메서드 (Result-Building Methods)

결과 빌더는 아래 설명한대로 정적 메서드를 구현합니다. 결과 빌더의 모든 기능은 정적 메서드를 통해 노출되므로 해당 타입의 인스턴스를 초기화 하지 않습니다. `buildBlock(_:)` 메서드는 필수입니다. DSL 에서 추가 기능을 활성화 하는 다른 메서드는 옵셔널 입니다. 결과 빌더 타입의 선언은 프로토콜 준수를 포함할 필요가 없습니다.

정적 메서드의 설명은 기호로 세가지 타입을 사용합니다. `Expression` 타입은 결과 빌더의 입력의 타입에 대한 기호이고 `Component` 는 부분 결과의 타입에 대한 기호이며 `FinalResult` 는 결과 빌더가 생성하는 결과의 타입에 대한 기호입니다. 이러한 타입을 결과 빌더가 사용하는 실제 타입으로 바꿉니다. 결과-빌딩 메서드가 `Expression` 또는 `FinalResult` 에 대한 타입을 지정하지 않으면 `Component` 와 기본적으로 동일합니다.

결과-빌딩 메서드는 다음과 같습니다:

`static func buildBlock(_ components: Component...) -> Component` \
부분 결과의 배열을 단일 부분 결과로 결합합니다. 결과 빌더는 이 메서드를 구현해야 합니다.

`static func buildOptional(_ component: Component?) -> Component` \
`nil` 이 가능한 부분 결과로 부터 부분 결과를 빌드합니다. `else` 절을 포함하지 않은 `if` 구문을 지원하려면 이 메서드를 구현해야 합니다.

`static func buildEither(first: Component) -> Component` \
일부 조건에 따라 다양한 값의 부분 결과를 빌드합니다. `switch` 구문과 `else` 절을 포함하는 `if` 구문을 제공하려면 이 메서드와 `buildEither(second:)` 를 모두 구현해야 합니다.

`static func buildEither(second: Component) -> Component` \
일부 조건에 따라 다양한 값의 부분 결과를 빌드합니다. `switch` 구문과 `else` 절을 포함하는 `if` 구문을 제공하려면 이 메서드와 `buildEither(first:)` 를 모두 구현해야 합니다.

`static func buildArray(_ components: [Component]) -> Component` \
부분 결과의 배열로 부터 부분 결과를 빌드합니다. `for` 루프를 지원하려면 이 메서드를 구현해야 합니다.

`static func buildExpression(_ expression: Expression) -> Component` \
표현식에서 부분 결과를 빌드합니다. 이 메서드를 구현하여 전처리 — 예를 들어 표현식을 내부 타입으로 변환 — 를 수행하거나 사용하는 곳에서 타입 추론을 위한 추가 정보를 제공하기 위해 구현할 수 있습니다.

`static func buildFinalResult(_ component: Component) -> FinalResult` \
부분 결과로 부터 최종 결과를 빌드합니다. 부분과 최종 결과에 대한 다른 타입을 사용하는 결과 빌더의 부분으로 이 메서드를 구현하거나 결과를 반환하기 전에 결과에 대해 다른 후처리를 진행하기 위해 이 메서드를 구현할 수 있습니다.

`static func buildLimitedAvailability(_ component: Component) -> Component` \
가용성 검사를 수행하는 컴파일러-제어 구문의 외부에서 타입 정보를 전파하거나 지우는 부분 결과를 빌드합니다. 조건부 간의 다양한 타입 정보를 지우기 위해 사용할 수 있습니다.

예를 들어 아래 코드는 정수의 배열을 빌드하는 간다한 결과 빌더를 정의합니다. 이 코드는 타입 별칭으로 `Component` 와 `Expression` 을 정의하여 아래 예제를 위의 메서드의 리스트보다 쉽게 일치하도록 만듭니다.

```swift
@resultBuilder
struct ArrayBuilder {
    typealias Component = [Int]
    typealias Expression = Int
    static func buildExpression(_ element: Expression) -> Component {
        return [element]
    }
    static func buildOptional(_ component: Component?) -> Component {
        guard let component = component else { return [] }
        return component
    }
    static func buildEither(first component: Component) -> Component {
        return component
    }
    static func buildEither(second component: Component) -> Component {
        return component
    }
    static func buildArray(_ components: [Component]) -> Component {
        return Array(components.joined())
    }
    static func buildBlock(_ components: Component...) -> Component {
        return Array(components.joined())
    }
}
```

#### 결과 변환 (Result Transformations)

다음 구문 변환은 결과-빌더 구문을 사용하는 코드에서 결과 빌더 타입의 정적 메서드를 호출하는 코드로 변환하기 위해 재귀적으로 적용됩니다:

* 결과 빌더가 `buildExpression(_:)` 메서드를 가지면 각 표현식은 해당 메서드에 대한 호출이 됩니다. 이 변환은 항상 첫번째 입니다. 예를 들어 다음의 선언은 동등합니다:

```swift
@ArrayBuilder var builderNumber: [Int] { 10 }
var manualNumber = ArrayBuilder.buildExpression(10)
```

* 할당 구문은 표현식 처럼 변환되지만 `()` 으로 평가되는 것으로 이해됩니다. 구체적으로 할당을 처리하기 위해 `()` 타입의 인수를 가지는 `buildExpression(_:)` 의 오버로드를 정의할 수 있습니다.
* 가용성 조건을 확인하는 분기 구문은 `buildLimitedAvailability(_:)` 메서드에 대한 호출이 됩니다. 이 변환은 `buildEither(first:)`, `buildEither(second:)`, 또는 `buildOptional(_:)` 에 대한 호출로 변환되기 전에 발생합니다. `buildLimitedAvailability(_:)` 메서드를 사용하여 어떤 분기를 사용하는지에 따라 변경되는 타입 정보를 지웁니다. 예를 들어 아래의 `buildEither(first:)` 와 `buildEither(second:)` 메서드는 두 분기에 대한 타입 정보를 캡처하는 제너릭 타입을 사용합니다.

```swift
protocol Drawable {
    func draw() -> String
}
struct Text: Drawable {
    var content: String
    init(_ content: String) { self.content = content }
    func draw() -> String { return content }
}
struct Line<D: Drawable>: Drawable {
    var elements: [D]
    func draw() -> String {
        return elements.map { $0.draw() }.joined(separator: "")
    }
}
struct DrawEither<First: Drawable, Second: Drawable>: Drawable {
    var content: Drawable
    func draw() -> String { return content.draw() }
}

@resultBuilder
struct DrawingBuilder {
    static func buildBlock<D: Drawable>(_ components: D...) -> Line<D> {
        return Line(elements: components)
    }
    static func buildEither<First, Second>(first: First)
        -> DrawEither<First, Second> {
            return DrawEither(content: first)
    }
    static func buildEither<First, Second>(second: Second)
        -> DrawEither<First, Second> {
            return DrawEither(content: second)
    }
}
```

그러나 이 접근 방식은 가용성 검사가 있는 코드에서 문제를 야기합니다:

```swift
@available(macOS 99, *)
struct FutureText: Drawable {
    var content: String
    init(_ content: String) { self.content = content }
    func draw() -> String { return content }
}
@DrawingBuilder var brokenDrawing: Drawable {
    if #available(macOS 99, *) {
        FutureText("Inside.future")  // Problem
    } else {
        Text("Inside.present")
    }
}
// The type of brokenDrawing is Line<DrawEither<Line<FutureText>, Line<Text>>>
```

위의 코드에서 `FutureText` 는 `DrawEither` 제너릭 타입에서 타입 중 하나이므로 `brokenDrawing` 의 타입의 부분으로 나타납니다. 이로 인해 런타임에 `FutureText` 를 사용할 수 없는 경우 해당 타입이 명시적으로 사용되지 않는 경우에도 프로그램이 중된될 수 있습니다.

이 문제를 해결하려면 타입 정보를 지우기 위해 `buildLimitedAvailability(_:)` 메서드를 구현합니다. 예를 들어 아래의 코드는 가용성 검사로 부터 `AnyDrawable` 값을 빌드합니다.

```swift
struct AnyDrawable: Drawable {
    var content: Drawable
    func draw() -> String { return content.draw() }
}
extension DrawingBuilder {
    static func buildLimitedAvailability(_ content: Drawable) -> AnyDrawable {
        return AnyDrawable(content: content)
    }
}

@DrawingBuilder var typeErasedDrawing: Drawable {
    if #available(macOS 99, *) {
        FutureText("Inside.future")
    } else {
        Text("Inside.present")
    }
}
// The type of typeErasedDrawing is Line<DrawEither<AnyDrawable, Line<Text>>>
```

* 분기 구문은 `buildEither(first:)` 와 `buildEither(second:)` 메서드에 대한 일련의 중첩된 호출이 됩니다. 구문의 조건과 케이스는 이진 트리의 잎 노드에 매핑되고 구문은 루트 노드에서 잎 노드로의 경로를 따라 `buildEither` 메서드에 대한 중첩된 호출이 됩니다.

예를 들어 세가지 케이스가 있는 switch 구문을 작성하면 컴파일러는 세개의 잎 노드로 이진 트리를 사용합니다. 마찬가지로 루트 노드에서 두번째 케이스까지의 경로는 "second child" 이고 "first child" 이므로 `buildEither(first: buildEither(second: ... ))` 와 같이 중첩된 호출이 됩니다. 다음의 선언은 동일합니다:

```swift
let someNumber = 19
@ArrayBuilder var builderConditional: [Int] {
    if someNumber < 12 {
        31
    } else if someNumber == 19 {
        32
    } else {
        33
    }
}

var manualConditional: [Int]
if someNumber < 12 {
    let partialResult = ArrayBuilder.buildExpression(31)
    let outerPartialResult = ArrayBuilder.buildEither(first: partialResult)
    manualConditional = ArrayBuilder.buildEither(first: outerPartialResult)
} else if someNumber == 19 {
    let partialResult = ArrayBuilder.buildExpression(32)
    let outerPartialResult = ArrayBuilder.buildEither(second: partialResult)
    manualConditional = ArrayBuilder.buildEither(first: outerPartialResult)
} else {
    let partialResult = ArrayBuilder.buildExpression(33)
    manualConditional = ArrayBuilder.buildEither(second: partialResult)
}
```

* `else` 절 없는 `if` 구문과 같이 값을 생성하지 않을 분기 구문은 `buildOptional(_:)` 에 대한 호출이 됩니다. `if` 구문의 조건이 충족되면 코드 블럭은 변환되고 인수로 전달됩니다; 그렇지 않으면 `buildOptional(_:)` 은 인수로 `nil` 가지고 호출됩니다. 예를 들어 다음의 선언은 동일합니다:

```swift
@ArrayBuilder var builderOptional: [Int] {
    if (someNumber % 2) == 1 { 20 }
}

var partialResult: [Int]? = nil
if (someNumber % 2) == 1 {
    partialResult = ArrayBuilder.buildExpression(20)
}
var manualOptional = ArrayBuilder.buildOptional(partialResult)
```

* 코드 블럭 또는 `do` 구문은 `buildBlock(_:)` 메서드에 대한 호출이 됩니다. 블럭 내부의 각 구문은 한번에 하나씩 변환되고 `buildBlock(_:)` 메서드에 대한 인수가 됩니다. 예를 들어 다음의 선언은 동일합니다:

```swift
@ArrayBuilder var builderBlock: [Int] {
    100
    200
    300
}

var manualBlock = ArrayBuilder.buildBlock(
    ArrayBuilder.buildExpression(100),
    ArrayBuilder.buildExpression(200),
    ArrayBuilder.buildExpression(300)
)
```

* `for` 루프는 임시 변수, `for` 루프, 그리고 `buildArray(_:)` 메서드에 대한 호출이 됩니다. 새로운 `for` 루프는 시퀀스를 반복하고 각 부분 결과를 해당 배열에 추가합니다. 임시 배열은 `buildArray(_:)` 호출에 인수로 전달됩니다. 예를 들어 다음의 선언은 동일합니다:

```swift
@ArrayBuilder var builderArray: [Int] {
    for i in 5...7 {
        100 + i
    }
}

var temporary: [[Int]] = []
for i in 5...7 {
    let partialResult = ArrayBuilder.buildExpression(100 + i)
    temporary.append(partialResult)
}
let manualArray = ArrayBuilder.buildArray(temporary)
```

* 결과 빌더가 `buildFinalResult(_:)` 메서드를 가지고 있으면 최종 결과는 해당 메서드에 대한 호출이 됩니다. 이 변환은 항상 마지막 입니다.

변환 동작은 임시 변수로 설명되지만 결과 빌더를 사용하는 것은 나머지 코드에서 볼 수 있는 새로운 선언을 생성하지 않습니다.

결과 빌더 변환 코드에서 `break`, `continue`, `defer`, `guard`, 또는 `return` 구문, `while` 구문, 또는 `do`-`catch` 구문을 사용할 수 없습니다.

변환 프로세스는 코드에서 선언을 변경하지 않으므로 임시 상수와 변수를 사용하여 부분적으로 표현식을 작성할 수 있습니다. `throw` 구문, 컴파일-시간 진단 구문, 또는 `return` 구문이 포함된 클로저도 변경하지 않습니다.

가능할 때마다 변환이 통합됩니다. 예를 들어 표현식 `4 + 5 * 6` 은 해당 함수를 여러번 호출하는 대신 `buildExpression(4 + 5 * 6)` 으로 됩니다. 마찬가지로 중첩된 분기 구문은 `buildEither` 메서드에 대한 호출의 단일 이진 트리가 됩니다.

#### 사용자 정의 결과-빌더 속성 (Custom Result-Builder Attributes)

결과 빌더 타입을 생성하면 동일한 이름의 사용자 정의 속성을 생성합니다. 다음 위치에 해당 속성을 적용할 수 있습니다:

* 함수 선언에서 결과 빌더는 함수의 본문을 빌드합니다.
* getter 를 포함하는 변수 또는 서브 스크립트 선언에서 결과 빌더는 getter 의 본문을 빌드합니다.
* 함수 선언의 파라미터에서 결과 빌더는 해당 인수로 전달되는 클로저의 본문을 빌드합니다.

결과 빌더 속성을 적용하는 것은 ABI 호환성에 영향을 주지 않습니다. 파라미터에 결과 빌더 속성을 적용하는 것은 해당 속성이 함수의 인터페이스의 부분으로 소스 호환성에 영향을 줄 수 있습니다.

### requires\_stored\_property\_inits

선언의 일부로 기본값을 제공하기 위해 클래스 내에 모든 저장된 프로퍼티를 요구하기 위해 클래스 선언에 이 속성을 적용합니다. 이 속성은 `NSManagedObject` 에서 상속한 모든 클래스에 대해 유추됩니다.

### testable

테스팅 모듈의 코드를 단순화 하는 접근 제어에 대한 변경으로 해당 모듈을 가져오기 위해 `import` 선언에 이 속성을 적용합니다. `internal` 접근-수준 수식어로 표시된 가져온 모듈의 엔티티는 `public` 접근-수준 수식어로 선언된 경우에 가져옵니다. `internal` 또는 `public` 접근-수준 수식어로 표시된 클래스와 클래스 멤버는 `open` 접근-수준 수식어로 선언된 경우에 가져옵니다. 가져온 모듈은 테스트가 활성화 된 상태로 컴파일되어야 합니다.

### UIApplicationMain

애플리케이션 대리자를 나타내기 위해 클래스에 이 속성을 적용합니다. 이 속성을 사용하는 것은 `UIApplicationMain` 함수를 호출하는 것과 대리자 클래스의 이름으로 클래스의 이름을 전달하는 것과 같습니다.

이 속성을 사용하지 않으면 [`UIApplicationMain(_:_:_:_:)`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain) 함수를 호출하는 최상위 수준의 코드를 가지는 `main.swift` 파일을 제공해야 합니다. 예를 들어 주 클래스로 `UIApplication` 의 사용자 정의 하위 클래스를 사용하면 이 속성을 사용하는 것 대신에 `UIApplicationMain(_:_:_:_:)` 함수를 호출합니다.

실행 가능하도록 만들기 위해 컴파일 한 Swift 코드는 [최상위-수준 코드 (Top-Level Code)](declarations.md#top-level-code) 에서 설명한대로 하나의 최상위-수준 시작 지점을 포함해야 합니다.

### unchecked

프로토콜의 요구사항의 강제성을 끄기위해 타입 선언 리스트의 적용된 프로토콜 타입에 이 속성을 적용합니다.

지원하는 프로토콜은 [전달 가능 \(Sendable\)](https://developer.apple.com/documentation/swift/sendable) 만 있습니다.

### usableFromInline

선언과 동일한 모듈에 정의된 인라인 가능 코드에서 해당 기호를 사용할 수 있도록 하기 위해 함수, 메서드, 계산된 프로퍼티, 서브 스크립트, 초기화 구문, 또는 초기화 해제 구문 선언에 이 속성을 적용합니다. 선언은 `internal` 접근 수준 수식어를 가지고 있어야 합니다. `usableFromInline` 으로 표시된 구조체나 클래스는 프로퍼티에 대해 public 또는 `usableFromInline` 인 타입만 사용할 수 있습니다. `usableFromInline` 으료 표시된 열거형은 케이스의 원시값과 연관된 값에 대해 public 또는 `usableFromInline` 인 타입만 사용할 수 있습니다.

`public` 접근 수준 수식어와 같이 이 속성은 모듈의 공개 인터페이스의 부분으로 선언을 노출합니다. `public` 과 다르게 컴파일러는 선언의 기호를 내보내더라도 모듈 외부의 코드에서 이름으로 참조되기 위해 `usableFromInline` 으로 표시된 선언을 허용하지 않습니다. 그러나 모듈 외부의 코드는 런타임 동작을 사용하여 선언의 기호와 상호작용 할 수 있습니다.

`inlinable` 속성으로 표시된 선언은 암시적으로 인라인 가능한 코드에서 사용가능 합니다. `inlinable` 또는 `usableFromInline` 은 `internal` 선언에 적용될 수 있지만 두 속성 모두 적용하는 것은 에러가 발생합니다.

### warn\_unqualified\_access

해당 함수 또는 메서드가 모듈 이름, 타입 이름, 또는 인스턴스 변수 또는 상수와 같이 선행 한정자 없이 사용될 때 경고를 나타내기 위해 최상위-수준 함수, 인스턴스 메서드, 또는 클래스 또는 정적 메서드에 이 속성을 적용합니다. 동일한 범위에서 접근할 수 있는 동일한 이름을 가지는 함수 간의 모호성을 방지하기 위해 이 속성을 사용합니다.

예를 들어 Swift 표준 라이브러리는 최상위-수준 [`min(_:_:)`](https://developer.apple.com/documentation/swift/1538339-min/) 함수와 비교 가능한 요소가 있는 시퀀스에 대한 [`min()`](https://developer.apple.com/documentation/swift/sequence/1641174-min) 메서드 모두 포함합니다. 시퀀스 메서드는 Sequence 확장 내에서 하나 또는 다른 것을 사용하려고 할 때 혼동을 줄이기 위해 `warn_unqualified_access` 속성으로 선언됩니다.

### 인터페이스 빌더에 사용되는 선언 속성 (Declaration Attributes Used by Interface Builder)

인터페이스 빌더 (Interface Builder) 속성은 Xcode 와 동기화 하기위해 인터페이스 빌더에서 사용되는 선언 속성입니다. Swift 는 다음의 인터페이스 빌더 속성을 제공합니다: `IBAction`, `IBSegueAction`, `IBOutlet`, `IBDesignable`, 그리고 `IBInspectable`. 이 속성은 개념적으로 Objective-C 와 동일합니다.

클래스의 프로퍼티 선언에 `IBOutlet` 과 `IBInspectable` 속성을 적용합니다. 클래스의 메서드 선언에 `IBAction` 과 `IBSegueAction` 속성을 클래스 선언에 `IBDesignable` 속성을 적용합니다.

`IBAction`, `IBSegueAction`, `IBOutlet`, `IBDesignable`, 또는 `IBInspectable` 속성을 적용하는 것은 `objc` 속성을 의미합니다.

## 타입 속성 (Type Attributes)

타입에만 타입 속성 (type attributes) 를 적용할 수 있습니다.

### autoclosure

인수 없이 해당 표현식을 클로저에 자동으로 래핑하여 표현식의 평가를 지연하기 위해 이 속성을 적용합니다. 메서드 또는 함수 선언에 파라미터의 타입에 인수를 사용하지 않고 표현식의 타입에 값을 반환하는 함수 타입 인 파라미터에 적용합니다. `autoclosure` 속성을 어떻게 사용하는지에 대한 예제는 [자동 클로저 (Autoclosures) ](../language-guide-1/closures.md#autoclosures) 와 [함수 타입 (Function Type)](types.md#function-type) 을 참고 바랍니다.

### convention

호출 규칙 (calling conventions) 을 나타내기 위해 함수의 타입에 이 속성을 적용합니다.

`convention` 속성은 항상 다음의 인수 중 하나에 나타납니다:

* `swift` 인수는 Swift 함수 참조를 나타냅니다. 이것은 Swift 에서 함수 값에 대한 표준 호출 규칙입니다.
* `block` 인수는 Objective-C 호환 블럭 참조를 나타냅니다. 함수 값은 객체 내에 호출 함수 (invocation function) 를 포함하는 `id`-호환성 Objective-C 객체 인 블럭 객체에 대한 참조로 표시됩니다. 호출 함수는 C 호출 규칙을 사용합니다.
* `c` 인수는 C 함수 참조를 나타냅니다. 함수 값은 컨텍스트를 전달하지 않으며 C 호출 규칙을 사용합니다.

몇가지 예외를 제외하고 모든 호출 규칙의 함수는 다른 호출 규칙이 필요할 때 사용될 수 있습니다. 제너릭이 아닌 전역 함수, 지역 변수를 캡처하지 않는 지역 함수 또는 지역 변수를 캡처하지 않는 클로저는 C 호출 규칙으로 변환될 수 있습니다. 다른 Swift 함수는 C 호출 규칙으로 변환될 수 없습니다. Objective-C 블럭 호출 규칙을 가지는 함수는 C 호출 규칙으로 변환될 수 없습니다.

### escaping

나중에 실행하기 위해 파라미터의 값이 저장될 수 있음을 나타내기 위해 메서드 또는 함수 선언의 파라미터 타입에 이 속성을 적용합니다. 이는 값이 호출 수명보다 오래 지속될 수 있음을 의미합니다. `escaping` 타입 속성을 가지는 함수 타입 파라미터는 프로퍼티나 메서드에 대해 `self.` 의 명시적 사용을 요구합니다. `escaping` 속성을 어떻게 사용하는지에 대한 예제는 [탈출 클로저 (Escaping Closures)](../language-guide-1/closures.md#escaping-closures) 를 참고 바랍니다.

### Sendable

함수 또는 클로저가 전송 가능하다는 것을 나타내기 위해 함수의 타입에 이 속성을 적용합니다. 함수 타입에 이 속성을 적용하면 비함수 \(non-function\) 타입이 [전달 가능 \(Sendable\)](https://developer.apple.com/documentation/swift/sendable) 프로토콜을 준수하는 것과 같은 의미를 가집니다.

함수 또는 클로저가 전송 가능한 값을 예상하는 컨텍스트에서 사용되고 함수 또는 클로저가 전송 가능한 요구사항을 충족하면 이 속성은 함수와 클로저에서 유추됩니다.

전송 가능한 함수 타입은 전송 불가능한 함수 타입의 서브타입입니다.

## Switch 케이스 속성 (Switch Case Attributes)

switch 케이스에만 switch 케이스 속성을 적용할 수 있습니다.

### unknown

코드가 컴파일 될 때 알려진 열거형의 케이스와 일치하지 않는 것으로 예상됨을 나타내기 위해 switch 케이스에 이 속성을 적용합니다. `unknown` 속성을 어떻게 사용하는지에 대한 예제는 [향후 열거형 케이스 전환 (Switching Over Future Enumeration Cases)](statements.md#switching-over-future-enumeration-cases) 을 참고 바랍니다.

> Grammar of an attribute:
>
> *attribute* → **`@`** *attribute-name* *attribute-argument-clause*_?_
>
> *attribute-name* → *identifier*
>
> *attribute-argument-clause* → **`(`** *balanced-tokens*_?_ **`)`**
>
> *attributes* → *attribute* *attributes*_?_
>
>
>
> *balanced-tokens* → *balanced-token* *balanced-tokens*_?_
>
> *balanced-token* → **`(`** *balanced-tokens*_?_ **`)`**
>
> *balanced-token* → **`[`** *balanced-tokens*_?_ **`]`**
>
> *balanced-token* → **`{`** *balanced-tokens*_?_ **`}`**
>
> *balanced-token* → Any identifier, keyword, literal, or operator
>
> *balanced-token* → Any punctuation except  **`(`**,  **`)`**,  **`[`**,  **`]`**,  **`{`**, or  **`}`**
