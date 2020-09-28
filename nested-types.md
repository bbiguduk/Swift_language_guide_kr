# 중첩된 타입 \(Nested Types\)

열거형은 특정 클래스 또는 구조체의 기능을 지원하기 위해 생성됩니다. 유사하게 더 복잡한 타입의 컨텍스트 내에서 사용하기 위해 순수하게 유틸리티 클래스와 구조체를 정의하는 것이 편리할 수 있습니다. 이를 위해 Swift는 _중첩된 타입 \(nested types\)_ 을 정의할 수 있으며 지원하는 타입의 정의 내에서 지원하는 열거형, 클래스, 그리고 구조체를 중첩할 수 있습니다.

다른 타입 내에서 타입을 중첩하려면 지원하는 타입의 외부 중괄호 내에 정의를 작성해야 합니다. 타입은 필요한만큼의 수준으로 중첩될 수 있습니다.

## 중첩된 타입의 동작 \(Nested Types in Action\)

아래의 예제는 블랙잭 게임에서 사용되는 게임 카드를 모델링하는 `BlackjackCard` 라는 구조체를 정의합니다. `BlackjackCard` 구조체는 `Suit` 와 `Rank` 라는 2개의 중첩된 열거형을 포함합니다.

블랙잭에서 에이스 카드의 값은 1 또는 11 입니다. 이러한 특징은 `Rank` 열거형 내에 중첩된 `Values` 라는 구조체로 나타냅니다:

```swift
struct BlackjackCard {

    // nested Suit enumeration
    enum Suit: Character {
        case spades = "♠", hearts = "♡", diamonds = "♢", clubs = "♣"
    }

    // nested Rank enumeration
    enum Rank: Int {
        case two = 2, three, four, five, six, seven, eight, nine, ten
        case jack, queen, king, ace
        struct Values {
            let first: Int, second: Int?
        }
        var values: Values {
            switch self {
            case .ace:
                return Values(first: 1, second: 11)
            case .jack, .queen, .king:
                return Values(first: 10, second: nil)
            default:
                return Values(first: self.rawValue, second: nil)
            }
        }
    }

    // BlackjackCard properties and methods
    let rank: Rank, suit: Suit
    var description: String {
        var output = "suit is \(suit.rawValue),"
        output += " value is \(rank.values.first)"
        if let second = rank.values.second {
            output += " or \(second)"
        }
        return output
    }
}
```

`Suit` 열거형은 기호를 나타내는 원시 `Character` 값과 함께 4개의 일반적인 카드 모양을 나타냅니다.

`Rank` 열거형은 액면가를 나타내는 원시 `Int` 값과 함께 가능한 13개의 카드 순위를 나타냅니다 \(이 원시 `Int` 값은 Jack, Queen, King, 그리고 Ace 카드에는 사용되지 않습니다\).

위에서 말했듯이 `Rank` 열거형은 `Values` 라는 자신의 중첩된 구조체를 더 정의합니다. 이 구조체는 대부분의 카드는 하나의 값을 가지지만 에이스 카드는 2개의 값을 가지는 사실을 캡슐화 합니다. `Values` 구조체는 아래는 나타내기 위한 2개의 프로퍼티를 정의합니다:

* `Int` 타입의 `first`
* `Int?` 또는 "옵셔널 `Int`" 타입의 `second`

`Rank` 는 `Values` 구조체의 인스턴스를 반환하는 `values` 라는 계산된 프로퍼티도 정의합니다. 이 계산된 프로퍼티는 카드의 순위를 고려하고 그 순위를 기반으로 적절한 값으로 새로운 `Values` 인스턴스를 초기화 합니다. `jack`, `queen`, `king`, 그리고 `ace` 에 대한 특별한 값을 사용합니다. 숫자 카드에 대해서는 순위의 원시 `Int` 값을 사용합니다.

`BlackjackCard` 구조체 자체는 `rank` 와 `suit` 의 2개의 프로퍼티를 가지고 있습니다. 이름과 카드의 값의 설명을 만들기 위해 `rank` 와 `suit` 에 저장된 값을 사용하는 `description` 이라는 계산된 프로퍼티도 정의합니다. `description` 프로퍼티는 화면에 표시하기 위해 두번째 값이 있는지 확인하고 있으면 두번째 값에 대해 상세 설명을 추가합니다.

`BlackjackCard` 는 사용자 지정 초기화 구문이 없는 구조체이기 때문에 [구조체 타입에 대한 멤버별 초기화 구문 \(Memberwise Initializers for Structure Types\)](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html#ID214) 에서 설명 했듯이 암시적 멤버별 초기화 구문을 가지고 있습니다. `theAceOfSpades` 라는 새로운 상수를 초기화 하기 위해 초기화 구문을 사용할 수 있습니다:

```swift
let theAceOfSpades = BlackjackCard(rank: .ace, suit: .spades)
print("theAceOfSpades: \(theAceOfSpades.description)")
// Prints "theAceOfSpades: suit is ♠, value is 1 or 11"
```

`Rank` 와 `Suit` 은 `BlackjackCard` 내에 중첩되어 있지만 타입은 컨텍스트로 부터 유추될 수 있기 때문에 이 인스턴스의 초기화는 케이스 이름 \(`.ace` 와 `.spades`\) 만으로 열거형 케이스를 참조할 수 있습니다. 위의 예제에서 `description` 프로퍼티는 스페이드의 에이스는 `1` 또는 `11` 의 값을 가지고 있다고 올바르게 보고 합니다.

## 중첩된 타입 참조 \(Referring to Nested Types\)

정의 컨텍스트 외부에서 중첩된 타입을 사용하기 위해 해당 이름에 중첩된 타입의 이름을 접두사로 붙입니다:

```swift
let heartsSymbol = BlackjackCard.Suit.hearts.rawValue
// heartsSymbol is "♡"
```

위의 예제에서 `Suit`, `Rank` 그리고 `Values` 의 이름은 정의된 컨텍스트에 따라 자연스럽게 규정되기 때문에 의도적으로 짧게 유지할 수 있습니다.

