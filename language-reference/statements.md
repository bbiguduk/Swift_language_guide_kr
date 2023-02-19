# 구문 (Statements)

<!--
In Swift, there are three kinds of statements: simple statements, compiler control statements, and control flow statements. Simple statements are the most common and consist of either an expression or a declaration. Compiler control statements allow the program to change aspects of the compiler’s behavior and include a conditional compilation block and a line control statement.

Control flow statements are used to control the flow of execution in a program. There are several types of control flow statements in Swift, including loop statements, branch statements, and control transfer statements. Loop statements allow a block of code to be executed repeatedly, branch statements allow a certain block of code to be executed only when certain conditions are met, and control transfer statements provide a way to alter the order in which code is executed. In addition, Swift provides a do statement to introduce scope, and catch and handle errors, and a defer statement for running cleanup actions just before the current scope exits.

A semicolon (;) can optionally appear after any statement and is used to separate multiple statements if they appear on the same line.
-->

Swift 에서 간단한 구문 (simple statements), 컴파일러 제어 구문 (compiler control statements), 그리고 제어 흐름 구문 (control flow statements) 의 세가지 구문이 있습니다. 간단한 구문은 가장 일반적이고 표현식 또는 선언으로 구성됩니다. 컴파일러 제어 구문은 프로그램이 컴파일러의 동작 측면을 변경할 수 있고 조건부 컴파일러 및 라인 제어 구문을 포함할 수 있습니다.

제어 흐름 구문은 프로그램에서 실행의 흐름을 제어하기 위해 사용됩니다. Swift 에서 루프 구문 (loop statements), 분기 구문 (branch statements), 그리고 제어 전송 구문 (control transfer statements) 를 포함하는 제어 흐름 구문의 몇가지 타입이 있습니다. 루프 제어문은 반복적으로 실행되는 코드의 블럭을 허용하고, 분기 구문은 조건이 일치 할 때만 실행되도록 코드의 블럭을 허용하고, 제어 전송 구문은 코드가 실행되는 순서를 변경하는 방법을 제공합니다. 또한 Swift 는 범위를 도입하고 에러를 잡고 처리하는 `do` 구문과 현재 범위가 종료하기 직전에 정리 동작을 실행하는 `defer` 구문을 제공합니다.

세미콜론 (`;`) 은 모든 구문에 선택적으로 나타날 수 있고 같은 줄에 여러 구문을 구분하기 위해 사용될 수 있습니다.

> GRAMMAR OF A STATEMENT\
> statement → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `;` $$_{opt}$$\
> statement → [declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration) `;` $$_{opt}$$\
> statement → [loop-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_loop-statement) `;` $$_{opt}$$\
> statement → [branch-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_branch-statement) `;` $$_{opt}$$\
> statement → [labeled-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_labeled-statement) `;` $$_{opt}$$\
> statement → [control-transfer-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_control-transfer-statement) `;` $$_{opt}$$\
> statement → [defer-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_defer-statement) `;` $$_{opt}$$\
> statement → [do-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_do-statement) `;` $$_{opt}$$\
> statement → [compiler-control-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compiler-control-statement)\
> statements → [statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statement) [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statements) $$_{opt}$$

## 루프 구문 (Loop Statements)

<!--
Loop statements allow a block of code to be executed repeatedly, depending on the conditions specified in the loop. Swift has three loop statements: a for-in statement, a while statement, and a repeat-while statement.

Control flow in a loop statement can be changed by a break statement and a continue statement and is discussed in Break Statement and Continue Statement below.
-->

루프 구문 (Loop statements) 은 루프에 지정한 조건에 따라 반복적으로 실행되는 코드의 블럭을 허용합니다. Swift 는 `for`-`in` 구문, `while` 구문, 그리고 `repeat`-`while` 구문과 같이 세가지 루프 구문을 가집니다.

루프 구문에서 제어 흐름은 `break` 구문과 `continue` 구문에 의해 변경될 수 있고 아래에 [중단 구문 (Break Statement)](statements.md#break-statement) 과 [계속 구문 (Continue Statement)](statements.md#continue-statement) 에 설명되어 있습니다.

> GRAMMAR OF A LOOP STATEMENT\
> loop-statement → [for-in-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_for-in-statement)\
> loop-statement → [while-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_while-statement)\
> loop-statement → [repeat-while-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_repeat-while-statement)

### For-In 구문 (For-In Statement)

<!--
A for-in statement allows a block of code to be executed once for each item in a collection (or any type) that conforms to the Sequence protocol.

A for-in statement has the following form:
-->

`for`-`in` 구문은 [`Sequence`](https://developer.apple.com/documentation/swift/sequence) 프로토콜을 준수하는 콜렉션 또는 모든 타입에서 각 아이템에 대해 한번 실행되기 위해 코드의 블럭을 허용합니다.

`for`-`in` 구문은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.09.29.png>)

<!--
The makeIterator() method is called on the collection expression to obtain a value of an iterator type—that is, a type that conforms to the IteratorProtocol protocol. The program begins executing a loop by calling the next() method on the iterator. If the value returned isn’t nil, it’s assigned to the item pattern, the program executes the statements, and then continues execution at the beginning of the loop. Otherwise, the program doesn’t perform assignment or execute the statements, and it’s finished executing the for-in statement.
-->

`makeIterator()` 메서드는 [`IteratorProtocol`](https://developer.apple.com/documentation/swift/iteratorprotocol) 프로토콜을 준수하는 타입인 반복기 타입의 값을 포함하기 위해 _콜렉션_ 표현식 (_collection_ expression) 에서 호출됩니다. 프로그램은 반복기에서 `next()` 메서드를 호출하여 루프를 실행합니다. 값이 `nil` 을 반환하지 않으면 _항목 (item)_ 패턴에 할당되고 프로그램은 _구문 (statements)_ 를 실행한 다음에 루프의 시작된 부분에서 계속 실행합니다. 그렇지 않으면 프로그램은 할당 또는 _구문 (statements)_ 실행을 수행하지 않고 `for`-`in` 구문 실행을 종료합니다.

> GRAMMAR OF A FOR-IN STATEMENT\
> for-in-statement → `for` `case` $$_{opt}$$ [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern) `in` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) [where-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_where-clause) $$_{opt}$$ [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)

### While 구문 (While Statement)

<!--
A while statement allows a block of code to be executed repeatedly, as long as a condition remains true.

A while statement has the following form:
-->

`while` 구문은 조건이 참으로 남아있는 한 반복적으로 실행되기 위해 코드의 블럭을 허용합니다.

`while` 구문은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.10.30.png>)

<!--
A while statement is executed as follows:

1. The condition is evaluated.

If true, execution continues to step 2. If false, the program is finished executing the while statement.

2. The program executes the statements, and execution returns to step 1.

Because the value of the condition is evaluated before the statements are executed, the statements in a while statement can be executed zero or more times.

The value of the condition must be of type Bool or a type bridged to Bool. The condition can also be an optional binding declaration, as discussed in Optional Binding.
-->

`while` 구문은 다음과 같이 실행됩니다:

1. _조건 (condition)_ 은 평가됩니다. `true` 이면 2번이 계속 실행됩니다. `false` 이면 프로그램은 `while` 구문 실행을 종료합니다.
2. 프로그램은 _구문 (statements)_ 을 실행하고 실행은 1번으로 돌아갑니다.

_조건 (condition)_ 의 값이 _구문 (statements)_ 이 실행되기 전에 평가되므로 `while` 구문의 _구문 (statements)_ 은 0번 또는 더 많이 실행될 수 있습니다.

_조건 (condition)_ 의 값은 타입 `Bool` 이거나 `Bool` 에 브릿지된 타입이어야 합니다. 조건은 [옵셔널 바인딩 (Optional Binding)](../language-guide-1/the-basics.md#optional-binding) 에서 설명한대로 옵셔널 바인딩 선언일 수도 있습니다.

> GRAMMAR OF A WHILE STATEMENT\
> while-statement → `while` [condition-list](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_condition-list) [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)\
> condition-list → [condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_condition) | [condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_condition) `,` [condition-list](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_condition-list)\
> condition → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) | [availability-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_availability-condition) | [case-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_case-condition) | [optional-binding-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_optional-binding-condition)\
> case-condition → `case` [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern) [initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer)\
> optional-binding-condition → `let` [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern) [initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer) $$_{opt}$$ | `var` [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern) [initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer) $$_{opt}$$

### Repeat-While 구문 (Repeat-While Statement)

<!--
A repeat-while statement allows a block of code to be executed one or more times, as long as a condition remains true.

A repeat-while statement has the following form:
-->

`repeat`-`while` 구문은 조건이 참인 경우에 한하여 한 번 이상 실행되기 위한 코드의 블럭을 허용합니다.

`repeat`-`while` 구문은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.12.34.png>)

<!--
A repeat-while statement is executed as follows:

1. The program executes the statements, and execution continues to step 2.

2. The condition is evaluated.

If true, execution returns to step 1. If false, the program is finished executing the repeat-while statement.

Because the value of the condition is evaluated after the statements are executed, the statements in a repeat-while statement are executed at least once.

The value of the condition must be of type Bool or a type bridged to Bool. The condition can also be an optional binding declaration, as discussed in Optional Binding.
-->

`repeat`-`while` 구문은 다음과 같이 실행됩니다:

1. 프로그램은 _구문 (statements)_ 를 실행하고 2번이 계속 실행됩니다.
2. _조건 (condition)_ 이 평가됩니다. `true` 이면 실행은 1번으로 돌아갑니다. `false` 이면 프로그램은 `repeat`-`while` 구문 실행이 종료됩니다.

_조건 (condition)_ 의 값이 평가되기 때문에 후에 _구문 (statements)_ 이 실행되고 `repeat`-`while` 구문에서 _구문 (statements)_ 은 적어도 한 번은 실행됩니다.

_조건 (condition)_ 의 값은 타입 `Bool` 이거나 `Bool` 로 브릿지된 타입이어야 합니다. 조건은 [옵셔널 바인딩 (Optional Binding)](../language-guide-1/the-basics.md#optional-binding) 에서 설명한대로 옵셔널 바인딩 선언일 수도 있습니다.

> GRAMMAR OF A REPEAT-WHILE STATEMENT\
> repeat-while-statement → `repeat` [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block) `while` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression)

## 분기 구문 (Branch Statements)

<!--
Branch statements allow the program to execute certain parts of code depending on the value of one or more conditions. The values of the conditions specified in a branch statement control how the program branches and, therefore, what block of code is executed. Swift has three branch statements: an if statement, a guard statement, and a switch statement.

Control flow in an if statement or a switch statement can be changed by a break statement and is discussed in Break Statement below.
-->

분기 구문 (Branch statements) 은 프로그램이 하나 이상의 조건의 값에 따라 코드의 특정 부분을 실행하는 것을 허용합니다. 분기 구문에서 지정된 조건의 값은 프로그램 분기 방법 및 코드의 어떤 블럭이 실행되는지 제어합니다. Swift 는 `if` 구문, `guard` 구문, 그리고 `switch` 구문의 세가지 분기 구문을 가지고 있습니다.

`if` 구문 또는 `switch` 구문에서 제어 흐름은 `break` 구문에 의해 변경될 수 있고 아래 [중단 구문 (Break Statement)](statements.md#break-statement) 에 설명되어 있습니다.

> GRAMMAR OF A BRANCH STATEMENT\
> branch-statement → [if-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_if-statement)\
> branch-statement → [guard-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_guard-statement)\
> branch-statement → [switch-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_switch-statement)

### If 구문 (If Statement)

<!--
An if statement is used for executing code based on the evaluation of one or more conditions.

There are two basic forms of an if statement. In each form, the opening and closing braces are required.

The first form allows code to be executed only when a condition is true and has the following form:
-->

`if` 구문은 하나 이상의 조건의 평가를 기반으로 코드 실행에 사용됩니다.

`if` 구문에는 두가지 기본 형식이 있습니다. 각 형식에서 여는 중괄호와 닫는 중괄호가 필요합니다.

첫번째 형식은 조건이 참이고 다음의 형식을 가지고 있을 때만 코드가 실행되도록 합니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.13.57.png>)

<!--
The second form of an if statement provides an additional else clause (introduced by the else keyword) and is used for executing one part of code when the condition is true and another part of code when the same condition is false. When a single else clause is present, an if statement has the following form:
-->

`if` 구문의 두번째 형식은 추가로 `else` 키워드로 도입된 _else 절 (else clause)_ 을 제공하고 조건이 참일 때 코드의 한 부분을 실행하고 동일한 조건이 거짓일 때 코드의 다른 부분을 실행하는데 사용됩니다. 하나의 else 절이 있는 경우에 `if` 구문은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.14.32.png>)

<!--
The else clause of an if statement can contain another if statement to test more than one condition. An if statement chained together in this way has the following form:
-->

`if` 구문의 else 절은 둘 이상의 조건을 검사하기 위해 다른 `if` 구문을 포함할 수 있습니다. 이러한 방법으로 연결된 `if` 구문은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.15.35.png>)

<!--
The value of any condition in an if statement must be of type Bool or a type bridged to Bool. The condition can also be an optional binding declaration, as discussed in Optional Binding.
-->

`if` 구문에서 모든 조건의 값은 타입 `Bool` 이거나 `Bool` 로 브릿지된 타입이어야 합니다. 조건은 [옵셔널 바인딩 (Optional Binding)](../language-guide-1/the-basics.md#optional-binding) 에서 설명한대로 옵셔널 바인딩 선언일 수도 있습니다.

> GRAMMAR OF AN IF STATEMENT\
> if-statement → `if` [condition-list](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_condition-list) [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block) [else-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_else-clause) $$_{opt}$$\
> else-clause → `else` [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block) | `else` [if-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_if-statement)

### Guard 구문 (Guard Statement)

<!--
A guard statement is used to transfer program control out of a scope if one or more conditions aren’t met.

A guard statement has the following form:
-->

`guard` 구문은 하나 이상의 조건이 충족되지 않을 때 범위 밖으로 프로그램 제어를 전송하기 위해 사용됩니다.

`guard` 구문은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.16.41.png>)

<!--
The value of any condition in a guard statement must be of type Bool or a type bridged to Bool. The condition can also be an optional binding declaration, as discussed in Optional Binding.

Any constants or variables assigned a value from an optional binding declaration in a guard statement condition can be used for the rest of the guard statement’s enclosing scope.

The else clause of a guard statement is required, and must either call a function with the Never return type or transfer program control outside the guard statement’s enclosing scope using one of the following statements:

* return
* break
* continue
* throw

Control transfer statements are discussed in Control Transfer Statements below. For more information on functions with the Never return type, see Functions that Never Return.
-->

`guard` 구문의 모든 조건의 값은 타입 `Bool` 이거나 `Bool` 로 브릿지된 타입이어야 합니다. 조건은 [옵셔널 바인딩 (Optional Binding)](../language-guide-1/the-basics.md#optional-binding) 에서 설명한대로 옵셔널 바인딩 선언일 수도 있습니다.

`guard` 구문 조건에서 옵셔널 바인딩 선언으로 할당된 모든 상수 또는 변수 값은 guard 구문을 둘러싼 범위 내에서 사용될 수 있습니다.

`guard` 구문의 `else` 절은 필수이고 `Never` 반환 타입 또는 다음 구문 중 하나를 사용하여 guard 구문을 둘러싼 범위 밖으로 프로그램 제어를 전송해야 합니다:

* `return`
* `break`
* `continue`
* `throw`

제어 전송 구문은 아래 [제어 전송 구문 (Control Transfer Statements)](statements.md#control-transfer-statements) 에서 설명되어 있습니다. Never 반환 타입이 있는 함수에 자세한 내용은 [Never 반환 함수 (Functions that Never Return)](declarations.md#functions-that-never-return) 을 참고 바랍니다.

> GRAMMAR OF A GUARD STATEMENT\
> guard-statement → `guard` [condition-list](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_condition-list) `else` [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)

### Switch 구문 (Switch Statement)

<!--
A switch statement allows certain blocks of code to be executed depending on the value of a control expression.

A switch statement has the following form:
-->

`switch` 구문은 제어 표현식의 값에 따라 특정 코드의 블럭이 실행됩니다.

`switch` 구문은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.17.41.png>)

<!--
The control expression of the switch statement is evaluated and then compared with the patterns specified in each case. If a match is found, the program executes the statements listed within the scope of that case. The scope of each case can’t be empty. As a result, you must include at least one statement following the colon (:) of each case label. Use a single break statement if you don’t intend to execute any code in the body of a matched case.

The values of expressions your code can branch on are very flexible. For example, in addition to the values of scalar types, such as integers and characters, your code can branch on the values of any type, including floating-point numbers, strings, tuples, instances of custom classes, and optionals. The value of the control expression can even be matched to the value of a case in an enumeration and checked for inclusion in a specified range of values. For examples of how to use these various types of values in switch statements, see Switch in Control Flow.

A switch case can optionally contain a where clause after each pattern. A where clause is introduced by the where keyword followed by an expression, and is used to provide an additional condition before a pattern in a case is considered matched to the control expression. If a where clause is present, the statements within the relevant case are executed only if the value of the control expression matches one of the patterns of the case and the expression of the where clause evaluates to true. For example, a control expression matches the case in the example below only if it’s a tuple that contains two elements of the same value, such as (1, 1).
-->

`switch` 구문의 _제어 표현식 (control expression)_ 은 평가된 다음에 각 케이스에 지정한 패턴과 비교됩니다. 일치하는 항목이 있으면 프로그램은 해당 케이스의 범위내에 _구문 (statements)_ 을 실행합니다. 각 케이스의 범위는 비어 있을 수 없습니다. 결과적으로 각 케이스 라벨의 콜론 (`:`) 다음에 적어도 하나의 구문이 포함되어야 합니다. 일치하는 케이스의 본문에서 코드를 실행하지 않으려면 단일 `break` 구문을 사용해야 합니다.

코드가 분기할 수 있는 표현식의 값은 매우 유연합니다. 예를 들어 정수와 문자와 같은 스칼라 타입의 값을 제외하고 코드는 부동 소수점 숫자, 문자열, 튜플, 사용자 정의 클래스의 인스턴스, 그리고 옵셔널을 포함하는 모든 타입의 값으로 분기할 수 있습니다. _제어 표현식 (control expression)_ 의 값은 열거형의 케이스의 값과 일치하고 지정된 값 범위에 포함되는지 확인할 수도 있습니다. `switch` 구문에서 값의 이러한 여러가지 타입을 어떻게 사용하는지에 대한 예제는 [제어 흐름 (Control Flow)](../language-guide-1/control-flow.md) 에 [Switch](../language-guide-1/control-flow.md#switch) 를 참고 바랍니다.

`switch` 케이스는 각 패턴 후에 선택적으로 `where` 절을 포함할 수 있습니다. _where 절 (where clause)_ 은 `where` 키워드 다음에 표현식을 붙여 도입되고 케이스에 패턴이 _제어 표현식 (control expression)_ 에 일치되는지 간주되기 전에 추가 조건을 제공하는데 사용됩니다. `where` 절이 있는 경우 연관된 케이스 내에 _구문 (statements)_ 은 _제어 표현식 (control expression)_ 의 값이 케이스의 패턴 중 하나와 일치하고 `where` 절의 표현식이 `true` 로 평가될 때만 실행됩니다. 예를 들어 _제어 표현식 (control expression)_ 은 `(1, 1)` 과 같이 동일한 값의 두 요소가 포함된 튜플인 경우에만 아래 예제의 케이스는 일치됩니다.

```swift
case let (x, y) where x == y:
```

<!--
As the above example shows, patterns in a case can also bind constants using the let keyword (they can also bind variables using the var keyword). These constants (or variables) can then be referenced in a corresponding where clause and throughout the rest of the code within the scope of the case. If the case contains multiple patterns that match the control expression, all of the patterns must contain the same constant or variable bindings, and each bound variable or constant must have the same type in all of the case’s patterns.

A switch statement can also include a default case, introduced by the default keyword. The code within a default case is executed only if no other cases match the control expression. A switch statement can include only one default case, which must appear at the end of the switch statement.

Although the actual execution order of pattern-matching operations, and in particular the evaluation order of patterns in cases, is unspecified, pattern matching in a switch statement behaves as if the evaluation is performed in source order—that is, the order in which they appear in source code. As a result, if multiple cases contain patterns that evaluate to the same value, and thus can match the value of the control expression, the program executes only the code within the first matching case in source order.
-->

위의 예제에서 볼 수 있듯이 케이스의 패턴은 `let` 키워드를 사용하여 상수에 바인딩 할 수 있고 `var` 키워드를 사용하여 변수에 바인딩 할 수도 있습니다. 이 상수 또는 변수는 해당 where 절과 케이스의 범위 내의 나머지 코드에서 참조될 수 있습니다. 케이스에 제어 표현식과 일치하는 여러 패턴을 포함하는 경우 모든 패턴에는 동일한 상수 또는 변수 바인딩이 포함되어야 하고 바인딩 된 변수 또는 상수는 케이스의 모든 패턴에서 동일한 타입을 가져야 합니다.

`switch` 구문은 `default` 키워드로 도입되는 기본 케이스를 포함할 수도 있습니다. 기본 케이스 내에 코드는 제어 표현식이 일치하는 케이스가 없는 경우에만 실행됩니다. `switch` 구문은 오직 하나의 기본 케이스만 포함되고 `switch` 구문의 마지막에 위치해야 합니다.

패턴-일치 동작 (pattern-matching operations) 의 실제 실행 순서, 그리고 특별히 케이스의 패턴의 평가 순서가 지정되지 않았지만 `switch` 구문의 패턴 일치는 소스 코드가 나타나는 순서대로 소스 순서대로 평가가 수행되도록 동작합니다. 결과적으로 여러 케이스에 동일한 값으로 평가되는 패턴이 포함되어 제어 표현식의 값과 일치할 수 있는 경우 프로그램은 소스 순서대로 일치하는 첫번째 케이스 내의 코드만 실행합니다.

#### Switch 구문은 완벽해야 함 (Switch Statements Must Be Exhaustive)

<!--
In Swift, every possible value of the control expression’s type must match the value of at least one pattern of a case. When this simply isn’t feasible (for example, when the control expression’s type is Int), you can include a default case to satisfy the requirement.
-->

Swift 에서 제어 표현식의 타입에서 가능한 모든 값은 적어도 하나의 케이스의 패턴의 값과 일치해야 합니다. 이것이 가능하지 않은 경우 (예를 들어 제어 표현식의 타입이 `Int` 인 경우) 요구사항을 충족하기 위해 기본 케이스를 포함할 수 있습니다.

#### 향후 열거형 케이스 전환 (Switching Over Future Enumeration Cases)

<!--
A nonfrozen enumeration is a special kind of enumeration that may gain new enumeration cases in the future—even after you compile and ship an app. Switching over a nonfrozen enumeration requires extra consideration. When a library’s authors mark an enumeration as nonfrozen, they reserve the right to add new enumeration cases, and any code that interacts with that enumeration must be able to handle those future cases without being recompiled. Code that’s compiled in library evolution mode, code in the standard library, Swift overlays for Apple frameworks, and C and Objective-C code can declare nonfrozen enumerations. For information about frozen and nonfrozen enumerations, see frozen.

When switching over a nonfrozen enumeration value, you always need to include a default case, even if every case of the enumeration already has a corresponding switch case. You can apply the @unknown attribute to the default case, which indicates that the default case should match only enumeration cases that are added in the future. Swift produces a warning if the default case matches any enumeration case that’s known at compiler time. This future warning informs you that the library author added a new case to the enumeration that doesn’t have a corresponding switch case.

The following example switches over all three existing cases of the standard library’s Mirror.AncestorRepresentation enumeration. If you add additional cases in the future, the compiler generates a warning to indicate that you need to update the switch statement to take the new cases into account.
-->

_비고정 열거형 (nonfrozen enumeration)_ 은 앱을 컴파일하고 출시한 후에도 나중에 새로운 열거형 케이스를 얻을 수 있는 특별한 종류의 열거형입니다. 비고정 열거형을 전환하려면 추가 고려사항이 필요합니다. 라이브러리 작성자가 비고정으로 열거형을 표시할 때 새로운 열거형 케이스를 추가할 권한이 있으며 해당 열거형과 상호작용하는 모든 코드는 다시 컴파일되지 않고 향후 케이스를 처리할 수 있습니다. 라이브러리 진화 모드에서 컴파일된 코드, 표준 라이브러리의 코드, Apple 프레임워크용 Swift 오버레이, 그리고 C 그리고 Objective-C 코드는 비고정 열거형을 선언할 수 있습니다. 고정과 비고정 열거형에 대한 자세한 내용은 [고정 (frozen)](attributes.md#frozen) 을 참고 바랍니다.

비고정 열거형 값을 변경할 때 열서형의 모든 케이스가 해당 전환 케이스가 있더라도 기본 케이스를 포함해야 합니다.향후에 추가된 열거형 케이스에만 일치되는 기본 케이스를 나타내기 위해 기본 케이스에 `@unknown` 속성을 적용할 수 있습니다. Swift 는 컴파일 시에 모든 열거형 케이스와 일치하는 기본 케이스가 있다면 경고를 발생시킵니다. 향후 이 경고는 라이브러리 작성자가 해당 스위치 케이스가 없는 열거형에 새로운 케이스를 추가했음을 알려줍니다.

다음 예제는 표준 라이브러리의 [`Mirror.AncestorRepresentation`](https://developer.apple.com/documentation/swift/mirror/ancestorrepresentation) 열거형의 세가지 기존 케이스를 모두 전환합니다. 향후에 추가 케이스를 추가하면 컴파일러는 새로운 케이스를 고려하기 위해 switch 구문을 업데이트 해야 한다고 나타내는 경고를 생성합니다.

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

<!--
After the code within a matched case has finished executing, the program exits from the switch statement. Program execution doesn’t continue or “fall through” to the next case or default case. That said, if you want execution to continue from one case to the next, explicitly include a fallthrough statement, which simply consists of the fallthrough keyword, in the case from which you want execution to continue. For more information about the fallthrough statement, see Fallthrough Statement below.
-->

일치한 케이스 내에 코드 실행이 완료되면 프로그램은 `switch` 구문을 종료합니다. 프로그램 실행이 계속되지 않거나 다음 케이스 또는 기본 케이스로 "이동 (fall through)" 되지 않습니다. 이 말은 한 케이스에서 다음 케이스로 계속해서 실행을 하려면 계속해서 실행되기 원하는 케이스에 간단하게 `fallthrough` 키워드로 구성된 `fallthrough` 구문을 명시적으로 포함해야 합니다. `fallthrough` 구문에 대한 자세한 내용은 아래의 [이동 구문 (Fallthrough Statement)](statements.md#fallthrough-statement) 을 참고 바랍니다.

> GRAMMAR OF A SWITCH STATEMENT\
> switch-statement → `switch` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `{` [switch-cases](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_switch-cases) $$_{opt}$$ `}`\
> switch-cases → [switch-case](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_switch-case) [switch-cases](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_switch-cases) $$_{opt}$$\
> switch-case → [case-label](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_case-label) [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statements)\
> switch-case → [default-label](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_default-label) [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statements)\
> switch-case → [conditional-switch-case](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_conditional-switch-case)\
> case-label → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ `case` [case-item-list](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_case-item-list) `:`\
> case-item-list → [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern) [where-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_where-clause) $$_{opt}$$ | [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern) [where-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_where-clause) $$_{opt}$$ `,` [case-item-list](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_case-item-list)\
> default-label → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ `default` `:`\
> where-clause → `where` [where-expression](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_where-expression)\
> where-expression → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression)\
> conditional-switch-case → [switch-if-directive-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_switch-if-directive-clause) [switch-elseif-directive-clauses](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_switch-elseif-directive-clauses) $$_{opt}$$ [switch-else-directive-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_switch-else-directive-clause) $$_{opt}$$ [endif-directive](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_endif-directive)\
> switch-if-directive-clause → [if-directive](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_if-directive) [compilation-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compilation-condition) [switch-cases](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_switch-cases) $$_{opt}$$\
> switch-elseif-directive-clauses → [elseif-directive-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_elseif-directive-clause) [switch-elseif-directive-clauses](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_switch-elseif-directive-clauses) $$_{opt}$$\
> switch-elseif-directive-clause → [elseif-directive](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_elseif-directive) [compilation-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compilation-condition) [switch-cases](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_switch-cases) $$_{opt}$$\
> switch-else-directive-clause → [else-directive](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_else-directive) [switch-cases](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_switch-cases) $$_{opt}$$

## 라벨 구문 (Labeled Statement)

<!--
You can prefix a loop statement, an if statement, a switch statement, or a do statement with a statement label, which consists of the name of the label followed immediately by a colon (:). Use statement labels with break and continue statements to be explicit about how you want to change control flow in a loop statement or a switch statement, as discussed in Break Statement and Continue Statement below.

The scope of a labeled statement is the entire statement following the statement label. You can nest labeled statements, but the name of each statement label must be unique.

For more information and to see examples of how to use statement labels, see Labeled Statements in Control Flow.
-->

접두사로 루프 구문, `if` 구문, `switch` 구문, 또는 `do` 구문을 라벨의 이름 뒤에 콜론 (`:`) 으로 구성된 _구문 라벨 (statement label)_ 을 붙일 수 있습니다. 아래 [중단 구문 (Break Statement)](statements.md#break-statement) 과 [계속 구문 (Continue Statement)](statements.md#continue-statement) 에서 설명한대로 루프 구문 또는 `switch` 구문에서 제어 흐름을 변경하려는 방법을 명시하려면 `break` 와 `continue` 구문과 함께 구문 라벨을 사용해야 합니다.

라벨 구문의 범위는 구문 라벨 다음에 오는 전체 구문입니다. 라벨 구문을 중첩할 수 있지만 각 구문 라벨의 이름은 고유해야 합니다.

구문 라벨을 어떻게 사용하는지 자세한 내용과 예제는 [제어 흐름 (Control Flow)](../language-guide-1/control-flow.md) 에 [라벨 구문 (Labeled Statements)](../language-guide-1/control-flow.md#labeled-statements) 을 참고 바랍니다.

> GRAMMAR OF A LABELED STATEMENT\
> labeled-statement → [statement-label](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statement-label) [loop-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_loop-statement)\
> labeled-statement → [statement-label](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statement-label) [if-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_if-statement)\
> labeled-statement → [statement-label](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statement-label) [switch-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_switch-statement)\
> labeled-statement → [statement-label](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statement-label) [do-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_do-statement)\
> statement-label → [label-name](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_label-name) `:`\
> label-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)

## 제어 전송 구문 (Control Transfer Statements)

<!--
Control transfer statements can change the order in which code in your program is executed by unconditionally transferring program control from one piece of code to another. Swift has five control transfer statements: a break statement, a continue statement, a fallthrough statement, a return statement, and a throw statement.
-->

제어 전송 구문 (Control transfer statements) 은 프로그램 제어를 한 코드에서 다른 코드로 무조건 전송하여 프로그램의 코드가 실행되는 순서를 변경할 수 있습니다. Swift 는 `break` 구문, `continue` 구문, `fallthrough` 구문, `return` 구문, 그리고 `throw` 구문의 다섯가지 제어 전송 구문이 있습니다.

> GRAMMAR OF A CONTROL TRANSFER STATEMENT\
> control-transfer-statement → [break-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_break-statement)\
> control-transfer-statement → [continue-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_continue-statement)\
> control-transfer-statement → [fallthrough-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_fallthrough-statement)\
> control-transfer-statement → [return-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_return-statement)\
> control-transfer-statement → [throw-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_throw-statement)

### Break 구문 (Break Statement)

<!--
A break statement ends program execution of a loop, an if statement, or a switch statement. A break statement can consist of only the break keyword, or it can consist of the break keyword followed by the name of a statement label, as shown below.
-->

`break` 구문은 루프, `if` 구문, 또는 `switch` 구문의 프로그램 실행을 종료합니다. `break` 구문은 아래에서 본대로 `break` 키워드로만 구성되거나 `break` 키워드 다음에 구문 라벨의 이름으로 구성될 수 있습니다.

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.22.25.png>)

<!--
When a break statement is followed by the name of a statement label, it ends program execution of the loop, if statement, or switch statement named by that label.

When a break statement isn’t followed by the name of a statement label, it ends program execution of the switch statement or the innermost enclosing loop statement in which it occurs. You can’t use an unlabeled break statement to break out of an if statement.

In both cases, program control is then transferred to the first line of code following the enclosing loop or switch statement, if any.

For examples of how to use a break statement, see Break and Labeled Statements in Control Flow.
-->

`break` 구문 다음에 구문 라벨의 이름이 오면 라벨로 명명된 루프, `if` 구문, 또는 `switch` 구문의 프로그램 실행을 종료합니다.

`break` 구문 다음에 구문 라벨의 이름이 없으면 `switch` 구문 또는 해당 구문이 발생한 가장 안쪽의 루프 구문의 프로그램 실행이 종료됩니다. `if` 구문을 벗어나기 위해 라벨이 없는 `break` 구문을 사용할 수 없습니다.

두 경우 모두 프로그램 제어는 루프나 `switch` 구문 다음 코드의 첫번째 줄로 전송됩니다.

break 구문을 어떻게 사용하는지에 대한 예제는 [제어 흐름 (Control Flow)](../language-guide-1/control-flow.md) 에 [Break](../language-guide-1/control-flow.md#break) 과 [라벨 구문 (Labeled Statements)](../language-guide-1/control-flow.md#labeled-statements) 를 참고 바랍니다.

> GRAMMAR OF A BREAK STATEMENT\
> break-statement → `break` [label-name](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_label-name) $$_{opt}$$

### Continue 구문 (Continue Statement)

<!--
A continue statement ends program execution of the current iteration of a loop statement but doesn’t stop execution of the loop statement. A continue statement can consist of only the continue keyword, or it can consist of the continue keyword followed by the name of a statement label, as shown below.
-->

`continue` 구문은 루프 구문의 현재 반복의 프로그램 실행을 종료하지만 루프 구문의 실행을 멈추지 않습니다. `continue` 구문은 아래에서 보는대로 `continue` 키워드로만 구성되거나 `continue` 키워드 다음에 구문 라벨의 이름으로 구성될 수 있습니다.

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.23.20.png>)

<!--
When a continue statement is followed by the name of a statement label, it ends program execution of the current iteration of the loop statement named by that label.

When a continue statement isn’t followed by the name of a statement label, it ends program execution of the current iteration of the innermost enclosing loop statement in which it occurs.

In both cases, program control is then transferred to the condition of the enclosing loop statement.

In a for statement, the increment expression is still evaluated after the continue statement is executed, because the increment expression is evaluated after the execution of the loop’s body.

For examples of how to use a continue statement, see Continue and Labeled Statements in Control Flow.
-->

`continue` 구문 다음에 구문 라벨의 이름이 따라오면 라벨로 명명된 루프 구문의 현재 반복의 프로그램 실행을 종료합니다.

`continue` 구문 다음에 구문 라벨의 이름이 따라오지 않으면 해당 구문이 발생한 가장 안쪽의 루프 구문의 현재 반복의 프로그램 실행이 종료됩니다.

두 경우 모두 프로그램 제어는 둘러싸인 루프 구문의 조건으로 전송됩니다.

`for` 구문에서 증가 표현식은 루프의 본문이 실행된 후에 평가되므로 `continue` 구문 후에도 계속 계산됩니다.

`continue` 구문 사용에 대한 예제는 [제어 흐름 (Control Flow)](../language-guide-1/control-flow.md) 에 [Continue](../language-guide-1/control-flow.md#continue) 와 [라벨 구문 (Labeled Statements)](../language-guide-1/control-flow.md#labeled-statements) 를 참고 바랍니다.

> GRAMMAR OF A CONTINUE STATEMENT\
> continue-statement → `continue` [label-name](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_label-name) $$_{opt}$$

### Fallthrough 구문 (Fallthrough Statement)

<!--
A fallthrough statement consists of the fallthrough keyword and occurs only in a case block of a switch statement. A fallthrough statement causes program execution to continue from one case in a switch statement to the next case. Program execution continues to the next case even if the patterns of the case label don’t match the value of the switch statement’s control expression.

A fallthrough statement can appear anywhere inside a switch statement, not just as the last statement of a case block, but it can’t be used in the final case block. It also can’t transfer control into a case block whose pattern contains value binding patterns.

For an example of how to use a fallthrough statement in a switch statement, see Control Transfer Statements in Control Flow.
-->

`fallthrough` 구문은 `fallthrough` 키워드로 구성되고 `switch` 구문의 케이스 블럭에서만 발생합니다. `fallthrough` 구문은 `switch` 구문의 한 케이스에서 다음 케이스로 계속해서 프로그램이 실행되도록 합니다. `switch` 구문의 제어 표현식의 값이 케이스 라벨의 패턴과 일치하지 않아도 다음 케이스는 계속 실행됩니다.

`fallthrough` 구문은 케이스 블럭의 마지막 구문 뿐만 아니라 `switch` 구문 내 어디든 나타날 수 있지만 마지막 케이스 블럭에서는 사용될 수 없습니다. 또한 패턴에 값 바인딩 패턴이 포함된 케이스 블럭으로 제어를 전송할 수 없습니다.

`switch` 구문에서 `fallthrough` 구문을 사용하는 방법에 대한 예제는 [제어 흐름 (Control Flow)](../language-guide-1/control-flow.md) 에 [제어 전송 구문 (Control Transfer Statements)](../language-guide-1/control-flow.md#control-transfer-statements) 를 참고 바랍니다.

> GRAMMAR OF A FALLTHROUGH STATEMENT\
> fallthrough-statement → `fallthrough`

### Retrun 구문 (Return Statement)

<!--
A return statement occurs in the body of a function or method definition and causes program execution to return to the calling function or method. Program execution continues at the point immediately following the function or method call.

A return statement can consist of only the return keyword, or it can consist of the return keyword followed by an expression, as shown below.
-->

`return` 구문은 함수 또는 메서드 정의의 본문에서 발생하고 호출한 함수 또는 메서드를 반환하기 위해 실행합니다. 함수 또는 메서드 호출 다음에서 계속해서 실행됩니다.

`return` 구문은 아래에 보는대로 `return` 키워드만으로 구성되거나 `return` 키워드 다음에 표현식으로 구성될 수 있습니다.

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.37.21.png>)

<!--
When a return statement is followed by an expression, the value of the expression is returned to the calling function or method. If the value of the expression doesn’t match the value of the return type declared in the function or method declaration, the expression’s value is converted to the return type before it’s returned to the calling function or method.
-->

`return` 구문 다음에 표현식이 오면 표현식의 값은 호출한 함수 또는 메서드에 반환됩니다. 표현식의 값이 함수 또는 메서드 선언에 선언된 반환 타입의 값과 일치하지 않으면 표현식의 값은 호출한 함수 또는 메서드에 반환되기 전에 반환 타입을 변환합니다.

<!--
NOTE
As described in Failable Initializers, a special form of the return statement (return nil) can be used in a failable initializer to indicate initialization failure.
-->

> NOTE\
> [실패 가능한 초기화 구문 (Failable Initializers)](declarations.md#failable-initializers) 에서 설명한대로 `return` 구문의 특수한 형식 (`return nil`) 은 초기화 실패를 나타내기 위해 실패 가능한 초기화 구문에서 사용될 수 있습니다.

<!--
When a return statement isn’t followed by an expression, it can be used only to return from a function or method that doesn’t return a value (that is, when the return type of the function or method is Void or ()).
-->

`return` 구문 다음에 표현식이 없으면 반환값이 없는 함수 또는 메서드에 반환하기 위해서만 사용될 수 있습니다 (즉, 함수 또는 메서드의 반환 타입이 `Void` 또는 `()` 인 경우에 해당됩니다).

> GRAMMAR OF A RETURN STATEMENT\
> return-statement → `return` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) $$_{opt}$$

### Throw 구문 (Throw Statement)

<!--
A throw statement occurs in the body of a throwing function or method, or in the body of a closure expression whose type is marked with the throws keyword.

A throw statement causes a program to end execution of the current scope and begin error propagation to its enclosing scope. The error that’s thrown continues to propagate until it’s handled by a catch clause of a do statement.

A throw statement consists of the throw keyword followed by an expression, as shown below.
-->

`throw` 구문은 던지는 함수 또는 메서드의 본문에서 발생하거나 타입에 `throws` 키워드가 표시된 클로저 표현식의 본문에서 발생합니다.

`throw` 구문은 프로그램이 현재 범위의 실행을 종료하고 둘러싸인 범위로 에러 전파를 시작합니다. 던져진 에러는 `do` 구문의 `catch` 절에 의해 처리될 때까지 계속 전파됩니다.

`throw` 구문은 아래에서 보는대로 `throw` 키워드 다음에 표현식으로 구성됩니다.

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.39.08.png>)

<!--
The value of the expression must have a type that conforms to the Error protocol.

For an example of how to use a throw statement, see Propagating Errors Using Throwing Functions in Error Handling.
-->

_표현식 (expression)_ 의 값은 `Error` 프로토콜을 준수하는 타입이어야 합니다.

`throw` 구문을 어떻게 사용하는지에 대한 예제는 [에처 처리 (Error Handling)](../language-guide-1/error-handling.md) 에 [던지기 함수를 이용한 에러 전파 (Propagating Errors Using Throwing Functions)](../language-guide-1/error-handling.md#propagating-errors-using-throwing-functions) 을 참고 바랍니다.

> GRAMMAR OF A THROW STATEMENT\
> throw-statement → `throw` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression)

## Defer 구문 (Defer Statement)

<!--
A defer statement is used for executing code just before transferring program control outside of the scope that the defer statement appears in.

A defer statement has the following form:
-->

`defer` 구문은 `defer` 구문이 나타나는 범위의 바깥으로 프로그램 제어가 전송되기 직전에 실행되는 코드에 사용됩니다.

`defer` 구문은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.40.00.png>)

<!--
The statements within the defer statement are executed no matter how program control is transferred. This means that a defer statement can be used, for example, to perform manual resource management such as closing file descriptors, and to perform actions that need to happen even if an error is thrown.

If multiple defer statements appear in the same scope, the order they appear is the reverse of the order they’re executed. Executing the last defer statement in a given scope first means that statements inside that last defer statement can refer to resources that will be cleaned up by other defer statements.
-->

`defer` 구문 내에 구문은 프로그램 제어가 전송되는 것과 관계없이 실행됩니다. 이것은 예를 들어 `defer` 구문은 파일 닫기와 같은 수동 리소스 관리를 수행하고 에러가 발생하더라도 수행되어야 하는 수행해야 하는 작업이 있는 경우에 사용될 수 있습니다.

같은 범위에 여러개의 `defer` 구문이 있다면 나타나는 순서는 실행되는 순서와 반대입니다. 주어진 범위에서 마지막 `defer` 구문을 먼저 실행한다는 것은 마지막 `defer` 구문 내의 구문이 다른 `defer` 구문에 의해 정리될 리소스를 참조할 수 있다는 의미입니다.

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
The statements in the defer statement can’t transfer program control outside of the defer statement.
-->

`defer` 구문에 구문은 `defer` 구문 바깥으로 프로그램 제어를 전송할 수 없습니다.

> GRAMMAR OF A DEFER STATEMENT\
> defer-statement → `defer` [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)

## Do 구문 (Do Statement)

<!--
The do statement is used to introduce a new scope and can optionally contain one or more catch clauses, which contain patterns that match against defined error conditions. Variables and constants declared in the scope of a do statement can be accessed only within that scope.

A do statement in Swift is similar to curly braces ({}) in C used to delimit a code block, and doesn’t incur a performance cost at runtime.

A do statement has the following form:
-->

`do` 구문은 새로운 범위를 도입하기 위해 사용되고 정의된 에러 조건과 일치하는 패턴을 포함하는 하나 이상의 `catch` 절을 선택적으로 포함할 수 있습니다. `do` 구문의 범위에서 선언된 변수와 상수는 해당 범위 내에서만 접근할 수 있습니다.

Swift 에서 `do` 구문은 코드 블럭을 구분하는데 사용되는 C 의 중괄호 (`{}`) 와 유사하며 런타임시 성능 비용이 발생하지 않습니다.

`do` 구문은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.41.19.png>)

<!--
If any statement in the do code block throws an error, program control is transferred to the first catch clause whose pattern matches the error. If none of the clauses match, the error propagates to the surrounding scope. If an error is unhandled at the top level, program execution stops with a runtime error.

Like a switch statement, the compiler attempts to infer whether catch clauses are exhaustive. If such a determination can be made, the error is considered handled. Otherwise, the error can propagate out of the containing scope, which means the error must be handled by an enclosing catch clause or the containing function must be declared with throws.

A catch clause that has multiple patterns matches the error if any of its patterns match the error. If a catch clause contains multiple patterns, all of the patterns must contain the same constant or variable bindings, and each bound variable or constant must have the same type in all of the catch clause’s patterns.

To ensure that an error is handled, use a catch clause with a pattern that matches all errors, such as a wildcard pattern (_). If a catch clause doesn’t specify a pattern, the catch clause matches and binds any error to a local constant named error. For more information about the patterns you can use in a catch clause, see Patterns.

To see an example of how to use a do statement with several catch clauses, see Handling Errors.
-->

`do` 코드 블럭의 구문에서 에러가 발생하면 프로그렘 제어는 패턴이 에러와 일치하는 첫번째 `catch` 절로 전송됩니다. 일치하는 절이 없으면 에러를 주변 범위로 전파합니다. 에러를 최상위 수준에서 처리되지 않으면 프로그램 실행은 런타임 에러와 함께 멈춥니다.

`switch` 구문처럼 컴파일러는 `catch` 절이 완벽한지 여부를 추론하려고 합니다. 그러한 결정을 내릴 수 있으면 에러가 처리된 것으로 간주됩니다. 그렇지 않으면 에러는 포함된 범위 밖으로 전파될 수 있습니다. 이것은 에러는 `catch` 절에서 처리되거나 포함한 함수가 `throws` 로 선언되어야 한다는 의미입니다.

여러 패턴을 가지는 `catch` 절은 패턴 중 하나가 에러와 일치하는 경우에 에러와 일치합니다. `catch` 절에 여러 패턴을 포함하는 경우 모든 패턴은 동일한 상수 또는 변수 바인딩이 포함되어야 하며 각 바인딩 된 변수 또는 상수는 모든 `catch` 절의 패턴에서 동일한 타입을 가져야 합니다.

에러가 처리되도록 보장하려면 와일드카드 패턴 (`_`) 과 같이 모든 에러를 일치하는 패턴으로 `catch` 절을 사용해야 합니다. `catch` 절에 패턴을 지정하지 않으면 `catch` 절은 모든 에러와 일치하고 `error` 라는 이름의 지역 상수에 바인딩 됩니다. `catch` 절에서 사용할 수 있는 패턴에 대한 자세한 내용은 [패턴 (Patterns)](patterns.md) 를 참고 바랍니다.

여러 `catch` 절과 `do` 구문을 사용하는 방법에 대한 예제는 [에러 처리 (Handling Errors)](../language-guide-1/error-handling.md#handling-errors) 를 참고 바랍니다.

> GRAMMAR OF A DO STATEMENT\
> do-statement → `do` [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block) [catch-clauses](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_catch-clauses) $$_{opt}$$\
> catch-clauses → [catch-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_catch-clause) [catch-clauses](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_catch-clauses) $$_{opt}$$\
> catch-clause → `catch` [catch-pattern-list](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_catch-pattern-list) $$_{opt}$$ [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)\
> catch-pattern-list → [catch-pattern](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_catch-pattern) | [catch-pattern](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_catch-pattern) `,` [catch-pattern-list](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_catch-pattern-list)\
> catch-pattern → [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern) [where-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_where-clause) $$_{opt}$$

## 컴파일러 제어 구문 (Compiler Control Statements)

<!--
Compiler control statements allow the program to change aspects of the compiler’s behavior. Swift has three compiler control statements: a conditional compilation block a line control statement, and a compile-time diagnostic statement.
-->

컴파일러 제어 구문은 컴파일러의 동작 측면을 변경할 수 있습니다. Swift 는 조건부 컴파일 블럭 (conditional compilation block), 라인 제어 구문 (line control statement), 그리고 컴파일-시간 진단 구문 (compile-time diagnostic statement) 의 세가지 컴파일러 제어 구문을 가지고 있습니다.

> GRAMMAR OF A COMPILER CONTROL STATEMENT\
> compiler-control-statement → [conditional-compilation-block](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_conditional-compilation-block)\
> compiler-control-statement → [line-control-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_line-control-statement)\
> compiler-control-statement → [diagnostic-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_diagnostic-statement)

### 조건부 컴파일 블럭 (Conditional Compilation Block)

<!--
A conditional compilation block allows code to be conditionally compiled depending on the value of one or more compilation conditions.

Every conditional compilation block begins with the #if compilation directive and ends with the #endif compilation directive. A simple conditional compilation block has the following form:
-->

조건부 컴파일 블럭 (conditional compilation block) 은 하나 이상의 컴파일러 조건의 값에 따라 코드를 조건부로 컴파일 할 수 있습니다.

모든 조건부 컴파일 블럭은 `#if` 컴파일 지시문으로 시작하고 `#endif` 컴파일 지시문으로 끝납니다. 간단한 조건부 컴파일 블럭은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.42.55.png>)

<!--
Unlike the condition of an if statement, the compilation condition is evaluated at compile time. As a result, the statements are compiled and executed only if the compilation condition evaluates to true at compile time.

The compilation condition can include the true and false Boolean literals, an identifier used with the -D command line flag, or any of the platform conditions listed in the table below.
-->

`if` 구문의 조건과 다르게 _컴파일러 조건 (compilation condition)_ 은 컴파일 시에 평가됩니다. 결과적으로 _구문 (statements)_ 은 컴파일 시에 _컴파일러 조건 (compilation condition)_ 이 `true` 로 평가될 때만 컴파일되고 실행됩니다.

_컴파일러 조건 (compilation condition)_ 은 `true` 와 `false` 불린 리터럴 (Boolean literals), `-D` 명령줄 플래그를 사용한 식별자, 또는 아래 표에 나열된 플랫폼 조건이 포함될 수 있습니다.

|         플랫폼 조건        |                         유효 인수                         |
| :-------------------: | :---------------------------------------------------: |
|         `os()`        | `macOS`, `iOS`, `watchOS`, `tvOS`, `Linux`, `Windows` |
|        `arch()`       |            `i386`, `x86_64`, `arm`, `arm64`           |
|       `swift()`       |                 `>=` 또는 `<` 다음에 버전 번호                 |
|      `compiler()`     |                 `>=` 또는 `<` 다음에 버전 번호                 |
|     `canImport()`     |                         모듈 이름                         |
| `targetEnvironment()` |               `simulator`, `macCatalyst`              |

<!--
The version number for the swift() and compiler() platform conditions consists of a major number, optional minor number, optional patch number, and so on, with a dot (.) separating each part of the version number. There must not be whitespace between the comparison operator and the version number. The version for compiler() is the compiler version, regardless of the Swift version setting passed to the compiler. The version for swift() is the language version currently being compiled. For example, if you compile your code using the Swift 5 compiler in Swift 4.2 mode, the compiler version is 5 and the language version is 4.2. With those settings, the following code prints all three messages:
-->

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

<!--
The argument for the canImport() platform condition is the name of a module that may not be present on all platforms. The module can include periods (.) in its name. This condition tests whether it’s possible to import the module, but doesn’t actually import it. If the module is present, the platform condition returns true; otherwise, it returns false.

The targetEnvironment() platform condition returns true when code is being compiled for the specified environment; otherwise, it returns false.
-->

`canImport()` 플랫폼 조건에 대한 인수는 모든 플랫폼에 존재하지 않을 수 있는 모듈의 이름입니다. 모듈은 이름에 마침표 (`.`)를 포함할 수 있습니다. 이 조건은 해당 모듈을 가져올 수 있는지 검사하지만 실제로 가져오지 않습니다. 모듈이 있으면 플랫폼 조건은 `true` 를 반환하고 그렇지 않으면 `false` 를 반환합니다.

`targetEnvironment()` 플랫폼 조건은 지정한 환경에 대해 코드가 컴파일 되면 `true` 를 반환하고 그렇지 않으면 `false` 를 반환합니다.

<!--
NOTE
The arch(arm) platform condition doesn’t return true for ARM 64 devices. The arch(i386) platform condition returns true when code is compiled for the 32–bit iOS simulator.
-->

> NOTE\
> `arch(arm)` 플랫폼 조건은 ARM 64 기기에 대해 `true` 를 반환하지 않습니다. `arch(i386)` 플랫폼 조건은 32-비트 iOS 시뮬레이터에 대해 코드가 컴파일 될 때 `true` 를 반환합니다.

<!--
You can combine and negate compilation conditions using the logical operators &&, ||, and ! and use parentheses for grouping. These operators have the same associativity and precedence as the logical operators that are used to combine ordinary Boolean expressions.

Similar to an if statement, you can add multiple conditional branches to test for different compilation conditions. You can add any number of additional branches using #elseif clauses. You can also add a final additional branch using an #else clause. Conditional compilation blocks that contain multiple branches have the following form:
-->

논리 연산자 `&&`, `||`, 그리고 `!` 사용하여 컴파일러 조건을 결합하고 부정할 수 있고 그룹화를 위한 소괄호를 사용할 수 있습니다. 이러한 연산자는 불린 표현식을 결합하는데 사용되는 논리 연산자와 동일한 연관성과 우선순위를 갖습니다.

`if` 구문과 유사하게 다른 컴파일러 조건에 대해 검사하기 위해 여러 조건부 분기를 추가할 수 있습니다. `#elseif` 절을 사용하여 원하는 만큼 추가 분기를 추가할 수 있습니다. `#else` 절을 사용하여 마지막 추가 분기를 추가할 수도 있습니다. 여러개 분기를 포함하는 조건부 컴파일러 블럭 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.47.08.png>)

<!--
NOTE
Each statement in the body of a conditional compilation block is parsed even if it’s not compiled. However, there’s an exception if the compilation condition includes a swift() or compiler() platform condition: The statements are parsed only if the language or compiler version matches what is specified in the platform condition. This exception ensures that an older compiler doesn’t attempt to parse syntax introduced in a newer version of Swift.
-->

> NOTE\
> 조건부 컴파일러 블럭의 본문에서 각 구문은 컴파일 되지 않아도 구문 분석됩니다. 그러나 컴파일러 조건이 swift() 또는 compiler() 플랫폼 조건을 포함하면 예외가 있습니다: 이 구문은 플랫폼 조건에서 지정한 언어 또는 컴파일러 버전이 일치하는 경우에만 분석됩니다. 이 예외는 이전 컴파일러가 최신 버전의 Swift 에 도입된 구문을 분석하지 않도록 합니다.

<!--
For information about how you can wrap explicit member expressions in conditional compilation blocks, see Explicit Member Expression.
-->

조건부 컴파일러 블럭에서 명시적 멤버 표현식을 래핑하는 방법에 대한 자세한 내용은 [명시적 멤버 표현식 (Explicit Member Expression)](expressions.md#explicit-member-expression) 을 참고하시기 바랍니다.

> GRAMMAR OF A CONDITIONAL COMPILATION BLOCK\
> conditional-compilation-block → [if-directive-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_if-directive-clause) [elseif-directive-clauses](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_elseif-directive-clauses) $$_{opt}$$ [else-directive-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_else-directive-clause) $$_{opt}$$ [endif-directive](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_endif-directive)\
> if-directive-clause → [if-directive](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_if-directive) [compilation-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compilation-condition) [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statements) $$_{opt}$$\
> elseif-directive-clauses → [elseif-directive-clause](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_elseif-directive-clause) [elseif-directive-clauses](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_elseif-directive-clauses) $$_{opt}$$\
> elseif-directive-clause → [elseif-directive](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_elseif-directive) [compilation-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compilation-condition) [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statements) $$_{opt}$$\
> else-directive-clause → [else-directive](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_else-directive) [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statements) $$_{opt}$$\
> if-directive → `#if`\
> elseif-directive → `#elseif`\
> else-directive → `#else`\
> endif-directive → `#endif`\
> compilation-condition → [platform-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_platform-condition)\
> compilation-condition → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> compilation-condition → [boolean-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_boolean-literal)\
> compilation-condition → `(` [compilation-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compilation-condition) `)`\
> compilation-condition → `!` [compilation-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compilation-condition)\
> compilation-condition → [compilation-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compilation-condition) `&&` [compilation-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compilation-condition)\
> compilation-condition → [compilation-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compilation-condition) `||` [compilation-condition](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compilation-condition)\
> platform-condition → `os` `(` [operating-system](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_operating-system) `)`\
> platform-condition → `arch` `(` [architecture](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_architecture) `)`\
> platform-condition → `swift` `(` `>=` [swift-version](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_swift-version) `)` | `swift` `(` `<` [swift-version](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_swift-version) `)`\
> platform-condition → `compiler` `(` `>=` [swift-version](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_swift-version) `)` | `compiler` `(` `<` [swift-version](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_swift-version) `)`\
> platform-condition → `canImport` `(` [module-name](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_module-name) `)`\
> platform-condition → `targetEnvironment` `(` [environment](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_environment) `)`\
> operating-system → `macOS` | `iOS` | `watchOS` | `tvOS` | `Linux` | `Windows`\
> architecture → `i386` | `x86_64` | `arm` | `arm64`\
> swift-version → [decimal-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_decimal-digits) [swift-version-continuation](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_swift-version-continuation) $$_{opt}$$\
> swift-version-continuation → `.` [decimal-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_decimal-digits) [swift-version-continuation](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_swift-version-continuation) $$_{opt}$$\
> environment → `simulator` | `macCatalyst`

### 라인 제어 구문 (Line Control Statement)

<!--
A line control statement is used to specify a line number and filename that can be different from the line number and filename of the source code being compiled. Use a line control statement to change the source code location used by Swift for diagnostic and debugging purposes.

A line control statement has the following forms:
-->

라인 제어 구문 (line control statement) 은 컴파일 되는 소스 코드의 라인 숫자와 파일 이름이 다를 수 있는 라인 숫자와 파일 이름을 지정하는데 사용됩니다. Swift 에서 진단과 디버깅을 목적으로 사용하는 소스 코드 위치를 변경하기 위해 라인 제어 구문을 사용합니다.

라인 제어 구문은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.52.02.png>)

<!--
The first form of a line control statement changes the values of the #line, #file, #fileID, and #filePath literal expressions, beginning with the line of code following the line control statement. The line number changes the value of #line, and is any integer literal greater than zero. The file path changes the value of #file, #fileID, and #filePath, and is a string literal. The specified string becomes the value of #filePath, and the last path component of the string is used by the value of #fileID. For information about #file, #fileID, and #filePath, see Literal Expression.

The second form of a line control statement, #sourceLocation(), resets the source code location back to the default line numbering and file path.
-->

라인 제어 구문의 첫번째 형식은 라인 제어 구문 다음에 코드의 라인에서 시작하여 `#line`, `#file`, `#fileID`, 그리고 `#filePath` 리터럴 표현식의 값을 변경합니다. _라인 숫자 (line number)_ 는 `#line` 의 값을 변경하고 0 보다 큰 모든 정수 리터럴 입니다. _파일 경로 (file path)_ 는 `#file`, `#fileID`, 그리고 `#filePath` 의 값을 변경하고 문자열 리터럴 입니다. 지정한 문자열은 `#filePath` 의 값이 되고 문자열의 마지막 구성요소는 `#fileID` 의 값으로 사용됩니다. `#file`, `#fileID`, 그리고 `#filePath` 에 자세한 내용은 [리터럴 표현식 (Literal Expression)](expressions.md#literal-expression) 을 참고 바랍니다.

라인 제어 구문의 두번째 형식 인 `#sourceLocation()` 은 소스 코드 위치를 기본 라인 숫자와 파일 경로로 초기화 합니다.

> GRAMMAR OF A LINE CONTROL STATEMENT\
> line-control-statement → `#sourceLocation` `(` `file:` [file-path](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_file-path) `,` `line:` [line-number](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_line-number) `)`\
> line-control-statement → `#sourceLocation` `(` `)`\
> line-number → 0 보다 큰 10진 정수\
> file-path → [static-string-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_static-string-literal)

### 컴파일-시간 진단 구문 (Compile-Time Diagnostic Statement)

<!--
A compile-time diagnostic statement causes the compiler to emit an error or a warning during compilation. A compile-time diagnostic statement has the following forms:
-->

컴파일-시간 진단 구문 (compile-time diagnostic statement) 은 컴파일 동안 컴파일러에서 에러 또는 경고가 발생합니다. 컴파일-시간 진단 구문은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.53.25.png>)

<!--
The first form emits the error message as a fatal error and terminates the compilation process. The second form emits the warning message as a nonfatal warning and allows compilation to proceed. You write the diagnostic message as a static string literal. Static string literals can’t use features like string interpolation or concatenation, but they can use the multiline string literal syntax.
-->

첫번째 형식은 치명적인 에러로 _에러 메세지 (error message)_ 를 내보내고 컴파일 프로세스를 중단합니다. 두번째 형식은 치명적이지 않은 경고로 _경고 메세지 (warning message)_ 를 내보냅니다. 정적 문자열 리터럴 (static string literal) 로 진단 메세지를 작성합니다. 정적 문자열 리터럴은 문자열 보간 또는 연결과 같은 기능을 사용할 수 없지만 여러줄 문자열 리터럴 구문은 사용할 수 있습니다.

> GRAMMAR OF A COMPILE-TIME DIAGNOSTIC STATEMENT\
> diagnostic-statement → `#error` `(` [diagnostic-message](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_diagnostic-message) `)`\
> diagnostic-statement → `#warning` `(` [diagnostic-message](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_diagnostic-message) `)`\
> diagnostic-message → [static-string-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_static-string-literal)

## 가용성 조건 (Availability Condition)

<!--
An availability condition is used as a condition of an if, while, and guard statement to query the availability of APIs at runtime, based on specified platforms arguments.

An availability condition has the following form:
-->

_가용성 조건 (availability condition)_ 은 지정된 플랫폼 인수를 기반으로 런타임 시에 API 의 가용성을 조사하기 위해 `if`, `while`, 그리고 `guard` 구문의 조건으로 사용됩니다.

가용성 조건은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 10.54.34.png>)

<!--
You use an availability condition to execute a block of code, depending on whether the APIs you want to use are available at runtime. The compiler uses the information from the availability condition when it verifies that the APIs in that block of code are available.

The availability condition takes a comma-separated list of platform names and versions. Use iOS, macOS, watchOS, and tvOS for the platform names, and include the corresponding version numbers. The * argument is required and specifies that on any other platform, the body of the code block guarded by the availability condition executes on the minimum deployment target specified by your target.

Unlike Boolean conditions, you can’t combine availability conditions using logical operators like && and ||. Instead of using ! to negate an availability condition, use an unavailability condition, which has the following form:
-->

런타임 시 API 가 사용가능 여부에 따라 코드의 블럭을 실행하기 위해 가용성 조건을 사용합니다. 컴파일러는 코드의 블럭이 가능한 API 인 경우 가용성 조건에서 정보를 사용합니다.

가용성 조건은 콤마로 구분된 플랫폼 이름과 버전을 가집니다. 플랫폼 이름으로 `iOS`, `macOS`, `watchOS`, 그리고 `tvOS` 를 사용하고 해당 버전 숫자를 포함합니다. `*` 인수는 필수이고 다른 플랫폼에서 가용성 조건으로 보호되는 코드 블럭의 본문이 타겟에 지정한 최소 배포 타겟에서 실행되도록 합니다.

불린 조건과 다르게 `&&` 와 `||` 와 같은 논리 연산자를 사용하여 가용성 조건을 결합할 수 없습니다. 비가용성 조건을 사용하기 위해 가용성 조건에 부정을 나타내기 위해 `!` 을 사용하는 대신에 다음의 형식을 가질 수 있습니다:

![](<../.gitbook/assets/unavailability_form.png>)

<!--
The #unavailable form is syntactic sugar that negates the condition. In an unavailability condition, the * argument is implicit and must not be included. It has the same meaning as the * argument in an availability condition.
-->

`#unavailable` 형식은 조건을 부정하는 구문입니다. 비가용성 조건에서 `*` 인수는 암시적이고 포함되어서는 안됩니다. `*` 의 의미는 가용성 조건과 같은 의미를 가집니다.

> GRAMMAR OF AN AVAILABILITY CONDITION\
> availability-condition → `#available` `(` [availability-arguments](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_availability-arguments) `)`\
> availability-condition → `#unavailable` `(` [availability-arguments](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_availability-arguments) `)`\
> availability-arguments → [availability-argument](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_availability-argument) | [availability-argument](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_availability-argument) `,` [availability-arguments](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_availability-arguments)\
> availability-argument → [platform-name](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_platform-name) [platform-version](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_platform-version)\
> availability-argument → `*`\
> platform-name → `iOS` | `iOSApplicationExtension`\
> platform-name → `macOS` | `macOSApplicationExtension`\
> platform-name → `macCatalyst` | `macCatalystApplicationExtension`\
> platform-name → `watchOS` | `watchOSApplicationExtension`\
> platform-name → `tvOS` | `tvOSApplicationExtension`\
> platform-version → [decimal-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_decimal-digits)\
> platform-version → [decimal-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_decimal-digits) `.` [decimal-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_decimal-digits)\
> platform-version → [decimal-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_decimal-digits) `.` [decimal-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_decimal-digits) `.` [decimal-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_decimal-digits)
