# 제너릭 파라미터와 인수 (Generic Parameters and Arguments)

이 챕터에서 제너릭 타입, 함수, 그리고 초기화 구문에 대한 파라미터와 인수를 설명합니다. 제너릭 타입, 함수, 서브 스크립트, 또는 초기화 구문을 선언할 때 제너릭 타입, 함수, 또는 초기화 구문이 동작할 수 있는 타입 파라미터를 지정합니다. 이러한 타입 파라미터는 제너릭 타입의 인스턴스가 생성되거나 제너릭 함수 또는 초기화 구문이 호출 될 때 실제 구체적인 타입 인수에 의해 대체되는 자리표시자 역할을 합니다.

Swift 의 제너릭에 대한 개요는 [제너릭 (Generics)](../language-guide-1/generics.md) 을 참고 바랍니다.

## 제너릭 파라미터 절 (Generic Parameter Clause)

_제너릭 파라미터 절 (generic parameter clause)_ 은 해당 파라미터의 관련된 모든 제약조건과 요구사항과 함께 제너릭 타입 또는 함수의 타입 파라미터를 지정합니다. 제너릭 파라미터 절은 꺾쇠 괄호 (<>) 로 둘러싸여 있고 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 3.42.01.png>)

_제너릭 파라미터 목록 (generic parameter list)_ 은 콤마로 구분된 제너릭 파라미터의 목록이고 각각 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 3.42.32.png>)

제너릭 파라미터는 _타입 파라미터 (type parameter)_ 와 옵셔널 _제약사항 (constraint)_ 로 구성됩니다. _타입 파라미터 (type parameter)_ 는 자리표시자 타입의 간단한 이름입니다 (예를 들어 `T`, `U`, `V`, `Key`, `Value`, 등). 함수 또는 초기화 구문의 시그니처를 포함하는 나머지 타입, 함수, 또는 초기화 구문 선언에서 타입 파라미터와 모든 연관된 타입에 접근할 수 있습니다.

_제약조건 (constraint)_ 은 타입 파라미터가 특정 클래스를 상속하거나 프로토콜 또는 프로토콜 구성을 준수하도록 지정합니다. 예를 들어 아래 제너릭 함수에서 제너릭 파라미터 `T: Comparable` 은 타입 파라미터 `T` 를 대신하는 모든 타입 인수는 `Comparable` 프로토콜을 준수해야 함을 나타냅니다.

```swift
func simpleMax<T: Comparable>(_ x: T, _ y: T) -> T {
    if x < y {
        return y
    }
    return x
}
```

예를 들어 `Int` 와 `Double` 은 `Comparable` 프로토콜을 준수하므로 이 함수는 두 타입의 인수를 허용합니다. 제너릭 타입과 다르게 제너릭 함수 또는 초기화 구문을 사용할 때 제너릭 인수 절을 지정하지 않습니다. 대신에 타입 인수는 함수나 초기화 구문에 전달되는 인수의 타입으로 부터 유추됩니다.

```swift
simpleMax(17, 42) // T is inferred to be Int
simpleMax(3.14159, 2.71828) // T is inferred to be Double
```

### 제너릭 Where 절 (Generic Where Clauses)

타입이나 함수의 바디의 열린 괄호 직전에 제너릭 `where` 절을 포함하여 타입 파라미터와 연관된 타입의 요구사항을 추가로 지정할 수 있습니다. 제너릭 `where` 절은 `where` 키워드 다음에 콤마로 구분된 하나 이상의 _요구사항 (requirements)_ 로 구성됩니다.

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 3.44.04.png>)

제너릭 `where` 절에서 _요구사항 (requirements)_ 은 클래스를 상속하거나 프로토콜 또는 프로토콜 구성을 준수하는 타입 파라미터를 지정합니다. 제너릭 `where` 절은 예를 들어 `<T: Comparable>` 은 `<T> where T: Comparable` 과 동일하듯 타입 파라미터에 제약사항을 간단하게 표현하는 구문 설탕을 제공하지만 타입 파라미터와 연관된 타입에 더 복잡한 제약사항을 제공하기 위해 사용할 수 있습니다. 예를 들어 프로토콜을 준수하도록 타입 파라미터의 연관된 타입을 제한할 수 있습니다. 예를 들어 `<S: Sequence> where S.Iterator.Element: Equatable` 은 `S` 가 `Sequence` 프로토콜을 준수하고 연관된 타입 `S.Iterator.Element` 는 `Equatable` 프로토콜을 준수하도록 지정합니다. 이 제약사항은 시퀀스의 각 요소는 동등함을 보장합니다.

`==` 연산자를 사용하여 두 타입이 동일해야 하는 요구사항을 지정할 수도 있습니다. 예를 들어 `<S1: Sequence, S2: Sequence> where S1.Iterator.Element == S2.Iterator.Element` 은 `S1` 과 `S2` 가 `Sequence` 프로토콜을 준수하고 두 시퀀스의 요소는 같은 타입이어야 한다는 제약사항이 있습니다.

타입 파라미터를 대체하는 모든 타입 인수는 타입 파라미터에 있는 모든 제약사항과 요구사항을 만족해야 합니다.

제너릭 `where` 절은 타입 파라미터를 포함하는 선언의 부분으로 또는 타입 파라미터를 포함하는 선언의 내부에 중첩된 선언의 부분으로 나타날 수 있습니다. 중첩된 선언에 대한 제너릭 `where` 절은 둘러싸는 선언의 타입 파라미터를 참조할 수 있습니다; 그러나 `where` 절의 요구사항은 작성된 선언에만 적용됩니다.

둘러싸는 선언도 `where` 절을 가지고 있으면 두 절의 요구사항은 결합됩니다. 아래 예제에서 `startsWithZero()` 는 `Element` 가 `SomeProtocol` 과 `Numeric` 모두 준수하는 경우에만 가능합니다.

```swift
extension Collection where Element: SomeProtocol {
    func startsWithZero() -> Bool where Element: Numeric {
        return first == .zero
    }
}
```

타입 파라미터에 다른 제약조건, 요구사항, 또는 둘 다 제공하여 제너릭 함수 또는 초기화 구문을 오버로드 할 수 있습니다. 오버로드 된 제너릭 함수 또는 초기화 구문을 호출할 때 컴파일러는 호출할 오버로드 된 함수 또는 초기화 구문을 확인하기 위해 이 제약조건을 사용합니다.

제너릭 `where` 절에 대한 자세한 정보와 제너릭 함수 선언의 예제를 보려면 [제너릭 Where 절 (Generic Where Clauses)](../language-guide-1/generics.md#where-generic-where-clauses) 을 참고 바랍니다.

> GRAMMAR OF A GENERIC PARAMETER CLAUSE\
> generic-parameter-clause → `<` [generic-parameter-list](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-list)  `>` \
> generic-parameter-list → [generic-parameter](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter) |  [generic-parameter](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter)  `,` [generic-parameter-list](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-list)\
> generic-parameter → [type-name](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-name)\
> generic-parameter → [type-name](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-name)  `:` [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-identifier)\
> generic-parameter → [type-name](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-name)  `:` [protocol-composition-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_protocol-composition-type)\
> generic-where-clause → `where` [requirement-list](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_requirement-list)\
> requirement-list → [requirement](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_requirement) |  [requirement](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_requirement)  `,` [requirement-list](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_requirement-list)\
> requirement → [conformance-requirement](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_conformance-requirement) |  [same-type-requirement](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_same-type-requirement)\
> conformance-requirement → [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-identifier)  `:` [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-identifier)\
> conformance-requirement → [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-identifier)  `:` [protocol-composition-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_protocol-composition-type)\
> same-type-requirement → [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-identifier)  `==` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type)

## 제너릭 인수 절 (Generic Argument Clause)

_제너릭 인수 절 (generic argument clause)_ 은 제너릭 타입의 타입 인수를 지정합니다. 제너릭 인수 절은 꺾쇠 괄호 (<>) 로 둘러싸여져 있고 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 3.45.58.png>)

_제너릭 인수 목록 (generic argument list)_ 은 콤마로 구분된 타입 인수의 목록입니다. _타입 인수 (type argument)_ 는 제너릭 타입의 제너릭 파라미터 절에서 해당 타입 파라미터를 대체하는 실제 구체적인 타입의 이름입니다. 결과는 해당 제너릭 타입의 특수 버전입니다. 아래 예제는 Swift 표준 라이브러리의 제너릭 딕셔너리 타입의 간단한 버전을 보여줍니다.

```swift
struct Dictionary<Key: Hashable, Value>: Collection, ExpressibleByDictionaryLiteral {
    /* ... */
}
```

제너릭 `Dictionary` 타입의 특수한 버전인 `Dictionary<String, Int>` 는 제너릭 파라미터 `Key: Hashable` 과 `Value` 를 구체적인 타입 인수 `String` 과 `Int` 로 대체하여 구성됩니다. 각 타입 인수는 제너릭 `where` 절에 지정된 추가 요구사항을 포함하여 대체하는 제너릭 파라미터의 모든 제약사항을 충족해야 합니다. 위의 예제에서 `Key` 타입 파라미터는 `Hashable` 프로토콜을 준수하도록 제한되므로 `String` 도 `Hashable` 프로토콜을 준수해야 합니다.

타입 파라미터를 적절한 제약사항과 요구사항을 충족하는 경우 그 자체가 제너릭 타입의 특별한 버전인 타입 인수로 대체할 수 있습니다. 예를 들어 요소 자체가 정수의 배열인 `Array<Int>` 은 `Array<Element>` 배열의 특별한 버전으로 타입 파라미터 `Element` 를 대체할 수 있습니다.

```swift
let arrayOfArrays: Array<Array<Int>> = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```

[제너릭 파라미터 절 (Generic Parameter Clause)](generic-parameters-and-arguments.md#generic-parameter-clause) 에서 언급했듯이 제너릭 함수 또는 초기화 구문의 타입 인수로 지정하기 위해 제너릭 인수 절을 사용하지 않습니다.

> GRAMMAR OF A GENERIC ARGUMENT CLAUSE\
> generic-argument-clause → `<` [generic-argument-list](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-argument-list)  `>` \
> generic-argument-list → [generic-argument](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-argument) |  [generic-argument](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-argument)  `,` [generic-argument-list](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-argument-list)\
> generic-argument → [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type)
