# 에러 처리 \(Error Handling\)

_에러 처리 \(Error handling\)_ 는 프로그램의 에러 조건에서 응답하고 복구하는 프로세스 입니다. Swift는 런타임에 복구 가능한 에러를 던지고 포착하고 전파하고 조작하기 위한 최고 수준의 지원을 제공합니다.

일부 작업은 항상 실행을 완료하거나 유용한 출력을 생성한다고 보장되지 않습니다. 옵셔널은 값이 없음을 나타내는데 사용되지만 작업이 실패할 경우 코드가 그에 따라 응답할 수 있도록 에러의 원인을 이해하는 것이 유용한 경우가 많습니다.

예를 들어 디스크의 파일에서 데이터를 읽고 처리하는 작업을 생각해 봅시다. 지정된 위치에 파일이 존재하지 않거나 파일에 읽기 권한이 없거나 적절한 형식으로 인코딩 되지 않는 것을 포함하여 작업이 실패할 수 있는 많은 방법이 있습니다. 이러한 다른 상황을 구분하면 프로그램에서 일부 에러를 해결하고 해결할 수 없는 에러를 전달할 수 있습니다.

> NOTE Swift에서 에러 처리는 Cocoa와 Objective-C에 `NSError` 를 사용하는 에러 처리 패턴과 상호 운용됩니다. 더많은 정보는 [Swift에서 Cocoa 에러 처리 \(Handling Cocoa Errors in Swift\)](https://developer.apple.com/documentation/swift/cocoa_design_patterns/handling_cocoa_errors_in_swift) 를 참고 바랍니다.

## 에러 표현과 던지기 \(Representing and Throwing Errors\)

Swift에서 에러는 `Error` 프로토콜에 준수하는 타입의 값으로 표현됩니다. 이 빈 프로토콜은 에러를 처리하는 것에 대해 사용될 수 있음을 나타냅니다.

Swift 열거형은 관련된 에러 조건의 그룹을 모델리하는데 특히 적합하며 관련값을 사용하여 에러의 특성에 대한 추가 정보를 전달할 수 있습니다. 예를 들어 다음은 게임 내에서 자동 판매기를 작동하는 에러 조건을 나타내는 방법입니다:

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
```

에러가 발생하면 예상치 못한 일이 발생하여 정상적인 흐름을 계속할 수 없음을 나타낼 수 있습니다. `throw` 구문을 사용하여 에러를 발생 시킵니다. 예를 들어 아래의 코드는 자동 판매기에 5개의 코인이 더 필요하다고 에러를 발생 시킵니다:

```swift
throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
```

## 에러 처리 \(Handling Errors\)

에러가 발생할 때 주변 코드의 부분이 에러 처리를 담당해야 합니다. 예를 들어 문제를 수정하거나 다른 방법을 시도하거나 사용자에게 에러를 알리는 방법으로 에러를 처리해야 합니다.

Swift에서는 에러를 처리하는 4가지 방법이 있습니다. 함수에서 해당 함수를 호출하는 코드로 에러를 전파하거나 `do`-`catch` 구문을 사용하거나 옵셔널 값으로 에러를 처리하거나 에러가 발생하지 않을 것이라고 주장할 수 있습니다. 각 접근은 아래에 설명되어 있습니다.

함수에서 에러가 발생하면 프로그램의 흐름이 변경되므로 코드에서 에러가 발생할 수 있는 위치를 신속하게 알 수 있어야 합니다. 코드에서 이러한 위치를 식별하려면 에러가 발생할 수 있는 함수, 메서드, 또는 초기화 구문 호출하는 코드 이전에 `try` 또는 `try?` 또는 `try!` 키워드를 작성합니다. 이 키워드는 아래에 설명되어 있습니다.

> NOTE Swift에서 에러 처리는 `try`, `catch` 그리고 `throw` 키워드를 사용하는 다른 언어에서 에러 처리와 유사합니다. Objective-C를 포함하여 많은 언어에서의 예외 처리와 달리 Swift에서 에러 처리는 계산 비용이 많이 드는 프로세스 인 호출 스택 해제가 포함되지 않습니다. 따라서 `throw` 구문의 성능 특성은 `return` 구문의 성능 특성과 비슷합니다.

### 던지기 함수를 이용한 에러 전파 \(Propagating Errors Using Throwing Functions\)

에러가 발생할 수 있는 함수, 메서드, 또는 초기화 구문을 나타내기 위해 파라미터 뒤에 함수의 선언에 `throws` 키워드를 작성합니다. `throws` 로 표시된 함수는 _던지기 함수_ 라고 합니다. 함수에 반환 타입이 지정되어 있으면 `throws` 키워드는 반환 화살표 \(`->`\) 전에 작성합니다.

```swift
func canThrowErrors() throws -> String

func cannotThrowErrors() -> String
```

던지기 함수는 내부에서 발생한 에러를 호출된 범위로 전파합니다.

> NOTE 던지기 함수는 에러를 전파만 할 수 있습니다. 던지기 선언이 되지 않은 함수 내에서 발생된 모든 에러는 함수 내에서 처리되어야 합니다.

아래 예제에서 `VendingMachine` 클래스는 요청된 항목이 불가능 하거나 품절이거나 현재 입금액을 초과하는 비용이 있는 경우 적절한 `VendingMachineError` 를 발생하는 `vend(itemNamed:)` 메서드를 가지고 있습니다:

```swift
struct Item {
    var price: Int
    var count: Int
}

class VendingMachine {
    var inventory = [
        "Candy Bar": Item(price: 12, count: 7),
        "Chips": Item(price: 10, count: 4),
        "Pretzels": Item(price: 7, count: 11)
    ]
    var coinsDeposited = 0

    func vend(itemNamed name: String) throws {
        guard let item = inventory[name] else {
            throw VendingMachineError.invalidSelection
        }

        guard item.count > 0 else {
            throw VendingMachineError.outOfStock
        }

        guard item.price <= coinsDeposited else {
            throw VendingMachineError.insufficientFunds(coinsNeeded: item.price - coinsDeposited)
        }

        coinsDeposited -= item.price

        var newItem = item
        newItem.count -= 1
        inventory[name] = newItem

        print("Dispensing \(name)")
    }
}
```

`vend(itemNamed:)` 메서드의 구현은 `guard` 구문을 사용하여 메서드를 일찍 종료시키고 스낵 구매 요구사항 중 하나라도 충족하지 않으면 적절한 에러를 발생합니다. `throw` 구문은 프로그램 제어를 즉시 전달하므로 항목은 요구사항이 모두 만족해야만 판매됩니다.

`vend(itemNamed:)` 메서드는 발생하는 에러를 전파하기 때문에 이 메서드를 호출하는 코드는 `do`-`catch` 구문, `try?` 또는 `try!` 를 사용하여 에러를 처리하거나 계속 전파해야 합니다. 예를 들어 아래 예제에서 `buyFavoriteSnack(person:vendingMachine:)` 은 던지기 함수이며 `vend(itemNamed:)` 메서드에서 발생한 에러는 `buyFavoriteSnack(person:vendingMachine:)` 함수가 호출된 지점까지 전파될 것입니다.

```swift
let favoriteSnacks = [
    "Alice": "Chips",
    "Bob": "Licorice",
    "Eve": "Pretzels",
]
func buyFavoriteSnack(person: String, vendingMachine: VendingMachine) throws {
    let snackName = favoriteSnacks[person] ?? "Candy Bar"
    try vendingMachine.vend(itemNamed: snackName)
}
```

이 예제에서 `buyFavoriteSnack(person:vendingMachine:)` 함수는 주어진 사람의 좋아하는 스낵을 검색하고 `vend(itemNamed:)` 메서드를 호출하여 그 제품을 구입합니다. `vend(itemNamed:)` 메서드는 에러를 발생할 수 있으므로 `try` 키워드를 앞에 두어 호출됩니다.

던지기 초기화 구문은 던지기 함수와 같은 방법으로 에러를 전파할 수 있습니다. 예를 들어 아래의 목록에서 `PurchasedSnack` 구조체의 초기화 구문은 초기화 프로세스 부분으로 던지기 함수를 호출하고 발생하는 모든 에러를 호출자에게 전파하여 처리합니다.

```swift
struct PurchasedSnack {
    let name: String
    init(name: String, vendingMachine: VendingMachine) throws {
        try vendingMachine.vend(itemNamed: name)
        self.name = name
    }
}
```

### Do-Catch 사용하여 에러 처리 \(Handling Errors Using Do-Catch\)

`do`-`catch` 구문을 사용하여 코드의 블럭을 실행하여 에러를 처리합니다. 에러가 `do` 절에서 발생되면 `catch` 절과 비교하여 에러를 처리할 수 있는 항목을 결정합니다.

다음은 `do`-`catch` 구문의 일반적인 형태입니다:

![Error Handling](../.gitbook/assets/17_errorhandling.png)

처리할 수 있는 에러가 무엇인지 나타내기 위해 `catch` 뒤에 패턴을 작성합니다. `catch` 절이 패턴을 가지고 있지 않다면 이 절은 모든 에러와 일치하고 `error` 라는 이름을 가진 지역 상수로 에러를 바인드 합니다. 자세한 내용은 [패턴 \(Patterns\)](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html) 을 참고 바랍니다.

예를 들어 다음 코드는 `VendingMachineError` 열거형에 3가지 모든 케이스에 대해 일치합니다.

```swift
var vendingMachine = VendingMachine()
vendingMachine.coinsDeposited = 8
do {
    try buyFavoriteSnack(person: "Alice", vendingMachine: vendingMachine)
    print("Success! Yum.")
} catch VendingMachineError.invalidSelection {
    print("Invalid Selection.")
} catch VendingMachineError.outOfStock {
    print("Out of Stock.")
} catch VendingMachineError.insufficientFunds(let coinsNeeded) {
    print("Insufficient funds. Please insert an additional \(coinsNeeded) coins.")
} catch {
    print("Unexpected error: \(error).")
}
// Prints "Insufficient funds. Please insert an additional 2 coins."
```

위의 예제에서 `buyFavoriteSnack(person:vendingMachine:)` 함수는 에러를 발생할 수 있으므로 `try` 표현식으로 호출됩니다. 에러가 발생하면 실행이 즉시 `catch` 절로 전송되어 전파가 계속 될 것인지 여부를 결정합니다. 패턴이 일치하지 않으면 에러는 마지막 `catch` 절에 의해 포착되고 지역 `error` 상수에 바인딩 됩니다. 에러가 발생하지 않으면 `do` 구문에 나머지 구문이 실행됩니다.

`catch` 절은 `do` 절에서 발생할 수 있는 모든 에러를 처리할 필요는 없습니다. `catch` 절이 에러를 처리하지 않으면 에러는 주변에 전파합니다. 그러나 전파된 에러는 _일부_ 주변 범위에서 처리되어야 합니다. 던지지 않는 함수에서는 `do`-`catch` 구문은 에러를 처리해야 합니다. 던지는 함수에서는 `do`-`catch` 구문이나 호출자가 에러를 처리해야 합니다. 에러가 처리되지 않고 범위의 최상위로 전파되면 런타임 에러를 발생합니다.

예를 들어 위의 예제는 `VendingMachineError` 가 아닌 모든 에러가 호출 함수에서 포착되도록 작성할 수 있습니다:

```swift
func nourish(with item: String) throws {
    do {
        try vendingMachine.vend(itemNamed: item)
    } catch is VendingMachineError {
        print("Couldn't buy that from the vending machine.")
    }
}

do {
    try nourish(with: "Beet-Flavored Chips")
} catch {
    print("Unexpected non-vending-machine-related error: \(error)")
}
// Prints "Couldn't buy that from the vending machine."
```

`nourish(with:)` 함수에서 `vend(itemNamed:)` 가 `VendingMachineError` 열거형에 케이스 중 하나의 에러를 발생하면 `nourish(with:)` 는 메세지를 출력하여 에러를 처리합니다. 그렇지 않으면 `nourish(with:)` 는 호출 부분으로 에러를 전파합니다. 이 에러는 일반적인 `catch` 절에 의해 포착됩니다.

연관된 에러를 포착하기 위한 다른 방법은 콤마로 구분하여 `catch` 다음에 목록 형식으로 작성하는 것입니다. 예를 들어:

```swift
func eat(item: String) throws {
    do {
        try vendingMachine.vend(itemNamed: item)
    } catch VendingMachineError.invalidSelection, VendingMachineError.insufficientFunds, VendingMachineError.outOfStock {
        print("Invalid selection, out of stock, or not enough money.")
    }
}
```

`eat(item:)` 함수는 포착할 자동 판매기 에러를 나열하며 에러 텍스트는 해당 목록의 항목에 해당합니다. 목록화 된 3가지 에러 중 어떤 에러가 발생하면 이 `catch` 절은 메세지를 출력하여 처리합니다. 나중에 추가될 수 있는 에러를 포함하여 다른 에러는 주변 범위로 전파됩니다.

### 에러를 옵셔널 값을 변환 \(Converting Errors to Optional Values\)

에러를 옵셔널 값으로 변환하여 처리하기 위해 `try?` 를 사용합니다. `try?` 표현식을 평가하는 동안 에러가 발생되면 이 표현식의 값은 `nil` 입니다. 예를 들어 아래 코드에서 `x` 와 `y` 는 같은 값을 가지고 동작합니다:

```swift
func someThrowingFunction() throws -> Int {
    // ...
}

let x = try? someThrowingFunction()

let y: Int?
do {
    y = try someThrowingFunction()
} catch {
    y = nil
}
```

`shomeThrowingFunction()` 이 에러를 발생하면 `x` 와 `y` 의 값은 `nil` 입니다. 그렇지 않으면 `x` 와 `y` 의 값은 반환된 함수 값입니다. `x` 와 `y` 는 `someThrowingFunction()` 이 반환하는 타입의 옵셔널 입니다. 여기서 함수는 정수를 반환하므로 `x` 와 `y` 는 옵셔널 옵셔널 정수입니다.

`try?` 를 사용하면 모든 에러를 같은 방식으로 처리하려는 경우 간결한 에러 처리 코드를 작성할 수 있습니다. 예를 들어 아래의 코드는 여러 접근방식을 사용하여 데이터를 가져오거나 모든 접근방식이 실패하면 `nil` 을 반환합니다.

```swift
func fetchData() -> Data? {
    if let data = try? fetchDataFromDisk() { return data }
    if let data = try? fetchDataFromServer() { return data }
    return nil
}
```

### 에러 전파 비활성화 \(Disabling Error Propagation\)

가끔 던지는 함수 또는 메서드가 실제로 런타임 에러를 발생하지 않는다는 사실을 알고 있습니다. 이러한 경우 표현식 전에 에러 전파를 비활성화 하기 위해 `try!` 를 작성할 수 있고 에러를 발생하지 않는다고 호출을 래핑할 수 있습니다. 에러가 발생한다면 런타임 에러를 얻습니다.

예를 들어 다음의 코드는 주어진 경로의 이미지를 로드하거나 이미지를 로드할 수 없을 때는 에러를 발생하는 `loadImage(atPath:)` 함수를 사용합니다. 이러한 경우 이미지는 이미지는 애플리케이션과 함께 제공되고 런타임에 에러가 발생하지 않으므로 에러 전파를 비활성화 하는 것이 적절합니다.

```swift
let photo = try! loadImage(atPath: "./Resources/John Appleseed.jpg")
```

## 정리 작업 지정 \(Specifying Cleanup Actions\)

코드 실행이 코드의 현재 블럭이 종료되기 직전에 `defer` 구문을 사용하여 일련의 구문을 실행합니다. 이 구문을 사용하여 에러가 발생하여 종료되거나 `return` 또는 `break` 와 같은 구문에 의해 종료되는 방식에 상관없이 필요한 정리를 수행할 수 있습니다. 예를 들어 `defer` 구문을 사용하여 파일 설명자가 닫히고 수동으로 할당된 메모리가 해제되도록 할 수 있습니다.

`defer` 구문은 현재 범위가 종료될 때까지 실행을 연기합니다. 이 구문은 `defer` 키워드와 나중에 실행될 구문으로 구성되어 있습니다. 지연된 구문은 `break` 또는 `return` 구문과 같이 구문의 밖으로 제어를 이동하거나 에러를 발생시키는 코드를 포함할 수 없습니다. 지연된 동작은 소스 코드에 작성된 순서와 반대로 실행됩니다. 즉 첫번째 `defer` 구문의 코드는 마지막에 실행되고 두번째 `defer` 구문의 코드는 마지막에서 두번째로 실행되는 식입니다. 소스 코드의 마지막 `defer` 구문은 마지막에 실행합니다.

```swift
func processFile(filename: String) throws {
    if exists(filename) {
        let file = open(filename)
        defer {
            close(file)
        }
        while let line = try file.readline() {
            // Work with the file.
        }
        // close(file) is called here, at the end of the scope.
    }
}
```

위의 예제는 `open(_:)` 함수에 `close(_:)` 에 대한 호출이 있는지 확인하기 위해 `defer` 구문을 사용합니다.

> NOTE 에러 처리 코드가 포함되지 않았을 때도 `defer` 구문을 사용할 수 있습니다.

