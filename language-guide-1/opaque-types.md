# 불투명 타입과 박싱 프로토콜 타입 (Opaque and Boxed Protocol Types)

값의 타입에 대한 자세한 구현을 숨깁니다.

Swift는 값의 타입에 대한 정보를 숨기는 두 가지 방법을 제공합니다:
불투명 타입(opaque type)과 박싱 프로토콜 타입(boxed protocol type)입니다.
반환값의 실제 타입이 비공개로 유지될 수 있으므로
모듈과 모듈을 호출하는 코드
사이의 경계에서
타입 정보를 숨기는 것이 유용합니다. 

불투명 타입을 반환하는 함수나 메서드는
반환하는 값의 타입 정보를 숨깁니다.
함수의 반환 타입으로 구체적인 타입을 제공하는 대신에
반환값이 어떤 프로토콜을 준수하는지 작성합니다.
불투명 타입은 타입 식별을 유지합니다 —--
컴파일러는 타입 정보에 접근할 수 있지만
모듈의 클라이언트는 그럴 수 없습니다.

박싱 프로토콜 타입은 주어진 프로토콜을 준수하는
타입의 인스턴스를 저장할 수 있습니다.
박싱 프로토콜 타입은 타입 식별을 유지하지 않습니다 —--
값의 구체적인 타입은 런타임까지 알 수 없으며,
저장되는 값에 따라 타입도 변경될 수 있습니다.

## 불투명 타입이 해결하는 문제 (The Problem that Opaque Types Solve)

예를 들어
ASCII 도형을 그리는 모듈을 작성한다고 가정합시다.
ASCII 도형의 기본 특성은
도형을 문자열로 표현하는 `draw()` 함수이며,
이 함수는 `Shape` 프로토콜의 요구사항으로 정의할 수 있습니다:

```swift
protocol Shape {
    func draw() -> String
}

struct Triangle: Shape {
    var size: Int
    func draw() -> String {
        var result: [String] = []
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

<!--
  - test: `opaque-result`

  ```swifttest
  -> protocol Shape {
         func draw() -> String
     }

  -> struct Triangle: Shape {
        var size: Int
        func draw() -> String {
            var result: [String] = []
            for length in 1...size {
                result.append(String(repeating: "*", count: length))
            }
            return result.joined(separator: "\n")
        }
     }
  -> let smallTriangle = Triangle(size: 3)
  -> print(smallTriangle.draw())
  </ *
  </ **
  </ ***
  ```
-->

아래 코드와 같이
제네릭을 사용하여 모양을 수직으로 뒤집는 것과 같은 작업을 구현할 수 있습니다.
그러나 이 접근방식에는 한계가 있습니다:
뒤집힌 결과의 타입은 이를 생성하는데 사용된
구체적인 제네릭 타입을 노출합니다.

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

<!--
  - test: `opaque-result`

  ```swifttest
  -> struct FlippedShape<T: Shape>: Shape {
         var shape: T
         func draw() -> String {
             let lines = shape.draw().split(separator: "\n")
             return lines.reversed().joined(separator: "\n")
         }
     }
  -> let flippedTriangle = FlippedShape(shape: smallTriangle)
  -> print(flippedTriangle.draw())
  </ ***
  </ **
  </ *
  ```
-->

아래의 코드와 같이 두 개의 모양을 수직으로 결합하는
`JoinedShape<T: Shape, U: Shape>` 구조체를 정의하면
뒤집힌 삼각형을 다른 삼각형과 결합하는 
`JoinedShape<Triangle, FlippedShape<Triangle>>`과 같은 타입을 생성합니다.

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

<!--
  - test: `opaque-result`

  ```swifttest
  -> struct JoinedShape<T: Shape, U: Shape>: Shape {
        var top: T
        var bottom: U
        func draw() -> String {
            return top.draw() + "\n" + bottom.draw()
        }
     }
  -> let joinedTriangles = JoinedShape(top: smallTriangle, bottom: flippedTriangle)
  -> print(joinedTriangles.draw())
  </ *
  </ **
  </ ***
  </ ***
  </ **
  </ *
  ```
-->

도형 생성에 대한 자세한 내용을 노출하면
ASCII 도형 모듈의 공개 인터페이스에
포함되지 않은 타입이
반환 타입을 명시해야 하므로 유출될 수 있습니다.
모듈 내에 코드는
다양한 방법으로 같은 도형을 구축할 수 있으며,
도형을 사용하는 모듈 외부에서의 다른 코드는
변환 목록에 대한 세부 구현 정보를
고려할 필요가 없습니다.
`JoinedShape`와 `FlippedShape` 같은 래퍼 타입은
모듈 사용자에게 중요하지 않으므로,
노출되지 않아야 합니다.
모듈의 공개 인터페이스는
도형을 결합하고 뒤집는 것과 같은 작업으로 구성되며,
이러한 작업은 다른 `Shape` 값을 반환합니다.

## 불투명 타입 반환 (Returning an Opaque Type)

불투명 타입은 제네릭 타입과 반대라고 생각할 수 있습니다.
제네릭 타입을 사용하면 함수를 호출하는 코드에서
해당 함수의 매개변수에 대한 타입을 선택하고
함수 구현에서 추상화된 방식으로 값을 반환할 수 있습니다.
예를 들어 다음 코드의 함수는
호출자에 따라 달라지는 타입을 반환합니다:

```swift
func max<T>(_ x: T, _ y: T) -> T where T: Comparable { ... }
```

<!--
  From https://developer.apple.com/documentation/swift/1538951-max
  Not test code because it won't actually compile
  and there's nothing to meaningfully test.
-->

`max(_:_:)` 호출하는 코드는 `x`와 `y`에 대한 값을 선택하고,
이 값의 타입에 따라 `T`의 구체적인 타입을 결정합니다.
코드 호출은
`Comparable` 프로토콜을 준수하는 모든 타입을 사용할 수 있습니다.
함수 내부의 코드는 일반적인 방식으로 작성되므로
호출자가 제공하는 타입이 무엇이든 처리할 수 있습니다.
`max(_:_:)`의 구현은
모든 `Comparable` 타입이 공유하는 기능만 사용합니다.

불투명 반환 타입을 사용하는 함수의 경우 이러한 역할이 반전됩니다.
불투명 타입을 사용하면 함수 구현에서
함수를 호출하는 코드에서 추상화된 방식으로
반환되는 값의 타입을 선택할 수 있습니다.
예를 들어 다음의 예시에서 함수는
도형의 실제 타입을 노출하지 않고 사다리꼴을 반환합니다.

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

<!--
  - test: `opaque-result`

  ```swifttest
  -> struct Square: Shape {
         var size: Int
         func draw() -> String {
             let line = String(repeating: "*", count: size)
             let result = Array<String>(repeating: line, count: size)
             return result.joined(separator: "\n")
         }
     }

  -> func makeTrapezoid() -> some Shape {
         let top = Triangle(size: 2)
         let middle = Square(size: 2)
         let bottom = FlippedShape(shape: top)
         let trapezoid = JoinedShape(
             top: top,
             bottom: JoinedShape(top: middle, bottom: bottom)
         )
         return trapezoid
     }
  -> let trapezoid = makeTrapezoid()
  -> print(trapezoid.draw())
  </ *
  </ **
  </ **
  </ **
  </ **
  </ *
  ```
-->

이 예시에서 `makeTrapezoid()` 함수는
`some Shape`로 반환 타입을 선언하고;
그 결과로 함수는
구체적인 타입을 지정하지 않고
`Shape` 프로토콜을 준수하는 특정 타입의 값을 반환합니다.
이러한 방식으로 `makeTrapezoid()`를 작성하면
공용 인터페이스로
만들어지는 도형을 타입으로 지정하지 않고도
반환하는 값이 도형인
공용 인터페이스의 본질적인 측면을 표현할 수 있습니다.
이 구현은 두 개의 삼각형과 하나의 사각형을 사용하지만,
이 함수는 반환 타입을 변경하지 않고
다양한 방법으로
다시 작성할 수 있습니다.

이 예시는 불투명 반환 타입이
제네릭 타입과 반대 역할임을 보여줍니다.
`makeTrapezoid()` 안에 코드는
필요한 모든 타입을 반환할 수 있지만
반환 타입이 `Shape` 프로토콜을 준수해야 합니다.
함수를 호출하는 코드는
`makeTrapezoid()`에 의해 반환된
모든 `Shape` 값이 동작할 수 있도록
제네릭 함수의 구현과 같이 일반적인 방식으로 작성되어야 합니다.

불투명 반환 타입은 제네릭과 결합할 수도 있습니다.
다음 코드의 함수는 모두
`Shape` 프로토콜을 준수하는 어떤 타입의 값을 반환합니다.

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

<!--
  - test: `opaque-result`

  ```swifttest
  -> func flip<T: Shape>(_ shape: T) -> some Shape {
         return FlippedShape(shape: shape)
     }
  -> func join<T: Shape, U: Shape>(_ top: T, _ bottom: U) -> some Shape {
         JoinedShape(top: top, bottom: bottom)
     }

  -> let opaqueJoinedTriangles = join(smallTriangle, flip(smallTriangle))
  -> print(opaqueJoinedTriangles.draw())
  </ *
  </ **
  </ ***
  </ ***
  </ **
  </ *
  ```
-->

이 예시의 `opaqueJoinedTriangles` 값은
[불투명 타입이 해결하는 문제 (The Problem that Opaque Types Solve)](#불투명-타입이-해결하는-문제-the-problem-that-opaque-types-solve) 섹션의
제네릭 예시에서 `joinedTriangles`와 동일합니다.
그러나 이 예시의 값과 다르게
`flip(_:)`과 `join(_:_:)`은
반환하는 제네릭 도형 연산자인 실제 타입을 래핑하여
불투명 반환 타입으로
해당 타입이 표시되지 않도록 합니다.
두 함수는 의존하는 타입이 제네릭이고 함수의 타입 매개변수가
`FlippedShape`와 `JoinedShape`에 필요한 타입 정보를 전달하기 때문에
모두 제네릭입니다.

불투명 반환 타입을 가진 함수가
여러 위치에서 반환하는 경우
가능한 모든 반환값은 동일한 타입을 가져야 합니다.
제네릭 함수의 경우
해당 반환 타입은 함수의 제네릭 타입 매개변수로 사용할 수 있지만
여전히 단일 타입이어야 합니다.
예를 들어
다음은 사각형에 대한 특수 케이스를 포함하는
*유효하지 않은* 버전의 도형 뒤집기 함수입니다:

```swift
func invalidFlip<T: Shape>(_ shape: T) -> some Shape {
    if shape is Square {
        return shape // Error: return types don't match
    }
    return FlippedShape(shape: shape) // Error: return types don't match
}
```

<!--
  - test: `opaque-result-err`

  ```swifttest
  >> protocol Shape {
  >>     func draw() -> String
  >> }
  >> struct Square: Shape {
  >>     func draw() -> String { return "#" }  // stub implementation
  >> }
  >> struct FlippedShape<T: Shape>: Shape {
  >>     var shape: T
  >>     func draw() -> String { return "#" } // stub implementation
  >> }
  -> func invalidFlip<T: Shape>(_ shape: T) -> some Shape {
         if shape is Square {
             return shape // Error: return types don't match
         }
         return FlippedShape(shape: shape) // Error: return types don't match
     }
  !$ error: function declares an opaque return type 'some Shape', but the return statements in its body do not have matching underlying types
  !! func invalidFlip<T: Shape>(_ shape: T) -> some Shape {
  !!      ^                                    ~~~~~~~~~~
  !$ note: return statement has underlying type 'T'
  !! return shape // Error: return types don't match
  !! ^
  !$ note: return statement has underlying type 'FlippedShape<T>'
  !! return FlippedShape(shape: shape) // Error: return types don't match
  !! ^
  ```
-->

이 함수를 `Square`로 호출하면 `Square`를 반환하고,
그렇지 않으면 `FlippedShape`를 반환합니다.
이것은 단일 타입의 값을 반환해야 한다는 요구사항을 위반하고
`invalidFlip(_:)` 코드는 유효하지 않습니다.
`invalidFlip(_:)`을 고치기 위한 한 가지 방법은 사각형의 특수한 경우를
`FlippedShape`의 구현 내부로 옮겨서
함수가 항상 `FlippedShape` 값을 반환하도록 합니다:

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

<!--
  - test: `opaque-result-special-flip`

  ```swifttest
  >> protocol Shape { func draw() -> String }
  >> struct Square: Shape {
  >>     func draw() -> String { return "#" }  // stub implementation
  >> }
  -> struct FlippedShape<T: Shape>: Shape {
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
-->

<!--
  Another way to fix it is with type erasure.
  Define a wrapper called AnyShape,
  and wrap whatever shape you created inside invalidFlip(_:)
  before returning it.
  That example is long enough that it breaks the flow here.
-->

항상 단일 타입을 반환해야 한다고 해서
불투명 반환 타입에 제네릭 사용을 막지는 않습니다.
다음은 함수의 타입 매개변수가
반환값의 구체적인 타입에 포함되는 예입니다:

```swift
func `repeat`<T: Shape>(shape: T, count: Int) -> some Collection {
    return Array<T>(repeating: shape, count: count)
}
```

<!--
  - test: `opaque-result`

  ```swifttest
  -> func `repeat`<T: Shape>(shape: T, count: Int) -> some Collection {
         return Array<T>(repeating: shape, count: count)
     }
  ```
-->

이 경우에
반환값의 구체적인 타입은
`T`에 따라 달라집니다:
전달되는 도형이 무엇이든
`repeat(shape:count:)`는 해당 도형의 배열을 생성하고 반환합니다.
그럼에도 불구하고
반환값의 구체적인 타입은 `[T]`를 가지므로
불투명 반환 타입을 가지는 함수는 단일 타입의 값만 가져야 한다는
요구사항을 충족합니다.

## 박싱 프로토콜 타입 (Boxed Protocol Types)

박싱 프로토콜 타입(Boxed protocol type)은
"프로토콜을 준수하는 *T* 타입이 존재합니다."라는 문구와 같이
*존재적 타입(existential type)*이라고도 합니다.
박싱 프로토콜을 만들려면
프로토콜의 이름 앞에 `any`를 붙입니다.
예를 들면 다음과 같습니다:

```swift
struct VerticalShapes: Shape {
    var shapes: [any Shape]
    func draw() -> String {
        return shapes.map { $0.draw() }.joined(separator: "\n\n")
    }
}

let largeTriangle = Triangle(size: 5)
let largeSquare = Square(size: 5)
let vertical = VerticalShapes(shapes: [largeTriangle, largeSquare])
print(vertical.draw())
```

<!--
  - test: `boxed-protocol-types`

  ```swifttest
   >> protocol Shape {
   >>    func draw() -> String
   >> }
   >> struct Triangle: Shape {
   >>    var size: Int
   >>    func draw() -> String {
   >>        var result: [String] = []
   >>        for length in 1...size {
   >>            result.append(String(repeating: "*", count: length))
   >>        }
   >>        return result.joined(separator: "\n")
   >>    }
   >> }
   >> struct Square: Shape {
   >>     var size: Int
   >>     func draw() -> String {
   >>         let line = String(repeating: "*", count: size)
   >>         let result = Array<String>(repeating: line, count: size)
   >>         return result.joined(separator: "\n")
   >>     }
   >
   -> struct VerticalShapes: Shape {
          var shapes: [any Shape]
          func draw() -> String {
              return shapes.map { $0.draw() }.joined(separator: "\n\n")
          }
      }
   ->
   -> let largeTriangle = Triangle(size: 5)
   -> let largeSquare = Square(size: 5)
   -> let vertical = VerticalShapes(shapes: [largeTriangle, largeSquare])
   -> print(vertical.draw())
   << *
   << **
   << ***
   << ****
   << *****
   <<-
   << *****
   << *****
   << *****
   << *****
   << *****
  ```
-->

위의 예시에서
`VerticalShapes`는 `[any Shape]`로 `shapes`의 타입을 선언합니다 ---
박싱된 `Shape` 요소의 배열입니다.
배열에 각 요소는 다른 타입이 올 수 있고,
각 타입은 `Shape` 프로토콜을 준수해야 합니다.
이런 런타임 유연성을 제공하기위해
Swift는 필요할 때 간접 계층을 추가합니다 ---
이런 간접 계층을 *박스(box)*라고 부르며,
성능 비용(performance cost)이 발생할 수 있습니다.

`VerticalShapes` 타입에는
`Shape` 프로토콜이 요구하는
메서드, 프로퍼티, 서브스크립트를 사용할 수 있습니다.
예를 들어, `VerticalShapes`의 `draw()` 메서드는
배열의 각 요소에서 `draw()` 메서드를 호출합니다.
`Shape`는 `draw()` 메서드를 요구하므로
이 메서드는 사용 가능합니다.
반대로
`Shape`에 의해 요구되지 않은 삼각형(triangle)의 `size` 프로퍼티 또는
다른 프로퍼티나 메서드를 접근하려고 하면 오류가 발생합니다.

`shapes`를 사용할 수 있는 세 가지 타입이 있습니다:

- `struct VerticalShapes<S: Shape>`와 `var shapes: [S]`을 작성하여
  제네릭을 사용하면,
  배열의 모든 요소가 동일한 도형 타입이 되고,
  그 타입의 식별자는
  외부 코드에 노출됩니다.

- `var shapes: [some Shape]`을 작성하여
  불투명 타입을 사용하면,
  배열의 모든 요소가 동일한 도형 타입이 되고
  그 타입의 식별자는 숨겨집니다.

- `var shapes: [any Shape]`을 작성하여 
  박싱 프로토콜 타입을 사용하면,
  배열에 서로 다른 타입의 요소를 저장할 수 있고,
  각 타입의 식별자는 숨겨집니다.

이 경우
박싱 프로토콜 타입은
`VerticalShapes`의 호출자가 다른 종류의 도형을 혼합할 수 있는 유일한 접근방식입니다.

박싱 값(boxed value)의 타입을 알고 있는 경우에
`as` 변환을 사용할 수 있습니다.
예를 들어:

```swift
if let downcastTriangle = vertical.shapes[0] as? Triangle {
    print(downcastTriangle.size)
}
// Prints "5"
```

더 자세한 내용은 [다운 캐스팅 (Downcasting)](./type-casting.md#다운-캐스팅-downcasting)을 참고바랍니다.

## 불투명 타입과 박싱 프로토콜 타입의 차이점 (Differences Between Opaque Types and Boxed Protocol Types)

불투명 타입을 반환하는 것은
함수의 반환 타입으로 박싱 프로토콜 타입을 사용하는 것과 매우 유사하지만,
이 두 종류의 반환 타입은
타입 정체성을 유지하는지 여부에서 차이가 있습니다.
불투명 타입은 하나의 특정 타입을 참조하지만,
함수를 호출하는 쪽에서는 그 타입을 알 수 없습니다;
박싱 프로토콜 타입은 프로토콜을 준수하는 모든 타입을 참조할 수 있습니다.
일반적으로
박싱 프로토콜 타입은
저장하는 값의 타입에 대해 더 많은 유연성을 제공하고
불투명 타입을 사용하면
이러한 타입에 대해 더 강력한 보장을 할 수 있습니다.

예를 들어
다음은 불투명 반환 타입 대신에
반환 타입으로 박싱 프로토콜 타입을 사용하는
`flip(_:)`의 버전입니다:

```swift
func protoFlip<T: Shape>(_ shape: T) -> Shape {
    return FlippedShape(shape: shape)
}
```

<!--
  - test: `opaque-result-existential-error`

  ```swifttest
  >> protocol Shape {
  >>     func draw() -> String
  >> }
  >> struct Triangle: Shape {
  >>     var size: Int
  >>     func draw() -> String { return "#" }  // stub implementation
  >> }
  >> struct Square: Shape {
  >>     var size: Int
  >>     func draw() -> String { return "#" }  // stub implementation
  >> }
  >> struct FlippedShape<T: Shape>: Shape {
  >>     var shape: T
  >>     func draw() -> String { return "#" } // stub implementation
  >> }
  -> func protoFlip<T: Shape>(_ shape: T) -> Shape {
        return FlippedShape(shape: shape)
     }
  ```
-->

`protoFlip(_:)`의 버전은
`flip(_:)`과 동일한 본문을 가지고,
항상 동일한 타입의 값을 반환합니다.
`flip(_:)`과 다르게
`protoFlip(_:)`이 반환하는 값은
항상 동일한 타입을 가질 필요가 없으며 ---
`Shape` 프로토콜을 준수하기만 하면 됩니다.
다르게 말하면
`protoFlip(_:)`은 `flip(_:)`이 만든 것보다
더 느슨한 API 계약을 가집니다.
이것은 여러 타입의 값을 반환하는 유연성을 보유합니다:

```swift
func protoFlip<T: Shape>(_ shape: T) -> Shape {
    if shape is Square {
        return shape
    }

    return FlippedShape(shape: shape)
}
```

<!--
  - test: `opaque-result-existential-error`

  ```swifttest
  -> func protoFlip<T: Shape>(_ shape: T) -> Shape {
        if shape is Square {
           return shape
        }

        return FlippedShape(shape: shape)
     }
  !$ error: invalid redeclaration of 'protoFlip'
  !! func protoFlip<T: Shape>(_ shape: T) -> Shape {
  !!      ^
  !$ note: 'protoFlip' previously declared here
  !! func protoFlip<T: Shape>(_ shape: T) -> Shape {
  !!      ^
  ```
-->

개정된 코드의 버전은
전달되는 도형에 따라
`Shape` 인스턴스나 `FlippedShape` 인스턴스를 반환합니다.
이 함수에 의해 반환된 두 개의 뒤집힌 모양은
완전히 다른 타입을 가집니다.
이 함수의 다른 유효한 버전은 같은 도형을 여러 번 뒤집을 때도
다른 타입의 값을 반환할 수 있습니다.
`protoFlip(_:)`의 구체적이지 않은 반환 타입은
타입 정보에 의존하는 많은 작업이
반환된 값에서 사용할 수 없음을 의미합니다.
예를 들어 이 함수에 의해 반환된 결과를 비교하는
`==` 연산자를 사용할 수 없습니다.

```swift
let protoFlippedTriangle = protoFlip(smallTriangle)
let sameThing = protoFlip(smallTriangle)
protoFlippedTriangle == sameThing  // Error
```

<!--
  - test: `opaque-result-existential-error`

  ```swifttest
  >> let smallTriangle = Triangle(size: 3)
  -> let protoFlippedTriangle = protoFlip(smallTriangle)
  -> let sameThing = protoFlip(smallTriangle)
  -> protoFlippedTriangle == sameThing  // Error
  !$ error: binary operator '==' cannot be applied to two 'any Shape' operands
  !! protoFlippedTriangle == sameThing  // Error
  !! ~~~~~~~~~~~~~~~~~~~~ ^  ~~~~~~~~~
  ```
-->

예시의 마지막 라인에서 몇 가지 이유로 오류가 발생합니다.
즉각적인 문제는 `Shape`는 프로토콜 요구사항으로
`==` 연산자를 포함하지 않습니다.
이것을 추가하려고 하면 다음 문제는
`==` 연산자는
좌변과 우변 인자의 타입을 알아야 합니다.
이러한 종류의 연산자는 일반적으로 프로토콜을 채택하는 구체적인 타입과 일치하는
`Self` 타입의 인자를 사용하지만
프로토콜에 `Self` 요구사항을 추가하면
프로토콜을 타입으로 사용할 때
발생하는 타입 삭제가 허용되지 않습니다.

함수의 반환 타입으로 박싱 프로토콜 타입을 사용하면
프로토콜을 준수하는 모든 타입을 유연하게 반환할 수 있습니다.
그러나 이러한 유연성의 대가는
반환된 값에 대해 일부 작업을 수행할 수 없습니다.
이 예시는 박싱 프로토콜 타입을 사용하여 보존되지 않는
구체적인 타입 정보에 따라
`==` 연산자를 사용할 수 없는 것을 보여줍니다.

이 접근방식의 다른 문제는 도형 변환이 중첩되지 않습니다.
삼각형을 뒤집은 결과는 `Shape` 타입의 값이고
`protoFlip(_:)` 함수는
`Shape` 프로토콜을 준수하는 타입을 인자로 가집니다.
그러나 박싱 프로토콜 타입의 값은
`protoFlip(_:)`에 의해 반환된 값은 `Shape`를 준수하지 않으므로 프로토콜을 준수하지 않습니다.
여러 변환을 적용하는
`protoFlip(protoFlip(smallTriangle))`같은 코드는
뒤집힌 도형이 `protoFlip(_:)`에 대해 유효하지 않은 인자입니다.

반대로
불투명 타입은 기본 타입의 정체성을 보존합니다.
Swift는 연관 타입을 추론할 수 있으므로,
박싱 프로토콜 타입을 반환값으로 사용할 수 없는 위치에
불투명 반환값을 사용할 수 있습니다.
예를 들어
다음은 [제네릭 (Generics)](./generics.md)의 `Container` 프로토콜의 버전입니다:

```swift
protocol Container {
    associatedtype Item
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
extension Array: Container { }
```

<!--
  - test: `opaque-result, opaque-result-existential-error`

  ```swifttest
  -> protocol Container {
         associatedtype Item
         var count: Int { get }
         subscript(i: Int) -> Item { get }
     }
  -> extension Array: Container { }
  ```
-->

프로토콜은 연관 타입을 가지고 있으므로
함수의 반환 타입으로 `Container`를 사용할 수 없습니다.
또한 제네릭 타입이 필요한지 추론하기 위해
함수 본문의 외부에 충분한 정보가 없으므로
제네릭 반환 타입의 제약조건을 사용할 수 없습니다.

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

<!--
  - test: `opaque-result-existential-error`

  ```swifttest
  // Error: Protocol with associated types can't be used as a return type.
  -> func makeProtocolContainer<T>(item: T) -> Container {
         return [item]
     }

  // Error: Not enough information to infer C.
  -> func makeProtocolContainer<T, C: Container>(item: T) -> C {
         return [item]
     }
  !$ error: use of protocol 'Container' as a type must be written 'any Container'
  !! func makeProtocolContainer<T>(item: T) -> Container {
  !!                                           ^~~~~~~~~
  !! any Container
  !$ error: cannot convert return expression of type '[T]' to return type 'C'
  !! return [item]
  !! ^~~~~~
  !! as! C
  ```
-->

반환 타입으로 불투명 타입 `some Container`을 사용하면 함수가 컨테이너를 반환한다고
API를 표현할 수 있고,
컨테이너의 타입을 명시하지 않을 수 있습니다:

```swift
func makeOpaqueContainer<T>(item: T) -> some Container {
    return [item]
}
let opaqueContainer = makeOpaqueContainer(item: 12)
let twelve = opaqueContainer[0]
print(type(of: twelve))
// Prints "Int"
```

<!--
  - test: `opaque-result`

  ```swifttest
  -> func makeOpaqueContainer<T>(item: T) -> some Container {
         return [item]
     }
  -> let opaqueContainer = makeOpaqueContainer(item: 12)
  -> let twelve = opaqueContainer[0]
  -> print(type(of: twelve))
  <- Int
  ```
-->

`twelve`의 타입은 `Int`로 추론되며
이것은 불투명 타입이 타입 추론이 동작한다는 것을 보여줍니다.
`makeOpaqueContainer(item:)`의 구현에서
불투명 컨테이너의 타입은 `[T]`입니다.
이 경우에 `T`는 `Int`이므로
반환값은 정수의 배열이고
연관 타입 `Item`은 `Int`로 추론됩니다.
`Container`의 서브스크립트는
`twelve`의 타입도 `Int`로 추론된다는 의미의 `Item`을 반환합니다.

<!--
  TODO: Expansion for the future

  You can combine the flexibility of returning a value of protocol type
  with the API-boundary enforcement of opaque types
  by using type erasure
  like the Swift standard library uses in the
  `AnySequence <//apple_ref/fake/AnySequence`_ type.

  protocol P { func f() -> Int }

  struct AnyP: P {
      var p: P
      func f() -> Int { return p.f() }
  }

  struct P1 {
      func f() -> Int { return 100 }
  }
  struct P2 {
      func f() -> Int { return 200 }
  }

  func opaque(x: Int) -> some P {
      let result: P
      if x > 100 {
          result = P1()
      }  else {
          result = P2()
      }
      return AnyP(p: result)
  }
-->


## 불투명 매개변수 타입 (Opaque Parameter Types)

불투명 타입을 반환하기 위해 `some`을 작성하는 것 외에도
함수, 서브스크립트, 이니셜라이저의
매개변수 타입에 `some`을 작성할 수도 있습니다.
그러나 매개변수 타입으로 `some`을 작성하는 것은
불투명 타입이 아닌 제네릭을 위한 축약형일 뿐입니다.
예를 들어
아래의 두 함수 모두 동일합니다:

```swift
func drawTwiceGeneric<SomeShape: Shape>(_ shape: SomeShape) -> String {
    let drawn = shape.draw()
    return drawn + "\n" + drawn
}

func drawTwiceSome(_ shape: some Shape) -> String {
    let drawn = shape.draw()
    return drawn + "\n" + drawn
}
```

`drawTwiceGeneric(_:)` 함수는
`SomeShape`라는 제네릭 타입 매개변수를 선언하고
`SomeShape`는 `Shape` 프로토콜을 준수하도록 요구하는 제약조건을 가집니다.
`drawTwiceSome(_:)` 함수는
인자에 `some Shape` 타입을 사용합니다.
이것은 해당 함수에 `Shape` 프로토콜을 준수하는 타입을 요구하는 제약조건과 함께
새롭고 이름이 없는 제네릭 타입 매개변수를 생성합니다.
제네릭 타입은 이름을 가지지 않으므로,
함수의 다른 곳에서는 타입을 참조할 수 없습니다.

두 개 이상의 매개변수 타입 앞에 `some`을 작성하면,
각 제네릭 타입은 독립적입니다.
예를 들어:

```swift
func combine(shape s1: some Shape, with s2: some Shape) -> String {
    return s1.draw() + "\n" + s2.draw()
}

combine(smallTriangle, trapezoid)
```

`combine(shape:with:)` 함수에서
첫 번째와 두 번째 매개변수 타입은
모두 `Shape` 프로토콜을 준수해야 하지만
둘 다 동일한 타입이어야 한다는 제약조건은 없습니다.
`combine(shape:with:)`를 호출할 때,
두 개의 다른 모양을 전달할 수 있습니다 ---
이 경우에는 하나는 삼각형이고 다른 하나는 사다리꼴입니다.

[제네릭 (Generics)](./generics.md)에서 설명한대로
명명 제네릭 타입 매개변수를 위한 문법과 다르게
이 경량 문법은
제네릭 `where` 절이나 동일 타입(`==`) 제약조건을 포함할 수 없습니다.
또한
매우 복잡한 제약조건에 경량 문법을 사용하면
읽기 어려울 수 있습니다.

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->