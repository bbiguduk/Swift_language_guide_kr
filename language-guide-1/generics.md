# 제네릭 (Generics)

여러 타입에 대해 동작하는 코드를 작성하고 해당 타입의 요구사항을 지정합니다.

*제네릭 코드(Generic code)*는 정의한 요구사항에 따라
모든 타입에서 동작할 수 있는 유연하고 재사용 가능한 함수와 타입을 작성할 수 있습니다.
중복을 피하고
명확하고 추상적인 방식으로 의도를 표현하는 코드를 작성할 수 있습니다.

제네릭은 Swift의 강력한 기능 중 하나이고,
Swift 표준 라이브러리의 많은 부분이 제네릭 코드로 작성되어 있습니다.
사실 모르고 있더라도
*Language Guide* 전체에서 제네릭을 사용합니다.
예를 들어 Swift의 `Array`와 `Dictionary` 타입은
둘 다 제네릭 컬렉션 입니다.
`Int` 값을 가진 배열이나
`String` 값을 가진 배열
또는 실제로 Swift에서 생성할 수 있는 다른 모든 타입에 대한 배열을 생성할 수 있습니다.
마찬가지로 모든 지정된 타입의 값을 저장하기 위한 딕셔너리를 생성할 수 있고
해당 타입에 대한 제한은 없습니다.

## 제네릭이 해결하는 문제 (The Problem that Generics Solve)

다음은 두 `Int` 값을 바꾸는
`swapTwoInts(_:_:)`라는 제네릭이 아닌 함수를 나타냅니다:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

<!--
  - test: `whyGenerics`

  ```swifttest
  -> func swapTwoInts(_ a: inout Int, _ b: inout Int) {
        let temporaryA = a
        a = b
        b = temporaryA
     }
  ```
-->

이 함수는 [In-Out 매개변수 (In-Out Parameters)](./functions.md#in-out-매개변수-in-out-parameters)에서 설명한대로
`a`와 `b`의 값을 바꾸기 위해 in-out 매개변수를 사용하여 만듭니다.

`swapTwoInts(_:_:)` 함수는 `b`의 값을 `a`로
그리고 `a`의 값을 `b`로 바꿉니다.
두 개의 `Int` 변수의 값을 바꾸기 위해 이 함수를 호출할 수 있습니다:

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// Prints "someInt is now 107, and anotherInt is now 3"
```

<!--
  - test: `whyGenerics`

  ```swifttest
  -> var someInt = 3
  -> var anotherInt = 107
  -> swapTwoInts(&someInt, &anotherInt)
  -> print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
  <- someInt is now 107, and anotherInt is now 3
  ```
-->

`swapTwoInts(_:_:)` 함수는 유용하지만 `Int` 값만 사용이 가능합니다.
두 개의 `String` 값이나
두 개의 `Double` 값을 바꾸길 원하면
아래의 `swapTwoStrings(_:_:)`와 `swapTwoDoubles(_:_:)` 함수와 같이
더 많은 함수를 작성해야 합니다:

```swift
func swapTwoStrings(_ a: inout String, _ b: inout String) {
    let temporaryA = a
    a = b
    b = temporaryA
}

func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

<!--
  - test: `whyGenerics`

  ```swifttest
  -> func swapTwoStrings(_ a: inout String, _ b: inout String) {
        let temporaryA = a
        a = b
        b = temporaryA
     }

  -> func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
        let temporaryA = a
        a = b
        b = temporaryA
     }
  ```
-->

`swapTwoInts(_:_:)`, `swapTwoStrings(_:_:)`, `swapTwoDoubles(_:_:)` 함수의
본문이 동일하다는 것을 알 수 있습니다.
차이점은 받아들이는 값의 타입만 다릅니다
(`Int`, `String`, `Double`).

*모든* 타입의 두 값을 바꾸는 단일 함수로 작성하면
더 유용하고 유연합니다.
제네릭 코드는 이러한 함수를 작성할 수 있습니다.
(이 함수의 제네릭 버전은 아래에 정의되어 있습니다.)

> Note: 이 세 개의 함수는
> `a`와 `b`의 타입이 모두 같아야 합니다.
> `a`와 `b`가 같은 타입이 아니면
> 바꾸는 것은 불가능합니다.
> Swift는 타입 안정 언어이고
> `String` 타입의 변수와
> `Double` 타입의 변수가
> 서로 값을 바꾸도록 허락하지 않습니다.
> 이러한 시도는 컴파일 오류가 발생합니다.

## 제네릭 함수 (Generic Functions)

*제네릭 함수(Generic functions)*는 모든 타입과 함께 동작할 수 있습니다.
다음은 `swapTwoValues(_:_:)`라는
위의 `swapTwoInts(_:_:)` 함수의 제네릭 버전입니다:

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

<!--
  - test: `genericFunctions`

  ```swifttest
  -> func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
        let temporaryA = a
        a = b
        b = temporaryA
     }
  ```
-->

<!--
  This could be done in one line using a tuple pattern: (a, b) = (b, a)
  That's probably not as approachable here, and the novel syntax to avoid an
  explicit placeholder variable might distract from the discussion of
  generics.
-->

`swapTwoValues(_:_:)` 함수의 본문은
`swapTwoInts(_:_:)` 함수의 본문과 동일합니다.
그러나 `swapTwoValues(_:_:)`의 첫 번째 줄은
`swapTwoInts(_:_:)`와 약간 다릅니다.
다음은 첫 번째 줄이 어떻게 다른지 보여줍니다:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int)
func swapTwoValues<T>(_ a: inout T, _ b: inout T)
```

<!--
  - test: `genericFunctionsComparison`

  ```swifttest
  -> func swapTwoInts(_ a: inout Int, _ b: inout Int)
  >> {
  >>    let temporaryA = a
  >>    a = b
  >>    b = temporaryA
  >> }
  -> func swapTwoValues<T>(_ a: inout T, _ b: inout T)
  >> {
  >>    let temporaryA = a
  >>    a = b
  >>    b = temporaryA
  >> }
  ```
-->

함수의 제네릭 버전은
`Int`, `String`, `Double`과 같은 *실제* 타입 이름 대신에
이 경우 `T`라는 *임의의* 타입 이름을 사용합니다.
이 임의의 타입 이름 `T`는 어떤 타입인지 나타내지 않지만
`T`가 무슨 타입이든
`a`와 `b`는 모두 같은 타입이어야 한다고 나타냅니다.
`T`의 실제 타입은
`swapTwoValues(_:_:)` 함수가 호출될 때마다 결정됩니다.

제네릭 함수와 제네릭이 아닌 함수 사이의 다른 차이점은
제네릭 함수의 이름(`swapTwoValues(_:_:)`)에
바로 임의의 타입 이름(`T`)이 꺾쇠 괄호 내(`<T>`)에 위치한다는 것입니다.
이 괄호는 `T`는 `swapTwoValues(_:_:)` 함수 정의 내에서
임의의 타입 이름이라고 Swift에게 말합니다.
`T`는 임의의 타입이므로, Swift는 `T`라는 실제 타입을 찾지 않습니다.

`swapTwoValues(_:_:)` 함수는 이제 `swapTwoInts`와 동일한 방식으로 호출할 수 있지만
두 값이 서로 동일한 타입이면
*모든* 타입의 두 값을 전달할 수 있다는 점이 다릅니다.
`swapTwoValues(_:_:)`가 호출될 때마다
`T`로 사용한 타입은 함수에 전달된 값의 타입으로 추론합니다.

아래의 두 예시에서 `T`는 각각 `Int`와 `String`으로 추론됩니다:

```swift
var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)
// someInt is now 107, and anotherInt is now 3

var someString = "hello"
var anotherString = "world"
swapTwoValues(&someString, &anotherString)
// someString is now "world", and anotherString is now "hello"
```

<!--
  - test: `genericFunctions`

  ```swifttest
  -> var someInt = 3
  -> var anotherInt = 107
  -> swapTwoValues(&someInt, &anotherInt)
  /> someInt is now \(someInt), and anotherInt is now \(anotherInt)
  </ someInt is now 107, and anotherInt is now 3

  -> var someString = "hello"
  -> var anotherString = "world"
  -> swapTwoValues(&someString, &anotherString)
  /> someString is now \"\(someString)\", and anotherString is now \"\(anotherString)\"
  </ someString is now "world", and anotherString is now "hello"
  ```
-->

> Note: 위에 정의된 `swapTwoValues(_:_:)` 함수는
> Swift 표준 라이브러리에 포함된
> `swap`이라는 제네릭 함수에 의해 영감을 받았습니다.
> 자체 코드에서 `swapTwoValues(_:_:)` 함수의 동작이 필요하다면,
> 직접 구현한 함수보다 Swift에 존재하는 `swap(_:_:)` 함수를 사용할 수 있습니다.

## 타입 매개변수 (Type Parameters)

위의 `swapTwoValues(_:_:)` 예시에서
임의의 타입 `T`는 *타입 매개변수(type parameter)*의 예입니다.
타입 매개변수는 임의의 타입을 지정하고 이름을 지정하며,
함수 이름 바로 뒤에
꺾쇠 괄호(예: `<T>`) 사이에 작성합니다.

타입 매개변수를 지정하면
함수의 매개변수 타입
(`swapTwoValues(_:_:)` 함수의 `a`와 `b` 매개변수),
함수의 반환 타입,
함수의 본문 내에 타입 어노테이션 등에 사용할 수 있습니다.
각각의 경우 타입 매개변수는
함수가 호출될 때 *실제* 타입으로 대체됩니다.
(위의 `swapTwoValues(_:_:)` 예시에서
`T`는 첫 번째 함수가 호출될 때 `Int`로 대체되고,
두 번째 호출될 때 `String`으로 대체됩니다.)

여러 타입 매개변수가 필요하다면
꺾쇠 괄호 안에 타입 매개변수 이름을 콤마로 구분해
나열할 수 있습니다.

## 타입 매개변수 이름 (Naming Type Parameters)

대부분의 경우 타입 매개변수는 타입 매개변수와
제네릭 타입 간의 관계나 함수 간의 관계를 나타내기 위해
`Dictionary<Key, Value>`에서 `Key`와 `Value`
그리고 `Array<Element>`에서 `Element`와 같이
설명이 포함된 이름을 사용합니다.
그러나 의미있는 관계가 없을 때는
위의 `swapTwoValues(_:_:)` 함수에서 `T`와 같이
`T`, `U`, `V`와 같은 단일 문자를 사용하여 이름을 지정하는 것이 일반적입니다.

타입 매개변수에 `T`와 `MyTypeParameter`와 같이
대문자 카멜 케이스 이름을 사용하여
값이 아닌 *타입*을 위한 플레이스 홀더라고 나타냅니다.

> Note:
> 타입 매개변수에 이름이 필요하지 않고
> 제네릭 타입 제약조건이 단순하다면,
> [불투명 매개변수 타입 (Opaque Parameter Types)](./opaque-types.md#불투명-매개변수-타입-opaque-parameter-types)에서 설명한대로
> 경량 문법을 사용할 수 있습니다.
<!--
Comparison between this syntax and the lightweight syntax
is in the Opaque Types chapter, not here ---
the reader hasn't learned about constraints yet,
so it wouldn't make sense to list what is/isn't supported.
-->

## 제네릭 타입 (Generic Types)

제네릭 함수 외에도
Swift에서는 *제네릭 타입(generic types)*을 정의할 수 있습니다.
제네릭 타입은 `Array`나 `Dictionary`처럼
*모든* 타입에서 동작할 수 있는 커스텀 클래스, 구조체, 열거형입니다.

이번 섹션은 `Stack`이라는 제네릭 컬렉션 타입을 어떻게 작성하는지 보여줍니다.
스택은 배열과 유사하게 순서가 있는 값의 집합이지만,
Swift의 `Array` 타입보다 더 제한된 연산만 허용합니다.
배열은 모든 위치에 값을 삽입하고 제거할 수 있습니다.
그러나 스택은 값을 컬렉션의 끝에 추가하는 것만 허락합니다
(새로운 값을 *푸시(push)*한다고 표현합니다).
마찬가지로 스택은 컬렉션의 끝부분의 값만 제거할 수 있습니다
(값을 *팝(pop)*한다고 표현합니다).

> Note: 스택의 개념은 네비게이션 계층도에서 뷰 컨트롤러를 모델링 하는
> `UINavigationController` 클래스에서 사용됩니다.
> `UINavigationController` 클래스에서
> 네비게이션 스택에 뷰 컨트롤러를 추가(푸시)하기 위해
> `pushViewController(_:animated:)` 메서드를 호출하고
> 네비게이션 스택에 뷰 컨트롤러를 삭제(팝)하기 위해
> `popViewControllerAnimated(_:)` 메서드를 호출합니다.
> 스택은 컬렉션 관리에서
> "후입 선출(LIFO)" 접근방식이 필요할 때 유용한 컬렉션 모델입니다.

아래의 그림은 스택에 대한 푸시와 팝에 대한 동작을 보여줍니다:

![Stack Push Pop](../.gitbook/assets/stackPushPop_2x~dark.png)

1. 스택에 현재 세 개의 값이 있습니다.
2. 네 번째 값은 스택의 상단에 푸시됩니다.
3. 스택은 이제 네 개의 값을 가지며 최근 값이 가장 상단에 위치합니다.
4. 스택의 최상단 항목을 팝합니다.
5. 값을 팝한 후에 스택은 다시 세 개의 값만 가집니다.

다음은 스택의 제네릭이 아닌 버전을 어떻게 작성하는지 나타내며,
이 경우 `Int` 값의 스택에 대한 것을 보여줍니다:

```swift
struct IntStack {
    var items: [Int] = []
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
}
```

<!--
  - test: `genericStack`

  ```swifttest
  -> struct IntStack {
        var items: [Int] = []
        mutating func push(_ item: Int) {
           items.append(item)
        }
        mutating func pop() -> Int {
           return items.removeLast()
        }
     }
  >> var intStack = IntStack()
  >> intStack.push(1)
  >> intStack.push(2)
  >> intStack.push(3)
  >> intStack.push(4)
  >> print("the stack now contains \(intStack.items.count) integers")
  << the stack now contains 4 integers
  ```
-->

이 구조체는 스택에 값을 저장하기 위해 `items`라는 `Array` 프로퍼티를 사용합니다.
`Stack`은 스택에 값을 푸시하고 팝하기 위해
`push`와 `pop`인 두 개의 메서드를 제공합니다.
구조체의 `items` 배열을 수정하거나 *변경*이 필요하므로
이 메서드는 `mutating`으로 표시됩니다.

그러나 위에 `IntStack` 타입은 `Int` 값만 사용할 수 있습니다.
*모든* 타입의 값으로 스택을 관리할 수 있는
제네릭 `Stack` 구조체를 정의하는 것이 더 유용합니다.

다음은 같은 코드의 제네릭 버전입니다:

```swift
struct Stack<Element> {
    var items: [Element] = []
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
```

<!--
  - test: `genericStack`

  ```swifttest
  -> struct Stack<Element> {
        var items: [Element] = []
        mutating func push(_ item: Element) {
           items.append(item)
        }
        mutating func pop() -> Element {
           return items.removeLast()
        }
     }
  ```
-->

`Stack`의 제네릭 버전은
기본적으로 제네릭이 아닌 버전과 동일하지만,
`Int`의 실제 타입 대신에
`Element`라는 타입 매개변수를 사용합니다.
이 타입 매개변수는 구조체의 이름 바로 다음에
꺾쇠 괄호 내에(`<Element>`) 작성합니다.

`Element`는 나중에 제공할 타입에 대한
임의의 이름을 정의합니다.
이 타입은 구조체의 정의 내 어디서나
`Element`로 참조할 수 있습니다.
이 경우에 `Element`는 아래의 세 군데에서 사용됩니다:

- `items`라는 프로퍼티를 생성할 때,
  `Element` 타입의 빈 배열로 초기화
- `item`이라는 단일 매개변수를 가지는 `push(_:)` 메서드를 지정할 때
  `Element` 타입의 매개변수
- `pop()` 메서드에 의해 반환된 값이
  `Element` 타입의 값을 반환

이제 제네릭 타입이므로
`Stack`은 `Array`나 `Dictionary`처럼
Swift에서 *모든* 유효한 타입의 스택을 생성하기위해 사용할 수 있습니다.

꺾쇠 괄호 내에 스택에 저장될 타입을 작성하여
새로운 `Stack` 인스턴스를 생성합니다.
예를 들어 문자열의 새로운 스택을 생성하려면,
`Stack<String>()`이라 작성합니다:

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")
stackOfStrings.push("cuatro")
// the stack now contains 4 strings
```

<!--
  - test: `genericStack`

  ```swifttest
  -> var stackOfStrings = Stack<String>()
  -> stackOfStrings.push("uno")
  -> stackOfStrings.push("dos")
  -> stackOfStrings.push("tres")
  -> stackOfStrings.push("cuatro")
  /> the stack now contains \(stackOfStrings.items.count) strings
  </ the stack now contains 4 strings
  ```
-->

다음은 `stackOfStrings`가 스택에 네 개의 값을 푸쉬한 후를 나타냅니다:

![Stack Pushed Four Strings](../.gitbook/assets/stackPushedFourStrings_2x~dark.png)

스택에 값을 팝하여 제거하고 반환되는 값은 `"cuatro"`입니다:

```swift
 let fromTheTop = stackOfStrings.pop()
// fromTheTop is equal to "cuatro", and the stack now contains 3 strings
```

<!--
  - test: `genericStack`

  ```swifttest
  -> let fromTheTop = stackOfStrings.pop()
  /> fromTheTop is equal to \"\(fromTheTop)\", and the stack now contains \(stackOfStrings.items.count) strings
  </ fromTheTop is equal to "cuatro", and the stack now contains 3 strings
  ```
-->

다음은 스택이 값을 팝한 후를 보여줍니다:

![Stack Poped One String](../.gitbook/assets/stackPoppedOneString_2x~dark.png)

## 제네릭 타입 확장 (Extending a Generic Type)

제네릭 타입을 확장할 때,
확장의 정의에 타입 매개변수 목록을 제공하지 않습니다.
대신에 *기존* 타입 정의의 타입 매개변수 목록이
확장의 본문 내에서 사용 가능하고
기존 타입 매개변수 이름을
그대로 사용할 수 있습니다.

다음의 예시는 스택에 팝 없이 스택의 가장 상단의 항목을 반환하는
`topItem`이라는 읽기 전용 연산 프로퍼티를
추가하기 위해 제네릭 `Stack` 타입을 확장합니다:

```swift
extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}
```

<!--
  - test: `genericStack`

  ```swifttest
  -> extension Stack {
        var topItem: Element? {
           return items.isEmpty ? nil : items[items.count - 1]
        }
     }
  ```
-->

`topItem` 프로퍼티는 `Element` 타입의 옵셔널 값을 반환합니다.
스택이 비어있다면 `topItem`은 `nil`을 반환하고;
스택이 비어 있지 않다면 `topItem`은 `items` 배열의 마지막 항목을 반환합니다.

이 확장은 타입 매개변수 목록을 정의하지 않습니다.
대신에 `Element`라는 `Stack` 타입의 타입 매개변수 이름을
`topItem` 연산 프로퍼티의
옵셔널 타입으로 확장 내에서 사용합니다.

`topItem` 연산 프로퍼티는 최상단 항목의 삭제없이 접근하고 조회하기 위해
모든 `Stack` 인스턴스에서 사용할 수 있습니다.

```swift
if let topItem = stackOfStrings.topItem {
    print("The top item on the stack is \(topItem).")
}
// Prints "The top item on the stack is tres."
```

<!--
  - test: `genericStack`

  ```swifttest
  -> if let topItem = stackOfStrings.topItem {
        print("The top item on the stack is \(topItem).")
     }
  <- The top item on the stack is tres.
  ```
-->

제네릭 타입의 확장은
아래 [제네릭 Where 절이 있는 확장 (Extensions with a Generic Where Clause)](#제네릭-where-절이-있는-확장-extensions-with-a-generic-where-clause)에서 설명했듯이
확장된 타입의 인스턴스는 새로운 기능을 얻기 위해
충족해야 할 요구사항도 포함할 수 있습니다.

## 타입 제약 (Type Constraints)

`swapTwoValues(_:_:)` 함수와 `Stack` 타입은 모든 타입에서 동작 가능합니다.
그러나 가끔 제네릭 함수와 제네릭 타입으로
사용할 수 있는 타입에
*타입 제약(type constraints)*을 강제로 포함해야 유용할 수 있습니다.
타입 제약은 타입 매개변수가
특정 클래스를 상속받거나
특정 프로토콜 또는 프로토콜 합성을 준수해야 함을 명시합니다.

예를 들어
Swift의 `Dictionary` 타입은
딕셔너리에 대해 키로 사용할 수 있는 타입에 제한을 둡니다.
[딕셔너리 (Dictionaries)](./collection-types.md#딕셔너리-dictionaries)에서 설명했듯이,
딕셔너리 키의 타입은 *hashable*이어야 합니다.
즉, 고유하게 표현할 수 있는 방법을 제공해야 합니다.
`Dictionary`는 키가 이미 포함되어 있는지 확인할 수 있도록
키가 hashable이어야 합니다.
이 요구사항이 없으면 `Dictionary`는
특정 키에 대해 값을 삽입이나 대체해야 하는지 알 수 없으며
이미 딕셔너리에 있는 주어진 키에 대한 값을 찾을 수 없습니다.

이 요구사항은 `Dictionary`의 키 타입에서 타입 제약으로 강제되고,
키 타입이 Swift 표준 라이브러리에 정의된 특별한 프로토콜인
`Hashable` 프로토콜을 준수해야 함을 명시합니다.
Swift의 기본 타입(`String`, `Int`, `Double`, `Bool` 등)은
기본적으로 hashable입니다.
`Hashable` 프로토콜을 준수하는 자체 커스텀 타입을 만드는 것에 대한
자세한 내용은
[Hashable 프로토콜 준수 (Conforming to the Hashable Protocol)](https://developer.apple.com/documentation/swift/hashable#2849490)를 참고바랍니다.

커스텀 제네릭 타입을 생성할 때 고유 타입 제약을 정의할 수 있고,
이러한 제약은 제네릭 프로그래밍에 많은 기능을 제공합니다.
`Hashable`같은 추상 개념은
구체적인 타입이 아닌
개념적 특성 측면에서 타입을 특성화 합니다.

### 타입 제약 문법 (Type Constraint Syntax)

타입 제약은 타입 매개변수 이름 뒤에 콜론과 함께
클래스나 프로토콜 제약을 작성하여
타입 매개변수 목록에 포함합니다.
제네릭 함수에서 타입 제약에 대한 기본 문법은 아래와 같습니다
(제네릭 타입에도 동일하게 적용됩니다):

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // function body goes here
}
```

<!--
  - test: `typeConstraints`

  ```swifttest
  >> class SomeClass {}
  >> protocol SomeProtocol {}
  -> func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
        // function body goes here
     }
  ```
-->

위의 가상 함수는 두 개의 타입 매개변수를 가집니다.
첫 번째 타입 매개변수인 `T`는
`T`가 `SomeClass`의 하위 클래스여야 함을 나타냅니다.
두 번째 타입 매개변수 `U`는
`U`가 `SomeProtocol` 프로토콜을 준수해야하는 타입 제약이 있습니다.

### 타입 제약 동작 (Type Constraints in Action)

다음은 `findIndex(ofString:in:)`이라는 제네릭이 아닌 함수이며,
이것은 찾을 `String` 값과
찾을 `String` 값의 배열을 인자로 받습니다.
`findIndex(ofString:in:)` 함수는
배열에서 일치하는 첫 번째 문자열의 인덱스를 옵셔널 `Int`로 반환하거나,
찾지 못하면 `nil`을 반환합니다:

```swift
func findIndex(ofString valueToFind: String, in array: [String]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

<!--
  - test: `typeConstraints`

  ```swifttest
  -> func findIndex(ofString valueToFind: String, in array: [String]) -> Int? {
        for (index, value) in array.enumerated() {
           if value == valueToFind {
              return index
           }
        }
        return nil
     }
  ```
-->

`findIndex(ofString:in:)` 함수는 문자열 배열에서 문자열 값을 찾기 위해 사용할 수 있습니다:

```swift
let strings = ["cat", "dog", "llama", "parakeet", "terrapin"]
if let foundIndex = findIndex(ofString: "llama", in: strings) {
    print("The index of llama is \(foundIndex)")
}
// Prints "The index of llama is 2"
```

<!--
  - test: `typeConstraints`

  ```swifttest
  -> let strings = ["cat", "dog", "llama", "parakeet", "terrapin"]
  -> if let foundIndex = findIndex(ofString: "llama", in: strings) {
        print("The index of llama is \(foundIndex)")
     }
  <- The index of llama is 2
  ```
-->

그러나 배열에서 값의 인덱스를 찾는 원리는 문자열뿐 아니라 어떤 타입에도 적용할 수 있습니다.
문자열 대신 `T` 타입을 사용하여
동일한 기능을 제네릭 함수로 작성할 수 있습니다.

다음은 `findIndex(of:in:)`이라는
`findIndex(ofString:in:)`의 제네릭 버전을 나타냅니다.
이 함수는 배열의 옵셔널 값이 아닌
옵셔널 인덱스를 반환하므로
반환 타입이 여전히 `Int?`입니다.
하지만 이 함수는 아래 설명된 이유로
컴파일되지 않습니다:

```swift
func findIndex<T>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

<!--
  - test: `typeConstraints-err`

  ```swifttest
  -> func findIndex<T>(of valueToFind: T, in array:[T]) -> Int? {
        for (index, value) in array.enumerated() {
           if value == valueToFind {
              return index
           }
        }
        return nil
     }
  !$ error: binary operator '==' cannot be applied to two 'T' operands
  !!       if value == valueToFind {
  !!          ~~~~~ ^  ~~~~~~~~~~~
  ```
-->

이 함수는 위와 같이 작성하면 컴파일되지 않습니다.
이 문제는 "`if value == valueToFind`"에서 발생합니다.
Swift의 모든 타입이 동등 연산자(`==`)로 비교할 수 있는 것은 아닙니다.
예를 들어 복잡한 데이터 모델을 표현하기 위해 클래스나 구조체를 생성한다면
해당 클래스나 구조체에 대해 "같음"의 의미는
Swift가 자동으로 추론할 수 없습니다.
이로 인해 이 코드가 모든 타입의 `T`에 대해
작동한다고 보장할 수 없으므로
컴파일 오류가 발생합니다.

그러나
Swift 표준 라이브러리는 `Equatable`이라는 프로토콜을 정의하며,
이 프로토콜을 준수하는 타입은
두 개의 값을 비교하기 위한
동등 연산자(`==`)와 비동등 연산자(`!=`)를 구현해야 합니다.
Swift의 표준 타입은 `Equatable` 프로토콜을 자동으로 지원합니다.

<!--
  TODO: write about how to make your own types conform to Equatable
  once we have some documentation that actually describes it.
  The text to use is something like:
  and you can make your own types conform to ``Equatable`` too,
  as described in <link>.
-->

`Equatable`인 모든 타입은 동등 연산자를 지원하기 때문에
`findIndex(of:in:)` 함수에서 안전하게 사용할 수 있습니다.
이 사실을 표현하기 위해 함수를 정의할 때
타입 매개변수의 정의에 `Equatable` 타입 제약을 작성합니다:

```swift
func findIndex<T: Equatable>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

<!--
  - test: `typeConstraintsEquatable`

  ```swifttest
  -> func findIndex<T: Equatable>(of valueToFind: T, in array:[T]) -> Int? {
        for (index, value) in array.enumerated() {
           if value == valueToFind {
              return index
           }
        }
        return nil
     }
  ```
-->

`findIndex(of:in:)`의 단일 타입 매개변수는 "`T` 타입은 `Equatable` 프로토콜을 준수합니다"라는 의미로
`T: Equatable`로 작성됩니다.

이제 `findIndex(of:in:)` 함수는 정상적으로 컴파일되고
`Double`이나 `String`과 같이 `Equatable`을 준수하는 타입에 사용할 수 있습니다:

```swift
let doubleIndex = findIndex(of: 9.3, in: [3.14159, 0.1, 0.25])
// doubleIndex is an optional Int with no value, because 9.3 isn't in the array
let stringIndex = findIndex(of: "Andrea", in: ["Mike", "Malcolm", "Andrea"])
// stringIndex is an optional Int containing a value of 2
```

<!--
  - test: `typeConstraintsEquatable`

  ```swifttest
  -> let doubleIndex = findIndex(of: 9.3, in: [3.14159, 0.1, 0.25])
  /> doubleIndex is an optional Int with no value, because 9.3 isn't in the array
  </ doubleIndex is an optional Int with no value, because 9.3 isn't in the array
  -> let stringIndex = findIndex(of: "Andrea", in: ["Mike", "Malcolm", "Andrea"])
  /> stringIndex is an optional Int containing a value of \(stringIndex!)
  </ stringIndex is an optional Int containing a value of 2
  ```
-->

<!--
  TODO: providing different type parameters on individual methods within a generic type
-->

<!--
  TODO: likewise providing type parameters for initializers
-->

## 연관 타입 (Associated Types)

프로토콜을 정의할 때,
프로토콜의 정의의 부분으로
하나 이상의 연관 타입(associated types)을 선언하는게 더 유용한 경우가 있습니다.
*연관 타입(associated type)*은 프로토콜의 부분으로 사용되는
타입의 임의의 이름을 제공합니다.
연관 타입에 사용할 실제 타입은
프로토콜을 채택할 때까지 정해지지 않습니다.
연관 타입은 `associatedtype` 키워드로 지정합니다.

### 연관 타입의 동작 (Associated Types in Action)

다음은 `Item`이라는 연관 타입을 선언하는
`Container`라는 프로토콜의 예입니다:

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

<!--
  - test: `associatedTypes, associatedTypes-err`

  ```swifttest
  -> protocol Container {
        associatedtype Item
        mutating func append(_ item: Item)
        var count: Int { get }
        subscript(i: Int) -> Item { get }
     }
  ```
-->

`Container` 프로토콜은 모든 컨테이너가 제공해야 하는
세 가지 필수 요건을 정의합니다:

- `append(_:)` 메서드를 사용하여 컨테이너에 새로운 항목을 추가할 수 있어야 합니다.
- `Int` 값을 반환하는 `count` 프로퍼티를 통해
  컨테이너 항목의 카운트에 접근할 수 있어야 합니다.
- `Int` 인덱스 값을 사용하는
  서브스크립트로 컨테이너의 각 항목을 조회할 수 있어야 합니다.

이 프로토콜은 컨테이너의 항목이
어떻게 저장되는지, 어떤 타입이어야 하는지 지정하지 않습니다.
이 프로토콜은 이 세 가지 기능만 제공하면
`Container`로 간주할 수 있습니다.
준수하는 타입은 이 세 가지 요구사항을 만족하는 한,
추가적인 기능을 제공할 수 있습니다.

`Container` 프로토콜을 준수하는 모든 타입은
저장하는 값의 타입을 지정할 수 있어야 합니다.
특히 올바른 타입의 항목만 컨테이너에
추가되야 하며,
서브스크립트에 의해 반환된 항목의 타입도 명확해야 합니다.

이러한 요구사항을 정의하기 위해
`Container` 프로토콜은
컨테이너에 대한 타입이 무엇인지 알지 못해도
컨테이너가 보유할 항목의 타입을 참조할 방법이 필요합니다.
`Container` 프로토콜은
`append(_:)` 메서드에 전달된 모든 값이
컨테이너의 요소 타입과 같은 타입을 가져야 하며,
컨테이너의 서브스크립트에 의해 반환된 값은
컨테이너의 요소 타입과 같은 타입으로 지정해야 합니다.

이를 달성하기 위해
`Container` 프로토콜은 `associatedtype Item`으로 작성한
`Item`이라는 연관 타입을 선언합니다.
이 프로토콜은 `Item`이 무엇인지 정의하지 않고 ---
해당 정보는 모든 준수하는 타입이 제공할 수 있도록 남겨둡니다.
그럼에도 불구하고 `Item` 별칭은
`Container`의 항목 타입을 참조하고
`append(_:)` 메서드와 서브스크립트에서 사용하는 타입을 정의하고
모든 `Container`의 동작이 예상되도록 제공합니다.

다음은 위의 [제네릭 타입 (Generic Types)](#제네릭-타입-generic-types)에서
제네릭이 아닌 `IntStack` 타입을
`Container` 프로토콜에 맞게 수정한 버전입니다:

```swift
struct IntStack: Container {
    // original IntStack implementation
    var items: [Int] = []
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
    // conformance to the Container protocol
    typealias Item = Int
    mutating func append(_ item: Int) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Int {
        return items[i]
    }
}
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> struct IntStack: Container {
        // original IntStack implementation
        var items: [Int] = []
        mutating func push(_ item: Int) {
           items.append(item)
        }
        mutating func pop() -> Int {
           return items.removeLast()
        }
        // conformance to the Container protocol
        typealias Item = Int
        mutating func append(_ item: Int) {
           self.push(item)
        }
        var count: Int {
           return items.count
        }
        subscript(i: Int) -> Int {
           return items[i]
        }
     }
  ```
-->

`IntStack` 타입은 `Container` 프로토콜의 세 가지 요구사항을 모두 구현하고,
이 요구사항을 만족하기 위해
`IntStack` 타입의 기존 기능을 래핑합니다.

또한 `IntStack`은 `Container`의 구현에 대해
`Item`이 `Int` 타입임을 지정합니다.
`typealias Item = Int`의 정의는 `Container` 프로토콜의 구현에 대해
`Item`의 추상적 타입에서 `Int`의 구체적인 타입으로 변환합니다.

Swift의 타입 추론 덕분에
실제로 `IntStack`의 정의에
`Item`의 구체적인 타입인 `Int`를 선언할 필요가 없습니다.
`IntStack`은 `Container` 프로토콜의 모든 요구사항을 준수하기 때문에,
Swift는 `append(_:)` 메서드의 `item` 매개변수의 타입과
서브스크립트의 반환 타입에서 사용하는 `Item`을
추론할 수 있습니다.
실제로 위의 코드에서 `typealias Item = Int` 줄을 지워도
`Item`에 대해 무슨 타입을 사용하는지 명확하기 때문에 동작에는 이상이 없습니다.

`Container` 프로토콜을 준수하는 제네릭 `Stack` 타입도 만들 수도 있습니다:

```swift
struct Stack<Element>: Container {
    // original Stack<Element> implementation
    var items: [Element] = []
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
    // conformance to the Container protocol
    mutating func append(_ item: Element) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Element {
        return items[i]
    }
}
```

<!--
  - test: `associatedTypes, associatedTypes-err`

  ```swifttest
  -> struct Stack<Element>: Container {
        // original Stack<Element> implementation
        var items: [Element] = []
        mutating func push(_ item: Element) {
           items.append(item)
        }
        mutating func pop() -> Element {
           return items.removeLast()
        }
        // conformance to the Container protocol
        mutating func append(_ item: Element) {
           self.push(item)
        }
        var count: Int {
           return items.count
        }
        subscript(i: Int) -> Element {
           return items[i]
        }
     }
  ```
-->

이번에는 타입 매개변수 `Element`는
`append(_:)` 메서드의 `item` 매개변수와
서브스크립트의 반환 타입으로 사용합니다.
따라서 Swift는 `Element`가
특정 컨테이너에 대해 `Item`으로 사용하는 적절한 타입이라고 추론할 수 있습니다.

### 기존 타입을 확장하여 연관 타입 지정 (Extending an Existing Type to Specify an Associated Type)

[확장으로 프로토콜 준수 추가 (Adding Protocol Conformance with an Extension)](./protocols.md#확장으로-프로토콜-준수-추가-adding-protocol-conformance-with-an-extension)에서 설명한대로
프로토콜의 준수성을 추가하기 위해 기존 타입을 확장할 수 있습니다.
여기에는 연관 타입이 있는 프로토콜을 포함합니다.

Swift의 `Array` 타입은 이미 `append(_:)` 메서드,
`count` 프로퍼티, `Int` 인덱스로 요소를 조회하는 서브스크립트를 제공합니다.
이 세 가지 기능은 `Container` 프로토콜의 요구사항과 일치합니다.
이것은 `Array`가 `Container` 프로토콜을 채택하도록 선언하는 것으로
간단하게 `Container` 프로토콜을 준수하도록 `Array`를 확장할 수 있다는 의미입니다.
[확장으로 프로토콜 채택 선언 (Declaring Protocol Adoption with an Extension)](./protocols.md#확장으로-프로토콜-채택-선언-declaring-protocol-adoption-with-an-extension)에서 설명했듯이
빈 확장을 사용하여 이 작업을 수행할 수 있습니다:

```swift
extension Array: Container {}
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> extension Array: Container {}
  ```
-->

배열의 기존 `append(_:)` 메서드와 서브스크립트는
위의 제네릭 `Stack` 타입과 같이
`Item`에 대해 사용할 적절한 타입을 추론할 수 있습니다.
확장 정의 후에 모든 `Array`는 `Container`로 사용할 수 있습니다.

### 연관 타입에 제약 추가 (Adding Constraints to an Associated Type)

타입 제약으로 연관 타입을 추가하여
프로토콜을 준수하는 타입이 해당 제약을 만족하도록 할 수 있습니다.
예를 들어
다음의 코드는 컨테이너의 항목이
`Equatable`을 준수해야 하는 것을 요구하는 `Container`의 버전을 정의합니다.

```swift
protocol Container {
    associatedtype Item: Equatable
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

<!--
  - test: `associatedTypes-equatable`

  ```swifttest
  -> protocol Container {
        associatedtype Item: Equatable
        mutating func append(_ item: Item)
        var count: Int { get }
        subscript(i: Int) -> Item { get }
     }
  ```
-->

이 `Container` 버전을 준수하려면
컨테이너의 `Item` 타입은 `Equatable` 프로토콜을 준수해야 합니다.

### 연관 타입의 제약조건에서 프로토콜 사용 (Using a Protocol in Its Associated Type’s Constraints)

프로토콜은 자신의 요구사항으로 자신을 사용할 수도 있습니다.
예를 들어
다음은 `suffix(_:)` 메서드의 요구사항을 추가한
`Container` 프로토콜입니다.
`suffix(_:)` 메서드는
컨테이너 긑에서 지정된 숫자 만큼의
`Suffix` 타입의 인스턴스에 저장한 요소를 반환합니다.

```swift
protocol SuffixableContainer: Container {
    associatedtype Suffix: SuffixableContainer where Suffix.Item == Item
    func suffix(_ size: Int) -> Suffix
}
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> protocol SuffixableContainer: Container {
         associatedtype Suffix: SuffixableContainer where Suffix.Item == Item
         func suffix(_ size: Int) -> Suffix
     }
  ```
-->

이 프로토콜에서
`Suffix`는 위의 `Container` 예시에서
`Item` 타입과 같은 연관 타입입니다.
`Suffix`는 두 개의 제약을 가지고 있습니다:
`SuffixableContainer` 프로토콜을 준수해야 하며
(현재 정의되어 있는 프로토콜입니다),
`Item` 타입은
컨테이너의 `Item` 타입과 동일해야 합니다.
`Item`에 대한 제약은
아래의 [제네릭 Where 절이 있는 연관 타입 (Associated Types with a Generic Where Clause)](#제네릭-where-절이-있는-연관-타입-associated-types-with-a-generic-where-clause)에서 설명할 제네릭 `where` 절입니다.

다음은 `SuffixableContainer` 프로토콜의 준수성을 추가하는
위 [제네릭 타입 (Generic Types)](#제네릭-타입-generic-types)의
`Stack` 타입의 확장입니다:

```swift
extension Stack: SuffixableContainer {
    func suffix(_ size: Int) -> Stack {
        var result = Stack()
        for index in (count-size)..<count {
            result.append(self[index])
        }
        return result
    }
    // Inferred that Suffix is Stack.
}
var stackOfInts = Stack<Int>()
stackOfInts.append(10)
stackOfInts.append(20)
stackOfInts.append(30)
let suffix = stackOfInts.suffix(2)
// suffix contains 20 and 30
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> extension Stack: SuffixableContainer {
         func suffix(_ size: Int) -> Stack {
             var result = Stack()
             for index in (count-size)..<count {
                 result.append(self[index])
             }
             return result
         }
         // Inferred that Suffix is Stack.
     }
  -> var stackOfInts = Stack<Int>()
  -> stackOfInts.append(10)
  -> stackOfInts.append(20)
  -> stackOfInts.append(30)
  >> assert(stackOfInts.suffix(0).items == [])
  -> let suffix = stackOfInts.suffix(2)
  // suffix contains 20 and 30
  >> assert(suffix.items == [20, 30])
  ```
-->

위의 예시에서
`Stack`에 대한 `Suffix` 연관 타입도 `Stack`이므로
`Stack`에서 접미사 연산자는 다른 `Stack`을 반환합니다.
또한
`SuffixableContainer`를 준수하는 타입은
자체와 다른 `Suffix` 타입을 가질 수 있습니다 ---
접미사 연산자는 다른 타입을 반환할 수 있다는 의미입니다.
예를 들어
다음은 `IntStack` 대신 `Stack<Int>`를 접미사 타입으로 사용하여
`SuffixableContainer` 준수성을 추가하는
제네릭이 아닌 `IntStack` 타입에 대한 확장입니다:

```swift
extension IntStack: SuffixableContainer {
    func suffix(_ size: Int) -> Stack<Int> {
        var result = Stack<Int>()
        for index in (count-size)..<count {
            result.append(self[index])
        }
        return result
    }
    // Inferred that Suffix is Stack<Int>.
}
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> extension IntStack: SuffixableContainer {
         func suffix(_ size: Int) -> Stack<Int> {
             var result = Stack<Int>()
             for index in (count-size)..<count {
                 result.append(self[index])
             }
             return result
         }
         // Inferred that Suffix is Stack<Int>.
     }
  >> var intStack = IntStack()
  >> intStack.append(10)
  >> intStack.append(20)
  >> intStack.append(30)
  >> assert(intStack.suffix(0).items == [])
  >> assert(intStack.suffix(2).items == [20, 30])
  ```
-->

## 제네릭 Where 절 (Generic Where Clauses)

[타입 제약 (Type Constraints)](#타입-제약-type-constraints)에서 설명한 타입 제약을 사용하면,
제네릭 함수, 서브스크립트, 타입에
연결된 타입 매개변수에 대한 요구사항을 정의할 수 있습니다.

연관 타입에 대한 요구사항을 정의하는 것도 유용할 수 있습니다.
*제네릭 where 절(generic where clause)*을 정의하여 이것을 수행할 수 있습니다.
제네릭 `where` 절을 사용하면
연관 타입이 특정 프로토콜을 준수해야 하거나
특정 타입 매개변수와 연관 타입이 동일해야 한다고 요구할 수 있습니다.
제네릭 `where` 절은 `where` 키워드로 시작하고,
이어서 연관 타입이나
타입과 연관 타입 사이의 동등 관계에 대한 제약이 따라옵니다.
타입이나 함수의 본문의
여는 중괄호 바로 전에 제네릭 `where` 절을 작성합니다.

아래의 예시는 두 `Container` 인스턴스가
같은 순서로 같은 항목을 가지고 있는지 확인하는
`allItemsMatch`라는 제네릭 함수를 정의합니다.
이 함수는 모든 항목이 일치하면 `true`의 Boolean 값을 반환하고
그렇지 않으면 `false`의 값을 반환합니다.

확인할 두 컨테이너는
같은 타입의 컨테이너 일 필요는 없지만(같을 수도 있음),
같은 타입의 항목을 가지고 있어야 합니다.
이 요구사항은 타입 제약과
제네릭 `where` 절의 조합으로 표현됩니다:

```swift
func allItemsMatch<C1: Container, C2: Container>
    (_ someContainer: C1, _ anotherContainer: C2) -> Bool
    where C1.Item == C2.Item, C1.Item: Equatable {

        // Check that both containers contain the same number of items.
        if someContainer.count != anotherContainer.count {
            return false
        }

        // Check each pair of items to see if they're equivalent.
        for i in 0..<someContainer.count {
            if someContainer[i] != anotherContainer[i] {
                return false
            }
        }

        // All items match, so return true.
        return true
}
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> func allItemsMatch<C1: Container, C2: Container>
           (_ someContainer: C1, _ anotherContainer: C2) -> Bool
           where C1.Item == C2.Item, C1.Item: Equatable {

        // Check that both containers contain the same number of items.
        if someContainer.count != anotherContainer.count {
           return false
        }

        // Check each pair of items to see if they're equivalent.
        for i in 0..<someContainer.count {
           if someContainer[i] != anotherContainer[i] {
              return false
           }
        }

        // All items match, so return true.
        return true
     }
  ```
-->

이 함수는
`someContainer`와 `anotherContainer`라는 두 개의 인자를 가집니다.
`someContainer` 인자는 `C1` 타입이고,
`anotherContainer` 인자는 `C2` 타입입니다.
`C1`과 `C2` 모두 함수가 호출될 때
결정되는 두 가지 컨테이너 타입에 대한 타입 매개변수입니다.

함수의 두 타입 매개변수에 대한 요구사항은 다음과 같습니다:

- `C1`은 `Container` 프로토콜을 준수해야 합니다(`C1: Container`).
- `C2`는 `Container` 프로토콜을 준수해야 합니다(`C2: Container`).
- `C1`에 대한 `Item`은 `C2`에 대한 `Item`과 동일해야 합니다
  (`C1.Item == C2.Item`).
- `C1`에 대한 `Item`은 `Equatable` 프로토콜을 준수해야 합니다
  (`C1.Item: Equatable`).

첫 번째와 두 번째 요구사항은 함수의 타입 매개변수 목록에 정의되고,
세 번째와 네 번째 요구사항은 함수의 제네릭 `where` 절에 정의됩니다.

이 요구사항은 아래를 의미합니다:

- `someContainer`는 `C1` 타입의 컨테이너 입니다.
- `anotherContainer`는 `C2` 타입의 컨테이너 입니다.
- `someContainer`와 `anotherContainer`는 같은 타입의 항목을 포함합니다.
- `someContainer`의 항목은 서로 다름을 확인하기 위해
  비동등 연산자(`!=`)를 사용하여 체크할 수 있습니다.

세 번째와 네 번째 요구사항은
`anotherContainer`의 항목은 `someContainer`의 항목과 정확히 동일한 타입이므로
`!=` 연산자로 확인할 수 있음을 의미합니다.

이 요구사항은 서로 다른 컨테이너 타입이더라도
두 컨테이너를 비교하기 위해 `allItemsMatch(_:_:)` 함수를 사용할 수 있습니다.

`allItemsMatch(_:_:)` 함수는
두 컨테이너 모두 항목을 같은 수만큼 가지고 있는지 확인하는 것으로 시작합니다.
항목의 수가 다르다면 일치하지 않으므로
함수는 `false`를 반환합니다.

이 검사를 수행한 후에 함수는 `for`-`in` 루프와 반열림 범위 연산자(`..<`)로
`someContainer`의 모든 항목을 반복합니다.
각 항목에 대해 함수는 `someContainer`의 항목이
`anotherContainer`의 항목과 동일한지 확인합니다.
두 항목이 동일하지 않으면 두 개의 컨테이너는 일치하지 않고
함수는 `false`를 반환합니다.

불일치하는 항목을 찾지 못하고 루프가 끝나면
두 개의 컨테이너는 일치하고 함수는 `true`를 반환합니다.

다음은 `allItemsMatch(_:_:)` 함수가 어떻게 동작하는지 보여줍니다:

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")

var arrayOfStrings = ["uno", "dos", "tres"]

if allItemsMatch(stackOfStrings, arrayOfStrings) {
    print("All items match.")
} else {
    print("Not all items match.")
}
// Prints "All items match."
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> var stackOfStrings = Stack<String>()
  -> stackOfStrings.push("uno")
  -> stackOfStrings.push("dos")
  -> stackOfStrings.push("tres")

  -> var arrayOfStrings = ["uno", "dos", "tres"]

  -> if allItemsMatch(stackOfStrings, arrayOfStrings) {
        print("All items match.")
     } else {
        print("Not all items match.")
     }
  <- All items match.
  ```
-->

위의 예시는 `String` 값을 저장하는 `Stack` 인스턴스를 생성하고,
스택에 세 개의 문자열을 푸시합니다.
이 예시는 스택과 동일한 세 개의 문자열을 포함하는
`Array` 인스턴스도 생성합니다.
스택과 배열은 다른 타입이지만
둘 다 `Container` 프로토콜을 준수하고,
둘 다 같은 타입의 값을 포함합니다.
따라서 이 둘을 컨테이너 인자로
`allItemsMatch(_:_:)` 함수를 호출합니다.
위의 예시에서 `allItemsMatch(_:_:)` 함수는
두 컨테이너의 모든 항목이 정확히 일치한다고 알려줍니다.

## 제네릭 Where 절이 있는 확장 (Extensions with a Generic Where Clause)

확장에 제네릭 `where` 절을 사용할 수도 있습니다.
아래의 예시는
이전 예시의 제네릭 `Stack` 구조체에
`isTop(_:)` 메서드를 추가하기 위해 확장합니다.

```swift
extension Stack where Element: Equatable {
    func isTop(_ item: Element) -> Bool {
        guard let topItem = items.last else {
            return false
        }
        return topItem == item
    }
}
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> extension Stack where Element: Equatable {
         func isTop(_ item: Element) -> Bool {
             guard let topItem = items.last else {
                 return false
             }
             return topItem == item
         }
     }
  ```
-->

새로운 `isTop(_:)` 메서드는
먼저 스택이 비어있는지 확인하고
그 다음에 주어진 항목과
스택의 가장 상단의 항목을 비교합니다.
제네릭 `where` 절이 없이 시도한다면
문제가 발생합니다:
`isTop(_:)`의 구현은 `==` 연산자를 사용하지만
`Stack`의 정의에서
항목은 `Equatable`을 준수하지 않으므로
`==` 연산자를 사용한 결과는 컴파일 오류가 발생합니다.
제네릭 `where` 절을 사용하여
확장에 새로운 요구사항을 추가할 수 있으므로,
확장은 스택의 항목이 `Equatable`을 준수할 때만
`isTop(_:)` 메서드를 추가합니다.

다음은 `isTop(_:)` 메서드가 어떻게 동작하는지 보여줍니다:

```swift
if stackOfStrings.isTop("tres") {
    print("Top element is tres.")
} else {
    print("Top element is something else.")
}
// Prints "Top element is tres."
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> if stackOfStrings.isTop("tres") {
        print("Top element is tres.")
     } else {
        print("Top element is something else.")
     }
  <- Top element is tres.
  ```
-->

`Equatable`을 준수하지 않은 항목을 가진 스택에서
`isTop(_:)` 메서드를 호출하면
컴파일 오류가 발생합니다.

```swift
struct NotEquatable { }
var notEquatableStack = Stack<NotEquatable>()
let notEquatableValue = NotEquatable()
notEquatableStack.push(notEquatableValue)
notEquatableStack.isTop(notEquatableValue)  // Error
```

<!--
  - test: `associatedTypes-err`

  ```swifttest
  -> struct NotEquatable { }
  -> var notEquatableStack = Stack<NotEquatable>()
  -> let notEquatableValue = NotEquatable()
  -> notEquatableStack.push(notEquatableValue)
  -> notEquatableStack.isTop(notEquatableValue)  // Error
  !$ error: value of type 'Stack<NotEquatable>' has no member 'isTop'
  !! notEquatableStack.isTop(notEquatableValue)  // Error
  !! ~~~~~~~~~~~~~~~~~ ^~~~~
  ```
-->

프로토콜 확장에도 제네릭 `where` 절을 사용할 수 있습니다.
아래의 예시는 이전 예시의 `Container` 프로토콜에
`startsWith(_:)` 메서드를 추가하기 위해 확장합니다.

```swift
extension Container where Item: Equatable {
    func startsWith(_ item: Item) -> Bool {
        return count >= 1 && self[0] == item
    }
}
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> extension Container where Item: Equatable {
        func startsWith(_ item: Item) -> Bool {
           return count >= 1 && self[0] == item
        }
     }
  ```
-->

<!--
  Using Container rather than Sequence/Collection
  to continue running with the same example through the chapter.
  This does, however, mean I can't use a for-in loop.
-->

`startsWith(_:)` 메서드는
먼저 컨테이너가 하나 이상의 항목을 가지고 있는지 확인하고
그런 다음
컨테이너의 첫 번째 항목이 주어진 항목과 일치하는지 확인합니다.
새로운 `startsWith(_:)` 메서드는
컨테이너의 항목이 `Equatable`을 준수하면
위에서 사용된 스택과 배열을 포함하여
`Container` 프로토콜을 준수하는 모든 타입에서 사용할 수 있습니다.

```swift
if [9, 9, 9].startsWith(42) {
    print("Starts with 42.")
} else {
    print("Starts with something else.")
}
// Prints "Starts with something else."
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> if [9, 9, 9].startsWith(42) {
        print("Starts with 42.")
     } else {
        print("Starts with something else.")
     }
  <- Starts with something else.
  ```
-->

위의 예시에서 제네릭 `where` 절은
`Item`이 프로토콜을 준수해야 한다고 요구하지만,
`Item`이 특정 타입을 요구하도록
제네릭 `where` 절을 작성할 수도 있습니다.
예를 들어:

```swift
extension Container where Item == Double {
    func average() -> Double {
        var sum = 0.0
        for index in 0..<count {
            sum += self[index]
        }
        return sum / Double(count)
    }
}
print([1260.0, 1200.0, 98.6, 37.0].average())
// Prints "648.9"
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> extension Container where Item == Double {
         func average() -> Double {
             var sum = 0.0
             for index in 0..<count {
                 sum += self[index]
             }
             return sum / Double(count)
         }
     }
  -> print([1260.0, 1200.0, 98.6, 37.0].average())
  <- 648.9
  ```
-->

이 예시는 `Item` 타입이 `Double`인 컨테이너에
`average()` 메서드를 추가합니다.
컨테이너의 항목을 더하고
컨테이너의 수로 나누어 평균을 계산합니다.
부동소수점 나누기가 가능하기 위해
카운트를 `Int`에서 `Double`로 명시적으로 변환합니다.

다른 곳에서 작성하는 제네릭 `where` 절과 마찬가지로
확장의 제네릭 `where` 절에
여러 요구사항을 포함할 수 있습니다.
콤마로 각 요구사항을 구분합니다.

<!--
  No example of a compound where clause
  because Container only has one generic part ---
  there isn't anything to write a second constraint for.
-->

## 컨텍스트 Where 절 (Contextual Where Clauses)

제네릭 타입의 컨텍스트에서 이미 작업중인 경우
별도의 제네릭 타입 제약조건이 없는 선언에
제네릭 `where` 절을 작성할 수 있습니다.
예를 들어
제네릭 타입의 서브스크립트나
제네릭 타입 확장의 메서드에
제네릭 `where` 절을 작성할 수 있습니다.
`Container` 구조체는 제네릭이고,
아래의 예시에서 `where` 절은
컨테이너에서 이러한 새로운 메서드를 사용할 수 있도록
충족해야 하는 타입 제약조건을 지정합니다.

```swift
extension Container {
    func average() -> Double where Item == Int {
        var sum = 0.0
        for index in 0..<count {
            sum += Double(self[index])
        }
        return sum / Double(count)
    }
    func endsWith(_ item: Item) -> Bool where Item: Equatable {
        return count >= 1 && self[count-1] == item
    }
}
let numbers = [1260, 1200, 98, 37]
print(numbers.average())
// Prints "648.75"
print(numbers.endsWith(37))
// Prints "true"
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> extension Container {
         func average() -> Double where Item == Int {
             var sum = 0.0
             for index in 0..<count {
                 sum += Double(self[index])
             }
             return sum / Double(count)
         }
         func endsWith(_ item: Item) -> Bool where Item: Equatable {
             return count >= 1 && self[count-1] == item
         }
     }
  -> let numbers = [1260, 1200, 98, 37]
  -> print(numbers.average())
  <- 648.75
  -> print(numbers.endsWith(37))
  <- true
  ```
-->

이 예시는
항목이 정수이면 `Container`에 `average()` 메서드를 추가하고,
항목이 `Equatable`을 준수하면 `endsWith(_:)` 메서드를 추가합니다.
두 함수 모두 `Container`의 기존 선언에
제네릭 `Item` 타입 매개변수에 대해 타입 제약조건을 추가한
제네릭 `where` 절을 포함합니다.

컨텍스트 `where` 절 사용없이 코드를 작성하고 싶다면,
두 개의 확장을 작성하고
각 확장에 제네릭 `where` 절을 추가하면 됩니다.
위의 예시와 아래의 예시는 같은 동작을 가집니다.

```swift
extension Container where Item == Int {
    func average() -> Double {
        var sum = 0.0
        for index in 0..<count {
            sum += Double(self[index])
        }
        return sum / Double(count)
    }
}
extension Container where Item: Equatable {
    func endsWith(_ item: Item) -> Bool {
        return count >= 1 && self[count-1] == item
    }
}
```

<!--
  - test: `associatedTypes-err`

  ```swifttest
  -> extension Container where Item == Int {
         func average() -> Double {
             var sum = 0.0
             for index in 0..<count {
                 sum += Double(self[index])
             }
             return sum / Double(count)
         }
     }
     extension Container where Item: Equatable {
         func endsWith(_ item: Item) -> Bool {
             return count >= 1 && self[count-1] == item
         }
     }
  ```
-->

컨텍스트 `where` 절을 사용하는 이 예시의 버전에서
각 메서드의 제네릭 `where` 절은
해당 메서드를 사용할 수 있도록
충족해야 할 요구사항을 명시하기 때문에
`average()`와 `endsWith(_:)`의 구현은
모두 같은 확장에 있습니다.
이러한 요구사항을 확장의 제네릭 `where` 절로 이동하면
동일한 상황에서 메서드를 사용할 수 있지만
요구사항 당 하나의 확장이 필요합니다.

## 제네릭 Where 절이 있는 연관 타입 (Associated Types with a Generic Where Clause)

연관 타입에 제네릭 `where` 절을 포함할 수 있습니다.
예를 들어 Swift 표준 라이브러리에서 `Sequence` 프로토콜이 사용하는 것과 같이
이터레이터(iterator)를 포함하는
`Container`의 버전을 만들고 싶다고 가정해 봅시다.
작성 방법은 아래와 같습니다:

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }

    associatedtype Iterator: IteratorProtocol where Iterator.Element == Item
    func makeIterator() -> Iterator
}
```

<!--
  - test: `associatedTypes-iterator`

  ```swifttest
  -> protocol Container {
        associatedtype Item
        mutating func append(_ item: Item)
        var count: Int { get }
        subscript(i: Int) -> Item { get }

        associatedtype Iterator: IteratorProtocol where Iterator.Element == Item
        func makeIterator() -> Iterator
     }
  ```
-->

<!--
  Adding makeIterator() to Container lets it conform to Sequence,
  although we don't call that out here.
-->

`Iterator`의 제네릭 `where` 절은
이터레이터(iterator)의 타입과 상관없이
컨테이너의 항목과 동일한 항목 타입의
요소를 탐색해야 합니다.
`makeIterator()` 함수는 컨테이너의 이터레이터(iterator)에 접근을 제공합니다.

<!--
  This example requires SE-0157 Recursive protocol constraints
  which is tracked by rdar://20531108

   that accepts a ranged of indexes it its subscript
   and returns a subcontainer ---
   similar to how ``Collection`` works in the Swift standard library.

   .. testcode:: associatedTypes-subcontainer

      -> protocol Container {
            associatedtype Item
            associatedtype SubContainer: Container where SubContainer.Item == Item

            mutating func append(_ item: Item)
            var count: Int { get }
            subscript(i: Int) -> Item { get }
            subscript(range: Range<Int>) -> SubContainer { get }
         }

   The generic ``where`` clause on ``SubContainer`` requires that
   the subcontainer must have the same item type as the container has,
   regardless of what type the subcontainer is.
   The original container and the subcontainer
   could be represented by the same type
   or by different types.
   The new subscript that accepts a range
   uses this new associated type as its return value.
-->

프로토콜이 다른 프로토콜을 상속할 때
프로토콜 선언에 제네릭 `where` 절을 포함하여
상속받은 연관 타입에 제약조건을 추가합니다.
예를 들어 다음의 코드는
`Item`이 `Comparable`을 준수하도록 요구하는
`ComparableContainer` 프로토콜을 선언합니다:

```swift
protocol ComparableContainer: Container where Item: Comparable { }
```

<!--
  - test: `associatedTypes`

  ```swifttest
  -> protocol ComparableContainer: Container where Item: Comparable { }
  ```
-->

<!--
  This version throws a warning as of Swift commit de66b0c25c70:
  "redeclaration of associated type %0 from protocol %1 is better
  expressed as a 'where' clause on the protocol"

   -> protocol ComparableContainer: Container {
          associatedtype Item: Comparable
      }
-->

<!--
  Exercise the new container -- this might not actually be needed,
  and it adds a level of complexity.

  function < (lhs: ComparableContainer, rhs: ComparableContainer) -> Bool {
      // Sort empty containers before nonempty containers.
      if lhs.count == 0 {
          return true
      } else if rhs.count  == 0 {
          return false
      }

      // Sort nonempty containers by their first element.
      // (In real code, you would want to compare the second element
      // if the first elements are equal, and so on.)
      return lhs[0] < rhs[0]
  }
-->

## 제네릭 서브스크립트 (Generic Subscripts)

서브스크립트도 제네릭으로 만들 수 있고,
제네릭 `where` 절을 포함할 수 있습니다.
`subscript` 다음 꺾쇠 괄호 안에 임의의 타입 이름을 작성하고,
서브스크립트의 본문의
열린 중괄호 전에 제네릭 `where` 절을 작성합니다.
예를 들어:

<!--
  The paragraph above borrows the wording used to introduce
  generics and 'where' clauses earlier in this chapter.
-->

```swift
extension Container {
    subscript<Indices: Sequence>(indices: Indices) -> [Item]
        where Indices.Iterator.Element == Int {
            var result: [Item] = []
            for index in indices {
                result.append(self[index])
            }
            return result
    }
}
```

<!--
  - test: `genericSubscript`

  ```swifttest
  >> protocol Container {
  >>    associatedtype Item
  >>    mutating func append(_ item: Item)
  >>    var count: Int { get }
  >>    subscript(i: Int) -> Item { get }
  >> }
  -> extension Container {
         subscript<Indices: Sequence>(indices: Indices) -> [Item]
                 where Indices.Iterator.Element == Int {
             var result: [Item] = []
             for index in indices {
                 result.append(self[index])
             }
             return result
         }
     }
  ```
-->

<!--
  - test: `genericSubscript`

  ```swifttest
  >> struct IntStack: Container {
        // original IntStack implementation
        var items: [Int] = []
        mutating func push(_ item: Int) {
           items.append(item)
        }
        mutating func pop() -> Int {
           return items.removeLast()
        }
        // conformance to the Container protocol
        typealias Item = Int
        mutating func append(_ item: Int) {
           self.push(item)
        }
        var count: Int {
           return items.count
        }
        subscript(i: Int) -> Int {
           return items[i]
        }
     }
  >> var s = IntStack()
  >> s.push(10); s.push(20); s.push(30)
  >> let items = s[ [0, 2] ]
  >> assert(items == [10, 30])
  ```
-->

`Container` 프로토콜의 확장은
시퀀스의 인덱스를 가지고 각 주어진 인덱스의 항목을 포함한
배열을 반환하는 서브스크립트를 추가합니다.
이 제네릭 서브스크립트는 다음과 같은 제약이 있습니다:

- 꺾쇠 괄호에 제네릭 매개변수 `Indices`는
  표준 라이브러리의
  `Sequence` 프로토콜을 준수하는 타입이어야 합니다.
- 서브스크립트는 `Indices` 타입의 인스턴스 인
  `indices`라는 단일 매개변수를 가집니다.
- 제네릭 `where` 절은
  시퀀스에 대한 이터레이터(iterator)가
  `Int` 타입의 요소여야 합니다.
  이렇게 하면 시퀀스의 인덱스는
  컨테이너에 사용되는 인덱스와 동일한 타입입니다.

종합하면 이 제약조건은
`indices` 파리미터에 전달된 값은
정수 시퀀스입니다.

## 암시적 제약조건 (Implicit Constraints)

명시적으로 제약조건을 작성하는 것 외에도
제네릭 코드의 많은 곳에서
[`Copyable`][]와 같은 일반적인 프로토콜 준수를 암시적으로 요구합니다.
<!-- When SE-0446 is implemented, add Escapable above. -->
이렇게 작성할 필요없는 제네릭 제약조건은
*암시적 제약조건(implicit constraints)*라고 합니다.
예를 들어 다음 함수 선언 모두
`MyType`이 복사 가능해야 함을 요구합니다:

[`Copyable`]: https://developer.apple.com/documentation/swift/copyable

```swift
function someFunction<MyType> { ... }
function someFunction<MyType: Copyable> { ... }
```

위 코드에서
첫 번째 선언은 암시적 제약조건을 가지고 있고
두 번째 버전은 명시적으로 준수를 나열합니다.
대부분의 코드에서
타입은 이러한 일반적인 프로토콜을 암시적으로 준수합니다.
자세한 내용은
[프로토콜 암시적 준수 (Implicit Conformance to a Protocol)](./protocols.md#프로토콜-암시적-준수-implicit-conformance-to-a-protocol)을 참고바랍니다.

Swift의 대부분 타입은 이러한 프로토콜을 준수하므로,
거의 모든 곳에 이것을 작성하는 것은 반복적입니다.
대신 예외적인 경우에만 표시함으로써
일반적인 제약조건이 생략된 곳을 강조할 수 있습니다.
암시적 제약조건을 억제하려면
프로토콜 이름 앞에 물결표(`~`)를 작성합니다.
`~Copyable`은 "복사 가능할 수도 있음"으로 읽을 수 있습니다 ---
이 억제된 제약조건은
이 위치에서 복사 가능한 타입과 복사 불가능한 타입 모두를 허용합니다.
`~Copyable`은 타입이 복사 불가능할 것을 *요구*하는 것이 아닙니다.
예를 들어:

```swift
func f<MyType>(x: inout MyType) {
    let x1 = x  // The value of x1 is a copy of x's value.
    let x2 = x  // The value of x2 is a copy of x's value.
}

func g<AnotherType: ~Copyable>(y: inout AnotherType) {
    let y1 = y  // The assignment consumes y's value.
    let y2 = y  // Error: Value consumed more than once.
}
```

위 코드에서
함수 `f()`는 `MyType`이 복사 가능할 것을 암시적으로 요구합니다.
함수 본문 내에서
`x`의 값은 할당으로 `x1`과 `x2`에 복사됩니다.
반면에 `g()`는 `AnotherType`에 암시적 제약조건을 억제하여
복사 가능한 값과 복사 불가능한 값 모두 전달할 수 있게 합니다.
함수 본문 내에서
`y`의 값을 복사할 수 없으며
이것은 `AnotherType`이 복사 불가능할 수 있기 때문입니다.
할당은 `y`의 값을 소비하며
값을 두 번이상 소비하면 오류가 발생합니다.
`y`와 같이 복사 불가능한 값은
in-out, borrowing, consuming 매개변수로 전달되어야 합니다 ---
더 많은 정보는
[Borrowing 매개변수와 Consuming 매개변수 (Borrowing and Consuming Parameters)](../language-reference/declarations.md#borrowing-매개변수와-consuming-매개변수-borrowing-and-consuming-parameters)를 참고바랍니다.

주어진 프로토콜에 암시적 제약조건이
언제 제네릭 코드에 포함되는지에 대한 자세한 내용은
해당 프로토콜의 참조를 참고바랍니다.

<!--
  TODO: Generic Enumerations
  --------------------------
-->

<!--
  TODO: Describe how Optional<Wrapped> works
-->

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->