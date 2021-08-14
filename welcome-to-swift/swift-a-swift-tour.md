# Swift 둘러보기 \(A Swift Tour\)

전통적으로 새로운 언어의 첫번째 프로그램은 "Hello, world!" 를 출력해야 한다고 합니다. Swift 에서는 이것을 한줄로 표기할 수 있습니다:

```swift
print("Hello, world!")
// Prints "Hello, world!"
```

C 또는 Objective-C 로 코드를 작성했다면 이 구문은 익숙합니다. Swift 에서 이 코드의 라인은 완벽한 프로그램입니다. 입력/출력 또는 문자열 처리와 같은 기능을 위한 별도의 라이브러리를 가져올 필요는 없습니다. 전역 범위로 작성한 코드는 프로그램의 전체에서 사용되기 때문에 `main()` 함수가 필요하지 않습니다. 모든 구문에 끝에 세미콜론을 작성할 필요도 없습니다.

이 둘러보기를 통해 다양한 프로그래밍 작업을 수행하는 방법을 보여줌으로써 Swift 에서 코드 작성을 시작할 수 있는 충분한 정보를 제공합니다. 이해하지 못하는 부분이 있어도 걱정하지 마시기 바랍니다. 이 둘러보기에서 소개된 모든 내용은 이 책의 나머지 부분에 자세히 설명되어 있습니다.

> NOTE  
> Xcode 에서 플레이그라운드를 사용하는 것이 가장 좋습니다. 플레이그라운드는 코드를 수정하고 결과를 즉시 확인할 수 있습니다.
>
> [플레이그라운드 다운로드](https://docs.swift.org/swift-book/GuidedTour/GuidedTour.playground.zip)

## 간단한 값 \(Simple Values\)

상수를 만들기 위해 `let` 을 사용하고 변수를 만들기 위해 `var` 를 사용합니다. 상수는 컴파일 때 알 필요는 없지만 반드시 한번은 할당 해야 합니다. 이것은 값은 한번만 할당되지만 여러 위치에서 사용할 수 있다는 의미입니다.

```swift
var myVariable = 42
myVariable = 50
let myConstant = 42
```

상수 또는 변수는 할당하려는 값과 동일한 타입이어야 합니다. 하지만 항상 타입을 명시해야 하는 것은 아닙니다. 상수 또는 변수를 생성할 때 값을 제공하면 컴파일러는 타입을 유추합니다. 위의 예제에서 `myVariable` 의 초기값이 정수이므로 컴파일러는 정수로 유추합니다.

초기값이 충분한 정보를 제공하지 않거나 초기값이 없는 경우 변수 뒤에 콜론으로 구분하여 타입을 지정해야 합니다.

```swift
let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
```

> EXPERIMENT  
> 명시적 타입이 `Float` 이고 값이 `4` 인 상수를 만들어 봅시다.

값은 다른 타입의 값으로 절대 변경되지 않습니다. 값을 다른 타입으로 변경해야 한다면 원하는 타입의 인스턴스를 명시적으로 만들어야 합니다.

```swift
let label = "The width is "
let width = 94
let widthLabel = label + String(width)
```

> EXPERIMENT  
> 마지막 줄에서 `String` 으로 변환하는 부분을 삭제해 보십시오. 어떠한 에러가 발생합니까?

문자열 값으로 포함하는 더 간단한 방법이 있습니다: 소괄호 안에 값을 작성하고 소괄호 전에 백슬래시 \(`\`\)를 작성하면 됩니다. 예를 들어:

```swift
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```

> EXPERIMENT  
> `\()` 을 사용하여 문자열에 부동 소수점 계산을 포함하고 인사말에 누군가의 이름을 포함해 보십시오.

여러줄의 문자열에 대해 쌍따옴표 3개 \(`"""`\)를 사용하면 됩니다. 닫는 따옴표의 들여쓰기와 일치하는 한 각 인용된 줄의 시작부분에 있는 들여쓰기는 제거됩니다. 예를 들어:

```swift
let quotation = """
I said "I have \(apples) apples."
And then I said "I have \(apples + oranges) pieces of fruit."
"""
```

대괄호 \(`[]`\) 를 사용하여 배열과 딕셔너리를 생성하고 대괄호에 인덱스 또는 키를 작성하여 해당 요소에 접근할 수 있습니다. 마지막 요소 뒤에 쉼표도 허용 합니다.

```swift
var shoppingList = ["catfish", "water", "tulips"]
shoppingList[1] = "bottle of water"

var occupations = [
    "Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
```

배열은 요소를 추가함에 따라 자동으로 크기가 늘어납니다.

```swift
shoppingList.append("blue paint")
print(shoppingList)
```

빈 배열 또는 빈 딕셔너리를 생성하기 위해 초기화 구문을 사용합니다.

```swift
let emptyArray: [String] = []
let emptyDictionary: [String: Float] = [:]
```

타입 정보를 유추할 수 있으면 `[]` 로 빈 배열을 작성하고 `[:]` 로 빈 딕셔너리를 작성할 수 있습니다 — 예를 들어 변수에 새로운 값을 설정할 때나 함수에 인수로 전달할 때 사용할 수 있습니다.

```swift
shoppingList = []
occupations = [:]
```

## 제어 흐름 \(Control Flow\)

조건문을 만드려면 `if` 와 `switch` 를 사용하고 루프를 만드려면 `for`-`in`, `while`, 그리고 `repeat`-`while` 을 사용합니다. 조건문이나 루프 변수를 둘러싼 소괄호는 선택사항 입니다. 바디를 둘러싼 중괄호는 필수사항 입니다.

```swift
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
for score in individualScores {
    if score > 50 {
        teamScore += 3
    } else {
        teamScore += 1
    }
}
print(teamScore)
// Prints "11"
```

if 문에서는 조건부가 반드시 부울 \(Boolean\)로 표현되어야 합니다—즉, `if score { ... }` 와 같은 코드는 암시적으로 0과 비교하는 것이 아니라 에러를 의미합니다.

`if` 와 `let` 을 사용하여 누락될 수 있는 값에 대해 사용할 수 있습니다. 이러한 값은 옵셔널로 표기됩니다. 옵셔널 값은 값을 포함하거나 값이 없음을 나타내는 `nil` 을 포함합니다. 옵셔널로 값을 표시하기 위해 값의 타입 뒤에 물음표 \(`?`\) 를 작성합니다.

```swift
var optionalString: String? = "Hello"
print(optionalString == nil)
// Prints "false"

var optionalName: String? = "John Appleseed"
var greeting = "Hello!"
if let name = optionalName {
    greeting = "Hello, \(name)"
}
```

> EXPERIMENT  
> `optionalName` 을 `nil` 로 변경하십시오. 어떤 인사말을 얻습니까? `optionalName` 이 `nil` 인 경우 다른 인사말을 설정하기 위해 `else` 절을 추가하십시오.

옵셔널 값이 `nil` 이면 조건은 `false` 이고 중괄호 안에 코드는 건너뜁니다. 옵셔널 값이 `nil` 이 아니면 옵셔널 값은 언래핑 되고 `let` 뒤의 상수로 할당되어 코드 블럭 안에서 언래핑 된 값으로 사용할 수 있습니다.

옵셔널 값을 처리하는 다른 방법은 `??` 연산자를 사용하여 기본값을 제공하는 것입니다. 옵셔널 값이 없다면 기본값이 대신 사용됩니다.

```swift
let nickname: String? = nil
let fullName: String = "John Appleseed"
let informalGreeting = "Hi \(nickname ?? fullName)"
```

스위치는 모든 종류의 데이터와 다양한 비교 작업을 지원합니다—스위치는 정수 및 동등성 비교로 제한되지 않습니다.

```swift
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":
    print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"):
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
// Prints "Is it a spicy red pepper?"
```

> EXPERIMENT  
> default 케이스를 삭제해 봅시다. 어떤 에러가 발생합니까?

패턴과 일치하는 값을 상수에 할당하기 위해 패턴에서 `let` 을 어떻게 사용하는지 확인 하시기 바랍니다.

일치하는 스위치 케이스 문을 실행하고 프로그램은 스위치 문을 종료합니다. 다음 케이스로 이어서 실행되지 않기 때문에 각 케이스 코드에 명시적으로 스위치 종료를 할 필요가 없습니다.

`for`-`in` 을 사용하여 각 키-값 쌍에 사용할 이름의 쌍을 제공하여 딕셔너리의 항목을 반복합니다. 딕셔너리는 순서가 없는 콜렉션 이므로 키와 값은 임의의 순서로 반복됩니다.

```swift
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
]
var largest = 0
for (_, numbers) in interestingNumbers {
    for number in numbers {
        if number > largest {
            largest = number
        }
    }
}
print(largest)
// Prints "25"
```

> EXPERIMENT  
> `_` 을 변수의 이름으로 변경하고 어떤 숫자가 가장 큰지 추적하십시오.

조건이 바뀔 때까지 코드의 블럭을 반복하려면 `while` 을 사용해야 합니다. 대신 루프의 조건이 끝에 있을 수 있으므로 적어도 한번은 루프가 실행되도록 합니다.

```swift
var n = 2
while n < 100 {
    n *= 2
}
print(n)
// Prints "128"

var m = 2
repeat {
    m *= 2
} while m < 100
print(m)
// Prints "128"
```

인덱스의 범위를 만들기 위해선 `..<` 을 사용하여 루프에 인덱스를 유지할 수 있습니다.

```swift
var total = 0
for i in 0..<4 {
    total += i
}
print(total)
// Prints "6"
```

가장 상위 값을 생략하는 범위를 만들기 위해 `..<` 을 사용하고 포함하려면 `...` 을 사용합니다.

## 함수와 클로저 \(Functions and Closures\)

함수를 선언하려면 `func` 을 사용합니다. 소괄호 안에 인수의 목록과 함수의 이름으로 호출합니다. 함수의 반환 타입에서 파라미터 이름과 타입을 구분하기 위해 `->` 을 사용합니다.

```swift
func greet(person: String, day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet(person: "Bob", day: "Tuesday")
```

> EXPERIMENT  
> `day` 파라미터를 지워 봅시다. 인사말에 오늘의 특별 점심을 포함하기 위해 파라미터를 추가해 봅시다.

기본적으로 함수는 파라미터 이름을 인수의 라벨로 사용합니다. 파라미터 이름 전에 인수 라벨을 작성하거나 인수 라벨을 사용하지 않으려면 `_` 을 작성해야 합니다.

```swift
func greet(_ person: String, on day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet("John", on: "Wednesday")
```

튜플을 사용하여 복합 값을 만듭니다—예를 들어 함수로 부터 여러개의 값을 반환할 때 사용합니다. 튜플의 요소는 이름 또는 번호로 참조할 수 있습니다.

```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0

    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        sum += score
    }

    return (min, max, sum)
}
let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])
print(statistics.sum)
// Prints "120"
print(statistics.2)
// Prints "120"
```

함수는 중첩될 수 있습니다. 중첩된 함수는 외부 함수에서 선언한 변수에 접근할 수 있습니다. 중첩된 함수를 사용하여 길거나 복잡한 함수에서 코드를 구성할 수 있습니다.

```swift
func returnFifteen() -> Int {
    var y = 10
    func add() {
        y += 5
    }
    add()
    return y
}
returnFifteen()
```

함수는 1급 타입 \(first-class type\) 입니다. 이것은 함수가 다른 함수를 값으로 반환할 수 있다는 의미입니다.

```swift
func makeIncrementer() -> ((Int) -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
```

함수는 다른 함수를 인수 중 하나로 가질 수 있습니다.

```swift
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)
```

함수는 나중에 호출될 수 있는 코드 블럭인 클로저의 특별한 케이스 입니다. 클로저에 있는 코드는 이미 중첩된 함수의 예제에서 보았듯이 클러저가 실행될 때 다른 범위에 있더라도 클로저가 생성된 범위에서 사용 가능한 변수와 함수와 같은 항목에 접근할 수 있습니다. 중괄호 \(`{}`\) 로 코드를 묶어 이름없이 클로저를 작성할 수 있습니다. 바디로 부터 인수와 반환 타입을 분리하기 위해 `in` 을 사용합니다.

```swift
numbers.map({ (number: Int) -> Int in
    let result = 3 * number
    return result
})
```

> EXPERIMENT  
> 모든 홀수에 대해 0을 반환도록 클로저를 다시 작성하십시오.

더 간단하게 클로저를 작성하기 위해 몇가지 선택사항이 있습니다. 대리자 \(delegate\) 에 대한 콜백과 같이 클로저의 타입을 이미 알고 있다면 파라미터의 타입, 반환 타입 또는 둘다 생략할 수 있습니다. 단일 클로저 구문은 암시적으로 구문의 값만 반환합니다.

```swift
let mappedNumbers = numbers.map({ number in 3 * number })
print(mappedNumbers)
// Prints "[60, 57, 21, 36]"
```

매우 짧은 클로저에 유용한 접근 방법으로 이름 대신 숫자로 파라미터를 참조할 수 있습니다. 함수의 마지막 인수로 전달된 클로저는 소괄호 뒤에 바로 나타날 수 있습니다. 클로저가 함수의 유일한 인수일 때 소괄호는 생략할 수 있습니다.

```swift
let sortedNumbers = numbers.sorted { $0 > $1 }
print(sortedNumbers)
// Prints "[20, 19, 12, 7]"
```

## 객체와 클래스 \(Objects and Classes\)

`class` 뒤에 클래스의 이름을 사용하여 클래스를 생성합니다. 클래스에서 프로퍼티 선언은 클래스의 컨텍스트 내에 있다는 점을 제외하고는 상수 또는 변수를 선언하는 방법과 동일합니다. 마찬가지로 메서드와 함수 선언도 동일한 방법으로 작성됩니다.

```swift
class Shape {
    var numberOfSides = 0
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

> EXPERIMENT  
> `let` 을 사용하여 상수 프로퍼티를 추가하고 인수를 가지는 다른 메서드를 추가해 보시기 바랍니다.

클래스 이름 뒤에 소괄호를 넣어 클래스의 인스턴스를 생성합니다. 인스턴스의 프로퍼티와 메서드에 접근하기 위헤 점 구문을 사용합니다.

```swift
var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()
```

이 버전의 `Shape` 클래스는  인스턴스가 생성될 때 클래스를 설정하는 초기화 구문이 누락되었습니다. `init` 을 사용하여 생성합니다.

```swift
class NamedShape {
    var numberOfSides: Int = 0
    var name: String

    init(name: String) {
        self.name = name
    }

    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

초기화 구문에서 `name` 인수와 `name` 프로퍼티를 구분하기 위해 `self` 가 어떻게 사용되는지 확인 하십시오. 초기화 구문의 인수는 클래스의 인스턴스를 생성할 때 함수 호출처럼 전달됩니다. 모든 프로퍼티는 `numberOfSides` 와 같이 선언 시나 `name` 과 같이 초기화 구문에서 값을 할당해야 합니다.

객체가 할당 해제되기 전에 어떠한 정리작업이 필요하여 초기화 해제 구문을 생성하려면 `deinit` 을 사용합니다.

하위 클래스는 클래스 이름 뒤에 콜론으로 구분하여 상위 클래스를 포함합니다. 클래스가 모든 표준 루트 클래스를 하위 클래스화 할 필요 없으므로 필요에 따라 상위 클래스를 포함하거나 생략할 수 있습니다.

상위 클래스의 구현을 재정의 하는 하위 클래스의 메서드는 `override` 로 표시됩니다—실수로 `override` 없이 메서드를 재정의 하면 에러로 컴파일러에 의해 감지됩니다. 컴파일러는 실제로 상위 클래스의 어떤 메서드도 실제로 재정의 하지 않는 `override` 가 있는 메서드를 감지합니다.

```swift
class Square: NamedShape {
    var sideLength: Double

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }

    func area() -> Double {
        return sideLength * sideLength
    }

    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)."
    }
}
let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()
```

> EXPERIMENT  
> 초기화 구문에 반지름과 이름을 인수로 가지는 `Circle` 이라는 `NamedShape` 의 하위 클래스를 만들어 보세요. `Circle` 클래스에 `area()` 와 `simpleDescription()` 메서드를 구현해 보세요.

저장된 단순 프로퍼티 외에도 프로퍼티는 getter 와 setter 를 가질 수 있습니다.

```swift
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }

    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
    }

    override func simpleDescription() -> String {
        return "An equilateral triangle with sides of length \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
print(triangle.perimeter)
// Prints "9.3"
triangle.perimeter = 9.9
print(triangle.sideLength)
// Prints "3.3000000000000003"
```

`perimeter` 의 setter 에서 새로운 값은 암시적으로 `newValue` 라는 이름을 가집니다. `set` 이후에 소괄호 안에 명시적으로 이름을 제공할 수 있습니다.

`EquilateralTriangle` 클래스의 초기화 구문은 세가지 단계가 있습니다:

1. 하위 클래스가 선언한 프로퍼티의 값을 설정합니다.
2. 상위 클래스의 초기화 구문을 호출합니다.
3. 상위 클래스에 의해 정의된 프로퍼티의 값을 변경합니다. 메서드, getter, 또는 setter 를 사용하는 추가 설정 작업도 이 시점에서 수행할 수 있습니다.

프로퍼티를 계산할 필요는 없지만 새로운 값을 설정하기 전과 후에 실행되는 코드를 제공하려면 `willSet` 과 `didSet` 을 사용하면 됩니다. 이렇게 제공한 코드는 초기화 구문의 외부에서 값이 변경될 때마다 실행됩니다. 예를 들어 아래의 클래스는 삼각형의 측면의 길이가 항상 사각형의 측면의 길이와 같다는 것을 확인합니다.

```swift
class TriangleAndSquare {
    var triangle: EquilateralTriangle {
        willSet {
            square.sideLength = newValue.sideLength
        }
    }
    var square: Square {
        willSet {
            triangle.sideLength = newValue.sideLength
        }
    }
    init(size: Double, name: String) {
        square = Square(sideLength: size, name: name)
        triangle = EquilateralTriangle(sideLength: size, name: name)
    }
}
var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
print(triangleAndSquare.square.sideLength)
// Prints "10.0"
print(triangleAndSquare.triangle.sideLength)
// Prints "10.0"
triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
print(triangleAndSquare.triangle.sideLength)
// Prints "50.0"
```

옵셔널 값으로 동작할 때 메서드, 프로퍼티, 그리고 서브 스크립트와 같은 동작 전에 `?` 을 작성할 수 있습니다. `?` 전의 값이 `nil` 이면 `?` 이후의 모든 것은 무시되고 전체 표현의 값은 `nil` 입니다. 그렇지 않으면 옵셔널 값은 언래핑 되고 `?` 후에 모든 동작은 언래핑 된 값으로 동작합니다. 이 경우 모드 전체 표현식의 값은 옵셔널 값입니다.

```swift
let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
let sideLength = optionalSquare?.sideLength
```

## 열거형과 구조체 \(Enumerations and Structures\)

열거형을 생성하기 위해 `enum` 을 사용합니다. 클래스와 다른 명명된 타입과 같이 열거형은 메서드를 가질 수 있습니다.

```swift
enum Rank: Int {
    case ace = 1
    case two, three, four, five, six, seven, eight, nine, ten
    case jack, queen, king

    func simpleDescription() -> String {
        switch self {
        case .ace:
            return "ace"
        case .jack:
            return "jack"
        case .queen:
            return "queen"
        case .king:
            return "king"
        default:
            return String(self.rawValue)
        }
    }
}
let ace = Rank.ace
let aceRawValue = ace.rawValue
```

> EXPERIMENT  
> 원시값으로 2개의 `Rank` 값을 비교하는 함수를 작성해 보세요.

기본적으로 Swift 는 0을 시작으로 매번 증가하는 원시값을 할당하지만 명시적으로 특정 값으로 이 동작을 변경할 수 있습니다. 위의 예제에서 `Ace` 는 명시적으로 `1` 의 값이 주어지고 나머지 원시값은 순서대로 할당됩니다. 열거형의 원시 타입으로 문자열 또는 부동 소수점도 사용할 수 있습니다. 열거형 케이스의 원시값에 접근하기 위해 `rawValue` 프로퍼티를 사용합니다.

원시값으로 부터 열거형의 인스턴스를 생성하기 위해 `init?(rawValue:)` 초기화 구문을 사용합니다. 원시값이 일치하는 열거형 케이스나 `Rank` 에 일치하는 항목이 없으면 `nil` 을 반환합니다.

```swift
if let convertedRank = Rank(rawValue: 3) {
    let threeDescription = convertedRank.simpleDescription()
}
```

열거형의 케이스 값은 원시값을 작성하는 다른 방법이 아니라 실제 값입니다. 실제로 의미 없는 원시값의 케이스의 경우에는 제공할 필요가 없습니다.

```swift
enum Suit {
    case spades, hearts, diamonds, clubs

    func simpleDescription() -> String {
        switch self {
        case .spades:
            return "spades"
        case .hearts:
            return "hearts"
        case .diamonds:
            return "diamonds"
        case .clubs:
            return "clubs"
        }
    }
}
let hearts = Suit.hearts
let heartsDescription = hearts.simpleDescription()
```

> EXPERIMENT  
> 스페이드와 클럽의 경우 "black" 을 반환하고 하드와 다이아몬드의 경우 "red" 를 반환하는 `color()` 메서드를 `Suit` 에 추가하십시오.

위에서 열거형의 `hearts` 케이스를 참조하는 두가지 방법에 유의하십시오: `hearts` 상수에 값을 할당할 때 명시적으로 타입을 지정하지 않았으므로 열거형 케이스 `Suit.hearts` 전체 이름으로 참조됩니다. 스위치에서 `self` 의 값은 이미 카드 \(suit\) 로 알고 있기 때문에 열거형 케이스는 짧은 형식인 `.hearts` 로 참조됩니다. 값의 타입을 이미 알고 있다면 언제나 짧은 형식을 사용할 수 있습니다.

열거형이 원시값을 갖는 경우 선언의 부분으로 결정됩니다. 이것은 특정 열거형 케이스의 모든 인스턴스는 항상 같은 원시값을 갖는다는 의미입니다. 열거형 케이스의 또다른 선택은 케이스와 관련된 값을 가지는 것입니다—이러한 값은 인스턴스를 생성할 때 결정되고 열거형 케이스의 각 인스턴스에 대해 다를 수 있습니다. 연관된 값은 열거형 케이스 인스턴스의 저장된 프로퍼티처럼 동작한다고 생각할 수 있습니다. 예를 들어 서버에서 일출과 일몰 시간에 대해 요청한다고 가정해 봅시다. 서버는 요청된 정보에 대한 응답을 하거나 무엇이 잘못되었는지에 대한 설명을 응답합니다.

```swift
enum ServerResponse {
    case result(String, String)
    case failure(String)
}

let success = ServerResponse.result("6:00 am", "8:09 pm")
let failure = ServerResponse.failure("Out of cheese.")

switch success {
case let .result(sunrise, sunset):
    print("Sunrise is at \(sunrise) and sunset is at \(sunset).")
case let .failure(message):
    print("Failure...  \(message)")
}
// Prints "Sunrise is at 6:00 am and sunset is at 8:09 pm."
```

> EXPERIMENT  
> `ServerResponse` 에 세번째 케이스를 추가하십시오.

스위치 케이스에 대한 값이 일치하는 부분으로 `ServerResponse` 값에서 일출과 일몰 시간이 어떻게 추출되는지 확인하시기 바랍니다.

구조체를 생성하기 위해 `struct` 를 사용합니다.구조체는 메서드와 초기화 구문을 포함하여 클래스와 동일한 동작을 많이 지원합니다. 구조체와 클래스의 가장 큰 차이점은 구조체는 코드에서 전달될 때 항상 복사되지만 클래스는 참조로 전달됩니다.

```swift
struct Card {
    var rank: Rank
    var suit: Suit
    func simpleDescription() -> String {
        return "The \(rank.simpleDescription()) of \(suit.simpleDescription())"
    }
}
let threeOfSpades = Card(rank: .three, suit: .spades)
let threeOfSpadesDescription = threeOfSpades.simpleDescription()
```

> EXPERIMENT  
> 랭크와 카드의 각 조합에 대한 카드 한장으로 전체 카드 덱을 포함하는 배열을 반환하는 함수를 작성하십시오.

## 프로토콜과 확장 \(Protocols and Extensions\)

프로토콜을 선언하려면 `protocol` 을 사용해야 합니다.

```swift
protocol ExampleProtocol {
    var simpleDescription: String { get }
    mutating func adjust()
}
```

클래스, 열거형, 그리고 구조체는 프로토콜을 채택할 수 있습니다.

```swift
class SimpleClass: ExampleProtocol {
    var simpleDescription: String = "A very simple class."
    var anotherProperty: Int = 69105
    func adjust() {
        simpleDescription += "  Now 100% adjusted."
    }
}
var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription

struct SimpleStructure: ExampleProtocol {
    var simpleDescription: String = "A simple structure"
    mutating func adjust() {
        simpleDescription += " (adjusted)"
    }
}
var b = SimpleStructure()
b.adjust()
let bDescription = b.simpleDescription
```

> EXPERIMENT  
> `ExampleProtocol` 에 다른 요구사항을 추가하십시오. 프로토콜을 여전히 준수하기 위해 `SimpleClass` 와 `SimpleStructure` 에 무엇을 바꿔야 할까요?

구조체를 수정하는 메서드를 표시하기위해 `SimpleStructure` 의 선언에서 `mutating` 키워드 사용을 주목하시기 바랍니다. 클래스의 메서드는 항상 클래스를 수정할 수 있으므로 `SimpleClass` 의 선언에는 `mutating` 으로 표시된 메서드가 필요하지 않습니다.

새로운 메서드와 계산된 프로퍼티와 같이 존재하는 타입에 기능을 추가하려면 `extension` 을 사용합니다. 확장 \(extension\) 을 사용하여 다른 곳에 선언된 타입 또는 라이브러리나 프레임워크에서 가져온 타입에 프로토콜 준수를 추가할 수 있습니다.

```swift
extension Int: ExampleProtocol {
    var simpleDescription: String {
        return "The number \(self)"
    }
    mutating func adjust() {
        self += 42
    }
}
print(7.simpleDescription)
// Prints "The number 7"
```

> EXPERIMENT  
> `Double` 타입에 대해 `absoluteValue` 프로퍼티를 추가하기 위해 확장을 작성하십시오.

다른 명명된 타입처럼 프로토콜 이름을 사용할 수 있습니다—예를 들어 타입이 다르지만 모두 단일 프로토콜을 준수하는 객체의 콜렉션을 생성할 수 있습니다. 타입이 프로토콜 타입인 값으로 동작하는 경우 프로토콜 정의 외부의 메서드를 사용할 수 없습니다.

```swift
let protocolValue: ExampleProtocol = a
print(protocolValue.simpleDescription)
// Prints "A very simple class.  Now 100% adjusted."
// print(protocolValue.anotherProperty)  // Uncomment to see the error
```

`protocolValue` 변수에 `SimpleClass` 의 런타임 타입을 가지고 있더라도 컴파일러는 주어진 `ExampleProtocol` 의 타입으로 처리합니다. 즉, 프로토콜 적합성 외에 클래스가 구현하는 메서드나 프로퍼티에 실수로 접근할 수 없습니다.

## 에러 처리 \(Error Handling\)

`Error` 프로토콜을 채택하는 모든 타입을 사용하여 에러를 나타냅니다.

```swift
enum PrinterError: Error {
    case outOfPaper
    case noToner
    case onFire
}
```

에러를 던지기 위해 `throw` 를 사용하고 에러를 던질 수 있는 함수를 나타내기 위해 `throws` 를 사용합니다. 함수에서 에러가 발생하면 함수는 즉시 반환되고 함수를 호출한 코드가 에러를 처리합니다.

```swift
func send(job: Int, toPrinter printerName: String) throws -> String {
    if printerName == "Never Has Toner" {
        throw PrinterError.noToner
    }
    return "Job sent"
}
```

에러를 처리하는 방법은 몇가지 존재합니다. 한가지 방법은 `do`-`catch` 를 사용하는 것입니다. `do` 블럭 내에서 앞에 `try` 를 작성하여 에러가 발생할 수 있는 코드임을 표시합니다. `catch` 블럭 내에서 에러는 다른 이름으로 지정하기 전까지는 자동으로 `error` 라는 이름으로 주어집니다.

```swift
do {
    let printerResponse = try send(job: 1040, toPrinter: "Bi Sheng")
    print(printerResponse)
} catch {
    print(error)
}
// Prints "Job sent"
```

> EXPERIMENT  
> `send(job:toPrinter:)` 함수에서 에러가 발생하도록 프린터 이름을 `"Never Has Toner"` 로 변경합니다.

특정 에러를 처리하는 여러개의 `catch` 블럭을 제공할 수 있습니다. 스위치에서 `case` 이후에 하는 것처럼 `catch` 이후에 패턴을 작성합니다.

```swift
do {
    let printerResponse = try send(job: 1440, toPrinter: "Gutenberg")
    print(printerResponse)
} catch PrinterError.onFire {
    print("I'll just put this over here, with the rest of the fire.")
} catch let printerError as PrinterError {
    print("Printer error: \(printerError).")
} catch {
    print(error)
}
// Prints "Job sent"
```

> EXPERIMENT  
> `do` 블럭 내에 에러를 발생하는 코드를 추가하십시오. 첫번째 `catch` 블럭에서 에러를 처리하려면 어떤 종류의 에러를 던져야 할까요? 두번째와 세번째 블럭에 대해선 무슨 에러를 던져야 할까요?

에러를 처리하는 다른 방법은 결과를 옵셔널로 바꾸는 `try?` 를 사용하는 것입니다. 함수에서 에러가 발생하면 특정 에러는 버려지고 결과는 `nil` 입니다. 그렇지 않으면 결과는 함수가 반환한 옵셔널 값을 포함합니다.

```swift
let printerSuccess = try? send(job: 1884, toPrinter: "Mergenthaler")
let printerFailure = try? send(job: 1885, toPrinter: "Never Has Toner")
```

함수를 반환하기 직전에 함수의 다른 모든 코드 다음에 실행되는 코드의 블럭을 작성하기 위해 `defer` 를 사용합니다. 이 코드는 함수에서 에러가 발생하는지 여부와 관계없이 실행됩니다. 서로 다른 시간에 실행되어야 하는 경우에도 설정 및 정리 코드를 나란히 작성하기 위해 `defer` 를 사용할 수 있습니다.

```swift
var fridgeIsOpen = false
let fridgeContent = ["milk", "eggs", "leftovers"]

func fridgeContains(_ food: String) -> Bool {
    fridgeIsOpen = true
    defer {
        fridgeIsOpen = false
    }

    let result = fridgeContent.contains(food)
    return result
}
fridgeContains("banana")
print(fridgeIsOpen)
// Prints "false"
```

## 제너릭 \(Generics\)

제너릭 \(generic\) 함수 또는 타입을 만들기 위해 꺾쇠 괄호 안에 이름을 작성합니다.

```swift
func makeArray<Item>(repeating item: Item, numberOfTimes: Int) -> [Item] {
    var result = [Item]()
    for _ in 0..<numberOfTimes {
        result.append(item)
    }
    return result
}
makeArray(repeating: "knock", numberOfTimes: 4)
```

제너릭 형태의 함수와 메서드 뿐만 아니라 클래스, 열거형, 그리고 구조체도 만들 수 있습니다.

```swift
// Reimplement the Swift standard library's optional type
enum OptionalValue<Wrapped> {
    case none
    case some(Wrapped)
}
var possibleInteger: OptionalValue<Int> = .none
possibleInteger = .some(100)
```

요구사항의 목록을 지정하기 위해 바디 바로 전에 `where` 을 사용합니다—예를 들어 타입이 프로토콜을 구현하도록 요구하거나 두 타입의 동일하도록 요구하거나 클래스에 특정 상위 클래스가 있어야 합니다.

```swift
func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> Bool
    where T.Element: Equatable, T.Element == U.Element
{
    for lhsItem in lhs {
        for rhsItem in rhs {
            if lhsItem == rhsItem {
                return true
            }
        }
    }
    return false
}
anyCommonElements([1, 2, 3], [3])
```

> EXPERIMENT  
> 두 시퀀스가 공통으로 가지는 요소의 배열을 반환하는 함수를 만들기 위해 `anyCommonElements(_:_:)` 함수를 수정하십시오.

`<T: Equatable>` 을 작성하는 것은 `<T> ... where T: Equatable` 을 작성하는 것과 같습니다.

