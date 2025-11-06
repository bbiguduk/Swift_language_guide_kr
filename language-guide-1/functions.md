{% hint style="info" %}
📢 **공지**  
이 문서는 더 이상 업데이트되지 않습니다.  
아래 새로운 URL로 업데이트 되었으니, 새로운 URL로 접속해 주세요.  
[Swift Book Korean](https://bbiguduk.github.io/swift-book-korean/documentation/tsplk/)
{% endhint %}

# 함수 (Functions)

함수를 정의하고 호출하며, 인자에 레이블을 지정하고 반환값을 사용합니다.

*함수(Functions)*는 특정 작업을 수행하는 독립적인 코드 블록입니다.
무슨 동작을 하는지 함수에 특정 이름을 부여할 수 있으며,
작업을 수행하기 위해 함수를 "호출"할 때 사용합니다.

Swift의 통합된 함수 문법은 매개변수 이름이 없는 단순한 C 스타일 함수에서
각 매개변수에 대한 이름과 인자 레이블이 있는
복잡한 Objective-C 스타일 메서드에 이르기까지
모든 것을 표현할 수 있을 만큼 유연합니다.
매개변수는 함수 호출을 단순화 하기위해 기본값을 제공할 수 있으며
함수가 실행을 완료하면 전달된 변수를 수정하는
in-out 매개변수로 전달할 수도 있습니다.

Swift의 모든 함수는
매개변수 타입과 반환 타입으로 구성된 함수 타입을 가집니다.
Swift에서는 함수 타입을 다른 타입처럼 사용할 수 있으므로,
함수를 다른 함수의 매개변수로 전달하거나
함수에서 함수를 반환하는 것이 가능합니다.
함수 내부에 또 다른 함수를 정의하여
특정 기능을 중첩 함수의 형태로 캡슐화할 수도 있습니다.

## 함수 정의와 호출 (Defining and Calling Functions)

함수를 정의할 때,
*매개변수(parameter)*라고 알고 있는
선택적으로 하나 이상의 이름이 지정된 값과 타입이 있는 입력값을 정의할 수 있습니다.
또한 *반환 타입(return type)*이라고 알고 있는
작업이 완료 되었을 때 반환할 값의 타입을
선택적으로 정의할 수 있습니다.

모든 함수는 함수가 수행하는 작업을 설명하는
*함수 이름*을 가지고 있습니다.
함수를 사용하려면, 함수의 이름으로 "호출"하고
함수의 매개변수 타입과 일치하는
입력값(*인자(argument)*)을 전달해야 합니다.
함수의 인자는 항상 함수의 매개변수 순서와
동일하게 제공해야 합니다.

아래 예시의 함수는 `greet(person:)`이라는 이름을 가지고,
사람의 이름을 입력으로 받아,
해당 사람에게 인사말을 반환하는 역할을 합니다.
이것을 수행하기 위해 `person`이라는 `String` 타입의
매개변수와
그 사람의 인사말을 포함하는
`String` 반환 타입을 정의합니다:

```swift
func greet(person: String) -> String {
    let greeting = "Hello, " + person + "!"
    return greeting
}
```

<!--
  - test: `definingAndCalling`

  ```swifttest
  -> func greet(person: String) -> String {
        let greeting = "Hello, " + person + "!"
        return greeting
     }
  ```
-->

이 정보의 모든 것은 `func` 키워드를 앞에 붙여
함수를 *정의*합니다.
*반환 화살표* `->`
(하이픈 뒤에 오른쪽 방향 꺾쇠를 붙임)
뒤에 반환 타입의 이름을 붙여 함수의 반환 타입을 나타냅니다.

함수 정의는 함수가 수행하는 작업,
받아야 할 입력값,
완료 후 반환하는 값을 설명합니다.
이러한 정의를 통해 코드의 다른 위치에서도
함수를 명확하게 호출할 수 있습니다.

```swift
print(greet(person: "Anna"))
// Prints "Hello, Anna!"
print(greet(person: "Brian"))
// Prints "Hello, Brian!"
```

<!--
  - test: `definingAndCalling`

  ```swifttest
  -> print(greet(person: "Anna"))
  <- Hello, Anna!
  -> print(greet(person: "Brian"))
  <- Hello, Brian!
  ```
-->

`greet(person: "Anna")`와 같이
`person` 인자 레이블 뒤에 `String` 값을 전달하여
`greet(person:)` 함수를 호출합니다.
함수는 `String` 값을 반환하므로,
`greet(person:)`은 위에서와 같이 반환값의 문자열을 출력하기 위해
`print(_:separator:terminator:)` 함수 안에서 호출할 수 있습니다.

> Note: `print(_:separator:terminator:)` 함수는
> 첫 번째 인자가 레이블을 가지고 있지 않고,
> 다른 인자는 기본값을 가지고 있으므로 생략할 수 있습니다.
> 이러한 함수 구문의 변형은 아래
> [함수 인자 레이블과 매개변수 이름 (Function Argument Labels and Parameter Names)](#함수-인자-레이블과-매개변수-이름-function-argument-labels-and-parameter-names)과
> [매개변수 기본값 (Default Parameter Values)](#매개변수-기본값-default-parameter-values)에서 자세히 설명합니다.

`greet(person:)` 함수의 본문은
`greeting`이라는 `String` 타입의 새로운 상수를 정의하고,
간단한 인사말 메세지 설정합니다.
이 인사말은 `return` 키워드를 사용하여 함수의 반환값으로 전달됩니다.
`return greeting`이라는 코드 줄에서
함수는 실행을 완료하고 `greeting`의 현재값을 반환합니다.

다른 입력값으로 `greet(person:)` 함수를 여러 번 호출할 수 있습니다.
위의 예시에서 `"Anna"`와 `"Brian"`의 입력값으로 호출할 경우
어떤 일이 생기는지 보여줍니다.
이 함수는 각 경우에 맞춰 인사말을 반환합니다.

이 함수의 본문을 더 간결하게 만들기 위해
메세지 생성과 반환 구문을 한 줄로 결합할 수 있습니다:

```swift
func greetAgain(person: String) -> String {
    return "Hello again, " + person + "!"
}
print(greetAgain(person: "Anna"))
// Prints "Hello again, Anna!"
```

<!--
  - test: `definingAndCalling`

  ```swifttest
  -> func greetAgain(person: String) -> String {
        return "Hello again, " + person + "!"
     }
  -> print(greetAgain(person: "Anna"))
  <- Hello again, Anna!
  ```
-->

## 함수 매개변수와 반환값 (Function Parameters and Return Values)

함수 매개변수와 반환값(Function parameters and return values)은 Swift에서 매우 유연합니다.
이름이 지정되지 않은 단일 매개변수가 있는 간단한 유틸리티 함수에서
매개변수 이름과 다른 매개변수 옵션이 있는 복잡한 함수에 이르기까지 모든 것을 정의할 수 있습니다.

### 매개변수 없는 함수 (Functions Without Parameters)

함수는 입력 매개변수 정의를 요구하지 않습니다.
여기 호출될 때마다 항상 같은 `String` 메세지를 반환하는
입력 매개변수가 없는 함수가 있습니다:

```swift
func sayHelloWorld() -> String {
    return "hello, world"
}
print(sayHelloWorld())
// Prints "hello, world"
```

<!--
  - test: `functionsWithoutParameters`

  ```swifttest
  -> func sayHelloWorld() -> String {
        return "hello, world"
     }
  -> print(sayHelloWorld())
  <- hello, world
  ```
-->

함수 정의는 어떠한 매개변수가 없더라도
함수의 이름 뒤에 소괄호가 필요합니다.
함수 이름은
함수가 호출될 때 빈 소괄호가 뒤따릅니다.

### 여러 개 매개변수가 있는 함수 (Functions With Multiple Parameters)

함수는 함수의 소괄호 내에 콤마로 구분하여
여러 개의 입력 매개변수를 가질 수 있습니다.

이 함수는 사람의 이름과
이미 인사를 받았는지를 입력으로 받아,
그 사람에게 적절한 인사말을 반환합니다:

```swift
func greet(person: String, alreadyGreeted: Bool) -> String {
    if alreadyGreeted {
        return greetAgain(person: person)
    } else {
        return greet(person: person)
    }
}
print(greet(person: "Tim", alreadyGreeted: true))
// Prints "Hello again, Tim!"
```

<!--
  - test: `definingAndCalling`

  ```swifttest
  -> func greet(person: String, alreadyGreeted: Bool) -> String {
         if alreadyGreeted {
             return greetAgain(person: person)
         } else {
             return greet(person: person)
         }
     }
  -> print(greet(person: "Tim", alreadyGreeted: true))
  <- Hello again, Tim!
  ```
-->

`person`이라는 레이블을 가진 `String` 타입의 인자값과
`alreadyGreeted`이라는 레이블을 가진 `Bool` 타입의 인자값을
소괄호 안에 콤마로 구분하여
`greet(person:alreadyGreeted:)` 함수로 전달하여 호출합니다.
이전 섹션에서 본
`greet(person:)` 함수와는 다른 함수인 것을 명심해야 합니다.
두 함수 모두 `greet`으로 시작하는 이름을 가지지만,
`greet(person:alreadyGreeted:)` 함수는 두 개의 인자를 가지지만
`greet(person:)` 함수는 오직 하나의 인자를 가집니다.

### 반환값 없는 함수 (Functions Without Return Values)

함수는 반환 타입 정의를 요구하지 않습니다.
다음은 반환하지 않고 `String` 타입의 값을 출력하는
`greet(person:)` 함수 버전입니다:

```swift
func greet(person: String) {
    print("Hello, \(person)!")
}
greet(person: "Dave")
// Prints "Hello, Dave!"
```

<!--
  - test: `functionsWithoutReturnValues`

  ```swifttest
  -> func greet(person: String) {
        print("Hello, \(person)!")
     }
  -> greet(person: "Dave")
  <- Hello, Dave!
  ```
-->

반환값이 필요하지 않기 때문에,
함수의 정의는 반환 화살표(`->`)나
반환 타입을 포함하지 않습니다.

> Note: 엄밀히 말하면, `greet(person:)` 함수는 반환값을 정의하지 않았지만,
> 여전히 반환값이 있습니다.
> 반환 타입이 정의되지 않은 함수는 `Void` 타입의 특별한 값을 반환합니다.
> 이것은 `()`로 쓰여진
> 빈 튜플입니다.

함수의 반환값은 호출될 때 무시될 수 있습니다:

```swift
func printAndCount(string: String) -> Int {
    print(string)
    return string.count
}
func printWithoutCounting(string: String) {
    let _ = printAndCount(string: string)
}
printAndCount(string: "hello, world")
// prints "hello, world" and returns a value of 12
printWithoutCounting(string: "hello, world")
// prints "hello, world" but does not return a value
```

<!--
  - test: `functionsWithoutReturnValues`

  ```swifttest
  -> func printAndCount(string: String) -> Int {
        print(string)
        return string.count
     }
  -> func printWithoutCounting(string: String) {
        let _ = printAndCount(string: string)
     }
  >> let a =
  -> printAndCount(string: "hello, world")
  << hello, world
  >> assert(a == 12)
  // prints "hello, world" and returns a value of 12
  -> printWithoutCounting(string: "hello, world")
  << hello, world
  // prints "hello, world" but doesn't return a value
  ```
-->

<!--
  Rewrite the above to avoid bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

첫 번째 함수 `printAndCount(string:)`은
문자열을 출력하고, 문자 갯수를 `Int`로 반환합니다.
두 번째 함수 `printWithoutCounting(string:)`은
첫 번째 함수를 호출하지만 반환값을 무시합니다.
두 번째 함수가 호출될 때,
첫 번째 함수에 의해 메세지는 출력되지만
반환된 값을 사용하지 않습니다.

> Note: 반환값은 무시할 수 있지만,
> 함수는 항상 값을 반환할 것입니다.
> 반환 타입이 있는 함수는
> 반환되는 값 없이 함수의 끝에서 값을 반환하지 않고
> 제어가 빠져나가게 해서는 안되며,
> 그렇게 시도할 경우 컴파일 시 오류가 발생합니다.

<!--
FIXME Unless the function is marked @discardableResult,
ignoring its return value triggers a compile-time warning.

If the returned value is coincidental
marking with @discardableResult is good,
like array.removeFirst(...) ---
otherwise, using `_ = foo()` at the call site is better.
-->

### 여러 개의 반환값이 있는 함수 (Functions with Multiple Return Values)

튜플 타입을 함수의 반환 타입으로 사용하면
여러 값을 하나의 복합 반환값으로 반환할 수 있습니다.

아래 예시는 `Int` 값의 배열에서 가장 작은 값과 가장 큰 값을 찾는
`minMax(array:)` 함수를 정의합니다:

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```

<!--
  - test: `tupleTypesAsReturnTypes`

  ```swifttest
  -> func minMax(array: [Int]) -> (min: Int, max: Int) {
        var currentMin = array[0]
        var currentMax = array[0]
        for value in array[1..<array.count] {
           if value < currentMin {
              currentMin = value
           } else if value > currentMax {
              currentMax = value
           }
        }
        return (currentMin, currentMax)
     }
  ```
-->

`minMax(array:)` 함수는 두 개의 `Int` 값을 포함한 튜플을 반환합니다.
이 값들은 함수의 반환값을 조회할 때 이름으로 접근할 수 있도록
`min`과 `max`로 레이블되어 있습니다.

`minMax(array:)` 함수의 본문은 배열의 첫 번째 정수의 값을
`currentMin`과 `currentMax`라 불리는 두 개의 동작 변수에
값을 설정하는 것으로 시작합니다.
그러면 함수는 배열의 나머지 값들을 반복하고
각 값이 `currentMin`과 `currentMax`의 값보다
더 작거나 더 큰지 확인합니다.
마지막으로 가장 작고 가장 큰 값은
두 개의 `Int` 값인 튜플로 반환됩니다.

튜플의 멤버 값은 함수의 반환 타입이므로
가장 작은 값과 가장 큰 값을 찾기 위해 점 문법으로 접근할 수 있습니다:

```swift
let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
print("min is \(bounds.min) and max is \(bounds.max)")
// Prints "min is -6 and max is 109"
```

<!--
  - test: `tupleTypesAsReturnTypes`

  ```swifttest
  -> let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
  -> print("min is \(bounds.min) and max is \(bounds.max)")
  <- min is -6 and max is 109
  ```
-->

함수에서 튜플을 반환할 때,
튜플의 멤버들은 이미 함수의 반환 타입에서 이름이 지정되어 있기 때문에
반환 시점에서 이름을 지정할 필요는 없습니다.

### 옵셔널 튜플 반환 타입 (Optional Tuple Return Types)

함수에서 반환되는 튜플 타입이
전체 튜플에 대해 "값이 없을" 가능성이 있는 경우,
*옵셔널* 튜플 반환 타입을 사용하여
전체 튜플이 `nil`일 수 있다는 사실을 반영할 수 있습니다.
`(Int, Int)?`나 `(String, Int, Bool)?`와 같이
튜플 타입의 닫는 소괄호 다음에
물음표를 붙여 옵셔널 튜플 반환 타입을 작성합니다.

> Note: `(Int, Int)?`와 같은 옵셔널 튜플 타입은
> `(Int?, Int?)`와 같이
> 옵셔널 타입을 가지는 튜플과는 다릅니다.
> 옵셔널 튜플 타입은 튜플 안에 각각의 값이 옵셔널이 아니라
> 전체 튜플이 옵셔널이라는 의미입니다.

위에서 `minMax(array:)` 함수는 두 개의 `Int` 값을 포함하는 튜플을 반환합니다.
그러나 이 함수는 전달된 배열에 대한 안정성 검사를 하지 않습니다.
`array` 인자가 빈 배열을 포함하면
위에서 정의된 `minMax(array:)` 함수는
`array[0]`을 접근할 때 런타임 오류가 발생합니다.

빈 배열을 안전하게 처리하기 위해선
`minMax(array:)` 함수는 옵셔널 튜플 반환 타입을 가지고
배열이 비어있을 때 `nil`을 반환해야 합니다:

```swift
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    if array.isEmpty { return nil }
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```

<!--
  - test: `tupleTypesAsReturnTypes2`

  ```swifttest
  -> func minMax(array: [Int]) -> (min: Int, max: Int)? {
        if array.isEmpty { return nil }
        var currentMin = array[0]
        var currentMax = array[0]
        for value in array[1..<array.count] {
           if value < currentMin {
              currentMin = value
           } else if value > currentMax {
              currentMax = value
           }
        }
        return (currentMin, currentMax)
     }
  ```
-->

옵셔널 바인딩을 통해 이 버전의 `minMax(array:)` 함수가
실제 튜플 값을 반환하는지 `nil`을 반환하는지 확인할 수 있습니다:

```swift
if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
    print("min is \(bounds.min) and max is \(bounds.max)")
}
// Prints "min is -6 and max is 109"
```

<!--
  - test: `tupleTypesAsReturnTypes2`

  ```swifttest
  -> if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
        print("min is \(bounds.min) and max is \(bounds.max)")
     }
  <- min is -6 and max is 109
  ```
-->

### 암시적 반환을 가진 함수 (Functions With an Implicit Return)

함수의 전체 본문이 한 줄로 표현이 된다면,
함수는 해당 표현식을 암시적으로 반환합니다.
예를 들어
아래의 두 함수는 모두 같은 동작을 합니다:

```swift
func greeting(for person: String) -> String {
    "Hello, " + person + "!"
}
print(greeting(for: "Dave"))
// Prints "Hello, Dave!"

func anotherGreeting(for person: String) -> String {
    return "Hello, " + person + "!"
}
print(anotherGreeting(for: "Dave"))
// Prints "Hello, Dave!"
```

<!--
  - test: `implicit-func-return`

  ```swifttest
  -> func greeting(for person: String) -> String {
        "Hello, " + person + "!"
     }
  -> print(greeting(for: "Dave"))
  <- Hello, Dave!

  -> func anotherGreeting(for person: String) -> String {
        return "Hello, " + person + "!"
     }
  -> print(anotherGreeting(for: "Dave"))
  <- Hello, Dave!
  ```
-->

`greeting(for:)` 함수의 전체 정의는
인사말 메세지를 반환하므로,
이것은 더 짧은 형태로 작성할 수 있습니다.
`anotherGreeing(for:)` 함수는 같은 인사말 메세지를 반환하지만,
`return` 키워드를 사용하여 더 긴 형식으로 작성합니다.
`return` 한 줄로만 작성된 함수는 `return`을 생략할 수 있습니다.

[간략한 Getter 선언 (Shorthand Getter Declaration)](./properties.md#간략한-getter-선언-shorthand-getter-declaration)에서 살펴보겠지만,
프로퍼티 getter 또한 암시적 반환을 사용할 수 있습니다.

> Note: 암시적 반환값으로 작성하는 코드는
> 반드시 어떤 값을 반환해야 합니다.
> 예를 들어
> 암시적 반환값으로
> `print(13)`을 사용할 수 없습니다.
> 그러나 `fatalError("Oh no!")`와 같이
> Swift가 암시적 반환이 일어나지 않는 것을
> 아는 경우에는
> 사용할 수 있습니다.

<!--
  - test: `implicit-return-print-instead`

  ```swifttest
  // This is ok:
  >> func testFatal() -> Int {
  >>     fatalError("Oh no!")
  >> }

  // But not this:
  >> func testPrint() -> Int {
  >>     print(13)
  >> }
  !$ error: cannot convert return expression of type '()' to return type 'Int'
  !! print(13)
  !! ^~~~~~~~~
  ```
-->

## 함수 인자 레이블과 매개변수 이름 (Function Argument Labels and Parameter Names)

각 함수 매개변수는 *인자 레이블(argument label)*과
*매개변수 이름(parameter name)*을 가지고 있습니다.
인자 레이블은 함수가 호출될 때 사용되고;
각 인자는 함수 호출 시 인자 레이블 다음에 작성합니다.
매개변수 이름은 함수의 구현 내부에서 사용됩니다.
기본적으로 매개변수는
매개변수 이름을 인자 레이블로 사용합니다.

```swift
func someFunction(firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}
someFunction(firstParameterName: 1, secondParameterName: 2)
```

<!--
  - test: `functionParameterNames`

  ```swifttest
  -> func someFunction(firstParameterName: Int, secondParameterName: Int) {
        // In the function body, firstParameterName and secondParameterName
        // refer to the argument values for the first and second parameters.
     }
  -> someFunction(firstParameterName: 1, secondParameterName: 2)
  ```
-->

모든 매개변수는 유니크한 이름을 가져야 합니다.
여러 매개변수에
동일한 인자 레이블을 가질 수 있지만,
유니크한 인자 레이블은 코드를 더욱 읽기 편하게 해줍니다.

<!--
  - test: `non-unique-external-name`

  ```swifttest
  -> func foo(external a: Int, external b: Int) {}
  -> foo(external: 7, external: 12)
  ```
-->

### 인자 레이블 지정 (Specifying Argument Labels)

공백으로 구분하여
매개변수 이름 앞에 인자 레이블을 작성합니다:

```swift
func someFunction(argumentLabel parameterName: Int) {
    // In the function body, parameterName refers to the argument value
    // for that parameter.
}
```

<!--
  - test: `externalParameterNames`

  ```swifttest
  -> func someFunction(argumentLabel parameterName: Int) {
        // In the function body, parameterName refers to the argument value
        // for that parameter.
     }
  ```
-->

다음은 사람의 이름과 고향을 가져와
인사말을 반환하는
`greet(person:)` 함수의 변형입니다:

```swift
func greet(person: String, from hometown: String) -> String {
    return "Hello \(person)!  Glad you could visit from \(hometown)."
}
print(greet(person: "Bill", from: "Cupertino"))
// Prints "Hello Bill!  Glad you could visit from Cupertino."
```

<!--
  - test: `externalParameterNames`

  ```swifttest
  -> func greet(person: String, from hometown: String) -> String {
         return "Hello \(person)!  Glad you could visit from \(hometown)."
     }
  -> print(greet(person: "Bill", from: "Cupertino"))
  <- Hello Bill!  Glad you could visit from Cupertino.
  ```
-->

인자 레이블을 사용하면 함수를 마치
문장처럼 표현력 있게 호출할 수 있으며,
동시에 함수 본문은 읽기 쉽고 의도를 명확하게 유지할 수 있습니다.

### 인자 레이블 생략 (Omitting Argument Labels)

매개변수에 인자 레이블을 원치 않으면,
명시적으로 인자 레이블 작성하는 대신에 언더바(`_`)를 작성합니다.

```swift
func someFunction(_ firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}
someFunction(1, secondParameterName: 2)
```

<!--
  - test: `omittedExternalParameterNames`

  ```swifttest
  -> func someFunction(_ firstParameterName: Int, secondParameterName: Int) {
        // In the function body, firstParameterName and secondParameterName
        // refer to the argument values for the first and second parameters.
     }
  -> someFunction(1, secondParameterName: 2)
  ```
-->

매개변수가 인자 레이블을 가지고 있다면,
함수를 호출할 때 인자는 *반드시* 레이블을 지정해야 합니다.

### 매개변수 기본값 (Default Parameter Values)

매개변수의 타입 뒤에 매개변수 값을 할당하여
함수의 매개변수에 *기본값(default value)*을 정의할 수 있습니다.
기본값이 정의되어 있다면, 함수를 호출할 때 매개변수를 생략할 수 있습니다.

```swift
func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
    // If you omit the second argument when calling this function, then
    // the value of parameterWithDefault is 12 inside the function body.
}
someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6) // parameterWithDefault is 6
someFunction(parameterWithoutDefault: 4) // parameterWithDefault is 12
```

<!--
  - test: `omittedExternalParameterNames`

  ```swifttest
  -> func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
        // If you omit the second argument when calling this function, then
        // the value of parameterWithDefault is 12 inside the function body.
     }
  -> someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6) // parameterWithDefault is 6
  -> someFunction(parameterWithoutDefault: 4) // parameterWithDefault is 12
  ```
-->

기본값이 없는 매개변수는
가본 값이 있는 매개변수보다
함수 매개변수 목록의 앞 쪽에 배치합니다.
기본값이 없는 매개변수는
일반적으로 함수의 의미에서 더 중요합니다 ---
이를 먼저 작성하면
기본값이 있는 매개변수가 생략되더라도
동일한 함수가 호출되고 있음을 쉽게 인식할 수 있습니다.

### 가변 매개변수 (Variadic Parameters)

*가변 매개변수(variadic parameter)*는 0개 이상의 특정 타입의 값을 허용합니다.
함수가 호출될 때 여러 개의 입력값이 전달될 수 있는
특정 매개변수는 가변 매개변수를 사용합니다.
가변 매개변수는 매개변수의 타입 이름 뒤에
마침표 세 개(`...`)를 추가하여 작성합니다.

가변 매개변수에 전달된 값은 함수 본문 내에서
적절한 타입의 배열로 사용될 수 있습니다.
예를 들어 `numbers`라는 이름과 `Double...` 타입을 가진 가변 매개변수는
함수 본문 내에서
`[Double]` 타입의 `numbers`라 불리는 상수 배열로 사용할 수 있습니다.

아래 예시는 길이에 관계없이 숫자 목록에 대한
*산술 평균*(*평균*)을 계산합니다:

```swift
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)
// returns 3.0, which is the arithmetic mean of these five numbers
arithmeticMean(3, 8.25, 18.75)
// returns 10.0, which is the arithmetic mean of these three numbers
```

<!--
  - test: `variadicParameters`

  ```swifttest
  -> func arithmeticMean(_ numbers: Double...) -> Double {
        var total: Double = 0
        for number in numbers {
           total += number
        }
        return total / Double(numbers.count)
     }
  >> let r0 =
  -> arithmeticMean(1, 2, 3, 4, 5)
  /> returns \(r0), which is the arithmetic mean of these five numbers
  </ returns 3.0, which is the arithmetic mean of these five numbers
  >> let r1 =
  -> arithmeticMean(3, 8.25, 18.75)
  /> returns \(r1), which is the arithmetic mean of these three numbers
  </ returns 10.0, which is the arithmetic mean of these three numbers
  ```
-->

<!--
  Rewrite the above to avoid bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

함수는 여러 개의 가변 매개변수를 가질 수 있습니다.
가변 매개변수 뒤에 오는 첫 번째 매개변수는
인자 레이블을 가지고 있어야 합니다.
인자 레이블은
가변 매개변수에 전달되는 인자와
가변 매개변수 뒤에 오는
매개변수에 전달되는 인자를 명확하게 합니다.

<!--
  - test: `variadic-parameters-and-labels`

  ```swifttest
  // Labeled, immediately after
  >> func f(_ a: Int..., b: String) {}

  // Unlabeled, not immediately after
  >> func g(_ a: Int..., b: String, _ c: Int) {}

  // Multiple
  >> func h(_a: Int..., b: String, _ c: Int..., d: String) {}
  ```
-->

<!--
  - test: `variadic-parameters-and-labels-failure`

  ```swifttest
  // Unlabeled, immediately after
  >> func f(_ a: Int..., _ b: String) {}
  !$ error: a parameter following a variadic parameter requires a label
  !! func f(_ a: Int..., _ b: String) {}
  !! ^
  ```
-->

### In-Out 매개변수 (In-Out Parameters)

함수 매개변수는 기본적으로 상수입니다.
함수 본문 내에서 매개변수 값을 변경하려고 하면
컴파일 시에 오류가 발생합니다.
이것은 실수로 매개변수의 값을 변경하는 것을 방지합니다.
함수가 매개변수 값을 변경하고,
함수 호출이 종료된 후에도 이러한 변경된 값을 유지하고 싶다면,
해당 매개변수를 *in-out 매개변수(in-out parameter)*로 정의해야 합니다.

in-out 매개변수는 매개변수의 타입 바로 전에
`inout` 키워드를 붙여서 작성합니다.
in-out 매개변수는 함수에 입력되고,
함수에서 수정되며,
함수가 끝난 후 원래 값 대신 수정된 값이 출력됩니다.
In-out 매개변수의 동작 방식과
관련된 컴파일러 최적화에 대한
자세한 설명은 [In-Out 매개변수 (In-Out Parameters)](../language-reference/declarations.md#in-out-매개변수-in-out-parameters)를 참고바랍니다.

in-out 매개변수에 전달할 수 있는 인자는 변수만 가능합니다.
상수나 리터럴 값은 수정할 수 없기 때문에
인자로 전달할 수 없습니다.
in-out 매개변수에 변수를 인자로 전달할 때는
변수 이름 앞에 앰퍼샌드(`&`)를 붙여서
해당 변수가 함수에 의해 수정될 수 있음을 나타냅니다.

> Note: in-out 매개변수는 기본값을 가질 수 없고,
> 가변 매개변수는 `inout`으로 표기할 수 없습니다.

다음 예시는 `a`와 `b`라 하는 두 개의 in-out 정수 매개변수를 가지는
`swapTwoInts(_:_:)`라는 함수입니다:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

<!--
  - test: `inoutParameters`

  ```swifttest
  -> func swapTwoInts(_ a: inout Int, _ b: inout Int) {
        let temporaryA = a
        a = b
        b = temporaryA
     }
  ```
-->

`swapTwoInts(_:_:)` 함수는 간단하게 `b`의 값을 `a`로
`a`의 값을 `b`로 바꿉니다.
이 함수는 `a`의 값을 `temporaryA`라 하는 임시 상수에 저장하고
`b`의 값을 `a`에 할당하고
`temporaryA`의 값을 `b`에 할당합니다.

`Int` 타입의 바꿀 두 개의 변수를 이용하여
`swapTwoInts(_:_:)` 함수를 호출할 수 있습니다.
`swapTwoInts(_:_:)` 함수에 전달할 때,
`someInt`와 `anotherInt`의 이름 앞에 앰퍼샌드를 붙여야 합니다:

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// Prints "someInt is now 107, and anotherInt is now 3"
```

<!--
  - test: `inoutParameters`

  ```swifttest
  -> var someInt = 3
  -> var anotherInt = 107
  -> swapTwoInts(&someInt, &anotherInt)
  -> print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
  <- someInt is now 107, and anotherInt is now 3
  ```
-->

위의 예시는
`someInt`와 `anotherInt`의 함수의 외부에서 정의되었지만
`swapTwoInts(_:_:)` 함수로 인해
원래 값이 수정되는 것을 보여줍니다.

> Note: in-out 매개변수는 함수에서 값을 반환하는 것과 다릅니다.
> 위의 `swapTwoInts` 예시는 반환 타입을 정의하거나 값을 반환하지 않지만,
> `someInt`와 `anotherInt`의 값은 수정됩니다.
> In-out 매개변수는 함수 본문 범위를 벗어난 곳에서도
> 영향을 미칠 수 있는 또 다른 방법입니다.

<!--
  TODO: you can pass a subproperty of something as an inout reference.
  Would be great to show an example of this as a way to avoid temporary variables.
-->

## 함수 타입 (Function Types)

모든 함수는 매개변수 타입과 반환 타입으로 구성된
특정 *함수 타입(function type)*이 있습니다.

예를 들어:

```swift
func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}
func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}
```

<!--
  - test: `functionTypes`

  ```swifttest
  -> func addTwoInts(_ a: Int, _ b: Int) -> Int {
        return a + b
     }
  >> print(type(of: addTwoInts))
  << (Int, Int) -> Int
  -> func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
        return a * b
     }
  >> print(type(of: multiplyTwoInts))
  << (Int, Int) -> Int
  ```
-->

이 예시는 `addTwoInts`와 `multiplyTwoInts`라 불리는
간단한 수학 방정식 함수를 정의합니다.
이 함수는 각각 두 개의 `Int` 값을 가지며,
적절한 수학 연산을 수행하여
`Int` 값을 반환합니다.

이 두 함수의 타입은 `(Int, Int) -> Int`입니다.
이것은 아래와 같이 읽을 수 있습니다:

"함수는 모두 `Int` 타입인 두 개의 매개변수를 가지며,
`Int` 타입을 반환합니다."

다음의 예시는 매개변수나 반환값이 없는 함수 입니다:

```swift
func printHelloWorld() {
    print("hello, world")
}
```

<!--
  - test: `functionTypes`

  ```swifttest
  -> func printHelloWorld() {
        print("hello, world")
     }
  >> print(type(of: printHelloWorld))
  << () -> ()
  ```
-->

이 함수의 타입은 `() -> Void`
또는 "함수는 매개변수가 없고 `Void`를 반환합니다."

### 함수 타입 사용 (Using Function Types)

Swift에서는 다른 타입처럼 함수 타입을 사용합니다.
예를 들어 함수 타입에 대해 상수나 변수로 정의 할 수 있고,
변수에 적절한 함수 타입을 할당할 수 있습니다:

```swift
var mathFunction: (Int, Int) -> Int = addTwoInts
```

<!--
  - test: `functionTypes`

  ```swifttest
  -> var mathFunction: (Int, Int) -> Int = addTwoInts
  ```
-->

이것은 이렇게 읽을 수 있습니다:

"'두 개의 `Int` 값과
`Int`를 반환하는 함수'의 타입을 가지는
`mathFunction`이라는 변수를 정의합니다.
그리고 이 새로운 변수에 `addTwoInts`라는 함수에 대한 참조를 할당합니다."

`addTwoInts(_:_:)` 함수는 `mathFunction` 변수와 같은 타입을 가지므로,
Swift의 타입 검사기에 의해 이 할당은 허용됩니다.

이제 `mathFunction`이라는 이름으로 할당된 함수를 호출할 수 있습니다:

```swift
print("Result: \(mathFunction(2, 3))")
// Prints "Result: 5"
```

<!--
  - test: `functionTypes`

  ```swifttest
  -> print("Result: \(mathFunction(2, 3))")
  <- Result: 5
  ```
-->

비함수 타입과 동일한 방식으로
같은 타입으로 일치하는 다른 함수를 같은 변수에 할당할 수 있습니다:

```swift
mathFunction = multiplyTwoInts
print("Result: \(mathFunction(2, 3))")
// Prints "Result: 6"
```

<!--
  - test: `functionTypes`

  ```swifttest
  -> mathFunction = multiplyTwoInts
  -> print("Result: \(mathFunction(2, 3))")
  <- Result: 6
  ```
-->

다른 타입과 마찬가지로,
함수 타입을 상수나 변수에 함수를 할당할 때
Swift가 함수 타입을 추론하도록 맡길 수 있습니다:

```swift
let anotherMathFunction = addTwoInts
// anotherMathFunction is inferred to be of type (Int, Int) -> Int
```

<!--
  - test: `functionTypes`

  ```swifttest
  -> let anotherMathFunction = addTwoInts
  >> print(type(of: anotherMathFunction))
  << (Int, Int) -> Int
  // anotherMathFunction is inferred to be of type (Int, Int) -> Int
  ```
-->

<!--
  TODO: talk about defining typealiases for function types somewhere?
-->

### 매개변수 타입으로 함수 타입 (Function Types as Parameter Types)

`(Int, Int) -> Int`와 같은 함수 타입을
다른 함수의 매개변수로 사용할 수 있습니다.
이렇게 하면 함수의 일부 구현을
해당 함수를 호출하는 쪽에서 제공하도록 할 수 있습니다.

다음의 예시는 위 수학 함수의 결과를 출력합니다:

```swift
func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
// Prints "Result: 8"
```

<!--
  - test: `functionTypes`

  ```swifttest
  -> func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
        print("Result: \(mathFunction(a, b))")
     }
  -> printMathResult(addTwoInts, 3, 5)
  <- Result: 8
  ```
-->

이 예시는 세 개의 매개변수를 가지는 `printMathResult(_:_:_:)`라는 함수를 정의합니다.
첫 번째 매개변수는 `mathFunction`이라 불리는 `(Int, Int) -> Int` 타입입니다.
이 첫 번째 매개변수의 인자로 해당 타입의 함수를 전달할 수 있습니다.
두 번째 세 번째 매개변수는 `a`와 `b`라 불리고 둘 다 `Int` 타입입니다.
이들은 제공된 수학 함수의 두 개의 입력값으로 사용됩니다.

`printMathResult(_:_:_:)` 호출할 때,
`addTwoInts(_:_:)` 함수와 정수 `3`과 `5`가 전달됩니다.
`3`과 `5`로 제공된 함수를 호출하고 `8`의 결과를 출력합니다.

`printMathResult(_:_:_:)`의 역할은
적절한 타입의 수학 함수를 호출하고 값을 출력합니다.
이것은 함수의 실제 구현 내용은 중요하지 않습니다 ---
중요한 것은 함수가 올바른 타입을 갖는다는 점입니다.
이렇게 하면 `printMathResult(_:_:_:)`는 자신의 일부 기능을
타입 안전성을 유지하면서 함수 호출자에게 넘길 수 있습니다.

### 반환 타입으로 함수 타입 (Function Types as Return Types)

다른 함수의 반환 타입으로 함수 타입을 사용할 수 있습니다.
반환하는 함수의 반환 화살표(`->`) 바로 뒤에
전체 함수 타입을 작성하여 이를 수행합니다.

다음 예시는 `stepForward(_:)`와 `stepBackward(_:)`라 불리는 간단한 두 개의 함수를 정의합니다.
`stepForward(_:)` 함수는 입력값에 1을 더해 반환하고,
`stepBackward(_:)` 함수는 입력값에 1을 빼고 반환합니다.
두 함수 모두 `(Int) -> Int` 타입입니다:

```swift
func stepForward(_ input: Int) -> Int {
    return input + 1
}
func stepBackward(_ input: Int) -> Int {
    return input - 1
}
```

<!--
  - test: `functionTypes`

  ```swifttest
  -> func stepForward(_ input: Int) -> Int {
        return input + 1
     }
  -> func stepBackward(_ input: Int) -> Int {
        return input - 1
     }
  ```
-->

다음의 함수는 반환 타입이 `(Int) -> Int` 인
`chooseStepFunction(backward:)`입니다.
`chooseStepFunction(backward:)` 함수는 `backward`라 불리는 Boolean 매개변수를 기반으로
`stepForward(_:)` 함수나 `stepBackward(_:)` 함수를 반환합니다:

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    return backward ? stepBackward : stepForward
}
```

<!--
  - test: `functionTypes`

  ```swifttest
  -> func chooseStepFunction(backward: Bool) -> (Int) -> Int {
        return backward ? stepBackward : stepForward
     }
  ```
-->

이제 `chooseStepFunction(backward:)`를 사용하여
한 방향이나 다른 방향으로 이동할 함수를 얻을 수 있습니다:

```swift
var currentValue = 3
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the stepBackward() function
```

<!--
  - test: `functionTypes`

  ```swifttest
  -> var currentValue = 3
  -> let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
  >> print(type(of: moveNearerToZero))
  << (Int) -> Int
  // moveNearerToZero now refers to the stepBackward() function
  ```
-->

위의 예시는 `currentValue`라는 변수를 점차적으로 0에 가깝게 이동하기 위해
양수 또는 음수 단계가 필요한지 판단합니다.
`currentValue`는 `3`의 초기값을 가지며,
이것은 `currentValue > 0`이 `true`를 반환한다는 의미이며,
`chooseStepFunction(backward:)`가 `stepBackward(_:)` 함수를 반환하도록 합니다.
반환된 함수의 참조는 `moveNearerToZero`라는 상수에 저장됩니다.

`moveNearerToZero`가 올바른 함수를 참조하므로,
0으로 계산하는데 사용할 수 있습니다:

```swift
print("Counting to zero:")
// Counting to zero:
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// 3...
// 2...
// 1...
// zero!
```

<!--
  - test: `functionTypes`

  ```swifttest
  -> print("Counting to zero:")
  </ Counting to zero:
  -> while currentValue != 0 {
        print("\(currentValue)... ")
        currentValue = moveNearerToZero(currentValue)
     }
  -> print("zero!")
  </ 3...
  </ 2...
  </ 1...
  </ zero!
  ```
-->

## 중첩 함수 (Nested Functions)

이 챕터에서 접한 모든 함수는
전역 범위에서 정의된 *전역 함수(global functions)*의 예입니다.
*중첩 함수(nested functions)*라고 하는
다른 함수 내에 함수를 정의할 수도 있습니다.

중첩 함수는 기본적으로 외부에서 보이지 않지만,
중첩 함수를 둘러싼 함수를 통해 호출될 수 있고 사용될 수 있습니다.
중첩 함수를 둘러싼 함수는 중첩 함수 중 하나를 반환하여
중첩 함수를 다른 범위에서 사용할 수도 있습니다.

위의 예시 `chooseStepFunction(backward:)`를 중첩 함수를 사용하고 반환하도록
다시 작성할 수 있습니다:

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backward ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    print("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
print("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```

<!--
  - test: `nestedFunctions`

  ```swifttest
  -> func chooseStepFunction(backward: Bool) -> (Int) -> Int {
        func stepForward(input: Int) -> Int { return input + 1 }
        func stepBackward(input: Int) -> Int { return input - 1 }
        return backward ? stepBackward : stepForward
     }
  -> var currentValue = -4
  -> let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
  >> print(type(of: moveNearerToZero))
  << (Int) -> Int
  // moveNearerToZero now refers to the nested stepForward() function
  -> while currentValue != 0 {
        print("\(currentValue)... ")
        currentValue = moveNearerToZero(currentValue)
     }
  -> print("zero!")
  </ -4...
  </ -3...
  </ -2...
  </ -1...
  </ zero!
  ```
-->

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->
