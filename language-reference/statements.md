# 구문 (Statements)

표현식을 그룹화하고 실행의 흐름을 제어합니다.

Swift 에서 간단한 구문 (simple statements), 컴파일러 제어 구문 (compiler control statements), 그리고 제어 흐름 구문 (control flow statements) 의 세가지 구문이 있습니다. 간단한 구문은 가장 일반적이고 표현식 또는 선언으로 구성됩니다. 컴파일러 제어 구문은 프로그램이 컴파일러의 동작 측면을 변경할 수 있고 조건부 컴파일러 및 라인 제어 구문을 포함할 수 있습니다.

제어 흐름 구문은 프로그램에서 실행의 흐름을 제어하기 위해 사용됩니다. Swift 에서 루프 구문 (loop statements), 분기 구문 (branch statements), 그리고 제어 전송 구문 (control transfer statements) 을 포함하는 제어 흐름 구문의 몇가지 타입이 있습니다. 루프 제어문은 반복적으로 실행되는 코드의 블럭을 허용하고, 분기 구문은 조건이 일치 할 때만 실행되도록 코드의 블럭을 허용하고, 제어 전송 구문은 코드가 실행되는 순서를 변경하는 방법을 제공합니다. 또한 Swift 는 범위를 도입하고 에러를 잡고 처리하는 `do` 구문과 현재 범위가 종료하기 직전에 정리 동작을 실행하는 `defer` 구문을 제공합니다.

세미콜론 (`;`) 은 모든 구문에 선택적으로 나타날 수 있고 같은 줄에 여러 구문을 구분하기 위해 사용될 수 있습니다.

> Grammar of a statement:
>
> *statement* → *expression* **`;`**_?_ \
> *statement* → *declaration* **`;`**_?_ \
> *statement* → *loop-statement* **`;`**_?_ \
> *statement* → *branch-statement* **`;`**_?_ \
> *statement* → *labeled-statement* **`;`**_?_ \
> *statement* → *control-transfer-statement* **`;`**_?_ \
> *statement* → *defer-statement* **`;`**_?_ \
> *statement* → *do-statement* **`;`**_?_ \
> *statement* → *compiler-control-statement* \
> *statements* → *statement* *statements*_?_

## 루프 구문 (Loop Statements)

루프 구문 (Loop statements) 은 루프에 지정한 조건에 따라 반복적으로 실행되는 코드의 블럭을 허용합니다. Swift 는 `for`-`in` 구문, `while` 구문, 그리고 `repeat`-`while` 구문과 같이 세가지 루프 구문을 가집니다.

루프 구문에서 제어 흐름은 `break` 구문과 `continue` 구문에 의해 변경될 수 있고 아래에 [중단 구문 (Break Statement)](statements.md#break-statement) 과 [계속 구문 (Continue Statement)](statements.md#continue-statement) 에 설명되어 있습니다.

> Grammar of a loop statement:
>
> *loop-statement* → *for-in-statement* \
> *loop-statement* → *while-statement* \
> *loop-statement* → *repeat-while-statement*

### For-In 구문 (For-In Statement)

`for`-`in` 구문은 [`Sequence`](https://developer.apple.com/documentation/swift/sequence) 프로토콜을 준수하는 콜렉션 또는 모든 타입에서 각 아이템에 대해 한번 실행되기 위해 코드의 블럭을 허용합니다.

`for`-`in` 구문은 다음의 형식을 가집니다:

```swift
for <#item#> in <#collection#> {
   <#statements#>
}
```

`makeIterator()` 메서드는 [`IteratorProtocol`](https://developer.apple.com/documentation/swift/iteratorprotocol) 프로토콜을 준수하는 타입인 반복기 타입의 값을 포함하기 위해 _콜렉션_ 표현식 (_collection_ expression) 에서 호출됩니다. 프로그램은 반복기에서 `next()` 메서드를 호출하여 루프를 실행합니다. 값이 `nil` 을 반환하지 않으면 _항목 (item)_ 패턴에 할당되고 프로그램은 _구문 (statements)_ 를 실행한 다음에 루프의 시작된 부분에서 계속 실행합니다. 그렇지 않으면 프로그램은 할당 또는 _구문 (statements)_ 실행을 수행하지 않고 `for`-`in` 구문 실행을 종료합니다.

> Grammar of a for-in statement:
>
> *for-in-statement* → **`for`** **`case`**_?_ *pattern* **`in`** *expression* *where-clause*_?_ *code-block*

### While 구문 (While Statement)

`while` 구문은 조건이 참으로 남아있는 한 반복적으로 실행되기 위해 코드의 블럭을 허용합니다.

`while` 구문은 다음의 형식을 가집니다:

```swift
while <#condition#> {
   <#statements#>
}
```

`while` 구문은 다음과 같이 실행됩니다:

1. _조건 (condition)_ 은 평가됩니다. `true` 이면 2번이 계속 실행됩니다. `false` 이면 프로그램은 `while` 구문 실행을 종료합니다.
2. 프로그램은 _구문 (statements)_ 을 실행하고 실행은 1번으로 돌아갑니다.

_조건 (condition)_ 의 값이 _구문 (statements)_ 이 실행되기 전에 평가되므로 `while` 구문의 _구문 (statements)_ 은 0번 또는 더 많이 실행될 수 있습니다.

_조건 (condition)_ 의 값은 타입 `Bool` 이거나 `Bool` 에 브릿지된 타입이어야 합니다. 조건은 [옵셔널 바인딩 (Optional Binding)](../language-guide-1/the-basics.md#optional-binding) 에서 설명한대로 옵셔널 바인딩 선언일 수도 있습니다.

> Grammar of a while statement:
>
> *while-statement* → **`while`** *condition-list* *code-block*
>
> *condition-list* → *condition* | *condition* **`,`** *condition-list* \
> *condition* → *expression* | *availability-condition* | *case-condition* | *optional-binding-condition*
>
> *case-condition* → **`case`** *pattern* *initializer* \
> *optional-binding-condition* → **`let`** *pattern* *initializer*_?_ | **`var`** *pattern* *initializer*_?_

### Repeat-While 구문 (Repeat-While Statement)

`repeat`-`while` 구문은 조건이 참인 경우에 한하여 한 번 이상 실행되기 위한 코드의 블럭을 허용합니다.

`repeat`-`while` 구문은 다음의 형식을 가집니다:

```swift
repeat {
   <#statements#>
} while <#condition#>
```

`repeat`-`while` 구문은 다음과 같이 실행됩니다:

1. 프로그램은 _구문 (statements)_ 을 실행하고 2번이 계속 실행됩니다.
2. _조건 (condition)_ 이 평가됩니다. `true` 이면 실행은 1번으로 돌아갑니다. `false` 이면 프로그램은 `repeat`-`while` 구문 실행이 종료됩니다.

_조건 (condition)_ 의 값이 평가되기 때문에 후에 _구문 (statements)_ 이 실행되고 `repeat`-`while` 구문에서 _구문 (statements)_ 은 적어도 한 번은 실행됩니다.

_조건 (condition)_ 의 값은 타입 `Bool` 이거나 `Bool` 로 브릿지된 타입이어야 합니다.

> Grammar of a repeat-while statement:
>
> *repeat-while-statement* → **`repeat`** *code-block* **`while`** *expression*

## 분기 구문 (Branch Statements)

분기 구문 (Branch statements) 은 프로그램이 하나 이상의 조건의 값에 따라 코드의 특정 부분을 실행하는 것을 허용합니다. 분기 구문에서 지정된 조건의 값은 프로그램 분기 방법 및 코드의 어떤 블럭이 실행되는지 제어합니다. Swift 는 `if` 구문, `guard` 구문, 그리고 `switch` 구문의 세가지 분기 구문을 가지고 있습니다.

`if` 구문 또는 `switch` 구문에서 제어 흐름은 `break` 구문에 의해 변경될 수 있고 아래 [중단 구문 (Break Statement)](statements.md#break-statement) 에 설명되어 있습니다.

> Grammar of a branch statement:
>
> *branch-statement* → *if-statement* \
> *branch-statement* → *guard-statement* \
> *branch-statement* → *switch-statement*

### If 구문 (If Statement)

`if` 구문은 하나 이상의 조건의 평가를 기반으로 코드 실행에 사용됩니다.

`if` 구문에는 두가지 기본 형식이 있습니다. 각 형식에서 여는 중괄호와 닫는 중괄호가 필요합니다.

첫번째 형식은 조건이 참이고 다음의 형식을 가지고 있을 때만 코드가 실행되도록 합니다:

```swift
if <#condition#> {
   <#statements#>
}
```

`if` 구문의 두번째 형식은 추가로 `else` 키워드로 도입된 _else 절 (else clause)_ 을 제공하고 조건이 참일 때 코드의 한 부분을 실행하고 동일한 조건이 거짓일 때 코드의 다른 부분을 실행하는데 사용됩니다. 하나의 else 절이 있는 경우에 `if` 구문은 다음의 형식을 가집니다:

```swift
if <#condition#> {
   <#statements to execute if condition is true#>
} else {
   <#statements to execute if condition is false#>
}
```

`if` 구문의 else 절은 둘 이상의 조건을 검사하기 위해 다른 `if` 구문을 포함할 수 있습니다. 이러한 방법으로 연결된 `if` 구문은 다음의 형식을 가집니다:

```swift
if <#condition 1#> {
   <#statements to execute if condition 1 is true#>
} else if <#condition 2#> {
   <#statements to execute if condition 2 is true#>
} else {
   <#statements to execute if both conditions are false#>
}
```

`if` 구문에서 모든 조건의 값은 타입 `Bool` 이거나 `Bool` 로 브릿지된 타입이어야 합니다. 조건은 [옵셔널 바인딩 (Optional Binding)](../language-guide-1/the-basics.md#optional-binding) 에서 설명한대로 옵셔널 바인딩 선언일 수도 있습니다.

> Grammar of an if statement:
>
> *if-statement* → **`if`** *condition-list* *code-block* *else-clause*_?_ \
> *else-clause* → **`else`** *code-block* | **`else`** *if-statement*

### Guard 구문 (Guard Statement)

`guard` 구문은 하나 이상의 조건이 충족되지 않을 때 범위 밖으로 프로그램 제어를 전송하기 위해 사용됩니다.

`guard` 구문은 다음의 형식을 가집니다:

```swift
guard <#condition#> else {
   <#statements#>
}
```

`guard` 구문의 모든 조건의 값은 타입 `Bool` 이거나 `Bool` 로 브릿지된 타입이어야 합니다. 조건은 [옵셔널 바인딩 (Optional Binding)](../language-guide-1/the-basics.md#optional-binding) 에서 설명한대로 옵셔널 바인딩 선언일 수도 있습니다.

`guard` 구문 조건에서 옵셔널 바인딩 선언으로 할당된 모든 상수 또는 변수 값은 guard 구문을 둘러싼 범위 내에서 사용될 수 있습니다.

`guard` 구문의 `else` 절은 필수이고 `Never` 반환 타입 또는 다음 구문 중 하나를 사용하여 guard 구문을 둘러싼 범위 밖으로 프로그램 제어를 전송해야 합니다:

* `return`
* `break`
* `continue`
* `throw`

제어 전송 구문은 아래 [제어 전송 구문 (Control Transfer Statements)](statements.md#control-transfer-statements) 에서 설명되어 있습니다. Never 반환 타입이 있는 함수에 자세한 내용은 [Never 반환 함수 (Functions that Never Return)](declarations.md#functions-that-never-return) 를 참고 바랍니다.

> Grammar of a guard statement:
>
> *guard-statement* → **`guard`** *condition-list* **`else`** *code-block*

### Switch 구문 (Switch Statement)

`switch` 구문은 제어 표현식의 값에 따라 특정 코드의 블럭이 실행됩니다.

`switch` 구문은 다음의 형식을 가집니다:

```swift
switch <#control expression#> {
case <#pattern 1#>:
    <#statements#>
case <#pattern 2#> where <#condition#>:
    <#statements#>
case <#pattern 3#> where <#condition#>,
    <#pattern 4#> where <#condition#>:
    <#statements#>
default:
    <#statements#>
}
```

`switch` 구문의 _제어 표현식 (control expression)_ 은 평가된 다음에 각 케이스에 지정한 패턴과 비교됩니다. 일치하는 항목이 있으면 프로그램은 해당 케이스의 범위내에 _구문 (statements)_ 을 실행합니다. 각 케이스의 범위는 비어 있을 수 없습니다. 결과적으로 각 케이스 라벨의 콜론 (`:`) 다음에 적어도 하나의 구문이 포함되어야 합니다. 일치하는 케이스의 본문에서 코드를 실행하지 않으려면 단일 `break` 구문을 사용해야 합니다.

코드가 분기할 수 있는 표현식의 값은 매우 유연합니다. 예를 들어 정수와 문자와 같은 스칼라 타입의 값을 제외하고 코드는 부동 소수점 숫자, 문자열, 튜플, 사용자 정의 클래스의 인스턴스, 그리고 옵셔널을 포함하는 모든 타입의 값으로 분기할 수 있습니다. _제어 표현식 (control expression)_ 의 값은 열거형의 케이스의 값과 일치하고 지정된 값 범위에 포함되는지 확인할 수도 있습니다. `switch` 구문에서 값의 이러한 여러가지 타입을 어떻게 사용하는지에 대한 예제는 [제어 흐름 (Control Flow)](../language-guide-1/control-flow.md) 에 [Switch](../language-guide-1/control-flow.md#switch) 를 참고 바랍니다.

`switch` 케이스는 각 패턴 후에 선택적으로 `where` 절을 포함할 수 있습니다. _where 절 (where clause)_ 은 `where` 키워드 다음에 표현식을 붙여 도입되고 케이스에 패턴이 _제어 표현식 (control expression)_ 에 일치되는지 간주되기 전에 추가 조건을 제공하는데 사용됩니다. `where` 절이 있는 경우 연관된 케이스 내에 _구문 (statements)_ 은 _제어 표현식 (control expression)_ 의 값이 케이스의 패턴 중 하나와 일치하고 `where` 절의 표현식이 `true` 로 평가될 때만 실행됩니다. 예를 들어 _제어 표현식 (control expression)_ 은 `(1, 1)` 과 같이 동일한 값의 두 요소가 포함된 튜플인 경우에만 아래 예제의 케이스는 일치됩니다.

```swift
case let (x, y) where x == y:
```

위의 예제에서 볼 수 있듯이 케이스의 패턴은 `let` 키워드를 사용하여 상수에 바인딩 할 수 있고 `var` 키워드를 사용하여 변수에 바인딩 할 수도 있습니다. 이 상수 또는 변수는 해당 where 절과 케이스의 범위 내의 나머지 코드에서 참조될 수 있습니다. 케이스에 제어 표현식과 일치하는 여러 패턴을 포함하는 경우 모든 패턴에는 동일한 상수 또는 변수 바인딩이 포함되어야 하고 바인딩 된 변수 또는 상수는 케이스의 모든 패턴에서 동일한 타입을 가져야 합니다.

`switch` 구문은 `default` 키워드로 도입되는 기본 케이스를 포함할 수도 있습니다. 기본 케이스 내에 코드는 제어 표현식이 일치하는 케이스가 없는 경우에만 실행됩니다. `switch` 구문은 오직 하나의 기본 케이스만 포함되고 `switch` 구문의 마지막에 위치해야 합니다.

패턴-일치 동작 (pattern-matching operations) 의 실제 실행 순서, 그리고 특별히 케이스의 패턴의 평가 순서가 지정되지 않았지만 `switch` 구문의 패턴 일치는 소스 코드가 나타나는 순서대로 소스 순서대로 평가가 수행되도록 동작합니다. 결과적으로 여러 케이스에 동일한 값으로 평가되는 패턴이 포함되어 제어 표현식의 값과 일치할 수 있는 경우 프로그램은 소스 순서대로 일치하는 첫번째 케이스 내의 코드만 실행합니다.

#### Switch 구문은 완벽해야 함 (Switch Statements Must Be Exhaustive)

Swift 에서 제어 표현식의 타입에서 가능한 모든 값은 적어도 하나의 케이스의 패턴의 값과 일치해야 합니다. 이것이 가능하지 않은 경우 (예를 들어 제어 표현식의 타입이 `Int` 인 경우) 요구사항을 충족하기 위해 기본 케이스를 포함할 수 있습니다.

#### 향후 열거형 케이스 전환 (Switching Over Future Enumeration Cases)

_비고정 열거형 (nonfrozen enumeration)_ 은 앱을 컴파일하고 출시한 후에도 나중에 새로운 열거형 케이스를 얻을 수 있는 특별한 종류의 열거형입니다. 비고정 열거형을 전환하려면 추가 고려사항이 필요합니다. 라이브러리 작성자가 비고정으로 열거형을 표시할 때 새로운 열거형 케이스를 추가할 권한이 있으며 해당 열거형과 상호작용하는 모든 코드는 다시 컴파일되지 않고 향후 케이스를 처리할 수 있습니다. 라이브러리 진화 모드에서 컴파일된 코드, Swift 표준 라이브러리의 코드, Apple 프레임워크용 Swift 오버레이, 그리고 C 그리고 Objective-C 코드는 비고정 열거형을 선언할 수 있습니다. 고정과 비고정 열거형에 대한 자세한 내용은 [고정 (frozen)](attributes.md#frozen) 을 참고 바랍니다.

비고정 열거형 값을 변경할 때 열거형의 모든 케이스가 해당 전환 케이스가 있더라도 기본 케이스를 포함해야 합니다.향후에 추가된 열거형 케이스에만 일치되는 기본 케이스를 나타내기 위해 기본 케이스에 `@unknown` 속성을 적용할 수 있습니다. Swift 는 컴파일 시에 모든 열거형 케이스와 일치하는 기본 케이스가 있다면 경고를 발생시킵니다. 향후 이 경고는 라이브러리 작성자가 해당 스위치 케이스가 없는 열거형에 새로운 케이스를 추가했음을 알려줍니다.

다음 예제는 Swift 표준 라이브러리의 [`Mirror.AncestorRepresentation`](https://developer.apple.com/documentation/swift/mirror/ancestorrepresentation) 열거형의 세가지 기존 케이스를 모두 전환합니다. 향후에 추가 케이스를 추가하면 컴파일러는 새로운 케이스를 고려하기 위해 switch 구문을 업데이트 해야 한다고 나타내는 경고를 생성합니다.

```swift
let representation: Mirror.AncestorRepresentation = .generated
switch representation {
case .customized:
    print("Use the nearest ancestor’s implementation.")
case .generated:
    print("Generate a default mirror for all ancestor classes.")
case .suppressed:
    print("Suppress the representation of all ancestor classes.")
@unknown default:
    print("Use a representation that was unknown when this code was compiled.")
}
// Prints "Generate a default mirror for all ancestor classes."
```

#### 실행이 암시적으로 케이스를 통과하지 않음 (Execution Does Not Fall Through Cases Implicitly)

일치한 케이스 내에 코드 실행이 완료되면 프로그램은 `switch` 구문을 종료합니다. 프로그램 실행이 계속되지 않거나 다음 케이스 또는 기본 케이스로 "이동 (fall through)" 되지 않습니다. 이 말은 한 케이스에서 다음 케이스로 계속해서 실행을 하려면 계속해서 실행되기 원하는 케이스에 간단하게 `fallthrough` 키워드로 구성된 `fallthrough` 구문을 명시적으로 포함해야 합니다. `fallthrough` 구문에 대한 자세한 내용은 아래의 [이동 구문 (Fallthrough Statement)](statements.md#fallthrough-statement) 을 참고 바랍니다.

> Grammar of a switch statement:
>
> *switch-statement* → **`switch`** *expression* **`{`** *switch-cases*_?_ **`}`** \
> *switch-cases* → *switch-case* *switch-cases*_?_ \
> *switch-case* → *case-label* *statements* \
> *switch-case* → *default-label* *statements* \
> *switch-case* → *conditional-switch-case*
>
> *case-label* → *attributes*_?_ **`case`** *case-item-list* **`:`** \
> *case-item-list* → *pattern* *where-clause*_?_ | *pattern* *where-clause*_?_ **`,`** *case-item-list* \
> *default-label* → *attributes*_?_ **`default`** **`:`**
>
> *where-clause* → **`where`** *where-expression* \
> *where-expression* → *expression*
>
> *conditional-switch-case* → *switch-if-directive-clause* *switch-elseif-directive-clauses*_?_ *switch-else-directive-clause*_?_ *endif-directive* \
> *switch-if-directive-clause* → *if-directive* *compilation-condition* *switch-cases*_?_ \
> *switch-elseif-directive-clauses* → *elseif-directive-clause* *switch-elseif-directive-clauses*_?_ \
> *switch-elseif-directive-clause* → *elseif-directive* *compilation-condition* *switch-cases*_?_ \
> *switch-else-directive-clause* → *else-directive* *switch-cases*_?_

## 라벨 구문 (Labeled Statement)

접두사로 루프 구문, `if` 구문, `switch` 구문, 또는 `do` 구문을 라벨의 이름 뒤에 콜론 (`:`) 으로 구성된 _구문 라벨 (statement label)_ 을 붙일 수 있습니다. 아래 [중단 구문 (Break Statement)](statements.md#break-statement) 과 [계속 구문 (Continue Statement)](statements.md#continue-statement) 에서 설명한대로 루프 구문 또는 `switch` 구문에서 제어 흐름을 변경하려는 방법을 명시하려면 `break` 와 `continue` 구문과 함께 구문 라벨을 사용해야 합니다.

라벨 구문의 범위는 구문 라벨 다음에 오는 전체 구문입니다. 라벨 구문을 중첩할 수 있지만 각 구문 라벨의 이름은 고유해야 합니다.

구문 라벨을 어떻게 사용하는지 자세한 내용과 예제는 [제어 흐름 (Control Flow)](../language-guide-1/control-flow.md) 에 [라벨 구문 (Labeled Statements)](../language-guide-1/control-flow.md#labeled-statements) 을 참고 바랍니다.

> Grammar of a labeled statement:
>
> *labeled-statement* → *statement-label* *loop-statement* \
> *labeled-statement* → *statement-label* *if-statement* \
> *labeled-statement* → *statement-label* *switch-statement* \
> *labeled-statement* → *statement-label* *do-statement*
>
> *statement-label* → *label-name* **`:`** \
> *label-name* → *identifier*

## 제어 전송 구문 (Control Transfer Statements)

제어 전송 구문 (Control transfer statements) 은 프로그램 제어를 한 코드에서 다른 코드로 무조건 전송하여 프로그램의 코드가 실행되는 순서를 변경할 수 있습니다. Swift 는 `break` 구문, `continue` 구문, `fallthrough` 구문, `return` 구문, 그리고 `throw` 구문의 다섯가지 제어 전송 구문이 있습니다.

> Grammar of a control transfer statement:
>
> *control-transfer-statement* → *break-statement* \
> *control-transfer-statement* → *continue-statement* \
> *control-transfer-statement* → *fallthrough-statement* \
> *control-transfer-statement* → *return-statement* \
> *control-transfer-statement* → *throw-statement*

### Break 구문 (Break Statement)

`break` 구문은 루프, `if` 구문, 또는 `switch` 구문의 프로그램 실행을 종료합니다. `break` 구문은 아래에서 본대로 `break` 키워드로만 구성되거나 `break` 키워드 다음에 구문 라벨의 이름으로 구성될 수 있습니다.

```swift
break
break <#label name#>
```

`break` 구문 다음에 구문 라벨의 이름이 오면 라벨로 명명된 루프, `if` 구문, 또는 `switch` 구문의 프로그램 실행을 종료합니다.

`break` 구문 다음에 구문 라벨의 이름이 없으면 `switch` 구문 또는 해당 구문이 발생한 가장 안쪽의 루프 구문의 프로그램 실행이 종료됩니다. `if` 구문을 벗어나기 위해 라벨이 없는 `break` 구문을 사용할 수 없습니다.

두 경우 모두 프로그램 제어는 루프나 `switch` 구문 다음 코드의 첫번째 줄로 전송됩니다.

break 구문을 어떻게 사용하는지에 대한 예제는 [제어 흐름 (Control Flow)](../language-guide-1/control-flow.md) 에 [Break](../language-guide-1/control-flow.md#break) 와 [라벨 구문 (Labeled Statements)](../language-guide-1/control-flow.md#labeled-statements) 을 참고 바랍니다.

> Grammar of a break statement:
>
> *break-statement* → **`break`** *label-name*_?_

### Continue 구문 (Continue Statement)

`continue` 구문은 루프 구문의 현재 반복의 프로그램 실행을 종료하지만 루프 구문의 실행을 멈추지 않습니다. `continue` 구문은 아래에서 보는대로 `continue` 키워드로만 구성되거나 `continue` 키워드 다음에 구문 라벨의 이름으로 구성될 수 있습니다.

```swift
continue
continue <#label name#>
```

`continue` 구문 다음에 구문 라벨의 이름이 따라오면 라벨로 명명된 루프 구문의 현재 반복의 프로그램 실행을 종료합니다.

`continue` 구문 다음에 구문 라벨의 이름이 따라오지 않으면 해당 구문이 발생한 가장 안쪽의 루프 구문의 현재 반복의 프로그램 실행이 종료됩니다.

두 경우 모두 프로그램 제어는 둘러싸인 루프 구문의 조건으로 전송됩니다.

`for` 구문에서 증가 표현식은 루프의 본문이 실행된 후에 평가되므로 `continue` 구문 후에도 계속 계산됩니다.

`continue` 구문 사용에 대한 예제는 [제어 흐름 (Control Flow)](../language-guide-1/control-flow.md) 에 [Continue](../language-guide-1/control-flow.md#continue) 과 [라벨 구문 (Labeled Statements)](../language-guide-1/control-flow.md#labeled-statements) 을 참고 바랍니다.

> Grammar of a continue statement:
>
> *continue-statement* → **`continue`** *label-name*_?_

### Fallthrough 구문 (Fallthrough Statement)

`fallthrough` 구문은 `fallthrough` 키워드로 구성되고 `switch` 구문의 케이스 블럭에서만 발생합니다. `fallthrough` 구문은 `switch` 구문의 한 케이스에서 다음 케이스로 계속해서 프로그램이 실행되도록 합니다. `switch` 구문의 제어 표현식의 값이 케이스 라벨의 패턴과 일치하지 않아도 다음 케이스는 계속 실행됩니다.

`fallthrough` 구문은 케이스 블럭의 마지막 구문 뿐만 아니라 `switch` 구문 내 어디든 나타날 수 있지만 마지막 케이스 블럭에서는 사용될 수 없습니다. 또한 패턴에 값 바인딩 패턴이 포함된 케이스 블럭으로 제어를 전송할 수 없습니다.

`switch` 구문에서 `fallthrough` 구문을 사용하는 방법에 대한 예제는 [제어 흐름 (Control Flow)](../language-guide-1/control-flow.md) 에 [제어 전송 구문 (Control Transfer Statements)](../language-guide-1/control-flow.md#control-transfer-statements) 을 참고 바랍니다.

> Grammar of a fallthrough statement:
>
> *fallthrough-statement* → **`fallthrough`**

### Retrun 구문 (Return Statement)

`return` 구문은 함수 또는 메서드 정의의 본문에서 발생하고 호출한 함수 또는 메서드를 반환하기 위해 실행합니다. 함수 또는 메서드 호출 한 곳에서 계속해서 실행됩니다.

`return` 구문은 아래에 보는대로 `return` 키워드만으로 구성되거나 `return` 키워드 다음에 표현식으로 구성될 수 있습니다.

```swift
return
return <#expression#>
```

`return` 구문 다음에 표현식이 오면 표현식의 값은 호출한 함수 또는 메서드에 반환됩니다. 표현식의 값이 함수 또는 메서드 선언에 선언된 반환 타입의 값과 일치하지 않으면 표현식의 값은 호출한 함수 또는 메서드에 반환되기 전에 반환 타입을 변환합니다.

> Note\
> [실패 가능한 초기화 구문 (Failable Initializers)](declarations.md#failable-initializers) 에서 설명한대로 `return` 구문의 특수한 형식 (`return nil`) 은 초기화 실패를 나타내기 위해 실패 가능한 초기화 구문에서 사용될 수 있습니다.

`return` 구문 다음에 표현식이 없으면 반환값이 없는 함수 또는 메서드에 반환하기 위해서만 사용될 수 있습니다 (즉, 함수 또는 메서드의 반환 타입이 `Void` 또는 `()` 인 경우에 해당됩니다).

> Grammar of a return statement:
>
> *return-statement* → **`return`** *expression*_?_

### Throw 구문 (Throw Statement)

`throw` 구문은 던지는 함수 또는 메서드의 본문에서 발생하거나 타입에 `throws` 키워드가 표시된 클로저 표현식의 본문에서 발생합니다.

`throw` 구문은 프로그램이 현재 범위의 실행을 종료하고 둘러싸인 범위로 에러 전파를 시작합니다. 던져진 에러는 `do` 구문의 `catch` 절에 의해 처리될 때까지 계속 전파됩니다.

`throw` 구문은 아래에서 보는대로 `throw` 키워드 다음에 표현식으로 구성됩니다.

```swift
throw <#expression#>
```

_표현식 (expression)_ 의 값은 `Error` 프로토콜을 준수하는 타입이어야 합니다.
`do` 구문이나 `throw` 구문을 포함하는 함수에
발생하는 에러의 타입을 선언하면,
*표현식* 의 값은 해당 타입의 인스턴스여야 합니다.

`throw` 구문을 어떻게 사용하는지에 대한 예제는 [에러 처리 (Error Handling)](../language-guide-1/error-handling.md) 에 [던지기 함수를 이용한 에러 전파 (Propagating Errors Using Throwing Functions)](../language-guide-1/error-handling.md#propagating-errors-using-throwing-functions) 를 참고 바랍니다.

> Grammar of a throw statement:
>
> *throw-statement* → **`throw`** *expression*

## Defer 구문 (Defer Statement)

`defer` 구문은 `defer` 구문이 나타나는 범위의 바깥으로 프로그램 제어가 전송되기 직전에 실행되는 코드에 사용됩니다.

`defer` 구문은 다음의 형식을 가집니다:

```swift
defer {
    <#statements#>
}
```

`defer` 구문 내에 구문은 프로그램 제어가 전송되는 것과 관계없이 실행됩니다. 이것은 예를 들어 `defer` 구문은 파일 닫기와 같은 수동 리소스 관리를 수행하고 에러가 발생하더라도 수행되어야 하는 작업이 있는 경우에 사용될 수 있습니다.

`defer` 구문은 `defer` 구문의 끝에서 부터 실행됩니다.

```swift
func f(x: Int) {
  defer { print("First defer") }

  if x < 10 {
    defer { print("Second defer") }
    print("End of if")
  }

  print("End of function")
}
f(x: 5)
// Prints "End of if"
// Prints "Second defer"
// Prints "End of function"
// Prints "First defer"
```

위의 코드에서 `if` 구문 안에 `defer` 는 `if` 구문이 함수의 범위보다 먼저 끝나기 때문에 함수 `f` 안에 선언된 `defer` 전에 실행됩니다.

같은 범위안에 여러개의 `defer` 구문이 있다면 나타나는 순서의 역순으로 실행됩니다. 범위의 마지막 `defer` 구문을 먼저 실행한다는 것은 다른 `defer` 구문에 의해 정리될 리소스를 참조할 수 있다는 의미입니다.

```swift
func f() {
    defer { print("First defer") }
    defer { print("Second defer") }
    print("End of function")
}
f()
// Prints "End of function"
// Prints "Second defer"
// Prints "First defer"
```

`defer` 구문안에 구문은 `defer` 구문의 밖으로 프로그램 제어를 전송할 수 없습니다.

> Grammar of a defer statement:
>
> *defer-statement* → **`defer`** *code-block*

## Do 구문 (Do Statement)

`do` 구문은 새로운 범위를 도입하기 위해 사용되고 정의된 에러 조건과 일치하는 패턴을 포함하는 하나 이상의 `catch` 절을 선택적으로 포함할 수 있습니다. `do` 구문의 범위에서 선언된 변수와 상수는 해당 범위 내에서만 접근할 수 있습니다.

Swift 에서 `do` 구문은 코드 블럭을 구분하는데 사용되는 C 의 중괄호 (`{}`) 와 유사하며 런타임시 성능 비용이 발생하지 않습니다.

`do` 구문은 다음의 형식을 가집니다:

```swift
do {
    try <#expression#>
    <#statements#>
} catch <#pattern 1#> {
    <#statements#>
} catch <#pattern 2#> where <#condition#> {
    <#statements#>
} catch <#pattern 3#>, <#pattern 4#> where <#condition#> {
    <#statements#>
} catch {
    <#statements#>
}
```

`do` 구문은 선택적으로 아래와 같은 형식을 가지는
에러의 타입을 지정할 수 있습니다:

```swift
do throws(<#type#>) {
    try <#expression#>
} catch <#pattern> {
    <#statements#>
} catch {
    <#statements#>
}
```

`do` 구문이 `throws` 구절을 포함하면,
`do` 본문은 지정된 *타입* 의 에러만 던질 수 있습니다.
*타입*은
`Error` 프로토콜을 준수하는 타입이거나,
`Error` 프로토콜을 준수하는 불투명한 타입,
또는 `any Error` 의 박스형 프로토콜이어야 합니다.
`do` 구문은 에러의 타입을 지정하지 않으면,
Swift 는 다음과 같이 에러 타입을 추론합니다:

- `do` 코드 본문에 모든 `throws` 구문과 `try` 표현식이
  에러 처리 메커니즘으로 완벽하게 처리되면,
  Swift 는 `do` 구문이 던지지 않는다고 추론합니다.

- `do` 코드 본문이 `Never` 를 던지는 것이 아니라
  하나의 에러 타입만을 던지는 코드가 포함되어 있는 경우,
  Swift 는 `do` 구문이 구체적인 에러 타입을 던진다고 추론합니다.

- `do` 코드 본문이 하나 이상의 에러 타입을 던지는 경우,
  Swift 는 `do` 구문은 `any Error` 를 던진다고 추론합니다.

명확한 타입을 가지는 에러 동작의 자세한 내용은,
[에러 타입 지정 (Specifying the Error Type)](../language-guide-1/error-handling.md#에러-타입-지정-specifying-the-error-type) 을 참고 바랍니다.

`do` 코드 블럭의 구문에서 에러가 발생하면 프로그렘 제어는 패턴이 에러와 일치하는 첫번째 `catch` 절로 전송됩니다. 일치하는 절이 없으면 에러를 주변 범위로 전파합니다. 에러를 최상위 수준에서 처리되지 않으면 프로그램 실행은 런타임 에러와 함께 멈춥니다.

`switch` 구문처럼 컴파일러는 `catch` 절이 완벽한지 여부를 추론하려고 합니다. 그러한 결정을 내릴 수 있으면 에러가 처리된 것으로 간주됩니다. 그렇지 않으면 에러는 포함된 범위 밖으로 전파될 수 있습니다. 이것은 에러는 `catch` 절에서 처리되거나 포함한 함수가 `throws` 로 선언되어야 한다는 의미입니다.

여러 패턴을 가지는 `catch` 절은 패턴 중 하나가 일치하는 경우에 에러와 일치합니다. `catch` 절에 여러 패턴을 포함하는 경우 모든 패턴은 동일한 상수 또는 변수 바인딩이 포함되어야 하며 각 바인딩 된 변수 또는 상수는 모든 `catch` 절의 패턴에서 동일한 타입을 가져야 합니다.

에러가 처리되도록 보장하려면 와일드카드 패턴 (`_`) 과 같이 모든 에러를 일치하는 패턴으로 `catch` 절을 사용해야 합니다. `catch` 절에 패턴을 지정하지 않으면 `catch` 절은 모든 에러와 일치하고 `error` 라는 이름의 지역 상수에 바인딩 됩니다. `catch` 절에서 사용할 수 있는 패턴에 대한 자세한 내용은 [패턴 (Patterns)](patterns.md) 을 참고 바랍니다.

여러 `catch` 절과 `do` 구문을 사용하는 방법에 대한 예제는 [에러 처리 (Handling Errors)](../language-guide-1/error-handling.md#handling-errors) 를 참고 바랍니다.

> Grammar of a do statement:
>
> *do-statement* → **`do`** *throws-clause*_?_ *code-block* *catch-clauses*_?_ \
> *catch-clauses* → *catch-clause* *catch-clauses*_?_ \
> *catch-clause* → **`catch`** *catch-pattern-list*_?_ *code-block* \
> *catch-pattern-list* → *catch-pattern* | *catch-pattern* **`,`** *catch-pattern-list* \
> *catch-pattern* → *pattern* *where-clause*_?_

## 컴파일러 제어 구문 (Compiler Control Statements)

컴파일러 제어 구문은 컴파일러의 동작 측면을 변경할 수 있습니다. Swift 는 조건부 컴파일 블럭 (conditional compilation block), 라인 제어 구문 (line control statement), 그리고 컴파일-시간 진단 구문 (compile-time diagnostic statement) 의 세가지 컴파일러 제어 구문을 가지고 있습니다.

> Grammar of a compiler control statement:
>
> *compiler-control-statement* → *conditional-compilation-block* \
> *compiler-control-statement* → *line-control-statement* \
> *compiler-control-statement* → *diagnostic-statement*

### 조건부 컴파일 블럭 (Conditional Compilation Block)

조건부 컴파일 블럭 (conditional compilation block) 은 하나 이상의 컴파일러 조건의 값에 따라 코드를 조건부로 컴파일 할 수 있습니다.

모든 조건부 컴파일 블럭은 `#if` 컴파일 지시문으로 시작하고 `#endif` 컴파일 지시문으로 끝납니다. 간단한 조건부 컴파일 블럭은 다음의 형식을 가집니다:

```swift
#if <#compilation condition#>
    <#statements#>
#endif
```

`if` 구문의 조건과 다르게 _컴파일러 조건 (compilation condition)_ 은 컴파일 시에 평가됩니다. 결과적으로 _구문 (statements)_ 은 컴파일 시에 _컴파일러 조건 (compilation condition)_ 이 `true` 로 평가될 때만 컴파일되고 실행됩니다.

_컴파일러 조건 (compilation condition)_ 은 `true` 와 `false` 불린 리터럴 (Boolean literals), `-D` 명령줄 플래그를 사용한 식별자, 또는 아래 표에 나열된 플랫폼 조건이 포함될 수 있습니다.

|         플랫폼 조건        |                         유효 인수                         |
| :-------------------: | :---------------------------------------------------: |
|         `os()`        | `macOS`, `iOS`, `watchOS`, `tvOS`, `visionOS`, `Linux`, `Windows` |
|        `arch()`       |            `i386`, `x86_64`, `arm`, `arm64`           |
|       `swift()`       |                 `>=` 또는 `<` 다음에 버전 번호                 |
|      `compiler()`     |                 `>=` 또는 `<` 다음에 버전 번호                 |
|     `canImport()`     |                         모듈 이름                         |
| `targetEnvironment()` |               `simulator`, `macCatalyst`              |

`swift()` 와 `compiler()` 플랫폼 조건에 대한 버전 번호는 버전 번호의 각 부분을 점 (`.`) 으로 구분하여 메이저 번호, 선택적으로 마이너 번호, 선택적으로 패치 번호, 등으로 구성됩니다. 비교 연산자와 버전 번호 사이에 공백이 없어야 합니다. `compiler()` 에 대한 버전은 컴파일러에 전달된 Swift 버전 설정과 상관없이 컴파일러 버전입니다. swift() 에 대한 버전은 현재 컴파일 되는 언어 버전입니다. 예를 들어 Swift 4.2 모드에서 Swift 5 컴파일러를 사용하여 코드를 컴파일 한다면 컴파일러 버전은 5 이고 언어 버전은 4.2 입니다. 이러한 설정으로 다음의 코드는 세가지 메세지를 모두 출력합니다:

```swift
#if compiler(>=5)
print("Compiled with the Swift 5 compiler or later")
#endif
#if swift(>=4.2)
print("Compiled in Swift 4.2 mode or later")
#endif
#if compiler(>=5) && swift(<5)
print("Compiled with the Swift 5 compiler or later in a Swift mode earlier than 5")
#endif
// Prints "Compiled with the Swift 5 compiler or later"
// Prints "Compiled in Swift 4.2 mode or later"
// Prints "Compiled with the Swift 5 compiler or later in a Swift mode earlier than 5"
```

`canImport()` 플랫폼 조건의 인수는 모든 플랫폼에 존재하지 않을 수 있는 모듈의 이름입니다. 모듈은 이름에 마침표 (`.`)를 포함할 수 있습니다. 이 조건은 해당 모듈을 가져올 수 있는지 검사하지만 실제로 가져오지 않습니다. 모듈이 있으면 플랫폼 조건은 `true` 를 반환하고 그렇지 않으면 `false` 를 반환합니다.

`targetEnvironment()` 플랫폼 조건은 지정한 환경에 대해 코드가 컴파일 되면 `true` 를 반환하고 그렇지 않으면 `false` 를 반환합니다.

> Note\
> `arch(arm)` 플랫폼 조건은 ARM 64 기기에 대해 `true` 를 반환하지 않습니다. `arch(i386)` 플랫폼 조건은 32-비트 iOS 시뮬레이터에 대해 코드가 컴파일 될 때 `true` 를 반환합니다.

논리 연산자 `&&`, `||`, 그리고 `!` 사용하여 컴파일러 조건을 결합하고 부정할 수 있고 그룹화를 위한 소괄호를 사용할 수 있습니다. 이러한 연산자는 부울 표현식을 결합하는데 사용되는 논리 연산자와 동일한 연관성과 우선순위를 갖습니다.

`if` 구문과 유사하게 다른 컴파일러 조건에 대해 검사하기 위해 여러 조건부 분기를 추가할 수 있습니다. `#elseif` 절을 사용하여 원하는 만큼 추가 분기를 추가할 수 있습니다. `#else` 절을 사용하여 마지막 추가 분기를 추가할 수도 있습니다. 여러개 분기를 포함하는 조건부 컴파일러 블럭 형식은 다음과 같습니다:

```swift
#if <#compilation condition 1#>
    <#statements to compile if compilation condition 1 is true#>
#elseif <#compilation condition 2#>
    <#statements to compile if compilation condition 2 is true#>
#else
    <#statements to compile if both compilation conditions are false#>
#endif
```

> Note\
> 조건부 컴파일러 블럭의 본문에서 각 구문은 컴파일 되지 않아도 구문 분석됩니다. 그러나 컴파일러 조건이 swift() 또는 compiler() 플랫폼 조건을 포함하면 예외가 있습니다: 이 구문은 플랫폼 조건에서 지정한 언어 또는 컴파일러 버전이 일치하는 경우에만 분석됩니다. 이 예외는 이전 컴파일러가 최신 버전의 Swift 에 도입된 구문을 분석하지 않도록 합니다.

조건부 컴파일러 블럭에서 명시적 멤버 표현식을 래핑하는 방법에 대한 자세한 내용은 [명시적 멤버 표현식 (Explicit Member Expression)](expressions.md#explicit-member-expression) 을 참고하시기 바랍니다.

> Grammar of a conditional compilation block:
>
> *conditional-compilation-block* → *if-directive-clause* *elseif-directive-clauses*_?_ *else-directive-clause*_?_ *endif-directive*
>
> *if-directive-clause* → *if-directive* *compilation-condition* *statements*_?_ \
> *elseif-directive-clauses* → *elseif-directive-clause* *elseif-directive-clauses*_?_ \
> *elseif-directive-clause* → *elseif-directive* *compilation-condition* *statements*_?_ \
> *else-directive-clause* → *else-directive* *statements*_?_ \
> *if-directive* → **`#if`** \
> *elseif-directive* → **`#elseif`** \
> *else-directive* → **`#else`** \
> *endif-directive* → **`#endif`**
>
> *compilation-condition* → *platform-condition* \
> *compilation-condition* → *identifier* \
> *compilation-condition* → *boolean-literal* \
> *compilation-condition* → **`(`** *compilation-condition* **`)`** \
> *compilation-condition* → **`!`** *compilation-condition* \
> *compilation-condition* → *compilation-condition* **`&&`** *compilation-condition* \
> *compilation-condition* → *compilation-condition* **`||`** *compilation-condition*
>
> *platform-condition* → **`os`** **`(`** *operating-system* **`)`** \
> *platform-condition* → **`arch`** **`(`** *architecture* **`)`** \
> *platform-condition* → **`swift`** **`(`** **`>=`** *swift-version* **`)`** | **`swift`** **`(`** **`<`** *swift-version* **`)`** \
> *platform-condition* → **`compiler`** **`(`** **`>=`** *swift-version* **`)`** | **`compiler`** **`(`** **`<`** *swift-version* **`)`** \
> *platform-condition* → **`canImport`** **`(`** *import-path* **`)`** \
> *platform-condition* → **`targetEnvironment`** **`(`** *environment* **`)`**
>
> *operating-system* → **`macOS`** | **`iOS`** | **`watchOS`** | **`tvOS`** | **`visionOS`** | **`Linux`** | **`Windows`** \
> *architecture* → **`i386`** | **`x86_64`** | **`arm`** | **`arm64`** \
> *swift-version* → *decimal-digits* *swift-version-continuation*_?_ \
> *swift-version-continuation* → **`.`** *decimal-digits* *swift-version-continuation*_?_ \
> *environment* → **`simulator`** | **`macCatalyst`**

### 라인 제어 구문 (Line Control Statement)

라인 제어 구문 (line control statement) 은 컴파일 되는 소스 코드의 라인 숫자와 파일 이름이 다를 수 있는 라인 숫자와 파일 이름을 지정하는데 사용됩니다. Swift 에서 진단과 디버깅을 목적으로 사용하는 소스 코드 위치를 변경하기 위해 라인 제어 구문을 사용합니다.

라인 제어 구문은 다음의 형식을 가집니다:

```swift
#sourceLocation(file: <#file path#>, line: <#line number#>)
#sourceLocation()
```

라인 제어 구문의 첫번째 형식은 라인 제어 구문 다음에 코드의 라인에서 시작하여 `#line`, `#file`, `#fileID`, 그리고 `#filePath` 리터럴 표현식의 값을 변경합니다. _라인 숫자 (line number)_ 는 `#line` 의 값을 변경하고 0 보다 큰 모든 정수 리터럴 입니다. _파일 경로 (file path)_ 는 `#file`, `#fileID`, 그리고 `#filePath` 의 값을 변경하고 문자열 리터럴 입니다. 지정한 문자열은 `#filePath` 의 값이 되고 문자열의 마지막 구성요소는 `#fileID` 의 값으로 사용됩니다. `#file`, `#fileID`, 그리고 `#filePath` 에 자세한 내용은 [리터럴 표현식 (Literal Expression)](expressions.md#literal-expression) 을 참고 바랍니다.

라인 제어 구문의 두번째 형식 인 `#sourceLocation()` 은 소스 코드 위치를 기본 라인 숫자와 파일 경로로 초기화 합니다.

> Grammar of a line control statement:
>
> *line-control-statement* → **`#sourceLocation`** **`(`** **`file:`** *file-path* **`,`** **`line:`** *line-number* **`)`** \
> *line-control-statement* → **`#sourceLocation`** **`(`** **`)`** \
> *line-number* → A decimal integer greater than zero \
> *file-path* → *static-string-literal*

### 컴파일-시간 진단 구문 (Compile-Time Diagnostic Statement)

Swift 5.9 이전에는 컴파일 중에 `#warning` 과 `#error` 구문은 진단을 내보냅니다. 이 동작은 이제 Swift 표준 라이브러리에 [`warning(_:)`](http://developer.apple.com/documentation/swift/documentation/swift/warning(_:)) 와 [`error(_:)`](http://developer.apple.com/documentation/swift/documentation/swift/error(_:)) 매크로로 제공됩니다.

## 가용성 조건 (Availability Condition)

_가용성 조건 (availability condition)_ 은 지정된 플랫폼 인수를 기반으로 런타임 시에 API 의 가용성을 조사하기 위해 `if`, `while`, 그리고 `guard` 구문의 조건으로 사용됩니다.

가용성 조건은 다음의 형식을 가집니다:

```swift
if #available(<#platform name#> <#version#>, <#...#>, *) {
    <#statements to execute if the APIs are available#>
} else {
    <#fallback statements to execute if the APIs are unavailable#>
}
```

런타임 시 API 가 사용가능 여부에 따라 코드의 블럭을 실행하기 위해 가용성 조건을 사용합니다. 컴파일러는 코드의 블럭이 가능한 API 인 경우 가용성 조건에서 정보를 사용합니다.

가용성 조건은 콤마로 구분된 플랫폼 이름과 버전을 가집니다. 플랫폼 이름으로 `iOS`, `macOS`, `watchOS`, `tvOS`, 그리고 `visionOS` 를 사용하고 해당 버전 숫자를 포함합니다. `*` 인수는 필수이고 다른 플랫폼에서 가용성 조건으로 보호되는 코드 블럭의 본문이 타겟에 지정한 최소 배포 타겟에서 실행되도록 합니다.

부울 조건과 다르게 `&&` 와 `||` 와 같은 논리 연산자를 사용하여 가용성 조건을 결합할 수 없습니다. 비가용성 조건을 사용하기 위해 가용성 조건에 부정을 나타내기 위해 `!` 을 사용하는 대신에 다음의 형식을 가질 수 있습니다:

```swift
if #unavailable(<#platform name#> <#version#>, <#...#>) {
    <#fallback statements to execute if the APIs are unavailable#>
} else {
    <#statements to execute if the APIs are available#>
}
```

`#unavailable` 형식은 조건을 부정하는 구문입니다. 비가용성 조건에서 `*` 인수는 암시적이고 포함되어서는 안됩니다. `*` 의 의미는 가용성 조건과 같은 의미를 가집니다.

> Grammar of an availability condition:
>
> *availability-condition* → **`#available`** **`(`** *availability-arguments* **`)`** \
> *availability-condition* → **`#unavailable`** **`(`** *availability-arguments* **`)`** \
> *availability-arguments* → *availability-argument* | *availability-argument* **`,`** *availability-arguments* \
> *availability-argument* → *platform-name* *platform-version* \
> *availability-argument* → **`*`**
>
> *platform-name* → **`iOS`** | **`iOSApplicationExtension`** \
> *platform-name* → **`macOS`** | **`macOSApplicationExtension`** \
> *platform-name* → **`macCatalyst`** | **`macCatalystApplicationExtension`** \
> *platform-name* → **`watchOS`** | **`watchOSApplicationExtension`** \
> *platform-name* → **`tvOS`** | **`tvOSApplicationExtension`** \
> *platform-name* → **`visionOS`** | **`visionOSApplicationExtension`**\
> *platform-version* → *decimal-digits* \
> *platform-version* → *decimal-digits* **`.`** *decimal-digits* \
> *platform-version* → *decimal-digits* **`.`** *decimal-digits* **`.`** *decimal-digits*
