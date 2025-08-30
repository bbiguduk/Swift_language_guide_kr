# 메모리 안전성 (Memory Safety)

메모리 접근할 때 충돌을 피하도록 코드를 구조화합니다.

기본적으로 Swift는 코드에서 발생할 안전하지 않은 동작을 방지합니다.
예를 들어
Swift는 변수가 사용되기 전에 초기화 되고,
메모리가 해제된 후에는 접근되지 않으며,
배열 인덱스는 범위를 벗어난 오류가 있는지 확인합니다.

Swift는 또한 동일한 메모리 영역에 대해
다중 접근이 충돌하지 않도록 보장하고,
이것은 메모리의 위치를 수정하는 코드가
해당 메모리에 대한 접근 권한을 갖도록 보장합니다.
Swift는 메모리를 자동으로 관리하기 때문에,
대부분의 경우 메모리 접근에 대해 생각할 필요가 없습니다.
그러나
어디서 충돌이 발생할 수 있는지 이해하는 것이 중요하며,
메모리 접근이 충돌하는 코드를 작성하지 않도록 해야 합니다.
코드에 충돌이 있다면,
컴파일 때나 런타임 때 오류가 발생합니다.

<!--
  TODO: maybe re-introduce this text...

  Memory safety refers to...
  The term *safety* usually refers to :newTerm:`memory safety`...
  Unsafe access to memory is available, if you ask for it explicitly...
-->

## 메모리 접근 충돌 이해 (Understanding Conflicting Access to Memory)

메모리에 접근하는 것은
변수의 값을 설정하거나
함수에 인자를 전달하는 것과 같은 동작을 할 때 코드에서 발생합니다.
예를 들어
다음 코드는 읽기 접근과 쓰기 접근 모두 포함합니다:

```swift
// A write access to the memory where one is stored.
var one = 1

// A read access from the memory where one is stored.
print("We're number \(one)!")
```

<!--
  - test: `memory-read-write`

  ```swifttest
  // A write access to the memory where one is stored.
  -> var one = 1

  // A read access from the memory where one is stored.
  -> print("We're number \(one)!")
  << We're number 1!
  ```
-->

<!--
  Might be worth a different example,
  or else I'm going to keep getting "We are Number One" stuck in my head.
-->

메모리 접근 충돌은
코드의 다른 부분이
같은 시간에 같은 메모리 위치에 접근할 때 발생할 수 있습니다.
같은 시간에 메모리 위치에 다중 접근은
예측할 수 없거나 일관성 없는 동작이 발생할 수 있습니다.
Swift는 코드의 여러 라인에 걸쳐
값을 수정할 수 있으므로,
값이 수정되는 중간에
값에 접근할 수 있습니다.

비슷한 문제로
종이에 쓰여진 예산을
어떻게 업데이트 하는지 생각해 볼 수 있습니다.
예산을 업데이트 하는 것은 2단계로 이루어집니다:
먼저 항목의 이름과 가격을 추가한 다음에
현재 목록에 있는 항목을 반영하기 위해
총 금액을 변경합니다.
업데이트 전후에
아래 그림과 같이
예산에서 정보를 읽고
올바른 답을 얻을 수 있습니다.

![Memory Shopping](../.gitbook/assets/memory_shopping_2x~dark.png)

예산에 항목을 추가하는 동안에는
총 금액이 새로 추가된 항목에 대해
업데이트되지 않았기 때문에
임시적으로 유효하지 않은 상태입니다.
항목을 추가하는 동안
총 금액을 읽으면
잘못된 정보를 얻습니다.

이 예시는 또한
메모리 접근 충돌을 수정할 때
발생할 수 있는 문제를 보여줍니다:
때로는 충돌을 수정하는 방법이 여러 가지일 수 있으며,
각 방법이 서로 다른 결과를 낼 수 있고,
어떤 결과가 올바른지 항상 명확하지 않습니다.
이 예시에서
기존 총 금액이 필요한지,
업데이트된 총 금액이 필요한지에 따라
$5이나 $320이 모두 올바른 답이 될 수 있습니다.
충돌 접근을 수정하기 전에
수행할 작업의 의도를 파악해야 합니다.

> Note: 동시성이나 다중 스레드 코드를 작성해본 적이 있다면,
> 메모리 접근 충돌이 익숙한 문제일 수 있습니다.
> 그러나
> 여기서 설명한 접근 충돌은
> 단일 스레드에서 발생할 수 있으며
> 동시성이나 다중 스레드 코드와는 관련이 *없습니다*.
>
> 단일 스레드 내에서
> 메모리 접근 충돌이 발생하면
> Swift는 컴파일 시나 런타임 시
> 오류가 발생합니다.
> 다중 스레드 코드의 경우
> [Thread Sanitizer](https://developer.apple.com/documentation/xcode/diagnosing_memory_thread_and_crash_issues_early)를
> 사용하여 스레드 간에 충돌하는 접근을 감지할 수 있습니다.

<!--
  TODO: The xref above doesn't seem to give enough information.
  What should I be looking for when I get to the linked page?
-->

### 메모리 접근의 특징 (Characteristics of Memory Access)

메모리 접근이 충돌을 일으킬 수 있는지 판단할 때 고려해야 할
특성은 세 가지입니다:
접근이 읽기 접근인지 쓰기 접근인지,
접근의 지속시간,
접근하는 메모리의 위치입니다.
구체적으로
아래의 조건을 모두 만족하는
두 접근이 있다면 충돌이 발생합니다:

- 두 접근이 모두 읽기 접근이거나, 두 접근이 모두 atomic이 아닙니다.
- 메모리의 같은 위치에 접근합니다.
- 접근하는 시간이 겹칩니다.

읽기 접근과 쓰기 접근의 차이는
일반적으로 명확합니다:
쓰기 접근은 메모리의 위치의 값을 변경하지만,
읽기 접근은 변경하지 않습니다.
메모리의 위치는
예를 들어 변수, 상수, 프로퍼티와 같이
실제로 접근하는 항목을 나타냅니다.
메모리 접근의 시간은
즉시(instantaneous) 발생하거나 장기적(long-term)일 수 있습니다.

접근이 *atomic*이라는 것은
[`Atomic`] 또는 [`AtomicLazyReference`]의 atomic 연산을 호출하는 경우이거나,
C atomic 연산자만 사용하는 경우를 의미합니다;
그렇지 않으면 nonatomic입니다.
C의 atomic 함수 목록은 `stdatomic(3)` 메뉴얼 페이지에서 확인 가능합니다.

[`Atomic`]: https://developer.apple.com/documentation/synchronization/atomic
[`AtomicLazyReference`]: https://developer.apple.com/documentation/synchronization/atomiclazyreference

<!--
  Using the C atomic functions from Swift
  requires some shimming that's out of scope for TSPL - for example:
  https://github.com/apple/swift-se-0282-experimental/tree/master/Sources/_AtomicsShims
-->

접근이 *즉시*인 경우는
접근이 시작되고 종료되기 전에
다른 코드를 실행할 수 없는 경우입니다.
일반적으로 두 개의 즉시 접근은 동시에 발생할 수 없습니다.
대부분 메모리 접근은 즉시 접근입니다.
예를 들어
아래 코드 목록의 모든 읽기 접근과 쓰기 접근은 즉시 접근입니다:

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
  - test: `memory-instantaneous`

  ```swifttest
  -> func oneMore(than number: Int) -> Int {
         return number + 1
     }

  -> var myNumber = 1
  -> myNumber = oneMore(than: myNumber)
  -> print(myNumber)
  <- 2
  ```
-->

그러나
메모리에 접근하는 여러 방법 중에는
다른 코드 실행에 걸쳐있는
*장기(long-term)* 접근도 있습니다.
즉각 접근(instantaneous access)과 장기 접근(long-term access)의 차이점은
장기 접근이 시작되고 종료되기 전에
다른 코드가 실행될 수 있다는 것입니다.
이것을 *겹침(overlap)*이라 합니다.
장기 접근은
다른 장기 접근이나 즉시 접근과 겹칠 수 있습니다.

겹치는 접근은 함수와 메서드의 in-out 매개변수를 사용하거나
구조체의 mutating 메서드를 사용하는
코드에서 주로 나타납니다.
Swift 코드에서 장기 접근이 사용되는 구체적인 경우는
아래에서 설명합니다.

## In-Out 매개변수에 대한 접근 충돌 (Conflicting Access to In-Out Parameters)

함수는 모든 in-out 매개변수에 대해
장기 쓰기 접근을 가지고 있습니다.
in-out 매개변수에 대한 쓰기 접근은
모든 일반 매개변수가 평가된 후에 시작되고,
해당 함수 호출이 끝날 때까지 지속됩니다.
In-out 매개변수가 여러 개인 경우
쓰기 접근은 매개변수가 나타나는 순서와 동일한 순서로 시작됩니다.

이 장기 쓰기 접근의 결과 중 하나는
범위 규칙과 접근 제어가 허용하더라도
기존 변수에 대한 모든 접근은 충돌을 생성하므로
in-out 파리미터로 전달된 원래 변수에
접근할 수 없습니다.
예를 들어:

```swift
var stepSize = 1

func increment(_ number: inout Int) {
    number += stepSize
}

increment(&stepSize)
// Error: conflicting accesses to stepSize
```

<!--
  - test: `memory-increment`

  ```swifttest
  -> var stepSize = 1

  -> func increment(_ number: inout Int) {
         number += stepSize
     }

  -> increment(&stepSize)
  // Error: conflicting accesses to stepSize
  xx Simultaneous accesses to 0x10e8667d8, but modification requires exclusive access.
  xx Previous access (a modification) started at  (0x10e86b032).
  xx Current access (a read) started at:
  ```
-->

위의 코드에서
`stepSize`는 전역 변수이고,
일반적으로 `increment(_:)` 내에서 접근할 수 있습니다.
그러나
`stepSize`에 읽기 접근은
`number`에 쓰기 접근과 겹칩니다.
아래 그림에서 보았듯이
`number`와 `stepSize` 모두 같은 메모리의 위치를 참조합니다.
읽기와 쓰기 접근이
같은 메모리를 참조하면서
충돌이 발생합니다.

![Memory Increment](../.gitbook/assets/memory_increment_2x~dark.png)

이 충돌을 해결하는 한 가지 방법은
`stepSize`의 복사본을 명확하게 지정하는 것입니다:

```swift
// Make an explicit copy.
var copyOfStepSize = stepSize
increment(&copyOfStepSize)

// Update the original.
stepSize = copyOfStepSize
// stepSize is now 2
```

<!--
  - test: `memory-increment-copy`

  ```swifttest
  >> var stepSize = 1
  >> func increment(_ number: inout Int) {
  >>     number += stepSize
  >> }
  // Make an explicit copy.
  -> var copyOfStepSize = stepSize
  -> increment(&copyOfStepSize)

  // Update the original.
  -> stepSize = copyOfStepSize
  /> stepSize is now \(stepSize)
  </ stepSize is now 2
  ```
-->

`increment(_:)` 호출 전에 `stepSize`의 복사본을 만들면,
`copyOfStepSize`는
현재 `stepSize` 값을 기준으로 증가됩니다.
읽기 접근은 쓰기 접근이 시작되기 전에 끝나므로
충돌이 일어나지 않습니다.

in-out 매개변수에 대한
장기 쓰기 접근의 또 다른 결과는
단일 변수를
같은 함수의
여러 in-out 매개변수에 전달하면
충돌이 발생한다는 것입니다.
예를 들어:

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
  - test: `memory-balance`

  ```swifttest
  -> func balance(_ x: inout Int, _ y: inout Int) {
         let sum = x + y
         x = sum / 2
         y = sum - x
     }
  -> var playerOneScore = 42
  -> var playerTwoScore = 30
  -> balance(&playerOneScore, &playerTwoScore)  // OK
  -> balance(&playerOneScore, &playerOneScore)
  // Error: conflicting accesses to playerOneScore
  !$ error: inout arguments are not allowed to alias each other
  !! balance(&playerOneScore, &playerOneScore)
  !!                          ^~~~~~~~~~~~~~~
  !$ note: previous aliasing argument
  !! balance(&playerOneScore, &playerOneScore)
  !!         ^~~~~~~~~~~~~~~
  !$ error: overlapping accesses to 'playerOneScore', but modification requires exclusive access; consider copying to a local variable
  !! balance(&playerOneScore, &playerOneScore)
  !!                          ^~~~~~~~~~~~~~~
  !$ note: conflicting access is here
  !! balance(&playerOneScore, &playerOneScore)
  !!         ^~~~~~~~~~~~~~~
  ```
-->

위의 `balance(_:_:)` 함수는
총 값을 균등하게 나누기 위해
두 매개변수를 수정합니다.
`playerOneScore`와 `playerTwoScore`를 전달하면
두 쓰기 접근은 동시에 실행되지만
메모리의 다른 위치를 접근하므로
충돌이 발생하지 않습니다.
반대로
두 매개변수 모두에 대해 값으로 `playerOneScore`를 전달하면
동시에 메모리의 같은 위치를
두 쓰기 접근이 수행되므로
충돌이 일어납니다.

> Note: 연산자는 함수이므로
> in-out 매개변수에 장기 접근을 할 수 있습니다.
> 예를 들어 `balance(_:_:)`가 `<^>`라는 연산자 함수라면
> `playerOneScore <^> playerOneScore`를 작성하면
> `balance(&playerOneScore, &playerOneScore)`와
> 동일한 충돌이 발생합니다.

## 메서드에서 self에 대한 접근 충돌 (Conflicting Access to self in Methods)

<!--
  This (probably?) applies to all value types,
  but structures are the only place you can observe it.
  Enumerations can have mutating methods
  but you can't mutate their associated values in place,
  and tuples can't have methods.
-->

<!--
  Methods behave like self is passed to the method as inout
  because, under the hood, that's exactly what happens.
-->

구조체의 mutating 메서드는
메서드가 호출되는 동안 `self`에 대한 쓰기 접근을 가집니다.
예를 들어 각 플레이어는 데미지를 입으면 줄어드는 체력량과
특수 능력을 사용하면 줄어드는 에너지량을 가지는
게임이 있다고 생각해 봅시다.

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
  - test: `memory-player-share-with-self`

  ```swifttest
  >> func balance(_ x: inout Int, _ y: inout Int) {
  >>     let sum = x + y
  >>     x = sum / 2
  >>     y = sum - x
  >> }
  -> struct Player {
         var name: String
         var health: Int
         var energy: Int

         static let maxHealth = 10
         mutating func restoreHealth() {
             health = Player.maxHealth
         }
     }
  ```
-->

위의 `restoreHealth()` 메서드에서
`self`에 쓰기 접근은 메서드의 처음에서 시작하고
메서드가 반환될 때까지 유지됩니다.
이 경우에 `restoreHealth()` 내부에
`Player` 인스턴스의 프로퍼티에 중복 접근하는
다른 코드는 없습니다.
아래의 `shareHealth(with:)` 메서드는
in-out 매개변수로 다른 `Player` 인스턴스를 가지고 있으며
중복 접근에 대한 가능성을 만듭니다.

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
  - test: `memory-player-share-with-self`

  ```swifttest
  -> extension Player {
         mutating func shareHealth(with teammate: inout Player) {
             balance(&teammate.health, &health)
         }
     }

  -> var oscar = Player(name: "Oscar", health: 10, energy: 10)
  -> var maria = Player(name: "Maria", health: 5, energy: 10)
  -> oscar.shareHealth(with: &maria)  // OK
  ```
-->

위의 예시에서
Oscar 플레이어의 체력을 Maria 플레이어와 함께 공유하기 위해
`shareHealth(with:)` 메서드 호출은
중복을 야기시키지 않습니다.
`oscar`는 mutating 메서드에서 `self`의 값이기 때문에
해당 메서드 호출 동안
`oscar`에 대한 쓰기 접근이 발생하고,
`maria`는 in-out 매개변수로 전달되기 때문에
`maria`에 대한 쓰기 접근이 발생합니다.
아래 그림과 같이
이 접근은 메모리의 다른 위치를 접근합니다.
두 쓰기 접근이 동시에 겹치지만
충돌은 일어나지 않습니다.

![Memory Share Health Maria](../.gitbook/assets/memory_share_health_maria_2x~dark.png)

그러나
`shareHealth(with:)`에 `oscar`를 전달하면
충돌이 발생합니다:

```swift
oscar.shareHealth(with: &oscar)
// Error: conflicting accesses to oscar
```

<!--
  - test: `memory-player-share-with-self`

  ```swifttest
  -> oscar.shareHealth(with: &oscar)
  // Error: conflicting accesses to oscar
  !$ error: inout arguments are not allowed to alias each other
  !! oscar.shareHealth(with: &oscar)
  !!                         ^~~~~~
  !$ note: previous aliasing argument
  !! oscar.shareHealth(with: &oscar)
  !! ^~~~~
  !$ error: overlapping accesses to 'oscar', but modification requires exclusive access; consider copying to a local variable
  !! oscar.shareHealth(with: &oscar)
  !!                          ^~~~~
  !$ note: conflicting access is here
  !! oscar.shareHealth(with: &oscar)
  !! ^~~~~~
  ```
-->

mutating 메서드는 `self`에 대한
쓰기 접근이 필요하고
동시에 in-out 매개변수는 `teammate`에
쓰기 접근이 필요합니다.
메서드 내에서
`self`와 `teammate` 모두는
아래 그림과 같이
메모리의 같은 위치를 참조합니다.
두 쓰기 접근이
같은 메모리를 참조하므로
충돌이 발생합니다.

![Memory Share Health Oscar](../.gitbook/assets/memory_share_health_oscar_2x~dark.png)

## 프로퍼티에 대한 접근 충돌 (Conflicting Access to Properties)

구조체, 튜플, 열거형과 같은 타입은
구조체의 프로퍼티나 튜플의 요소와 같이
개별 구성 값으로 이루어져 있습니다.
이것은 값 타입이기 때문에 값의 일부분을 변경하면
전체 값이 변경되고,
이것은 프로퍼티 중 하나에 읽기 접근이나 쓰기 접근은
전체 값의 읽기 접근이나 쓰기 접근을 요구합니다.
예를 들어
튜플의 요소에 쓰기 접근이 겹치면
충돌이 발생합니다:

```swift
var playerInformation = (health: 10, energy: 20)
balance(&playerInformation.health, &playerInformation.energy)
// Error: conflicting access to properties of playerInformation
```

<!--
  - test: `memory-tuple`

  ```swifttest
  >> func balance(_ x: inout Int, _ y: inout Int) {
  >>     let sum = x + y
  >>     x = sum / 2
  >>     y = sum - x
  >> }
  -> var playerInformation = (health: 10, energy: 20)
  -> balance(&playerInformation.health, &playerInformation.energy)
  // Error: conflicting access to properties of playerInformation
  xx Simultaneous accesses to 0x10794d848, but modification requires exclusive access.
  xx Previous access (a modification) started at  (0x107952037).
  xx Current access (a modification) started at:
  ```
-->

위의 예시에서
`balance(_:_:)`를 호출할 때 튜플의 요소를 전달하면
`playerInformation`에 중복 쓰기 접근이므로
충돌이 발생합니다.
`playerInformation.health`와 `playerInformation.energy`는
`balance(_:_:)` 함수를 호출할 때
쓰기 접근이 필요하다는 의미의
in-out 매개변수로 전달합니다.
이 경우 튜플 요소의 쓰기 접근은
튜플 전체의 쓰기 접근을 요구합니다.
이것은 `playerInformation`에 두 쓰기 접근이
겹치므로
충돌이 발생합니다.

아래의 코드는 전역 변수에 저장된
구조체의 프로퍼티에
쓰기 접근이 중복될 때
동일한 오류가 발생하는 것을 보여줍니다.

```swift
var holly = Player(name: "Holly", health: 10, energy: 10)
balance(&holly.health, &holly.energy)  // Error
```

<!--
  - test: `memory-share-health-global`

  ```swifttest
  >> struct Player {
  >>     var name: String
  >>     var health: Int
  >>     var energy: Int
  >> }
  >> func balance(_ x: inout Int, _ y: inout Int) {
  >>     let sum = x + y
  >>     x = sum / 2
  >>     y = sum - x
  >> }
  -> var holly = Player(name: "Holly", health: 10, energy: 10)
  -> balance(&holly.health, &holly.energy)  // Error
  xx Simultaneous accesses to 0x10794d848, but modification requires exclusive access.
  xx Previous access (a modification) started at  (0x107952037).
  xx Current access (a modification) started at:
  ```
-->

실제로는
대부분의 구조체 프로퍼티에 접근하는 것은
겹쳐도 안전합니다.
예를 들어
위의 예시에서 변수 `holly`는
전역 변수가 아닌 지역 변수로 변경하면
컴파일러는 구조체의 저장 프로퍼티에
중복 접근이 안전하다고 판단합니다:

```swift
func someFunction() {
    var oscar = Player(name: "Oscar", health: 10, energy: 10)
    balance(&oscar.health, &oscar.energy)  // OK
}
```

<!--
  - test: `memory-share-health-local`

  ```swifttest
  >> struct Player {
  >>     var name: String
  >>     var health: Int
  >>     var energy: Int
  >> }
  >> func balance(_ x: inout Int, _ y: inout Int) {
  >>     let sum = x + y
  >>     x = sum / 2
  >>     y = sum - x
  >> }
  -> func someFunction() {
         var oscar = Player(name: "Oscar", health: 10, energy: 10)
         balance(&oscar.health, &oscar.energy)  // OK
     }
  >> someFunction()
  ```
-->

위의 예시에서
Oscar의 체력과 에너지는
`balance(_:_:)`에 두 개의 in-out 매개변수로 전달합니다.
컴파일러는 두 개의 저장 프로퍼티가 어떤식으로도 상호작용 하지 않으므로
메모리 안정성이 유지된다고 판단할 수 있습니다.

구조체 프로퍼티의 중복 접근에 대한
제한은
메모리 안정성을 위해 항상 필요한 것은 아닙니다.
메모리 안정성을 보장하는 것을 목표로 하지만,
배타적 접근(exclusive access)은 메모리 안정성보다 더 엄격한 요구사항입니다 ---
이것은 어떤 코드는 메모리 안정성은 유지하지만,
배타적 접근을 위반할 수 있다는 의미입니다.
Swift는 컴파일러가 메모리의 배타적이지 않은 접근(nonexclusive access)이 안전하다고 증명할 수 있으면
해당 코드를 허용합니다.
특히
아래의 조건이 적용되면
구조체의 프로퍼티에 중복 접근은 안전하다고 증명할 수 있습니다:

- 인스턴스의 저장 프로퍼티만 접근하고,
  연산 프로퍼티나 클래스 프로퍼티에는 접근하지 않습니다.
- 구조체는 전역 변수가 아닌
  지역 변수의 값입니다.
- 구조체는 클로저에 의해 캡쳐되지 않거나
  nonescaping 클로저에 의해서만 캡처됩니다.

컴파일러가 접근이 안전하다는 것을 증명할 수 없으면
접근을 허용하지 않습니다.

<!--
  Because there's no syntax
  to mutate an enum's associated value in place,
  we can't show that overlapping mutations
  to two different associated values on the same enum
  would violate exclusivity.
  Otherwise, we'd want an example of that
  in this section too --
  it's the moral equivalent of property access.
-->

<!--
  .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. .. ..
-->

<!--
  In Swift 4, the only way to create a long-term read
  is to use implicit pointer conversion
  when passing a value as a nonmutating unsafe pointer parameter,
  as in the example below.
  There's discussion in <rdar://problem/33115142>
  about changing the semantics of nonmutating method calls
  to be long-term reads,
  but it's not clear if/when that change will land.

  ::

      var global = 4

      func foo(_ x: UnsafePointer<Int>){
          global = 7
      }

      foo(&global)
      print(global)

      // Simultaneous accesses to 0x106761618, but modification requires exclusive access.
      // Previous access (a read) started at temp2`main + 87 (0x10675e417).
      // Current access (a modification) started at:
      // 0    libswiftCore.dylib                 0x0000000106ac7b90 swift_beginAccess + 605
      // 1    temp2                              0x000000010675e500 foo(_:) + 39
      // 2    temp2                              0x000000010675e3c0 main + 102
      // 3    libdyld.dylib                      0x00007fff69c75144 start + 1
      // Fatal access conflict detected.
-->

<!--
  TEXT FOR THE FUTURE

  Versions of Swift before Swift 5 ensure memory safety
  by aggressively making a copy of the shared mutable state
  when a conflicting access is possible.
  The copy is no longer shared, preventing the possibility of conflicts.
  However, the copying approach has a negative impact
  on performance and memory usage.
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