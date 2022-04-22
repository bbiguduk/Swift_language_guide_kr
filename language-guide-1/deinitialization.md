# 초기화 해제 \(Deinitialization\)

<!--
A deinitializer is called immediately before a class instance is deallocated. You write deinitializers with the deinit keyword, similar to how initializers are written with the init keyword. Deinitializers are only available on class types.
-->

_초기화 해제 구문 \(deinitializer\)_ 는 클래스 인스턴스가 할당 해제되기 전에 즉시 호출됩니다. 초기화 구문은 `init` 키워드로 작성하는 것과 유사하게 초기화 해제는 `deinit` 키워드로 작성합니다. 초기화 해제는 클래스 타입에서만 사용 가능합니다.

## 초기화 해제 동작 \(How Deinitialization Works\)

<!--
Swift automatically deallocates your instances when they’re no longer needed, to free up resources. Swift handles the memory management of instances through automatic reference counting (ARC), as described in Automatic Reference Counting. Typically you don’t need to perform manual cleanup when your instances are deallocated. However, when you are working with your own resources, you might need to perform some additional cleanup yourself. For example, if you create a custom class to open a file and write some data to it, you might need to close the file before the class instance is deallocated.

Class definitions can have at most one deinitializer per class. The deinitializer doesn’t take any parameters and is written without parentheses:
-->

Swift는 더이상 필요하지 않을 때 자원 확보를 위해 인스턴스를 자동으로 할당 해제합니다. Swift는 [자동 참조 카운팅 \(Automatic Reference Counting\)](automatic-reference-counting.md) 에서 설명되어 있듯이 _자동 참조 카운팅 \(ARC\)_ 를 통해 인스턴스의 메모리를 관리합니다. 일반적으로 인스턴스가 할당 해제될 때 수동으로 수행할 필요는 없습니다. 그러나 자체 자원으로 작업하는 경우 추가 정리로 직접 수행이 필요한 경우가 있습니다. 예를 들어 파일을 열고 데이터를 작성하는 클래스를 생성하면 클래스 인스턴스를 할당 해제하기 전에 파일을 닫아야 합니다.

클래스 초기화 해제는 클래스 당 하나의 초기화 해제 구문만 가지고 있습니다. 이 초기화 해제 구문은 파라미터가 없고 소괄호 없이 작성됩니다:

```swift
deinit {
    // perform the deinitialization
}
```

<!--
Deinitializers are called automatically, just before instance deallocation takes place. You aren’t allowed to call a deinitializer yourself. Superclass deinitializers are inherited by their subclasses, and the superclass deinitializer is called automatically at the end of a subclass deinitializer implementation. Superclass deinitializers are always called, even if a subclass doesn’t provide its own deinitializer.

Because an instance isn’t deallocated until after its deinitializer is called, a deinitializer can access all properties of the instance it’s called on and can modify its behavior based on those properties (such as looking up the name of a file that needs to be closed).
-->

초기화 해제 구문은 인스턴스가 할당 해제 되기 직전에 자동으로 호출됩니다. 초기화 해제 구문을 직접 호출할 수는 없습니다. 상위 클래스 초기화 해제 구문은 하위 클래스로 상속되고 상위 클래스 초기화 해제 구문은 하위 클래스 초기화 해제 구문 구현이 끝날 때 자동으로 호출됩니다. 상위 클래스 초기화 해제 구문은 하위 클래스가 자신의 초기화 해제 구문을 제공하지 않더라도 항상 호출됩니다.

인스턴스는 초기화 해제 구문이 호출되기 전에는 할당 해제되지 않기 때문에 예를 들어 닫아야 하는 파일의 이름에 접근하듯 초기화 해제 구문은 인스턴스의 모든 프로퍼티에 접근할 수 있고 프로퍼티 기반으로 동작을 수정할 수 있습니다.

## 초기화 해제 구문 동작 \(Deinitializers in Action\)

<!--
Here’s an example of a deinitializer in action. This example defines two new types, Bank and Player, for a simple game. The Bank class manages a made-up currency, which can never have more than 10,000 coins in circulation. There can only ever be one Bank in the game, and so the Bank is implemented as a class with type properties and methods to store and manage its current state:
-->

다음은 초기화 해제 구문에 대한 동작의 예입니다. 이 예제는 간단한 게임을 위해 `Bank` 와 `Player` 라는 2개의 새로운 타입을 정의합니다. `Bank` 클래스는 유통중인 코인이 10,000개를 넘을 수 없는 구성 통화를 관리합니다. 게임에서는 하나의 `Bank` 만 가질 수 있으므로 `Bank` 는 현재 상태를 저장하고 관리하기 위해 타입 프로퍼티와 메서드를 가진 클래스로 구현됩니다:

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
Bank keeps track of the current number of coins it holds with its coinsInBank property. It also offers two methods—distribute(coins:) and receive(coins:)—to handle the distribution and collection of coins.

The distribute(coins:) method checks that there are enough coins in the bank before distributing them. If there aren’t enough coins, Bank returns a smaller number than the number that was requested (and returns zero if no coins are left in the bank). It returns an integer value to indicate the actual number of coins that were provided.

The receive(coins:) method simply adds the received number of coins back into the bank’s coin store.

The Player class describes a player in the game. Each player has a certain number of coins stored in their purse at any time. This is represented by the player’s coinsInPurse property:
-->

`Bank` 는 `coinsInBank` 프로퍼티를 통해 현재 코인의 갯수를 추적합니다. 또한 코인의 분배와 수집을 처리하기 위해 `distribute(coins:)` 와 `receive(coins:)` 인 2개의 메서드를 제공합니다.

`distribute(coins:)` 메서드는 코인을 분배하기 전에 은행에 코인이 충분한지 검사합니다. 코인이 충분하지 않으면 `Bank` 는 요청된 수보다 더 작은 수를 반환하고 코인이 은행에 남아있지 않은 경우 0을 반환합니다. 제공된 실제 코인 수를 나타내는 정수를 반환합니다.

`receive(coins:)` 메서드는 받은 코인의 수를 은행의 코인 저장소에 추가합니다.

`Player` 클래스는 게임에 플레이어를 설명합니다. 각 플레이어는 언제든지 지갑에 특정 수의 동전을 보관합니다. 이것은 플레이어의 `coinsInPurse` 프로퍼티에 표시됩니다:

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
Each Player instance is initialized with a starting allowance of a specified number of coins from the bank during initialization, although a Player instance may receive fewer than that number if not enough coins are available.

The Player class defines a win(coins:) method, which retrieves a certain number of coins from the bank and adds them to the player’s purse. The Player class also implements a deinitializer, which is called just before a Player instance is deallocated. Here, the deinitializer simply returns all of the player’s coins to the bank:
-->

각 `Player` 인스턴스는 초기화 하는 동안 은행에서 지정된 수의 코인을 시작 허용량으로 초기화 되지만 사용할 수 있는 코인이 충분하지 않은 경우 `Player` 인스턴스는 해당 수보다 적게 받을 수 있습니다.

`Player` 클래스는 은행에서 특정 수의 코인을 가져오고 플레이어의 지갑에 더하는 `win(coins:)` 메서드를 정의합니다. `Player` 클래스는 `Player` 인스턴스가 할당 해제되기 전에 호출되는 초기화 해제 구문도 구현합니다. 여기서 초기화 해제 구문은 단순하게 플레이어의 모든 코인을 은행에 반환합니다:

```swift
var playerOne: Player? = Player(coins: 100)
print("A new player has joined the game with \(playerOne!.coinsInPurse) coins")
// Prints "A new player has joined the game with 100 coins"
print("There are now \(Bank.coinsInBank) coins left in the bank")
// Prints "There are now 9900 coins left in the bank"
```

<!--
A new Player instance is created, with a request for 100 coins if they’re available. This Player instance is stored in an optional Player variable called playerOne. An optional variable is used here, because players can leave the game at any point. The optional lets you track whether there’s currently a player in the game.

Because playerOne is an optional, it’s qualified with an exclamation point (!) when its coinsInPurse property is accessed to print its default number of coins, and whenever its win(coins:) method is called:
-->

새로운 `Player` 인스턴스는 가능하면 100개의 코인을 요청하여 생성됩니다. `Player` 인스턴스는 `playerOne` 이라는 옵셔널 `Player` 변수에 저장됩니다. 플레이어는 언제나 게임을 떠날 수 있기 때문에 옵셔널 변수가 사용됩니다. 이 옵셔널은 현재 게임에서 플레이어 인지 확인합니다.

`playerOne` 은 옵셔널 이기 때문에 기본 코인의 수를 출력하기 위해 `coinsInPurse` 프로퍼티에 접근할 때와 `win(coins:)` 메서드가 호출 될 때마다 느낌표 \(`!`\)로 한정됩니다:

```swift
playerOne!.win(coins: 2_000)
print("PlayerOne won 2000 coins & now has \(playerOne!.coinsInPurse) coins")
// Prints "PlayerOne won 2000 coins & now has 2100 coins"
print("The bank now only has \(Bank.coinsInBank) coins left")
// Prints "The bank now only has 7900 coins left"
```

<!--
Here, the player has won 2,000 coins. The player’s purse now contains 2,100 coins, and the bank has only 7,900 coins left.
-->

여기서 플레이어는 2,000 코인을 가지고 있습니다. 플레이어의 지갑은 2,100 코인이 있고 은행은 7,900 코인이 남아 있습니다.

```swift
playerOne = nil
print("PlayerOne has left the game")
// Prints "PlayerOne has left the game"
print("The bank now has \(Bank.coinsInBank) coins")
// Prints "The bank now has 10000 coins"
```

<!--
The player has now left the game. This is indicated by setting the optional playerOne variable to nil, meaning “no Player instance.” At the point that this happens, the playerOne variable’s reference to the Player instance is broken. No other properties or variables are still referring to the Player instance, and so it’s deallocated in order to free up its memory. Just before this happens, its deinitializer is called automatically, and its coins are returned to the bank.
-->

플레이어는 지금 게임을 떠납니다. 이것을 나타내기 위해 `playerOne` 옵셔널 변수에 "`Player` 인스턴스 없음"을 나타내는 `nil` 을 설정합니다. 이 시점에 `Player` 인스턴스에 대한 `playerOne` 변수의 참조가 끊어집니다. 다른 프로퍼티 또는 변수는 여전히 `Player` 인스턴스를 참조하고 있으므로 메모리 확보를 위해 할당 해제 됩니다. 이것이 일어나기 직전에 자동으로 초기화 해제 구문이 호출되고 코인은 은행에 반환됩니다.

