# 구조체와 클래스 (Structures and Classes)

데이터를 캡슐화하는 커스텀 타입을 모델링합니다.

*구조체(Structures)*와 *클래스(classes)*는 범용적이고 유용한 구조로,
프로그램 코드를 구성하는 핵심적인 빌딩 블록 역할을 합니다.
상수, 변수, 함수를 정의하는 것과 같은 구문을 사용하여
구조체와 클래스에 프로퍼티와 메서드를 정의하여 기능을 추가할 수 있습니다.

다른 프로그래밍 언어와 달리,
Swift는 커스텀 구조체와 클래스에 대해
별도의 인터페이스와 구현 파일을 만들 필요가 없습니다.
Swift에서 단일 파일로 구조체나 클래스를 정의하면,
해당 클래스나 구조체에 대한 외부 인터페이스가
자동으로 다른 코드에서 사용할 수 있습니다.

> Note: 클래스의 인스턴스는 전통적으로 *객체(object)*라고 불립니다.
> 그러나 Swift에서는 구조체와 클래스가
> 다른 언어보다 훨씬 유사한 기능을 제공하기 때문에,
> 이 챕터의 대부분에서는
> 구조체와 클래스 인스턴스 모두에 적용되는 공통 기능을 설명합니다.
> 따라서 이 문서에서는 더 일반적인 용어인 *인스턴스(instance)*를 사용합니다.

## 구조체와 클래스 비교 (Comparing Structures and Classes)

Swift에서 구조체와 클래스는 공통점이 많습니다.
둘 다 다음과 같은 기능을 지원합니다:

- 값을 저장하기 위한 프로퍼티 정의
- 기능을 제공하기 위한 메서드 정의
- 서브스크립트를 통해 값에 접근할 수 있는 문법 제공
- 초기화 상태를 설정하기 위한 이니셜라이저 정의
- 기본 구현을 넘어서 기능을 확장할 수 있도록 확장 지원
- 특정 기능을 표준화하기 위한 프로토콜 채택 가능

더 자세한 내용은
[프로퍼티 (Properties)](./properties.md), [메서드 (Methods)](./methods.md), [서브스크립트 (Subscripts)](./subscripts.md), [초기화 (Initialization)](./initialization.md),
[확장 (Extensions)](./extensions.md), [프로토콜 (Protocols)](./protocols.md)을 참고바랍니다.

클래스는 구조체에 없는 추가적인 기능도 지원합니다:

- 상속을 사용하면 다른 클래스의 특성을 상속할 수 있습니다.
- 타입 캐스팅을 사용하면 런타임에 클래스 인스턴스의 타입을 확인하고 해석할 수 있습니다.
- 디이니셜라이저(Deinitializers)을 사용하면 클래스의 인스턴스가 할당된 리소스를 해제할 수 있습니다.
- 참조 카운팅은 하나의 클래스 인스턴스를 여러 참조가 공유할 수 있습니다.

더 자세한 내용은
[상속 (Inheritance)](./inheritance.md), [타입 캐스팅 (Type Casting)](./type-casting.md), [소멸 (Deinitialization)](./deinitialization.md),
[자동 순환 카운팅 (Automatic Reference Counting)](./automatic-reference-counting.md)을 참고바랍니다.

클래스가 지원하는 추가 기능은
더 높은 복잡성을 수반합니다.
일반적인 지침으로는
구조체는 예측 가능하고 이해하기 쉬우므로 구조체를 선호하며,
클래스는 필요한 경우에만 사용하는 것이 좋습니다.
실제 개발에서는 대부분의 커스텀 타입이
구조체나 열거형으로 작성됩니다.
더 자세한 비교는
[구조체와 클래스 선택 (Choosing Between Structures and Classes)](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes)을 참고바랍니다.

> Note: 클래스와 액터(actor)는 많은 특성과 동작을 공유합니다.
> 액터에 대한 자세한 내용은 [동시성 (Concurrency)](./concurrency.md)을 참고바랍니다.

### 정의 문법 (Definition Syntax)

구조체와 클래스는 유사한 문법으로 정의됩니다.
구조체는 `struct` 키워드로
클래스는 `class` 키워드로 시작합니다.
둘 경우 모두 중괄호 안에 전체 정의를 작성합니다:

```swift
struct SomeStructure {
    // structure definition goes here
}
class SomeClass {
    // class definition goes here
}
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> struct SomeStructure {
        // structure definition goes here
     }
  -> class SomeClass {
        // class definition goes here
     }
  ```
-->

> Note: 새로운 구조체나 클래스를 정의할 때마다,
> 새로운 Swift 타입을 정의합니다.
> Swift의 표준 타입
> (`String`, `Int`, `Bool` 등)과
> 일관되도록, 타입 이름은 `UpperCamelCase`
> (`SomeStructure`, `SomeClass` 등)로 작성합니다.
> 프로퍼티와 메서드는 타입 이름과 구분을 위해 `lowerCamelCase`
> (`frameRate`, `incrementCount` 등)으로
> 작성합니다.

다음은 구조체 정의와 클래스 정의의 예시입니다:

```swift
struct Resolution {
    var width = 0
    var height = 0
}
class VideoMode {
    var resolution = Resolution()
    var interlaced = false
    var frameRate = 0.0
    var name: String?
}
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> struct Resolution {
        var width = 0
        var height = 0
     }
  -> class VideoMode {
        var resolution = Resolution()
        var interlaced = false
        var frameRate = 0.0
        var name: String?
     }
  ```
-->

위 예시는 픽셀 기반의 화면 해상도를 설명하는
`Resolution`이라는 새로운 구조체를 정의합니다.
이 구조체는 `width`와 `height`라 불리는 저장 프로퍼티를 가지고 있습니다.
저장 프로퍼티는 구조체나 클래스의 일부로
함께 저장되는 상수나 변수입니다.
이 두 프로퍼티는 정수값 `0`으로 초기값이 설정되므로
`Int` 타입으로 추론됩니다.

위 예시는 비디오 화면에 대한 특정 비디오 모드를 나타내는
`VideoMode`라는 새로운 클래스를 정의합니다.
이 클래스는 네 개의 저장 프로퍼티를 가지고 있습니다.
첫 번째 `resolution`은 새로운 `Resolution` 인스턴스로 초기화되며,
이에 따라 해당 프로퍼티의 타입은 자동으로 `Resolution`으로 추론됩니다.
다른 세 프로퍼티는,
`interlaced`는 `false`("비인터레이스 비디오")로
재생 프레임 속도는 `0.0`으로
`name`은 옵셔널 `String` 값으로 초기화 되어
새로운 `VideoMode` 인스턴스를 초기화합니다.
`name` 프로퍼티는 옵셔널 타입이므로 기본값은 자동으로 `nil`이며,
"`name` 값 없음"입니다.

### 구조체와 클래스 인스턴스 (Structure and Class Instances)

`Resolution` 구조체 정의와 `VideoMode` 클래스 정의는
`Resolution`이나 `VideoMode`가 어떤 구조를 가지는지만 설명합니다.
특정 해상도나 비디오 모드를 직접 나타내는 것은 아닙니다.
실제 데이터를 사용하려면, 해당 구조체나 클래스의 인스턴스 생성이 필요합니다.

인스턴스 생성 문법은 구조체와 클래스 모두 매우 유사합니다:

```swift
let someResolution = Resolution()
let someVideoMode = VideoMode()
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> let someResolution = Resolution()
  -> let someVideoMode = VideoMode()
  ```
-->

구조체와 클래스 모두 새로운 인스턴스를 생성하기위해 이니셜라이저를 사용합니다.
이니셜라이저의 가장 단순한 형태는 `Resolution()`나 `VideoMode()`와 같이
클래스나 구조체 타입 이름 뒤에 빈 소괄호를 붙여 사용하는 것입니다.
이렇게 하면 모든 프로퍼티가 기본값으로 초기화되는
클래스나 구조체의 새로운 인스턴스를 생성합니다.
클래스와 구조체 초기화에 대한 자세한 내용은
[초기화 (Initialization)](./initialization.md)를 참고바랍니다.

<!--
  TODO: note that you can only use the default constructor if you provide default values
  for all properties on a structure or class.
-->

### 프로퍼티 접근 (Accessing Properties)

*점 표기법(dot syntax)*을 사용하여 인스턴스의 프로퍼티에 접근할 수 있습니다.
점 표기법에서는 인스턴스 이름 뒤에 마침표(`.`)를 붙이고,
그 뒤에 공백없이 프로퍼티 이름을 작성합니다:

```swift
print("The width of someResolution is \(someResolution.width)")
// Prints "The width of someResolution is 0"
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> print("The width of someResolution is \(someResolution.width)")
  <- The width of someResolution is 0
  ```
-->

이 예시에서
`someResolution.width`는 `someResolution` 인스턴스의 `width` 프로퍼티를 참조하고
기본값 `0`을 반환합니다.

`VideoMode` 인스턴스의 `resolution` 프로퍼티에 `width` 프로퍼티와 같이
프로퍼티 안의 하위 프로퍼티에 접근할 수 있습니다:

```swift
print("The width of someVideoMode is \(someVideoMode.resolution.width)")
// Prints "The width of someVideoMode is 0"
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> print("The width of someVideoMode is \(someVideoMode.resolution.width)")
  <- The width of someVideoMode is 0
  ```
-->

변수 프로퍼티에 새로운 값을 할당하기 위해 점 표기법을 사용할 수 있습니다:

```swift
someVideoMode.resolution.width = 1280
print("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// Prints "The width of someVideoMode is now 1280"
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> someVideoMode.resolution.width = 1280
  -> print("The width of someVideoMode is now \(someVideoMode.resolution.width)")
  <- The width of someVideoMode is now 1280
  ```
-->

### 구조체의 멤버와이즈 이니셜라이저 (Memberwise Initializers for Structure Types)

모든 구조체는 새로운 구조체 인스턴스의 멤버 프로퍼티를 초기화 할 때 사용할 수 있는
자동으로 생성되는 *멤버와이즈 이니셜라이저(memberwise intializer)*를 가집니다.
새로운 인스턴스의 프로퍼티 초기값은
이름으로 멤버와이즈 초기화에 전달할 수 있습니다:

```swift
let vga = Resolution(width: 640, height: 480)
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> let vga = Resolution(width: 640, height: 480)
  ```
-->

구조체와 다르게, 클래스 인스턴스는 멤버와이즈 초기화를 제공하지 않습니다.
초기화에 대한 자세한 설명은 [초기화 (Initialization)](./initialization.md)을 참고바랍니다.

<!--
  - test: `classesDontHaveADefaultMemberwiseInitializer`

  ```swifttest
  -> class C { var x = 0, y = 0 }
  -> let c = C(x: 1, y: 1)
  !$ error: argument passed to call that takes no arguments
  !! let c = C(x: 1, y: 1)
  !!         ^~~~~~~~~~~~
  !!-
  ```
-->

## 구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)

*값 타입(value type)*은 변수나 상수에 할당되거나
함수에 전달될 때
*복사*되는 타입입니다.

<!--
  Alternate definition:
  A type has value semantics when
  mutation of one variable of that type
  can never be observed through a different variable of the same type.
-->

실제로 값 타입에 대해 이전 챕터에서 광범위하게 다뤘습니다.
실제로 Swift의 기본 타입 ---
정수, 부동소수점, Boolean, 문자열, 배열, 딕셔너리 ---
는 모두 값 타입이며, 내부적으로 구조체로 구현되어 있습니다.

Swift에서 모든 구조체와 열거형은 값 타입입니다.
이것은 생성한 구조체와 열거형 인스턴스와 ---
가지고 있는 프로퍼티 중 모든 값 타입은 ---
코드에서 전달되거나 할당될 때 항상 복사됩니다.

> Note: 배열, 딕셔너리, 문자열과 같은
> Swift 표준 라이브러리에 정의된 컬렉션은
> 복사에 따른 성능 비용을 줄이기 위해 최적화된 방식을 사용합니다.
> 복사를 즉시 수행하지 않고,
> 원본 인스턴스와 복사본이
> 동일한 메모리 영역을 공유합니다.
> 컬렉션 중 하나가 수정되면,
> 요소는 수정되기 직전에 복사됩니다.
> 코드에서 보이는 동작은
> 항상 바로 복사가 일어나는 것처럼 보입니다.

이전 예시에서의 `Resolution` 구조체를 사용하는 다음 예시를 살펴봅시다:

```swift
let hd = Resolution(width: 1920, height: 1080)
var cinema = hd
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> let hd = Resolution(width: 1920, height: 1080)
  -> var cinema = hd
  ```
-->

이 예시는 `hd`라는 상수를 선언하고
풀 HD 비디오
(1920 픽셀 너비와 1080 픽셀 높이)의
너비와 높이를 초기화하는 `Resolution` 인스턴스를 생성합니다.

그리고 나서 `cinema`라는 변수를 선언하고
`hd`의 현재값을 설정합니다.
`Resolution`은 구조체이므로,
기존 인스턴스의 *복사본*이 만들어지고
이 복사본이 `cinema`에 할당됩니다.
`hd`와 `cinema`가 현재 같은 너비와 높이를 가지지만,
내부적으로는 완벽하게 다른 인스턴스 입니다.

다음으로 `cinema`의 `width` 프로퍼티를 디지털 시네마 프로젝션에 사용되는 약간 더 넓은 2K 표준
(2048 픽셀 너비와 1080 픽셀 높이)으로
수정됩니다:

```swift
cinema.width = 2048
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> cinema.width = 2048
  ```
-->

`cinema`의 `width` 프로퍼티를 확인하면
`2048`로 바뀐 것을 확인할 수 있습니다:

```swift
print("cinema is now \(cinema.width) pixels wide")
// Prints "cinema is now 2048 pixels wide"
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> print("cinema is now \(cinema.width) pixels wide")
  <- cinema is now 2048 pixels wide
  ```
-->

그러나 기존 `hd` 인스턴스의 `width` 프로퍼티는
`1920`의 기존값을 그대로 가지고 있습니다:

```swift
print("hd is still \(hd.width) pixels wide")
// Prints "hd is still 1920 pixels wide"
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> print("hd is still \(hd.width) pixels wide")
  <- hd is still 1920 pixels wide
  ```
-->

`cinema`에 `hd`의 현재값이 주어졌을 때,
`hd`에 저장된 *값*은 새로운 `cinema` 인스턴스에 복사됩니다.
그 결과 두 인스턴스는 동일한 숫자 값을 가지지만,
서로 완전히 독립적인 인스턴스 입니다.
분리된 인스턴스이기 때문에,
아래의 그림과 같이
`cinema`에 너비를 `2048`로 설정해도
`hd`에 저장된 너비에는 영향을 주지 않습니다:

![Shared State Struct](../.gitbook/assets/sharedStateStruct_2x~dark.png)

열거형에서도 같은 동작이 이뤄집니다:

```swift
enum CompassPoint {
    case north, south, east, west
    mutating func turnNorth() {
        self = .north
    }
}
var currentDirection = CompassPoint.west
let rememberedDirection = currentDirection
currentDirection.turnNorth()

print("The current direction is \(currentDirection)")
print("The remembered direction is \(rememberedDirection)")
// Prints "The current direction is north"
// Prints "The remembered direction is west"
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> enum CompassPoint {
        case north, south, east, west
        mutating func turnNorth() {
           self = .north
        }
     }
  -> var currentDirection = CompassPoint.west
  -> let rememberedDirection = currentDirection
  -> currentDirection.turnNorth()

  -> print("The current direction is \(currentDirection)")
  -> print("The remembered direction is \(rememberedDirection)")
  <- The current direction is north
  <- The remembered direction is west
  ```
-->

`rememberedDirection`에 `currentDirection`의 값이 할당될 때,
실질적으로 해당 값의 복사본이 설정됩니다.
이후에 `currentDirection`의 값을 변경해도
`rememberedDirection`의 저장된 원래 값 복사본에는 영향을 주지 않습니다.

<!--
  TODO: Should I give an example of passing a value type to a function here?
-->

## 클래스는 참조 타입 (Classes Are Reference Types)

값 타입과 다르게, *참조 타입(reference types)*은
변수나 상수에 할당될 때나
함수로 전달될 때 복사되지 않습니다.
복사본 대신에 존재하는 같은 인스턴스에 대한 참조가 사용됩니다.

다음은 위에 정의된 `VideoMode` 클래스를 사용하는 예시입니다:

```swift
let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> let tenEighty = VideoMode()
  -> tenEighty.resolution = hd
  -> tenEighty.interlaced = true
  -> tenEighty.name = "1080i"
  -> tenEighty.frameRate = 25.0
  ```
-->

이 예시는 `tenEighty`라는 새로운 상수를 선언하고
`VideoMode` 클래스의 새로운 인스턴스를 참조하도록 설정합니다.
비디오 모드는 이전에 `1920` x `1080`의 HD 해상도의 복사본이 할당됩니다.
인터레이스로 설정되고
이름을 `"1080i"`로 설정하고
프레임 속도를 초당 `25.0` 프레임으로 설정합니다.

다음으로 `tenEighty`는 `alsoTenEighty`라는 새로운 상수에 할당되고
`alsoTenEighty`의 프레임 속도를 수정합니다:

```swift
let alsoTenEighty = tenEighty
alsoTenEighty.frameRate = 30.0
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> let alsoTenEighty = tenEighty
  -> alsoTenEighty.frameRate = 30.0
  ```
-->

클래스는 참조 타입이므로
`tenEighty`와 `alsoTenEighty`는 실질적으로 *같은* `VideoMode` 인스턴스를 참조합니다.
실제로는 아래 그림과 같이
하나의 인스턴스에 다른 두 개의 이름을 가지고 있는 것입니다:

![Shared State Class](../.gitbook/assets/sharedStateClass_2x~dark.png)

`tenEighty`의 `frameRate` 프로퍼티를 확인하면
`VideoMode` 인스턴스에서
`30.0`의 새로운 프레임 속도가 올바르게 설정된 것을 보여줍니다:

```swift
print("The frameRate property of tenEighty is now \(tenEighty.frameRate)")
// Prints "The frameRate property of tenEighty is now 30.0"
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> print("The frameRate property of tenEighty is now \(tenEighty.frameRate)")
  <- The frameRate property of tenEighty is now 30.0
  ```
-->

이 예시는 참조 타입이 얼마나 추론하기 어려울 수 있는지 보여줍니다.
`tenEighty`와 `alsoTenEighty`가 프로그램 내에서 서로 멀리 떨어진 위치에 존재한다면,
비디오 모드가 어디서 어떻게 변경되는지 추적하기 어려울 수 있습니다.
`tenEighty`를 사용하는 코드가 있다면,
`alsoTenEighty`를 사용하는 코드 역시 고려해야 하고,
그 반대도 마찬가지입니다.
이에 비해 값 타입은
동일한 값과 상호작용하는 모든 코드가 소스 파일에 가까이 있기 때문에
이해하고 유지보수하기 훨씬 쉽습니다.

`tenEighty`와 `alsoTenEighty`는 변수가 아닌
*상수*로 선언됩니다.
그러나 `tenEighty` 와 `alsoTenEighty` 상수 자체는 실제로 변경되지 않으므로
`tenEighty.frameRate`와 `alsoTenEighty.frameRate`는 여전히 변경 가능합니다.
`tenEighty`와 `alsoTenEighty` 자체는 `VideoMode` 인스턴스를 "저장"하지 않습니다 ---
대신에 `VideoMode` 인스턴스를 둘 다 *참조*합니다.
변경되는 것은 `VideoMode` 인스턴스 자체가 아니라,
`VideoMode` 인스턴스 내부의 `frameRate` 프로퍼티 입니다.

<!--
  TODO: reiterate here that arrays and dictionaries are value types rather than reference types,
  and demonstrate what that means for the values they store
  when they themselves are value types or reference types.
  Also make a note about what this means for key copying,
  as per the swift-discuss email thread "Dictionaries and key copying"
  started by Alex Migicovsky on Mar 1 2014.
-->

<!--
  TODO: Add discussion about how
  a struct that has a member of some reference type
  is itself actually a reference type,
  and about how you can make a class that's a value type.
-->

### 동일성 연산자 (Identity Operators)

클래스는 참조 타입이기 때문에,
여러 상수나 변수가 내부적으로
동일한 클래스 인스턴스를 참조할 수 있습니다.
(구조체와 열거형은
상수나 변수에 할당되거나
함수에 전달될 때 복사되기 때문에 클래스와 같지 않습니다.)

<!--
  - test: `structuresDontSupportTheIdentityOperators`

  ```swifttest
  -> struct S { var x = 0, y = 0 }
  -> let s1 = S()
  -> let s2 = S()
  -> if s1 === s2 { print("s1 === s2") } else { print("s1 !== s2") }
  !$ error: argument type 'S' expected to be an instance of a class or class-constrained type
  !! if s1 === s2 { print("s1 === s2") } else { print("s1 !== s2") }
  !!       ^
  !$ error: argument type 'S' expected to be an instance of a class or class-constrained type
  !! if s1 === s2 { print("s1 === s2") } else { print("s1 !== s2") }
  !!       ^
  ```
-->

<!--
  - test: `enumerationsDontSupportTheIdentityOperators`

  ```swifttest
  -> enum E { case a, b }
  -> let e1 = E.a
  -> let e2 = E.b
  -> if e1 === e2 { print("e1 === e2") } else { print("e1 !== e2") }
  !$ error: argument type 'E' expected to be an instance of a class or class-constrained type
  !! if e1 === e2 { print("e1 === e2") } else { print("e1 !== e2") }
  !!       ^
  !$ error: argument type 'E' expected to be an instance of a class or class-constrained type
  !! if e1 === e2 { print("e1 === e2") } else { print("e1 !== e2") }
  !!       ^
  ```
-->

두 개의 상수나 변수가 같은 클래스 인스턴스를 참조하는지 확인하는 것이
유용할 수 있습니다.
이를 위해 Swift는 두 가지 동일성 연산자를 제공합니다:

- 동일 인스턴스(`===`)
- 동일하지 않은 인스턴스(`!==`)

이 연산자를 사용하여 두 상수나 변수가 하나의 동일한 인스턴스를 참조하는지 확인할 수 있습니다:

```swift
if tenEighty === alsoTenEighty {
    print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
}
// Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
```

<!--
  - test: `ClassesAndStructures`

  ```swifttest
  -> if tenEighty === alsoTenEighty {
        print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
     }
  <- tenEighty and alsoTenEighty refer to the same VideoMode instance.
  ```
-->

*동일 인스턴스*(세 개의 등호로 표시나 `===`)는
*같음(equal to)*(두 개의 등호로 표시나 `==`)과 같다는 의미는 아닙니다.
*동일 인스턴스*는
클래스 타입의 두 상수나 변수가 동일한 클래스 인스턴스를 참조한다는 의미입니다.
*같음*은
두 인스턴스가 값의 관점에서 동일한지,
타입 설계자가 정의한 "동등함"의 기준에 따라 값이 같다고 간주되는지를 의미합니다.

커스텀 구조체와 클래스를 정의할 때,
두 인스턴스가 같은지의 여부를 결정하는 것은 사용자의 몫입니다.
`==`와 `!=` 연산자의 사용자 정의 방법은
[동등 연산자 (Equivalence Operators)](./advanced-operators.md#동등-연산자-equivalence-operators)에 자세히 설명되어 있습니다.

<!--
  - test: `classesDontGetEqualityByDefault`

  ```swifttest
  -> class C { var x = 0, y = 0 }
  -> let c1 = C()
  -> let c2 = C()
  -> if c1 == c2 { print("c1 == c2") } else { print("c1 != c2") }
  !$ error: binary operator '==' cannot be applied to two 'C' operands
  !! if c1 == c2 { print("c1 == c2") } else { print("c1 != c2") }
  !!    ~~ ^  ~~
  ```
-->

<!--
  - test: `structuresDontGetEqualityByDefault`

  ```swifttest
  -> struct S { var x = 0, y = 0 }
  -> let s1 = S()
  -> let s2 = S()
  -> if s1 == s2 { print("s1 == s2") } else { print("s1 != s2") }
  !$ error: binary operator '==' cannot be applied to two 'S' operands
  !! if s1 == s2 { print("s1 == s2") } else { print("s1 != s2") }
  !!    ~~ ^  ~~
  ```
-->

<!--
  TODO: This needs clarifying with regards to function references.
-->

### 포인터 (Pointers)

C, C++, 또는 Objective-C에 익숙하다면,
메모리의 주소를 참조하기 위해 *포인터(pointers)*를 사용한다는 것을 알고 있을 것입니다.
어떤 참조 타입의 인스턴스를 가르키기위해 Swift 상수나 변수는
C의 포인터와 유사하지만,
메모리 주소를 직접 가르키는 포인터가 아니며,
포인터 표시를 위해
별표(`*`)를 작성할 필요가 없습니다.
대신 Swift에서는 이러한 참조를 다른 상수나 변수를 선언하듯이 간단하게 정의할 수 있습니다.
직접 포인터와 상호작용해야 하는 경우를 위해
Swift 표준 라이브러리는 포인터 및 버퍼 타입을 제공합니다 ---
자세한 내용은 [수동 메모리 관리 (Manual Memory Management)](https://developer.apple.com/documentation/swift/swift_standard_library/manual_memory_management)를 참고바랍니다.

<!--
  TODO: functions aren't "instances". This needs clarifying.
-->

<!--
  TODO: Add a justification here to say why this is a good thing.
-->

<!--
  QUESTION: what's the deal with tuples and reference types / value types?
-->

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