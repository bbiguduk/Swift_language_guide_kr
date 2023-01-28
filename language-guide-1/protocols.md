# 프로토콜 \(Protocols\)

<!--
A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. The protocol can then be adopted by a class, structure, or enumeration to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to conform to that protocol.

In addition to specifying requirements that conforming types must implement, you can extend a protocol to implement some of these requirements or to implement additional functionality that conforming types can take advantage of.
-->

_프로토콜 \(protocol\)_ 은 메서드, 프로퍼티, 그리고 특정 작업이나 기능의 부분이 적합한 다른 요구사항의 청사진을 정의합니다. 프로토콜은 요구사항의 구현을 제공하기 위해 클래스, 구조체, 또는 열거형에 의해 채택될 수 있습니다. 프로토콜의 요구사항에 충족하는 모든 타입은 프로토콜에 _준수 \(conform\)_ 한다고 합니다.

준수하는 타입의 요구사항을 지정하는 것 외에도 요구사항의 일부를 구현 하거나 준수하는 타입에 추가 기능을 구현하기 위해 프로토콜을 확장할 수 있습니다.

## 프로토콜 구문 \(Protocol Syntax\)

<!--
You define protocols in a very similar way to classes, structures, and enumerations:
-->

클래스, 구조체, 그리고 열거형과 유사한 방법으로 프로토콜을 정의합니다:

```swift
protocol SomeProtocol {
    // protocol definition goes here
}
```

<!--
Custom types state that they adopt a particular protocol by placing the protocol’s name after the type’s name, separated by a colon, as part of their definition. Multiple protocols can be listed, and are separated by commas:
-->

사용자 정의 타입은 콜론으로 구분된 타입의 이름 뒤에 특정 프로토콜의 이름을 위치시켜 정의의 부분으로 특정 프로토콜을 채택합니다. 여러 프로토콜은 콤마로 구분되고 목록화 할 수 있습니다:

```swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
    // structure definition goes here
}
```

<!--
If a class has a superclass, list the superclass name before any protocols it adopts, followed by a comma:
-->

클래스가 상위 클래스를 가진 경우에 콤마로 구분하여 채택한 모든 프로토콜 전에 상위 클래스 이름을 위치 시킵니다:

```swift
class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
    // class definition goes here
}
```

## 프로퍼티 요구사항 \(Property Requirements\)

<!--
A protocol can require any conforming type to provide an instance property or type property with a particular name and type. The protocol doesn’t specify whether the property should be a stored property or a computed property—it only specifies the required property name and type. The protocol also specifies whether each property must be gettable or gettable and settable.

If a protocol requires a property to be gettable and settable, that property requirement can’t be fulfilled by a constant stored property or a read-only computed property. If the protocol only requires a property to be gettable, the requirement can be satisfied by any kind of property, and it’s valid for the property to be also settable if this is useful for your own code.

Property requirements are always declared as variable properties, prefixed with the var keyword. Gettable and settable properties are indicated by writing { get set } after their type declaration, and gettable properties are indicated by writing { get }.
-->

프로토콜은 특정 이름과 타입을 가진 인스턴스 프로퍼티 또는 타입 프로퍼티를 제공하기 위해 모든 준수하는 타입을 요구할 수 있습니다. 프로토콜은 요구된 프로퍼티 이름과 타입만 지정하고 프로퍼티가 저장된 프로퍼티 또는 계산된 프로퍼티 인지에 대한 것은 지정하지 않습니다. 프로토콜은 각 프로퍼티가 gettable 인지 gettable과 settable 인지도 지정해줘야 합니다.

프로토콜이 gettable과 settable 인 프로퍼티를 요구할 경우 프로퍼티 요구사항은 저장된 프로퍼티 상수 또는 읽기전용 계산된 프로퍼티는 충족할 수 없습니다. 프로토콜이 gettable 인 프로퍼티 만 요구할 경우 이 요구사항은 모든 종류의 프로퍼티에 충족될 수 있고 이것이 유용한 경우 settable 또한 프로퍼티에 대해 유효합니다.

프로퍼티 요구사항은 항상 `var` 키워드와 함께 변수 프로퍼티로 선언됩니다. gettable과 settable 프로퍼티는 타입 선언 뒤에 `{ get set }` 으로 작성하여 나타내고 gettable 프로퍼티는 `{ get }` 으로 작성하여 나타냅니다.

```swift
protocol SomeProtocol {
    var mustBeSettable: Int { get set }
    var doesNotNeedToBeSettable: Int { get }
}
```

<!--
Always prefix type property requirements with the static keyword when you define them in a protocol. This rule pertains even though type property requirements can be prefixed with the class or static keyword when implemented by a class:
-->

항상 프로토콜에서 정의할 때 `static` 키워드를 타입 프로퍼티 요구사항에 접두사로 둡니다. 이 규칙은 타입 프로퍼티 요구사항은 클래스에 의해 구현될 때 `class` 또는 `static` 키워드를 붙일 수 있는 경우에도 적용됩니다:

```swift
protocol AnotherProtocol {
    static var someTypeProperty: Int { get set }
}
```

<!--
Here’s an example of a protocol with a single instance property requirement:
-->

다음은 단일 인스턴스 프로퍼티 요구사항을 가지는 프로토콜의 예입니다:

```swift
protocol FullyNamed {
    var fullName: String { get }
}
```

<!--
The FullyNamed protocol requires a conforming type to provide a fully qualified name. The protocol doesn’t specify anything else about the nature of the conforming type—it only specifies that the type must be able to provide a full name for itself. The protocol states that any FullyNamed type must have a gettable instance property called fullName, which is of type String.

Here’s an example of a simple structure that adopts and conforms to the FullyNamed protocol:
-->

`fullyNamed` 프로토콜은 완벽한 이름을 제공하기 위해 준수하는 타입을 요구합니다. 이 프로토콜은 다른 준수하는 타입을 지정하지 않으며 타입이 자체에 대한 전체 이름을 제공해야 된다고만 지정합니다. 이 프로토콜은 모든 `FullyNamed` 타입이 `String` 타입의 `fullName` 이라는 gettable 인스턴스 프로퍼티를 가져야 합니다.

다음은 `FullyNamed` 프로토콜은 채택하고 준수하는 구조체에 대한 예입니다:

```swift
struct Person: FullyNamed {
    var fullName: String
}
let john = Person(fullName: "John Appleseed")
// john.fullName is "John Appleseed"
```

<!--
This example defines a structure called Person, which represents a specific named person. It states that it adopts the FullyNamed protocol as part of the first line of its definition.

Each instance of Person has a single stored property called fullName, which is of type String. This matches the single requirement of the FullyNamed protocol, and means that Person has correctly conformed to the protocol. (Swift reports an error at compile time if a protocol requirement isn’t fulfilled.)

Here’s a more complex class, which also adopts and conforms to the FullyNamed protocol:
-->

이 예제는 특정 이름을 가진 사람을 나타내는 `Person` 이라는 구조체를 정의합니다. 정의의 첫번째 줄의 부분으로 `FullyNamed` 프로토콜을 채택합니다.

`Person` 의 각 인스턴스는 `String` 타입의 `fullName` 이라는 단일 저장된 프로퍼티를 가집니다. 이것은 `FullyNamed` 프로토콜의 단일 요구사항과 일치하고 `Person` 은 프토콜을 올바르게 준수하고 있다고 얘기합니다 \(Swift는 프로토콜 요구사항이 충족되지 않으면 컴파일 시 에러가 발생합니다\).

다음은 `FullyNamed` 프로토콜은 채택하고 준수하는 더 복잡한 클래스입니다:

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
This class implements the fullName property requirement as a computed read-only property for a starship. Each Starship class instance stores a mandatory name and an optional prefix. The fullName property uses the prefix value if it exists, and prepends it to the beginning of name to create a full name for the starship.
-->

이 클래스는 스타십에 대해 계산된 읽기전용 프로퍼티로 `fullName` 프로퍼티 요구사항을 구현합니다. 각 `Starship` 클래스 인스턴스는 필수로 `name` 과 옵셔널 `prefix` 를 저장합니다. `fullName` 프로퍼티는 `prefix` 값이 존재하면 사용하고 스타십에 대한 전체 이름을 생성하기 위해 `name` 의 앞에 추가합니다.

## 메서드 요구사항 \(Method Requirements\)

<!--
Protocols can require specific instance methods and type methods to be implemented by conforming types. These methods are written as part of the protocol’s definition in exactly the same way as for normal instance and type methods, but without curly braces or a method body. Variadic parameters are allowed, subject to the same rules as for normal methods. Default values, however, can’t be specified for method parameters within a protocol’s definition.

As with type property requirements, you always prefix type method requirements with the static keyword when they’re defined in a protocol. This is true even though type method requirements are prefixed with the class or static keyword when implemented by a class:
-->

프로토콜은 준수하는 타입에 의해 구현되기 위해 지정한 인스턴스 메서드와 타입 메서드를 요구할 수 있습니다. 이 메서드는 일반적인 인스턴스와 타입 메서드와 같은 방식으로 명시적으로 프로토콜의 정의의 부분으로 작성되지만 중괄호가 없거나 메서드 본문이 없습니다. 일반적인 메서드와 같은 규칙에 따라 가변 파라미터는 허용됩니다. 그러나 기본 값은 프로토콜의 정의 내에서 메서드 파라미터에 대해 지정될 수 없습니다.

타입 프로퍼티 요구사항과 마찬가지로 프로토콜에 정의될 때 `static` 키워드를 항상 타입 메서드 요구사항 앞에 표기합니다. 클래스에 의해 구현될 때 타입 메서드 요구사항에 `class` 또는 `static` 키워드가 접두사로 붙는 경우에도 마찬가지입니다:

```swift
protocol SomeProtocol {
    static func someTypeMethod()
}
```

<!--
The following example defines a protocol with a single instance method requirement:
-->

아래의 예제는 단일 인스턴스 메서드 요구사항을 가지는 프로토콜을 정의합니다:

```swift
protocol RandomNumberGenerator {
    func random() -> Double
}
```

<!--
This protocol, RandomNumberGenerator, requires any conforming type to have an instance method called random, which returns a Double value whenever it’s called. Although it’s not specified as part of the protocol, it’s assumed that this value will be a number from 0.0 up to (but not including) 1.0.

The RandomNumberGenerator protocol doesn’t make any assumptions about how each random number will be generated—it simply requires the generator to provide a standard way to generate a new random number.

Here’s an implementation of a class that adopts and conforms to the RandomNumberGenerator protocol. This class implements a pseudorandom number generator algorithm known as a linear congruential generator:
-->

`RandomNumberGenerator` 프로토콜은 호출될 때마다 `Double` 값을 반환하는 `random` 이라는 인스턴스 메서드를 가지는 모든 준수하는 타입을 요구합니다. 프로토콜 부분으로 지정되지 않았지만 이 값은 `0.0` 부터 `1.0` 미만의 숫자라고 가정합니다.

`RandomNumberGenerator` 프로토콜은 각 난수가 생성되는 방법에 대해 어떠한 것도 가정하지 않습니다. 단순히 생성기가 새로운 난수를 생성하는 표준 방법을 제공하면 됩니다.

다음은 `RandomNumberGenerator` 프로토콜을 채택하고 준수하는 클래스의 구현입니다. 이 클래스는 _선형 합동 생성기 \(linear congruential generator\)_ 로 알려진 의사 난수 \(pseudorandom number\) 생성기 알고리즘을 구현합니다:

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

## 메서드 요구사항 변경 \(Mutating Method Requirements\)

<!--
It’s sometimes necessary for a method to modify (or mutate) the instance it belongs to. For instance methods on value types (that is, structures and enumerations) you place the mutating keyword before a method’s func keyword to indicate that the method is allowed to modify the instance it belongs to and any properties of that instance. This process is described in Modifying Value Types from Within Instance Methods.

If you define a protocol instance method requirement that’s intended to mutate instances of any type that adopts the protocol, mark the method with the mutating keyword as part of the protocol’s definition. This enables structures and enumerations to adopt the protocol and satisfy that method requirement.
-->

메서드가 속한 인스턴스를 수정 또는 변경 해야하는 경우가 있습니다. 값 타입 \(구조체와 열거형\)에 대한 인스턴스 메서드의 경우 메서드의 `func` 키워드 앞에 `mutating` 키워드를 위치시켜 메서드가 속한 인스턴스와 인스턴스의 모든 프로퍼티를 수정할 수 있음을 나타냅니다. 이 프로세스는 [인스턴스 메서드 내에서 값 타입 수정 \(Modifying Value Types from Within Instance Methods\)](methods.md#modifying-value-types-from-within-instance-methods) 에 설명되어 있습니다.

프로토콜을 채택하는 모든 타입의 인스턴스를 변경하기 위한 프로토콜 인스턴스 메서드 요구사항을 정의하는 경우 프로토콜의 정의의 부분으로 `mutating` 키워드로 메서드를 표시합니다. 이를 통해 구조체와 열거형이 프로토콜을 채택하고 메서드 요구사항을 충족할 수 있습니다.

<!--
NOTE
If you mark a protocol instance method requirement as mutating, you don’t need to write the mutating keyword when writing an implementation of that method for a class. The mutating keyword is only used by structures and enumerations.
-->

> NOTE   
> `mutating` 으로 프로토콜 인스턴스 메서드 요구사항을 표시하면 클래스에 대한 해당 메서드의 구현을 작성할 때 `mutating` 키워드를 작성할 필요가 없습니다. `mutating` 키워드는 구조체와 열거형에 의해서만 사용됩니다.

<!--
The example below defines a protocol called Togglable, which defines a single instance method requirement called toggle. As its name suggests, the toggle() method is intended to toggle or invert the state of any conforming type, typically by modifying a property of that type.

The toggle() method is marked with the mutating keyword as part of the Togglable protocol definition, to indicate that the method is expected to mutate the state of a conforming instance when it’s called:
-->

아래의 예제는 `toggle` 이라는 단일 인스턴스 메서드 요구사항을 정의하는 `Togglable` 이라는 프로토콜을 정의합니다. 이름에서 알 수 있듯이 `toggle()` 메서드는 해당 타입의 프로퍼티를 수정하여 모든 준수하는 타입의 상태를 전환하거나 반전하기 위한 것입니다.

`toggle()` 메서드는 호출될 때 준수하는 인스턴스의 상태를 변경하기 위한 메서드를 나타내기 위해 `Togglable` 프로토콜 정의의 부분으로 `mutating` 키워드로 표시됩니다:

```swift
protocol Togglable {
    mutating func toggle()
}
```

<!--
If you implement the Togglable protocol for a structure or enumeration, that structure or enumeration can conform to the protocol by providing an implementation of the toggle() method that’s also marked as mutating.

The example below defines an enumeration called OnOffSwitch. This enumeration toggles between two states, indicated by the enumeration cases on and off. The enumeration’s toggle implementation is marked as mutating, to match the Togglable protocol’s requirements:
-->

구조체 또는 열거형에 대해 `Togglable` 프로토콜을 구현하면 해당 구조체 또는 열거형은 `mutating` 으로 표시된 `toggle()` 메서드의 구현을 제공하는 프로토콜을 준수할 수 있습니다.

아래의 예제는 `OnOffSwitch` 라는 열거형을 정의합니다. 이 열거형은 열거형 케이스 인 `on` 과 `off` 를 나타내기 위해 2개의 상태를 변경합니다. 이 열거형의 `toggle` 구현은 `Togglable` 프로토콜의 요구사항을 일치 시키기 위해 `mutating` 으로 표시됩니다:

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

## 초기화 구문 요구사항 \(Initializer Requirements\)

<!--
Protocols can require specific initializers to be implemented by conforming types. You write these initializers as part of the protocol’s definition in exactly the same way as for normal initializers, but without curly braces or an initializer body:
-->

프로토콜은 준수하는 타입에 의해 지정된 구현된 초기화 구문을 요구할 수 있습니다. 일반적인 초기화 구문과 동일한 방식으로 명시적으로 프로토콜의 정의의 부분으로 초기화 구문을 작성하지만 중괄호 또는 초기화 구문 본문 없이 작성합니다:

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}
```

### 프로토콜 초기화 구문 요구사항의 클래스 구현 \(Class Implementations of Protocol Initializer Requirements\)

<!--
You can implement a protocol initializer requirement on a conforming class as either a designated initializer or a convenience initializer. In both cases, you must mark the initializer implementation with the required modifier:
-->

지정된 초기화 구문 또는 편의 초기화 구문으로 준수하는 클래스에 프로토콜 초기화 구문 요구사항을 구현할 수 있습니다. 이 모든 케이스에 대해 `required` 수식어와 함께 초기화 구문 구현에 표시해야 합니다:

```swift
class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // initializer implementation goes here
    }
}
```

<!--
The use of the required modifier ensures that you provide an explicit or inherited implementation of the initializer requirement on all subclasses of the conforming class, such that they also conform to the protocol.

For more information on required initializers, see Required Initializers.
-->

`required` 수식어를 사용하면 준수하는 클래스의 모든 하위 클래스에 초기화 구문 요구사항의 명시적 또는 상속된 구현을 제공하여 프로토콜을 준수할 수 있습니다.

더 자세한 정보는 [필수 초기화 구문 \(Required Initializers\)](initialization.md#required-initializers) 을 참고 바랍니다.

<!--
NOTE
You don’t need to mark protocol initializer implementations with the required modifier on classes that are marked with the final modifier, because final classes can’t subclassed. For more about the final modifier, see Preventing Overrides.
-->

> NOTE   
> final 클래스는 하위 클래스 될 수 없으므로 `final` 수식어로 표시된 클래스에 `required` 수식어를 프로토콜 초기화 구문 구현에 표시할 필요가 없습니다. `final` 수식어에 대한 자세한 내용은 [재정의 방지 \(Preventing Overrides\)](inheritance.md#preventing-overrides) 를 참고 바랍니다.

<!--
If a subclass overrides a designated initializer from a superclass, and also implements a matching initializer requirement from a protocol, mark the initializer implementation with both the required and override modifiers:
-->

하위 클래스가 상위 클래스의 지정된 초기화 구문을 재정의 하고 프로토콜로 부터 일치하는 초기화 구문 요구사항이 구현되면 `required` 와 `override` 수식어 둘 다 초기화 구문 구현에 표시합니다:

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

### 실패 가능한 초기화 구문 요구사항 \(Failable Initializer Requirements\)

<!--
Protocols can define failable initializer requirements for conforming types, as defined in Failable Initializers.

A failable initializer requirement can be satisfied by a failable or nonfailable initializer on a conforming type. A nonfailable initializer requirement can be satisfied by a nonfailable initializer or an implicitly unwrapped failable initializer.
-->

프로토콜은 [실패 가능한 초기화 구문 \(Failable Initializers\)](initialization.md#failable-initializers) 에 정의 된대로 준수하는 타입에 대해 실패 가능한 초기화 구문 요구사항을 정의할 수 있습니다.

실패 가능한 초기화 구문 요구사항은 준수하는 타입에 실패 가능하거나 실패 불가능한 초기화 구문에 의해 충족될 수 있습니다. 실패 불가능한 초기화 구문 요구사항은 실패 불가능한 초기화 구문 또는 암시적 언래핑 된 실패 가능한 초기화 구문에 의해 충족될 수 있습니다.

## 타입으로 프로토콜 \(Protocols as Types\)

<!--
Protocols don’t actually implement any functionality themselves. Nonetheless, you can use protocols as a fully fledged types in your code. Using a protocol as a type is sometimes called an existential type, which comes from the phrase “there exists a type T such that T conforms to the protocol”.

You can use a protocol in many places where other types are allowed, including:

* As a parameter type or return type in a function, method, or initializer
* As the type of a constant, variable, or property
* As the type of items in an array, dictionary, or other container
-->

프로토콜 자체는 어떤 기능도 구현하지 않습니다. 그럼에도 불구하고 프로토콜을 코드에서 완전한 타입으로 사용할 수 있습니다. 타입으로 프로토콜을 사용하는 것은 "T가 프로토콜을 준수하는 타입 T가 존재한다" 라는 구절에서 비롯된 _존재 타입 \(existential type\)_ 이라고 합니다.

다음을 포함하여 다른 타입이 허용되는 여러 위치에서 프로토콜을 사용할 수 있습니다:

* 함수, 메서드, 또는 초기화 구문에서 파라미터 타입 또는 반환 타입으로
* 상수, 변수, 또는 프로퍼티의 타입으로
* 배열, 딕셔너리, 또는 다른 컨테이너에서 항목의 타입

<!--
NOTE
Because protocols are types, begin their names with a capital letter (such as FullyNamed and RandomNumberGenerator) to match the names of other types in Swift (such as Int, String, and Double).
-->

> NOTE   
> 프로토콜은 타입이므로 Swift에서 다른 타입 \(`Int`, `String`, 그리고 `Double`\)과 일치하도록 프로토콜 이름을 대문자로 시작합니다 \(`FullyNamed` 그리고 `RandomNumberGenerator`\).

<!--
Here’s an example of a protocol used as a type:
-->

다음은 타입으로 사용한 프로토콜의 예입니다:

```swift
class Dice {
    let sides: Int
    let generator: RandomNumberGenerator
    init(sides: Int, generator: RandomNumberGenerator) {
        self.sides = sides
        self.generator = generator
    }
    func roll() -> Int {
        return Int(generator.random() * Double(sides)) + 1
    }
}
```

<!--
This example defines a new class called Dice, which represents an n-sided dice for use in a board game. Dice instances have an integer property called sides, which represents how many sides they have, and a property called generator, which provides a random number generator from which to create dice roll values.

The generator property is of type RandomNumberGenerator. Therefore, you can set it to an instance of any type that adopts the RandomNumberGenerator protocol. Nothing else is required of the instance you assign to this property, except that the instance must adopt the RandomNumberGenerator protocol. Because its type is RandomNumberGenerator, code inside the Dice class can only interact with generator in ways that apply to all generators that conform to this protocol. That means it can’t use any methods or properties that are defined by the underlying type of the generator. However, you can downcast from a protocol type to an underlying type in the same way you can downcast from a superclass to a subclass, as discussed in Downcasting.

Dice also has an initializer, to set up its initial state. This initializer has a parameter called generator, which is also of type RandomNumberGenerator. You can pass a value of any conforming type in to this parameter when initializing a new Dice instance.

Dice provides one instance method, roll, which returns an integer value between 1 and the number of sides on the dice. This method calls the generator’s random() method to create a new random number between 0.0 and 1.0, and uses this random number to create a dice roll value within the correct range. Because generator is known to adopt RandomNumberGenerator, it’s guaranteed to have a random() method to call.

Here’s how the Dice class can be used to create a six-sided dice with a LinearCongruentialGenerator instance as its random number generator:
-->

이 예제는 보드 게임에서 사용하기 위한 _n_ 면의 주사위를 나타내는 `Dice` 라는 새로운 클래스를 정의합니다. `Dice` 인스턴스는 얼마나 많은 면을 가지고 있는지를 나타내는 `sides` 라는 정수 프로퍼티를 가지고 주사위 굴린 값을 생성하기 위해 난수 생성기를 제공하는 `generator` 라는 프로퍼티를 가집니다.

`generator` 프로퍼티는 `RandomNumberGenerator` 타입입니다. 따라서 `RandomNumberGenerator` 프로토콜을 채택하는 _모든_ 타입에 인스턴스로 설정할 수 있습니다. 인스턴스가 `RandomNumberGenerator` 프로토콜을 채택해야 된다는 것을 제외하고 이 프로퍼티에 할당하는 인스턴스에 다른 것은 필요하지 않습니다. 타입은 `RandomNumberGenerator` 이므로 `Dice` 클래스 내의 코드는 이 프로토콜을 준수하는 모든 생성기에 적용하는 방식으로만 `generator` 와 상호작용 할 수 있습니다. 생성기의 기본 타입으로 정의된 메서드 또는 프로퍼티는 사용할 수 없습니다. 그러나 [다운 캐스팅 \(Downcasting\)](type-casting.md#downcasting) 에서 설명 했듯이 상위 클래스에서 하위 클래스로 다운 캐스트 할 수 있는 방법과 동일하게 프로토콜 타입에서 기본 타입으로 다운 캐스트 할 수 있습니다.

`Dice` 는 초기화 상태를 설정하기 위해 초기화 구문을 가지고 있습니다. 이 초기화 구문은 `RandomNumberGenerator` 타입의 `generator` 라는 파라미터를 가지고 있습니다. 새로운 `Dice` 인스턴스로 초기화 할 때 파라미터로 준수하는 타입의 값을 전달할 수 있습니다.

`Dice` 는 1과 주사위의 면의 숫자 사이의 정수 값을 반환하는 `roll` 인스턴스 메서드를 제공합니다. 이 메서드는 `0.0` 과 `1.0` 사이의 새로운 난수를 생성하는 생성기의 `random()` 메서드를 호출하고 올바른 범위 내의 주사위 굴림값을 생성하기 위해 난수를 사용합니다. `generator` 는 `RandomNumberGenerator` 를 채택하기 때문에 `random()` 메서드를 호출할 수 있습니다.

아래는 `Dice` 클래스가 난수 생성서기로 `LinearCongruentialGenerator` 인스턴스와 6면 주사위를 생성하기 위해 어떻게 사용될 수 있는지 보여줍니다:

```swift
var d6 = Dice(sides: 6, generator: LinearCongruentialGenerator())
for _ in 1...5 {
    print("Random dice roll is \(d6.roll())")
}
// Random dice roll is 3
// Random dice roll is 5
// Random dice roll is 4
// Random dice roll is 5
// Random dice roll is 4
```

## 위임 \(Delegation\)

<!--
Delegation is a design pattern that enables a class or structure to hand off (or delegate) some of its responsibilities to an instance of another type. This design pattern is implemented by defining a protocol that encapsulates the delegated responsibilities, such that a conforming type (known as a delegate) is guaranteed to provide the functionality that has been delegated. Delegation can be used to respond to a particular action, or to retrieve data from an external source without needing to know the underlying type of that source.

The example below defines two protocols for use with dice-based board games:
-->

_위임 \(Delegation\)_ 은 클래스 또는 구조체가 책임의 일부를 다른 타입의 인스턴스에 넘겨주거나 위임할 수 있도록 하는 디자인 패턴입니다. 이 디자인 패턴은 위임된 기능을 제공하기 위해 준수하는 타입 \(대리자라고 함\)이 보장되도록 위임된 책임을 캡슐화하는 프로토콜을 정의하여 구현합니다. 위임은 특정 작업에 응답하거나 해당 소스의 기본 타입을 알 필요 없이 외부 소스에서 데이터를 검색하는데 사용할 수 있습니다.

아래 예제는 주사위 기반 보드게임에 사용할 2가지 프로토콜을 정의합니다:

```swift
protocol DiceGame {
    var dice: Dice { get }
    func play()
}
protocol DiceGameDelegate: AnyObject {
    func gameDidStart(_ game: DiceGame)
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int)
    func gameDidEnd(_ game: DiceGame)
}
```

<!--
The DiceGame protocol is a protocol that can be adopted by any game that involves dice.

The DiceGameDelegate protocol can be adopted to track the progress of a DiceGame. To prevent strong reference cycles, delegates are declared as weak references. For information about weak references, see Strong Reference Cycles Between Class Instances. Marking the protocol as class-only lets the SnakesAndLadders class later in this chapter declare that its delegate must use a weak reference. A class-only protocol is marked by its inheritance from AnyObject, as discussed in Class-Only Protocols.

Here’s a version of the Snakes and Ladders game originally introduced in Control Flow. This version is adapted to use a Dice instance for its dice-rolls; to adopt the DiceGame protocol; and to notify a DiceGameDelegate about its progress:
-->

`DiceGame` 프로토콜은 주사위를 포함하는 모든 게임에 의해 채택될 수 있는 프로토콜입니다.

`DiceGameDelegate` 프로토콜은 `DiceGame` 의 진행사항을 추적하기 위해 채택될 수 있습니다. 강한 참조 사이클을 방지하기 위해 위임자는 약한 참조로 선언됩니다. 약한 참조에 대한 자세한 내용은 [클래스 인스턴스 사이의 강한 참조 사이클 \(Strong Reference Cycles Between Class Instances\)](automatic-reference-counting.md#strong-reference-cycles-between-class-instances) 을 참고 바랍니다. 프로토콜을 클래슬 전용 프로토콜로 표시하면 이 챕터의 뒷부분에서 `SnakesAndLadders` 클래스는 위임자가 약한 참조로 사용되어야 한다고 선언할 수 있습니다. [클래스 전용 프로토콜 \(Class-Only Protocols\)](protocols.md#class-only-protocols) 은 `AnyObject` 의 상속으로 표시됩니다.

다음은 [제어 흐름 \(Control Flow\)](control-flow.md) 에서 기존에 도입된 _Snakes and Ladders_ 게임의 버전입니다. 이 버전은 주사위 굴림에 대해 `Dice` 인스턴스를 사용하기 위해 채택하고 `DiceGame` 프로토콜을 채택하고 진행사항에 대해 알리기 위해 `DiceGameDelegate` 를 채택합니다:

```swift
class SnakesAndLadders: DiceGame {
    let finalSquare = 25
    let dice = Dice(sides: 6, generator: LinearCongruentialGenerator())
    var square = 0
    var board: [Int]
    init() {
        board = Array(repeating: 0, count: finalSquare + 1)
        board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
        board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
    }
    weak var delegate: DiceGameDelegate?
    func play() {
        square = 0
        delegate?.gameDidStart(self)
        gameLoop: while square != finalSquare {
            let diceRoll = dice.roll()
            delegate?.game(self, didStartNewTurnWithDiceRoll: diceRoll)
            switch square + diceRoll {
            case finalSquare:
                break gameLoop
            case let newSquare where newSquare > finalSquare:
                continue gameLoop
            default:
                square += diceRoll
                square += board[square]
            }
        }
        delegate?.gameDidEnd(self)
    }
}
```

<!--
For a description of the Snakes and Ladders gameplay, see Break.

This version of the game is wrapped up as a class called SnakesAndLadders, which adopts the DiceGame protocol. It provides a gettable dice property and a play() method in order to conform to the protocol. (The dice property is declared as a constant property because it doesn’t need to change after initialization, and the protocol only requires that it must be gettable.)

The Snakes and Ladders game board setup takes place within the class’s init() initializer. All game logic is moved into the protocol’s play method, which uses the protocol’s required dice property to provide its dice roll values.

Note that the delegate property is defined as an optional DiceGameDelegate, because a delegate isn’t required in order to play the game. Because it’s of an optional type, the delegate property is automatically set to an initial value of nil. Thereafter, the game instantiator has the option to set the property to a suitable delegate. Because the DiceGameDelegate protocol is class-only, you can declare the delegate to be weak to prevent reference cycles.

DiceGameDelegate provides three methods for tracking the progress of a game. These three methods have been incorporated into the game logic within the play() method above, and are called when a new game starts, a new turn begins, or the game ends.

Because the delegate property is an optional DiceGameDelegate, the play() method uses optional chaining each time it calls a method on the delegate. If the delegate property is nil, these delegate calls fail gracefully and without error. If the delegate property is non-nil, the delegate methods are called, and are passed the SnakesAndLadders instance as a parameter.

This next example shows a class called DiceGameTracker, which adopts the DiceGameDelegate protocol:
-->

_Snakes and Ladders_ 게임 플레이에 대한 설명은 [중단 \(Break\)](control-flow.md#break) 을 참고 바랍니다.

이 게임의 버전은 `DiceGame` 프로토콜을 채택하는 `SnakesAndLadders` 라는 클래스로 래핑됩니다. 프로토콜을 준수하기 위해 gettable `dice` 프로퍼티와 `play()` 메서드를 제공합니다 \(초기화 후에 변경할 필요가 없고 프로토콜은 오직 gettable만 요구하므로 `dice` 프로퍼티는 상수 프로퍼티로 선언됩니다\).

_Snakes and Ladders_ 게임보드 설정은 클래스의 `init()` 초기화 구문 내에서 발생합니다. 모든 게임 로직은 주사위 굴림값을 제공하기 위해 프로토콜의 요구된 `dice` 프로퍼티를 사용하는 프로토콜의 `play` 메서드에서 이동됩니다.

위임자는 게임 플레이 하기위해 요구되지 않으므로 `delegate` 프로퍼티는 _옵셔널_ `DiceGameDelegate` 로 정의됩니다. 옵셔널 타입이므로 `delegate` 프로퍼티는 자동으로 초기값을 `nil` 로 설정합니다. 그 후에 게임 인스턴스는 적절한 위임자로 설정할 수 있는 옵션이 있습니다. `DiceGameDelegate` 프로토콜은 클래스 전용 이므로 참조 사이클을 막기위해 `weak` 로 위임자를 선언할 수 있습니다.

`DiceGameDelegate` 는 게임의 진행사항을 추적하기 위해 3개의 메서드를 제공합니다. 이 3개의 메서드는 위의 `play()` 메서드 내의 게임 로직으로 통합되었고 새로운 게임이 시작되고 새로운 턴이 시작되거나 게임이 종료될 때 호출됩니다.

`delegate` 프로퍼티는 _옵셔널_ `DiceGameDelegate` 이므로 `play()` 메서드는 위임자에서 메서드를 호출할 때마다 옵셔널 체이닝을 사용합니다. `delegate` 프로퍼티가 `nil` 이면 이 위임자 호출은 정상적으로 에러없이 실패합니다. `delegate` 프로퍼티가 `nil` 이 아니면 이 위임자 메서드가 호출되고 파라미터로 `SnakesAndLadders` 인스턴스는 전달됩니다.

다음 예제는 `DiceGameDelegate` 프로토콜을 채택하는 `DiceGameTracker` 라는 클래스는 보여줍니다:

```swift
class DiceGameTracker: DiceGameDelegate {
    var numberOfTurns = 0
    func gameDidStart(_ game: DiceGame) {
        numberOfTurns = 0
        if game is SnakesAndLadders {
            print("Started a new game of Snakes and Ladders")
        }
        print("The game is using a \(game.dice.sides)-sided dice")
    }
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int) {
        numberOfTurns += 1
        print("Rolled a \(diceRoll)")
    }
    func gameDidEnd(_ game: DiceGame) {
        print("The game lasted for \(numberOfTurns) turns")
    }
}
```

<!--
DiceGameTracker implements all three methods required by DiceGameDelegate. It uses these methods to keep track of the number of turns a game has taken. It resets a numberOfTurns property to zero when the game starts, increments it each time a new turn begins, and prints out the total number of turns once the game has ended.

The implementation of gameDidStart(_:) shown above uses the game parameter to print some introductory information about the game that’s about to be played. The game parameter has a type of DiceGame, not SnakesAndLadders, and so gameDidStart(_:) can access and use only methods and properties that are implemented as part of the DiceGame protocol. However, the method is still able to use type casting to query the type of the underlying instance. In this example, it checks whether game is actually an instance of SnakesAndLadders behind the scenes, and prints an appropriate message if so.

The gameDidStart(_:) method also accesses the dice property of the passed game parameter. Because game is known to conform to the DiceGame protocol, it’s guaranteed to have a dice property, and so the gameDidStart(_:) method is able to access and print the dice’s sides property, regardless of what kind of game is being played.

Here’s how DiceGameTracker looks in action:
-->

`DiceGameTracker` 는 `DiceGameDelegate` 에 의해 요구된 3개의 메서드 모두 구현합니다. 이 메서드를 사용하여 게임의 턴 수를 추적합니다. 게임이 시작될 때 `numberOfTurns` 프로퍼티를 0으로 재설정하고 새로운 턴이 시작될 때마다 증가하고 게임이 종료될 때 턴의 총 수를 출력합니다.

위에서 보이는 `gameDidStart(_:)` 의 구현은 곧 플레이 할 게임에 대한 일부 소개 정보를 출력하기 위해 `game` 파라미터를 사용합니다. `game` 파라미터는 `SnakesAndLadders` 가 아닌 `DiceGame` 의 타입을 가지므로 `gameDidStart(_:)` 는 `DiceGame` 프로토콜의 부분으로 구현된 메서드와 프로퍼티로만 접근하고 사용할 수 있습니다. 그러나 이 메서드는 여전히 기본 인스턴스의 타입을 조회하기 위해 타입 캐스팅을 사용할 수 있습니다. 예를 들어 `game` 은 `SnakesAndLadders` 의 인스턴스인지 확인하고 적절한 메세지를 출력합니다.

`gameDidStart(_:)` 메서드는 전달된 `game` 파라미터의 `dice` 프로퍼티에 접근합니다. `game` 은 `DiceGame` 프로토콜을 준수하므로 `dice` 프로퍼티가 보장되므로 `gameDidStart(_:)` 메서드는 어떤 종류의 게임을 플레이 하든 주사위의 `sides` 프로퍼티에 접근하고 출력할 수 있습니다.

다음은 `DiceGameTracker` 의 동작을 보여줍니다:

```swift
let tracker = DiceGameTracker()
let game = SnakesAndLadders()
game.delegate = tracker
game.play()
// Started a new game of Snakes and Ladders
// The game is using a 6-sided dice
// Rolled a 3
// Rolled a 5
// Rolled a 4
// Rolled a 5
// The game lasted for 4 turns
```

## 확장으로 프로토콜 준수성 추가 \(Adding Protocol Conformance with an Extension\)

<!--
You can extend an existing type to adopt and conform to a new protocol, even if you don’t have access to the source code for the existing type. Extensions can add new properties, methods, and subscripts to an existing type, and are therefore able to add any requirements that a protocol may demand. For more about extensions, see Extensions.
-->

기존 타입에 대해 소스 코드에서 접근할 수 없지만 새로운 프로토콜을 채택하고 준수하기 위해 기존 타입을 확장할 수 있습니다. 확장은 기존 타입에 새로운 프로퍼티, 메서드, 그리고 서브 스크립트를 추가할 수 있으므로 프로토콜이 요구할 수 있는 모든 요구사항을 추가할 수 있습니다. 자세한 내용은 [확장 \(Extensions\)](extensions.md) 을 참고 바랍니다.

<!--
NOTE
Existing instances of a type automatically adopt and conform to a protocol when that conformance is added to the instance’s type in an extension.
-->

> NOTE   
> 타입의 기존 인스턴스는 확장에 인스턴스의 타입이 추가될 때 자동으로 프로토콜을 채택하고 준수합니다.

<!--
For example, this protocol, called TextRepresentable, can be implemented by any type that has a way to be represented as text. This might be a description of itself, or a text version of its current state:
-->

예를 들어 `TextRepresentable` 이라는 프로토콜은 텍스트로 표현할 수 있는 모든 타입으로 구현될 수 있습니다. 이것은 자신의 설명이거나 현재 상태의 텍스트 버전일 수 있습니다:

```swift
protocol TextRepresentable {
    var textualDescription: String { get }
}
```

<!--
The Dice class from above can be extended to adopt and conform to TextRepresentable:
-->

위에서 `Dice` 클래스는 `TextRepresentable` 을 채택하고 준수하기 위해 확장될 수 있습니다:

```swift
extension Dice: TextRepresentable {
    var textualDescription: String {
        return "A \(sides)-sided dice"
    }
}
```

<!--
This extension adopts the new protocol in exactly the same way as if Dice had provided it in its original implementation. The protocol name is provided after the type name, separated by a colon, and an implementation of all requirements of the protocol is provided within the extension’s curly braces.

Any Dice instance can now be treated as TextRepresentable:
-->

이 확장은 `Dice` 가 원래 구현에서 제공한 것과 똑같은 방식으로 새로운 프로토콜을 채택합니다. 프로토콜 이름은 콜론으로 구분된 타입 이름 뒤에 제공되고 프로토콜의 모든 요구사항의 구현은 확장의 중괄호 내에 제공됩니다.

이제 모든 `Dice` 인스턴스를 `TextRepresentable` 로 처리할 수 있습니다:

```swift
let d12 = Dice(sides: 12, generator: LinearCongruentialGenerator())
print(d12.textualDescription)
// Prints "A 12-sided dice"
```

<!--
Similarly, the SnakesAndLadders game class can be extended to adopt and conform to the TextRepresentable protocol:
-->

마찬가지로 `SnakesAndLadders` 게임 클래스는 `TextRepresentable` 프로토콜을 채택하고 준수하기 위해 확장될 수 있습니다:

```swift
extension SnakesAndLadders: TextRepresentable {
    var textualDescription: String {
        return "A game of Snakes and Ladders with \(finalSquare) squares"
    }
}
print(game.textualDescription)
// Prints "A game of Snakes and Ladders with 25 squares"
```

### 조건적으로 프로토콜 준수 \(Conditionally Conforming to a Protocol\)

<!--
A generic type may be able to satisfy the requirements of a protocol only under certain conditions, such as when the type’s generic parameter conforms to the protocol. You can make a generic type conditionally conform to a protocol by listing constraints when extending the type. Write these constraints after the name of the protocol you’re adopting by writing a generic where clause. For more about generic where clauses, see Generic Where Clauses.

The following extension makes Array instances conform to the TextRepresentable protocol whenever they store elements of a type that conforms to TextRepresentable.
-->

일반 타입은 타입의 일반 파라미터가 프로토콜을 준수하는 경우와 같은 특정 조건에서만 프로토콜의 요구사항을 충족시킬 수 있습니다. 타입을 확장할 때 제약조건을 나열하여 일반 타입이 프로토콜을 조건적으로 준수할 수 있도록 만들 수 있습니다. 일반적인 `where` 절을 작성하여 채택중인 프로토콜의 이름 뒤에 제약조건을 작성합니다. 일반 `where` 절에 대한 자세한 내용은 [제너릭 Where 절 \(Generic Where Clauses\)](generics.md#where-generic-where-clauses) 을 참고 바랍니다.

다음 확장은 `Array` 인스턴스가 `TextRepresentable` 을 준수하는 타입의 항목을 저장할 때마다 `TextRepresentable` 프로토콜을 준수하도록 합니다.

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

### 확장과 함께 프로토콜 채택 선언 \(Declaring Protocol Adoption with an Extension\)

<!--
If a type already conforms to all of the requirements of a protocol, but hasn’t yet stated that it adopts that protocol, you can make it adopt the protocol with an empty extension:
-->

타입이 이미 프로토콜의 모든 요구사항을 준수하지만 해당 프로토콜을 책택한다고 아직 명시하지 않은 경우 빈 확장을 사용하여 프로토콜을 채택하도록 만들 수 있습니다:

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
Instances of Hamster can now be used wherever TextRepresentable is the required type:
-->

`Hamster` 의 인스턴스는 `TextRepresentable` 이 요구된 타입 어디서든 사용될 수 있습니다:

```swift
let simonTheHamster = Hamster(name: "Simon")
let somethingTextRepresentable: TextRepresentable = simonTheHamster
print(somethingTextRepresentable.textualDescription)
// Prints "A hamster named Simon"
```

<!--
NOTE
Types don’t automatically adopt a protocol just by satisfying its requirements. They must always explicitly declare their adoption of the protocol.
-->

> NOTE   
> 타입은 요구사항이 충족된다고 해서 프로토콜을 자동으로 채택하지 않습니다. 항상 프로토콜 채택을 명시적으로 선언해야 합니다.

## 합성된 구현을 사용하여 프로토콜 채택 \(Adopting a Protocol Using a Synthesized Implementation\)

<!--
Swift can automatically provide the protocol conformance for Equatable, Hashable, and Comparable in many simple cases. Using this synthesized implementation means you don’t have to write repetitive boilerplate code to implement the protocol requirements yourself.

Swift provides a synthesized implementation of Equatable for the following kinds of custom types:

* Structures that have only stored properties that conform to the Equatable protocol
* Enumerations that have only associated types that conform to the Equatable protocol
* Enumerations that have no associated types

To receive a synthesized implementation of ==, declare conformance to Equatable in the file that contains the original declaration, without implementing an == operator yourself. The Equatable protocol provides a default implementation of !=.

The example below defines a Vector3D structure for a three-dimensional position vector (x, y, z), similar to the Vector2D structure. Because the x, y, and z properties are all of an Equatable type, Vector3D receives synthesized implementations of the equivalence operators.
-->

Swift는 많은 경우에 `Equatable`, `Hashable`, 그리고 `Comparable` 에 대해 프로토콜 준수성을 자동으로 제공할 수 있습니다. 합성된 구현을 사용하면 프로토콜 요구사항 구현을 위해 반복적인 상용구 코드를 작성할 필요가 없습니다.

Swift는 다음과 같은 사용자 정의 타입에 대해 `Equatable` 의 합성된 구현을 제공합니다:

* `Equatable` 프로토콜을 준수하는 저장된 프로토콜만 있는 구조체
* `Equatable` 프로토콜을 준수하는 연관된 타입만 있는 열거형
* 연관된 타입이 없는 열거형

`==` 의 합성된 구현을 받기 위해선 `==` 연산자를 직접 구현하지 않고 원래 선언을 포함한 파일에서 `Equatable` 에 대한 준수성을 선언합니다. `Equatable` 프로토콜은 `!=` 의 기본 구현을 제공합니다.

아래의 예제는 `Vector2D` 구조체와 유사한 3차원의 벡터 `(x, y, z)` 에 대한 `Vector3D` 구조체를 정의합니다. `x`, `y`, 그리고 `z` 프로퍼티는 모두 `Equatable` 타입이므로 `Vector3D` 는 등가 연산자의 합성된 구현을 받습니다.

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
Swift provides a synthesized implementation of Hashable for the following kinds of custom types:

* Structures that have only stored properties that conform to the Hashable protocol
* Enumerations that have only associated types that conform to the Hashable protocol
* Enumerations that have no associated types

To receive a synthesized implementation of hash(into:), declare conformance to Hashable in the file that contains the original declaration, without implementing a hash(into:) method yourself.

Swift provides a synthesized implementation of Comparable for enumerations that don’t have a raw value. If the enumeration has associated types, they must all conform to the Comparable protocol. To receive a synthesized implementation of <, declare conformance to Comparable in the file that contains the original enumeration declaration, without implementing a < operator yourself. The Comparable protocol’s default implementation of <=, >, and >= provides the remaining comparison operators.

The example below defines a SkillLevel enumeration with cases for beginners, intermediates, and experts. Experts are additionally ranked by the number of stars they have.
-->

Swift는 아래와 같은 사용자 정의 타입에 대해 `Hashable` 에 합성된 구현을 제공합니다:

* `Hashable` 프로토콜을 준수하는 저장된 프로퍼티만 가지는 구조체
* `Hashable` 프로토콜을 준수하는 연관된 타입만 가지는 열거형
* 연관된 타입이 없는 열거형

`hash(into:)` 에 합성된 구현을 받기 위해선 `hash(into:)` 메서드를 직접 구현하지 않고 원래 선언을 포함한 파일에서 `Hashable` 에 대한 준수성을 선언합니다.

Swift는 원시값이 없는 열거형에 대해 `Comparable` 에 합성된 구현을 제공합니다. 열거형이 연관된 타입을 가지고 있다면 모두 `Comparable` 프로토콜을 준수해야 합니다. `<` 의 합성된 구현을 받기 위해선 `<` 연산자를 직접 구현하지 않고 원래 열거형 선언을 포함한 파일에서 `Comparable` 에 대한 준수성을 선언합니다. `<=`, `>`, 그리고 `>=` 의 `Comparable` 프로토콜의 기본 구현은 나머지 비교 연산자를 제공합니다.

아래의 예제는 초보자, 중급자, 그리고 전문가의 케이스를 가진 `SkillLevel` 열거형을 정의합니다. 전문가는 가진 별의 숫자에 따라 추가적으로 순위가 매겨집니다.

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

## 프로토콜 타입의 콜렉션 \(Collections of Protocol Types\)

<!--
A protocol can be used as the type to be stored in a collection such as an array or a dictionary, as mentioned in Protocols as Types. This example creates an array of TextRepresentable things:
-->

프로토콜은 [타입으로의 프로토콜 \(Protocols as Types\)](protocols.md#protocols-as-types) 에서 언급했듯이 배열 또는 딕셔너리와 같은 콜렉션에 저장되기 위해 타입으로 사용될 수 있습니다. 이 예제는 `TextRepresentable` 에 대한 배열을 생성합니다:

```swift
let things: [TextRepresentable] = [game, d12, simonTheHamster]
```

<!--
It’s now possible to iterate over the items in the array, and print each item’s textual description:
-->

이제 배열에 항목을 반복할 수 있고 각 항목의 설명을 출력할 수 있습니다:

```swift
for thing in things {
    print(thing.textualDescription)
}
// A game of Snakes and Ladders with 25 squares
// A 12-sided dice
// A hamster named Simon
```

<!--
Note that the thing constant is of type TextRepresentable. It’s not of type Dice, or DiceGame, or Hamster, even if the actual instance behind the scenes is of one of those types. Nonetheless, because it’s of type TextRepresentable, and anything that’s TextRepresentable is known to have a textualDescription property, it’s safe to access thing.textualDescription each time through the loop.
-->

`thing` 상수는 `TextRepresentable` 타입입니다. 배후에서 실제 인스턴스가 `Dice` 또는 `DiceGame` 또는 `Hamster` 중 하나 이지만 이것의 타입은 아닙니다. 그럼에도 불구하고 이것은 `TextRepresentable` 타입이고 `TextRepresentable` 은 `textualDescription` 프로퍼티를 가지고 있다는 것을 알고 있으므로 루프를 통해 매번 `thing.textualDescription` 에 안전하게 접근할 수 있습니다.

## 프로토콜 상속 \(Protocol Inheritance\)

<!--
A protocol can inherit one or more other protocols and can add further requirements on top of the requirements it inherits. The syntax for protocol inheritance is similar to the syntax for class inheritance, but with the option to list multiple inherited protocols, separated by commas:
-->

프로토콜은 하나 또는 그 이상의 다른 프로토콜을 _상속_ 할 수 있고 상속한 요구사항 위에 요구사항을 더 추가할 수 있습니다. 프로토콜 상속에 대한 구문은 클래스 상속에 대한 구문과 유사하지만 콤마로 구분하여 여러개의 상속된 프로토콜을 목록화 하는 옵션을 가지고 있습니다:

```swift
protocol InheritingProtocol: SomeProtocol, AnotherProtocol {
    // protocol definition goes here
}
```

<!--
Here’s an example of a protocol that inherits the TextRepresentable protocol from above:
-->

다음은 위에 `TextRepresentable` 프로토콜을 상속하는 프로토콜의 예입니다:

```swift
protocol PrettyTextRepresentable: TextRepresentable {
    var prettyTextualDescription: String { get }
}
```

<!--
This example defines a new protocol, PrettyTextRepresentable, which inherits from TextRepresentable. Anything that adopts PrettyTextRepresentable must satisfy all of the requirements enforced by TextRepresentable, plus the additional requirements enforced by PrettyTextRepresentable. In this example, PrettyTextRepresentable adds a single requirement to provide a gettable property called prettyTextualDescription that returns a String.

The SnakesAndLadders class can be extended to adopt and conform to PrettyTextRepresentable:
-->

이 예제는 `TextRepresentable` 을 상속하는 `PrettyTextRepresentable` 이라는 새로운 프로토콜을 정의합니다. `PrettyTextRepresentable` 을 채택하는 모든 것은 `TextRepresentable` 과 `PrettyTextRepresentable` 의 모든 요구사항을 충족해야 합니다. 이 예제에서 `PrettyTextRepresentable` 은 `String` 을 반환하는 `prettyTextualDescription` 이라는 gettable 프로퍼티를 제공하기 위해 하나의 요구사항을 추가합니다.

`SnakesAndLadders` 클래스는 `PrettyTextRepresentable` 을 채택하고 준수하기 위해 확장될 수 있습니다:

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
This extension states that it adopts the PrettyTextRepresentable protocol and provides an implementation of the prettyTextualDescription property for the SnakesAndLadders type. Anything that’s PrettyTextRepresentable must also be TextRepresentable, and so the implementation of prettyTextualDescription starts by accessing the textualDescription property from the TextRepresentable protocol to begin an output string. It appends a colon and a line break, and uses this as the start of its pretty text representation. It then iterates through the array of board squares, and appends a geometric shape to represent the contents of each square:

* If the square’s value is greater than 0, it’s the base of a ladder, and is represented by ▲.
* If the square’s value is less than 0, it’s the head of a snake, and is represented by ▼.
* Otherwise, the square’s value is 0, and it’s a “free” square, represented by ○.

The prettyTextualDescription property can now be used to print a pretty text description of any SnakesAndLadders instance:
-->

이 확장은 `PrettyTextRepresentable` 프로토콜을 채택하고 `SnakesAndLadders` 타입에 대해 `prettyTextualDescription` 프로퍼티의 구현을 제공한다고 나타냅니다. `PrettyTextRepresentable` 인 모든 것은 `TextRepresentable` 이어야 하므로 `prettyTextualDescription` 의 구현은 출력 문자열을 시작하기 위해 `TextRepresentable` 프로토콜에서 `textualDescription` 프로퍼티를 접근하는 것으로 시작합니다. 콜론과 줄바꿈을 추가하고 정리된 텍스트 표현을 시작으로 사용합니다. 그런다음 보드 사각형의 배열을 통해 반복하고 각 사각형의 내용을 나타내기 위해 모양을 추가합니다:

* 사각형의 값이 `0` 보다 크면 사다리의 밑부분이 되고  `▲` 로 표시됩니다.
* 사각형의 값이 `0` 보다 작으면 뱀의 머리이고 `▼` 으로 표시됩니다.
* 그렇지 않으면 사각형의 값은 `0` 이고 "자유" 정사각형이며 `○` 으로 표시됩니다.

`prettyTextualDescription` 프로퍼티는 이제 모든 `SnakesAndLadders` 인스턴스의 텍스트 설명을 출력하기 위해 사용될 수 있습니다:

```swift
print(game.prettyTextualDescription)
// A game of Snakes and Ladders with 25 squares:
// ○ ○ ▲ ○ ○ ▲ ○ ○ ▲ ▲ ○ ○ ○ ▼ ○ ○ ○ ○ ▼ ○ ○ ▼ ○ ▼ ○
```

## 클래스 전용 프로토콜 \(Class-Only Protocols\)

<!--
You can limit protocol adoption to class types (and not structures or enumerations) by adding the AnyObject protocol to a protocol’s inheritance list.
-->

프로토콜 채택을 프로토콜의 상속 목록에 `AnyObject` 프로토콜을 추가하여 구조체 또는 열거형이 아닌 클래스 타입으로 제한할 수 있습니다.

```swift
protocol SomeClassOnlyProtocol: AnyObject, SomeInheritedProtocol {
    // class-only protocol definition goes here
}
```

<!--
In the example above, SomeClassOnlyProtocol can only be adopted by class types. It’s a compile-time error to write a structure or enumeration definition that tries to adopt SomeClassOnlyProtocol.
-->

위의 예제에서 `SomeClassOnlyProtocol` 은 클래스 타입에만 채택될 수 있습니다. `SomeClassOnlyProtocol` 을 구조체 또는 열거형 정의에 채택하면 컴파일 시 에러가 발생합니다.

<!--
NOTE
Use a class-only protocol when the behavior defined by that protocol’s requirements assumes or requires that a conforming type has reference semantics rather than value semantics. For more about reference and value semantics, see Structures and Enumerations Are Value Types and Classes Are Reference Types.
-->

> NOTE   
> 프로토콜의 요구사항에 의해 정의된 동작이 준수하는 타입에 값 의미 체계가 아닌 참조 의미 체계가 있다고 가정하거나 요구하는 경우 클래스 전용 프로토콜을 사용합니다. 참조와 값 의미 체계에 대한 자세한 내용은 [구조체와 열거형은 값 타입 \(Structures and Enumerations Are Value Types\)](structures-and-classes.md#structures-and-enumerations-are-value-types) 와 [클래스는 참조 타입 \(Classes Are Reference Types\)](structures-and-classes.md#classes-are-reference-types) 을 참고 바랍니다.

## 프로토콜 구성 \(Protocol Composition\)

<!--
It can be useful to require a type to conform to multiple protocols at the same time. You can combine multiple protocols into a single requirement with a protocol composition. Protocol compositions behave as if you defined a temporary local protocol that has the combined requirements of all protocols in the composition. Protocol compositions don’t define any new protocol types.

Protocol compositions have the form SomeProtocol & AnotherProtocol. You can list as many protocols as you need, separating them with ampersands (&). In addition to its list of protocols, a protocol composition can also contain one class type, which you can use to specify a required superclass.

Here’s an example that combines two protocols called Named and Aged into a single protocol composition requirement on a function parameter:
-->

동시에 여러개의 프로토콜을 준수하는 타입을 요구하는 것이 유용할 수 있습니다. _프로토콜 구성 \(protocol composition\)_ 을 사용하여 여러 프로토콜을 단일 요구사항으로 결합할 수 있습니다. 프로토콜 구성은 구성에 모든 프로토콜의 결합된 요구사항을 가진 임시 로컬 프로토콜로 정의된 것처럼 동작합니다. 프로토콜 구성은 새로운 프로토콜 타입을 정의하지 않습니다.

프로토콜 구성은 `SomeProtocol & AnotherProtocol` 형식입니다. 앰퍼샌드 \(`&`\)로 구분하여 많은 프로토콜을 목록화 할 수 있습니다. 프로토콜 목록 외에도 프로토콜 구성은 요구된 상위 클래스를 지정하는데 사용할 수 있는 하나의 클래스 타입을 포함할 수도 있습니다.

다음은 `Named` 와 `Aged` 라는 두 프로토콜을 함수 파라미터에 단일 프로토콜 구성 요구사항으로 결합한 예입니다:

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
In this example, the Named protocol has a single requirement for a gettable String property called name. The Aged protocol has a single requirement for a gettable Int property called age. Both protocols are adopted by a structure called Person.

The example also defines a wishHappyBirthday(to:) function. The type of the celebrator parameter is Named & Aged, which means “any type that conforms to both the Named and Aged protocols.” It doesn’t matter which specific type is passed to the function, as long as it conforms to both of the required protocols.

The example then creates a new Person instance called birthdayPerson and passes this new instance to the wishHappyBirthday(to:) function. Because Person conforms to both protocols, this call is valid, and the wishHappyBirthday(to:) function can print its birthday greeting.

Here’s an example that combines the Named protocol from the previous example with a Location class:
-->

이 예제에서 `Named` 프로토콜은 `name` 이라는 gettable `String` 프로퍼티인 단일 요구사항을 가지고 있습니다. `Aged` 프로토콜은 `age` 라는 gettable `Int` 프로퍼티인 단일 요구사항을 가지고 있습니다. 두 프로토콜 모두 `Person` 이라는 구조체에 의해 채택됩니다.

예제는 `wishHappyBirthday(to:)` 함수도 정의합니다. `celebrator` 파라미터의 타입은 "`Named` 와 `Aged` 프로토콜 모두 준사하는 타입" 이라는 의미인 `Named & Aged` 입니다. 요구된 프로토콜 모두 준수하는 한 함수에 전달되는 특정 유형은 중요하지 않습니다.

그런다음 `birthdayPerson` 이라는 새로운 `Person` 인스턴스를 생성하고 `wishHappyBirthday(to:)` 함수에 새로운 인스턴스를 전달합니다. `Person` 은 프로토콜 모두 준수하기 때문에 이 호출은 유효하고 `wishHappyBirthday(to:)` 함수는 생일 메세지를 출력할 수 있습니다.

다음은 `Location` 클래스와 이전 예제에서의 `Named` 프로토콜을 결합한 예입니다:

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
The beginConcert(in:) function takes a parameter of type Location & Named, which means “any type that’s a subclass of Location and that conforms to the Named protocol.” In this case, City satisfies both requirements.

Passing birthdayPerson to the beginConcert(in:) function is invalid because Person isn’t a subclass of Location. Likewise, if you made a subclass of Location that didn’t conform to the Named protocol, calling beginConcert(in:) with an instance of that type is also invalid.
-->

`beginConcert(in:)` 함수는 "`Location` 의 하위 클래스와 `Named` 프로토콜을 준수하는 모든 타입" 이라는 뜻의 `Location & Named` 타입의 파라미터를 가집니다. 이 경우 `City` 는 이 요구사항에 충족합니다.

`Person` 은 `Location` 의 하위 클래스가 아니므로 `beginConcert(in:)` 함수로 `birthdayPerson` 전달은 유효하지 않습니다. 마찬가지로 `Named` 프로토콜을 준수하지 않고 `Location` 의 하위 클래스를 만들어 타입의 인스턴스로 `beginConcert(in:)` 을 호출하면 유효하지 않습니다.

## 프로토콜 준수에 대한 검사 \(Checking for Protocol Conformance\)

<!--
You can use the is and as operators described in Type Casting to check for protocol conformance, and to cast to a specific protocol. Checking for and casting to a protocol follows exactly the same syntax as checking for and casting to a type:

* The is operator returns true if an instance conforms to a protocol and returns false if it doesn’t.
* The as? version of the downcast operator returns an optional value of the protocol’s type, and this value is nil if the instance doesn’t conform to that protocol.
* The as! version of the downcast operator forces the downcast to the protocol type and triggers a runtime error if the downcast doesn’t succeed.

This example defines a protocol called HasArea, with a single property requirement of a gettable Double property called area:
-->

프로토콜 준수성에 대해 확인하고 특정 프로토콜로 캐스팅 하기 위해 [타입 캐스팅 \(Type Casting\)](type-casting.md) 에서 설명했듯이 `is` 와 `as` 연산자를 사용할 수 있습니다. 프로토콜을 확인하고 캐스팅하는 것은 타입을 확인하고 캐스팅 하는 것과 정확하게 같은 구문을 따릅니다:

* `is` 연산자는 인스턴스가 프로토콜을 준수한다면 `true` 를 반환하고 그렇지 않으면 `false` 를 반환합니다.
* 다운 캐스트 연산자의 `as?` 버전은 프로토콜의 타입의 옵셔널 값을 반환하고 인스턴스가 프로토콜을 준수하지 않으면 그 값은 `nil` 입니다.
* 다운 캐스트 연산자의 `as!` 버전은 프로토콜 타입으로 강제로 다운 캐스팅 하고 다운 캐스트가 성공하지 못하면 런타임 에러를 트리거 합니다.

아래 예제는 `area` 라는 gettable `Double` 프로퍼티의 단일 프로퍼티 요구사항을 가지는 `HasArea` 라는 프로토콜을 정의합니다:

```swift
protocol HasArea {
    var area: Double { get }
}
```

<!--
Here are two classes, Circle and Country, both of which conform to the HasArea protocol:
-->

다음은 모두 `HasArea` 프로토콜을 준수하는 `Circle` 과 `Country` 인 2개의 클래스 입니다:

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
The Circle class implements the area property requirement as a computed property, based on a stored radius property. The Country class implements the area requirement directly as a stored property. Both classes correctly conform to the HasArea protocol.

Here’s a class called Animal, which doesn’t conform to the HasArea protocol:
-->

`Circle` 클래스는 저장된 `radius` 프로퍼티 기반으로 계산된 프로퍼티로 `area` 프로퍼티 요구사항을 구현합니다. `Country` 클래스는 저장된 프로퍼티로 직접 `area` 요구사항을 구현합니다. 두 클래스 모두 `HasArea` 프로토콜을 준수합니다.

다음은 `HasArea` 프로토콜을 준수하지 않는 `Animal` 이라는 클래스 입니다:

```swift
class Animal {
    var legs: Int
    init(legs: Int) { self.legs = legs }
}
```

<!--
The Circle, Country and Animal classes don’t have a shared base class. Nonetheless, they’re all classes, and so instances of all three types can be used to initialize an array that stores values of type AnyObject:
-->

`Circle`, `Country` 그리고 `Animal` 클래스는 공유된 기본 클래스가 없습니다. 그럼에도 불구하고 모두 클래스 이므로 모든 세가지 타입의 인스턴스는 타입 `AnyObject` 의 값을 저장하는 배열을 초기화 하기위해 사용될 수 있습니다:

```swift
let objects: [AnyObject] = [
    Circle(radius: 2.0),
    Country(area: 243_610),
    Animal(legs: 4)
]
```

<!--
The objects array is initialized with an array literal containing a Circle instance with a radius of 2 units; a Country instance initialized with the surface area of the United Kingdom in square kilometers; and an Animal instance with four legs.

The objects array can now be iterated, and each object in the array can be checked to see if it conforms to the HasArea protocol:
-->

`objects` 배열은 2의 반지름을 가진 `Circle` 인스턴스, 영국의 표면적으로 초기화 된 `Country` 인스턴스 그리고 4개의 다리를 가진 `Animal` 인스턴스를 포함하는 배열 리터럴로 초기화 됩니다.

`objects` 배열은 이제 반복될 수 있고 배열의 각 객체는 `HasArea` 프로토콜을 준수하는지 확인할 수 있습니다:

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
Whenever an object in the array conforms to the HasArea protocol, the optional value returned by the as? operator is unwrapped with optional binding into a constant called objectWithArea. The objectWithArea constant is known to be of type HasArea, and so its area property can be accessed and printed in a type-safe way.

Note that the underlying objects aren’t changed by the casting process. They continue to be a Circle, a Country and an Animal. However, at the point that they’re stored in the objectWithArea constant, they’re only known to be of type HasArea, and so only their area property can be accessed.
-->

배열의 객체가 `HasArea` 프로토콜을 준수할 때마다 `as?` 연산자에 의해 반환된 옵셔널 값은 `objectWithArea` 라는 상수에 옵셔널 바인딩으로 언래핑 됩니다. `objectWithArea` 상수는 `HasArea` 타입으로 알고 있으므로 `area` 프로퍼티는 접근 가능하고 안전하게 출력될 수 있습니다.

기본 객체는 캐스팅 프로세스에 의해 변경되지 않습니다. 그것은 계속해서 `Circle`, `Country`, 그리고 `Animal` 입니다. 그러나 `objectWithArea` 상수에 저장될 때 `HasArea` 타입으로만 알고 있으므로 `area` 프로퍼티만 접근 가능합니다.

## 옵셔널 프로토콜 요구사항 \(Optional Protocol Requirements\)

<!--
You can define optional requirements for protocols. These requirements don’t have to be implemented by types that conform to the protocol. Optional requirements are prefixed by the optional modifier as part of the protocol’s definition. Optional requirements are available so that you can write code that interoperates with Objective-C. Both the protocol and the optional requirement must be marked with the @objc attribute. Note that @objc protocols can be adopted only by classes that inherit from Objective-C classes or other @objc classes. They can’t be adopted by structures or enumerations.

When you use a method or property in an optional requirement, its type automatically becomes an optional. For example, a method of type (Int) -> String becomes ((Int) -> String)?. Note that the entire function type is wrapped in the optional, not the method’s return value.

An optional protocol requirement can be called with optional chaining, to account for the possibility that the requirement was not implemented by a type that conforms to the protocol. You check for an implementation of an optional method by writing a question mark after the name of the method when it’s called, such as someOptionalMethod?(someArgument). For information on optional chaining, see Optional Chaining.

The following example defines an integer-counting class called Counter, which uses an external data source to provide its increment amount. This data source is defined by the CounterDataSource protocol, which has two optional requirements:
-->

프로토콜에 대해 _옵셔널 요구사항 \(optional requirements\)_ 을 정의할 수 있습니다. 이 요구사항은 프로토콜을 준수하는 타입으로 구현될 필요가 없습니다. 옵셔널 요구사항은 프로토콜의 정의의 부분으로 `optional` 수식어를 앞에 붙입니다. 옵셔널 요구사항은 Objective-C와 상호운용되는 코드를 작성할 수 있습니다. 프로토콜과 옵셔널 요구사항 모두 `@objc` 속성으로 표시되어야 합니다. `@objc` 프로토콜은 Objective-C 클래스나 다른 `@objc` 클래스로 부터 상속한 클래스에만 채택될 수 있습니다. 구조체나 열거형에 의해 채택될 수 없습니다.

옵셔널 요구사항에서 메서드나 프로퍼티를 사용할 때 그것의 타입은 자동으로 옵셔널이 됩니다. 예를 들어 `(Int) -> String` 타입의 메서드는 `((Int) -> String)?` 이 됩니다. 전체 함수 타입은 메서드의 반환값이 아니라 옵셔널로 래핑됩니다.

옵셔널 프로토콜 요구사항은 프로토콜을 준수하는 타입에 의해 요구사항이 구현되지 않았을 가능성을 나타내기 위해 옵셔널 체이닝으로 호출될 수 있습니다. 호출될 때 `someOptionalMethod?(someArgument)` 와 같이 메서드의 이름 뒤에 물음표를 작성하여 옵셔널 메서드의 구현에 대해 확인합니다. [옵셔널 체이닝 \(Optional Chaining\)](optional-chaining.md) 에서 더 자세한 내용을 확인할 수 있습니다.

다음 예제는 증가하는 값을 제공하기 위해 외부 데이터 소스를 사용하는 `Counter` 라는 정수 카운팅 클래스를 정의합니다. 이 데이터 소스는 2개의 옵셔널 요구사항을 가진 `CounterDataSource` 프로토콜에 의해 정의됩니다:

```swift
@objc protocol CounterDataSource {
    @objc optional func increment(forCount count: Int) -> Int
    @objc optional var fixedIncrement: Int { get }
}
```

<!--
The CounterDataSource protocol defines an optional method requirement called increment(forCount:) and an optional property requirement called fixedIncrement. These requirements define two different ways for data sources to provide an appropriate increment amount for a Counter instance.
-->

`CounterDataSource` 프로토콜은 `increment(forCount:)` 라는 옵셔널 메서드 요구사항과 `fixedIncrement` 라는 옵셔널 프로퍼티 요구사항을 정의합니다. 이 요구사항은 `Counter` 인스턴스에 대해 적절한 증가값을 제공하기 위해 데이터 소스에 대한 2가지 다른 방법을 정의합니다.

<!--
NOTE
Strictly speaking, you can write a custom class that conforms to CounterDataSource without implementing either protocol requirement. They’re both optional, after all. Although technically allowed, this wouldn’t make for a very good data source.
-->

> NOTE   
> 엄밀히 말하면 프로토콜 요구사항을 구현하지 않고도 `CounterDataSource` 를 준수하는 사용자 정의 클래스를 작성할 수 있습니다. 둘다 옵셔널 입니다. 기술적으로는 허용되지만 좋은 데이터 소스로는 적합합지 않습니다.

<!--
The Counter class, defined below, has an optional dataSource property of type CounterDataSource?:
-->

아래 정의된 `Counter` 클래스는 `CounterDataSource?` 타입의 옵셔널 `dataSource` 프로퍼티를 가지고 있습니다:

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
The Counter class stores its current value in a variable property called count. The Counter class also defines a method called increment, which increments the count property every time the method is called.

The increment() method first tries to retrieve an increment amount by looking for an implementation of the increment(forCount:) method on its data source. The increment() method uses optional chaining to try to call increment(forCount:), and passes the current count value as the method’s single argument.

Note that two levels of optional chaining are at play here. First, it’s possible that dataSource may be nil, and so dataSource has a question mark after its name to indicate that increment(forCount:) should be called only if dataSource isn’t nil. Second, even if dataSource does exist, there’s no guarantee that it implements increment(forCount:), because it’s an optional requirement. Here, the possibility that increment(forCount:) might not be implemented is also handled by optional chaining. The call to increment(forCount:) happens only if increment(forCount:) exists—that is, if it isn’t nil. This is why increment(forCount:) is also written with a question mark after its name.

Because the call to increment(forCount:) can fail for either of these two reasons, the call returns an optional Int value. This is true even though increment(forCount:) is defined as returning a non-optional Int value in the definition of CounterDataSource. Even though there are two optional chaining operations, one after another, the result is still wrapped in a single optional. For more information about using multiple optional chaining operations, see Linking Multiple Levels of Chaining.

After calling increment(forCount:), the optional Int that it returns is unwrapped into a constant called amount, using optional binding. If the optional Int does contain a value—that is, if the delegate and method both exist, and the method returned a value—the unwrapped amount is added onto the stored count property, and incrementation is complete.

If it’s not possible to retrieve a value from the increment(forCount:) method—either because dataSource is nil, or because the data source doesn’t implement increment(forCount:)—then the increment() method tries to retrieve a value from the data source’s fixedIncrement property instead. The fixedIncrement property is also an optional requirement, so its value is an optional Int value, even though fixedIncrement is defined as a non-optional Int property as part of the CounterDataSource protocol definition.

Here’s a simple CounterDataSource implementation where the data source returns a constant value of 3 every time it’s queried. It does this by implementing the optional fixedIncrement property requirement:
-->

`Counter` 클래스는 `count` 라는 프로퍼티 변수에 현재값을 저장합니다. `Counter` 클래스는 메서드가 호출될 때마다 `count` 프로퍼티를 증가하는 `increment` 라는 메서드도 정의합니다.

`increment()` 메서드는 먼저 데이터 소스에 `increment(forCount:)` 메서드의 구현을 통해 증가값을 조회하려고 합니다. `increment()` 메서드는 `increment(forCount:)` 호출에 대해 옵셔널 체이닝을 사용하고 메서드의 단일 인자로 현재 `count` 값을 전달합니다.

여기서 2단계 옵셔널 체이닝을 사용합니다. 먼저 `dataSource` 는 `nil` 이 가능하므로 `dataSource` 가 `nil` 이 아닌 경우에만 `increment(forCount:)` 호출해야 된다는 것을 나타내기 위해 `dataSource` 는 이름 뒤에 물음표를 붙입니다. 두번째로 옵셔널 요구사항 이므로 `dataSource` 가 존재하더라도 `increment(forCount:)` 가 구현되었다고 보장하지 않습니다. 여기서 `increment(forCount:)` 가 구현되지 않은 가능성은 옵셔널 체이닝에 의해 처리됩니다. `increment(forCount:)` 가 존재할 경우에만 `increment(forCount:)` 호출이 이뤄지고 존재하지 않으면 `nil` 입니다. 이것이 `increment(forCount:)` 이름 뒤에 물음표가 붙는 이유입니다.

`increment(forCount:)` 를 호출하는 것은 위의 2가지 이유로 실패할 수 있으므로 이 호출은 _옵셔널_ `Int` 값을 반환합니다. `increment(forCount:)` 가 `CounterDataSource` 의 정의에서 옵셔널이 아닌 `Int` 값으로 반환하더라도 마찬가지입니다. 2개의 옵셔널 체이닝 연산자가 차례로 있지만 결과는 여전히 단일 옵셔널로 래핑됩니다. 여러개 옵셔널 체이닝 연산자 사용에 대한 자세한 내용은 [여러 수준의 체이닝 연결 \(Linking Multiple Levels of Chaining\)](optional-chaining.md#linking-multiple-levels-of-chaining) 을 참고 바랍니다.

`increment(forCount:)` 호출 후에 반환한 옵셔널 `Int` 는 옵셔널 바인딩을 사용하여 `amount` 라는 상수에 언래핑 됩니다. 옵셔널 `Int` 에 값이 포함되어 있다면 —이것은 위임자와 메서드가 모두 존재하고 메서드가 값을 반환 한 경우— 언래핑된 `amount` 는 저장된 `count` 프로퍼티에 추가하고 증가를 완료합니다.

`dataSource` 가 `nil` 이거나 데이터 소스가 `increment(forCount:)` 를 구현하지 않아 `increment(forCount:)` 로 부터 값을 조회할 수 없는 경우에는 `increment()` 메서드는 데이터 소스의 `fixedIncrement` 프로퍼티를 대신 조회하려고 합니다. `fixedIncrement` 프로퍼티도 옵셔널 요구사항이므로 그 값은 `fixedIncrement` 가 `CounterDataSource` 프로토콜 정의의 부분으로 옵셔널이 아닌 `Int` 프로퍼티로 정의되었어도 옵셔널 `Int` 값입니다.

다음은 데이터 소스가 매번 조회할 때마다 `3` 의 상수값을 반환하는 간단한 `CounterDataSource` 구현 입니다. 옵셔널 `fixedIncrement` 프로퍼티 요구사항을 구현하여 이것을 수행합니다:

```swift
class ThreeSource: NSObject, CounterDataSource {
    let fixedIncrement = 3
}
```

<!--
You can use an instance of ThreeSource as the data source for a new Counter instance:
-->

새로운 `Counter` 인스턴스에 대해 데이터 소스로 `ThreeSource` 의 인스턴스를 사용할 수 있습니다:

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
The code above creates a new Counter instance; sets its data source to be a new ThreeSource instance; and calls the counter’s increment() method four times. As expected, the counter’s count property increases by three each time increment() is called.

Here’s a more complex data source called TowardsZeroSource, which makes a Counter instance count up or down towards zero from its current count value:
-->

위의 코드는 새로운 `Counter` 인스턴스를 생성하고 그것의 데이터 소스를 새로운 `ThreeSouce` 인스턴스로 설정하고 카운터의 `increment()` 메서드를 4번 호출합니다. 예상대로 카운터의 `count` 프로퍼티는 `increment()` 가 호출될 때마다 3씩 증가합니다.

다음은 `Counter` 인스턴스를 현재 `count` 값에서 0으로 증가 또는 감소 시키는 `TowardsZeroSource` 라는 더 복잡한 데이터 소스입니다:

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
The TowardsZeroSource class implements the optional increment(forCount:) method from the CounterDataSource protocol and uses the count argument value to work out which direction to count in. If count is already zero, the method returns 0 to indicate that no further counting should take place.

You can use an instance of TowardsZeroSource with the existing Counter instance to count from -4 to zero. Once the counter reaches zero, no more counting takes place:
-->

`TowardsZeroSource` 클래스는 `CounterDataSource` 프로토콜로 부터 옵셔널 `increment(forCount:)` 메서드를 구현하고 카운트 방향을 정하기 위해 `count` 인자값을 사용합니다. `count` 가 0이면 이 메서드는 더이상 카운트 작업을 진행하지 않음을 나타내기 위해 `0` 을 반환합니다.

`-4` 부터 0까지 카운트 하기 위해 존재하는 `Counter` 인스턴스와 함께 `TowardsZeroSource` 의 인스턴스를 사용할 수 있습니다. 카운터가 0에 도달하면 더이상 카운팅이 동작하지 않습니다:

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

## 프로토콜 확장 \(Protocol Extensions\)

<!--
Protocols can be extended to provide method, initializer, subscript, and computed property implementations to conforming types. This allows you to define behavior on protocols themselves, rather than in each type’s individual conformance or in a global function.

For example, the RandomNumberGenerator protocol can be extended to provide a randomBool() method, which uses the result of the required random() method to return a random Bool value:
-->

프로토콜은 타입을 준수하는 타입에 제공하기 위해 메서드, 초기화 구문, 서브 스크립트, 그리고 계산된 프로퍼티 구현을 확장될 수 있습니다. 이를 통해 각 타입의 개별 적합성 또는 전역 함수가 아닌 프로토콜 자체에 동작을 정의할 수 있습니다.

예를 들어 `RandomNumberGenerator` 프로토콜은 임의의 `Bool` 값을 반환하기 위해 필요한 `random()` 메서드의 결과를 사용하는 `randomBool()` 메서드를 제공하기 위해 확장될 수 있습니다:

```swift
extension RandomNumberGenerator {
    func randomBool() -> Bool {
        return random() > 0.5
    }
}
```

<!--
By creating an extension on the protocol, all conforming types automatically gain this method implementation without any additional modification.
-->

프로토콜 확장을 생성함으로써 모든 준수하는 타입은 추가 수정없이 메서드 구현을 자동으로 얻습니다.

```swift
let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// Prints "Here's a random number: 0.3746499199817101"
print("And here's a random Boolean: \(generator.randomBool())")
// Prints "And here's a random Boolean: true"
```

<!--
Protocol extensions can add implementations to conforming types but can’t make a protocol extend or inherit from another protocol. Protocol inheritance is always specified in the protocol declaration itself.
-->

프로토콜 확장은 준수하는 타입에 구현을 추가할 수 있지만 프로토콜을 확장하거나 다른 프로토콜을 상속할 수 없습니다. 프로토콜 상속은 항상 프로토콜 선언 자체에 지정됩니다.

### 기본 구현 제공 \(Providing Default Implementations\)

<!--
You can use protocol extensions to provide a default implementation to any method or computed property requirement of that protocol. If a conforming type provides its own implementation of a required method or property, that implementation will be used instead of the one provided by the extension.
-->

해당 프로토콜의 모든 메서드 또는 계산된 프로퍼티 요구사항에 기본 구현을 제공하기 위해 프로토콜 확장을 사용할 수 있습니다. 준수하는 타입이 필수 메서드 또는 프로퍼티의 자체 구현을 제공하면 해당 구현은 확장에 의해 제공되는 구현 대신 사용됩니다.

<!--
NOTE
Protocol requirements with default implementations provided by extensions are distinct from optional protocol requirements. Although conforming types don’t have to provide their own implementation of either, requirements with default implementations can be called without optional chaining.
-->

> NOTE   
> 확장에 의해 제공된 기본 구현을 가진 프로토콜 요구사항은 옵셔널 프로토콜 요구사항과 다릅니다. 준수하는 타입이 자체 구현을 제공할 필요는 없지만 기본 구현을 가진 요구사항은 옵셔널 체이닝 없이 호출될 수 있습니다.

<!--
For example, the PrettyTextRepresentable protocol, which inherits the TextRepresentable protocol can provide a default implementation of its required prettyTextualDescription property to simply return the result of accessing the textualDescription property:
-->

예를 들어 `TextRepresentable` 프로토콜을 상속하는 `PrettyTextRepresentable` 프로토콜은 `textualDescription` 프로퍼티 접근의 결과를 반환하기 위해 필요한 `prettyTextualDescription` 프로퍼티의 기본 구현을 제공할 수 있습니다:

```swift
extension PrettyTextRepresentable  {
    var prettyTextualDescription: String {
        return textualDescription
    }
}
```

### 프로토콜 확장에 제약사항 추가 \(Adding Constraints to Protocol Extensions\)

<!--
When you define a protocol extension, you can specify constraints that conforming types must satisfy before the methods and properties of the extension are available. You write these constraints after the name of the protocol you’re extending by writing a generic where clause. For more about generic where clauses, see Generic Where Clauses.

For example, you can define an extension to the Collection protocol that applies to any collection whose elements conform to the Equatable protocol. By constraining a collection’s elements to the Equatable protocol, a part of the standard library, you can use the == and != operators to check for equality and inequality between two elements.
-->

프로토콜 확장을 정의할 때 확장의 메서드와 프로퍼티를 사용할 수 있기 전에 준수하는 타입이 충족해야 하는 제약조건을 지정할 수 있습니다. 일반적인 `where` 절을 작성하여 확장하는 프로토콜의 이름 뒤에 제약조건을 작성합니다. 자세한 내용은 [제너릭 Where 절 \(Generic Where Clauses\)](generics.md#where-generic-where-clauses) 을 참고 바랍니다.

예를 들어 `Equatable` 프로토콜을 준수하는 항목의 모든 콜렉션에 적용하는 `Collection` 프로토콜의 확장을 정의할 수 있습니다. 콜렉션의 요소를 표준 라이브러리의 일부인 `Equatable` 프로토콜로 제한하면 두 요소간의 같음과 다름에 대한 확인을 위해 `==` 와 `!=` 연산자를 사용할 수 있습니다.

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
The allEqual() method returns true only if all the elements in the collection are equal.

Consider two arrays of integers, one where all the elements are the same, and one where they aren’t:
-->

`allEqual()` 메서드는 콜렉션에 모든 요소가 같을 때만 `true` 를 반환합니다.

모든 요소가 같고 하나만 다른 정수의 2개의 배열을 생각해 봅시다:

```swift
let equalNumbers = [100, 100, 100, 100, 100]
let differentNumbers = [100, 100, 200, 100, 200]
```

<!--
Because arrays conform to Collection and integers conform to Equatable, equalNumbers and differentNumbers can use the allEqual() method:
-->

배열은 `Collection` 을 준수하고 정수는 `Equatable` 을 준수하므로 `equalNumbers` 와 `differentNumbers` 는 `allEqual()` 메서드를 사용할 수 있습니다:

```swift
print(equalNumbers.allEqual())
// Prints "true"
print(differentNumbers.allEqual())
// Prints "false"
```

<!--
NOTE
If a conforming type satisfies the requirements for multiple constrained extensions that provide implementations for the same method or property, Swift uses the implementation corresponding to the most specialized constraints.
-->

> NOTE   
> 준수하는 타입이 같은 메서드 또는 프로퍼티에 대한 구현을 제공하는 여러 제약조건의 확장에 대한 요구사항을 충족한다면 Swift는 가장 전문화 된 제약조건에 해당하는 구현을 사용합니다.

