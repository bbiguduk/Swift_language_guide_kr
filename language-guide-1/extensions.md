# 확장 \(Extensions\)

<!--
Extensions add new functionality to an existing class, structure, enumeration, or protocol type. This includes the ability to extend types for which you don’t have access to the original source code (known as retroactive modeling). Extensions are similar to categories in Objective-C. (Unlike Objective-C categories, Swift extensions don’t have names.)

Extensions in Swift can:

* Add computed instance properties and computed type properties
* Define instance methods and type methods
* Provide new initializers
* Define subscripts
* Define and use new nested types
* Make an existing type conform to a protocol

In Swift, you can even extend a protocol to provide implementations of its requirements or add additional functionality that conforming types can take advantage of. For more details, see Protocol Extensions.
-->

_확장 \(Extensions\)_ 확장한 클래스, 구조체, 열거형, 또는 프로토콜 타입에 새로운 기능을 추가합니다. 이것은 기존 소스 코드에 접근 권한이 없는 타입을 확장하는 기능이 포함됩니다 \(_소급 모델링 \(retroactive modeling\)_ 이라고 함\). 확장은 Objective-C에서 카테고리와 유사합니다 \(Objective-C 카테고리와 달리 Swift 확장은 이름이 없습니다\).

Swift에서 확장은 다음을 수행할 수 있습니다:

* 계산된 인스턴스 프로퍼티와 계산된 타입 프로퍼티 추가
* 인스턴스 메서드와 타입 메서드 정의
* 새로운 초기화 구문 제공
* 서브 스크립트 정의
* 새로운 중첩된 타입 정의와 사용
* 기존 타입이 프로토콜을 준수하도록 함

Swift에서 프로토콜을 확장하여 그것의 요구사항의 구현을 제공하거나 준수하는 타입의 기능을 추가할 수도 있습니다. 더 자세한 내용은 [프로토콜 확장 \(Protocol Extensions\)](protocols.md#protocol-extensions) 를 참고 바랍니다.

<!--
NOTE
Extensions can add new functionality to a type, but they can’t override existing functionality.
-->

> NOTE   
> 확장은 타입에 새로운 기능을 추가할 수 있지만 기존 기능을 재정의 할 수는 없습니다.

## 확장 구문 \(Extension Syntax\)

<!--
Declare extensions with the extension keyword:
-->

`extension` 키워드를 사용하여 확장을 선언합니다:

```swift
extension SomeType {
    // new functionality to add to SomeType goes here
}
```

<!--
An extension can extend an existing type to make it adopt one or more protocols. To add protocol conformance, you write the protocol names the same way as you write them for a class or structure:
-->

확장은 하나 이상의 프로토콜을 채택하여 기존 타입을 확장할 수 있습니다. 프로토콜 준수를 추가하려면 클래스 또는 구조체에 대해 작성하는 것과 동일하게 프로토콜 이름을 작성합니다:

```swift
extension SomeType: SomeProtocol, AnotherProtocol {
    // implementation of protocol requirements goes here
}
```

<!--
Adding protocol conformance in this way is described in Adding Protocol Conformance with an Extension.

An extension can be used to extend an existing generic type, as described in Extending a Generic Type. You can also extend a generic type to conditionally add functionality, as described in Extensions with a Generic Where Clause.
-->

이러한 방법에 대한 자세한 설명은 [확장으로 프로토콜 준수 추가 \(Adding Protocol Conformance with an Extension\)](protocols.md#adding-protocol-conformance-with-an-extension) 을 참고 바랍니다.

확장은 [제너릭 타입 확장 \(Extending a Generic Type\)](generics.md#extending-a-generic-type) 에 설명된대로 기존 일반 타입을 확장하기 위해 사용될 수 있습니다. [제너릭 Where 절이 있는 확장 \(Extensions with a Generic Where Clause\)](generics.md#where-extensions-with-a-generic-where-clause) 에 설명된대로 일반 타입을 확장하여 조건부로 기능을 추가할 수 있습니다.

<!--
NOTE
If you define an extension to add new functionality to an existing type, the new functionality will be available on all existing instances of that type, even if they were created before the extension was defined.
-->

> NOTE   
> 기존 타입에 새로운 기능을 추가하기 위해 확장을 정의한다면 새로운 기능은 확장이 정의되기 전에 생성되었어도 모든 기존에 인스턴스에서 사용 가능합니다.

## 계산된 프로퍼티 \(Computed Properties\)

<!--
Extensions can add computed instance properties and computed type properties to existing types. This example adds five computed instance properties to Swift’s built-in Double type, to provide basic support for working with distance units:
-->

확장은 기존 타입에 계산된 인스턴스 프로퍼티와 계산된 타입 프로퍼티를 추가할 수 있습니다. 이 예제는 거리 단위에 대한 작업에 대해 기본적으로 제공하기 위해 Swift의 `Double` 타입에 5개의 계산된 인스턴스 프로퍼티를 추가합니다:

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
These computed properties express that a Double value should be considered as a certain unit of length. Although they’re implemented as computed properties, the names of these properties can be appended to a floating-point literal value with dot syntax, as a way to use that literal value to perform distance conversions.

In this example, a Double value of 1.0 is considered to represent “one meter”. This is why the m computed property returns self—the expression 1.m is considered to calculate a Double value of 1.0.

Other units require some conversion to be expressed as a value measured in meters. One kilometer is the same as 1,000 meters, so the km computed property multiplies the value by 1_000.00 to convert into a number expressed in meters. Similarly, there are 3.28084 feet in a meter, and so the ft computed property divides the underlying Double value by 3.28084, to convert it from feet to meters.

These properties are read-only computed properties, and so they’re expressed without the get keyword, for brevity. Their return value is of type Double, and can be used within mathematical calculations wherever a Double is accepted:
-->

이 계산된 프로퍼티는 특정 길이의 단위로 `Double` 값을 표현합니다. 계산된 프로퍼티로 구현되지만 이러한 프로퍼티의 이름은 거리 변환을 수행하기 위해 해당 리터럴 값을 사용하는 방법으로 부동 소수점 값에 점 구문을 사용할 수 있습니다.

이 예제에서 `1.0` 의 `Double` 값은 "1 미터" 로 표시됩니다. `1.m` 표현식은 `Double` 값의 `1.0` 을 계산한 것으로 간주되므로 `m` 계산된 프로퍼티가 `self` 를 반환하는 이유입니다.

다른 단위는 미터로 계산된 값으로 표현되기 위해 변환을 요구합니다. 1 킬로미터는 1,000 미터와 같으므로 `km` 계산된 프로퍼티는 미터로 표현된 숫자로 변환하기 위해 `1_000.00` 의 값을 곱합니다. 유사하게 3.28084 피트는 1 미터 이므로 `ft` 계산된 프로퍼티는 피트를 미터로 변환하기 위해 `3.28084` 의 `Double` 값으로 나눕니다.

이 프로퍼티는 읽기전용 계산된 프로퍼티 이므로 간결성을 위해 `get` 키워드 없이 표현됩니다. 반환값은 `Double` 타입의 값이고 `Double` 이 허용되는 수학적 계산 내에서는 사용될 수 있습니다:

```swift
let aMarathon = 42.km + 195.m
print("A marathon is \(aMarathon) meters long")
// Prints "A marathon is 42195.0 meters long"
```

<!--
NOTE
Extensions can add new computed properties, but they can’t add stored properties, or add property observers to existing properties.
-->

> NOTE   
> 확장은 새로운 계산된 프로퍼티를 추가할 수 있지만 저장된 프로퍼티나 기존 프로퍼티에 프로퍼티 관찰자를 추가할 수 없습니다.

## 초기화 구문 \(Initializers\)

<!--
Extensions can add new initializers to existing types. This enables you to extend other types to accept your own custom types as initializer parameters, or to provide additional initialization options that were not included as part of the type’s original implementation.

Extensions can add new convenience initializers to a class, but they can’t add new designated initializers or deinitializers to a class. Designated initializers and deinitializers must always be provided by the original class implementation.

If you use an extension to add an initializer to a value type that provides default values for all of its stored properties and doesn’t define any custom initializers, you can call the default initializer and memberwise initializer for that value type from within your extension’s initializer. This wouldn’t be the case if you had written the initializer as part of the value type’s original implementation, as described in Initializer Delegation for Value Types.

If you use an extension to add an initializer to a structure that was declared in another module, the new initializer can’t access self until it calls an initializer from the defining module.

The example below defines a custom Rect structure to represent a geometric rectangle. The example also defines two supporting structures called Size and Point, both of which provide default values of 0.0 for all of their properties:
-->

확장은 기존 타입에 새로운 초기화 구문을 추가할 수 있습니다. 이것은 초기화 구문 파라미터로 고유한 사용자 정의 타입을 받아들이기 위해 다른 타입을 확장하거나 타입의 기존 구현의 부분으로 포함되지 않는 추가 초기화 옵션을 제공할 수 있습니다.

확장은 클래스에 새로운 편의 초기화 구문을 추가할 수 있지만 새로운 지정된 초기화 구문이나 초기화 해제 구문은 클래스에 추가할 수 없습니다. 지정된 초기화 구문과 초기화 해제 구문은 항상 기존 클래스 구현에 의해 제공되어야 합니다.

모든 저장된 프로퍼티에 기본값을 제공하고 모든 사용자 정의 초기화 구문을 정의하지 않은 값 타입으로 초기화 구문을 추가하기 위해 확장을 사용하면 확장의 초기화 구문 내에서 값 타입에 대해 기본 초기화 구문과 멤버별 초기화 구문을 호출할 수 있습니다. [값 타입을 위한 초기화 구문 위임 \(Initializer Delegation for Value Types\)](initialization.md#initializer-delegation-for-value-types) 에서 설명한대로 값 타입의 기존 구현의 부분으로 초기화 구문을 작성한 경우에는 해당되지 않습니다.

다른 모듈에서 선언된 구조체에 초기화 구문을 추가하기 위해 확장을 사용한다면 새로운 초기화 구문은 정의한 모듈에서 초기화 구문을 호출할 때까지 `self` 를 접근할 수 없습니다.

아래의 예제는 사각형을 나타내기 위해 사용자 정의 `Rect` 구조체를 정의합니다. 이 예제는 모든 프로퍼티에 `0.0` 의 기본값을 제공하는 `Size` 와 `Point` 라는 2개의 구조체도 정의합니다:

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
Because the Rect structure provides default values for all of its properties, it receives a default initializer and a memberwise initializer automatically, as described in Default Initializers. These initializers can be used to create new Rect instances:
-->

`Rect` 구조체는 모든 프로퍼티에 대해 기본값을 제공하므로 [기본 초기화 구문 \(Default Initializers\)](initialization.md#default-initializers) 에서 설명한대로 자동으로 기본 초기화 구문과 멤버별 초기화 구문을 받습니다. 이 초기화 구문은 새로운 `Rect` 인스턴스를 생성하기 위해 사용될 수 있습니다:

```swift
let defaultRect = Rect()
let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
   size: Size(width: 5.0, height: 5.0))
```

<!--
You can extend the Rect structure to provide an additional initializer that takes a specific center point and size:
-->

특정 중심점과 크기를 가지는 초기화 구문을 제공하기 위해 `Rect` 구조체를 확장할 수 있습니다:

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
This new initializer starts by calculating an appropriate origin point based on the provided center point and size value. The initializer then calls the structure’s automatic memberwise initializer init(origin:size:), which stores the new origin and size values in the appropriate properties:
-->

이 새로운 초기화 구문은 제공된 `center` 점과 `size` 값을 기반으로 적절한 원점을 계산하는 것으로 시작합니다. 그러면 이 초기화 구문은 적절한 프로퍼티에 새로운 원점과 크기값을 저장하는 구조체의 자동 멤버별 초기화 구문 `init(origin:size:)` 를 호출합니다:

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                      size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
```

<!--
NOTE
If you provide a new initializer with an extension, you are still responsible for making sure that each instance is fully initialized once the initializer completes.
-->

> NOTE   
> 확장으로 새로운 초기화 구문을 제공하면 초기화 구문이 완료되면 각 인스턴스가 완전히 초기화 되었는지 확인해야 합니다.

## 메서드 \(Methods\)

<!--
Extensions can add new instance methods and type methods to existing types. The following example adds a new instance method called repetitions to the Int type:
-->

확장은 기존 타입에 새로운 인스턴스 메서드와 타입 메서드를 추가할 수 있습니다. 아래 예제는 `Int` 타입으로 `repetitions` 라는 새로운 인스턴스 메서드를 추가합니다:

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
The repetitions(task:) method takes a single argument of type () -> Void, which indicates a function that has no parameters and doesn’t return a value.

After defining this extension, you can call the repetitions(task:) method on any integer to perform a task that many number of times:
-->

`repetitions(task:)` 메서드는 파라미터가 없고 반환값이 없는 함수를 나타내는 `() -> Void` 타입의 인자를 가집니다.

이 확장을 정의한 후에 여러번 작업을 수행하기 위해 모든 정수로 `repetitions(task:)` 메서드를 호출할 수 있습니다:

```swift
3.repetitions {
    print("Hello!")
}
// Hello!
// Hello!
// Hello!
```

### 인스턴스 메서드 변경 \(Mutating Instance Methods\)

<!--
Instance methods added with an extension can also modify (or mutate) the instance itself. Structure and enumeration methods that modify self or its properties must mark the instance method as mutating, just like mutating methods from an original implementation.

The example below adds a new mutating method called square to Swift’s Int type, which squares the original value:
-->

확장에서 추가된 인스턴스 메서드는 인스턴스 자체로 수정 또는 변경할 수 있습니다. `self` 또는 프로퍼티를 수정하는 구조체와 열거형 메서드는 기존 구현의 변경 메서드와 같이 `mutating` 으로 인스턴스 메서드를 표시해야 합니다.

아래 예제는 원래값을 제곱하는 Swift의 `Int` 타입의 `square` 라는 새로운 변경 메서드를 추가합니다:

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

## 서브 스크립트 \(Subscripts\)

<!--
Extensions can add new subscripts to an existing type. This example adds an integer subscript to Swift’s built-in Int type. This subscript [n] returns the decimal digit n places in from the right of the number:

* 123456789[0] returns 9
* 123456789[1] returns 8

…and so on:
-->

확장은 기존 타입에 새로운 서브 스크립트를 추가할 수 있습니다. 이 예제는 Swift의 `Int` 타입에 정수 서브 스크립트를 추가합니다. 이 서브 스크립트 `[n]` 은 숫자의 오른쪽 부터 `n` 에 위치하는 자리의 숫자를 반환합니다:

* `123456789[0]` 은 `9` 를 반환
* `123456789[1]` 은 `8` 을 반환

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
If the Int value doesn’t have enough digits for the requested index, the subscript implementation returns 0, as if the number had been padded with zeros to the left:
-->

`Int` 값이 요청된 인덱스에 대해 충분한 자리를 가지고 있지 않다면 숫자 왼쪽에 0이 채워진 것처럼 서브 스크립트 구현은 `0` 을 반환합니다:

```swift
746381295[9]
// returns 0, as if you had requested:
0746381295[9]
```

## 중첩된 타입 \(Nested Types\)

<!--
Extensions can add new nested types to existing classes, structures, and enumerations:
-->

확장은 기존 클래스, 구조체, 그리고 열거형에 새로운 중첩된 타입을 추가할 수 있습니다:

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
This example adds a new nested enumeration to Int. This enumeration, called Kind, expresses the kind of number that a particular integer represents. Specifically, it expresses whether the number is negative, zero, or positive.

This example also adds a new computed instance property to Int, called kind, which returns the appropriate Kind enumeration case for that integer.

The nested enumeration can now be used with any Int value:
-->

이 예제는 `Int` 에 새로운 중첩된 열거형을 추가합니다. `Kind` 라는 열거형은 특정 정수 표현에 대한 숫자의 종류를 나타냅니다. 명확하게 숫자가 음수, 0, 또는 양수 인지 표현합니다.

이 예제는 정수에 대해 적절한 `Kind` 열거형 케이스를 반환하는 `kind` 라는 새로운 계산된 인스턴스 프로퍼티를 `Int` 에 추가합니다.

이 중첩된 열거형은 모든 `Int` 값에서 사용될 수 있습니다:

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
This function, printIntegerKinds(_:), takes an input array of Int values and iterates over those values in turn. For each integer in the array, the function considers the kind computed property for that integer, and prints an appropriate description.
-->

`printIntegerKinds(_:)` 함수는 `Int` 값의 배열을 입력 받고 반복합니다. 배열의 각 정수에 대해 함수는 해당 정수의 `kind` 계산된 프로퍼티를 고려하고 적절한 설명을 출력합니다.

<!--
NOTE
number.kind is already known to be of type Int.Kind. Because of this, all of the Int.Kind case values can be written in shorthand form inside the switch statement, such as .negative rather than Int.Kind.negative.
-->

> NOTE   
> `number.kind` 는 `Int.Kind` 타입으로 이미 알고 있습니다. 이러한 점 때문에 모든 `Int.Kind` 케이스 값은 `switch` 구문 내에서 `Int.Kind.negative` 가 아닌 `.negative` 로 짧게 작성될 수 있습니다.

