# 메서드 (Methods)

인스턴스나 타입에 속한 함수를 정의하고 호출할 수 있습니다.

*메서드(Methods)*는 특정 타입에 연결된 함수입니다.
클래스, 구조체, 열거형은 해당 타입의 인스턴스 동작을 위한 특정 작업과 기능을 캡슐화하는
인스턴스 메서드를 정의할 수 있습니다.
클래스, 구조체, 열거형은 타입 자체와 연결된
타입 메서드를 정의할 수도 있습니다.
타입 메서드는 Objective-C에서 클래스 메서드와 유사합니다.

Swift에서 구조체와 열거형도 메서드를 정의할 수 있다는 점은
C와 Objective-C와의 가장 큰 차이점입니다.
Objective-C에서는 오직 클래스만 메서드를 정의할 수 있습니다.
Swift에서는 클래스, 구조체, 열거형 중 어떤 것을 사용하든
메서드를 정의할 수 있는 유연함을 제공합니다.

## 인스턴스 메서드 (Instance Methods)

*인스턴스 메서드(Instance methods)*는 특정 클래스, 구조체, 열거형의
인스턴스에 속하는 함수입니다.
이 메서드는 인스턴스의 기능을 지원하며,
인스턴스 프로퍼티에 접근하고 수정하는 방법을 제공하거나,
인스턴스의 목적에 관련된 기능을 구현하는데 사용됩니다.
인스턴스 메서드는 [함수 (Functions)](./functions.md)에서 설명한대로
함수 구문과 완벽하게 동일합니다.

인스턴스 메서드는 해당 타입의 중괄호 내에 작성합니다.
인스턴스 메서드는 해당 타입에 속한 다른 인스턴스 메서드와 프로퍼티에 암시적으로 접근할 수 있습니다.
인스턴스 메서드는 반드시 해당 타입의 특정 인스턴스에 대해 호출할 수 있습니다.
인스턴스 없이 단독으로 호출할 수는 없습니다.

다음은 작업이 발생한 횟수를 계산하는데 사용할 수 있는
간단한 `Counter` 클래스 정의의 예시입니다:

```swift
class Counter {
    var count = 0
    func increment() {
        count += 1
    }
    func increment(by amount: Int) {
        count += amount
    }
    func reset() {
        count = 0
    }
}
```

<!--
  - test: `instanceMethods`

  ```swifttest
  -> class Counter {
        var count = 0
        func increment() {
           count += 1
        }
        func increment(by amount: Int) {
           count += amount
        }
        func reset() {
           count = 0
        }
     }
  ```
-->

`Counter` 클래스는 다음과 같은 세 가지 인스턴스 메서드를 정의합니다:

- `increment()`는 `1`씩 카운터를 증가시킵니다.
- `increment(by: Int)`는 지정된 정수 크기만큼 카운터를 증가시킵니다.
- `reset()`은 카운터를 0으로 재설정합니다.

`Counter` 클래스는 현재 카운터 값을 추적하는
`count` 프로퍼티 변수도 선언합니다.

프로퍼티와 동일하게 점 표기법으로 인스턴스 메서드를 호출합니다:

```swift
let counter = Counter()
// the initial counter value is 0
counter.increment()
// the counter's value is now 1
counter.increment(by: 5)
// the counter's value is now 6
counter.reset()
// the counter's value is now 0
```

<!--
  - test: `instanceMethods`

  ```swifttest
  -> let counter = Counter()
  /> the initial counter value is \(counter.count)
  </ the initial counter value is 0
  -> counter.increment()
  /> the counter's value is now \(counter.count)
  </ the counter's value is now 1
  -> counter.increment(by: 5)
  /> the counter's value is now \(counter.count)
  </ the counter's value is now 6
  -> counter.reset()
  /> the counter's value is now \(counter.count)
  </ the counter's value is now 0
  ```
-->

함수 매개변수는 [함수 인자 레이블과 매개변수 이름 (Function Argument Labels and Parameter Names)](./functions.md#함수-인자-레이블과-매개변수-이름-function-argument-labels-and-parameter-names)에서 설명했듯이
함수 본문 내에서 사용하는 이름과
함수 호출할 때 사용하는 인자 레이블을 모두 가질 수 있습니다.
메서드도 타입에 속한 함수이므로,
메서드 매개변수도 동일한 규칙을 따릅니다.

### self 프로퍼티 (The self Property)

모든 타입의 인스턴스는 `self` 라는 암시적 프로퍼티를 가지고 있으며,
이는 해당 인스턴스 자체를 나타냅니다.
인스턴스 메서드 내에서
현재 인스턴스를 참조하기 위해 `self` 프로퍼티를 사용합니다.

위 예시에서 `increment()` 메서드는 아래와 같이 작성될 수 있습니다:

```swift
func increment() {
    self.count += 1
}
```

<!--
  - test: `instanceMethodsIncrement`

  ```swifttest
  >> class Counter {
  >> var count: Int = 0
     func increment() {
        self.count += 1
     }
  >> }
  ```
-->

<!--
  NOTE: I'm slightly cheating with my testing of this excerpt, but it works!
-->

실제로 코드에서 `self`를 자주 작성할 필요는 없습니다.
`self`를 명시적으로 작성하지 않으면,
Swift는 메서드 내부에서 알려진 프로퍼티나 메서드 이름을 사용할 때,
현재 인스턴스의 프로퍼티나 메서드를 참조하는 것으로 간주합니다.
이러한 기본 동작은 `Counter`의 세 인스턴스 메서드에서
`self.count`가 아닌 `count`를 사용하는 방식에서 확인할 수 있습니다.

이 규칙의 주요 예외사항은 인스턴스 메서드의 매개변수 이름이
그 인스턴스의 프로퍼티 이름과 동일한 경우입니다.
이러한 경우 매개변수 이름이 더 우선되므로,
프로퍼티를 참조하려면 더 명확한 방식으로 접근해야 합니다.
매개변수 이름과 프로퍼티 이름이 충돌할 경우
`self` 프로퍼티를 사용하여 구분합니다.

다음 예시에서 `x`라는 메서드 매개변수와
같은 이름을 가진 인스턴스 프로퍼티를 구분하기 위해 `self`를 사용합니다:

```swift
struct Point {
    var x = 0.0, y = 0.0
    func isToTheRightOf(x: Double) -> Bool {
        return self.x > x
    }
}
let somePoint = Point(x: 4.0, y: 5.0)
if somePoint.isToTheRightOf(x: 1.0) {
    print("This point is to the right of the line where x == 1.0")
}
// Prints "This point is to the right of the line where x == 1.0"
```

<!--
  - test: `self`

  ```swifttest
  -> struct Point {
        var x = 0.0, y = 0.0
        func isToTheRightOf(x: Double) -> Bool {
           return self.x > x
        }
     }
  -> let somePoint = Point(x: 4.0, y: 5.0)
  -> if somePoint.isToTheRightOf(x: 1.0) {
        print("This point is to the right of the line where x == 1.0")
     }
  <- This point is to the right of the line where x == 1.0
  ```
-->

`self` 접두사가 없으면,
Swift는 두 번 모두 `x`가 메서드의 매개변수 `x`를 가르키는 것으로 간주합니다.

### 인스턴스 메서드 내부에서 값 타입 수정 (Modifying Value Types from Within Instance Methods)

구조체와 열거형은 *값 타입*입니다.
기본적으로 값 타입의 프로퍼티는 인스턴스 메서드 내부에서 수정할 수 없습니다.

<!--
  TODO: find out why.  once I actually know why, explain it.
-->

그러나 특정 메서드 내에서
구조체나 열거형의 프로퍼티 수정이 필요하다면
해당 메서드에 대한 *변경* 동작을 명시적으로 허용할 수 있습니다.
그러면 해당 메서드 내부에서 프로퍼티를
변경할 수 있고
메서드가 끝나면 기존 구조체에 변경 사항이 반영됩니다.
이 메서드 내에서 `self`에 완전히 새로운 인스턴스를 할당할 수도 있으며,
메서드가 종료되면 기존 인스턴스가 이 새 인스턴스로 대체하게 됩니다.

이 동작을 허용하려면 해당 메서드 `func` 키워드 앞에
`mutating` 키워드를 작성하면 됩니다:

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0)
print("The point is now at (\(somePoint.x), \(somePoint.y))")
// Prints "The point is now at (3.0, 4.0)"
```

<!--
  - test: `selfStructures`

  ```swifttest
  -> struct Point {
        var x = 0.0, y = 0.0
        mutating func moveBy(x deltaX: Double, y deltaY: Double) {
           x += deltaX
           y += deltaY
        }
     }
  -> var somePoint = Point(x: 1.0, y: 1.0)
  -> somePoint.moveBy(x: 2.0, y: 3.0)
  -> print("The point is now at (\(somePoint.x), \(somePoint.y))")
  <- The point is now at (3.0, 4.0)
  ```
-->

위의 `Point` 구조체는 mutating 메서드인 `moveBy(x:y:)`를 정의하며,
이 메서드는 `Point` 인스턴스를 일정한 값만큼 이동시킵니다.
새로운 위치를 반환하는 대신에
이 메서드는 실제로 호출된 인스턴스를 직접 수정합니다.
이를 위해 메서드 정의에 `mutating` 키워드를 추가하여
프로퍼티를 수정을 허용합니다.

[상수 구조체 인스턴스의 저장 프로퍼티 (Stored Properties of Constant Structure Instances)](./properties.md#상수-구조체-인스턴스의-저장-프로퍼티-stored-properties-of-constant-structure-instances)에서 설명했듯이
프로퍼티가 변수 프로퍼티라도 변경할 수 없기 때문에
구조체 타입의 상수에 대해 mutating 메서드를 호출할 수 없습니다:

```swift
let fixedPoint = Point(x: 3.0, y: 3.0)
fixedPoint.moveBy(x: 2.0, y: 3.0)
// this will report an error
```

<!--
  - test: `selfStructures-err`

  ```swifttest
  >> struct Point {
  >>    var x = 0.0, y = 0.0
  >>    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
  >>       x += deltaX
  >>       y += deltaY
  >>    }
  >> }
  -> let fixedPoint = Point(x: 3.0, y: 3.0)
  -> fixedPoint.moveBy(x: 2.0, y: 3.0)
  !$ error: cannot use mutating member on immutable value: 'fixedPoint' is a 'let' constant
  !! fixedPoint.moveBy(x: 2.0, y: 3.0)
  !! ~~~~~~~~~~ ^
  !$ note: change 'let' to 'var' to make it mutable
  !! let fixedPoint = Point(x: 3.0, y: 3.0)
  !! ^~~
  !! var
  // this will report an error
  ```
-->

<!--
  TODO: talk about nonmutating as well.
  Struct setters are implicitly 'mutating' by default and thus don't work on 'let's.
  However, JoeG says that this ought to work
  if the setter for the computed property is explicitly defined as @!mutating.
-->

### Mutating 메서드 내에서 self 할당 (Assigning to self Within a Mutating Method)

mutating 메서드에서는 암시적인 `self` 프로퍼티에 완전히 새로운 인스턴스를 할당할 수 있습니다.
위에서 본 `Point` 예시는 다음과 같은 방식으로 작성할 수도 있습니다:

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```

<!--
  - test: `selfStructuresAssign`

  ```swifttest
  -> struct Point {
        var x = 0.0, y = 0.0
        mutating func moveBy(x deltaX: Double, y deltaY: Double) {
           self = Point(x: x + deltaX, y: y + deltaY)
        }
     }
  >> var somePoint = Point(x: 1.0, y: 1.0)
  >> somePoint.moveBy(x: 2.0, y: 3.0)
  >> print("The point is now at (\(somePoint.x), \(somePoint.y))")
  << The point is now at (3.0, 4.0)
  ```
-->

이 mutating 메서드 `moveBy(x:y:)`는
`x`와 `y` 값이 대상 위치로 설정된 새로운 구조체를 생성하고, 이를 `self`에 할당합니다.
이 대체 버전의 메서드를 호출한 최종 결과는
이전 버전의 호출 결과와 동일합니다.

열거형의 mutating 메서드는 동일한 열거형의 다른 케이스를
암시적인 `self`에 설정할 수 있습니다:

```swift
enum TriStateSwitch {
    case off, low, high
    mutating func next() {
        switch self {
        case .off:
            self = .low
        case .low:
            self = .high
        case .high:
            self = .off
        }
    }
}
var ovenLight = TriStateSwitch.low
ovenLight.next()
// ovenLight is now equal to .high
ovenLight.next()
// ovenLight is now equal to .off
```

<!--
  - test: `selfEnumerations`

  ```swifttest
  -> enum TriStateSwitch {
        case off, low, high
        mutating func next() {
           switch self {
              case .off:
                 self = .low
              case .low:
                 self = .high
              case .high:
                 self = .off
           }
        }
     }
  -> var ovenLight = TriStateSwitch.low
  -> ovenLight.next()
  // ovenLight is now equal to .high
  -> ovenLight.next()
  // ovenLight is now equal to .off
  ```
-->

이 예시는 세 가지 상태를 가지는 스위치를 나타내는 열거형을 정의합니다.
이 스위치는 `next()` 메서드가 호출될 때마다
전원 상태
(`off`, `low`, `high`)를 순환합니다.

## 타입 메서드 (Type Methods)

위에 설명한 인스턴스 메서드는
특정 타입의 인스턴스에서 호출되는 메서드입니다.
타입 자체에서 호출되는 메서드도 정의할 수 있습니다.
이런 종류의 메서드를 *타입 메서드(type methods)*라고 합니다.
메서드의 `func` 키워드 앞에 `static` 키워드를
작성하여 타입 메서드를 나타냅니다.
클래스는 대신 `class` 키워드를 사용하여
하위 클래스가 해당 메서드를 재정의 할 수 있습니다.

> Note: Objective-C에서는 클래스에서만 타입 수준의 메서드(type-level methods)를 정의할 수 있습니다.
> Swift에서는 모든 클래스, 구조체, 열거형에 대해서 타입 메서드를 정의할 수 있습니다.
> 각 타입 메서드는 해당 타입에 명시적으로 속하게 됩니다.

타입 메서드는 인스턴스 메서드처럼 점 표기법을 사용하여 호출합니다.
그러나 해당 타입의 인스턴스가 아닌 타입자체에 대해 호출합니다.
`SomeClass`라는 클래스에 타입 메서드를 호출하는 방법은 다음과 같습니다:

```swift
class SomeClass {
    class func someTypeMethod() {
        // type method implementation goes here
    }
}
SomeClass.someTypeMethod()
```

<!--
  - test: `typeMethods`

  ```swifttest
  -> class SomeClass {
        class func someTypeMethod() {
           // type method implementation goes here
        }
     }
  -> SomeClass.someTypeMethod()
  ```
-->

타입 메서드의 본문 내에서
암시적인 `self` 프로퍼티는 타입의 인스턴스가 아닌
해당 타입 자체를 가르킵니다.
인스턴스 프로퍼티와 인스턴스 메서드 매개변수에서와 같이
`self`를 타입 프로퍼티와 타입 메서드 매개변수를
명확하게 사용할 수 있다는 의미입니다.

일반적으로 타입 메서드 내부에서 타입 이름 없이 사용하는
메서드나 프로퍼티 이름은 모두 타입 수준의 메서드나 프로퍼티를 참조합니다.
타입 메서드 안에서는 다른 타입 메서드를
타입 이름 없이 호출할 수 있습니다.
유사하게 구조체와 열거형에서 정의된 타입 메서드는 타입 프로퍼티에도
마찬가지로 타입 이름 없이 접근할 수 있습니다.

아래 예시는 게임 내에서 플레이어의 레벨이나 스테이지 진행사항을 추적하는
`LevelTracker`라는 구조체를 정의합니다.
이것은 1인용 게임이지만,
단일 디바이스에서 여러 명의 플레이어를 위한 정보를 저장할 수 있습니다.

게임을 처음 시작할 때는 1레벨을 제외한 모든 레벨이 잠겨있습니다.
플레이어가 레벨을 끝낼 때마다
디바이스에 모든 플레이어가 플레이 할 수 있도록 레벨이 잠금 해제됩니다.
`LevelTracker` 구조체는 게임의 레벨이 잠금 해제되는 것을 추적하기 위해
타입 프로퍼티와 메서드를 사용합니다.
각 플레이어의 현재 레벨은 인스턴스 프로퍼티를 통해 추적합니다.

```swift
struct LevelTracker {
    static var highestUnlockedLevel = 1
    var currentLevel = 1

    static func unlock(_ level: Int) {
        if level > highestUnlockedLevel { highestUnlockedLevel = level }
    }

    static func isUnlocked(_ level: Int) -> Bool {
        return level <= highestUnlockedLevel
    }

    @discardableResult
    mutating func advance(to level: Int) -> Bool {
        if LevelTracker.isUnlocked(level) {
            currentLevel = level
            return true
        } else {
            return false
        }
    }
}
```

<!--
  - test: `typeMethods`

  ```swifttest
  -> struct LevelTracker {
        static var highestUnlockedLevel = 1
        var currentLevel = 1

  ->    static func unlock(_ level: Int) {
           if level > highestUnlockedLevel { highestUnlockedLevel = level }
        }

  ->    static func isUnlocked(_ level: Int) -> Bool {
           return level <= highestUnlockedLevel
        }

  ->    @discardableResult
        mutating func advance(to level: Int) -> Bool {
           if LevelTracker.isUnlocked(level) {
              currentLevel = level
              return true
           } else {
              return false
           }
        }
     }
  ```
-->

`LevelTracker` 구조체는 모든 플레이어가 잠금 해제한 가장 높은 레벨을 추적합니다.
이 값은 `highestUnlockedLevel`이라는 타입 프로퍼티에 저장됩니다.

`LevelTracker`는 `highestUnlockedLevel` 프로퍼티와 함께
동작하는 두 개의 타입 함수도 정의합니다.
첫 번째 타입 함수는 `unlock(_:)`이라 하며,
새로운 레벨이 잠금 해제될 때마다 `highestUnlockedLevel`의 값을 갱신합니다.
두 번째 타입 함수는 `isUnlocked(_:)`이라는 편의 타입 함수이며,
특정 레벨이 이미 잠금 해제되있다면 `true`를 반환합니다.
(이 타입 메서드는 `LevelTracker.highestUnlockedLevel`로 작성할 필요없이
`highestUnlockedLevel` 타입 프로퍼티를 접근할 수 있습니다.)

타입 프로퍼티와 타입 메서드 외에도
`LevelTracker`는 게임을 통해 각 플레이어의 진행사항을 추적합니다.
플레이어가 현재 플레이 중인 레벨을 추적하는
`currentLevel`이라는 인스턴스 프로퍼티를 사용합니다.

`currentLevel` 프로퍼티를 관리하기 위해
`LevelTracker`는 `advance(to:)`라는 인스턴스 메서드를 정의합니다.
`currentLevel`을 갱신 전에
이 메서드는 요청된 새 레벨이 이미 잠금 해제되었는지 판단합니다.
`advance(to:)` 메서드는 `currentLevel`을 설정가능한지 아닌지를
나타내기 위해 Boolean 값을 반환합니다.
`advance(to:)` 메서드를 호출하여
반환값을 무시하는
경우도 있기 때문에,
이 함수는 `@discardableResult` 속성으로 표시됩니다.
더 자세한 내용은
[속성 (Attributes)](../language-reference/attributes.md)을 참고바랍니다.

`LevelTracker` 구조체는 아래에 나오는
각 플레이어의 진행 상태를 추적하고 업데이트하는 `Player` 클래스와 함께 사용됩니다:

```swift
class Player {
    var tracker = LevelTracker()
    let playerName: String
    func complete(level: Int) {
        LevelTracker.unlock(level + 1)
        tracker.advance(to: level + 1)
    }
    init(name: String) {
        playerName = name
    }
}
```

<!--
  - test: `typeMethods`

  ```swifttest
  -> class Player {
        var tracker = LevelTracker()
        let playerName: String
        func complete(level: Int) {
           LevelTracker.unlock(level + 1)
           tracker.advance(to: level + 1)
        }
        init(name: String) {
           playerName = name
        }
     }
  ```
-->

`Player` 클래스는 플레이어의 진행 상태를 추적하기 위해
새로운 `LevelTracker`의 인스턴스를 생성합니다.
플레이어가 특정 레벨을 완료했는지 판단하는
`complete(level:)`이라는 메서드도 제공합니다.
이 메서드는 모든 플레이어에게 다음 레벨을 잠금 해제하고
다음 레벨로 이동하기 위해 플레이어의 진행 상태를 업데이트 합니다.
(이전 줄에서 `LevelTracker.unlock(_:)`을 호출하여
해당 레벨을 잠금 해제하기 때문에
`advance(to:)`의 Boolean 반환값은 무시됩니다.)

새로운 플레이어를 위한 `Player` 클래스의 인스턴스를 생성할 수 있고
플레이어가 레벨1을 완료하면 어떤 일이 생기는지 알 수 있습니다:

```swift
var player = Player(name: "Argyrios")
player.complete(level: 1)
print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
// Prints "highest unlocked level is now 2"
```

<!--
  - test: `typeMethods`

  ```swifttest
  -> var player = Player(name: "Argyrios")
  -> player.complete(level: 1)
  -> print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
  <- highest unlocked level is now 2
  ```
-->

게임에서 아직 잠금 해제되지 않은 레벨로 이동을 하려는
두 번째 플레이어를 생성하면
플레이어의 현재 레벨을 실패로 설정합니다:

```swift
player = Player(name: "Beto")
if player.tracker.advance(to: 6) {
    print("player is now on level 6")
} else {
    print("level 6 has not yet been unlocked")
}
// Prints "level 6 has not yet been unlocked"
```

<!--
  - test: `typeMethods`

  ```swifttest
  -> player = Player(name: "Beto")
  -> if player.tracker.advance(to: 6) {
        print("player is now on level 6")
     } else {
        print("level 6 hasn't yet been unlocked")
     }
  <- level 6 hasn't yet been unlocked
  ```
-->

<!--
  TODO: Method Binding
  --------------------

  TODO: you can get a function that refers to a method, either with or without the 'self' argument already being bound:
  class C {
     func foo(x: Int) -> Float { ... }
  }
  var c = C()
  var boundFunc = c.foo   // a function with type (Int) -> Float
  var unboundFunc = C.foo // a function with type (C) -> (Int) -> Float

  TODO: selector-style methods can be referenced as foo.bar:bas:
  (see Doug's comments from the 2014-03-12 release notes)
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
