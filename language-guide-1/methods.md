# 메서드 \(Methods\)

<!--
Methods are functions that are associated with a particular type. Classes, structures, and enumerations can all define instance methods, which encapsulate specific tasks and functionality for working with an instance of a given type. Classes, structures, and enumerations can also define type methods, which are associated with the type itself. Type methods are similar to class methods in Objective-C.

The fact that structures and enumerations can define methods in Swift is a major difference from C and Objective-C. In Objective-C, classes are the only types that can define methods. In Swift, you can choose whether to define a class, structure, or enumeration, and still have the flexibility to define methods on the type you create.
-->

_메서드 \(Methods\)_ 는 특정 타입과 연관된 함수입니다. 클래스, 구조체, 그리고 열거형은 주어진 타입의 인스턴스 동작을 위한 특정 작업과 기능을 캡슐화하는 인스턴스 메서드를 정의할 수 있습니다. 클래스, 구조체, 그리고 열거형은 타입 자체와 연관된 타입 메서드를 정의할 수도 있습니다. 타입 메서드는 Objective-C에서 클래스 메서드와 유사합니다.

Swift에서 구조체와 열거형에 메서드를 정의할 수 있다는 사실은 C와 Objective-C와의 가장 큰 차이점입니다. Objective-C에서 클래스는 메서드만 정의할 수 있는 유일한 타입입니다. Swift에서는 클래스, 구조체, 또는 열거형을 정의할 지 선택할 수 있으며 생성한 타입에 대한 메서드를 유연하게 정의할 수 있습니다.

## 인스턴스 메서드 \(Instance Methods\)

<!--
Instance methods are functions that belong to instances of a particular class, structure, or enumeration. They support the functionality of those instances, either by providing ways to access and modify instance properties, or by providing functionality related to the instance’s purpose. Instance methods have exactly the same syntax as functions, as described in Functions.

You write an instance method within the opening and closing braces of the type it belongs to. An instance method has implicit access to all other instance methods and properties of that type. An instance method can be called only on a specific instance of the type it belongs to. It can’t be called in isolation without an existing instance.

Here’s an example that defines a simple Counter class, which can be used to count the number of times an action occurs:
-->

_인스턴스 메서드 \(Instance methods\)_ 는 특정 클래스, 구조체, 또는 열거형의 인스턴스에 속하는 함수입니다. 인스턴스 프로퍼티에 접근하고 수정하는 방법을 제공하거나 인스턴스의 목적과 관련된 기능을 제공합니다. 인스턴스 메서드는 [함수 \(Functions\)](functions.md) 에서 설명한 대로 함수 구문과 완벽하게 동일합니다.

인스턴스 메서드는 속하는 타입의 중괄호 내에 작성합니다. 인스턴스 메서드는 다른 인스턴스 메서드와 타입의 프로퍼티에 암시적으로 접근할 수 있습니다. 인스턴스 메서드는 자신이 속한 타입의 특정 인스턴스에서만 호출될 수 있습니다. 기존 인스턴스 없이는 호출할 수 없습니다.

다음은 작업이 발생한 횟수를 계산하는데 사용할 수 있는 간단한 `Counter` 클래스 정의의 예입니다:

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
The Counter class defines three instance methods:

* increment() increments the counter by 1.
* increment(by: Int) increments the counter by a specified integer amount.
* reset() resets the counter to zero.

The Counter class also declares a variable property, count, to keep track of the current counter value.

You call instance methods with the same dot syntax as properties:
-->

`Counter` 클래스는 3개의 인스턴스 메서드를 정의합니다:

* `increment()` 는 `1` 씩 카운터를 증가시킵니다.
* `increment(by: Int)` 는 특정 정수 크기만큼 카운터를 증가시킵니다.
* `reset()` 은 카운터를 0으로 재설정 합니다.

`Counter` 클래스는 현재 카운터 값을 추적하는 `count` 프로퍼티 변수도 선언합니다.

프로퍼티 동일하게 점 구문으로 인스턴스 메서드를 호출합니다:

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
Function parameters can have both a name (for use within the function’s body) and an argument label (for use when calling the function), as described in Function Argument Labels and Parameter Names. The same is true for method parameters, because methods are just functions that are associated with a type.
-->

함수 파라미터는 [함수 인수 라벨과 파라미터 명 \(Function Argument Labels and Parameter Names\)](functions.md#function-argument-labels-and-parameter-names) 에서 설명 했듯이 함수의 본문 내에서 사용하는 이름과 함수를 호출할 때 사용하는 인수 라벨 둘다 가질 수 있습니다. 메서드는 타입과 관련된 함수이므로 메서드 파라미터도 동일하게 적용됩니다.

### self 프로퍼티 \(The self Property\)

<!--
Every instance of a type has an implicit property called self, which is exactly equivalent to the instance itself. You use the self property to refer to the current instance within its own instance methods.

The increment() method in the example above could have been written like this:
-->

타입의 모든 인스턴스는 인스턴스 자체와 정확하게 일치하는 `self` 라는 암시적 프로퍼티를 가지고 있습니다. 자체 인스턴스 메서드 내에서 현재 인스턴스를 참조하기 위해 `self` 프로퍼티를 사용합니다.

위 예제에서 `increment()` 메서드는 아래와 같이 작성될 수 있습니다:

```swift
func increment() {
    self.count += 1
}
```

<!--
In practice, you don’t need to write self in your code very often. If you don’t explicitly write self, Swift assumes that you are referring to a property or method of the current instance whenever you use a known property or method name within a method. This assumption is demonstrated by the use of count (rather than self.count) inside the three instance methods for Counter.

The main exception to this rule occurs when a parameter name for an instance method has the same name as a property of that instance. In this situation, the parameter name takes precedence, and it becomes necessary to refer to the property in a more qualified way. You use the self property to distinguish between the parameter name and the property name.

Here, self disambiguates between a method parameter called x and an instance property that’s also called x:
-->

실제로 코드에서 `self` 를 꼭 작성할 필요가 없습니다. `self` 를 명시적으로 작성하지 않으면 Swift는 메서드 내에서 이미 알고 있는 프로퍼티 또는 메서드 이름을 사용할 때마다 현재 인스턴스의 프로퍼티 또는 메서드를 참조한다고 가정합니다. 이 가정은 `Counter` 에 3개의 인스턴스 메서드 내에서 `self.count` 가 아닌 `count` 를 사용하여 입증됩니다.

이 규칙의 주요 예외사항은 인스턴스 메서드에 파라미터 명이 그 인스턴스에 프로퍼티 명과 동일할 때 발생합니다. 이러한 경우 파라미터 명이 더 우선시 되고 프로퍼티를 참조하려면 더 규정된 방식으로 참조해야 합니다. 파라미터 명과 프로퍼티 명을 분리하기 위해 `self` 프로퍼티를 사용합니다.

여기서 `self` 는 `x` 라는 메서드 파라미터와 `x` 라고 하는 인스턴스 프로퍼티를 명확하게 합니다:

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
Without the self prefix, Swift would assume that both uses of x referred to the method parameter called x.
-->

`self` 접두사가 없으면 Swift는 `x` 의 2가지 사용이 모두 `x` 라는 메서드 파라미터를 참조한다고 가정합니다.

### 인스턴스 메서드 내에서 값 타입 수정 \(Modifying Value Types from Within Instance Methods\)

<!--
Structures and enumerations are value types. By default, the properties of a value type can’t be modified from within its instance methods.

However, if you need to modify the properties of your structure or enumeration within a particular method, you can opt in to mutating behavior for that method. The method can then mutate (that is, change) its properties from within the method, and any changes that it makes are written back to the original structure when the method ends. The method can also assign a completely new instance to its implicit self property, and this new instance will replace the existing one when the method ends.

You can opt in to this behavior by placing the mutating keyword before the func keyword for that method:
-->

구조체와 열거형은 _값 타입_ 입니다. 기본적으로 값 타입의 프로퍼티는 인스턴스 메서드 내에서 수정될 수 없습니다.

그러나 특정 메서드 내에서 구조체나 열거형의 프로퍼티 수정이 필요하다면 해당 메서드에 대한 동작을 _변경_ 하도록 선택할 수 있습니다. 그러면 이 메서드는 메서드 내에서 프로퍼티를 변경할 수 있고 메서드가 끝나면 기존 구조체에 변경사항이 작성됩니다. 이 메서드는 암시적 `self` 프로퍼티에 새로운 인스턴스를 할당하고 새로운 인스턴스는 메서드가 종료되면 기존에 존재하던 인스턴스를 대체하게 됩니다.

해당 메서드는 `func` 키워드 전에 `mutating` 키워드를 위치 시켜 이 동작을 선택할 수 있습니다:

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
The Point structure above defines a mutating moveBy(x:y:) method, which moves a Point instance by a certain amount. Instead of returning a new point, this method actually modifies the point on which it’s called. The mutating keyword is added to its definition to enable it to modify its properties.

Note that you can’t call a mutating method on a constant of structure type, because its properties can’t be changed, even if they’re variable properties, as described in Stored Properties of Constant Structure Instances:
-->

위의 `Point` 구조체는 포함한 양만큼 `Point` 인스턴스를 이동시키는 변경가능한 `moveBy(x:y:)` 메서드를 정의합니다. 새로운 위치를 반환하는 대신에 이 메서드는 실제로 호출된 위치를 수정합니다. `mutating` 키워드는 프로퍼티를 수정할 수 있도록 정의에 추가됩니다.

[상수 구조체 인스턴스의 저장된 프로퍼티 \(Stored Properties of Constant Structure Instances\)](properties.md#stored-properties-of-constant-structure-instances) 에서 설명했듯이 프로퍼티가 프로퍼티 변수이더라도 변경할 수 없기 때문에 구조체 타입의 상수에 대해 변경 메서드를 호출할 수 없습니다:

```swift
let fixedPoint = Point(x: 3.0, y: 3.0)
fixedPoint.moveBy(x: 2.0, y: 3.0)
// this will report an error
```

### 변경 메서드 내에서 self 할당 \(Assigning to self Within a Mutating Method\)

<!--
Mutating methods can assign an entirely new instance to the implicit self property. The Point example shown above could have been written in the following way instead:
-->

변경 메서드 \(Mutating methods\)는 암시적 `self` 프로퍼티를 완전히 새로운 인스턴스로 할당할 수 있습니다. 위에서 본 `Point` 예제는 대신 다음과 같은 방식으로 작성되었을 수도 있습니다:

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```

<!--
This version of the mutating moveBy(x:y:) method creates a new structure whose x and y values are set to the target location. The end result of calling this alternative version of the method will be exactly the same as for calling the earlier version.

Mutating methods for enumerations can set the implicit self parameter to be a different case from the same enumeration:
-->

이 버전의 변경가능한 `moveBy(x:y:)` 메서드는 `x` 와 `y` 값이 대상 위치로 설정된 새로운 구조체를 생성합니다. 이 대체 버전의 메서드를 호출한 최종 결과는 이전 버전의 호출 결과와 동일합니다.

열거형의 변경 메서드는 동일한 열거형에서 다른 케이스로 암시적으로 `self` 파라미터를 설정할 수 있습니다:

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
This example defines an enumeration for a three-state switch. The switch cycles between three different power states (off, low and high) every time its next() method is called.
-->

이 예제는 3개의 상태를 나타내는 스위치를 열거형으로 정의합니다. 이 스위치는 `next()` 메서드가 호출될 때마다 3개의 전원 상태 \(`off`, `low`, `high`\)를 순환합니다.

## 타입 메서드 \(Type Methods\)

<!--
Instance methods, as described above, are methods that you call on an instance of a particular type. You can also define methods that are called on the type itself. These kinds of methods are called type methods. You indicate type methods by writing the static keyword before the method’s func keyword. Classes can use the class keyword instead, to allow subclasses to override the superclass’s implementation of that method.
-->

위에 설명한 인스턴스 메서드는 특정 타입의 인스턴스에서 호출하는 메서드 입니다. 타입 자체에서 호출되는 메서드도 정의할 수 있습니다. 이런 종류의 메서드를 _타입 메서드 \(type methods\)_ 라고 합니다. 메서드의 `func` 키워드 전에 `static` 키워드를 작성하여 타입 메서드를 나타냅니다. 클래스는 대신 `class` 키워드를 사용하여 하위 클래스가 해당 메서드의 수퍼 클래스 구현을 재정의 할 수 있습니다.

<!--
NOTE
In Objective-C, you can define type-level methods only for Objective-C classes. In Swift, you can define type-level methods for all classes, structures, and enumerations. Each type method is explicitly scoped to the type it supports.
-->

> NOTE   
> Objective-C에서는 Objective-C 클래스에 대해서만 타입 레벨 메서드 \(type-level methods\)를 정의할 수 있습니다. Swift에서는 모든 클래스, 구조체, 그리고 열거형에 대해서 타입 레벨 메서드를 정의할 수 있습니다. 각 타입 메서드는 지원하는 타입으로 명시적으로 범위가 지정됩니다.

<!--
Type methods are called with dot syntax, like instance methods. However, you call type methods on the type, not on an instance of that type. Here’s how you call a type method on a class called SomeClass:
-->

타입 메서드는 인스턴스 메서드처럼 점 구문으로 호출됩니다. 그러나 해당 타입의 인스턴스가 아닌 타입으로 타입 메서드를 호출합니다. `SomeClass` 라는 클래스에서 타입 메서드를 호출하는 방법은 다음과 같습니다:

```swift
class SomeClass {
    class func someTypeMethod() {
        // type method implementation goes here
    }
}
SomeClass.someTypeMethod()
```

<!--
Within the body of a type method, the implicit self property refers to the type itself, rather than an instance of that type. This means that you can use self to disambiguate between type properties and type method parameters, just as you do for instance properties and instance method parameters.

More generally, any unqualified method and property names that you use within the body of a type method will refer to other type-level methods and properties. A type method can call another type method with the other method’s name, without needing to prefix it with the type name. Similarly, type methods on structures and enumerations can access type properties by using the type property’s name without a type name prefix.

The example below defines a structure called LevelTracker, which tracks a player’s progress through the different levels or stages of a game. It’s a single-player game, but can store information for multiple players on a single device.

All of the game’s levels (apart from level one) are locked when the game is first played. Every time a player finishes a level, that level is unlocked for all players on the device. The LevelTracker structure uses type properties and methods to keep track of which levels of the game have been unlocked. It also tracks the current level for an individual player.
-->

타입 메서드의 본문 내에서 암시적 `self` 프로퍼티는 타입의 인스턴스가 아닌 타입 자체를 참조합니다. 인스턴스 프로퍼티와 인스턴스 메서드 파라미터에서와 같이 `self` 를 타입 프로퍼티와 타입 메서드 파라미터를 명확하게 하기위해 사용할 수 있다는 의미입니다.

일반적으로 타입 메서드의 본문 내에서 사용하는 정규화되지 않은 메서드와 프로퍼티 이름은 다른 타입 레벨 메서드와 프로퍼티를 참조합니다. 타입 메서드는 타입 이름을 접두어로 필요치 않고 다른 메서드의 이름으로 다른 타입 메서드를 호출할 수 있습니다. 유사하게 구조체와 열거형에서 타입 메서드는 타입 이름 접두어 없이 타입 프로퍼티의 이름을 사용하여 타입 프로퍼티에 접근할 수 있습니다.

아래 예제는 게임의 다른 레벨 또는 스테이지를 통해 사용자의 진행상황을 추적하는 `LevelTracker` 라는 구조체를 정의합니다. 이것은 1인용 게임이지만 단일 디바이스에서 여러명의 플레이어를 위한 정보를 저장할 수 있습니다.

첫번째 레벨을 제외한 게임의 모든 레벨은 게임을 처음 플레이할 때 잠겨있습니다. 플레이어가 레벨을 끝낼 때마다 디바이스에 모든 플레이어가 플레이 할 수 있도록 레벨이 풀립니다. `LevelTracker` 구조체는 게임의 레벨이 풀리는 것을 추적하기 위해 타입 프로퍼티와 메서드를 사용합니다. 각 플레이어의 현재 레벨도 추적합니다.

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
The LevelTracker structure keeps track of the highest level that any player has unlocked. This value is stored in a type property called highestUnlockedLevel.

LevelTracker also defines two type functions to work with the highestUnlockedLevel property. The first is a type function called unlock(_:), which updates the value of highestUnlockedLevel whenever a new level is unlocked. The second is a convenience type function called isUnlocked(_:), which returns true if a particular level number is already unlocked. (Note that these type methods can access the highestUnlockedLevel type property without your needing to write it as LevelTracker.highestUnlockedLevel.)

In addition to its type property and type methods, LevelTracker tracks an individual player’s progress through the game. It uses an instance property called currentLevel to track the level that a player is currently playing.

To help manage the currentLevel property, LevelTracker defines an instance method called advance(to:). Before updating currentLevel, this method checks whether the requested new level is already unlocked. The advance(to:) method returns a Boolean value to indicate whether or not it was actually able to set currentLevel. Because it’s not necessarily a mistake for code that calls the advance(to:) method to ignore the return value, this function is marked with the @discardableResult attribute. For more information about this attribute, see Attributes.

The LevelTracker structure is used with the Player class, shown below, to track and update the progress of an individual player:
-->

`LevelTracker` 구조체는 모든 플레이어가 푼 가장 높은 레벨을 추적합니다. 이 값은 `highestUnlockedLevel` 이라는 타입 프로퍼티에 저장됩니다.

`LevelTracker` 는 `highestUnlockedLevel` 프로퍼티와 함께 동작하는 2개의 타입 함수도 정의합니다. 첫번째 타입 함수는 `unlock(_:)` 이라 하며 새로운 레벨이 풀릴 때마다 `highestUnlockedLevel` 의 값을 업데이트 합니다. 두번째 타입 함수는 `isUnlocked(_:)` 이라 하며 편의 타입 함수이며 특정 레벨이 이미 풀려있다면 `true` 를 반환합니다 \(이 타입 메서드는 `LevelTracker.highestUnlockedLevel` 로 작성할 필요없이 `highestUnlockedLevel` 타입 프로퍼티를 접근할 수 있습니다\).

타입 프로퍼티와 타입 메서드 외에도 `LevelTracker` 는 게임을 통해 각 플레이어의 진행사항을 추적합니다. 플레이어가 현재 플레이 중인 레벨을 추적하는 `currentLevel` 이라는 인스턴스 프로퍼티를 사용합니다.

`currentLevel` 프로퍼티 관리를 돕기위해 `LevelTracker` 는 `advance(to:)` 라는 인스턴스 메서드를 정의합니다. `currentLevel` 업데이트 전에 이 메서드는 요청된 새 레벨이 이미 풀렸는지 판단합니다. `advance(to:)` 메서드는 `currentLevel` 을 설정가능한지 아닌지를 나타내기 위해 부울값으로 반환합니다. `advance(to:)` 메서드를 호출하여 반환값을 무시하는 코드가 실수가 아니기 때문에 이 함수는 `@discardableResult` 속성으로 표시됩니다. 더 자세한 내용은 [속성 \(Attributes\)](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html) 을 참고 바랍니다.

`LevelTracker` 구조체는 아래에서 봤듯이 각 플레이어의 진행상태를 추적하고 업데이트 하기 위해 `Player` 클래스와 함께 사용됩니다:

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
The Player class creates a new instance of LevelTracker to track that player’s progress. It also provides a method called complete(level:), which is called whenever a player completes a particular level. This method unlocks the next level for all players and updates the player’s progress to move them to the next level. (The Boolean return value of advance(to:) is ignored, because the level is known to have been unlocked by the call to LevelTracker.unlock(_:) on the previous line.)

You can create an instance of the Player class for a new player, and see what happens when the player completes level one:
-->

`Player` 클래스는 플레이어의 진행상태를 추적하기 위해 새로운 `LevelTracker` 의 인스턴스를 생성합니다. 플레이어가 특정 레벨을 완료했는지 판단하는 `complete(level:)` 이라는 메서드도 제공합니다. 이 메서드는 모든 플레이어에게 다음 레벨을 풀고 다음 레벨로 이동하기 위해 플레이어의 진행상태를 업데이트 합니다 \(이전 라인에서 `LevelTracker.unlock(_:)` 을 호출하여 해당 레벨을 풀기 때문에 `advance(to:)` 의 부울 반환 값은 무시됩니다\).

새로운 플레이어를 위한 `Player` 클래스의 인스턴스를 생성할 수 있고 플레이어가 레벨1을 완료하면 어떤 일이 생기는지 알 수 있습니다:

```swift
var player = Player(name: "Argyrios")
player.complete(level: 1)
print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
// Prints "highest unlocked level is now 2"
```

<!--
If you create a second player, whom you try to move to a level that’s not yet unlocked by any player in the game, the attempt to set the player’s current level fails:
-->

게임에서 아직 풀리지 않은 레벨로 이동을 하려는 2번째 플레이어를 생성하면 플레이어의 현재 레벨을 실패로 설정합니다:

```swift
player = Player(name: "Beto")
if player.tracker.advance(to: 6) {
    print("player is now on level 6")
} else {
    print("level 6 has not yet been unlocked")
}
// Prints "level 6 has not yet been unlocked"
```

