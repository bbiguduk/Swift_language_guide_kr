# 기본 (The Basics)

일반적인 종류의 데이터로 동작하고 기본 구문을 작성합니다.

Swift는 정수에 사용되는 `Int`,
부동소수점(floating-point) 값에 사용되는 `Double`,
Boolean 값을 나타내는 `Bool`,
그리고 텍스트를 나타내는 `String`과 같은
기본적인 데이터 타입을 제공합니다.
Swift는 [컬렉션 타입 (Collection Types)](./collection-types.md)에서 설명된 대로
`Array`, `Set`, `Dictionary`라는
세 가지 기본 컬렉션 타입을 제공합니다.

Swift는 변수를 식별 가능한 이름으로 값을 저장하고 참조합니다.
Swift는 값을 변경할 수 없는 변수를 광범위하게 사용합니다.
이러한 변수를 상수라고 하며, 값을 변경할 필요가 없을 때
코드를 더 안전하고 명확하게 만들기위해 Swift에서 사용됩니다.

익숙한 타입 외에도,
Swift는 튜플(tuple)과 같은 고급 타입도 있습니다.
튜플은 값을 그룹화 하여 생성하거나 전달할 수 있습니다.
함수에서 여러 값을 단일복합값으로 반환할 때 튜플을 사용할 수 있습니다.

Swift는 옵셔널(optional) 타입을 사용하여
값이 존재하지 않는 상태를 처리합니다.
옵셔널은 "값이 *있고* *x*이다"
또는 "값이 *없다*"를 알려줍니다.
옵셔널은 코드가
항상 값을 사용하기 전에 값이 누락되었는지 확인하는 것을 보장하고,
옵셔널이 아닌 값은 값이 절대로 누락되지 않도록 보장합니다.

Swift는 안전한 언어이며,
이것은 개발 과정에서 가능한 빨리
여러 범주의 버그를 찾고 수정하는 것을 더 쉽게 만들고
특정 종류의 버그가 발생하지 않도록 보장합니다.
타입 안전성(Type safety)은 코드가 다루는 값의 타입에 대해
명확하게 할 수 있도록 해줍니다.
`String`을 요구하는 코드에서
타입 안전성은 실수로 `Int`를 전달하는 것을 방지합니다.
메모리 안정성(Memory Safety)은 초기화되지 않은 메모리나 할당 해제된 객체가 아닌
유효한 데이터만 사용하도록 보장하며,
여러 코드가 동시에 실행되는 프로그램에서도
데이터를 안전하게 다루도록 보장합니다.
Swift는 대부분의 안전성 검사를 코드를 빌드하는 동안 수행하며,
일부 경우에는 코드가 실행되는 동안 검사를 수행합니다.

## 상수와 변수 (Constants and Variables)

상수와 변수는 이름
(`maximumNumberOfLoginAttempts` 또는 `welcomeMessage`)과
특정 타입
(숫자 `10` 또는 문자열 `"Hello"`와 같은 타입)의 값을 연결합니다.
*상수(Constant)*의 값은 최초 지정 후 변경이 불가능하지만,
*변수(Variable)*는 다른 값으로 변경이 가능합니다.

### 상수와 변수 선언 (Declaring Constants and Variables)

상수와 변수는 사용하기 전에 선언이 되어야 합니다.
상수는 `let` 키워드와 함께 선언하고
변수는 `var` 키워드와 함께 선언합니다.
다음의 예시는 상수와 변수를 사용하여
어떻게 사용자 로그인 시도 횟수를 추적하는지를 보여줍니다:

```swift
let maximumNumberOfLoginAttempts = 10
var currentLoginAttempt = 0
```

<!--
  - test: `constantsAndVariables`

  ```swifttest
  -> let maximumNumberOfLoginAttempts = 10
  -> var currentLoginAttempt = 0
  ```
-->

이 코드는 아래와 같이 읽을 수 있습니다:

"`maximumNumberOfLoginAttempts` 인 새로운 상수를 선언하고,
`10`이라는 값을 설정합니다.
그리고 `currentLoginAttempt` 인 새로운 변수를 선언하고,
`0`으로 초기값을 설정합니다."

이 예시에서,
최대 로그인 시도 횟수는 최대값은 절대 변경되지 않아야 하므로
상수로 선언했습니다.
현재 로그인 시도 횟수는 로그인 실패 시 값을 증가시켜야 하므로
변수로 선언했습니다.

코드에서 저장된 값이 변경되지 않으면,
`let` 키워드를 사용하여 상수로 선언합니다.
변경되는 값을 저장하려면 변수를 사용합니다.

상수 또는 변수로 선언할 때,
위의 예시처럼
선언과 함께 값을 지정할 수 있습니다.
또는
프로그램 내에서 초기값을 나중에 할당할 수 있지만,
처음으로 해당 값을 읽기 전에
반드시 값이 지정되어야 합니다.

```swift
var environment = "development"
let maximumNumberOfLoginAttempts: Int
// maximumNumberOfLoginAttempts has no value yet.

if environment == "development" {
    maximumNumberOfLoginAttempts = 100
} else {
    maximumNumberOfLoginAttempts = 10
}
// Now maximumNumberOfLoginAttempts has a value, and can be read.
```

<!--
  - test: `constantsWithDeferredInitialization`

  ```swifttest
  -> var environment = "development"
  -> let maximumNumberOfLoginAttempts: Int
  -> if environment == "development" {
         maximumNumberOfLoginAttempts = 100
     } else {
         maximumNumberOfLoginAttempts = 10
     }
  >> print(maxNumberOfLoginAttempts)
  << 100
  ```
-->

이 예시에서,
최대 로그인 시도 횟수는 상수이고,
이 값은 환경에 따라 달라집니다.
개발 환경에서는
100의 값을 가지고;
다른 환경에서는 10을 가집니다.
`if` 구문의 각 조건에서
어떠한 값으로 `maximumNumberOfLoginAttempts`을 초기화하고,
이 상수는 항상 값이 있음을 보장합니다.
이 방법으로 초기값을 설정할 때,
Swift가 어떻게 코드를 검사하는지 자세한 내용은
[상수 선언 (Constant Declaration)](../language-reference/declarations.md#상수-선언-constant-declaration)을 참고바랍니다.

여러개의 상수 또는 여러개의 변수를 선언할 때 콤마로 구분하여
한줄로 선언이 가능합니다:

```swift
var x = 0.0, y = 0.0, z = 0.0
```

<!--
  - test: `multipleDeclarations`

  ```swifttest
  -> var x = 0.0, y = 0.0, z = 0.0
  >> print(x, y, z)
  << 0.0 0.0 0.0
  ```
-->

### 타입 명시 (Type Annotations)

상수 또는 변수를 선언할 때, 저장할 수 있는 값의 종류를 명확하게 하기위해
*타입 명시(Type Annotation)*를 제공할 수 있습니다.
타입 명시는 상수 또는 변수 이름 뒤에 콜론과
공백 한칸 뒤에 사용할 타입 이름을 적어 사용합니다.

이 예시는 `welcomeMessage`라는 변수에
`String` 값을 저장할 수 있는 변수를 나타내는 타입 명시를 제공합니다:

```swift
var welcomeMessage: String
```

<!--
  - test: `typeAnnotations`

  ```swifttest
  -> var welcomeMessage: String
  ```
-->

위 코드의 선언에 있는 콜론은 "...의 타입은..." 을 의미하므로
아래와 같이 읽을 수 있습니다:

"선언한 변수는 `welcomeMessage`라고 하며 이것의 타입은 `String`입니다."

"...의 타입은 `String`입니다."라는 의미는 "모든 `String` 값은 저장 가능합니다." 입니다.
저장할 수 있는 "어떠한 타입"(또는 "어떠한 종류")라고 생각합니다.

`welcomeMessage` 변수는 아무런 오류 없이 어떠한 문자열 값을 설정할 수 있습니다:

```swift
welcomeMessage = "Hello"
```

<!--
  - test: `typeAnnotations`

  ```swifttest
  -> welcomeMessage = "Hello"
  >> print(welcomeMessage)
  << Hello
  ```
-->

동일한 타입의 연관된 변수들을 한 줄로 콤마로 구분하고,
마지막 변수 이름 뒤에 타입 명시를 통해 선언할 수 있습니다:

```swift
var red, green, blue: Double
```

<!--
  - test: `typeAnnotations`

  ```swifttest
  -> var red, green, blue: Double
  ```
-->

> Note: 실제로 타입 명시가 필요한 경우는 드뭅니다.
> 상수 또는 변수를 선언할 때 초기값을 지정하면
> Swift는 [타입 안전성과 타입 추론 (Type Safety and Type Inference)](#타입-안전성과-타입-추론-type-safety-and-type-inference)에서 설명한대로
> 해당 상수 또는 변수에 사용될 타입을 거의 항상 추론할 수 있습니다.
> 위의 `welcomeMessage` 예시에서 초기값을 지정하지 않았으므로,
> `welcomeMessage` 변수의 타입은 초기값에서 유추되지 않고
> 타입을 명시 하였습니다.

### 상수와 변수의 이름 (Naming Constants and Variables)

상수와 변수 이름은 유니코드(Unicode) 문자를 포함하여
대부분의 문자를 포함할 수 있습니다:

```swift
let n = 3.14159
let 你好 = "你好世界"
let 🐶🐮 = "dogcow"
```

<!--
  - test: `constantsAndVariables`

  ```swifttest
  -> let π = 3.14159
  -> let 你好 = "你好世界"
  -> let 🐶🐮 = "dogcow"
  ```
-->

상수와 변수 이름은
공백, 수학적 기호, 화살표, 내부에서 사용하는 유니코드 스칼라 값,
선과 박스를 그리는 문자를 포함할 수 없습니다.
숫자는 이름의 다른곳에는 포함될 수 있지만
숫자로 시작하는 이름은 선언할 수 없습니다.

특정 타입으로 상수 또는 변수를 선언하면
동일한 이름으로 다시 선언하거나
다른 타입의 값을 저장하도록 변경하여 선언할 수 없습니다.
상수를 변수로 바꾸거나
변수를 상수로 바꿀 수도 없습니다.

> Note: Swift 예약어와 동일한 이름의 상수 또는 변수를 제공해야 한다면
> 이름을 백틱(`` ` ``)으로 감싸야 합니다.
> 그러나 특별한 경우가 아니라면 예약어를 이름으로 사용하는 것은 피하는 것이 좋습니다.

이미 선언된 변수에 동일한 타입의 다른 값으로 변경할 수 있습니다.
이 예시에서 `friendlyWelcome` 값은
`"Hello!"`에서 `"Bonjour!"`로 변경됩니다:

```swift
var friendlyWelcome = "Hello!"
friendlyWelcome = "Bonjour!"
// friendlyWelcome is now "Bonjour!"
```

<!--
  - test: `constantsAndVariables`

  ```swifttest
  -> var friendlyWelcome = "Hello!"
  -> friendlyWelcome = "Bonjour!"
  /> friendlyWelcome is now \"\(friendlyWelcome)\"
  </ friendlyWelcome is now "Bonjour!"
  ```
-->

변수와 다르게 상수 값은 지정된 이후에는 변경할 수 없습니다.
값을 변경하려고 하면 코드가 컴파일 될 때 오류가 발생합니다:

```swift
let languageName = "Swift"
languageName = "Swift++"
// This is a compile-time error: languageName cannot be changed.
```

<!--
  - test: `constantsAndVariables_err`

  ```swifttest
  -> let languageName = "Swift"
  -> languageName = "Swift++"
  // This is a compile-time error: languageName cannot be changed.
  !$ error: cannot assign to value: 'languageName' is a 'let' constant
  !! languageName = "Swift++"
  !! ^~~~~~~~~~~~
  !! /tmp/swifttest.swift:1:1: note: change 'let' to 'var' to make it mutable
  !! let languageName = "Swift"
  !! ^~~
  !! var
  ```
-->

### 상수와 변수 출력 (Printing Constants and Variables)

`print(_:separator:terminator:)` 함수로 상수 또는 변수의 현재값을 출력할 수 있습니다:

```swift
print(friendlyWelcome)
// Prints "Bonjour!"
```

<!--
  - test: `constantsAndVariables`

  ```swifttest
  -> print(friendlyWelcome)
  <- Bonjour!
  ```
-->

`print(_:separator:terminator:)` 함수는
하나 이상의 값을
출력하는 전역 함수입니다.
예를 들어, Xcode에서
`print(_:separator:terminator:)` 함수는 Xcode "콘솔(console)" 창에 결과를 출력합니다.
`separator`와 `terminator` 매개변수는 기본값을 가지고 있으므로
함수를 호출할 때 생략할 수 있습니다.
기본적으로 이 함수는 줄바꿈을 출력하고 종료됩니다.
줄바꿈 없이 값을 출력하려면,
`terminator`로 빈 문자열을 전달하면 됩니다 --- 예를 들어,
`print(someValue, terminator: "")`처럼 사용합니다.
매개변수 기본값에 대한 자세한 내용은
[매개변수 기본값 (Default Parameter Values)](./functions.md#매개변수-기본값-default-parameter-values)을 참고바랍니다.

<!--
  - test: `printingWithoutNewline`

  ```swifttest
  >> let someValue = 10
  -> print(someValue, terminator: "")
  -> print(someValue)
  << 1010
  ```
-->

<!--
  QUESTION: have I referred to Xcode's console correctly here?
  Should I mention other output streams, such as the REPL / playgrounds?
-->

<!--
  NOTE: this is a deliberately simplistic description of what you can do with print().
  It will be expanded later on.
-->

Swift는 *문자열 보간(String interpolation)*을 사용하여 상수나 변수의 이름을
더 긴 문자열에 포함하고,
Swift가 상수나 변수의 현재값으로 대체하도록 합니다.
이름을 소괄호로 감싸고 여는 소괄호 전에 역슬래시를 추가해야 합니다:

```swift
print("The current value of friendlyWelcome is \(friendlyWelcome)")
// Prints "The current value of friendlyWelcome is Bonjour!"
```

<!--
  - test: `constantsAndVariables`

  ```swifttest
  -> print("The current value of friendlyWelcome is \(friendlyWelcome)")
  <- The current value of friendlyWelcome is Bonjour!
  ```
-->

> Note: 문자열 보간과 함께 사용할 수 있는 모든 옵션은
> [문자열 삽입 (String Interpolation)](./strings-and-characters.md#문자열-삽입-string-interpolation)에 자세히 설명되어 있습니다.

## 주석 (Comments)

코드에서 설명이나 기록을 위해
실행되지 않는 문자를 추가할 땐 주석(Comments)을 사용합니다.
주석은 코드가 컴파일 될 때 Swift 컴파일러에 의해 무시됩니다.

Swift에서 주석은 C에서 주석을 다는 방법과 유사합니다.
한줄 주석은 두개의 슬래시(`//`)로 시작합니다:

```swift
// This is a comment.
```

<!--
  - test: `comments`

  ```swifttest
  -> // This is a comment.
  ```
-->

여러줄 주석은 슬래시 뒤에 애스터리스크(`/*`)로 시작하고
애스터리스크 뒤에 슬래시(`*/`)로 끝납니다:

```swift
/* This is also a comment
but is written over multiple lines. */
```

<!--
  - test: `comments`

  ```swifttest
  -> /* This is also a comment
     but is written over multiple lines. */
  ```
-->

C에서 여러줄 주석과 다르게,
Swift는 여러줄 주석 안에 다른 여러줄 주석을 통해 중첩시킬 수 있습니다.
여러줄 주석 블록을 시작한 다음
첫번째 블록 내에서 두번째 여러줄 주석을 시작하여 중첩된 주석을 작성합니다.
그런 다음에 두번째 블록을 닫은 다음 첫번째 블록을 닫습니다:

```swift
/* This is the start of the first multiline comment.
 /* This is the second, nested multiline comment. */
This is the end of the first multiline comment. */
```

<!--
  - test: `comments`

  ```swifttest
  -> /* This is the start of the first multiline comment.
        /* This is the second, nested multiline comment. */
     This is the end of the first multiline comment. */
  ```
-->

중첩된 여러줄 주석을 사용하면 코드에 이미 여러줄 주석이 포함되어 있어도
큰 코드 블록을 빠르고 쉽게 주석 처리할 수 있습니다.

## 세미콜론 (Semicolons)

많은 다른 언어와 다르게,
Swift는 코드의 각 구문 후에 세미콜론(`;`)은
필수가 아닙니다.
그러나 여러 구문을 한줄로 작성할 경우
세미콜론은 *필수로 작성되어야 합니다*:

```swift
let cat = "🐱"; print(cat)
// Prints "🐱"
```

<!--
  - test: `semiColons`

  ```swifttest
  -> let cat = "🐱"; print(cat)
  <- 🐱
  ```
-->

## 정수 (Integers)

*정수(Integers)*는 `42`와 `-23`과 같은
분수가 아닌 전체 숫자입니다.
정수는 *부호가 있는 정수(signed) (양수, 0, 음수)*나
*부호가 없는 정수(unsigned) (양수나 0)*이 있습니다.

Swift는 8, 16, 32, 64 비트 형태의 부호가 있는 정수와 부호가 없는 정수를 지원합니다.
이러한 정수는 8-bit 부호가 없는 정수는 `UInt8`
그리고 32-bit 부호가 있는 정수는 `Int32`와 같이
C와 비슷한 네이밍 형태를 가집니다.
Swift의 모든 타입과 마찬가지로, 정수 타입은 대문자로 시작합니다.

### 정수 범위 (Integer Bounds)

각 정수 타입의 `min`과 `max` 프로퍼티를 통해
각 정수 타입의 최소값과 최대값에 접근할 수 있습니다:

```swift
let minValue = UInt8.min  // minValue is equal to 0, and is of type UInt8
let maxValue = UInt8.max  // maxValue is equal to 255, and is of type UInt8
```

<!--
  - test: `integerBounds`

  ```swifttest
  -> let minValue = UInt8.min  // minValue is equal to 0, and is of type UInt8
  -> let maxValue = UInt8.max  // maxValue is equal to 255, and is of type UInt8
  >> print(minValue, maxValue)
  << 0 255
  ```
-->

이러한 프로퍼티의 값은 적절한 크기의 숫자 타입
(위 예에서 `UInt8`)이므로
동일한 타입의 다른 값과 함께 표현식에 사용될 수 있습니다.

### Int

대부분의 경우 코드에서 사용할 정수의 특정 사이즈를 결정할 필요는 없습니다.
Swift는 현재 플랫폼의 네이티브 사이즈와 같은
정수 타입, `Int`를 제공합니다:

- 32-bit 플랫폼에서 `Int`는 `Int32`와 같은 크기를 가짐
- 64-bit 플랫폼에서 `Int`는 `Int64`와 같은 크기를 가짐

특정 크기의 정수로 작업해야 하는 경우가 아니라면
항상 코드의 정수값을 사용할 때 `Int`를 사용하세요.
이것은 코드 일관성과 상호 운용성을 지원합니다.
32-bit 플랫폼에서도 `Int`는 `-2,147,483,648`과 `2,147,483,647` 사이의 값을 저장할 수 있으며,
일반적인 사용성에 문제가 없습니다.

### UInt

Swift는 또한 현재 플랫폼의 네이티브 사이즈와 같은
정수 타입, `UInt`를 제공합니다:

- 32-bit 플랫폼에서 `UInt`는 `UInt32`와 같은 크기를 가짐
- 64-bit 플랫폼에서 `UInt`는 `UInt64`와 같은 크기를 가짐

> Note: `UInt`는 플랫폼의 네이티브 사이즈와 같은 크기의
> 부호 없는 정수 타입이 필요한 경우에만 사용하세요.
> 저장될 값이 음수가 아니어도
> `Int`를 더 선호합니다.
> 정수값에 `Int`를 일관되게 사용하면
> 코드 상호 운용성을 지원하고
> [타입 안전성과 타입 추론 (Type Safety and Type Inference)](#타입-안전성과-타입-추론-type-safety-and-type-inference)에서 설명한대로 다른 숫자 형식간에 변환 할 필요가 없습니다.

## 부동소수점 숫자 (Floating-Point Numbers)

*부동소수점 숫자(Floating-point numbers)*는
`3.14159`, `0.1`, `-273.15`와 같은 분수 성분을 가진 숫자입니다.

부동소수점은 정수 타입의 값 범위보다 더 넓은 범위의 표현이 가능하고
`Int`보다 더 크거나 작은 값 저장이 가능합니다.
Swift는 2개의 부호를 가진 부동소수점 숫자 타입을 제공합니다:

- `Double`은 64-bit 부동소수점 숫자를 표기
- `Float`은 32-bit 부동소수점 숫자를 표기

> Note: `Double`은 최소 15자리의 소수점 정확도를 가지고 있는것에 반해,
> `Float`은 더 적은 6자리의 정확도를 가집니다.
> 사용할 적절한 부동소수점 타입은
> 코드에서 작업해야하는 값의 특성과 범위에 따라 다릅니다.
> 두 타입 중에는 `Double`이 선호됩니다.

<!--
  TODO: Explicitly mention situations where Float is appropriate,
  such as when optimizing for storage size of collections?
-->

<!--
  TODO: mention infinity, -infinity etc.
-->

## 타입 안전성과 타입 추론 (Type Safety and Type Inference)

Swift 프로그램의 모든 값은 타입을 가집니다.
상수, 변수, 프로퍼티를 포함하여
값을 저장하는 모든 곳에도
타입을 가집니다.
타입 주석을 사용하여 명시적으로 타입을 작성하거나
Swift는 초기값으로부터 타입을 추론할 수 있습니다.
코드를 작성하는 모든 곳에서 제공하는 값의 타입은
그 값을 사용하는 곳에서의 타입과 일치해야 합니다.
예를 들어
코드에서 `String`을 요구하면,
실수로 `Int`를 전달할 수 없습니다.
이러한 검사로 인해 Swift는 *타입 안전(type-safe)* 언어입니다.

타입 안전 언어는 코드에서 다루는 값의 타입에 대해
명확하게 정의하도록 권장합니다.
한 타입의 값은 다른 타입으로 암시적으로 변환되지 않습니다.
그러나 일부 타입은 명시적으로 변환될 수 있습니다.
코드를 빌드할 때,
Swift는 타입 안전성에 대해 코드를 검사하고
일치하지 않는 타입을 오류로 표시합니다.

타입 검사는 다른 타입의 값으로 작업할 때 오류를 피하는데 도움이 됩니다.
그러나 이것이 선언하는 모든 상수와 변수의
타입을 지정해야 한다는 것은 아닙니다.
필요한 값의 특정 타입을 지정하지 않으면
Swift는 적절한 타입으로 *타입 추론(Type Inference)*을 사용합니다.
타입 추론을 통해 컴파일러는 코드를 컴파일 할 때
제공한 값을 검사하여
특정 표현식의 타입을 자동으로 추론할 수 있습니다.

타입 추론 때문에, Swift는 C나 Objective-C와 같은 언어보다
타입 선언을 더 적게 요구합니다.
상수와 변수는 여전히 명시적으로 타입을 지정하지만
타입을 지정하는 대부분의 동작은 자동으로 처리됩니다.

타입 추론은 상수나 변수에
초기값을 선언할 때 아주 유용합니다.
이것은 종종 상수나 변수를 선언할 때
*리터럴 값(literal value)*(또는 *리터럴(literal)*)을 할당하여 이루어집니다.
(리터럴 값은 아래 예에서 `42` 및 `3.14159`와 같은
소스 코드에 직접 표시되는 값입니다.)

예를 들어 어떤 타입인지 선언하지 않고
새로운 상수를 `42`의 리터럴 값으로 지정하면
Swift는 정수처럼 보이는 숫자로 초기화 했기 때문에
상수를 `Int`라고 추론합니다:

```swift
let meaningOfLife = 42
// meaningOfLife is inferred to be of type Int
```

<!--
  - test: `typeInference`

  ```swifttest
  -> let meaningOfLife = 42
  // meaningOfLife is inferred to be of type Int
  >> print(type(of: meaningOfLife))
  << Int
  ```
-->

반대로 어떤 부동소수점 리터럴의 타입인지 선언하지 않으면,
Swift는 `Double`이라고 추론합니다:

```swift
let pi = 3.14159
// pi is inferred to be of type Double
```

<!--
  - test: `typeInference`

  ```swifttest
  -> let pi = 3.14159
  // pi is inferred to be of type Double
  >> print(type(of: pi))
  << Double
  ```
-->

Swift는 부동소수점 숫자의 타입을 추론할 때,
항상 `Double`(`Float` 보다)을 선택합니다.

정수 리터럴과 부동소수점 리터럴을 하나의 표현식에서 결합하면,
`Double` 타입으로 추론합니다:

```swift
let anotherPi = 3 + 0.14159
// anotherPi is also inferred to be of type Double
```

<!--
  - test: `typeInference`

  ```swifttest
  -> let anotherPi = 3 + 0.14159
  // anotherPi is also inferred to be of type Double
  >> print(type(of: anotherPi))
  << Double
  ```
-->

`3`의 리터럴 값은 명시적인 타입이 없으므로,
부동소수점 리터럴이 존재하는 것을 기반으로
`Double`이 추론됩니다.

## 숫자 리터럴 (Numeric Literals)

정수 리터럴은 아래와 같이 쓸 수 있습니다:

- 접두사 없는 *10진수*
- `0b` 접두사로 *2진수*
- `0o` 접두사로 *8진수*
- `0x` 접두사로 *16진수*

아래의 예에서 모든 정수 리터럴은 10진수 `17`의 값을 가집니다:

```swift
let decimalInteger = 17
let binaryInteger = 0b10001       // 17 in binary notation
let octalInteger = 0o21           // 17 in octal notation
let hexadecimalInteger = 0x11     // 17 in hexadecimal notation
```

<!--
  - test: `numberLiterals`

  ```swifttest
  -> let decimalInteger = 17
  -> let binaryInteger = 0b10001       // 17 in binary notation
  -> let octalInteger = 0o21           // 17 in octal notation
  -> let hexadecimalInteger = 0x11     // 17 in hexadecimal notation
  >> print(binaryInteger, octalInteger, hexadecimalInteger)
  << 17 17 17
  ```
-->

부동소수점 리터럴은 10진수(접두사 없음)
또는 16진수(접두사 `0x`)일 수 있습니다.
소수점 양쪽에 항상 숫자(또는 16진수)가 있어야 합니다.
10진수는 대문자나 소문자 `e`로 표시되는
*지수*를 가질 수도 있습니다.
16진수는 대문자나 소문자 p로 표시되는
*지수*를 가질 수도 있습니다.

<!--
  - test: `float-required-vs-optional-exponent-err`

  ```swifttest
  -> let hexWithout = 0x1.5
  !$ error: hexadecimal floating point literal must end with an exponent
  !! let hexWithout = 0x1.5
  !!                       ^
  ```
-->

<!--
  - test: `float-required-vs-optional-exponent`

  ```swifttest
  -> let hexWith = 0x1.5p7
  -> let decimalWithout = 0.5
  -> let decimalWith = 0.5e7
  ```
-->

지수가 `x`인 10진수는
기본 숫자에 10ˣ가 곱해집니다:

- `1.25e2`는 1.25 x 10², 또는 `125.0`.
- `1.25e-2`는 1.25 x 10⁻² , 또는 `0.0125`.

지수가 `x`인 16진수는
기본 숫자에 2ˣ가 곱해집니다:

- `0xFp2`는 15 x 2² , 또는 `60.0`.
- `0xFp-2`는 15 x 2⁻² , 또는 `3.75`.

아래의 예에서 모든 부동소수점 리터럴은 10진수 `12.1875`를 가집니다:

```swift
let decimalDouble = 12.1875
let exponentDouble = 1.21875e1
let hexadecimalDouble = 0xC.3p0
```

<!--
  - test: `numberLiterals`

  ```swifttest
  -> let decimalDouble = 12.1875
  -> let exponentDouble = 1.21875e1
  -> let hexadecimalDouble = 0xC.3p0
  ```
-->

숫자 리터럴은 읽기 쉽게 만드는 추가 포맷을 포함할 수 있습니다.
정수와 부동소수점 모두 추가 0으로 채워질 수 있으며
가독성을 돕기 위해 밑줄을 포함할 수 있습니다.
어떤 형식도 리터럴의 기본값에 영향을 주지 않습니다:

```swift
let paddedDouble = 000123.456
let oneMillion = 1_000_000
let justOverOneMillion = 1_000_000.000_000_1
```

<!--
  - test: `numberLiterals`

  ```swifttest
  -> let paddedDouble = 000123.456
  -> let oneMillion = 1_000_000
  -> let justOverOneMillion = 1_000_000.000_000_1
  ```
-->

## 숫자 타입 변환 (Numeric Type Conversion)

음수를 사용하지 않더라도
코드에서 상수와 변수가 정수로 사용이 되면 `Int` 타입을 사용하세요.
일반적인 상황에서 기본 정수 타입을 사용하면,
정수 상수와 변수가 코드에서 즉시 호환 가능하며
정수 리터럴 값의 추론된 타입과 일치하게 됩니다.

다른 정수 타입은 특정 작업에 필요하거나
외부 소스에서 크기가 명시된 데이터가 필요하거나
성능, 메모리 사용량 또는 다른 최적화가 필요한 경우에만 사용하세요.
이러한 상황에서 명시적으로 크기가 지정된 타입을 사용하면
실수로 인한 값 초과를 방지하고
사용되는 데이터의 특성을 알 수 있습니다.

### 정수 변환 (Integer Conversion)

정수를 저장할 수 있는 상수나 변수의 숫자 범위는
숫자 타입에 따라 다릅니다.
`Int8` 상수나 변수는 `-128`과 `127` 사이의 숫자를 저장할 수 있는
반면에 `UInt8` 상수나 변수는 `0`과 `255` 사이의 숫자를 저장할 수 있습니다.
크기가 지정된 정수 타입의 상수나 변수에 맞지않는 숫자는
컴파일 시에 오류가 발생합니다:

```swift
let cannotBeNegative: UInt8 = -1
// UInt8 cannot store negative numbers, and so this will report an error
let tooBig: Int8 = Int8.max + 1
// Int8 cannot store a number larger than its maximum value,
// and so this will also report an error
```

<!--
  - test: `constantsAndVariablesOverflowError`

  ```swifttest
  -> let cannotBeNegative: UInt8 = -1
  // UInt8 can't store negative numbers, and so this will report an error
  -> let tooBig: Int8 = Int8.max + 1
  // Int8 can't store a number larger than its maximum value,
  // and so this will also report an error
  !! /tmp/swifttest.swift:2:29: error: arithmetic operation '127 + 1' (on type 'Int8') results in an overflow
  !! let tooBig: Int8 = Int8.max + 1
  !!                    ~~~~~~~~ ^ ~
  !! /tmp/swifttest.swift:1:31: error: negative integer '-1' overflows when stored into unsigned type 'UInt8'
  !! let cannotBeNegative: UInt8 = -1
  !!                                ^
  ```
-->

숫자 타입은 다른 범위의 값을 저장할 수 있으므로,
경우에 따라 숫자 타입 변환을 타입별로 선택해야 합니다.
이 방식은 숨겨진 변환 오류를 방지하고,
코드에서 타입 변환 의도를 명확하게 만드는데 도움을 줍니다.

특정 숫자 타입을 다른 타입으로 변환하려면,
기존값을 사용하여 원하는 타입의 새로운 숫자를 초기화해야 합니다.
아래의 예시에서
상수 `twoThousand`는 `UInt16` 타입인 반면에
상수 `one`은 `UInt8` 타입입니다.
두 상수는 타입이 다르기 때문에
직접 더할 수 없습니다.
대신 이 예시에서 `UInt16(one)`을 호출하여
`one`의 값을 새로운 `UInt16`으로 초기화하면
기존값 대신에 이 값을 사용합니다:

```swift
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + UInt16(one)
```

<!--
  - test: `typeConversion`

  ```swifttest
  -> let twoThousand: UInt16 = 2_000
  -> let one: UInt8 = 1
  -> let twoThousandAndOne = twoThousand + UInt16(one)
  >> print(twoThousandAndOne)
  << 2001
  ```
-->

양쪽의 타입이 `UInt16`이므로,
덧셈은 이제 제대로 동작합니다.
출력 상수(`twoThousandAndOne`)은 두 `UInt16` 값을 더하므로
`UInt16` 타입으로 추론됩니다.

`SomeType(ofInitialValue)`는 Swift 타입의 초기화를 호출하고
초기화 값을 전달하는 기본적인 방법입니다.
내부적으로 `UInt16`은 `UInt8` 값을 허용하는 초기화가 있으므로
초기화는 기존 `UInt8`에서 새 `UInt16`을 만드는데 사용됩니다.
그러나 여기서 아무 타입이나 전달할 수 없습니다 ---
`UInt16`이 초기화를 제공하는 타입이어야만 합니다.
기존 타입을 확장하여 새로운 타입
(자신이 정의한 새로운 타입)
을 받아들이는 초기화를 제공하는 것은 [확장 (Extensions)](./extensions.md)에서 다룹니다.

### 정수와 부동소수점 변환 (Integer and Floating-Point Conversion)

정수와 부동소수점 숫자 타입의 변환은 명시적으로 변환해야 합니다:

```swift
let three = 3
let pointOneFourOneFiveNine = 0.14159
let pi = Double(three) + pointOneFourOneFiveNine
// pi equals 3.14159, and is inferred to be of type Double
```

<!--
  - test: `typeConversion`

  ```swifttest
  -> let three = 3
  -> let pointOneFourOneFiveNine = 0.14159
  -> let pi = Double(three) + pointOneFourOneFiveNine
  /> pi equals \(pi), and is inferred to be of type Double
  </ pi equals 3.14159, and is inferred to be of type Double
  ```
-->

여기서 상수 `three`는 타입 `Double`의 새로운 값으로 생성하는데 사용되어
덧셈의 양쪽이 동일한 타입입니다.
이 변환이 없으면 덧셈이 허용되지 않습니다.

부동소수점을 정수로 변환하는 것 또한 명시적으로 변환해야 합니다.
정수 타입은 `Double`이나 `Float`로 초기화 될 수 있습니다:

```swift
let integerPi = Int(pi)
// integerPi equals 3, and is inferred to be of type Int
```

<!--
  - test: `typeConversion`

  ```swifttest
  -> let integerPi = Int(pi)
  /> integerPi equals \(integerPi), and is inferred to be of type Int
  </ integerPi equals 3, and is inferred to be of type Int
  ```
-->

부동소수점 값을 이런 방식으로 새로운 정수값으로 초기화할 때 항상 소수점 아래를 버림합니다.
이것은 `4.75`는 `4`, 그리고 `-3.9`는 `-3`이 된다는 의미입니다.

> Note: 숫자 상수와 변수를 결합하는 규칙은
> 숫자 리터럴 규칙과 다릅니다.
> 리터럴 값 `3`은 숫자 리터럴에 명시적인 타입이 없으므로
> 리터럴 값 `0.14159`에 직접 추가할 수 있습니다.
> 이것의 타입은 컴파일러가 실행되는 시점에서만 추론됩니다.

<!--
  NOTE: this section on explicit conversions could be included in the Operators section.
  I think it's more appropriate here, however,
  and helps to reinforce the “just use Int” message.
-->

## 타입 별칭 (Type Aliases)

*타입 별칭(Type aliases)*은 이미 존재하는 타입을 다른 이름으로 정의합니다.
타입 별칭은 `typealias` 키워드를 사용하여 정의할 수 있습니다.

타입 별칭은 외부 소스에서 특정 크기의 데이터로 작업할 때와 같이
상황에 맞는 이름으로
기존 타입을 참조하려는 경우에 유용합니다:

```swift
typealias AudioSample = UInt16
```

<!--
  - test: `typeAliases`

  ```swifttest
  -> typealias AudioSample = UInt16
  ```
-->

타입 별칭을 정의하면
원래 이름을 사용할 수 있는 모든 위치에서 별칭을 사용할 수 있습니다:

```swift
var maxAmplitudeFound = AudioSample.min
// maxAmplitudeFound is now 0
```

<!--
  - test: `typeAliases`

  ```swifttest
  -> var maxAmplitudeFound = AudioSample.min
  /> maxAmplitudeFound is now \(maxAmplitudeFound)
  </ maxAmplitudeFound is now 0
  ```
-->

여기서 `AudioSample`은 `UInt16`의 별칭으로 정의됩니다.
별칭이므로
`AudioSample.min`에 대한 호출은 실제로 `UInt16.min`을 호출하며,
`maxAmplitudeFound` 변수에 `0`이라는 초기값을 제공합니다.

## Booleans

Swift는 `Bool`이라 불리는 기본 *Boolean* 타입이 있습니다.
Boolean 값은 논리적 값이라고 불리는데,
이는 참 또는 거짓만 가질 수 있기 때문입니다.
Swift는 2개의 Boolean 상수 값인
`true`와 `false`를 제공합니다:

```swift
let orangesAreOrange = true
let turnipsAreDelicious = false
```

<!--
  - test: `booleans`

  ```swifttest
  -> let orangesAreOrange = true
  -> let turnipsAreDelicious = false
  ```
-->

`orangesAreOrange`와 `turnipsAreDelicious`의 타입은
Boolean 리터럴 값으로 초기화되었기 때문에
`Bool`로 추론되었습니다.
위의 `Int`와 `Double`에서와 같이
상수나 변수를 초기화 시 `true` 또는 `false`로 선언하면
이를 `Bool` 타입으로 선언할 필요가 없습니다.

Boolean 값은 `if` 구문과 같은
조건문으로 동작할 때 특히 유용합니다:

```swift
if turnipsAreDelicious {
    print("Mmm, tasty turnips!")
} else {
    print("Eww, turnips are horrible.")
}
// Prints "Eww, turnips are horrible."
```

<!--
  - test: `booleans`

  ```swifttest
  -> if turnipsAreDelicious {
        print("Mmm, tasty turnips!")
     } else {
        print("Eww, turnips are horrible.")
     }
  <- Eww, turnips are horrible.
  ```
-->

`if` 구문 같은 조건문은 [제어 흐름 (Control Flow)](./control-flow.md)에서 자세히 다룹니다.

Swift의 타입 안전성은 Boolean이 아닌 값이 `Bool`로 대체되는 것을 방지합니다.
아래 예시는 컴파일 시 오류를 발생시킵니다:

```swift
let i = 1
if i {
    // this example will not compile, and will report an error
}
```

<!--
  - test: `booleansNotBoolean`

  ```swifttest
  -> let i = 1
  -> if i {
        // this example will not compile, and will report an error
     }
  !$ error: type 'Int' cannot be used as a boolean; test for '!= 0' instead
  !! if i {
  !!   ^
  !! ( != 0)
  ```
-->

그러나 아래와 같은 예시는 정상 동작합니다:

```swift
let i = 1
if i == 1 {
    // this example will compile successfully
}
```

<!--
  - test: `booleansIsBoolean`

  ```swifttest
  -> let i = 1
  -> if i == 1 {
        // this example will compile successfully
     }
  ```
-->

`i == 1` 비교 결과는 `Bool` 타입이므로,
이 두번째 예시는 타입 검사를 정상적으로 수행할 수 있습니다.
`i == 1`과 같은 비교는 [기본 연산자 (Basic Operators)](./basic-operators.md)에서 설명합니다.

Swift의 타입 안전성에 대한 다른 예시처럼
이 방법은 실수로 인한 오류를 방지하고
코드 섹션의 의도가 항상 명확하게 전달되도록 보장합니다.

## 튜플 (Tuples)

*튜플(Tuples)*은 여러 값을 단일복합값으로 그룹화 합니다.
튜플안에 값은 어떠한 타입도 가능하며
서로 같은 타입일 필요는 없습니다.

이 예시에서 `(404, "Not Found")`은 *HTTP 상태 코드(HTTP status code)*를 나타내는 튜플입니다.
HTTP 상태 코드는 웹 페이지를 요청할 때마다 웹 서버가 반환하는 특정 값입니다.
`404 Not Found` 상태 코드는 요청한 웹 페이지가 존재하지 않을 때 반환됩니다.

```swift
let http404Error = (404, "Not Found")
// http404Error is of type (Int, String), and equals (404, "Not Found")
```

<!--
  - test: `tuples`

  ```swifttest
  -> let http404Error = (404, "Not Found")
  /> http404Error is of type (Int, String), and equals (\(http404Error.0), \"\(http404Error.1)\")
  </ http404Error is of type (Int, String), and equals (404, "Not Found")
  ```
-->

`(404, "Not Found")` 튜플은 HTTP 상태 코드에 두 개별 값인
`Int`와 `String`을 함께 그룹화하여 제공합니다:
숫자와 사람이 읽을 수 있는 설명을 제공합니다.
이것은 "튜플의 타입은 `(Int, String)` 입니다"라고 설명할 수 있습니다.

모든 타입의 조합으로 튜플을 만들 수 있으며,
원하는 만큼 다양한 타입을 포함할 수 있습니다.
`(Int, Int, Int)` 타입의 튜플이나 `(String, Bool)` 타입의 튜플,
또는 실제로 필요한 다른 어떠한 조합도
만들 수 있습니다.

튜플의 내용을 별도의 상수나 변수로 *분해(decompose)*하여
평소와 같이 접근할 수 있습니다:

```swift
let (statusCode, statusMessage) = http404Error
print("The status code is \(statusCode)")
// Prints "The status code is 404"
print("The status message is \(statusMessage)")
// Prints "The status message is Not Found"
```

<!--
  - test: `tuples`

  ```swifttest
  -> let (statusCode, statusMessage) = http404Error
  -> print("The status code is \(statusCode)")
  <- The status code is 404
  -> print("The status message is \(statusMessage)")
  <- The status message is Not Found
  ```
-->

튜플의 값 중 일부만 필요한 경우,
튜플을 분해할 때
밑줄(`_`)로 튜플의 일부를 무시할 수 있습니다:

```swift
let (justTheStatusCode, _) = http404Error
print("The status code is \(justTheStatusCode)")
// Prints "The status code is 404"
```

<!--
  - test: `tuples`

  ```swifttest
  -> let (justTheStatusCode, _) = http404Error
  -> print("The status code is \(justTheStatusCode)")
  <- The status code is 404
  ```
-->

또는,
0으로 시작하는 인덱스를 사용하여 튜플의 개별 요소 값에 접근할 수 있습니다:

```swift
print("The status code is \(http404Error.0)")
// Prints "The status code is 404"
print("The status message is \(http404Error.1)")
// Prints "The status message is Not Found"
```

<!--
  - test: `tuples`

  ```swifttest
  -> print("The status code is \(http404Error.0)")
  <- The status code is 404
  -> print("The status message is \(http404Error.1)")
  <- The status message is Not Found
  ```
-->

튜플을 정의할 때 튜플의 요소에 이름을 정할 수 있습니다:

```swift
let http200Status = (statusCode: 200, description: "OK")
```

<!--
  - test: `tuples`

  ```swifttest
  -> let http200Status = (statusCode: 200, description: "OK")
  ```
-->

튜플 요소에 이름이 있다면,
요소의 값에 요소 이름을 사용하여 접근이 가능합니다:

```swift
print("The status code is \(http200Status.statusCode)")
// Prints "The status code is 200"
print("The status message is \(http200Status.description)")
// Prints "The status message is OK"
```

<!--
  - test: `tuples`

  ```swifttest
  -> print("The status code is \(http200Status.statusCode)")
  <- The status code is 200
  -> print("The status message is \(http200Status.description)")
  <- The status message is OK
  ```
-->

튜플은 특히 함수의 반환값으로 유용합니다.
예를 들어 웹 페이지를 가져오는 함수는 페이지 가져오기의 성공 또는 실패를 설명하기위해
`(Int, String)` 튜플 타입을 반환할 수 있습니다.
이 함수는 각각 다른 타입의
두 개의 값을 반환함으로써,
단일 타입의 단일 값만 반환할 수 있는 경우보다
유용합니다.
자세한 내용은 [여러 개의 반환값이 있는 함수 (Functions with Multiple Return Values)](./functions.md#여러-개의-반환값이-있는-함수-functions-with-multiple-return-values)를 참조 바랍니다.

> Note: 튜플은 관련된 값의 간단한 그룹에 유용합니다.
> 복잡한 데이터 구조를 생성하는데는 맞지 않습니다.
> 데이터 구조가 복잡한 경우
> 튜플이 아닌 클래스(class)나 구조체(structure)를 사용하세요.
> 자세한 내용은 [구조체와 클래스 (Classes And Structures)](./structures-and-classes.md)를 참조 바랍니다.

## 옵셔널 (Optionals)

값이 없는 경우에 *옵셔널(optional)*을 사용합니다.
옵셔널은 2가지 가능성이 있습니다:
지정된 타입의 값이 *있고*
옵셔널을 풀어서 값에 접근하거나
값이 *없을* 수도 있습니다.

누락될 수 있는 값의 예로
Swift의 `Int` 타입은
`String` 값을 `Int` 값으로 변환하는 초기화가 존재합니다.
그러나 일부 문자열만 정수로 변환할 수 있습니다.
문자열 `"123"`은 숫자값 `123`으로 변환될 수 있지만,
문자열 `"hello, world"`는 변환할 숫자값이 없습니다.
아래 예시는 `String`을 `Int`로 초기화하는 것을 보여줍니다:

```swift
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)
// The type of convertedNumber is "optional Int"
```

<!--
  - test: `optionals`

  ```swifttest
  -> let possibleNumber = "123"
  -> let convertedNumber = Int(possibleNumber)
  // convertedNumber is inferred to be of type "Int?", or "optional Int"
  >> print(type(of: convertedNumber))
  << Optional<Int>
  ```
-->

위의 코드에서 초기화가 실패할 수 있으므로,
`Int`가 아닌 *옵셔널* `Int`를 반환합니다.

옵셔널 타입을 작성하려면,
옵셔널을 포함하는 타입의 이름 다음에
물음표(`?`)를 작성합니다 ---
예를 들어, 옵셔널 `Int`의 타입은 `Int?`입니다.
옵셔널 `Int`는 항상 어떠한 `Int` 값
또는 값이 없음을 포함합니다.
`Bool`이나 `String` 값과 같은 다른 타입을 포함할 수 없습니다.

### nil

옵셔널 변수에 특수한 값 `nil`로 지정하여
값이 없는 상태를 나타낼 수 있습니다:

```swift
var serverResponseCode: Int? = 404
// serverResponseCode contains an actual Int value of 404
serverResponseCode = nil
// serverResponseCode now contains no value
```

<!--
  - test: `optionals`

  ```swifttest
  -> var serverResponseCode: Int? = 404
  /> serverResponseCode contains an actual Int value of \(serverResponseCode!)
  </ serverResponseCode contains an actual Int value of 404
  -> serverResponseCode = nil
  // serverResponseCode now contains no value
  ```
-->

기본값이 없이 옵셔널 변수를 정의하면,
이 변수는 자동적으로 `nil`로 설정됩니다:

```swift
var surveyAnswer: String?
// surveyAnswer is automatically set to nil
```

<!--
  - test: `optionals`

  ```swifttest
  -> var surveyAnswer: String?
  // surveyAnswer is automatically set to nil
  ```
-->

`if` 구문은 옵셔널이 값을 포함하고 있는지 여부를
`nil`과 비교하여 확인할 수 있습니다.
이 비교는 "같음" 연산자(`==`)
또는 "같지 않음" 연산자(`!=`)를 사용하여 수행됩니다.

옵셔널에 값이 있다면 `nil`과 "같지 않음"으로 간주됩니다:

```swift
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)

if convertedNumber != nil {
    print("convertedNumber contains some integer value.")
}
// Prints "convertedNumber contains some integer value."
```

<!--
  - test: `optionals`

  ```swifttest
  -> if convertedNumber != nil {
        print("convertedNumber contains some integer value.")
     }
  <- convertedNumber contains some integer value.
  ```
-->

옵셔널이 아닌 상수나 변수에는 `nil`을 사용할 수 없습니다.
코드에서 상수나 변수가 특정 조건에서
값이 없이 동작하려면,
적절한 타입의 옵셔널 값을 선언해야합니다.
옵셔널이 아닌 값으로 선언된 상수나 변수는
`nil` 값이 포함되지 않도록 보장됩니다.
옵셔널이 아닌 값에 `nil`을 할당하면,
컴파일 오류가 발생합니다.

옵셔널과 옵셔널이 아닌 값의 분리는
누락된 정보를 명시적으로 표시할 수 있고,
누락된 값을 처리하는 코드를 더 쉽게 작성할 수 있습니다.
옵셔널을 옵셔널이 아닌 값으로 처리하는 실수는 방지하기 위해
컴파일 때 오류가 발생합니다.
값을 언래핑한 후에는
`nil`에 대한 값을 검사할 필요가 없으므로
코드의 다른 부분에서
이 값을 동일하게 확인할 필요가 없습니다.

옵셔널 값에 접근할 때
코드에서 `nil`과 `nil`이 아닌 것에 대해 모두 처리해야 합니다.
값이 없을 때 수행할 수 있는 작업이 몇가지 있으며,
이는 다음 섹션에서 설명합니다:

- 값이 `nil`일 때 해당 값에 대한 동작을 건너뜁니다.

- `nil` 값을 전파하는 방법으로,
  `nil` 을 반환하거나
  [옵셔널 체이닝 (Optional Chaining)](./optional-chaining.md)에서 설명한 `?.` 연산자를 사용할 수 있습니다.

- `??` 연산자를 사용해서 대체값을 제공합니다.

- `!` 연산자를 사용해서 프로그램을 멈춥니다.

> Note:
> Objective-C에서 `nil`은 객체가 존재하지 않는 포인터입니다.
> Swift에서 `nil`은 포인터가 아닙니다 --- 특정 타입의 값이 없다는 의미입니다.
> *모든* 타입의 옵셔널은 객체 타입 뿐만 아니라 `nil`로 설정할 수 있습니다.

### 옵셔널 바인딩 (Optional Binding)

옵셔널 바인딩(optional binding)은 옵셔널이 값을 포함하고 있는지 확인하고
값이 있는 경우 해당 값을 임시로 상수나 변수로 사용할 수 있게 해줍니다.
옵셔널 바인딩은 `if`, `guard`, `while` 구문에서
옵셔널에 값이 있는지 체크하고
단일 동작의 일부로
상수나 변수로 추출할 수 있습니다.
`if`, `guard`, `while` 구문은
[제어 흐름 (Control Flow)](./control-flow.md)에서 자세히 다룹니다.

`if` 구문에서 옵셔널 바인딩은 아래와 같이 사용합니다:

```swift
if let <#constantName#> = <#someOptional#> {
   <#statements#>
}
```

강제 언래핑(forced unwrapping)보다 옵셔널 바인딩을 사용하여
[옵셔널 (Optionals)](#옵셔널-optionals) 섹선에 있는
예시의 `possibleNumber`를 다시 작성할 수 있습니다:

```swift
if let actualNumber = Int(possibleNumber) {
    print("The string \"\(possibleNumber)\" has an integer value of \(actualNumber)")
} else {
    print("The string \"\(possibleNumber)\" could not be converted to an integer")
}
// Prints "The string "123" has an integer value of 123"
```

<!--
  - test: `optionals`

  ```swifttest
  -> if let actualNumber = Int(possibleNumber) {
        print("The string \"\(possibleNumber)\" has an integer value of \(actualNumber)")
     } else {
        print("The string \"\(possibleNumber)\" couldn't be converted to an integer")
     }
  <- The string "123" has an integer value of 123
  ```
-->

이 코드는 아래와 같이 읽을 수 있습니다:

"`Int(possibleNumber)`에 의해 반환된 옵셔널 `Int`에 값이 포함되어 있으면,
`actualNumber`라는 새로운 상수에 옵셔널에 포함된 값을 설정합니다."

변환에 성공하면,
`actualNumber` 상수는
`if` 구문의 첫번째 중괄호 내에서 사용할 수 있습니다.
옵셔널에서 포함된 값으로 초기화되었고,
옵셔널이 아닌 타입입니다.
이런 경우에 `possibleNumber`의 타입은 `Int?`이므로
`actualNumber`의 타입은 `Int`입니다.

값에 접근한 후에
기존 옵셔널 상수나 변수를 참조할 필요가 없다면
같은 이름으로 새로운 상수 또는 변수를 사용할 수 있습니다:

```swift
let myNumber = Int(possibleNumber)
// Here, myNumber is an optional integer
if let myNumber = myNumber {
    // Here, myNumber is a non-optional integer
    print("My number is \(myNumber)")
}
// Prints "My number is 123"
```

<!--
  - test: `optionals`

  ```swifttest
  -> let myNumber = Int(possibleNumber)
  // Here, myNumber is an optional integer
  -> if let myNumber = myNumber {
         // Here, myNumber is a non-optional integer
         print("My number is \(myNumber)")
     }
  <- My number is 123
  ```
-->

이 코드는 이전 예시에서의 코드와 마찬가지로
`myNumber`가 값이 있는지 없는지를 먼저 확인합니다.
`myNumber`가 값이 있다면,
해당 값이 새로운 `myNumber`라는 상수에 설정됩니다.
`if` 구문 안에서
`myNumber`는 새로운 옵셔널이 아닌 상수를 참조합니다.
`if` 구문의 앞이나 뒤에서
`myNumber`를 작성하면 원래 옵셔널 정수 상수를 참조합니다.

이러한 코드는 일반적이기 때문에
옵셔널 값을 언래핑 하는데 더 짧게 사용할 수 있습니다:
언래핑할 상수나 변수의 이름만 작성합니다.
언래핑된 새로운 상수나 변수는
암시적으로 같은 이름의 옵셔널 값을 사용합니다.

```swift
if let myNumber {
    print("My number is \(myNumber)")
}
// Prints "My number is 123"
```

<!--
  - test: `optionals`

  ```swifttest
  -> if let myNumber {
         print("My number is \(myNumber)")
     }
  <- My number is 123
  ```
-->

옵셔널 바인딩은 상수와 변수 둘다 사용이 가능합니다.
`if` 구문에 첫번째 중괄호에서
`myNumber`의 값을 변경하고 싶다면
`if var myNumber`로 대신 쓸 수 있으며
옵셔널에 포함된 값은
상수가 아닌 변수로 사용할 수 있습니다.
`if` 구문내에서 `myNumber`를 바꾸면
이것은 지역 변수에만 적용되며,
기존 옵셔널 상수나 변수에 적용되지 *않습니다*.

필요한 경우 `if` 구문에
쉼표로 구분하여
옵셔널 바인딩 및 Boolean 조건을 여러개 포함할 수 있습니다.
옵셔널 바인딩 값 중 하나가 `nil`이거나
Boolean 조건이 `false`로 판단되면
전체 `if` 구문의 조건은
`false`로 간주됩니다.
다음의 예는 같은 `if` 구문입니다:

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

<!--
  - test: `multipleOptionalBindings`

  ```swifttest
  -> if let firstNumber = Int("4"), let secondNumber = Int("42"), firstNumber < secondNumber && secondNumber < 100 {
        print("\(firstNumber) < \(secondNumber) < 100")
     }
  <- 4 < 42 < 100

  -> if let firstNumber = Int("4") {
         if let secondNumber = Int("42") {
             if firstNumber < secondNumber && secondNumber < 100 {
                 print("\(firstNumber) < \(secondNumber) < 100")
             }
         }
     }
  <- 4 < 42 < 100
  ```
-->

<!--
  The example above uses multiple optional bindings
  to show that you can have more than one
  and to show the short-circuiting behavior.
  It has multiple Boolean conditions
  to show that you should join logically related conditions
  using the && operator instead of a comma.
-->

`if` 구문에서 옵셔널 바인딩으로 생성된 상수와 변수는
`if` 구문의 본문 내에서만 사용 가능합니다.
반대로 `guard` 구문에서 생성된 상수와 변수는
[이른 종료 (Early Exit)](./control-flow.md#이른-종료-early-exit)에서 설명되어 있듯이 
`guard` 구문 다음에 코드에서 사용 가능합니다.

### 대체값 제공 (Providing a Fallback Value)

누락된 값을 처리하는 다른 방법은 
nil-결합 연산자(`??`)를 사용하여 기본값을 제공하는 방법입니다.
옵셔널에서 `??`의 왼편이 `nil`이 아니면,
값은 언래핑되고 사용됩니다.
그렇지 않으면 `??`의 오른편에 값이 사용됩니다.
예를 들어,
아래 코드는 이름이 지정되면 지정된 이름에 인사하고,
이름이 `nil`이면 일반적인 인사를 사용합니다.

```swift
let name: String? = nil
let greeting = "Hello, " + (name ?? "friend") + "!"
print(greeting)
// Prints "Hello, friend!"
```

<!--
.. testcode:: optionalFallback

   ```swifttest
   -> let name: String? = nil
   -> let greeting = "Hello, " + (name ?? "friend") + "!"
   -> print(greeting)
   <- Hello, friend!
   ```
-->

대체 값 제공하기 위한 `??` 사용에 대한 더 자세한 내용은
[Nil-병합 연산자 (Nil-Coalescing Operator)](./basic-operators.md#nil-병합-연산자-nil-coalescing-operator)를 참고바랍니다.

### 강제 언래핑 (Force Unwrapping)

프로그래머의 오류나 원치않는 상태와 같은
실패를 `nil`로 표현하려면,
옵셔널의 이름 뒤에 느낌표(`!`)를 추가하여
접근할 수 있습니다.
이것을 옵셔널 값의 *강제 언래핑(force unwrapping)*이라고 합니다.
`nil`이 아닌 값에 강제 언래핑을 하면,
언래핑된 값을 결과로 얻습니다.
`nil` 값을 강제 언래핑하면 런타임 오류가 발생합니다.

`!`은 [`fatalError(_:file:line:)`][]의 짧은 표현입니다.
예를 들어, 아래 코드는 동일한 접근을 보여줍니다:

[`fatalError(_:file:line:)`]: https://developer.apple.com/documentation/swift/fatalerror(_:file:line:)

```swift
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)

let number = convertedNumber!

guard let number = convertedNumber else {
    fatalError("The number was invalid")
}
```

위 코드의 두 버전은 값을 항상 가지는
`convertedNumber`에 의존하는 것을 보여줍니다.
위의 접근방식 중 하나를 사용해서
코드에 작성하면,
실행 중에 요구사항이 참인지 확인할 수 있습니다.

데이터 요구사항을 강제하고
실행 중에 가정을 확인하는 방법에 대한 자세한 내용은
[어설션과 전제 조건 (Assertions and Preconditions)](#어설션과-전제-조건-assertions-and-preconditions)을 참고바랍니다.

### 암시적 언래핑 옵셔널 (Implicitly Unwrapped Optionals)

위에서 설명한대로,
옵셔널은 상수나 변수가 "값이 없음"을 허용합니다.
옵셔널은 값이 존재하는지 `if` 구문에서 체크할 수 있고
값이 존재한다면
옵셔널 값에 접근하기위해 옵셔널 바인딩을 이용할 수 있습니다.

때로는 프로그램 구조에서 옵셔널이 값을 처음 설정한 후에는
항상 값을 갖는 것은 분명합니다.
이러한 경우 항상 값이 있다고 가정할 수 있으므로
접근할 때마다 옵셔널의 값을 확인하고
언래핑 할 필요가 없습니다.

이러한 옵셔널은 *암시적 언래핑 옵셔널(implicitly unwrapped optionals)*이라고 합니다.
옵셔널을 만들기 위해 타입뒤에 물음표(`String?`)를 작성하는 대신에
느낌표(`String!`)로 암시적 언래핑 옵셔널을 작성합니다.
사용할 때 옵셔널 이름의 뒤에 느낌표를 위치시키는 것보다
선언할 때 옵셔널 타입 뒤에 느낌표를 위치시키는 것이 더 좋습니다.

암시적 언래핑 옵셔널은
옵셔널의 값이 처음 정의된 직후에 존재하는 것으로 확인되고
그 이후 모든 시점에 존재한다고 가정할 수 있는 경우에 유용합니다.
Swift에서 암시적 언래핑 옵셔널은
[미소유 참조와 암시적 언래핑 옵셔널 프로퍼티 (Unowned References and Implicitly Unwrapped Optional Properties)](./automatic-reference-counting.md#미소유-참조와-암시적-언래핑-옵셔널-프로퍼티-unowned-references-and-implicitly-unwrapped-optional-properties)에서 설명한 대로 클래스 초기화 중에 주로 사용합니다.

나중에 변수가 `nil`이 될 가능성이 있다면
암시적 언래핑 옵셔널을 사용하면 안됩니다.
변수를 사용하는 동안
`nil` 값을 확인해야 한다면 일반적인 옵셔널 타입을 사용해야 합니다.

암시적 언래핑 옵셔널은 내부적으로 옵셔널이지만,
옵셔널에 접근할 때마다 옵셔널 값을 언래핑할 필요없이
옵셔널이 아닌 값처럼 사용할 수 있습니다.
다음 예시는 명시적 `String`로서 값에 접근할 때
옵셔널 문자열과 암시적 언래핑 옵셔널 문자열의
동작 차이를 보여줍니다:

```swift
let possibleString: String? = "An optional string."
let forcedString: String = possibleString! // Requires explicit unwrapping

let assumedString: String! = "An implicitly unwrapped optional string."
let implicitString: String = assumedString // Unwrapped automatically
```

<!--
  - test: `implicitlyUnwrappedOptionals`

  ```swifttest
  -> let possibleString: String? = "An optional string."
  -> let forcedString: String = possibleString! // requires an exclamation point

  -> let assumedString: String! = "An implicitly unwrapped optional string."
  -> let implicitString: String = assumedString // no need for an exclamation point
  ```
-->

암시적 언래핑 옵셔널은
필요할 경우 옵셔널을 강제로 언래핑할 수 있도록 허용한다고 생각할 수 있습니다.
암시적 언래핑 옵셔널을 사용할 때,
Swift는 처음에 기존의 옵셔널 값으로 사용하려고 합니다;
사용이 불가능할 경우, Swift는 값을 강제로 언래핑합니다.
위의 코드에서
옵셔널 값 `assumedString`은
`implicitString`이 명시적으로 옵셔널이 아닌 `String` 타입이기 때문에
`implicitString`에 값을 할당하기 전에 강제로 언래핑됩니다.
아래의 코드에서
`optionalString`은 명시적 타입이 없으므로
일반 옵셔널로 취급합니다.

```swift
let optionalString = assumedString
// The type of optionalString is "String?" and assumedString isn't force-unwrapped.
```

<!--
  - test: `implicitlyUnwrappedOptionals`

  ```swifttest
  -> let optionalString = assumedString
  // The type of optionalString is "String?" and assumedString isn't force-unwrapped.
  >> print(type(of: optionalString))
  << Optional<String>
  ```
-->

암시적 언래핑 옵셔널이 `nil`이고 값에 접근하려고 하면
런타임 오류가 발생합니다.
결과는 값을 포함하지 않는 일반적인 옵셔널을
강제 언래핑을 하기위해 느낌표를 작성한 것과 같습니다.

암시적 언래핑 옵셔널은
일반 옵셔널과 같은 방법으로 `nil` 체크를 할 수 있습니다:

```swift
if assumedString != nil {
    print(assumedString!)
}
// Prints "An implicitly unwrapped optional string."
```

<!--
  - test: `implicitlyUnwrappedOptionals`

  ```swifttest
  -> if assumedString != nil {
        print(assumedString!)
     }
  <- An implicitly unwrapped optional string.
  ```
-->

옵셔널 바인딩과 함께 암시적 언래핑 옵셔널은
단일 구문으로 해당 값을 확인하고 언래핑할 수 있습니다.

```swift
if let definiteString = assumedString {
    print(definiteString)
}
// Prints "An implicitly unwrapped optional string."
```

<!--
  - test: `implicitlyUnwrappedOptionals`

  ```swifttest
  -> if let definiteString = assumedString {
        print(definiteString)
     }
  <- An implicitly unwrapped optional string.
  ```
-->

## 메모리 안전성 (Memory Safety)

위의 [타입 안전성과 타입 추론 (Type Safety and Type Inference)](#타입-안전성과-타입-추론-type-safety-and-type-inference)에서 설명한대로
타입 불일치를 방지하기 위한 검사 외에도
Swift는 유효하지 않은 메모리 작업을 방지하도록 코드를 보호합니다.
이 보호는 *메모리 안전성(memory safety)*라고 하며
다음의 요구사항을 포함합니다:

- 값은 읽기 전에 설정됩니다.
  초기화되지 않은 메모리 영역과의 상호작용을 방지하는 이 보호를
  *확정 초기화(definite intialization)*라고도 합니다.
- 배열과 버퍼는 유효한 인덱스로만 접근됩니다.
  경계를 벗어난 접근을 방지하는 이 보호를
  *경계 안전성(bounds safety)*라고도 합니다.
- 메모리는 값의 수명 중에만 접근됩니다.
  할당 해제 후 사용(use-after-free) 오류를 방지하는 이 보호를
  *수명 안전성(lifetime safety)*라고도 합니다.
- 메모리 접근은 증명 가능한 안전한 방식으로만 겹칩니다.
  동시성 코드에서 발생 가능한 데이터 경쟁을 방지하는 이 보호를
  *스레드 안전성(thread safety)*라고도 합니다.

이러한 보장을 제공하지 않는 언어에서 작업한다면,
위에 나열된
오류와 버그에 대해 익숙할 것입니다.
이러한 문제를 겪어보지 않았더라도 괜찮습니다;
Swift의 안전한 코드는 이러한 문제를 피합니다.
Swift가 어떻게 초기값 설정을 보장하는지에 대한 자세한 내용은
[초기화 (Initialization)](./initialization.md)를 참고하고,
Swift가 어떻게 동시성 코드에서 메모리 안전성을 검사하는지에 대한 자세한 내용은
[동시성 (Concurrency)](./concurrency.md)을 참고하고,
Swift가 어떻게 메모리의 겹치는 접근을 검사하는지에 대한 자세한 내용은
[메모리 안전성 (Memory Safety)](./memory-safety.md)을 참고바랍니다.

때로는 예를 들어 언어나 표준 라이브러리의 한계 때문에
안전성의 경계 밖에서 작업을 해야할 수도 있습니다.
이러한 경우에 Swift는 일부 API의 안전하지 않은 버전을 제공합니다.
이름에 "unsafe", "unchecked", "unmanaged"와 같은
단어를 포함하는 타입이나 메서드를 사용할 때는,
안전에 대한 책임을 직접 처리해야 합니다.

Swift의 안전한 코드도 오류와 예기치 않은 실패로
프로그램의 실행이 중단될 수 있습니다.
안전성이 코드의 완벽한 실행을 보장하지 않습니다.
Swift는 아래 [오류 처리 (Error Handling)](#오류-처리-error-handling)와
[어설션과 전제 조건 (Assertions and Preconditions)](#어설션과-전제-조건-assertions-and-preconditions)에서 설명한대로
오류를 나타내고 복구하는 여러 방법을 제공합니다.
그러나 일부 경우에
오류를 처리하는 *유일한* 방법은 실행을 중단하는 것입니다.
서비스가 예기치 않게 중단되지 않아야 한다면,
구성요소 중 하나가 예기치 않게 중단되더라도 복구할 수 있도록
전체 아키텍처에 내결함성을 통합해야 합니다.

## 오류 처리 (Error Handling)

*오류 처리(error handling)*는 프로그램 실행 중에
프로그램이 만날 수 있는 오류 조건에 대응하기 위해 사용됩니다.

값의 존재 유무를 사용하여
함수의 성공 또는 실패를 전달할 수 있는
옵셔널과 달리
오류 처리는 오류 원인을 판별하고
필요한 경우 오류를 프로그램의 다른 부분으로 전파할 수 있습니다.

함수가 오류 조건을 만나면, 그 함수는 오류를 *발생*시킵니다.
해당 함수의 호출자는 오류를 *포착*하고 적절하게 응답할 수 있습니다.

```swift
func canThrowAnError() throws {
    // this function may or may not throw an error
}
```

<!--
  - test: `errorHandling`

  ```swifttest
  >> enum SimpleError: Error {
  >>    case someError
  >> }
  >> let condition = true
  -> func canThrowAnError() throws {
        // this function may or may not throw an error
  >>    if condition {
  >>       throw SimpleError.someError
  >>    }
     }
  ```
-->

함수는 선언에 `throws` 키워드를 포함시켜
오류가 발생할 수 있음을 나타냅니다.
오류를 발생할 수 있는 함수를 호출할 때는
표현식 앞에 `try` 키워드를 붙여야 합니다.

Swift는 `catch` 절에 의해 처리될 때까지
현재 범위에서 오류를 자동으로 전파합니다.

```swift
do {
    try canThrowAnError()
    // no error was thrown
} catch {
    // an error was thrown
}
```

<!--
  - test: `errorHandling`

  ```swifttest
  -> do {
  ->    try canThrowAnError()
  >>    print("No Error")
  ->    // no error was thrown
  -> } catch {
  >>    print("Error")
  ->    // an error was thrown
  -> }
  << Error
  ```
-->

`do` 구문은 새로운 범위를 생성하여
오류가 하나 이상의 `catch` 절로 전파될 수 있도록 합니다.

다음은 오류 처리로
다양한 오류 조건에 대응하는 방법의 예입니다:

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

<!--
  - test: `errorHandlingTwo`

  ```swifttest
  >> enum SandwichError: Error {
  >>     case outOfCleanDishes
  >>     case missingIngredients([String])
  >> }
  >> func washDishes() { print("Wash dishes") }
  >> func buyGroceries(_ shoppingList: [String]) { print("Buy \(shoppingList)") }
  -> func makeASandwich() throws {
         // ...
     }
  >> func eatASandwich() {}

  -> do {
         try makeASandwich()
         eatASandwich()
     } catch SandwichError.outOfCleanDishes {
         washDishes()
     } catch SandwichError.missingIngredients(let ingredients) {
         buyGroceries(ingredients)
     }
  ```
-->

이 예시에서 `makeASandwich()` 함수는
깨끗한 그릇을 사용할 수 없거나
재료가 없는 경우 오류가 발생할 것입니다.
`makeASandwich()` 함수는 오류를 발생할 수 있으므로,
`try` 표현식으로 래핑되어 호출합니다.
함수 호출을 `do` 구문으로 래핑하면
모든 오류는
`catch` 절로 전파됩니다.

오류가 발생하지 않으면 `eatASandwich()` 함수는 호출됩니다.
`SandwichError.outOfCleanDishes` 오류가 발생하면,
`washDishes()` 함수가 호출됩니다.
`SandwichError.missingIngredients` 오류가 발생하면,
`catch` 패턴에 의해 캡쳐된 `[String]` 값과 함께
`buyGroceries(_:)` 함수가 호출됩니다.

오류 발생, 포착, 전파는
[오류 처리 (Error Handling)](./error-handling.md)에서 자세히 다룹니다.

## 어설션과 전제 조건 (Assertions and Preconditions)

*어설션(Assertion)*과 *전제 조건(Precondition)*은
실행 중에 이루어지는 검사입니다.
추가 코드를 실행하기 전에
이를 사용하여 필수 조건이 충족되는지 확인할 수 있습니다.
어설션이나 전제 조건의 Boolean 조건이
`true`이면
코드는 평소와 같이 실행됩니다.
조건이 `false`로 판단되면,
프로그램의 현재 상태는 유효하지 않습니다;
코드 실행은 종료되고 앱은 종료됩니다.

어설션과 전제 조건은
가정과
기대치를 표현하므로
코드의 일부로
포함할 수 있습니다.
어설션은 개발 과정에서 실수와 잘못된 가정을 찾는데 도움이 되고,
전제 조건은 프로덕션 환경에서 문제를 감지하는데 도움이 됩니다.

실행 중에 예상한 값을 검사하는 것 외에도
어설션과 전제 조건은 코드 내에서
유용한 문서 형식이 됩니다.
위의 [오류 처리 (Error Handling)](#오류-처리-error-handling)와 다르게
어설션과 전제 조건은 복구 가능하거나
예상되는 오류에 사용되지 않습니다.
실패한 어설션이나 전제 조건은
유효하지 않은 프로그램 상태를 나타내기 때문에
실패한 상태를 처리할 방법은 없습니다.
유효하지 않은 상태에서 복구하는 것은 불가능합니다.
어설션이 실패하면,
프로그램의 데이터 중 하나가 유효하지 않다는 의미입니다 ---
그러나 그것이 왜 유효하지 않거나
다른 상태도 유효하지 않은지 알 수 없습니다.

어설션과 전제조건을 사용하는 것은
유효하지 않는 조건이 발생하지 않게
코드를 디자인하기 위함입니다.
그러나
유효한 데이터 및 상태를 강제하는데 어설션과 전제 조건을 사용하면
잘못된 상태가 발생할 경우
앱이 더 예측 가능하게 종료되고
문제를 디버깅하는데 도움이 됩니다.
가정이 검사되지 않으면,
다른 코드가 실패하기 시작하고,
사용자 데이터가 손상된 후에야
이런 종류의 문제를 알 수 있습니다.
유효하지 않은 상태가 감지되는 즉시 실행을 중지하면
해당 유효하지 않은 상태로 인한 피해를 제한하는데 도움이 됩니다.

어설션과 전제 조건의 차이점은 언제 체크되는지에 있습니다:
어설션은 오직 디버그 빌드에서 체크되지만
전제 조건은 디버그와 프로덕션 빌드에서 체크됩니다.
프로덕션 빌드일 때,
어설션 내부의 조건은 실행되지 않습니다.
이 의미는 프로덕션에서 성능의 영향이 없이
개발 단계에서
많은 양의 어설션을 사용할 수 있다는 뜻입니다.

### 어설션을 통한 디버깅 (Debugging with Assertions)

<!--
  If your code triggers an assertion while running in a debug environment,
  such as when you build and run an app in Xcode,
  you can see exactly where the invalid state occurred
  and query the state of your app at the time that the assertion was triggered.
  An assertion also lets you provide a suitable debug message as to the nature of the assert.
-->

Swift 표준 라이브러리에
[`assert(_:_:file:line:)`](https://developer.apple.com/documentation/swift/1541112-assert) 함수로
어설션을 작성할 수 있습니다.
이 함수에 `true`나 `false`로 판단될 표현식과
조건이 `false`일 경우 출력될 메세지를 전달합니다.
예를 들어:

```swift
let age = -3
assert(age >= 0, "A person's age can't be less than zero.")
// This assertion fails because -3 is not >= 0.
```

<!--
  - test: `assertions-1`

  ```swifttest
  -> let age = -3
  -> assert(age >= 0, "A person's age can't be less than zero.")
  xx assert
  // This assertion fails because -3 isn't >= 0.
  ```
-->

이 예에서 코드는 `age`가 음수가 아니고
`age >= 0`이 `true`일 경우 이어서 실행됩니다.
`age`가 음수이면
`age >= 0`은 `false`가 되고,
어설션은 실패되고 애플리케이션은 종료됩니다.

어설션 메세지를 생략할 수 있습니다 ---
예를 들어 평범하게 조건만 반복되는 경우에 사용합니다.

```swift
assert(age >= 0)
```

<!--
  - test: `assertions-2`

  ```swifttest
  >> let age = -3
  -> assert(age >= 0)
  xx assert
  ```
-->

<!--
  - test: `assertionsCanUseStringInterpolation`

  ```swifttest
  -> let age = -3
  -> assert(age >= 0, "A person's age can't be less than zero, but value is \(age).")
  xx assert
  ```
-->

이미 코드에서 조건을 검사하고 있는 경우,
[`assertionFailure(_:file:line:)`](https://developer.apple.com/documentation/swift/1539616-assertionfailure) 함수를
사용하여
어설션이 실패했음을 나타낼 수 있습니다.
예를 들어:

```swift
if age > 10 {
    print("You can ride the roller-coaster or the ferris wheel.")
} else if age >= 0 {
    print("You can ride the ferris wheel.")
} else {
    assertionFailure("A person's age can't be less than zero.")
}
```

<!--
  - test: `assertions-3`

  ```swifttest
  >> let age = -3
  -> if age > 10 {
         print("You can ride the roller-coaster or the ferris wheel.")
     } else if age >= 0 {
         print("You can ride the ferris wheel.")
     } else {
         assertionFailure("A person's age can't be less than zero.")
     }
  xx assert
  ```
-->

### 강제 전제 조건 (Enforcing Preconditions)

조건이 거짓일 가능성이 있지만
코드 실행을 계속하려면 *반드시* 참이어야 하는 경우 전제 조건을 사용합니다.
예를 들어, 어떤 값들이 범위를 벗어나는지
또는 함수에 유효한 값이 전달되는지 체크하기 위해 전제 조건을 사용합니다.

[`precondition(_:_:file:line:)`](https://developer.apple.com/documentation/swift/1540960-precondition) 함수로
전제 조건을 작성할 수 있습니다.
이 함수에 `true`나 `false`로 판단될 표현식과
조건이 `false`일 경우 출력될 메세지를 전달합니다.
예를 들어:

```swift
// In the implementation of a subscript...
precondition(index > 0, "Index must be greater than zero.")
```

<!--
  - test: `preconditions`

  ```swifttest
  >> let index = -1
  // In the implementation of a subscript...
  -> precondition(index > 0, "Index must be greater than zero.")
  xx assert
  ```
-->

[`preconditionFailure(_:file:line:)`](https://developer.apple.com/documentation/swift/1539374-preconditionfailure) 함수를
호출하여
실패가 발생했음을 나타낼 수 있습니다 ---
예를 들어, 스위치 문에서 기본 케이스가 선택되었지만,
모든 유효한 입력 데이터는
스위치의 다른 케이스 중 하나에서 처리되어야 하는 경우에 사용할 수 있습니다.

> Note: 체크하지 않는 모드(`-Ounchecked`)로 컴파일하면,
> 전제 조건을 검사하지 않습니다.
> 컴파일러는 전제 조건이 항상 참이라고 가정하고
> 코드를 알맞게 최적화 합니다.
> 그러나 `fatalError(_:file:line:)` 함수는 최적화 설정과 무관하게
> 항상 중지를 실행합니다.
>
> 프로토타입과 초기 개발 단계에서
> 아직 구현되지 않은 기능에서
> `fatalError(_:file:line:)`을 사용할 수 있으며
> `fatalError("Unimplemented")`와 같이 작성할 수 있습니다.
> 어설션이나 전제 조건과 다르게
> 치명적인 오류는
> 절대 최적화 되지 않기 때문에
> 이 구현을 만나면 항상 중지됩니다.

<!--
  "\ " in the first cell below lets it be empty.
  Otherwise RST treats the row as a continuation.

  ============ =====  ==========  ===============================
  \            Debug  Production  Production with ``-Ounchecked``
  ============ =====  ==========  ===============================
  Assertion    Yes    No          No
  ------------ -----  ----------  -------------------------------
  Precondition Yes    Yes         No
  ------------ -----  ----------  -------------------------------
  Fatal Error  Yes    Yes         Yes
  ============ =====  ==========  ===============================
-->

<!--
  TODO: In Xcode, can you set a breakpoint on assertion/precondition failure?
  If so, mention that fact and give a link to a guide that shows you how.
  In LLDB, 'breakpoint set -E swift' catches when errors are thrown,
  but doesn't stop at assertions.
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