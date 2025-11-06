{% hint style="info" %}
📢 **공지**  
이 문서는 더 이상 업데이트되지 않습니다.  
아래 새로운 URL로 업데이트 되었으니, 새로운 URL로 접속해 주세요.  
[Swift Book Korean](https://bbiguduk.github.io/swift-book-korean/documentation/tsplk/)
{% endhint %}

# 클로저 (Closures)

함수 이름 없이 함께 실행되어야 하는 코드를 그룹화합니다.

*클로저(Closures)*는 코드에서 전달되고
사용할 수 있는 독립적인 기능 블록입니다.
Swift의 클로저는
다른 프로그래밍 언어에서
클로저, 익명 함수, 람다, 블록과 유사합니다.

클로저는 정의된 컨텍스트에서
상수나 변수에 대한 참조를 캡처하고 저장할 수 있습니다.
이러한 상수와 변수를 *클로저가 포착한다(closing over)*라고 합니다.
Swift는 캡처에 대한 메모리 관리를 자동으로 처리합니다.

> Note: 캡처에 대한 개념이 생소하더라도 걱정하지 마세요.
> 아래 [캡처값 (Capturing Values)](#캡처값-capturing-values)에서 자세히 다루도록 하겠습니다.

[함수 (Functions)](./functions.md)에서 소개한 전역 함수와 중첩 함수는
클로저의 특별한 형태입니다.
클로저는 세 가지 형태 중 하나를 취합니다:

- 전역 함수는 이름을 가지고
  어떠한 값도 캡처하지 않는 클로저입니다.
- 중첩 함수는 이름을 가지고
  외부 함수로부터 값을 캡처할 수 있는 클로저입니다.
- 클로저 표현식은 주변 컨텍스트에서 값을 캡처할 수 있는
  간결한 구문으로 작성된 이름이 없는 클로저입니다.

Swift의 클로저 표현식은 명확한 스타일을 가지고 있으며,
일반적인 상황에서 간결하고 복잡하지 않은 문법을 유도하는 여러 최적화를 제공합니다.
이러한 최적화에는 다음이 포함됩니다:

- 컨텍스트에서 매개변수와 반환값 타입 유추
- 단일 표현식 클로저의 암시적 반환
- 축약된 인자 이름
- 후행 클로저 구문

## 클로저 표현식 (Closure Expressions)

[중첩 함수 (Nested Functions)](./functions.md#중첩-함수-nested-functions)에서 소개된 중첩 함수는
더 큰 함수의 부분으로
독립적인 코드 블록을 이름 붙여 정의할 수 있는 편리한 수단입니다.
그러나 전체 선언과 이름 없이
함수와 유사한 구조의 짧은 형태로 작성하는 것이 때때로 유용합니다.
특히 함수나 메서드가 다른 함수를
하나 이상의 인자로 받을 때 그렇습니다.

*클로저 표현식(Closure expressions)*은 간단하고 집중적인 구문으로 인라인 클로저를 작성하는 방법입니다.
클로저 표현식은 명확성이나 의도를 잃지 않고 더 짧은 형태로 클로저를 작성 하기위한
여러 구문 최적화를 제공합니다.
아래의 클로저 표현식 예시는
`sorted(by:)` 메서드의 동일한 기능을 여러 단계에 걸쳐
점차 간결하게 표현함으로써 이러한 최적화를 보여줍니다.

### 정렬 메서드 (The Sorted Method)

Swift의 표준 라이브러리는
주어진 정렬 클로저의 결과에 따라,
알려진 타입의 값들로 구성된 배열을 정렬합니다.
정렬이 완료되면,
`sorted(by:)` 메서드는 원본 배열과 같은 타입과 같은 크기를 가진 새로운 배열을 반환하며,
그 요소들은 올바른 정렬 순서로 배치되어 있습니다.
기존 배열은 `sorted(by:)` 메서드로 수정되지 않습니다.

아래 예시의 클로저 표현식은 알파벳 역순으로 `String` 값의 배열을 정렬하기 위해
`sorted(by:)` 메서드를 사용합니다.
다음은 정렬할 초기 배열입니다:

```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
```

<!--
  - test: `closureSyntax`

  ```swifttest
  -> let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
  ```
-->

`sorted(by:)` 메서드는 배열의 요소 타입과 동일한
두 개의 인자를 받아,
첫 번째 값이 두 번째 값보다 앞에 와야 하는지를 판단하여
`Bool` 값을 반환하는 클로저를 인자로 받습니다.
정렬 클로저는 첫 번째 값이 두 번째 값보다
먼저 와야 하면 `true`를 반환하고,
그렇지 않으면 `false`를 반환해야 합니다.

이 예시는 `String` 값의 배열을 정렬하고,
정렬 클로저는 `(String, String) -> Bool` 타입의 함수여야 합니다.

정렬 클로저를 제공하는 한가지 방법은 올바른 타입의 일반 함수를 작성하고
`sorted(by:)` 메서드의 인자로 전달하는 것입니다:

```swift
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var reversedNames = names.sorted(by: backward)
// reversedNames is equal to ["Ewa", "Daniella", "Chris", "Barry", "Alex"]
```

<!--
  - test: `closureSyntax`

  ```swifttest
  -> func backward(_ s1: String, _ s2: String) -> Bool {
        return s1 > s2
     }
  -> var reversedNames = names.sorted(by: backward)
  /> reversedNames is equal to \(reversedNames)
  </ reversedNames is equal to ["Ewa", "Daniella", "Chris", "Barry", "Alex"]
  ```
-->

첫 번째 문자열(`s1`)이 두 번째 문자열(`s2`)보다 크다면
`backward(_:_:)` 함수는 `true`를 반환하고,
이것은 정렬된 배열에 `s2` 전에 `s1`이 나타납니다.
문자열의 문자가
"더 크다"는 "알파벳 순으로 더 뒤에 나타난다"는 의미입니다.
이것은 문자 `B`는 문자 `A`보다 "더 크다"이고,
문자열 `"Tom"`은 문자열 `"Tim"`보다 더 큽니다.
알파벳 역순으로 정렬될 때
`"Barry"`는 `"Alex"`보다 앞에 위치합니다.

그러나 이 함수는 사실상 단일 표현식 함수(`a > b`)이기 때문에
이렇게 길게 작성하는 것은 다소 번거롭습니다.
이 예시에서는 클로저 표현식 구문을 사용하여
정렬 클로저를 인라인으로 작성하는 것이 좋습니다.

### 클로저 표현식 문법 (Closure Expression Syntax)

클로저 표현식 문법은 아래와 같은 일반적인 형태를 가지고 있습니다:

```swift
{ (<#parameters#>) -> <#return type#> in
   <#statements#>
}
```

클로저 표현식 문법에서 *매개변수*는
in-out 매개변수로 정의할 수 있지만,
기본값을 가질 수는 없습니다.
가변 매개변수의 이름을 지정하면 가변 매개변수를 사용할 수 있습니다.
튜플은 매개변수 타입이나 반환 타입으로 사용될 수도 있습니다.

아래 예시는 위에서
`backward(_:_:)` 함수의 클로저 표현입니다:

```swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

<!--
  - test: `closureSyntax`

  ```swifttest
  -> reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
        return s1 > s2
     })
  >> assert(reversedNames == ["Ewa", "Daniella", "Chris", "Barry", "Alex"])
  ```
-->

이 인라인 클로저에서 매개변수와 반환 타입의 선언은
`backward(_:_:)` 함수에서 선언한 것과 동일합니다.
두 경우 모두 `(s1: String, s2: String) -> Bool`로 작성합니다.
그러나 인라인 클로저 표현식에서는
매개변수와 반환 타입은 중괄호 바깥이 아닌
*안에* 작성합니다.

클로저 본문의 시작은 `in` 키워드로 시작합니다.
이 키워드는
클로저의 매개변수와 반환 타입 정의가 끝남을 나타내며,
클로저의 본문이 시작함을 나타냅니다.

클로저의 본문이 너무 짧으면,
한 줄로 작성할 수 있습니다:

```swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```

<!--
  - test: `closureSyntax`

  ```swifttest
  -> reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 } )
  >> assert(reversedNames == ["Ewa", "Daniella", "Chris", "Barry", "Alex"])
  ```
-->

이것은 `sorted(by:)` 메서드에 대한 전체 호출 구조는 동일하게 유지된다는 것을 보여줍니다.
소괄호는 여전히 메서드의 전체 인자를 둘러싸고 있습니다.
그러나 그 인자는 인라인 클로저로 작성되었습니다.

### 컨텍스트로부터 타입 추론 (Inferring Type From Context)

정렬 클로저가 메서드의 인자로 전달되기 때문에,
Swift는 매개변수 타입과
반환되는 값의 타입을 유추할 수 있습니다.
`sorted(by:)` 메서드는 문자열 배열에서 호출되므로,
인자는 `(String, String) -> Bool` 타입의 함수여야 합니다.
이는 `(String, String)`과 `Bool` 타입을
클로저 표현식 정의에 일부러 작성할 필요가 없음을 의미합니다.
모든 타입은 유추할 수 있기 때문에,
반환 화살표(`->`)와 매개변수의 이름을 둘러싼 소괄호를
생략할 수 있습니다:

```swift
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
```

<!--
  - test: `closureSyntax`

  ```swifttest
  -> reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
  >> assert(reversedNames == ["Ewa", "Daniella", "Chris", "Barry", "Alex"])
  ```
-->

클로저를 함수나 메서드의 인라인 클로저 표현식으로 전달할 때,
항상 매개변수 타입과 반환 타입을 유추할 수 있습니다.
따라서 클로저가 함수나 메서드 인자로 사용될 때,
완전한 형태로 인라인 클로저를 작성할 필요가 없습니다.

그럼에도 불구하고 원하는 경우 타입을 명시적으로 작성할 수 있으며,
코드의 의미가 모호해지는 것을 방지할 수 있다면 그렇게 하는 것이 좋습니다.
`sorted(by:)` 메서드의 경우,
정렬이 발생한다는 사실에서 클로저의 목적이 명확하며,
문자열 배열의 정렬을 지원하기 때문에
코드를 읽는 사람이 클로저가 `String` 값으로
작동할 가능성이 있다고 자연스럽게 추측할 수 있습니다.

### 단일 표현식 클로저의 암시적 반환 (Implicit Returns from Single-Expression Closures)

단일 표현식 클로저(Single-expression closures)는
이전 예시에서 `return` 키워드를 생략하여
단일 표현식의 결과를 암시적으로 반환할 수 있습니다:

```swift
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
```

<!--
  - test: `closureSyntax`

  ```swifttest
  -> reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
  >> assert(reversedNames == ["Ewa", "Daniella", "Chris", "Barry", "Alex"])
  ```
-->

여기서 `sorted(by:)` 메서드의 인자는 함수 타입이므로,
클로저가 `Bool` 값을 반환해야 한다는 것이 명확합니다.
클로저 본문이 단일 표현식(`s1 > s2`)으로 구성되어 있고,
이 표현식이 `Bool` 값을 반환하기 때문에
모호함이 없으므로 `return` 키워드를 생략할 수 있습니다.

### 축약 인자 이름 (Shorthand Argument Names)

Swift는 인라인 클로저에
`$0`, `$1`, `$2` 등 클로저의 인자값으로 참조하는데 사용할 수 있는
자동으로 축약 인자 이름(shorthand argument names)을 제공합니다.

클로저 표현식에 이런 축약 인자 이름을 사용한다면,
선언에 클로저의 인자 목록을 생략할 수 있습니다.
축약 인자 이름의 타입은
함수 타입에서 추론되고,
사용한 축약 인자 중 가장 큰 번호가
클로저가 받는 인자의 개수를 결정합니다.
클로저 표현식이 오직 본문으로만 이루어져 있다면,
`in` 키워드도 생략할 수 있습니다:

```swift
reversedNames = names.sorted(by: { $0 > $1 } )
```

<!--
  - test: `closureSyntax`

  ```swifttest
  -> reversedNames = names.sorted(by: { $0 > $1 } )
  >> assert(reversedNames == ["Ewa", "Daniella", "Chris", "Barry", "Alex"])
  ```
-->

여기서 `$0`와 `$1`은 클로저의 첫 번째와 두 번째 `String` 인자를 나타냅니다.
`$1`이 축약 인자에서 가장 높은 숫자이므로,
클로저는 두 개의 인자가 있는 것으로 간주됩니다.
여기서 `sorted(by:)` 함수는
인자가 모두 문자열인 클로저로 기대하므로,
축약 인자 `$0`과 `$1`은 모두 타입 `String` 입니다.

<!--
  - test: `closure-syntax-arity-inference`

  ```swifttest
  >> let a: [String: String] = [:]
  >> var b: [String: String] = [:]
  >> b.merge(a, uniquingKeysWith: { $1 })
  >> b.merge(a, uniquingKeysWith: { $0 })
  !$ error: contextual closure type '(String, String) throws -> String' expects 2 arguments, but 1 was used in closure body
  !! b.merge(a, uniquingKeysWith: { $0 })
  !! ^
  ```
-->

### 연산자 메서드 (Operator Methods)

사실 위의 클로저 표현식을 *더 짧게* 작성하는 방법이 있습니다.
Swift의 `String` 타입은
보다 큰 연산자(`>`)의
문자열 별 구현을 `String` 타입의 매개변수 두 개가 있는 메서드로 정의하고,
`Bool` 타입의 값을 반환합니다.
이것은 `sorted(by:)` 메서드에 필요한 메서드 타입과 정확하게 일치합니다.
따라서 보다 큰 연산자 자체를 인자로 전달하면,
Swift는 문자열에 특화된 구현을 사용하겠다는 의도로 자동으로 추론합니다:

```swift
reversedNames = names.sorted(by: >)
```

<!--
  - test: `closureSyntax`

  ```swifttest
  -> reversedNames = names.sorted(by: >)
  >> assert(reversedNames == ["Ewa", "Daniella", "Chris", "Barry", "Alex"])
  ```
-->

연산자 메서드에 대한 자세한 내용은 [연산자 메서드 (Operator Methods)](./advanced-operators.md#연산자-메서드-operator-methods)를 참고바랍니다.

## 후행 클로저 (Trailing Closures)

함수의 마지막 인자로 클로저 표현식을 전달해야 하고
클로저 표현식이 긴 경우
*후행 클로저(trailing closure)*로 작성하는 것이 유용할 수 있습니다.
후행 클로저는 함수의 인자이지만
함수 호출의 소괄호 다음에 작성합니다.
후행 클로저 구문을 사용할 때,
해당 클로저의 인자 레이블을
함수 호출 시에 작성하지 않습니다.
함수 호출은 여러개의 후행 클로저를 포함할 수 있지만;
아래 예시에서는 하나의 후행 클로저만 사용합니다.

```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // function body goes here
}

// Here's how you call this function without using a trailing closure:

someFunctionThatTakesAClosure(closure: {
    // closure's body goes here
})

// Here's how you call this function with a trailing closure instead:

someFunctionThatTakesAClosure() {
    // trailing closure's body goes here
}
```

<!--
  - test: `closureSyntax`

  ```swifttest
  -> func someFunctionThatTakesAClosure(closure: () -> Void) {
        // function body goes here
     }

  -> // Here's how you call this function without using a trailing closure:

  -> someFunctionThatTakesAClosure(closure: {
        // closure's body goes here
     })

  -> // Here's how you call this function with a trailing closure instead:

  -> someFunctionThatTakesAClosure() {
        // trailing closure's body goes here
     }
  ```
-->

위의 [클로저 표현식 문법 (Closure Expression Syntax)](#클로저-표현식-문법-closure-expression-syntax) 섹션에 문자열 정렬 클로저는
후행 클로저로 `sorted(by:)` 메서드의 소괄호 바깥에 작성될 수 있습니다:

```swift
reversedNames = names.sorted() { $0 > $1 }
```

<!--
  - test: `closureSyntax`

  ```swifttest
  -> reversedNames = names.sorted() { $0 > $1 }
  >> assert(reversedNames == ["Ewa", "Daniella", "Chris", "Barry", "Alex"])
  ```
-->

클로저 표현식이
함수나 메서드의 유일한 인자로 제공되고,
그 표현식을 후행 클로저로 작성하는 경우에는,
함수를 호출할 때 함수나 메서드 이름 뒤에 소괄호 `()`를 작성할 필요가 없습니다:

```swift
reversedNames = names.sorted { $0 > $1 }
```

<!--
  - test: `closureSyntax`

  ```swifttest
  -> reversedNames = names.sorted { $0 > $1 }
  >> assert(reversedNames == ["Ewa", "Daniella", "Chris", "Barry", "Alex"])
  ```
-->

후행 클로저는 클로저가 길어서
한 줄로 인라인 작성하기 불가능할 때 유용합니다.
예를 들어 Swift의 `Array` 타입은
단일 인자로 클로저 표현식을 가지는 `map(_:)` 메서드가 있습니다.
이 클로저는 배열의 각 아이템에 대해 한 번 호출되고,
아이템에 대해 매핑된 대체 값(다른 타입일 수 있음)이 반환됩니다.
`map(_:)`에 전달한 클로저에 작성된 코드에 따라
매핑 특성과 반환된 값의 타입을
지정합니다.

제공된 클로저에 각 배열의 요소를 적용한 후에
`map(_:)` 메서드는 기존 배열에 해당 값과 같은 순서로
새로 매핑된 값의 새로운 배열을 반환합니다.

다음은 `Int` 값의 배열을 `String` 값의 배열로 변환하기 위해
후행 클로저와 `map(_:)` 메서드를 어떻게 사용하는지 나타냅니다.
배열 `[16, 58, 510]`은 새로운 배열
`["OneSix", "FiveEight", "FiveOneZero"]`을 생성하는데 사용됩니다:

```swift
let digitNames = [
    0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
]
let numbers = [16, 58, 510]
```

<!--
  - test: `arrayMap`

  ```swifttest
  -> let digitNames = [
        0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
        5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
     ]
  -> let numbers = [16, 58, 510]
  ```
-->

위 코드는 정수와 그 정수에 맞는
영어 표기를 매핑하는 딕셔너리를 생성합니다.
문자열로 변환하기 위한 정수 배열도 정의합니다.

`numbers` 배열을 사용하여 후행 클로저로 `map(_:)` 메서드로
클로저 표현식을 전달하여 `String` 값의 배열을 생성할 수 있습니다:

```swift
let strings = numbers.map { (number) -> String in
    var number = number
    var output = ""
    repeat {
        output = digitNames[number % 10]! + output
        number /= 10
    } while number > 0
    return output
}
// strings is inferred to be of type [String]
// its value is ["OneSix", "FiveEight", "FiveOneZero"]
```

<!--
  - test: `arrayMap`

  ```swifttest
  -> let strings = numbers.map { (number) -> String in
        var number = number
        var output = ""
        repeat {
           output = digitNames[number % 10]! + output
           number /= 10
        } while number > 0
        return output
     }
  // strings is inferred to be of type [String]
  /> its value is [\"\(strings[0])\", \"\(strings[1])\", \"\(strings[2])\"]
  </ its value is ["OneSix", "FiveEight", "FiveOneZero"]
  ```
-->

`map(_:)` 메서드는 배열의 각 요소마다 클로저 표현식을 한 번씩 호출합니다.
매핑할 배열의 값에서 추론할 수 있으므로,
클로저의 입력 매개변수 인 `number` 타입을 지정할 필요가 없습니다.

이 예시에서
변수 `number`는 클로저의 `number` 매개변수 값으로 초기화되기 때문에,
값은 클로저 본문 내에서 수정될 수 있습니다.
(함수와 클로저의 매개변수는 기본적으로 상수입니다.)
클로저 표현식은 매핑된 출력 배열에 저장될 타입을 나타내기 위해
`String` 타입도 반환 타입으로 지정합니다.

클로저 표현식은 호출될 때마다 `output`이라는 문자열을 만듭니다.
나머지 연산자(`number % 10`)를 이용하여 `number`의 마지막 숫자를 계산하고,
`digitNames` 딕셔너리에서 적절한 문자열로 변환하는데 사용합니다.
클로저는 0보다 큰 정수에 대한 문자열 표현을 생성할 수 있습니다.

> Note: `digitNames` 딕셔너리의 서브스크립트 호출 뒤에
> 느낌표(`!`)가 붙는 이유는,
> 딕셔너리 서브스크립트는 키가 존재하지 않을 수 있기 때문에
> 옵셔널 값을 반환하기 때문입니다.
> 위의 예시에서 `digitNames` 딕셔너리에
> `number % 10`은 항상 유효한 서브스크립트 키를 보장하므로
> 느낌표는 서브스크립트의 옵셔널 반환값에 저장된 `String` 값을
> 강제로 언래핑 하는데 사용됩니다.

`digitNames` 딕셔너리에서 반환된 문자열이
`output` *앞에* 추가되어
숫자의 문자열 버전을 역순으로 조립합니다.
(표현식 `number % 10`은
`16`의 경우 `6`, `58`의 경우 `8`, `510`의 경우 `0`을 제공합니다.)

그러면 `number` 변수는 `10`으로 나누어 집니다.
이것은 정수이기 때문에, 소수점 아래는 버림이 되고,
`16`은 `1`, `58`은 `5`, `510`은 `51`이 됩니다.

이 과정은 `number`가 `0`이 될 때까지 반복하고,
최종적으로 만들어진 `output` 문자열이 클로저에 의해 반환되고,
`map(_:)` 메서드에 의해 결과 배열에 추가됩니다.

위의 예시에서 사용된 후행 클로저 구문은,
클로저의 기능을 해당 함수를 지원하는 바로 뒤에
깔끔하게 캡슐화하여 작성할 수 있게 해줍니다.
이를 통해 클로저 전체를
`map(_:)` 메서드의 소괄호로 감쌀 필요가 없습니다.

함수가 여러 개의 클로저를 인자로 받을 경우,
첫 번재 후행 클로저는 인자 레이블 없이 작성하고,
그 이후의 후행 클로저는 인자 레이블을 명시해야 합니다.
예를 들어
아래의 함수는 사진 갤러리에서 사진을 불러오는 기능을 수행합니다:

```swift
func loadPicture(from server: Server, completion: (Picture) -> Void, onFailure: () -> Void) {
    if let picture = download("photo.jpg", from: server) {
        completion(picture)
    } else {
        onFailure()
    }
}
```

<!--
  - test: `multiple-trailing-closures`

  ```swifttest
  >> struct Server { }
  >> struct Picture { }
  >> func download(_ path: String, from server: Server) -> Picture? {
  >>     return Picture()
  >> }
  -> func loadPicture(from server: Server, completion: (Picture) -> Void, onFailure: () -> Void) {
         if let picture = download("photo.jpg", from: server) {
             completion(picture)
         } else {
             onFailure()
         }
     }
  ```
-->

이 함수를 호출하여 사진을 불러올 때,
두 개의 클로저를 전달합니다.
첫 번째 클로저는 사진 다운로드 완료 후에
사진을 표시하는 완료 핸들러입니다.
두 번째 클로저는 오류를 표시하는
오류 핸들러입니다.

```swift
loadPicture(from: someServer) { picture in
    someView.currentPicture = picture
} onFailure: {
    print("Couldn't download the next picture.")
}
```

<!--
  - test: `multiple-trailing-closures`

  ```swifttest
  >> struct View {
  >>    var currentPicture = Picture() { didSet { print("Changed picture") } }
  >> }
  >> var someView = View()
  >> let someServer = Server()
  -> loadPicture(from: someServer) { picture in
         someView.currentPicture = picture
     } onFailure: {
         print("Couldn't download the next picture.")
     }
  << Changed picture
  ```
-->

이 예시에서
`loadPicture(from:completion:onFailure:)` 함수는
네트워크 작업을 백그라운드로 전달하고
네트워크 작업이 완료되면 두 완료 핸들러 중 하나를 호출합니다.
이러한 방식으로 함수를 작성하면
두 상황을 모두 처리하는 하나의 클로저를 사용하는 대신
성공적인 다운로드 후 사용자 인터페이스를 업데이트 하는 코드와
네트워크 오류를 처리하는 코드를 명확하게 분리할 수 있습니다.

> Note: 완료 핸들러(Completion handlers)는
> 여러 핸들러가 중첩되어 있으면 읽기 어려울 수 있습니다.
> 이것을 대체하기 위해선 [동시성 (Concurrency)](./concurrency.md)에서 설명되어진 것과 같이
> 비동기 코드를 사용하면 됩니다.

## 캡처값 (Capturing Values)

클로저는 정의된 주변 컨텍스트로부터
상수와 변수를 *캡처(capture)*할 수 있습니다.
그러면 클로저는 상수와 변수를 정의한
원래 범위가 더 이상 존재하지 않더라도
본문 내에서 해당 상수와 변수의 값을 참조하고 수정할 수 있습니다.

Swift에서 값을 캡처할 수 있는 가장 간단한 클로저 형태는
다른 함수의 본문 내에 작성하는 중첩 함수입니다.
중첩 함수는 바깥 함수의 어떠한 인자도 캡처할 수 있고
바깥 함수 내에 정의된 상수와 변수를 캡처할 수도 있습니다.

아래는 `incrementer`라는 중첩 함수가 포함된
`makeIncrementer`라는 함수의 예시입니다.
중첩된 `incrementer()` 함수는
자신을 둘러싼 컨텍스트에서
`runningTotal`과 `amount`인 두 값을 캡처합니다.
이 값을 캡처한 후에
`incrementer`는 호출될 때마다 `runningTotal`을 `amount`만큼 증가시키는
`makeIncrementer`에 의해 클로저로 반환됩니다.

```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementer() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementer
}
```

<!--
  - test: `closures`

  ```swifttest
  -> func makeIncrementer(forIncrement amount: Int) -> () -> Int {
        var runningTotal = 0
        func incrementer() -> Int {
           runningTotal += amount
           return runningTotal
        }
        return incrementer
     }
  ```
-->

`makeIncrementer`의 반환 타입은 `() -> Int`입니다.
이것은 단순한 값이 아닌 *함수*를 반환한다는 의미입니다.
반환하는 함수에는 매개변수가 없으며,
호출될 때마다 `Int` 값을 반환합니다.
함수가 다른 함수를 반환하는 방법을 알아보려면
[반환 타입으로 함수 타입 (Function Types as Return Types)](./functions.md#반환-타입으로-함수-타입-function-types-as-return-types)을 참고바랍니다.

`makeIncrementer(forIncrement:)` 함수는 반환될 현재 증가분을 저장하기 위해
`runningTotal`이라는 정수 변수를 정의합니다.
이 변수는 `0`으로 초기화 됩니다.

`makeIncrementer(forIncrement:)` 함수는 `forIncrement`라는 인자 레이블과 `amount`라는 매개변수 이름을 가지는
하나의 `Int` 매개변수를 가집니다.
이 매개변수에 전달된 인자값은
반환된 증가 함수가 호출될 때마다
`runningTotal`을 얼마나 증가시켜야 하는지 지정합니다.
`makeIncrementer` 함수는 실제 증가를 수행하는
`incrementer`라는 중첩 함수를 정의합니다.
이 함수는 간단하게 `amount`를 `runningTotal`에 더하고 그 결과를 반환합니다.

단독으로 고려할 때,
`incrementer()` 함수는 비정상적으로 보일 수 있습니다:

```swift
func incrementer() -> Int {
    runningTotal += amount
    return runningTotal
}
```

<!--
  - test: `closuresPullout`

  ```swifttest
  -> func incrementer() -> Int {
  >>    var runningTotal = 0
  >>    let amount = 1
        runningTotal += amount
        return runningTotal
     }
  ```
-->

`incrementer()` 함수는 매개변수가 없음에도
함수 본문 내에 `runningTotal`과 `amount`를 참조하고 있습니다.
둘러싸인 함수에 `runningTotal`과 `amount`에 대한
*참조(reference)*를 캡처하고 함수 내에서 사용합니다.
참조를 캡처하는 것은 `makeIncrementer` 호출이 종료될 때,
`runningTotal`과 `amount`가 사라지지 않고
다음에 `incrementer` 함수가 호출될 때
`runningTotal`을 사용할 수 있습니다.

> Note: 최적화를 위해,
> Swift는 값이 클로저에 의해 변경되지 않고,
> 클로저가 생성된 후 값이 변경되지 않는 경우
> 해당 값을 *복사*하여 저장할 수 있습니다.
>
> Swift는 더이상 필요하지 않을때 변수를 처리하는 것과
> 관련된 모든 메모리 관리도 처리합니다.

다음은 `makeIncrementer` 동작에 대한 예시입니다:

```swift
let incrementByTen = makeIncrementer(forIncrement: 10)
```

<!--
  - test: `closures`

  ```swifttest
  -> let incrementByTen = makeIncrementer(forIncrement: 10)
  ```
-->

이 예시에서는 호출될 때마다 `runningTotal` 변수에
`10`을 더하는 증가 함수를 참조하도록
`incrementByTen`이라는 상수를 설정합니다.
함수를 여러 번 호출하면, 이 동작이 어떻게 수행되는지 확인할 수 있습니다:

```swift
incrementByTen()
// returns a value of 10
incrementByTen()
// returns a value of 20
incrementByTen()
// returns a value of 30
```

<!--
  - test: `closures`

  ```swifttest
  >> let r0 =
  -> incrementByTen()
  /> returns a value of \(r0)
  </ returns a value of 10
  >> let r1 =
  -> incrementByTen()
  /> returns a value of \(r1)
  </ returns a value of 20
  >> let r2 =
  -> incrementByTen()
  /> returns a value of \(r2)
  </ returns a value of 30
  ```
-->

<!--
  Rewrite the above to avoid discarding the function's return value.
  Tracking bug is <rdar://problem/35301593>
-->

두 번째 증가 함수를 생성하면,
새로운 별도의 `runningTotal` 변수에 대한 고유한 참조를 저장합니다:

```swift
let incrementBySeven = makeIncrementer(forIncrement: 7)
incrementBySeven()
// returns a value of 7
```

<!--
  - test: `closures`

  ```swifttest
  -> let incrementBySeven = makeIncrementer(forIncrement: 7)
  >> let r3 =
  -> incrementBySeven()
  /> returns a value of \(r3)
  </ returns a value of 7
  ```
-->

기존 증가 함수(`incrementByTen`)를 다시 호출하면,
해당 함수가 캡처한 자신의 `runningTotal` 변수만 계속 증가하며,
`incrementBySeven`으로 캡처된 변수에는 영향을 주지 않습니다:

```swift
incrementByTen()
// returns a value of 40
```

<!--
  - test: `closures`

  ```swifttest
  >> let r4 =
  -> incrementByTen()
  /> returns a value of \(r4)
  </ returns a value of 40
  ```
-->

> Note: 클래스 인스턴스의 프로퍼티에 클로저를 할당하고,
> 클로저가 인스턴스나 인스턴스의 멤버를 참조하여 해당 인스턴스를 캡처하면
> 클로저와 인스턴스 사이에 강한 참조 순환이 생성됩니다.
> Swift는 *캡처 리스트*를 사용하여 이러한 강한 참조 순환을 끊습니다.
> 자세한 내용은 [클로저의 강한 순환 참조 (Strong Reference Cycles for Closures)](./automatic-reference-counting.md#클로저의-강한-순환-참조-strong-reference-cycles-for-closures)를 참고바랍니다.

## 클로저는 참조 타입 (Closures Are Reference Types)

위 예시에서
`incrementBySeven`과 `incrementByTen`은 상수이지만
이러한 상수가 참조하는 클로저는
캡처한 `runningTotal` 변수를 계속 증가시킬 수 있습니다.
이는 함수와 클로저가 *참조 타입(reference types)*이기 때문입니다.

함수나 클로저를 상수나 변수에 할당할 때마다,
실제로 해당 상수나 변수를
함수나 클로저에 대한 *참조*로 설정합니다.
위의 예시에서
`incrementByTen`이 *참조하는* 클로저 자체는 상수이지만,
그 클로저의 내용은 고정된 것이 아닙니다.

이것은 또한 서로 다른 두 상수나 변수에 클로저를 할당한다면
두 상수나 변수는 모두 같은 클로저를 참조한다는 의미입니다.

```swift
let alsoIncrementByTen = incrementByTen
alsoIncrementByTen()
// returns a value of 50

incrementByTen()
// returns a value of 60
```

<!--
  - test: `closures`

  ```swifttest
  -> let alsoIncrementByTen = incrementByTen
  >> let r5 =
  -> alsoIncrementByTen()
  /> returns a value of \(r5)
  </ returns a value of 50

  >> let r6 =
  -> incrementByTen()
  /> returns a value of \(r6)
  </ returns a value of 60
  ```
-->

위의 예시는 `alsoIncrementByTen` 호출은
`incrementByTen` 호출과 같음을 보여줍니다.
두 상수 모두 같은 클로저를 참조하기 때문에,
둘 다 증가하고 같은 러닝 합계를 반환합니다.

## 탈출 클로저 (Escaping Closures)

함수의 인자로 클로저를 전달하지만
함수가 반환된 후 호출되는 클로저를
함수를 *탈출(escape)*한다고 말합니다.
클로저를 매개변수로 갖는 함수를 선언할 때,
이 클로저가 탈출할 수 있음을 나타내기 위해
매개변수의 타입 전에 `@escaping`을 작성합니다.

클로저가 탈출할 수 있는 한 가지 방법은
함수 바깥에 정의된 변수에 저장되는 경우입니다.
예를 들어
비동기적 작업을 시작하는 대부분의 함수는
완료 핸들러로 클로저를 인자로 사용합니다.
이 함수는 작업을 시작한 후에 반환되지만
작업이 완료될 때까지 클로저가 호출되지 않습니다 ---
클로저는 나중에 호출되려면 탈출해야 합니다.
예를 들어:

```swift
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}
```

<!--
  - test: `noescape-closure-as-argument, implicit-self-struct`

  ```swifttest
  -> var completionHandlers: [() -> Void] = []
  -> func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
         completionHandlers.append(completionHandler)
     }
  ```
-->

`someFunctionWithEscapingClosure(_:)` 함수는 인자로 클로저를 가지고 있고
함수 외부에 선언된 배열에 추가합니다.
함수의 매개변수에 `@escaping`을 표시하지 않으면,
컴파일시 오류가 발생합니다.

클래스 인스턴스를 참조하는 `self`가
탈출 클로저 안에서 사용되는 경우에는 특히 주의가 필요합니다.
탈출 클로저에 `self` 캡처는
강한 참조 순환이 생기기 쉽습니다.
참조 순환에 대한 자세한 내용은
[자동 참조 카운팅 (Automatic Reference Counting)](./automatic-reference-counting.md)을 참고바랍니다.

일반적으로 클로저는 클로저 내부에서 변수를 사용할 때
암시적으로 변수를 캡처하지만,
이 경우에는 명시적으로 작성해야 합니다.
`self`를 캡처하려면,
사용할 때 명시적으로 `self`를 작성하거나,
클로저의 캡처 리스트에 `self`를 포함합니다.
`self`를 명시적으로 작성하는 것은 의도를 분명하게 표현하고,
참조 순환이 없음을 확인하도록 유도하는 역할도 합니다.
예를 들어 아래 코드에서
`someFunctionWithEscapingClosure(_:)`에 전달된 클로저는
명시적으로 `self`를 참조합니다.
반대로 `someFunctionWithNonescapingClosure(_:)`에 전달된 클로저는
탈출하지 않는 클로저이므로, 암시적으로 `self`를 참조할 수 있습니다.

```swift
func someFunctionWithNonescapingClosure(closure: () -> Void) {
    closure()
}

class SomeClass {
    var x = 10
    func doSomething() {
        someFunctionWithEscapingClosure { self.x = 100 }
        someFunctionWithNonescapingClosure { x = 200 }
    }
}

let instance = SomeClass()
instance.doSomething()
print(instance.x)
// Prints "200"

completionHandlers.first?()
print(instance.x)
// Prints "100"
```

<!--
  - test: `noescape-closure-as-argument`

  ```swifttest
  -> func someFunctionWithNonescapingClosure(closure: () -> Void) {
         closure()
     }

  -> class SomeClass {
         var x = 10
         func doSomething() {
             someFunctionWithEscapingClosure { self.x = 100 }
             someFunctionWithNonescapingClosure { x = 200 }
         }
     }

  -> let instance = SomeClass()
  -> instance.doSomething()
  -> print(instance.x)
  <- 200

  -> completionHandlers.first?()
  -> print(instance.x)
  <- 100
  ```
-->

다음은 클로저의 캡처 리스트에 포함하여
`self`를 캡처하고 암시적으로
`self`를 참조하는 `doSomething()` 입니다:

```swift
class SomeOtherClass {
    var x = 10
    func doSomething() {
        someFunctionWithEscapingClosure { [self] in x = 100 }
        someFunctionWithNonescapingClosure { x = 200 }
    }
}
```

<!--
  - test: `noescape-closure-as-argument`

  ```swifttest
  -> class SomeOtherClass {
         var x = 10
         func doSomething() {
             someFunctionWithEscapingClosure { [self] in x = 100 }
             someFunctionWithNonescapingClosure { x = 200 }
         }
     }
  >> completionHandlers = []
  >> let instance2 = SomeOtherClass()
  >> instance2.doSomething()
  >> print(instance2.x)
  << 200
  >> completionHandlers.first?()
  >> print(instance2.x)
  << 100
  ```
-->

`self`가 구조체나 열거형 인스턴스이면,
항상 암시적으로 `self`를 참조할 수 있습니다.
그러나
탈출 클로저는 구조체나 열거형 인스턴스이면 `self`에 대한
변경 가능한 참조를 캡처할 수 없습니다.
구조체와 열거형은 [구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)](./structures-and-classes.md#구조체와-열거형은-값-타입-structures-and-enumerations-are-value-types)에서 설명했듯이
공유된 가변성을 허용하지 않습니다.

```swift
struct SomeStruct {
    var x = 10
    mutating func doSomething() {
        someFunctionWithNonescapingClosure { x = 200 }  // Ok
        someFunctionWithEscapingClosure { x = 100 }     // Error
    }
}
```

<!--
  - test: `struct-capture-mutable-self`

  ```swifttest
  >> var completionHandlers: [() -> Void] = []
  >> func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
  >>     completionHandlers.append(completionHandler)
  >> }
  >> func someFunctionWithNonescapingClosure(closure: () -> Void) {
  >>     closure()
  >> }
  -> struct SomeStruct {
         var x = 10
         mutating func doSomething() {
             someFunctionWithNonescapingClosure { x = 200 }  // Ok
             someFunctionWithEscapingClosure { x = 100 }     // Error
         }
     }
  !$ error: escaping closure captures mutating 'self' parameter
  !! someFunctionWithEscapingClosure { x = 100 }     // Error
  !! ^
  !$ note: captured here
  !! someFunctionWithEscapingClosure { x = 100 }     // Error
  !! ^
  ```
-->

위의 예시에서 `someFunctionWithEscapingClosure` 함수 호출은
변경 가능한 메서드 내부에 있기 때문에
오류이고
`self`는 변경 가능합니다.
이것은 탈출 클로저는 구조체인 `self`를 변경가능한 참조로
캡처할 수 없다는 규칙을 위반합니다.

<!--
  - test: `noescape-closure-as-argument`

  ```swifttest
  // Test the non-error portion of struct-capture-mutable-self
  >> struct SomeStruct {
  >>     var x = 10
  >>     mutating func doSomething() {
  >>         someFunctionWithNonescapingClosure { x = 200 }
  >>     }
  >> }

  >> completionHandlers = []
  >> var instance3 = SomeStruct()
  >> instance3.doSomething()
  >> print(instance3.x)
  << 200
  ```
-->

<!--
  - test: `noescape-closure-as-argument`

  ```swifttest
  >> struct S {
  >>     var x = 10
  >>     func doSomething() {
  >>         someFunctionWithEscapingClosure { print(x) }  // OK
  >>     }
  >> }

  >> completionHandlers = []
  >> var s = S()
  >> s.doSomething()
  >> s.x = 99  // No effect on self.x already captured -- S is a value type
  >> completionHandlers.first?()
  << 10
  ```
-->

## 자동 클로저 (Autoclosures)

*자동 클로저(autoclosure)*는 함수에 인자로 전달되는 표현식을 감싸기 위해
자동으로 생성되는 클로저입니다.
이 클로저는 인자를 가지지 않으며,
호출될 때,
내부에 감싸진 표현식의 값을 반환합니다.
이러한 구문상의 편의를 통해 명시적 클로저 대신에 일반 표현식을 작성하여
함수의 매개변수 주위의 중괄호를 생략할 수 있습니다.

자동 클로저를 가지는 함수를 *호출*하는 것은 일반적이지만
이러한 함수를 *구현*하는 것은 일반적이지 않습니다.
예를 들어
`assert(condition:message:file:line:)` 함수는
`condition`과 `message` 매개변수에 대한 자동 클로저를 가집니다;
`condition` 매개변수는 오직 디버그 빌드에서만 평가되고,
`message` 매개변수는 `condition`이 `false`에서만 평가됩니다.

클로저가 호출될 때까지 코드 내부 실행이 되지 않기 때문에,
자동 클로저는 평가를 지연시킬 수 있습니다.
평가 지연은
사이드 이펙트가 있거나 계산 비용이 큰
코드를 제어된 시점에 실행하고자 할 때 유용합니다.
아래 코드는 클로저가 어떻게 평가를 지연하는지 보여줍니다.

```swift
var customersInLine = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
print(customersInLine.count)
// Prints "5"

let customerProvider = { customersInLine.remove(at: 0) }
print(customersInLine.count)
// Prints "5"

print("Now serving \(customerProvider())!")
// Prints "Now serving Chris!"
print(customersInLine.count)
// Prints "4"
```

<!--
  - test: `autoclosures`

  ```swifttest
  -> var customersInLine = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
  -> print(customersInLine.count)
  <- 5

  -> let customerProvider = { customersInLine.remove(at: 0) }
  -> print(customersInLine.count)
  <- 5

  -> print("Now serving \(customerProvider())!")
  <- Now serving Chris!
  -> print(customersInLine.count)
  <- 4
  ```
-->

<!--
  Using remove(at:) instead of popFirst() because the latter only works
  with ArraySlice, not with Array:
      customersInLine[0..<3].popLast()     // fine
      customersInLine[0..<3].popFirst()    // fine
      customersInLine.popLast()            // fine
      customersInLine.popFirst()           // FAIL
  It also returns an optional, which complicates the listing.
-->

<!--
  TODO: It may be worth describing the differences between ``lazy`` and autoclousures.
-->

클로저 내부의 코드에 의해
`customersInLine` 배열의 첫 번째 요소는 삭제되지만,
클로저가 실제로 호출되기 전까지 삭제되지 않습니다.
클로저가 호출되지 않으면,
클로저 내부의 표현식은 평가되지 않고,
배열의 요소도 삭제되지 않습니다.
`customerProvider` 타입은 `String`이 아니고
`() -> String`입니다 ---
매개변수가 없고 문자열을 반환하는 함수입니다.

함수의 인자로 클로저를 전달할 때도
동일한 지연 평가 동작을 가질 수 있습니다.

```swift
// customersInLine is ["Alex", "Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: { customersInLine.remove(at: 0) } )
// Prints "Now serving Alex!"
```

<!--
  - test: `autoclosures-function`

  ```swifttest
  >> var customersInLine = ["Alex", "Ewa", "Barry", "Daniella"]
  /> customersInLine is \(customersInLine)
  </ customersInLine is ["Alex", "Ewa", "Barry", "Daniella"]
  -> func serve(customer customerProvider: () -> String) {
         print("Now serving \(customerProvider())!")
     }
  -> serve(customer: { customersInLine.remove(at: 0) } )
  <- Now serving Alex!
  ```
-->

위에서 `serve(customer:)` 함수는
고객의 이름을 반환하는 명시적 클로저를 인자를 가집니다.
아래 `serve(customer:)`의 버전은
같은 동작을 수행하지만, 명시적 클로저를 가지는 대신에
매개변수 타입에 `@autoclosure` 속성을 표기하여
자동 클로저를 가집니다.
이제 이 함수는
마치 `String` 타입 값을 인자로 받는 것처럼 호출할 수 있습니다.
`customerProvider` 매개변수의 타입은
`@autoclosure` 속성으로 표시되어 있기 때문에
인자는 자동으로 클로저로 변환됩니다.

```swift
// customersInLine is ["Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: @autoclosure () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: customersInLine.remove(at: 0))
// Prints "Now serving Ewa!"
```

<!--
  - test: `autoclosures-function-with-autoclosure`

  ```swifttest
  >> var customersInLine = ["Ewa", "Barry", "Daniella"]
  /> customersInLine is \(customersInLine)
  </ customersInLine is ["Ewa", "Barry", "Daniella"]
  -> func serve(customer customerProvider: @autoclosure () -> String) {
         print("Now serving \(customerProvider())!")
     }
  -> serve(customer: customersInLine.remove(at: 0))
  <- Now serving Ewa!
  ```
-->

> Note: 자동 클로저 남용은 코드 이해를 어렵게 만들 수 있습니다.
> 평가가 지연된다는 사실이
> 컨텍스트와 함수 이름만으로도 명확히 드러나야 합니다.

탈출이 허용되는 자동 클로저를 사용하고 싶다면,
`@autoclosure`와 `@escaping` 속성을 함께 사용해야 합니다.
`@escaping` 속성은 위의 [탈출 클로저 (Escaping Closures)](#탈출-클로저-escaping-closures)에 설명되어 있습니다.

```swift
// customersInLine is ["Barry", "Daniella"]
var customerProviders: [() -> String] = []
func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {
    customerProviders.append(customerProvider)
}
collectCustomerProviders(customersInLine.remove(at: 0))
collectCustomerProviders(customersInLine.remove(at: 0))

print("Collected \(customerProviders.count) closures.")
// Prints "Collected 2 closures."
for customerProvider in customerProviders {
    print("Now serving \(customerProvider())!")
}
// Prints "Now serving Barry!"
// Prints "Now serving Daniella!"
```

<!--
  - test: `autoclosures-function-with-escape`

  ```swifttest
  >> var customersInLine = ["Barry", "Daniella"]
  /> customersInLine is \(customersInLine)
  </ customersInLine is ["Barry", "Daniella"]
  -> var customerProviders: [() -> String] = []
  -> func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {
         customerProviders.append(customerProvider)
     }
  -> collectCustomerProviders(customersInLine.remove(at: 0))
  -> collectCustomerProviders(customersInLine.remove(at: 0))

  -> print("Collected \(customerProviders.count) closures.")
  <- Collected 2 closures.
  -> for customerProvider in customerProviders {
         print("Now serving \(customerProvider())!")
     }
  <- Now serving Barry!
  <- Now serving Daniella!
  ```
-->

위의 코드에서
`customerProvider` 인자로
전달된 클로저를 호출하는 대신에
`collectCustomerProviders(_:)` 함수는
클로저를 `customerProviders` 배열에 추가합니다.
이 배열은 함수의 범위 밖에 선언되어 있기 때문에,
배열에 저장된 클로저는 함수가 반환된 후에 실행될 수 있다는 의미입니다.
그 결과
`customerProvider` 인자의 값은
함수의 범위를 벗어날 수 있어야 합니다.

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->