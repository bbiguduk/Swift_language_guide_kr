# 소멸 (Deinitialization)

정리가 필요한 리소스를 해제합니다.

*디이니셜라이저(deinitializer)*는 클래스 인스턴스가 할당 해제되기 직전에 호출됩니다.
이니셜라이저는 `init` 키워드로 작성하는 것과 유사하게
디이니셜라이저는 `deinit` 키워드로 작성합니다.
디이니셜라이저는 클래스 타입에서만 사용 가능합니다.

## 소멸 동작 방식 (How Deinitialization Works)

Swift는 인스턴스가 더 이상 필요하지 않을 때,
자원 확보를 위해 인스턴스를 자동으로 할당 해제합니다.
Swift는 [자동 순환 카운팅 (Automatic Reference Counting)](./automatic-reference-counting.md)에서 설명했듯이
*자동 참조 카운팅(ARC)*을 통해
인스턴스의 메모리를 관리합니다.
일반적으로 인스턴스가 할당 해제될 때 수동으로 정리 작업을 수행할 필요는 없습니다.
그러나 직접 관리하는 리소스가 있다면,
추가적인 정리작업이 필요한 경우가 있습니다.
예를 들어 파일을 열고 데이터를 작성하는 클래스를 생성했다면,
클래스 인스턴스를 할당 해제하기 전에 파일을 닫아야 합니다.

클래스 디이니셜라이저는 클래스 당 하나의 디이니셜라이저만 가지고 있습니다.
디이니셜라이저는 매개변수가 없고
소괄호 없이 작성합니다:

```swift
deinit {
    // perform the deinitialization
}
```

<!--
  - test: `deinitializer`

  ```swifttest
  >> class Test {
  -> deinit {
        // perform the deinitialization
     }
  >> }
  ```
-->

디이니셜라이저는 인스턴스가 할당 해제 되기 직전에 자동으로 호출됩니다.
디이니셜라이저를 직접 호출할 수는 없습니다.
상위 클래스 디이니셜라이저는 하위 클래스로 상속되고,
상위 클래스 디이니셜라이저는 하위 클래스 디이니셜라이저 구현이 끝날 때
자동으로 호출됩니다.
상위 클래스 디이니셜라이저는 하위 클래스가 자신의 디이니셜라이저를 제공하지 않더라도
항상 호출됩니다.

인스턴스는 디이니셜라이저가 호출되기 전에는 할당 해제되지 않기 때문에,
디이니셜라이저는 인스턴스의 모든 프로퍼티에 접근할 수 있고
프로퍼티 기반으로 동작을 변경할 수 있습니다
(닫아야 하는 파일의 이름을 조회하는 경우 등).

## 디이니셜라이저 동작 (Deinitializers in Action)

다음은 디이니셜라이저에 대한 동작의 예시입니다.
이 예시는 간단한 게임을 위해 `Bank`와 `Player`라는 두 개의 새로운 타입을 정의합니다.
`Bank` 클래스는 유통 중인 코인이 10,000개를 넘을 수 없는
가상 화폐를 관리합니다.
게임에서는 하나의 `Bank`만 가질 수 있으므로,
`Bank`는 현재 상태를 저장하고 관리하는
타입 프로퍼티와 타입 메서드를 가지는 클래스로 구현됩니다:

```swift
class Bank {
    static var coinsInBank = 10_000
    static func distribute(coins numberOfCoinsRequested: Int) -> Int {
        let numberOfCoinsToVend = min(numberOfCoinsRequested, coinsInBank)
        coinsInBank -= numberOfCoinsToVend
        return numberOfCoinsToVend
    }
    static func receive(coins: Int) {
        coinsInBank += coins
    }
}
```

<!--
  - test: `deinitializer`

  ```swifttest
  -> class Bank {
        static var coinsInBank = 10_000
        static func distribute(coins numberOfCoinsRequested: Int) -> Int {
           let numberOfCoinsToVend = min(numberOfCoinsRequested, coinsInBank)
           coinsInBank -= numberOfCoinsToVend
           return numberOfCoinsToVend
        }
        static func receive(coins: Int) {
           coinsInBank += coins
        }
     }
  ```
-->

`Bank`는 `coinsInBank` 프로퍼티를 통해 현재 코인의 갯수를 추적합니다.
또한 코인의 분배와 회수를 처리하기 위해
`distribute(coins:)`와 `receive(coins:)`인 두 개의 메서드를 제공합니다.

`distribute(coins:)` 메서드는 코인을 분배하기 전에 은행에 코인이 충분한지 검사합니다.
코인이 충분하지 않으면,
`Bank`는 요청된 수보다 더 작은 수를 반환합니다
(그리고 코인이 은행에 남아있지 않은 경우 0을 반환합니다).
제공된 실제 코인 수를 나타내는 정수를 반환합니다.

`receive(coins:)` 메서드는 회수한 코인의 수를 은행의 코인 저장소에 추가합니다.

`Player` 클래스는 게임에 플레이어를 나타냅니다.
각 플레이어는 언제든지 지갑에 특정 수의 동전을 보관합니다.
이것은 플레이어의 `coinsInPurse` 프로퍼티에 표시됩니다:

```swift
class Player {
    var coinsInPurse: Int
    init(coins: Int) {
        coinsInPurse = Bank.distribute(coins: coins)
    }
    func win(coins: Int) {
        coinsInPurse += Bank.distribute(coins: coins)
    }
    deinit {
        Bank.receive(coins: coinsInPurse)
    }
}
```

<!--
  - test: `deinitializer`

  ```swifttest
  -> class Player {
        var coinsInPurse: Int
        init(coins: Int) {
           coinsInPurse = Bank.distribute(coins: coins)
        }
        func win(coins: Int) {
           coinsInPurse += Bank.distribute(coins: coins)
        }
        deinit {
           Bank.receive(coins: coinsInPurse)
        }
     }
  ```
-->

각 `Player` 인스턴스는 초기화 하는 동안 은행에서 지정된 수의 코인을
시작 허용량으로 초기화되지만
사용할 수 있는 코인이 충분하지 않은 경우
`Player` 인스턴스는 해당 수보다 적게 받을 수 있습니다.

`Player` 클래스는 은행에서 특정 수의 코인을 가져오고
플레이어의 지갑에 더하는
`win(coins:)` 메서드를 정의합니다.
`Player` 클래스는 `Player` 인스턴스가 할당 해제되기 전에 호출되는
디이니셜라이저도 구현합니다.
여기서 디이니셜라이저는 단순하게 플레이어의 모든 코인을 은행에 반환합니다:

```swift
var playerOne: Player? = Player(coins: 100)
print("A new player has joined the game with \(playerOne!.coinsInPurse) coins")
// Prints "A new player has joined the game with 100 coins"
print("There are now \(Bank.coinsInBank) coins left in the bank")
// Prints "There are now 9900 coins left in the bank"
```

<!--
  - test: `deinitializer`

  ```swifttest
  -> var playerOne: Player? = Player(coins: 100)
  -> print("A new player has joined the game with \(playerOne!.coinsInPurse) coins")
  <- A new player has joined the game with 100 coins
  -> print("There are now \(Bank.coinsInBank) coins left in the bank")
  <- There are now 9900 coins left in the bank
  ```
-->

새로운 `Player` 인스턴스는 가능하면 100개의 코인을 요청하여 생성됩니다.
`Player` 인스턴스는 `playerOne`이라는 옵셔널 `Player` 변수에 저장됩니다.
플레이어는 언제나 게임을 떠날 수 있기 때문에 옵셔널 변수가 사용됩니다.
이 옵셔널은 현재 게임의 플레이어인지 확인합니다.

`playerOne`은 옵셔널이기 때문에, 기본 코인의 수를 출력하기 위해 `coinsInPurse` 프로퍼티에 접근할 때와
`win(coins:)` 메서드가 호출될 때마다
느낌표(`!`)를 붙입니다:

```swift
playerOne!.win(coins: 2_000)
print("PlayerOne won 2000 coins & now has \(playerOne!.coinsInPurse) coins")
// Prints "PlayerOne won 2000 coins & now has 2100 coins"
print("The bank now only has \(Bank.coinsInBank) coins left")
// Prints "The bank now only has 7900 coins left"
```

<!--
  - test: `deinitializer`

  ```swifttest
  -> playerOne!.win(coins: 2_000)
  -> print("PlayerOne won 2000 coins & now has \(playerOne!.coinsInPurse) coins")
  <- PlayerOne won 2000 coins & now has 2100 coins
  -> print("The bank now only has \(Bank.coinsInBank) coins left")
  <- The bank now only has 7900 coins left
  ```
-->

여기서 플레이어는 2,000 코인을 가지고 있습니다.
플레이어의 지갑은 2,100 코인이 있고,
은행은 7,900 코인이 남아 있습니다.

```swift
playerOne = nil
print("PlayerOne has left the game")
// Prints "PlayerOne has left the game"
print("The bank now has \(Bank.coinsInBank) coins")
// Prints "The bank now has 10000 coins"
```

<!--
  - test: `deinitializer`

  ```swifttest
  -> playerOne = nil
  -> print("PlayerOne has left the game")
  <- PlayerOne has left the game
  -> print("The bank now has \(Bank.coinsInBank) coins")
  <- The bank now has 10000 coins
  ```
-->

플레이어는 이제 게임을 떠납니다.
이것을 나타내기 위해 옵셔널 `playerOne` 변수에
"`Player` 인스턴스 없음"을 나타내는 `nil`을 설정합니다.
이 시점에
`Player` 인스턴스에 대한 `playerOne` 변수의 참조가 끊어집니다.
다른 프로퍼티나 변수는 여전히 `Player` 인스턴스를 참조하고 있으므로,
메모리 확보를 위해 할당 해제 됩니다.
이것이 일어나기 직전에 자동으로 디이니셜라이저가 호출되고,
코인은 은행에 반환됩니다.

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->
