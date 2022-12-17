# 함수 \(Functions\)

<!--
Functions are self-contained chunks of code that perform a specific task. You give a function a name that identifies what it does, and this name is used to “call” the function to perform its task when needed.

Swift’s unified function syntax is flexible enough to express anything from a simple C-style function with no parameter names to a complex Objective-C-style method with names and argument labels for each parameter. Parameters can provide default values to simplify function calls and can be passed as in-out parameters, which modify a passed variable once the function has completed its execution.

Every function in Swift has a type, consisting of the function’s parameter types and return type. You can use this type like any other type in Swift, which makes it easy to pass functions as parameters to other functions, and to return functions from functions. Functions can also be written within other functions to encapsulate useful functionality within a nested function scope.
-->

_함수 \(Functions\)_ 는 특정 작업을 수행하는 코드 모음 형태입니다. 무슨 동작을 하는지 함수에 특정 이름을 줄 수 있으며 필요할 때 작업을 수행하기 위해 함수를 "호출" 할 때 사용됩니다.

Swift의 통합 함수 구문은 파라미터 이름이 없는 단순한 C 스타일 함수에서 각 파라미터에 대한 이름과 인자가 있는 복잡한 Objective-C 스타일 메서드에 이르기까지 모든 것을 표현할 수 있을만큼 유연합니다. 파라미터는 함수 호출을 단순화 하기위해 기본값을 제공할 수 있으며 함수가 실행을 완료하면 전달된 변수를 수정하는 in-out 파라미터로 전달될 수 있습니다.

Swift의 모든 함수에는 함수의 파라미터 타입과 반환 타입으로 구성된 타입이 있습니다. Swift의 다른 타입과 마찬가지로 이 타입을 사용할 수 있으므로 함수를 파라미터로 다른 함수에 전달하고 함수에서 함수를 반환하기가 쉽습니다. 함수는 중첩된 함수 범위내에서 유용한 기능을 캡슐화하기 위해 다른 함수 내에 작성될 수도 있습니다.

## 함수 정의와 호출 \(Defining and Calling Functions\)

<!--
When you define a function, you can optionally define one or more named, typed values that the function takes as input, known as parameters. You can also optionally define a type of value that the function will pass back as output when it’s done, known as its return type.

Every function has a function name, which describes the task that the function performs. To use a function, you “call” that function with its name and pass it input values (known as arguments) that match the types of the function’s parameters. A function’s arguments must always be provided in the same order as the function’s parameter list.

The function in the example below is called greet(person:), because that’s what it does—it takes a person’s name as input and returns a greeting for that person. To accomplish this, you define one input parameter—a String value called person—and a return type of String, which will contain a greeting for that person:
-->

함수를 정의할 때 _파라미터_ 라고 알고 있는 함수가 입력으로 받는 하나 이상의 타입된 값을 선택적으로 정의할 수 있습니다. 또한 _반환 타입_ 이라고 알고 있는 작업이 완료 되었을 때 함수가 다시 전달하는 값의 타입을 선택적으로 정의할 수 있습니다.

모든 함수는 함수가 수행하는 작업을 설명하는 _함수 이름_ 을 가지고 있습니다. 함수를 사용하려면 함수의 이름으로 "호출"하고 함수의 파라미터와 일치하는 인자라고 알려진 입력 값을 전달해야 합니다. 함수의 인자는 항상 함수의 파라미터 순서와 동일하게 제공해야 합니다.

아래 예제의 함수는 그 사람의 이름을 입력으로 받아 그 사람의 인사말을 반환하기 때문에 `greet(person:)` 이라 불립니다. 이것을 수행하기 위해 `person` 이라 불리는 `String` 값인 하나의 파라미터와 그 사람의 인사말을 포함한 `String` 반환 타입을 정의합니다:

```swift
func greet(person: String) -> String {
    let greeting = "Hello, " + person + "!"
    return greeting
}
```

<!--
All of this information is rolled up into the function’s definition, which is prefixed with the func keyword. You indicate the function’s return type with the return arrow -> (a hyphen followed by a right angle bracket), which is followed by the name of the type to return.

The definition describes what the function does, what it expects to receive, and what it returns when it’s done. The definition makes it easy for the function to be called unambiguously from elsewhere in your code:
-->

이 정보의 모든 것은 `func` 키워드를 앞에 붙여 함수를 _정의_ 합니다. _반환 활살표_ `->` \(하이픈 뒤에 오른쪽 방향 꺽쇄를 붙임\) 뒤에 반환 타입의 이름을 붙여 함수의 반환 타입을 나타냅니다.

정의는 함수가 수행하는 작업, 수신할 것으로 예상되는 작업 및 완료 시 반환되는 작업을 설명합니다. 정의를 통해 코드의 다른 위치에서 함수를 명확하게 호출할 수 있습니다.

```swift
print(greet(person: "Anna"))
// Prints "Hello, Anna!"
print(greet(person: "Brian"))
// Prints "Hello, Brian!"
```

<!--
You call the greet(person:) function by passing it a String value after the person argument label, such as greet(person: "Anna"). Because the function returns a String value, greet(person:) can be wrapped in a call to the print(_:separator:terminator:) function to print that string and see its return value, as shown above.
-->

`greet(person: "Anna")` 와 같이 `person` 인자 뒤에 `String` 값을 전달하여 `greet(person:)` 함수를 호출합니다. 함수는 `String` 값을 반환하므로 `greet(person:)` 은 위에서와 같이 반환 값의 문자열을 출력하기 위해 `print(_:separator:terminator:)` 함수로 래핑할 수 있습니다.

<!--
NOTE
The print(_:separator:terminator:) function doesn’t have a label for its first argument, and its other arguments are optional because they have a default value. These variations on function syntax are discussed below in Function Argument Labels and Parameter Names and Default Parameter Values.
-->

> NOTE  
> `print(_:separator:terminator:)` 함수는 첫번째 인자의 라벨을 가지고 있지 않고 다른 인자는 기본값을 가지고 있으므로 선택입니다. 함수 구문의 이러한 변형은 아래 [함수 인자 라벨과 파라미터 이름 \(Function Argument Labels and Parameter Names\)](functions.md#function-argument-labels-and-parameter-names) 와 [파라미터 기본값 \(Default Parameter Values\)](functions.md#default-parameter-values) 에서 자세히 설명합니다.

<!--
The body of the greet(person:) function starts by defining a new String constant called greeting and setting it to a simple greeting message. This greeting is then passed back out of the function using the return keyword. In the line of code that says return greeting, the function finishes its execution and returns the current value of greeting.

You can call the greet(person:) function multiple times with different input values. The example above shows what happens if it’s called with an input value of "Anna", and an input value of "Brian". The function returns a tailored greeting in each case.

To make the body of this function shorter, you can combine the message creation and the return statement into one line:
-->

`greet(person:)` 함수의 바디는 `greeting` 으로 불리는 새로운 `String` 상수 정의와 간단한 인사말 메세지 설정으로 시작합니다. 이 인사말은 `return` 키워드를 사용하여 함수의 바깥으로 전달됩니다. `return greeting` 이라는 코드 줄에서 함수는 실행을 완료하고 인사말의 현재값을 반환합니다.

다른 입력값으로 `greet(person:)` 함수를 여러번 호출할 수 있습니다. 위의 예제에서 `"Anna"` 와 `"Brian"` 의 입력값으로 호출할 경우 어떤일이 생기는지 보여줍니다. 이 함수는 각 경우에 맞춰 인사말을 반환합니다.

이 함수의 바디를 더 짧게 만들기위해 메세지 생성과 반환 구문을 한줄로 결합할 수 있습니다:

```swift
func greetAgain(person: String) -> String {
    return "Hello again, " + person + "!"
}
print(greetAgain(person: "Anna"))
// Prints "Hello again, Anna!"
```

## 함수 파라미터와 반환값 \(Function Parameters and Return Values\)

<!--
Function parameters and return values are extremely flexible in Swift. You can define anything from a simple utility function with a single unnamed parameter to a complex function with expressive parameter names and different parameter options.
-->

함수 파라미터와 반환값 \(Function parameters and return values\)은 Swift에서 매우 유연합니다. 이름이 지정되지 않은 단일 파라미터가 있는 간단한 유틸리티 함수에서 파라미터 이름과 다른 파라미터 옵션이 있는 복잡한 함수에 이르기까지 모든 것을 정의할 수 있습니다.

### 파라미터 없는 함수 \(Functions Without Parameters\)

<!--
Functions aren’t required to define input parameters. Here’s a function with no input parameters, which always returns the same String message whenever it’s called:
-->

함수는 입력 파라미터 정의를 요구하지 않습니다. 여기 호출될 때마다 항상 같은 `String` 메세지를 반환하는 입력 파라미터가 없는 함수가 있습니다:

```swift
func sayHelloWorld() -> String {
    return "hello, world"
}
print(sayHelloWorld())
// Prints "hello, world"
```

<!--
The function definition still needs parentheses after the function’s name, even though it doesn’t take any parameters. The function name is also followed by an empty pair of parentheses when the function is called.
-->

함수 정의는 어떠한 파라미터가 없더라도 함수의 이름 뒤에 소괄호가 필요합니다. 함수 이름은 함수가 호출될 때 빈 소괄호가 뒤따릅니다.

### 여러개 파라미터가 있는 함수 \(Functions With Multiple Parameters\)

<!--
Functions can have multiple input parameters, which are written within the function’s parentheses, separated by commas.

This function takes a person’s name and whether they have already been greeted as input, and returns an appropriate greeting for that person:
-->

함수는 함수의 소괄호 내에 콤마로 구분하여 여러개의 입력 파라미터를 가질 수 있습니다.

이 함수는 사람의 이름과 이미 인사말을 가지는지에 대해 입력으로 받으며 입력된 사람에 따라 적절한 인사말을 반환합니다:

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
You call the greet(person:alreadyGreeted:) function by passing it both a String argument value labeled person and a Bool argument value labeled alreadyGreeted in parentheses, separated by commas. Note that this function is distinct from the greet(person:) function shown in an earlier section. Although both functions have names that begin with greet, the greet(person:alreadyGreeted:) function takes two arguments but the greet(person:) function takes only one.
-->

`person` 이라 라벨을 가진 `String` 인자값과 `alreadyGreeted` 이라 라벨을 가진 `Bool` 인자값을 소괄호 안에 콤마로 구분하여 `greet(person:alreadyGreeted:)` 함수로 전달하여 호출합니다. 이전 섹션에서 본 `greet(person:)` 함수와는 다른 함수인 것을 명심해야 합니다. 두 함수 모두 `greet` 으로 시작하는 이름을 가지지만 `greet(person:alreadyGreeted:)` 함수는 2개의 인자를 가지지만 `greet(person:)` 함수는 오직 하나의 인자를 가집니다.

### 반환값 없는 함수 \(Functions Without Return Values\)

<!--
Functions aren’t required to define a return type. Here’s a version of the greet(person:) function, which prints its own String value rather than returning it:
-->

함수는 반환 타입 정의를 요구하지 않습니다. 다음은 반환하지 않고 `String` 값을 출력하는 `greet(person:)` 함수 버전입니다:

```swift
func greet(person: String) {
    print("Hello, \(person)!")
}
greet(person: "Dave")
// Prints "Hello, Dave!"
```

<!--
Because it doesn’t need to return a value, the function’s definition doesn’t include the return arrow (->) or a return type.
-->

반환값이 필요하지 않기 때문에 함수의 정의는 반환 화살표 \(`->`\) 또는 반환 타입을 포함하지 않습니다.

<!--
NOTE
Strictly speaking, this version of the greet(person:) function does still return a value, even though no return value is defined. Functions without a defined return type return a special value of type Void. This is simply an empty tuple, which is written as ().
-->

> NOTE  
> 엄밀히 말하면 `greet(person:)` 함수는 반환값을 정의하지 않았지만 여전히 반환값이 있습니다. 반환 타입이 정의되지 않은 함수는 `Void` 타입의 특별한 값을 반환합니다. 이것은 `()` 로 쓰여진 빈 튜플입니다.

<!--
The return value of a function can be ignored when it’s called:
-->

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
The first function, printAndCount(string:), prints a string, and then returns its character count as an Int. The second function, printWithoutCounting(string:), calls the first function, but ignores its return value. When the second function is called, the message is still printed by the first function, but the returned value isn’t used.
-->

첫번째 함수 `printAndCount(string:)` 은 문자열을 출력하고 문자 갯수를 `Int` 로 반환합니다. 두번째 함수 `printWithoutCounting(string:)` 은 첫번째 함수를 호출하지만 반환값을 무시합니다. 두번째 함수가 호출될 때 첫번째 함수에 의해 메세지는 출력되지만 반환된 값은 사용하지 않습니다.

<!--
NOTE
Return values can be ignored, but a function that says it will return a value must always do so. A function with a defined return type can’t allow control to fall out of the bottom of the function without returning a value, and attempting to do so will result in a compile-time error.
-->

> NOTE  
> 반환값은 무시할 수 있지만 함수는 항상 값을 반환할 것입니다. 정의된 반환 타입이 있는 함수는 반환되는 값 없이 함수의 바닥에서 밖으로 빠져 나오는것을 허락하지 않고 그렇게 하면 컴파일 시 에러가 발생합니다.

### 여러개의 반환값이 있는 함수 \(Functions with Multiple Return Values\)

<!--
You can use a tuple type as the return type for a function to return multiple values as part of one compound return value.

The example below defines a function called minMax(array:), which finds the smallest and largest numbers in an array of Int values:
-->

단일 혼합형 반환값의 부분으로 여러개의 값을 반환하기 위해 함수의 반환 타입으로 튜플 타입을 사용할 수 있습니다.

아래 예제는 `Int` 값의 배열에서 가장 작은값과 가장 큰값을 찾는 `minMax(array:)` 함수를 정의합니다:

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
The minMax(array:) function returns a tuple containing two Int values. These values are labeled min and max so that they can be accessed by name when querying the function’s return value.

The body of the minMax(array:) function starts by setting two working variables called currentMin and currentMax to the value of the first integer in the array. The function then iterates over the remaining values in the array and checks each value to see if it’s smaller or larger than the values of currentMin and currentMax respectively. Finally, the overall minimum and maximum values are returned as a tuple of two Int values.

Because the tuple’s member values are named as part of the function’s return type, they can be accessed with dot syntax to retrieve the minimum and maximum found values:
-->

`minMax(array:)` 함수는 2개의 `Int` 값을 포함한 튜플을 반환합니다. 이 값들은 함수의 반환 값을 조회할 때 이름으로 접근할 수 있도록 `min` 과 `max` 로 라벨되어 있습니다.

`minMax(array:)` 함수의 바디는 배열의 첫번째 정수의 값을 `currentMin` 과 `currentMax` 라 불리는 2개의 동작 변수에 값을 설정하는 것으로 시작합니다. 그러면 함수는 배열의 나머지 값들을 반복하고 각 값이 `currentMin` 과 `currentMax` 의 값보다 더 작거나 더 큰지 확인합니다. 마지막으로 가장 작고 가장 큰 값은 2개의 `Int` 값인 튜플로 반환됩니다.

튜플의 멤버 값은 함수의 반환 타입이므로 가장 작은 값과 가장 큰 값을 찾기 위해 점 구문으로 접근할 수 있습니다:

```swift
let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
print("min is \(bounds.min) and max is \(bounds.max)")
// Prints "min is -6 and max is 109"
```

<!--
Note that the tuple’s members don’t need to be named at the point that the tuple is returned from the function, because their names are already specified as part of the function’s return type.
-->

튜플의 멤버 이름은 함수의 반환 타입의 일부로 이미 지정되어 있으므로 함수에서 튜플이 반환되는 시점에 이름을 지정할 필요가 없습니다.

### 옵셔널 튜플 반환 타입 \(Optional Tuple Return Types\)

<!--
If the tuple type to be returned from a function has the potential to have “no value” for the entire tuple, you can use an optional tuple return type to reflect the fact that the entire tuple can be nil. You write an optional tuple return type by placing a question mark after the tuple type’s closing parenthesis, such as (Int, Int)? or (String, Int, Bool)?.
-->

함수에서 반환되는 튜플 타입이 전체 튜플에 대해 "값이 없을" 가능성이 있는 경우 _옵셔널_ 튜플 반환 타입을 사용하여 전체 튜플이 `nil` 일 수 있다는 사실을 반영할 수 있습니다. `(Int, Int)?` 또는 `(String, Int, Bool)?` 와 같이 튜플 타입의 닫는 소괄호 다음에 물음표를 붙여 옵셔널 튜플 반환 타입을 작성합니다.

<!--
NOTE
An optional tuple type such as (Int, Int)? is different from a tuple that contains optional types such as (Int?, Int?). With an optional tuple type, the entire tuple is optional, not just each individual value within the tuple.
-->

> NOTE  
> `(Int, Int)?` 와 같은 옵셔널 튜플 타입은 `(Int?, Int?)` 와 같이 옵셔널 타입을 가지는 튜플과는 다릅니다. 옵셔널 튜플 타입은 튜플 안에 각각의 값이 옵셔널이 아니라 전체 튜플이 옵셔널이라는 의미입니다.

<!--
The minMax(array:) function above returns a tuple containing two Int values. However, the function doesn’t perform any safety checks on the array it’s passed. If the array argument contains an empty array, the minMax(array:) function, as defined above, will trigger a runtime error when attempting to access array[0].

To handle an empty array safely, write the minMax(array:) function with an optional tuple return type and return a value of nil when the array is empty:
-->

위에서 `minMax(array:)` 함수는 2개의 `Int` 값을 포함하는 튜플을 반환합니다. 그러나 이 함수는 배열이 전달될 때 아무런 안정성 확인을 하지 않습니다. `array` 인자가 빈 배열을 포함하면 위에서 정의된 `minMax(array:)` 함수는 `array[0]` 을 접근할 때 런타임 에러가 발생합니다.

빈 배열을 안전하게 처리하기 위해선 `minMax(array:)` 함수는 옵셔널 튜플 반환 타입을 가지고 배열이 비어있을 때 `nil` 을 반환합니다:

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
You can use optional binding to check whether this version of the minMax(array:) function returns an actual tuple value or nil:
-->

옵셔널 바인딩을 통해 이 버전의 `minMax(array:)` 함수가 실제 튜플 값을 반환하는지 `nil` 을 반환하는지 확인할 수 있습니다:

```swift
if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
    print("min is \(bounds.min) and max is \(bounds.max)")
}
// Prints "min is -6 and max is 109"
```

### 암시적 반환을 가진 함수 \(Functions With an Implicit Return\)

<!--
If the entire body of the function is a single expression, the function implicitly returns that expression. For example, both functions below have the same behavior:
-->

함수의 전체 바디가 한줄로 표현이 된다면 함수는 맹목적으로 표현식을 반환합니다. 예를 들어 아래의 두 함수는 모두 같은 동작을 합니다:

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
The entire definition of the greeting(for:) function is the greeting message that it returns, which means it can use this shorter form. The anotherGreeting(for:) function returns the same greeting message, using the return keyword like a longer function. Any function that you write as just one return line can omit the return.

As you’ll see in Shorthand Getter Declaration, property getters can also use an implicit return.
-->

`greeting(for:)` 함수의 전체 정의는 인사말 메세지를 반환하고 이것은 짧은 표현으로 사용할 수 있다는 의미입니다. `anotherGreeing(for:)` 함수는 긴 함수와 같이 `return` 키워드를 사용하여 같은 인사말 메세지를 반환합니다. 단일 `return` 으로 작성된 함수는 `return` 을 생략할 수 있습니다.

[짧은 게터 선언 \(Shorthand Getter Declaration\)](properties.md#getter-shorthand-getter-declaration) 에서 살펴보겠지만 프로퍼티 게터 또한 맹목적 반환을 사용할 수 있습니다.

<!--
NOTE
The code you write as an implicit return value needs to return some value. For example, you can’t use print(13) as an implicit return value. However, you can use a function that never returns like fatalError("Oh no!") as an implicit return value, because Swift knows that the implicit return doesn’t happen.
-->

> NOTE   
> 암시적 반환값으로 작성하는 코드는 일부값을 반환하기 위해 필요합니다. 예를 들어 암시적 반환값으로 `print(13)` 을 사용할 수 없습니다. 그러나 `fatalError("Oh no!")` 와 같이 Swift 가 암시적 반환이 일어나지 않는 것을 아는 경우에는 사용할 수 있습니다.

## 함수 인자 라벨과 파라미터 이름 \(Function Argument Labels and Parameter Names\)

<!--
Each function parameter has both an argument label and a parameter name. The argument label is used when calling the function; each argument is written in the function call with its argument label before it. The parameter name is used in the implementation of the function. By default, parameters use their parameter name as their argument label.
-->

각 함수 파라미터는 _인자 라벨 \(argument label\)_ 과 _파라미터 이름 \(parameter name\)_ 을 가지고 있습니다. 인자 라벨은 함수가 호출될 때 사용되고 각 인자는 함수 호출 시 인자 라벨 다음에 작성합니다. 파라미터 이름은 함수를 구현할 때 사용됩니다. 기본적으로 파라미터는 인자 라벨로 파라미터 이름을 사용합니다.

```swift
func someFunction(firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}
someFunction(firstParameterName: 1, secondParameterName: 2)
```

<!--
All parameters must have unique names. Although it’s possible for multiple parameters to have the same argument label, unique argument labels help make your code more readable.
-->

모든 파라미터는 유니크한 이름을 가져야 합니다. 여러 파라미터에 동일한 인자 라벨을 가질 수 있지만 유니크한 인자 라벨은 코드를 더욱 읽기 편하게 해줍니다.

### 인수 라벨 지정 \(Specifying Argument Labels\)

<!--
You write an argument label before the parameter name, separated by a space:
-->

공백으로 구분하여 파라미터 이름 앞에 인자 라벨을 작성합니다:

```swift
func someFunction(argumentLabel parameterName: Int) {
    // In the function body, parameterName refers to the argument value
    // for that parameter.
}
```

<!--
Here’s a variation of the greet(person:) function that takes a person’s name and hometown and returns a greeting:
-->

다음은 사람의 이름과 고향을 가져와 인사말을 반환하는 `greet(person:)` 함수의 변형입니다:

```swift
func greet(person: String, from hometown: String) -> String {
    return "Hello \(person)!  Glad you could visit from \(hometown)."
}
print(greet(person: "Bill", from: "Cupertino"))
// Prints "Hello Bill!  Glad you could visit from Cupertino."
```

<!--
The use of argument labels can allow a function to be called in an expressive, sentence-like manner, while still providing a function body that’s readable and clear in intent.
-->

인수 라벨을 사용하면 문장과 같은 표현방식으로 함수를 호출할 수 있는 동시에 읽기 쉽고 의도가 명확한 함수 바디를 제공할 수 있습니다.

### 인수 라벨 생략 \(Omitting Argument Labels\)

<!--
If you don’t want an argument label for a parameter, write an underscore (_) instead of an explicit argument label for that parameter.
-->

파라미터에 인수 라벨을 원치 않으면 명시적인 인수 라벨 대신에 언더바 \(`_`\)를 작성합니다.

```swift
func someFunction(_ firstParameterName: Int, secondParameterName: Int) {
    // In the function body, firstParameterName and secondParameterName
    // refer to the argument values for the first and second parameters.
}
someFunction(1, secondParameterName: 2)
```

<!--
If a parameter has an argument label, the argument must be labeled when you call the function.
-->

파라미터가 인수 라벨을 가지고 있다면 함수를 호출할 때 인수는 _반드시_ 라벨을 지정해야 합니다.

### 파라미터 기본값 \(Default Parameter Values\)

<!--
You can define a default value for any parameter in a function by assigning a value to the parameter after that parameter’s type. If a default value is defined, you can omit that parameter when calling the function.
-->

파라미터의 타입 뒤에 파라미터 값을 할당하여 함수의 파라미터에 _기본값 \(default value\)_ 을 정의할 수 있습니다. 기본값이 정의되어 있다면 함수를 호출할 때 파라미터를 생략할 수 있습니다.

```swift
func someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {
    // If you omit the second argument when calling this function, then
    // the value of parameterWithDefault is 12 inside the function body.
}
someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6) // parameterWithDefault is 6
someFunction(parameterWithoutDefault: 4) // parameterWithDefault is 12
```

<!--
Place parameters that don’t have default values at the beginning of a function’s parameter list, before the parameters that have default values. Parameters that don’t have default values are usually more important to the function’s meaning—writing them first makes it easier to recognize that the same function is being called, regardless of whether any default parameters are omitted.
-->

기본값이 없는 파라미터는 함수의 파라미터 목록 시작부분에 위치하고 기본값이 있는 파라미터 전에 위치합니다. 기본값이 없는 파라미터는 일반적으로 함수의 의미에 더 중요합니다. 이를 먼저 작성하면 기본 파라미터가 생략되었는지 여부에 관계없이 동일한 함수가 호출되고 있음을 쉽게 인식할 수 있습니다.

### 가변 파라미터 \(Variadic Parameters\)

<!--
A variadic parameter accepts zero or more values of a specified type. You use a variadic parameter to specify that the parameter can be passed a varying number of input values when the function is called. Write variadic parameters by inserting three period characters (...) after the parameter’s type name.

The values passed to a variadic parameter are made available within the function’s body as an array of the appropriate type. For example, a variadic parameter with a name of numbers and a type of Double... is made available within the function’s body as a constant array called numbers of type [Double].

The example below calculates the arithmetic mean (also known as the average) for a list of numbers of any length:
-->

_가변 파라미터 \(variadic parameter\)_ 는 0개 이상의 특정 타입의 값을 허용합니다. 함수가 호출될 때 여러개의 입력값이 전달될 수 있는 특정 파라미터는 가변 파라미터를 사용합니다. 가변 파라미터는 파라미터의 타입 이름 뒤에 세개의 기간 문자 \(`...`\)를 추가하여 작성합니다.

가변 파라미터에 전달된 값은 함수 바디 내에서 적절한 타입의 배열로 사용할 수 있습니다. 예를 들어 `numbers` 라는 이름과 `Double...` 타입을 가진 가변 파라미터는 함수 바디 내에서 `[Double]` 타입의 `numbers` 라 불리는 상수 배열로 사용할 수 있습니다.

아래 예제는 길이에 관계없이 숫자 목록에 대한 _산술 평균_ 을 계산합니다:

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
A function can have multiple variadic parameters. The first parameter that comes after a variadic parameter must have an argument label. The argument label makes it unambiguous which arguments are passed to the variadic parameter and which arguments are passed to the parameters that come after the variadic parameter.
-->

함수는 여러개의 가변 파라미터를 가질 수 있습니다. 가변 파라미터 뒤에 오는 첫번째 파라미터는 인수 라벨을 가지고 있어야 합니다. 인수 라벨은 가변 파라미터에 전달되는 인수와 가변 파라미터 뒤에 오는 파라미터에 전달되는 인수를 명확하게 합니다.

### In-Out 파라미터 \(In-Out Parameters\)

<!--
Function parameters are constants by default. Trying to change the value of a function parameter from within the body of that function results in a compile-time error. This means that you can’t change the value of a parameter by mistake. If you want a function to modify a parameter’s value, and you want those changes to persist after the function call has ended, define that parameter as an in-out parameter instead.

You write an in-out parameter by placing the inout keyword right before a parameter’s type. An in-out parameter has a value that’s passed in to the function, is modified by the function, and is passed back out of the function to replace the original value. For a detailed discussion of the behavior of in-out parameters and associated compiler optimizations, see In-Out Parameters.

You can only pass a variable as the argument for an in-out parameter. You can’t pass a constant or a literal value as the argument, because constants and literals can’t be modified. You place an ampersand (&) directly before a variable’s name when you pass it as an argument to an in-out parameter, to indicate that it can be modified by the function.
-->

함수 파라미터는 기본적으로 상수입니다. 해당 함수의 바디 내에서 함수 파라미터 값을 변경하려고 하면 컴파일 타입 에러가 발생합니다. 이것은 실수로 파라미터의 값을 변경할 수 없다는 것을 의미합니다. 함수의 파라미터 값을 변경하고 함수 호출이 종료된 후에도 이러한 변경된 값을 유지하고 싶다면 _in-out 파라미터 \(in-out parameter\)_ 로 대신 정의하십시오.

in-out 파라미터는 파라미터의 타입 바로 전에 `inout` 키워드를 작성합니다. in-out 파라미터는 함수로 전달하는 값을 가지고 있고 함수로 부터 이 값을 수정하고 원래 값을 대체하기위해 함수 밖으로 다시 되돌려 줍니다. In-out 파라미터와 컴파일러 최적화의 동작에 대한 자세한 설명은 [In-Out 파라미터 \(In-Out Parameters\)](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#ID545) 를 참고 바랍니다.

in-out 파라미터의 인자로 변수만 전달할 수 있습니다. 상수와 반복은 수정할 수 없기 때문에 인자로 상수 또는 반복 값은 전달할 수 없습니다. 함수에 수정가능함을 알리기 위해 in-out 파라미터에 인자로 전달할 때 변수의 이름 앞에 앰퍼샌드 \(`&`\)를 붙여줍니다.

<!--
NOTE
In-out parameters can’t have default values, and variadic parameters can’t be marked as inout.
-->

> NOTE  
> in-out 파라미터는 기본값을 가질 수 없고 가변 파라미터는 `inout` 으로 표기할 수 없습니다.

<!--
Here’s an example of a function called swapTwoInts(_:_:), which has two in-out integer parameters called a and b:
-->

다음 예제의 함수는 `a` 와 `b` 라 하는 2개의 in-out 정수 파라미터를 가지는 `swapTwoInts(_:_:)` 입니다:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

<!--
The swapTwoInts(_:_:) function simply swaps the value of b into a, and the value of a into b. The function performs this swap by storing the value of a in a temporary constant called temporaryA, assigning the value of b to a, and then assigning temporaryA to b.

You can call the swapTwoInts(_:_:) function with two variables of type Int to swap their values. Note that the names of someInt and anotherInt are prefixed with an ampersand when they’re passed to the swapTwoInts(_:_:) function:
-->

`swapTwoInts(_:_:)` 함수는 간단하게 `b` 의 값을 `a` 로 `a` 의 값을 `b` 로 바꿉니다. 이 함수는 `a` 의 값을 `temporaryA` 라 하는 임시 상수에 저장하고 `b` 의 값을 `a` 에 할당하고 `temporaryA` 의 값을 `b` 에 할당합니다.

`Int` 타입의 바꿀 2개의 변수를 이용하여 `swapTwoInts(_:_:)` 함수를 호출할 수 있습니다. `swapTwoInts(_:_:)` 함수에 전달할 때 `someInt` 와 `anotherInt` 의 이름 앞에 앰퍼샌드를 붙여야 합니다:

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// Prints "someInt is now 107, and anotherInt is now 3"
```

<!--
The example above shows that the original values of someInt and anotherInt are modified by the swapTwoInts(_:_:) function, even though they were originally defined outside of the function.
-->

위의 예제는 `someInt` 와 `anotherInt` 의 기본값이 함수의 바깥에서 정의되었지만 `swapTwoInts(_:_:)` 함수로 인해 원래값이 수정되는 것을 보여줍니다.

<!--
NOTE
In-out parameters aren’t the same as returning a value from a function. The swapTwoInts example above doesn’t define a return type or return a value, but it still modifies the values of someInt and anotherInt. In-out parameters are an alternative way for a function to have an effect outside of the scope of its function body.
-->

> NOTE  
> in-out 파라미터는 함수에서 값을 반환하는 것과 다릅니다. 위의 `swapTwoInts` 예제는 반환 타입을 정의하거나 값을 반환하지 않지만 `someInt` 와 `anotherInt` 의 값은 여전히 수정합니다. In-out 파라미터는 함수가 함수 바디의 범위를 벗어나 영향을 미치는 다른 방법입니다.

## 함수 타입 \(Function Types\)

<!--
Every function has a specific function type, made up of the parameter types and the return type of the function.

For example:
-->

모든 함수는 파라미터 타입과 반환 타입으로 구성된 특정 _함수 타입 \(function type\)_ 이 있습니다.

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
This example defines two simple mathematical functions called addTwoInts and multiplyTwoInts. These functions each take two Int values, and return an Int value, which is the result of performing an appropriate mathematical operation.

The type of both of these functions is (Int, Int) -> Int. This can be read as:

“A function that has two parameters, both of type Int, and that returns a value of type Int.”

Here’s another example, for a function with no parameters or return value:
-->

이 예제는 `addTwoInts` 와 `multiplyTwoInts` 라 불리는 간단한 수학 방정식 함수를 정의합니다. 이 함수는 각각 2개의 `Int` 값을 가지며 적절한 수학 연산을 수행하여 `Int` 값을 반환합니다.

이 두 함수의 타입은 `(Int, Int) -> Int` 입니다. 이것은 아래와 같이 읽을 수 있습니다:

"함수는 모두 `Int` 타입인 2개의 파라미터를 가지며 `Int` 타입을 반환합니다."

다음의 예제는 파라미터 또는 반환 값이 없는 함수 입니다:

```swift
func printHelloWorld() {
    print("hello, world")
}
```

<!--
The type of this function is () -> Void, or “a function that has no parameters, and returns Void.”
-->

이 함수의 타입은 `() -> Void` 또는 "함수는 파라미터가 없고 `Void` 를 반환합니다."

### 함수 타입 사용 \(Using Function Types\)

<!--
You use function types just like any other types in Swift. For example, you can define a constant or variable to be of a function type and assign an appropriate function to that variable:
-->

Swift에서 다른 타입처럼 함수 타입을 사용합니다. 예를 들어 함수 타입에 대해 상수 또는 변수로 정의 할 수 있고 변수에 적절한 함수 타입을 할당할 수 있습니다:

```swift
var mathFunction: (Int, Int) -> Int = addTwoInts
```

<!--
This can be read as:

“Define a variable called mathFunction, which has a type of ‘a function that takes two Int values, and returns an Int value.’ Set this new variable to refer to the function called addTwoInts.”

The addTwoInts(_:_:) function has the same type as the mathFunction variable, and so this assignment is allowed by Swift’s type-checker.

You can now call the assigned function with the name mathFunction:
-->

이것은 이렇게 읽을 수 있습니다:

"'2개의 `Int` 값과 `Int` 를 반환하는 함수' 의 타입을 가지는 `mathFunction` 이라는 변수를 정의합니다. `addTwoInts` 라는 함수를 참조하기 위해 새로운 변수에 설정 하십시오."

`addTwoInts(_:_:)` 함수는 `mathFunction` 변수와 같은 타입을 가지므로 Swift의 타입 검사기는 할당을 허락합니다.

`mathFunction` 이라는 이름으로 할당된 함수를 호출할 수 있습니다:

```swift
print("Result: \(mathFunction(2, 3))")
// Prints "Result: 5"
```

<!--
A different function with the same matching type can be assigned to the same variable, in the same way as for nonfunction types:
-->

비함수 타입과 동일한 방식으로 같은 타입으로 일치하는 다른 함수를 같은 변수에 할당할 수 있습니다:

```swift
mathFunction = multiplyTwoInts
print("Result: \(mathFunction(2, 3))")
// Prints "Result: 6"
```

<!--
As with any other type, you can leave it to Swift to infer the function type when you assign a function to a constant or variable:
-->

다른 타입과 마찬가지로 상수 또는 변수에 함수를 할당할 때 함수 타입을 추론하기 위해 Swift에 맡길 수 있습니다:

```swift
let anotherMathFunction = addTwoInts
// anotherMathFunction is inferred to be of type (Int, Int) -> Int
```

### 파라미터 타입으로 함수 타입 \(Function Types as Parameter Types\)

<!--
You can use a function type such as (Int, Int) -> Int as a parameter type for another function. This enables you to leave some aspects of a function’s implementation for the function’s caller to provide when the function is called.

Here’s an example to print the results of the math functions from above:
-->

`(Int, Int) -> Int` 와 같은 함수 타입을 다른 함수의 파라미터로 사용할 수 있습니다. 이렇게 하면 함수 호출자가 함수가 호출될 때 제공할 함수 구현의 일부 측면을 남길 수 있습니다.

다음의 예제는 위 수학 함수의 결과를 출력합니다:

```swift
func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
// Prints "Result: 8"
```

<!--
This example defines a function called printMathResult(_:_:_:), which has three parameters. The first parameter is called mathFunction, and is of type (Int, Int) -> Int. You can pass any function of that type as the argument for this first parameter. The second and third parameters are called a and b, and are both of type Int. These are used as the two input values for the provided math function.

When printMathResult(_:_:_:) is called, it’s passed the addTwoInts(_:_:) function, and the integer values 3 and 5. It calls the provided function with the values 3 and 5, and prints the result of 8.

The role of printMathResult(_:_:_:) is to print the result of a call to a math function of an appropriate type. It doesn’t matter what that function’s implementation actually does—it matters only that the function is of the correct type. This enables printMathResult(_:_:_:) to hand off some of its functionality to the caller of the function in a type-safe way.
-->

이 예제는 3개의 파라미터를 가지는 `printMathResult(_:_:_:)` 라는 함수를 정의합니다. 첫번째 파라미터는 `mathFunction` 이라 불리는 `(Int, Int) -> Int` 타입입니다. 이 첫번째 파라미터의 인수로 해당 타입의 함수를 전달할 수 있습니다. 두번째 세번째 파라미터는 `a` 와 `b` 라 불리고 둘다 `Int` 타입입니다. 이들은 제공된 수학 함수의 2개의 입력값으로 사용됩니다.

`printMathResult(_:_:_:)` 호출할 때 `addTwoInts(_:_:)` 함수와 정수 `3` 과 `5` 가 전달됩니다. `3` 과 `5` 로 제공된 함수를 호출하고 `8` 의 결과를 출력합니다.

`printMathResult(_:_:_:)` 의 역할은 적절한 타입의 수학 함수를 호출하고 값을 출력합니다. 이것은 함수의 실질 동작 구현이 어떻게 되는지는 상관없고 함수의 올바른 타입에 대해서만 관여합니다. 이렇게 하면 `printMathResult(_:_:_:)` 가 해당 기능의 일부를 타입이 안전한 방식으로 함수 호출자에게 전달할 수 있습니다.

### 반환 타입으로 함수 타입 \(Function Types as Return Types\)

<!--
You can use a function type as the return type of another function. You do this by writing a complete function type immediately after the return arrow (->) of the returning function.

The next example defines two simple functions called stepForward(_:) and stepBackward(_:). The stepForward(_:) function returns a value one more than its input value, and the stepBackward(_:) function returns a value one less than its input value. Both functions have a type of (Int) -> Int:
-->

다른 함수의 반환 타입으로 함수 타입을 사용할 수 있습니다. 반환하는 함수의 반환 화살표 \(`->`\) 바로 뒤에 완전한 함수 타입을 작성하여 이를 수행합니다.

다음 예제는 `stepForward(_:)` 와 `stepBackward(_:)` 라 불리는 간단한 2개의 함수를 정의합니다. `stepForward(_:)` 함수는 입력값에 1을 더해 반환하고 `stepBackward(_:)` 함수는 입력값에 1을 빼고 반환합니다. 두 함수 모두 `(Int) -> Int` 타입입니다:

```swift
func stepForward(_ input: Int) -> Int {
    return input + 1
}
func stepBackward(_ input: Int) -> Int {
    return input - 1
}
```

<!--
Here’s a function called chooseStepFunction(backward:), whose return type is (Int) -> Int. The chooseStepFunction(backward:) function returns the stepForward(_:) function or the stepBackward(_:) function based on a Boolean parameter called backward:
-->

다음의 함수는 반환 타입이 `(Int) -> Int` 인 `chooseStepFunction(backward:)` 입니다. `chooseStepFunction(backward:)` 함수는 `backward` 부울 파라미터를 토대로 `stepForward(_:)` 함수 또는 `stepBackward(_:)` 함수를 반환합니다:

```swift
func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    return backward ? stepBackward : stepForward
}
```

<!--
You can now use chooseStepFunction(backward:) to obtain a function that will step in one direction or the other:
-->

이제 `chooseStepFunction(backward:)` 를 사용하여 한방향 또는 다른 방향으로 이동할 함수를 얻을 수 있습니다:

```swift
var currentValue = 3
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
// moveNearerToZero now refers to the stepBackward() function
```

<!--
The example above determines whether a positive or negative step is needed to move a variable called currentValue progressively closer to zero. currentValue has an initial value of 3, which means that currentValue > 0 returns true, causing chooseStepFunction(backward:) to return the stepBackward(_:) function. A reference to the returned function is stored in a constant called moveNearerToZero.

Now that moveNearerToZero refers to the correct function, it can be used to count to zero:
-->

위의 예제는 `currentValue` 라는 변수를 점차적으로 0에 가깝게 이동하기 위해 양수 또는 음수 단계가 필요한지 여부를 결정합니다. `currentValue` 는 `3` 의 초기값을 가지며 이것은 `currentValue > 0` 이 `true` 를 반환한다는 의미이며 `chooseStepFunction(backward:)` 가 `stepBackward(_:)` 함수를 반환하도록 합니다. 반환된 함수의 참조는 `moveNearerToZero` 라는 상수에 저장됩니다.

`moveNearerToZero` 가 올바른 함수를 참조하므로 0으로 계산하는데 사용할 수 있습니다:

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

## 중첩 함수 \(Nested Functions\)

<!--
All of the functions you have encountered so far in this chapter have been examples of global functions, which are defined at a global scope. You can also define functions inside the bodies of other functions, known as nested functions.

Nested functions are hidden from the outside world by default, but can still be called and used by their enclosing function. An enclosing function can also return one of its nested functions to allow the nested function to be used in another scope.

You can rewrite the chooseStepFunction(backward:) example above to use and return nested functions:
-->

이 챕터에서 접한 모든 함수는 전역 범위에서 정의된 _전역 함수 \(global functions\)_ 의 예입니다. _중첩 함수 \(nested functions\)_ 라고 하는 다른 함수 내에 함수를 정의할 수도 있습니다.

중첩 함수는 기본적으로 바깥에서 보이지 않지만 중첩 함수를 둘러싼 함수를 통해 호출될 수 있고 사용될 수 있습니다. 중첩 함수를 둘러싼 함수는 중첩 함수 중 하나를 반환하여 중첩 함수를 다른 범위에서 사용할 수도 있습니다.

위의 예제 `chooseStepFunction(backward:)` 를 중첩 함수를 사용하고 반환하도록 다시 작성할 수 있습니다:

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

