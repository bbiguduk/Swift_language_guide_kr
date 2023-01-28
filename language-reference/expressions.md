# 표현식 (Expressions)

<!--
In Swift, there are four kinds of expressions: prefix expressions, infix expressions, primary expressions, and postfix expressions. Evaluating an expression returns a value, causes a side effect, or both.

Prefix and infix expressions let you apply operators to smaller expressions. Primary expressions are conceptually the simplest kind of expression, and they provide a way to access values. Postfix expressions, like prefix and infix expressions, let you build up more complex expressions using postfixes such as function calls and member access. Each kind of expression is described in detail in the sections below.
-->

Swift 에서 접두사 표현식 (prefix expressions), 이진 표현식 (binary expressions), 기본 표현식 (primary expressions), 그리고 접미사 표현식 (postfix expressions) 의 네가지 표현식이 있습니다. 표현식을 수행하면 값을 반환하거나 에러가 발생하거나 둘다 발생합니다.

접두사 그리고 이진 표현식은 더 작은 표현식에 연산자를 적용할 수 있습니다. 기본 표현식은 개념적으로 가장 간단한 표현식이며 값을 접근하는 방법을 제공합니다. 접두사와 이진 표현식과 같이 접미사 표현식은 함수 호출과 멤버 접근과 같이 접미사를 사용하여 더 복잡한 표현식을 작성할 수 있습니다. 표현식의 각 종류는 아래 섹션에서 자세하게 설명 합니다.

> GRAMMAR OF AN EXPRESSION\
> expression → [try-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_try-operator) $$_{opt}$$ [await-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_await-operator) $$_{opt}$$ [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_prefix-expression) [binary-expressions](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_binary-expressions) $$_{opt}$$\
> expression-list → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) | [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `,` [expression-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression-list)

## 접두사 표현식 (Prefix Expressions)

<!--
Prefix expressions combine an optional prefix operator with an expression. Prefix operators take one argument, the expression that follows them.

For information about the behavior of these operators, see Basic Operators and Advanced Operators.

For information about the operators provided by the Swift standard library, see Operator Declarations.
-->

_접두사 표현식 (Prefix expressions)_ 는 옵셔널 접두사 연산자 (prefix operator) 와 표현식을 결합합니다. 접두사 연산자는 그 뒤에 오는 표현식 인 하나의 인수를 가집니다.

이 연산자의 동작에 대한 자세한 설명은 [기본 연산자 (Basic Operators)](../language-guide-1/basic-operators.md) 와 [고급 연산자 (Advanced Operators)](../language-guide-1/advanced-operators.md) 를 참고 바랍니다.

Swift 표준 라이브러리에 의해 제공되는 연산자의 자세한 설명은 [연산자 선언 (Operator Declarations)](https://developer.apple.com/documentation/swift/swift\_standard\_library/operator\_declarations) 를 참고 바랍니다.

> GRAMMAR OF A PREFIX EXPRESSION\
> prefix-expression → [prefix-operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_prefix-operator) $$_{opt}$$ [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression)\
> prefix-expression → [in-out-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_in-out-expression)

### In-Out 표현식 (In-Out Expression)

<!--
An in-out expression marks a variable that’s being passed as an in-out argument to a function call expression.
-->

_in-out 표현식 (in-out expression)_ 은 in-out 인수로 함수 호출 표현식에 전달되는 변수를 표시합니다.

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.26.56.png>)

<!--
For more information about in-out parameters and to see an example, see In-Out Parameters.

In-out expressions are also used when providing a non-pointer argument in a context where a pointer is needed, as described in Implicit Conversion to a Pointer Type.
-->

in-out 파라미터에 대한 설명과 예제는 [In-Out 파라미터 (In-Out Parameters)](../language-guide-1/functions.md#in-out-in-out-parameters) 를 참고 바랍니다.

in-out 표현식은 [포인터 타입에 대한 암시적 변환 (Implicit Conversion to a Pointer Type)](expressions.md#implicit-conversion-to-a-pointer-type) 에서 설명한 대로 포인터가 필요한 컨텍스트에서 비포인터 인수 (non-pointer argument) 를 제공할 때도 사용됩니다.

> GRAMMAR OF AN IN-OUT EXPRESSION\
> in-out-expression → `&` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)

### Try 연산자 (Try Operator)

<!--
A try expression consists of the try operator followed by an expression that can throw an error. It has the following form:
-->

_try 표현식 (try expression)_ 은 에러를 던질 수 있게 `try` 연산자 다음에 표현식으로 구성됩니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.28.00.png>)

<!--
The value of a try expression is the value of the expression.

An optional-try expression consists of the try? operator followed by an expression that can throw an error. It has the following form:
-->

_옵셔널 try 표현식 (optional-try expression)_ 은 에러를 던질 수 있게 `try?` 연산자 다음에 표현식으로 구성됩니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.28.35.png>)

<!--
If the expression doesn’t throw an error, the value of the optional-try expression is an optional containing the value of the expression. Otherwise, the value of the optional-try expression is nil.

A forced-try expression consists of the try! operator followed by an expression that can throw an error. It has the following form:
-->

_표현식_ 이 에러를 던지지 않는다면 옵셔널 try 표현식의 값은 _표현식_ 의 값을 포함하는 옵셔널입니다. 그렇지 않으면 옵셔널 try 표현식의 값은 `nil` 입니다.

_강제 try 표현식 (forced-try expression)_ 은 에러를 던질 수 있게 `try!` 연산자 다음에 표현식으로 구성됩니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.29.08.png>)

<!--
The value of a forced-try expression is the value of the expression. If the expression throws an error, a runtime error is produced.

When the expression on the left-hand side of an infix operator is marked with try, try?, or try!, that operator applies to the whole infix expression. That said, you can use parentheses to be explicit about the scope of the operator’s application.
-->

강제 try 표현식의 값은 표현식의 값입니다. _표현식_ 이 에러를 던지면 런타임 에러가 발생합니다.

중위 연산자의 왼쪽 표현식에 `try`, `try?`, 또는 `try!` 로 표시되면 해당 연산자는 중위 표현식 전체에 적용됩니다. 즉, 괄호를 사용하여 명시적으로 연산자의 적용 범위를 명시할 수 있습니다.

```swift
// try applies to both function calls
sum = try someThrowingFunction() + anotherThrowingFunction()

// try applies to both function calls
sum = try (someThrowingFunction() + anotherThrowingFunction())

// Error: try applies only to the first function call
sum = (try someThrowingFunction()) + anotherThrowingFunction()
```

<!--
A try expression can’t appear on the right-hand side of an infix operator, unless the infix operator is the assignment operator or the try expression is enclosed in parentheses.

If an expression includes both the try and await operator, the try operator must appear first.

For more information and to see examples of how to use try, try?, and try!, see Error Handling.
-->

중위 연산자가 할당 연산자 이거나 `try` 표현식이 괄호로 묶여있지 않으면 `try` 표현식은 중위 연산자의 오른쪽에 나타날 수 없습니다.

더 자세한 정보와 `try`, `try?`, 그리고 `try!` 사용법에 대한 예제는 [에러 처리 (Error Handling)](../language-guide-1/error-handling.md) 을 참고 바랍니다.

> GRAMMAR OF A TRY EXPRESSION\
> try-operator → `try` | `try` `?` | `try` `!`

## Await 연산자 (Await Operator)

<!--
An await expression consists of the await operator followed by an expression that uses the result of an asynchronous operation. It has the following form:
-->

_await 표현식 (await expression)_ 은 `await` 연산자 다음에 비동기 동작의 결과를 사용하는 표현식으로 구성됩니다. 형식은 다음과 같습니다:

![](../.gitbook/assets/await\_expression.png)

<!--
The value of an await expression is the value of the expression.

An expression marked with await is called a potential suspension point. Execution of an asynchronous function can be suspended at each expression that’s marked with await. In addition, execution of concurrent code is never suspended at any other point. This means code between potential suspension points can safely update state that requires temporarily breaking invariants, provided that it completes the update before the next potential suspension point.

An await expression can appear only within an asynchronous context, such as the trailing closure passed to the async(priority:operation:) function. It can’t appear in the body of a defer statement, or in an autoclosure of synchronous function type.

When the expression on the left-hand side of an infix operator is marked with the await operator, that operator applies to the whole infix expression. That said, you can use parentheses to be explicit about the scope of the operator’s application.
-->

`await` 표현식의 값은 _표현식 (expression)_ 의 값입니다.

`await` 로 표시된 표현식을 _잠재적 중단 지점 (potential suspension point)_ 이라 합니다. 비동기 함수의 실행은 `await` 로 표시된 각 표현식에서 일시 중단될 수 있습니다. 또한 동시 코드의 실행은 다른 시점에서 중단되지 않습니다. 이는 잠재적 중단 지점 사이의 코드가 다음 잠재적 중단 지점 이전에 업데이트를 완료하는 경우 일시적으로 중단을 깨야하는 상태를 안전하게 업데이트 할 수 있음을 의미합니다.

`await` 표현식은 `async(priority:operation:)` 함수에 전달된 후행 클로저와 같은 비동기 컨텍스트 내에서만 나타날 수 있습니다. `defer` 구문의 본문이나 동기 함수 타입의 자동 클로저 (autoclosure) 내에서는 나타날 수 없습니다.

중위 연산자 (infix operator) 의 좌항에 `await` 연산자로 표시되면 해당 연산자는 전체 중위 표현식에 적용됩니다. 즉, 괄호를 사용하여 연산자의 적용 범위를 명시할 수 있습니다.

```swift
// await applies to both function calls
sum = await someAsyncFunction() + anotherAsyncFunction()

// await applies to both function calls
sum = await (someAsyncFunction() + anotherAsyncFunction())

// Error: await applies only to the first function call
sum = (await someAsyncFunction()) + anotherAsyncFunction()
```

<!--
An await expression can’t appear on the right-hand side of an infix operator, unless the infix operator is the assignment operator or the await expression is enclosed in parentheses.

If an expression includes both the await and try operator, the try operator must appear first.
-->

`await` 표현식은 중위 연산자가 할당 연산자 이거나 `await` 표현식이 괄호로 묶인 경우가 아니면 중위 연산자의 우항에 나타날 수 없습니다.

표현식에 `await` 와 `try` 연산자가 모두 포함되면 `try` 연산자가 먼저 나타나야 합니다.

> GRAMMAR OF AN AWAIT EXPRESSION\
> await-operator → `await`

## 중위 표현식 (Infix Expressions)

<!--
Infix expressions combine an infix binary operator with the expression that it takes as its left- and right-hand arguments. It has the following form:
-->

_중위 표현식 (Infix expressions)_ 은 좌항과 우항 인수를 가지는 표현식과 중위 이항 연산자 (infix binary operator) 를 결합합니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.31.00.png>)

<!--
For information about the behavior of these operators, see Basic Operators and Advanced Operators.

For information about the operators provided by the Swift standard library, see Operator Declarations.
-->

이 연산자의 동작에 대한 자세한 설명은 [기본 연산자 (Basic Operators)](../language-guide-1/basic-operators.md) 와 [고급 연산자 (Advanced Operators)](../language-guide-1/advanced-operators.md) 를 참고 바랍니다.

Swift 표준 라이브러리에 의해 제공되는 연산자에 대한 자세한 내용은 [연산자 선언 (Operator Declarations)](https://developer.apple.com/documentation/swift/swift\_standard\_library/operator\_declarations) 를 참고 바랍니다.

<!--
NOTE
At parse time, an expression made up of infix operators is represented as a flat list. This list is transformed into a tree by applying operator precedence. For example, the expression 2 + 3 * 5 is initially understood as a flat list of five items, 2, +, 3, *, and 5. This process transforms it into the tree (2 + (3 * 5)).
-->

> NOTE\
> 구문 분석 시 중위 연산자로 구성된 표현식은 단순 목록으로 표현됩니다. 이 목록은 연산자 우선순위를 적용하여 트리로 변환됩니다. 예를 들어 표현식 `2 + 3 * 5` 는 처음에는 5개의 항목 `2`, `+`, `3`, `*`, 그리고 `5` 의 단순 목록으로 이해됩니다. 이 프로세스는 트리 (2 + (3 \* 5)) 로 변환합니다.
>
> GRAMMAR OF A INFIX EXPRESSION\
> infix-expression → [infix-operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_infix-operator) [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_prefix-expression)\
> infix-expression → [assignment-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_assignment-operator) [try-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_try-operator) $$_{opt}$$ [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_prefix-expression)\
> infix-expression → [conditional-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_conditional-operator) [try-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_try-operator) $$_{opt}$$ [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_prefix-expression)\
> infix-expression → [type-casting-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_type-casting-operator)\
> infix-expressions → [infix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_infix-expression) [infix-expressions](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_infix-expressions) $$_{opt}$$

### 할당 연산자 (Assignment Operator)

<!--
The assignment operator sets a new value for a given expression. It has the following form:
-->

_할당 연산자 (assignment operator)_ 는 주어진 표현식에 대해 새로운 값을 설정합니다. 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.32.26.png>)

<!--
The value of the expression is set to the value obtained by evaluating the value. If the expression is a tuple, the value must be a tuple with the same number of elements. (Nested tuples are allowed.) Assignment is performed from each part of the value to the corresponding part of the expression. For example:
-->

_표현식_ 의 값은 평가한 _값_ 에 의해 얻어진 값으로 설정됩니다. _표현식_ 이 튜플이면 _값_ 은 동일한 수의 요소의 튜플이어야 합니다. (중첩된 튜플도 가능합니다.) _값_ 의 각 부분에서 _표현식_ 의 해당 부분으로 할당이 수행됩니다. 예를 들어:

```swift
(a, _, (b, c)) = ("test", 9.45, (12, 3))
// a is "test", b is 12, c is 3, and 9.45 is ignored
```

<!--
The assignment operator doesn’t return any value.
-->

할당 연산자는 모든 값을 반환하지 않습니다.

> GRAMMAR OF AN ASSIGNMENT OPERATOR\
> assignment-operator → `=`

### 삼항 조건 연산자 (Ternary Conditional Operator)

<!--
The ternary conditional operator evaluates to one of two given values based on the value of a condition. It has the following form:
-->

_삼항 조건 연산자 (ternary conditional operator)_ 는 조건의 값을 기반으로 주어진 두 개의 값 중 하나로 나타냅니다. 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.33.56.png>)

<!--
If the condition evaluates to true, the conditional operator evaluates the first expression and returns its value. Otherwise, it evaluates the second expression and returns its value. The unused expression isn’t evaluated.

For an example that uses the ternary conditional operator, see Ternary Conditional Operator.
-->

_조건_ 이 `true` 이면 조건부 연산자는 첫번째 표현식을 평가하고 해당 값을 반환합니다. 그렇지 않으면 두번째 표현식을 평가하고 그것의 값을 반환합니다. 사용하지 않은 표현식은 평가되지 않습니다.

삼항 조건 연산자 사용에 대한 예제는 [삼항 조건 연산자 (Ternary Conditional Operator)](../language-guide-1/basic-operators.md#ternary-conditional-operator) 를 참고 바랍니다.

> GRAMMAR OF A CONDITIONAL OPERATOR\
> conditional-operator → `?` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `:`

### 타입 캐스팅 연산자 (Type-Casting Operators)

<!--
There are four type-casting operators: the is operator, the as operator, the as? operator, and the as! operator.

They have the following form:
-->

`is` 연산자, `as` 연산자, `as?` 연산자, 그리고 `as!` 연산자 인 4개의 타입 캐스팅 연산자 (type-casting operators) 가 있습니다.

다음의 형식을 가지고 있습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.35.04.png>)

<!--
The is operator checks at runtime whether the expression can be cast to the specified type. It returns true if the expression can be cast to the specified type; otherwise, it returns false.

The as operator performs a cast when it’s known at compile time that the cast always succeeds, such as upcasting or bridging. Upcasting lets you use an expression as an instance of its type’s supertype, without using an intermediate variable. The following approaches are equivalent:
-->

`is` 연산자는 _표현식_ 이 지정된 _타입_ 으로 캐스팅 될 수 있는지 런타임 시에 확인합니다. _표현식_ 이 지정된 _타입_ 으로 캐스팅 될 수 있으면 `true` 를 반환하고 그렇지 않으면 `false` 를 반환합니다.

`as` 연산자는 업캐스팅 (upcasting) 또는 브릿징 (bridging) 과 같이 캐스트가 항상 성공하는 것으로 컴파일 시 알려진 경우에 캐스팅을 수행합니다. 업캐스팅 (Upcasting) 을 사용하면 중간 변수를 사용하지 않고 표현식을 해당 타입의 상위 타입 인스턴스로 사용할 수 있습니다. 다음 접근은 동일합니다:

```swift
func f(_ any: Any) { print("Function for Any") }
func f(_ int: Int) { print("Function for Int") }
let x = 10
f(x)
// Prints "Function for Int"

let y: Any = x
f(y)
// Prints "Function for Any"

f(x as Any)
// Prints "Function for Any"
```

<!--
Bridging lets you use an expression of a Swift standard library type such as String as its corresponding Foundation type such as NSString without needing to create a new instance. For more information on bridging, see Working with Foundation Types.

The as? operator performs a conditional cast of the expression to the specified type. The as? operator returns an optional of the specified type. At runtime, if the cast succeeds, the value of expression is wrapped in an optional and returned; otherwise, the value returned is nil. If casting to the specified type is guaranteed to fail or is guaranteed to succeed, a compile-time error is raised.

The as! operator performs a forced cast of the expression to the specified type. The as! operator returns a value of the specified type, not an optional type. If the cast fails, a runtime error is raised. The behavior of x as! T is the same as the behavior of (x as? T)!.

For more information about type casting and to see examples that use the type-casting operators, see Type Casting.
-->

브릿징 (Bridging) 을 사용하면 새로운 인스턴스를 생성할 필요없이 `NSString` 과 같은 해당 Foundation 타입으로 `String` 과 같은 Swift 표준 라이브러리 타입의 표현식으로 사용할 수 있습니다. 브릿징에 대한 자세한 설명은 [Foundation 타입 동작 (Working with Foundation Types)](https://developer.apple.com/documentation/swift/imported\_c\_and\_objective-c\_apis/working\_with\_foundation\_types) 를 참고 바랍니다.

`as?` 연산자는 지정한 _타입_ 으로 _표현식_ 의 조건부 캐스팅을 수행합니다. `as?` 연산자는 지정한 _타입_ 의 옵셔널로 반환합니다. 런타임에 캐스팅이 성공하면 _표현식_ 의 값은 옵셔널로 래핑되고 반환됩니다; 그렇지 않으면 반환된 값은 `nil` 입니다. 지정된 _타입_ 으로 캐스팅이 실패하거나 성공이 보장되면 컴파일 시 에러가 발생합니다.

`as!` 연산자는 지정한 _타입_ 으로 _표현식_ 의 강제 캐스팅을 수행합니다. `as!` 연산자는 옵셔널 타입이 아닌 지정한 _타입_ 의 값을 반환합니다. 캐스팅이 실패하면 런타임 에러가 발생합니다. `x as! T` 의 동작은 `(x as? T)!` 의 동작과 동일합니다.

타입 캐스팅에 대한 자세한 내용과 타입 캐스팅 연산자 사용에 대한 예제는 [타입 캐스팅 (Type Casting)](../language-guide-1/type-casting.md) 을 참고 바랍니다.

> GRAMMAR OF A TYPE-CASTING OPERATOR\
> type-casting-operator → `is` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type)\
> type-casting-operator → `as` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type)\
> type-casting-operator → `as` `?` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type)\
> type-casting-operator → `as` `!` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type)

## 기본 표현식 (Primary Expressions)

<!--
Primary expressions are the most basic kind of expression. They can be used as expressions on their own, and they can be combined with other tokens to make prefix expressions, infix expressions, and postfix expressions.
-->

_기본 표현식 (Primary expressions)_ 은 표현식의 가장 기본입니다. 표현식 자체로 사용될 수 있으며 접두사 표현식, 중위 표현식, 그리고 접미사 표현식을 만들기 위해 다른 토큰과 결합될 수 있습니다.

> GRAMMAR OF A PRIMARY EXPRESSION\
> primary-expression → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) [generic-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-argument-clause) $$_{opt}$$\
> primary-expression → [literal-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_literal-expression)\
> primary-expression → [self-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_self-expression)\
> primary-expression → [superclass-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_superclass-expression)\
> primary-expression → [closure-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_closure-expression)\
> primary-expression → [parenthesized-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_parenthesized-expression)\
> primary-expression → [tuple-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_tuple-expression)\
> primary-expression → [implicit-member-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_implicit-member-expression)\
> primary-expression → [wildcard-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_wildcard-expression)\
> primary-expression → [key-path-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_key-path-expression)\
> primary-expression → [selector-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_selector-expression)\
> primary-expression → [key-path-string-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_key-path-string-expression)

### 리터럴 표현식 (Literal Expression)

<!--
A literal expression consists of either an ordinary literal (such as a string or a number), an array or dictionary literal, a playground literal, or one of the following special literals:
-->

_리터럴 표현식 (literal expression)_ 은 일반 리터럴 (문자열 또는 숫자 등), 배열 또는 딕셔너리 리터럴, 플레이그라운드 리터럴 또는 다음 특수 리터럴 중 하나로 구성됩니다:

|    Literal   |        Type        |                             Value                            |
| :----------: | :----------------: | :----------------------------------------------------------: |
|    `#file`   |      `String`      |                      파일이 표시되는 파일의 경로입니다.                     |
|   `#fileID`  |      `String`      |                      파일과 모듈을 나타내는 이름입니다.                     |
|  `#filePath` |      `String`      |                      파일이 표시되는 파일의 경로입니다.                     |
|    `#line`   |        `Int`       |                       숫자로 나타내는 라인수 입니다.                      |
|   `#column`  |        `Int`       |                      숫자로 시작하는 열 번호 입니다.                      |
|  `#function` |      `String`      |                          선언의 이름입니다.                          |
| `#dsohandle` | `UnsafeRawPointer` | 표시되는 위치에서 사용되는 동적 공유 객체 (dynamic shared object) (DSO) 처리입니다. |

<!--
The string value of #file depends on the language version, to enable migration from the old #filePath behavior to the new #fileID behavior. Currently, #file has the same value as #filePath. In a future version of Swift, #file will have the same value as #fileID instead. To adopt the future behavior, replace #file with #fileID or #filePath as appropriate.

The string value of a #fileID expression has the form module/file, where file is the name of the file in which the expression appears and module is the name of the module that this file is part of. The string value of a #filePath expression is the full file-system path to the file in which the expression appears. Both of these values can be changed by #sourceLocation, as described in Line Control Statement. Because #fileID doesn’t embed the full path to the source file, unlike #filePath, it gives you better privacy and reduces the size of the compiled binary. Avoid using #filePath outside of tests, build scripts, or other code that doesn’t become part of the shipping program.
-->

`#file` 의 문자열 값은 이전 `#filePath` 동작에서 새로운 `#fileID` 동작으로 변경 가능하도록 언어 버전에 따라 다릅니다. 현재 `#file` 은 `#filePath` 와 동일한 값을 가집니다. Swift 의 향후 버전에서는 `#file` 은 `#fileID` 와 동일한 값을 가질 것입니다. 향후 동작을 채택하려면 `#file` 을 `#fileID` 또는 `#filePath` 로 적절하게 바꾸십시오.

`#fileID` 표현식의 문자열 값은 _모듈 (module)/파일 (file)_ 의 형식을 가지며 _파일 (file)_ 은 표현식이 나타내는 파일의 이름이고 _모듈 (module)_ 은 이 파일이 속한 모듈의 이름을 나타냅니다. `#filePath` 표현식의 문자열 값은 표현식이 나타내는 파일의 전체 파일시스템 경로입니다. 이 두 값은 [라인 제어 구문 (Line Control Statement)](statements.md#line-control-statement) 에서 설명한대로 `#sourceLocation` 에 의해 변경될 수 있습니다. #fileID 는 #filePath 와 달리 소스 파일의 전체 경로를 포함하지 않으므로 개인정보보호를 강화하고 컴파일 된 바이너리의 크기를 더 줄입니다. 테스트, 빌드 스크립트, 또는 다른 프로그램의 일부가 되는 코드에 `#filePath` 사용은 피해야 합니다.

<!--
NOTE
To parse a #fileID expression, read the module name as the text before the first slash (/) and the filename as the text after the last slash. In the future, the string might contain multiple slashes, such as MyModule/some/disambiguation/MyFile.swift.
-->

> NOTE\
> `#fileID` 표현식을 분석하려면 모듈 이름을 첫 번째 슬래시 (`/`) 앞의 텍스트로 읽고 파일 이름을 마지막 슬래시 이후 문자열로 읽습니다. 문자열은 `MyModule/some/disambiguation/MyFile.swift` 와 같이 여러개의 슬래시가 포함될 수 있습니다.

<!--
Inside a function, the value of #function is the name of that function, inside a method it’s the name of that method, inside a property getter or setter it’s the name of that property, inside special members like init or subscript it’s the name of that keyword, and at the top level of a file it’s the name of the current module.

When used as the default value of a function or method parameter, the special literal’s value is determined when the default value expression is evaluated at the call site.
-->

함수 내에서 `#function` 의 값은 해당 함수의 이름이고 메서드 내에서 해당 메서드의 이름이고 프로퍼티 getter 또는 setter 내에서 해당 프로퍼티의 이름이고 `init` 또는 `subscript` 와 같은 툭수 멤버 내에서 해당 키워드의 이름이고 파일의 최상위 수준에서는 현재 모듈의 이름입니다.

함수 또는 메서드 파라미터의 기본값으로 사용될 때 특수 리터럴의 값은 기본값 표현식은 호출될 때 결정됩니다.

```swift
func logFunctionName(string: String = #function) {
    print(string)
}
func myFunction() {
    logFunctionName() // Prints "myFunction()".
}
```

<!--
An array literal is an ordered collection of values. It has the following form:
-->

_배열 리터럴 (array literal)_ 은 순서가 있는 값의 콜렉션입니다. 형식은 아래와 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.45.50.png>)

<!--
The last expression in the array can be followed by an optional comma. The value of an array literal has type [T], where T is the type of the expressions inside it. If there are expressions of multiple types, T is their closest common supertype. Empty array literals are written using an empty pair of square brackets and can be used to create an empty array of a specified type.
-->

배열의 마지막 표현식은 옵셔널 콤마가 따라올 수 있습니다. 배열 리터럴의 값은 `T` 는 표현식 내의 타입인 타입 `[T]` 를 가집니다. 여러 타입의 표현식이 있는 경우 `T` 는 가장 가까운 공통 상위 타입 (supertype) 입니다. 빈 배열 리터럴은 빈 대괄호 쌍을 사용하여 작성하고 지정된 타입의 빈 배열을 생성하는데 사용될 수 있습니다.

```swift
var emptyArray: [Double] = []
```

<!--
A dictionary literal is an unordered collection of key-value pairs. It has the following form:
-->

_딕셔너리 리터럴 (dictionary literal)_ 은 순서가 없는 키-값 쌍의 콜렉션입니다. 형식은 아래와 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.46.56.png>)

<!--
The last expression in the dictionary can be followed by an optional comma. The value of a dictionary literal has type [Key: Value], where Key is the type of its key expressions and Value is the type of its value expressions. If there are expressions of multiple types, Key and Value are the closest common supertype for their respective values. An empty dictionary literal is written as a colon inside a pair of brackets ([:]) to distinguish it from an empty array literal. You can use an empty dictionary literal to create an empty dictionary literal of specified key and value types.
-->

딕셔너리에 마지막 표현식은 옵셔널 콤마가 따라올 수 있습니다. 딕셔너리 리터럴의 값은 `Key` 는 키 표현식의 타입이고 `Value` 는 값 표현식의 타입인 타입 `[Key: Value]` 를 가집니다. 여러 타입의 표현식이 있는 경우 `Key` 와 `Value` 는 해당 값에 대해 가장 가까운 공통 상위 타입입니다. 빈 딕셔너리 리터럴은 빈 배열 리터럴과 구분하기 위해 대괄호 쌍내에 콜론 (`[:]`) 을 작성합니다. 지정한 키와 값 타입으로 빈 딕셔너리 리터럴을 생성하기 위해 빈 딕셔너리 리터럴을 사용할 수 있습니다.

```swift
var emptyDictionary: [String: Double] = [:]
```

<!--
A playground literal is used by Xcode to create an interactive representation of a color, file, or image within the program editor. Playground literals in plain text outside of Xcode are represented using a special literal syntax.

For information on using playground literals in Xcode, see Add a color, file, or image literal in Xcode Help.
-->

_플레이그라운드 리터럴 (playground literal)_ 은 프로그램 편집기 내에서 색상, 파일, 또는 이미지의 상호 표현을 생성하기 위해 Xcode 에 의해 사용됩니다. Xcode 의 외부 플레인 텍스트에서 플레이그라운드 리터럴은 특수 리터럴 구문을 사용하여 표현됩니다.

Xcode 에서 플레이그라운드 리터럴 사용에 대한 정보는 Xcode 도움에 [색상, 파일, 또는 이미지 리터럴 추가하기 (Add a color, file, or image literal)](https://help.apple.com/xcode/mac/current/#/dev4c60242fc) 을 참고 바랍니다.

> GRAMMAR OF A LITERAL EXPRESSION\
> literal-expression → [literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_literal)\
> literal-expression → [array-literal](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_array-literal) | [dictionary-literal](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_dictionary-literal) | [playground-literal](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_playground-literal)\
> literal-expression → `#file` | `#fileID` | `#filePath`\
> literal-expression → `#line` | `#column` | `#function` | `#dsohandle`\
> array-literal → `[` [array-literal-items](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_array-literal-items) $$_{opt}$$ `]`\
> array-literal-items → [array-literal-item](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_array-literal-item) `,` $$_{opt}$$ | [array-literal-item](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_array-literal-item) `,` [array-literal-items](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_array-literal-items)\
> array-literal-item → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression)\
> dictionary-literal → `[` [dictionary-literal-items](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_dictionary-literal-items) `]` | `[` `:` `]`\
> dictionary-literal-items → [dictionary-literal-item](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_dictionary-literal-item) `,` $$_{opt}$$ | [dictionary-literal-item](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_dictionary-literal-item) `,` [dictionary-literal-items](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_dictionary-literal-items)\
> dictionary-literal-item → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression)\
> playground-literal → `#colorLiteral` `(` `red` `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `,` `green` `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `,` `blue` `:`[expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `,` `alpha` `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `)`\
> playground-literal → `#fileLiteral` `(` `resourceName` `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `)`\
> playground-literal → `#imageLiteral` `(` `resourceName` `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `)`

### Self 표현식 (Self Expression)

<!--
The self expression is an explicit reference to the current type or instance of the type in which it occurs. It has the following forms:
-->

`self` 표현식은 현재 타입 또는 해당 타입의 인스턴스에 대한 명시적 참조입니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.49.49.png>)

<!--
In an initializer, subscript, or instance method, self refers to the current instance of the type in which it occurs. In a type method, self refers to the current type in which it occurs.

The self expression is used to specify scope when accessing members, providing disambiguation when there’s another variable of the same name in scope, such as a function parameter. For example:
-->

초기화 구문, 서브 스크립트, 또는 인스턴스 메서드에서 `self` 는 해당 타입의 현재 인스턴스를 참조합니다. 타입 메서드에서 `self` 는 현재 타입을 참조합니다.

`self` 표현식은 멤버에 접근할 때 범위를 지정하는데 사용되고 함수 파라미터와 같이 범위에 같은 이름의 다른 변수가 있을 때 명확성을 제공합니다. 예를 들어:

```swift
class SomeClass {
    var greeting: String
    init(greeting: String) {
        self.greeting = greeting
    }
}
```

<!--
In a mutating method of a value type, you can assign a new instance of that value type to self. For example:
-->

값 타입의 변경가능 메서드 (mutating method) 에서 해당 값 타입의 새 인스턴스를 `self` 에 할당할 수 있습니다. 예를 들어:

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```

> GRAMMAR OF A SELF EXPRESSION\
> self-expression → `self` | [self-method-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_self-method-expression) | [self-subscript-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_self-subscript-expression) | [self-initializer-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_self-initializer-expression)\
> self-method-expression → `self` `.` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> self-subscript-expression → `self` `[` [function-call-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_function-call-argument-list) `]`\
> self-initializer-expression → `self` `.` `init`

### 상위클래스 표현식 (Superclass Expression)

<!--
A superclass expression lets a class interact with its superclass. It has one of the following forms:
-->

_상위클래스 표현식 (superclass expression)_ 을 사용하면 클래스가 상위클래스와 상호작용 할 수 있습니다. 다음 형식 중 하나가 있습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.52.12.png>)

<!--
The first form is used to access a member of the superclass. The second form is used to access the superclass’s subscript implementation. The third form is used to access an initializer of the superclass.

Subclasses can use a superclass expression in their implementation of members, subscripting, and initializers to make use of the implementation in their superclass.
-->

첫번째 형식은 상위클래스의 멤버에 접근하기 위해 사용됩니다. 두번째 형식은 상위클래스의 서브 스크립트 구현에 접근하기 위해 사용됩니다. 세번째 형식은 상위클래스의 초기화 구문에 접근하기 위해 사용됩니다.

하위클래스는 멤버, 서브 스크립팅 그리고 초기화 구문에서 상위클래스 표현식을 사용하여 상위클래스의 구현을 사용할 수 있습니다.

> GRAMMAR OF A SUPERCLASS EXPRESSION\
> superclass-expression → [superclass-method-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_superclass-method-expression) | [superclass-subscript-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_superclass-subscript-expression) | [superclass-initializer-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_superclass-initializer-expression)\
> superclass-method-expression → `super` `.` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> superclass-subscript-expression → `super` `[` [function-call-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_function-call-argument-list) `]`\
> superclass-initializer-expression → `super` `.` `init`

### 클로저 표현식 (Closure Expression)

<!--
A closure expression creates a closure, also known as a lambda or an anonymous function in other programming languages. Like a function declaration, a closure contains statements, and it captures constants and variables from its enclosing scope. It has the following form:
-->

_클로저 표현식 (closure expression)_ 은 다른 프로그래밍 언어에서 _람다 (lambda)_ 또는 _익명 함수 (anonymous function)_ 이라고도 알고 있는 클로저를 생성합니다. 함수 선언과 같이 클로저는 구문을 포함하고 둘러싸인 범위에서 상수와 변수를 캡처합니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 10.53.51.png>)

<!--
The parameters have the same form as the parameters in a function declaration, as described in Function Declaration.

Writing throws or async in a closure expression explicitly marks a closure as throwing or asynchronous.
-->

_파라미터 (parameters)_ 는 [함수 선언 (Function Declaration)](declarations.md#function-declaration) 에서 설명 했듯이 함수 선언에서 파라미터 형식과 동일합니다.

클로저 표현식에 명시적으로 `throws` 또는 `async` 작성하는 것은 클로저를 throwing 또는 비동기를 나타냅니다.

![](<../.gitbook/assets/closure_expression.png>)

<!--
If the body of a closure includes a try expression, the closure is understood to be throwing. Likewise, if it includes an await expression, it’s understood to be asynchronous.

There are several special forms that allow closures to be written more concisely:

* A closure can omit the types of its parameters, its return type, or both. If you omit the parameter names and both types, omit the in keyword before the statements. If the omitted types can’t be inferred, a compile-time error is raised.
* A closure may omit names for its parameters. Its parameters are then implicitly named $ followed by their position: $0, $1, $2, and so on.
* A closure that consists of only a single expression is understood to return the value of that expression. The contents of this expression are also considered when performing type inference on the surrounding expression.

The following closure expressions are equivalent:
-->

클로저의 본문에 try 표현식이 포함되어 있다면 클로저는 throwing 으로 이해됩니다. 마찬가지로 클로저에 `await` 표현식이 포함되어 있다면 그것은 비동기로 이해됩니다.

클로저를 보다 간결하게 작성할 수 있는 몇가지 특별한 형식이 있습니다:

* 클로저는 파라미터의 타입, 반환 타입 또는 둘 다 생략할 수 있습니다. 파라미터 이름과 타입 모두 생략하는 경우 구문 전에 `in` 키워드도 생략합니다. 생략한 타입을 유추할 수 없을 때 컴파일 시 에러가 발생합니다.
* 클로저는 파라미터 이름을 생략할 수 있습니다. 파라미터는 암시적으로 `$` 다음에 위치가 붙어 이름이 붙여집니다: `$0`, `$1`, `$2`, 등.
* 단일 표현식으로만 구성된 클로저는 해당 포현식의 값을 반환하는 것으로 이해됩니다. 이 표현식의 내용은 주변 표현식에 대한 타입 추론을 수행할 때도 고려됩니다.

다음의 클로저 표현식은 동일합니다:

```swift
myFunction { (x: Int, y: Int) -> Int in
    return x + y
}

myFunction { x, y in
    return x + y
}

myFunction { return $0 + $1 }

myFunction { $0 + $1 }
```

<!--
For information about passing a closure as an argument to a function, see Function Call Expression.

Closure expressions can be used without being stored in a variable or constant, such as when you immediately use a closure as part of a function call. The closure expressions passed to myFunction in code above are examples of this kind of immediate use. As a result, whether a closure expression is escaping or nonescaping depends on the surrounding context of the expression. A closure expression is nonescaping if it’s called immediately or passed as a nonescaping function argument. Otherwise, the closure expression is escaping.

For more information about escaping closures, see Escaping Closures.
-->

함수에 인수로 클로저를 전달하는 것에 대한 정보는 [함수 호출 표현식 (Function Call Expression)](expressions.md#function-call-expression) 을 참고 바랍니다.

클로저 표현식은 함수 호출의 일부로 클로저를 즉시 사용할 때와 같이 변수나 상수에 저장하지 않고 사용될 수 있습니다. 위 코드에서 `myFunction` 에 전달된 클로저 표현식은 이러한 종류의 즉각적인 사용의 예제입니다. 결과적으로 클로저 표현식은 이스케이프 (escaping) 인지 비이스케이프 (nonescaping) 인지 여부는 주변의 컨텍스트에 의해 결정됩니다. 클로저 표현식은 즉시 호출되거나 비이스케이프 함수 인수로 전달되면 비이스케이프 입니다. 그렇지 않으면 클로저 표현식은 이스케이프 입니다.

이스케이프 클로저에 대한 자세한 내용은 [이스케이프 클로저 (Escaping Closures)](../language-guide-1/closures.md#escaping-closures) 를 참고 바랍니다.

#### **캡처 목록 (Capture Lists)**

<!--
By default, a closure expression captures constants and variables from its surrounding scope with strong references to those values. You can use a capture list to explicitly control how values are captured in a closure.

A capture list is written as a comma-separated list of expressions surrounded by square brackets, before the list of parameters. If you use a capture list, you must also use the in keyword, even if you omit the parameter names, parameter types, and return type.

The entries in the capture list are initialized when the closure is created. For each entry in the capture list, a constant is initialized to the value of the constant or variable that has the same name in the surrounding scope. For example in the code below, a is included in the capture list but b is not, which gives them different behavior.
-->

기본적으로 클로저 표현식은 해당 값의 강한 참조를 사용하여 주변 범위에서 상수와 변수를 캡처합니다. _캡처 목록 (capture list)_ 를 사용하여 클로저에서 값이 캡쳐되는 방법을 명시적으로 제어할 수 있습니다

캡처 목록은 파라미터의 목록 전에 대괄호로 둘러싸여 콤마로 구분하여 작성됩니다. 캡처 목록을 사용하면 파라미터 이름, 파라미터 타입, 그리고 반환 타입을 생략하더라도 `in` 키워드를 사용해야 합니다.

캡처 목록의 항목은 클로저가 생성될 때 초기화 됩니다. 캡처 목록의 각 항목에 대해 상수는 주변 범위에서 이름이 같은 상수 또는 변수의 값으로 초기화 됩니다. 예를 들어 아래 코드에서 `a` 는 캡처 목록에 포함되지만 `b` 는 포함되지 않으므로 다른 동작을 보여줍니다.

```swift
var a = 0
var b = 0
let closure = { [a] in
    print(a, b)
}

a = 10
b = 10
closure()
// Prints "0 10"
```

<!--
There are two different things named a, the variable in the surrounding scope and the constant in the closure’s scope, but only one variable named b. The a in the inner scope is initialized with the value of the a in the outer scope when the closure is created, but their values aren’t connected in any special way. This means that a change to the value of a in the outer scope doesn’t affect the value of a in the inner scope, nor does a change to a inside the closure affect the value of a outside the closure. In contrast, there’s only one variable named b—the b in the outer scope—so changes from inside or outside the closure are visible in both places.

This distinction isn’t visible when the captured variable’s type has reference semantics. For example, there are two things named x in the code below, a variable in the outer scope and a constant in the inner scope, but they both refer to the same object because of reference semantics.
-->

주변 범위에서 변수와 클로저의 범위에서 상수로 `a` 라는 이름으로 두가지가 있지만 `b` 는 변수로 하나만 존재합니다. 내부 범위의 `a` 는 클로저가 생성될 때 외부 범위에서 `a` 의 값으로 초기화 되지만 해당 값은 특별한 방법으로 연결되지 않습니다. 이것은 외부 범위의 `a` 의 값이 변경되더라도 내부 범위의 `a` 의 값은 영향을 받지 않고 클로저 내에 `a` 가 변경되도 클로저 외부의 `a` 의 값은 영향을 받지 않습니다. 반대로 외부 범위에 `b` 는 하나의 변수로만 있으므로 클로저 내부 또는 외부에서 변경되면 두 위치 모두에 반영됩니다.

캡처된 변수의 타입이 의미가 있는 참조인 경우 구분이 표시되지 않습니다. 예를 들어 아래 코드에서 `x` 라는 이름의 두가지가 있는데 외부 범위의 변수와 내부 범위의 상수이지만 둘다 참조 의미이기 때문에 동일한 객체를 참조합니다.

```swift
class SimpleClass {
    var value: Int = 0
}
var x = SimpleClass()
var y = SimpleClass()
let closure = { [x] in
    print(x.value, y.value)
}

x.value = 10
y.value = 10
closure()
// Prints "10 10"
```

<!--
If the type of the expression’s value is a class, you can mark the expression in a capture list with weak or unowned to capture a weak or unowned reference to the expression’s value.
-->

표현식의 값의 타입이 클래스라면 표현식의 값을 약한 또는 미소유 참조로 캡처하기 위해 `weak` 또는 `unowned` 로 캡처 목록에 표현식을 표시할 수 있습니다.

```swift
myFunction { print(self.title) }                    // implicit strong capture
myFunction { [self] in print(self.title) }          // explicit strong capture
myFunction { [weak self] in print(self!.title) }    // weak capture
myFunction { [unowned self] in print(self.title) }  // unowned capture
```

<!--
You can also bind an arbitrary expression to a named value in a capture list. The expression is evaluated when the closure is created, and the value is captured with the specified strength. For example:
-->

캡처 목록의 명명된 값에 임의의 표현식을 바인딩 할 수도 있습니다. 표현식은 클로저가 생성될 때 평가되고 값은 지정된 강도로 캡처됩니다. 예를 들어:

```swift
// Weak capture of "self.parent" as "parent"
myFunction { [weak parent = self.parent] in print(parent!.title) }
```

<!--
For more information and examples of closure expressions, see Closure Expressions. For more information and examples of capture lists, see Resolving Strong Reference Cycles for Closures.
-->

클로저 표현식에 자세한 내용과 예제는 [클로저 표현식 (Closure Expressions)](../language-guide-1/closures.md#closure-expressions) 를 참고 바랍니다. 캡처 목록에 자세한 내용과 예제는 [클로저에 대한 강한 참조 사이클 해결 (Resolving Strong Reference Cycles for Closures)](../language-guide-1/automatic-reference-counting.md#resolving-strong-reference-cycles-for-closures) 를 참고 바랍니다.

> GRAMMAR OF A CLOSURE EXPRESSION\
> closure-expression → `{` [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes) $$_{opt}$$ [closure-signature](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_closure-signature) $$_{opt}$$ [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statements) $$_{opt}$$ `}`\
> closure-signature → [capture-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_capture-list) opt [closure-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_closure-parameter-clause) `throws` $$_{opt}$$ [function-result](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_function-result) $$_{opt}$$ `in`\
> closure-signature → [capture-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_capture-list) `in`\
> closure-parameter-clause → `(` `)` | `(` [closure-parameter-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_closure-parameter-list) `)` | [identifier-list](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier-list)\
> closure-parameter-list → [closure-parameter](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_closure-parameter) | [closure-parameter](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_closure-parameter) `,` [closure-parameter-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_closure-parameter-list)\
> closure-parameter → [closure-parameter-name](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_closure-parameter-name) [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation) $$_{opt}$$\
> closure-parameter → [closure-parameter-name](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_closure-parameter-name) [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation) `...`\
> closure-parameter-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> capture-list → `[` [capture-list-items](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_capture-list-items) `]`\
> capture-list-items → [capture-list-item](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_capture-list-item) | [capture-list-item](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_capture-list-item) `,` [capture-list-items](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_capture-list-items)\
> capture-list-item → [capture-specifier](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_capture-specifier) $$_{opt}$$ [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> capture-list-item → [capture-specifier](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_capture-specifier) $$_{opt}$$ [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) `=` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression)\
> capture-list-item → [capture-specifier](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_capture-specifier) $$_{opt}$$ [self-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_self-expression)\
> capture-specifier → `weak` | `unowned` | `unowned(safe)` | `unowned(unsafe)`

### 암시적 멤버 표현식 (Implicit Member Expression)

<!--
An implicit member expression is an abbreviated way to access a member of a type, such as an enumeration case or a type method, in a context where type inference can determine the implied type. It has the following form:
-->

_암시적 멤버 표현식 (implicit member expression)_ 은 타입 유추가 암시된 타입을 결정할 수 있는 컨텍스트에서 열거형 케이스 또는 타입 메서드와 같은 타입의 멤버에 접근하기 위한 축약된 방법입니다. 다음과 같은 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.00.55.png>)

<!--
For example:
-->

예를 들어:

```swift
var x = MyEnumeration.someValue
x = .anotherValue
```

<!--
If the inferred type is an optional, you can also use a member of the non-optional type in an implicit member expression.
-->

유추된 타입이 옵셔널이면 암시적 멤버 표현식에서 옵셔널이 아닌 타입의 멤버로 사용할 수도 있습니다.

```swift
var someOptional: MyEnumeration? = .someValue
```

<!--
Implicit member expressions can be followed by a postfix operator or other postfix syntax listed in Postfix Expressions. This is called a chained implicit member expression. Although it’s common for all of the chained postfix expressions to have the same type, the only requirement is that the whole chained implicit member expression needs to be convertible to the type implied by its context. Specifically, if the implied type is an optional you can use a value of the non-optional type, and if the implied type is a class type you can use a value of one of its subclasses. For example:
-->

암시적 멤버 표현식 뒤에는 접미사 연산자 (postfix operator) 또는 [접미사 표현식 (Postfix Expressions)](expressions.md#post-expression) 에 나열된 다른 접미사 구문이 올 수 있습니다. 이것을 _연결된 암시적 멤버 표현식 (chained postfix expressions)_ 이라 합니다. 연결된 접미사 표현식이 동일한 타입을 갖는 것이 일반적이지만 유일한 요구사항은 전체 연결된 암시적 멤버 표현식이 컨텍스트에 의해 암시된 타입으로 변환될 수 있어야 한다는 것입니다. 특히, 암시적 타입이 옵셔널인 경우 옵셔널 타입이 아닌 값을 사용할 수 있고 암시적 타입이 클래스 타입인 경우 해당 하위 클래스 중 하나의 값을 사용할 수 있습니다. 예를 들어:

```swift
class SomeClass {
    static var shared = SomeClass()
    static var sharedSubclass = SomeSubclass()
    var a = AnotherClass()
}
class SomeSubclass: SomeClass { }
class AnotherClass {
    static var s = SomeClass()
    func f() -> SomeClass { return AnotherClass.s }
}
let x: SomeClass = .shared.a.f()
let y: SomeClass? = .shared
let z: SomeClass = .sharedSubclass
```

<!--
In the code above, the type of x matches the type implied by its context exactly, the type of y is convertible from SomeClass to SomeClass?, and the type of z is convertible from SomeSubclass to SomeClass.
-->

위의 코드에서 `x` 의 타입은 컨텍스트가 암시하는 타입과 정확히 일치하고 `y` 의 타입은 `SomeClass` 에서 `SomeClass?` 로 변환 가능하며 `z` 의 타입은 `SomeSubclass` 에서 `SomeClass` 로 변환 가능합니다.

> GRAMMAR OF A IMPLICIT MEMBER EXPRESSION\
> implicit-member-expression → `.` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> implicit-member-expression → `.` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) `.` [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression)

### 괄호 안 표현식 (Parenthesized Expression)

<!--
A parenthesized expression consists of an expression surrounded by parentheses. You can use parentheses to specify the precedence of operations by explicitly grouping expressions. Grouping parentheses don’t change an expression’s type—for example, the type of (1) is simply Int.
-->

_괄호 안 표현식 (parenthesized expression)_ 은 표현식을 괄호로 둘러싸인 것으로 구성됩니다. 괄호를 사용하여 표현식을 명시적으로 그룹화 하여 작업의 우선순위를 지정할 수 있습니다. 그룹화 괄호는 표현식의 타입을 변경하지 않습니다—예를 들어 `(1)` 의 타입은 단순히 `Int` 입니다.

> GRAMMAR OF A PARENTHESIZED EXPRESSION\
> parenthesized-expression → `(` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `)`

### 튜플 표현식 (Tuple Expression)

<!--
A tuple expression consists of a comma-separated list of expressions surrounded by parentheses. Each expression can have an optional identifier before it, separated by a colon (:). It has the following form:
-->

_튜플 표현식 (tuple expression)_ 은 괄호호 묶인 콤마로 구분된 표현식의 목록으로 구성됩니다. 각 표현식은 표현식 앞에 콜론 (`:`) 으로 구분된 식별자를 가질 수 있습니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.03.12.png>)

<!--
Each identifier in a tuple expression must be unique within the scope of the tuple expression. In a nested tuple expression, identifiers at the same level of nesting must be unique. For example, (a: 10, a: 20) is invalid because the label a appears twice at the same level. However, (a: 10, b: (a: 1, x: 2)) is valid—although a appears twice, it appears once in the outer tuple and once in the inner tuple.

A tuple expression can contain zero expressions, or it can contain two or more expressions. A single expression inside parentheses is a parenthesized expression.
-->

튜플 표현식의 각 식별자는 튜플 표현식의 범위 내에서 고유해야 합니다. 중첩된 튜플 표현식에서 동일한 수준의 중첩한 식별자는 고유해야 합니다. 예를 들어 `(a: 10, a: 20)` 은 라벨 `a` 가 동일한 수준에서 두번 나타나므로 유효하지 않습니다. 그러나 `(a: 10, b: (a: 1, x:2))` 는 `a` 가 두번 나타나도 외부 튜플과 내부 튜플에서 나타나므로 유효합니다.

튜플 표현식은 0개의 표현식을 포함하거나 2개 이상의 표현식을 포함할 수 있습니다. 괄호 안에 단일 표현식은 괄호 안 표현식 입니다.

<!--
NOTE
Both an empty tuple expression and an empty tuple type are written () in Swift. Because Void is a type alias for (), you can use it to write an empty tuple type. However, like all type aliases, Void is always a type—you can’t use it to write an empty tuple expression.
-->

> NOTE\
> 빈 튜플 표현식과 빈 튜플 타입 모두 Swift 에서 `()` 로 작성됩니다. `Void` 는 `()` 에 대한 타입 별칭이므로 빈 튜플 타입을 작성하기위해 사용할 수 있습니다. 그러나 모든 타입 별칭과 마찬가지로 `Void` 는 항상 타입이므로 빈 튜플 표현식으로 작성하기 위해 사용할 수 없습니다.
>
> GRAMMAR OF A TUPLE EXPRESSION\
> tuple-expression → `(` `)` | `(` [tuple-element](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_tuple-element) `,` [tuple-element-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_tuple-element-list) `)`\
> tuple-element-list → [tuple-element](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_tuple-element) | [tuple-element](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_tuple-element) `,` [tuple-element-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_tuple-element-list)\
> tuple-element → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) | [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression)

### 와일드카드 표현식 (Wildcard Expression)

<!--
A wildcard expression is used to explicitly ignore a value during an assignment. For example, in the following assignment 10 is assigned to x and 20 is ignored:
-->

_와일드카드 표현식 (wildcard expression)_ 은 할당 중에 값을 명시적으로 무시하는데 사용됩니다. 예를 들어 다음의 할당에서 10 은 `x` 에 할당되고 20 은 무시됩니다:

```swift
(x, _) = (10, 20)
// x is 10, and 20 is ignored
```

> GRAMMAR OF A WILDCARD EXPRESSION\
> wildcard-expression → `_`

### 키-경로 표현식 (Key-Path Expression)

<!--
A key-path expression refers to a property or subscript of a type. You use key-path expressions in dynamic programming tasks, such as key-value observing. They have the following form:
-->

_키-경로 표현식 (key-path expression)_ 은 타입의 프로퍼티 또는 서브 스크립트를 참조합니다. 키-값 관찰 (key-value observing) 과 같은 동적 프로그래밍 작업 (dynamic programming tasks) 에서 키-경로 표현식을 사용합니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.05.56.png>)

<!--
The type name is the name of a concrete type, including any generic parameters, such as String, [Int], or Set<Int>.

The path consists of property names, subscripts, optional-chaining expressions, and forced unwrapping expressions. Each of these key-path components can be repeated as many times as needed, in any order.

At compile time, a key-path expression is replaced by an instance of the KeyPath class.

To access a value using a key path, pass the key path to the subscript(keyPath:) subscript, which is available on all types. For example:
-->

_타입 이름 (type name)_ 은 `String`, `[Int]`, 또는 `Set<Int>` 와 같은 모든 제너릭 파라미터를 포함한 구체적 타입의 이름입니다.

_경로 (path)_ 는 프로퍼티 이름, 서브 스크립트, 옵셔널 체이닝 표현식, 그리고 강제 언래핑한 표현식으로 구성됩니다. 이러한 각 키-경로 요소는 순서에 상관없이 필요한 만큼 여러번 반복할 수 있습니다.

컴파일 시에 키-경로 표현식은 [`KeyPath`](https://developer.apple.com/documentation/swift/keypath) 클래스의 인스턴스에 의해 대체됩니다.

키 경로를 사용하여 값에 접근하려면 모든 타입에서 사용할 수 있는 `subscript(keyPath:)` 서브 스크립트에 키 경로를 전달해야 합니다. 예를 들어:

```swift
struct SomeStructure {
    var someValue: Int
}

let s = SomeStructure(someValue: 12)
let pathToProperty = \SomeStructure.someValue

let value = s[keyPath: pathToProperty]
// value is 12
```

<!--
The type name can be omitted in contexts where type inference can determine the implied type. The following code uses \.someProperty instead of \SomeClass.someProperty:
-->

_타입 이름 (type name)_ 은 타입 추론이 암시된 타입으로 결정할 수 있는 컨텍스트에서는 생략될 수 있습니다. 다음 코드는 `\SomeClass.someProperty` 대신에 `\.someProperty` 를 사용합니다:

```swift
class SomeClass: NSObject {
    @objc dynamic var someProperty: Int
    init(someProperty: Int) {
        self.someProperty = someProperty
    }
}

let c = SomeClass(someProperty: 10)
c.observe(\.someProperty) { object, change in
    // ...
}
```

<!--
The path can refer to self to create the identity key path (\.self). The identity key path refers to a whole instance, so you can use it to access and change all of the data stored in a variable in a single step. For example:
-->

_경로 (path)_ 는 식별자 키 경로 (`\.self`) 를 생성하기 위해 `self` 를 참조할 수 있습니다. 식별자 키 경로 (identity key path) 는 전체 인스턴스를 참조하므로 이를 사용하여 변수에 저장된 모든 데이터를 한번에 접근하고 변경할 수 있습니다. 예를 들어:

```swift
var compoundValue = (a: 1, b: 2)
// Equivalent to compoundValue = (a: 10, b: 20)
compoundValue[keyPath: \.self] = (a: 10, b: 20)
```

<!--
The path can contain multiple property names, separated by periods, to refer to a property of a property’s value. This code uses the key path expression \OuterStructure.outer.someValue to access the someValue property of the OuterStructure type’s outer property:
-->

_경로 (path)_ 는 프로퍼티의 값의 프로퍼티를 참조하기 위해 마침표로 구분된 여러 프로퍼티 이름이 포함될 수 있습니다. 이 코드는 `OuterStructure` 타입의 `outer` 프로퍼티의 `someValue` 프로퍼티를 접근하기 위해 키 경로 표현식 `\OuterStructure.outer.someValue` 을 사용합니다:

```swift
struct OuterStructure {
    var outer: SomeStructure
    init(someValue: Int) {
        self.outer = SomeStructure(someValue: someValue)
    }
}

let nested = OuterStructure(someValue: 24)
let nestedKeyPath = \OuterStructure.outer.someValue

let nestedValue = nested[keyPath: nestedKeyPath]
// nestedValue is 24
```

<!--
The path can include subscripts using brackets, as long as the subscript’s parameter type conforms to the Hashable protocol. This example uses a subscript in a key path to access the second element of an array:
-->

_경로 (path)_ 는 서브 스크립트의 파라미터 타입이 `Hashable` 프로토콜을 준수하는 한 대괄호를 사용하여 서브 스크립트를 포함할 수 있습니다. 이 예제는 배열의 두번째 요소를 접근하기 위해 키 경로로 서브 스크립트를 사용합니다:

```swift
let greetings = ["hello", "hola", "bonjour", "안녕"]
let myGreeting = greetings[keyPath: \[String].[1]]
// myGreeting is 'hola'
```

<!--
The value used in a subscript can be a named value or a literal. Values are captured in key paths using value semantics. The following code uses the variable index in both a key-path expression and in a closure to access the third element of the greetings array. When index is modified, the key-path expression still references the third element, while the closure uses the new index.
-->

서브 스크립트에서 사용된 값은 명명된 값 또는 리터럴 일 수 있습니다. 값은 값 의미로 사용하여 키 경로에서 캡처됩니다. 다음 코드는 키-경로 표현식과 클로저 모두에서 변수 `index` 를 사용하여 `greetings` 배역의 세번째 요소를 접근합니다. `index` 가 수정될 때 클로저는 새로운 인덱스를 사용하는 동안 키-경로 표현식은 여전히 세번째 요소를 참조합니다.

```swift
var index = 2
let path = \[String].[index]
let fn: ([String]) -> String = { strings in strings[index] }

print(greetings[keyPath: path])
// Prints "bonjour"
print(fn(greetings))
// Prints "bonjour"

// Setting 'index' to a new value doesn't affect 'path'
index += 1
print(greetings[keyPath: path])
// Prints "bonjour"

// Because 'fn' closes over 'index', it uses the new value
print(fn(greetings))
// Prints "안녕"
```

<!--
The path can use optional chaining and forced unwrapping. This code uses optional chaining in a key path to access a property of an optional string:
-->

_경로 (path)_ 는 옵셔널 체이닝과 강제 언래핑을 사용할 수 있습니다. 이 코드는 옵셔널 문자열의 프로퍼티를 접근하기 위해 키 경로에 옵셔널 체이닝을 사용합니다:

```swift
let firstGreeting: String? = greetings.first
print(firstGreeting?.count as Any)
// Prints "Optional(5)"

// Do the same thing using a key path.
let count = greetings[keyPath: \[String].first?.count]
print(count as Any)
// Prints "Optional(5)"
```

<!--
You can mix and match components of key paths to access values that are deeply nested within a type. The following code accesses different values and properties of a dictionary of arrays by using key-path expressions that combine these components.
-->

타입 내에 깊게 중첩된 값을 접근하기 위해 키 경로의 구성요소를 혼합하고 매치할 수 있습니다. 다음 코드는 구성요소를 결합한 키-경로 표현식을 사용하여 배열의 딕셔너리의 다른 값과 프로퍼티를 접근합니다.

```swift
let interestingNumbers = ["prime": [2, 3, 5, 7, 11, 13, 17],
                          "triangular": [1, 3, 6, 10, 15, 21, 28],
                          "hexagonal": [1, 6, 15, 28, 45, 66, 91]]
print(interestingNumbers[keyPath: \[String: [Int]].["prime"]] as Any)
// Prints "Optional([2, 3, 5, 7, 11, 13, 17])"
print(interestingNumbers[keyPath: \[String: [Int]].["prime"]![0]])
// Prints "2"
print(interestingNumbers[keyPath: \[String: [Int]].["hexagonal"]!.count])
// Prints "7"
print(interestingNumbers[keyPath: \[String: [Int]].["hexagonal"]!.count.bitWidth])
// Prints "64"
```

<!--
You can use a key path expression in contexts where you would normally provide a function or closure. Specifically, you can use a key path expression whose root type is SomeType and whose path produces a value of type Value, instead of a function or closure of type (SomeType) -> Value.
-->

일반적으로 함수 또는 클로저를 제공하는 컨텍스트에서 키 경로 표현식을 사용할 수 있습니다. 특히, 루트 타입이 `SomeType` 이고 타입 `(SomeType) -> Value` 의 함수 또는 클로저 대신에 타입 `Value` 의 값을 생성하는 키 경로 표현식을 사용할 수 있습니다.

```swift
struct Task {
    var description: String
    var completed: Bool
}
var toDoList = [
    Task(description: "Practice ping-pong.", completed: false),
    Task(description: "Buy a pirate costume.", completed: true),
    Task(description: "Visit Boston in the Fall.", completed: false),
]

// Both approaches below are equivalent.
let descriptions = toDoList.filter(\.completed).map(\.description)
let descriptions2 = toDoList.filter { $0.completed }.map { $0.description }
```

<!--
Any side effects of a key path expression are evaluated only at the point where the expression is evaluated. For example, if you make a function call inside a subscript in a key path expression, the function is called only once as part of evaluating the expression, not every time the key path is used.
-->

키 경로 표현식의 문제는 표현식이 평가되는 시점에만 평가됩니다. 예를 들어 키 경로 표현식에 서브 스크립트 내에 함수 호출하는 경우 함수는 키 경로가 사용될 때마다가 아니라 표현식 평가의 일부로 한번만 호출됩니다.

```swift
func makeIndex() -> Int {
    print("Made an index")
    return 0
}
// The line below calls makeIndex().
let taskKeyPath = \[Task][makeIndex()]
// Prints "Made an index"

// Using taskKeyPath doesn't call makeIndex() again.
let someTask = toDoList[keyPath: taskKeyPath]
```

<!--
For more information about using key paths in code that interacts with Objective-C APIs, see Using Objective-C Runtime Features in Swift. For information about key-value coding and key-value observing, see Key-Value Coding Programming Guide and Key-Value Observing Programming Guide.
-->

Objective-C API 와 함께 상혹작용하는 코드에서 키 경로를 사용하는 것에 대한 자세한 내용은 [Swift 에서 Objective-C 런타임 특성 사용 (Using Objective-C Runtime Features in Swift)](https://developer.apple.com/documentation/swift/using\_objective-c\_runtime\_features\_in\_swift) 를 참고 바랍니다. 키-값 코딩과 키-값 관찰에 대한 자세한 내용은 [키-값 코딩 프로그래밍 가이드 (Key-Value Coding Programming Guide)](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/KeyValueCoding/index.html#//apple\_ref/doc/uid/10000107i) 와 [키-값 관찰 프로그래밍 가이드 (Key-Value Observing Programming Guide)](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple\_ref/doc/uid/10000177i) 를 참고 바랍니다.

> GRAMMAR OF A KEY-PATH EXPRESSION\
> key-path-expression → `\` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type) $$_{opt}$$ `.` [key-path-components](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_key-path-components)\
> key-path-components → [key-path-component](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_key-path-component) | [key-path-component](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_key-path-component) `.` [key-path-components](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_key-path-components)\
> key-path-component → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) [key-path-postfixes](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_key-path-postfixes) $$_{opt}$$ | [key-path-postfixes](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_key-path-postfixes)\
> key-path-postfixes → [key-path-postfix](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_key-path-postfix) [key-path-postfixes](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_key-path-postfixes) $$_{opt}$$\
> key-path-postfix → `?` | `!` | `self` | `[` [function-call-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_function-call-argument-list) `]`

### 선택기 표현식 (Selector Expression)

<!--
A selector expression lets you access the selector used to refer to a method or to a property’s getter or setter in Objective-C. It has the following form:
-->

선택기 표현식 (selector expression) 을 사용하면 Objective-C 에 메서드 또는 프로퍼티의 getter 또는 setter 를 참조하는데 사용되는 선택기 (selector) 에 접근할 수 있습니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.13.06.png>)

<!--
The method name and property name must be a reference to a method or a property that’s available in the Objective-C runtime. The value of a selector expression is an instance of the Selector type. For example:
-->

_메서드 이름 (method name)_ 과 _프로퍼티 이름 (property name)_ 은 Objective-C 런타임에 사용할 수 있는 메서드 또는 프로퍼티에 대한 참조여야 합니다. 선택기 표현식의 값은 `Selector` 타입의 인스턴스입니다. 예를 들어:

```swift
class SomeClass: NSObject {
    @objc let property: String

    @objc(doSomethingWithInt:)
    func doSomething(_ x: Int) { }

    init(property: String) {
        self.property = property
    }
}
let selectorForMethod = #selector(SomeClass.doSomething(_:))
let selectorForPropertyGetter = #selector(getter: SomeClass.property)
```

<!--
When creating a selector for a property’s getter, the property name can be a reference to a variable or constant property. In contrast, when creating a selector for a property’s setter, the property name must be a reference to a variable property only.

The method name can contain parentheses for grouping, as well the as operator to disambiguate between methods that share a name but have different type signatures. For example:
-->

프로퍼티의 getter 에 대한 선택기를 생성할 때 _프로퍼티 이름 (property name)_ 은 변수 또는 상수 프로퍼티에 참조될 수 있습니다. 반대로 프로퍼티의 setter 에 대한 선택기를 생성할 때 _프로퍼티 이름 (property name)_ 은 변수 프로퍼티에만 참조될 수 있습니다.

_메서드 이름 (method name)_ 은 그룹화 (grouping) 을 위해 소괄호와 이름을 공유하지만 타입 서명이 다른 메서드를 명확하기 하기위해 `as` 연산자도 포함할 수 있습니다. 예를 들어:

```swift
extension SomeClass {
    @objc(doSomethingWithString:)
    func doSomething(_ x: String) { }
}
let anotherSelector = #selector(SomeClass.doSomething(_:) as (SomeClass) -> (String) -> Void)
```

<!--
Because a selector is created at compile time, not at runtime, the compiler can check that a method or property exists and that they’re exposed to the Objective-C runtime.
-->

선택기는 런타임이 아닌 컴파일 시 생성되기 때문에 컴파일러는 메서드 또는 프로퍼티가 존재하고 Objective-C 런타임에 노출되는지 확인할 수 있습니다.

<!--
NOTE
Although the method name and the property name are expressions, they’re never evaluated.
-->

> NOTE\
> _메서드 이름 (method name)_ 과 _프로퍼티 이름 (property name)_ 은 표현식이지만 절대 평가되지 않습니다.

<!--
For more information about using selectors in Swift code that interacts with Objective-C APIs, see Using Objective-C Runtime Features in Swift.
-->

Objective-C API 와 상호작용하는 Swift 코드에서 선택기 사용에 대한 자세한 내용은 [Swift 에서 Objective-C 런타임 특성 사용 (Using Objective-C Runtime Features in Swift)](https://developer.apple.com/documentation/swift/using\_objective-c\_runtime\_features\_in\_swift) 를 참고 바랍니다.

> GRAMMAR OF A SELECTOR EXPRESSION\
> selector-expression → `#selector` `(` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `)`\
> selector-expression → `#selector` `(` `getter:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `)`\
> selector-expression → `#selector` `(` `setter:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `)`

### 키-경로 문자열 표현식 (Key-Path String Expression)

<!--
A key-path string expression lets you access the string used to refer to a property in Objective-C, for use in key-value coding and key-value observing APIs. It has the following form:
-->

키-경로 문자열 표현식 (key-path string expression) 을 사용하면 키-값 코딩 (key-value coding) 과 키-값 관찰 (key-value observing) API 에서 사용하기 위해 Objective-C 에서 프로퍼티 참조에 사용되는 문자열을 접근할 수 있습니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.17.51.png>)

<!--
The property name must be a reference to a property that’s available in the Objective-C runtime. At compile time, the key-path string expression is replaced by a string literal. For example:
-->

_프로퍼티 이름 (property name)_ 은 Objective-C 런타임에 사용할 수 있는 프로퍼티를 참조해야 합니다. 컴파일 시에 키-경로 문자열 표현식은 문자열 리터럴에 의해 대체됩니다. 예를 들어:

```swift
class SomeClass: NSObject {
    @objc var someProperty: Int
    init(someProperty: Int) {
        self.someProperty = someProperty
    }
}

let c = SomeClass(someProperty: 12)
let keyPath = #keyPath(SomeClass.someProperty)

if let value = c.value(forKey: keyPath) {
    print(value)
}
// Prints "12"
```

<!--
When you use a key-path string expression within a class, you can refer to a property of that class by writing just the property name, without the class name.
-->

클래스 내에 키-경로 문자열 표현식을 사용할 때 클래스 이름 없이 프로퍼티 이름 만으로 해당 클래스의 프로퍼티에 참조할 수 있습니다.

```swift
extension SomeClass {
    func getSomeKeyPath() -> String {
        return #keyPath(someProperty)
    }
}
print(keyPath == c.getSomeKeyPath())
// Prints "true"
```

<!--
Because the key path string is created at compile time, not at runtime, the compiler can check that the property exists and that the property is exposed to the Objective-C runtime.

For more information about using key paths in Swift code that interacts with Objective-C APIs, see Using Objective-C Runtime Features in Swift. For information about key-value coding and key-value observing, see Key-Value Coding Programming Guide and Key-Value Observing Programming Guide.
-->

키 경로 문자열은 런타임이 아닌 컴파일 시에 생성되기 때문에 컴파일러는 프로퍼티가 존재하고 해당 프로퍼티가 Objective-C 런타임에 노출되는지 확인할 수 있습니다.

Objective-C API 와 상호작용하는 Swift 코드에서 키 경로를 사용하는 것에 대한 자세한 내용은 [Swift 에서 Objective-C 런타임 특성 사용 (Using Objective-C Runtime Features in Swift)](https://developer.apple.com/documentation/swift/using\_objective-c\_runtime\_features\_in\_swift) 를 참고 바랍니다. 키-값 코딩과 키-값 관찰에 대한 자세한 내용은 [키-값 코딩 프로그래밍 가이드 (Key-Value Coding Programming Guide)](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/KeyValueCoding/index.html#//apple\_ref/doc/uid/10000107i) 와 [키-값 관찰 프로그래밍 가이드 (Key-Value Observing Programming Guide)](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple\_ref/doc/uid/10000177i) 를 참고 바랍니다.

<!--
NOTE
Although the property name is an expression, it’s never evaluated.
-->

> NOTE\
> _프로퍼티 이름 (property name)_ 은 표현식이지만 절대 평가되지 않습니다.
>
> GRAMMAR OF A KEY-PATH STRING EXPRESSION\
> key-path-string-expression → `#keyPath` `(` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) `)`

## 접미사 표현식 (Postfix Expressions)

<!--
Postfix expressions are formed by applying a postfix operator or other postfix syntax to an expression. Syntactically, every primary expression is also a postfix expression.

For information about the behavior of these operators, see Basic Operators and Advanced Operators.

For information about the operators provided by the Swift standard library, see Operator Declarations.
-->

_접미사 표현식 (Postfix expressions)_ 은 접미사 연산자 (postfix operator) 또는 다른 접미사 구문 (other postfix syntax) 을 표현식에 적용하여 형성됩니다. 구문적으로 모든 기본 표현식은 접미사 표현식 입니다.

이러한 연산자의 동작에 대한 자세한 내용은 [기본 연산자 (Basic Operators)](../language-guide-1/basic-operators.md) 와 [고급 연산자 (Advanced Operators)](../language-guide-1/advanced-operators.md) 를 참고 바랍니다.

Swift 표준 라이브러리에 의해 제공되는 연산자에 대한 자세한 내용은 [연산자 선언 (Operator Declarations)](https://developer.apple.com/documentation/swift/swift\_standard\_library/operator\_declarations) 을 참고 바랍니다.

> GRAMMAR OF A POSTFIX EXPRESSION\
> postfix-expression → [primary-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_primary-expression)\
> postfix-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) [postfix-operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_postfix-operator)\
> postfix-expression → [function-call-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_function-call-expression)\
> postfix-expression → [initializer-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_initializer-expression)\
> postfix-expression → [explicit-member-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_explicit-member-expression)\
> postfix-expression → [postfix-self-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-self-expression)\
> postfix-expression → [subscript-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_subscript-expression)\
> postfix-expression → [forced-value-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_forced-value-expression)\
> postfix-expression → [optional-chaining-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_optional-chaining-expression)

### 함수 호출 표현식 (Function Call Expression)

<!--
A function call expression consists of a function name followed by a comma-separated list of the function’s arguments in parentheses. Function call expressions have the following form:
-->

_함수 호출 표현식 (function call expression)_ 은 함수 이름 다음에 소괄호로 둘러싸이고 콤마로 구분된 함수의 인수의 목록으로 구성됩니다. 함수 호출 표현식 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.22.08.png>)

<!--
The function name can be any expression whose value is of a function type.

If the function definition includes names for its parameters, the function call must include names before its argument values, separated by a colon (:). This kind of function call expression has the following form:
-->

_함수 이름 (function name)_ 은 값이 함수 타입인 모든 표현식이 될 수 있습니다.

함수 정의가 파라미터에 대한 이름을 포함하는 경우 함수 호출은 콜론 (`:`) 으로 분리하여 인수값 전에 이름을 반드시 포함해야 합니다. 이러한 종류의 함수 호출 표현식 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.22.46.png>)

<!--
A function call expression can include trailing closures in the form of closure expressions immediately after the closing parenthesis. The trailing closures are understood as arguments to the function, added after the last parenthesized argument. The first closure expression is unlabeled; any additional closure expressions are preceded by their argument labels. The example below shows the equivalent version of function calls that do and don’t use trailing closure syntax:
-->

함수 호출 표현식은 닫는 소괄호 바로 다음에 클로저 표현식의 형식으로 후행 클로저 (trailing closures) 를 포함할 수 있습니다. 이 후행 클로저는 마지막 괄호 인수 후에 추가된 함수의 인수로 이해됩니다. 첫번째 클로저 표현식은 라벨이 없습니다; 모든 추가 클로저 표현식은 인수 라벨이 앞에 옵니다. 아래 예제에서 후행 클로저 구문을 사용하거나 사용하지 않는 함수 호출의 동등한 버전을 보여줍니다:

```swift
// someFunction takes an integer and a closure as its arguments
someFunction(x: x, f: { $0 == 13 })
someFunction(x: x) { $0 == 13 }

// anotherFunction takes an integer and two closures as its arguments
anotherFunction(x: x, f: { $0 == 13 }, g: { print(99) })
anotherFunction(x: x) { $0 == 13 } g: { print(99) }
```

<!--
If the trailing closure is the function’s only argument, you can omit the parentheses.
-->

후행 클로저만 함수의 인수인 경우 소괄호를 생략할 수 있습니다.

```swift
// someMethod takes a closure as its only argument
myData.someMethod() { $0 == 13 }
myData.someMethod { $0 == 13 }
```

<!--
To include the trailing closures in the arguments, the compiler examines the function’s parameters from left to right as follows:
-->

인수에 후행 클로저를 포함하려면 컴파일러는 다음과 같이 왼쪽에서 오른쪽으로 함수의 파라미터를 검사합니다:

| 후행 클로저 |     파라미터    |                                    동작                                   |
| :----: | :---------: | :---------------------------------------------------------------------: |
|   라벨   |      라벨     |             라벨이 동일하면 클로저는 파라미터와 매치됩니다; 그렇지 않으면 파라미터를 건너뜁니다.             |
|   라벨   |    라벨 없음    |                               파라미터를 건너뜁니다.                              |
|  라벨 없음 | 라벨 또는 라벨 없음 | 아래 정의된대로 파라미터가 구조적으로 함수 타입과 유사하면 클로저는 파라미터와 매치됩니다; 그렇지 않으면 파라미터를 건너뜁니다. |

<!--
The trailing closure is passed as the argument for the parameter that it matches. Parameters that were skipped during the scanning process don’t have an argument passed to them—for example, they can use a default parameter. After finding a match, scanning continues with the next trailing closure and the next parameter. At the end of the matching process, all trailing closures must have a match.

A parameter structurally resembles a function type if the parameter isn’t an in-out parameter, and the parameter is one of the following:

* A parameter whose type is a function type, like (Bool) -> Int
* An autoclosure parameter whose wrapped expression’s type is a function type, like @autoclosure () -> ((Bool) -> Int)
* A variadic parameter whose array element type is a function type, like ((Bool) -> Int)...
* A parameter whose type is wrapped in one or more layers of optional, like Optional<(Bool) -> Int>
* A parameter whose type combines these allowed types, like (Optional<(Bool) -> Int>)...

When a trailing closure is matched to a parameter whose type structurally resembles a function type, but isn’t a function, the closure is wrapped as needed. For example, if the parameter’s type is an optional type, the closure is wrapped in Optional automatically.

To ease migration of code from versions of Swift prior to 5.3—which performed this matching from right to left—the compiler checks both the left-to-right and right-to-left orderings. If the scan directions produce different results, the old right-to-left ordering is used and the compiler generates a warning. A future version of Swift will always use the left-to-right ordering.
-->

후행 클로저는 매치하는 파라미터에 대해 인수로 전달됩니다. 검색 프로세스 중에 건너뛴 파라미터에는 인수가 전달되지 않습니다—예를 들어 기본 파라미터를 사용할 수 있습니다. 일치하는 항목을 찾은 후에 다음 후행 클로저와 다음 파라미터 검색을 계속 합니다. 일치 프로세스가 끝나면 모든 후행 클로저는 매치되어야 합니다.

파라미터가 in-out 파라미터가 아니고 파라미터가 다음 중 하나인 경우 _구조적 (structurally)_ 으로 함수 타입과 _유사합니다 (resembles)_:

* `(Bool) -> Int` 와 같이 타입이 함수 타입인 파라미터
* `@autoclosure () -> ((Bool) -> Int)` 와 같이 래핑된 표현식의 타입이 함수 타입인 autoclosure 파라미터
* `((Bool) -> Int)...` 와 같이 배열 요소 타입이 함수 타입인 가변 파라미터
* `Optional<(Bool) -> Int>` 와 같이 타입이 하나 이상의 옵셔널 레이어에 래핑된 파라미터
* `(Optional<(Bool) -> Int>)...` 와 같이 타입이 이러한 허용된 타입을 결합하는 파라미터

후행 클로저는 타입이 함수 타입과 구조적으로 유사하지만 함수는 아닌 파리미터와 일치하면 클로저는 필요에 따라 래핑됩니다. 예를 들어 파라미터의 타입이 옵셔널 타입인 경우 클로저는 자동으로 `Optional` 로 래핑됩니다.

오른쪽에서 왼쪽으로 매칭을 수행하는 Swift 5.3 이전 버전에서 코드를 쉽게 마이그레이션 하기위해 컴파일러는 왼쪽에서 오른쪽과 오른쪽에서 왼쪽으로 모두 확인합니다. 검색 방향이 다른 결과를 가져오면 오래된 오른쪽에서 왼쪽 순서가 사용되고 컴파일러는 경고를 발생시킵니다. Swift 의 향후 버전은 항상 왼쪽에서 오른쪽 순서가 사용될 것입니다.

```swift
typealias Callback = (Int) -> Int
func someFunction(firstClosure: Callback? = nil,
                  secondClosure: Callback? = nil) {
    let first = firstClosure?(10)
    let second = secondClosure?(20)
    print(first ?? "-", second ?? "-")
}

someFunction()  // Prints "- -"
someFunction { return $0 + 100 }  // Ambiguous
someFunction { return $0 } secondClosure: { return $0 }  // Prints "10 20"
```

<!--
In the example above, the function call marked “Ambiguous” prints “- 120” and produces a compiler warning on Swift 5.3. A future version of Swift will print “110 -”.

A class, structure, or enumeration type can enable syntactic sugar for function call syntax by declaring one of several methods, as described in Methods with Special Names.
-->

위의 예제에서 "Ambiguous" 로 표시된 함수 호출은 "- 120" 을 출력하고 Swift 5.3 에서 컴파일러는 경고가 발생합니다. Swift 햐우 버전은 "110 -" 이 출력될 것입니다.

클래스, 구조체, 또는 열거형 타입은 [특수한 이름이 있는 메서드 (Methods with Special Names)](declarations.md#methods-with-special-names) 에서 설명한대로 여러 메서드 중 하나로 선언하여 함수 호출 구문에 대한 문법적 설탕 (syntactic sugar) 를 활성화 할 수 있습니다.

#### **포인터 타입으로 암시적 변환 (Implicit Conversion to a Pointer Type)**

<!--
In a function call expression, if the argument and parameter have a different type, the compiler tries to make their types match by applying one of the implicit conversions in the following list:

* inout SomeType can become UnsafePointer<SomeType> or UnsafeMutablePointer<SomeType>
* inout Array<SomeType> can become UnsafePointer<SomeType> or UnsafeMutablePointer<SomeType>
* Array<SomeType> can become UnsafePointer<SomeType>
* String can become UnsafePointer<CChar>

The following two function calls are equivalent:
-->

함수 호출 표현식에서 인수와 파라미터가 다른 타입을 갖는 경우 컴파일러는 다음 목록의 암시적 변환 중 하나를 적용하여 해당 타입을 일치시키려고 합니다:

* `inout SomeType` 은 `UnsafePointer<SomeType>` 또는 `UnsafeMutablePointer<SomeType>` 이 될 수 있습니다.
* `inout Array<SomeType>` 은 `UnsafePointer<SomeType>` 또는 `UnsafeMutablePointer<SomeType>` 이 될 수 있습니다.
* `Array<SomeType>` 은 `UnsafePointer<SomeType>` 이 될 수 있습니다.
* `String` 은 `UnsafePointer<CChar>` 가 될 수 있습니다.

다음 두가지 함수 호출은 동일합니다:

```swift
func unsafeFunction(pointer: UnsafePointer<Int>) {
    // ...
}
var myNumber = 1234

unsafeFunction(pointer: &myNumber)
withUnsafePointer(to: myNumber) { unsafeFunction(pointer: $0) }
```

<!--
A pointer that’s created by these implicit conversions is valid only for the duration of the function call. To avoid undefined behavior, ensure that your code never persists the pointer after the function call ends.
-->

이러한 암시적 변환에 의해 생성된 포인터는 함수 호출 동안에만 유효합니다. 정의되지 않은 동작을 방지하려면 함수 호출이 끝난 후에 코드가 포인터를 유지되지 않도록 해야합니다.

<!--
NOTE
When implicitly converting an array to an unsafe pointer, Swift ensures that the array’s storage is contiguous by converting or copying the array as needed. For example, you can use this syntax with an array that was bridged to Array from an NSArray subclass that makes no API contract about its storage. If you need to guarantee that the array’s storage is already contiguous, so the implicit conversion never needs to do this work, use ContiguousArray instead of Array.
-->

> NOTE\
> 암시적으로 배열을 안전하지 않은 포인터 (unsafe pointer) 로 변환할 때 Swift 는 필요에 따라 배열을 변환하거나 복사하여 배열의 저장소가 연속되도록 합니다. 예를 들어 저장소에 대한 API 계약을 작성하지 않은 `NSArray` 하위클래스에서 `Array` 로 연결된 배열에 이 구문을 사용할 수 있습니다. 배열의 저장소가 이미 연속됨을 보장해야 하므로 암시적 변환은 이 작업을 수행할 필요가 없는 경우 `Array` 대신 `ContiguousArray` 를 사용하십시오.

<!--
Using & instead of an explicit function like withUnsafePointer(to:) can help make calls to low-level C functions more readable, especially when the function takes several pointer arguments. However, when calling functions from other Swift code, avoid using & instead of using the unsafe APIs explicitly.
-->

`withUnsafePointer(to:)` 와 같이 명시적 함수 대신 `&` 사용하는 것은 특히 함수가 여러 포인터 인수를 가지고 있을 때 저수준 C 함수 (low-level C functions) 를 더 읽기 쉽게 호출할 수 있습니다. 그러나 다른 Swift 코드에서 함수를 호출할 때 안전하지 않은 API 를 명시적으로 사용하는 대신 `&` 을 사용하는 것은 피해야 합니다.

> GRAMMAR OF A FUNCTION CALL EXPRESSION\
> function-call-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) [function-call-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_function-call-argument-clause)\
> function-call-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) [function-call-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_function-call-argument-clause) opt [trailing-closures](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_trailing-closures)\
> function-call-argument-clause → `(` `)` | `(` [function-call-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_function-call-argument-list) `)`\
> function-call-argument-list → [function-call-argument](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_function-call-argument) | [function-call-argument](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_function-call-argument) `,` [function-call-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_function-call-argument-list)\
> function-call-argument → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression) | [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression)\
> function-call-argument → [operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_operator) | [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) `:` [operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_operator)\
> trailing-closures → [closure-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_closure-expression) [labeled-trailing-closures](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_labeled-trailing-closures) $$_{opt}$$\
> labeled-trailing-closures → [labeled-trailing-closure](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_labeled-trailing-closure) [labeled-trailing-closures](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_labeled-trailing-closures) $$_{opt}$$\
> labeled-trailing-closure → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) `:` [closure-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_closure-expression)

### 초기화 구문 표현식 (Initializer Expression)

<!--
An initializer expression provides access to a type’s initializer. It has the following form:
-->

_초기화 구문 표현식 (initializer expression)_ 은 타입의 초기화 구문에 접근하는 것을 제공합니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.33.27.png>)

<!--
You use the initializer expression in a function call expression to initialize a new instance of a type. You also use an initializer expression to delegate to the initializer of a superclass.
-->

타입의 새로운 인스턴스를 초기화하기 위해 함수 호출 표현식 내에 초기화 구문 표현식을 사용합니다. 상위클래스의 초기화 구문을 위임하기 위해 초기화 구문 표현식을 사용할 수도 있습니다.

```swift
class SomeSubClass: SomeSuperClass {
    override init() {
        // subclass initialization goes here
        super.init()
    }
}
```

<!--
Like a function, an initializer can be used as a value. For example:
-->

함수와 같이 초기화 구문은 값으로 사용될 수 있습니다. 예를 들어:

```swift
// Type annotation is required because String has multiple initializers.
let initializer: (Int) -> String = String.init
let oneTwoThree = [1, 2, 3].map(initializer).reduce("", +)
print(oneTwoThree)
// Prints "123"
```

<!--
If you specify a type by name, you can access the type’s initializer without using an initializer expression. In all other cases, you must use an initializer expression.
-->

이름으로 타입을 지정하면 초기화 구문 표현식 사용없이 타입의 초기화 구문에 접근할 수 있습니다. 다른 모든 상황에서는 초기화 구문 표현식을 사용해야 합니다.

```swift
let s1 = SomeType.init(data: 3)  // Valid
let s2 = SomeType(data: 1)       // Also valid

let s3 = type(of: someValue).init(data: 7)  // Valid
let s4 = type(of: someValue)(data: 5)       // Error
```

> GRAMMAR OF AN INITIALIZER EXPRESSION\
> initializer-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) `.` `init`\
> initializer-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) `.` `init` `(` [argument-names](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_argument-names) `)`

### 명시적 멤버 표현식 (Explicit Member Expression)

<!--
An explicit member expression allows access to the members of a named type, a tuple, or a module. It consists of a period (.) between the item and the identifier of its member.
-->

_명시적 멤버 표현식 (explicit member expression)_ 은 명명된 타입, 튜플, 또는 모듈의 멤버에 접근을 허용합니다. 이것은 멤버의 아이템과 식별자 사이에 마침표 (`.`) 으로 구성됩니다.

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.36.37.png>)

<!--
The members of a named type are named as part of the type’s declaration or extension. For example:
-->

명명된 타입의 멤버는 타입의 선언 또는 표현식의 부분으로 명명됩니다. 예를 들어:

```swift
class SomeClass {
    var someProperty = 42
}
let c = SomeClass()
let y = c.someProperty  // Member access
```

<!--
The members of a tuple are implicitly named using integers in the order they appear, starting from zero. For example:
-->

튜플의 멤버는 0을 시작으로 순서대로 정수를 사용하여 암시적으로 명명됩니다. 예를 들어:

```swift
var t = (10, 20, 30)
t.0 = t.1
// Now t is (20, 20, 30)
```

<!--
The members of a module access the top-level declarations of that module.

Types declared with the dynamicMemberLookup attribute include members that are looked up at runtime, as described in Attributes.

To distinguish between methods or initializers whose names differ only by the names of their arguments, include the argument names in parentheses, with each argument name followed by a colon (:). Write an underscore (_) for an argument with no name. To distinguish between overloaded methods, use a type annotation. For example:
-->

모듈의 멤버는 해당 모듈의 최상위 선언에 접근합니다.

`dynamicMemberLookup` 속성으로 선언된 타입은 [속성 (Attributes)](attributes.md) 에서 설명한대로 런타임 시 조회되는 멤버가 포함됩니다.

인수 이름만으로 이름이 다른 메서드 또는 초기화 구문을 구별하려면 인수 이름을 소괄호 안에 포함하고 각 인수 이름 뒤에 콜론 (`:`) 을 붙입니다. 인수에 이름이 없는 경우 언더바 (`_`) 를 작성합니다. 오버로드된 메서드를 구별하려면 타입 주석을 사용합니다. 예를 들어:

```swift
class SomeClass {
    func someMethod(x: Int, y: Int) {}
    func someMethod(x: Int, z: Int) {}
    func overloadedMethod(x: Int, y: Int) {}
    func overloadedMethod(x: Int, y: Bool) {}
}
let instance = SomeClass()

let a = instance.someMethod              // Ambiguous
let b = instance.someMethod(x:y:)        // Unambiguous

let d = instance.overloadedMethod        // Ambiguous
let d = instance.overloadedMethod(x:y:)  // Still ambiguous
let d: (Int, Bool) -> Void  = instance.overloadedMethod(x:y:)  // Unambiguous
```

<!--
If a period appears at the beginning of a line, it’s understood as part of an explicit member expression, not as an implicit member expression. For example, the following listing shows chained method calls split over several lines:
-->

줄 시작에 마침표가 나타나면 암시적 멤버 표현식이 아닌 명시적 멤버 표현식의 부분으로 이해됩니다. 예를 들어 다음 목록은 여러줄로 분할된 메서드 호출 체인을 보여줍니다:

```swift
let x = [10, 3, 20, 15, 4]
    .sorted()
    .filter { $0 > 5 }
    .map { $0 * 100 }
```

<!--
You can combine this multiline chained syntax with compiler control statements to control when each method is called. For example, the following code uses a different filtering rule on iOS:
-->

이 여러줄로 연결된 구문을 컴파일러 제어문과 결합하여 각 메서드가 호출되는 시기를 제어할 수 있습니다. 예를 들어 다음 코드는 iOS 에서 다른 필터링 규칙을 사용합니다:

```swift
let numbers = [10, 20, 33, 43, 50]
#if os(iOS)
.filter { $0 < 40 }
#else
.filter { $0 > 25 }
#endif
```

<!--
Between #if, #endif, and other compilation directives, the conditional compilation block can contain an implicit member expression followed by zero or more postfixes, to form a postfix expression. It can also contain another conditional compilation block, or a combination of these expressions and blocks.

You can use this syntax anywhere that you can write an explicit member expression, not just in top-level code.

In the conditional compilation block, the branch for the #if compilation directive must contain at least one expression. The other branches can be empty.
-->

`#if`, `#endif` 와 다른 컴파일 지시문 사이에 조건부 컴파일 블록은 암시적 멤버 표현식 뒤에 접미사를 포함하지 않거나 많은 접미사를 포함하여 접미사 표현식을 형성할 수 있습니다. 다른 조건부 컴파일러 블럭 또는 이러한 표현식과 블럭을 포함할 수도 있습니다.

최상위 코드 뿐만 아니라 명시적 멤버 표현식을 작성할 수 있는 모든 곳에서 이 구문을 사용할 수 있습니다.

조건부 컴파일러 블럭에서 `#if` 컴파일러 지시문의 분기에는 하나 이상의 표현식이 포함되어야 합니다. 다른 분기는 비어있을 수 있습니다.

> GRAMMAR OF AN EXPLICIT MEMBER EXPRESSION\
> explicit-member-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) `.` [decimal-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_decimal-digits)\
> explicit-member-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) `.` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) [generic-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-argument-clause) $$_{opt}$$\
> explicit-member-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) `.` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) `(` [argument-names](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_argument-names) `)`\
> explicit-member-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) [conditional-compilation-block](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_conditional-compilation-block)\
> argument-names → [argument-name](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_argument-name) [argument-names](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_argument-names) $$_{opt}$$\
> argument-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) `:`

### 접미사 Self 표현식 (Postfix Self Expression)

<!--
A postfix self expression consists of an expression or the name of a type, immediately followed by .self. It has the following forms:
-->

접미사 `self` 표현식 (postfix `self` expression) 은 표현식 또는 타입의 이름 뒤에 `.self` 로 구성됩니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.40.20.png>)

<!--
The first form evaluates to the value of the expression. For example, x.self evaluates to x.

The second form evaluates to the value of the type. Use this form to access a type as a value. For example, because SomeClass.self evaluates to the SomeClass type itself, you can pass it to a function or method that accepts a type-level argument.
-->

첫번째 형식은 _표현식 (expression)_ 의 값으로 평가됩니다. 예를 들어 `x.self` 는 `x` 로 평가됩니다.

두번째 형식은 _타입 (type)_ 의 값으로 평가됩니다. 값으로 타입을 접근하려면 이 형식을 사용합니다. 예를 들어 `SomeClass.self` 는 `SomeClass` 타입 자체로 평가되므로 타입 수준 인수 (type-level argument) 를 허용하는 함수나 메서드에 전달할 수 있습니다.

> GRAMMAR OF A POSTFIX SELF EXPRESSION\
> postfix-self-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) `.` `self`

### 서브 스크립트 표현식 (Subscript Expression)

<!--
A subscript expression provides subscript access using the getter and setter of the corresponding subscript declaration. It has the following form:
-->

_서브 스크립트 표현식 (subscript expression)_ 은 해당 서브 스크립트 선언의 getter 와 setter 를 사용하여 서브 스크립트에 접근을 제공합니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.41.49.png>)

<!--
To evaluate the value of a subscript expression, the subscript getter for the expression’s type is called with the index expressions passed as the subscript parameters. To set its value, the subscript setter is called in the same way.

For information about subscript declarations, see Protocol Subscript Declaration.
-->

서브 스크립트 표현식의 값을 평가하기 위해 _표현식 (expression)_ 의 타입에 대한 서브 스크립트 getter 는 서브 스크립트 파라미터로 _인덱스 표현식 (index expressions)_ 을 전달하여 호출됩니다. 값을 설정하기 위해선 서브 스크립트 setter 는 동일한 방식으로 호출됩니다.

서브 스크립트 선언에 대한 자세한 내용은 [프로토콜 서브 스크립트 선언 (Protocol Subscript Declaration)](declarations.md#protocol-subscript-declaration) 을 참고 바랍니다.

> GRAMMAR OF A SUBSCRIPT EXPRESSION\
> subscript-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) `[` [function-call-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_function-call-argument-list) `]`

### 강제-값 표현식 (Forced-Value Expression)

<!--
A forced-value expression unwraps an optional value that you are certain isn’t nil. It has the following form:
-->

_강제-값 표현식 (forced-value expression)_ 은 `nil` 이 아니라고 확신하는 옵셔널 값을 언래핑 합니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.43.09.png>)

<!--
If the value of the expression isn’t nil, the optional value is unwrapped and returned with the corresponding non-optional type. Otherwise, a runtime error is raised.

The unwrapped value of a forced-value expression can be modified, either by mutating the value itself, or by assigning to one of the value’s members. For example:
-->

_표현식 (expression)_ 의 값이 `nil` 이 아니라면 옵셔널 값은 언래핑 되고 해당 옵셔널이 아닌 타입을 반환합니다. 그렇지 않으면 런타임 에러가 발생합니다.

강제-값 표현식의 언래핑된 값은 값 자체를 변경하거나 값의 멤버중 하나에 할당하여 수정할 수 있습니다. 예를 들어:

```swift
var x: Int? = 0
x! += 1
// x is now 1

var someDictionary = ["a": [1, 2, 3], "b": [10, 20]]
someDictionary["a"]![0] = 100
// someDictionary is now ["a": [100, 2, 3], "b": [10, 20]]
```

> GRAMMAR OF A FORCED-VALUE EXPRESSION\
> forced-value-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) `!`

### 옵셔널-체이닝 표현식 (Optional-Chaining Expression)

<!--
An optional-chaining expression provides a simplified syntax for using optional values in postfix expressions. It has the following form:
-->

_옵셔널-체이닝 표현식 (optional-chaining expression)_ 은 접미사 표현식으로 옵셔널 값을 사용하기 위해 간단한 구문을 제공합니다. 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-21 오후 11.45.05.png>)

<!--
The postfix ? operator makes an optional-chaining expression from an expression without changing the expression’s value.

Optional-chaining expressions must appear within a postfix expression, and they cause the postfix expression to be evaluated in a special way. If the value of the optional-chaining expression is nil, all of the other operations in the postfix expression are ignored and the entire postfix expression evaluates to nil. If the value of the optional-chaining expression isn’t nil, the value of the optional-chaining expression is unwrapped and used to evaluate the rest of the postfix expression. In either case, the value of the postfix expression is still of an optional type.

If a postfix expression that contains an optional-chaining expression is nested inside other postfix expressions, only the outermost expression returns an optional type. In the example below, when c isn’t nil, its value is unwrapped and used to evaluate .property, the value of which is used to evaluate .performAction(). The entire expression c?.property.performAction() has a value of an optional type.
-->

접미사 `?` 연산자는 표현식의 값 변경없이 표현식에서 옵셔널-체이닝 표현식을 만듭니다.

옵셔널-체이닝 표현식은 접미사 표현식 내에 나타나야 하고 접미사 표현식이 특별한 방법으로 평가되도록 합니다. 옵셔널-체이닝 표현식의 값이 `nil` 이면 접미사 표현식의 다른 모든 동작은 무시되고 전체 접미사 표현식은 `nil` 로 평가됩니다. 옵셔널-체이닝 표현식의 값이 `nil` 이 아니면 옵셔널-체이닝 표현식의 값은 언래핑되고 나머지 접미사 표현식에 평가하기 위해 사용됩니다. 두 경우 모두 접미사 표현식의 값은 여전히 옵셔널 타입입니다.

옵셔널-체이닝 표현식이 포함된 접미사 표현식이 다른 접미사 표현식 내에 중첩되었다면 가장 바깥쪽 표현식만 옵셔널 타입을 반환합니다. 아래의 예제에서 `c` 가 `nil` 이 아닐 때 `.performAction()` 을 평가하기 위해 사용되는 `.property` 를 평가하는데 사용됩니다. 전체 표현식 `c?.property.performAction()` 은 옵셔널 타입의 값을 가집니다.

```swift
var c: SomeClass?
var result: Bool? = c?.property.performAction()
```

<!--
The following example shows the behavior of the example above without using optional chaining.
-->

다음의 예제는 옵셔널 체이닝 사용없이 위의 예제의 동작을 보여줍니다.

```swift
var result: Bool?
if let unwrappedC = c {
    result = unwrappedC.property.performAction()
}
```

<!--
The unwrapped value of an optional-chaining expression can be modified, either by mutating the value itself, or by assigning to one of the value’s members. If the value of the optional-chaining expression is nil, the expression on the right-hand side of the assignment operator isn’t evaluated. For example:
-->

옵셔널-체이닝 표현식의 언래핑된 값은 값 자체를 변경하거나 값의 멤버중 하나에 할당을 통해 변경될 수 있습니다. 옵셔널-체이닝 표현식의 값이 `nil` 이라면 할당 연산자의 오른편의 표현식은 평가되지 않습니다. 예를 들어:

```swift
func someFunctionWithSideEffects() -> Int {
    return 42  // No actual side effects.
}
var someDictionary = ["a": [1, 2, 3], "b": [10, 20]]

someDictionary["not here"]?[0] = someFunctionWithSideEffects()
// someFunctionWithSideEffects isn't evaluated
// someDictionary is still ["a": [1, 2, 3], "b": [10, 20]]

someDictionary["a"]?[0] = someFunctionWithSideEffects()
// someFunctionWithSideEffects is evaluated and returns 42
// someDictionary is now ["a": [42, 2, 3], "b": [10, 20]]
```

> GRAMMAR OF AN OPTIONAL-CHAINING EXPRESSION\
> optional-chaining-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_postfix-expression) `?`
