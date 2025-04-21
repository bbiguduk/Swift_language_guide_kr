# 매크로 (Macros)

컴파일 때 코드를 생성하기 위해 매크로를 사용합니다.

매크로 (Macro) 는 소스코드를 컴파일 할 때 변환하므로 반복적인 코드를 직접 작성하지 않아도 됩니다. 컴파일하는 동안 Swift 코드를 빌드하기 전에 코드에 모든 매크로를 확장합니다.

![A diagram showing an overview of macro expansion.  On the left, a stylized representation of Swift code.  On the right, the same code with several lines added by the macro.](../.gitbook/assets/macro-expansion~dark@2x.png)

매크로 확장은 항상 추가 작업입니다: 매크로는 새로운 코드를 추가하지만, 기존의 코드를 절대 삭제 또는 수정하지 않습니다.

매크로 입력과 매크로 확장의 출력은 구문적으로 Swift 코드가 유효한지 확인됩니다. 마찬가지로 매크로에 전달하는 값과 매크로에 의해 생성된 코드의 값이 올바른 타입인지 확인됩니다. 추가적으로 매크로를 확장할 때 매크로의 구현에서 에러가 발생하면 컴파일러는 컴파일 에러로 처리합니다. 이러한 보장은 매크로를 사용하는 코드에 대해 더 쉽게 추론할 수 있으며, 매크로를 잘못 사용하거나 매크로 구현에 버그가 있는 경우 이러한 문제를 쉽게 알 수 있습니다.

Swift 는 두 종류의 매크로를 가지고 있습니다:

- _독립 매크로 (Freestanding macro)_ 는 선언에 첨부되지 않고 자체적으로 나타납니다.

- _첨부 매크로 (Attached macro)_ 는 매크로가 첨부된 선언을 수정합니다.

첨부 매크로와 독립 매크로는 약간 다르게 호출하지만 모두 매크로 확장에 대해 동일한 모델을 따르고 동일한 접근방식을 사용하여 구현합니다. 다음 섹션에서 해당 두 종류 매크로에 대해 더 자세히 설명합니다.

## 독립 매크로 (Freestanding Macros)

독립 매크로 (freestanding macro) 를 호출하기 위해 이름 앞에 숫자 기호 (`#`) 를 작성하고 이름 뒤 소괄호 안에 매크로의 인수를 작성합니다. 예를 들어:

```swift
func myFunction() {
    print("Currently running \(#function)")
    #warning("Something's wrong")
}
```

첫 번째 줄에서 `#function` 은 Swift 표준 라이브러리에 [`function()`](<https://developer.apple.com/documentation/swift/function()>) 을 호출합니다. 이 코드를 컴파일 할 때 Swift 는 `#function` 을 현재 함수의 이름으로 대체하는 매크로의 구현을 호출합니다. 이 코드를 실행하고 `myFunction()` 을 호출할 때 "Currently running myFunction()" 을 보여줍니다. 두 번째 줄에서 `#warning` 은 Swift 표준 라이브러리에서 [`warning(_:)`](<https://developer.apple.com/documentation/swift/warning(_:)>) 매크로를 호출하여 사용자 지정 컴파일 경고를 생성합니다.

독립 매크로는 `#function` 과 같이 값을 생성하거나 `#warning` 과 같이 컴파일 때 동작을 수행할 수 있습니다.

## 첨부 매크로 (Attached Macros)

첨부 매크로 (attached macro) 를 호출하려면 매크로 이름 앞에 기호 (`@`) 를 작성하고 매크로 이름 뒤 소괄호에 인수를 작성합니다.

첨부 매크로는 첨부된 선언을 수정합니다. 새로운 메서드를 정의하거나 프로토콜의 준수성을 추가하는 것과 같이 해당 선언에 코드를 추가합니다.

예를 들어, 매크로를 사용하지 않는 다음의 코드를 살펴봅시다:

```swift
struct SundaeToppings: OptionSet {
    let rawValue: Int
    static let nuts = SundaeToppings(rawValue: 1 << 0)
    static let cherry = SundaeToppings(rawValue: 1 << 1)
    static let fudge = SundaeToppings(rawValue: 1 << 2)
}
```

이 코드에서 `SundaeToppings` 옵션 셋의 각 옵션은 반복적이고 수동적인 초기화 구문 호출을 포함합니다. 이러한 코드는 줄 끝에 잘못된 숫자를 입력하는 것과 같은 새로운 옵션을 추가할 때 실수하기 쉽습니다.

다음의 코드는 매크로를 사용한 버전입니다:

```swift
@OptionSet<Int>
struct SundaeToppings {
    private enum Options: Int {
        case nuts
        case cherry
        case fudge
    }
}
```

이 버전의 `SundaeToppings` 는 `@OptionSet` 매크로를 호출합니다. 이 매크로는 private 열거형에 케이스의 목록을 읽고 각 옵션에 대한 상수 목록을 생성하고 [`OptionSet`](https://developer.apple.com/documentation/swift/optionset) 프로토콜의 준수성을 추가합니다.

비교를 위해 `@OptionSet` 매크로의 확장 버전은 다음과 같습니다. 이 코드를 작성하지 않고 매크로의 확장을 보기위해 Swift 에 요청한 경우에만 볼 수 있습니다.

```swift
struct SundaeToppings {
    private enum Options: Int {
        case nuts
        case cherry
        case fudge
    }

    typealias RawValue = Int
    var rawValue: RawValue
    init() { self.rawValue = 0 }
    init(rawValue: RawValue) { self.rawValue = rawValue }
    static let nuts: Self = Self(rawValue: 1 << Options.nuts.rawValue)
    static let cherry: Self = Self(rawValue: 1 << Options.cherry.rawValue)
    static let fudge: Self = Self(rawValue: 1 << Options.fudge.rawValue)
}
extension SundaeToppings: OptionSet { }
```

private 열거형 뒤에 모든 코드는 `@OptionSet` 매크로에서 가져옵니다. static 변수를 생성하기 위해 매크로를 사용하는 `SundaeToppings` 의 버전은 이전의 수동으로 된 코드보다 더 읽기 쉽고 유지보수 하기도 더 쉽습니다.

## 매크로 선언 (Macro Declarations)

대부분 Swift 코드에서 함수 또는 타입과 같은 기호를 구현할 때, 별도의 선언이 없습니다. 그러나, 매크로의 경우 선언과 구현은 분리되어 있습니다. 매크로의 선언은 매크로의 이름, 매크로가 가지는 파라미터, 어디서 사용될 수 있는지, 어떤 코드를 생성하는지가 포함됩니다. 매크로의 구현은 Swift 코드를 생성하여 매크로를 확장하는 코드를 포함합니다.

매크로는 `macro` 키워드로 선언합니다. 예를 들어, 이전 예제에서 사용된 `@OptionSet` 매크로에 대한 선언의 일부분입니다:

```swift
public macro OptionSet<RawType>() =
        #externalMacro(module: "SwiftMacros", type: "OptionSetMacro")
```

첫 번째 줄은 매크로의 이름과 매크로의 인수를 지정합니다 - 이름은 `OptionSet` 이고 인수는 가지고 있지 않습니다. 두 번째 줄은 Swift 표준 라이브러리에 [`externalMacro(module:type:)`](<https://developer.apple.com/documentation/swift/externalmacro(module:type:)>) 매크로를 사용하여 Swift 에 매크로의 구현 위치를 알려줍니다. 이 경우에 `SwiftMacros` 모듈은 `@OptionSet` 매크로를 구현하는 `OptionSetMacro` 를 포함합니다.

`OptionSet` 은 첨부 매크로이므로, 매크로의 이름은 구조체와 클래스 이름처럼 대문자 카멜 케이스로 사용합니다. 독립 매크로는 변수와 함수 이름처럼 소문자 카멜 케이스로 이름을 가집니다.

> Note:
> 매크로는 항상 `public` 으로 선언됩니다. 매크로를 선언하는 코드는 매크로를 사용하는 코드의 모듈과 다르므로, public 이 아닌 매크로는 적용할 수 없습니다.

매크로 선언은 매크로가 호출될 수 있는 코드의 위치와 매크로가 생성할 수 있는 코드의 종류인 매크로의 _역할 (role)_ 을 정의합니다. 모든 매크로는 하나 이상의 역할을 가지고 있고 매크로 선언의 앞에 속성의 부분으로 작성합니다. 다음은 `@OptionSet` 에 대한 역할의 속성을 포함한 선언의 일부분을 나타냅니다:

```swift
@attached(member)
@attached(extension, conformances: OptionSet)
public macro OptionSet<RawType>() =
        #externalMacro(module: "SwiftMacros", type: "OptionSetMacro")
```

`@attached` 속성은 각 매크로 역할에 대해 한번씩 선언에서 두 번 나타납니다. 첫 번째는 `@attached(member)` 를 사용하고 매크로가 적용된 타입에 새로운 멤버를 추가한다고 나타냅니다. `@OptionSet` 매크로는 `OptionSet` 프로토콜에 의해 요구되는 `init(rawValue:)` 초기화 구문과 멤버를 추가합니다.
두 번째는 `@attached(extension, conformances: OptionSet)` 를 사용하고 `@OptionSet` 이 `OptionSet` 프로토콜의 준수성을 추가한다고 나타냅니다.
`@OptionSet` 매크로는 매크로를 적용한 타입을 확장하여 `OptionSet` 프로토콜의 준수성을 추가합니다.

독립 매크로에 대해 매크로의 역할을 지정하기 위해 `@freestanding` 속성을 작성합니다:

```
@freestanding(expression)
public macro line<T: ExpressibleByIntegerLiteral>() -> T =
        /* ... location of the macro implementation... */
```

위에 `#line` 매크로는 `expression` 역할을 가집니다. 매크로 표현식은 값을 생성하거나 컴파일 때 경고를 생성하듯이 어떠한 동작을 수행합니다.

매크로의 역할 외에도 매크로의 선언은 매크로가 생성하는 기호의 이름에 대한 정보를 제공합니다. 매크로 선언이 이름을 제공하면 해당 이름을 사용하는 선언만 생성하므로 생성된 코드는 이해하고 디버그 하는데 도움을 줍니다. 다음은 `@OptionSet` 선언의 전체입니다:

```swift
@attached(member, names: named(RawValue), named(rawValue),
        named(`init`), arbitrary)
@attached(extension, conformances: OptionSet)
public macro OptionSet<RawType>() =
        #externalMacro(module: "SwiftMacros", type: "OptionSetMacro")
```

위의 선언에서 `@attached(member)` 매크로는 `@OptionSet` 매크로가 생성하는 각 기호에 대해 `names:` 라벨 뒤에 인수로 포함합니다. 매크로는 `RawValue`, `rawValue`, 그리고 `init` 의 이름인 기호에 대한 선언을 추가합니다 - 이러한 이름은 미리 알고 있기 때문에, 명시적으로 매크로 선언에 나열합니다. 

매크로 선언은 매크로를 사용할 때까지 알 수 없는 이름의 선언을 생성하기위해 이름의 뒤에 `arbitrary` 도 포함합니다. 예를 들어, `@OptionSet` 매크로가 위에 `SundaeToppings` 에 적용하면, 열거형 케이스 인 `nuts`, `cherry`, 그리고 `fudge` 에 해당하는 타입 프로퍼티를 생성합니다.

매크로 역할의 종류와 더 자세한 내용은 [속성 (Attributes)](../language-reference/attributes.md) 에 [attached](../language-reference/attributes.md#attached) 와 [freestanding](../language-reference/attributes.md#freestanding) 을 참고 바랍니다.

## 매크로 확장 (Macro Expansion)

매크로를 사용하는 Swift 코드를 빌드할 때 컴파일러는 매크로의 구현을 확장하기 위해 호출합니다.

![Diagram showing the four steps of expanding macros.  The input is Swift source code.  This becomes a tree, representing the code's structure.  The macro implementation adds branches to the tree.  The result is Swift source with additional code.](../.gitbook/assets/macro-expansion-full~dark%402x.png)

특히, Swift 는 아래와 같은 방식으로 매크로를 확장합니다:

1. 컴파일러는 코드를 읽고 구문의 메모리 표현을 생성합니다.

2. 컴파일러는 메모리 표현의 일부분을 매크로 구현에 전송하여 매크로를 확장합니다.

3. 컴파일러는 확장된 형태로 매크로 호출을 대체합니다.

4. 컴파일러는 확장된 소스 코드를 사용하여 완료될 때까지 계속 진행합니다.

특정 단계를 진행하려면, 다음을 참고 바랍니다:

```
let magicNumber = #fourCharacterCode("ABCD")
```

`#fourCharacterCode` 매크로는 4글자의 문자열을 가지고 문자열의 ASCII 값에 해당하는 부호 없는 32비트 정수를 반환합니다. 일부 파일 형식은 압축되어 있지만 디버거에서 읽을 수 있기 때문에 데이터를 식별하기 위해 정수를 사용합니다. 아래 [매크로 구현 (Implementing a Macro)](#매크로-구현-implementing-a-macro) 섹션에서는 이러한 매크로를 어떻게 구현하는지 보여줍니다.

위 코드에서 매크로를 확장하기 위해, 컴파일러는 Swift 파일을 읽고 _추상 구문 트리 (abstract syntax tree)_ 또는 AST 라고 알려진 해당 코드의 메모리 표현을 생성합니다. AST 는 컴파일러나 매크로 구현과 같이 해당 구조와 상호작용하는 코드를 더 쉽게 작성하기 위해 코드의 구조를 명시적으로 만듭니다. 다음은 일부 상세정보를 단순화한 위 코드에 대한 AST 표현입니다.

![A tree diagram, with a constant as the root element.  The constant has a name, magic number, and a value.  The constant's value is a macro call.  The macro call has a name, fourCharacterCode, and arguments.  The argument is a string literal, ABCD.](../.gitbook/assets/macro-ast-original~dark%402x.png)

위 다이어그램은 이 코드의 구조가 메모리에서 어떻게 표현되는지 보여줍니다. AST 에서 각 요소는 소스코드의 일부분에 해당합니다. "상수 선언 (Constant declaration)" AST 요소는 상수 선언의 두 부분을 표현하는 두 개의 하위 요소를 가지고 있습니다: 두 부분은 상수의 이름과 상수의 값입니다. "매크로 호출 (Macro call)" 요소는 매크로의 이름과 매크로에 전달되는 인수의 목록을 표현하는 하위 요소들을 가지고 있습니다.

이 AST 의 구성의 부분으로, 컴파일러는 소스코드가 Swift 에 유효한지 확인합니다. 예를 들어, `#fourCharacterCode` 는 문자열이어야 한다는 하나의 인수를 가집니다. 정수 인수를 전달하거나 문자열 리터럴 끝에 쌍따옴표 (`"`) 를 빼먹으면, 에러가 발생합니다. 

컴파일러는 매크로를 호출하는 위치를 코드에서 찾고, 해당 매크로를 구현한 외부 바이너리를 로드합니다. 각 매크로 호출에 대해 컴파일러는 AST 의 부분을 매크로의 구현에 전달합니다. 다음은 AST 의 해당 부분을 표현합니다:

![A tree diagram, with a macro call as the root element.  The macro call has a name, fourCharacterCode, and arguments.  The argument is a string literal, ABCD.](../.gitbook/assets/macro-ast-input~dark%402x.png)

`#fourCharacterCode` 매크로의 구현은 매크로를 확장할 때 입력으로 이 부분의 AST 를 읽습니다. 매크로의 구현은 입력으로 받은 AST 에서만 동작합니다. 이 의미는 매크로는 앞, 뒤에 오는 코드에 관계없이 항상 같은 방식으로 확장합니다. 이 제한은 Swift 가 변경되지 않은 매크로 확장을 피할 수 있으므로, 매크로 확장을 더 쉽게 이해하도록 돕고, 코드 빌드 속도에 도움을 줍니다. Swift 는 매크로를 구현한 코드를 제한하여 매크로 작성자가 실수로 다른 입력을 읽는 것을 방지합니다:

- 매크로 구현에 전달된 AST 는 매크로를 표현하는 AST 요소만 포함하고 앞 또는 뒤에 오는 코드를 포함하지 않습니다.

- 매크로 구현은 파일 시스템 또는 네트워크 접근을 방지하는 샌드박스 환경 (sandboxed environment) 에서 실행됩니다.

이러한 안전장치 외에도, 매크로의 작성자는 매크로의 입력 외에 항목을 읽거나 수정하지 않을 책임이 있습니다. 예를 들어, 매크로의 확장은 현재시간에 의존하지 않아야 합니다.

`#fourCharacterCode` 의 구현은 확장된 코드를 포함하는 새로운 AST 를 생성합니다. 다음은 코드가 컴파일러에 무엇을 반환하는지 나타냅니다:

![A tree diagram with the integer literal 1145258561 of type UInt32.](../.gitbook/assets/macro-ast-output~dark%402x.png)

컴파일러가 이 확장을 받으면, 매크로 호출을 포함하는 AST 요소를 매크로의 확장이 포함된 요소로 대체합니다. 매크로 확장 후에, 컴파일러는 프로그램이 여전히 Swift 에 유효하고 모든 타입이 올바른지 다시 확인합니다. 그러면 컴파일 될 수 있는 최종 AST 가 생성됩니다.

![A tree diagram, with a constant as the root element.  The constant has a name, magic number, and a value.  The constant's value is the integer literal 1145258561 of type UInt32](../.gitbook/assets/macro-ast-result~dark%402x.png)


This AST corresponds to Swift code like this:

```
let magicNumber = 1145258561 as UInt32
```

이 예제에서, 입력 소스코드는 하나의 매크로만 가지지만, 실제 프로그램에서는 동일한 매크로의 여러 인스턴스와 다른 매크로에 대한 여러 호출이 있을 수 있습니다. 컴파일러는 한 번에 하나씩 매크로를 확장합니다.

하나의 매크로가 다른 매크로 안에서 나타나면, 외부 매크로가 먼저 확장됩니다 - 이렇게 되면 확장되기 전에 외부 매크로는 내부 매크로를 수정할 수 있습니다.

## 매크로 구현 (Implementing a Macro)

매크로를 구현하기 위해, 두 개의 구성요소를 만듭니다: 매크로 확장을 수행하는 타입과 API 로 노출하도록 매크로를 선언한 라이브러리 입니다. 매크로 구현은 매크로의 클라이언트 빌드의 부분으로 수행되기 때문에, 매크로와 해당 클라이언트를 함께 개발하는 경우에도 이러한 부분은 매크로를 사용하는 코드와 별개로 빌드됩니다.

Swift Package Manager 를 사용하여 새로운 매크로를 생성하기 위해, `swift package init --type macro` 를 수행합니다 - 이것은 매크로 구현과 선언에 대한 템플릿을 포함하여 몇 개의 파일을 생성합니다.

기존 프로젝트에 매크로를 추가하기 위해,
다음과 같이 `Package.swift` 파일에 처음을 수정합니다:

- `swift-tools-version` 에 Swift tools 버전을 5.9 이상으로 지정합니다.
- `CompilerPluginSupport` 모듈을 가져옵니다.
- `platforms` 목록에 최소 배포 타겟으로 macOS 10.15 를 포함합니다.

아래의 코드는 `Package.swift` 파일의 예제를 보여줍니다.

```swift
// swift-tools-version: 5.9

import PackageDescription
import CompilerPluginSupport

let package = Package(
    name: "MyPackage",
    platforms: [ .iOS(.v17), .macOS(.v13)],
    // ...
)
```

다음으로 기존 `Package.swift` 파일에
매크로 구현에 대한 타겟과
매크로 라이브러리에 대한 타겟을 추가합니다.
예를 들어,
해당 프로젝트와 일치하는 이름으로 변경하여
다음과 같이 추가할 수 있습니다:

```swift
targets: [
    // Macro implementation that performs the source transformations.
    .macro(
        name: "MyProjectMacros",
        dependencies: [
            .product(name: "SwiftSyntaxMacros", package: "swift-syntax"),
            .product(name: "SwiftCompilerPlugin", package: "swift-syntax")
        ]
    ),

    // Library that exposes a macro as part of its API.
    .target(name: "MyProject", dependencies: ["MyProjectMacros"]),
]
```

위 코드는 두 개의 타겟을 정의합니다: `MyProjectMacros` 는 매크로의 구현을 포함하고, `MyProject` 는 해당 매크로를 사용가능하도록 만듭니다.

매크로의 구현은 AST 를 사용하여 구조화된 방식으로 Swift 코드와 상호작용하기 위해 [SwiftSyntax](https://github.com/swiftlang/swift-syntax) 모듈을 사용합니다. Swift Package Manager 로 새로운 매크로 패키지를 생성하면, 생성된 `Package.swift` 파일은 자동으로 SwiftSyntax 에 대한 의존성을 추가합니다. 기존 프로젝트에 매크로를 추가하면, `Package.swift` 파일에 SwiftSyntax 에 대한 의존성을 추가합니다:

```swift
dependencies: [
    .package(url: "https://github.com/swiftlang/swift-syntax", from: "509.0.0")
],
```

위 코드에서 `some-tag` 에 대한 표시자를 사용할 SwiftSyntax 의 버전에 대한 Git 태그로 대체합니다. 

매크로의 역할에 따라, 매크로 구현이 준수하는 SwiftSystem 의 프로토콜이 있습니다. 예를 들어, 이전 섹선에 `#fourCharacterCode` 를 생각해 봅시다. 다음은 해당 매크로를 구현하는 구조체입니다: 

```swift
import SwiftSyntax
import SwiftSyntaxMacros

public struct FourCharacterCode: ExpressionMacro {
    public static func expansion(
        of node: some FreestandingMacroExpansionSyntax,
        in context: some MacroExpansionContext
    ) throws -> ExprSyntax {
        guard let argument = node.argumentList.first?.expression,
              let segments = argument.as(StringLiteralExprSyntax.self)?.segments,
              segments.count == 1,
              case .stringSegment(let literalSegment)? = segments.first
        else {
            throw CustomError.message("Need a static string")
        }

        let string = literalSegment.content.text
        guard let result = fourCharacterCode(for: string) else {
            throw CustomError.message("Invalid four-character code")
        }

        return "\(raw: result) as UInt32"
    }
}

private func fourCharacterCode(for characters: String) -> UInt32? {
    guard characters.count == 4 else { return nil }

    var result: UInt32 = 0
    for character in characters {
        result = result << 8
        guard let asciiValue = character.asciiValue else { return nil }
        result += UInt32(asciiValue)
    }
    return result
}
enum CustomError: Error { case message(String) }
```

이 매크로를 기존에 Swift Package Manager 프로젝트에 추가하는 경우에,
매크로 타겟에 대한 시작지점으로 역할하고
타겟을 정의하는 매크로의 목록의 타입을 추가합니다:

```swift
import SwiftCompilerPlugin

@main
struct MyProjectMacros: CompilerPlugin {
    var providingMacros: [Macro.Type] = [FourCharacterCode.self]
}
```

`#fourCharacterCode` 매크로는 표현식을 생성하는 독립 매크로이므로, 구현한 `FourCharacterCode` 타입은 `ExpressionMacro` 프로토콜을 준수합니다. `ExpressionMacro` 프로토콜은 AST 를 확장하는 `expansion(of:in:)` 메서드 인 하나의 요구사항을 가집니다. 매크로 역할과 해당 SwiftSystem 프로토콜의 목록은 [속성 (Attributes)](../language-reference/attributes.md) 에 [attached](../language-reference/attributes.md#attached) 와 [freestanding](../language-reference/attributes.md#freestanding) 을 참고 바랍니다.

`#fourCharacterCode` 매크로를 확장하기 위해, Swift 는 이 매크로를 사용하는 코드에 대한 AST 를 매크로 구현을 포함하는 라이브러리로 보냅니다. 라이브러리에서 Swift 는 `FourCharacterCode.expansion(of:in:)` 을 호출하고, AST 와 컨텍스트를 인수로 메서드에 전달합니다. `expansion(of:in:)` 의 구현은 `#fourCharacterCode` 에 인수로 전달된 문자열을 찾고, 부호없는 32-bit 정수 리터럴 값으로 계산합니다.

위 예제에서, 첫 번째 `guard` 블럭은 AST 에서 문자열 리터럴을 추출하고, `literalSegment` 에 해당 AST 요소를 할당합니다. 두 번째 `guard` 블럭에서 private `fourCharacterCode(for:)` 함수를 호출합니다. 해당 블럭들은 매크로가 올바르게 사용되지 않으면 에러가 발생합니다 - 에러 메세지는 잘못된 호출 부분에서 컴파일 에러가 됩니다. 예를 들어, `#fourCharacterCode("AB" + "CD")` 로 매크로를 호출하면 컴파일러는 "Need a static string." 이라는 에러가 발생합니다.

`expansion(of:in:)` 매크로는 AST 에서 표현식을 표현하는 SwiftSyntax 의 타입인 `ExprSyntax` 의 인스턴스를 반환합니다. 이 타입은 `StringLiteralConvertible` 프로토콜을 준수하기 때문에, 매크로 구현은 결과를 생성하기 위해 가벼운 구문 (lightweight syntax) 으로 문자열 리터럴을 사용합니다. 매크로 구현에서 반환하는 모든 SwiftSyntax 타입은 `StringLiteralConvertible` 을 준수하므로, 모든 종류의 매크로를 구현할 때 이 접근방식을 사용할 수 있습니다.

## 매크로 개발과 디버깅 (Developing and Debugging Macros)

매크로는 테스트를 사용하는 개발에 적합합니다: 외부상태에 의존하지 않고 외부상태에 변경없이 하나의 AST 를 다른 AST 로 변환합니다. 추가로, 테스트에 입력설정을 단순화하는 문자열 리터럴에서 구문 노드를 생성할 수 있습니다. 예상되는 값과 비교하기 위해 문자열을 가져오는 AST 의 `description` 프로퍼티를 읽을 수도 있습니다. 예를 들어, 다음은 이전 섹션에 `#fourCharacterCode` 매크로의 테스트 입니다: 

```swift
let source: SourceFileSyntax =
    """
    let abcd = #fourCharacterCode("ABCD")
    """

let file = BasicMacroExpansionContext.KnownSourceFile(
    moduleName: "MyModule",
    fullFilePath: "test.swift"
)

let context = BasicMacroExpansionContext(sourceFiles: [source: file])

let transformedSF = source.expand(
    macros:["fourCharacterCode": FourCharacterCode.self],
    in: context
)

let expectedDescription =
    """
    let abcd = 1145258561 as UInt32
    """

precondition(transformedSF.description == expectedDescription)
```

위 예제는 사전 조건을 사용하여 매크로를 테스트 하지만, 테스트 프레임워크를 사용할 수도 있습니다. 
