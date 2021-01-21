# 기본 연산자 \(Basic Operators\)

_연산자 \(operator\)_ 는 값을 체크, 변경, 또는 결합하기 위해 사용하는 기호 또는 구 입니다. 예를 들어 더하기 연산자 \(`+`\)는 `let i = 1 + 2` 에서처럼 두 숫자를 더하고 논리 AND 연산자 \(`&&`\)는 `if enteredDoorCode && passedRetinaScan` 에서처럼 두 부울 값을 결합합니다.

Swift는 C와 같은 언어에서 알고있는 연산자를 지원하고 코딩 에러를 제거하기 위해 여러 기능을 향상시킵니다. 대입 연산자 \(`=`\)는 등호 연산자 \(`==`\)를 의도했을 때 실수로 사용되지 않도록 값을 반환하지 않습니다. 산술 연산자 \(`+`, `-`, `*`, `/`, `%` 등\)는 타입이 수용할 수 있는 값의 범위보다 크거나 작은 숫자로 작업을 수행하려 할 때 예기치 않은 결과를 피하기 위해 값 오버플로우를 감지하고 허락하지 않습니다. [오버플로우 연산자 \(Overflow Operators\)](advanced-operators.md#overflow-operators) 에서 설명한 대로 Swift 오버플로우 연산자를 사용하여 값 오버플로우 동작을 결정할 수 있습니다.

Swift는 C에서는 없는 값의 범위를 나타내는 `a..<b` 및 `a...b` 와 같은 범위 연산자를 제공합니다.

이 챕터에서는 Swift의 기본 연산자에 대해 설명합니다. [고급 연산자 \(Advanced Operators\)](advanced-operators.md) 에서는 Swift의 고급 연산자에 대해 다루고 사용자 정의 연산자를 어떻게 정의하고 사용자 정의 타입을 위한 표준 연산자의 구현은 어떻게 하는지 설명합니다.

## 술어 \(Terminology\)

연산자는 단항 \(unary\), 이항 \(binary\), 또는 삼항 \(ternary\)입니다:

* _단항 \(Unary\)_ 연산자는 `-a` 처럼 단일 항목에 동작합니다. 단항 _접두사_ 연산자는 `!b` 처럼 항목 바로 직전에 위치하고 단항 _접미사_ 연산자는 `c!` 처럼 항목 바로 다음에 위치합니다.
* _이항 \(Binary\)_ 연산자는 `2 + 3` 처럼 2개의 항목에 동작하고 2개의 항목 사이에 위치해야 하므로 위치는 _고정_ 입니다.
* _삼항 \(Ternary\)_ 연산자는 3개의 항목에 동작합니다. C 처럼 Swift는 하나의 삼항 연산자만 있으며 삼항 조건 연산자 \(`a ? b : c`\)입니다.

연산자가 영향을 주는 값은 _피연산자_ 입니다. `1 + 2` 표현식에서 `+` 기호는 이항 연산자이고 값 `1` 과 `2` 는 피연산자 입니다.

## 대입 연산자 \(Assignment Operator\)

_대입 연산자 \(assignment operator\)_ \(`a = b`\) 는 `b` 의 값으로 초기화 되거나 업데이트 됩니다:

```swift
let b = 10
var a = 5
a = b
// a is now equal to 10
```

대입의 우항이 여러개의 값이 있는 튜플이라면 튜플의 요소는 여러개의 상수 또는 변수로 한번에 분해할 수 있습니다:

```swift
let (x, y) = (1, 2)
// x is equal to 1, and y is equal to 2
```

C와 Objective-C에서의 대입 연산자와 다르게 Swift의 대입 연산자는 값을 반환하지 않습니다. 아래 예문은 유효하지 않습니다:

```swift
if x = y {
    // This is not valid, because x = y does not return a value.
}
```

이 특징은 등호 연산자 \(`==`\)가 실제로 사용되어야 할 곳에 실수로 할당 연산자 \(`=`\)가 사용되는 것을 방지합니다. `if x = y` 가 유효하지 않는 것처럼 Swift는 코드에서 이런 종류의 에러를 피할 수 있도록 도와줍니다.

## 산술 연산자 \(Arithmetic Operators\)

Swift는 모든 숫자 타입에 대해 4개의 기본 _산술 연산자 \(arithmetic operators\)_ 를 제공합니다:

* 덧셈 \(`+`\)
* 뺄셈 \(`-`\)
* 곱셈 \(`*`\)
* 나눗셈 \(`/`\)

```swift
1 + 2       // equals 3
5 - 3       // equals 2
2 * 3       // equals 6
10.0 / 2.5  // equals 4.0
```

C와 Objective-C에서의 산술 연산자와 다르게 Swift 산술 연산자는 기본적으로 값이 오버플로우 되는 것을 허락하지 않습니다. `a &+ b` 와 같은 Swift의 오버플로우 연산자를 사용하여 값 오버플로우 동작을 선택할 수 있습니다. 자세한 내용은 [오버플로우 연산자 \(Overflow Operators\)](advanced-operators.md#overflow-operators) 를 참고 바랍니다.

덧셈 연산자는 `String` 연결도 지원합니다:

```swift
"hello, " + "world"  // equals "hello, world"
```

### 나머지 연산자 \(Remainder Operator\)

_나머지 연산자 \(remainder operator\)_ \(`a % b`\)는 `a` 안에 들어갈 `b` 의 배수가 몇인지를 계산하고 남은 값 \(나머지\)을 반환합니다.

> NOTE   
> 나머지 연산자 \(`%`\)는 다른 언어에서는 _모듈로 연산자 \(modulo operator\)_ 라고 합니다. 그러나 음수에 대한 Swift의 동작은 모듈로 연산이 아닌 나머지 입니다.

여기 나머지 연산자가 어떻게 동작하는지 살펴봅시다. `9 % 4` 를 계산하기위해 `9` 안에 얼마나 많은 `4` 가 들어가는지 알아야 합니다:

![](../.gitbook/assets/02_remainderInteger_2x.png)

`9` 에 `4` 가 2번 들어가고 오렌지 색과 마찬가지로 `1` 이 남는 것을 알 수 있습니다.

Swift에서 이것을 작성해보면:

```swift
9 % 4    // equals 1
```

`a % b` 의 결과를 구하기 위해 `%` 연산자는 아래의 방정식을 계산하고 출력으로 `remainder` 를 반환합니다:

`a = (b x some multiplier) + remainder`

여기서 `some multiplier` 는 `a` 에 들어갈 `b` 의 최대배수 입니다.

이 방정식에 `9` 와 `4` 를 넣고 계산하면:

`9 = (4 x 2) + 1`

`a` 에 음수를 이용하여 나머지를 계산할 때도 같은 메서드로 적용합니다:

```swift
-9 % 4   // equals -1
```

방정식에 `-9` 와 `4` 를 넣고 계산하면:

`-9 = (4 x -2) + -1`

`-1` 의 나머지를 얻습니다.

`b` 에 음수는 무시됩니다. 이것은 `a % b` 와 `a % -b` 는 항상 같은 결과를 얻는다는 의미입니다.

### 단항 빼기 연산자 \(Unary Minus Operator\)

숫자 값의 부호는 `-` 접미사를 사용하여 변경할 수 있으며 이것을 _단항 빼기 연산자 \(unary minus operator\)_ 라고 합니다.

```swift
let three = 3
let minusThree = -three       // minusThree equals -3
let plusThree = -minusThree   // plusThree equals 3, or "minus minus three"
```

단항 빼기 연산자 \(`-`\)는 공백없이 작동하는 값 바로 앞에 추가됩니다.

### 단항 더하기 연산자 \(Unary Plus Operator\)

_단항 더하기 연산자_ \(`+`\)는 어떠한 변경없이 그 값을 그대로 반환합니다:

```swift
let minusSix = -6
let alsoMinusSix = +minusSix  // alsoMinusSix equals -6
```

단항 더하기 연산자는 실제로 아무런 동작을 하지 않지만 음수에 단항 빼기 연산자를 사용할 때 양수에 대칭을 위해 사용할 수 있습니다.

## 복합 대입 연산자 \(Compound Assignment Operators\)

C처럼 Swift는 대입 \(`=`\)과 다른 연산자를 결합한 _복합 대입 연산자 \(compound assignment operators\)_ 를 제공합니다. 아래 예제는 _덧셈 대입 연산자_ \(`+=`\) 입니다:

```swift
var a = 1
a += 2
// a is now equal to 3
```

표현식 `a += 2` 는 `a = a + 2` 의 짧은 표현입니다. 효과적으로 덧셈과 대입은 동시에 수행되고 하나의 연산자로 결합됩니다.

> NOTE   
> 복합 대입 연산자는 값을 반환하지 않습니다. 예를 들어 `let b = a += 2` 로 작성할 수 없습니다.

Swift 표준 라이브러리에서 제공하는 연산자에 대한 내용은 [연산자 선언 \(Operator Declarations\)](https://developer.apple.com/documentation/swift/operator_declarations) 를 참고 바랍니다.

## 비교 연산자 \(Comparison Operators\)

Swift는 아래의 비교 연산자 \(comparison operators\)를 제공합니다:

* 같음 \(`a == b`\)
* 다름 \(`a != b`\)
* 보다 큼 \(`a > b`\)
* 보다 작음 \(`a < b`\)
* 보다 크거나 같음 \(`a >= b`\)
* 보다 작거나 같은 \(`a <= b`\)

> NOTE   
> Swift는 또한 2개의 객체 참조가 동일한 객체 인스턴스를 참조하는지 판별하는 2개의 _식별 연산자 \(identity operators\)_ \(`===` 와 `!==`\)를 제공합니다. 자세한 내용은 [식별 연산자 \(Identity Operators\)](structures-and-classes.md#identity-operators) 를 참고 바랍니다.

각 비교 연산자는 구문이 `true` 인지 아닌지 판단하기 위해 `Bool` 값을 반환합니다:

```swift
1 == 1   // true because 1 is equal to 1
2 != 1   // true because 2 is not equal to 1
2 > 1    // true because 2 is greater than 1
1 < 2    // true because 1 is less than 2
1 >= 1   // true because 1 is greater than or equal to 1
2 <= 1   // false because 2 is not less than or equal to 1
```

비교 연산자는 `if` 구문과 같은 조건 구문에서 사용되기도 합니다:

```swift
let name = "world"
if name == "world" {
    print("hello, world")
} else {
    print("I'm sorry \(name), but I don't recognize you")
}
// Prints "hello, world", because name is indeed equal to "world".
```

`if` 구문에 대한 자세한 내용은 [제어 흐 \(Control Flow\)](control-flow.md) 를 참고 바랍니다.

같은 타입과 같은 갯수의 값을 가지고 있는 2개의 튜플은 비교할 수 있습니다. 튜플은 2개의 값이 다를 때까지 왼쪽에서 오른쪽으로 한번에 하나씩 비교합니다. 두개의 값이 비교되고 해당 비교 결과에 따라 튜플 비교의 전체 결과가 결정됩니다. 모든 요소가 동일하면 튜플은 같습니다. 예를 들어:

```swift
(1, "zebra") < (2, "apple")   // true because 1 is less than 2; "zebra" and "apple" are not compared
(3, "apple") < (3, "bird")    // true because 3 is equal to 3, and "apple" is less than "bird"
(4, "dog") == (4, "dog")      // true because 4 is equal to 4, and "dog" is equal to "dog"
```

위의 예제 첫번째 줄에서 왼쪽에서 오른쪽으로 비교하는 동작을 확인할 수 있습니다. 튜플의 다른 어떤값과 상관없이 `1` 이 `2` 보다 작기 때문에 `(1, "zebra")` 는 `(2, "apple")` 보다 작습니다. 튜플의 첫번째 요소에 의해 비교가 이미 마쳤기 때문에 `"zebra"` 가 `"apple"` 보다 더 작은지 여부는 아무런 상관이 없습니다. 그러나 튜플의 첫번째 요소가 같을 때는 두번째 요소가 _비교됩니다._ - 위 예제에서 두번째와 세번째 줄의 결과를 살펴보면 알 수 있습니다.

튜플은 해당 튜플의 각 값에 연산자를 적용할 수 있을 때에만 주어진 연산자로 비교할 수 있습니다. 예를 들어 아래의 코드에서 `String` 과 `Int` 값은 `<` 연산자를 사용하여 비교가 가능하므로 타입 `(String, Int)` 의 2개의 튜플은 비교할 수 있습니다. 반대로 `<` 연산자는 `Bool` 값에 적용할 수 없기 때문에 타입 `(String, Bool)` 의 2개의 튜플은 `<` 연산자로 비교할 수 없습니다.

```swift
("blue", -1) < ("purple", 1)        // OK, evaluates to true
("blue", false) < ("purple", true)  // Error because < can't compare Boolean values
```

> NOTE   
> Swift 표준 라이브러리는 7개 미만의 요소를 가지고 있는 튜플에 대해 튜플 비교 연산자를 제공합니다. 7개 이상의 요소의 튜플을 비교하려면 비교 연산자를 직접 구현해야 합니다.

## 삼항 조건 연산자 \(Ternary Conditional Operator\)

_삼항 조건 연산자 \(ternary conditional operator\)_ 는 `question ? Answer1 : answer2` 형태의 3가지 부분으로 이루어진 특별한 연산자입니다. `question` 이 참 또는 거짓인지에 따라 2개의 표현식 중 하나를 나타내는 식입니다. `question` 이 참이라면 `answer1` 을 반환하고 반대면 `answer2` 를 반환합니다.

삼항 조건 연산자는 아래의 코드를 줄여서 표현한 것입니다:

```swift
if question {
    answer1
} else {
    answer2
}
```

테이블 열의 높이를 계산하는 예제가 있습니다. 테이블 열은 헤더를 가지고 있다면 콘텐츠 높이보다 50 포인트 더 커야 하고 헤더가 없다면 20 포인트 더 커야 합니다:

```swift
let contentHeight = 40
let hasHeader = true
let rowHeight = contentHeight + (hasHeader ? 50 : 20)
// rowHeight is equal to 90
```

위의 예제는 아래의 코드를 짧게 표현한 것입니다:

```swift
let contentHeight = 40
let hasHeader = true
let rowHeight: Int
if hasHeader {
    rowHeight = contentHeight + 50
} else {
    rowHeight = contentHeight + 20
}
// rowHeight is equal to 90
```

첫번째 예에서 삼항 조건 연산자를 사용하면 `rowHeight` 를 한줄의 코드로 올바른 값으로 설정할 수 있으며 두번째 예에서 사용된 코드보다 간결합니다.

삼항 조건 연산자는 두 표현식 중 하나를 결정하기 위한 효율적으로 간결한 표현을 제공합니다. 그러나 삼항 조건 연산자 사용시에는 주의가 필요합니다. 너무 남용하면 읽기 어려운 코드가 될 수 있습니다. 삼항 조건 연산자의 여러 인스턴스를 하나의 복합 구문으로 결합하는 것을 피해야 합니다.

## Nil-결합 연산자 \(Nil-Coalescing Operator\)

_nil-결합 연산자 \(nil-coalescing operator\)_ \(`a ?? b`\)는 옵셔널 `a` 에 값이 있으면 `a` 를 풀거나 `a` 가 `nil` 이면 기본값 `b` 를 반환합니다. 표현식 `a` 는 항상 옵셔널 타입 입니다. 표현식 `b` 는 `a` 에 저장된 타입과 같아야 합니다.

nil-결합 연산자는 아래 코드를 짧게 표현할 수 있습니다:

```swift
a != nil ? a! : b
```

위 코드는 삼항 조건 연산자를 사용하고 `a` 가 `nil` 이 아닐경우 `a` 안에 래핑된 값을 접근하기 위해 강제로 언래핑 \(`a!`\) 하며 `a` 가 `nil` 일 경우 `b` 를 반환합니다. nil-결합 연산자는 조건 검사 및 언래핑을 간결하고 읽기 쉬운 형태로 캡슐화 합니다.

> NOTE   
> `a` 의 값이 `nil` 이 아닐경우 `b` 는 절대 반환되지 않습니다. 이러한 경우를 _연산 생략 \(short-circuit evaluation\)_ 이라 합니다.

아래 예제는 기본 컬러 이름과 옵셔널 사용자가 정의한 컬러 이름 중 선택하기 위해 nil-결합 연산자를 사용합니다:

```swift
let defaultColorName = "red"
var userDefinedColorName: String?   // defaults to nil

var colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName is nil, so colorNameToUse is set to the default of "red"
```

`userDefinedColorName` 변수는 기본값이 `nil` 인 옵셔널 `String` 으로 정의됩니다. `userDefinedColorName` 은 옵셔널 타입이기 때문에 값을 선택하기 위해 nil-결합 연산자를 사용할 수 있습니다. 위의 예제에서 연산자는 `colorNameToUse` 라는 `String` 변수의 초기값을 결정하는데 사용됩니다. `userDefinedColorName` 이 `nil` 이기 때문에 표현식 `userDefinedColorName ?? defaultColorName` 은 `defaultColorName` 또는 `"red"` 의 값을 반환합니다.

`userDefinedColorName` 에 `nil` 이 아닌 값을 대입하고 nil-결합 연산자로 체크를 다시 수행하면 기본값 대신에 `userDefinedColorName` 에 래핑된 값을 사용합니다:

```swift
userDefinedColorName = "green"
colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName is not nil, so colorNameToUse is set to "green"
```

## 범위 연산자 \(Range Operators\)

Swift는 값의 범위를 짧게 표현하기 위해 몇몇의 _범위 연산자 \(range operators\)_ 를 포함합니다.

### 닫힌 범위 연산자 \(Closed Range Operator\)

_닫힌 범위 연산자 \(closed range operator\)_ \(`a...b`\)는 값 `a` 와 `b` 가 포함된 `a` 부터 `b` 까지의 범위 실행을 정의합니다. `a` 의 값은 `b` 보다 클 수 없습니다.

닫힌 범위 연산자는 `for`-`in` 루프와 같이 모든 값을 사용할 범위를 반복할 때 유용합니다:

```swift
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```

`for`-`in` 루프에 대한 자세한 설명은 [제어 흐름 \(Control Flow\)](control-flow.md) 을 참고 바랍니다.

### 반-열림 범위 연산자 \(Half-Open Range Operator\)

_반-열림 범위 연산자 \(half-open range operator\)_ \(`a..<b`\)는 `b` 가 포함되지 않은 `a` 부터 `b` 까지의 범위 실행을 정의합니다. 이것은 마지막 값은 포함되지 않지만 처음 값은 포함되므로 _반-열림 \(half-open\)_ 이라 합니다. 닫힌 범위 연산자와 같이 `a` 의 값은 `b` 보다 클 수 없습니다. `a` 의 값이 `b` 와 같다면 결과 범위는 비어있을 것입니다.

반-열림 범위는 리스트의 길이까지 순차적으로 작업하는 것이 유용한 배열과 같은 0부터 시작하는 리스트와 함께 사용할 때 유용합니다:

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
    print("Person \(i + 1) is called \(names[i])")
}
// Person 1 is called Anna
// Person 2 is called Alex
// Person 3 is called Brian
// Person 4 is called Jack
```

배열은 4개의 아이템을 가지고 있지만 `0..<count` 는 반-열림 범위이기 때문에 오직 `3` \(배열의 마지막 아이템의 인덱스\) 까지 카운트 합니다. 배열에 대한 자세한 설명은 [배열 \(Arrays\)](collection-types.md#arrays) 을 참고 바랍니다.

### 단-방향 범위 \(One-Sided Ranges\)

닫힌 범위 연산자는 한방향으로 계속되는 범위에 대한 대체 형식입니다. 인덱스 2에서 배열 끝까지 배열의 모든 요소를 포함하는 범위가 그 예입니다. 이러한 경우 범위 연산자의 한쪽을 생략할 수 있습니다. 이러한 범위의 종류는 연산자가 오직 한쪽의 값만 가지고 있으므로 _단-방향 범위 \(one-sided range\)_ 라고 합니다. 예를 들어:

```swift
for name in names[2...] {
    print(name)
}
// Brian
// Jack

for name in names[...2] {
    print(name)
}
// Anna
// Alex
// Brian
```

반-열림 연산자 또한 마지막 값만 기입하여 단-방향 형식을 가질 수 있습니다. 보통 형식과 같이 마지막 값은 범위에 포함되지 않습니다. 예를 들어:

```swift
for name in names[..<2] {
    print(name)
}
// Anna
// Alex
```

단-방향 범위는 위와 같은 상황 말고도 다른 컨텍스트에서도 사용될 수 있습니다. 반복이 어디에서 시작해야 하는지 명확하지 않기 때문에 반복문에서 단-방향 범위의 첫번째 값을 생략할 수 없습니다. 반복무에서 단-방향 범위의 마지막 값은 생략할 수 있습니다. 그러나 범위가 무한정 계속되므로 루프에 대해 명시적으로 종료 조건을 추가해야 합니다. 아래 코드와 같이 단-방향 범위에 특정 값이 포함되어 있는지 체크할 수 있습니다.

```swift
let range = ...5
range.contains(7)   // false
range.contains(4)   // true
range.contains(-1)  // true
```

## 논리 연산자 \(Logical Operators\)

_논리 연산자 \(Logical operators\)_ 는 부울 로직 값을 `true` 와 `false` 로 수정하거나 결합합니다. Swift는 C-기반 언어에서 볼 수 있는 3개의 표준 논리 연산자를 제공합니다:

* 논리적 NOT \(`!a`\)
* 논리적 AND \(`a && b`\)
* 논리적 OR \(`a || b`\)

### 논리적 NOT 연산자 \(Logical NOT Operator\)

_논리적 NOT 연산자 \(logical NOT operator\)_ \(`!a`\)는 부울 값을 `true` 를 `false` 로 `false` 를 `true` 와 같이 반대로 만듭니다.

논리적 NOT 연산자는 접두사 연산자이며 동작할 값 바로 앞에 공백없이 위치합니다. 이것은 아래 예제와 같이 "`a` 가 아니다" 라고 읽을 수 있습니다:

```swift
let allowedEntry = false
if !allowedEntry {
    print("ACCESS DENIED")
}
// Prints "ACCESS DENIED"
```

`if !allowedEntry` 구문은 "엔트리가 허락되지 않는다면" 이라고 읽을 수 있습니다. 다음 라인은 "엔트리가 허락되지 않음" 이 참일 때, 이 말은 `allowedEntry` 가 `false` 일 경우에만 실행된다는 의미입니다.

예제에서 처럼 부울 상수와 변수 이름은 중복 부정과 논리적 상태의 혼동을 피하기 위해 읽기 쉽고 간결해야 합니다.

### 논리적 AND 연산자 \(Logical AND Operator\)

_논리적 AND 연산자 \(logical AND operator\)_ \(`a && b`\)는 두 값이 모두 `true` 여야 `true` 를 표현하는 논리적 표현식을 만듭니다.

두 값중 하나라도 `false` 이면 결과는 `false` 입니다. 사실 _첫번째_ 값이 `false` 이면 `true` 의 결과를 얻을 수 없기 때문에 두번째 값은 살펴보지 않습니다. 이러한 경우를 _연산 생략 \(short-circuit evaluation\)_ 이라 합니다.

이 예제는 2개의 `Bool` 값이 `true` 일 경우에만 접근을 허락합니다:

```swift
let enteredDoorCode = true
let passedRetinaScan = false
if enteredDoorCode && passedRetinaScan {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "ACCESS DENIED"
```

### 논리적 OR 연산자 \(Logical OR Operator\)

_논리적 OR 연산자 \(logical OR operator\)_ \(`a || b`\)는 2개의 인접한 파이프 문자로 만들어진 삽입 연산자입니다. 이것을 사용하여 2개의 값 중 하나라도 `true` 이면 표현식이 `true` 가 되는 논리적 표현식을 만듭니다.

위에서 논리적 AND 연산자와 마찬가지로 논리적 OR 연산자는 연산 생략 \(short-circuit evaluation\)을 사용하여 표현식을 고려합니다. 논리적 OR 연산자의 좌변이 `true` 이면 표현식의 결과는 바뀌지 않으므로 우변은 고려하지 않습니다.

아래 예제에서 첫번째 `Bool` 값 \(`hasDoorKey`\)은 `false` 이지만 두번째 값 \(`knowsOverridePassword`\)는 `true` 입니다. 하나의 값이 `true` 이기 때문에 표현식은 `true` 가 되고 접근이 허용됩니다:

```swift
let hasDoorKey = false
let knowsOverridePassword = true
if hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "Welcome!"
```

### 논리적 연산자 결합 \(Combining Logical Operators\)

긴 복합 표현식을 생성하기 위해 여러개의 논리적 연산자를 결합할 수 있습니다:

```swift
if enteredDoorCode && passedRetinaScan || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "Welcome!"
```

이 예제는 긴 복합 표현식을 생성하기 위해 여러개의 `&&` 와 `||` 연산자를 사용하였습니다. 그러나 `&&` 와 `||` 연산자는 오직 2개의 값에서만 동작하므로 이것은 3개의 작은 표현식이 함께 엮어있는 형태입니다. 이 예제는 아래와 같이 읽을 수 있습니다:

올바른 출입문 코드를 입력하고 망막 스캔을 통과했거나 유효한 키를 가지고 있거나 긴급 비밀번호를 알고 있을 경우에 접근이 허락됩니다.

`enteredDoorCode`, `passedRetinaScan`, 그리고 `hasDoorKey` 의 값을 기반으로 첫번째 2개의 표현식은 `false` 입니다. 그러나 긴급 비밀번호는 알고 있으므로 이 표현식의 결과는 `true` 입니다.

> NOTE   
> Swift 논리적 연산자 `&&` 와 `||` 은 왼쪽 우선결합 \(left-associative\) 입니다. 그 의미는 여러개의 논리적 연산자로 이루어진 복합 표현식은 가장 왼쪽부터 판단한다는 뜻입니다.

### 명시적 소괄호 \(Explicit Parentheses\)

소괄호를 포함하는 것이 영향이 없을 때 복합 표현식을 읽기 쉽게 하기위해 소괄호를 포함하는 것은 때때로 유용합니다. 위의 출입문 접근 예제에서 의도를 명확하게 하기위해 복합 표현식의 첫번째 부분을 소괄호를 추가하면 유용합니다:

```swift
if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "Welcome!"
```

소괄호는 처음 두개의 값을 전체 논리에서 별도의 부분으로 고려되도록 해줍니다. 복합 표현식의 결과는 바뀌지 않지만 전체적인 의도는 명확해 집니다. 가독성은 항상 간결성보다 선호됩니다. 의도를 명확하게 하는데 도움이 되는 곳에 소괄호를 사용해야 합니다.

