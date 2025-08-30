# 초기화 (Initialization)

타입의 저장 프로퍼티에 초기값을 설정하고 초기 설정을 수행합니다.

*초기화(Initialization)*는 클래스, 구조체, 열거형의 인스턴스를
사용하기 위해 준비하는 과정입니다.
이 과정에서는 인스턴스의 각 저장 프로퍼티에 초기값을 설정하고
새로운 인스턴스가 사용되기 전에
다른 설정이나 초기화를 수행하는 것을 포함합니다.

특정 타입의 새로운 인스턴스를 생성하기 위해
특정 메서드를 호출하는 것처럼
*이니셜라이저(initializers)*을 정의하여 초기화를 구현합니다.
Objective-C 이니셜라이저과 달리 Swift 이니셜라이저는 값을 반환하지 않습니다.
초기화의 주요 역할은 처음 사용되기 전에
타입의 새로운 인스턴스가 올바르게 초기화되는 것을 보장하는 것입니다.

클래스 타입의 인스턴스는 클래스의 인스턴스가 할당 해제되기 전에 정리를 수행하는
*디이니셜라이저(deinitializer)*도 구현할 수 있습니다.
자세한 내용은 [소멸 (Deinitialization)](./deinitialization.md)를 참고바랍니다.

## 저장 프로퍼티에 초기값 설정 (Setting Initial Values for Stored Properties)

클래스와 구조체는 해당 클래스나 구조체의 인스턴스가 생성되기 전까지
모든 저장 프로퍼티에
적절한 초기값을 *반드시* 설정해야 합니다.
저장 프로퍼티는 불확정한 상태로 남아있을 수 없습니다.

이니셜라이저 내에서 저장 프로퍼티의 초기값을 설정하거나,
프로퍼티의 정의 시 기본값을 할당할 수 있습니다.
이러한 동작에 대해 다음 섹션에 설명하도록 하겠습니다.

> Note: 저장 프로퍼티에 기본값을 할당하거나
> 이니셜라이저 내에 값을 설정할 때,
> 해당 프로퍼티의 값은
> 프로퍼티 관찰자 호출없이 직접 설정됩니다.

### 이니셜라이저 (Initializers)

*이니셜라이저(Initializers)*는 특정 타입의 새로운 인스턴스를 생성하기 위해 호출됩니다.
가장 간단한 형식의 이니셜라이저는 `init` 키워드를 사용하여 작성하며
매개변수가 없는 인스턴스 메서드와 같습니다:

```swift
init() {
    // perform some initialization here
}
```

<!--
  - test: `initializerSyntax`

  ```swifttest
  >> class Test {
  -> init() {
        // perform some initialization here
     }
  >> }
  ```
-->

아래의 예시는 화씨 온도를 저장하는
`Fahrenheit`라는 새로운 구조체를 정의합니다.
`Fahrenheit` 구조체는 `Double` 타입의 `temperature`라는
하나의 저장 프로퍼티를 가지고 있습니다:

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

<!--
  - test: `fahrenheitInit`

  ```swifttest
  -> struct Fahrenheit {
        var temperature: Double
        init() {
           temperature = 32.0
        }
     }
  -> var f = Fahrenheit()
  -> print("The default temperature is \(f.temperature)° Fahrenheit")
  <- The default temperature is 32.0° Fahrenheit
  ```
-->

이 구조체는 매개변수가 없는 단일 이니셜라이저 `init`을 정의하며,
`32.0`(물이 얼어버리는 화씨 온도)의 값이
저장된 온도로 초기화합니다.

### 기본 프로퍼티 값 (Default Property Values)

위에서 본 것처럼
이니셜라이저 내에서 저장 프로퍼티의 초기값을 설정할 수 있습니다.
또는 프로퍼티 선언 시 *기본 프로퍼티 값(default property value)*을
지정할 수 있습니다.
프로퍼티를 정의할 때
프로퍼티의 초기값을 할당하는 것으로 기본 프로퍼티 값을 지정합니다.

> Note: 프로퍼티가 항상 같은 초기값을 갖는다면,
> 이니셜라이저 내에서 값을 설정하기보다 기본값을 제공하는 것이 좋습니다.
> 결과는 같지만,
> 기본값은 프로퍼티의 초기화를 선언에 더 가깝게 연결합니다.
> 이것은 이니셜라이저를 더 짧고, 명확하게 하고
> 기본값으로부터 프로퍼티의 타입을 유추할 수 있습니다.
> 기본값은 이 챕터 후반부에 설명하듯이
> 기본 이니셜라이저와 이니셜라이저 상속을
> 더 간단한 형태로 작성할 수 있습니다.

프로퍼티가 선언될 때
`temperature`에 기본값을 제공하여
더 간단한 형태로 위의 `Fahrenheit` 구조체를 작성할 수 있습니다:

```swift
struct Fahrenheit {
    var temperature = 32.0
}
```

<!--
  - test: `fahrenheitDefault`

  ```swifttest
  -> struct Fahrenheit {
        var temperature = 32.0
     }
  ```
-->

## 커스텀 이니셜라이저 (Customizing Initialization)

초기화 과정은
입력 매개변수와 선택적 프로퍼티 타입,
또는 상수 프로퍼티를 초기화 중에 할당하는 방식으로
커스텀할 수 있습니다.

### 초기화 매개변수 (Initialization Parameters)

이니셜라이저 정의 시 *초기화 매개변수(initialization parameters)*를 제공하여
초기화 과정을 커스텀할 수 있습니다.
초기화 매개변수는 함수 및 메서드 매개변수와
동일한 기능과 구문을 가지고 있습니다.

다음 예시는 섭씨 온도를 저장하는
`Celsius`라는 구조체를 정의합니다.
`Celsius` 구조체는 `init(fromFahrenheit:)`와 `init(fromKelvin:)`이라는
두 개의 커스텀 이니셜라이저를 구현하며,
각각 다른 온도 단위에서 값을 받아
구조체의 새로운 인스턴스를 초기화합니다:

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

<!--
  - test: `initialization`

  ```swifttest
  -> struct Celsius {
        var temperatureInCelsius: Double
        init(fromFahrenheit fahrenheit: Double) {
           temperatureInCelsius = (fahrenheit - 32.0) / 1.8
        }
        init(fromKelvin kelvin: Double) {
           temperatureInCelsius = kelvin - 273.15
        }
     }
  -> let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
  /> boilingPointOfWater.temperatureInCelsius is \(boilingPointOfWater.temperatureInCelsius)
  </ boilingPointOfWater.temperatureInCelsius is 100.0
  -> let freezingPointOfWater = Celsius(fromKelvin: 273.15)
  /> freezingPointOfWater.temperatureInCelsius is \(freezingPointOfWater.temperatureInCelsius)
  </ freezingPointOfWater.temperatureInCelsius is 0.0
  ```
-->

첫 번째 이니셜라이저는 `fromFahrenheit`라는 인자 레이블과
`fahrenheit`라는 매개변수 이름을 가진 하나의 초기화 매개변수를 가지고 있습니다.
두 번째 이니셜라이저는 `fromKelvin`라는 인자 레이블과
`kelvin`이라는 매개변수 이름을 가진 하나의 초기화 매개변수를 가지고 있습니다.
두 이니셜라이저 모두 전달받은 하나의 값을
섭씨 온도로 변환하고,
`temperatureInCelsius`라는 프로퍼티에 값을 저장합니다.

<!--
  TODO: I need to provide an example of default values for initializer parameters,
  to show they can help you to get multiple initializers "for free" (after a fashion).
-->

### 매개변수 이름과 인자 레이블 (Parameter Names and Argument Labels)

함수와 메서드의 매개변수와 마찬가지로,
초기화 매개변수는 이니셜라이저의 본문 내에서
사용하는 매개변수 이름과
이니셜라이저를 호출할 때 사용하는 인자 레이블 모두 가질 수 있습니다.

그러나 이니셜라이저는 함수와 메서드처럼
소괄호 앞에 식별용 함수 이름을 가지지 않습니다.
따라서 이니셜라이저의 매개변수 이름과 타입이
어떤 이니셜라이저를 호출해야 하는지 식별하는데 특히 중요한 역할을 합니다.
이러한 이유 때문에 Swift는 인자 레이블을 제공하지 않으면
이니셜라이저의 *모든* 매개변수에 대해 자동으로 인자 레이블을 제공합니다.

다음 예시는 `red`, `green`, `blue`라는 세 개의 상수 프로퍼티를 가지는
`Color`라는 구조체를 정의합니다.
이 프로퍼티는 색상에서의 빨강, 초록, 파랑의 비율을 나타내기 위해
`0.0`에서 `1.0`사이의 값을 저장합니다.

`Color`는 빨강, 초록, 파랑 성분에 대해
`Double` 타입의 세 개의 적절한 이름의 매개변수를 가진
이니셜라이저를 제공합니다.
`Color`는 단일 `white` 매개변수를 가지는 두 번째 이니셜라이저도 제공하는데,
세 가지 생상 성분 모두에 동일한 값을 지정할 때 사용합니다.

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

<!--
  - test: `externalParameterNames, externalParameterNames-err`

  ```swifttest
  -> struct Color {
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
-->

두 이니셜라이저 모두 각 이니셜라이저 매개변수에 이름이 지정된 값을 제공하여
새로운 `Color` 인스턴스를 생성할 수 있습니다:

```swift
let magenta = Color(red: 1.0, green: 0.0, blue: 1.0)
let halfGray = Color(white: 0.5)
```

<!--
  - test: `externalParameterNames`

  ```swifttest
  -> let magenta = Color(red: 1.0, green: 0.0, blue: 1.0)
  -> let halfGray = Color(white: 0.5)
  >> assert(halfGray.red == 0.5)
  >> assert(halfGray.green == 0.5)
  >> assert(halfGray.blue == 0.5)
  ```
-->

이러한 이니셜라이저를 호출할 때는
반드시 인자 레이블을 사용해야 합니다.
인자 레이블이 정의되어 있는 경우,
이를 생략하면 컴파일 시 오류가 발생합니다:

```swift
let veryGreen = Color(0.0, 1.0, 0.0)
// this reports a compile-time error - argument labels are required
```

<!--
  - test: `externalParameterNames-err`

  ```swifttest
  -> let veryGreen = Color(0.0, 1.0, 0.0)
  // this reports a compile-time error - argument labels are required
  !$ error: missing argument labels 'red:green:blue:' in call
  !! let veryGreen = Color(0.0, 1.0, 0.0)
  !! ^
  !! red: green:  blue:
  ```
-->

### 인자 레이블이 없는 이니셜라이저 매개변수 (Initializer Parameters Without Argument Labels)

이니셜라이저 매개변수에 대해 인자 레이블을 사용하고 싶지 않은 경우,
매개변수 인자 레이블 위치에 언더바(`_`)를 작성하여
기본 동작을 무시할 수 있습니다.

다음은 [초기화 매개변수 (Initialization Parameters)](#초기화-매개변수-initialization-parameters)에서의
`Celsius` 예시를 확장한 것으로
이미 섭씨 온도 값인 `Double` 값을 이용해
새로운 `Celsius` 인스턴스를 생성할 수 있는 추가 이니셜라이저를 포함하고 있습니다:

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

<!--
  - test: `initializersWithoutExternalParameterNames`

  ```swifttest
  -> struct Celsius {
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
  -> let bodyTemperature = Celsius(37.0)
  /> bodyTemperature.temperatureInCelsius is \(bodyTemperature.temperatureInCelsius)
  </ bodyTemperature.temperatureInCelsius is 37.0
  ```
-->

이니셜라이저 호출 `Celsius(37.0)`은
인자 레이블 없이도 그 의도가 명확합니다.
따라서 이니셜라이저를 `init(_ celsius: Double)`로 작성하여
이름 없는 `Double` 값을 제공하여 호출할 수 있습니다.

### 옵셔널 프로퍼티 타입 (Optional Property Types)

커스텀 타입에 저장 프로퍼티가 있는데, 해당 프로퍼티가 논리적으로 "값이 없을 수 있음"이 허용되는 경우 ---
초기화 중에 값을 절정할 수 없거나,
나중에 "값 없음" 상태가 될 수 있는 경우 ---
그 프로퍼티를 옵셔널 타입으로 선언할 수 있습니다.
옵셔널 타입(optional type)의 프로퍼티는 자동으로 초기화 동안
"아직 값 없음"을 가진다는 의도를 위해
`nil`의 값으로 초기화 됩니다.

다음의 예시는 `response`라는 옵셔널 `String` 프로퍼티를 가지는
`SurveyQuestion`이라는 클래스를 정의합니다:

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

<!--
  - test: `surveyQuestionVariable`

  ```swifttest
  -> class SurveyQuestion {
        var text: String
        var response: String?
        init(text: String) {
           self.text = text
        }
        func ask() {
           print(text)
        }
     }
  -> let cheeseQuestion = SurveyQuestion(text: "Do you like cheese?")
  -> cheeseQuestion.ask()
  <- Do you like cheese?
  -> cheeseQuestion.response = "Yes, I do like cheese."
  ```
-->

설문 질문에 대한 응답은 실제로 질문되기 전까지 알 수 없으므로,
`response` 프로퍼티는 `String?` 타입,
즉 "옵셔널 `String`" 타입으로 선언됩니다.
새로운 인스턴스 `SurveyQuestion`이 초기화 될 때,
"아직 문자열 없음"이라는 의미의 기본값 `nil`이 자동으로 할당됩니다.

### 초기화 중 상수 프로퍼티 값 할당 (Assigning Constant Properties During Initialization)

상수 프로퍼티는 초기화가 완료되기 전까지만
값을 확정적으로 할당하면 되므로,
초기화 중 언제든지 값을 할당할 수 있습니다.
상수 프로퍼티에 값이 할당되면
더 이상 수정할 수 없습니다.

<!--
  - test: `constantPropertyAssignment`

  ```swifttest
  >> struct S {
        let c: Int
        init() {
           self.c = 1
           self.c = 2
        }
     }
  !$ error: immutable value 'self.c' may only be initialized once
  !! self.c = 2
  !! ^
  !$ note: change 'let' to 'var' to make it mutable
  !! let c: Int
  !! ^~~
  !! var
  ```
-->

<!--
  - test: `constantPropertyAssignmentWithInitialValue`

  ```swifttest
  >> struct S {
        let c: Int = 0
        init() {
           self.c = 1
        }
     }
  !$ error: immutable value 'self.c' may only be initialized once
  !! self.c = 1
  !! ^
  !$ note: initial value already provided in 'let' declaration
  !! let c: Int = 0
  !! ^
  !$ note: change 'let' to 'var' to make it mutable
  !! let c: Int = 0
  !! ^~~
  !! var
  ```
-->

> Note: 클래스 인스턴스의 경우,
> 상수 프로퍼티는 해당 프로퍼티를 정의한 클래스에서만
> 초기화 중에 수정할 수 있습니다.
> 하위 클래스에서는 해당 상수 프로퍼티를 수정할 수 없습니다.

위의 `SurveyQuestion` 예시를 수정하여
`SurveyQuestion`의 인스턴스가 생성되면 질문이 바뀌지 않는다는 것을 나타내기 위해
질문의 `text` 프로퍼티를 변수 프로퍼티 대신 상수 프로퍼티로 정의할 수 있습니다.
이제 `text` 프로퍼티는 상수이지만,
여전히 클래스의 이니셜라이저 내에서는 값을 설정할 수 있습니다:

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

<!--
  - test: `surveyQuestionConstant`

  ```swifttest
  -> class SurveyQuestion {
        let text: String
        var response: String?
        init(text: String) {
           self.text = text
        }
        func ask() {
           print(text)
        }
     }
  -> let beetsQuestion = SurveyQuestion(text: "How about beets?")
  -> beetsQuestion.ask()
  <- How about beets?
  -> beetsQuestion.response = "I also like beets. (But not with cheese.)"
  ```
-->

## 기본 이니셜라이저 (Default Initializers)

Swift는 모든 프로퍼티에 기본값을 제공하고
별도의 이니셜라이저를 하나도 제공하지 않는
구조체나 클래스에 대해
*기본 이니셜라이저(default initializer)*를 제공합니다.
기본 이니셜라이저는 모든 프로퍼티가 기본값으로 설정된
새로운 인스턴스를 생성합니다.

<!--
  - test: `defaultInitializersForStructAndClass`

  ```swifttest
  -> struct S { var s: String = "s" }
  -> assert(S().s == "s")
  -> class A { var a: String = "a" }
  -> assert(A().a == "a")
  -> class B: A { var b: String = "b" }
  -> assert(B().a == "a")
  -> assert(B().b ==  "b")
  ```
-->

이 예시는 쇼핑 목록에 있는
항목의 이름, 수량, 구매 상태를 캡슐화하는
`ShoppingListItem`이라는 클래스를 정의합니다:

```swift
class ShoppingListItem {
    var name: String?
    var quantity = 1
    var purchased = false
}
var item = ShoppingListItem()
```

<!--
  - test: `initialization`

  ```swifttest
  -> class ShoppingListItem {
        var name: String?
        var quantity = 1
        var purchased = false
     }
  -> var item = ShoppingListItem()
  ```
-->

`ShoppingListItem` 클래스의 모든 프로퍼티는 기본값을 가지고 있고,
상속받은 상위 클래스가 없는 기본 클래스이므로,
`ShoppingListItem`은 자동으로 모든 프로퍼티를 기본값으로 설정한
새로운 인스턴스를 생성하는 기본 이니셜라이저 구현을 제공합니다.
(`name` 프로퍼티는 코드에서 값을 작성하지 않았지만
옵셔널 `String` 프로퍼티이므로,
자동으로 `nil`의 기본값이 할당됩니다.)
위의 예시는 `ShoppingListItem` 클래스의 기본 이니셜라이저를 사용하여
`ShoppingListItem()`으로
클래스의 새로운 인스턴스를 생성하고,
이를 `item`이라는 변수에 할당합니다.

### 구조체 타입의 멤버와이즈 이니셜라이저 (Memberwise Initializers for Structure Types)

구조체 타입은 커스텀 이니셜라이저를 정의하지 않으면
자동으로 *멤버와이즈 이니셜라이저(memberwise initializer)*를 받습니다.
기본 이니셜라이저와 다르게
구조체는 저장 프로퍼티에 기본값이 없어도
멤버와이즈 이니셜라이저를 받습니다.

<!--
  - test: `memberwiseInitializersDontRequireDefaultStoredPropertyValues`

  ```swifttest
  -> struct S { var int: Int; var string: String }
  -> let s = S(int: 42, string: "hello")

  -> struct SS { var int = 10; var string: String }
  -> let ss = SS(int: 42, string: "hello")
  ```
-->

멤버와이즈 이니셜라이저는 새로운 구조체 인스턴스의 멤버 프로퍼티를 초기화하는
간편한 방법입니다.
새로운 인스턴스의 프로퍼티에 대한 초기값은
이름으로 멤버와이즈 이니셜라이저에 전달할 수 있습니다.

아래의 예시는 `width`와 `height`라는 두 개의 프로퍼티를 가지는
`Size`라는 구조체를 정의합니다.
두 프로퍼티 모두 `0.0`의 기본값이 할당되어
`Double` 타입으로 추론됩니다.

`Size` 구조체는 자동으로
새로운 `Size` 인스턴스를 초기화할 수 있는
`init(width:height:)` 멤버와이즈 이니셜라이저를 받습니다:

```swift
struct Size {
    var width = 0.0, height = 0.0
}
let twoByTwo = Size(width: 2.0, height: 2.0)
```

<!--
  - test: `initialization`

  ```swifttest
  -> struct Size {
        var width = 0.0, height = 0.0
     }
  -> let twoByTwo = Size(width: 2.0, height: 2.0)
  ```
-->

멤버와이즈 이니셜라이저를 호출할 때,
기본값을 가지는
모든 프로퍼티는 생략할 수 있습니다.
위의 예시에서
`Size` 구조체는 `height`와 `width` 프로퍼티 둘 다
기본값을 가지고 있습니다.
하나 또는 프로퍼티 둘 다 생략할 수 있고,
이니셜라이저는 생략된 항목에 대해 기본값을 사용합니다.
예를 들어:

```swift
let zeroByTwo = Size(height: 2.0)
print(zeroByTwo.width, zeroByTwo.height)
// Prints "0.0 2.0"

let zeroByZero = Size()
print(zeroByZero.width, zeroByZero.height)
// Prints "0.0 0.0"
```

<!--
  - test: `initialization`

  ```swifttest
  -> let zeroByTwo = Size(height: 2.0)
  -> print(zeroByTwo.width, zeroByTwo.height)
  <- 0.0 2.0

  -> let zeroByZero = Size()
  -> print(zeroByZero.width, zeroByZero.height)
  <- 0.0 0.0
  ```
-->

## 값 타입을 위한 이니셜라이저 위임 (Initializer Delegation for Value Types)

이니셜라이저는 인스턴스의 초기화의 부분을 수행하기 위해 다른 이니셜라이저를 호출할 수 있습니다.
이 과정을 *이니셜라이저 위임(initializer delegation)*이라고 하며,
여러 이니셜라이저에 중복되는 코드를 피할 수 있도록 해줍니다.

이니셜라이저 위임이 동작하는 방식과
허용하는 위임 형태에 대한 규칙은
값 타입과 클래스 타입에 따라 다릅니다.
값 타입(구조체와 열거형)은 상속을 지원하지 않기 때문에,
이니셜라이저 위임 과정이 비교적 단순하며,
오직 자신이 제공하는 다른 이니셜라이저에만 위임할 수 있습니다.
그러나 클래스는 [상속 (Inheritance)](./inheritance.md)에서 설명했듯이
다른 클래스로부터 상속받을 수 있습니다.
이는 상속받은 모든 저장 프로퍼티에 적절한 값이 할당되도록
추가적인 책임이 있음을 의미합니다.
이러한 책임은
아래 [클래스 상속과 초기화 (Class Inheritance and Initialization)](#클래스-상속과-초기화-class-inheritance-and-initialization)에 설명되어 있습니다.

값 타입에서 커스텀 이니셜라이저를 작성할 때 같은 값 타입 내의
다른 이니셜라이저를 참조하기 위해 `self.init`을 사용합니다.
이니셜라이저 내에서만 `self.init`을 호출할 수 있습니다.

값 타입에 대해 커스텀 이니셜라이저를 정의하면
해당 타입에 대해 기본 이니셜라이저
(또는 구조체의 경우 멤버와이즈 이니셜라이저)에 더 이상 접근할 수 없습니다.
이 제약은 복잡한 이니셜라이저에서 제공하는
필수 설정 절차를
자동 이니셜라이저를 사용해 우회해버리는 상황을 방지하기 위한 것입니다.

> Note: 기본 이니셜라이저와
> 멤버와이즈 이니셜라이저를 그대로 유지하면서도
> 커스텀 이니셜라이저를 추가하고 싶다면,
> 커스텀 이니셜라이저를 값 타입의 원래 구현이 아닌
> 확장에서 작성해야 합니다.
> 자세한 내용은 [확장 (Extensions)](./extensions.md)을 참고바랍니다.

다음의 예시는 기하학적 사각형을 나타내는 `Rect` 구조체를 정의합니다.
이 예시는 `Size`와 `Point`라는 두 개의 보조 구조체가 필요하고,
둘 다 모든 프로퍼티에 대해 `0,0`의 기본값을 제공합니다:

```swift
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
```

<!--
  - test: `valueDelegation`

  ```swifttest
  -> struct Size {
        var width = 0.0, height = 0.0
     }
  -> struct Point {
        var x = 0.0, y = 0.0
     }
  ```
-->

아래의 `Rect` 구조체는 세 가지 방법 중 하나로 초기화할 수 있습니다 ---
기본 0으로 초기화된 `origin`과 `size`를 사용하거나,
특정한 원점과 크기를 제공하거나,
특정한 중앙점과 크기를 제공하는 방법입니다.
이러한 초기화 옵션은
`Rect` 구조체의 정의에 포함된 세 개의 커스텀 이니셜라이저로 구현되어 있습니다:

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

<!--
  - test: `valueDelegation`

  ```swifttest
  -> struct Rect {
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
-->

첫 번째 `Rect` 이니셜라이저인 `init()`은
구조체가 커스텀 이니셜라이저를 가지지 않았을 경우
자동으로 제공받게 되는 기본 이니셜라이저와 기능적으로 동일합니다.
이 이니셜라이저는 빈 중괄호 `{}`로 표시된
빈 본문을 가집니다.
이 이니셜라이저를 호출하면
`origin`과 `size` 프로퍼티 각각
`Point(x: 0.0, y: 0.0)`와
`Size(width: 0.0, height: 0.0)`이라는
프로퍼티 정의의 기본값으로 초기화 되는 `Rect` 인스턴스를 반환합니다:

```swift
let basicRect = Rect()
// basicRect's origin is (0.0, 0.0) and its size is (0.0, 0.0)
```

<!--
  - test: `valueDelegation`

  ```swifttest
  -> let basicRect = Rect()
  /> basicRect's origin is (\(basicRect.origin.x), \(basicRect.origin.y)) and its size is (\(basicRect.size.width), \(basicRect.size.height))
  </ basicRect's origin is (0.0, 0.0) and its size is (0.0, 0.0)
  ```
-->

두 번째 `Rect` 이니셜라이저인 `init(origin:size:)`는
구조체가 커스텀 이니셜라이저가 없으면
자동으로 제공받는 멤버와이즈 이니셜라이저와 기능적으로 동일합니다.
이 이니셜라이저는 전달된 `origin`과 `size` 인자값을
해당 저장 프로퍼티에 단순히 할당합니다:

```swift
let originRect = Rect(origin: Point(x: 2.0, y: 2.0),
                      size: Size(width: 5.0, height: 5.0))
// originRect's origin is (2.0, 2.0) and its size is (5.0, 5.0)
```

<!--
  - test: `valueDelegation`

  ```swifttest
  -> let originRect = Rect(origin: Point(x: 2.0, y: 2.0),
        size: Size(width: 5.0, height: 5.0))
  /> originRect's origin is (\(originRect.origin.x), \(originRect.origin.y)) and its size is (\(originRect.size.width), \(originRect.size.height))
  </ originRect's origin is (2.0, 2.0) and its size is (5.0, 5.0)
  ```
-->

세 번째 `Rect` 이니셜라이저인 `init(center:size:)`는 약간 더 복잡합니다.
이 이니셜라이저는 `center`와 `size` 값을 기반으로
적절한 원점을 계산하는 것으로 시작합니다.
그런 다음 `init(origin:size:)` 이니셜라이저를 호출(또는 *위임*)하여
새로 계산된 원점과 크기로 해당 프로퍼티에 저장합니다:

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                      size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
```

<!--
  - test: `valueDelegation`

  ```swifttest
  -> let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
        size: Size(width: 3.0, height: 3.0))
  /> centerRect's origin is (\(centerRect.origin.x), \(centerRect.origin.y)) and its size is (\(centerRect.size.width), \(centerRect.size.height))
  </ centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
  ```
-->

`init(center:size:)` 이니셜라이저는
`origin`과 `size`의 새로운 값을 해당 프로퍼티에 직접 할당할 수 있습니다.
그러나 `init(center:size:)` 이니셜라이저가 이미 해당 기능을 정확히 제공하는
기존 이니셜라이저를 활용하는 것이
더 편리하고 의도도 더 명확하게 드러낼 수 있습니다.

> Note: `init()`과 `init(origin:size:)` 이니셜라이저를
> 직접 정의하지 않고 이 예시를 작성하는 다른 방법은
> [확장 (Extensions)](./extensions.md)을 참고바랍니다.

## 클래스 상속과 초기화 (Class Inheritance and Initialization)

클래스의 저장 프로퍼티는 ---
상위 클래스로부터 상속받은 프로퍼티를 포함하여 ---
모두 초기화 과정에서 초기값이 *반드시* 할당되어야 합니다.

Swift는 클래스의 모든 저장 프로퍼티가 초기값을 가질 수 있도록
두 가지 종류의 이니셜라이저를 정의합니다.
이 이니셜라이저들은 지정 이니셜라이저(designated initializer)와 편의 이니셜라이저(convenience initializer)로 알려져 있습니다.

### 지정 이니셜라이저와 편의 이니셜라이저 (Designated Initializers and Convenience Initializers)

*지정 이니셜라이저(Designated initializers)*는 클래스의 기본 이니셜라이저입니다.
지정 이니셜라이저는 해당 클래스에서 정의한 모든 프로퍼티를 완벽하게 초기화하고,
초기화 과정을 상위 클래스 체인으로 이어가기 위해
적절한 상위 클래스의 이니셜라이저를 호출합니다.

클래스는 일반적으로 지정 이니셜라이저를 매우 적게 가지며,
클래스에 하나만 있는 경우도 흔합니다.
지정 이니셜라이저는 초기화가 이루어지는 "중심 경로(funnel)" 역할을 하며,
이 경로를 통해 초기화가 상위 클래스 체인으로 이어집니다.

모든 클래스는 최소 하나의 지정 이니셜라이저를 가지고 있어야 합니다.
경우에 따라 이 요구사항은
아래의 [자동 이니셜라이저 상속 (Automatic Initializer Inheritance)](#자동-이니셜라이저-상속-automatic-initializer-inheritance)에서 설명했듯이
상위 클래스의 지정 이니셜라이저를 하나 이상 상속받는 것으로 충족됩니다.

*편의 이니셜라이저(Convenience initializers)*는 클래스의 보조 이니셜라이저입니다.
편의 이니셜라이저는 동일한 클래스 내의
지정 이니셜라이저를 호출하되,
일부 매개변수를 기본값으로 설정하여 호출할 수 있습니다.
특정 사용 사례나 입력 타입에 대한 해당 클래스의 인스턴스를 생성하기 위해
편의 이니셜라이저를 정의할 수도 있습니다.

클래스에 편의 이니셜라이저가 반드시 필요한 것은 아니며, 필요하지 않다면 제공하지 않아도 됩니다.
일반적인 초기화 패턴을 간편하게 처리하거나,
클래스 초기화의 의도를 더 명확히 하고자 할 때 편의 이니셜라이저를 만드는 것이 좋습니다.

### 지정 이니셜라이저와 편의 이니셜라이저의 문법 (Syntax for Designated and Convenience Initializers)

클래스의 지정 이니셜라이저는 값 타입의 간단한 이니셜라이저와
동일한 방법으로 작성됩니다:

```swift
init(<#parameters#>) {
   <#statements#>
}
```

편의 이니셜라이저는 동일한 형식으로 작성되지만,
`init` 키워드 앞에 `convenience` 한정자를
공백으로 구분하여 작성합니다:

```swift
convenience init(<#parameters#>) {
   <#statements#>
}
```

### 클래스 타입의 이니셜라이저 위임 (Initializer Delegation for Class Types)

지정 이니셜라이저와 편의 이니셜라이저 사이의 관계를 단순화하기 위해,
Swift는 이니셜라이저 사이의 위임 호출에 대해 다음 세 가지 규칙을 적용합니다:

- **규칙 1**:
  지정 이니셜라이저는 자신과 가장 가까운 상위 클래스의 지정 이니셜라이저를 호출해야 합니다.

- **규칙 2**:
  편의 이니셜라이저는 *같은* 클래스의 다른 이니셜라이저를 호출해야 합니다.

- **규칙 3**:
  편의 이니셜라이저는 결국 지정 이니셜라이저를 호출해야 합니다.

이 규칙을 기억하는 간단한 방법은 아래와 같습니다:

- 지정 이니셜라이저는 항상 위로 위임해야 합니다.
- 편의 이니셜라이저는 항상 옆으로 위임해야 합니다.

이러한 규칙은 아래의 그림에 설명되어 있습니다:

![Initializer Delegation](../.gitbook/assets/initializerDelegation01_2x~dark.png)

여기에서 상위 클래스는 하나의 지정 이니셜라이저와 두 개의 편의 이니셜라이저를 가지고 있습니다.
하나의 편의 이니셜라이저는 다른 편의 이니셜라이저를 호출하고,
그 이니셜라이저는 다시 지정 이니셜라이저를 호출합니다.
이는 위의 규칙 2와 3을 충족합니다.
상위 클래스는 더 상위의 상위 클래스를 가지고 있지 않으므로 규칙 1은 적용되지 않습니다.

그림에 하위 클래스는 두 개의 지정 이니셜라이저와 하나의 편의 이니셜라이저를 가지고 있습니다.
편의 이니셜라이저는 같은 클래스의 다른 이니셜라이저를 호출해야 하므로,
지정 이니셜라이저 중 하나를 호출해야 합니다.
이는 위의 규칙 2와 3을 충족합니다.
두 개의 지정 이니셜라이저는 모두 상위 클래스의 지정 이니셜라이저를 호출해야 하므로,
규칙 1도 충족됩니다.

> Note: 이러한 규칙은 각 클래스의 인스턴스를 *생성*하는 방법에 영향을 주지 않습니다.
> 위 다이어그램에 모든 이니셜라이저는
> 해당 클래스의 완전한 인스턴스를 생성하는 데 사용할 수 있습니다.
> 규칙은 클래스의 이니셜라이저 구현을 작성할 때만 영향을 줍니다.

아래 그림은 네 개의 클래스에 대한 더 복잡한 클래스 계층도를 보여줍니다.
이 계층도에서 지정 이니셜라이저는
클래스 초기화의 "중심 경로"로 작동하며,
클래스 간의 관계를 단순화하는 방식을 설명합니다:

![Initializer Delegation2](../.gitbook/assets/initializerDelegation02_2x~dark.png)

### 2단계 초기화 (Two-Phase Initialization)

Swift에서 클래스 초기화는 두 단계로 이루어집니다.
1단계에서는 각 저장 프로퍼티가 해당 프로퍼티를 정의한 클래스에 의해
초기값을 할당받습니다.
모든 저장 프로퍼티의 초기 상태가 결정되면,
2단계가 시작되며,
각 클래스는 새 인스턴스가 사용 가능 상태가 되기 전에
자신의 저장 프로퍼티를 추가로 구성할 수 있는 기회를 가집니다.

2단계 초기화는 클래스 계층 구조 안에서 각 클래스에 완전한 유연성을 부여하면서도,
안전한 초기화를 보장합니다.
2단계 초기화는 프로퍼티 값이 초기화되기 전에
접근하는 것을 방지하고,
다른 이니셜라이저가
예기치 않게 다른 값을 설정하는 것을 방지합니다.

> Note: Swift의 2단계 초기화는 Objective-C의 초기화와 유사합니다.
> 주요 차이점은 Objective-C에서는 1단계 동안
> 모든 프로퍼티에 0 또는 null 값(`0` 또는 `nil`)을 할당합니다.
> Swift는 커스텀 초기값을 설정할 수 있고,
> `0`이나 `nil`이 유효한 초기값이 아닌
> 타입도 처리할 수 있다는 점에서 더 유연합니다.

Swift 컴파일러는 2단계 초기화가 오류없이 완료되도록
다음 네 가지 안전성 검사를 수행합니다:

- **안전성 검사 1**:
  지정 이니셜라이저는 상위 클래스 이니셜라이저를 호출하기 전에
  자신이 정의한 모든 프로퍼티를 초기화해야 합니다.

위에서 언급했듯이
객체의 메모리는 모든 저장 프로퍼티의 초기 상태가 경정된 이후에야
완전히 초기화된 것으로 간주합니다.
이 규칙이 충족하려면 지정 이니셜라이저는
위임을을 넘기기 전에 자신의 모든 프로퍼티를 초기화해야 합니다.

- **안전성 검사 2**:
  지정 이니셜라이저는 상속받은 프로퍼티에 값을 할당하기 전에
  상위 클래스 이니셜라이저를 호출해야 합니다.
  그렇지 않으면 지정 이니셜라이저에 할당한 새로운 값이
  상위 클래스의 초기화 과정 중에 덮어써질 수 있습니다.

- **안전성 검사 3**:
  편의 이니셜라이저는 *모든* 프로퍼티
  (같은 클래스에 정의한 프로퍼티 포함)에
  값을 할당하기 전에 다른 이니셜라이저를 호출해야 합니다.
  그렇지 않으면 편의 이니셜라이저에 할당한 새로운 값은
  자체 클래스의 지정 이니셜라이저에 의해 덮어써질 수 있습니다.

- **안전성 검사 4**:
  초기화의 1단계가 완료되기 전까지는
  어떤 인스턴스 메서드도 호출할 수 없으며,
  인스턴스 프로퍼티를 읽거나
  `self`를 값으로 참조할 수 없습니다.

1단계가 끝나기 전까지는 클래스 인스턴스가 완전히 유효하지 않습니다.
1단계가 끝나고 클래스 인스턴스가 유효한 것으로 판단된 후에만
프로퍼티에 접근할 수 있고 메서드를 호출할 수 있습니다.

다음은 위의 네 가지 검사 규칙에 따른 2단계 초기화의 흐름입니다:

**1단계**

- 클래스의 지정 또는 편의 이니셜라이저가 호출됩니다.
- 해당 클래스 인스턴스의 메모리가 할당됩니다.
  하지만 아직 초기화되지는 않았습니다.
- 지정 이니셜라이저는
  해당 클래스가 정의한 저장 프로퍼티가 모두 값이 있는지 확인합니다.
  이러한 저장 프로퍼티의 메모리를 초기화합니다.
- 상위 클래스의 지정 이니셜라이저를 호출해
  해당 클래스의 프로퍼티를 초기화합니다.
- 이 과정은 상속 체계의 최상위 클래스까지 계속됩니다.
- 최상위 클래스에서
  모든 저장 프로퍼티가 초기화되면,
  인스턴스의 메모리는 완전히 초기화된 것으로 간주되며 1단계가 완료됩니다.

**2단계**

- 상속 체계의 최상위 클래스부터 아래 방향으로 내려가면서
  각 지정 이니셜라이저는 인스턴스를 추가로 구성할 수 있습니다.
  이니셜라이저는 이제 `self`에 접근할 수 있으며
  프로퍼티를 수정하거나 메서드를 호출할 수 있습니다.
- 마지막으로 처음 호출된 편의 이니셜라이저가
  인스턴스를 구성하거나 `self`를 사용할 수 있습니다.

다음은 하위 클래스와 상위 클래스를 대상으로 한 초기화 호출의 1단계를 보여줍니다:

![Two-Phase Initialization 1](../.gitbook/assets/twoPhaseInitialization01_2x~dark.png)

이 예시에서 초기화는 하위 클래스의 편의 이니셜라이저를
호출하며 시작합니다.
이 편의 이니셜라이저는 아직 프로퍼티를 수정할 수 없습니다.
이것은 같은 클래스의 지정 이니셜라이저로 위임합니다.

지정 이니셜라이저는 안전성 검사 1에 따라
하위 클래스의 모든 프로퍼티가 초기화되었는지 확인합니다. 그런 다음 상위 클래스의 지정 이니셜라이저를
호출하여 상위 체계까지 초기화를 계속합니다.

상위 클래스의 지정 이니셜라이저는
상위 클래스의 모든 프로퍼티가 초기화되었는지 확인합니다.
더 이상 초기화할 상위 클래스가 없으므로,
위임은 여기서 끝납니다.

상위 클래스의 모든 프로퍼티가 초기화되면
메모리는 완전히 초기화된 것으로 간주하고 1단계가 완료됩니다.

다음은 같은 초기화 호출에 대한 2단계를 나타냅니다:

![Two-Phase Initialization 2](../.gitbook/assets/twoPhaseInitialization02_2x~dark.png)

상위 클래스의 지정 이니셜라이저는
인스턴스를 추가로 구성할 수 있는 기회를 가집니다
(꼭 하지 않아도 됩니다).

상위 클래스의 지정 이니셜라이저가 완료되면,
하위 클래스의 지정 이니셜라이저도 추가 구성을 수행할 수 있습니다
(역시 선택 사항입니다).

마지막으로 하위 클래스의 지정 이니셜라이저가 완료되면,
처음 호출되었던 편의 이니셜라이저가 최종적으로 인스턴스를
추가로 구성할 수 있습니다.

### 이니셜라이저 상속과 재정의 (Initializer Inheritance and Overriding)

Objective-C의 하위 클래스와 다르게,
Swift 하위 클래스는 기본적으로 상위 클래스의 이니셜라이저를 상속하지 않습니다.
Swift는 단순한 이니셜라이저가
더 특화된 하위 클래스에 상속되어,
완전히 초기화되지 않거나 잘못 초기화된
인스턴스를 생성하는 상황을 방지합니다.

> Note: 상위 클래스의 이니셜라이저는 특정 조건을 만족할 경우에만 상속되지만,
> 안전하고 적절한 경우에만 상속됩니다.
> 자세한 내용은 아래의 [자동 이니셜라이저 상속 (Automatic Initializer Inheritance)](#자동-이니셜라이저-상속-automatic-initializer-inheritance)을 참고바랍니다.

하위 클래스에서
상위 클래스와 동일한 이니셜라이저를 제공하고 싶다면,
하위 클래스 안에 해당 이니셜라이저의 커스텀 구현을 제공해야 합니다.

하위 클래스의 이니셜라이저가 상위 클래스의 *지정* 이니셜라이저와 시그니처가 일치한다면,
이는 해당 지정 이니셜라이저를 재정의하는 것입니다.
따라서 하위 클래스의 이니셜라이저 정의 앞에 `override` 수정자를 작성해야 합니다.
[기본 이니셜라이저 (Default Initializers)](#기본-이니셜라이저-default-initializers)에서 설명했듯이
자동으로 제공된 기본 이니셜라이저를 재정의하는 경우에도 마찬가지입니다.

프로퍼티, 메서드, 서브스크립트를 재정의할 때처럼,
`override` 수정자가 사용되면 Swift는
해당 상위 클래스에 재정의할 수 있는 지정 이니셜라이저가 실제로 존재하는지 확인하고,
재정의하는 이니셜라이저의 매개변수 역시 올바르게 정의되었는지 검증합니다.

> Note: 상위 클래스의 지정 이니셜라이저를 재정의할 때는,
> 하위 클래스에서 편의 이니셜라이저로 구현하더라도 항상 `override` 수정자를 작성해야 합니다.

<!--
  - test: `youHaveToWriteOverrideWhenOverridingADesignatedInitializer`

  ```swifttest
  -> class C {
        init() {}
     }
  -> class D1: C {
        // this is correct
        override init() {}
     }
  -> class D2: C {
        // this isn't correct
        init() {}
     }
  !$ error: overriding declaration requires an 'override' keyword
  !! init() {}
  !! ^
  !! override
  !$ note: overridden declaration is here
  !! init() {}
  !! ^
  ```
-->

<!--
  - test: `youHaveToWriteOverrideEvenWhenOverridingADefaultInitializer`

  ```swifttest
  -> class C {
        var i = 0
     }
  -> class D1: C {
        // this is correct
        override init() {}
     }
  -> class D2: C {
        // this isn't correct
        init() {}
     }
  !$ error: overriding declaration requires an 'override' keyword
  !! init() {}
  !! ^
  !! override
  !$ note: overridden declaration is here
  !! class C {
  !! ^
  ```
-->

반대로 하위 클래스에서 상위 클래스의 *편의* 이니셜라이저와 시그니처가 일치하는 이니셜라이저를 작성하는 경우,
위의 [클래스 타입의 이니셜라이저 위임 (Initializer Delegation for Class Types)](#클래스-타입의-이니셜라이저-위임-initializer-delegation-for-class-types)에서 설명한 규칙에 따라
해당 상위 클래스의 편의 이니셜라이저를 하위 클래스에서 직접 호출할 수는 없습니다.
따라서 하위 클래스는 상위 클래스의 이니셜라이저를 재정의하는 것이 아닙니다.
결과적으로 상위 클래스의 편의 이니셜라이저와 시그니처가 같은 구현을 제공하더라도
`override` 수정자를 작성하지 않습니다.

<!--
  - test: `youDoNotAndCannotWriteOverrideWhenOverridingAConvenienceInitializer`

  ```swifttest
  -> class C {
        var i: Int
        init(someInt: Int) {
           i = someInt
        }
        convenience init() {
           self.init(someInt: 42)
        }
     }
  -> class D1: C {
        // override for designated, so needs the override modifier
        override init(someInt: Int) {
           super.init(someInt: someInt)
        }
        // not technically an override, so doesn't need the override modifier
        convenience init() {
           self.init(someInt: 42)
        }
     }
  -> class D2: C {
        // override for designated, so needs the override modifier
        override init(someInt: Int) {
           super.init(someInt: someInt)
        }
        // this isn't correct - "override" isn't required
        override convenience init() {
           self.init(someInt: 42)
        }
     }
  !$ error: initializer does not override a designated initializer from its superclass
  !! override convenience init() {
  !! ~~~~~~~~             ^
  !$ note: attempt to override convenience initializer here
  !! convenience init() {
  !! ^
  ```
-->

아래 예시는 `Vehicle`이라는 기본 클래스를 정의합니다.
이 기본 클래스는 `numberOfWheels`라는 저장 프로퍼티를 선언하며,
이 프로퍼티는 기본 `Int` 값 `0`을 가집니다.
`numberOfWheels` 프로퍼티는 `description`이라는 연산 프로퍼티에서 사용되어,
해당 차량의 특성을 설명하는 `String` 타입의 설명을 생성합니다:

```swift
class Vehicle {
    var numberOfWheels = 0
    var description: String {
        return "\(numberOfWheels) wheel(s)"
    }
}
```

<!--
  - test: `initializerInheritance`

  ```swifttest
  -> class Vehicle {
        var numberOfWheels = 0
        var description: String {
           return "\(numberOfWheels) wheel(s)"
        }
     }
  ```
-->

`Vehicle` 클래스는 저장 프로퍼티에 대해 기본값을 제공하고,
별도의 커스텀 이니셜라이저는 제공하지 않습니다.
그 결과 [기본 이니셜라이저 (Default Initializers)](#기본-이니셜라이저-default-initializers)에서 설명했듯이
자동으로 기본 이니셜라이저를 얻습니다.
기본 이니셜라이저(사용 가능한 경우)는 항상 클래스의 지정 이니셜라이저로 간주되며,
이를 통해 `numberOfWheels` 값이 `0`인 새로운 `Vehicle` 인스턴스를 생성할 수 있습니다:

```swift
let vehicle = Vehicle()
print("Vehicle: \(vehicle.description)")
// Vehicle: 0 wheel(s)
```

<!--
  - test: `initializerInheritance`

  ```swifttest
  -> let vehicle = Vehicle()
  -> print("Vehicle: \(vehicle.description)")
  </ Vehicle: 0 wheel(s)
  ```
-->

다음 예시는 `Bicycle`이라는 `Vehicle`의 하위 클래스를 정의합니다:

```swift
class Bicycle: Vehicle {
    override init() {
        super.init()
        numberOfWheels = 2
    }
}
```

<!--
  - test: `initializerInheritance`

  ```swifttest
  -> class Bicycle: Vehicle {
        override init() {
           super.init()
           numberOfWheels = 2
        }
     }
  ```
-->

`Bicycle` 하위 클래스는 커스텀 지정 이니셜라이저인 `init()`을 정의합니다.
이 지정 이니셜라이저는 `Bicycle`의 상위 클래스의 지정 이니셜라이저와 시그니처가 일치하므로,
`Bicycle`에서 이 이니셜라이저를 정의할 때는 `override` 수정자를 표기해야 합니다.

`Bicycle`의 `init()` 이니셜라이저는 먼저 `super.init()`을 호출하여,
`Bicycle`의 상위 클래스인 `Vehicle`의 기본 이니셜라이저를 호출합니다.
이 호출을 통해 `Bicycle`이 해당 프로퍼티를 수정하기 전에,
`Vehicle`로부터 상속받은 `numberOfWheels` 프로퍼티가 초기화되도록 보장됩니다.
`super.init()` 호출 이후에는
기존의 `numberOfWheels` 값을 새로운 값인 `2`로 변경합니다.

`Bicycle` 인스턴스를 생성하면,
상속받은 `description` 연산 프로퍼티를 호출하여
`numberOfWheels` 프로퍼티가 어떻게 업데이트되었는지 확인할 수 있습니다:

```swift
let bicycle = Bicycle()
print("Bicycle: \(bicycle.description)")
// Bicycle: 2 wheel(s)
```

<!--
  - test: `initializerInheritance`

  ```swifttest
  -> let bicycle = Bicycle()
  -> print("Bicycle: \(bicycle.description)")
  </ Bicycle: 2 wheel(s)
  ```
-->

하위 클래스의 이니셜라이저가
초기화 과정의 2단계에서 별도의 커스터마이징을 수행하지 않고,
상위 클래스가 동기적이며 인자가 없는 지정 이니셜라이저를 가지고 있다면,
모든 하위 클래스의 저장 프로퍼티에 값을 할당한 후에
`super.init()` 호출을 생략할 수 있습니다.
상위 클래스의 이니셜라이저가 비동기라면,
명시적으로 `await super.init()`을 작성해야 합니다.

아래 예시는 `Vehicle`의 다른 하위 클래스인 `Hoverboard`라는 클래스를 정의합니다.
`Hoverboard` 클래스의 이니셜라이저에서는 `color` 프로퍼티만 설정합니다.
`super.init()`을 명시적으로 호출하는 대신에,
상위 클래스의 이니셜라이저가 암시적으로 호출되어
초기화가 완료됩니다.

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

<!--
  - test: `initializerInheritance`

  ```swifttest
  -> class Hoverboard: Vehicle {
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
-->

`Hoverboard`의 인스턴스는 `Vehicle` 이니셜라이저에서 제공하는
기본 바퀴 수를 사용합니다.

```swift
let hoverboard = Hoverboard(color: "silver")
print("Hoverboard: \(hoverboard.description)")
// Hoverboard: 0 wheel(s) in a beautiful silver
```

<!--
  - test: `initializerInheritance`

  ```swifttest
  -> let hoverboard = Hoverboard(color: "silver")
  -> print("Hoverboard: \(hoverboard.description)")
  </ Hoverboard: 0 wheel(s) in a beautiful silver
  ```
-->

> Note: 하위 클래스는 초기화 중에 상속받은 변수 프로퍼티는 수정할 수 있지만,
> 상수 프로퍼티는 수정할 수 없습니다.

<!--
  - test: `youCantModifyInheritedConstantPropertiesFromASuperclass`

  ```swifttest
  -> class C {
        let constantProperty: Int
        var variableProperty: Int
        init() {
           // this is fine - a class can set its own constant and variable properties during init
           constantProperty = 0
           variableProperty = 0
        }
     }
  -> class D1: C {
        override init() {
           // this is fine - a subclass can set its superclass's variable properties during init
           super.init()
           variableProperty = 0
        }
     }
  -> class D2: C {
        override init() {
           // this is wrong - a subclass can't set its superclass's constant properties during init
           super.init()
           constantProperty = 0
        }
     }
  !$ error: cannot assign to property: 'constantProperty' is a 'let' constant
  !! constantProperty = 0
  !! ^~~~~~~~~~~~~~~~
  !$ note: change 'let' to 'var' to make it mutable
  !! let constantProperty: Int
  !! ^~~
  !! var
  ```
-->

### 자동 이니셜라이저 상속 (Automatic Initializer Inheritance)

위에서 언급했듯이,
하위 클래스는 기본적으로 상위 클래스의 이니셜라이저를 상속받지 않습니다.
그러나 특정 조건이 충족하면 상위 클래스의 이니셜라이저가 자동으로 상속됩니다.
실제로 이것은
대부분의 경우에 이니셜라이저를 재정의할 필요 없이,
안전한 경우 최소한의 코드로 상위 클래스의 이니셜라이저를 상속받을 수 있습니다.

하위 클래스에서 도입한 모든 새로운 프로퍼티에 기본값을 제공하면,
아래의 두 가지 규칙이 적용됩니다:

- **규칙 1**:
  하위 클래스가 자체 지정 이니셜라이저를 정의하지 않으면,
  상위 클래스의 모든 지정 이니셜라이저를 자동으로 상속받습니다.

- **규칙 2**:
  하위 클래스가 상위 클래스의 *모든* 지정 이니셜라이저를
  구현한 경우 ---
  규칙 1에 따라 상속받거나,
  직접 구현함으로써 ---
  상위 클래스의 모든 편의 이니셜라이저도 자동으로 상속됩니다.

이러한 규칙은 하위 클래스가 자체적으로 편의 이니셜라이저를 추가하더라도 여전히 적용됩니다.

> Note: 규칙 2를 만족시키기 위해, 하위 클래스가 상위 클래스의 지정 이니셜라이저를
> 편의 이니셜라이저 형태로 구현할 수도 있습니다.

<!--
  TODO: feedback from Beto is that this note is a little hard to parse.
  Perhaps this point should be left until the later "in action" example,
  where this principle is demonstrated?
-->

<!--
  TODO: There are rare cases in which we automatically insert a call to super.init() for you.
  When is this? Either way, I need to mention it in here.
-->

### 지정 이니셜라이저와 편의 이니셜라이저의 동작 (Designated and Convenience Initializers in Action)

다음 예시는 지정 이니셜라이저, 편의 이니셜라이저,
자동 이니셜라이저 상속이 실제로 어떻게 동작하는지 보여줍니다.
이 예시는 `Food`, `RecipeIngredient`, `ShoppingListItem`이라는
세 개의 클래스로 구성된 계층 구조를 정의하고,
이니셜라이저가 상호작용하는 방식을 보여줍니다.

계층 구조의 기반이 되는 클래스는 `Food`이며,
이는 음식 이름을 캡슐화하기 위한 간단한 클래스입니다.
`Food` 클래스는 `name`이라는 `String` 프로퍼티를 도입하고,
`Food` 인스턴스를 생성하기 위한 두 개의 이니셜라이저를 제공합니다:

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

<!--
  - test: `designatedConvenience`

  ```swifttest
  -> class Food {
        var name: String
        init(name: String) {
           self.name = name
        }
        convenience init() {
           self.init(name: "[Unnamed]")
        }
     }
  ```
-->

아래의 그림은 `Food` 클래스의 이니셜라이저 체인을 나타냅니다:

![Initializer Example 1](../.gitbook/assets/initializersExample01_2x~dark.png)

클래스는 기본 멤버와이즈 이니셜라이저를 제공하지 않기 때문에,
`Food` 클래스는 `name`이라는 하나의 인자를 가지는
지정 이니셜라이저를 제공합니다.
이 이니셜라이저는 특정 이름으로 새로운 `Food` 인스턴스를 생성할 때 사용할 수 있습니다:

```swift
let namedMeat = Food(name: "Bacon")
// namedMeat's name is "Bacon"
```

<!--
  - test: `designatedConvenience`

  ```swifttest
  -> let namedMeat = Food(name: "Bacon")
  /> namedMeat's name is \"\(namedMeat.name)\"
  </ namedMeat's name is "Bacon"
  ```
-->

`Food` 클래스의 `init(name: String)` 이니셜라이저는
새로운 `Food` 인스턴스에 모든 저장 프로퍼티는
완벽하게 초기화 되므로
*지정* 이니셜라이저로 제공됩니다.
`Food` 클래스는 상위 클래스가 없으므로,
`init(name: String)` 이니셜라이저는 초기화를 완료하기 위해
`super.init()`을 호출할 필요가 없습니다.

`Food` 클래스는 인자가 없는 *편의* 이니셜라이저 `init()`도 제공합니다.
`init()` 이니셜라이저는 `[Unnamed]`을 `name` 값으로 사용하여
새로운 음식 이름을 설정하기 위해
`Food` 클래스의 `init(name: String)` 이니셜라이저로 위임합니다:

```swift
let mysteryMeat = Food()
// mysteryMeat's name is "[Unnamed]"
```

<!--
  - test: `designatedConvenience`

  ```swifttest
  -> let mysteryMeat = Food()
  /> mysteryMeat's name is \"\(mysteryMeat.name)\"
  </ mysteryMeat's name is "[Unnamed]"
  ```
-->

계층 구조의 두 번째 클래스는 `Food`의 하위 클래스인 `RecipeIngredient`입니다.
`RecipeIngredient` 클래스는 요리 레시피의 재료를 모델링합니다.
`quantity`라는 `Int` 타입의 프로퍼티를 추가로 도입하고
(이 클래스는 `Food`로 부터 `name` 프로퍼티도 상속받음)
`RecipeIngredient` 인스턴스를 생성하기 위한 두 개의 이니셜라이저를 정의합니다:

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

<!--
  - test: `designatedConvenience`

  ```swifttest
  -> class RecipeIngredient: Food {
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
-->

아래의 그림은 `RecipeIngredient` 클래스의 이니셜라이저 체인을 보여줍니다:

![Initializers Example 2](../.gitbook/assets/initializersExample02_2x~dark.png)

`RecipeIngredient` 클래스는 하나의 지정 이니셜라이저
`init(name: String, quantity: Int)`를 가지고 있으며,
이를 통해 새로운 `RecipeIngredient` 인스턴스의 모든 프로퍼티를 초기화할 수 있습니다.
이 이니셜라이저는 먼저 전달받은 `quantity` 인자를
`RecipeIngredient` 클래스에서 새롭게 추가한
`quantity` 프로퍼티에 할당합니다.
그런 후에 `Food` 클래스의 `init(name: String)` 이니셜라이저로
위임합니다.
이 과정은 위의 [2단계 초기화 (Two-Phase Initialization)](#2단계-초기화-two-phase-initialization)에서 설명한
안전성 검사 1에 충족합니다.

`RecipeIngredient`는 편의 이니셜라이저인 `init(name: String)`도 정의하고 있으며,
이것은 이름만으로 `RecipeIngredient` 인스턴스를 생성하기 위해 사용합니다.
이 편의 이니셜라이저는 수량 없이 `RecipeIngredient` 인스턴스를 생성할 때,
수량을 `1`로 설정합니다.
이러한 편의 이니셜라이저의 정의는
여러 개의 단일 수량 `RecipeIngredient` 인스턴스를
더 빠르고 간편하게 생성할 수 있으며,
코드 중복을 피할 수 있습니다.
이 편의 이니셜라이저는 클래스의 지정 이니셜라이저에
`quantity` 값으로 `1`을 전달하여 위임합니다.

`RecipeIngredient`가 `init(name: String)`을 편의 이니셜라이저로 제공하지만,
`Food`의 `init(name: String)` *지정* 이니셜라이저와 같은 매개변수를 가지고 있습니다.
이 편의 이니셜라이저는 상위 클래스의 지정 이니셜라이저를 재정의하기 때문에,
`override` 수정자를 붙여줘야 합니다
(자세한 내용은 [이니셜라이저 상속과 재정의 (Initializer Inheritance and Overriding)](#이니셜라이저-상속과-재정의-initializer-inheritance-and-overriding)에 설명되어 있습니다).

`RecipeIngredient`는
편의 이니셜라이저로 `init(name: String)`을 제공하지만,
`RecipeIngredient`는 상위 클래스의 지정 이니셜라이저의
모든 구현을 제공했습니다.
따라서 `RecipeIngredient`는 자동으로
모든 상위 클래스의 편의 이니셜라이저를 상속받습니다.

이 예시에서 `RecipeIngredient`의 상위 클래스인 `Food`는
`init()`이라는 단일 편의 이니셜라이저를 가지고 있습니다.
따라서 이 이니셜라이저는 `RecipeIngredient`에 의해 상속됩니다.
상속된 `init()`은 `Food`에서와 동일하게 동작하지만,
`Food`의 `init(name: String)`이 아닌
`RecipeIngredient`의 `init(name: String)`으로 위임됩니다.

이 세 가지 이니셜라이저 모두 새로운 `RecipeIngredient` 인스턴스를 생성할 수 있습니다:

```swift
let oneMysteryItem = RecipeIngredient()
let oneBacon = RecipeIngredient(name: "Bacon")
let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)
```

<!--
  - test: `designatedConvenience`

  ```swifttest
  -> let oneMysteryItem = RecipeIngredient()
  -> let oneBacon = RecipeIngredient(name: "Bacon")
  -> let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)
  ```
-->

계층 구조에서 세 번째이자 마지막 클래스는
`ShoppingListItem`이라는 `RecipeIngredient`의 하위 클래스입니다.
`ShoppingListItem` 클래스는 쇼핑 목록에 표시되는 요리 재료를 모델링합니다.

쇼핑 목록에 모든 아이템은 "미구매"로 시작합니다.
이것을 표현하기 위해
`ShoppingListItem`은 기본값이 `false`인
`purchased`라는 Boolean 프로퍼티를 도입합니다.
`ShoppingListItem`은 `ShoppingListItem` 인스턴스의 설명을 제공하는
`description`이라는 연산 프로퍼티도 추가합니다:

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

<!--
  - test: `designatedConvenience`

  ```swifttest
  -> class ShoppingListItem: RecipeIngredient {
        var purchased = false
        var description: String {
           var output = "\(quantity) x \(name)"
           output += purchased ? " ✔" : " ✘"
           return output
        }
     }
  ```
-->

> Note: 쇼핑 목록의 항목들은(여기서 모델링된 방식에 따르면) 항상 미구매로 시작하기 때문에,
> `ShoppingListItem`은 `purchased`에 대한 초기값을
> 제공하기 위한 이니셜라이저를 정의하지 않습니다.

`ShoppingListItem`은 자신이 새롭게 도입한 모든 프로퍼티에 대해 기본값을 제공하고,
별도의 이니셜라이저를 정의하지 않으므로,
자동으로 상위 클래스의 *모든* 지정 이니셜라이저와 편의 이니셜라이저를
상속받습니다.

아래의 그림은 세 클래스에 대한 모든 이니셜라이저 체인을 나타냅니다:

![Initializers Example 3](../.gitbook/assets/initializersExample03_2x~dark.png)

상속받은 세 가지 이니셜라이저를 모두 사용하여
새로운 `ShoppingListItem` 인스턴스를 생성할 수 있습니다:

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

<!--
  - test: `designatedConvenience`

  ```swifttest
  -> var breakfastList = [
        ShoppingListItem(),
        ShoppingListItem(name: "Bacon"),
        ShoppingListItem(name: "Eggs", quantity: 6),
     ]
  -> breakfastList[0].name = "Orange juice"
  -> breakfastList[0].purchased = true
  -> for item in breakfastList {
        print(item.description)
     }
  </ 1 x Orange juice ✔
  </ 1 x Bacon ✘
  </ 6 x Eggs ✘
  ```
-->

여기서 `breakfastList`라는 새로운 배열은
세 개의 새로운 `ShoppingListItem` 인스턴스를 포함하는 배열 리터럴로 생성됩니다.
배열의 타입은 `[ShoppingListItem]`으로 추론됩니다.
배열이 생성된 후에
배열의 첫 번째 `ShoppingListItem`의 이름을
`"[Unnamed]"`에서 `"Orange juice"`로 변경하고,
구매된 것으로 표시합니다.
배열의 각 항목에 대한 설명을 출력하면,
기본 상태가 예상대로 설정되었음을 확인할 수 있습니다.

<!--
  TODO: talk about the general factory initializer pattern,
  and how Swift's approach to initialization removes the need for most factories.
-->

<!--
  NOTE: We import some Obj-C-imported factory initializers as init() -> MyType,
  but you can't currently write these in Swift yourself.
  After conferring with Doug, I've decided not to include these in the Guide
  if you can't write them yourself in pure Swift.
-->

<!--
  TODO: Feedback from Beto is that it would be useful to indicate the flow
  through these inherited initializers.
-->

## 실패 가능한 이니셜라이저 (Failable Initializers)

클래스, 구조체, 열거형을 정의할 때,
이니셜라이저가 실패할 수 있도록 만드는 것이 유용할 수 있습니다.
이 실패는 유효하지 않은 초기화 매개변수 값,
필수 외부 리소스의 부재
또는 초기화를 성공적으로 완료하지 못하게 하는 다른 조건에 의해 발생할 수 있습니다.

실패 가능한 초기화 조건에 대응하기 위해,
클래스, 구조체, 열거형 정의 내에서
하나 이상의 실패 가능한 이니셜라이저를 정의할 수 있습니다.
실패 가능한 이니셜라이저는
`init` 키워드 뒤에 물음표(`init?`)를 표기하여 작성합니다.

> Note: 동일한 매개변수 타입과 이름을 가지는
> 실패 가능한 이니셜라이저와 실패하지 않는 이니셜라이저를 정의할 수 없습니다.

<!--
  - test: `failableAndNonFailableInitializersCannotMatch`

  ```swifttest
  -> struct S {
        let s: String
        init(s: String) { self.s = s }
        init?(s: String) { self.s = s }
     }
  !$ error: invalid redeclaration of 'init(s:)'
  !!            init?(s: String) { self.s = s }
  !!            ^
  !$ note: 'init(s:)' previously declared here
  !!            init(s: String) { self.s = s }
  !!            ^
  ```
-->

실패 가능한 이니셜라이저는 초기화하려는 타입의 *옵셔널* 값을 생성합니다.
초기화 중 실패를 유동하고 싶을 때는
실패 가능한 이니셜라이저 내에 `return nil`을 작성하여 나타냅니다.

> Note: 엄밀히 말하면 이니셜라이저는 값을 반환하지 않습니다.
> 오히려 이니셜라이저의 역할은 초기화가 끝날 때까지
> `self`가 완전하고 올바르게 초기화되도록 보장하는 것입니다.
> 초기화 실패를 나타내기 위해 `return nil`을 작성하지만,
> 초기화 성공을 나타내기 위해 `return` 키워드를 사용하지는 않습니다.

예를 들어 숫자 타입 변환에서 실패 가능한 이니셜라이저가 사용됩니다.
숫자 타입 간 변환 시 값이 정확하게 유지되는지 확인하려면,
`init(exactly:)` 이니셜라이저를 사용합니다.
변환으로 인해 값이 유지되지 못할 경우,
이니셜라이저는 실패합니다.

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

<!--
  - test: `failableInitializers`

  ```swifttest
  -> let wholeNumber: Double = 12345.0
  -> let pi = 3.14159

  -> if let valueMaintained = Int(exactly: wholeNumber) {
         print("\(wholeNumber) conversion to Int maintains value of \(valueMaintained)")
     }
  <- 12345.0 conversion to Int maintains value of 12345

  -> let valueChanged = Int(exactly: pi)
  // valueChanged is of type Int?, not Int

  -> if valueChanged == nil {
         print("\(pi) conversion to Int doesn't maintain value")
     }
  <- 3.14159 conversion to Int doesn't maintain value
  ```
-->

아래 예시는 `Animal`이라는 구조체를 정의하고,
`species`라는 `String` 상수 프로퍼티를 가지고 있습니다.
`Animal` 구조체는 `species`라는 하나의 매개변수를 가진
실패 가능한 이니셜라이저도 정의합니다.
이 이니셜라이저는 전달된 `species` 값이 빈 문자열인지 확인합니다.
빈 문자열이면, 초기화는 실패됩니다.
반대로 `species` 프로퍼티의 값은 설정되고 초기화는 성공합니다:

```swift
struct Animal {
    let species: String
    init?(species: String) {
        if species.isEmpty { return nil }
        self.species = species
    }
}
```

<!--
  - test: `failableInitializers`

  ```swifttest
  -> struct Animal {
        let species: String
        init?(species: String) {
           if species.isEmpty { return nil }
           self.species = species
        }
     }
  ```
-->

이 실패 가능한 이니셜라이저를 사용하여 새로운 `Animal` 인스턴스를 초기화하고,
초기화가 성공했는지 확인할 수 있습니다:

```swift
let someCreature = Animal(species: "Giraffe")
// someCreature is of type Animal?, not Animal

if let giraffe = someCreature {
    print("An animal was initialized with a species of \(giraffe.species)")
}
// Prints "An animal was initialized with a species of Giraffe"
```

<!--
  - test: `failableInitializers`

  ```swifttest
  -> let someCreature = Animal(species: "Giraffe")
  // someCreature is of type Animal?, not Animal

  -> if let giraffe = someCreature {
        print("An animal was initialized with a species of \(giraffe.species)")
     }
  <- An animal was initialized with a species of Giraffe
  ```
-->

실패 가능한 이니셜라이저의 `species` 매개변수에 빈 문자열 값을 전달하면,
이니셜라이저는 초기화가 실패합니다:

```swift
let anonymousCreature = Animal(species: "")
// anonymousCreature is of type Animal?, not Animal

if anonymousCreature == nil {
    print("The anonymous creature could not be initialized")
}
// Prints "The anonymous creature could not be initialized"
```

<!--
  - test: `failableInitializers`

  ```swifttest
  -> let anonymousCreature = Animal(species: "")
  // anonymousCreature is of type Animal?, not Animal

  -> if anonymousCreature == nil {
        print("The anonymous creature couldn't be initialized")
     }
  <- The anonymous creature couldn't be initialized
  ```
-->

> Note: 빈 문자열 값(`"Giraffe"`가 아닌 `""`)을 확인하는 것은
> *옵셔널* `String` 값이 없음을 나타내는 `nil`을 확인하는 것과는 다릅니다.
> 위의 예시에서 빈 문자열(`""`)은 유효한 옵셔널이 아닌 `String`입니다.
> 그러나 `species` 프로퍼티의 값으로 빈 문자열을 갖는 것은
> 동물에 대해 적절하지 않습니다.
> 이러한 제약을 모델링하기 위해
> 실패 가능한 이니셜라이저는 빈 문자열이 발견되면 초기화에 실패합니다.

### 열거형의 실패 가능한 이니셜라이저 (Failable Initializers for Enumerations)

하나 이상의 매개변수를 기반으로
적절한 열거형 케이스를 선택하기 위해 실패 가능한 이니셜라이저를 사용할 수 있습니다.
이 이니셜라이저는 제공된 매개변수가 적절한 열거형 케이스와 일치하지 않으면
실패할 수 있습니다.

아래의 예시는 `TemperatureUnit`이라는 열거형을 정의하며,
가능한 세 가지 상태(`kelvin`, `celsius`, `fahrenheit`)를 가집니다.
실패 가능한 이니셜라이저는 온도 단위를 나타내는
`Character` 값에 대한 적절한 열거형 케이스를 찾습니다:

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

<!--
  - test: `failableInitializers`

  ```swifttest
  -> enum TemperatureUnit {
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
-->

이 실패 가능한 이니셜라이저를 사용하여
세 가지 가능한 상태 중 적절한 열거형 케이스를 선택하고,
매개변수가 어떠한 상태도 일치하지 않으면
초기화를 실패하게 만들 수 있습니다:

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

<!--
  - test: `failableInitializers`

  ```swifttest
  -> let fahrenheitUnit = TemperatureUnit(symbol: "F")
  -> if fahrenheitUnit != nil {
        print("This is a defined temperature unit, so initialization succeeded.")
     }
  <- This is a defined temperature unit, so initialization succeeded.

  -> let unknownUnit = TemperatureUnit(symbol: "X")
  -> if unknownUnit == nil {
        print("This isn't a defined temperature unit, so initialization failed.")
     }
  <- This isn't a defined temperature unit, so initialization failed.
  ```
-->

### 원시값을 가진 열거형의 실패 가능한 이니셜라이저 (Failable Initializers for Enumerations with Raw Values)

원시값을 가진 열거형은
적절한 원시값 타입의 `rawValue`라는 매개변수를 받아,
일치하는 값을 찾으면 일치하는 열거형 케이스를 선택하거나,
일치하는 값이 없으면 초기화 실패를 나타내기 위해
자동으로 실패 가능한 이니셜라이저를 갖습니다.

위의 `TemperatureUnit` 예시를
`Character` 타입의 원시값을 사용하도록 다시 작성하면,
`init?(rawValue:)` 이니셜라이저를 활용할 수 있습니다:

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

<!--
  - test: `failableInitializersForEnumerations`

  ```swifttest
  -> enum TemperatureUnit: Character {
        case kelvin = "K", celsius = "C", fahrenheit = "F"
     }

  -> let fahrenheitUnit = TemperatureUnit(rawValue: "F")
  -> if fahrenheitUnit != nil {
        print("This is a defined temperature unit, so initialization succeeded.")
     }
  <- This is a defined temperature unit, so initialization succeeded.

  -> let unknownUnit = TemperatureUnit(rawValue: "X")
  -> if unknownUnit == nil {
        print("This isn't a defined temperature unit, so initialization failed.")
     }
  <- This isn't a defined temperature unit, so initialization failed.
  ```
-->

### 초기화 실패의 전파 (Propagation of Initialization Failure)

클래스, 구조체, 열거형의 실패 가능한 이니셜라이저는
같은 클래스, 구조체, 열거형 내의 다른 실패 가능한 이니셜라이저로 위임할 수 있습니다.
마찬가지로 하위 클래스의 실패 가능한 이니셜라이저도 상위 클래스의 실패 가능한 이니셜라이저로 위임할 수 있습니다.

이러한 경우에 위임된 이니셜라이저가 초기화를 실패하면,
전체 초기화 과정은 즉시 실패로 처리되며,
이후의 초기화 코드는 실행되지 않습니다.

<!--
  - test: `delegatingAcrossInAStructurePropagatesInitializationFailureImmediately`

  ```swifttest
  -> struct S {
        init?(string1: String) {
           self.init(string2: string1)
           print("Hello!") // this should never be printed, because initialization has already failed
        }
        init?(string2: String) { return nil }
     }
  -> let s = S(string1: "bing")
  -> assert(s == nil)
  ```
-->

<!--
  - test: `delegatingAcrossInAClassPropagatesInitializationFailureImmediately`

  ```swifttest
  -> class C {
        convenience init?(string1: String) {
           self.init(string2: string1)
           print("Hello!") // this should never be printed, because initialization has already failed
        }
        init?(string2: String) { return nil }
     }
  -> let c = C(string1: "bing")
  -> assert(c == nil)
  ```
-->

<!--
  - test: `delegatingUpInAClassPropagatesInitializationFailureImmediately`

  ```swifttest
  -> class C {
        init?(string1: String) { return nil }
     }
  -> class D: C {
        init?(string2: String) {
           super.init(string1: string2)
           print("Hello!") // this should never be printed, because initialization has already failed
        }
     }
  -> let d = D(string2: "bing")
  -> assert(d == nil)
  ```
-->

> Note: 실패 가능한 이니셜라이저는 실패하지 않는 이니셜라이저로 위임할 수도 있습니다.
> 실패하지 않는 기존 초기화 과정에
> 실패 가능성을 추가해야 할 때 이 방식을 사용합니다.

아래 예시는 `CartItem`이라는 `Product`의 하위 클래스를 정의합니다.
`CartItem` 클래스는 온라인 쇼핑 카트에 있는 상품을 모델링합니다.
`CartItem`은 `quantity`라는 저장 상수 프로퍼티를 도입하고,
적어도 이 프로퍼티는 `1`이상의 값을 가지도록 보장합니다:

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

<!--
  - test: `failableInitializers`

  ```swifttest
  -> class Product {
        let name: String
        init?(name: String) {
           if name.isEmpty { return nil }
           self.name = name
        }
     }
  >> let p = Product(name: "")

  -> class CartItem: Product {
        let quantity: Int
        init?(name: String, quantity: Int) {
           if quantity < 1 { return nil }
           self.quantity = quantity
           super.init(name: name)
        }
     }
  ```
-->

`CartItem`의 실패 가능한 이니셜라이저는
`quantity` 값이 `1`이상인지 확인하는 것으로 시작합니다.
`quantity`가 유효하지 않으면,
전체 초기화 과정은 즉시 실패하고
더 이상 초기화 코드는 실행되지 않습니다.
마찬가지로 `Product`의 실패 가능한 이니셜라이저는
`name` 값을 검사하고,
`name`이 빈 문자열이라면
즉시 초기화 프로세스는 실패합니다.

이름을 가지고 `1`이상의 양을 가진 `CartItem` 인스턴스를 생성하면,
초기화는 성공합니다:

```swift
if let twoSocks = CartItem(name: "sock", quantity: 2) {
    print("Item: \(twoSocks.name), quantity: \(twoSocks.quantity)")
}
// Prints "Item: sock, quantity: 2"
```

<!--
  - test: `failableInitializers`

  ```swifttest
  -> if let twoSocks = CartItem(name: "sock", quantity: 2) {
        print("Item: \(twoSocks.name), quantity: \(twoSocks.quantity)")
     }
  <- Item: sock, quantity: 2
  ```
-->

`quantity` 값이 `0`인 `CartItem` 인스턴스를 생성하려고 하면,
`CartItem` 이니셜라이저는 초기화에 실패합니다:

```swift
if let zeroShirts = CartItem(name: "shirt", quantity: 0) {
    print("Item: \(zeroShirts.name), quantity: \(zeroShirts.quantity)")
} else {
    print("Unable to initialize zero shirts")
}
// Prints "Unable to initialize zero shirts"
```

<!--
  - test: `failableInitializers`

  ```swifttest
  -> if let zeroShirts = CartItem(name: "shirt", quantity: 0) {
        print("Item: \(zeroShirts.name), quantity: \(zeroShirts.quantity)")
     } else {
        print("Unable to initialize zero shirts")
     }
  <- Unable to initialize zero shirts
  ```
-->

마찬가지로 빈 `name` 값을 가진 `CartItem` 인스턴스를 생성하려고 하면,
상위 클래스인 `Product`의 이니셜라이저가 초기화에 실패합니다:

```swift
if let oneUnnamed = CartItem(name: "", quantity: 1) {
    print("Item: \(oneUnnamed.name), quantity: \(oneUnnamed.quantity)")
} else {
    print("Unable to initialize one unnamed product")
}
// Prints "Unable to initialize one unnamed product"
```

<!--
  - test: `failableInitializers`

  ```swifttest
  -> if let oneUnnamed = CartItem(name: "", quantity: 1) {
        print("Item: \(oneUnnamed.name), quantity: \(oneUnnamed.quantity)")
     } else {
        print("Unable to initialize one unnamed product")
     }
  <- Unable to initialize one unnamed product
  ```
-->

### 실패 가능한 이니셜라이저 재정의 (Overriding a Failable Initializer)

다른 이니셜라이저처럼
상위 클래스의 실패 가능한 이니셜라이저를 하위 클래스에서 재정의할 수 있습니다.
또한 하위 클래스에서 상위 클래스의 실패 가능한 이니셜라이저를
*실패하지 않는* 이니셜라이저로 재정의할 수도 있습니다.
이를 통해 상위 클래스의 초기화는 실패할 수 있지만,
하위 클래스에서는 실패하지 않는 초기화를 정의할 수 있습니다.

실패 가능한 상위 클래스 이니셜라이저를 실패하지 않는 하위 클래스 이니셜라이저로 재정의하면,
상위 클래스의 이니셜라이저를 호출할 때에는
반드시 강제 언래핑을 통해 호출해야 합니다.

> Note: 실패 가능한 이니셜라이저는 실패하지 않는 이니셜라이저로 재정의할 수 있지만,
> 그 반대는 불가능합니다.

<!--
  - test: `youCannotOverrideANonFailableInitializerWithAFailableInitializer`

  ```swifttest
  -> class C {
        init() {}
     }
  -> class D: C {
        override init?() {}
     }
  !$ error: failable initializer 'init()' cannot override a non-failable initializer
  !!            override init?() {}
  !!                     ^
  !$ note: non-failable initializer 'init()' overridden here
  !!            init() {}
  !!            ^
  ```
-->

아래의 예시는 `Document`라는 클래스를 정의합니다.
이 클래스는 비어 있지 않은 문자열 값이나 `nil`은 가능하지만,
빈 문자열은 불가능한
`name` 프로퍼티를 가지고 초기화할 수 있는 문서를 모델링합니다:

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

<!--
  - test: `failableInitializers`

  ```swifttest
  -> class Document {
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
-->

다음 예시는 `Document`의 하위 클래스인 `AutomaticallyNamedDocument`를 정의합니다.
`AutomaticallyNamedDocument` 하위 클래스는
`Document`에서 제공하는 지정 이니셜라이저 둘 다 재정의합니다.
이러한 재정의는 `AutomaticallyNamedDocument` 인스턴스가
이름 없이 초기화되거나
빈 문자열이 `init(name:)` 이니셜라이저에 전달되면
초기 `name` 값을 `"[Untitled]"`로 갖도록 보장합니다:

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

<!--
  - test: `failableInitializers`

  ```swifttest
  -> class AutomaticallyNamedDocument: Document {
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
-->

`AutomaticallyNamedDocument`는 상위 클래스의
실패 가능한 `init?(name:)` 이니셜라이저를 실패하지 않는 `init(name:)` 이니셜라이저로 재정의합니다.
`AutomaticallyNamedDocument`는 빈 문자열의 경우를
상위 클래스와 다르게 처리하므로,
이니셜라이저가 실패할 필요가 없으므로
대신 실패하지 않는 이니셜라이저 버전을 제공합니다.

하위 클래스의 실패하지 않는 이니셜라이저 구현에서
상위 클래스의 실패 가능한 이니셜라이저를 호출할 때
강제 언래핑을 사용할 수 있습니다.
예를 들어 아래의 `UntitleDocument` 하위 클래스는 항상 `"[Untitled]"` 이름을 가지고,
초기화 동안에 상위 클래스의
실패 가능한 `init(name:)` 이니셜라이저를 사용합니다.

```swift
class UntitledDocument: Document {
    override init() {
        super.init(name: "[Untitled]")!
    }
}
```

<!--
  - test: `failableInitializers`

  ```swifttest
  -> class UntitledDocument: Document {
        override init() {
           super.init(name: "[Untitled]")!
        }
     }
  ```
-->

이러한 경우에 상위 클래스의 `init(name:)` 이니셜라이저가
빈 문자열을 이름으로 받아 호출 된 경우,
강제 언래핑 작업을 통해 런타임 오류가 발생합니다.
그러나 문자열 리터럴로 호출되기 때문에,
이니셜라이저가 실패하지 않는 것을 알 수 있으므로,
이 경우 런타임 오류가 발생하지 않습니다.

### init! 실패 가능한 이니셜라이저 (The init! Failable Initializer)

일반적으로 `init` 키워드 뒤에 물음표를 배치하여(`init?`)
적절한 타입의 옵셔널 인스턴스를 생성하는
실패 가능한 이니셜라이저를 정의합니다.
또는 적절한 타입의 암시적 언래핑 옵셔널 인스턴스를 생성하는
실패 가능한 이니셜라이저를 정의할 수 있습니다.
물음표 대신에 `init` 키워드 뒤에
느낌표를 위치시키면 됩니다(`init!`).

`init?`에서 `init!`으로 또는 그 반대로 위임할 수 있으며,
`init?`를 `init!`으로 또는 그 반대로 재정의 할 수 있습니다.
`init`에서 `init!`으로 위임할 수도 있지만
그렇게 하면 `init!` 이니셜라이저가 초기화에 실패하면
단정문(assertion)이 발생합니다.

<!--
  - test: `structuresCanDelegateAcrossFromOptionalToIUO`

  ```swifttest
  -> struct S {
        init?(optional: Int) { self.init(iuo: optional) }
        init!(iuo: Int) {}
     }
  ```
-->

<!--
  - test: `structuresCanDelegateAcrossFromIUOToOptional`

  ```swifttest
  -> struct S {
        init!(iuo: Int) { self.init(optional: iuo) }
        init?(optional: Int) {}
     }
  ```
-->

<!--
  - test: `classesCanDelegateAcrossFromOptionalToIUO`

  ```swifttest
  -> class C {
        convenience init?(optional: Int) { self.init(iuo: optional) }
        init!(iuo: Int) {}
     }
  ```
-->

<!--
  - test: `classesCanDelegateAcrossFromIUOToOptional`

  ```swifttest
  -> class C {
        convenience init!(iuo: Int) { self.init(optional: iuo) }
        init?(optional: Int) {}
     }
  ```
-->

<!--
  - test: `classesCanDelegateUpFromOptionalToIUO`

  ```swifttest
  -> class C {
        init!(iuo: Int) {}
     }
  -> class D: C {
        init?(optional: Int) { super.init(iuo: optional) }
     }
  ```
-->

<!--
  - test: `classesCanDelegateUpFromIUOToOptional`

  ```swifttest
  -> class C {
        init?(optional: Int) {}
     }
  -> class D: C {
        init!(iuo: Int) { super.init(optional: iuo) }
     }
  ```
-->

<!--
  - test: `classesCanOverrideOptionalWithIUO`

  ```swifttest
  -> class C {
        init?(i: Int) {}
     }
  -> class D: C {
        override init!(i: Int) { super.init(i: i) }
     }
  ```
-->

<!--
  - test: `classesCanOverrideIUOWithOptional`

  ```swifttest
  -> class C {
        init!(i: Int) {}
     }
  -> class D: C {
        override init?(i: Int) { super.init(i: i) }
     }
  ```
-->

<!--
  - test: `structuresCanDelegateAcrossFromNonFailingToIUO`

  ```swifttest
  -> struct S {
        init(nonFailing: Int) { self.init(iuo: nonFailing) }
        init!(iuo: Int) {}
     }
  ```
-->

<!--
  - test: `classesCanDelegateAcrossFromNonFailingToIUO`

  ```swifttest
  -> class C {
        convenience init(nonFailing: Int) { self.init(iuo: nonFailing) }
        init!(iuo: Int) {}
     }
  ```
-->

<!--
  - test: `classesCanDelegateUpFromNonFailingToIUO`

  ```swifttest
  -> class C {
        init!(iuo: Int) {}
     }
  -> class D: C {
        init(nonFailing: Int) { super.init(iuo: nonFailing) }
     }
  ```
-->

<!--
  - test: `structuresAssertWhenDelegatingAcrossFromNonFailingToNilIUO`

  ```swifttest
  -> struct S {
        init(nonFailing: Int) { self.init(iuo: nonFailing) }
        init!(iuo: Int) { return nil }
     }
  -> let s = S(nonFailing: 42)
  xx assertion
  ```
-->

<!--
  - test: `classesAssertWhenDelegatingAcrossFromNonFailingToNilIUO`

  ```swifttest
  -> class C {
        convenience init(nonFailing: Int) { self.init(iuo: nonFailing) }
        init!(iuo: Int) { return nil }
     }
  -> let c = C(nonFailing: 42)
  xx assertion
  ```
-->

<!--
  - test: `classesAssertWhenDelegatingUpFromNonFailingToNilIUO`

  ```swifttest
  -> class C {
        init!(iuo: Int) { return nil }
     }
  -> class D: C {
        init(nonFailing: Int) { super.init(iuo: nonFailing) }
     }
  -> let d = D(nonFailing: 42)
  xx assertion
  ```
-->

## 필수 이니셜라이저 (Required Initializers)

클래스 이니셜라이저 정의 앞에 `required` 수정자를 작성하여,
클래스를 상속받는 모든 하위 클래스가 해당 이니셜라이저를 구현해야 함을 나타냅니다:

```swift
class SomeClass {
    required init() {
        // initializer implementation goes here
    }
}
```

<!--
  - test: `requiredInitializers`

  ```swifttest
  -> class SomeClass {
        required init() {
           // initializer implementation goes here
        }
     }
  ```
-->

<!--
  - test: `requiredDesignatedInitializersMustBeImplementedBySubclasses`

  ```swifttest
  -> class C {
        required init(i: Int) {}
     }
  -> class D: C {
        init() {}
     }
  !$ error: 'required' initializer 'init(i:)' must be provided by subclass of 'C'
  !! }
  !! ^
  !$ note: 'required' initializer is declared in superclass here
  !!    required init(i: Int) {}
  !!             ^
  ```
-->

<!--
  - test: `requiredConvenienceInitializersMustBeImplementedBySubclasses`

  ```swifttest
  -> class C {
        init() {}
        required convenience init(i: Int) {
           self.init()
        }
     }
  -> class D: C {
        init(s: String) {}
     }
  !$ error: 'required' initializer 'init(i:)' must be provided by subclass of 'C'
  !! }
  !! ^
  !$ note: 'required' initializer is declared in superclass here
  !!    required convenience init(i: Int) {
  !!                         ^
  ```
-->

이니셜라이저 요구사항이 상속 계층의 더 아래 하위 클래스에도 적용된다는 것을 나타내기 위해
필수 이니셜라이저를 하위 클래스에서 구현할 때에도
반드시 이니셜라이저 정의 앞에 `required` 수정자를 작성해야 합니다.
필수 지정 이니셜라이저를 재정의할 때 `override` 수정자를 작성하지 않습니다:

```swift
class SomeSubclass: SomeClass {
    required init() {
        // subclass implementation of the required initializer goes here
    }
}
```

<!--
  - test: `requiredInitializers`

  ```swifttest
  -> class SomeSubclass: SomeClass {
        required init() {
           // subclass implementation of the required initializer goes here
        }
     }
  ```
-->

<!--
  - test: `youCannotWriteOverrideWhenOverridingARequiredDesignatedInitializer`

  ```swifttest
  -> class C {
        required init() {}
     }
  -> class D: C {
        override required init() {}
     }
  !$ warning: 'override' is implied when overriding a required initializer
  !!    override required init() {}
  !! ~~~~~~~~~         ^
  !!-
  !$ note: overridden required initializer is here
  !!    required init() {}
  !!             ^
  ```
-->

> Note: 상속받은 이니셜라이저로 요구사항을 충족할 수 있으면,
> 필수 이니셜라이저를 명시적으로 구현을 제공하지 않아도 됩니다.

<!--
  - test: `youCanSatisfyARequiredDesignatedInitializerWithAnInheritedInitializer`

  ```swifttest
  -> class C {
        var x = 0
        required init(i: Int) {}
     }
  -> class D: C {
        var y = 0
     }
  ```
-->

<!--
  - test: `youCanSatisfyARequiredConvenienceInitializerWithAnInheritedInitializer`

  ```swifttest
  -> class C {
        var x = 0
        init(i: Int) {}
        required convenience init() {
           self.init(i: 42)
        }
     }
  -> class D: C {
        var y = 0
     }
  ```
-->

<!--
  FIXME: This section still doesn't describe why required initializers are useful.
  This is because the reason for their usefulness -
  construction through a metatype of some protocol type with an initializer requirement -
  used to be broken due to
  <rdar://problem/13695680> Constructor requirements in protocols (needed for NSCoding).
  As of early 2015 that bug has been fixed.
  See the corresponding FIXME in the Protocols chapter introduction too.
-->

## 클로저나 함수로 기본 프로퍼티 값 설정 (Setting a Default Property Value with a Closure or Function)

저장 프로퍼티의 기본값에 커스텀나 초기 설정이 필요한 경우,
해당 프로퍼티에 대한 커스텀된 기본값을 제공하기 위해
클로저나 전역 함수를 사용할 수 있습니다.
프로퍼티가 속한 타입의 새로운 인스턴스가 초기화 될 때마다,
클로저나 함수가 호출되고
그것의 반환값은 프로퍼티의 기본값으로 할당됩니다.

이러한 종류의 클로저나 함수는 일반적으로
프로퍼티와 같은 타입의 임시 값을 생성하고
원하는 초기 상태를 나타내도록 해당 값을 조정한 다음
프로퍼티의 기본값으로 사용되기 위해 임시 값을 반환합니다.

다음은 기본 프로퍼티 값을 제공하기 위해
클로저를 사용하는 방법에 대한 예시입니다:

```swift
class SomeClass {
    let someProperty: SomeType = {
        // create a default value for someProperty inside this closure
        // someValue must be of the same type as SomeType
        return someValue
    }()
}
```

<!--
  - test: `defaultPropertyWithClosure`

  ```swifttest
  >> class SomeType {}
  -> class SomeClass {
        let someProperty: SomeType = {
           // create a default value for someProperty inside this closure
           // someValue must be of the same type as SomeType
  >>       let someValue = SomeType()
           return someValue
        }()
     }
  ```
-->

클로저의 중괄호 끝에 빈 소괄호 쌍이 옵니다.
이것은 Swift가 클로저를 즉시 실행하도록 합니다.
만약 소괄호를 생략한다면
클로저의 반환값이 아닌
프로퍼티에 클로저 자체를 할당하려고 합니다.

<!--
  TODO: feedback from Peter is that this is very close to the syntax for
  a computed property that doesn't define a separate getter.
  He's right, and it would be good to provide an additional example -
  perhaps with a stored property that's assigned the result of a function -
  to make the difference more explicit.
-->

> Note: 프로퍼티를 초기화하기 위해 클로저를 사용하면,
> 클로저가 실행될 때
> 다른 인스턴스는 아직 초기화 되지 않았다는 것을 기억해야 합니다.
> 해당 프로퍼티에 기본값이 있더라도
> 클로저 내에서 다른 프로퍼티 값에 접근할 수 없다는 의미입니다.
> 또한 암시적인 `self`를 사용할 수 없으며,
> 인스턴스의 메서드를 호출할 수 없습니다.

아래 예시는 체스 게임을 위한 보드를 모델링하는
`Chessboard`라는 구조체를 정의합니다.
체스는 검은색과 하얀색의 사각형이 번갈아 가며
8 x 8 보드에서 플레이 됩니다.

![Chessboard](../.gitbook/assets/chessBoard_2x~dark.png)

게임보드를 표현하기 위해
`Chessboard` 구조체는 64개의 `Bool` 값 배열인
`boardColors`라는 하나의 프로퍼티를 가집니다.
배열에 `true` 값은 검은색 사각형을 표시하고
`false` 값은 하얀색 사각형을 표시합니다.
배열의 첫 번째 항목은 게임보드의 좌측 상단을 나타내고,
배열의 마지막 항목은 게임보드의 우측 하단을 나타냅니다.

`boardColors` 배열은 색깔 값을 설정하기 위한 클로저로 초기화됩니다:

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

<!--
  - test: `chessboard`

  ```swifttest
  -> struct Chessboard {
        let boardColors: [Bool] = {
           var temporaryBoard: [Bool] = []
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
-->

새로운 `Chessboard` 인스턴스가 생성될 때마다 클로저가 실행되고,
`boardColors`의 기본값은 계산되어 반환됩니다.
위의 예시의 클로저는
`temporaryBoard`라는 임시 배열에
보드의 각 사각형에 대한 적절한 색깔을 계산하고 설정하고
설정이 완료되면
클로저의 반환값으로 임시 배열을 반환합니다.
반환된 배열 값은 `boardColors`에 저장되고
`squareIsBlackAt(row:coloumn:)` 유틸리티 함수로 조회할 수 있습니다:

```swift
let board = Chessboard()
print(board.squareIsBlackAt(row: 0, column: 1))
// Prints "true"
print(board.squareIsBlackAt(row: 7, column: 7))
// Prints "false"
```

<!--
  - test: `chessboard`

  ```swifttest
  -> let board = Chessboard()
  >> assert(board.boardColors == [false, true, false, true, false, true, false, true, true, false, true, false, true, false, true, false, false, true, false, true, false, true, false, true, true, false, true, false, true, false, true, false, false, true, false, true, false, true, false, true, true, false, true, false, true, false, true, false, false, true, false, true, false, true, false, true, true, false, true, false, true, false, true, false])
  -> print(board.squareIsBlackAt(row: 0, column: 1))
  <- true
  -> print(board.squareIsBlackAt(row: 7, column: 7))
  <- false
  ```
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