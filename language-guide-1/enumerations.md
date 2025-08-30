# 열거형 (Enumerations)

가능한 값의 목록을 정의하는 커스텀 타입을 모델링합니다.

*열거형(enumeration)*은 관련된 값의 그룹을 위한 공통된 타입을 정의하고
이 값을 코드에서 타입 안전성을 유지하면서 다룰 수 있도록 해줍니다.

C에 익숙하다면,
C 열거형이 관련된 이름을 정수값 집합에 매핑한다는 점을 알고 있을 것입니다.
Swift에서의 열거형은 훨씬 유연하고,
열거형의 각 케이스에 값을 제공하지 않아도 됩니다.
값(원시값)이 각 열거형 케이스로 제공된다면,
그 값은 문자열, 문자,
정수, 부동소수점 타입일 수 있습니다.

또는 열거형 케이스는
다른 언어에서 유니언이나 variant로 알려진 기능과 유사하게
각 열거형 케이스에 연관값을 지정하여 케이스마다 서로 다른 타입의 값을 저장할 수도 있습니다.
하나의 열거형 내에 관련된 케이스를 정의하면서,
각 케이스에 적절한 타입의 다른 값을 연관시킬 수 있습니다.

Swift의 열거형은 그 자체로 일급 타입입니다.
열거형의 현재값에 대한
추가 정보를 제공하는 연산 프로퍼티(computed property)와
열거형이 나타내는 값과
관련된 기능을 제공하는 인스턴스 메서드와 같이
기존에는 클래스만 지원하던 많은 기능을 채택할 수 있습니다.
열거형은 케이스 초기값을 제공하기 위해 이니셜라이저를 정의할 수도 있습니다;
열거형은 기존 구현을 넘어 기능적으로 확장될 수도 있고;
표준 기능을 제공하기 위해 프로토콜을 채택할 수 있습니다.

이러한 기능의 자세한 내용은
[프로퍼티 (Properties)](./properties.md), [메서드 (Methods)](./methods.md), [초기화 (Initialization)](./initialization.md),
[확장 (Extensions)](./extensions.md), [프로토콜 (Protocols)](./protocols.md)을 참고바랍니다.

<!--
  TODO: this chapter should probably mention that enums without associated values
  are hashable and equatable by default (and what that means in practice)
-->

## 열거형 문법 (Enumeration Syntax)

열거형은 `enum` 키워드와
중괄호 안에 모든 정의를 위치시켜 나타냅니다:

```swift
enum SomeEnumeration {
    // enumeration definition goes here
}
```

<!--
  - test: `enums`

  ```swifttest
  -> enum SomeEnumeration {
        // enumeration definition goes here
     }
  ```
-->

다음 예시는 나침반의 네 가지 주요 방향을 나타냅니다:

```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

<!--
  - test: `enums`

  ```swifttest
  -> enum CompassPoint {
        case north
        case south
        case east
        case west
     }
  ```
-->

열거형에 정의된 값
(`north`, `south`, `east`, `west`)은
*열거형 케이스(enumeration cases)*입니다.
`case` 키워드를 사용하여 새로운 열거형 케이스를 선언합니다.

> Note: Swift 열거형 케이스는 C와 Objective-C 같은 언어와는 달리,
> 기본적으로 정수값을 가지지 않습니다.
> 위 예시 `CompassPoint`에
> `north`, `south`, `east`, `west`는
> 암시적으로
> `0`, `1`, `2`, `3`에 해당하지 않습니다.
> 대신 각 열거형 케이스는 그 자체로 독립적인 값이며,
> 명시적으로 정의된 `CompassPoint` 타입을 가집니다.

여러 개의 케이스는 콤마로 구분하여 한 줄로 표기할 수 있습니다:

```swift
enum Planet {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

<!--
  - test: `enums`

  ```swifttest
  -> enum Planet {
        case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
     }
  ```
-->

각 열거형 정의는 새로운 타입을 정의합니다.
Swift의 다른 타입처럼 열거형의 이름
(`CompassPoint`, `Planet`)은
대문자로 시작합니다.
열거형 타입 이름은 코드가 보다 자연스럽고 명확하게 읽히기 때문에,
복수형보다는 단수형을 사용하는 것이 좋습니다:

```swift
var directionToHead = CompassPoint.west
```

<!--
  - test: `enums`

  ```swifttest
  -> var directionToHead = CompassPoint.west
  ```
-->

`directionToHead` 타입은
`CompassPoint`의 가능한 값 중 하나로 초기화될 때, 추론됩니다.
한 번 `directionToHead`가 `CompassPoint`로 선언되면,
이후에는 더 짧게 점 문법을 사용하여 다른 `CompassPoint` 값으로 설정할 수 있습니다:

```swift
directionToHead = .east
```

<!--
  - test: `enums`

  ```swifttest
  -> directionToHead = .east
  ```
-->

`directionToHead`의 타입은 이미 알고 있으므로,
값을 설정할 때 타입을 생략할 수 있습니다.
이러한 방식은 명시적으로 타입이 지정된 열거형 값으로 작업할 때 코드를 쉽게 읽을 수 있습니다.

## 스위치 구문에서 열거형 값 일치 (Matching Enumeration Values with a Switch Statement)

개별 열거형 값을 `switch` 구문을 사용하여 일치시킬 수 있습니다:

```swift
directionToHead = .south
switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue")
}
// Prints "Watch out for penguins"
```

<!--
  - test: `enums`

  ```swifttest
  -> directionToHead = .south
  -> switch directionToHead {
        case .north:
           print("Lots of planets have a north")
        case .south:
           print("Watch out for penguins")
        case .east:
           print("Where the sun rises")
        case .west:
           print("Where the skies are blue")
     }
  <- Watch out for penguins
  ```
-->

이 코드를 아래와 같이 읽을 수 있습니다:

"`directionToHead` 값을 고려합니다.
그 값이 `.north`인 경우,
`"Lots of planets have a north"`를 출력합니다.
그 값이 `.south`인 경우,
`"Watch out for penguins"`를 출력합니다."

…그리고 나머지 케이스도 마찬가지입니다.

[제어 흐름 (Control Flow)](./control-flow.md)에서 설명했듯이,
`switch` 구문은 열거형 케이스를 고려할 때 완벽해야 합니다.
`.west`에 대한 `case`가 생략된다면,
`CompassPoint`의 모든 케이스를 고려하지 않았기 때문에
컴파일 되지 않습니다.
이러한 완전성 요구는 열거형 케이스를 실수로 누락하는 것을 방지합니다.

모든 열거형 케이스에 대해 `case`를 제공하는 것이 적절하지 않은 경우,
명시되지 않은 나머지 모든 케이스를 처리할 수 있도록 `default` 케이스를 제공할 수 있습니다:

```swift
let somePlanet = Planet.earth
switch somePlanet {
case .earth:
    print("Mostly harmless")
default:
    print("Not a safe place for humans")
}
// Prints "Mostly harmless"
```

<!--
  - test: `enums`

  ```swifttest
  -> let somePlanet = Planet.earth
  -> switch somePlanet {
        case .earth:
           print("Mostly harmless")
        default:
           print("Not a safe place for humans")
     }
  <- Mostly harmless
  ```
-->

## 열거형 케이스 반복 (Iterating over Enumeration Cases)

일부 열거형에서는
해당 열거형의 모든 케이스를 모아놓은 컬렉션이 있으면 유용합니다.
이를 위해
열거형 이름 뒤에 `: CaseIterable`을 작성하여 활성화 합니다.
그러면 Swift는 열거형 타입에 `allCases` 프로퍼티를 통해
모든 케이스의 컬렉션을 제공합니다.
다음은 그 예시입니다:

```swift
enum Beverage: CaseIterable {
    case coffee, tea, juice
}
let numberOfChoices = Beverage.allCases.count
print("\(numberOfChoices) beverages available")
// Prints "3 beverages available"
```

<!--
  - test: `enums`

  ```swifttest
  -> enum Beverage: CaseIterable {
         case coffee, tea, juice
     }
  -> let numberOfChoices = Beverage.allCases.count
  -> print("\(numberOfChoices) beverages available")
  <- 3 beverages available
  ```
-->

위의 예시에서
`Beverage` 열거형에 모든 케이스를 포함하는
컬렉션에 접근하기 위해 `Beverage.allCases`를 사용합니다.
일반 컬렉션처럼 `allCases`를 사용할 수 있습니다 ---
컬렉션의 요소는 열거형 타입의 인스턴스이기 때문에
이 경우에 각 요소는 `Beverage` 값입니다.
위의 예시는 얼마나 많은 케이스가 존재하는지 계산하고,
아래의 예시는 `for`-`in` 루프를 사용하여 모든 케이스를 반복합니다.

```swift
for beverage in Beverage.allCases {
    print(beverage)
}
// coffee
// tea
// juice
```

<!--
  - test: `enums`

  ```swifttest
  -> for beverage in Beverage.allCases {
         print(beverage)
     }
  << coffee
  << tea
  << juice
  // coffee
  // tea
  // juice
  ```
-->

위 예시에서 사용된 문법은
열거형이 [`CaseIterable`](https://developer.apple.com/documentation/swift/caseiterable) 프로토콜을
준수하도록 표시한 것입니다.
프로토콜에 대한 자세한 내용은 [프로토콜 (Protocols)](./protocols.md)을 참조 바랍니다.

## 연관값 (Associated Values)

이전 섹션의 예시는 열거형 케이스가
그 자체로 정의된(그리고 타입이 지정된) 값임을 보여줍니다.
상수나 변수를 `Planet.earth`로 설정하고,
나중에 값을 확인할 수 있습니다.
그러나 경우에 따라 각 케이스 값에
다른 타입의 데이터를 함께 저장해야 할 때가 있습니다.
이러한 추가적인 정보를 *연관값(associated value)*이라고 하며,
이 값은 해당 케이스를 사용할 때마다 달라질 수 있습니다.

Swift의 열거형은 어떤 타입이든 연관값을 저장할 수 있도록 정의할 수 있으며,
필요하다면 케이스마다 서로 다른 타입의 연관값을 가질 수도 있습니다.
이와 유사한 개념은 다른 프로그래밍 언어에서
*식별 집합체(discriminated unions)*, *태그 집합체(tagged unions)*, *변형 가능한 집합체(variants)*로
알려져 있습니다.

예를 들어 재고 추적 시스템이
두 가지 타입의 바코드로 제품을 추적해야 된다고 가정해 보겠습니다.
어떤 제품은 숫자 `0`에서 `9`를 사용하는
UPC 형식의 1D 바코드 레이블이 부착되어 있습니다.
각 바코드에는 숫자 시스템과
이어서 다섯 자리의 제조업체 코드와 다섯 자리의 제품 코드가 있습니다.
다음에는 코드가 올바르게 스캔되었는지 확인하기 위해 검사 숫자가 있습니다:

![Barcode UPC](../.gitbook/assets/barcode_UPC_2x~dark.png)

다른 제품은 모든 ISO 8859-1 문자도 사용할 수 있고
2,953개의 문자까지 인코딩할 수 있는
QR 코드 형식의 2D 바코드 레이블이 부착되어 있습니다:

![Barcode QR](../.gitbook/assets/barcode_QR_2x~dark.png)

재고 추적 시스템은 UPC 바코드를
네 개의 정수로 된 튜플로 저장하고
QR 코드 바코드를 모든 길이의 문자열로 저장하는 것이 편리합니다.

Swift에서 두 타입의 바코드를 정의하는 열거형은 다음과 같습니다:

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
```

<!--
  - test: `enums`

  ```swifttest
  -> enum Barcode {
        case upc(Int, Int, Int, Int)
        case qrCode(String)
     }
  ```
-->

이것은 아래와 같이 읽을 수 있습니다:

"\(`Int, Int, Int, Int`\) 타입의 연관값을 가진
`upc`나
`String` 타입의 연관값을 가진 `qrCode`를 취할 수 있는
`Barcode`라는 열거형 타입을 정의합니다."

이 정의는 실제 `Int`나 `String` 값을 제공하지 않습니다 ---
이것은 단지 `Barcode.upc`나 `Barcode.qrCode`와 같을 때,
`Barcode` 상수와 변수에 저장할 수 있는
연관값의 *타입*을 정의할 뿐입니다.

그러면 이러한 타입을 이용하여 새로운 바코드를 생성할 수 있습니다:

```swift
var productBarcode = Barcode.upc(8, 85909, 51226, 3)
```

<!--
  - test: `enums`

  ```swifttest
  -> var productBarcode = Barcode.upc(8, 85909, 51226, 3)
  ```
-->

이 예시는 `productBarcode`라는 새로운 변수를 생성하고,
연관값으로 `(8, 85909, 51226, 3)`이라는 튜플을 가진
`Barcode.upc` 값을 할당합니다.

같은 제품에 대해 다른 바코드 타입을 할당할 수도 있습니다:

```swift
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```

<!--
  - test: `enums`

  ```swifttest
  -> productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
  ```
-->

이 시점에서
기존의 `Barcode.upc`와 그에 해당하는 정수값은
새로운 `Barcode.qrCode`와 해당 문자열로 대체됩니다.
`Barcode` 타입의 상수와 변수는 `.upc`나 `.qrCode`
(연관값과 함께)
저장할 수 있지만 동시에 둘 다 저장할 수 없습니다.

[스위치 구문에서 열거형 값 일치 (Matching Enumeration Values with a Switch Statement)](./enumerations.md#스위치-구문에서-열거형-값-일치-matching-enumeration-values-with-a-switch-statement)에서의
예시와 유사하게
스위치 구문을 이용하여 바코드 타입을 확인할 수 있습니다.
그러나 이번에는
스위치 구문을 통해 연관값을 추출할 수 있습니다.
`switch` 케이스의 본문 내에서 사용하기 위해
상수(`let` 접두사)나
변수(`var` 접두사)로 연관값을 추출합니다:

```swift
switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```

<!--
  - test: `enums`

  ```swifttest
  -> switch productBarcode {
        case .upc(let numberSystem, let manufacturer, let product, let check):
           print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
        case .qrCode(let productCode):
           print("QR code: \(productCode).")
     }
  <- QR code: ABCDEFGHIJKLMNOP.
  ```
-->

열거형 케이스의 연관값 모두 상수로 추출하거나
변수로 추출하려면,
케이스 이름 앞에 `let`이나 `var`을 선언하여 표현할 수 있습니다:

```swift
switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```

<!--
  - test: `enums`

  ```swifttest
  -> switch productBarcode {
        case let .upc(numberSystem, manufacturer, product, check):
           print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
        case let .qrCode(productCode):
           print("QR code: \(productCode).")
     }
  <- QR code: ABCDEFGHIJKLMNOP.
  ```
-->

열거형의 케이스 중 하나만 일치시키는 경우에 ---
예를 들어
일치되는 케이스의 연관값을 추출하는 경우 ---
스위치 구문의 전체 케이스를 작성하는 대신
`if`-`case` 구문을 사용할 수 있습니다.
다음은 그 예시입니다:

```swift
if case .qrCode(let productCode) = productBarcode {
    print("QR code: \(productCode).")
}
```

이 코드의
`productBarcode` 변수는
`.qrCode(let productCode)` 패턴과 일치합니다.
그리고 스위치 케이스에서와 같이
`let`은 상수로 연관값을 추출합니다.
`if`-`case` 구문에 대한 자세한 내용은
[패턴 (Patterns)](./control-flow.md#패턴-patterns)을 참고바랍니다.

## 원시값 (Raw Values)

[연관값 (Associated Values)](#연관값-associated-values)에서의 바코드 예시는
열거형 케이스에 다른 타입의 연관값을
저장할 수 있음을 보여줍니다.
연관값과는 별개로,
열거형 케이스는 모두 동일한 타입의 기본값
(원시값(raw values))으로
미리 채워질 수도 있습니다.

다음은 명명된 열거형 케이스와 함께 ASCII 값을 저장하는 예시입니다:

```swift
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```

<!--
  - test: `rawValues`

  ```swifttest
  -> enum ASCIIControlCharacter: Character {
        case tab = "\t"
        case lineFeed = "\n"
        case carriageReturn = "\r"
     }
  ```
-->

여기서 `ASCIIControlCharacter`라는 열거형의 원시값은
`Character` 타입으로 정의되고,
일반적인 ASCII 제어 문자 중 일부를 설정합니다.
`Character` 값은 [문자열과 문자 (Strings and Characters)](./strings-and-characters.md)에 설명되어 있습니다.

원시값은
문자열, 문자, 정수 또는 부동소수점 숫자 타입이 가능합니다.
각 원시값은 열거형 선언 내에서 유일한 값이어야 합니다.

열거형에 추가값을 부여하기 위해
원시값과 연관값 모두 사용할 수 있지만
둘의 차이점을 이해하는 것이 중요합니다.
원시값은 위의 세 가지 ASCII 코드와 같이
코드에서 열거형 케이스를 정의할 때
선택합니다.
특정 열거형 케이스의 원시값은 항상 동일합니다.
반면에
연관값은 열거형 케이스 중 하나를 사용하여
새로운 상수나 변수를 생성할 때 선택하고,
특정 열거형 케이스의 연관값은 다른 값을 선택할 수 있습니다.

### 암시적으로 할당된 원시값 (Implicitly Assigned Raw Values)

정수나 문자열 원시값이 저장된 열거형으로 동작 시,
각 케이스에 명시적으로 원시값을 가질 필요가 없습니다.
원시값을 설정하지 않으면 Swift는 자동으로 값을 할당합니다.

예를 들어 정수를 원시값으로 사용하면,
각 케이스의 암시적 값은 이전 케이스보다 하나씩 증가시킵니다.
첫 번째 케이스에 값이 지정되지 않으면 `0`으로 설정합니다.

아래의 열거형은 태양으로부터 각 행성의 순서를 정수값으로 나타내는
이전 `Planet` 열거형을 확장한 버전입니다:

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

<!--
  - test: `rawValues`

  ```swifttest
  -> enum Planet: Int {
        case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
     }
  ```
-->

위의 예시에서
`Planet.mercury`는 명시적으로 원시값 `1`을 가지고,
`Planet.venus`는 암시적으로 원시값 `2`를 가집니다.

원시값으로 문자열이 사용될 때,
각 케이스의 이름 자체가 암시적으로 원시값으로 자동 할당됩니다.

아래의 열거형은 각 방향의 이름을 문자열 원시값으로 나타내는
이전 `CompassPoint` 열거형을 확장한 버전입니다:

```swift
enum CompassPoint: String {
    case north, south, east, west
}
```

<!--
  - test: `rawValues`

  ```swifttest
  -> enum CompassPoint: String {
        case north, south, east, west
     }
  ```
-->

위의 예시에서
`CompassPoint.south`는 암시적으로 `"south"`라는 원시값을 가지고, 다른 케이스도 마찬가지로 케이스 이름을 원 값으로 자동 할당받습니다.

`rawValue` 프로퍼티를 사용하여 열거형 케이스의 원시값에 접근할 수 있습니다:

```swift
let earthsOrder = Planet.earth.rawValue
// earthsOrder is 3

let sunsetDirection = CompassPoint.west.rawValue
// sunsetDirection is "west"
```

<!--
  - test: `rawValues`

  ```swifttest
  -> let earthsOrder = Planet.earth.rawValue
  /> earthsOrder is \(earthsOrder)
  </ earthsOrder is 3

  -> let sunsetDirection = CompassPoint.west.rawValue
  /> sunsetDirection is \"\(sunsetDirection)\"
  </ sunsetDirection is "west"
  ```
-->

### 원시값으로 초기화 (Initializing from a Raw Value)

원시값 타입으로 열거형을 정의하면,
열거형은 원시값의 타입(`rawValue`라는 매개변수)을 사용하는
이니셜라이저를 제공하고
열거형 케이스나 `nil`을 반환합니다.
이 이니셜라이저를 사용하여 열거형의 새 인스턴스를 생성할 수 있습니다.

이 예시는 `7` 원시값으로 천왕성을 식별합니다:

```swift
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet is of type Planet? and equals Planet.uranus
```

<!--
  - test: `rawValues`

  ```swifttest
  -> let possiblePlanet = Planet(rawValue: 7)
  >> print(type(of: possiblePlanet))
  << Optional<Planet>
  >> assert(possiblePlanet == .uranus)
  // possiblePlanet is of type Planet? and equals Planet.uranus
  ```
-->

모든 `Int` 값이 행성과 일치하는 것은 아닙니다.
이러한 점 때문에, 원시값 이니셜라이저는 항상 *옵셔널* 열거형 케이스를 반환합니다.
위의 예시에서 `possiblePlanet`은 `Planet?` 타입이나
"옵셔널 `Planet`" 타입입니다.

> Note: 원시값 이니셜라이저는 모든 원시값을 열거형 케이스로 반환할 수 없으므로,
> 실패 가능 이니셜라이저입니다.
> 자세한 내용은 [실패 가능한 이니셜라이저 (Failable Initializers)](./initialization.md#실패-가능한-이니셜라이저-failable-initializers)를 참고바랍니다.

`11`의 위치로 행성을 찾는다면,
원시값 이니셜라이저에서 반환된 옵셔널 `Planet` 값은 `nil`입니다:

```swift
let positionToFind = 11
if let somePlanet = Planet(rawValue: positionToFind) {
    switch somePlanet {
    case .earth:
        print("Mostly harmless")
    default:
        print("Not a safe place for humans")
    }
} else {
    print("There isn't a planet at position \(positionToFind)")
}
// Prints "There isn't a planet at position 11"
```

<!--
  - test: `rawValues`

  ```swifttest
  -> let positionToFind = 11
  -> if let somePlanet = Planet(rawValue: positionToFind) {
        switch somePlanet {
           case .earth:
              print("Mostly harmless")
           default:
              print("Not a safe place for humans")
        }
     } else {
        print("There isn't a planet at position \(positionToFind)")
     }
  <- There isn't a planet at position 11
  ```
-->

이 예시는 `11`의 원시값으로 행성을 찾기위해 옵셔널 바인딩을 사용합니다.
`if let somePlanet = Planet(rawValue: 11)` 구문은 옵셔널 `Planet`을 생성하고,
해당 값이 존재한다면 옵셔널 `Planet`의 값을 `somePlanet`에 설정합니다.
이 경우 `11`의 위치로 행성을 가져올 수 없으므로,
`else` 구문이 실행됩니다.

<!--
  TODO: Switch around the order of this chapter so that all of the non-union stuff
  is together, and the union bits (aka Associated Values) come last.
-->

## 재귀 열거형 (Recursive Enumerations)

*재귀 열거형(recursive enumeration)*은
열거형 케이스에 하나 이상의 연관값으로
열거형의 다른 인스턴스를 가지고 있는 열거형입니다.
열거형 케이스가 재귀적임을 나타내기 위해
케이스 작성 전에 `indirect`를 작성하여,
컴파일러에게 간접 참조(indirection) 계층을 삽입하도록 지시합니다.

예를 들어 다음은 간단한 산술 표현식을 저장하는 열거형입니다:

```swift
enum ArithmeticExpression {
    case number(Int)
    indirect case addition(ArithmeticExpression, ArithmeticExpression)
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

<!--
  - test: `recursive-enum-intro`

  ```swifttest
  -> enum ArithmeticExpression {
         case number(Int)
         indirect case addition(ArithmeticExpression, ArithmeticExpression)
         indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
     }
  ```
-->

열거형 시작 전에 `indirect`를 작성하여
연관값을 가진 모든 열거형 케이스에 간접 참조를 적용할 수 있습니다:

```swift
indirect enum ArithmeticExpression {
    case number(Int)
    case addition(ArithmeticExpression, ArithmeticExpression)
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

<!--
  - test: `recursive-enum`

  ```swifttest
  -> indirect enum ArithmeticExpression {
         case number(Int)
         case addition(ArithmeticExpression, ArithmeticExpression)
         case multiplication(ArithmeticExpression, ArithmeticExpression)
     }
  ```
-->

이 열거형은 세 가지 종류의 산술 표현식을 저장할 수 있습니다:
단순 숫자,
두 표현식의 덧셈,
두 표현식의 곱셈.
`addition`과 `multiplication` 케이스는
산술 표현식인 연관값을 가지고 있습니다 ---
이 연관값은 중첩 표현식을 가능하게 해줍니다.
예를 들어 `(5 + 4) * 2` 표현식은
곱셈의 오른쪽은 하나의 숫자를 가지고 있고
왼쪽은 다른 표현식을 가지고 있습니다.
데이터는 중첩되기 때문에,
데이터를 저장하는 열거형도 중첩을 지원해야 합니다 ---
이것은 재귀 열거형을 의미입니다.
아래의 코드는 `(5 + 4) * 2`에 대해 생성되는
`ArithmeticExpression` 재귀 열거형을 나타냅니다:

```swift
let five = ArithmeticExpression.number(5)
let four = ArithmeticExpression.number(4)
let sum = ArithmeticExpression.addition(five, four)
let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))
```

<!--
  - test: `recursive-enum`

  ```swifttest
  -> let five = ArithmeticExpression.number(5)
  -> let four = ArithmeticExpression.number(4)
  -> let sum = ArithmeticExpression.addition(five, four)
  -> let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))
  ```
-->

재귀 함수는 재귀 구조를 가진 데이터로 작업하는
직관적인 방법입니다.
예를 들어 다음은 산술 표현식을 평가하는 함수입니다:

```swift
func evaluate(_ expression: ArithmeticExpression) -> Int {
    switch expression {
    case let .number(value):
        return value
    case let .addition(left, right):
        return evaluate(left) + evaluate(right)
    case let .multiplication(left, right):
        return evaluate(left) * evaluate(right)
    }
}

print(evaluate(product))
// Prints "18"
```

<!--
  - test: `recursive-enum`

  ```swifttest
  -> func evaluate(_ expression: ArithmeticExpression) -> Int {
         switch expression {
             case let .number(value):
                 return value
             case let .addition(left, right):
                 return evaluate(left) + evaluate(right)
             case let .multiplication(left, right):
                 return evaluate(left) * evaluate(right)
         }
     }

  -> print(evaluate(product))
  <- 18
  ```
-->

이 함수는 단순 숫자인 경우
해당 연관값을 그대로 반환하여 평가합니다.
덧셈이나 곱셈인 경우에는
왼쪽 표현식을 평가하고,
오른쪽 표현식을 평가한 다음에
두 결과를 더하거나 곱하여 평가합니다.

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