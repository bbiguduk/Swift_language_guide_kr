# 중첩 타입 (Nested Types)

타입의 범위 내에 다른 타입을 정의합니다.

열거형은 특정 클래스나 구조체의 기능을 지원하기 위해 생성합니다.
유사하게 더 복잡한 타입의 컨텍스트 내에서 사용하기 위해
유틸리티 구조체와 일반적으로 특정 타입과 함께 사용되는 프로토콜을
정의하는 것이 편리할 수 있습니다.
이를 위해 Swift는 *중첩 타입(nested types)*을 정의할 수 있으며,
지원하는 타입의 정의 내에서
열거형, 구조체, 프로토콜과 같은 타입을 중첩할 수 있습니다.

다른 타입 내에서 타입을 중첩하려면
지원하는 타입의 외부 중괄호 내에 정의를 작성해야 합니다.
타입은 필요에 따라 원하는 만큼 여러 단계로 중첩할 수 있습니다.

## 중첩 타입의 동작 (Nested Types in Action)

아래의 예시는 블랙잭 게임에서 사용되는 카드 게임을 모델링하는
`BlackjackCard`라는 구조체를 정의합니다.
`BlackjackCard` 구조체는 `Suit`와 `Rank`라는
두 중첩된 열거형을 포함합니다.

블랙잭에서 에이스 카드의 값은 1이나 11입니다.
이러한 특징은 `Rank` 열거형 내에 중첩된
`Values`라는 구조체로 나타냅니다:

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

<!--
  - test: `nestedTypes`

  ```swifttest
  -> struct BlackjackCard {

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
-->

`Suit` 열거형은 기호를 나타내기 위해 `Character` 타입을 원시값으로
네 가지 일반적인 카드 모양을 나타냅니다.

`Rank` 열거형은 카드 값을 나타내기 위해 `Int` 타입을 원시값으로
가능한 13가지 카드 순위를 나타냅니다.
(이 `Int` 타입의 원시값은 Jack, Queen, King, Ace 카드에는 사용되지 않습니다.)

위에서 말했듯이 `Rank` 열거형은
`Values`라는 자신의 중첩된 구조체를 더 정의합니다.
이 구조체는 대부분의 카드는 하나의 값을 가지지만
에이스 카드는 두 값을 가지는 사실을 캡슐화합니다.
`Values` 구조체는 다음과 같이 두 프로퍼티를 정의합니다:

- `Int` 타입의 `first`
- `Int?`나 "옵셔널 `Int`" 타입의 `second`

`Rank`는 `Values` 구조체의 인스턴스를 반환하는
`values`라는 연산 프로퍼티도 정의합니다.
이 연산 프로퍼티는 카드의 순위를 고려하고
그 순위를 기반으로 적절한 값으로 새로운 `Values` 인스턴스를 초기화합니다.
`jack`, `queen`, `king`, `ace`에 대한 특별한 값을 사용합니다.
숫자 카드에 대해서는 순위의 `Int` 타입의 원시값을 사용합니다.

`BlackjackCard` 구조체는 `rank`와 `suit`의 두 프로퍼티를 가지고 있습니다.
이름과 카드의 값 설명을 만들기 위해
`rank`와 `suit`에 저장된 값을 사용하는
`description`이라는 연산 프로퍼티도 정의합니다.
`description` 프로퍼티는 화면에 표시하기 위해
두 번째 값이 있는지 확인하고 있으면,
두 번째 값에 대해 상세 설명을 추가합니다.

`BlackjackCard`는 커스텀 이니셜라이저가 없는 구조체이기 때문에,
[구조체 타입의 멤버와이즈 이니셜라이저 (Memberwise Initializers for Structure Types)](./initialization.md#구조체-타입의-멤버와이즈-이니셜라이저-memberwise-initializers-for-structure-types)에서 설명했듯이
암시적 멤버별 이니셜라이저를 가지고 있습니다.
`theAceOfSpades`라는 새로운 상수를 초기화하기 위해 이니셜라이저를 사용할 수 있습니다:

```swift
let theAceOfSpades = BlackjackCard(rank: .ace, suit: .spades)
print("theAceOfSpades: \(theAceOfSpades.description)")
// Prints "theAceOfSpades: suit is ♠, value is 1 or 11"
```

<!--
  - test: `nestedTypes`

  ```swifttest
  -> let theAceOfSpades = BlackjackCard(rank: .ace, suit: .spades)
  -> print("theAceOfSpades: \(theAceOfSpades.description)")
  <- theAceOfSpades: suit is ♠, value is 1 or 11
  ```
-->

`Rank`와 `Suit`는 `BlackjackCard` 내에 중첩되어 있지만,
타입은 컨텍스트로부터 유추될 수 있기 때문에
이 인스턴스의 초기화는 케이스 이름(`.ace`와 `.spades`)만으로
열거형 케이스를 참조할 수 있습니다.
위의 예시에서 `description` 프로퍼티는
스페이드의 에이스는 `1`이나 `11`의 값을 가지고 있다고 올바르게 보여줍니다.

## 중첩 타입 참조 (Referring to Nested Types)

중첩 타입을 그 정의된 범위 밖에서 사용하려면
그 타입이 중첩ㅚㄴ 외부 타입의 이름을 접두사로 붙여야 합니다:

```swift
let heartsSymbol = BlackjackCard.Suit.hearts.rawValue
// heartsSymbol is "♡"
```

<!--
  - test: `nestedTypes`

  ```swifttest
  -> let heartsSymbol = BlackjackCard.Suit.hearts.rawValue
  /> heartsSymbol is \"\(heartsSymbol)\"
  </ heartsSymbol is "♡"
  ```
-->

위의 예시에서
`Suit`, `Rank`, `Values`의 이름은 정의된 컨텍스트에 따라 자연스럽게 규정되기 때문에
이름을 의도적으로 짧게 유지할 수 있습니다.

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->