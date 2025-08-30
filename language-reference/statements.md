# 구문 (Statements)

표현식을 그룹화하고 실행 흐름을 제어합니다.

Swift에서 세 종류의 명령어가 있습니다: 단순 구문(simple statements), 컴파일러 제어 구문(compiler control statements),
제어 흐름 구문(control flow statements)입니다.
단순 구문은 가장 일반적이고 표현식이나 선언으로 구성되어 있습니다.
컴파일러 제어 구문은 프로그램이 컴파일러의 동작 일부를 변경할 수 있고
조건부 컴파일 블록과 라인 제어 구문이 포함됩니다.

제어 흐름 구문은 프로그램에서 실행 흐름을 제어하기 위해 사용됩니다.
Swift는 다양한 제어 흐름 구문을 포함하고 있으며,
루프 구문(loop statements), 분기 구문(branch statements), 제어 전송 구문(control transfer statements)이 있습니다.
루프 구문은 코드 블록을 반복적으로 실행하고,
분기 구문은 특정 조건이 일치 할 때만
코드 블록을 실행하고,
제어 전송 구문은 코드의 실행 순서를 변경하는 방법을 제공합니다.
또한 Swift는 새로운 범위를 만들고
오류를 잡고 처리하는 `do` 구문과
현재 범위가 종료되기 직전에 정리 동작을 실행하는 `defer` 구문을 제공합니다.

세미콜론(`;`)은 모든 구문에 선택적으로 나타날 수 있고
같은 줄에 여러 구문을 구분하기 위해 사용할 수 있습니다.

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

<!--
  NOTE: Removed semicolon-statement as syntactic category,
  because, according to Doug, they're not really statements.
  For example, you can't have
      if foo { ; }
  but you should be able to if it's truly considered a statement.
  The semicolon isn't even required for the compiler; we just added
  rules that require them in some places to enforce a certain amount
  of readability.
-->

## 루프 구문 (Loop Statements)

루프 구문(Loop statement)은 루프에 명시한 조건에 따라
특정 코드 블록을 반복적으로 실행되도록 합니다.
Swift는 세 가지 루프 구문을 가지고 있습니다:
`for`-`in` 구문,
`while` 구문,
`repeat`-`while` 구문입니다.

루프 구문에서 제어 흐름은 `break` 구문과
`continue` 구문에 의해 변경될 수 있고 아래에 [중단 구문 (Break Statement)](#중단-구문-break-statement)과
[계속 구문 (Continue Statement)](#계속-구문-continue-statement)에 설명되어 있습니다.

> Grammar of a loop statement:
>
> *loop-statement* → *for-in-statement* \
> *loop-statement* → *while-statement* \
> *loop-statement* → *repeat-while-statement*

### For-In 구문 (For-In Statement)

`for`-`in` 구문은
[`Sequence`](https://developer.apple.com/documentation/swift/sequence) 프로토콜을 준수하는
컬렉션이나 타입의 각 항목에 대해
한 번씩 코드 블록을 실행합니다.

`for`-`in` 구문은 다음의 형식을 가집니다:

```swift
for <#item#> in <#collection#> {
   <#statements#>
}
```

*컬렉션* 표현식(collection expression)에 대해 `makeIterator()` 메서드가 호출되어
[`IteratorProtocol`](https://developer.apple.com/documentation/swift/iteratorprotocol) 프로토콜을
준수하는 타입인
이터레이터(iterator) 타입의 값을 반환합니다.
프로그램은 이터레이터의 `next()` 메서드를 호출하여
루프를 실행합니다.
값이 `nil`을 반환하지 않으면
위 형식의 *항목(item)* 패턴에 할당되고,
프로그램은 위 형식의 *구문(statements)*을 실행한 다음에
루프의 처음으로 돌아가 위 동작을 계속 실행합니다.
그렇지 않으면 프로그램은 할당이나 위 형식의 *구문(statements)* 실행을 수행하지 않고
`for`-`in` 구문 실행을 종료합니다.

> Grammar of a for-in statement:
>
> *for-in-statement* → **`for`** **`case`**_?_ *pattern* **`in`** *expression* *where-clause*_?_ *code-block*

### While 구문 (While Statement)

`while` 구문은 조건이 참으로 남아있는 한
반복적으로 코드 블록을 실행합니다.

`while` 구문은 다음의 형식을 가집니다:

```swift
while <#condition#> {
   <#statements#>
}
```

`while` 구문은 다음과 같이 실행됩니다:

1. 위 형식의 *조건(condition)*은 평가됩니다.
   
   `true`이면 2번이 계속 실행됩니다.
   `false`이면 프로그램은 `while` 구문 실행을 종료합니다.
2. 프로그램은 위 형식의 *구문(statements)*을 실행하고 다시 1번으로 돌아갑니다.

위 형식의 *조건(condition)*의 값이 위 형식의 *구문(statements)*이 실행되기 전에 평가되므로,
`while` 구문의 위 형식의 *구문(statements)*은 한 번도 실행되지 않거나 여러 번 실행될 수 있습니다.

위 형식의 *조건(condition)*의 값은
`Bool` 타입이거나 `Bool`에 브리지된 타입이어야 합니다.
조건은 [옵셔널 바인딩 (Optional Binding)](../language-guide-1/the-basics.md#옵셔널-바인딩-optional-binding)에서 설명한대로
옵셔널 바인딩 선언일 수도 있습니다.

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

`repeat`-`while` 구문은 조건이 참인 경우에 한하여
코드 블록을 한 번 이상 실행합니다.

`repeat`-`while` 구문은 다음의 형식을 가집니다:

```swift
repeat {
   <#statements#>
} while <#condition#>
```

`repeat`-`while` 구문은 다음과 같이 실행됩니다:

1. 프로그램은 위 형식의 *구문(statements)*을 실행하고,
   2번이 계속 실행됩니다.
2. 위 형식의 *조건(condition)*이 평가됩니다.
   
   `true`이면 다시 1번으로 돌아갑니다.
   `false`이면 프로그램은 `repeat`-`while` 구문 실행을 종료합니다.

위 형식의 *구문(statements)*이 실행된 후에 위 형식의 *조건(condition)*의 값이 평가되기 때문에,
`repeat`-`while` 구문에서 위 형식의 *구문(statements)*은 무조건 한 번은 실행됩니다.

위 형식의 *조건(condition)*의 값은
`Bool` 타입이거나 `Bool`로 브리지된 타입이어야 합니다.

> Grammar of a repeat-while statement:
>
> *repeat-while-statement* → **`repeat`** *code-block* **`while`** *expression*

## 분기 구문 (Branch Statements)

분기 구문(Branch statement)은 프로그램이 하나 이상의 조건의 값에 따라
코드의 특정 부분을 실행하는 것을 허용합니다.
분기 구문에서 지정된 조건의 값은
프로그램 분기 방법 및 코드의 어떤 블록이 실행되는지 제어합니다.
Swift는 세 가지 분기 구문을 가지고 있습니다:
`if` 구문, `guard` 구문, `switch` 구문입니다.

`if` 구문이나 `switch` 구문의 제어 흐름은 `break` 구문에 의해 변경될 수 있고
아래 [중단 구문 (Break Statement)](#중단-구문-break-statement)에 설명되어 있습니다.

> Grammar of a branch statement:
>
> *branch-statement* → *if-statement* \
> *branch-statement* → *guard-statement* \
> *branch-statement* → *switch-statement*

### If 구문 (If Statement)

`if` 구문은 하나 이상의 조건 평가를 기반으로
코드 실행에 사용됩니다.

`if` 구문에는 두 가지 기본 형식이 있습니다.
각 형식은 여는 중괄호와 닫는 중괄호가 필요합니다.

첫 번째 형식은 조건이 참일 때만 코드를 실행하고
다음의 형식을 가지고 있습니다:

```swift
if <#condition#> {
   <#statements#>
}
```

`if` 구문의 두 번째 형식은 추가로 *else 절(else clause)*을 제공하고
(`else` 키워드 사용)
조건이 참일 때 코드의 한 부분을 실행하고
동일한 조건이 거짓일 때 코드의 다른 부분을 실행하는데 사용됩니다.
하나의 else 절이 있는 경우에 `if` 구문은 다음의 형식을 가집니다:

```swift
if <#condition#> {
   <#statements to execute if condition is true#>
} else {
   <#statements to execute if condition is false#>
}
```

`if` 구문의 else 절은 둘 이상의 조건을 검사할 수 있는
다른 `if` 구문을 포함할 수 있습니다.
이러한 방법으로 연결된 `if` 구문은 다음의 형식을 가집니다:

```swift
if <#condition 1#> {
   <#statements to execute if condition 1 is true#>
} else if <#condition 2#> {
   <#statements to execute if condition 2 is true#>
} else {
   <#statements to execute if both conditions are false#>
}
```

`if` 구문의 모든 조건 값은
`Bool` 타입이거나 `Bool`로 브리지된 타입이어야 합니다.
조건은 [옵셔널 바인딩 (Optional Binding)](../language-guide-1/the-basics.md#옵셔널-바인딩-optional-binding)에서 설명한대로
옵셔널 바인딩 선언일 수도 있습니다.

> Grammar of an if statement:
>
> *if-statement* → **`if`** *condition-list* *code-block* *else-clause*_?_ \
> *else-clause* → **`else`** *code-block* | **`else`** *if-statement*

### Guard 구문 (Guard Statement)

`guard` 구문은 하나 이상의 조건이 충족되지 않을 때
범위 밖으로 프로그램 제어를 전달하기 위해 사용됩니다.

`guard` 구문은 다음의 형식을 가집니다:

```swift
guard <#condition#> else {
   <#statements#>
}
```

`guard` 구문의 모든 조건의 값은
`Bool` 타입이거나 `Bool`로 브리지된 타입이어야 합니다.
조건은 [옵셔널 바인딩 (Optional Binding)](../language-guide-1/the-basics.md#옵셔널-바인딩-optional-binding)에서 설명한대로
옵셔널 바인딩 선언일 수도 있습니다.

`guard` 구문 조건에서 옵셔널 바인딩 선언으로
할당된 모든 상수나 변수 값은
guard 구문을 둘러싼 범위 내에서 사용할 수 있습니다.

`guard` 구문의 `else` 절은 필수이고
`Never` 반환 타입을 가진 함수를 호출하거나
다음 구문 중 하나를 사용하여
guard 구문을 둘러싼 범위 밖으로 프로그램 제어를 전달해야 합니다:

- `return`
- `break`
- `continue`
- `throw`

제어 전송 구문은 아래 [제어 전송 구문 (Control Transfer Statements)](#제어-전송-구문-control-transfer-statements)에 설명되어 있습니다.
`Never` 반환 타입이 가진 함수에 자세한 내용은
[반환하지 않는 함수 (Functions that Never Return)](./declarations.md#반환하지-않는-함수-functions-that-never-return)를 참고바랍니다.

> Grammar of a guard statement:
>
> *guard-statement* → **`guard`** *condition-list* **`else`** *code-block*

### Switch 구문 (Switch Statement)

`switch` 구문은 제어 표현식의 값에 따라
특정 코드 블록을 실행합니다.

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

`switch` 구문에서 위 형식의 *제어 표현식(control expression)*은 평가된 다음에
각 케이스에 지정된 위 형식의 패턴과 비교합니다.
일치하는 항목이 있으면,
프로그램은 해당 케이스의 범위 내의 위 형식의 *구문(statements)*을 실행합니다.
각 케이스의 범위는 비어 있을 수 없습니다.
결과적으로 각 케이스 레이블의 콜론(`:`) 다음에
적어도 하나의 구문이 포함되어야 합니다.
일치하는 케이스의 본문에서 코드를 실행하지 않으려면 `break` 구문을 사용해야 합니다.

코드가 분기할 수 있는 표현식의 값은 매우 유연합니다.
예를 들어 정수와 문자와 같은 스칼라 타입의 값을 제외하고
코드는 부동소수점 숫자, 문자열, 튜플, 커스텀 클래스의 인스턴스, 옵셔널을
포함한 모든 타입의 값으로 분기할 수 있습니다.
*제어 표현식(control expression)*의 값은 열거형의 케이스의 값과 일치하는지 확인할 수도 있고,
지정된 값 범위에 포함되는지 확인할 수도 있습니다.
`switch` 구문에서 값의 이러한 여러 가지 타입을 어떻게 사용하는지에 대한 예시는
[제어 흐름 (Control Flow)](../language-guide-1/control-flow.md)의 [Switch](../language-guide-1/control-flow.md#switch)를 참고바랍니다.

`switch` 케이스는 각 패턴 후에 선택적으로 `where` 절을 포함할 수 있습니다.
*where 절(where clause)*은 `where` 키워드 다음에 표현식을 붙여 작성하고
케이스의 패턴이 *제어 표현식(control expression)*에 일치하는지 확인하기 전에
추가 조건을 제공하는데 사용합니다.
`where` 절이 있는 경우 연관된 케이스 내의 *구문(statements)*은
*제어 표현식(control expression)*의 값이 케이스의 패턴 중 하나와 일치하고
`where` 절의 표현식이 `true`로 평가될 때만 실행됩니다.
예를 들어 아래 예시에서 *제어 표현식(control expression)*은 `(1, 1)`과 같이 동일한 값의 두 요소가 포함된 튜플인 경우에만
케이스가 일치합니다.

```swift
case let (x, y) where x == y:
```

<!--
  - test: `switch-case-statement`

  ```swifttest
  >> switch (1, 1) {
  -> case let (x, y) where x == y:
  >> break
  >> default: break
  >> }
  ```
-->

위의 예시에서 볼 수 있듯이, 케이스의 패턴은
`let` 키워드를 사용하여 상수에 바인딩할 수도 있습니다(`var` 키워드를 사용하여 변수에 바인딩할 수도 있습니다).
이 상수(또는 변수)는 해당 `where` 절과
케이스 범위 내의 나머지 코드에서 참조할 수 있습니다.
케이스에 제어 표현식과 일치하는 여러 패턴을 포함하는 경우
모든 패턴에는 동일한 상수나 변수 바인딩이 포함되어야 하고
바인딩된 변수나 상수는 케이스의 모든 패턴에서
동일한 타입을 가져야 합니다.

<!--
  The discussion above about multi-pattern cases
  matches discussion of multi-pattern catch under Do Statement.
-->

`switch` 구문은 `default` 키워드로 기본 케이스를 포함할 수도 있습니다.
기본 케이스 내의 코드는 제어 표현식이 일치하는 케이스가 없는 경우에만 실행됩니다.
`switch` 구문은 오직 하나의 기본 케이스만 포함하고,
`switch` 구문의 마지막에 위치해야 합니다.

패턴-일치 연산(pattern-matching operations)의 실제 실행 순서,
특히 케이스 패턴의 평가 순서는 지정되지 않았지만
`switch` 구문의 패턴 일치는
소스 코드가 나타나는 순서대로
평가가 수행되도록 동작합니다.
결과적으로 여러 케이스에 동일한 값으로 평가되는 패턴이 포함되어
제어 표현식의 값과 일치할 수 있는 경우
프로그램은 소스 순서에서 첫 번째로 일치하는 케이스 내의 코드만 실행합니다.

<!--
  - test: `switch-case-with-multiple-patterns`

  ```swifttest
  >> let tuple = (1, 1)
  >> switch tuple {
  >>     case (let x, 5), (let x, 1): print(x)
  >>     default: print(2)
  >> }
  << 1
  ```
-->

<!--
  - test: `switch-case-with-multiple-patterns-err`

  ```swifttest
  >> let tuple = (1, 1)
  >> switch tuple {
  >>     case (let x, 5), (let x as Any, 1): print(1)
  >>     default: print(2)
  >> }
  !$ error: pattern variable bound to type 'Any', expected type 'Int'
  !! case (let x, 5), (let x as Any, 1): print(1)
  !!                       ^
  ```
-->

#### Switch 구문은 완벽해야 함 (Switch Statements Must Be Exhaustive)

Swift에서
제어 표현식의 타입에서 가능한 모든 값은
케이스 패턴 중 적어도 하나의 값과 일치해야 합니다.
이것이 가능하지 않은 경우
(예를 들어 제어 표현식의 타입이 `Int`인 경우),
요구사항을 충족하기 위해 기본 케이스를 포함할 수 있습니다.

#### 향후 열거형 케이스 전환 (Switching Over Future Enumeration Cases)

*비고정 열거형(nonfrozen enumeration)*은
앱을 컴파일하고 출시한 후에도
나중에 새로운 열거형 케이스를 얻을 수 있는
특별한 종류의 열거형입니다.
비고정 열거형을 전환하려면 추가 고려사항이 있습니다.
라이브러리 작성자가 비고정으로 열거형을 표시하면,
새로운 열거형 케이스를 추가할 권한이 있으며
해당 열거형과 상호작용하는 모든 코드는 다시 컴파일되지 않고 향후 케이스를 처리할 수 있어야 합니다.
라이브러리 진화 모드에서 컴파일된 코드,
Swift 표준 라이브러리의 코드,
Apple 프레임워크용 Swift 오버레이,
C 그리고 Objective-C 코드는 비고정 열거형을 선언할 수 있습니다.
고정과 비고정 열거형에 대한 자세한 내용은
[frozen](./attributes.md#frozen)을 참고바랍니다.

비고정 열거형 값을 변경할 때,
열거형의 모든 케이스에 해당 switch 케이스가 있더라도
기본 케이스를 포함해야 합니다.
기본 케이스에 `@unknown` 속성을 적용하여
기본 케이스가 향후에 추가될
열거형 케이스에만 일치하도록 나타낼 수 있습니다.
Swift는
컴파일 시에 알고있는 열거형 케이스와
일치하는 기본 케이스가 있다면 경고를 발생시킵니다.
향후 이 경고는 라이브러리 작성자가
해당 switch 케이스가 없는 열거형에
새로운 케이스를 추가했음을 알려줍니다.

다음 예시는
Swift 표준 라이브러리의 [`Mirror.AncestorRepresentation`](https://developer.apple.com/documentation/swift/mirror/ancestorrepresentation) 열거형의
세 가지 기존 케이스를 모두 전환합니다.
향후에 추가 케이스를 추가하면,
컴파일러는 새로운 케이스를 고려하기 위해
switch 구문을 업데이트 해야 한다고
경고를 생성합니다.

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

<!--
  - test: `unknown-case`

  ```swifttest
  -> let representation: Mirror.AncestorRepresentation = .generated
  -> switch representation {
     case .customized:
         print("Use the nearest ancestor’s implementation.")
     case .generated:
         print("Generate a default mirror for all ancestor classes.")
     case .suppressed:
         print("Suppress the representation of all ancestor classes.")
  -> @unknown default:
         print("Use a representation that was unknown when this code was compiled.")
     }
  <- Generate a default mirror for all ancestor classes.
  ```
-->

#### 실행은 암시적으로 케이스를 통과하지 않음 (Execution Does Not Fall Through Cases Implicitly)

일치한 케이스 내에 코드 실행이 완료되면,
프로그램은 `switch` 구문을 종료합니다.
프로그램 실행은 다음 케이스나 기본 케이스로 계속되거나 "이동(fall through)"되지 않습니다.
이 말은 한 케이스에서 다음 케이스로 계속해서 실행을 하려면,
계속해서 실행되기 원하는 케이스에
`fallthrough` 키워드로 구성된
`fallthrough` 구문을 명시적으로 포함해야 합니다.
`fallthrough` 구문에 대한 자세한 내용은
아래의 [Fallthrough 구문 (Fallthrough Statement)](#fallthrough-구문-fallthrough-statement)을 참고바랍니다.

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

<!--
  The grammar above uses attributes-OPT to match what's used
  in all other places where attributes are allowed,
  although as of Swift 4.2 only a single attribute (@unknown) is allowed.
-->

## 레이블 구문 (Labeled Statement)

접두사로 루프 구문, `if` 구문, `switch` 구문, `do` 구문 앞에
*구문 레이블(statement label)*을 붙일 수 있으며,
이것은 레이블 이름 뒤에 콜론(`:`)을 붙여 구성합니다.
아래 [중단 구문 (Break Statement)](#중단-구문-break-statement)과
[계속 구문 (Continue Statement)](#계속-구문-continue-statement)에서 설명한대로
`break`와 `continue` 구문과 함께 구문 레이블을 사용하여
루프 구문이나 `switch` 구문에서 제어 흐름을 어떻게 변경할지 명시적으로 나타낼 수 있습니다.

레이블 구문의 범위는 구문 레이블 다음에 오는 전체 구문입니다.
레이블 구문을 중첩할 수 있지만 각 구문 레이블의 이름은 고유해야 합니다.

구문 레이블을 어떻게 사용하는지
자세한 내용과 예시는
[제어 흐름 (Control Flow)](../language-guide-1/control-flow.md)의 [레이블이 있는 구문 (Labeled Statements)](../language-guide-1/control-flow.md#레이블이-있는-구문-labeled-statements)을 참고바랍니다.

<!--
  - test: `backtick-identifier-is-legal-label`

  ```swifttest
  -> var i = 0
  -> `return`: while i < 100 {
         i += 1
         if i == 10 {
             break `return`
         }
     }
  -> print(i)
  << 10
  ```
-->

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

제어 전송 구문(Control transfer statements)은 프로그램 제어를 한 코드에서 다른 코드로
무조건 전송하여 프로그램의 코드가 실행되는 순서를 변경할 수 있습니다.
Swift는 다섯 가지 제어 전송 구문을 가지고 있습니다: `break` 구문, `continue` 구문,
`fallthrough` 구문, `return` 구문, `throw` 구문입니다.

> Grammar of a control transfer statement:
>
> *control-transfer-statement* → *break-statement* \
> *control-transfer-statement* → *continue-statement* \
> *control-transfer-statement* → *fallthrough-statement* \
> *control-transfer-statement* → *return-statement* \
> *control-transfer-statement* → *throw-statement*

### 중단 구문 (Break Statement)

`break` 구문은 루프, `if` 구문, `switch` 구문의
프로그램 실행을 종료합니다.
`break` 구문은 `break` 키워드로만 구성되거나
`break` 키워드 다음에 구문 레이블의 이름으로 구성될 수 있습니다.
형식은 다음과 같습니다.

```swift
break
break <#label name#>
```

`break` 구문 다음에 구문 레이블의 이름이 오면,
해당 레이블로 명명된 루프, `if` 구문, `switch` 구문의
프로그램 실행을 종료합니다.

`break` 구문 다음에 구문 레이블의 이름이 없으면,
`switch` 구문이나 해당 구문이 발생한 가장 안쪽의 루프 구문의
프로그램 실행이 종료됩니다.
`if` 구문을 벗어나기 위해 레이블이 없는 `break` 구문을 사용할 수 없습니다.

두 경우 모두 프로그램 제어는
루프나 `switch` 구문 다음 코드의 첫 번째 줄로 전송됩니다.

break 구문을 어떻게 사용하는지에 대한 예시는
[제어 흐름 (Control Flow)](../language-guide-1/control-flow.md)의
[중단 (Break)](../language-guide-1/control-flow.md#중단-break)과 [레이블이 있는 구문 (Labeled Statements)](../language-guide-1/control-flow.md#레이블이-있는-구문-labeled-statements)을 참고바랍니다.

> Grammar of a break statement:
>
> *break-statement* → **`break`** *label-name*_?_

### 계속 구문 (Continue Statement)

`continue` 구문은 루프 구문의 현재 이터레이션의 프로그램 실행을 종료하지만
루프 구문의 실행을 멈추지 않습니다.
`continue` 구문은 `continue` 키워드로만 구성되거나,
`continue` 키워드 다음에 구문 레이블의 이름으로 구성될 수 있습니다.
형식은 다음과 같습니다.

```swift
continue
continue <#label name#>
```

`continue` 구문 다음에 구문 레이블의 이름이 오면,
해당 레이블로 명명된 루프 구문의
현재 이터레이션의 프로그램 실행을 종료합니다.

`continue` 구문 다음에 구문 레이블의 이름이 없으면,
해당 구문이 발생한 가장 안쪽의 루프 구문의
현재 이터레이션의 프로그램 실행이 종료됩니다.

두 경우 모두 프로그램 제어는
둘러싸인 루프 구문의 조건으로 전송됩니다.

`for` 구문에서
증분 표현식은 루프의 본문이 실행된 후에 평가되므로
`continue` 구문 후에도 계속 평가됩니다.

`continue` 구문 사용에 대한 예시는
[제어 흐름 (Control Flow)](../language-guide-1/control-flow.md)의
[계속 (Continue)](../language-guide-1/control-flow.md#계속-continue)과 [레이블이 있는 구문 (Labeled Statements)](../language-guide-1/control-flow.md#레이블이-있는-구문-labeled-statements)을 참고바랍니다.

> Grammar of a continue statement:
>
> *continue-statement* → **`continue`** *label-name*_?_

### Fallthrough 구문 (Fallthrough Statement)

`fallthrough` 구문은 `fallthrough` 키워드로 구성되고
`switch` 구문의 케이스 블록에서만 발생합니다.
`fallthrough` 구문은 `switch` 구문의 한 케이스에서 다음 케이스로
계속해서 프로그램이 실행되도록 합니다.
`switch` 구문의 제어 표현식의 값이
케이스 레이블의 패턴과 일치하지 않아도
다음 케이스는 계속 실행됩니다.

`fallthrough` 구문은 케이스 블록의 마지막 구문 뿐만 아니라
`switch` 구문 내 어디든 나타날 수 있지만
마지막 케이스 블록에서는 사용할 수 없습니다.
또한 값 바인딩 패턴이 포함된
케이스 블록으로 제어를 전송할 수도 없습니다.

`switch` 구문에서 `fallthrough` 구문을 사용하는 방법에 대한 예시는
[제어 흐름 (Control Flow)](../language-guide-1/control-flow.md)의
[제어 전송 구문 (Control Transfer Statements)](../language-guide-1/control-flow.md#제어-전송-구문-control-transfer-statements)을 참고바랍니다.

> Grammar of a fallthrough statement:
>
> *fallthrough-statement* → **`fallthrough`**

### Retrun 구문 (Return Statement)

`return` 구문은 함수나 메서드 정의의 본문에서 발생하고
호출한 함수나 메서드에 반환하기 위해 실행합니다.
프로그램 실행은 함수나 메서드 호출 바로 다음부터 계속해서 실행됩니다.

`return` 구문은 `return` 키워드만으로 구성되거나
`return` 키워드 다음에 표현식으로 구성될 수 있습니다. 형식은 다음과 같습니다.

```swift
return
return <#expression#>
```

`return` 구문 다음에 표현식이 오면,
표현식의 값은 호출한 함수나 메서드에 반환됩니다.
표현식의 값이 함수나 메서드 선언에 선언된
반환 타입의 값과 일치하지 않으면,
표현식의 값은 호출한 함수나 메서드에 반환되기 전에
반환 타입을 변환합니다.

> Note: [실패 가능한 이니셜라이저 (Failable Initializers)](./declarations.md#실패-가능한-이니셜라이저-failable-initializers)에서 설명한대로 `return` 구문의 특수한 형식(`return nil`)은
> 초기화 실패를 나타내기 위해 실패 가능한 이니셜라이저에서 사용할 수 있습니다.

<!--
  TODO: Discuss how the conversion takes place and what is allowed to be converted
  in the (yet to be written) chapter on subtyping and type conversions.
-->

`return` 구문 다음에 표현식이 없으면,
반환값이 없는 함수나 메서드에 반환하기 위해서만 사용될 수 있습니다
(즉, 함수나 메서드의 반환 타입이 `Void`나 `()`인 경우에 해당됩니다).

> Grammar of a return statement:
>
> *return-statement* → **`return`** *expression*_?_

### Throw 구문 (Throw Statement)

`throw` 구문은 오류를 던지는 함수나 메서드의 본문이나
타입에 `throws` 키워드가 표시된 클로저 표현식의 본문에서 발생합니다.

`throw` 구문은 프로그램이 현재 범위의 실행을 종료하고
둘러싸인 범위로 오류 전파를 시작합니다.
던져진 오류는 `do` 구문의
`catch` 절에 의해 처리될 때까지 계속 전파됩니다.

`throw` 구문은 `throw` 키워드 다음에
표현식으로 구성됩니다. 형식은 다음과 같습니다.

```swift
throw <#expression#>
```

위 형식의 *표현식(expression)*의 값은
`Error` 프로토콜을 준수하는 타입이어야 합니다.
`do` 구문이나 `throw` 구문을 포함하는 함수에
발생하는 오류의 타입을 선언하면,
위 형식의 *표현식(expression)*의 값은 해당 타입의 인스턴스여야 합니다.

`throw` 구문을 어떻게 사용하는지에 대한 예시는
[오류 처리 (Error Handling)](./error-handling.md)의
[던지는 함수를 이용한 오류 전파 (Propagating Errors Using Throwing Functions)](./error-handling.md#던지는-함수를-이용한-오류-전파-propagating-errors-using-throwing-functions)를 참고바랍니다.

> Grammar of a throw statement:
>
> *throw-statement* → **`throw`** *expression*

## Defer 구문 (Defer Statement)

`defer` 구문은 `defer` 구문이 나타나는
범위의 바깥으로 프로그램 제어가 전송되기 직전에
실행되는 코드에 사용됩니다.

`defer` 구문은 다음의 형식을 가집니다:

```swift
defer {
    <#statements#>
}
```

`defer` 구문 내의 구문은 프로그램 제어가
어떻게 전송되는 것과 관계없이 실행됩니다.
이것은 예를 들어 `defer` 구문은
파일 닫기와 같은 수동 리소스 관리나
오류가 발생하더라도 수행되어야 하는 작업이 있는 경우에 사용할 수 있습니다.

`defer` 구문의 위 형식의 *구문(statement)*은
`defer` 구문을 둘러싸는 범위의 끝에서 실행됩니다.

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

<!--
  ```swifttest
  -> func f(x: Int) {
    defer { print("First defer") }

    if x < 10 {
      defer { print("Second defer") }
      print("End of if")
    }

    print("End of function")
  }
  f(x: 5)
  <- End of if
  <- Second defer
  <- End of function
  <- First defer
  ```
-->

위의 코드에서
`if` 구문 내의 `defer`는
함수 `f` 내에 선언된 `defer`보다 먼저 실행되는데,
이것은 `if` 구문의 범위가
함수의 범위보다 먼저 끝나기 때문입니다.

같은 범위 안에 여러 개의 `defer` 구문이 있다면,
나타나는 순서의 역순으로 실행됩니다.
범위의 마지막 `defer` 구문을 먼저 실행한다는 것은
해당 마지막 `defer` 구문 내의 구문이
다른 `defer` 구문에 의해 정리될 리소스를 참조할 수 있다는 의미입니다.

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

<!--
  ```swifttest
  -> func f() {
         defer { print("First defer") }
         defer { print("Second defer") }
         print("End of function")
     }
     f()
  <- End of function
  <- Second defer
  <- First defer
  ```
-->

`defer` 구문 내의 구문은
`defer` 구문의 밖으로 프로그램 제어를 전송할 수 없습니다.

> Grammar of a defer statement:
>
> *defer-statement* → **`defer`** *code-block*

## Do 구문 (Do Statement)

`do` 구문은 새로운 범위를 도입하기 위해 사용되고
정의된 오류 조건과 일치하는 패턴을 포함하는
하나 이상의 `catch` 절을 포함할 수 있습니다.
`do` 구문의 범위에서 선언된 변수와 상수는
해당 범위 내에서만 접근할 수 있습니다.

Swift에서 `do` 구문은
코드 블록을 구분하는데 사용되는 C의 중괄호(`{}`)와 유사하며
런타임에 성능 비용이 발생하지 않습니다.

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

`do` 구문은 선택적으로 오류의 타입을 지정할 수 있으며,
아래와 같은 형식을 가집니다:

```swift
do throws(<#type#>) {
    try <#expression#>
} catch <#pattern> {
    <#statements#>
} catch {
    <#statements#>
}
```

`do` 구문이 `throws` 절을 포함하면,
`do` 본문은 지정된 *타입(type)*의 오류만 던질 수 있습니다.
위 형식의 *타입(type)*은
`Error` 프로토콜을 준수하는 구체적인 타입이거나,
`Error` 프로토콜을 준수하는 불투명 타입,
박싱 프로토콜 타입 `any Error`이어야 합니다.
`do` 구문은 오류의 타입을 지정하지 않으면,
Swift는 다음과 같이 오류 타입을 추론합니다:

- `do` 코드 본문의 모든 `throws` 구문과 `try` 표현식이
  오류 처리 메커니즘으로 완벽하게 처리되면,
  Swift는 `do` 구문이 오류를 던지지 않는다고 추론합니다.

- `do` 코드 본문이
  `Never`를 던지는 것이 아니라
  하나의 오류 타입만을
  던지는 코드가 포함되어 있는 경우,
  Swift는 `do` 구문이 구체적인 오류 타입을 던진다고 추론합니다.

- `do` 코드 본문이
  하나 이상의 오류 타입을 던지는 경우,
  Swift는 `do` 구문은
  `any Error`를 던진다고 추론합니다.

명시적 타입을 가지는 오류 동작의 자세한 내용은,
[오류 타입 지정 (Specifying the Error Type)](../language-guide-1/error-handling.md#오류-타입-지정-specifying-the-error-type)을 참고바랍니다.

`do` 코드 블록의 구문에서 오류가 발생하면
프로그렘 제어는
패턴이 일치하는 첫 번째 `catch` 절로 전송됩니다.
일치하는 절이 없으면,
오류를 주변 범위로 전파합니다.
오류를 최상위 수준에서 처리되지 않으면,
프로그램 실행은 런타임 오류로 중단됩니다.

`switch` 구문처럼
컴파일러는 `catch` 절이 완벽한지 여부를 추론하려고 합니다.
그러한 결정을 내릴 수 있으면 오류가 처리된 것으로 간주합니다.
그렇지 않으면 오류는 포함된 범위 밖으로 전파될 수 있습니다.
이것은
오류는 `catch` 절에서 처리되거나
포함한 함수가 `throws`로 선언되어야 한다는 의미입니다.

여러 패턴을 가지는 `catch` 절은
패턴 중 하나가 일치하는 경우에 오류와 일치합니다.
`catch` 절에 여러 패턴을 포함하는 경우
모든 패턴은 동일한 상수나 변수 바인딩이 포함되어야 하며
각 바인딩된 변수나 상수는
모든 `catch` 절의 패턴에서 동일한 타입을 가져야 합니다.

<!--
  The discussion above of multi-pattern catch
  matches the discussion of multi-pattern case under Switch Statement.
-->

오류가 처리되도록 보장하려면
와일드카드 패턴(`_`)과 같이
모든 오류를 일치하는 패턴으로 `catch` 절을 사용해야 합니다.
`catch` 절에 패턴을 지정하지 않으면,
`catch` 절은 모든 오류와 일치하고 `error`라는 이름의 지역 상수에 바인딩 됩니다.
`catch` 절에서 사용할 수 있는 패턴에 대한 자세한 내용은
[패턴 (Patterns)](./patterns.md)을 참고바랍니다.

`do` 구문과 여러 `catch` 절을 사용하는 방법에 대한 예시는
[오류 처리 (Handling Errors)](../language-guide-1/error-handling.md#오류-처리-error-handling)를 참고바랍니다.

> Grammar of a do statement:
>
> *do-statement* → **`do`** *throws-clause*_?_ *code-block* *catch-clauses*_?_ \
> *catch-clauses* → *catch-clause* *catch-clauses*_?_ \
> *catch-clause* → **`catch`** *catch-pattern-list*_?_ *code-block* \
> *catch-pattern-list* → *catch-pattern* | *catch-pattern* **`,`** *catch-pattern-list* \
> *catch-pattern* → *pattern* *where-clause*_?_

## 컴파일러 제어 구문 (Compiler Control Statements)

컴파일러 제어 구문(compiler control statement)은 컴파일러의 동작 측면을 변경할 수 있습니다.
Swift는 세 가지 컴파일러 제어 구문을 가지고 있습니다:
조건부 컴파일 블록(conditional compilation block),
라인 제어 구문(line control statement),
컴파일 시점 진단 구문(compile-time diagnostic statement)입니다.

> Grammar of a compiler control statement:
>
> *compiler-control-statement* → *conditional-compilation-block* \
> *compiler-control-statement* → *line-control-statement* \
> *compiler-control-statement* → *diagnostic-statement*

### 조건부 컴파일 블록 (Conditional Compilation Block)

조건부 컴파일 블록(conditional compilation block)은 하나 이상의 컴파일러 조건의 값에 따라
코드를 조건부로 컴파일할 수 있습니다.

모든 조건부 컴파일 블록은 `#if` 컴파일 지시문으로 시작하고
`#endif` 컴파일 지시문으로 끝납니다.
간단한 조건부 컴파일 블록은 다음의 형식을 가집니다:

```swift
#if <#compilation condition#>
    <#statements#>
#endif
```

`if` 구문의 조건과 다르게
위 형식의 *컴파일러 조건(compilation condition)*은 컴파일 시에 평가됩니다.
결과적으로
위 형식의 *구문(statements)*은 컴파일 시에 위 형식의 *컴파일러 조건(compilation condition)*이
`true`로 평가될 때만 컴파일되고 실행됩니다.

위 형식의 *컴파일러 조건(compilation condition)*은 `true`와 `false` Boolean 리터럴(Boolean literals),
`-D` 명령줄 플래그를 사용한 식별자,
아래 표에 나열된 플랫폼 조건을 포함할 수 있습니다.

| 플랫폼 조건 | 유효 인자 |
| -------- | ------- |
| `os()` | `macOS`, `iOS`, `watchOS`, `tvOS`, `visionOS`, `Linux`, `Windows` |
| `arch()` | `arm`, `arm64`, `i386`, `wasm32`, `x86_64` |
| `swift()` | `>=` 또는 `<` 다음에 버전 번호 |
| `compiler()` | `>=` 또는 `<` 다음에 버전 번호 |
| `canImport()` | 모듈 이름 |
| `targetEnvironment()` | `simulator`, `macCatalyst` |

<!--
  The lists above match <https://www.swift.org/platform-support/>
  and include only platforms with *official* support.
  For the full list of operating systems and architectures,
  including those with unofficial or experimental support,
  see the values of
  SupportedConditionalCompilationOSs and SupportedConditionalCompilationArches
  in the file lib/Basic/LangOptions.cpp.
  The compiler also accepts pretty much any string --
  for example "#if os(toaster)" compiles just fine,
  but Swift doesn't actually support running on a toaster oven --
  so don't rely on that when checking possible os/arch values.
-->

<!--
  The target environment "UIKitForMac"
  is understood by the compiler as a synonym for "macCatalyst",
  but that spelling is marked "Must be removed" outside of a few places,
  so it's omitted from the table above.
-->

`swift()`와 `compiler()` 플랫폼 조건에 대한 버전 번호는
버전 번호의 각 부분을 점(`.`)으로 구분하여
주 번호, 선택적 부 번호, 선택적 패치 번호 등으로 구성됩니다.
비교 연산자와 버전 번호 사이에 공백이 없어야 합니다.
`compiler()`에 대한 버전은
컴파일러에 전달된 Swift 버전 설정과 상관없이 컴파일러 버전입니다.
`swift()`에 대한 버전은 현재 컴파일되는 언어 버전입니다.
예를 들어 Swift 4.2 모드에서 Swift 5 컴파일러를 사용하여 코드를 컴파일 한다면,
컴파일러 버전은 5이고 언어 버전은 4.2입니다.
이러한 설정으로
다음의 코드는 세가지 메세지를 모두 출력합니다:

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

<!--
  ```swifttest
  -> #if compiler(>=5)
     print("Compiled with the Swift 5 compiler or later")
     #endif
     #if swift(>=4.2)
     print("Compiled in Swift 4.2 mode or later")
     #endif
     #if compiler(>=5) && swift(<5)
     print("Compiled with the Swift 5 compiler or later in a Swift mode earlier than 5")
     #endif
  <- Compiled with the Swift 5 compiler or later
  <- Compiled in Swift 4.2 mode or later
  // Prints "Compiled with the Swift 5 compiler or later in a Swift mode earlier than 5"
  ```
-->

<!--
  That testcode is cheating by explicitly printing the third line of output,
  since it's not actually running in Swift 4.2 mode.
-->

`canImport()` 플랫폼 조건의 인자는
모든 플랫폼에 존재하지 않을 수 있는 모듈의 이름입니다.
모듈은 이름에 마침표(`.`)를 포함할 수 있습니다.
이 조건은 해당 모듈을 가져올 수 있는지 검사하지만
실제로 가져오지 않습니다.
모듈이 있으면 플랫폼 조건은 `true`를 반환하고;
그렇지 않으면 `false`를 반환합니다.

<!--
  - test: `canImport_A, canImport`

  ```swifttest
  >> public struct SomeStruct {
  >>     public init() { }
  >> }
  ```
-->

<!--
  - test: `canImport_A.B, canImport`

  ```swifttest
  >> public struct AnotherStruct {
  >>     public init() { }
  >> }
  ```
-->

<!--
  - test: `canImport`

  ```swifttest
  >> import canImport_A
  >> let s = SomeStruct()
  >> #if canImport(canImport_A)
  >> #else
  >> #error("Can't import A")
  >> #endif

  >> #if canImport(canImport_A.B)
  >> #else
  >> #error("Can't import A.B")
  >> #endif
  ```
-->

`targetEnvironment()` 플랫폼 조건은
지정한 환경에 대해 코드가 컴파일 되면 `true`를 반환하고;
그렇지 않으면 `false`를 반환합니다.

> Note: `arch(arm)` 플랫폼 조건은 ARM 64 기기에 대해 `true`를 반환하지 않습니다.
> `arch(i386)` 플랫폼 조건은
> 32-비트 iOS 시뮬레이터에 대해 코드가 컴파일될 때 `true`를 반환합니다.

<!--
  - test: `pound-if-swift-version`

  ```swifttest
  -> #if swift(>=2.1)
         print(1)
     #endif
  << 1
  -> #if swift(>=2.1) && true
         print(2)
     #endif
  << 2
  -> #if swift(>=2.1) && false
         print(3)
     #endif
  -> #if swift(>=2.1.9.9.9.9.9.9.9.9.9)
         print(5)
     #endif
  << 5
  ```
-->

<!--
  - test: `pound-if-swift-version-err`

  ```swifttest
  -> #if swift(>= 2.1)
         print(4)
     #endif
  !$ error: unary operator cannot be separated from its operand
  !! #if swift(>= 2.1)
  !!           ^ ~
  !!-
  ```
-->

<!--
  - test: `pound-if-compiler-version`

  ```swifttest
  -> #if compiler(>=4.2)
         print(1)
     #endif
  << 1
  -> #if compiler(>=4.2) && true
         print(2)
     #endif
  << 2
  -> #if compiler(>=4.2) && false
         print(3)
     #endif
  ```
-->

논리 연산자 `&&`, `||`, `!`를 사용하여
컴파일 조건을 결합하고 부정할 수 있으며
그룹화를 위한 소괄호를 사용할 수 있습니다.
이러한 연산자는 Boolean 표현식을 결합하는데 사용되는 논리 연산자와
동일한 결합방향과 우선순위를 갖습니다.

`if` 구문과 유사하게
다른 컴파일 조건에 대해 검사하기 위해 여러 조건부 분기를 추가할 수 있습니다.
`#elseif` 절을 사용하여 원하는 만큼 분기를 추가할 수 있습니다.
`#else` 절을 사용하여 마지막 분기를 추가할 수도 있습니다.
여러 개 분기를 포함하는 조건부 컴파일 블록
형식은 다음과 같습니다:

```swift
#if <#compilation condition 1#>
    <#statements to compile if compilation condition 1 is true#>
#elseif <#compilation condition 2#>
    <#statements to compile if compilation condition 2 is true#>
#else
    <#statements to compile if both compilation conditions are false#>
#endif
```

> Note: 조건부 컴파일 블록의 본문에서 각 구문(statement)은
> 컴파일 되지 않아도 구문 분석됩니다.
> 그러나 컴파일 조건이
> `swift()`나 `compiler()` 플랫폼 조건을 포함하면 예외가 있습니다:
> 이 구문은
> 플랫폼 조건에서 지정한
> 언어나 컴파일러 버전이 일치하는 경우에만 분석됩니다.
> 이 예외는 이전 컴파일러가
> 최신 버전의 Swift에 도입된 구문을 분석하지 않도록 합니다.

조건부 컴파일 블록에서 명시적 멤버 표현식을
래핑하는 방법에 대한 자세한 내용은
[명시적 멤버 표현식 (Explicit Member Expression)](./expressions.md#명시적-멤버-표현식-explicit-member-expression)을 참고바랍니다.

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
> *architecture* → **`arm`** | **`arm64`** | **`i386`** | **`wasm32`** | **`x86_64`** \
> *swift-version* → *decimal-digits* *swift-version-continuation*_?_ \
> *swift-version-continuation* → **`.`** *decimal-digits* *swift-version-continuation*_?_ \
> *environment* → **`simulator`** | **`macCatalyst`**

<!--
  Testing notes:

  !!true doesn't work but !(!true) does -- this matches normal expressions
  #if can be nested, as expected

  Also, the body of a conditional compilation block contains *zero* or more statements.
  Thus, this is allowed:
      #if
      #elseif
      #else
      #endif
-->

### 라인 제어 구문 (Line Control Statement)

라인 제어 구문(line control statement)은 컴파일 중인 소스 코드의 라인 번호와 파일 이름이 다를 수 있는
라인 번호와 파일 이름을 지정하는데 사용됩니다.
라인 제어 구문을 사용하여 Swift가 진단과 디버깅을 목적으로 사용하는
소스 코드 위치를 변경할 수 있습니다.

라인 제어 구문은 다음의 형식을 가집니다:

```swift
#sourceLocation(file: <#file path#>, line: <#line number#>)
#sourceLocation()
```

라인 제어 구문의 첫 번째 형식은
라인 제어 구문 다음 줄의 코드부터
`#line`, `#file`, `#fileID`, `#filePath` 리터럴 표현식의 값을 변경합니다.
*라인 번호(line number)*는 `#line`의 값을 변경하고,
0보다 큰 모든 정수 리터럴 입니다.
*파일 경로(file path)*는 `#file`, `#fileID`, `#filePath`의 값을 변경하고,
문자열 리터럴 입니다.
지정한 문자열은 `#filePath`의 값이 되고,
문자열의 마지막 구성요소는 `#fileID`의 값으로 사용됩니다.
`#file`, `#fileID`, `#filePath`의 자세한 내용은
[리터럴 표현식 (Literal Expression)](./expressions.md#리터럴-표현식-literal-expression)을 참고바랍니다.

라인 제어 구문의 두 번째 형식 인 `#sourceLocation()`은
소스 코드 위치를 기본 라인 번호와 파일 경로로 초기화 합니다.

> Grammar of a line control statement:
>
> *line-control-statement* → **`#sourceLocation`** **`(`** **`file:`** *file-path* **`,`** **`line:`** *line-number* **`)`** \
> *line-control-statement* → **`#sourceLocation`** **`(`** **`)`** \
> *line-number* → A decimal integer greater than zero \
> *file-path* → *static-string-literal*

### 컴파일 시점 진단 구문 (Compile-Time Diagnostic Statement)

Swift 5.9 이전에는
컴파일 중에 `#warning`과 `#error` 구문은 진단 정보를 생성했습니다.
이 동작은 이제
Swift 표준 라이브러리에 [`warning(_:)`][]과 [`error(_:)`][] 매크로로 제공됩니다.

[`warning(_:)`]: https://developer.apple.com/documentation/swift/warning(_:)
[`error(_:)`]: https://developer.apple.com/documentation/swift/error(_:)

## 가용성 조건 (Availability Condition)

*가용성 조건(availability condition)*은 지정된 플랫폼 인자를 기반으로
런타임에 API의 가용성을 조사하기 위해
`if`, `while`, `guard` 구문의 조건으로 사용됩니다.

가용성 조건은 다음의 형식을 가집니다:

```swift
if #available(<#platform name#> <#version#>, <#...#>, *) {
    <#statements to execute if the APIs are available#>
} else {
    <#fallback statements to execute if the APIs are unavailable#>
}
```

가용성 조건을 사용하여 런타임에 API가 사용 가능 여부에 따라
코드 블록을 실행합니다.
컴파일러는 코드의 블록이 가능한 API인 경우
가용성 조건의 정보를 사용합니다.

가용성 조건은 콤마로 구분된 플랫폼 이름과 버전을 가집니다.
플랫폼 이름으로 `iOS`, `macOS`, `watchOS`, `tvOS`, `visionOS`를 사용하고
해당 버전 숫자를 포함합니다.
`*` 인자는 필수이고 다른 플랫폼에서
가용성 조건으로 보호되는 코드 블록의 본문이
지정한 최소 배포 타겟에서 실행되도록 합니다.

Boolean 조건과 다르게 `&&`와 `||` 같은 논리 연산자를 사용하여
가용성 조건을 결합할 수 없습니다.
가용성 조건에 부정을 나타내기 위해 `!`을 사용하는 대신에
다음의 형식을 가지는 불가능성 조건을 사용합니다:

```swift
if #unavailable(<#platform name#> <#version#>, <#...#>) {
    <#fallback statements to execute if the APIs are unavailable#>
} else {
    <#statements to execute if the APIs are available#>
}
```

`#unavailable` 형식은 조건을 부정하는 구문입니다.
불가능성 조건에서
`*` 인자는 암시적이고 포함해서는 안됩니다.
`*`의 의미는 가용성 조건과 같은 의미를 가집니다.

> Grammar of an availability condition:
>
> *availability-condition* → **`#available`** **`(`** *availability-arguments* **`)`** \
> *availability-condition* → **`#unavailable`** **`(`** *availability-arguments* **`)`** \
> *availability-arguments* → *availability-argument* | *availability-argument* **`,`** *availability-arguments* \
> *availability-argument* → *platform-name* *platform-version* \
> *availability-argument* → **`*`**
>
>
>
> *platform-name* → **`iOS`** | **`iOSApplicationExtension`** \
> *platform-name* → **`macOS`** | **`macOSApplicationExtension`** \
> *platform-name* → **`macCatalyst`** | **`macCatalystApplicationExtension`** \
> *platform-name* → **`watchOS`** | **`watchOSApplicationExtension`** \
> *platform-name* → **`tvOS`** | **`tvOSApplicationExtension`** \
> *platform-name* → **`visionOS`** | **`visionOSApplicationExtension`** \
> *platform-version* → *decimal-digits* \
> *platform-version* → *decimal-digits* **`.`** *decimal-digits* \
> *platform-version* → *decimal-digits* **`.`** *decimal-digits* **`.`** *decimal-digits*

<!--
  If you need to add a new platform to this list,
  you probably need to update the list under @available too.
-->

<!--
  - test: `pound-available-platform-names`

  ```swifttest
  >> if #available(iOS 1, iOSApplicationExtension 1,
  >>               macOS 1, macOSApplicationExtension 1,
  >>               macCatalyst 1, macCatalystApplicationExtension 1,
  >>               watchOS 1, watchOSApplicationExtension 1,
  >>               tvOS 1, tvOSApplicationExtension 1,
  >>               visionOS 1, visionOSApplicationExtension 1, *) {
  >>     print("a")
  >> } else {
  >>     print("b")
  >> }
  >> if #available(madeUpPlatform 1, *) {
  >>     print("c")
  >> }
  >> if #unavailable(fakePlatform 1) {
  >>     print("d")
  >> } else {
  >>     print("dd")
  >> }
  !$ warning: unrecognized platform name 'madeUpPlatform'
  !$ if #available(madeUpPlatform 1, *) {
  !$               ^
  !$ warning: unrecognized platform name 'fakePlatform'
  !$ if #unavailable(fakePlatform 1) {
  !$                 ^
  << a
  << c
  << dd
  ```
-->

<!--
  - test: `empty-availability-condition`

  ```swifttest
  >> if #available(*) { print("1") }
  << 1
  ```
-->

<!--
  - test: `empty-unavailability-condition`

  ```swifttest
  >> if #unavailable() { print("2") }
  !$ error: expected platform name
  !$ if #unavailable() { print("2") }
  !$                ^
  ```
-->

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