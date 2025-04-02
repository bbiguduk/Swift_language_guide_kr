# 초기화 \(Initialization\)

타입의 저장된 프로퍼티에 초기값을 설정하고 초기 설정을 수행합니다.

_초기화 \(Initialization\)_ 는 인스턴스의 클래스, 구조체, 또는 열거형을 사용하기 위해 준비하는 단계입니다. 이 단계에는 인스턴스에 각 저장된 프로퍼티에 초기값을 설정하고 새로운 인스턴스가 사용할 준비가 되기 전에 다른 설정이나 초기화를 수행하는 것을 포함합니다.

특정 타입에 새로운 인스턴스를 생성하기 위해 특수 메서드를 호출하는 것처럼 _초기화 구문 \(initializers\)_ 을 정의하여 초기화를 구현합니다. Objective-C 초기화 구문과 달리 Swift 초기화 구문은 값을 반환하지 않습니다. 초기화의 주요 역할은 처음 사용되기 전에 타입의 새로운 인스턴스가 올바르게 초기화되는 것을 보장하는 것입니다.

클래스 타입의 인스턴스는 클래스의 인스턴스가 할당 해제되기 전에 정리를 수행하는 _초기화 해제 \(deinitializer\)_ 도 구현할 수 있습니다. 자세한 내용은 [초기화 해제 \(Deinitialization\)](deinitialization.md) 를 참고 바랍니다.

## 저장된 프로퍼티에 초기값 설정 \(Setting Initial Values for Stored Properties\)

클래스와 구조체는 해당 클래스 또는 구조체의 인스턴스가 생성될 때까지 모든 저장된 프로퍼티에 적절한 초기값을 _반드시_ 설정해야 합니다. 저장된 프로퍼티는 확정되지 않은 상태로 남아 있을 수 없습니다.

초기화 구문 내에서 저장된 프로퍼티에 초기값을 설정하거나 프로퍼티의 정의 부분으로 기본 프로퍼티 값을 할당할 수 있습니다. 이러한 동작에 대해 다음 섹션에 설명 하도록 하겠습니다.

> Note   
> 저장된 프로퍼티에 기본값을 할당하거나 초기화 구문 내에서 초기값을 설정할 때 해당 프로퍼티의 값은 모든 프로퍼티 관찰자 호출없이 직접 설정됩니다.

### 초기화 구문 \(Initializers\)

_초기화 구문 \(Initializers\)_ 은 특정 타입의 새로운 인스턴스를 생성하기 위해 호출됩니다. 가장 간단한 형식으로 초기화 구문은 `init` 키워드를 사용하여 작성하며 파라미터가 없는 인스턴스 메서드와 같습니다:

```swift
init() {
    // perform some initialization here
}
```

아래의 예제는 화씨 눈금으로 표현된 온도를 저장하는 `Fahrenheit` 라는 새로운 구조체를 정의합니다. `Fahrenheit` 구조체는 `Double` 타입의 `temperature` 라는 하나의 저장된 프로퍼티를 가지고 있습니다:

```swift
struct Fahrenheit {
    var temperature: Double
    init() {
        temperature = 32.0
    }
}
var f = Fahrenheit()
print("The default temperature is \(f.temperature)° Fahrenheit")
// Prints "The default temperature is 32.0° Fahrenheit"
```

이 구조체는 `32.0` \(물이 얼어버리는 화씨 온도\)의 값으로 저장된 온도로 초기화되고 파라미터가 없는 `init` 의 단일 초기화 구문을 정의합니다.

### 기본 프로퍼티 값 \(Default Property Values\)

위에서 보았듯이 초기화 구문 내에서 저장된 프로퍼티에 초기값을 설정할 수 있습니다. 또는 프로퍼티의 선언의 일부로 _기본 프로퍼티 값 \(default property value\)_ 을 지정합니다. 프로퍼티가 정의될 때 프로퍼티에 초기값을 할당하는 것으로 기본 프로퍼티 값을 지정합니다.

> Note   
> 프로퍼티가 항상 같은 초기값을 갖는다면 초기화 구문 내에서 값을 설정하기보다 기본값을 제공하십시오. 결과는 같지만 기본값은 프로퍼티의 초기화를 선언에 더 가깝게 연결합니다. 이것은 초기화 구문을 더 짧고, 명확하게 하고 기본값으로 부터 프로퍼티의 타입을 유추할 수 있습니다. 기본값은 이 챕터 후반부에 설명하듯이 기본 초기화 구문과 초기화 구문 상속을 더 쉽게 활용할 수 있습니다.

프로퍼티가 선언될 때 `temperature` 에 기본값을 제공하여 더 간단한 형식으로 위의 `Fahrenheit` 구조체를 작성할 수 있습니다:

```swift
struct Fahrenheit {
    var temperature = 32.0
}
```

## 초기화 구문 사용자화 \(Customizing Initialization\)

입력 파라미터와 옵셔널 프로퍼티 타입이나 다음 섹션에서 설명하듯이 초기화 중에 상수 프로퍼티 할당으로 초기화 단계를 사용자화 할 수 있습니다.

### 초기화 파라미터 \(Initialization Parameters\)

초기화 단계를 사용자화 하는 값의 타입과 이름을 정의하기 위해 초기화의 정의의 부분으로 _초기화 파라미터 \(initialization parameters\)_ 를 제공할 수 있습니다. 초기화 파라미터는 함수와 메서드 파라미터로 동일한 기능과 구문을 가지고 있습니다.

다음 예제는 섭씨로 표현되는 온도를 저장하는 `Celsius` 라는 구조체를 정의합니다. `Celsius` 구조체는 다른 온도 체계에서의 값으로 구조체의 새로운 인스턴스를 초기화하기 위해 `init(fromFahrenheit:)` 와 `init(fromKelvin:)` 이라 하는 2개의 초기화 구문을 구현합니다:

```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
}
let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
// boilingPointOfWater.temperatureInCelsius is 100.0
let freezingPointOfWater = Celsius(fromKelvin: 273.15)
// freezingPointOfWater.temperatureInCelsius is 0.0
```

첫번째 초기화 구문은 `fromFahrenheit` 의 인수 라벨과 `fahrenheit` 의 파라미터 명을 가진 하나의 초기화 파라미터를 가지고 있습니다. 두번째 초기화 구문은 `fromKelvin` 의 인수 라벨과 `kelvin` 의 파라미터 명을 가진 하나의 초기화 파라미터를 가지고 있습니다. 두 초기화 구문 모두 단일 인수를 섭씨 값으로 변환하고 `temperatureInCelsius` 라는 프로퍼티에 값을 저장합니다.

### 파라미터 이름과 인수 라벨 \(Parameter Names and Argument Labels\)

함수와 메서드 파라미터와 마찬가지로 초기화 파라미터는 초기화 구문의 본문 내에서 사용하는 파라미터 명과 초기화 구문을 호출할 때 사용하는 인수 라벨 모두 가질 수 있습니다.

그러나 초기화 구문은 함수와 메서드 처럼 소괄호 앞에 식별 함수 이름을 가지지 않습니다. 따라서 초기화 구문의 파라미터의 이름과 타입은 어떤 초기화 구문을 호출해야 하는지 식별하는데 특히 중요한 역할을 합니다. 이러한 이유 때문에 Swift는 제공하지 않으면 초기화 구문에서 _모든_ 파라미터에 대해 자동적으로 인수 라벨을 제공합니다.

다음 예제는 `red`, `green`, 그리고 `blue` 라는 3개의 프로퍼티 상수를 가지는 `Color` 라는 구조체를 정의합니다. 이 프로퍼티는 색깔에서 빨간색, 초록색, 그리고 파란색을 나타내기 위해 `0.0` 에서 `1.0` 사이의 값을 저장합니다.

`Color` 는 빨간색, 초록색, 그리고 파란색의 컴포넌트를 위해 `Double` 타입의 3개의 적절한 명칭의 파라미터를 가진 초기화 구문을 제공합니다. `Color` 는 3개의 색깔 컴포넌트에 동일한 값을 제공할 때 사용하는 단일 `white` 파라미터를 가진 두번째 초기화 구문도 제공합니다.

```swift
struct Color {
    let red, green, blue: Double
    init(red: Double, green: Double, blue: Double) {
        self.red   = red
        self.green = green
        self.blue  = blue
    }
    init(white: Double) {
        red   = white
        green = white
        blue  = white
    }
}
```

두 초기화 구문 모두 각 초기화 구문 파라미터에 값을 제공하여 새로운 `Color` 인스턴스를 생성할 수 있습니다:

```swift
let magenta = Color(red: 1.0, green: 0.0, blue: 1.0)
let halfGray = Color(white: 0.5)
```

인수 라벨을 사용하지 않고 초기화 구문을 호출할 수 없습니다. 인수 라벨은 정의 되었다면 항상 사용되어야 하고 생략 시 컴파일 시 에러가 발생합니다:

```swift
let veryGreen = Color(0.0, 1.0, 0.0)
// this reports a compile-time error - argument labels are required
```

### 인수 라벨 없는 초기화 구문 파라미터 \(Initializer Parameters Without Argument Labels\)

초기화 구문 파라미터에 인수 라벨 사용을 원치 않을 경우 명시적으로 인수 라벨 대신에 언더바 \(`_`\)를 작성하여 기본 동작을 재정의 합니다.

다음은 섭씨 온도에 대한 `Double` 값으로 새로운 `Celsius` 인스턴스를 생성하기 위해 초기화 구문을 추가한 [초기화 파라미터 \(Initialization Parameters\)](initialization.md#initialization-parameters) 에서의 `Celsius` 예제의 확장된 버전입니다:

```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
    init(_ celsius: Double) {
        temperatureInCelsius = celsius
    }
}
let bodyTemperature = Celsius(37.0)
// bodyTemperature.temperatureInCelsius is 37.0
```

초기화 구문 호출 `Celsius(37.0)` 은 인수 라벨이 필요치 않다는 의도가 명확합니다. 따라서 초기화 구문을 `init(_ celsius: Double)` 로 작성하여 이름없는 `Double` 값을 제공하여 호출할 수 있습니다.

### 옵셔널 프로퍼티 타입 \(Optional Property Types\)

사용자 타입이 초기화 동안 값을 설정할 수 없거나 추후에 "값 없음"을 가질 수 있기 때문에 논리적으로 "값 없음"을 가질 수 있는 하나의 저장된 프로퍼티를 가지고 있다면 _옵셔널_ 타입으로 프로퍼티를 선언합니다. 옵셔널 타입 \(optional type\)의 프로퍼티는 자동적으로 초기화 동안 "아직 값 없음"을 가진다는 의도를 위해 `nil` 의 값으로 초기화 됩니다.

다음의 예제는 `response` 라는 옵셔널 `String` 프로퍼티를 가지는 `SurveyQuestion` 이라는 클래스를 정의합니다:

```swift
class SurveyQuestion {
    var text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}
let cheeseQuestion = SurveyQuestion(text: "Do you like cheese?")
cheeseQuestion.ask()
// Prints "Do you like cheese?"
cheeseQuestion.response = "Yes, I do like cheese."
```

설문 질문 응답은 질문되기 전까지 알 수 없으므로 `response` 프로퍼티는 `String?` 타입 또는 "옵셔널 `String`"타입으로 선언됩니다. 새로운 인스턴스 `SurveyQuestion` 이 초기화 될 때 "아직 문자열 없음" 이라는 의미의 기본값 `nil` 이 자동으로 할당됩니다.

### 초기화 동안 프로퍼티 상수 할당 \(Assigning Constant Properties During Initialization\)

초기화가 완료될 때까지 한정된 값으로 설정되는 한 초기화 중 언제든지 프로퍼티 상수에 값을 할당할 수 있습니다. 프로퍼티 상수에 값이 할당되면 더이상 수정할 수 없습니다.

> Note   
> 클래스 인스턴스의 경우 초기화 하는 동안 프로퍼티 상수를 수정하는 것은 해당 프로퍼티를 도입한 클래스에 의해서만 가능합니다. 하위 클래스에서 수정할 수 없습니다.

`SurveyQuestion` 의 인스턴스가 생성되면 질문이 바뀌지 않는다는 것을 나타내기 위해 질문의 `text` 프로퍼티는 프로퍼티 변수보다 프로퍼티 상수로 사용하기 위해 위의 `SurveyQuestion` 예제를 수정할 수 있습니다. `text` 프로퍼티는 현재 상수이지만 여전히 클래스의 초기화 구문 내에서 설정할 수 있습니다:

```swift
class SurveyQuestion {
    let text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}
let beetsQuestion = SurveyQuestion(text: "How about beets?")
beetsQuestion.ask()
// Prints "How about beets?"
beetsQuestion.response = "I also like beets. (But not with cheese.)"
```

## 기본 초기화 구문 \(Default Initializers\)

Swift는 모든 프로퍼티에 대한 기본값을 제공하고 적어도 하나의 초기화 구문을 제공하지 않는 모든 구조체 또는 클래스에 대해 _기본 초기화 구문 \(default initializer\)_ 를 제공합니다. 기본 초기화 구문은 모든 프로퍼티가 기본값으로 설정된 새로운 인스턴스를 생성합니다.

이 예제는 쇼핑 리스트에 있는 항목의 이름, 수량, 그리고 구매 상태를 캡슐화하는 `ShoppingListItem` 이라는 클래스를 정의합니다:

```swift
class ShoppingListItem {
    var name: String?
    var quantity = 1
    var purchased = false
}
var item = ShoppingListItem()
```

`ShoppingListItem` 클래스의 모든 프로퍼티는 기본값을 가지고 있고 상위 클래스가 없이 기본 클래스 이므로 `ShoppingListItem` 은 자동적으로 모든 프로퍼티를 기본값 \(`name` 프로퍼티는 코드에서 값을 작성하지 않았지만 옵셔널 `String` 프로퍼티 이므로 자동적으로 `nil` 의 기본값을 받습니다\)으로 설정한 새로운 인스턴스를 생성하는 기본 초기화 구문 구현을 갖습니다. 위의 예제는 `ShoppingListItem()` 으로 작성한 초기화 구문으로 클래스의 새로운 인스턴스를 생성하기 위해 `ShoppingListItem` 클래스에 대한 기본 초기화 구문을 사용하고 `item` 이라는 변수에 새로운 인스턴스를 할당합니다.

### 구조체 타입에 대한 멤버별 초기화 구문 \(Memberwise Initializers for Structure Types\)

구조체 타입은 자신의 사용자화 초기화 구문을 정의하지 않으면 자동적으로 _멤버별 초기화 구문 \(memberwise initializer\)_ 을 받습니다. 기본 초기화 구문과 다르게 구조체는 기본값을 가지지 않은 저장된 프로퍼티 라도 멤버별 초기화 구문을 받습니다.

멤버별 초기화 구문은 새로운 구조체 인스턴스에 멤버 프로퍼티를 초기화 하기 위한 짧은 구문 방법입니다. 새로운 인스턴스의 프로퍼티를 위한 초기화 값은 이름으로 멤버별 초기화 구문으로 전달될 수 있습니다.

아래의 예제는 `width` 와 `height` 라는 2개의 프로퍼티를 가지는 `Size` 라는 구조체를 정의합니다. 두 프로퍼티 모두 `0.0` 의 기본값이 할당되어 `Double` 타입으로 추론됩니다.

`Size` 구조체는 자동으로 새로운 `Size` 인스턴스를 초기화 하기 위해 사용할 수 있는 `init(width:height:)` 멤버별 초기화 구문을 받습니다:

```swift
struct Size {
    var width = 0.0, height = 0.0
}
let twoByTwo = Size(width: 2.0, height: 2.0)
```

멤버별 초기화 구문을 호출할 때 기본값을 가지는 모든 프로퍼티의 값은 생략할 수 있습니다. 위의 예제에서 `Size` 구조체는 `height` 와 `width` 프로퍼티 둘다 기본값을 가지고 있습니다. 하나 또는 프로퍼티 둘다 생략할 수 있고 초기화 구문은 생략한 값을 기본값으로 사용합니다. 예를 들어:

```swift
let zeroByTwo = Size(height: 2.0)
print(zeroByTwo.width, zeroByTwo.height)
// Prints "0.0 2.0"

let zeroByZero = Size()
print(zeroByZero.width, zeroByZero.height)
// Prints "0.0 0.0"
```

## 값 타입을 위한 초기화 구문 위임 \(Initializer Delegation for Value Types\)

초기화 구문은 인스턴스의 초기화의 부분을 수행하기 위해 다른 초기화 구문을 호출할 수 있습니다. _초기화 구문 위임 \(initializer delegation\)_ 이라는 이 프로세스는 여러 초기화 구문에서 코드가 중복되는 것을 방지합니다.

초기화 구문 위임이 동작하는 방식과 허용하는 위임 형식에 대한 규칙은 값 타입과 클래스 타입에 따라 다릅니다. 값 타입 \(구조체와 열거형\)은 상속을 지원하지 않고 초기화 구문 위임 프로세스는 자신이 제공하는 다른 초기화 구문에만 위임할 수 있으므로 비교적 간단합니다. 그러나 클래스는 [상속 \(Inheritance\)](inheritance.md) 에서 설명했듯이 다른 클래스에서 상속할 수 있습니다. 이는 클래스는 상속하는 모든 저장된 프로퍼티에 초기화 중에 적절한 값이 할당되도록 하는 추가 책임이 있음을 의미합니다. 이러한 책임은 아래 [클래스 상속과 초기화 \(Class Inheritance and Initialization\)](initialization.md#class-inheritance-and-initialization) 에 설명되어 있습니다.

값 타입의 경우 자체 사용자 화 초기화 구문을 작성할 때 같은 값 타입으로 부터 다른 초기화 구문을 참조하기 위해 `self.init` 을 사용합니다. 초기화 구문 내에서만 `self.init` 을 호출할 수 있습니다.

값 타입에 대해 사용자 화 초기화 구문을 정의한다면 그 타입에 대해 기본 초기화 구문 또는 구조체의 경우 멤버별 초기화 구문에 더이상 접근할 수 없습니다. 이 제약은 자동 초기화 구문을 사용하는 누군가가 더 복잡한 초기화 구문에서 제공하는 추가 필수 설정을 실수로 우회하는 것을 방지합니다.

> Note   
> 기본 초기화 구문과 멤버별 초기화 구문, 그리고 자체 사용자 화 초기화 구문을 사용하여 사용자 화 값 타입을 초기화 하려면 값 타입의 기존 구현의 부분이 아닌 확장에 사용자 화 초기화 구문을 작성해야 합니다. 자세한 내용은 [확장 \(Extensions\)](extensions.md) 을 참고 바랍니다.

다음의 예제는 기하학적 사각형을 표현하기 위해 사용자 화 `Rect` 구조체를 정의합니다. 이 예제는 `Size` 와 `Point` 라는 2개의 지원하는 구조체가 필요하고 둘다 모든 프로퍼티에 대해 `0,0` 의 기본값을 제공합니다:

```swift
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
```

아래의 `Rect` 구조체를 기본 0으로 초기화된 `origin` 과 `size` 를 사용하거나 특정 원점과 크기를 제공하거나 특정 중앙점과 크기를 제공하는 세가지 방법 중 하나로 초기화할 수 있습니다. 이러한 초기화 옵션은 `Rect` 구조체의 정의의 부분인 3개의 사용자화 초기화 구문으로 표시됩니다:

```swift
struct Rect {
    var origin = Point()
    var size = Size()
    init() {}
    init(origin: Point, size: Size) {
        self.origin = origin
        self.size = size
    }
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```

첫번째 `Rect` 초기화 구문인 `init()` 은 자체 사용자 화 초기화 구문이 없으면 구조체는 수신했을 때 기본 초기화 구문과 기능적으로 동일합니다. 이 초기화 구문은 빈 중괄호 `{}` 로 표시된 빈 본문을 가집니다. 이 초기화 구문을 호출하면 `origin` 과 `size` 프로퍼티는 프로퍼티 정의에서 `Point(x: 0.0, y: 0.0)` 과 `Size(width: 0.0, height: 0.0)` 의 기본값으로 초기화 되는 `Rect` 인스턴스를 반환합니다:

```swift
let basicRect = Rect()
// basicRect's origin is (0.0, 0.0) and its size is (0.0, 0.0)
```

두번째 `Rect` 초기화 구문인 `init(origin:size:)` 는 자체 사용자 화 초기화 구문이 없으면 구조체는 수신했을 때 멤버별 초기화 구문과 기능적으로 동일합니다. 이 초기화 구문은 단순하게 `origin` 과 `size` 인수값을 적절한 저장된 프로퍼티에 할당합니다:

```swift
let originRect = Rect(origin: Point(x: 2.0, y: 2.0),
                      size: Size(width: 5.0, height: 5.0))
// originRect's origin is (2.0, 2.0) and its size is (5.0, 5.0)
```

세번째 `Rect` 초기화 구문인 `init(center:size:)` 는 좀 더 복잡합니다. `center` 점과 `size` 값을 기반으로 적절한 원점을 계산하는 것으로 시작합니다. 그런 다음 적절한 프로퍼티로 새로운 원점과 크기로 저장하는 `init(origin:size:)` 초기화 구문을 호출 또는 위임합니다:

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                      size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
```

`init(center:size:)` 초기화 구문은 적절한 프로퍼티 자체에 `origin` 과 `size` 의 새로운 값을 할당할 수 있습니다. 그러나 `init(center:size:)` 초기화 구문이 해당 기능을 이미 제공하는 기존 초기화 구문을 활용하는 것이 더 편리하고 명확합니다.

> Note   
> `init()` 과 `init(origin:size:)` 초기화 구문을 직접 정의하지 않고 이 예제를 작성하는 다른 방법은 [확장 \(Extensions\)](extensions.md) 을 참고 바랍니다.

## 클래스 상속과 초기화 \(Class Inheritance and Initialization\)

상위 클래스로 부터 상속한 클래스의 모든 프로퍼티를 포함하는 모든 클래스의 저장된 프로퍼티는 초기화 중에 _반드시_ 초기값이 할당되어야 합니다.

Swift는 모든 저장된 프로퍼티가 초기값을 받을 수 있도록 클래스 타입에 대해 2가지의 초기화 구문을 정의합니다. 이들은 지정된 초기화 구문 (designated initializer) 과 편의 초기화 구문 (convenience initializer) 으로 알려져 있습니다.

### 지정된 초기화 구문과 편의 초기화 구문 \(Designated Initializers and Convenience Initializers\)

_지정된 초기화 구문 \(Designated initializers\)_ 은 클래스에 주 초기화 구문입니다. 지정된 초기화 구문은 해당 클래스에 의해 도입된 모든 프로퍼티를 완벽하게 초기화 하고 적절한 상위 클래스 초기화를 호출하여 상위 클래스 체인까지 초기화 프로세스를 계속합니다.

클래스는 지정된 초기화 구문이 거의 없는 경향이 있으며 클래스에 하나만 있는 경우가 일반적입니다. 지정된 초기화 구문은 초기화가 수행되고 초기화 프로세스가 상위 클래스 체인까지 계속되는 "funnel" 지점입니다.

모든 클래스는 적어도 하나의 지정된 초기화 구문을 가지고 있어야 합니다. 경우에 따라 이 요구사항은 아래의 [자동 초기화 구문 상속 \(Automatic Initializer Inheritance\)](initialization.md#automatic-initializer-inheritance) 에서 설명 하듯이 상위 클래스에서 하나 이상의 지정된 초기화 구문을 상속하는 것으로 충족됩니다.

_편의 초기화 구문 \(Convenience initializers\)_ 은 클래스에 대해 초기화 구문을 지원하는 보조 초기화 구문입니다. 지정된 초기화 구문의 파라미터를 기본값으로 설정하여 편의 초기화 구문과 동일한 클래스에서 지정된 초기화 구문을 호출하도록 편의 초기화 구문을 정의할 수 있습니다. 특정 사용 케이스 또는 입력 값 타입에 대한 해당 클래스의 인스턴스를 생성하기 위해 편의 초기화 구문을 정의할 수도 있습니다.

클래스에 필요하지 않은 경우 편의 초기화 구문을 제공할 필요가 없습니다. 일반적인 초기화 패턴에 대한 바로가기가 시간을 절약하거나 클래스 초기화를 더 명확하게 만들 때마다 편리한 초기화 구문을 만듭니다.

### 지정된 초기화 구문과 편의 초기화 구문 \(Syntax for Designated and Convenience Initializers\)

클래스에 대한 지정된 초기화 구문은 값 타입에 대한 간단한 초기화 구문과 동일한 방법으로 작성됩니다:

```swift
init(<#parameters#>) {
   <#statements#>
}
```

편의 초기화 구문은 같은 스타일로 작성되지만 공백으로 구분하여 `init` 키워드 전에 `convenience` 수정자를 작성합니다:

```swift
convenience init(<#parameters#>) {
   <#statements#>
}
```

### 클래스 타입에 대한 초기화 구문 위임 \(Initializer Delegation for Class Types\)

지정된 초기화 구문과 편의 초기화 구문 사이의 관계를 단순화 하기위해 Swift는 초기화 사이의 위임 호출에 대한 3가지 규칙을 적용합니다:

* **규칙 1**

  지정된 초기화 구문은 상위 클래스로 부터 지정된 초기화 구문을 호출해야만 합니다.

* **규칙 2**

  편의 초기화 구문은 _같은_ 클래스로 부터 다른 초기화 구문을 호출해야만 합니다.

* **규칙 3**

  편의 초기화 구문은 궁극적으로 지정된 초기화 구문을 호출해야만 합니다.

이것을 기억하는 간단한 방법은 아래와 같습니다:

* 지정된 초기화 구문은 항상 위로 위임해야 합니다.
* 편의 초기화 구문은 항상 옆으로 위임해야 합니다.

이러한 규칙은 아래의 그림에 설명되어 있습니다:

![Initializer Delegation](../.gitbook/assets/initializerDelegation01_2x~dark.png)

여기에서 상위 클래스는 하나의 지정된 초기화 구문과 2개의 편의 초기화 구문을 가지고 있습니다. 하나의 편의 초기화 구문은 다른 편의 초기화 구문을 호출하고 차례로 지정된 초기화 구문을 호출합니다. 이는 위의 규칙 2와 3을 충족합니다. 상위 클래스는 더이상의 상위 클래스를 가지고 있지 않으므로 규칙 1은 적용되지 않습니다.

그림에 하위 클래스는 2개의 지정된 초기화 구문과 하나의 편의 초기화 구문을 가지고 있습니다. 편의 초기화 구문은 같은 클래스의 다른 초기화 구문만 호출할 수 있으므로 2개의 지정된 초기화 구문 중 하나를 호출해야만 합니다. 이것은 위의 규칙 2와 3을 충족합니다. 지정된 초기화 구문 모두 위의 규칙 1을 충족하기 위해 상위 클래스의 단일 지정된 초기화 구문을 호출해야만 합니다.

> Note   
> 이러한 규칙은 클래스의 사용자가 각 클래스의 인스턴스를 생성하는 방법에 영향을 주지 않습니다. 위 다이어그램에 모든 초기화 구문을 사용하여 자신이 속한 클래스의 완전히 초기화 된 인스턴스를 만들 수 있습니다. 규칙은 클래스의 초기화 구문의 구현을 어떻게 작성해야 하는지만 영향을 줍니다.

아래 그림은 4개의 클래스에 대한 더 복잡한 클래스 계층도를 보여줍니다. 이 계층도에서 지정된 초기화 구문이 클래스 초기화에 대해 "funnel" 지점으로 어떻게 동작하는지 클래스 간의 상호 관계를 단순화 하여 보여줍니다:

![Initializer Delegation2](../.gitbook/assets/initializerDelegation02_2x~dark.png)

### 2단계 초기화 \(Two-Phase Initialization\)

Swift에서 클래스 초기화는 2단계 프로세스 입니다. 첫번째 단계에서는 각 저장된 프로퍼티가 해당 프로퍼티를 도입한 클래스에 의해 초기값이 할당됩니다. 모든 저장된 프로퍼티에 초기 상태가 결정되면 두번째 단계가 시작되고 새 인스턴스가 사용할 준비가 된 것으로 간주하기 전에 각 클래스에 저장된 프로퍼티를 추가로 사용자화 할 수 있는 기회가 주어집니다.

2단계 초기화 프로세스를 사용하면 초기화를 안전하게 수행하는 동시에 클래스 계층도의 각 클래스에 완전한 유연성을 제공합니다. 2단계 초기화는 프로퍼티 값이 초기화 되기전에 접근하는 것을 막고 다른 초기화 구문이 예기치 않게 다른 값을 설정하는 것을 막습니다.

> Note   
> Swift의 2단계 초기화 프로세스는 Objective-C의 초기화와 유사합니다. 주요 차이점은 첫번재 단계 동안 Objective-C는 모든 프로퍼티에 0 또는 null 값 \(`0` 또는 `nil`\)을 할당합니다. Swift의 초기화 흐름은 사용자 정의 초기값을 설정할 수 있고 `0` 또는 `nil` 이 유효한 기본값이 아닌 타입에 대처할 수 있다는 점에서 더 유연합니다.

Swift의 컴파일러는 에러없이 2단계 초기화가 완료되었는지 확인하기 위해 4가지의 검사를 수행합니다:

* **안전 점검 1**

  지정된 초기화 구문은 상위 클래스 초기화 구문에 위임되기 전에 클래스에 의해 도입된 모든 프로퍼티가 초기화 되었는지 확인합니다.

위에서 언급했듯이 객체의 메모리는 모든 저장된 프로퍼티의 초기상태가 알려지면 완전히 초기화 된것으로 간주합니다. 이 규칙이 충족하려면 지정된 초기화 구문은 체인을 넘기기 전에 모든 프로퍼티가 초기화 되었는지 확인해야 합니다.

* **안전 점검 2**

  지정된 초기화 구문은 상속된 프로퍼티에 값을 할당하기 전에 상위 클래스 초기화 구문에 위임해야 합니다. 그렇지 않으면 지정된 초기화 구문에 할당한 새로운 값은 자체 초기화 부분으로 상위 클래스에 의해 덮어 쓰여집니다.

* **안전 점검 3**

  편의 초기화 구문은 _모든_ 프로퍼티 \(같은 클래스에 정의한 프로퍼티 포함\)에 값을 할당하기 전에 다른 초기화 구문에 위임해야 합니다. 그렇지 않으면 편의 초기화 구문에 할당한 새로운 값은 자체 클래스의 지정된 초기화 구문에 의해 덮어 쓰여집니다.

* **안전 점검 4**

  초기화 구문은 첫번째 초기화가 완료될 때까지 인스턴스 메서드를 호출하거나 인스턴스 프로퍼티의 값을 읽거나 `self` 를 값으로 참조할 수 없습니다.

클래스 인스턴스는 첫번째 단계가 끝날 때까지 완전히 유효하지 않습니다. 첫번째 단계가 끝날 때 클래스 인스턴스가 유효한 것으로 판단된 후에만 프로퍼티는 접근할 수 있고 메서드를 호출할 수 있습니다.

위의 4개의 안전 점검을 기반으로 2단계 초기화가 수행되는 방식은 아래와 같습니다:

**1 단계**

* 지정된 또는 편의 초기화 구문은 클래스에서 호출됩니다.
* 클래스에 새로운 인스턴스에 대한 메모리는 할당됩니다. 메모리는 아직 초기화 되지 않았습니다.
* 클래스에 대한 지정된 초기화 구문은 클래스에 의해 도입된 모든 저장된 프로퍼티가 값을 가지고 있는지 확인합니다. 이러한 저장된 프로퍼티에 대한 메모리는 초기화 됩니다.
* 지정된 초기화 구문은 자체 저장된 프로퍼티에 동일한 작업을 수행하기 위해 상위 클래스 초기화 구문에 전달됩니다.
* 이것은 최상위 체인까지 클래스 상속 체인 위로 계속됩니다.
* 최상위 체인에 도달하고 체인에 마지막 클래스가 모든 저장된 프로퍼티가 값을 가지고 있다고 확인하면 인스턴스의 메모리는 완벽하게 초기화 되었다고 간주하고 첫단계가 완료됩니다.

**2 단계**

* 체인의 최상위에서 아래로 내려가면 체인에 각 지정된 초기화 구문은 인스턴스를 추가로 사용자 정의할 수 있는 옵션이 있습니다. 초기화 구문은 이제 `self` 로 접근할 수 있으며 프로퍼티를 수정할 수 있고 인스턴스 메서드를 호출하는 등에 작업을 수행할 수 있습니다.
* 마지막으로 체인에 모든 편의 초기화 구문은 인스턴스를 사용자 정의하고 `self` 로 작업할 수 있는 옵션이 있습니다.

다음은 첫번째 단계에서 가상 하위 클래스와 상위 클래스에 대한 초기화 호출을 찾는 방법을 나타냅니다:

![Two-Phase Initialization 1](../.gitbook/assets/twoPhaseInitialization01_2x~dark.png)

이 예제에서 초기화는 하위 클래스의 편의 초기화 구문을 호출하며 시작합니다. 이 편의 초기화 구문은 아직 모든 프로퍼티를 수정할 수 없습니다. 이것은 같은 클래스의 지정된 초기화 구문으로 위임합니다.

지정된 초기화 구문은 안전 점검 1에 따라 하위 클래스의 모든 프로퍼티가 값을 가지고 있는지 확인합니다. 그런 다음 상위 클래스에서 지정된 초기화 구문을 호출하여 체인까지 초기화를 계속합니다.

상위 클래스의 지정된 초기화 구문은 상위 클래스의 모든 프로퍼티가 값을 가지고 있는지 확인합니다. 초기화 할 상위 클래스가 없으므로 추가 위임이 필요치 않습니다.

상위 클래스의 모든 프로퍼티가 초기값을 가지자마자 메모리는 완전히 초기화 되었다고 간주하고 첫번째 단계가 완료됩니다.

다음은 2 단계에서 같은 초기화 호출을 찾는 방법을 나타냅니다:

![Two-Phase Initialization 2](../.gitbook/assets/twoPhaseInitialization02_2x~dark.png)

상위 클래스의 지정된 초기화 구문은 이제 인스턴스를 추가로 사용자화 할 수 있는 기회 \(필수는 아님\)를 가집니다.

상위 클래스의 지정된 초기화 구문이 완료되면 하위 클래스의 지정된 초기화 구문은 추가 사용자 정의 \(필수는 아님\)를 수행할 수 있습니다.

마지막으로 하위 클래스의 지정된 초기화 구문이 완료되면 원래 호출되었던 편의 초기화 구문이 추가 사용자 정의를 수행할 수 있습니다.

### 초기화 구문 상속과 재정의 \(Initializer Inheritance and Overriding\)

Objective-C의 하위 클래스와 다르게 Swift 하위 클래스는 기본적으로 상위 클래스 초기화 구문을 상속하지 않습니다. Swift의 접근방식은 상위 클래스의 간단한 초기화 구문이 더 특별한 일을 위한 하위 클래스에 상속되고 완전히 또는 올바르게 초기화 되지않은 하위 클래스의 새로운 인스턴스를 생성하기 위해 사용되는 상황을 방지합니다.

> Note   
> 상위 클래스 초기화 구문은 특정 상황에서 상속되지만 안전하고 적절한 경우에만 상속됩니다. 더 자세한 내용은 아래의 [자동 초기화 구문 상속 \(Automatic Initializer Inheritance\)](initialization.md#automatic-initializer-inheritance) 을 참고 바랍니다.

사용자 정의 하위 클래스가 상위 클래스와 동일한 초기화 구문 중 하나 이상을 표시하려면 하위 클래스 내에서 해당 초기화 구문의 사용자 정의 구현을 제공할 수 있습니다.

상위 클래스에 _지정된_ 초기화 구문과 일치하는 하위 클래스 초기화 구문을 작성할 때 지정된 초기화 구문의 재정의를 효과적으로 제공합니다. 따라서 하위 클래스의 초기화 구문 정의 전에 `override` 수식어를 작성해야만 합니다. [기본 초기화 구문 \(Default Initializers\)](initialization.md#default-initializers) 에서 설명 했듯이 자동으로 제공된 기본 초기화 구문을 재정의하는 경우에도 마찬가지입니다.

재정의 된 프로퍼티, 메서드 또는 서브 스크립트와 마찬가지로 `override` 수식어가 있으면 Swift가 상위 클래스에 재정의 할 일치하는 지정된 초기화 구문이 있는지 확인하고 재정의 할 초기화 구문의 파라미터가 의도한대로 지정되었는지 확인하도록 합니다.

> Note   
> 초기화 구문의 하위 클래스의 구현이 편의 초기화 구문이라도 상위 클래스에 지정된 초기화 구문을 재정의할 때 항상 `override` 수식어를 작성합니다.

반대로 상위 클래스에 _편의_ 초기화 구문과 일치하는 하위 클래스 초기화 구문을 작성하는 경우 [클래스 타입에 대한 초기화 구문 위임 \(Initializer Delegation for Class Types\)](initialization.md#initializer-delegation-for-class-types) 에서 설명한 규칙에 따라 해당 상위 클래스 편의 초기화 구문은 하위 클래스에서 직접적으로 호출될 수 없습니다. 따라서 하위 클래스는 상위 클래스 초기화 구문에 재정의를 제공하지 않습니다. 결과적으로 상위 클래스 편의 초기화 구문의 일치하는 구현을 제공할 때 `override` 수식어를 작성하지 않습니다.

아래의 예제는 `Vehicle` 이라는 기본 클래스를 정의합니다. 이 기본 클래스는 기본 `Int` 값 `0` 을 가진 `numberOfWheels` 라는 저장된 프로퍼티를 선언합니다. `numberOfWheels` 프로퍼티는 탈 것의 특징을 설명하는 `String` 을 생성하기 위해 `description` 이라는 계산된 프로퍼티에 의해 사용됩니다:

```swift
class Vehicle {
    var numberOfWheels = 0
    var description: String {
        return "\(numberOfWheels) wheel(s)"
    }
}
```

`Vehicle` 클래스는 저장된 프로퍼티에 대해서만 기본값을 제공하고 모든 사용자 정의 초기화 구문 자체는 제공하지 않습니다. 그 결과 [기본 초기화 구문 \(Default Initializers\)](initialization.md#default-initializers) 에서 설명 했듯이 기본 초기화 구문을 자동으로 받습니다. 사용가능한 경우 기본 초기화 구문은 항상 클래스에 대해 지정된 초기화 구문이고 `0` 인 `numberOfWheels` 를 가진 새로운 `Vehicle` 인스턴스를 생성하기 위해 사용됩니다:

```swift
let vehicle = Vehicle()
print("Vehicle: \(vehicle.description)")
// Vehicle: 0 wheel(s)
```

다음 예제는 `Bicycle` 이라는 `Vehicle` 의 하위 클래스를 정의합니다:

```swift
class Bicycle: Vehicle {
    override init() {
        super.init()
        numberOfWheels = 2
    }
}
```

`Bicycle` 하위 클래스는 `init()` 인 지정된 초기화 구문을 정의합니다. 이 지정된 초기화 구문은 `Bicycle` 의 상위 클래스에 지정된 초기화 구문과 일치하기 때문에 초기화 구문의 `Bicycle` 버전은 `override` 수식어가 표기됩니다.

`Bicycle` 에 대한 `init()` 초기화 구문은 `Bicycle` 클래스의 상위 클래스인 `Vehicle` 에 대한 기본 초기화 구문을 호출하는 `super.init()` 을 호출하는 것으로 시작합니다. 상속된 프로퍼티 `numberOfWheels` 는 `Bicycle` 이 프로퍼티를 수정할 기회를 가지기 전에 `Vehicle` 에 의해 초기화 됩니다. `super.init()` 호출 후에 `numberOfWheels` 의 기존 값은 새로운 값 `2` 로 대체됩니다.

`Bicycle` 의 인스턴스를 생성하면 `numberOfWheels` 프로퍼티가 어떻게 업데이트 되었는지 확인하기 위해 상속된 `description` 계산된 프로퍼티를 호출할 수 있습니다:

```swift
let bicycle = Bicycle()
print("Bicycle: \(bicycle.description)")
// Bicycle: 2 wheel(s)
```

하위 클래스 초기화 구문이 초기화 프로세스의 2 단계에서 사용자 정의 없이 수행되고 상위 클래스가 동기적이며 인수가 없는 지정된 초기화 구문을 가지고 있다면 모든 하위 클래스의 저장된 프로퍼티에 값을 할당한 후에 `super.init()` 호출을 생략할 수 있습니다. 상위 클래스의 초기화 구문이 비동기적이라면 명시적으로 `await super.init()` 을 작성해야 합니다.

아래 예제는 `Hoverboard` 라는 `Vehicle` 의 다른 하위 클래스를 정의합니다. 초기화 구문에서 `Hoverboard` 클래스는 `color` 프로퍼티만 설정합니다. 이 초기화 구문은 `super.init()` 을 명시적으로 호출하는 대신에 상위 클래스의 초기화 구문을 암시적으로 호출함으로써 프로세스를 완료합니다.

```swift
class Hoverboard: Vehicle {
    var color: String
    init(color: String) {
        self.color = color
        // super.init() implicitly called here
    }
    override var description: String {
        return "\(super.description) in a beautiful \(color)"
    }
}
```

`Hoverboard` 의 인스턴스는 `Vehicle` 초기화 구문에 의해 제공된 바퀴의 기본 갯수를 사용합니다.

```swift
let hoverboard = Hoverboard(color: "silver")
print("Hoverboard: \(hoverboard.description)")
// Hoverboard: 0 wheel(s) in a beautiful silver
```

> Note   
> 하위 클래스는 초기화 중에 상속된 프로퍼티 변수를 수정할 수 있지만 상속된 프로퍼티 상수는 수정할 수 없습니다.

### 자동 초기화 구문 상속 \(Automatic Initializer Inheritance\)

위에서 언급했듯이 하위 클래스는 기본적으로 상위 클래스 초기화 구문을 상속하지 않습니다. 그러나 특정 조건이 충족하면 상위 클래스 초기화 구문은 자동으로 상속됩니다. 실제로 이것은 대부분의 경우에 초기화 구문 재정의를 작성할 필요가 없으며 안전하면 상위 클래스 초기화 구문을 최소한의 노력으로 상속할 수 있습니다.

하위 클래스에 도입한 모든 새로운 프로퍼티에 기본값을 제공하면 아래의 2가지 규칙이 적용됩니다:

**규칙 1**

​ 하위 클래스가 지정된 초기화 구문을 정의하지 않았으면 자동으로 상위 클래스에 지정된 초기화 구문을 모두 상속합니다.

**규칙 2**

​ 하위 클래스가 규칙 1에 따라 상속하거나 정의의 부분으로 사용자 정의 구현을 제공하여 모든 상위 클래스 지정된 초기화 구문의 구현을 제공하면 모든 상위 클래스 편의 초기화 구문을 자동으로 상속합니다.

이러한 규칙은 하위 클래스가 편의 초기화 구문을 추가할 때도 적용됩니다.

> Note   
> 하위 클래스는 규칙 2를 만족하는 부분으로 하위 클래스 편의 초기화 구문으로 상위 클래스 지정된 초기화 구문을 구현할 수 있습니다.

### 지정된 초기화 구문과 편의 초기화 구문 동작 \(Designated and Convenience Initializers in Action\)

다음의 예제는 지정된 초기화 구문, 편의 초기화 구문, 그리고 자동 상속 초기화 구문의 동작을 보여줍니다. 이 예제는 `Food`, `RecipeIngredient`, 그리고 `ShoppingListItem` 이라는 3개의 클래스의 계층을 정의하고 초기화 구문이 상호 작용하는 방식을 보여줍니다.

계층도에 기본 클래스는 식품의 이름을 캡슐화 하는 간단한 클래스인 `Food` 입니다. `Food` 클래스는 `name` 이라는 `String` 프로퍼티를 도입하고 `Food` 인스턴스를 생성하기 위한 2개의 초기화 구문을 제공합니다:

```swift
class Food {
    var name: String
    init(name: String) {
        self.name = name
    }
    convenience init() {
        self.init(name: "[Unnamed]")
    }
}
```

아래의 그림은 `Food` 클래스를 위한 초기화 구문 체인을 나타냅니다:

![Initializer Example 1](../.gitbook/assets/initializersExample01_2x~dark.png)

클래스는 기본 멤버별 초기화 구문을 가지고 있지 않으므로 `Food` 클래스는 `name` 이라는 하나의 인수를 가지는 지정된 초기화 구문을 제공합니다. 이 초기화 구문은 특정 이름으로 새로운 `Food` 인스턴스를 생성하기 위해 사용될 수 있습니다:

```swift
let namedMeat = Food(name: "Bacon")
// namedMeat's name is "Bacon"
```

`Food` 클래스에 `init(name: String)` 초기화 구문은 새로운 `Food` 인스턴스에 모든 저장된 프로퍼티는 완벽하게 초기화 되므로 _지정된_ 초기화 구문으로 제공됩니다. `Food` 클래스는 상위 클래스를 가지고 있지 않으므로 `init(name: String)` 초기화 구문은 초기화를 완료하기 위해 `super.init()` 을 호출할 필요가 없습니다.

`Food` 클래스는 인수가 없는 `init()` 의 _편의_ 초기화 구문도 제공합니다. `init()` 초기화 구문은 `[Unnamed]` 의 `name` 값으로 `Food` 클래스의 `init(name: String)` 으로 위임하여 새로운 음식을 위한 기본 이름을 제공합니다:

```swift
let mysteryMeat = Food()
// mysteryMeat's name is "[Unnamed]"
```

계층도에 두번째 클래스는 `RecipeIngredient` 라는 `Food` 의 하위 클래스 입니다. `RecipeIngredient` 클래스는 요리 레시피에 재료를 모델링합니다. `quantity` \(추가로 `Food` 로 부터 `name` 프로퍼티도 상속함\)라는 `Int` 프로퍼티를 도입하고 `RecipeIngredient` 인스턴스를 생성하는 2개의 초기화 구문을 정의합니다:

```swift
class RecipeIngredient: Food {
    var quantity: Int
    init(name: String, quantity: Int) {
        self.quantity = quantity
        super.init(name: name)
    }
    override convenience init(name: String) {
        self.init(name: name, quantity: 1)
    }
}
```

아래의 그림은 `RecipeIngredient` 클래스에 대한 초기화 구문 체인을 보여줍니다:

![Initializers Example 2](../.gitbook/assets/initializersExample02_2x~dark.png)

`RecipeIngredient` 클래스는 새로운 `RecipeIngredient` 인스턴스에 모든 프로퍼티를 채울 수 있는 `init(name: String, quantity: Int)` 인 하나의 지정된 초기화 구문을 가지고 있습니다. 이 초기화 구문은 `RecipeIngredient` 에 도입된 새로운 프로퍼티 인 `quantity` 프로퍼티에 전달된 `quantity` 인수를 할당하는 것으로 시작합니다. 그런 후에 `Food` 클래스에 `init(name: String)` 초기화 구문으로 위임합니다. 이 프로세스는 위에서 설명한 [2단계 초기화 \(Two-Phase Initialization\)](initialization.md#2-two-phase-initialization) 에 안전 점검 1에 충족합니다.

`RecipeIngredient` 는 이름으로만 `RecipeIngredient` 인스턴스를 생성하기 위해 사용되는 `init(name: String)` 인 편의 초기화 구문도 정의합니다. 이 편의 초기화 구문은 명시적인 양 없이 생성되는 모든 `RecipeIngredient` 인스턴스에 대해 `1` 의 양으로 가정합니다. 이러한 편의 초기화 구문의 정의는 `RecipeIngredient` 인스턴스를 더 빠르고 편리하게 생성하도록 하고 여러개의 단일 양의 `RecipeIngredient` 인스턴스를 생성할 때 코드 중복을 피할 수 있습니다. 이 편의 초기화 구문은 `quantity` 값 `1` 을 전달하여 클래스의 지정된 초기화 구문에 위임합니다.

`RecipeIngredient` 에 제공된 `init(name: String)` 편의 초기화 구문은 `Food` 에 `init(name: String)` _지정된_ 초기화 구문으로 같은 파라미터를 가지고 있습니다. 이 편의 초기화 구문은 상위 클래스의 지정된 초기화 구문을 재정의 하기 때문에 [초기화 구문 상속과 재정의 \(Initializer Inheritance and Overriding\)](initialization.md#initializer-inheritance-and-overriding) 에서 설명 했듯이 `override` 수식어를 붙여줘야 합니다.

`RecipeIngredient` 는 편의 초기화 구문으로 `init(name: String)` 을 제공하지만 `RecipeIngredient` 는 상위 클래스의 지정된 초기화 구문의 모든 구현을 제공했습니다. 따라서 `RecipeIngredient` 는 자동으로 모든 상위 클래스의 편의 초기화 구문도 상속합니다.

이 예제에서 `RecipeIngredient` 에 대한 상위 클래스는 `init()` 이라는 단일 편의 초기화 구문을 가지는 `Food` 입니다. 따라서 이 초기화 구문은 `RecipeIngredient` 에 의해 상속됩니다. 상속된 `init()` 버전은 `Food` 버전이 아닌 `init(name: String)` 에 `RecipeIngredient` 버전에 위임 한다는 점을 제외하고 `Food` 버전과 동일하고 동작합니다.

이 세가지 초기화 구문 모두 새로운 `RecipeIngredient` 인스턴스를 생성하기 위해 사용될 수 있습니다:

```swift
let oneMysteryItem = RecipeIngredient()
let oneBacon = RecipeIngredient(name: "Bacon")
let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)
```

계층도에 세번째와 마지막 클래스는 `ShoppingListItem` 이라는 `RecipeIngredient` 의 하위 클래스 입니다. `ShoppingListItem` 클래스는 쇼핑 리스트에 표시되는 레시피 재료를 모델링 합니다.

쇼핑 리스트에 모든 아이템은 "미구매"로 시작합니다. 이것을 표시하기 위해 `ShoppingListItem` 은 기본값이 `false` 인 `purchased` 라는 부울 프로퍼티를 도입합니다. `ShoppingListItem` 은 `ShoppingListItem` 인스턴스의 설명을 제공하는 계산된 `description` 프로퍼티도 추가합니다:

```swift
class ShoppingListItem: RecipeIngredient {
    var purchased = false
    var description: String {
        var output = "\(quantity) x \(name)"
        output += purchased ? " ✔" : " ✘"
        return output
    }
}
```

> Note   
> `ShoppingListItem` 은 여기에 모델링 된대로 쇼핑 리스트에 아이템은 항상 미구매로 시작하기 때문에 `purchased` 에 대해 초기값을 제공하기 위한 초기화 구문을 정의하지 않습니다.

도입한 모든 프로퍼티에 대해 기본값을 제공하고 초기화 구문 자체를 정의하지 않으므로 `ShoppingListItem` 은 자동으로 상위 클래스에서 _모든_ 지정된 초기화 구문과 편의 초기화 구문을 상속합니다.

아래의 그림은 세 클래스에 대한 모든 초기화 구문 체인을 나타냅니다:

![Initializers Example 3](../.gitbook/assets/initializersExample03_2x~dark.png)

상속된 세가지 초기화 구문을 모두 사용하여 새로운 `ShoppingListItem` 인스턴스를 생성할 수 있습니다:

```swift
var breakfastList = [
    ShoppingListItem(),
    ShoppingListItem(name: "Bacon"),
    ShoppingListItem(name: "Eggs", quantity: 6),
]
breakfastList[0].name = "Orange juice"
breakfastList[0].purchased = true
for item in breakfastList {
    print(item.description)
}
// 1 x Orange juice ✔
// 1 x Bacon ✘
// 6 x Eggs ✘
```

여기서 `breakfastList` 라는 새로운 배열은 3개의 새로운 `ShoppingListItem` 인스턴스를 포함하는 배열 반복 구문으로 생성됩니다. 배열의 타입은 `[ShoppingListItem]` 으로 추론됩니다. 배열이 생성된 후에 배열의 첫번째 `ShoppingListItem` 에 이름은 `"[Unnamed]"` 에서 `"Orange juice"` 로 변경되고 구매된 것으로 표기합니다. 배열에 각 아이템에 설명을 출력하면 기본 상태가 예상되로 설정되었음을 보여줍니다.

## 실패 가능한 초기화 구문 \(Failable Initializers\)

초기화가 실패할 수 있는 것에 대한 클래스, 구조체, 또는 열거형을 정의하는 것이 유용할 수 있습니다. 이 실패는 유효하지 않은 초기화 파라미터 값, 필수 외부 리소스 부재 또는 초기화 성공을 방해하는 기타 다른 조건에 의해 실패될 수 있습니다.

실패할 수 있는 초기화 조건을 대처하려면 실패 가능한 초기화 구문을 클래스, 구조체, 또는 열거형 일부로 정의합니다. `init` 키워드 뒤에 물음표를 표기하여 실패 가능한 초기화 구문을 작성합니다 \(`init?`\).

> Note   
> 동일한 파라미터 타입과 이름으로 실패 가능한 초기화 구문과 실패 불가능한 초기화 구문을 정의할 수 없습니다.

실패 가능한 초기화 구문은 초기화 하는 타입의 _옵셔널_ 값을 생성합니다. 실패 가능한 초기화 구문 내에 `return nil` 을 작성하여 초기화가 실패될 수 있는 지점을 나타냅니다.

> Note   
> 엄밀히 말하면 초기화 구문은 값을 반환하지 않습니다. 오히려 초기화 구문의 역할은 초기화가 끝날 때까지 `self` 가 완전하고 정확하게 초기화 되도록 하는 것입니다. 초기화 실패를 나타내기 위해 `return nil` 을 작성하지만 초기화 성공을 나타내기 위해 `return` 키워드를 사용하지 않습니다.

예를 들어 실패 가능한 초기화 구문은 숫자 타입 변환을 위해 구현됩니다. 숫자 타입 간의 변환이 값을 정확하게 유지하도록 하려면 `init(exactly:)` 초기화 구문을 사용합니다. 타입 변환이 값을 유지할 수 없으면 초기화 구문은 실패합니다.

```swift
let wholeNumber: Double = 12345.0
let pi = 3.14159

if let valueMaintained = Int(exactly: wholeNumber) {
    print("\(wholeNumber) conversion to Int maintains value of \(valueMaintained)")
}
// Prints "12345.0 conversion to Int maintains value of 12345"

let valueChanged = Int(exactly: pi)
// valueChanged is of type Int?, not Int

if valueChanged == nil {
    print("\(pi) conversion to Int does not maintain value")
}
// Prints "3.14159 conversion to Int does not maintain value"
```

아래 예제는 `species` 라는 `String` 프로퍼티 상수를 가진 `Animal` 이라는 구조체를 정의합니다. `Animal` 구조체는 `species` 라는 하나의 파라미터를 가진 실패 가능한 초기화 구문도 정의합니다. 이 초기화 구문은 `species` 값이 빈 문자열이 초기화 구문에 전달되는지 검사합니다. 빈 문자열을 찾으면 초기화는 실패됩니다. 반대로 `species` 프로퍼티의 값은 설정되고 초기화는 성공합니다:

```swift
struct Animal {
    let species: String
    init?(species: String) {
        if species.isEmpty { return nil }
        self.species = species
    }
}
```

이 실패 가능한 초기화 구문을 사용하여 새로운 `Animal` 인스턴스를 초기화하고 초기화가 성공했는지 검사할 수 있습니다:

```swift
let someCreature = Animal(species: "Giraffe")
// someCreature is of type Animal?, not Animal

if let giraffe = someCreature {
    print("An animal was initialized with a species of \(giraffe.species)")
}
// Prints "An animal was initialized with a species of Giraffe"
```

실패 가능한 초기화 구문의 `species` 파라미터에 빈 문자열 값을 전달하면 초기화 구문은 초기화에 실패합니다:

```swift
let anonymousCreature = Animal(species: "")
// anonymousCreature is of type Animal?, not Animal

if anonymousCreature == nil {
    print("The anonymous creature could not be initialized")
}
// Prints "The anonymous creature could not be initialized"
```

> Note   
> `"Giraffe"` 대신 `""` 와 같은 빈 문자열 값을 검사하는 것은 _옵셔널_ `String` 에 값이 없음을 나타내는 `nil` 을 검사하는 것과는 다릅니다. 위의 예제에서 빈 문자열 \(`""`\)은 유효하고 옵셔널이 아닌 `String` 입니다. 그러나 `species` 프로퍼티에 값으로 빈 문자열을 갖는 것은 동물에 대해 적절하지 않습니다. 이 제한을 모델링 하기위해 실패 가능한 초기화 구문은 빈 문자열을 찾으면 초기화에 실패합니다.

### 열거형을 위한 실패 가능한 초기화 구문 \(Failable Initializers for Enumerations\)

하나 이상의 파라미터를 기반으로 적절한 열거형 케이스를 선택하기 위해 실패 가능한 초기화 구문을 사용할 수 있습니다. 이 초기화 구문은 제공된 파라미터가 적절한 열거형 케이스와 일치하지 않으면 실패할 수 있습니다.

아래의 예제는 3가지 가능한 상태 \(`kelvin`, `celsius`, `fahrenheit`\)를 가진 `TemperatureUnit` 이라는 열거형을 정의합니다. 실패 가능한 초기화 구문은 온도 표기를 표시하는 `Character` 값에 대한 적절한 열거형 케이스를 찾는데 사용됩니다:

```swift
enum TemperatureUnit {
    case kelvin, celsius, fahrenheit
    init?(symbol: Character) {
        switch symbol {
        case "K":
            self = .kelvin
        case "C":
            self = .celsius
        case "F":
            self = .fahrenheit
        default:
            return nil
        }
    }
}
```

3가지 가능한 상태에 대한 적절한 열거형 케이스를 선택하고 파라미터가 어떠한 상태도 일치하지 않으면 실패를 발생하기 위해 실패 가능한 초기화 구문을 사용할 수 있습니다:

```swift
let fahrenheitUnit = TemperatureUnit(symbol: "F")
if fahrenheitUnit != nil {
    print("This is a defined temperature unit, so initialization succeeded.")
}
// Prints "This is a defined temperature unit, so initialization succeeded."

let unknownUnit = TemperatureUnit(symbol: "X")
if unknownUnit == nil {
    print("This is not a defined temperature unit, so initialization failed.")
}
// Prints "This is not a defined temperature unit, so initialization failed."
```

### 원시값을 가진 열거형에 대한 실패 가능한 초기화 구문 \(Failable Initializers for Enumerations with Raw Values\)

원시값을 가진 열거형은 적절한 원시값 타입의 `rawValue` 라는 파라미터를 가지고 일치하는 값을 찾으면 일치하는 열거형 케이스를 선택하거나 일치하는 값이 없으면 초기화 실패를 나타내는 실패 가능한 초기화 구문을 자동으로 받습니다.

`Character` 타입의 원시값을 사용하고 `init?(rawValue:)` 초기화 구문의 장점을 가지는 위의 `TemperatureUnit` 예제를 재작성할 수 있습니다:

```swift
enum TemperatureUnit: Character {
    case kelvin = "K", celsius = "C", fahrenheit = "F"
}

let fahrenheitUnit = TemperatureUnit(rawValue: "F")
if fahrenheitUnit != nil {
    print("This is a defined temperature unit, so initialization succeeded.")
}
// Prints "This is a defined temperature unit, so initialization succeeded."

let unknownUnit = TemperatureUnit(rawValue: "X")
if unknownUnit == nil {
    print("This is not a defined temperature unit, so initialization failed.")
}
// Prints "This is not a defined temperature unit, so initialization failed."
```

### 초기화 실패 전파 \(Propagation of Initialization Failure\)

클래스, 구조체, 또는 열거형의 실패 가능한 초기화 구문은 같은 클래스, 구조체, 또는 열거형에 다른 실패 가능한 초기화 구문으로 위임할 수 있습니다. 마찬가지로 하위 클래스 실패 가능한 초기화 구문은 상위 클래스 실패 가능한 초기화 구문으로 위임할 수 있습니다.

두 경우 모두 초기화 실패를 유발하는 다른 초기화 구문에 위임하면 전체 초기화 프로세스는 즉시 실패하고 더이상 초기화 코드는 실행되지 않습니다.

> Note   
> 실패 가능한 초기화 구문은 실패 불가능한 초기화 구문에 위임 할수도 있습니다. 실패하지 않는 기존 초기화 프로세스에 실패 상태를 추가해야 하는 경우에 사용합니다.

아래 예제는 `CartItem` 이라는 `Product` 의 하위 클래스를 정의합니다. `CartItem` 클래스는 온라인 쇼핑 카트에 있는 상품을 모델링합니다. `CartItem` 은 `quantity` 라는 저장된 상수 프로퍼티를 도입하고 적어도 이 프로퍼티는 `1` 이상의 값을 갖습니다:

```swift
class Product {
    let name: String
    init?(name: String) {
        if name.isEmpty { return nil }
        self.name = name
    }
}

class CartItem: Product {
    let quantity: Int
    init?(name: String, quantity: Int) {
        if quantity < 1 { return nil }
        self.quantity = quantity
        super.init(name: name)
    }
}
```

`CartItem` 에 대한 실패 가능한 초기화 구문은 `quantity` 값에 `1` 또는 그 이상의 값을 받는지 확인하는 것으로 시작합니다. `quantity` 가 유효하지 않으면 전체 초기화 프로세스는 즉시 실패하고 더이상 초기화 코드는 실행되지 않습니다. 마찬가지로 `Product` 에 대한 실패 가능한 초기화 구문은 `name` 값을 검사하고 `name` 이 빈 문자열이라면 즉시 초기화 구문 프로세스는 실패합니다.

이름을 가지고 `1` 또는 그 이상의 양을 가진 `CartItem` 인스턴스를 생성하면 초기화는 성공합니다:

```swift
if let twoSocks = CartItem(name: "sock", quantity: 2) {
    print("Item: \(twoSocks.name), quantity: \(twoSocks.quantity)")
}
// Prints "Item: sock, quantity: 2"
```

`0` 의 `quantity` 값을 가진 `CartItem` 인스턴스를 생성하려고 하면 `CartItem` 초기화 구문은 초기화에 실패합니다:

```swift
if let zeroShirts = CartItem(name: "shirt", quantity: 0) {
    print("Item: \(zeroShirts.name), quantity: \(zeroShirts.quantity)")
} else {
    print("Unable to initialize zero shirts")
}
// Prints "Unable to initialize zero shirts"
```

마찬가지로 빈 `name` 값을 가진 `CartItem` 인스턴스를 생성하려고 하면 상위 클래스 `Product` 초기화 구문은 초기화에 실패합니다:

```swift
if let oneUnnamed = CartItem(name: "", quantity: 1) {
    print("Item: \(oneUnnamed.name), quantity: \(oneUnnamed.quantity)")
} else {
    print("Unable to initialize one unnamed product")
}
// Prints "Unable to initialize one unnamed product"
```

### 실패 가능한 초기화 구문 재정의 \(Overriding a Failable Initializer\)

다른 초기화 구문처럼 상위 클래스 실패 가능한 초기화 구문을 하위 클래스에 재정의 할 수 있습니다. 또는 하위 클래스 _실패 불가능한_ 초기화 구문으로 상위 클래스 실패 가능한 초기화 구문을 재정의 할 수 있습니다. 이를 통해 상위 클래스의 초기화가 실패 하더라도 초기화를 실패할 수 없는 하위 클래스를 정의할 수 있습니다.

실패 가능한 상위 클래스 초기화 구문을 실패 불가능한 하위 클래스 초기화 구문으로 재정의하면 상위 클래스 초기화 구문으로 위임하는 방법은 실패 가능한 상위 클래스 초기화 구문의 값을 강제로 언래핑 하는 것 뿐입니다.

> Note   
> 실패 가능한 초기화 구문을 실패 불가능한 초기화 구문으로 재정의 할 수 있지만 그 반대는 불가능합니다.

아래의 예제는 `Document` 라는 클래스를 정의합니다. 이 클래스는 비어 있지 않은 문자열 값이나 `nil` 은 가능하지만 빈 문자열은 불가능한 `name` 프로퍼티를 가지고 초기화 될 수 있는 문서를 모델링 합니다:

```swift
class Document {
    var name: String?
    // this initializer creates a document with a nil name value
    init() {}
    // this initializer creates a document with a nonempty name value
    init?(name: String) {
        if name.isEmpty { return nil }
        self.name = name
    }
}
```

다음 예제는 `AutomaticallyNamedDocument` 라는 `Document` 의 하위 클래스를 정의합니다. `AutomaticallyNamedDocument` 하위 클래스는 `Document` 에 의해 도입된 지정된 초기화 구문 둘다 재정의합니다. 이러한 재정의는 인스턴스가 이름 없이 초기화 되거나 빈 문자열이 `init(name:)` 초기화 구문에 전달되면 `AutomaticallyNamedDocument` 인스턴스는 초기 `name` 값이 `"[Untitled]"` 임을 보장합니다:

```swift
class AutomaticallyNamedDocument: Document {
    override init() {
        super.init()
        self.name = "[Untitled]"
    }
    override init(name: String) {
        super.init()
        if name.isEmpty {
            self.name = "[Untitled]"
        } else {
            self.name = name
        }
    }
}
```

`AutomaticallyNamedDocument` 는 상위 클래스의 실패 가능한 `init?(name:)` 초기화 구문을 실패 불가능한 `init(name:)` 초기화 구문으로 재정의 합니다. `AutomaticallyNamedDocument` 는 빈 문자열 케이스를 상위 클래스와 다르게 처리하므로 초기화 구문이 실패할 필요가 없으므로 대신 실패 불가능한 초기화 구문 버전을 제공합니다.

하위 클래스의 실패 불가능한 초기화 구문의 부분으로 상위 클래스에 실패 가능한 초기화 구문을 호출하기 위해 초기화 구문에서 강제 언래핑을 사용할 수 있습니다. 예를 들어 아래의 `UntitleDocument` 하위 클래스는 항상 `"[Untitled]"` 이름을 가지고 상위 클래스 초기화 동안에 실패 가능한 `init(name:)` 초기화 구문을 사용합니다.

```swift
class UntitledDocument: Document {
    override init() {
        super.init(name: "[Untitled]")!
    }
}
```

이 경우 상위 클래스에 `init(name:)` 초기화 구문이 이름으로 빈 문자열로 호출 된 경우 강제 언래핑 작업을 통해 런타임 에러가 발생합니다. 그러나 문자열 리터럴로 호출되기 때문에 초기화 구문은 실패하지 않는 것을 알 수 있으므로 이 경우 런타임 에러가 발생하지 않습니다.

### init! 실패 가능한 초기화 구문 \(The init! Failable Initializer\)

일반적으로 `init` 키워드 뒤에 물음표를 배치하여 \(`init?`\) 적절한 타입의 옵셔널 인스턴스를 생성하는 실패 가능한 초기화 구문을 정의합니다. 또는 적절한 타입의 암시적으로 언래핑된 옵셔널 인스턴스를 생성하는 실패 가능한 초기화 구문을 정의할 수 있습니다. 물음표 대신에 `init` 키워드 뒤에 느낌표를 위치 시키면 됩니다 \(`init!`\).

`init?` 에서 `init!` 로 또는 그 반대로 위임할 수 있으며 `init?` 를 `init!` 로 또는 그 반대로 재정의 할 수 있습니다. `init` 에서 `init!` 으로 위임할 수도 있지만 그렇게 하면 `init!` 초기화 구문에 의해 초기화가 실패합니다.

## 필수 초기화 구문 \(Required Initializers\)

클래스 초기화 구문에 정의 앞에 `required` 수식어를 작성하여 클래스의 모든 하위 클래스가 해당 초기화 구문을 구현해야 함을 나타냅니다:

```swift
class SomeClass {
    required init() {
        // initializer implementation goes here
    }
}
```

또한 초기화 구문 요구사항이 체인의 추가 하위 클래스에 적용됨을 나타내기 위해 필수 초기화 구문에 모든 하위 클래스 구현 전에 `required` 수식어를 작성해야 합니다. 지정된 필수 초기화 구문을 재정의 할 때 `override` 수식어를 작성하지 않습니다:

```swift
class SomeSubclass: SomeClass {
    required init() {
        // subclass implementation of the required initializer goes here
    }
}
```

> Note   
> 상속된 초기화 구문으로 요구사항을 충족할 수 있으면 필수 초기화 구문의 명시적 구현을 제공하지 않아도 됩니다.

## 클로저 또는 함수를 사용하여 기본 프로퍼티 값 설정 \(Setting a Default Property Value with a Closure or Function\)

저장된 프로퍼티의 기본값에 일부 사용자 정의 또는 설정이 필요한 경우 해당 프로퍼티에 대한 사용자 정의된 기본값을 제공하기 위해 클로저 또는 전역 함수를 사용할 수 있습니다. 프로퍼티가 속한 타입의 새로운 인스턴스가 초기화 될 때마다 클로저나 함수는 호출되고 그것의 반환값은 프로퍼티의 기본값으로 할당됩니다.

이러한 종류의 클로저나 함수는 일반적으로 프로퍼티로 같은 타입의 임시값을 생성하고 원하는 초기상태를 나타내도록 해당 값을 조정한 다음 프로퍼티의 기본값으로 사용되기 위해 임시값을 반환합니다.

다음은 기본 프로퍼티 값을 제공하기 위해 클로저를 사용하는 방법에 대한 윤곽입니다:

```swift
class SomeClass {
    let someProperty: SomeType = {
        // create a default value for someProperty inside this closure
        // someValue must be of the same type as SomeType
        return someValue
    }()
}
```

클로저의 중괄호 끝에는 빈 소괄호 쌍이 옵니다. 이것은 Swift가 클로저를 즉시 실행하도록 합니다. 만약 소괄호를 생략한다면 클로저의 반환값이 아닌 프로퍼티에 클로저 자체를 할당하려고 합니다.

> Note   
> 프로퍼티를 초기화하기 위해 클로저를 사용하면 클로저가 실행될 때 다른 인스턴스는 아직 초기화 되지 않았다는 것을 기억해야 합니다. 해당 프로퍼티에 기본값이 있더라도 클로저 내에서 다른 프로퍼티 값에 접근할 수 없다는 의미입니다. 또한 암시적으로 `self` 프로퍼티를 사용하거나 인스턴스의 메서드를 호출할 수 없습니다.

아래 예제는 체스 게임을 위한 보드를 모델링하는 `Chessboard` 라는 구조체를 정의합니다. 체스는 검은색과 하얀색의 사각형이 번갈아 가며 8 x 8 보드에서 플레이 됩니다.

![Chessboard](../.gitbook/assets/chessBoard_2x~dark.png)

게임보드를 표현하기 위해 `Chessboard` 구조체는 64개의 `Bool` 값 배열인 `boardColors` 라는 하나의 프로퍼티를 가집니다. 배열에 `true` 값은 검은색 사각형을 표시하고 `false` 값은 하얀색 사각형을 표시합니다. 배열의 첫번째 항목은 게임보드에 좌측상단을 나타내고 배열에 마지막 항목은 게임보드의 우측하단을 나타냅니다.

`boardColors` 배열은 색깔값을 설정하기 위한 클로저로 초기화 됩니다:

```swift
struct Chessboard {
    let boardColors: [Bool] = {
        var temporaryBoard = [Bool]()
        var isBlack = false
        for i in 1...8 {
            for j in 1...8 {
                temporaryBoard.append(isBlack)
                isBlack = !isBlack
            }
            isBlack = !isBlack
        }
        return temporaryBoard
    }()
    func squareIsBlackAt(row: Int, column: Int) -> Bool {
        return boardColors[(row * 8) + column]
    }
}
```

새로운 `Chessboard` 인스턴스가 생성될 때마다 클로저가 실행되고 `boardColors` 의 기본값은 계산되고 반환됩니다. 위의 예제의 클로저는 `temporaryBoard` 라는 임시 배열에 보드의 각 사각형에 대한 적절한 색깔을 계산하고 설정하고 설정이 완료되면 클로저의 반환값으로 임시 배열을 반환합니다. 반환된 배열값은 `boardColors` 에 저장되고 `squareIsBlackAt(row:coloumn:)` 유틸리티 함수로 조회할 수 있습니다:

```swift
let board = Chessboard()
print(board.squareIsBlackAt(row: 0, column: 1))
// Prints "true"
print(board.squareIsBlackAt(row: 7, column: 7))
// Prints "false"
```

