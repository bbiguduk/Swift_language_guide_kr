# 메모리 안전성 \(Memory Safety\)

<!--
By default, Swift prevents unsafe behavior from happening in your code. For example, Swift ensures that variables are initialized before they’re used, memory isn’t accessed after it’s been deallocated, and array indices are checked for out-of-bounds errors.

Swift also makes sure that multiple accesses to the same area of memory don’t conflict, by requiring code that modifies a location in memory to have exclusive access to that memory. Because Swift manages memory automatically, most of the time you don’t have to think about accessing memory at all. However, it’s important to understand where potential conflicts can occur, so you can avoid writing code that has conflicting access to memory. If your code does contain conflicts, you’ll get a compile-time or runtime error.
-->

기본적으로 Swift는 코드에서 발생할 안전하지 않은 동작을 방지합니다. 예를 들어 Swift는 변수가 사용되기 전에 초기화 되고 메모리가 할당 해제된 후에는 접근되지 않으며 배열 인덱스는 범위를 벗어난 에러가 있는지 확인합니다.

Swift는 메모리의 위치를 수정하는 코드가 해당 메모리에 대한 독점 접근 권한을 갖도록 요구함으로써 동일한 메모리 영역에 대해 다중 접근이 충돌나지 않도록 합니다. Swift는 메모리를 자동으로 관리하기 때문에 대부분의 경우에 메모리 접근에 대해 생각할 필요가 없습니다. 그러나 잠재적으로 충돌이 발생할 수 있는 것을 이해해야 작성한 코드가 메모리에 접근 충돌을 피할 수 있습니다. 코드에 충돌이 포함되어 있다면 컴파일이나 런타임 에러가 발생합니다.

## 메모리에 충돌하는 접근 이해 \(Understanding Conflicting Access to Memory\)

<!--
Access to memory happens in your code when you do things like set the value of a variable or pass an argument to a function. For example, the following code contains both a read access and a write access:
-->

메모리에 접근하는 것은 변수의 값을 설정하거나 함수에 인수를 전달하는 것과 같은 동작을 할 때 코드에서 발생합니다. 예를 들어 다음 코드는 읽기 접근과 쓰기 접근 모두 포함합니다:

```swift
// A write access to the memory where one is stored.
var one = 1

// A read access from the memory where one is stored.
print("We're number \(one)!")
```

<!--
A conflicting access to memory can occur when different parts of your code are trying to access the same location in memory at the same time. Multiple accesses to a location in memory at the same time can produce unpredictable or inconsistent behavior. In Swift, there are ways to modify a value that span several lines of code, making it possible to attempt to access a value in the middle of its own modification.

You can see a similar problem by thinking about how you update a budget that’s written on a piece of paper. Updating the budget is a two-step process: First you add the items’ names and prices, and then you change the total amount to reflect the items currently on the list. Before and after the update, you can read any information from the budget and get a correct answer, as shown in the figure below.
-->

메모리에 충돌하는 접근은 코드의 다른 부분이 같은 시간에 메모리의 같은 위치에 접근할 때 발생할 수 있습니다. 같은 시간에 메모리의 위치에 다중 접근은 예측할 수 없거나 일관성 없는 동작이 발생할 수 있습니다. Swift에는 코드의 여러 라인에 걸쳐있는 값을 수정하기 위한 방법이 있어서 자체 수정 중에 값에 접근할 수 있습니다.

종이에 쓰여진 예산을 어떻게 업데이트 하는지 생각해 보면 비슷한 문제를 확인할 수 있습니다. 예산을 업데이트 하는 것은 2단계 프로세스 입니다: 먼저 항목들의 이름과 가격을 추가한 다음에 현재 목록에 있는 항목을 반영하기 위해 총 금액을 변경합니다. 업데이트 전후에 아래 그림과 같이 예산에서 모든 정보를 읽고 올바른 답을 얻을 수 있습니다.

![Memory Shopping](../.gitbook/assets/25_memory_shopping_2x.png)

<!--
While you’re adding items to the budget, it’s in a temporary, invalid state because the total amount hasn’t been updated to reflect the newly added items. Reading the total amount during the process of adding an item gives you incorrect information.

This example also demonstrates a challenge you may encounter when fixing conflicting access to memory: There are sometimes multiple ways to fix the conflict that produce different answers, and it’s not always obvious which answer is correct. In this example, depending on whether you wanted the original total amount or the updated total amount, either $5 or $320 could be the correct answer. Before you can fix the conflicting access, you have to determine what it was intended to do.
-->

예산에 항목을 추가하는 동안 총 금액이 새로 추가된 항목을 반영하기 위해 업데이트 되지 않았기 때문에 임시적으로 유효하지 않은 상태입니다. 항목을 추가하는 동안 총 금액을 읽으면 잘못된 정보가 제공됩니다.

이 예제는 또한 메모리에 충돌하는 접근을 수정할 때 발생할 수 있는 문제를 보여줍니다: 때로는 충돌을 수정하는 여러가지 방법이 있으며 어떤 답변이 올바른지 항상 명확하지 않습니다. 이 예제에서 기존 총 금액 또는 업데이트 된 총 금액을 원하는지에 따라 $5 또는 $320이 정답이 될 수 있습니다. 충돌하는 접근을 수정하려면 수행할 작업의 의도를 파악해야 합니다.

<!--
NOTE
If you’ve written concurrent or multithreaded code, conflicting access to memory might be a familiar problem. However, the conflicting access discussed here can happen on a single thread and doesn’t involve concurrent or multithreaded code.

If you have conflicting access to memory from within a single thread, Swift guarantees that you’ll get an error at either compile time or runtime. For multithreaded code, use Thread Sanitizer to help detect conflicting access across threads.
-->

> NOTE   
> 동시 또는 다중 쓰레드 코드를 작성한 경우 메모리 접근 충돌이 익숙한 문제일 수 있습니다. 그러나 여기서 설명한 충돌 접근은 단일 쓰레드에서 발생할 수 있으며 동시 또는 다중 쓰레드 코드를 포함하지 않습니다.
>
> 단일 쓰레드 내에서 메모리 접근이 충돌하는 경우 Swift는 컴파일 이나 런타임 시 에러가 발생합니다. 다중 쓰레드 코드의 경우 [Thread Sanitizer](https://developer.apple.com/documentation/code_diagnostics/thread_sanitizer) 사용하여 쓰레드 간에 충돌하는 접근을 감지할 수 있습니다.

### 메모리 접근의 특징 \(Characteristics of Memory Access\)

<!--
There are three characteristics of memory access to consider in the context of conflicting access: whether the access is a read or a write, the duration of the access, and the location in memory being accessed. Specifically, a conflict occurs if you have two accesses that meet all of the following conditions:

* At least one is a write access or a nonatomic access.
* They access the same location in memory.
* Their durations overlap.

The difference between a read and write access is usually obvious: a write access changes the location in memory, but a read access doesn’t. The location in memory refers to what is being accessed—for example, a variable, constant, or property. The duration of a memory access is either instantaneous or long-term.

An operation is atomic if it uses only C atomic operations; otherwise it’s nonatomic. For a list of those functions, see the stdatomic(3) man page.

An access is instantaneous if it’s not possible for other code to run after that access starts but before it ends. By their nature, two instantaneous accesses can’t happen at the same time. Most memory access is instantaneous. For example, all the read and write accesses in the code listing below are instantaneous:
-->

충돌 접근의 맥락에서 고려해야 할 메모리 접근의 3가지 특성이 있습니다: 접근하는 동안 읽기 또는 쓰기 접근인지 여부와 접근된 메모리의 위치입니다. 특히 아래의 조건을 모두 만족하는 2개의 접근이 있다면 충돌이 발생합니다:

* 적어도 하나의 쓰기 접근 또는 nonatomic 접근입니다.
* 메모리의 같은 위치에 접근합니다.
* 접근하는 시간이 겹칩니다.

읽기와 쓰기 접근 간의 차이는 일반적으로 명확합니다: 쓰기 접근은 메모리의 위치를 변경하지만 읽기 접근은 변경하지 않습니다. 메모리의 위치는 예를 들어 변수, 상수 또는 프로퍼티와 같이 접근 중인 항목을 나타냅니다. 메모리 접근 기간은 순간적이거나 장기적입니다.

C atomic 연산만 사용하는 경우 _atomic_ 이고 그렇지 않으면 nonatomic 입니다. 이러한 함수 목록은 `stdatomic(3)` 페이지를 참고 바랍니다.

접근이 시작되고 종료되기 전에 다른 코드를 실행할 수 없는 경우 접근은 _즉시_ 이루어집니다. 일반적으로 2개의 즉시 접근은 동시에 발생할 수 없습니다. 대부분 메모리 접근은 즉각적입니다. 예를 들어 아래 코드 목록의 모든 읽기와 쓰기 접근은 즉각적입니다:

```swift
func oneMore(than number: Int) -> Int {
    return number + 1
}

var myNumber = 1
myNumber = oneMore(than: myNumber)
print(myNumber)
// Prints "2"
```

<!--
However, there are several ways to access memory, called long-term accesses, that span the execution of other code. The difference between instantaneous access and long-term access is that it’s possible for other code to run after a long-term access starts but before it ends, which is called overlap. A long-term access can overlap with other long-term accesses and instantaneous accesses.

Overlapping accesses appear primarily in code that uses in-out parameters in functions and methods or mutating methods of a structure. The specific kinds of Swift code that use long-term accesses are discussed in the sections below.
-->

그러나 다른 코드 실행에 걸쳐있는 _장기 \(long-term\)_ 접근이라는 메모리 접근 방법은 여러가지 입니다. 즉각 접근 \(instantaneous access\)와 장기 접근 \(long-term access\) 간의 차이점은 장기 접근이 시작되고 종료되기 전에 다른 코드가 실행될 수 있다는 것입니다. 이것을 _오버랩 \(overlap\)_ 이라 합니다. 장기 접근은 다른 장기 접근과 즉각 접근과 오버랩 될 수 있습니다.

겹치는 접근은 함수와 메서드에서 in-out 파미터를 사용하거나 구조체의 변경하는 메서드를 사용하는 코드에 주로 나타납니다. 장기 접근을 사용하는 Swift 코드의 특정 종류는 아래 섹션에서 설명합니다.

## In-Out 파라미터에 충돌 접근 \(Conflicting Access to In-Out Parameters\)

<!--
A function has long-term write access to all of its in-out parameters. The write access for an in-out parameter starts after all of the non-in-out parameters have been evaluated and lasts for the entire duration of that function call. If there are multiple in-out parameters, the write accesses start in the same order as the parameters appear.

One consequence of this long-term write access is that you can’t access the original variable that was passed as in-out, even if scoping rules and access control would otherwise permit it—any access to the original creates a conflict. For example:
-->

함수는 모든 in-out 파라미터에 장기 쓰기 접근을 가지고 있습니다. in-out 파라미터에 대한 쓰기 접근은 모든 non-in-out 파라미터가 평가된 후에 시작되고 해당 함수 호출되는 동안 지속됩니다. In-out 파라미터가 여러개 인 경우 쓰기 접근은 파라미터가 나타나는 순서와 동일한 순서로 시작됩니다.

이 장기 쓰기 접근의 결과 중 하나는 범위 규칙과 접근 제어가 허용하더라도 기존에 대한 모든 접근은 충돌을 생성하므로 전달된 원래 변수에 접근할 수 없다는 것입니다. 예를 들어:

```swift
var stepSize = 1

func increment(_ number: inout Int) {
    number += stepSize
}

increment(&stepSize)
// Error: conflicting accesses to stepSize
```

<!--
In the code above, stepSize is a global variable, and it’s normally accessible from within increment(_:). However, the read access to stepSize overlaps with the write access to number. As shown in the figure below, both number and stepSize refer to the same location in memory. The read and write accesses refer to the same memory and they overlap, producing a conflict.
-->

위의 코드에서 `stepSize` 는 전역 변수이고 일반적으로 `increment(_:)` 내에서 접근할 수 있습니다. 그러나 `stepSize` 에 읽기 접근은 `number` 에 쓰기 접근과 오버랩 됩니다. 아래 그림에서 보았듯이 `number` 와 `stepSize` 모두 같은 메모리의 위치를 참조합니다. 읽기와 쓰기 접근은 같은 메모리를 참조하고 오버랩되어 충돌이 발생합니다.

![Memory Increment](../.gitbook/assets/25_memory_increment_2x.png)

<!--
One way to solve this conflict is to make an explicit copy of stepSize:
-->

이 충돌을 해결하는 한가지 방법은 `stepSize` 의 복사본을 명확하게 지정하는 것입니다:

```swift
// Make an explicit copy.
var copyOfStepSize = stepSize
increment(&copyOfStepSize)

// Update the original.
stepSize = copyOfStepSize
// stepSize is now 2
```

<!--
When you make a copy of stepSize before calling increment(_:), it’s clear that the value of copyOfStepSize is incremented by the current step size. The read access ends before the write access starts, so there isn’t a conflict.

Another consequence of long-term write access to in-out parameters is that passing a single variable as the argument for multiple in-out parameters of the same function produces a conflict. For example:
-->

`increment(_:)` 호출 전에 `stepSize` 의 복사본을 만들 때 `copyOfStepSize` 의 값이 현재 수만큼 증가된다는 것은 명확합니다. 읽기 접근은 쓰기 접근이 시작되기 전에 끝나므로 충돌이 일어나지 않습니다.

in-out 파라미터에 대한 장기 쓰기 접근의 또 다른 결과는 같은 함수의 여러개의 in-out 파라미터에 대해 인수로 단일 변수를 전달하면 충돌이 발생한다는 것입니다. 예를 들어:

```swift
func balance(_ x: inout Int, _ y: inout Int) {
    let sum = x + y
    x = sum / 2
    y = sum - x
}
var playerOneScore = 42
var playerTwoScore = 30
balance(&playerOneScore, &playerTwoScore)  // OK
balance(&playerOneScore, &playerOneScore)
// Error: conflicting accesses to playerOneScore
```

<!--
The balance(_:_:) function above modifies its two parameters to divide the total value evenly between them. Calling it with playerOneScore and playerTwoScore as arguments doesn’t produce a conflict—there are two write accesses that overlap in time, but they access different locations in memory. In contrast, passing playerOneScore as the value for both parameters produces a conflict because it tries to perform two write accesses to the same location in memory at the same time.
-->

위의 `balance(_:_:)` 함수는 총 값을 균등하게 나누기 위해 두 파라미터를 수정합니다. `playerOneScore` 와 `playerTwoScore` 를 인수로 사용하여 호출하면 두 쓰기 접근은 동시에 오버랩 되지만 메모리의 다른 위치를 접근하므로 충돌이 발생하지 않습니다. 반대로 두 파라미터 모두에 대해 값으로 `playerOneScore` 를 전달하면 동시에 메모리의 같은 위치를 두 쓰기 접근이 수행되므로 충돌이 일어납니다.

<!--
NOTE
Because operators are functions, they can also have long-term accesses to their in-out parameters. For example, if balance(_:_:) was an operator function named <^>, writing playerOneScore <^> playerOneScore would result in the same conflict as balance(&playerOneScore, &playerOneScore).
-->

> NOTE   
> 연산자는 함수이므로 in-out 파라미터에 장기 접근을 할 수도 있습니다. 예를 들어 `balance(_:_:)` 가 `<^>`라는 연산자 함수라면 `playerOneScore <^> playerOneScore` 를 작성하면 `balance(&playerOneScore, &playerOneScore)` 와 동일한 충돌이 발생합니다.

## 메서드에서 self에 충돌 접근 \(Conflicting Access to self in Methods\)

<!--
A mutating method on a structure has write access to self for the duration of the method call. For example, consider a game where each player has a health amount, which decreases when taking damage, and an energy amount, which decreases when using special abilities.
-->

구조체의 변경 메서드는 메서드 호출 동안 `self` 에 대한 쓰기 접근을 가집니다. 예를 들어 각 플레이어는 데미지를 입으면 줄어드는 체력량과 특수 능력을 사용하면 줄어드는 에너지량을 가지는 게임이 있다고 생각해 봅시다.

```swift
struct Player {
    var name: String
    var health: Int
    var energy: Int

    static let maxHealth = 10
    mutating func restoreHealth() {
        health = Player.maxHealth
    }
}
```

<!--
In the restoreHealth() method above, a write access to self starts at the beginning of the method and lasts until the method returns. In this case, there’s no other code inside restoreHealth() that could have an overlapping access to the properties of a Player instance. The shareHealth(with:) method below takes another Player instance as an in-out parameter, creating the possibility of overlapping accesses.
-->

위의 `restoreHealth()` 메서드에서 `self` 에 쓰기 접근은 메서드의 처음에서 시작하고 메서드가 반환될 때까지 유지됩니다. 이 경우에 `restoreHealth()` 내부에 `Player` 인스턴스의 프로퍼티에 중복 접근하는 다른 코드는 없습니다. 아래의 `shareHealth(with:)` 메서드는 in-out 파라미터로 다른 `Player` 인스턴스를 가지고 있으며 중복 접근에 대한 가능성을 만듭니다.

```swift
extension Player {
    mutating func shareHealth(with teammate: inout Player) {
        balance(&teammate.health, &health)
    }
}

var oscar = Player(name: "Oscar", health: 10, energy: 10)
var maria = Player(name: "Maria", health: 5, energy: 10)
oscar.shareHealth(with: &maria)  // OK
```

<!--
In the example above, calling the shareHealth(with:) method for Oscar’s player to share health with Maria’s player doesn’t cause a conflict. There’s a write access to oscar during the method call because oscar is the value of self in a mutating method, and there’s a write access to maria for the same duration because maria was passed as an in-out parameter. As shown in the figure below, they access different locations in memory. Even though the two write accesses overlap in time, they don’t conflict.
-->

위의 예제에서 Maria의 플레이어와 함께 체력을 공유하기 위해 Oscar의 플레이어에 대한 `shareHealth(with:)` 메서드 호출은 중복을 야기 시키지 않습니다. `oscar` 는 변경 함수에서 `self` 의 값이기 때문에 `oscar` 에 대한 쓰기 접근이 있고 `maria` 는 in-out 파라미터로 전달되기 때문에 `maria` 에 대한 쓰기 접근이 있습니다. 아래 그림과 같이 메모리의 다른 위치를 접근합니다. 두 쓰기 접근이 동시에 오버랩 되지만 충돌이 일어나지 않습니다.

![Memory Share Health Maria](../.gitbook/assets/25_memory_share_health_maria_2x.png)

<!--
However, if you pass oscar as the argument to shareHealth(with:), there’s a conflict:
-->

그러나 `shareHealth(with:)` 의 인수로 `oscar` 를 전달하면 충돌이 일어납니다:

```swift
oscar.shareHealth(with: &oscar)
// Error: conflicting accesses to oscar
```

<!--
The mutating method needs write access to self for the duration of the method, and the in-out parameter needs write access to teammate for the same duration. Within the method, both self and teammate refer to the same location in memory—as shown in the figure below. The two write accesses refer to the same memory and they overlap, producing a conflict.
-->

변경 메서드는 `self` 에 대한 쓰기 접근이 필요하고 in-out 파라미터는 `teammate` 에 쓰기 접근이 필요합니다. 메서드 내에서 `self` 와 `teammate` 모두는 아래 그림과 같이 메모리의 같은 위치를 참조합니다. 두 쓰기 접근이 같은 메모리를 참조하고 오버랩 되므로 충돌이 일어납니다.

![Memory Share Health Oscar](../.gitbook/assets/25_memory_share_health_oscar_2x.png)

## 프로퍼티에서 충돌 접근 \(Conflicting Access to Properties\)

<!--
Types like structures, tuples, and enumerations are made up of individual constituent values, such as the properties of a structure or the elements of a tuple. Because these are value types, mutating any piece of the value mutates the whole value, meaning read or write access to one of the properties requires read or write access to the whole value. For example, overlapping write accesses to the elements of a tuple produces a conflict:
-->

구조체, 튜플, 그리고 열거형과 같은 타입은 구조체의 프로퍼티 또는 튜플의 요소와 같은 개별 구성값으로 구성됩니다. 이것은 값 타입이기 때문에 값의 일부분을 변경하면 전체 값이 변경됩니다. 이것은 프로퍼티 중 하나에 읽기 또는 쓰기 접근은 전체 값에 읽기 또는 쓰기 접근을 요구합니다. 예를 들어 튜플의 요소에 쓰기 접근이 겹치면 충돌이 일어납니다:

```swift
var playerInformation = (health: 10, energy: 20)
balance(&playerInformation.health, &playerInformation.energy)
// Error: conflicting access to properties of playerInformation
```

<!--
In the example above, calling balance(_:_:) on the elements of a tuple produces a conflict because there are overlapping write accesses to playerInformation. Both playerInformation.health and playerInformation.energy are passed as in-out parameters, which means balance(_:_:) needs write access to them for the duration of the function call. In both cases, a write access to the tuple element requires a write access to the entire tuple. This means there are two write accesses to playerInformation with durations that overlap, causing a conflict.

The code below shows that the same error appears for overlapping write accesses to the properties of a structure that’s stored in a global variable.
-->

위의 예제에서 튜플의 요소에서 `balance(_:_:)` 를 호출하는 것은 `playerInformation` 에 중복 쓰기 접근이므로 충돌이 일어납니다. `playerInformation.health` 와 `playerInformation.energy` 모두는 `balance(_:_:)` 는 함수를 호출하는 동안 쓰기 접근이 필요하다는 의미의 in-out 파라미터로 전달됩니다. 이 경우 모두 튜플 요소에 쓰기 접근은 모든 튜플에 쓰기 접근을 요구합니다. 이것은 `playerInformation` 에 두 쓰기 접근이 오버랩 되므로 충돌이 일어납니다.

아래의 코드는 전역 변수에 저장된 구조체의 프로퍼티에 쓰기 접근이 중복될 때 동일한 에러가 발생하는 것을 보여줍니다.

```swift
var holly = Player(name: "Holly", health: 10, energy: 10)
balance(&holly.health, &holly.energy)  // Error
```

<!--
In practice, most access to the properties of a structure can overlap safely. For example, if the variable holly in the example above is changed to a local variable instead of a global variable, the compiler can prove that overlapping access to stored properties of the structure is safe:
-->

실제로 대부분의 구조체의 프로퍼티에 접근하는 것은 안전하게 오버랩 될 수 있습니다. 예를 들어 위의 예제에서 변수 `holly` 는 전역 변수가 아닌 지역 변수로 변경되면 컴파일러는 구조체의 저장된 프로퍼티에 중복 접근이 안전하다고 증명할 수 있습니다:

```swift
func someFunction() {
    var oscar = Player(name: "Oscar", health: 10, energy: 10)
    balance(&oscar.health, &oscar.energy)  // OK
}
```

<!--
In the example above, Oscar’s health and energy are passed as the two in-out parameters to balance(_:_:). The compiler can prove that memory safety is preserved because the two stored properties don’t interact in any way.

The restriction against overlapping access to properties of a structure isn’t always necessary to preserve memory safety. Memory safety is the desired guarantee, but exclusive access is a stricter requirement than memory safety—which means some code preserves memory safety, even though it violates exclusive access to memory. Swift allows this memory-safe code if the compiler can prove that the nonexclusive access to memory is still safe. Specifically, it can prove that overlapping access to properties of a structure is safe if the following conditions apply:

* You’re accessing only stored properties of an instance, not computed properties or class properties.
* The structure is the value of a local variable, not a global variable.
* The structure is either not captured by any closures, or it’s captured only by nonescaping closures.

If the compiler can’t prove the access is safe, it doesn’t allow the access.
-->

위의 예제에서 Oscar의 체력와 에너지는 `balance(_:_:)` 에 2개의 in-out 파라미터로 전달됩니다. 컴파일러는 메모리 안정성은 2개의 저장된 프로퍼티는 어떤식으로도 상호작용 하지 않으므로 안전하다고 증명할 수 있습니다.

구조체의 프로퍼티에 중복 접근에 대한 제한은 메모리 안정성을 위해 항상 필요한 것은 아닙니다. 메모리 안정성은 보장되지만 배타적 접근 \(exclusive access\)은 메모리 안정성 보다 더 엄격한 요구사항입니다. 이것은 일부 코드는 메모리에서 배타적 접근을 위반하더라도 메모리 안정성을 유지한다는 의미입니다. Swift는 컴파일러가 메모리에 배타적이지 않은 접근 \(nonexclusive access\)이 여전히 안전하다고 증명할 수 있으면 메모리 안전 코드를 허용합니다. 특히 아래의 조건이 적용되면 구조체의 프로퍼티에 중복 접근은 안전하다고 증명할 수 있습니다:

* 계산된 프로퍼티 또는 클래스 프로퍼티가 아닌 인스턴스의 저장된 프로퍼티만 접근합니다.
* 구조체는 전역 변수가 아닌 지역 변수의 값입니다.
* 구조체는 클로저에 의해 캡쳐되지 않거나 nonescaping 클로저에 의해서만 캡처됩니다.

컴파일러가 접근이 안전하다는 것을 증명할 수 없으면 접근을 허용하지 않습니다.

