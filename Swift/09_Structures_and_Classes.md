# 구조체와 클래스 (Structures and Classes)

_구조체 (Structures)_ 와 _클래스 (classes)_ 는 프로그램 코드의 구성 요소가 되는 범용의 유연한 구조입니다. 상수, 변수, 그리고 함수를 정의하는 것과 같은 구문을 사용하여 구조체와 클래스에 프로퍼티와 메서드를 기능적으로 추가할 수 있습니다.

다른 프로그래밍 언어와 달리 Swift는 사용자 정의 구조체와 클래에 대해 별도의 인터페이스와 구현 파일을 만들 필요가 없습니다. Swift에서 단일 파일로 구조체 또는 클래스를 정의하면 해당 클래스 또는 구조체에 대한 외부 인터페이스가 자동으로 다른 코드에서 사용할 수 있습니다.

> NOTE
> 클래스의 인스턴스는 전통적으로 _객체 (object)_ 라고 알고 있습니다. 그러나 Swift 구조체와 클래스는 다른 언어보다 기능적으로 훨씬 가깝고 이 챕터의 대부분은 클래스 또는 구조체 타입의 인스턴스에 적용되는 기능을 설명합니다. 이 때문에 좀 더 일반적인 용어인 _인스턴스 (instance)_ 사용됩니다.

## 구조체와 클래스의 비교 (Comparing Structures and Classes)

Swift에서 구조체와 클래스는 공통점이 많습니다. 둘다 아래의 내용이 가능합니다:

* 값을 저장하는 프로퍼티 정의
* 기능 제공을 위한 메서드 정의
* 서브 스크립트 구문을 사용하여 값에 접근을 제공하는 서브 스크립트 정의
* 초기화 상태를 설정하기 위한 초기화 정의
* 기본 구현을 넘어 기능적 확장을 위한 확장
* 특정 종류의 표준 기능을 제공하는 프로토콜 준수

더 자세한 내용은 [프로퍼티 (Properties)](https://docs.swift.org/swift-book/LanguageGuide/Properties.html), [메서드 (Methods)](https://docs.swift.org/swift-book/LanguageGuide/Methods.html), [서브 스크립트 (Subscripts)](https://docs.swift.org/swift-book/LanguageGuide/Subscripts.html), [초기화 (Initialization)](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html), [확장 (Extensions)](https://docs.swift.org/swift-book/LanguageGuide/Extensions.html), [프로토콜 (Protocols)](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html) 을 참고 바랍니다.

클래스는 구조체에 없는 추가적인 기능이 있습니다:

* 상속을 사용하면 한 클래스가 다른 클래스의 특성을 상속할 수 있습니다.
* 타입 캐스팅을 사용하면 런타임에 클래스 인스턴스의 타입을 확인하고 해석할 수 있습니다.
* Deinitalizers를 사용하면 클래스의 인스턴스가 할당된 리소스를 해제할 수 있도록 합니다.
* 참조 카운팅은 하나 이상의 클래스 인스턴스 참조를 허락합니다.

더 자세한 내용은 [상속 (Inheritance)](https://docs.swift.org/swift-book/LanguageGuide/Inheritance.html), [타입 캐스팅 (Type Casting)](https://docs.swift.org/swift-book/LanguageGuide/TypeCasting.html), [Deinitialization](https://docs.swift.org/swift-book/LanguageGuide/Deinitialization.html), [자동 참조 카운팅 (Automatic Reference Counting)](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html) 를 참고 바랍니다.

클래스가 지원하는 추가 기능은 복잡성이 증가합니다. 일반적인 지침으로는 추론하기 쉬운 구조체를 선호하고 적절하거나 필요할 때 클래스를 사용합니다. 실질적으로 정의하는 대부분의 사용자 정의 데이터 타입이 구조체와 열거형 이라는 것을 의미합니다. 더 자세한 비교는 [구조체와 클래스 선택 (Choosing Between Structures and Classes)](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes) 를 참고 바랍니다.

### 정의 구문 (Definition Syntax)

구조체와 클래스는 유사한 정의 구문을 가지고 있습니다. 구조체는 `struct` 키워드로 클래스는 `class` 키워드로 시작합니다. 둘다 중괄호 안에 전체 정의가 위치합니다:

```swift
struct SomeStructure {
    // structure definition goes here
}
class SomeClass {
    // class definition goes here
}
```

> NOTE
> 새로운 구조체 또는 클래스를 정의할 때마다 새로운 Swift 타입을 정의합니다. 표준 Swift 타입 (`String`, `Int`, `Bool` 와 같은)의 대소문자와 일치하도록 타입 `UpperCamelCase` 이름 (`SomeStructure` 와 `SomeClass` 와 같이)을 지정하십시오. 프로퍼티와 메서드는 타입 이름과 구분을 위해 `lowerCamelCase` 이름 (`frameRate` 와 `incrementCount` 와 같이)으로 지정하십시오.

다음은 구조체 정의와 클래스 정의의 예입니다:

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

위 예제는 픽셀기반의 화면 해상도를 설명하는 `Resolution` 이라는 새로운 구조체를 정의합니다. 이 구조체는 `width` 와 `height` 라 불리는 저장된 프로퍼티를 가지고 있습니다. 저장된 프로퍼티는 구조체 또는 클래스의 일부로 묶여 저장되는 상수 또는 변수입니다. 이 두 프로퍼티는 정수값 `0` 으로 초기값이 설정되므로 `Int` 타입으로 유추됩니다.

위 예제는 비디오 화면을 위한 특정 비디오 모드를 설명하는 `VideoMode` 라는 새로운 클래스를 정의합니다. 이 클래스는 4개의 저장된 프로퍼티를 가지고 있습니다. 첫번재 `resolution` 은 `Resolution` 의 프로퍼티 타입으로 유추되는 새로운 `Resolution` 구조체 인스턴스로 초기화 됩니다. 다른 3가지 프로퍼티 경우 새로운 `VideoMode` 인스턴스는 `interlaced` 설정은 `false` ("비인터레이스 비디오")로 재생 프레임 속도는 `0.0` 으로 그리고 `name` 이라는 옵셔널 `String` 값으로 초기화 됩니다. `name` 프로퍼티는 옵셔널 타입이므로 자동적으로 `nil` 기본값으로 주어지거나 "`name` 값 없음" 으로 주어집니다.

### 구조체와 클래스 인스턴스 (Structure and Class Instances)

`Resolution` 구조체 정의와 `VideoMode` 클래스 정의는 오직 `Resolution` 또는 `VideoMode` 의 모양만 설명합니다. 자체적인 해상도 또는 비디오 모드에 대해 설명하지 않습니다. 그렇게 하려면 구조체 또는 클래스의 인스턴스 생성이 필요합니다.

인스턴스 생성 구문은 구조체와 클래스 모두 매우 유사합니다:

```swift
let someResolution = Resolution()
let someVideoMode = VideoMode()
```

구조체와 클래스 모두 새로운 인스턴스를 위해 초기화 구문을 사용합니다. 초기화 구문의 가장 간단한 형태는 `Resolution()` 또는 `VideoMode()` 와 같이 클래스 또는 구조체 타입 이름 뒤에 빈 소괄호를 붙여 사용하는 것입니다. 이렇게 하면 모든 프로퍼티가 기본값으로 초기화되는 클래스 또는 구조체의 새로운 인스턴스를 생성합니다. 클래스와 구조체 초기화에 대한 자세한 내용은 [초기화 (Initialization)](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html) 를 참고 바랍니다.

### 프로퍼티 접근 (Accessing Properties)

_점 구문 (dot syntax)_ 을 사용하여 인스턴스의 프로퍼티에 접근할 수 있습니다. 점 구문은 인스턴스 이름 뒤에 구분자 (`.`)로 분리하고 공백 없이 프로퍼티 이름을 작성합니다:

```swift
print("The width of someResolution is \(someResolution.width)")
// Prints "The width of someResolution is 0"
```

이 예제에서 `someResolution.width` 는 `someResolution` 에 프로퍼티 `width` 를 참조하고 기본값 `0` 을 반환합니다.

`VideoMode` 의 `resolution` 프로퍼티에 `width` 프로퍼티와 같이 서브 프로퍼티에 접근할 수 있습니다:

```swift
print("The width of someVideoMode is \(someVideoMode.resolution.width)")
// Prints "The width of someVideoMode is 0"
```

변수 프로퍼티에 새로운 값을 할당하기 위해 점 구문을 사용할 수 있습니다:

```swift
someVideoMode.resolution.width = 1280
print("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// Prints "The width of someVideoMode is now 1280"
```

### 구조체 타입에 대한 멤버별 초기화 (Memberwise Initializers for Structure Types)

모든 구조체는 새로운 구조체 인스턴스의 멤버 프로퍼티를 초기화 할 때 사용할 수 있는 자동적으로 생성된 _멤버별 초기화 (memberwise intializer)_ 를 가지고 있습니다. 새로운 인스턴스에 프로퍼티 초기값은 이름으로 멤버별 초기화에 전달될 수 있습니다:

```swift
let vga = Resolution(width: 640, height: 480)
```

구조체와 반대로 클래스 인스턴스는 멤버별 초기화를 받지 않습니다. 자세한 설명은 [초기화 (Initialization)](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html) 을 참고 바랍니다.

## 구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)

_값 타입 (value type)_ 은 변수 또는 상수에 할당될 때나 함수에 전달될 때 _복사_ 되는 값인 타입입니다.

실제로 값 타입에 대해 이전 챕터에서 광범위하게 다뤘습니다. 실제로 Swift에서 정수, 부동 소수점, 부울, 문자열, 배열 그리고 딕셔너리와 같은 기본 타입의 모두는 값 타입이고 구조체로 구현되어 있습니다.

Swift에서 모든 구조체와 열거형은 값 타입입니다. 이것은 생성한 구조체와 열거형 인스턴스와 프로퍼티로 포함된 모든 값 타입은 코드에서 전달될 때 복사된다는 의미입니다.

> NOTE
> 배열, 딕셔너리, 문자열과 같은 표준 라이브러리에 정의된 콜렉션은 최적화를 사용하여 복사 성능 비용을 줄입니다. 즉시 복사본을 만드는 대신에 이러한 콜렉션은 원본 인스턴스와 복사본 간에 요소가 저장된 메모리를 공유합니다. 콜렉션의 복사본 중 하나가 수정되면 요소는 수정되기 직전에 복사됩니다. 코드에서 보이는 동작은 항상 바로 복사가 일어나는 것처럼 보입니다.

이전 예제에서의 `Resolution` 구조체를 사용하는 다음 예제를 살펴봅시다:

```swift
let hd = Resolution(width: 1920, height: 1080)
var cinema = hd
```

이 예제는 `hd` 라는 상수를 선언하고 풀 HD 비디오 (1920 픽셀 너비와 1080 픽셀 높이)의 너비와 높이를 초기화하는 `Resolution` 인스턴스를 설정합니다.

그리고 나서 `cinema` 라는 변수를 선언하고 `hd` 의 현재 값을 설정합니다. `Resolution` 은 구조체 이므로 기존 인스턴스의 _복사본_ 이 만들어지고 이 새 복사본에 `cinema` 가 할당됩니다. `hd` 와 `cinema` 가 현재 같은 너비와 높이를 가지지만 2개는 완벽하게 다른 인스턴스 입니다.

다음으로 `cinema` 의 `width` 프로퍼티를 디지털 시네마 프로젝션에 사용되는 약간 더 넓은 2K 표준 (2048 픽셀 너비와 1080 픽셀 높이)으로 수정됩니다:

```swift
cinema.width = 2048
```

`cinema` 의 `width` 프로퍼티를 체크하면 `2048` 로 바뀐 것을 확인할 수 있습니다:

```swift
print("cinema is now \(cinema.width) pixels wide")
// Prints "cinema is now 2048 pixels wide"
```

그러나 기존 `hd` 인스턴스의 `width` 프로퍼티는 `1920` 의 기존값을 그대로 가지고 있습니다:

```swift
print("hd is still \(hd.width) pixels wide")
// Prints "hd is still 1920 pixels wide"
```

`cinema` 에 `hd` 에 현재값이 주어졌을 때 `hd` 에 저장된 _값_ 은 새로운 `cinema` 인스턴스에 복사됩니다. 마지막 결과는 숫자값을 포함한 2개의 완벽히 분리된 인스턴스 입니다. 그러나 분리된 인스턴스이기 때문에 아래의 그림과 같이 `cinema` 에 너비를 `2048` 로 설정해도 `hd` 에 저장된 너비에는 영향을 주지 않습니다:

![Shared State Struct](/Users/a156618/Desktop/Boram/Google/Documents/Boram_Docs/Swift/09_sharedStateStruct_2x.png)

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

`rememberedDirection` 은 `currentDirection` 에 값이 할당될 때 실질적으로 복사본이 설정됩니다. 이후에 `currentDirection` 에 값을 변경해도 `rememberedDirection` 에 저장된 원래 값의 복사본에는 영향을 주지 않습니다.

## 클래스는 참조 타입 (Classes Are Reference Types)

값 타입과 반대로 _참조 타입 (reference types)_ 은 변수 또는 상수에 할당될 때나 함수로 전달될 때 복사되지 않습니다. 복사본 대신에 존재하는 같은 인스턴스에 대한 참조가 사용됩니다.

다음은 위에 정의된 `VideoMode` 를 사용하는 예입니다:

```swift
let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0
```

이 예제는 `tenEighty` 라는 새로운 상수를 선언하고 `VideoMode` 클래스의 새로운 인스턴스를 참조하도록 설정합니다. 비디오 모드는 이전에 `1920` x `1080` 의 HD 해상도의 복사본이 할당됩니다. 인터레이스로 설정되고 이름을 `"1080i"` 로 설정하고 프레임 속도를 초당 `25.0` 프레임으로 설정합니다.

다음으로 `tenEighty` 는 `alsoTenEighty` 라는 새로운 상수에 할당되고 `alsoTenEighty` 의 프레임 속도를 수정합니다:

```swift
let alsoTenEighty = tenEighty
alsoTenEighty.frameRate = 30.0
```

클래스는 참조 타입이므로 `tenEighty` 와 `alsoTenEighty` 는 실질적으로 _같은_ `VideoMode` 인스턴스를 참조합니다. 실제로는 아래 그림과 같이 같은 하나의 인스턴스에 다른 2개의 이름을 가지고 있는 것입니다:

![Shared State Class](/Users/a156618/Desktop/Boram/Google/Documents/Boram_Docs/Swift/09_sharedStateClass_2x.png)

`tenEighty` 의 `frameRate` 프로퍼티를 체크하면 `VideoMode` 인스턴스에서 `30.0` 의 새로운 프레임 속도가 올바르게 설정된 것을 보여줍니다:

```swift
print("The frameRate property of tenEighty is now \(tenEighty.frameRate)")
// Prints "The frameRate property of tenEighty is now 30.0"
```

이 예제는 참조 타입이 어떻게 추론하기 어려울 수 있는지 보여줍니다. `tenEighty` 와 `alsoTenEighty` 가 프로그램 코드가 멀리 떨어져 있다면 비디오 모드가 변경되는 모든 방법을 찾기 어려울 수 있습니다. `tenEighty` 를 사용할 때마다 `alsoTenEighty` 를 사용하는 코드를 생각해야 하며 그 반대도 마찬가지 입니다. 반대로 값 타입은 동일한 값과 상호작용하는 모든 코드가 소스 파일에 가까이 있기 때문에 추론하기가 더 쉽습니다.

`tenEighty` 와 `alsoTenEighty` 는 변수가 아닌 _상수_ 로 선언됩니다. 그러나 `tenEighty` 와 `alsoTenEighty` 상수 자체는 실제로 변경되지 않으므로 `tenEighty.frameRate` 와 `alsoTenEighty.frameRate` 는 여전히 변경 가능합니다. `tenEighty` 와 `alsoTenEighty` 자체는 `VideoMode` 인스턴스를 "저장"하지 않습니다. 대신에 `VideoMode` 인스턴스를 둘다 _참조_ 합니다. 변경되는 것은 `VideoMode` 에 대한 상수 참조의 값이 아니라 `VideoMode` 의 `frameRate` 프로퍼티 입니다.

### 식별 연산자 (Identity Operators)

클래스는 참조 타입이기 때문에 클래스의 같은 단일 인스턴스에 참조하는 여러개의 상수와 변수가 가능합니다 (구조체와 열거형은 상수 또는 변수 또는 함수에 전달할 때 항상 복사되기 때문에 클래스와 같지 않습니다).

2개의 상수 또는 변수가 클래스의 같은 인스턴스를 참조하는지 확인하는 것이 유용할 수 있습니다. 이를 위해 Swift는 2가지 식별 연산자를 제공합니다:

- 동일 인스턴스 (Identical to) (`===`)
- 동일하지 않은 인스턴스 (Not identical to) (`!==`)

이 연산자를 사용하여 2개의 상수 또는 변수가 하나의 동일한 인스턴스를 참조하는지 확인할 수 있습니다:

```swift
if tenEighty === alsoTenEighty {
    print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
}
// Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
```

_동일 인스턴스_ (3개의 등호로 표시 또는 `===`)는 _같음 (equal to)_ (2개의 등호로 표시 또는 `==`)과 같다는 의미는 아닙니다. _동일 인스턴스_ 는 클래스 타입의 2개의 상수 또는 변수가 동일한 클래스 인스턴스를 잠조한다는 의미입니다. _같음_ 은 두 인스턴스의 값이 동일하거나 동등하다는 것을 의미합니다.

커스텀 구조체와 클래스를 정의할 때 두 인스턴스가 같은지의 여부를 결정하는 것은 사용자의 몫입니다. `==` 와 `!=` 연산자의 자체 구현 선언의 프로세스는 [등가 연산자 (Equivalence Operators)](https://docs.swift.org/swift-book/LanguageGuide/AdvancedOperators.html#ID45) 에 자세히 설명되어 있습니다.

### 포인터 (Pointers)

C, C++, 또는 Objective-C에 경험이 있다면 메모리의 주소를 참조하기 위해 _포인터 (pointers)_ 를 사용한다는 것을 알고 있을 것입니다. 일부 참조 타입의 인스턴스를 참조하기 위한 Swift 상수 또는 변수는 C의 포인터와 유사하지만 메모리의 주소에 대한 직접적인 포인터가 아니며 포인터 표시를 위해 별표 (`*`)를 작성할 필요가 없습니다. 대신에 이러한 참조는 Swift의 다른 상수 또는 변수처럼 정의됩니다. 표준 라이브러리는 포인터와 직접 상호작용이 필요한 경우 사용할 수 있는 포인터와 버퍼 타입을 제공합니다. 자세한 내용은 [수동 메모리 관리 (Manual Memory Management)](https://developer.apple.com/documentation/swift/swift_standard_library/manual_memory_management) 를 참고 바랍니다.

