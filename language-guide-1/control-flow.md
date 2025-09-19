# 제어 흐름 (Control Flow)

분기, 루프, 이른 종료로 코드를 구성합니다.

Swift는 다양한 제어 흐름(control flow) 구문을 제공합니다.
이것은 여러 번 작업을 수행하는 `while` 루프를 포함하고;
조건에 따라 다르게 실행되는
`if`, `guard`, `switch` 구문과;
코드에서 다른 포인트로 실행 플로우를 전달하는
`break`와 `continue`를 포함합니다.
Swift는 배열, 딕셔너리, 범위, 문자열, 그리고 다른 연속적인 것에 대한
반복을 쉽게 조회하는 `for`-`in` 루프를 제공합니다.
Swift는 또한 현재 범위를 벗어날 때 수행되는
`defer` 구문을 제공합니다.

Swift의 `switch` 구문은 C와 같은 언어들의 구문보다
훨씬 더 강력합니다.
케이스들은 간격 일치, 튜플, 특정 타입으로의 변환을 포함하여
다른 많은 패턴을 비교할 수 있습니다.
`switch` 케이스에서 일치된 값은 케이스 문 안에서 사용할 수 있는
임시의 상수나 변수로 사용이 가능하고
복잡한 일치 조건은 각 케이스에 대해 `where` 절로 표현될 수 있습니다.

## For-In 루프 (For-In Loops)

배열에 아이템, 범위의 숫자, 문자열에 문자와 같은
연속된 것에 대해 `for`-`in` 루프를 사용하여 반복할 수 있습니다.

이 예시는 `for`-`in` 루프를 사용하여 배열의 아이템을 반복합니다:

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("Hello, \(name)!")
}
// Hello, Anna!
// Hello, Alex!
// Hello, Brian!
// Hello, Jack!
```

<!--
  - test: `forLoops`

  ```swifttest
  -> let names = ["Anna", "Alex", "Brian", "Jack"]
  -> for name in names {
        print("Hello, \(name)!")
     }
  </ Hello, Anna!
  </ Hello, Alex!
  </ Hello, Brian!
  </ Hello, Jack!
  ```
-->

딕셔너리의 키-값 쌍 접근을 위해 반복을 사용할 수도 있습니다.
딕셔너리의 각 아이템은 딕셔너리가 반복될 때,
`(key, value)` 튜플로 반환되고
`for`-`in` 루프 본문 내에서 사용하기 위해
`(key, value)` 튜플의 멤버를 명시적으로 이름을 가진 상수로 분해할 수 있습니다.
아래의 예시에서 딕셔너리의 키는 `animalName` 상수로 분해되고,
딕셔너리의 값은 `legCount` 상수로 분해됩니다.

```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}
// cats have 4 legs
// ants have 6 legs
// spiders have 8 legs
```

<!--
  - test: `forLoops`

  ```swifttest
  -> let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
  -> for (animalName, legCount) in numberOfLegs {
        print("\(animalName)s have \(legCount) legs")
     }
  </ cats have 4 legs
  </ ants have 6 legs
  </ spiders have 8 legs
  ```
-->

`Dictionary`는 기본적으로 순서가 없으며,
반복으로 가져올
아이템에 대한 순서를 보장하지 않습니다.
특히
아이템을 딕셔너리에 삽입하는 순서는
아이템이 반복되는 순서를 정의하지 않습니다.
배열과 딕셔너리의 자세한 내용은 [컬렉션 타입 (Collection Types)](./collection-types.md)을 참고바랍니다.

<!--
  TODO: provide some advice on how to iterate over a Dictionary in order
  (perhaps sorted by key), using a predicate or array sort or some kind.
-->

숫자 범위에 대해 `for`-`in` 루프를 사용할 수도 있습니다.
이 예시는 5의 배수 항목을 몇 개 출력합니다:

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

<!--
  - test: `forLoops`

  ```swifttest
  -> for index in 1...5 {
        print("\(index) times 5 is \(index * 5)")
     }
  </ 1 times 5 is 5
  </ 2 times 5 is 10
  </ 3 times 5 is 15
  </ 4 times 5 is 20
  </ 5 times 5 is 25
  ```
-->

이 반복되는 시퀀스는
닫힌 범위 연산자(`...`)를 사용하여 표시되는
`1`부터 `5`까지의 숫자 범위 입니다.
`index`의 값은 범위의 첫 번째 숫자(`1`)로 설정되고,
루프 안의 구문이 실행됩니다.
이 경우 루프는 `index`의 현재값을 5배 하여 출력하는
하나의 구문만 포함합니다.
구문이 실행된 후에,
`index`의 값은 범위의 두 번째 값(`2`)로 업데이트 되고,
`print(_:separator:terminator:)` 함수가 다시 호출됩니다.
이 프로세스는 범위의 끝에 도달할 때까지 계속 됩니다.

위의 예시에서 `index`는 루프의 각 반복이 시작할 때
자동으로 설정되는 상수입니다.
따라서 `index`를 사용하기 전에 선언할 필요가 없습니다.
`let` 선언 키워드가 필요없이
루프 선언에 포함되어 암시적으로 선언됩니다.

시퀀스로부터 각 값이 필요하지 않으면,
변수 이름의 위치에 언더바를 사용하여 값을 무시할 수 있습니다.

```swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")
// Prints "3 to the power of 10 is 59049"
```

<!--
  - test: `forLoops`

  ```swifttest
  -> let base = 3
  -> let power = 10
  -> var answer = 1
  -> for _ in 1...power {
        answer *= base
     }
  -> print("\(base) to the power of \(power) is \(answer)")
  <- 3 to the power of 10 is 59049
  ```
-->

위의 예시는 한 숫자를 다른 숫자의 제곱으로 계산하는 방법을 보여줍니다
(이 경우 `3`의 `10`제곱).
`1`로 시작하고 `10`으로 끝나는 닫힌 범위를 사용하여
시작값 `1`
(즉, `3`의 `0`제곱)에
`3`을 `10`번 곱합니다.
이 계산에서 루프의 각 카운터 값은 불필요 합니다 ---
이 코드는 간단하게 올바른 숫자만큼 실행되기만 하면 됩니다.
루프 변수 위치에 사용된
언더바 문자(`_`)는
각 값을 무시하고
루프의 각 반복동안 현재값에 접근하는 것을 제공하지 않습니다.

어떤 상황에선 양 끝점을 포함하는
닫힌 범위를 사용하지 않을 수 있습니다.
시계 페이스에 매 분마다 눈금을 그리는 것을 생각해 봅시다.
`0`분을 시작으로 `60`개의 눈금을 그려야 합니다.
이럴 경우 반열림 범위 연산자(`..<`)를 사용하여
가장 최소값은 포함하지만 최대값은 포함되지 않게 사용할 수 있습니다.
범위에 대한 자세한 내용은 [범위 연산자 (Range Operators)](./basic-operators.md#범위-연산자-range-operators)를 참고바랍니다.

```swift
let minutes = 60
for tickMark in 0..<minutes {
    // render the tick mark each minute (60 times)
}
```

<!--
  - test: `forLoops`

  ```swifttest
  -> let minutes = 60
  >> var result: [Int] = []
  -> for tickMark in 0..<minutes {
        // render the tick mark each minute (60 times)
  >>    result.append(tickMark)
     }
  >> print(result.first!, result.last!, result.count)
  << 0 59 60
  ```
-->

어떤 사용자는 UI에 더 적은 눈금을 원할 수도 있습니다.
매 `5`분마다 눈금을 그리기 원할 수 있습니다.
원하지 않는 눈금을 건너뛰기 위해 `stride(from:to:by:)` 함수를 사용 하세요.

```swift
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
    // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55)
}
```

<!--
  - test: `forLoops`

  ```swifttest
  -> let minuteInterval = 5
  >> result = []
  -> for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
        // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55)
  >>      result.append(tickMark)
     }
  >> print(result.first!, result.last!, result.count)
  << 0 55 12
  ```
-->

닫힌 범위에서도 `stride(from:through:by:)`를 사용할 수 있습니다:

```swift
let hours = 12
let hourInterval = 3
for tickMark in stride(from: 3, through: hours, by: hourInterval) {
    // render the tick mark every 3 hours (3, 6, 9, 12)
}
```

<!--
  - test: `forLoops`

  ```swifttest
  -> let hours = 12
  -> let hourInterval = 3
  -> for tickMark in stride(from: 3, through: hours, by: hourInterval) {
        // render the tick mark every 3 hours (3, 6, 9, 12)
  >>    print(tickMark)
     }
  << 3
  << 6
  << 9
  << 12
  ```
-->

위의 예시는 범위, 배열, 딕셔너리, 문자열을
조회하기 위해 `for`-`in` 루프를 사용합니다.
그러나 이 구문은 *모든* 컬렉션을 조회할 수 있으며,
[시퀀스 (Sequence)](https://developer.apple.com/documentation/swift/sequence) 프로토콜을 준수하는
사용자가 정의한 클래스나 컬렉션 타입도 포함됩니다.

<!--
  TODO: for (index, object) in enumerate(collection)
  and also for i in indices(collection) { collection[i] }
-->

## While 루프 (While Loops)

`while` 루프는 조건이 `false`가 될 때까지 구문을 수행합니다.
이러한 루프는 첫 번째 반복이 시작되기 전에
반복 횟수를 알 수 없을 때 가장 잘 사용됩니다.
Swift는 두 종류의 `while` 루프를 제공합니다:

- `while`은 루프가 시작할 때마다 조건을 비교합니다.
- `repeat`-`while`은 루프가 끝날 때 마다 조건을 비교합니다.

### While

`while` 루프는 단일 조건을 평가하는 것으로 시작합니다.
조건이 `true`이면,
조건이 `false`가 될 때까지 구문은 반복 됩니다.

다음은 `while` 루프의 기본 형식 입니다:

```swift
while <#condition#> {
   <#statements#>
}
```

이 예시는 *Snakes and Ladders* 게임을 플레이 합니다
(또는 *Chutes and Ladders*):

<!-- Apple Books screenshot begins here. -->

![Snakes and Ladders](../.gitbook/assets/snakesAndLadders_2x~dark.png)

게임의 규칙은 다음과 같습니다:

- 이 보드는 25개의 정사각형을 가지고 있고, 25에 착륙하거나 25를 넘는 것이 목표입니다.
- 플레이어의 시작 정사각형을
  보드의 좌측 하단의 모서리로 "정사각형 0"이라 합니다.
- 각 턴마다 주사위를 굴리고,
  위의 점선 화살표 방향으로 각 숫자만큼 정사각형을 이동합니다.
- 턴이 사다리의 바닥에서 끝나면 사다리를 타고 올라갑니다.
- 턴이 뱀의 머리에서 끝나면 뱀의 꼬리로 내려갑니다.

게임 보드는 `Int` 값의 배열로 표현됩니다.
크기는 `finalSquare`라는 상수를 기반으로 하며
배열을 초기화하고
나중에 예시에서 승리 조건을 확인하는데 사용됩니다.
"정사각형 0"에서 플레이어는 시작하기 때문에
보드는 `Int` 값 0을 포함하여 25가 아닌 26의 크기로 초기화 됩니다.

```swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
```

<!--
  - test: `snakesAndLadders1`

  ```swifttest
  -> let finalSquare = 25
  -> var board = [Int](repeating: 0, count: finalSquare + 1)
  >> assert(board == [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0])
  ```
-->

그런 다음 일부 사각형은 뱀과 사다리에 대해 더 구체적인 값을 갖도록 설정됩니다.
사다리 기반의 정사각형은 보드의 위로 이동하기 때문에 양수를 가지는 반면에,
뱀 머리의 정사각형은 보드의 아래로 이동하기 때문에 음수를 가집니다.

```swift
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
```

<!--
  - test: `snakesAndLadders1`

  ```swifttest
  -> board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
  -> board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
  ```
-->

<!-- Apple Books screenshot ends here. -->

정사각형 3은 정사각형 11로 이동하는 사다리 바닥이 포함됩니다.
이것을 표현하기 위해 `board[03]`은 정수값 `8`
(`3`과 `11`의 차이)과
동등한 `+08`을 대입합니다.
값과 구문을 정렬하기 위해
단항 덧셈 연산자(`+i`)는
단항 빼기 연산자(`-i`)를 명시적으로 사용하고
`10`보다 작은 숫자의 앞에 0을 채웁니다.
(두 스타일 모두 꼭 필요한 것은 아니지만
코드가 깔끔해 집니다.)

```swift
var square = 0
var diceRoll = 0
while square < finalSquare {
    // roll the dice
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    // move by the rolled amount
    square += diceRoll
    if square < board.count {
        // if we're still on the board, move up or down for a snake or a ladder
        square += board[square]
    }
}
print("Game over!")
```

<!--
  - test: `snakesAndLadders1`

  ```swifttest
  -> var square = 0
  -> var diceRoll = 0
  -> while square < finalSquare {
        // roll the dice
        diceRoll += 1
        if diceRoll == 7 { diceRoll = 1 }
  >>    print("diceRoll is \(diceRoll)")
        // move by the rolled amount
        square += diceRoll
  >>    print("after diceRoll, square is \(square)")
        if square < board.count {
           // if we're still on the board, move up or down for a snake or a ladder
           square += board[square]
  >>       print("after snakes or ladders, square is \(square)")
        }
     }
  -> print("Game over!")
  << diceRoll is 1
  << after diceRoll, square is 1
  << after snakes or ladders, square is 1
  << diceRoll is 2
  << after diceRoll, square is 3
  << after snakes or ladders, square is 11
  << diceRoll is 3
  << after diceRoll, square is 14
  << after snakes or ladders, square is 4
  << diceRoll is 4
  << after diceRoll, square is 8
  << after snakes or ladders, square is 8
  << diceRoll is 5
  << after diceRoll, square is 13
  << after snakes or ladders, square is 13
  << diceRoll is 6
  << after diceRoll, square is 19
  << after snakes or ladders, square is 8
  << diceRoll is 1
  << after diceRoll, square is 9
  << after snakes or ladders, square is 18
  << diceRoll is 2
  << after diceRoll, square is 20
  << after snakes or ladders, square is 20
  << diceRoll is 3
  << after diceRoll, square is 23
  << after snakes or ladders, square is 23
  << diceRoll is 4
  << after diceRoll, square is 27
  << Game over!
  ```
-->

위의 예시는 주사위 굴리기에 매우 간단한 접근방식을 사용합니다.
난수를 생성하는 대신에
`diceRoll`은 `0`의 값으로 시작합니다.
매 `while` 루프를 실행할 때 마다
`diceRoll`은 1씩 증가하고
너무 커졌는지 확인합니다.
반환된 값이 `7`과 같을 때마다
주사위 굴림이 너무 커지므로 값을 `1`로 재설정 합니다.
그 결과 `diceRoll` 값은 항상
`1`, `2`, `3`, `4`, `5`, `6`, `1`, `2` 등과 같은 시퀀스 입니다.

주사위를 굴리고 난 후에 플레이어는 `diceRoll` 정사각형으로 앞으로 이동합니다.
주사위 굴림이 플레이어를 정사각형 25를 넘어 이동할 수 있으며,
이럴 경우 게임이 끝납니다.
이 시나리오에 대처하기 위해
`board` 배열의 `count` 프로퍼티가 `square`보다 작은지 확인합니다.
`square`가 유효하다면, `board[square]`에
현재 `square` 값을 저장하고
플레이어를 이동합니다.

> Note: 확인을 수행하지 않으면,
> `board[square]`는 `board` 배열의 범위를 넘는 값을 접근하려 시도할 수 있습니다.
> 이런 경우 런타임 오류가 발생합니다.

현재 `while` 루프의 실행이 끝나면,
루프를 다시 실행해야 되는지 알기 위해 조건을 확인합니다.
플레이어가 정사각형 숫자 `25`에 위치하거나 넘으면,
루프 조건은 `false`가 되고 게임은 종료됩니다.

`while` 루프가 시작될 때
게임의 길이가 명확하지 않으므로 이러한 경우 `while` 루프가 적절합니다.
대신에 루프는 특정 조건이 만족할 때까지 실행됩니다.

### Repeat-While

`while` 루프의 다른 하나는
루프의 조건을 판단하기 *전에*
루프 블록을 처음에 한번 먼저 통과하는
`repeat`-`while` 루프가 있습니다.
조건이 `false`가 될 때까지 루프를 반복합니다.

> Note: Swift에 `repeat`-`while` 루프는
> 다른 언어의 `do`-`while` 루프와 유사합니다.

다음은 `repeat`-`while` 루프의 일반적인 형식입니다:

```swift
repeat {
   <#statements#>
} while <#condition#>
```

*Snakes and Ladders* 예시에서
`while` 루프보다 `repeat`-`while` 루프를 쓰는 것이 더 좋습니다.
`finalSquare`, `board`, `square`, `diceRoll`의 값은
`while` 루프에서와 같은 방식으로 초기화 됩니다.

```swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0
```

<!--
  - test: `snakesAndLadders2`

  ```swifttest
  -> let finalSquare = 25
  -> var board = [Int](repeating: 0, count: finalSquare + 1)
  >> assert(board == [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0])
  -> board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
  -> board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
  -> var square = 0
  -> var diceRoll = 0
  ```
-->

이 게임의 버전에서는
루프에서 *첫 번째* 행동은 사다리인지 뱀인지 확인합니다.
보드에는 플레이러를 바로 정사각형 25로 보내는 사다리가 없으므로,
사다리를 타고 게임에서 이기는 것은 불가능합니다.
따라서 루프의 첫 번째 동작으로 뱀이나 사다리를 확인하는 것이 안전합니다.

게임 시작할 때 플레이어는 "정사각형 0"에 있습니다.
`board[0]`은 항상 `0`과 같으며 영향이 없습니다.

```swift
repeat {
    // move up or down for a snake or ladder
    square += board[square]
    // roll the dice
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    // move by the rolled amount
    square += diceRoll
} while square < finalSquare
print("Game over!")
```

<!--
  - test: `snakesAndLadders2`

  ```swifttest
  -> repeat {
        // move up or down for a snake or ladder
        square += board[square]
  >>      print("after snakes or ladders, square is \(square)")
        // roll the dice
        diceRoll += 1
        if diceRoll == 7 { diceRoll = 1 }
  >>    print("diceRoll is \(diceRoll)")
        // move by the rolled amount
        square += diceRoll
  >>    print("after diceRoll, square is \(square)")
  -> } while square < finalSquare
  -> print("Game over!")
  << after snakes or ladders, square is 0
  << diceRoll is 1
  << after diceRoll, square is 1
  << after snakes or ladders, square is 1
  << diceRoll is 2
  << after diceRoll, square is 3
  << after snakes or ladders, square is 11
  << diceRoll is 3
  << after diceRoll, square is 14
  << after snakes or ladders, square is 4
  << diceRoll is 4
  << after diceRoll, square is 8
  << after snakes or ladders, square is 8
  << diceRoll is 5
  << after diceRoll, square is 13
  << after snakes or ladders, square is 13
  << diceRoll is 6
  << after diceRoll, square is 19
  << after snakes or ladders, square is 8
  << diceRoll is 1
  << after diceRoll, square is 9
  << after snakes or ladders, square is 18
  << diceRoll is 2
  << after diceRoll, square is 20
  << after snakes or ladders, square is 20
  << diceRoll is 3
  << after diceRoll, square is 23
  << after snakes or ladders, square is 23
  << diceRoll is 4
  << after diceRoll, square is 27
  << Game over!
  ```
-->

뱀과 사다리를 코드는 확인 후에
주사위를 굴리고 플레이어를 `diceRoll` 정사각형에 따라 앞으로 이동합니다.
그러면 현재 루프는 종료됩니다.

루프의 조건(`while square < finalSquare`)은 이전과 같지만,
이번에는 루프가 처음 실행된 후 *끝*에서 평가됩니다.
이전 예시의 `while` 루프보다
이 게임에서는 `repeat`-`while` 루프가 더 적합합니다.
위의 `repeat`-`while` 루프에서
`square += board[square]`는 루프의 `while` 조건이 `square` 가 항상 보드 위에 있는 것을
확인 한 *직후*에 실행 됩니다.
이 동작은 이전 예시에서 `while` 루프 버전에서
배열의 범위를 체크할 필요가 없습니다.

## 조건 구문 (Conditional Statements)

특정 조건에 따라 다양한 코드를 실행하는 것은 유용한 경우가 있습니다.
오류가 발생했을 때 추가 코드를 실행하거나,
값이 너무 크거나 작을 때 메세지를 출력하려고 싶을 수 있습니다.
이러한 동작을 위해 코드의 일부를 조건부로 만들어야 합니다.

Swift는 코드에 조건부를 추가하는 두 가지 방법이 있습니다:
`if` 구문과 `switch` 구문이 있습니다.
일반적으로 `if` 구문은
몇 가지 가능한 결과만 있는 단순한 조건을 평가할 때 적합합니다.
`switch` 구문은
여러 가지 가능한 조합을 가진 더 복잡한 조건에 적합하고,
패턴 일치를 통해
적절한 코드 분기를 선택하는데 유용합니다.

### If

가장 간단한 형식으로
`if` 구문은 단일 `if` 조건을 갖습니다.
조건이 `true`일 경우에만 구문을 실행합니다.

```swift
var temperatureInFahrenheit = 30
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
}
// Prints "It's very cold. Consider wearing a scarf."
```

<!--
  - test: `ifElse`

  ```swifttest
  -> var temperatureInFahrenheit = 30
  -> if temperatureInFahrenheit <= 32 {
        print("It's very cold. Consider wearing a scarf.")
     }
  <- It's very cold. Consider wearing a scarf.
  ```
-->

위 예시는 기온이
화씨 32도 보다 작거나 같은지 확인합니다
(물이 어는 온도 입니다).
만약 화씨 32도 보다 작거나 같으면 메세지가 출력됩니다.
그렇지 않으면 메세지가 출력되지 않으며,
`if` 구문의 닫힘 중괄호 이후에 코드 실행이 계속됩니다.

`if` 구문은 `if` 조건이 `false`일 때,
*그 밖의 다른 구문*을
제공할 수 있습니다.
이 구문은 `else` 키워드로 표기합니다.

```swift
temperatureInFahrenheit = 40
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else {
    print("It's not that cold. Wear a T-shirt.")
}
// Prints "It's not that cold. Wear a T-shirt."
```

<!--
  - test: `ifElse`

  ```swifttest
  -> temperatureInFahrenheit = 40
  -> if temperatureInFahrenheit <= 32 {
        print("It's very cold. Consider wearing a scarf.")
     } else {
        print("It's not that cold. Wear a T-shirt.")
     }
  <- It's not that cold. Wear a T-shirt.
  ```
-->

두 중괄호 중 하나는 항상 실행됩니다.
기온이 증가하여 화씨 `40`도를 가지므로,
더 이상 추워서 스카프를 해야 한다고 충고하지 않으며
`else` 구문이 실행 됩니다.

추가 절을 고려하기 위해
여러 `if` 구문을 연결할 수 있습니다.

```swift
temperatureInFahrenheit = 90
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
} else {
    print("It's not that cold. Wear a T-shirt.")
}
// Prints "It's really warm. Don't forget to wear sunscreen."
```

<!--
  - test: `ifElse`

  ```swifttest
  -> temperatureInFahrenheit = 90
  -> if temperatureInFahrenheit <= 32 {
        print("It's very cold. Consider wearing a scarf.")
     } else if temperatureInFahrenheit >= 86 {
        print("It's really warm. Don't forget to wear sunscreen.")
     } else {
        print("It's not that cold. Wear a T-shirt.")
     }
  <- It's really warm. Don't forget to wear sunscreen.
  ```
-->

여기서 `if` 구문은 특정 따뜻한 기온에 대한 조건이 추가 되었습니다.
마지막 `else` 절은 남아있고
어떤 기온이 너무 따뜻하거나 너무 춥지 않을 경우에 응답을 출력합니다.

그러나 마지막 `else` 절은 옵셔널이고
이 조건이 완벽하게 필요가 없으면 제외할 수 있습니다.

```swift
temperatureInFahrenheit = 72
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
}
```

<!--
  - test: `ifElse`

  ```swifttest
  -> temperatureInFahrenheit = 72
  -> if temperatureInFahrenheit <= 32 {
        print("It's very cold. Consider wearing a scarf.")
     } else if temperatureInFahrenheit >= 86 {
        print("It's really warm. Don't forget to wear sunscreen.")
     }
  ```
-->

기온이 너무 춥지도 따뜻하지도 않아 `if`나
`else if` 조건에 포함되지 않으므로
아무런 메세지가 출력되지 않습니다.

Swift는 값을 설정할 때 사용할 수 있는
`if`의 축약형을 제공합니다.
예를 들어,
아래 코드와 같습니다:

```swift
let temperatureInCelsius = 25
let weatherAdvice: String

if temperatureInCelsius <= 0 {
    weatherAdvice = "It's very cold. Consider wearing a scarf."
} else if temperatureInCelsius >= 30 {
    weatherAdvice = "It's really warm. Don't forget to wear sunscreen."
} else {
    weatherAdvice = "It's not that cold. Wear a T-shirt."
}

print(weatherAdvice)
// Prints "It's not that cold. Wear a T-shirt."
```

여기서, 각 분기는 `if` 구문 이후에 출력되는
`weatherAdvice` 상수에 값을 설정합니다.

`if` 표현식을
다른 표현을 사용하여
더 간결하게 코드를 작성할 수 있습니다:

```swift
let weatherAdvice = if temperatureInCelsius <= 0 {
    "It's very cold. Consider wearing a scarf."
} else if temperatureInCelsius >= 30 {
    "It's really warm. Don't forget to wear sunscreen."
} else {
    "It's not that cold. Wear a T-shirt."
}

print(weatherAdvice)
// Prints "It's not that cold. Wear a T-shirt."
```

이 `if` 표현식에서
각 분기는 하나의 값을 포함합니다.
분기의 조건이 참이면,
값은 `weatherAdvice`에 할당되고
`if` 표현식 전체에서 사용됩니다.
모든 `if` 분기는 `else if` 분기나 `else` 분기를 가지고 있어,
분기 중 하나에 항상 일치하고
`if` 표현식이 어떤 조건이 참인지 상관없이
값을 항상 생성합니다.

할당 구문은 `if` 표현식 바깥에서 시작하므로,
각 분기 안에 `weatherAdvice =`를 반복할 필요가 없습니다.
대신에
`if` 표현식의 각 분기는
`weatherAdvice`에 대한 세 가지 가능한 값 중 하나를 생성하고
할당합니다.

`if` 표현식의 모든 분기는
동일한 타입의 값을 포함해야 합니다.
Swift는 각 분기의 타입을 개별적으로 확인하기 때문에
여러 타입과 사용할 수 있는 `nil`과 같은 값은
Swift가 `if` 표현식의 타입을 자동으로 결정하는 것을 방지합니다.
대신에 타입을 명시적으로 지정해야 합니다 ---
예를 들어:

```swift
let freezeWarning: String? = if temperatureInCelsius <= 0 {
    "It's below freezing. Watch for ice!"
} else {
    nil
}
```

위의 코드에서
`if` 표현식 중 하나의 분기는 문자열 값을 가지고
다른 분기는 `nil` 값을 가집니다.
`nil` 값은 모든 옵셔널 타입의 값으로 사용될 수 있으므로,
[타입 명시 (Type Annotations)](./the-basics.md#타입-명시-type-annotations)에서 설명했듯이
`freezeWarning`은 옵셔널 문자열이라고 명시적으로 작성되어야 합니다.

타입 정보를 제공하기 위한 다른 방법은
`freezeWarning`에 대한 타입을 명시적으로 제공하는 대신에
`nil`에 대한 타입을 명시적으로 제공하는 것입니다:

```swift
let freezeWarning = if temperatureInCelsius <= 0 {
    "It's below freezing. Watch for ice!"
} else {
    nil as String?
}
```

`if` 표현식은 오류를 발생시키거나 반환하지 않는 `fatalError(_:file:line:)`와 같은 함수를
호출하여 예기치 않은 실패에 대응할 수 있습니다.
예를 들어:

```swift
let weatherAdvice = if temperatureInCelsius > 100 {
    throw TemperatureError.boiling
} else {
    "It's a reasonable temperature."
}
```

이 예시에서
`if` 표현식은 예보된 온도가
100℃ --- 물의 끓는점보다 높은지 확인합니다.
이렇게 높은 온도에서는 텍스트의 요약을 반환하는 대신에
`.boiling` 오류를 발생시킵니다.
`if` 표현식은 오류를 발생할 수 있지만,
`try`를 전에 작성하지 않습니다.
오류에 대한 자세한 내용은 [오류 처리 (Error Handling)](./error-handling.md)을 참고바랍니다.

위의 예시에서와 같이
할당의 오른편으로
`if` 표현식을 사용하는 것 외에
함수나 클로저가 반환하는 값으로도 사용할 수 있습니다.

### Switch

`switch` 구문은 값을 고려하고
가능한 여러 개의 일치하는 패턴과 비교합니다.
그런 다음 첫 번째로 일치하는 패턴을 기반으로
적절한 코드 블록을 실행합니다.
`switch` 구문은 여러 가능한 상태에 응답하기 위해
`if` 구문의 대체 구문으로 제공합니다.

가장 간단한 형식으로 `switch` 구문은
하나 이상의 같은 타입의 값과 비교합니다.

```swift
switch <#some value to consider#> {
case <#value 1#>:
    <#respond to value 1#>
case <#value 2#>,
    <#value 3#>:
    <#respond to value 2 or 3#>
default:
    <#otherwise, do something else#>
}
```

모든 `switch` 구문은 각각 `case` 키워드로 시작하는
여러 개의 *케이스*로 구성되어 있습니다.
특정 값과 비교하는 것 외에도
Swift는 각 케이스에 대해 더 복잡한 일치 패턴을 지정하는
여러 가지 방법을 제공합니다.
이 옵션은 이 챕터 뒤에서 다루겠습니다.

`if` 구문의 본문과 같이, 각 `case`는 코드 실행 부분이 분리되어 있습니다.
`switch` 구문은 실행될 부분을 선택합니다.
이 절차를 값의 *스위칭(switching)*이라 알려져 있습니다.

모든 `switch` 구문은 *완벽*해야 합니다.
이것은 고려 중인 타입의 가능한 모든 값은
`switch` 케이스 중 하나와 일치해야 합니다.
가능한 모든 값에 대한 케이스를 제공하는 것이 적절하지 않은 경우,
명시적으로 해결되지 않은 모든 값을 포함하도록 기본 케이스를 정의할 수 있습니다.
기본 케이스는 `default` 키워드로 나타내고
항상 마지막에 위치합니다.

이 예시는 `switch` 구문을 사용하여
`someCharacter`의 소문자를 고려합니다:

```swift
let someCharacter: Character = "z"
switch someCharacter {
case "a":
    print("The first letter of the Latin alphabet")
case "z":
    print("The last letter of the Latin alphabet")
default:
    print("Some other character")
}
// Prints "The last letter of the Latin alphabet"
```

<!--
  - test: `switch`

  ```swifttest
  -> let someCharacter: Character = "z"
  -> switch someCharacter {
        case "a":
           print("The first letter of the Latin alphabet")
        case "z":
           print("The last letter of the Latin alphabet")
        default:
           print("Some other character")
     }
  <- The last letter of the Latin alphabet
  ```
-->

`switch` 구문의 첫번째 케이스는
영어 알파벳의 첫 번째 문자인 `a`와 일치하고,
두 번째 케이스는 마지막 문자인 `z`와 일치합니다.
`switch`는 모든 알파벳 문자 뿐만 아니라
모든 가능한 문자에 대한 케이스를 가지고 있어야 하므로,
이 `switch` 구문은 `a`와 `z`을 제외한 다른 모든 문자는
`default` 케이스를 사용합니다.
이렇게 함으로 `switch` 구문은 완벽하다는 것을 보장합니다.

`if` 구문과 같이,
`switch` 구문도 아래와 같은 형식을 가지고 있습니다:

```swift
let anotherCharacter: Character = "a"
let message = switch anotherCharacter {
case "a":
    "The first letter of the Latin alphabet"
case "z":
    "The last letter of the Latin alphabet"
default:
    "Some other character"
}

print(message)
// Prints "The first letter of the Latin alphabet"
```

이 예시에서
`switch` 표현식에 각 케이스는
`anotherCharacter`와 동일한 케이스일 때 사용될
`message`에 대한 값을 포함합니다.
`switch`는 항상 동일한 케이스가 존해하기 때문에,
값은 항상 할당됩니다.

`if` 표현식과 마찬가지로,
주어진 케이스에 대해 값을 제공하는 대신에
오류를 발생시키거나
반환되지 않는 `fatalError(_:file:line:)`와 같은 함수를 호출할 수 있습니다.
위의 예시에서와 같이
할당의 오른편에 있는
`switch` 표현식을
함수나 클로저가 반환하는 값으로 사용할 수 있습니다. 

#### 명시적 Fallthrough (No Implicit Fallthrough)

C와 Objective-C의 `switch` 구문과 다르게,
Swift의 `switch` 구문은
기본적으로 각 케이스의 바닥에서 다음 케이스로 바로 실행되지 않습니다.
명시적으로 `break` 구문 요청 없이
처음 일치하는 `switch` 케이스가 완료되자마자
`switch` 구문 전체가 끝납니다.
이러한 점은 `switch` 구문을 더 안전하고 C의 `switch` 구문보다 사용하기 쉽게 해주고
실수로 `switch` 케이스가 하나 이상 실행되는 것을 피할 수 있습니다.

> Note: Swift에서 `break`는 요구되지 않지만,
> 특정 케이스를 무시하거나 일치하는 케이스의 실행이 완료되기 전에
> 빠져나와야할 경우 `break` 구문을 사용할 수 있습니다.
> 자세한 내용은 [Switch 구문에서 중단 (Break in a Switch Statement)](#switch-구문에서-중단-break-in-a-switch-statement)을 참고바랍니다.

각 케이스의 본문은 *반드시* 적어도 하나의 실행가능한 구문이 포함되어야 합니다.
아래의 코드는 첫번째 케이스가 비어 있으므로 유효하지 않습니다:

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a": // Invalid, the case has an empty body
case "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// This will report a compile-time error.
```

<!--
  - test: `noFallthrough`

  ```swifttest
  -> let anotherCharacter: Character = "a"
  -> switch anotherCharacter {
        case "a": // Invalid, the case has an empty body
        case "A":
           print("The letter A")
        default:
           print("Not the letter A")
     }
  !$ error: 'case' label in a 'switch' must have at least one executable statement
  !!      case "a": // Invalid, the case has an empty body
  !!      ^~~~~~~~~
  !!                break
  // This will report a compile-time error.
  ```
-->

C의 `switch` 구문과 다르게,
이 `switch` 구문은 `"a"`와 `"A"` 둘 다 일치하지 않습니다.
`case "a":`에 어떠한 실행가능한 구문이 없기 때문에
컴파일 때 오류가 발생합니다.
이 접근방식은 한 케이스에서 다른 케이스로의 우발적인 실행을 방지하고
의도를 더 명확하게 하고 안전한 코드를 만듭니다.

`"a"`와 `"A"` 모두 일치하는
단일 케이스의 `switch`를 만드려면
두 값을 콤마로 구분하여
하나로 결합하여 구성합니다.

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a", "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// Prints "The letter A"
```

<!--
  - test: `compoundCaseInsteadOfFallthrough`

  ```swifttest
  -> let anotherCharacter: Character = "a"
  -> switch anotherCharacter {
        case "a", "A":
           print("The letter A")
        default:
           print("Not the letter A")
     }
  <- The letter A
  ```
-->

읽기 쉽게 하기위해
합성 케이스는 여러줄로 작성할 수도 있습니다.
합성 케이스에 대한 자세한 내용은
[합성 케이스 (Compound Cases)](#합성-케이스-compound-cases)를 참고바랍니다.

> Note: 특정 `switch` 케이스 끝에서 명시적으로 다음 케이스로 떨어뜨리려면
> [Fallthrough](#fallthrough)에서 설명한대로
> `fallthrough` 키워드를 사용합니다.

#### 범위 일치 (Interval Matching)

`switch` 케이스에서 값이 특정 범위에 포함되는지 확인할 수 있습니다.
이 예시에서는 숫자 범위를 사용하여
모든 크기의 숫자에도 자연스러운 언어 표현을 제공합니다:

<!--
  REFERENCE
  Saturn has 62 moons with confirmed orbits.
-->

```swift
let approximateCount = 62
let countedThings = "moons orbiting Saturn"
let naturalCount: String
switch approximateCount {
case 0:
    naturalCount = "no"
case 1..<5:
    naturalCount = "a few"
case 5..<12:
    naturalCount = "several"
case 12..<100:
    naturalCount = "dozens of"
case 100..<1000:
    naturalCount = "hundreds of"
default:
    naturalCount = "many"
}
print("There are \(naturalCount) \(countedThings).")
// Prints "There are dozens of moons orbiting Saturn."
```

<!--
  - test: `intervalMatching`

  ```swifttest
  -> let approximateCount = 62
  -> let countedThings = "moons orbiting Saturn"
  -> let naturalCount: String
  -> switch approximateCount {
     case 0:
         naturalCount = "no"
     case 1..<5:
         naturalCount = "a few"
     case 5..<12:
         naturalCount = "several"
     case 12..<100:
         naturalCount = "dozens of"
     case 100..<1000:
         naturalCount = "hundreds of"
     default:
         naturalCount = "many"
     }
  -> print("There are \(naturalCount) \(countedThings).")
  <- There are dozens of moons orbiting Saturn.
  ```
-->

위의 예시에서 `approximateCount`는 `switch` 구문에서 평가됩니다.
각 `case`는 숫자 값이나 범위를 비교합니다.
`approximateCount`의 값은 12와 100 사이에 속하므로,
`naturalCount`는 `"dozens of"` 값이 할당되고,
`switch` 구문을 빠져나옵니다.

#### 튜플 (Tuples)

같은 `switch` 구문에 여러 값인 튜플을 사용할 수 있습니다.
튜플의 각 요소는 다른 값이나 값의 범위를 판단할 수 있습니다.
가능한 어떠한 값도 일치하도록
와일드카드 패턴(wildcard pattern)으로 알려진
언더바 문자(`_`)를 사용할 수 있습니다.

아래 예시는 타입 `(Int, Int)`의 단순한 튜플로 표현된
(x, y) 좌표를 받아서,
예시 다음에 나오는 그래프에서 해당 좌료를 분류합니다.

```swift
let somePoint = (1, 1)
switch somePoint {
case (0, 0):
    print("\(somePoint) is at the origin")
case (_, 0):
    print("\(somePoint) is on the x-axis")
case (0, _):
    print("\(somePoint) is on the y-axis")
case (-2...2, -2...2):
    print("\(somePoint) is inside the box")
default:
    print("\(somePoint) is outside of the box")
}
// Prints "(1, 1) is inside the box"
```

<!--
  - test: `tuples`

  ```swifttest
  -> let somePoint = (1, 1)
  -> switch somePoint {
        case (0, 0):
           print("\(somePoint) is at the origin")
        case (_, 0):
           print("\(somePoint) is on the x-axis")
        case (0, _):
           print("\(somePoint) is on the y-axis")
        case (-2...2, -2...2):
           print("\(somePoint) is inside the box")
        default:
           print("\(somePoint) is outside of the box")
     }
  <- (1, 1) is inside the box
  ```
-->

![Coordinate Graph Simple](../.gitbook/assets/coordinateGraphSimple_2x~dark.png)

`switch` 구문은 해당 점이
원점(0, 0)에 있는지,
빨간색 x축 위에 있는지,
초록색 y축 위에 있는지,
원점을 중심으로 하는 파란색 4 x 4 박스 내부에 있는지
외부에 있는지 판단합니다.

C와 다르게 Swift는 여러 `switch` 케이스가 같은 값인지 고려할 수 있습니다.
사실 좌표(0, 0)는 이 예시의 모든 4개의 케이스에 일치합니다.
그러나 여러 개가 일치할 수 있다면,
첫 번째 일치하는 케이스가 항상 사용됩니다.
좌표(0, 0)는 첫번째 `case (0, 0)`와 일치하고,
다른 모든 케이스는 무시됩니다.

#### 값 바인딩 (Value Bindings)

`switch` 케이스는 일치하는 값을 임시적으로 상수나 변수로 이름을 가질 수 있으며,
케이스 본문 안에서 사용할 수 있습니다.
값은 케이스의 본문 내부에서 임시적 상수나 변수로 바인드 되기 때문에
이러한 동작을 *값 바인딩(value binding)*이라 합니다.

아래 예시는 튜플의 타입 `(Int, Int)`로 표현된
좌표 (x, y)를 받아서,
이어지는 그래프에서 해당 좌표를 분류합니다:

```swift
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}
// Prints "on the x-axis with an x value of 2"
```

<!--
  - test: `valueBindings`

  ```swifttest
  -> let anotherPoint = (2, 0)
  -> switch anotherPoint {
        case (let x, 0):
           print("on the x-axis with an x value of \(x)")
        case (0, let y):
           print("on the y-axis with a y value of \(y)")
        case let (x, y):
           print("somewhere else at (\(x), \(y))")
     }
  <- on the x-axis with an x value of 2
  ```
-->

![Coordinate Graph Medium](../.gitbook/assets/coordinateGraphMedium_2x~dark.png)

이 `switch` 구문은 좌표가
빨간색인 x축에 있는지,
초록색인 y축에 있는지,
또는 다른 곳(축 위가 아닌)에 있는지 판단합니다.

이 세 개의 `switch` 케이스는 자리 상수 `x`와 `y`를 선언하며,
`anotherPoint`에서 임시적으로 하나나 두 개의 튜플 값을 받습니다.
첫 번째 케이스인 `case (let x, 0)`는
`y` 좌표 값이 `0`이고
`x` 좌표 값은 임시적 상수 `x`에 할당한 것과 일치합니다.
비슷하게 두 번째 케이스 `case (0, let y)`는
`x` 좌표 값이 `0`이고
`y` 좌표 값은 임시적 상수 `y`에 할당한 것과 일치합니다.

임시적 상수가 선언된 후에
케이스의 코드 블록 안에서 사용될 수 있습니다.
여기서는 좌표를 분리해 출력합니다.

이 `switch` 구문은 `default` 케이스를 가지고 있지 않습니다.
마지막 케이스 `case let (x, y)`는
어떠한 값도 일치할 수 있는 두 개의 상수를 가지는 튜플로 선언합니다.
`anotherPoint`는 두 값의 튜플이기 때문에
가능한 남아있는 모든 케이스와 일치하며,
`switch` 구문을 완벽하게 하기위해 `default` 케이스가 필요치 않습니다.

#### Where

`switch` 케이스는 추가 조건으로 `where` 절을 사용할 수 있습니다.

아래 예시는 그래프에 (x, y)를 분류합니다:

```swift
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}
// Prints "(1, -1) is on the line x == -y"
```

<!--
  - test: `where`

  ```swifttest
  -> let yetAnotherPoint = (1, -1)
  -> switch yetAnotherPoint {
        case let (x, y) where x == y:
           print("(\(x), \(y)) is on the line x == y")
        case let (x, y) where x == -y:
           print("(\(x), \(y)) is on the line x == -y")
        case let (x, y):
           print("(\(x), \(y)) is just some arbitrary point")
     }
  <- (1, -1) is on the line x == -y
  ```
-->

![Coordinate Graph Complex](../.gitbook/assets/coordinateGraphComplex_2x~dark.png)

`switch` 구문은
`x == y`인 초록색 대각선 위에 있는지,
`x == -y`인 보라색 대각선 위에 있거나
아니면 그 외에 위치하는지 판단합니다.

세 개의 `switch` 케이스는 자리 표시자 상수 `x`와 `y`를 선언하며,
이들은 `yetAnotherPoint` 튜플의 두 값을 임시로 저장합니다.
이 상수는 `where` 절의 일부로 사용되어
동적인 필터를 생성합니다.
`switch` 케이스는 `where` 절의 조건이
`true`일 때만 현재 `point` 값을 일치시킵니다.

이전 예시와 마찬가지로 마지막 케이스는 가능한 남아있는 값과 모두 일치하므로,
`switch` 구문을 완벽하게 하기 위해 `default` 케이스가 필요치 않습니다.

#### 합성 케이스 (Compound Cases)

같은 본문을 공유하는 여러 개의 스위치 케이스는
`case` 다음에 각 패턴 사이에 콤마로 구분하여
여러 개의 패턴을 결합할 수 있습니다.
여러 패턴 중 어떤 패턴이 일치하면 해당 케이스로 고려됩니다.
패턴이 길면 여러줄로 작성할 수 있습니다.
예를 들어:

```swift
let someCharacter: Character = "e"
switch someCharacter {
case "a", "e", "i", "o", "u":
    print("\(someCharacter) is a vowel")
case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
     "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
    print("\(someCharacter) is a consonant")
default:
    print("\(someCharacter) is not a vowel or a consonant")
}
// Prints "e is a vowel"
```

<!--
  - test: `compound-switch-case`

  ```swifttest
  -> let someCharacter: Character = "e"
  -> switch someCharacter {
         case "a", "e", "i", "o", "u":
             print("\(someCharacter) is a vowel")
         case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
             "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
             print("\(someCharacter) is a consonant")
         default:
             print("\(someCharacter) isn't a vowel or a consonant")
     }
  <- e is a vowel
  ```
-->

`switch` 구문의 첫 번째 케이스는
영어의 다섯 개의 소문자 모음이 일치합니다.
비슷하게 두 번째 케이스는 모든 영어 소문자 자음이 일치합니다.
마지막으로 `default` 케이스는 다른 어떠한 문자와 일치합니다.

혼합 케이스(Compound cases)는 값 바인딩을 포함할 수도 있습니다.
혼합 케이스의 모든 패턴은
값 바인딩의 같은 집합을 포함해야 하고
각 바인딩은 혼합 케이스에 모든 패턴으로 부터
같은 타입의 값을 얻어야 합니다.
혼합 케이스 부분이 일치하는 것과 상관없이
케이스의 본문에 코드는
바인딩을 위해 값에 항상 접근할 수 있고
값은 항상 같은 타입을
가집니다.

```swift
let stillAnotherPoint = (9, 0)
switch stillAnotherPoint {
case (let distance, 0), (0, let distance):
    print("On an axis, \(distance) from the origin")
default:
    print("Not on an axis")
}
// Prints "On an axis, 9 from the origin"
```

<!--
  - test: `compound-switch-case`

  ```swifttest
  -> let stillAnotherPoint = (9, 0)
  -> switch stillAnotherPoint {
         case (let distance, 0), (0, let distance):
             print("On an axis, \(distance) from the origin")
         default:
             print("Not on an axis")
     }
  <- On an axis, 9 from the origin
  ```
-->

위의 `case`는 두 개의 패턴을 가지고 있습니다:
`(let distance, 0)`은 x축 위의 좌표와 일치하고
`(0, let distance)`는 y축 위의 좌표와 일치합니다.
두 패턴 모두 `distance` 바인딩을 포함하고
`distance`는 두 패턴 모두 정수입니다 ---
즉, `case`의 본문 안에 코드는
항상 `distance`로 값에 접근할 수 있다는 의미입니다.

## 패턴 (Patterns)

이전 예시에서 각 스위치 케이스는
어떤 값이 해당 케이스에 일치하는지 나타내는 패턴을 포함합니다.
`if` 구문의 조건으로도 패턴을 사용할 수 있습니다.
다음은 그 예시입니다:

```swift
let somePoint = (12, 100)
if case (let x, 100) = somePoint {
    print("Found a point on the y=100 line, at \(x)")
}
// Prints "Found a point on the y=100 line, at 12"
```

이 코드에서
`if` 구문의 조건은 `case`로 시작하고
조건이 Boolean 값 대신 패턴으로 나타냅니다.
패턴이 일치하면,
`if` 구문의 조건은 참이라고 여겨지며,
`if` 구문의 본문 코드를 실행합니다.
`if case` 다음에 작성할 수 있는 패턴은
스위치 케이스에 작성할 수 있는 패턴과 동일합니다.

`for`-`in` 루프에서는
`case`를 작성하지 않고도
값 바인딩 패턴을 사용하여 값의 부분으로 이름을 줄 수 있습니다:

```swift
let points = [(10, 0), (30, -30), (-20, 0)]

for (x, y) in points {
    if y == 0 {
        print("Found a point on the x-axis at \(x)")
    }
}
// Prints "Found a point on the x-axis at 10"
// Prints "Found a point on the x-axis at -20"
```

위 `for`-`in` 루프는 튜플의 배열을 반복하며,
튜플의 첫 번째와 두 번째 요소를
`x`와 `y` 상수에 바인딩합니다.
루프 내의 구문은 점이 x축 위에 있는지 확인하는 `if` 구문과 같이
이 상수를 사용할 수 있습니다.
이 코드를 작성하는 더 간결한 방법은
`for`-`case`-`in` 루프를 사용하여
값 바인딩과 조건을 결합하는 것입니다.
아래 코드는 위의 `for`-`in` 루프와 동일한 동작을 하는 코드입니다:

```swift
for case (let x, 0) in points {
    print("Found a point on the x-axis at \(x)")
}
// Prints "Found a point on the x-axis at 10"
// Prints "Found a point on the x-axis at -20"
```

이 코드에서
조건은 패턴의 일부로
`for`-`case`-`in` 루프에 결합됩니다.
`for`-`case`-`in` 루프 내의 구문은 x축 위에 있는 점일 때만 실행됩니다.
이 코드는 위의 `for`-`in` 루프와 동일한 결과를 생성하지만,
컬렉션의 특정 요소만
반복하는 더 간결한 방법입니다.

`for`-`case`-`in` 루프는 추가 조건을 확인하는
`where` 절을 추가할 수도 있습니다.
루프 내의 구문은
현재 요소가 `where` 절과 일치할 때만 실행됩니다.
예를 들어:

```swift
for case let (x, y) in points where x == y || x == -y  {
    print("Found (\(x), \(y)) along a line through the origin")
}
// Prints "Found (30, -30) along a line through the origin"
```

이 코드는 튜플의 첫 번째와 두 번째 요소를
`x`와 `y` 상수로 바인딩한 다음에
해당 값을 `where` 절에서 확인합니다.
`where` 절이 `true`이면,
`for` 루프의 본문 내 코드를 실행합니다;
그렇지 않으면 다음 요소로 반복이 계속됩니다.

패턴은 값을 바인딩할 수 있으므로,
`if`-`case` 구문과 `for`-`case`-`in` 루프는
[연관값 (Associated Values)](./enumerations.md#연관값-associated-values)에서 설명한대로
연관값을 가진 열거형과 작업하는데 유용합니다.

## 제어 전송 구문 (Control Transfer Statements)

*제어 전송 구문(Control transfer statements)*은 한 코드에서 다른 코드로
제어를 변경하여 코드가 실행되는 순서를 변경합니다.
Swift는 다섯 개의 제어 전송 구문이 있습니다:

- `continue`
- `break`
- `fallthrough`
- `return`
- `throw`

`continue`, `break`, `fallthrough` 구문은 아래 설명되어 있습니다.
`return` 구문은 [함수 (Functions)](./functions.md)에 설명되어 있고,
`throw` 구문은 [던지는 함수를 이용한 오류 전파 (Propagating Errors Using Throwing Functions)](./error-handling.md#던지는-함수를-이용한-오류-전파-propagating-errors-using-throwing-functions)에 설명되어 있습니다.

### 계속 (Continue)

`continue` 구문은 현재 수행 중인 작업을 중단하고,
루프의 다음 반복을 시작하도록 지시합니다.
이것은 "현재 루프 반복을 끝냈다."라고 선언하지만,
루프 자체를 완전히 종료하지는 않습니다.

아래 예시는 비밀의 퍼즐 구문을 생성하기 위해
소문자 문자열에서 모음과 공백을 삭제합니다:

```swift
let puzzleInput = "great minds think alike"
var puzzleOutput = ""
let charactersToRemove: [Character] = ["a", "e", "i", "o", "u", " "]
for character in puzzleInput {
    if charactersToRemove.contains(character) {
        continue
    }
    puzzleOutput.append(character)
}
print(puzzleOutput)
// Prints "grtmndsthnklk"
```

<!--
  - test: `continue`

  ```swifttest
  -> let puzzleInput = "great minds think alike"
  -> var puzzleOutput = ""
  -> let charactersToRemove: [Character] = ["a", "e", "i", "o", "u", " "]
  -> for character in puzzleInput {
        if charactersToRemove.contains(character) {
           continue
        }
        puzzleOutput.append(character)
     }
  -> print(puzzleOutput)
  <- grtmndsthnklk
  ```
-->

위 코드는 모음이나 공백이 일치하면,
현재 반복의 루프를 종료하고 즉시
다음 반복을 시작하기 위해 `continue` 키워드를 호출합니다.

### 중단 (Break)

`break` 구문은 전체 제어 흐름 구문을 즉시 종료합니다.
`break` 구문은 `switch` 내부나 루프 구문에서
`switch`나 루프 구문을
다른 경우보다 일찍 종료시킬 때 사용될 수 있습니다.

#### 루프 구문에서 중단 (Break in a Loop Statement)

루프 구문 내에서 사용할 때,
`break`는 루프의 실행을 즉시 종료하고
제어를 루프의 닫기 중괄호(`}`) 다음으로 이전합니다.
루프의 현재 반복으로 부터 코드는 더이상 실행되지 않고,
루프의 반복은 더 이상 시작되지 않습니다.

<!--
  TODO: I need an example here.
-->

#### Switch 구문에서 중단 (Break in a Switch Statement)

`switch` 구문 내에서 사용할 때,
`break`는 `switch` 구문을 즉시 종료하고
제어를 `switch` 구문의 닫힌 중괄호(`}`) 다음으로
이동시킵니다.

이러한 동작은 일치할 때 사용될 수 있고 `switch` 구문에서 하나 이상의 케이스를 무시할 때 사용됩니다.
Swift의 `switch` 구문은 완벽하고
빈 케이스를 허락하지 않기 때문에,
의도를 명시하기 위해 의도적으로
케이스를 일치시키고 무시해야 하는 경우가 있습니다.
무시할 케이스의 전체 본문으로 `break` 구문을 작성하여 이를 수행합니다.
해당 케이스가 `switch` 구문과 일치하면
케이스 내부의 `break` 구문이 `switch` 구문의 실행을 즉시 종료합니다.

> Note: `switch` 케이스에 주석만 포함되어 있으면 컴파일 시 오류가 발생합니다.
> 주석은 구문이 아니며 `switch` 케이스는 무시되지 않습니다.
> 항상 `switch` 케이스를 무시하려면 `break` 구문을 사용하십시오.

아래 예시는 `Character` 값을 바꾸고
네 언어중 하나의 언어로 숫자 기호를 표기하는지 판단합니다.
간결함을 위해 단일 `switch` 케이스에 여러 값이 포함됩니다.

```swift
let numberSymbol: Character = "三"  // Chinese symbol for the number 3
var possibleIntegerValue: Int?
switch numberSymbol {
case "1", "١", "一", "๑":
    possibleIntegerValue = 1
case "2", "٢", "二", "๒":
    possibleIntegerValue = 2
case "3", "٣", "三", "๓":
    possibleIntegerValue = 3
case "4", "٤", "四", "๔":
    possibleIntegerValue = 4
default:
    break
}
if let integerValue = possibleIntegerValue {
    print("The integer value of \(numberSymbol) is \(integerValue).")
} else {
    print("An integer value could not be found for \(numberSymbol).")
}
// Prints "The integer value of 三 is 3."
```

<!--
  - test: `breakInASwitchStatement`

  ```swifttest
  -> let numberSymbol: Character = "三"  // Chinese symbol for the number 3
  -> var possibleIntegerValue: Int?
  -> switch numberSymbol {
        case "1", "١", "一", "๑":
           possibleIntegerValue = 1
        case "2", "٢", "二", "๒":
           possibleIntegerValue = 2
        case "3", "٣", "三", "๓":
           possibleIntegerValue = 3
        case "4", "٤", "四", "๔":
           possibleIntegerValue = 4
        default:
           break
     }
  -> if let integerValue = possibleIntegerValue {
        print("The integer value of \(numberSymbol) is \(integerValue).")
     } else {
        print("An integer value couldn't be found for \(numberSymbol).")
     }
  <- The integer value of 三 is 3.
  ```
-->

이 예시는 `numberSymbol`이
`1`부터 `4`의 숫자 기호가
라틴어, 아랍어, 중국어, 태국어 인지 판단합니다.
일치하는 것을 찾으면,
`switch` 구문의 케이스 중에 하나는
적절한 정수값을
`possibleIntegerValue`라 불리는 옵셔널 `Int?` 변수에 할당합니다.

`switch` 구문이 실행을 완료한 후에
이 예시에서는 옵셔널 바인딩을 사용하여 값이 존재하는지 확인합니다.
`possibleIntegerValue` 변수는 옵셔널 타입이기 때문에
초기값은 `nil`을 가지며,
옵셔널 바인딩이 성공하려면
`switch` 구문의 케이스 중 하나에서
실제 값이 할당되어야 합니다.

위 예시에서 가능한 모든 `Character` 값을 리스트화 할 수 없기 때문에
`default` 케이스로 일치되지 않는 문자를 처리합니다.
`default` 케이스는 어떠한 동작도 수행할 필요가 없으므로,
`break` 구문만 적습니다.
`default` 케이스에 일치되자마자,
`break` 구문은 `switch` 구문의 실행을 종료하고,
이어서 `if let` 구문을 실행합니다.

### Fallthrough

Swift에서 `switch` 구문은 각 케이스의 맨 아래에서 다음 케이스로 넘어가지 않습니다.
첫 번째 케이스가 일치하자마자 `switch` 구문의 실행은 완료됩니다.
반대로 C는 다음 케이스로 넘어가는 것을 막기 위해
모든 `switch` 케이스 마지막에 명시적으로 `break` 구문을 명시적으로 넣어야 합니다.
기본적으로 다음 케이스로 넘어가지 않는다는 것은 Swift의 `switch` 구문이
C의 대응문 보다 훨씬 간결하고 예측 가능하다는 것을 의미하므로
실수로 여러 `switch` 케이스를 실행하지 않습니다.

C처럼 다음 케이스로 넘어가려면
`fallthrough` 키워드로 케이스 별로 동작을 선택할 수 있습니다.
아래 예시에서 `fallthrough`를 사용하여 숫자의 설명을 생성합니다.

```swift
let integerToDescribe = 5
var description = "The number \(integerToDescribe) is"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
print(description)
// Prints "The number 5 is a prime number, and also an integer."
```

<!--
  - test: `fallthrough`

  ```swifttest
  -> let integerToDescribe = 5
  -> var description = "The number \(integerToDescribe) is"
  -> switch integerToDescribe {
        case 2, 3, 5, 7, 11, 13, 17, 19:
           description += " a prime number, and also"
           fallthrough
        default:
           description += " an integer."
     }
  -> print(description)
  <- The number 5 is a prime number, and also an integer.
  ```
-->

이 예시는 `description`이라 불리는 새로운 `String` 변수를 선언하고
초기값을 할당합니다.
이 함수는 `switch` 구문을 사용하여 `integerToDescribe`의 값을 고려합니다.
`integerToDescribe`의 값이 리스트에서의 소수 중 하나이면
소수라는 것을 나타내는
텍스트를 `description` 뒤에 추가합니다.
그러면 `fallthrough` 키워드를 사용하여 `default` 케이스 또한 동작하게 됩니다.
`default` 케이스는 추가 설명을 덧붙이고,
`switch` 구문은 완료됩니다.

`integerToDescribe`의 값이 리스트에 없는 소수이면
첫 번째 `switch` 케이스와 전혀 일치하지 않습니다.
특정 케이스가 없기 때문에
`integerToDescribe`는 `default` 케이스와 일치됩니다.

`switch` 구문 실행이 완료된 후에
숫자의 설명은 `print(_:separator:terminator:)` 함수를 사용하여 출력됩니다.
이 예시에서
숫자 `5`는 소수로써 유효합니다.

> Note: `fallthrough` 키워드는 `switch` 케이스 실행을 위한
> 케이스 조건을 확인하지 않습니다.
> `fallthrough` 키워드는 간단하게
> C의 표준 `switch` 구문 동작처럼
> 다음 케이스(또는 `default` 케이스)로 코드 실행을 직접적으로 이동시킵니다.

### 레이블이 있는 구문 (Labeled Statements)

Swift에서는 루프문과 조건문을
다른 루프문이나 조건문 안에
중첩하여 복잡한 제어 흐름 구조를 만들 수 있습니다.
그러나 루프문과 조건문은 모두 `break` 구문을 사용하여
실행을 조기에 종료할 수 있습니다.
따라서 `break` 구문이 종료할 대상이 되는 루프문이나 조건문을
명확하게 지정하는 것이 유용할 때가 있습니다.
마찬가지로 여러 개의 중첩된 루프문을 가지고 있다면,
`continue` 구문이 영향을 미칠 루프를 명확하게 지정하는 것도
유용할 수 있습니다.

이러한 목적을 달성하기 위해,
루프문이나 조건문에 *구문 레이블(statement label)*을 사용할 수 있습니다.
조건문에서
레이블이 있는 구문에 실행을 종료하기 위해
`break` 구문과 구문 레이블을 사용할 수 있습니다.
루프문에서
레이블이 있는 구문에 실행을 종료하거나 이어서 진행하기 위해
`break`나 `continue` 구문과 함께 구문 레이블을 사용할 수 있습니다.

레이블이 있는 구문(labeled statement)을 나타내는
키워드와 같은 줄에 위치하고, 바로 다음에 콜론을 붙여 표시합니다.
다음은 `while` 루프에서 이 구문을 사용하는 예시이며,
동일한 원리가 모든 루프 및 `switch` 구문에도 적용됩니다:

```swift
<#label name#>: while <#condition#> {
   <#statements#>
}
```

다음 예시는 이 챕터의 이전에 봤던
*Snakes and Ladders* 게임에 적절한 버전을 위해 레이블이 있는 `while` 루프와
`break`와 `continue` 구문을 사용합니다.
이번에는 게임에 추가 규칙이 있습니다:

- 이기려면 정사각형 25에 *정확히* 위치해야 합니다.

주사위를 굴려 정사각형 25를 넘어간다면,
정확히 정사각형 25에 위치할 때까지 주사위를 굴려야 합니다.

게임 보드는 이전과 동일합니다.

![Snakes and Ladders](../.gitbook/assets/snakesAndLadders_2x~dark.png)

`finalSquare`, `board`, `square`, `diceRoll`은
이전과 동일하게 초기화 됩니다:

```swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0
```

<!--
  - test: `labels`

  ```swifttest
  -> let finalSquare = 25
  -> var board = [Int](repeating: 0, count: finalSquare + 1)
  >> assert(board == [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0])
  -> board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
  -> board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
  -> var square = 0
  -> var diceRoll = 0
  ```
-->

이 버전의 게임은 `while` 루프와 `switch` 구문을 사용하여
게임의 로직을 구현합니다.
`while` 루프에는 `gameLoop`라는 구문 레이블이 붙어 있으며,
이는 해당 루프가 Snakes and Ladders 게임의 메인 게임 루프임을 나타냅니다.

`while` 루프의 조건은 정사각형 25에 정확하게 착지를 위한 조건을 반영한
`while square != finalSquare`입니다.

```swift
gameLoop: while square != finalSquare {
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    switch square + diceRoll {
    case finalSquare:
        // diceRoll will move us to the final square, so the game is over
        break gameLoop
    case let newSquare where newSquare > finalSquare:
        // diceRoll will move us beyond the final square, so roll again
        continue gameLoop
    default:
        // this is a valid move, so find out its effect
        square += diceRoll
        square += board[square]
    }
}
print("Game over!")
```

<!--
  - test: `labels`

  ```swifttest
  -> gameLoop: while square != finalSquare {
        diceRoll += 1
        if diceRoll == 7 { diceRoll = 1 }
  >>    print("diceRoll is \(diceRoll)")
        switch square + diceRoll {
           case finalSquare:
              // diceRoll will move us to the final square, so the game is over
  >>          print("finalSquare, game is over")
              break gameLoop
           case let newSquare where newSquare > finalSquare:
              // diceRoll will move us beyond the final square, so roll again
  >>          print("move too far, roll again")
              continue gameLoop
           default:
              // this is a valid move, so find out its effect
              square += diceRoll
  >>          print("after diceRoll, square is \(square)")
              square += board[square]
  >>          print("after snakes or ladders, square is \(square)")
        }
     }
  -> print("Game over!")
  << diceRoll is 1
  << after diceRoll, square is 1
  << after snakes or ladders, square is 1
  << diceRoll is 2
  << after diceRoll, square is 3
  << after snakes or ladders, square is 11
  << diceRoll is 3
  << after diceRoll, square is 14
  << after snakes or ladders, square is 4
  << diceRoll is 4
  << after diceRoll, square is 8
  << after snakes or ladders, square is 8
  << diceRoll is 5
  << after diceRoll, square is 13
  << after snakes or ladders, square is 13
  << diceRoll is 6
  << after diceRoll, square is 19
  << after snakes or ladders, square is 8
  << diceRoll is 1
  << after diceRoll, square is 9
  << after snakes or ladders, square is 18
  << diceRoll is 2
  << after diceRoll, square is 20
  << after snakes or ladders, square is 20
  << diceRoll is 3
  << after diceRoll, square is 23
  << after snakes or ladders, square is 23
  << diceRoll is 4
  << move too far, roll again
  << diceRoll is 5
  << move too far, roll again
  << diceRoll is 6
  << move too far, roll again
  << diceRoll is 1
  << after diceRoll, square is 24
  << after snakes or ladders, square is 16
  << diceRoll is 2
  << after diceRoll, square is 18
  << after snakes or ladders, square is 18
  << diceRoll is 3
  << after diceRoll, square is 21
  << after snakes or ladders, square is 21
  << diceRoll is 4
  << finalSquare, game is over
  << Game over!
  ```
-->

각 루프가 시작될 때 주사위는 굴려집니다.
즉시 플레이어를 이동하는 것보다,
루프는 이동의 결과를 고려하고
이동이 가능한지 판단하기 위해 `switch` 구문을 사용합니다:

- 주사위 굴림이 플레이어를 마지막 정사각형으로 이동시킨다면
  게임은 종료됩니다.
  `break gameLoop` 구문은 게임 종료를 위해
  `while` 루프의 바깥으로 제어를 이동합니다.
- 주사위 굴림이 플레이어를 마지막 정사각형을 *넘어* 이동시킨다면,
  이 이동은 유효하지 않고 플레이어는 주사위를 다시 굴려야 합니다.
  `continue gameLoop` 구문은 현재 `while` 루프 반복을 종료하고
  루프의 다음 반복을 시작합니다.
- 다른 모든 케이스에 해당하는 주사위 굴림은 유효합니다.
  플레이어는 `diceRoll`만큼 이동하고,
  게임 로직은 뱀과 사다리에 대해 확인합니다.
  그러면 루프는 종료되고, 다음 턴이 요구되는지 결정하기 위해
  `while` 조건으로 되돌아 갑니다.

> Note: 위에서 `break` 구문이 `gameLoop` 레이블을 사용하지 않았다면,
> `while` 구문을 빠져나오는 것이 아니라, `switch` 구문을 빠져나옵니다.
> `gameLoop` 레이블을 사용하면 어떤 제어문을 종료해야 하는지 명확해집니다.
>
> 루프의 다음 반복으로 이동하기위해 `continue gameLoop` 호출할 때
> 반드시 `gameLoop` 레이블을 사용할 필요는 않습니다.
> 게임에서 오직 하나의 루프만 존재하므로,
> `continue` 구문이 영향을 미칠 루프가 명확하기 때문입니다.
> 그러나 `continue` 구문에서 `gameLoop` 레이블을 사용해도 아무런 문제가 없습니다.
> 이렇게 하면 `break` 구문과의 일관성을 유지할 수 있으며,
> 게임의 로직을 더 읽기 쉽고 이해하기 쉽게 만듭니다.

## 이른 종료 (Early Exit)

`if` 구문처럼 `guard` 구문은
표현식의 Boolean 값에 따라 특정 구문을 실행합니다.
`guard` 구문은 특정 조건이 참이어야만
`guard` 구문 이후의 코드가 실행되도록 요구하는 데 사용됩니다.
`if` 구문과 다르게,
`guard` 구문은 항상 `else` 절을 가지고 있습니다 ---
`else` 절 안의 코드는 조건이 참이 아닐 경우에 실행됩니다.

```swift
func greet(person: [String: String]) {
    guard let name = person["name"] else {
        return
    }

    print("Hello \(name)!")

    guard let location = person["location"] else {
        print("I hope the weather is nice near you.")
        return
    }

    print("I hope the weather is nice in \(location).")
}

greet(person: ["name": "John"])
// Prints "Hello John!"
// Prints "I hope the weather is nice near you."
greet(person: ["name": "Jane", "location": "Cupertino"])
// Prints "Hello Jane!"
// Prints "I hope the weather is nice in Cupertino."
```

<!--
  - test: `guard`

  ```swifttest
  -> func greet(person: [String: String]) {
         guard let name = person["name"] else {
             return
         }

         print("Hello \(name)!")

         guard let location = person["location"] else {
             print("I hope the weather is nice near you.")
             return
         }

         print("I hope the weather is nice in \(location).")
     }

  -> greet(person: ["name": "John"])
  <- Hello John!
  <- I hope the weather is nice near you.
  -> greet(person: ["name": "Jane", "location": "Cupertino"])
  <- Hello Jane!
  <- I hope the weather is nice in Cupertino.
  ```
-->

`guard` 구문의 조건이 충족되면,
코드 실행은 `guard` 구문의 닫는 중괄호 다음으로 이어집니다.
조건의 일부로 옵셔널 바인딩을 사용하여
값이 할당된 모든 변수나 상수는
`guard` 문이 존재하는
나머지 코드 블록에서 사용할 수 있습니다.

조건이 충족되지 않으면,
`else` 안의 코드가 실행됩니다.
이 분기는 반드시 제어 흐름을 전환하여
`guard` 구문이 포함된 코드 블록을 빠져나가야 합니다.
이를 위해 `return`, `break`, `continue`, `throw`와 같은
제어 전달 문을 사용할 수 있으며,
또는 `fatalError(_:file:line:)`처럼 반환되지 않는
함수나 메서드를 호출할 수도 있습니다.

`guard` 구문을 사용하면
동일한 조건 검사를 `if` 구문으로 수행하는 것보다
코드의 가독성이 향상됩니다.
이를 통해 일반적으로 실행되는 코드를
`else` 블록으로 래핑하지 않고 작성할 수 있으며
요구에 충족하지 않는 부분을 요구사항 근처에서
처리할 수 있도록 해줍니다.

## 연기된 동작 (Deferred Actions)

`if`와 `while`과 같은 제어 흐름 구조는
코드의 일부분이 실행되거나
실행 횟수를 제어할 수 있는 반면에,
`defer` 구문은 특정 코드가 *언제* 실행될지를 제어합니다.
현재 범위의 마지막에 실행되는
코드 블록을 작성하기 위해 `defer` 블록을 사용합니다.
예를 들어:

```swift
var score = 1
if score < 10 {
    defer {
        print(score)
    }
    score += 5
}
// Prints "6"
```

<!--
  - test: `defer-with-if`

  ```swifttest
  -> var score = 1
  -> if score < 10 {
  ->     defer {
  ->         print(score)
  ->     }
  ->     score += 5
  -> }
  <- 6
  ```
-->

위의 예시에서
`defer` 블록 안의 코드는
`if` 구문의 본문이 종료되기 전에 실행됩니다.
먼저 `score`를 5 증가시키는
`if` 구문의 코드가 수행됩니다.
그런 다음 `if` 구문의 범위가 종료되기 전에
`score`를 출력하는 코드가 수행됩니다.

프로그램이 어떻게 종료되든
`defer` 안의 코드는 항상 수행됩니다.
여기에는 함수에서 조기에 반환되거나,
`for` 루프를 빠져나가거나,
오류를 발생시키는 경우에도 실행됩니다.
이러한 동작 방식 덕분에 `defer`는
특정 작업의 쌍을 보장해야 하는 경우에 유용합니다 ---
수동으로 메모리를 할당하고 해제하는 경우,
저수준 파일 디스크립터를 열고 닫는 경우,
데이터베이스에서 트랜잭션을 시작하고 종료와 같은 동작에서 활용할 수 있습니다 ---
두 동작을 코드에서 나란히 작성할 수 있기 때문입니다.
예를 들어,
다음의 코드는 점수에 100을 더하고 뺌으로써
임시로 보너스를 주는 코드입니다:

```swift
var score = 3
if score < 100 {
    score += 100
    defer {
        score -= 100
    }
    // Other code that uses the score with its bonus goes here.
    print(score)
}
// Prints "103"
```

<!--
  - test: `defer-paired-actions`

  ```swift
  -> var score = 3
  -> if score < 100 {
  ->     score += 100
  ->     defer {
  ->         score -= 100
  ->     }
  ->     // Other code that uses the score with its bonus goes here.
  ->     print(score)
  -> }
  <- 103
  ```
-->

같은 범위에 하나 이상의 `defer` 블록을 작성하면,
첫 번째 `defer` 블록은 마지막에 수행됩니다.

```swift
if score < 10 {
    defer {
        print(score)
    }
    defer {
        print("The score is:")
    }
    score += 5
}
// Prints "The score is:"
// Prints "6"
```

<!--
  - test: `defer-with-if`

  ```swifttest
  -> if score < 10 {
  ->     defer {
  ->         print(score)
  ->     }
  ->     defer {
  ->         print("The score is:")
  ->     }
  ->     score += 5
  -> }
  <- 6
  ```
-->

프로그램이 실행을 중지하면 ---
예를 들어, 런타임 오류 또는 충돌로 인해 ---
지연된 코드가 실행되지 않습니다.
그러나 오류가 발생한 후에는 지연된 코드가 실행됩니다;
`defer`를 오류 처리와 함께 사용하는 방법에 대한 자세한 내용은
[정리 작업 지정 (Specifying Cleanup Actions)](./error-handling.md#정리-작업-지정-specifying-cleanup-actions)을 참고바랍니다.

## API 가용성 검사 (Checking API Availability)

Swift는 API 가용성 검사(checking API availability)를 기본적으로 지원하여,
특정 배포 타겟에서
사용할 수 없는 API를 실수로 사용하는 것을 방지합니다.

컴파일러는 SDK의 가용성 정보를 사용하여
코드에서 사용하는 모든 API가
프로젝트에서 지정한 배포 대상에서 사용할 수 있는지 확인합니다.
Swift는 사용할 수 없는 API를 사용하려고 하면
컴파일 시 오류를 발생합니다.

`if` 또는 `guard` 구문에서 *가용성 조건(availability condition)*을 사용하여,
런타임에 원하는 API가 사용 가능한지 여부에 따라
코드 블록을 조건부로 실행할 수 있습니다.
컴파일러는 해당 코드 블록 내 API의 가용성을 확인할 때
가용성 조건에서 제공된 정보를 사용합니다.

```swift
if #available(iOS 10, macOS 10.12, *) {
    // Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS
} else {
    // Fall back to earlier iOS and macOS APIs
}
```

<!--
  - test: `availability`

  ```swifttest
  -> if #available(iOS 10, macOS 10.12, *) {
         // Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS
     } else {
         // Fall back to earlier iOS and macOS APIs
     }
  ```
-->

iOS에서 위의 특정 가용성 조건의
`if` 구문의 본문은 iOS 10 이상 버전에서만 실행되고;
macOS에서는 macOS 10.12 이상 버전에서만 실행됩니다.
마지막 인자 `*`는 필수이며 다른 플랫폼에서
`if`의 본문이 최소 타겟 버전에서 실행되도록 지정합니다.

일반적인 형식에서
가용성 조건은 플랫폼 이름 및 버전 목록을 포함합니다.
`iOS`, `macOS`, `watchOS`, `tvOS`, `visionOS`와 같은 플랫폼 이름을 사용하고 ---
자세한 내용은 [선언 속성 (Declaration Attributes)](../language-reference/attributes.md#선언-속성-declaration-attributes)을 참고바랍니다.
iOS 8 또는 macOS 10.10과 같은 메이저 버전 번호를 지정하는 것 외에
iOS 11.2.6과 macOS 10.13.3과 같은 마이너 버전 번호로 지정할 수 있습니다.

```swift
if #available(<#platform name#> <#version#>, <#...#>, *) {
    <#statements to execute if the APIs are available#>
} else {
    <#fallback statements to execute if the APIs are unavailable#>
}
```

`guard` 구문과 함께 가용성 조건을 사용하면,
해당 코드 블록 내에서 나머지 코드에 적용되는
가용성 정보가 더 구체화됩니다.

```swift
@available(macOS 10.12, *)
struct ColorPreference {
    var bestColor = "blue"
}

func chooseBestColor() -> String {
    guard #available(macOS 10.12, *) else {
        return "gray"
    }
    let colors = ColorPreference()
    return colors.bestColor
}
```

<!--
  - test: `guard-with-pound-available`

  ```swifttest
  -> @available(macOS 10.12, *)
  -> struct ColorPreference {
         var bestColor = "blue"
     }

  -> func chooseBestColor() -> String {
        guard #available(macOS 10.12, *) else {
            return "gray"
        }
        let colors = ColorPreference()
        return colors.bestColor
     }
  >> print(chooseBestColor())
  << blue
  ```
-->

위의 예시에서
`ColorPreference` 구조체는 macOS 10.12 이상을 요구합니다.
`chooseBestColor()` 함수는 가용성 가드로 시작합니다.
플랫폼 버전이 `ColorPreference`를 사용하기에 너무 예전 버전이면,
항상 사용 가능한 동작으로 대체됩니다.
`guard` 구문 이후는
macOS 10.12 이상 버전을 요구하는 API를 사용할 수 있습니다.

`#available` 이외에도
Swift는 비가용성 조건을 사용하여 반대로 확인할 수도 있습니다.
예를 들어, 아래의 내용은 동일한 내용을 확인합니다:

```swift
if #available(iOS 10, *) {
} else {
    // Fallback code
}

if #unavailable(iOS 10) {
    // Fallback code
}
```

<!--
  - test: `availability-and-unavailability`

  ```swifttest
  -> if #available(iOS 10, *) {
     } else {
        // Fallback code
     }

  -> if #unavailable(iOS 10) {
        // Fallback code
     }
  ```
-->

`#unavailable`를 사용하는 형식은 대체 코드만을 포함할 때
더 읽기 쉬운 코드로 만들어 줍니다.

<!--
  FIXME
  Not a general purpose condition; can't combine with &&, etc.
  You can use it with if-let, and other Boolean conditions, using a comma
-->

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->