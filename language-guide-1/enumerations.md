# 열거형 \(Enumerations\)

_열거형 \(enumeration\)_ 은 관련된 값의 그룹을 위한 일반 타입을 정의하고 코드에서 타입-세이프 방법으로 값을 동작하게 합니다.

C에 익숙하다면 C 열거형은 정수값을 설정하기 위해 연관된 이름을 할당하는 것을 알고 있습니다. Swift에서의 열거형은 훨씬 유연하고 열거형의 각 케이스에 값을 제공하지 않아도 됩니다. 값 \(원시값\)이 각 열거형 케이스로 제공된다면 그 값은 문자열, 문자 또는 정수 또는 부동 소수 타입일 수 있습니다.

또는 열거형 케이스는 각 다른 케이스 값으로 저장될 모든 타입의 관련된 값을 지정할 수 있습니다. 하나의 열거형의 일부로 관련된 케이스의 공통 세트를 정의할 수 있으며 각각은 연관된 적절한 타입의 다른값 세트를 가지고 있습니다.

Swift의 열거형은 그 자체로 1급 타입입니다. 열거형의 현재값에 대한 추가 정보를 제공하는 계산된 프로퍼티와 열거형이 나타내는 값과 관련된 기능을 제공하는 인스턴스 메서드와 같이 전통적으로 클래스에서만 지원되는 많은 기능을 채택합니다. 열거형은 케이스 초기값을 제공하기 위해 초기화를 정의할 수도 있습니다. 열거형은 기존 구현을 넘어 기능적으로 확장될 수도 있고 표준 기능을 제공하기 위해 프로토콜을 준수할 수 있습니다.

이러한 기능의 자세한 내용은 [프로퍼티 \(Properties\)](properties.md), [메서드 \(Methods\)](methods.md), [초기화 \(Initialization\)](initialization.md), [확장 \(Extensions\)](extensions.md), 와 [프로토콜 \(Protocols\)](protocols.md) 을 참고 바랍니다.

## 열거형 구문 \(Enumeration Syntax\)

열거형은 `enum` 키워드와 중괄호 안에 모든 정의를 위치시켜 나타냅니다:

```swift
enum SomeEnumeration {
    // enumeration definition goes here
}
```

다음 예제는 나침반의 4개의 주요 포인트를 나타냅니다:

```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

열거형 안에 정의된 값 \(`north`, `south`, `east`, `west`\)은 _열거형 케이스 \(enumeration cases\)_ 입니다. 새로운 열거형 케이스를 나타내기 위해 `case` 키워드를 사용합니다.

> NOTE   
> Swift 열거형 케이스 C와 Objective-C 처럼 기본적으로 정수값을 설정하지 않습니다. 위 예제 `CompassPoint` 에 `north`, `south`, `east`, `west` 는 `0`, `1`, `2`, `3` 과 같지 않습니다. 대신 다른 열거형 케이스는 `CompassPoint` 의 명시적으로 정의된 타입으로 자체 값입니다.

여러개의 케이스는 콤마로 구분하여 한줄로 표기할 수 있습니다:

```swift
enum Planet {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

각 열거형 정의는 새로운 타입으로 정의합니다. Swift의 다른 타입처럼 대문자로 시작하는 그들의 이름 \(`CompassPoint` 와 `Planet`\)이 타입입니다. 열거형 타입에 복수 이름이 아닌 단수 이름으로 지정하여 읽기 쉽습니다:

```swift
var directionToHead = CompassPoint.west
```

`directionToHead` 타입은 `CompassPoint` 의 가능한 값중 하나로 초기화 될 때 유추됩니다. `directionToHead` 는 `CompassPoint` 로 선언되고 더 짧게 점 구문을 사용하여 다른 `CompassPoint` 값을 설정할 수 있습니다:

```swift
directionToHead = .east
```

`directionToHead` 의 타입은 이미 알고 있으므로 값을 설정할 때 타입을 삭제할 수 있습니다. 따라서 명시적으로 타입화 된 열거형 값으로 작업할 때 코드를 쉽게 읽을 수 있습니다.

## Switch 구문에서 열거형 값 일치 \(Matching Enumeration Values with Switch Statement\)

`switch` 구문으로 각각의 열거형 값을 일치시킬 수 있습니다:

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

이 코드를 아래와 같이 읽을 수 있습니다:

"`directionToHead` 값을 고려합니다. `.north` 와 같은 케이스일 경우 `"Lots of planets have a north"` 를 출력합니다. `.south` 와 같은 케이스일 경우 `"Watch out for penguins"` 를 출력합니다."

등등

[제어 흐름 \(Control Flow\)](control-flow.md) 에서 설명 했듯이 `switch` 구문은 열거형 케이스를 고려할 때 완벽해야 합니다. `.west` 에 대한 `case` 가 생략된다면 `CompassPoint` 케이스에 대해 모든 목록이 고려되지 않았으므로 이 코드는 컴파일 되지 않습니다. 완전성을 요구하면 열거형 케이스가 실수로 생략되지 않도록 합니다.

모든 열거형 케이스에 대해 `case` 를 제공하는 것이 적절하지 않은 경우 명시적으로 해결되지 않은 사례를 포함하는 `default` 케이스를 제공할 수 있습니다:

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

## 열거형 케이스 반복 \(Iterating over Enumeration Cases\)

일부 열거형의 경우 열거형의 모든 케이스를 수집하는 것이 유용합니다. 열거형 이름 뒤에 `: CaseIterable` 을 작성하여 활성화 합니다. Swift는 열거형 타입에 `allCases` 프로퍼티로 모든 케이스를 수집하고 방출합니다. 다음은 예제입니다:

```swift
enum Beverage: CaseIterable {
    case coffee, tea, juice
}
let numberOfChoices = Beverage.allCases.count
print("\(numberOfChoices) beverages available")
// Prints "3 beverages available"
```

위의 예제에서 `Beverage` 열거형에 모든 케이스를 포함하는 콜렉션에 접근하기 위해 `Beverage.allCases` 를 작성합니다. 다른 콜렉션 처럼 `allCases` 를 사용할 수 있습니다. 콜렉션의 요소는 열거형 타입의 인스턴스 이기 때문에 이 경우에는 `Beverage` 값들입니다. 위의 예제는 얼마나 많은 케이스가 존재하는지 계산하고 아래의 예제는 `for` 루프를 사용하여 모든 케이스를 반복합니다.

```swift
for beverage in Beverage.allCases {
    print(beverage)
}
// coffee
// tea
// juice
```

위 예제에서 사용된 구문은 열거형이 [`CaseIterable`](https://developer.apple.com/documentation/swift/caseiterable) 프로토콜을 준수하는 것으로 표시합니다. 자세한 내용은 [프로토콜 \(Protocols\)](protocols.md) 을 참조 바랍니다.

## 연관된 값 \(Associated Values\)

이전 섹션에서 예제는 열거형 케이스가 어떻게 정의되고 값이 입력되는지 보여줍니다. 상수 또는 변수를 `Planet.earth` 로 설정하고 나중에 값을 체크할 수 있습니다. 그러나 경우에 따라 이러한 케이스 값과 함께 다른 타입의 값을 저장할 수 있는 것이 유용합니다. 이 추가적인 정보를 _연관된 값 \(associated value\)_ 라고 하면 해당 케이스를 코드에서 값으로 사용할 때마다 달라집니다.

주어진 타입의 연관된 값을 저장하기 위해 Swift 열거형을 정의할 수 있고 이 값 타입은 필요에 따라 열거형의 각 케이스에 따라 달라질 수 있습니다. 이와 유사한 열거형은 다른 프로그래밍 언어에서 _식별된 집합체 \(discriminated unions\)_, _태그된 집합체 \(tagged unions\)_ 또는 _변형 가능한 집합체 \(variants\)_ 로 알려져 있습니다.

예를 들어 재고 추적 시스템이 2가지 타입의 바코드로 제품을 추적해야 된다고 가정해 보겠습니다. 어떤 제품은 숫자 `0` 에서 `9` 를 사용하는 UPC 형식의 1D 바코드 라벨이 부착되어 있습니다. 각 바코드에는 숫자 시스템과 이어서 5자리의 제조업체 코드와 5자리의 제품 코드가 있습니다. 다음에는 코드가 올바르게 스캔되었는지 확인하기 위해 검사 숫자가 있습니다:

![Barcode UPC](../.gitbook/assets/08_barcode_upc_2x.png)

다른 제품은 어떠한 ISO 8859-1 문자도 사용할 수 있고 2,953개의 문자까지 인코딩할 수 있는 QR 코드 형식의 2D 바코드 라벨이 부착되어 있습니다:

![Barcode QR](../.gitbook/assets/08_barcode_QR_2x.png)

재고 추적 시스템은 UPC 바코드를 4개의 정수로 된 튜플로 저장하고 QR 코드 바코드를 모든 길이의 문자열로 저장하는 것이 편리합니다.

Swift에서 두 타입의 바코드를 정의하는 열거형은 다음과 같습니다:

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
```

이것은 아래와 같이 읽을 수 있습니다:

"`(Int, Int, Int, Int)` 타입의 연관된 값을 가진 `upc` 또는 `String` 타입의 연관된 값을 가진 `qrCode` 를 취할 수 있는 `Barcode` 라는 열거형 타입을 정의합니다."

이 정의는 어떠한 실질적인 `Int` 또는 `String` 값을 제공하지 않습니다. 이것은 단지 `Barcode.upc` 또는 `Barcode.qrCode` 와 같을 때 `Barcode` 상수와 변수에 저장할 수 있는 연관된 값의 _타입_ 을 정의할 뿐입니다.

그러면 이러한 타입을 이용하여 새로운 바코드를 생성할 수 있습니다:

```swift
var productBarcode = Barcode.upc(8, 85909, 51226, 3)
```

이 예제는 `productBarcode` 라 불리는 새로운 변수를 생성하고 연관된 튜플값인 `(8, 85909, 51226, 3)` 을 `Barcode.upc` 의 값으로 할당합니다.

같은 상품의 다른 바코드 타입을 할당할 수 있습니다:

```swift
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```

여기서 기존의 `Barcode.upc` 와 그것의 정수 값은 새로운 `Barcode.qrCode` 와 그것의 문자열 값으로 대체됩니다. `Barcode` 타입의 상수와 변수는 `.upc` 또는 `.qrCode` 모두 이것들과 연관된 값으로 저장할 수 있지만 오직 하나의 값만 저장할 수 있습니다.

[스위치 구문으로 열거형 값 일치 \(Matching Enumeration Values with a Switch Statement\)](enumerations.md#switch-matching-enumeration-values-with-switch-statement) 에서의 예제와 유사하게 스위치 구문을 이용하여 다른 바코드 타입을 확인할 수 있습니다. 그러나 이번에는 관련값이 스위치 구문의 일부로 추출됩니다. `switch` 케이스의 바디 내에서 사용하기 위해 상수 \(`let` 접두사\) 또는 변수 \(`var` 접두사\)로 각 연관된 값을 추출합니다:

```swift
switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```

열거형 케이스를 위한 연관된 값 모두 상수로 추출하거나 변수로 추출하려면 간결하게 케이스 이름 앞에 `var` 또는 `let` 을 선언하면 됩니다:

```swift
switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```

## 원시값 \(Raw Values\)

[연관된 값 \(Associated Values\)](enumerations.md#associated-values) 에서의 바코드 예제는 어떻게 열거형 케이스에 다른 타입의 연관된 값을 저장한다고 선언 하는지를 보여줍니다. 연관된 값의 대안으로 열거형 케이스는 모두 동일한 타입의 기본값 \(_원시값 \(raw values\)_\)으로 미리 채워질 수 있습니다.

다음은 명명된 열거형 케이스와 함께 원시 ASCII 값을 저장하는 예입니다:

```swift
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```

여기서 `ASCIIControlCharacter` 라 하는 열거형의 원시값은 `Character` 타입으로 정의되고 일반적인 ASCII 제어 문자 중 일부를 선정합니다. `Character` 값은 [문자열과 문자 \(Strings and Characters\)](strings-and-characters.md) 에서 설명되어 있습니다.

원시값은 문자열, 문자, 또는 어떠한 정수 또는 부동소수점 숫자 타입이 가능합니다. 각 원시값은 열거형 선언부 내에서 유일한 값이어야 합니다.

> NOTE   
> 원시값은 연관된 값 \(associated values\)과 같지 않습니다. 원시값은 위의 3개의 ASCII 코드처럼 코드에서 열거형을 처음 정의할 때 미리 설정하는 값입니다. 특정 열거형 케이스를 위한 원시값은 항상 같습니다. 연관된 값은 열거형 케이스 중 하나를 기반으로 새로운 상수 또는 변수를 생성할 때 설정하고 달라질 수 있습니다.

### 암시적으로 할당된 원시값 \(Implicitly Assigned Raw Values\)

정수 또는 문자열 원시값이 저장된 열거형으로 동작 시 각 케이스에 명시적으로 원시값을 가질 필요가 없습니다. 원시값을 설정하지 않으면 Swift는 자동적으로 값을 할당합니다.

예를 들어 정수를 원시값으로 사용하면 각 케이스의 암시적 값은 이전 케이스보다 하나씩 증가시킵니다. 첫번째 케이스에 값 설정이 안되어 있으면 `0` 으로 설정합니다.

아래의 열거형은 태양으로 부터 각 행성의 순서를 정수값으로 나타내는 이전 `Planet` 열거형의 변형된 버전입니다:

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

위의 예제에서 `Planet.mercury` 는 명시적으로 원시값 `1` 을 가지고 `Planet.venus` 는 암시적으로 원시값 `2` 를 가집니다.

원시값으로 문자열이 사용될 때 각 케이스의 암시적 값은 케이스의 이름의 문자입니다.

아래의 열거형은 각 방향의 이름을 문자열 원시값으로 나타내는 이전 `CompassPoint` 열거형의 변형된 버전입니다:

```swift
enum CompassPoint: String {
    case north, south, east, west
}
```

위의 예제에서 `CompassPoint.south` 는 `"south"` 의 암시적 원시값을 가지고 있습니다.

`rawValue` 프로퍼티를 사용하여 열거형 케이스의 원시값에 접근할 수 있습니다:

```swift
let earthsOrder = Planet.earth.rawValue
// earthsOrder is 3

let sunsetDirection = CompassPoint.west.rawValue
// sunsetDirection is "west"
```

### 원시값으로 초기화 \(Initializing from a Raw Value\)

원시값 타입으로 열거형을 정의한다면 열거형은 원시값의 타입 \(`rawValue` 라는 파라미터\)을 사용하는 초기화를 자동으로 수신하고 열거형 케이스 또는 `nil` 을 반환합니다. 이 초기화를 이용하여 열거형의 새 인스턴스를 만들 수 있습니다.

이 예제는 `7` 원시값으로 천왕성을 식별합니다:

```swift
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet is of type Planet? and equals Planet.uranus
```

그러나 가능한 모든 `Int` 값으로 행성을 찾지 않습니다. 이러한 점 때문에 원시값 초기화는 항상 _옵셔널_ 열거형 케이스를 반환합니다. 위의 예제에서 `possiblePlanet` 은 `Planet?` 또는 "옵셔널 `Planet`" 타입입니다.

> NOTE   
> 원시값 초기화는 모든 원시값을 열거형 케이스로 반환할 수 없으므로 초기화가 실패할 수 있습니다. 더 자세한 내용은 [실패 가능한 초기화 구 \(Failable Initializers\)](initialization.md#failable-initializers) 을 참고 바랍니다.

`11` 의 위치로 행성을 찾는다면 원시값 초기화로 부터 반환된 옵셔널 `Planet` 값은 `nil` 입니다:

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

이 예제는 `11` 의 원시값으로 행성을 찾기위해 옵셔널 바인딩을 사용합니다. `if let somePlanet = Planet(rawValue: 11)` 구문은 옵셔널 `Planet` 을 생성하고 가져올 수 있다면 옵셔널 `Planet` 의 값을 `somePlanet` 에 설정합니다. 이 경우 `11` 의 위치로 행성을 가져올 수 없으므로 대신에 `else` 구문이 실행됩니다.

## 재귀 열거형 \(Recursive Enumerations\)

_재귀 열거형 \(recursive enumeration\)_ 은 열거형 케이스에 하나 이상의 연관된 값으로 열거형의 다른 인스턴스를 가지고 있는 열거형입니다. 열거형 케이스가 재귀적임을 나타내기 위해 케이스 작성 전에 `indirect` 를 작성하여 컴파일러에게 필요한 간접 \(indirection\) 계층을 삽입하도록 지시합니다.

예를 들어 다음은 간단한 산술 표현식을 저장하는 열거형입니다:

```swift
enum ArithmeticExpression {
    case number(Int)
    indirect case addition(ArithmeticExpression, ArithmeticExpression)
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

열거형 시작 전에 `indirect` 를 작성하여 연관된 값을 가진 모든 열거형 케이스에 간접을 활성화 할 수 있습니다:

```swift
indirect enum ArithmeticExpression {
    case number(Int)
    case addition(ArithmeticExpression, ArithmeticExpression)
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

이 열거형은 숫자, 2개의 표현식의 덧셈, 2개의 표현식의 곱셈의 3가지의 산술 표현식을 저장할 수 있습니다. `addition` 과 `multiplication` 케이스는 산술 표현식인 연관된 값을 가지고 있고 이 연관된 값은 중첩 표현식을 가능하게 해줍니다. 예를 들어 `(5 + 4) * 2` 표현식은 곱셈의 우항은 하나의 숫자를 가지고 있고 좌항은 다른 표현식을 가지고 있습니다. 데이터는 중첩되기 때문에 데이터를 저장하는 열거형도 중첩을 지원해야 합니다. 이것은 열거형은 재귀적이어야 한다는 의미입니다. 아래의 코드는 `(5 + 4) * 2` 에 대해 생성되는 `ArithmeticExpression` 재귀 열거형을 나타냅니다:

```swift
let five = ArithmeticExpression.number(5)
let four = ArithmeticExpression.number(4)
let sum = ArithmeticExpression.addition(five, four)
let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))
```

재귀 함수는 재귀 구조를 가진 데이터로 작업하는 간단한 방법입니다. 예를 들어 다음은 산술 표현식을 판단하는 함수입니다:

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

이 함수는 단순하게 관련된 값을 반환하여 숫자를 판단합니다. 좌항의 식을 판단하고 우항의 식을 판단한 다음에 이를 더하거나 곱하여 덧셈 또는 곱셈을 판단합니다.

