# 속성 (Attributes)

선언과 타입에 정보를 추가합니다.

Swift에는 두 종류의 속성(attributes)이 있습니다 —--
선언에 적용하는 속성과 타입에 적용하는 속성이 있습니다.
속성은 선언이나 타입에 추가적인 정보를 제공합니다.
예를 들어
함수 선언에 `discardableResult` 속성은
함수가 값을 반환하지만 반환값이 사용되지 않을 때
컴파일러는 경고를 생성하지 않는 것을 나타냅니다.

속성을 지정하려면 `@` 기호 다음에 속성의 이름과
속성이 허용하는 인자를 작성합니다:

```swift
@<#attribute name#>
@<#attribute name#>(<#attribute arguments#>)
```

어떤 선언 속성은 속성에 대한 추가 정보와
특정 선언에 어떻게 적용하는지 지정하는
인자를 허용합니다.
이러한 *속성 인자(attribute arguments)*는 소괄호로 감싸고
해당 형식이 속한 속성에 의해 정의됩니다.

첨부 매크로와 프로퍼티 래퍼도 속성 문법을 사용합니다.
매크로 확장에 대한 자세한 내용은
[매크로 확장 표현식 (Macro Expansion Expression)](./expressions.md#매크로-확장-표현식-macro-expansion-expression)을 참고바랍니다.
프로퍼티 래퍼에 대한 자세한 내용은
[propertyWrapper](#propertywrapper)를 참고바랍니다.

## 선언 속성 (Declaration Attributes)

선언 속성(declaration attribute)은 선언에만 적용할 수 있습니다.

### attached

매크로 선언에 `attached` 속성을 적용합니다.
이 속성의 인자는 매크로의 역할을 나타냅니다.
하나의 매크로가 여러 역할이 있는 경우
각 역할에 대해 `attached` 매크로를 여러 번 적용해야 합니다.

<!-- TODO:
If there's a stable URL we can use, make the macro protocols below links.
-->

이 속성에 첫 번째 인자는
매크로 역할을 나타냅니다:

- Peer 매크로:
  이 속성의 첫 번째 인자로 `peer`를 작성합니다.
  이 매크로 구현 타입은 `PeerMacro` 프로토콜을 준수합니다.
  이러한 매크로는 매크로가 적용된
  선언과 동일한 범위에
  새로운 선언을 생성합니다.
  예를 들어
  구조체의 메서드에 peer 매크로를 적용하면
  해당 구조체에 추가로 메서드와 프로퍼티를 정의할 수 있습니다.

- Member 매크로:
  이 속성의 첫 번째 인자로 `member`를 작성합니다.
  이 매크로 구현 타입은 `MemberMacro` 프로토콜을 준수합니다.
  이러한 매크로는 매크로가 적용된
  타입이나 확장의 멤버인
  새로운 선언을 생성합니다.
  예를 들어
  구조체 선언에 member 매크로를 적용하면
  해당 구조체에 추가로 메서드와 프로퍼티를 정의할 수 있습니다.

- Member 속성:
  이 속성의 첫 번째 인자로 `memberAttribute`를 작성합니다.
  이 매크로 구현 타입은 `MemberAttributeMacro` 프로토콜을 준수합니다.
  이러한 매크로는 매크로가 적용된
  타입이나 확장의 멤버에 속성을 추가합니다.

- Accessor 매크로:
  이 속성의 첫 번째 인자로 `accessor`를 작성합니다.
  이 매크로 구현 타입은 `AccessorMacro` 프로토콜을 준수합니다.
  이 매크로는 저장 프로퍼티에 접근자를 추가하여
  연산 프로퍼티로 변경합니다.

- Extension 매크로:
  이 속성의 첫 번째 인자로 `extension`을 작성합니다.
  이 매크로 구현 타입은 `ExtensionMacro` 프로토콜을 준수합니다.
  이러한 매크로는 `where` 절로
  프로토콜 준수를 추가할 수 있고,
  매크로가 적용된 타입의 새로운 멤버 선언을 추가할 수 있습니다.
  매크로가 프로토콜 준수를 추가하면,
  `conformances:` 인자를 포함하고 프로토콜을 지정합니다.
  준수 목록은 프로토콜 이름,
  준수하는 목록 아이템의 타입 별칭,
  복합 프로토콜 준수 목록 아이템을 포함합니다.
  중첩 타입에서 확장 매크로는
  해당 파일의 최상위 레벨에서 확장합니다.
  확장, 타입 별칭, 함수 내에 중첩된 타입에
  확장 매크로를 작성하거나
  확장 매크로를 사용하여 peer 매크로가 있는 확장을 추가할 수 없습니다.

peer와 member 매크로 역할은 매크로가 생성하는 기호의 이름을 나열하는
`names:` 인자를 요구합니다.
accessor 매크로 역할은 매크로가 `willSet`이나 `didSet` 프로퍼티 관찰자를 생성한다면
`names:` 인자를 요구합니다.
프로퍼티 관찰자를 생성하는 accessor 매크로는
프로퍼티 관찰자는 저장 프로퍼티에만 적용가능하므로 다른 accessor 매크로를 추가할 수 없습니다.
매크로가 확장 내에 선언을 추가한다면
확장 매크로 역할도 `names:` 인자를 요구합니다.
`names:` 인자가 매크로 선언에 포함되면,
매크로 구현은
해당 목록과 일치하는 이름의 기호로만 생성해야 합니다.
그렇다고 해서
매크로가 목록에 있는 모든 이름에 대한 기호를 생성할 필요는 없습니다.
해당 인자의 값은 다음의 목록 중 하나 이상을 나열할 수 있습니다:

- `named(<#name#>)`
  여기서 *이름(name)*은 미리 알려진
  고정된 기호 이름입니다.

- `overloaded`
  기존 기호와 동일한 이름인 경우입니다.

- `prefixed(<#prefix#>)`
  여기서 *접두사(prefix)*는 기호 이름 앞에 붙는
  고정된 문자열로 시작하는 이름입니다.

- `suffixed(<#suffix#>)`
  여기서 *접미사(suffix)*는 기호 이름 뒤에 붙는
  고정된 문자열로 끝나는 이름입니다.

- `arbitrary`
  매크로 확장 시점까지 결정할 수 없는 이름입니다.

특수한 경우로
프로퍼티 래퍼(property wrapper)와 유사하게 동작하는 매크로에 대해
`prefixed($)`를 작성할 수 있습니다.
<!--
TODO TR: Is there any more detail about this case?
-->

### available

이 속성을 적용하여 특정 Swift 언어 버전이나
특정 플랫폼과 운영체제 버전과 관련된
선언의 생명주기를 나타냅니다.

`available` 속성은
두 개 이상의 콤마로 구분된 속성 인자의 목록으로 나타냅니다.
이러한 인자는 다음의 플랫폼이나 언어 이름 중 하나로 시작합니다:

- `iOS`
- `iOSApplicationExtension`
- `macOS`
- `macOSApplicationExtension`
- `macCatalyst`
- `macCatalystApplicationExtension`
- `watchOS`
- `watchOSApplicationExtension`
- `tvOS`
- `tvOSApplicationExtension`
- `visionOS`
- `visionOSApplicationExtension`
- `swift`

<!--
  If you need to add a new platform to this list,
  you probably need to update platform-name in the grammar too.
-->

<!--
  For the list in source, see include/swift/AST/PlatformKinds.def
-->

또한 별표(`*`)를 사용하여
위에 나열된 모든 플랫폼 이름에 대한 선언의 가용성을 나타낼 수도 있습니다.
Swift 버전을 사용하여 지정한 가용
`available` 속성은
별표를 사용할 수 없습니다.

나머지 인자는 순서에 상관없이 나타날 수 있으며
중요한 마일스톤을 포함하여
선언의 생명주기에 대한 추가 정보를 지정할 수 있습니다.

- `unavailable` 인자는
  지정된 플랫폼에서 선언을 사용할 수 없음을 나타냅니다.
  이 인자는 Swift 버전 가용성을 지정할 때 사용될 수 없습니다.
- `introduced` 인자는
  선언이 도입된 플랫폼이나 언어의 첫 번째 버전을 나타냅니다.
  다음과 같은 형식을 가집니다:

  ```swift
  introduced: <#version number#>
  ```
  위 형식의 *버전 번호(version number)*는 마침표로 구분된
  하나에서 세 개의 양수로 구성됩니다.
- `deprecated` 인자는
  선언이 사용 중단된 플랫폼이나 언어의 첫 번째 버전을 나타냅니다.
  다음과 같은 형식을 가집니다:

  ```swift
  deprecated: <#version number#>
  ```
  옵셔널 *버전 번호(version number)*는 마침표로 구분된
  하나에서 세 개의 양수로 구성됩니다.
  버전 번호를 생략하면 사용 중단된 시기에 대한 정보를 제공하지 않고
  선언이 현재 사용 중단됨을 나타냅니다.
  버전 숫자를 생략하면 콜론(`:`)도 생략합니다.
- `obsoleted` 인자는
  선언이 폐기된 플랫폼이나 언어의 첫 번째 버전을 나타냅니다.
  선언이 폐기되면,
  지정된 플랫폼이나 언어에서 제거되고 더 이상 사용이 불가능합니다.
  다음과 같은 형식을 가집니다:

  ```swift
  obsoleted: <#version number#>
  ```
  *버전 번호(version number)*는 마침표로 구분된 하나에서 세 개의 양수로 구성됩니다.

- `noasync` 인자는
  선언된 기호가 비동기 컨텍스트에서
  직접적으로 사용될 수 없음을 나타냅니다.

  Swift 동시성은 잠재적 중단 지점 후에
  다른 스레드에서 재개될 수 있으므로,
  스레드 로컬 스토리지(thread-local storage), 락(locks), 뮤텍스(mutexes), 세마포어(semaphores)와 같은 요소를
  중단 지점을 넘어 사용하면 올바르지 않은 결과를 가져올 수 있습니다.

  이런 문제를 피하기 위해
  기호의 선언에 `@available(*, noasync)` 속성을 추가합니다:

  ```swift
  extension pthread_mutex_t {

    @available(*, noasync)
    mutating func lock() {
        pthread_mutex_lock(&self)
    }

    @available(*, noasync)
    mutating func unlock() {
        pthread_mutex_unlock(&self)
    }
  }
  ```

  이 속성은 누군가가 비동기 컨텍스트에서 이 기호를 사용하려고 하면
  컴파일 시 오류가 발생합니다.
  이 기호에 대해
  추가적인 정보를 제공하기 위해 `message` 인자를 사용할 수도 있습니다.
  
  ```swift
  @available(*, noasync, message: "Migrate locks to Swift concurrency.")
  mutating func lock() {
    pthread_mutex_lock(&self)
  }
  ```

  잠재적으로 안전하지 않은 기호를 안전한 방식으로 사용함을
  보장할 수 있다면,
  이것을 동기 함수로 감싸고
  비동기 컨텍스트에서 해당 함수를 호출할 수 있습니다.
  
  ```swift

  // Provide a synchronous wrapper around methods with a noasync declaration.
  extension pthread_mutex_t {
    mutating func withLock(_ operation: () -> ()) {
      self.lock()
      operation()
      self.unlock()
    }
  }

  func downloadAndStore(key: Int,
                      dataStore: MyKeyedStorage,
                      dataLock: inout pthread_mutex_t) async {
    // Safely call the wrapper in an asynchronous context.
    dataLock.withLock {
      dataStore[key] = downloadContent()
    }
  }
  ```

  `noasync` 인자는 대부분 선언에 사용할 수 있습니다;
  그러나 디이니셜라이저 선언에는 사용할 수 없습니다.
  Swift는 동기와 비동기 컨텍스트 모두에서
  클래스의 디이니셜라이저를 호출할 수 있어야 합니다.

- `message` 인자는 `deprecated`, `obsoleted`, `noasync`로 표시된 선언
  사용에 대한 경고나 오류를 생성할 때,
  컴파일러가 표시하는 텍스트 메세지를 제공합니다.
  다음과 같은 형식을 가집니다:

  ```swift
  message: <#message#>
  ```
  위 형식의 *메세지(message)*는 문자열 리터럴로 구성됩니다.
- `renamed` 인자는
  이름이 변경된 새로운 선언을 나타내는 텍스트 메세지를 제공합니다.
  컴파일러는 이름이 변경된 선언 사용에 대한 오류를 생성할 때,
  새로운 이름을 표시합니다.
  다음과 같은 형식을 가집니다:

  ```swift
  renamed: <#new name#>
  ```
  위 형식의 *새로운 이름(new name)*은 문자열 리터럴로 구성됩니다.

  프레임워크나 라이브러리의 릴리즈 간에
  변경된 선언의 이름을 나타내기 위해
  아래와 같이 타입 별칭 선언에
  `renamed`와 `unavailable` 인자로
  `available` 속성을 적용할 수 있습니다.
  이 조합으로 인해 컴파일 시
  선언이 변경되었다는 오류가 발생합니다.

  ```swift
  // First release
  protocol MyProtocol {
      // protocol definition
  }
  ```

<!--
  - test: `renamed1`

  ```swifttest
  -> // First release
  -> protocol MyProtocol {
           // protocol definition
     }
  ```
-->

  ```swift
  // Subsequent release renames MyProtocol
  protocol MyRenamedProtocol {
      // protocol definition
  }

  @available(*, unavailable, renamed: "MyRenamedProtocol")
  typealias MyProtocol = MyRenamedProtocol
  ```

  <!--
    - test: `renamed2`

    ```swifttest
    -> // Subsequent release renames MyProtocol
    -> protocol MyRenamedProtocol {
           // protocol definition
       }

    -> @available(*, unavailable, renamed: "MyRenamedProtocol")
       typealias MyProtocol = MyRenamedProtocol
    ```
  -->

단일 선언에 여러 개의 `available` 속성을 적용하여
다른 플랫폼과
다른 Swift 버전에서 선언의 가용성을 지정할 수 있습니다.
`available` 속성이 적용된 선언은
현재 타겟이 지정한 플랫폼이나 언어 버전과 일치하지 않으면
무시됩니다.
여러 개의 `available` 속성을 사용하는 경우
효과적인 가용성은
플랫폼과 Swift 가용성의 조합입니다.

<!--
  - test: `multipleAvailableAttributes`

  ```swifttest
  -> @available(iOS 9, *)
  -> @available(macOS 10.9, *)
  -> func foo() { }
  -> foo()
  ```
-->

`available` 속성이 플랫폼이나 언어 이름 인자 외에
`introduced` 인자를 지정하는 경우
아래와 같은 축약 문법을 사용할 수 있습니다:

```swift
@available(<#platform name#> <#version number#>, *)
@available(swift <#version number#>)
```

`available` 속성에 대한 축약 문법은
여러 플랫폼에 대한 가용성을 간결하게 표현합니다.
두 형식은 동일하지만
가능하면 축약 형식을 선호합니다.

```swift
@available(iOS 10.0, macOS 10.12, *)
class MyClass {
    // class definition
}
```

<!--
  - test: `availableShorthand`

  ```swifttest
  -> @available(iOS 10.0, macOS 10.12, *)
  -> class MyClass {
         // class definition
     }
  ```
-->

Swift 버전 숫자를 사용하여 가용성을 지정하는
`available` 속성은
선언에 플랫폼 가용성을 추가로 지정할 수 없습니다.
대신에 Swift 버전 가용성과 하나 이상의 플랫폼 가용성을 지정하기 위해
분리된 `available` 속성을 사용합니다.

```swift
@available(swift 3.0.2)
@available(macOS 10.12, *)
struct MyStruct {
    // struct definition
}
```

<!--
  - test: `availableMultipleAvailabilities`

  ```swifttest
  -> @available(swift 3.0.2)
  -> @available(macOS 10.12, *)
  -> struct MyStruct {
         // struct definition
     }
  ```
-->

### backDeployed

이 속성을 함수, 메드, 서브스크립트, 연산 프로퍼티에 적용하여
해당 기호 구현의 복사본을
기호를 호출하거나 접근하는 프로그램에 포함시킵니다.
이 속성을 사용하여 운영체제에 포함된 API와 같이
플랫폼의 일부로 제공되는 기호에 주석을 답니다.
이 속성은 기호에 접근하는 프로그램에 해당 구현의 복사본을 포함하여
사용할 수 있게 합니다.
구현 복사본은 *클라이언트로 내보내기(emitting into the client)*라고 합니다.

이 속성은 이 기호를 제공하는 플랫폼의 첫 번째 버전을 나타내는
`before:` 인자를 가지고 있습니다.
이 플랫폼 버전은 `available` 속성에 지정한 플랫폼 버전과
같은 의미를 가집니다.
`available` 속성과 다르게
목록은 모든 버전을 참조하는 별표(`*`)를 포함할 수 없습니다.
예를 들어 아래의 코드를 살펴봅시다:

```swift
@available(iOS 16, *)
@backDeployed(before: iOS 17)
func someFunction() { /* ... */ }
```

위의 예시에서,
iOS SDK는 iOS 17부터 `someFunction()`을 제공합니다.
추가로,
SDK는 백 배포(back deployment)를 사용하여 iOS 16에서도 `someFunction()`을 사용할 수 있도록 합니다.

이 함수를 호출하는 코드를 컴파일할 때,
Swift는 함수의 구현을 찾는 간접 참조 계층을 삽입합니다.
코드가 이 함수를 포함하는 SDK 버전에서 실행되면,
SDK의 구현이 사용됩니다.
그렇지 않으면 호출자에 포함된 복사본이 사용됩니다.
위의 예시에서
`someFunction()` 호출은 iOS 17이상의 버전에서 실행하면,
SDK의 구현이 사용되고,
iOS 16에서 실행하면
호출자에 포함된 `someFunction()`의 복사본이 사용됩니다.

> Note:
> 호출자의 최소 배포 타겟이
> 기호를 포함하는 SDK의 처음 버전과
> 같거나 더 높은 버전이면
> 컴파일러는 런타임 검사를 최적화하고
> SDK의 구현을 직접 호출할 수 있습니다.
> 이 경우에
> 백-배포된 기호에 직접 접근하면,
> 컴파일러가 클라이언트에서 기호의 구현 복사를
> 생략할 수도 있습니다.

<!--
Stripping out the copy emitted into the client
depends on a chain of optimizations that must all take place --
inlining the thunk,
constant-folding the availability check,
and stripping the emitted copy as dead code --
and the details could change over time,
so we don't guarantee in docs that it always happens.
-->

다음 기준을 충족하는 함수, 메서드, 서브스크립트, 연산 프로퍼티는
백 배포될 수 있습니다:

- 선언이 `public`이나 `@usableFromInline`입니다.
- 클래스 인스턴스 메서드와 클래스 타입 메서드의 경우
  메서드는 `final`로 표기하고 `@objc`로 표기하지 않습니다.
- 구현은 [inlinable](#inlinable)에서 설명한대로
  인라인 가능한 함수에 대한 요구사항을 충족합니다.

### discardableResult

이 속성을 함수나 메서드 선언에 적용하여
값을 반환하는 함수나 메서드의
결과를 사용하지 않고
호출할 때 컴파일러가 경고를 표시하지 않도록 합니다.

### dynamicCallable

이 속성을 클래스, 구조체, 열거형, 프로토콜에 적용하여
해당 타입의 인스턴스를 호출 가능한 함수처럼 취급할 수 있습니다.
이 타입은 `dynamicallyCall(withArguments:)` 메서드,
`dynamicallyCall(withKeywordArguments:)` 메서드
또는 둘 다 구현해야 합니다.

동적으로 호출 가능한 타입의 인스턴스를
여러 인자를 받는 함수인 것처럼 호출할 수 있습니다.

```swift
@dynamicCallable
struct TelephoneExchange {
    func dynamicallyCall(withArguments phoneNumber: [Int]) {
        if phoneNumber == [4, 1, 1] {
            print("Get Swift help on forums.swift.org")
        } else {
            print("Unrecognized number")
        }
    }
}

let dial = TelephoneExchange()

// Use a dynamic method call.
dial(4, 1, 1)
// Prints "Get Swift help on forums.swift.org"

dial(8, 6, 7, 5, 3, 0, 9)
// Prints "Unrecognized number"

// Call the underlying method directly.
dial.dynamicallyCall(withArguments: [4, 1, 1])
```

<!--
  - test: `dynamicCallable`

  ```swifttest
  -> @dynamicCallable
  -> struct TelephoneExchange {
         func dynamicallyCall(withArguments phoneNumber: [Int]) {
             if phoneNumber == [4, 1, 1] {
                 print("Get Swift help on forums.swift.org")
             } else {
                 print("Unrecognized number")
             }
         }
     }

  -> let dial = TelephoneExchange()

  -> // Use a dynamic method call.
  -> dial(4, 1, 1)
  <- Get Swift help on forums.swift.org

  -> dial(8, 6, 7, 5, 3, 0, 9)
  <- Unrecognized number

  -> // Call the underlying method directly.
  -> dial.dynamicallyCall(withArguments: [4, 1, 1])
  << Get Swift help on forums.swift.org
  ```
-->

`dynamicallyCall(withArguments:)` 메서드의 선언은
위의 예시에서 `[Int]`와 같이
[`ExpressibleByArrayLiteral`](https://developer.apple.com/documentation/swift/expressiblebyarrayliteral) 프로토콜을
준수하는 단일 매개변수만 가져야 합니다.
반환 타입은 모든 타입이 가능합니다.

`dynamicallyCall(withKeywordArguments:)` 메서드를 구현하면,
동적 메서드 호출에 레이블을 포함할 수 있습니다.

```swift
@dynamicCallable
struct Repeater {
    func dynamicallyCall(withKeywordArguments pairs: KeyValuePairs<String, Int>) -> String {
        return pairs
            .map { label, count in
                repeatElement(label, count: count).joined(separator: " ")
            }
            .joined(separator: "\n")
    }
}

let repeatLabels = Repeater()
print(repeatLabels(a: 1, b: 2, c: 3, b: 2, a: 1))
// a
// b b
// c c c
// b b
// a
```

<!--
  - test: `dynamicCallable`

  ```swifttest
  -> @dynamicCallable
     struct Repeater {
         func dynamicallyCall(withKeywordArguments pairs: KeyValuePairs<String, Int>) -> String {
             return pairs
                 .map { label, count in
                     repeatElement(label, count: count).joined(separator: " ")
                 }
                 .joined(separator: "\n")
         }
     }

  -> let repeatLabels = Repeater()
  -> print(repeatLabels(a: 1, b: 2, c: 3, b: 2, a: 1))
  </ a
  </ b b
  </ c c c
  </ b b
  </ a
  ```
-->

`dynamicallyCall(withKeywordArguments:)` 메서드의 선언은
[`ExpressibleByDictionaryLiteral`](https://developer.apple.com/documentation/swift/expressiblebydictionaryliteral)
프로토콜을
준수하는 단일 매개변수를 가지고
반환 타입은 모든 타입이 가능합니다.
매개변수의 [`Key`](https://developer.apple.com/documentation/swift/expressiblebydictionaryliteral/2294108-key)는
[`ExpressibleByStringLiteral`](https://developer.apple.com/documentation/swift/expressiblebystringliteral)을
준수해야 합니다.
이전 예시는 매개변수 타입으로
[`KeyValuePairs`](https://developer.apple.com/documentation/swift/keyvaluepairs)를 사용하므로
호출자는 중복 매개변수 레이블을 포함할 수 있습니다 —--
`a`와 `b`는 `repeat`을 호출하는데 여러 번 나타납니다.

`dynamicallyCall` 메서드 모두 구현하면,
`dynamicallyCall(withKeywordArguments:)`는
메서드 호출이 키워드 인자를 포함할 때 호출됩니다.
다른 경우에는 `dynamicallyCall(withArguments:)`가 호출됩니다.

동적으로 호출 가능한 인스턴스는 
`dynamicallyCall` 메서드 구현 중 하나에서
지정한 타입과 일치하는 인자와 반환값으로만 호출할 수 있습니다.
다음 예시에서 호출은
`KeyValuePairs<String, String>`을 가지는
`dynamicallyCall(withArguments:)`의 구현이 없으므로 컴파일되지 않습니다.

```swift
repeatLabels(a: "four") // Error
```

<!--
  - test: `dynamicCallable-err`

  ```swifttest
  >> @dynamicCallable
  >> struct Repeater {
  >>     func dynamicallyCall(withKeywordArguments pairs: KeyValuePairs<String, Int>) -> String {
  >>         return pairs
  >>             .map { label, count in
  >>                 repeatElement(label, count: count).joined(separator: " ")
  >>             }
  >>             .joined(separator: "\n")
  >>     }
  >> }
  >> let repeatLabels = Repeater()
  -> repeatLabels(a: "four") // Error
  !$ error: cannot convert value of type 'String' to expected argument type 'Int'
  !! repeatLabels(a: "four") // Error
  !! ^
  ```
-->

### dynamicMemberLookup

이 속성을 클래스, 구조체, 열거형, 프로토콜에 적용하여
런타임 시 멤버를 이름으로 찾을 수 있도록 합니다.
이 타입은 `subscript(dynamicMemberLookup:)` 서브스크립트를 구현해야 합니다.

명시적 멤버 표현식에서
명명된 멤버에 대한 해당 선언이 없는 경우
표현식은 타입의 `subscript(dynamicMemberLookup:)` 서브스크립트를
호출하는 것으로 이해하고
멤버에 대한 정보를 인자로 전달합니다.
서브스크립트는 키 경로나 멤버 이름 중 하나인 매개변수를 사용할 수 있습니다;
두 서브스크립트를 모두 구현하면
키 경로 인자를 가지는 서브스크립트가 사용됩니다.

`subscript(dynamicMemberLookup:)`의 구현은
[`KeyPath`](https://developer.apple.com/documentation/swift/keypath),
[`WritableKeyPath`](https://developer.apple.com/documentation/swift/writablekeypath),
[`ReferenceWritableKeyPath`](https://developer.apple.com/documentation/swift/referencewritablekeypath)
타입의 인자를 사용하여 키 경로를 받을 수 있습니다.
대부분 `String`인
[`ExpressibleByStringLiteral`](https://developer.apple.com/documentation/swift/expressiblebystringliteral) 프로토콜을
준수하는 타입의 인자를 사용하여 멤버 이름을 받을 수 있습니다.
서브스크립트의 반환 타입은 모든 타입이 가능합니다.

멤버 이름에 의한 동적 멤버 조회는
다른 언어의 데이터를 Swift로 연결할 때와 같이
컴파일 시에 타입을 확인할 수 없는
데이터에 대한 래퍼 타입을 생성하기 위해 사용될 수 있습니다.
예를 들어:

```swift
@dynamicMemberLookup
struct DynamicStruct {
    let dictionary = ["someDynamicMember": 325,
                      "someOtherMember": 787]
    subscript(dynamicMember member: String) -> Int {
        return dictionary[member] ?? 1054
    }
}
let s = DynamicStruct()

// Use dynamic member lookup.
let dynamic = s.someDynamicMember
print(dynamic)
// Prints "325"

// Call the underlying subscript directly.
let equivalent = s[dynamicMember: "someDynamicMember"]
print(dynamic == equivalent)
// Prints "true"
```

<!--
  - test: `dynamicMemberLookup`

  ```swifttest
  -> @dynamicMemberLookup
  -> struct DynamicStruct {
         let dictionary = ["someDynamicMember": 325,
                           "someOtherMember": 787]
         subscript(dynamicMember member: String) -> Int {
             return dictionary[member] ?? 1054
         }
     }
  -> let s = DynamicStruct()

  // Use dynamic member lookup.
  -> let dynamic = s.someDynamicMember
  -> print(dynamic)
  <- 325

  // Call the underlying subscript directly.
  -> let equivalent = s[dynamicMember: "someDynamicMember"]
  -> print(dynamic == equivalent)
  <- true
  ```
-->

키 경로에 의한 동적 멤버 조회는
컴파일 시 타입 검사를 제공하는 방법으로
래퍼 타입을 구현하는데 사용될 수 있습니다.
예를 들어:

```swift
struct Point { var x, y: Int }

@dynamicMemberLookup
struct PassthroughWrapper<Value> {
    var value: Value
    subscript<T>(dynamicMember member: KeyPath<Value, T>) -> T {
        get { return value[keyPath: member] }
    }
}

let point = Point(x: 381, y: 431)
let wrapper = PassthroughWrapper(value: point)
print(wrapper.x)
```

<!--
  - test: `dynamicMemberLookup`

  ```swifttest
  -> struct Point { var x, y: Int }

  -> @dynamicMemberLookup
     struct PassthroughWrapper<Value> {
         var value: Value
         subscript<T>(dynamicMember member: KeyPath<Value, T>) -> T {
             get { return value[keyPath: member] }
         }
     }

  -> let point = Point(x: 381, y: 431)
  -> let wrapper = PassthroughWrapper(value: point)
  -> print(wrapper.x)
  << 381
  ```
-->

### freestanding

`freestanding` 속성을 적용하여
독립 매크로(freestanding macro)를 선언합니다.

<!--

For the future, when other roles are supported:

The arguments to this attribute indicate the macro's roles:

- `expression`
  A macro that produces an expression

- `declaration`
  A macro that produces a declaration

Or are those supported today?
I see #error and #warning as @freestanding(declaration)
in the stdlib already:

https://github.com/swiftlang/swift/blob/main/stdlib/public/core/Macros.swift#L102
-->

### frozen

이 속성을 구조체 선언이나 열거형 선언에 적용하여
타입에 대해 만들 수 있는 변경의 종류를 제한합니다.
이 속성은 라이브러리 진화 모드(library evolution mode)로 컴파일할 때만 허용됩니다.
이후 버전의 라이브러리는
열거형의 케이스나
구조체의 저장 인스턴스 프로퍼티를
추가, 제거, 재정렬로 선언을 변경할 수 없습니다.
이러한 변경은 비고정 타입(nonfrozen types)에서 허용되지만
고정 타입(frozen types)에 대해 ABI 호환성을 깨뜨립니다.

<!--
  - test: `can-use-frozen-without-evolution`

  ```swifttest
  >> @frozen public enum E { case x, y }
  >> @frozen public struct S { var a: Int = 10 }
  ```
-->

<!--
  <rdar://problem/54041692> Using @frozen without Library Evolution has inconsistent error messages [SE-0260]
-->

<!--
  - test: `frozen-is-fine-with-evolution`

  ```swifttest
  >> @frozen public enum E { case x, y }
  >> @frozen public struct S { var a: Int = 10 }
  ```
-->

라이브러리 진화 모드에서
비고정 구조체와 열거형의 멤버와 상호작용하는 코드는
향후 버전의 라이브러리에서
해당 타입의 일부 멤버를 추가, 제거, 재정렬하더라도
다시 컴파일되지 않고 계속 작업할 수 있는 방식으로 컴파일됩니다.
컴파일러는 런타임에 정보 검색 및
간접 계층 추가와 같은
기술을 사용하여 이를 가능하게 합니다.
구조체나 열거형을 고정(frozen)으로 표시하면
성능을 얻기 위해 이러한 유연성을 포기합니다:
라이브러리의 향후 버전은 타입을 제한적으로 변경할 수 있지만
컴파일러는 타입의 멤버와 상호작용하는 코드에서
추가 최적화를 수행할 수 있습니다.

고정 타입,
고정 구조체의 저장 프로퍼티와
고정 열거형 케이스의 연관값의 타입은
public이거나 `usableFromInline` 속성으로 표시되어야 합니다.
고정 구조체의 프로퍼티는 프로퍼티 관찰자를 가질 수 없고
저장 인스턴스 프로퍼티에 대해 초기값을 제공하는 표현식은
[inlinable](#inlinable)에서 설명한대로
인라인 가능 함수와 동일한 제한사항을 따릅니다.

<!--
  - test: `frozen-struct-prop-init-cant-refer-to-private-type`

  ```swifttest
  >> public protocol P { }
  >> private struct PrivateStruct: P { }
  >>         public struct S1 { var fine: P = PrivateStruct() }
  >> @frozen public struct S2 { var nope: P = PrivateStruct() }
  !$ error: struct 'PrivateStruct' is private and cannot be referenced from a property initializer in a '@frozen' type
  !! @frozen public struct S2 { var nope: P = PrivateStruct() }
  !!                                          ^
  !$ note: struct 'PrivateStruct' is not '@usableFromInline' or public
  !! private struct PrivateStruct: P { }
  !!                ^
  !$ error: initializer 'init()' is private and cannot be referenced from a property initializer in a '@frozen' type
  !! @frozen public struct S2 { var nope: P = PrivateStruct() }
  !! ^
  !$ note: initializer 'init()' is not '@usableFromInline' or public
  !! private struct PrivateStruct: P { }
  !! ^
  ```
-->

커맨드 라인에서 라이브러리 진화 모드를 활성화 하려면
`-enable-library-evolution` 옵션을 Swift 컴파일러에 전달해야 합니다.
Xcode에서 가능하게 하려면
[Xcode 도움말 (Xcode Help)](https://help.apple.com/xcode/mac/current/#/dev04b3a04ba)에서 설명한대로
"Build Libraries for Distribution" 빌드 설정
(`BUILD_LIBRARY_FOR_DISTRIBUTION`)을 Yes로 설정해야 합니다.

<!--
  This is the first time we're talking about a specific compiler flag/option.
  In the long term, the discussion of library evolution mode
  will need to move to a new chapter in the guide
  that also talks about things like @available and ABI.
  See <rdar://problem/51929017> TSPL: Give guidance to library authors about @available @frozen and friends
-->

고정 열거형에 대한 switch 구문은 [향후 열거형 케이스 전환 (Switching Over Future Enumeration Cases)](./statements.md#향후-열거형-케이스-전환-switching-over-future-enumeration-cases)에서 설명한대로
`default` 케이스를 요구하지 않습니다.
고정 열거형을 사용할 때
`default`나 `@unknown default` 케이스를 포함하면
해당 코드는 실행되지 않기 때문에 경고가 생성됩니다.

<!--
  - test: `NoUnknownDefaultOverFrozenEnum`

  ```swifttest
  >> public enum E { case x, y }
  >> @frozen public enum F { case x, y }
  ```
-->

<!--
  - test: `NoUnknownDefaultOverFrozenEnum_Test1`

  ```swifttest
  >> import NoUnknownDefaultOverFrozenEnum
  >> func main() {
  >>     let e = NoUnknownDefaultOverFrozenEnum.E.x
  >>     switch e {
  >>         case .x: print(9)
  >>         case .y: print(8)
  >>         @unknown default: print(0)
  >>     }
  >> }
  // Note that there's no warning -- this is fine because E isn't frozen.
  ```
-->

<!--
  - test: `NoUnknownDefaultOverFrozenEnum_Test2`

  ```swifttest
  >> import NoUnknownDefaultOverFrozenEnum
  >> func main() {
  >>     let f = NoUnknownDefaultOverFrozenEnum.F.x
  >>     switch f {
  >>         case .x: print(9)
  >>         case .y: print(8)
  >>         @unknown default: print(0)
  >>     }
  >> }
  // --- Main warning ---
  !! /tmp/sourcefile_0.swift:7:18: warning: case is already handled by previous patterns; consider removing it
  !! @unknown default: print(0)
  !! ~~~~~~~~~^~~~~~~~~~~~~~~~~
  !! /tmp/sourcefile_0.swift:7:9: warning: default will never be executed
  !! @unknown default: print(0)
  !! ^
  // --- Junk/ancillary warnings ---
  !! /tmp/sourcefile_0.swift:4:12: warning: switch condition evaluates to a constant
  !! switch f {
  !! ^
  !! /tmp/sourcefile_0.swift:6:24: note: will never be executed
  !! case .y: print(8)
  !! ^
  ```
-->

### GKInspectable

이 속성을 적용하여 커스텀 GameplayKit 구성요소 프로퍼티를
SpriteKit 에디터 UI에 노출합니다.
이 속성을 적용하면 `objc` 속성도 암시적으로 포함합니다.

<!--
  See also <rdar://problem/27287369> Document @GKInspectable attribute
  which we will want to link to, once it's written.
-->

### globalActor

이 속성을 액터, 구조체, 열겨헝, final 클래스에 적용합니다.
이 타입은 `shared`라는 정적 프로퍼티를 정의해야 하며,
이 프로퍼티는 액터의 공유 인스턴스를 제공합니다.

전역 액터는 액터 격리의 개념을
코드의 여러 곳 ---
여러 타입, 모듈과 같은 ---
에 분산된 상태로 만들며 동시성 코드에서 전역 변수를 안전하게 접근할 수 있도록 합니다.
전역 액터가
`shared` 프로퍼티의 값으로 제공하는 액터는
모든 상태의 접근을 직렬화합니다.
또한 모든 코드가 동일한 스레드에서 실행되어야 하는 경우와 같이
동시성 코드의 제약조건을 모델링하는 데 전역 액터를 사용할 수도 있습니다.

전역 액터는 [`GlobalActor`][] 프로토콜을 암시적 준수합니다.
메인 액터는 [메인 액터 (The Main Actor)](../language-guide-1/concurrency.md#메인-액터-the-main-actor)에서 설명한대로
표준 라이브러리에서 제공하는 전역 액터입니다.
대부분 코드에서는 새로운 전역 액터를 정의하는 대신 메인 액터를 사용할 수 있습니다.

[`GlobalActor`]: https://developer.apple.com/documentation/swift/globalactor

### inlinable

이 속성을
함수, 메서드, 연산 프로퍼티, 서브스크립트,
편의 이니셜라이저, 디이니셜라이저 선언에 적용하여
해당 선언의 구현을
public 인터페이스 일부로 노출합니다.
컴파일러는 호출 지점에서 인라인 가능 기호에 대한 호출을
기호 구현의 복사본으로 대체할 수 있습니다.

인라인 가능 코드는
모든 모듈에 선언된 `open`과 `public` 기호와 상호작용할 수 있고,
동일한 모듈에 선언되어
`usableFromInline` 속성으로 표시된
`internal` 기호와도 상호작용할 수 있습니다.
인라인 가능 코드는 `private`이나 `fileprivate` 기호와 상호작용할 수 없습니다.

이 속성은
함수 내에 중첩된 선언이나
`fileprivate` 또는 `private` 선언에 적용될 수 없습니다.
인라인 가능 함수 내에 선언된 함수와 클로저는
이 속성으로 표시될 수 없더라도
암시적으로 인라인 가능합니다.

<!--
  - test: `cant-inline-private`

  ```swifttest
  >> @inlinable private func f() { }
  !$ error: '@inlinable' attribute can only be applied to public declarations, but 'f' is private
  !! @inlinable private func f() { }
  !! ^~~~~~~~~~~
  ```
-->

<!--
  - test: `cant-inline-nested`

  ```swifttest
  >> public func outer() {
  >>    @inlinable func f() { }
  >> }
  !$ error: '@inlinable' attribute can only be applied to public declarations, but 'f' is private
  !! @inlinable func f() { }
  !! ^~~~~~~~~~~
  !!-
  ```
-->

<!--
  TODO: When we get resilience, this will actually be a problem.
  Until then, per discussion with [Contributor 6004], there's no (supported) way
  for folks to get into the state where this behavior would be triggered.

  If a project uses a module that includes inlinable functions,
  the inlined copies aren't necessarily updated
  when the module's implementation of the function changes.
  For this reason,
  an inlinable function must be compatible with
  every past version of that function.
  In most cases, this means
  externally visible aspects of their implementation can't be changed.
  For example,
  an inlinable hash function can't change what algorithm is used ---
  inlined copies outside the module would use the old algorithm
  and the noninlined copy would use the new algorithm,
  yielding inconsistent results.
-->

### main

이 속성을 구조체, 클래스, 열거형 선언에 적용하여
프로그램 흐름의 최상위 시작점을 포함하는 것을 나타냅니다.
이 타입은 인자가 없고 `Void`를 반환하는
`main` 타입 함수를 제공해야 합니다.
예를 들어:

```swift
@main
struct MyTopLevel {
    static func main() {
        // Top-level code goes here
    }
}
```

<!--
  - test: `atMain`

  ```swifttest
  -> @main
  -> struct MyTopLevel {
  ->     static func main() {
  ->         // Top-level code goes here
  >>         print("Hello")
  ->     }
  -> }
  << Hello
  ```
-->

`main` 속성의 요구사항을 설명하는 또 다른 방법은
이 속성을 작성하는 타입이
다음 가상의 프로토콜을 준수하는 타입과
동일한 요구사항을 충족해야 한다는 것입니다:

```swift
protocol ProvidesMain {
    static func main() throws
}
```

<!--
  - test: `atMain_ProvidesMain`

  ```swifttest
  -> protocol ProvidesMain {
         static func main() throws
     }
  ```
-->

실행 가능하도록 만들기 위해 컴파일한 Swift 코드는
[최상위-수준 코드 (Top-Level Code)](./declarations.md#최상위-수준-코드-top-level-code)에서 설명한대로
최상위 시작점을 포함해야 합니다.

<!--
  - test: `no-at-main-in-top-level-code`

  ```swifttest
  // This is the same example as atMain, but without :compile: true.
  >> @main
  >> struct MyTopLevel {
  >>     static func main() {
  >>         print("Hello")
  >>     }
  >> }
  !$ error: 'main' attribute cannot be used in a module that contains top-level code
  !! @main
  !! ^
  !$ note: top-level code defined in this source file
  !! @main
  !! ^
  ```
-->

<!--
  - test: `atMain_library`

  ```swifttest
  -> // In file "library.swift"
  -> open class C {
         public static func main() { print("Hello") }
     }
  ```
-->

<!--
  - test: `atMain_client`

  ```swifttest
  -> import atMain_library
  -> @main class CC: C { }
  ```
-->

### nonobjc

이 속성을
메서드, 프로퍼티, 서브스크립트, 이니셜라이저 선언에 적용하여
암시적으로 `objc` 속성을 억제합니다.
`nonobjc` 속성은
선언이 Objective-C에서 표현 가능하더라도
컴파일러에게 Objective-C 코드에서 해당 선언을 사용할 수 없도록 합니다.

이 속성을 확장에 적용하면
명시적으로 `objc` 속성으로 표시되지 않은
확장의 모든 멤버에 적용하는 것과
동일한 효과를 가집니다.

`nonobjc` 속성은 `objc` 속성으로 표시된
클래스에서 브리징 메서드에 대한 순환성을 해결하고
`objc` 속성으로 표시된 클래스의
메서드와 이니셜라이저의 오버로딩을 허용하기 위해 사용합니다.

`nonobjc` 속성으로 표시된 메서드는
`objc` 속성으로 표시된 메서드를 재정의할 수 없습니다.
그러나 `objc` 속성으로 표시된 메서드는
`nonobjc` 속성으로 표시된 메서드를 재정의할 수 있습니다.
유사하게 `nonobjc` 속성으로 표시된 메서드는
`objc` 속성으로 표시된 메서드에 대한
프로토콜 요구사항을 충족할 수 없습니다.

### NSApplicationMain

> Deprecated:
> 이 속성은 사용 중단되었습니다;
> 대신 [main](#main) 속성을 사용합니다.
> Swift 6에서
> 이 속성을 사용하면 오류가 발생합니다.

이 속성을 클래스에 적용하여
앱 델리게이트임을 나타냅니다.
이 속성을 사용하는 것은
`NSApplicationMain(_:_:)` 함수를 호출하는 것과 동일합니다.

이 속성을 사용하지 않으면,
`main.swift` 파일에 다음과 같이
`NSApplicationMain(_:_:)` 함수를 호출하는 최상위 코드를 제공해야 합니다:

```swift
import AppKit
NSApplicationMain(CommandLine.argc, CommandLine.unsafeArgv)
```

<!--
  Above code isn't tested because it hangs the REPL indefinitely,
  which is correct behavior if you call a non-returning function like this.
-->

실행 파일을 만들기 위해 컴파일한 Swift 코드는
[최상위-수준 코드 (Top-Level Code)](./declarations.md#최상위-수준-코드-top-level-code)에서 설명한대로
하나의 최상위 시작점을 포함할 수 있습니다.

### NSCopying

이 속성을 클래스의 변수 저장 프로퍼티에 적용합니다.
이 속성은 프로퍼티의 setter가 프로퍼티 값 자체 대신
`copyWithZone(_:)` 메서드에 의해 반환된
프로퍼티 값의 *복사본(copy)*으로 합성되도록 합니다.
프로퍼티의 타입은 `NSCopying` 프로토콜을 준수해야 합니다.

`NSCopying` 속성은 Objective-C `copy` 프로퍼티 속성과
유사하게 동작합니다.

<!--
  TODO: If and when Dave includes a section about this in the Guide,
  provide a link to the relevant section.
-->

### NSManaged

이 속성을 `NSManagedObject`를 상속받는 클래스의
인스턴스 메서드나 변수 저장 프로퍼티에 적용하여
코어 데이터(Core Data)가 관련 엔티티 설명에 따라
런타임 시 해당 구현을 동적으로 제공하는 것을 나타냅니다.
`NSManaged` 속성으로 표시된 프로퍼티의 경우
코어 데이터(Core Data)는 런타임에 저장소도 제공합니다.
이 속성을 적용하면 `objc` 속성도 암시적으로 포함합니다.

### objc

이 속성을 Objective-C로 표현될 수 있는 모든 선언에 적용합니다 ---
예를 들어 중첩되지 않은 클래스, 프로토콜,
제네릭이 아닌 열거형(정수 원시값 타입으로 제한),
클래스 및 프로토콜의 프로퍼티, 메서드(getter와 setter 포함),
프로토콜의 옵셔널 멤버,
이니셜라이저, 서브스크립트에 적용됩니다.
`objc` 속성은
해당 선언이 Objective-C 코드에서 사용 가능함을 컴파일러에게 알려줍니다.

이 속성을 확장에 적용하면
명시적으로 `nonobjc` 속성으로 표시되지 않은
확장의 모든 멤버에 적용하는 것과
동일한 효과를 가집니다.

컴파일러는 암시적으로 Objective-C에 정의된 모든 클래스의 하위 클래스에
`objc` 속성을 추가합니다.
그러나 하위 클래스는 제네릭이 아니어야 하며,
제네릭 클래스를 상속해선 안됩니다.
이러한 기준을 충족하는 하위 클래스에
명시적으로 `objc` 속성을 추가하여
아래에서 설명한대로 Objective-C 이름을 지정할 수 있습니다.
`objc` 속성으로 표시된 프로토콜은
이 속성이 표시되지 않은 프로토콜을 상속할 수 없습니다.

`objc` 속성은 다음과 같은 경우에 암시적으로 추가됩니다:

- 선언이 하위 클래스의 재정의이고,
  하위 클래스의 선언이 `objc` 속성을 가지고 있습니다.
- 선언이 `objc` 속성을 가지는 프로토콜의
  요구사항을 충족합니다.
- 선언이 `IBAction`, `IBSegueAction`, `IBOutlet`,
  `IBDesignable`, `IBInspectable`,
  `NSManaged`, `GKInspectable` 속성을 가지고 있습니다.

열거형에 `objc` 속성을 적용하면,
각 열거형 케이스는 열거형 이름과 케이스 이름의 연결로
Objective-C 코드에 노출됩니다.
케이스 이름의 첫 번째 문자는 대문자입니다.
예를 들어 Swift `Planet` 열거형의 `venus`라는 케이스는
Objective-C 코드에서 `PlanetVenus`라는 케이스로 노출됩니다.

`objc` 속성은 선택적으로 단일 속성 인자를 받으며,
이 인자는 식별자로 구성됩니다.
이 식별자는 `objc` 속성을 적용하는 엔티티에 대해
Objective-C에 노출될 이름을 지정합니다.
이 인자를 사용하여
클래스, 열거형, 열거형 케이스, 프로토콜,
메서드, getter, setter, 이니셜라이저의 이름을 지정할 수 있습니다.
클래스, 프로토콜, 열거형에 대해
Objective-C 이름을 지정하는 경우,
[Objective-C를 사용한 프로그래밍 (Programming with Objective-C)](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011210)의
[규칙 (Conventions)](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Conventions/Conventions.html#//apple_ref/doc/uid/TP40011210-CH10-SW1)에서 설명한대로
이름에 세 글자 접두사를 포함해야 합니다.
아래 예시는
`ExampleClass`의 `enabled` 프로퍼티에 대한 getter를
프로퍼티 이름 자체 대신
`isEnabled`로 Objective-C 코드에 노출합니다.

```swift
class ExampleClass: NSObject {
    @objc var enabled: Bool {
        @objc(isEnabled) get {
            // Return the appropriate value
        }
    }
}
```

<!--
  - test: `objc-attribute`

  ```swifttest
  >> import Foundation
  -> class ExampleClass: NSObject {
  ->    @objc var enabled: Bool {
  ->       @objc(isEnabled) get {
  ->          // Return the appropriate value
  >>          return true
  ->       }
  ->    }
  -> }
  ```
-->

더 자세한 내용은
[Objective-C에 Swift 가져오기 (Importing Swift into Objective-C)](https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis/importing_swift_into_objective-c)를 참고바랍니다.

> Note: `objc` 속성의 인자는
> 해당 선언에 대한 런타임 이름도 변경할 수 있습니다.
> [`NSClassFromString(_:)`](https://developer.apple.com/documentation/foundation/1395135-nsclassfromstring)과 같이
> Objective-C 런타임과 상호작용하는 함수를 호출할 때와
> 앱의 info.plist 파일에 클래스 이름을 지정할 때
> 런타임 이름을 사용합니다.
> 인자를 전달하여 이름을 지정하면,
> 해당 이름은 Objective-C 코드와
> 런타임 이름으로 사용됩니다.
> 인자를 생략하면,
> Objective-C 코드에서 사용된 이름은 Swift 코드의 이름과 동일하고
> 런타임 이름은 일반 Swift 컴파일러
> 이름 변경 규칙을 따릅니다.

### objcMembers

이 속성을 클래스 선언에 적용하여
클래스의 모든 Objective-C 호환 멤버,
확장, 하위 클래스, 그리고 모든 확장의 하위 클래스에
암시적으로 `objc` 속성을 적용합니다.

대부분의 코드는 필요한 선언만 노출시키기 위해
대신 `objc` 속성을 사용합니다.
많은 선언의 노출이 필요하다면
`objc` 속성을 가진 확장 내에 그룹화할 수 있습니다.
`objcMembers` 속성은
Objective-C 런타임의 인트로스펙션 기능(introspection facilities)을
많이 사용하는 라이브러리에 대한 편의를 위한 것입니다.
필요하지 않은 곳에 `objc` 속성을 적용하면
바이너리 크기를 증가시키고 성능에 부정적인 영향을 미칠 수 있습니다.

<!--
  The binary size comes from the additional thunks
  to translate between calling conventions.
  The performance of linking and launch are slower
  because of the larger symbol table slowing dyld down.
-->

### preconcurrency

이 속성을 선언에 적용하여
엄격한 동시성 검사를 억제합니다.
다음과 같은 선언에
이 속성을 적용할 수 있습니다:

- 임포트(Imports)
- 구조체(Structures), 클래스(classes), 액터(actors)
- 열거형(Enumerations)과 열거형 케이스(enumeration cases)
- 프로토콜(Protocols)
- 변수(Variables)와 상수(constants)
- 서브스크립트(Subscripts)
- 이니셜라이저(Initializers)
- 함수(Functions)

임포트(import) 선언에서
이 속성은 임포트한 모듈의 타입을 사용하는 코드에서
동시성 검사의 엄격함을 줄입니다.
특히
명시적으로 non-sendable로 표시되지 않은
임포트한 모듈의 타입은
sendable 타입을 요구하는 컨텍스트에서 사용될 수 있습니다.

다른 선언에서
이 속성은 선언되는 기호를 시용하는 코드에 대해
동시성 검사의 엄격성을 감소시킵니다.
최소한의 동시성 검사를 가지는 범위에서 이 기호를 사용하면,
`Sendable` 요구사항이나 전역 액터와 같은
해당 기호로 지정된 동시성 관련 제약조건이
검사되지 않습니다.

이 속성은 엄격한 동시성 검사로 코드를 변환하는데
다음과 같이 사용할 수 있습니다:

1. 엄격한 검사를 활성화합니다.
2. 엄격한 검사를 활성화하지 않은 모듈에 대해
   임포트 선언에 `preconcurrency` 속성을 표시합니다.
3. 모듈을 엄격한 검사로 변환 후에,
   `preconcurrency` 속성을 삭제합니다.
   컴파일러는 임포트에 있는 `preconcurrency` 속성이
   더 이상 영향을 미치지 않고 삭제되어야 하는 위치에
   경고를 나타냅니다.

다른 선언의 경우,
엄격한 검사에 대한 변환이 되지 않은
클라이언트가 있는 경우에,
동시성 관련 제약조건을 선언에 추가할 때
`preconcurrency` 속성을 추가합니다.
`preconcurrency` 속성은 클라언트가 모두 변환된 후에 삭제합니다.

Objective-C의 선언은 항상
`preconcurrency` 속성이 선언된 것처럼 임포트됩니다.

### propertyWrapper

이 속성을 클래스, 구조체, 열거형 선언에 적용하여
프로퍼티 래퍼(protperty wrapper)로 사용합니다.
타입에 이 속성을 적용하면,
타입과 동일한 이름으로 커스텀 속성을 생성합니다.
이 새로운 속성을 클래스, 구조체, 열거형의 프로퍼티에 적용하여
래퍼 타입의 인스턴스로 프로퍼티에 대한 접근을 감쌉니다;
이 속성을 지역 저장 변수 선언에 적용하면
같은 방식으로 변수에 대한 접근을 감쌉니다.
연산 변수, 전역 변수, 상수는 프로퍼티 래퍼를 사용할 수 없습니다.

<!--
  - test: `property-wrappers-can-go-on-stored-variable`

  ```swifttest
  >> @propertyWrapper struct UselessWrapper { var wrappedValue: Int }
  >> func f() {
  >>     @UselessWrapper var d: Int = 20
  >>     print(d)
  >> }
  >> f()
  << 20
  ```
-->

<!--
  - test: `property-wrappers-cant-go-on-constants`

  ```swifttest
  >> @propertyWrapper struct UselessWrapper { var wrappedValue: Int }
  >> func f() {
  >>     @UselessWrapper let d: Int = 20
  >>     print(d)
  >> }
  !$ error: property wrapper can only be applied to a 'var'
  !! @UselessWrapper let d: Int = 20
  !! ^
  ```
-->

<!--
  - test: `property-wrappers-cant-go-on-computed-variable`

  ```swifttest
  >> @propertyWrapper struct UselessWrapper { var wrappedValue: Int }
  >> func f() {
  >>     @UselessWrapper var d: Int { return 20 }
  >>     print(d)
  >> }
  >> f()
  !$ error: property wrapper cannot be applied to a computed property
  !! @UselessWrapper var d: Int { return 20 }
  !! ^
  ```
-->

래퍼는 `wrappedValue` 인스턴스 프로퍼티를 정의해야 합니다.
프로퍼티의 *래핑값(wrapped value)*은
이 프로퍼티의 getter와 setter를 노출하는 값입니다.
대부분의 경우 `wrappedValue`는 연산값이지만,
저장값일 수도 있습니다.
래퍼는
래핑값에 필요한 기본 저장소를 정의하고 관리합니다.
컴파일러는 래핑 프로퍼티의 이름 앞에 언더바(`_`)로
래퍼 타입의 인스턴스에 대해 저장소를 합성합니다 ---
예를 들어 `someProperty`에 대한 래퍼는 `_someProperty`로 저장됩니다.
래퍼에 대한 합성된 저장소는 `private`의 접근 제어 수준을 가집니다.

프로퍼티 래퍼를 가지는 프로퍼티는
`willSet`과 `didSet` 블록을 포함할 수 있지만
컴파일러가 합성한 `get`이나 `set` 블록을 재정의할 수 없습니다.

Swift는 프로퍼티 래퍼의 초기화에 대해
두 가지의 편의 문법(syntactic sugar)을 제공합니다.
래핑값의 정의에서 할당 문법을 사용하여
할당의 오른쪽에 있는 표현식을
프로퍼티 래퍼 이니셜라이저의
`wrappedValue` 매개변수로 전달할 수 있습니다.
또한 프로퍼티에 속성을 적용할 때
인자를 제공할 수도 있으며
이 인자는 프로퍼티 래퍼의 이니셜라이저로 전달됩니다.
예를 들어 아래 코드에서
`SomeStruct`는 `SomeWrapper`가 정의한 각 이니셜라이저를 호출합니다.

```swift
@propertyWrapper
struct SomeWrapper {
    var wrappedValue: Int
    var someValue: Double
    init() {
        self.wrappedValue = 100
        self.someValue = 12.3
    }
    init(wrappedValue: Int) {
        self.wrappedValue = wrappedValue
        self.someValue = 45.6
    }
    init(wrappedValue value: Int, custom: Double) {
        self.wrappedValue = value
        self.someValue = custom
    }
}

struct SomeStruct {
    // Uses init()
    @SomeWrapper var a: Int

    // Uses init(wrappedValue:)
    @SomeWrapper var b = 10

    // Both use init(wrappedValue:custom:)
    @SomeWrapper(custom: 98.7) var c = 30
    @SomeWrapper(wrappedValue: 30, custom: 98.7) var d
}
```

<!--
  - test: `propertyWrapper`

  ```swifttest
  -> @propertyWrapper
  -> struct SomeWrapper {
         var wrappedValue: Int
         var someValue: Double
         init() {
             self.wrappedValue = 100
             self.someValue = 12.3
         }
         init(wrappedValue: Int) {
             self.wrappedValue = wrappedValue
             self.someValue = 45.6
         }
         init(wrappedValue value: Int, custom: Double) {
             self.wrappedValue = value
             self.someValue = custom
         }
     }

  -> struct SomeStruct {
  ->     // Uses init()
  ->     @SomeWrapper var a: Int

  ->     // Uses init(wrappedValue:)
  ->     @SomeWrapper var b = 10

  ->     // Both use init(wrappedValue:custom:)
  ->     @SomeWrapper(custom: 98.7) var c = 30
  ->     @SomeWrapper(wrappedValue: 30, custom: 98.7) var d
  -> }
  ```
-->

<!--
  Comments in the SomeStruct part of the example above
  are on the line before instead of at the end of the line
  because the last example gets too long to fit on one line.
-->

<!--
  Initialization of a wrapped property using ``init(wrappedValue:)``
  can be split across multiple statements.
  However, you can only see that behavior using local variables
  which currently can't have a property wrapper.
  It would look like this:

  -> @SomeWrapper var e
  -> e = 20  // Uses init(wrappedValue:)
  -> e = 30  // Uses the property setter
-->

래핑 프로퍼티의 *투영값(projected value)*은
프로퍼티 래퍼가 추가 기능을 노출하기 위해 사용할 수 있는 두 번째 값입니다.
프로퍼티 래퍼 타입의 작성자는
투영값의 의미를 결정하고
투영값이 노출하는 인터페이스를 정의할 책임이 있습니다.
프로퍼티 래퍼에서 값을 예상하려면,
래퍼 타입에 `projectedValue` 인스턴스 프로퍼티를 정의합니다.
컴파일러는 래핑 프로퍼티의 이름 앞에 달러 기호(`$`)로
투영값에 대한 식별자를 합성합니다 ---
예를 들어 `someProperty`에 대한 투영값은 `$someProperty`입니다.
투영값은 기존의 래핑 프로퍼티와
동일한 접근 제어 수준을 가집니다.

```swift
@propertyWrapper
struct WrapperWithProjection {
    var wrappedValue: Int
    var projectedValue: SomeProjection {
        return SomeProjection(wrapper: self)
    }
}
struct SomeProjection {
    var wrapper: WrapperWithProjection
}

struct SomeStruct {
    @WrapperWithProjection var x = 123
}
let s = SomeStruct()
s.x           // Int value
s.$x          // SomeProjection value
s.$x.wrapper  // WrapperWithProjection value
```

<!--
  - test: `propertyWrapper-projection`

  ```swifttest
  -> @propertyWrapper
  -> struct WrapperWithProjection {
         var wrappedValue: Int
         var projectedValue: SomeProjection {
             return SomeProjection(wrapper: self)
         }
  }
  -> struct SomeProjection {
         var wrapper: WrapperWithProjection
  }

  -> struct SomeStruct {
  ->     @WrapperWithProjection var x = 123
  -> }
  -> let s = SomeStruct()
  >> _ =
  -> s.x           // Int value
  >> _ =
  -> s.$x          // SomeProjection value
  >> _ =
  -> s.$x.wrapper  // WrapperWithProjection value
  ```
-->

### resultBuilder

이 속성을 클래스, 구조체, 열거형에 적용하여
해당 타입을 결과 빌더(result builder)로 사용합니다.
*결과 빌더(result builder)*는
중첩된 데이터 구조를 단계별로 구축하는 타입입니다.
결과 빌더를 사용하여 중첩된 데이터 구조를 자연스럽고 선언적인 방식으로 생성하기 위한
도메인 특화 언어(DSL)를 구현합니다.
`resultBuilder` 속성을 어떻게 사용하는지에 대한 예시는
[결과 빌더 (Result Builders)](../language-guide-1/advanced-operators.md#결과-빌더-result-builders)를 참고바랍니다.

#### 결과-구축 메서드 (Result-Building Methods)

결과 빌더는 아래에 설명된 정적 메서드를 구현합니다.
결과 빌더의 모든 기능은
정적 메서드를 통해 노출되므로
해당 타입의 인스턴스를 초기화하지 않습니다.
결과 빌더는 `buildBlock(_:)` 메서드를 구현하거나
`buildPartialBlock(first:)`와
`buildPartialBlock(accumulated:next:)` 메서드를 모두 구현해야 합니다.
DSL에서 추가 기능을 가능하게 하는
다른 메서드는
선택 사항입니다.
결과 빌더 타입의 선언은
프로토콜 준수를 포함할 필요가 없습니다.

정적 메서드의 설명은 세 가지 타입을 플레이스 홀더로 사용합니다.
`Expression` 타입은
결과 빌더의 입력 타입에 대한 플레이스 홀더이고,
`Component`는 부분 결과 타입에 대한 플레이스 홀더이며,
`FinalResult`는
결과 빌더가 생성하는 결과 타입에 대한 플레이스 홀더입니다.
이러한 타입은 결과 빌더가 사용하는 실제 타입으로 대체해야 합니다.
결과-구축 메서드가
`Expression`이나 `FinalResult`에 대한 타입을 지정하지 않으면
`Component`와 동일한 타입으로 기본 설정됩니다.

결과-구축 메서드는 다음과 같습니다:

- `static func buildBlock(_ components: Component...) -> Component`:
  부분 결과의 배열을 단일 부분 결과로 결합합니다.

- `static func buildPartialBlock(first: Component) -> Component`:
  첫 번째 컴포넌트로부터 부분 결과 컴포넌트를 구축합니다.
  한 번에 하나의 컴포넌트로 블록을 구축하는 것을 지원하기위해
  이 메서드와 `buildPartialBlock(accumulated:next:)` 메서드를 모두 구현합니다.
  `buildBlock(_:)`에 비해,
  이 접근은 인자의 다양한 수를 처리하는
  일반적인 오버로드의 필요성을 줄여줍니다.

- `static func buildPartialBlock(accumulated: Component, next: Component) -> Component`:
  축적된 컴포넌트와 새로운 컴포넌트를 결합하여
  부분 결과 컴포넌트를 구축합니다.
  한 번에 하나의 컴포넌트로 블록을 구축하는 것을 지원하기위해
  이 메서드와 `buildPartialBlock(first:)` 메서드를 모두 구현합니다.
  `buildBlock(_:)`에 비해,
  이 접근은 인자의 다양한 수를 처리하는
  일반적인 오버로드의 필요성을 줄여줍니다.

결과 빌더는 위에 나열된 세 가지 블록-구축 메서드 모두 구현할 수 있습니다.
이 경우에 가용성에 따라 호출되는 메서드가 결정됩니다.
기본적으로 Swift는 `buildPartialBlock(first:)`와 `buildPartialBlock(accumulated:next:)` 메서드를 호출합니다.
Swift가 대신 `buildBlock(_:)`을 호출하도록 하려면
`buildPartialBlock(first:)`와
`buildPartialBlock(accumulated:next:)`에 지정한 가용성보다
먼저 결과 빌더를 감싸고 있는 선언부에 가용성을 지정해야 합니다.

추가적인 결과-구축 메서드는 다음과 같습니다:

- `static func buildOptional(_ component: Component?) -> Component`:
  `nil`이 가능한 부분 결과로부터 부분 결과를 구축합니다.
  `else` 절을 포함하지 않은
  `if` 구문을 지원하려면 이 메서드를 구현해야 합니다.

- `static func buildEither(first: Component) -> Component`:
  일부 조건에 따라 값이 달라지는 부분 결과를 구축합니다.
  `switch` 구문과
  `else` 절을 포함하는 `if` 구문을 제공하려면
  이 메서드와 `buildEither(second:)`를 모두 구현해야 합니다.

- `static func buildEither(second: Component) -> Component`:
  일부 조건에 따라 값이 달리지는 부분 결과를 구축합니다.
  `switch` 구문과
  `else` 절을 포함하는 `if` 구문을 제공하려면
  이 메서드와 `buildEither(first:)`를 모두 구현해야 합니다.

- `static func buildArray(_ components: [Component]) -> Component`:
  부분 결과 배열로부터 부분 결과를 구축합니다.
  `for` 루프를 지원하려면 이 메서드를 구현해야 합니다.

- `static func buildExpression(_ expression: Expression) -> Component`:
  표현식으로부터 부분 결과를 구축합니다.
  이 메서드를 구현하여 전처리
  (예: 표현식을 내부 타입으로 변환)를 수행하거나
  사용하는 곳에서 타입 추론을 위한 추가 정보를 제공하기 위해 구현할 수 있습니다.

- `static func buildFinalResult(_ component: Component) -> FinalResult`:
  부분 결과로부터 최종 결과를 구축합니다.
  부분과 최종 결과에 대한 다른 타입을 사용하거나
  결과를 반환하기 전에 다른 후처리를 수행하는
  결과 빌더의 부분으로 이 메서드를 구현할 수 있습니다.

- `static func buildLimitedAvailability(_ component: Component) -> Component`:
  타입 정보를 지우는 부분 결과를 구축합니다.
  가용성을 검사하는
  컴파일러-제어 구문 외부로 
  타입 정보가 전파되는 것을 막기위해 이 메서드를 구현할 수 있습니다.

예를 들어 아래 코드는
정수의 배열을 구축하는 간단한 결과 빌더를 정의합니다.
이 코드는 타입 별칭으로 `Component`와 `Expression`을 정의하여
아래 예시를 위의 메서드 목록과 쉽게 일치하도록 만듭니다.

```swift
@resultBuilder
struct ArrayBuilder {
    typealias Component = [Int]
    typealias Expression = Int
    static func buildExpression(_ element: Expression) -> Component {
        return [element]
    }
    static func buildOptional(_ component: Component?) -> Component {
        guard let component = component else { return [] }
        return component
    }
    static func buildEither(first component: Component) -> Component {
        return component
    }
    static func buildEither(second component: Component) -> Component {
        return component
    }
    static func buildArray(_ components: [Component]) -> Component {
        return Array(components.joined())
    }
    static func buildBlock(_ components: Component...) -> Component {
        return Array(components.joined())
    }
}
```

<!--
  - test: `array-result-builder`

  ```swifttest
  -> @resultBuilder
  -> struct ArrayBuilder {
         typealias Component = [Int]
         typealias Expression = Int
         static func buildExpression(_ element: Expression) -> Component {
             return [element]
         }
         static func buildOptional(_ component: Component?) -> Component {
  >>         print("Building optional...", component as Any)
             guard let component = component else { return [] }
             return component
         }
         static func buildEither(first component: Component) -> Component {
  >>         print("Building first...", component)
             return component
         }
         static func buildEither(second component: Component) -> Component {
  >>         print("Building second...", component)
             return component
         }
         static func buildArray(_ components: [Component]) -> Component {
             return Array(components.joined())
         }
         static func buildBlock(_ components: Component...) -> Component {
             return Array(components.joined())
         }
     }
  ```
-->

#### 결과 변환 (Result Transformations)

다음 구문 변환은 결과-구축 구문을 사용하는 코드에서
결과 빌더 타입의 정적 메서드를 호출하는 코드로
변환하기 위해 재귀적으로 적용됩니다:

- 결과 빌더가 `buildExpression(_:)` 메서드가 있으면,
  각 표현식은 해당 메서드에 대한 호출이 됩니다.
  이 변환은 항상 가장 먼저 일어납니다.
  예를 들어 다음의 선언은 동등합니다:

  ```swift
  @ArrayBuilder var builderNumber: [Int] { 10 }
  var manualNumber = ArrayBuilder.buildExpression(10)
  ```

  <!--
    - test: `array-result-builder`

    ```swifttest
    -> @ArrayBuilder var builderNumber: [Int] { 10 }
    -> var manualNumber = ArrayBuilder.buildExpression(10)
    >> assert(builderNumber == manualNumber)
    ```
  -->
- 할당 구문은 표현식처럼 변환되지만
  `()`로 평가되는 것으로 이해됩니다.
  할당을 특별히 처리하기 위해 `()` 타입의 인자를 가지는
  `buildExpression(_:)` 의 오버로드를 정의할 수 있습니다.
- 가용성 조건을 검사하는 분기 구문은
  `buildLimitedAvailability(_:)` 메서드가 구현되어 있으면,
  해당 메서드에 대한 호출이 됩니다.
  `buildLimitedAvailability(_:)`가 구현되어 있지 않다면,
  가용성 조건을 검사하는 분기 구문은
  다른 분기 구문과 동일한 변환을 사용합니다.
  이 변환은 `buildEither(first:)`, `buildEither(second:)`, `buildOptional(_:)`에 대한
  호출로 변환되기 전에 발생합니다.

  `buildLimitedAvailability(_:)` 메서드는 어떤 분기를 사용하는지에 따라 변경되는
  타입 정보를 지우기 위해 사용합니다.
  예를 들어
  아래의 `buildEither(first:)`와 `buildEither(second:)` 메서드는
  두 분기에 대한 타입 정보를 캡처하는 제네릭 타입을 사용합니다.

  ```swift
  protocol Drawable {
      func draw() -> String
  }
  struct Text: Drawable {
      var content: String
      init(_ content: String) { self.content = content }
      func draw() -> String { return content }
  }
  struct Line<D: Drawable>: Drawable {
      var elements: [D]
      func draw() -> String {
          return elements.map { $0.draw() }.joined(separator: "")
      }
  }
  struct DrawEither<First: Drawable, Second: Drawable>: Drawable {
      var content: Drawable
      func draw() -> String { return content.draw() }
  }

  @resultBuilder
  struct DrawingBuilder {
      static func buildBlock<D: Drawable>(_ components: D...) -> Line<D> {
          return Line(elements: components)
      }
      static func buildEither<First, Second>(first: First)
          -> DrawEither<First, Second> {
              return DrawEither(content: first)
      }
      static func buildEither<First, Second>(second: Second)
          -> DrawEither<First, Second> {
              return DrawEither(content: second)
      }
  }
  ```

  <!-- Comment block with swifttest for the code listing above is after the end of this bulleted list, due to tooling limitations. -->

  그러나 이 접근방식은 가용성 검사가 있는 코드에서 문제가 발생합니다:

  ```swift
  @available(macOS 99, *)
  struct FutureText: Drawable {
      var content: String
      init(_ content: String) { self.content = content }
      func draw() -> String { return content }
  }
  @DrawingBuilder var brokenDrawing: Drawable {
      if #available(macOS 99, *) {
          FutureText("Inside.future")  // Problem
      } else {
          Text("Inside.present")
      }
  }
  // The type of brokenDrawing is Line<DrawEither<Line<FutureText>, Line<Text>>>
  ```

  <!-- Comment block with swifttest for the code listing above is after the end of this bulleted list, due to tooling limitations. -->

  위의 코드에서
  `FutureText`는 `DrawEither` 제네릭 타입의 타입 중 하나이므로
  `brokenDrawing`의 타입의 부분으로 나타납니다.
  이 타입이 명시적으로 사용되지 않더라도
  런타임에 `FutureText`를 사용할 수 없는 경우
  프로그램이 충돌할 수 있습니다.

  이 문제를 해결하려면
  `buildLimitedAvailability(_:)` 메서드를 구현하여
  항상 사용가능한 타입을 반환하여 타입 정보를 지워야 합니다.
  예를 들어 아래의 코드는 가용성 검사로부터
  `AnyDrawable` 값을 구축합니다.

  ```swift
  struct AnyDrawable: Drawable {
      var content: Drawable
      func draw() -> String { return content.draw() }
  }
  extension DrawingBuilder {
      static func buildLimitedAvailability(_ content: some Drawable) -> AnyDrawable {
          return AnyDrawable(content: content)
      }
  }

  @DrawingBuilder var typeErasedDrawing: Drawable {
      if #available(macOS 99, *) {
          FutureText("Inside.future")
      } else {
          Text("Inside.present")
      }
  }
  // The type of typeErasedDrawing is Line<DrawEither<AnyDrawable, Line<Text>>>
  ```

  <!-- Comment block with swifttest for the code listing above is after the end of this bulleted list, due to tooling limitations. -->

- 분기 구문은 `buildEither(first:)`와 `buildEither(second:)` 메서드에 대한
  일련의 중첩된 호출이 됩니다.
  구문의 조건과 케이스는
  이진 트리의 리프 노드에 매핑되고
  구문은
  루트 노드에서 해당 리프 노드로의 경로를 따라
  `buildEither` 메서드에 대한 중첩된 호출이 됩니다.

  예를 들어 세 가지 케이스가 있는 switch 구문을 작성하면,
  컴파일러는 세 개의 리프 노드를 가진 이진 트리를 사용합니다.
  마찬가지로
  루트 노드에서 두 번째 케이스까지의 경로는
  "second child"이고 그 다음이 "first child"이므로
  해당 케이스는 `buildEither(first: buildEither(second: ... ))`와 같은
  중첩된 호출이 됩니다.
  다음의 선언은 동일합니다:

  ```swift
  let someNumber = 19
  @ArrayBuilder var builderConditional: [Int] {
      if someNumber < 12 {
          31
      } else if someNumber == 19 {
          32
      } else {
          33
      }
  }

  var manualConditional: [Int]
  if someNumber < 12 {
      let partialResult = ArrayBuilder.buildExpression(31)
      let outerPartialResult = ArrayBuilder.buildEither(first: partialResult)
      manualConditional = ArrayBuilder.buildEither(first: outerPartialResult)
  } else if someNumber == 19 {
      let partialResult = ArrayBuilder.buildExpression(32)
      let outerPartialResult = ArrayBuilder.buildEither(second: partialResult)
      manualConditional = ArrayBuilder.buildEither(first: outerPartialResult)
  } else {
      let partialResult = ArrayBuilder.buildExpression(33)
      manualConditional = ArrayBuilder.buildEither(second: partialResult)
  }
  ```

  <!--
    - test: `array-result-builder`

    ```swifttest
    -> let someNumber = 19
    -> @ArrayBuilder var builderConditional: [Int] {
           if someNumber < 12 {
               31
           } else if someNumber == 19 {
               32
           } else {
               33
           }
       }
    << Building second... [32]
    << Building first... [32]

    -> var manualConditional: [Int]
    -> if someNumber < 12 {
           let partialResult = ArrayBuilder.buildExpression(31)
           let outerPartialResult = ArrayBuilder.buildEither(first: partialResult)
           manualConditional = ArrayBuilder.buildEither(first: outerPartialResult)
       } else if someNumber == 19 {
           let partialResult = ArrayBuilder.buildExpression(32)
           let outerPartialResult = ArrayBuilder.buildEither(second: partialResult)
           manualConditional = ArrayBuilder.buildEither(first: outerPartialResult)
       } else {
           let partialResult = ArrayBuilder.buildExpression(33)
           manualConditional = ArrayBuilder.buildEither(second: partialResult)
       }
    >> assert(builderConditional == manualConditional)
    << Building second... [32]
    << Building first... [32]
    ```
  -->
- `else` 절이 없는 `if` 구문과 같이
  값을 생성하지 않을 수도 있는 분기 구문은
  `buildOptional(_:)`에 대한 호출이 됩니다.
  `if` 구문의 조건이 충족되면,
  코드 블록이 변환되고 인자로 전달됩니다;
  그렇지 않으면 `buildOptional(_:)`이 `nil`을 인자로 받아 호출됩니다.
  예를 들어 다음의 선언은 동일합니다:

  ```swift
  @ArrayBuilder var builderOptional: [Int] {
      if (someNumber % 2) == 1 { 20 }
  }

  var partialResult: [Int]? = nil
  if (someNumber % 2) == 1 {
      partialResult = ArrayBuilder.buildExpression(20)
  }
  var manualOptional = ArrayBuilder.buildOptional(partialResult)
  ```

  <!--
    - test: `array-result-builder`

    ```swifttest
    -> @ArrayBuilder var builderOptional: [Int] {
           if (someNumber % 2) == 1 { 20 }
       }
    << Building optional... Optional([20])

    -> var partialResult: [Int]? = nil
    -> if (someNumber % 2) == 1 {
           partialResult = ArrayBuilder.buildExpression(20)
       }
    -> var manualOptional = ArrayBuilder.buildOptional(partialResult)
    << Building optional... Optional([20])
    >> assert(builderOptional == manualOptional)
    ```
  -->
- 결과 빌더가
  `buildPartialBlock(first:)`와
  `buildPartialBlock(accumulated:next:)` 메서드를 구현하는 경우,
  코드 블록이나 `do` 구문은 해당 메서드를 호출합니다.
  블록의 첫 번째 구문은
  `buildPartialBlock(first:)` 메서드의 인자로 변환되고,
  나머지 구문은
  `buildPartialBlock(accumulated:next:)` 메서드에 대한
  중첩된 호출이 됩니다.
  예를 들어 다음 선언은 동일한 구문입니다:

  ```swift
  struct DrawBoth<First: Drawable, Second: Drawable>: Drawable {
      var first: First
      var second: Second
      func draw() -> String { return first.draw() + second.draw() }
  }

  @resultBuilder
  struct DrawingPartialBlockBuilder {
      static func buildPartialBlock<D: Drawable>(first: D) -> D {
          return first
      }
      static func buildPartialBlock<Accumulated: Drawable, Next: Drawable>(
          accumulated: Accumulated, next: Next
      ) -> DrawBoth<Accumulated, Next> {
          return DrawBoth(first: accumulated, second: next)
      }
  }

  @DrawingPartialBlockBuilder var builderBlock: some Drawable {
      Text("First")
      Line(elements: [Text("Second"), Text("Third")])
      Text("Last")
  }

  let partialResult1 = DrawingPartialBlockBuilder.buildPartialBlock(first: Text("first"))
  let partialResult2 = DrawingPartialBlockBuilder.buildPartialBlock(
      accumulated: partialResult1,
      next: Line(elements: [Text("Second"), Text("Third")])
  )
  let manualResult = DrawingPartialBlockBuilder.buildPartialBlock(
      accumulated: partialResult2,
      next: Text("Last")
  )
  ```

  <!--
    - test: `drawing-partial-block-builder`

    ```swifttest
    -> @resultBuilder
    -> struct DrawingPartialBlockBuilder {
           static func buildPartialBlock<D: Drawable>(first: D) -> D {
               return first
           }
           static func buildPartialBlock<Accumulated: Drawable, Next: Drawable>(
               accumulated: Accumulated, next: Next
           ) -> DrawBoth<Accumulated, Next> {
               return DrawBoth(first: accumulated, second: next)
           }
       }
    -> @DrawingPartialBlockBuilder var builderBlock: some Drawable {
          Text("First")
          Line(elements: [Text("Second"), Text("Third")])
          Text("Last")
       }

    -> let partialResult1 = DrawingPartialBlockBuilder.buildPartialBlock(first: Text("first"))
    -> let partialResult2 = DrawingPartialBlockBuilder.buildPartialBlock(
          accumulated: partialResult1,
          next: Line(elements: [Text("Second"), Text("Third")])
       )
       let manualResult = DrawingPartialBlockBuilder.buildPartialBlock(
           accumulated: partialResult2,
           next: Text("Last")
       )
    >> assert(type(of: builderBlock) == type(of: manualResult))
    ```
  -->
- 그렇지 않으면 코드 블록이나 `do` 구문은
  `buildBlock(_:)` 메서드에 대한 호출이 됩니다.
  블록 내부의 각 구문은
  한 번에 하나씩 변환되고
  `buildBlock(_:)` 메서드에 대한 인자가 됩니다.
  예를 들어 다음의 선언은 동일합니다:

  ```swift
  @ArrayBuilder var builderBlock: [Int] {
      100
      200
      300
  }

  var manualBlock = ArrayBuilder.buildBlock(
      ArrayBuilder.buildExpression(100),
      ArrayBuilder.buildExpression(200),
      ArrayBuilder.buildExpression(300)
  )
  ```

  <!--
    - test: `array-result-builder`

    ```swifttest
    -> @ArrayBuilder var builderBlock: [Int] {
           100
           200
           300
       }

    -> var manualBlock = ArrayBuilder.buildBlock(
           ArrayBuilder.buildExpression(100),
           ArrayBuilder.buildExpression(200),
           ArrayBuilder.buildExpression(300)
       )
    >> assert(builderBlock == manualBlock)
    ```
  -->
- `for` 루프는 임시 변수, `for` 루프,
  `buildArray(_:)` 메서드에 대한 호출이 됩니다.
  새로운 `for` 루프는 시퀀스를 반복하고
  각 부분 결과를 해당 배열에 추가합니다.
  임시 배열은 `buildArray(_:)` 호출에 인자로 전달됩니다.
  예를 들어 다음의 선언은 동일합니다:

  ```swift
  @ArrayBuilder var builderArray: [Int] {
      for i in 5...7 {
          100 + i
      }
  }

  var temporary: [[Int]] = []
  for i in 5...7 {
      let partialResult = ArrayBuilder.buildExpression(100 + i)
      temporary.append(partialResult)
  }
  let manualArray = ArrayBuilder.buildArray(temporary)
  ```

  <!--
    - test: `array-result-builder`

    ```swifttest
    -> @ArrayBuilder var builderArray: [Int] {
           for i in 5...7 {
               100 + i
           }
       }

    -> var temporary: [[Int]] = []
    -> for i in 5...7 {
           let partialResult = ArrayBuilder.buildExpression(100 + i)
           temporary.append(partialResult)
       }
    -> let manualArray = ArrayBuilder.buildArray(temporary)
    >> assert(builderArray == manualArray)
    ```
  -->
- 결과 빌더가 `buildFinalResult(_:)` 메서드를 가지고 있으면,
  최종 결과는 해당 메서드에 대한 호출이 됩니다.
  이 변환은 항상 마지막에 일어납니다.

<!--
  - test: `result-builder-limited-availability-broken, result-builder-limited-availability-ok`, `drawing-partial-result-builder`

  ```swifttest
  -> protocol Drawable {
         func draw() -> String
     }
  -> struct Text: Drawable {
         var content: String
         init(_ content: String) { self.content = content }
         func draw() -> String { return content }
     }
  -> struct Line<D: Drawable>: Drawable {
         var elements: [D]
         func draw() -> String {
             return elements.map { $0.draw() }.joined(separator: "")
         }
     }
  -> struct DrawEither<First: Drawable, Second: Drawable>: Drawable {
         var content: Drawable
         func draw() -> String { return content.draw() }
     }

  -> @resultBuilder
     struct DrawingBuilder {
         static func buildBlock<D: Drawable>(_ components: D...) -> Line<D> {
             return Line(elements: components)
         }
         static func buildEither<First, Second>(first: First)
                 -> DrawEither<First, Second> {
             return DrawEither(content: first)
         }
         static func buildEither<First, Second>(second: Second)
                 -> DrawEither<First, Second> {
             return DrawEither(content: second)
         }
     }
  ```
-->

<!--
  - test: `result-builder-limited-availability-broken`

  ```swifttest
  -> @available(macOS 99, *)
  -> struct FutureText: Drawable {
         var content: String
         init(_ content: String) { self.content = content }
         func draw() -> String { return content }
     }
  -> @DrawingBuilder var brokenDrawing: Drawable {
         if #available(macOS 99, *) {
             FutureText("Inside.future")  // Problem
         } else {
             Text("Inside.present")
         }
     }
  /> The type of brokenDrawing is \(type(of: brokenDrawing))
  </ The type of brokenDrawing is Line<DrawEither<Line<FutureText>, Line<Text>>>
  !$ warning: result builder 'DrawingBuilder' does not implement 'buildLimitedAvailability'; this code may crash on earlier versions of the OS
  !! if #available(macOS 99, *) {
  !! ^
  !$ note: add 'buildLimitedAvailability(_:)' to the result builder 'DrawingBuilder' to erase type information for less-available types
  !! struct DrawingBuilder {
  !! ^
  ```
-->

<!--
  - test: `result-builder-limited-availability-ok`

  ```swifttest
  >> @available(macOS 99, *)
  >> struct FutureText: Drawable {
  >>     var content: String
  >>     init(_ content: String) { self.content = content }
  >>     func draw() -> String { return content }
  >> }
  >> @DrawingBuilder var x: Drawable {
  >>     if #available(macOS 99, *) {
  >>         FutureText("Inside.future")
  >>     } else {
  >>         Text("Inside.present")
  >>     }
  >> }
  -> struct AnyDrawable: Drawable {
         var content: Drawable
         func draw() -> String { return content.draw() }
     }
  -> extension DrawingBuilder {
         static func buildLimitedAvailability(_ content: some Drawable) -> AnyDrawable {
             return AnyDrawable(content: content)
         }
     }

  -> @DrawingBuilder var typeErasedDrawing: Drawable {
         if #available(macOS 99, *) {
             FutureText("Inside.future")
         } else {
             Text("Inside.present")
         }
     }
  /> The type of typeErasedDrawing is \(type(of: typeErasedDrawing))
  </ The type of typeErasedDrawing is Line<DrawEither<AnyDrawable, Line<Text>>>
  ```
-->

변환 동작은 임시 변수로 설명되지만
결과 빌더를 사용하는 것은 나머지 코드에서 볼 수 있는
새로운 선언을 생성하지 않습니다.

결과 빌더 변환하는 코드에서
`break`, `continue`, `defer`, `guard`, `return` 구문,
`while` 구문,
`do`-`catch` 구문을
사용할 수 없습니다.

변환 과정은 코드에서 선언을 변경하지 않으므로
임시 상수와 변수를 사용하여
표현식을 부분적으로 구축할 수 있습니다.
또한 `throw` 구문,
컴파일 시점 진단 구문,
`return` 구문이 포함된 클로저도
변경하지 않습니다.

가능하면 변환이 병합됩니다.
예를 들어 표현식 `4 + 5 * 6`은
해당 함수를 여러 번 호출하는 대신 `buildExpression(4 + 5 * 6)`이 됩니다.
마찬가지로 중첩된 분기 구문은
`buildEither` 메서드에 대한 단일 이진 호출 트리가 됩니다.

<!--
  - test: `result-builder-transform-complex-expression`

  ```swifttest
  >> @resultBuilder
  >> struct ArrayBuilder {
  >>     static func buildExpression(_ element: Int) -> [Int] {
  >>         print("Building", element)
  >>         return [element]
  >>     }
  >>     static func buildBlock(_ components: [Int]...) -> [Int] {
  >>         return Array(components.joined())
  >>     }
  >> }
  >> @ArrayBuilder var x: [Int] {
  >>     10+12*3
  >> }
  << Building 46
  >> print(x)
  << [46]
  ```
-->

#### 커스텀 결과-빌더 속성 (Custom Result-Builder Attributes)

결과 빌더 타입을 생성하면 동일한 이름의 커스텀 속성을 생성합니다.
이 속성은 다음 위치에 적용할 수 있습니다:

- 함수 선언에 적용하면,
  결과 빌더는 함수의 본문을 구축합니다.
- getter를 포함하는 변수나 서브스크립트 선언에 적용하면,
  결과 빌더는 getter의 본문을 구축합니다.
- 함수 선언의 매개변수에 적용하면,
  결과 빌더는 해당 인자로 전달되는
  클로저의 본문을 구축합니다.

결과 빌더 속성을 적용하는 것은 ABI 호환성에 영향을 주지 않습니다.
매개변수에 결과 빌더 속성을 적용하는 것은
해당 속성을 함수의 인터페이스의 부분으로 만들고
이것은 소스 호환성에 영향을 줄 수 있습니다.

### requires_stored_property_inits

이 속성을 클래스 선언에 적용하여
클래스 내의 모든 저장 프로퍼티가
정의의 일부로 기본값을 제공하도록 요구합니다.
이 속성은 `NSManagedObject`를 상속하는
모든 클래스에 대해 추론됩니다.

<!--
  - test: `requires_stored_property_inits-requires-default-values`

  ```swifttest
  >> @requires_stored_property_inits class DefaultValueProvided {
         var value: Int = -1
         init() { self.value = 0 }
     }
  -> @requires_stored_property_inits class NoDefaultValue {
         var value: Int
         init() { self.value = 0 }
     }
  !$ error: stored property 'value' requires an initial value
  !! var value: Int
  !! ^
  !$ note: class 'NoDefaultValue' requires all stored properties to have initial values
  !! @requires_stored_property_inits class NoDefaultValue {
  !! ^
  ```
-->

### testable

이 속성을 `import` 선언에 적용하여
모듈의 코드를 테스트하기 쉽게
접근 제어를 변경하여 해당 모듈을 가져옵니다.
임포트한 모듈에서
`internal` 접근-수준 수정자로 표시된 엔티티는
`public` 접근-수준 수정자로 선언된 것처럼 가져옵니다.
`internal`이나 `public` 접근-수준 수정자로 표시된
클래스와 클래스 멤버는
`open` 접근-수준 수정자로 선언된 것처럼 가져옵니다.
임포트한 모듈은 테스트가 활성화된 상태로 컴파일되어야 합니다.

### UIApplicationMain

> Deprecated:
> 이 속성은 사용 중단되었습니다;
> 대신 [main](#main) 속성을 사용합니다.
> Swift 6에서
> 이 속성을 사용하면 오류가 발생합니다.

이 속성을 클래스에 적용하여
앱 델리게이트임을 나타냅니다.
이 속성을 사용하는 것은
`UIApplicationMain` 함수를 호출하는 것과
이 클래스의 이름을 델리게이트 클래스의 이름으로 전달하는 것과 동일합니다.

이 속성을 사용하지 않으면,
[`UIApplicationMain(_:_:_:_:)`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain) 함수를 호출하는
최상위 수준의 코드가 포함된 `main.swift` 파일을 제공해야 합니다.
예를 들어
주 클래스로
`UIApplication`의 커스텀 하위 클래스를 사용하면
이 속성을 사용하는 것 대신
`UIApplicationMain(_:_:_:_:)` 함수를 호출해야 합니다.

실행 파일을 만들기 위해 컴파일 한 Swift 코드는
[최상위-수준 코드 (Top-Level Code)](./declarations.md#최상위-수준-코드-top-level-code)에서 설명한대로
하나의 최상위-수준 시작점을 포함해야 합니다.

### unchecked

이 속성을 타입 선언의 채택된 프로토콜 목록의 일부로
프로토콜 타입에 적용하여
해당 프로토콜의 요구사항 적용을 끕니다.

지원되는 유일한 프로토콜은 [Sendable](https://developer.apple.com/documentation/swift/sendable)입니다.

### usableFromInline

이 속성을
함수, 메서드, 연산 프로퍼티, 서브스크립트,
이니셜라이저, 디이니셜라이저 선언에 적용하여
해당 선언과 동일한 모듈에 정의된
인라인 가능 코드에서 해당 기호를 사용할 수 있도록 허용합니다.
선언은 `internal` 접근 수준 수정자를 가지고 있어야 합니다.
`usableFromInline`으로 표시된 구조체나 클래스는
프로퍼티에 대해 public이나 `usableFromInline`인 타입만 사용할 수 있습니다.
`usableFromInline`으로 표시된 열거형은
케이스의 원시값과 연관값에 대해
public이나 `usableFromInline`인 타입만 사용할 수 있습니다.

`public` 접근 수준 수정자와 같이
이 속성은
모듈의 공개 인터페이스의 부분으로 선언을 노출합니다.
`public`과 다르게
컴파일러는 `usableFromInline`으로 표시된 선언이
선언의 기호를 내보내지더라도
모듈 외부의 코드에서 이름으로 참조되는 것을 허용하지 않습니다.
그러나 모듈 외부의 코드는
런타임 동작을 사용하여 선언의 기호와 상호작용할 수 있습니다.

`inlinable` 속성으로 표시된 선언은
암시적으로 인라인 가능 코드에서 사용 가능합니다.
`inlinable`이나 `usableFromInline` 중 하나를
`internal` 선언에 적용할 수 있지만
두 속성 모두 적용하는 것은 오류가 발생합니다.

<!--
  - test: `usableFromInline-and-inlinable-is-redundant`

  ```swifttest
  >> @usableFromInline @inlinable internal func f() { }
  !$ warning: '@usableFromInline' attribute has no effect on '@inlinable' global function 'f()'
  !! @usableFromInline @inlinable internal func f() { }
  !! ^~~~~~~~~~~~~~~~~~
  ```
-->

### warn_unqualified_access

이 속성을
최상위 함수, 인스턴스 메서드, 클래스나 정정 메서드에 적용하여
모듈 이름, 타입 이름, 인스턴스 변수나 상수와 같은
선행 한정자 없이
해당 함수나 메서드 사용될 때 경고를 발생시킵니다.
이 속성은 동일한 범위에서 접근할 수 있는 동일한 이름을 가지는
함수 간의 모호성을 줄이는데 도움을 주기 위해 사용됩니다.

예를 들어
Swift 표준 라이브러리는 최상위
[`min(_:_:)`](https://developer.apple.com/documentation/swift/1538339-min/)
함수와
비교 가능한 요소가 있는 시퀀스에 대한
[`min()`](https://developer.apple.com/documentation/swift/sequence/1641174-min) 메서드 모두 포함합니다.
시퀀스 메서드는 `Sequence` 확장 내에서 어느 하나를 사용하려할 때
혼동을 줄이기 위해
`warn_unqualified_access` 속성으로 선언됩니다.

### 인터페이스 빌더에서 사용되는 선언 속성 (Declaration Attributes Used by Interface Builder)

인터페이스 빌더(Interface Builder) 속성은
Xcode와 동기화하기 위해 인터페이스 빌더에서 사용하는 선언 속성입니다.
Swift는 다음의 인터페이스 빌더 속성을 제공합니다:
`IBAction`, `IBSegueAction`, `IBOutlet`,
`IBDesignable`, `IBInspectable`.
이 속성은 개념적으로
Objective-C와 동일합니다.

<!--
  TODO: Need to link to the relevant discussion of these attributes in Objc.
-->

`IBOutlet`과 `IBInspectable` 속성은
클래스의 프로퍼티 선언에 적용합니다.
`IBAction`과 `IBSegueAction` 속성은
클래스의 메서드 선언에 적용하고
`IBDesignable` 속성은 클래스 선언에 적용합니다.

`IBAction`, `IBSegueAction`, `IBOutlet`,
`IBDesignable`, `IBInspectable` 속성을 적용하는 것은
`objc` 속성도 암시적으로 포함합니다.

## 타입 속성 (Type Attributes)

타입 속성은 타입에만 적용할 수 있습니다.

### autoclosure

이 속성을 적용하여 인자를 받지 않는 클로저로 표현식을 자동으로 감싸서
해당 표현식의 평가를 지연시킵니다.
이 속성은 함수나 메서드 선언의 매개변수 타입에 적용하여
인자를 받지 않고
표현식의 타입과 같은 값을 반환하는 함수 타입의 매개변수에 사용합니다. 
`autoclosure` 속성을 어떻게 사용하는지에 대한 예시는
[자동 클로저 (Autoclosures)](../language-guide-1/closures.md#자동-클로저-autoclosures)와 [함수 타입 (Function Type)](./types.md#함수-타입-function-type)을 참고바랍니다.

### convention

이 속성을 함수의 타입에 적용하여
호출 규약(calling conventions)을 나타냅니다.

`convention` 속성은
항상 다음의 인자 중 하나와 함께 사용됩니다:

- `swift` 인자는 Swift 함수 참조를 나타냅니다.
  이것은 Swift에서 함수 값에 대한 표준 호출 규약입니다.
- `block` 인자는 Objective-C 호환 블록 참조를 나타냅니다.
  함수 값은 블록 객체에 대한 참조로 표현되며
  이 객체는 자체 호출 함수를 객체 내에 내장하는
  `id`-호환 Objective-C 객체입니다.
  호출 함수는 C 호출 규약을 사용합니다.
- `c` 인자는 C 함수 참조를 나타냅니다.
  함수 값은 어떤 컨텍스트를 전달하지 않으며, C 호출 규약을 사용합니다.

<!--
  Note: @convention(thin) is private, even though it doesn't have an underscore
  https://forums.swift.org/t/12087/6
-->

몇 가지 예외를 제외하고
어떤 호출 규약을 가진 함수라도
다른 호출 규약이 필요한 함수에 사용할 수 있습니다.
제네릭이 아닌 전역 함수,
지역 변수를 캡처하지 않는 지역 함수,
지역 변수를 캡처하지 않는 클로저는
C 호출 규약으로 변환될 수 있습니다.
다른 Swift 함수는 C 호출 규약으로 변환될 수 없습니다.
Objective-C 블록 호출 규약을 가지는 함수는
C 호출 규약으로 변환될 수 없습니다.

### escaping

이 속성을 함수나 메서드 선언의 매개변수 타입에 적용하여
해당 매개변수의 값이 나중에 실행하기 위해 저장될 수 있음을 나타냅니다.
이는 값이 호출 수명보다 오래 지속될 수 있음을 의미합니다.
`escaping` 타입 속성을 가지는 함수 타입 매개변수는
프로퍼티나 메서드에 대해 `self.`를 명시적으로 사용해야 합니다.
`escaping` 속성을 어떻게 사용하는지에 대한 예시는
[탈출 클로저 (Escaping Closures)](../language-guide-1/closures.md#탈출-클로저-escaping-closures)를 참고바랍니다.

### Sendable

이 속성을 함수 타입에 적용하여
함수나 클로저가 Sendable을 나타냅니다.
함수 타입에 이 속성을 적용하는 것은
비함수(non-function) 타입이 [Sendable](https://developer.apple.com/documentation/swift/sendable) 프로토콜을
준수하는 것과 같은 의미를 가집니다.

이 속성은 함수나 클로저가
Sendable 값을 기대하는
컨텍스트에서 사용되고
함수나 클로저가 Sendable 요구사항을 충족하는 경우 추론됩니다.

Sendable 함수 타입은
Sendable 하지 않은 함수 타입의 하위 타입입니다.

## 스위치 케이스 속성 (Switch Case Attributes)

스위치 케이스 속성은 스위치 케이스에만 적용할 수 있습니다.

### unknown

이 속성을 스위치 케이스에 적용하여
코드가 컴파일될 때
알려진 열거형의 어떤 케이스와도
일치하지 않는 것으로 예상됨을 나타냅니다.
`unknown` 속성을 어떻게 사용하는지에 대한 예시는
[향후 열거형 케이스 전환 (Switching Over Future Enumeration Cases)](./statements.md#향후-열거형-케이스-전환-switching-over-future-enumeration-cases)을 참고바랍니다.

> Grammar of an attribute:
>
> *attribute* → **`@`** *attribute-name* *attribute-argument-clause*_?_ \
> *attribute-name* → *identifier* \
> *attribute-argument-clause* → **`(`** *balanced-tokens*_?_ **`)`** \
> *attributes* → *attribute* *attributes*_?_
>
> *balanced-tokens* → *balanced-token* *balanced-tokens*_?_ \
> *balanced-token* → **`(`** *balanced-tokens*_?_ **`)`** \
> *balanced-token* → **`[`** *balanced-tokens*_?_ **`]`** \
> *balanced-token* → **`{`** *balanced-tokens*_?_ **`}`** \
> *balanced-token* → Any identifier, keyword, literal, or operator \
> *balanced-token* → Any punctuation except  **`(`**,  **`)`**,  **`[`**,  **`]`**,  **`{`**, or  **`}`**

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
