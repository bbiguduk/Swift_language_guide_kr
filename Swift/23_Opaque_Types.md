# 불투명한 타입 (Opaque Types)

불투명한 반환 타입이 있는 함수 또는 메서드는 반환값의 타입 정보를 가립니다. 함수의 반환 타입으로 구체적인 타입을 제공하는 대신에 반환값은 지원되는 프로토콜 측면에서 설명됩니다. 반환값의 기본 타입이 비공개로 유지될 수 있으므로 모듈과 모듈을 호출하는 코드 사이의 경계에서 타입 정보를 숨기는 것이 유용합니다. 타입이 프로토콜 타입 인 값을 반환하는 것과 달리 불투명한 타입은 타입 정체성을 보존합니다—컴파일러는 타입 정보에 접근할 수 있지만 모듈의 클라이언트는 그럴 수 없습니다.

## 불투명한 타입이 해결하는 문제 (The Problem That Opaque Types Solve)

예를 들어 ASCII 그림 모양을 그리는 모듈을 작성한다고 가정합시다. ASCII 그림 모양의 기본 특성은 `Shape` 프로토콜에 대한 요구사항으로 사용될 수 있는 모양의 문자열 표현을 반환하는 `draw()` 함수 입니다:

```swift
protocol Shape {
    func draw() -> String
}

struct Triangle: Shape {
    var size: Int
    func draw() -> String {
        var result = [String]()
        for length in 1...size {
            result.append(String(repeating: "*", count: length))
        }
        return result.joined(separator: "\n")
    }
}
let smallTriangle = Triangle(size: 3)
print(smallTriangle.draw())
// *
// **
// ***
```

아래 코드와 같이 제너릭을 사용하여 모양을 수직으로 뒤집는 것과 같은 작업을 구현할 수 있습니다. 그러나 이 접근 방식에는 중요한 제한이 있습니다: 뒤집힌 결과는 이를 생성하는데 정확한 제너릭 타입을 노출합니다.

```swift
struct FlippedShape<T: Shape>: Shape {
    var shape: T
    func draw() -> String {
        let lines = shape.draw().split(separator: "\n")
        return lines.reversed().joined(separator: "\n")
    }
}
let flippedTriangle = FlippedShape(shape: smallTriangle)
print(flippedTriangle.draw())
// ***
// **
// *
```

아래의 코드와 같이 두개의 모양을 수직으로 결합하는 `JoinedShape<T: Shape, U: Shape>` 구조체를 정의하는 이 접근방식은 뒤집힌 삼각형을 다른 삼각형과 결합하는 `JoinedShape<FlippedShape<Triangle>, Triangle>` 과 같은 타입을 생성합니다.

```swift
struct JoinedShape<T: Shape, U: Shape>: Shape {
    var top: T
    var bottom: U
    func draw() -> String {
        return top.draw() + "\n" + bottom.draw()
    }
}
let joinedTriangles = JoinedShape(top: smallTriangle, bottom: flippedTriangle)
print(joinedTriangles.draw())
// *
// **
// ***
// ***
// **
// *
```

모양 생성에 대한 자세한 내용을 표출하면 전체 반환 타입을 명시해야 하므로 ASCII 그림 모듈의 공개 인터페이스에 포함되지 않은 타입이 유출될 수 있습니다. 모듈내에 코드는 다양한 방법으로 같은 모양을 구축할 수 있으며 모양을 사용하는 모듈 바깥에서의 다른 코드는 변환 목록에 대한 세부 구현 정보를 고려할 필요가 없습니다. `JoinedShape` 와 `FlippedShape` 와 같은 래퍼 타입은 모듈의 사용자에게 중요하지 않으며 표시되지 않아야 합니다. 모듈의 공개 인터페이스는 모양을 결합하고 뒤집는 것과 같은 작업으로 구성되며 이러한 작업은 다른 `Shape` 값을 반환합니다.

## 불투명한 타입 반환 (Returning an Opaque Type)

불투명한 타입은 제너릭 타입과 반대라고 생각할 수 있습니다. 제너릭 타입을 사용하면 함수를 호출하는 코드에서 해당 함수의 파라미터에 대한 타입을 선택하고 함수 구현에서 추상화된 방식으로 값을 반환할 수 있습니다. 예를 들어 다음 코드의 함수는 호출자에 따라 달라지는 타입을 반환합니다:

```swift
func max<T>(_ x: T, _ y: T) -> T where T: Comparable { ... }
```

`max(_:_:)` 호출하는 코드는 `x` 와 `y` 에 대한 값을 선택하고 이 값의 타입에 따라 `T` 의 구체적인 타입을 결정합니다. 코드 호출은 `Comparable` 프로토콜을 준수하는 모든 타입을 사용할 수 있습니다. 함수 내부의 코드는 일반적인 방식으로 작성되므로 호출자가 제공하는 타입이 무엇이든 처리할 수 있습니다. `max(_:_:)` 의 구현은 모든 `Comparable` 타입이 공유하는 기능만 사용합니다.

불투명한 반환 타입을 사용하는 함수의 경우 이러한 역할이 반전됩니다. 불투명한 타입을 사용하면 함수 구현에서 함수를 호출하는 코드에서 추상화 된 방식으로 반환되는 값의 타입을 선택할 수 있습니다. 예를 들어 다음의 예제에서 함수는 모양의 기본 타입을 노출하지 않고 사다리꼴을 반환합니다.

```swift
struct Square: Shape {
    var size: Int
    func draw() -> String {
        let line = String(repeating: "*", count: size)
        let result = Array<String>(repeating: line, count: size)
        return result.joined(separator: "\n")
    }
}

func makeTrapezoid() -> some Shape {
    let top = Triangle(size: 2)
    let middle = Square(size: 2)
    let bottom = FlippedShape(shape: top)
    let trapezoid = JoinedShape(
        top: top,
        bottom: JoinedShape(top: middle, bottom: bottom)
    )
    return trapezoid
}
let trapezoid = makeTrapezoid()
print(trapezoid.draw())
// *
// **
// **
// **
// **
// *
```

이 예제에서 `makeTrapezoid()` 함수는 `some Shape` 로 반환 타입을 선언하고 그 결과 이 함수는 특정 구체적인 타입을 지정하지 않고 `Shape` 프로토콜을 준수하는 특정 타입의 값을 반환합니다. 이러한 방식으로 `makeTrapezoid()` 를 작성하면 공용 인터페이스의 부분으로 만들어지는 모양을 타입으로 지정하지 않고도 반환하는 값이 모양 인 공용 인터페이스의 기본 측면을 표현할 수 있습니다. 이 구현은 2개의 삼각형과 하나의 사각형을 사용하지만 이 함수는 반환 타입을 변경하지 않고 다양한 방법으로 사다리꼴을 그리기 위해 다시 작성할 수 있습니다.

이 예제는 불투명한 반환 타입은 제너릭 타입의 반대와 같은 방식을 강조합니다. `makeTrapezoid()` 안에 코드는 호출하는 코드가 제너릭 함수처럼 해당 타입이 `Shape` 프로토콜을 준수하는 한 필요한 모든 타입을 반환할 수 있습니다. 함수를 호출하는 코드는 `makeTrapezoid()` 에 의해 반환된 모든 `Shape` 값과 함께 동작할 수 있도록 제너릭 함수의 구현과 같이 일반적인 방식으로 작성되어야 합니다.

불투명한 반환 타입은 제너릭과 결합할 수도 있습니다. 다음 코드의 함수는 모두 `Shape` 프로토콜을 준수하는 일부 타입의 값을 반환합니다.

```swift
func flip<T: Shape>(_ shape: T) -> some Shape {
    return FlippedShape(shape: shape)
}
func join<T: Shape, U: Shape>(_ top: T, _ bottom: U) -> some Shape {
    JoinedShape(top: top, bottom: bottom)
}

let opaqueJoinedTriangles = join(smallTriangle, flip(smallTriangle))
print(opaqueJoinedTriangles.draw())
// *
// **
// ***
// ***
// **
// *
```

이 예제의 `opaqueJoinedTriangles` 의 값은 이 챕터의 이전 [불투명한 타입이 해결하는 문제 (The Problem That Opaque Types Solve)](https://docs.swift.org/swift-book/LanguageGuide/OpaqueTypes.html#ID613) 섹션에 제너릭 예제에서 `joinedTriangles` 와 동일합니다. 그러나 이 예제의 값과 달리 `flip(_:)` 과 `join(_:_:)` 은 불투명한 반환 타입으로 반환하는 제너릭 모양 연산자 인 기본 타입을 래핑하여 해당 타입이 표시되지 않도록 합니다. 두 함수는 의존하는 타입이 제너릭이고 함수에 대한 타입 파라미터가 `FlippedShape` 와 `JoinedShape` 에 필요한 타입 정보를 전달하기 때문에 모두 제너릭입니다.

불투명한 반환 타입을 가진 함수가 여러 위치에서 반환하는 경우 가능한 모든 반환 값은 동일한 타입을 가져야 합니다. 제너릭 함수의 경우 해당 반환 타입은 함수의 제너릭 타입 파라미터로 사용할 수 있지만 여전히 단일 타입이어야 합니다. 예를 들어 다음은 사각형에 대한 특수 케이스를 포함하는 잘못된 버전의 모양-뒤집기 함수 입니다:

```swift
func invalidFlip<T: Shape>(_ shape: T) -> some Shape {
    if shape is Square {
        return shape // Error: return types don't match
    }
    return FlippedShape(shape: shape) // Error: return types don't match
}
```

이 함수를 `Square` 와 함께 호출하면 `Square` 를 반한하고 그렇지 않으면 `FlippedShape` 를 반환합니다. 이것은 단일 타입의 값을 반환해야 한다는 요구사항을 위반하고 `invalidFlip(_:)` 코드를 유효하지 않게 만듭니다. `invalidFlip(_:)` 을 고치기 위한 한가지 방법은 사각형의 특수한 경우를 `FlippedShape` 의 구현으로 옮기는 것입니다. 이렇게 하면 이 함수는 항상 `FlippedShape` 값을 반환할 수 있습니다:

```swift
struct FlippedShape<T: Shape>: Shape {
    var shape: T
    func draw() -> String {
        if shape is Square {
            return shape.draw()
        }
        let lines = shape.draw().split(separator: "\n")
        return lines.reversed().joined(separator: "\n")
    }
}
```

항상 단일 타입을 반환해야 한다고 해서 불투명 반환 타입에 제너릭 사용을 막지는 않습니다. 다음은 반환하는 값의 기본 타입에 타입 파라미터를 통합하는 함수의 예입니다:

```swift
func `repeat`<T: Shape>(shape: T, count: Int) -> some Collection {
    return Array<T>(repeating: shape, count: count)
}
```

이 경우에 반환값의 기본 타입은 `T` 에 따라 달라집니다: 전달되는 모양이 무엇이든 `repeat(shape:count:)` 는 해당 모양의 배열을 생성하고 반환합니다. 그럼에도 불구하고 반환값은 항상 동일한 기본 타입 인 `[T]` 를 가지므로 불투명한 반환 타입을 가진 함수는 단일 타입의 값만 가져야 한다는 요구사항을 따릅니다.

## 불투명한 타입과 프로토콜 타입의 차이점 (Differences Between Opaque Types and Protocol Types)

불투명한 타입을 반환하는 것은 함수의 반환 타입으로 프로토콜 타입을 사용하는 것과 매우 유사하지만 이 두 종류의 반환 타입은 타입 정체성을 유지하는지 여부가 다릅니다. 불투명한 타입은 하나의 특정 타입을 참조하지만 함수 호출자는 어떤 타입인지 볼 수 없습니다. 프로토콜 타입은 프로토콜을 준수하는 모든 타입을 참조할 수 있습니다. 일반적으로 프로토콜 타입은 저장하는 값의 기본 타입에 대해 더 많은 유연성을 제공하고 불투명한 타입을 사용하면 이러한 기본 타입에 대해 더 강력한 보증을 할 수 있습니다.

예를 들어 다음은 불투명한 반환 타입 대신에 반환 타입으로 프로토콜 타입을 사용하는 `flip(_:)` 의 버전입니다:

```swift
func protoFlip<T: Shape>(_ shape: T) -> Shape {
    return FlippedShape(shape: shape)
}
```

`protoFlip(_:)` 의 버전은 `flip(_:)` 과 같은 바디를 가지고 항상 같은 타입의 값을 반환합니다. `flip(_:)` 과 다르게 `protoFlip(_:)` 이 반환하는 값은 항상 같은 타입을 가질 필요가 없으며 `Shape` 프로토콜을 준수하기만 하면 됩니다. 달리 말하면 `protoFlip(_:)` 은 `flip(_:)` 이 만든 것보다 더 느슨한 API 계약을 만듭니다. 이것은 여러 타입의 값을 반환하는 유연성을 보유합니다:

```swift
func protoFlip<T: Shape>(_ shape: T) -> Shape {
    if shape is Square {
        return shape
    }

    return FlippedShape(shape: shape)
}
```

개정된 코드의 버전은 전달되는 모양에 따라 `Shape` 인스턴스 또는 `FlippedShape` 인스턴스를 반환합니다. 이 함수에 의해 반환된 2개의 뒤집힌 모양은 완전히 다른 타입을 가집니다. 이 함수의 다른 유효한 버전은 같은 모양의 여러 인스턴스를 뒤집을 때 다른 타입의 값을 반환할 수 있습니다. `protoFlip(_:)` 의 구체적이지 않은 반환 타입은 타입 정보에 의존하는 많은 작업이 반환된 값에서 사용할 수 없음을 의미합니다. 예를 들어 이 함수에 의해 반환된 결과를 비교하는 `==` 연산자를 작성할 수 없습니다.

```swift
let protoFlippedTriangle = protoFlip(smallTriangle)
let sameThing = protoFlip(smallTriangle)
protoFlippedTriangle == sameThing  // Error
```

예제의 마지막 라인에서 몇가지 이유로 에러가 발생합니다. 즉각적인 문제는 `Shape` 는 프로토콜 요구사항의 부분으로 `==` 연산자를 포함하지 않습니다. 이것을 추가하려고 하면 다음 문제는 `==` 연산자는 좌항과 우항 인수의 타입을 알아야 합니다. 이러한 종류의 연산자는 일반적으로 프로토콜을 채택하는 구체적인 타입과 일치하는 `Self` 타입의 인수를 사용하지만 프로토콜에 `Self` 요구사항을 추가하면 프로토콜을 타입으로 사용할 때 발생하는 타입 삭제가 허용되지 않습니다.

함수의 반환 타입으로 프로토콜 타입을 사용하면 프로토콜을 준수하는 모든 타입을 유연하게 반환할 수 있습니다. 그러나 이러한 유연성의 대가는 반환된 값에 대해 일부 작업을 수행할 수 없습니다. 이 예제는 프로토콜 타입을 사용하여 보존되지 않는 특정 타입 정보에 따라 `==` 연산자를 사용할 수 없는 것을 보여줍니다.

이 접근 방식의 다른 문제는 모양 변형이 중첩되지 않습니다. 삼각형을 뒤집은 결과는 `Shape` 타입의 값이고 `protoFlip(_:)` 함수는 `Shape` 프로토콜을 준수하는 일부 타입의 인수를 가집니다. 그러나 프로토콜 타입의 값은 `protoFlip(_:)` 에 의해 반환된 값은 `Shape` 를 준수하지 않으므로 프로토콜을 준수하지 않습니다. 여러 변형을 적용하는 `protoFlip(protoFlip(smallTriangle))` 같은 코드는 뒤집힌 모양이 `protoFlip(_:)` 에 대해 유효하지 않은 인수이므로 유효하지 않습니다.

반대로 불투명한 타입은 기본 타입의 정체성을 보존합니다. Swift는 연관된 타입을 유추할 수 있으므로 프로토콜 타입을 반환값으로 사용할 수 없는 위치에 불투명한 반환값을 사용할 수 있습니다. 예를 들어 다음은 [제너릭 (Generics)](https://docs.swift.org/swift-book/LanguageGuide/Generics.html) 의 `Container` 프로토콜의 버전입니다:

```swift
protocol Container {
    associatedtype Item
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
extension Array: Container { }
```

프로토콜은 연관된 타입을 가지고 있으므로 함수의 반환 타입으로 `Container` 를 사용할 수 없습니다. 또한 제너릭 타입이 필요한지 추론하기 위해 함수 바디의 외부에 충분한 정보가 없으므로 제너릭 반환 타입의 제약조건으로 사용할 수 없습니다.

```swift
// Error: Protocol with associated types can't be used as a return type.
func makeProtocolContainer<T>(item: T) -> Container {
    return [item]
}

// Error: Not enough information to infer C.
func makeProtocolContainer<T, C: Container>(item: T) -> C {
    return [item]
}
```

반환 타입으로 불투명한 타입 `some Container` 을 사용하면 함수는 컨테이너를 반환하지만 컨테이너의 타입 지정을 거부하므로 원하는 API가 표현됩니다:

```swift
func makeOpaqueContainer<T>(item: T) -> some Container {
    return [item]
}
let opaqueContainer = makeOpaqueContainer(item: 12)
let twelve = opaqueContainer[0]
print(type(of: twelve))
// Prints "Int"
```

`twelve` 의 타입은 `Int` 로 유추되며 이것은 불투명한 타입이 타입 추론이 동작한다는 것을 보여줍니다. `makeOpaqueContainer(item:)` 의 구현에서 불투명한 컨테이너의 기본 타입은 `[T]` 입니다. 이 경우에 `T` 는 `Int` 이므로 반환값은 정수의 배열이고 `Item` 연관된 타입은 `Int` 로 유추됩니다. `Container` 에서 서브 스크립트는 `twelve` 의 타입도 `Int` 로 유추된다는 의미의 `Item` 을 반환합니다.