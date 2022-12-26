# 기본 (The Basics)

Swift는 iOS, macOS, watchOS, 그리고 tvOS app 개발을 위한 새로운 프로그래밍 언어입니다. 그럼에도 불구하고 Swift에 많은 부분이 C와 Objective-C 개발경험과 비슷할 것입니다.

Swift는 정수에 대한 `Int`, 부동 소수점에 대한 `Double` 및 `Float`, 부울 값에 대한 `Bool` 및 텍스트 데이터에 대한 `String`을 포함하여 C와 Objective-C 모든 기초 타입을 자체 버전에 맞게 제공합니다. Swift는 또한 [콜렉션 타입 (Collection Types)]()에서 자세히 다룰 Array, Set, 그리고 Dictionary 인 3개의 기본 콜렉션 타입을 제공합니다.

C와 마찬가지로 Swift는 변수를 식별 가능한 이름으로 값을 저장하고 참조합니다. Swift는 또한 값을 변경할 수 없는 변수를 광범위하게 사용합니다. 이러한 변수를 상수라고 하며 C에서의 상수보다 훨씬 더 강력합니다. 상수는 Swift 전체에서 사용되며 변경할 필요가 없는 값으로 작업할 때 코드를 보다 안전하고 명확하게 만들 수 있습니다.

익숙한 타입 외에도 Swift는 Objective-C에는 없는 튜플 (tuple)이라는 고급 타입이 있습니다. 튜플은 값을 그룹화 하여 생성하거나 전달할 수 있습니다. 튜플을 사용하여 함수의 여러값을 단일 복합 값으로 반환할 수 있습니다.

Swift는 또한 값이 존재하지 않는 상태를 처리하기 위해 옵셔널 (Optional) 타입이 있습니다. 옵셔널은 _"값이 있고 x와 같다"_ 또는 _"값이 없다"_ 를 알려줍니다. 옵셔널을 사용하는 것은 Objective-C에서 포인터로 nil을 사용하는 것과 유사하지만 클래스 (class) 뿐만 아니라 모든 타입에서 사용할 수 있습니다. 옵셔널은 Objective-C에 nil 포인터보다 더 안전하고 광범위하게 사용되는 점이 Swift의 가장 강력한 핵심 기능입니다.

Swift는 _타입-세이프 (type-safe)_ 언어입니다. 타입 세이프란 언어가 값에 대해 타입을 명확하도록 도와주고 코드에서 같이 동작할 수 있도록 해줍니다. 타입 세이프티는 만약 `String`을 요구하는 코드에서 실수로 `Int`로 전달하는 것을 막아줍니다. 마찬가지로 옵셔널이 아닌 `String`을 요구하는 코드에 옵셔널 `String`을 전달하는 것을 막아줍니다. 타입 세이프티는 개발 단계에서 가능한 빠르게 에러를 찾고 고칠 수 있게 도와줍니다.

## 상수와 변수 (Constants and Variables)

상수와 변수는 이름 (`maximumNumberOfLoginAttempts` 또는 `welcomeMessage`)과 특정 타입 (숫자 `10` 또는 문자열 `"Hello"`와 같은 타입)의 값을 연결합니다. _상수 (Constant)_ 의 값은 최초 지정 후 변경이 불가능하지만 _변수 (Variable)_ 은 다른 값으로 변경이 가능합니다.

### 상수와 변수 선언 (Declaring Contants and Variables)

상수와 변수는 사용하기 전에 반드시 선언이 되어야 합니다. 상수는 `let` 키워드와 함께 선언하고 변수는 `var` 키워드와 함께 선언합니다. 다음의 예제는 상수와 변수를 사용하여 어떻게 사용자 로그인 시도 횟수를 추적하는지를 보여줍니다:

```swift {.line-numbers}
let maximumNumberOfLoginAttempts = 10
var currentLoginAttempt = 0
```

이 코드는 아래와 같이 읽을 수 있습니다:

_"`maximumNumberOfLoginAttempts` 인 새로운 상수를 선언하고 `10` 이라는 값을 설정합니다. 그리고 `currentLoginAttempt` 인 새로운 변수를 선언하고 `0` 이라는 값으로 초기화 하였습니다."_

이 예제에서 최대 로그인 시도 횟수는 최대값은 절대 변경되지 않아야 하므로 상수로 선언하였습니다. 현재 로그인 시도 횟수는 로그인 실패 시 값을 증가시켜야 하므로 변수로 선언하였습니다.

여러개의 상수 또는 여러개의 변수를 선언할 때 콤마로 구분하여 한줄로 선언이 가능합니다:

``` swift
var x = 0.0, y = 0.0, z = 0.0
```

> NOTE
> 코드에서 저장한 값이 변경되지 않는다면 항상 `let` 키워드로 상수로 선언해야 합니다. 변수는 오직 값을 저장하고 변경이 필요할 때 선언합니다.

### 타입 명시 (Type Annotations)

상수 또는 변수를 선언할 때 저장할 수 있는 값의 종류를 명확하게 하기위해 _타입 명시 (Type Annotation)_ 를 제공할 수 있습니다. 타입 명시는 상수 또는 변수 이름 뒤에 콜론과 공백 한칸 뒤에 사용할 타입 이름을 적어 사용합니다.

이 예제는 `welcomeMessage` 라는 변수에 `String` 값을 저장할 수 있는 변수를 나타내는 타입 명시를 제공합니다:

``` swift
var welcomeMessage: String
```

위 코드에서 선언에 있는 콜론은 "...의 타입은..." 을 의미하므로 아래와 같이 읽을 수 있습니다:

_"선언한 변수는 `welcomeMessage` 라고 하며 이것의 타입은 `String` 입니다."_

"...의 타입은 `String` 입니다." 라는 의미는 "어떤 `String` 값은 저장 가능합니다." 입니다. 저장할 수 있는 "어떠한 타입" (또는 "어떠한 종류") 라고 생각하십시오.

`welcomeMessage` 변수는 아무런 오류 없이 어떠한 문자열 값을 지정할 수 있습니다:

``` swift
welcomeMessage = "Hello"
```

같은 타입에 여러개의 연관된 변수를 콤마로 변수 이름을 구분하고 마지막 변수 이름 뒤에 하나의 타입 명시를 통해 한줄로 선언할 수 있습니다:

```swift
var red, green, blue: Double
```

> NOTE
> 실제로 타입 명시가 필요한 경우는 드뭅니다. 상수 또는 변수를 선언할 때 초기값을 지정하면 Swift는 [타입 세이프티와 타입 추론 (Type Safety and Type Inference)]() 에서 나와있는대로 해당 상수 또는 변수에 사용될 타입을 거의 항상 유추할 수 있습니다. 위의 `welcomeMessage` 예제에서 초기값을 지정하지 않았으므로 `welcomeMessage` 변수의 타입은 초기값에서 유추되지 않고 타입을 명시 하였습니다.

### 상수와 변수의 이름 (Naming Constants and Variables)

상수와 변수 이름은 유니코드 (Unicode) 문자를 포함하여 대부분의 문자를 포함할 수 있습니다:

```swift
let n = 3.14159
let 你好 = "你好世界"
let 🐶🐮 = "dogcow"
```

상수와 변수 이름은 공백, 수학적 기호, 화살표, 개인전용 유니코드 스칼라 값, 또는 선과 박스를 그리는 문자를 포함할 수 없습니다. 숫자는 이름의 다른곳에는 포함될 수 있지만 숫자로 시작하는 이름은 선언할 수 없습니다.

특정 타입으로 상수 또는 변수를 선언하면 동일한 이름으로 다시 선언하거나 다른 타입의 값을 저장하도록 변경하여 선언할 수 없습니다. 상수를 변수로 바꾸거나 변수를 상수로 바꿀 수도 없습니다.

> NOTE
> Swift 키워드와 동일한 이름의 상수 또는 변수를 제공해야 한다면 이름을 백틱 (`)으로 묶어야 합니다. 그러나 선택의 여지가 없을때까지는 키워드를 이름으로 사용하지 말아야 합니다.

동일한 타입의 다른 값으로 이미 선언된 변수에 값을 변경할 수 있습니다. 예제에서 `friendlyWelcome` 값은 `"Hello!"` 에서 `"Bonjour!"` 변경됩니다:

```swift
var friendlyWelcome = "Hello!"
friendlyWelcome = "Bonjour!"
// friendlyWelcome is now "Bonjour!"
```

변수와 달리 상수 값은 지정된 이후에는 변경할 수 없습니다. 값을 변경하려고 하면 코드가 컴파일 될 때 오류가 발생합니다:

```swift
let languageName = "Swift"
languageName = "Swift++"
// This is a compile-time error: languageName cannot be changed.
```

### 상수와 변수 출력 (Printing Constants and Variables)

`print(_:separator:terminator:)` 함수로 상수 또는 변수의 현재 값을 출력할 수 있습니다:

```swift
print(friendlyWelcome)
// Prints "Bonjour!"
```

`print(_:separator:terminator:)` 함수는 하나 또는 그 이상의 값을 적절하게 출력하는 전역 함수입니다. 예를 들어 Xcode에서 `print(_:separator:terminator:)` 함수는 Xcode "콘솔 (console)" 창에 결과를 출력합니다. `separator` 와 `terminator` 파라미터는 기본 값을 가지고 있으므로 함수를 호출할 때 생략할 수 있습니다. 기본적으로 이 함수는 줄바꿈을 출력하고 종료됩니다. 줄바꿈 없이 값을 출력하려면 예를 들어 `print(someValue, terminator: "")` 와 같이 `terminator` 에 빈 문자열을 넘겨주면 됩니다. 파라미터 기본값에 대한 자세한 내용은 [파라미터 기본 값 (Default Parameter Values)]() 을 참고 바랍니다.

Swift는 긴 문자열에 상수 또는 변수의 이름을 포함하여 Swift가 상수 또는 변수의 현재 값으로 대체하기 위해 _문자열 삽입 (String interpolation)_ 을 사용합니다. 이름을 소괄호로 감싸고 여는 소괄호 전에 백슬래시를 추가해야 합니다:

```swift
print("The current value of friendlyWelcome is \(friendlyWelcome)")
// Prints "The current value of friendlyWelcome is Bonjour!"
```

> NOTE
> 문자열 삽입에서 사용할 수 있는 모든 옵션은 [문자열 삽입 (String interpolation)]() 에 자세히 설명되어 있습니다.

## 주석 (Comments)

코드에서 설명 또는 기록을 위해 실행되지 않는 문자를 추가할 땐 주석 (Comments)을 사용합니다. 주석은 코드가 컴파일 될 때 Swift 컴파일러에 의해 무시됩니다.

Swift에서 주석은 C에서 주석을 다는 방법과 유사합니다. 한줄 주석은 두개의 슬래시 (//)로 시작합니다:

```swift
// This is a comment.
```

여러줄 주석은 슬래시 뒤에 애스터리스크 (/*) 로 시작하고 애스터리스크 뒤에 슬래시 (\*/)로 끝납니다:

```swift
/* This is also a comment
but is written over multiple lines. */
```

C에서 여러줄 주석과 다르게 Swift는 여러줄 주석 안에 다른 여러줄 주석을 통해 중첩시킬 수 있습니다. 여러줄 주석 블럭을 시작한 다음 첫번째 블럭 내에서 두번째 여러줄 주석을 시작하여 중첩된 주석을 작성합니다. 그런다음 두번째 블럭을 닫은 다음 첫번째 블럭을 닫습니다:

```swift
/* This is the start of the first multiline comment.
 /* This is the second, nested multiline comment. */
This is the end of the first multiline comment. */
```

중첩된 여러줄 주석을 사용하면 코드에 이미 여러줄 주석이 포함되어 있어도 큰 코드 블럭을 빠르고 쉽게 주석 처리할 수 있습니다.

## 세미콜론 (Semicolons)

많은 다른 언어와 다르게 Swift는 코드의 각 구문 후에 세미콜론 (;)은 필수조건이 아닙니다. 그러나 여러 구문을 한줄로 작성할 경우 세미콜론은 _필수로 작성되어야 합니다_:

```swift
let cat = "🐱"; print(cat)
// Prints "🐱"
```

## 정수 (Integers)

_정수 (Integers)_ 는 `42` 와 `-23` 과 같은 분수가 아닌 전체 숫자입니다. 정수는 _부호가 있는 정수 (signed) (양수, 0, 또는 음수)_ 또는 _부호가 없는 정수 (unsigned) (양수 또는 0)_ 이 있습니다.

Swift는 8, 16, 32, 그리고 64 비트 형태의 부호가 있는 정수와 부호가 없는 정수를 지원합니다. 이러한 정수는 8-bit 부호가 없는 정수는 `UInt8` 그리고 32-bit 부호가 있는 정수는 `Int32`와 같이 C와 비슷한 네이밍 형태를 가집니다. Swift의 모든 타입과 마찬가지로 정수 타입은 대문자로 시작합니다.

### 정수 범위 (Integer Bounds)

각 정수 타입의 `min` 과 `max` 프로퍼티를 통해 각 정수 타입의 최소값과 최대값을 가져올 수 있습니다:

```swift
let minValue = UInt8.min  // minValue is equal to 0, and is of type UInt8
let maxValue = UInt8.max  // maxValue is equal to 255, and is of type UInt8
```

이러한 프로퍼티의 값은 적절한 크기의 숫자 타입 (위 예에서 `UInt8`)이므로 동일한 타입의 다른 값과 함께 표현식에 사용될 수 있습니다.

### Int

대부분의 경우 코드에서 사용할 정수의 특정 사이즈를 결정할 필요는 없습니다. Swift는 현재 플랫폼의 네이티브 사이즈와 같은 `Int` 인 정수 타입을 제공합니다:

* 32-bit 플랫폼에서 `Int` 는 `Int32` 와 같은 크기를 가짐
* 64-bit 플랫폼에서 `Int` 는 `Int64` 와 같은 크기를 가짐

특정 크기의 정수로 작업해야 하는 경우가 아니라면 항상 코드의 정수 값을 사용할 때 `Int` 를 사용하십시오. 이것은 코드 일관성과 상호 운용성을 지원합니다. 32-bit 플랫폼에서도 `Int` 는 -2,147,483,648 과 2,147,483,647 사이의 값을 저장할 수 있으며 일반적인 사용성에 문제가 없습니다.

### UInt

Swift는 또한 현재 플랫폼의 네이티브 사이즈와 같은 `UInt` 인 정수 타입을 제공합니다:

* 32-bit 플랫폼에서 `UInt` 는 `UInt32` 와 같은 크기를 가짐
* 64-bit 플랫폼에서 `UInt` 는 `UInt64` 와 같은 크기를 가짐

> NOTE
> `UInt` 는 플랫폼의 네이티브 사이즈와 같은 크기의 부호없는 정수 타입이 필요한 경우에만 사용하십시오. 저장될 값이 음수가 아니어도 `Int` 를 더 선호합니다. 정수값에 `Int` 를 일관되게 사용하면 코드 상호 운용성을 지원하고 [타입 세이프티 와 타입 유추 (Type Safety and Type Inference)]()에 설명 된대로 다른 숫자 형식간에 변환 할 필요가 없습니다.

## 부동 소수점 숫자 (Floating-Point Numbers)

_부동 소수점 숫자 (Floating-point numbers)_ 는 3.14159, 0.1, 및 -273.15와 같은 분수 성분을 가진 숫자입니다.

부동 소수점은 정수 타입의 값 범위보다 더 넓은 범위의 표현이 가능하고 `Int` 보다 더 크거나 작은 값 저장이 가능합니다. Swift는 2개의 부호를 가진 부동 소수점 숫자 타입을 제공합니다:

* `Double` 은 64-bit 부동 소수점 숫자를 표기
* `Float` 는 32-bit 부동 소수점 숫자를 표기

> NOTE
> `Double` 은 최소 15자리의 소수점 정확도를 가지고 있는것에 반해 `Float` 는 더 적은 6자리의 정확도를 가집니다. 사용할 적절한 부동 소수점 타입은 코드에서 작업해야하는 값의 특성과 범위에 따라 다릅니다. 두 타입 중에는 `Double` 이 선호됩니다.

## 타입 세이프티와 타입 유추 (Type Safety and Type Inference)

Swift는 _타입-세이프 (type-safe)_ 언어입니다. 타입 세이프 언어를 사용하면 코드가 사용할 수 있는 값의 타입을 명확하게 알 수 있습니다. 코드의 일부에서 `String` 이 필요한 경우 실수로 `Int` 를 전달할 수 없습니다.

Swift는 타입 세이프이기 때문에 코드를 컴파일 할 때 타입 검사를 수행하고 일치하지 않는 타입을 오류로 표시합니다. 이를 통해 개발 단계에서 가능한 빨리 오류를 포착하고 수정할 수 있습니다.

타입 검사는 다른 타입의 값으로 작업할 때 오류를 피하는데 도움이 됩니다. 그러나 이것이 선언하는 모든 상수와 변수의 타입을 지정해야 한다는 것은 아닙니다. 필요한 값의 특정 타입을 지정하지 않으면 Swift는 적절한 타입으로 _타입 유추 (Type Inference)_ 를 사용합니다. 타입 유추를 통해 컴파일러는 코드를 컴파일 할 때 제공한 값을 검사하여 특정 식의 타입을 자동으로 유추할 수 있습니다.

타입 유추 때문에 Swift는 C 또는 Objective-C와 같은 언어보다 타입 선언이 더 적게 요구됩니다. 상수와 변수는 여전히 명시적으로 타입을 지정하지만 타입을 지정하는 많은 동작을 도와줍니다.

타입 유추는 상수 또는 변수에 초기값을 선언할 때 아주 유용합니다. 이것은 종종 선언하는 시점에 상수 또는 변수에 리터럴 값 (literal value) 또는 리터럴 (literal)을 지정하여 수행됩니다. (리터럴 값은 아래 예에서 42 및 3.14159와 같은 소스 코드에 직접 표시되는 값입니다)

예를 들어 어떤 타입인지 선언하지 않고 새로운 상수를 42의 리터럴 값으로 지정하면 Swift는 정수처럼 보이는 숫자로 초기화 했기 때문에 상수가 `Int` 라고 추론합니다:

```swift
let meaningOfLife = 42
// meaningOfLife is inferred to be of type Int
```

반대로 어떤 부동 소수점 리터럴의 타입인지 선언하지 않으면 Swift는 `Double` 이라고 추론합니다:

```swift
let pi = 3.14159
// pi is inferred to be of type Double
```

Swift는 부동 소수점 숫자의 타입을 추론할 때 항상 `Float` 보다 `Double` 을 선택합니다.

표현식에서 정수와 부동소수 리터럴을 결합하면 컨텍스트에서는 `Double` 타입으로 유추합니다:

```swift
let anotherPi = 3 + 0.14159
// anotherPi is also inferred to be of type Double
```

3인 리터럴 값에는 명시적인 타입이 없으므로 추가로 부동 소수점 리터럴이 존재하는 경우 `Double` 이 유추됩니다.

## 숫자 리터럴 (Numberic Literals)

정수 리터럴은 아래와 같이 쓸 수 있습니다:

* 접두사 없는 _10진수_
* 0b 접두사로 _2진수_
* 0o 접두사로 _8진수_
* 0x 접두사로 _16진수_

아래의 예에서 모든 정수 리터럴은 10진수 17의 값을 가집니다:

```swift
let decimalInteger = 17
let binaryInteger = 0b10001       // 17 in binary notation
let octalInteger = 0o21           // 17 in octal notation
let hexadecimalInteger = 0x11     // 17 in hexadecimal notation
```

부동 소수점 리터럴은 10진수 (접두사 없음) 또는 16진수 (접두사 0x) 일 수 있습니다. 소수점 양쪽에 항상 숫자 (또는 16진수)가 있어야 합니다. 10진수는 대문자 또는 소문자 e로 표시되는 _지수_ 를 가질 수도 있습니다. 16진수는 대문자 또는 소문자 p로 표시되는 _지수_ 를 가질 수도 있습니다.

지수가 exp인 10진수는 기본 숫자에 10<sup>exp</sup> 가 곱해집니다:

* 1.25e2 는 1.25 x 10<sup>2</sup>, 또는 125.0
* 1.25e-2 는 1.25 x 10<sup>-2</sup>, 또는 0.0125

지수가 exp인 16진수는 기본 숫자에 2<sup>exp</sup> 가 곱해집니다:

* 0xFp2 는 15 x 2<sup>2</sup>, 또는 60.0
* 0xFp-2 는 15 x 2<sup>-2</sup>, 또는 3.75

아래의 예에서 모든 부동 소수점 리터럴은 10진수 12.1875를 가집니다:

```swift
let decimalDouble = 12.1875
let exponentDouble = 1.21875e1
let hexadecimalDouble = 0xC.3p0
```

숫자 리터럴은 읽기 쉽게 만드는 추가 포맷을 포함할 수 있습니다. 정수와 부동 소수점 모두 추가 0으로 채워질 수 있으며 가독성을 돕기 위해 밑줄을 포함할 수 있습니다. 어떤 형식도 리터럴의 기본 값에 영향을 주지 않습니다:

```swift
let paddedDouble = 000123.456
let oneMillion = 1_000_000
let justOverOneMillion = 1_000_000.000_000_1
```

## 숫자 타입 변환 (Numeric Type Conversion)

음수를 사용하지 않더라도 코드에서 상수와 변수가 정수로 사용이 되면 `Int` 타입을 사용합니다. 일반적인 상황에서 기본 정수 타입을 사용하는 것은 정수 값의 상수와 변수가 코드에서 즉시 상호 운용 가능하며 정수 리터럴 값의 유추 된 타입이 일치한다는 것을 의미합니다.

외부 소스에서 명시적으로 크기가 지정된 데이터 또는 성능, 메모리 사용, 또는 다른 성능 최적화를 위해 특별히 필요한 경우에만 다른 정수 타입을 사용하십시오. 이러한 상황에서 명시적으로 크기의 타입을 사용하면 실수로 인한 값 초과를 포착하고 사용중인 데이터의 특성을 알 수 있습니다.

### 정수 변환 (Integer Conversion)

정수를 저장할 수 있는 상수 또는 변수의 숫자 범위는 각 숫자 타입에 따라 다릅니다. `Int8` 상수 또는 변수는 -128 과 127 사이의 숫자를 저장할 수 있는 반면에 `UInt8` 상수 또는 변수는 0과 255 사이의 숫자를 저장할 수 있습니다. 크기가 지정된 정수 타입의 상수 또는 변수에 맞지않는 숫자는 컴파일 시에 오류가 발생합니다:

```swift
let cannotBeNegative: UInt8 = -1
// UInt8 cannot store negative numbers, and so this will report an error
let tooBig: Int8 = Int8.max + 1
// Int8 cannot store a number larger than its maximum value,
// and so this will also report an error
```

각 숫자 타입은 다른 범위의 값을 저장할 수 있으므로 숫자 타입 변환을 각 타입별로 선택해야 합니다. 이 옵트인 (opt-in) 방식은 숨겨진 변환 오류를 방지하고 코드에서 타입 변환 의도를 명시적으로 만드는데 도움을 줍니다.

특정 숫자 타입을 다른 숫자 타입으로 변환하려면 기존값으로 원하는 타입의 새 숫자를 초기화합니다. 아래의 예제에서 상수 `twoThousand` 는 `UInt16` 타입인 반면에 상수 `one` 은 `UInt8` 타입입니다. 2 상수는 타입이 다르기 때문에 직접 더할 수 없습니다. 대신 이 예제에서 `UInt16(one)` 을 호출하여 `one` 의 값을 새로운 `UInt16` 으로 초기화하면 기존 값 대신에 새로운 값을 사용합니다:

```swift
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + UInt16(one)
```

양쪽의 타입이 `UInt16` 이므로 덧셈은 이제 제대로 동작합니다. 출력 상수 (`twoThousandAndOne`)은 두 `UInt16` 값을 더하므로 `UInt16` 타입으로 유추됩니다.

`SomeType (ofInitialValue)` 는 Swift 타입의 초기화를 호출하고 초기화 값을 전달하는 기본적인 방법입니다. 이전에 `UInt16` 은 `UInt8` 값을 허용하는 초기화가 있으므로 초기화는 기존 `UInt8` 에서 새 `UInt16` 을 만드는데 사용됩니다. 그러나 `UInt16` 이 제공하는 초기화 타입 이외에는 전달할 수 없습니다. 기존 타입을 확장하여 새로운 타입 (자신이 정의한 새로운 타입)을 받아들이는 초기화를 제공하는 것은 [확장 (Extensions)]() 에서 다룹니다.

### 정수와 부동 소수점 변환 (Integer and Floating-Point Conversion)

정수와 부동 소수점 숫자 타입의 변환은 명시적으로 변환해야 합니다:

```swift
let three = 3
let pointOneFourOneFiveNine = 0.14159
let pi = Double(three) + pointOneFourOneFiveNine
// pi equals 3.14159, and is inferred to be of type Double
```

여기서 상수 `three` 는 타입 `Double` 의 새로운 값으로 생성하는데 사용되어 덧셈의 양쪽이 동일한 타입입니다. 이 변환이 없으면 덧셈이 허용되지 않습니다.

부동 소수점을 정수로 변환하는 것 또한 명시적으로 변환해야 합니다. 정수 타입은 `Double` 또는 `Float` 로 초기화 될 수 있습니다:

```swift
let integerPi = Int(pi)
// integerPi equals 3, and is inferred to be of type Int
```

부동 소수점 값은 새로운 정수 값으로 초기화할 때 소수점 아래를 버림합니다. 이것은 4.75는 4, 그리고 -3.9는 -3이 된다는 의미입니다.

> NOTE
> 숫자 상수와 변수를 결합하는 규칙은 숫자 리터럴 규칙과 다릅니다. 리터럴 값 3은 숫자 리터럴에 명시적인 타입이 없으므로 리터럴 값 0.14159에 직접 추가할 수 있습니다. 이것의 타입은 컴파일러가 실행되는 시점에서만 유추됩니다.

## 타입 별칭 (Type Aliases)

_타입 별칭 (Type aliases)_ 은 이미 존재하는 타입을 다른 이름으로 정의합니다. 타입 별칭은 `typealias` 키워드를 사용하여 정의할 수 있습니다.

타입 별칭은 외부 소스에서 특정 크기의 데이터로 작업할 때와 같이 상황에 맞는 이름으로 기존 타입을 참조하려는 경우에 유용합니다:

```swift
typealias AudioSample = UInt16
```

타입 별칭을 정의하면 원래 이름을 사용할 수 있는 모든 위치에서 별칭을 사용할 수 있습니다:

```swift
var maxAmplitudeFound = AudioSample.min
// maxAmplitudeFound is now 0
```

여기서 `AudioSample` 은 `UInt16` 의 별칭으로 정의됩니다. 별칭이므로 `AudioSample.min` 에 대한 호출은 실제로 `UInt16.min` 을 호출하며 `maxAmplitudeFound` 변수의 초기값은 0입니다.

## 부울 (Booleans)

Swift는 `Bool` 이라 불리는 기본 _부울 (Boolean) 타입_ 이 있습니다. 부울 값은 오직 `true` 또는 `false` 값만 가지므로 논리적으로 참조됩니다. Swift는 2개의 부울 상수 값인 `true` 와 `false` 를 제공합니다:

```swift
let orangesAreOrange = true
let turnipsAreDelicious = false
```

`orangesAreOrange` 와 `turnipsAreDelicious` 의 타입은 부울 리터럴 값으로 초기화 되어 `Bool` 로 유추되었습니다. 위의 `Int` 와 `Double` 에서와 같이 상수 또는 변수를 초기화 시 `true` 또는 `false` 로 선언하면 상수 또는 변수를 `Bool` 타입으로 선언할 필요가 없습니다. 타입 유추는 Swift 코드가 이미 알고 있는 타입의 다른 값으로 상수 또는 변수를 초기화시에 더 간결하고 읽기 쉽습니다.

부울 값은 `if` 구문과 같은 조건문으로 동작할 때 특히 유용합니다:

```swift
if turnipsAreDelicious {
    print("Mmm, tasty turnips!")
} else {
    print("Eww, turnips are horrible.")
}
// Prints "Eww, turnips are horrible."
```

`if` 구문 같은 조건문은 [제어흐름 (Control Flow)](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html) 에서 자세히 다룹니다.

Swift의 타입 세이프티는 부울이 아닌 값이 `Bool` 로 대체되는 것을 방지합니다. 아래 예제는 컴파일 시 에러를 발생합니다:

```swift
let i = 1
if i {
    // this example will not compile, and will report an error
}
```

그러나 아래와 같은 예제는 정상 동작합니다:

```swift
let i = 1
if i == 1 {
    // this example will compile successfully
}
```

`i == 1` 비교 결과는 `Bool` 타입이므로 이 두번째 예제는 타입 검사를 정상적으로 수행할 수 있습니다. `i == 1` 과 같은 비교는 [기본 연산자 (Basic Operators)](https://docs.swift.org/swift-book/LanguageGuide/BasicOperators.html) 에서 설명합니다.

Swift의 타입 세이프티에 대한 다른 예제와 마찬가지로 이 방법은 실수로 인한 오류를 피하고 코드의 특정 섹션의 의도를 항상 명확하도록 보장합니다.

## 튜플 (Tuples)

_튜플 (Tuples)_ 은 여러값을 단일 복합 값으로 그룹화 합니다. 튜플안에 값은 어떠한 타입도 가능하며 서로 같은 타입일 필요는 없습니다.

이 예제에서 `(404, "Not Found")` 는 _HTTP 상태 코드 (HTTP status code)_ 를 나타내는 튜플입니다. HTTP 상태 코드는 웹 페이지를 요청할 때마다 웹 서버가 반환하는 특정 값입니다. `404 Not Found` 상태 코드는 요청한 웹 페이지가 존재하지 않을 때 반환됩니다.

```swift
let http404Error = (404, "Not Found")
// http404Error is of type (Int, String), and equals (404, "Not Found")
```

`(404, "Not Found")` 튜플은 HTTP 상태 코드에 2개의 개별 값인 숫자와 사람이 읽을 수 있는 설명을 제공하는 `Int` 와 `String` 을 함께 그룹화하여 제공합니다. 이것은 "튜플의 타입은 `(Int, String)`" 이라고 설명할 수 있습니다.

모든 타입의 튜플을 만들 수 있으며 원하는 만큼 다른 타입을 포함할 수 있습니다. 튜플의 타입 `(Int, Int, Int)` 또는 `(String, Bool)` 또는 실제로 필요한 다른 어떠한 것도 만들 수 있습니다.

튜플의 내용을 별도의 상수 또는 변수로 _분해_ 하여 평소와 같이 접근할 수 있습니다:

```swift
let (statusCode, statusMessage) = http404Error
print("The status code is \(statusCode)")
// Prints "The status code is 404"
print("The status message is \(statusMessage)")
// Prints "The status message is Not Found"
```

튜플의 값 중 일부만 필요한 경우 튜플을 분해할 때 밑줄 (_)로 튜플의 일부를 무시할 수 있습니다:

```swift
let (justTheStatusCode, _) = http404Error
print("The status code is \(justTheStatusCode)")
// Prints "The status code is 404"
```

또는 0에서 시작하는 인덱스를 사용하여 튜플의 개별 요소 값에 접근할 수 있습니다:

```swift
print("The status code is \(http404Error.0)")
// Prints "The status code is 404"
print("The status message is \(http404Error.1)")
// Prints "The status message is Not Found"
```

튜플을 정의할 때 튜플의 요소에 이름을 정할 수 있습니다:

```swift
let http200Status = (statusCode: 200, description: "OK")
```

튜플 요소에 이름이 있다면 요소의 값에 요소 이름으로 접근이 가능합니다:

```swift
print("The status code is \(http200Status.statusCode)")
// Prints "The status code is 200"
print("The status message is \(http200Status.description)")
// Prints "The status message is OK"
```

튜플은 함수의 반환 값으로 특히 유용합니다. 웹 페이지를 검색하는 함수는 페이지 검색의 성공 또는 실패를 설명하기위해 `(Int, String)` 튜플 타입을 반환할 수 있습니다. 이 함수는 각각 다른 타입의 2가지 고유한 값으로 튜플을 반환함으로써 단일 타입의 단일 값만 반환할 수 있는 경우보다 유용합니다. 자세한 내용은 [반환값이 여러개인 함수 (Functions with Multiple Return Values)](https://docs.swift.org/swift-book/LanguageGuide/Functions.html#ID164) 를 참조 바랍니다.

> NOTE
> 튜플은 관련된 값의 간단한 그룹에 유용합니다. 복잡한 데이터 구조를 생성하는데는 맞지 않습니다. 데이터 구조가 복잡한 경우 튜플이 아닌 클래스 (class) 또는 구조체 (structure)를 사용하십시오. 자세한 내용은 [구조체와 클래스 (Structures and Classes)](https://docs.swift.org/swift-book/LanguageGuide/ClassesAndStructures.html) 를 참조 바랍니다.

## 옵셔널 (Optionals)

값이 없는 경우에 _옵셔널 (optionals)_ 을 사용합니다. 옵셔널은 2가지 가능성이 있습니다: 값이 있고 옵셔널을 풀어서 값에 접근하거나 값이 없을 수도 있습니다.

> NOTE
> 옵셔널의 개념은 C 또는 Objective-C 에는 존재하지 않는 개념입니다. Objective-C에서 가장 가까운 것은 객체를 반환하는 메서드에서 `nil` (유효한 객체가 없음)을 반환하는 것입니다. 그러나 이것은 오직 객체에 대해서만 동작하고 구조체, 기본 C 타입, 또는 열거형 값에서는 동작하지 않습니다. 이러한 타입의 경우 Objective-C 메서드는 일반적으로 값이 없음을 나타내는 `NSNotFound` 와 같은 특수한 값을 반환합니다. 이러한 방법은 메서드 호출자가 특수한 값에 대해 대응하고 체크해야 된다는 것을 알고 있다고 가정합니다. Swift의 옵셔널은 특수한 상수 없이 _어떠한 타입_ 에 대해서 값이 없음을 나타낼 수 있습니다.

다음의 예는 옵셔널을 사용하여 값이 없는 상황에 대처하는 방법을 나타냅니다. Swift의 `Int` 타입은 `String` 값을 `Int` 값으로 변환하는 초기화가 존재합니다. 그러나 모든 문자열을 정수로 변환할 수 없습니다. 문자열 "123"은 숫자값 123으로 변환될 수 있지만 문자열 "hello, world"는 변환할 숫자값이 없습니다.

아래 예제는 `String` 을 `Int` 로 초기화하는 것을 보여줍니다:

```swift
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)
// convertedNumber is inferred to be of type "Int?", or "optional Int"
```

초기화가 실패할 수 있으므로 `Int` 가 아닌 _옵셔널_ `Int` 를 반환합니다. 옵셔널 `Int` 는 `Int` 가 아닌 `Int?` 로 작성합니다. 물음표는 값의 포함여부가 옵셔널이라는 것을 나타내고 옵셔널이란 _어떠한_ `Int` 값이 있거나 값이 없을 수 있다는 의미입니다 (여기서는 `Bool` 값 또는 `String` 값을 포함할 수 없고 `Int` 이거나 아무런 값이 없을 수 있습니다).

### nil

옵셔널 변수에 특수한 값 `nil` 로 지정하여 값이 없는 상태를 나타낼 수 있습니다:

```swift
var serverResponseCode: Int? = 404
// serverResponseCode contains an actual Int value of 404
serverResponseCode = nil
// serverResponseCode now contains no value
```

> NOTE
> 옵셔널이 아닌 상수와 변수에는 `nil` 을 사용할 수 없습니다. 코드에서 상수 또는 변수가 값이 없는 상태에서 동작이 필요하다면 항상 해당 타입의 옵셔널 값으로 선언해야 합니다.

기본값이 없이 옵셔널 변수를 정의하면 이 변수는 자동적으로 `nil` 로 설정됩니다:

```swift
var surveyAnswer: String?
// surveyAnswer is automatically set to nil
```

> NOTE
> Swift의 `nil` 은 Objective-C의 `nil` 과 다릅니다. Objective-C에서의 `nil` 은 존재하지 않는 객체에 대한 포인터입니다. Swift에서의 `nil` 은 특정 타입의 값이 없음을 나타내며 포인터가 아닙니다. 객체 타입 뿐만 아니라 _모든_ 타입의 옵셔널을 `nil` 로 설정할 수 있습니다.

### if 구문과 강제로 풀기 (If Statements and Forced Unwrapping)

`if` 구문은 옵셔널과 `nil` 을 비교하여 옵셔널에 값이 포함되어 있는지 확인할 수 있습니다. "같음" 연산자 (==) 또는 "같지 않음" 연산자 (!=)로 비교를 수행할 수 있습니다.

옵셔널에 값이 있다면 `nil` 과 "같지 않음"으로 간주됩니다:

```swift
if convertedNumber != nil {
    print("convertedNumber contains some integer value.")
}
// Prints "convertedNumber contains some integer value."
```

옵셔널에 값이 포함되어 있다고 확신하면 옵셔널 이름 끝에 느낌표 (!)를 추가하여 값에 접근할 수 있습니다. 여기서 느낌표란 "이 옵셔널은 확실히 값을 가지고 있습니다. 사용해도 괜찮습니다."라는 의미입니다. 이것을 옵셔널의 값에 대한 _강제로 풀기 (forced unwrapping)_ 라 합니다:

```swift
if convertedNumber != nil {
    print("convertedNumber has an integer value of \(convertedNumber!).")
}
// Prints "convertedNumber has an integer value of 123."
```

[제어흐름 (Control Flow)](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html) 에서 `if` 구문에 대해 자세히 다뤄보겠습니다.

> NOTE
> `!` 를 사용하여 값이 없는 옵셔널에 사용하면 런타임 에러가 발생합니다. 항상 `!` 를 사용하여 값을 강제로 풀기 전에 옵셔널에 `nil` 이 아닌 값이 있다는 것을 확실시 해야 합니다.

### 옵셔널 바인딩 (Optional Binding)

_옵셔널 바인딩 (optional binding)_ 은 옵셔널이 값을 포함하고 있는지 확인하고 값이 있는 경우 해당 값을 임시 상수 또는 변수로 사용할 수 있게 해줍니다. 옵셔널 바인딩은 `if` 와 `while` 구문에서 옵셔널에 값이 있는지 체크하고 단일 동작의 일부로 상수 또는 변수로 추출할 수 있습니다. `if` 와 `while` 구문은 [제어흐름 (Control Flow)](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html) 에서 자세히 다룹니다.

`if` 구문에서 옵셔널 바인딩은 아래와 같이 사용합니다:

```swift
if let 'constantName' = 'someOptional' {
    'statements'
}
```

강제로 풀기 (forced unwrapping) 보다 옵셔널 바인딩을 사용하여 [옵셔널 (Optionals)]() 섹선에 있는 예제의 `possibleNumber` 를 다시 작성할 수 있습니다:

```swift
if let actualNumber = Int(possibleNumber) {
    print("The string \"\(possibleNumber)\" has an integer value of \(actualNumber)")
} else {
    print("The string \"\(possibleNumber)\" could not be converted to an integer")
}
// Prints "The string "123" has an integer value of 123"
```

이 코드는 아래와 같이 읽을 수 있습니다:

"`Int(possibleNumber)` 에 의해 반환된 옵셔널 `Int` 에 값이 포함되어 있으면 `actualNumber` 라는 새로운 상수에 옵셔널에 포함된 값을 설정합니다."

변환에 성공하면 `actualNumber` 상수는 `if` 구문에 첫번째 중괄호 내에서 사용할 수 있습니다. `actualNumber` 상수는 이미 옵셔널 _안_에 값을 가지고 초기화 되었으며 값에 접근하기 위해 `!` 접미사를 사용할 필요가 없습니다. 이 예제에서 `actualNumber` 는 간단하게 변환 결과를 출력하는데 사용됩니다.

옵셔널 바인딩은 상수와 변수 둘다 사용이 가능합니다. `if` 구문에 첫번째 중괄호에서 `actualNumber` 의 값을 변경하고 싶다면 `if var actualNumber` 로 대신 쓸 수 있으며 옵셔널에 포함된 값은 상수가 아닌 변수로 사용할 수 있습니다.

필요한 경우 `if` 구문에 쉼표로 구분하여 옵셔널 바인딩 및 부울 조건을 여러개 포함할 수 있습니다. 옵셔널 바인딩 값 중 하나가 `nil` 이거나 부울 조건이 `false` 로 판단되면 전체 `if` 구문의 조건은 `false` 로 간주됩니다. 다음의 예는 같은 `if` 구문입니다:

```swift
if let firstNumber = Int("4"), let secondNumber = Int("42"), firstNumber < secondNumber && secondNumber < 100 {
    print("\(firstNumber) < \(secondNumber) < 100")
}
// Prints "4 < 42 < 100"

if let firstNumber = Int("4") {
    if let secondNumber = Int("42") {
        if firstNumber < secondNumber && secondNumber < 100 {
            print("\(firstNumber) < \(secondNumber) < 100")
        }
    }
}
// Prints "4 < 42 < 100"
```

> NOTE
> `if` 구문에서 옵셔널 바인딩으로 생성된 상수와 변수는 오직 `if` 구문의 본문 안에서만 사용가능합니다. 반면에 `guard` 구문으로 생성된 상수와 변수는 [이른 종료 (Early Exit)](https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html#ID525) 에 설명된 것처럼 `guard` 구문 다음 코드 라인부터 사용 가능합니다.

### 암시적으로 언래핑된 옵셔널 (Implicitly Unwrapped Optionals)

위에서 설명했듯이 옵셔널은 상수 또는 변수가 "값이 없음"을 허락하고 알려줍니다. 옵셔널은 값이 존재하는지 `if` 구문에서 체크할 수 있고 값이 존재한다면 옵셔널 값에 접근하기위해 옵셔널 바인딩을 이용할 수 있습니다.

때로는 프로그램 구조에서 옵셔널이 값을 처음 설정한 후에는 항상 값을 갖는 것은 분명합니다. 이러한 경우 _항상_ 값이 있다고 가정할 수 있으므로 접근할 때마다 옵셔널의 값을 확인하고 언래핑 할 필요가 없습니다.

이러한 옵셔널은 _암시적으로 언래핑된 옵셔널 (implicitly unwrapped optionals)_로 정의됩니다. 옵셔널을 만들기위해 타입뒤에 물음표 (`String?`)를 작성하는 대신에 느낌표 (`String!`) 로 암시적으로 언래핑된 옵셔널을 작성합니다. 사용할 때 옵셔널 이름의 뒤에 느낌표를 위치시키는 것보다 선언할 때 옵셔널 타입 뒤에 느낌표를 위치시키는 것이 더 좋습니다.

암시적으로 언래핑된 옵셔널은 옵셔널이 처음 정의된 직후에 옵셔널의 값이 존재하는 것으로 확인되고 그 이후 모든 시점에 존재한다고 가정할 수 있는 경우에 유용합니다. Swift에서 암시적으로 언래핑된 옵셔널은 [소유되지 않은 참조와 암시적으로 언래핑된 옵셔널 프로퍼티 (Unowned References and Implicitly Unwrapped Optional Properties)](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html#ID55) 에서 설명한 대로 클래스 초기화 중에 주로 사용합니다.

암시적으로 언래핑된 옵셔널은 내부적으로 옵셔널이지만 옵셔널에 접근할 때마다 옵셔널 값을 풀 필요없이 옵셔널이 아닌 값처럼 사용할 수도 있습니다. 다음 예제는 명시적 `String` 로서 랩핑된 값에 접근할 때 옵셔널 문자열과 암시적으로 언래핑된 옵셔널 문자열의 동작 차이를 보여줍니다:

```swift
let possibleString: String? = "An optional string."
let forcedString: String = possibleString! // requires an exclamation point

let assumedString: String! = "An implicitly unwrapped optional string."
let implicitString: String = assumedString // no need for an exclamation point
```

필요한 경우 암시적으로 언래핑된 옵셔널은 옵셔널을 강제로 언래핑을 허락하는 것으로 생각할 수 있습니다. 암시적으로 언래핑된 옵셔널을 사용할 때 Swift는 처음에 기존의 옵셔널 값으로 사용하려고 하고 사용이 불가능할 경우 Swift는 값을 강제로 언래핑 합니다. 위의 코드에서 옵셔널 값 `assumedString` 은 `implicitString` 이 명시적으로 옵셔널이 아닌 `String` 타입이기 때문에 `implicitString` 에 값을 할당하기 전에 강제로 언래핑 됩니다. 아래의 코드에서 `optionalString` 은 명시적 타입이 없으므로 기본적으로 옵셔널입니다.

```swift
let optionalString = assumedString
// The type of optionalString is "String?" and assumedString isn't force-unwrapped.
```

암시적으로 언래핑된 옵셔널이 `nil` 이고 래핑된 값에 접근하려고 하면 런타임 에러가 발생합니다. 이 결과는 값이 없는 옵셔널 뒤에 느낌표를 배치한 것과 같습니다.

암시적으로 언래핑된 옵셔널은 일반 옵셔널과 같은 방법으로 `nil` 체크를 할 수 있습니다:

```swift
if assumedString != nil {
    print(assumedString!)
}
// Prints "An implicitly unwrapped optional string."
```

옵셔널 바인딩과 함께 암시적으로 언래핑된 옵셔널은 단일 구문으로 해당 값을 확인하고 언래핑할 수 있습니다.

```swift
if let definiteString = assumedString {
    print(definiteString)
}
// Prints "An implicitly unwrapped optional string."
```

> NOTE
> 나중에 변수가 `nil` 이 될 가능성이 있다면 암시적으로 언래핑된 옵셔널을 사용하지 말아야 합니다. 변수의 수명에 따라 `nil` 값을 확인해야 한다면 항상 기본 옵셔널을 사용해야 합니다.

## 에러 처리 (Error Handling)

프로그램이 실행되는 동안 에러가 발생할 때 처리하기위해 _에러 처리 (error handling)_를 사용합니다.

값의 존재 유무를 사용하여 함수의 성공 또는 실패를 전달할 수 있는 옵셔널과 달리 에러 처리를 사용하면 에러 원인을 판별하고 필요한 경우 에러를 프로그램의 다른 부분으로 전파 할 수 있습니다.

함수에 에러 조건이 되면 에러가 _발생_합니다. 해당 함수의 호출자는 에러를 _포착_하고 적절하게 응답할 수 있습니다.

```swift
func canThrowAnError() throws {
    // this function may or may not throw an error
}
```

함수는 선언에 `throws` 키워드를 포함시켜 에러가 발생할 수 있음을 나타냅니다. 에러를 발생할 수 있는 함수를 호출할 때는 표현식 앞에 `try` 키워드를 붙여야 합니다.

Swift는 `catch` 절에 의해 처리될 때까지 현재 범위에서 에러를 자동으로 전파합니다.

```swift
do {
    try canThrowAnError()
    // no error was thrown
} catch {
    // an error was thrown
}
```

`do` 구문은 에러를 하나 이상의 `catch` 절로 전파할 수 있는 새로운 범위를 만듭니다.

다음은 에러 처리를 사용하여 다양한 에러 조건에 응답하는 방법의 예입니다:

```swift
func makeASandwich() throws {
    // ...
}

do {
    try makeASandwich()
    eatASandwich()
} catch SandwichError.outOfCleanDishes {
    washDishes()
} catch SandwichError.missingIngredients(let ingredients) {
    buyGroceries(ingredients)
}
```

이 예에서 `makeASandwich()` 함수는 깨끗한 그릇을 사용할 수 없거나 재료가 없는 경우 에러가 발생할 것입니다. `makeASandwich()` 함수는 에러를 발생할 수 있으므로 `try` 표현식으로 래핑됩니다. 함수 호출을 `do` 구문으로 래핑하면 어떠한 에러도 `catch` 절로 전파됩니다.

에러가 발생하지 않으면 `eatASandwich()` 함수가 호출됩니다. `SandwichError.outOfCleanDishes` 에러가 발생하면 `washDishes()` 함수가 호출됩니다. `SandwichError.missingIngredients` 에러가 발생하면 `catch` 패턴에 의해 캡쳐된 `[String]` 값과 함께 `buyGroceries(_:)` 함수가 호출됩니다.

에러 발생, 포착, 전파는 [에러 처리 (Error Handling)](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html) 에서 자세히 다룹니다.

## 주장과 전제조건 (Assertions and Preconditions)

_주장과 전제조건 (Assertions and preconditions)_ 은 런타임시 발생하는 조건입니다. 추가 코드를 실행하기 전에 이를 사용하여 필수조건이 충족되는지 확인할 수 있습니다. 주장 또는 전제조건의 부울 조건이 `true` 이면 코드는 평소와 같이 진행됩니다. 조건이 `false` 로 판단되면 프로그램의 현재 상태는 유효하지 않아 코드 실행은 종료되고 앱은 종료됩니다.

주장과 전제조건은 가정과 기대치를 표현하므로 코드의 일부로 포함할 수 있습니다. 주장은 개발과정에서 실수와 잘못된 가정을 찾는데 도움이 되고 전제조건은 프로덕션 문제를 감지하는데 도움이 됩니다.

런타임 시 기대치를 확인하는 것 이외에 주장과 전제조건은 또한 코드 내에서 유용한 문서 형식이 됩니다. 위의 [에러 처리 (Error Handling)](https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html#ID515) 와 다르게 주장과 전제조건은 복구 가능하거나 예상되는 에러에 사용되지 않습니다. 실패한 주장 또는 전제조건은 유효하지 않은 프로그램 상태를 나타내기 때문에 실패한 상태를 잡을 방법은 없습니다.

주장과 전제조건을 사용하는 것은 유효하지 않는 조건이 발생하지 않게 코드를 디자인하기 위함입니다. 그러나 유효한 데이터 및 상태를 적용하기 위해 이를 사용하면 유효하지 않은 상태가 발생하면 앱이 종료되기 때문에 더 쉽게 문제에 대해 디버깅 할 수 있습니다. 유효하지 않은 상태가 감지되는 즉시 실행을 중지하면 해당 유효하지 않은 상태로 인한 피해를 제한하는데 도움이 됩니다.

주장과 전제조건의 차이점은 언제 체크되는지에 있습니다: 주장은 오직 디버그 빌드에서 체크되지만 전제조건은 디버그와 프로덕션 빌드에서 체크됩니다. 프로덕션 빌드일 때 주장 내부의 조건은 실행되지 않습니다. 이 의미는 프로덕션에서 성능의 영향이 없이 개발 단계에서 많은 양의 주장을 사용할 수 있다는 뜻입니다.

### 주장을 통한 디버깅 (Debugging with Assertions)

Swift 표준 라이브러리에 [`assert(_:_:file:line:)`](https://developer.apple.com/documentation/swift/1541112-assert) 함수로 주장을 작성할 수 있습니다. 이 함수에 `true` 또는 `false` 로 판단될 표현식과 조건이 `false` 일 경우 출력될 메세지를 전달합니다. 예를 들어:

```swift
let age = -3
assert(age >= 0, "A person's age can't be less than zero.")
// This assertion fails because -3 is not >= 0.
```

이 예에서 코드는 `age` 가 음수가 아니고 `age >= 0` 이 `true` 일 경우 이어서 실행됩니다. `age` 가 음수이면 `age >= 0` 은 `false` 가 되고 주장은 실패되고 애플리케이션은 종료됩니다.

예를 들어 평범하게 조건만 반복될 때 메세지를 생략할 수 있습니다.

```swift
assert(age >= 0)
```

코드가 이미 조건이 체크되었다면 주장이 실패되었는지를 알 수 있는 [`assertionFailure(_:file:line:)`](https://developer.apple.com/documentation/swift/1539616-assertionfailure) 함수를 사용합니다. 예를 들어:

```swift
if age > 10 {
    print("You can ride the roller-coaster or the ferris wheel.")
} else if age >= 0 {
    print("You can ride the ferris wheel.")
} else {
    assertionFailure("A person's age can't be less than zero.")
}
```

### 강제 전제조건 (Enforcing Preconditions)

조건이 거짓 일 가능성이 있을 때마다 전제조건을 사용하지만 코드가 순차적으로 실행되려면 _확실하게_ 참이어야 합니다. 예를 들어 어떤 값들이 범위를 벗어나는지 또는 함수에 유효한 값이 전달되는지 체크하기위해 전제조건을 사용합니다.

[`precondition(_:_:file:line:)`](https://developer.apple.com/documentation/swift/1540960-precondition) 함수로 전제조건을 작성할 수 있습니다. 이 함수에 `true` 또는 `false` 로 판단될 표현식과 조건이 `false` 일 경우 출력될 메세지를 전달합니다. 예를 들어:

```swift
// In the implementation of a subscript...
precondition(index > 0, "Index must be greater than zero.")
```

[`preconditionFailure(_:file:line:)`](https://developer.apple.com/documentation/swift/1539374-preconditionfailure) 함수를 호출하여 실패가 발생했음을 알릴 수 있습니다. 예를 들어 유효한 데이터는 switch 의 default 케이스가 아닌 다른 케이스에서 처리되어야 합니다.

>  NOTE
> 체크하지 않는 모드 (`-Ounchecked`)로 컴파일하면 전제조건은 체크하지 않습니다. 컴파일러는 전제조건은 항상 참이라고 가정하고 코드에 알맞게 최적화 합니다. 그러나 `fatalError(_:file:line:)` 함수는 최적화 설정과 무관하게 항상 중지를 실행합니다.
>
> 프로토타입과 초기 개발단계에서 아직 구현되지 않은 기능에서 `fatalError(_:file:line:)` 을 사용할 수 있으며 `fatalError("Unimplemented")` 와 같이 작성할 수 있습니다. 주장 또는 전제조건과 다르게 치명적인 에러는 절대 최적화 되지 않기 때문에 이 구현을 만나면 항상 중지됩니다.
