# 선언 (Declarations)

<!--
A declaration introduces a new name or construct into your program. For example, you use declarations to introduce functions and methods, to introduce variables and constants, and to define enumeration, structure, class, and protocol types. You can also use a declaration to extend the behavior of an existing named type and to import symbols into your program that are declared elsewhere.

In Swift, most declarations are also definitions in the sense that they’re implemented or initialized at the same time they’re declared. That said, because protocols don’t implement their members, most protocol members are declarations only. For convenience and because the distinction isn’t that important in Swift, the term declaration covers both declarations and definitions.
-->

_선언 (declaration)_ 은 프로그램에 새로운 이름 또는 구성을 도입합니다. 예를 들어 함수와 메서드를 도입하기 위해, 변수와 상수를 도입하기 위해, 그리고 열거형, 구조체, 클래스, 그리고 프로토콜 타입을 정의하기 위해 선언을 사용합니다. 기존에 명명된 타입의 동작을 확장하기 위해 그리고 다른 곳에서 선언된 기호를 프로그램으로 가져오기 위해 선언을 사용할 수도 있습니다.

Swift 에서 대부분의 선언은 선언과 동시에 구현되거나 초기화 된다는 의미에서도 정의이기도 합니다. 이 말은 프로토콜은 멤버를 정의하지 않으므로 대부분 프로토콜 멤버는 선언만 있습니다. 편의상 그리고 Swift 에서 구별이 그다지 중요하지 않으므로 _선언 (declaration)_ 은 선언과 정의를 모두 포함합니다.

> GRAMMAR OF A DECLARATION\
> declaration → [import-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_import-declaration)\
> declaration → [constant-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_constant-declaration)\
> declaration → [variable-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-declaration)\
> declaration → [typealias-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_typealias-declaration)\
> declaration → [function-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_function-declaration)\
> declaration → [enum-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_enum-declaration)\
> declaration → [struct-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_struct-declaration)\
> declaration → [class-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_class-declaration)\
> declaration → [actor-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_actor-declaration) \
> declaration → [protocol-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_protocol-declaration)\
> declaration → [initializer-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer-declaration)\
> declaration → [deinitializer-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_deinitializer-declaration)\
> declaration → [extension-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_extension-declaration)\
> declaration → [subscript-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_subscript-declaration)\
> declaration → [operator-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_operator-declaration)\
> declaration → [precedence-group-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-declaration)\
> declarations → [declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration) [declarations](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declarations) $$_{opt}$$

## 최상위-수준 코드 (Top-Level Code)

<!--
The top-level code in a Swift source file consists of zero or more statements, declarations, and expressions. By default, variables, constants, and other named declarations that are declared at the top-level of a source file are accessible to code in every source file that’s part of the same module. You can override this default behavior by marking the declaration with an access-level modifier, as described in Access Control Levels.

There are two kinds of top-level code: top-level declarations and executable top-level code. Top-level declarations consist of only declarations, and are allowed in all Swift source files. Executable top-level code contains statements and expressions, not just declarations, and is allowed only as the top-level entry point for the program.

The Swift code you compile to make an executable can contain at most one of the following approaches to mark the top-level entry point, regardless of how the code is organized into files and modules: the main attribute, the NSApplicationMain attribute, the UIApplicationMain attribute, a main.swift file, or a file that contains top-level executable code.
-->

Swift 소스 파일에서 최상위-수준 코드는 0 또는 그 이상의 구문, 선언, 그리고 표현식으로 구성됩니다. 기본적으로 소스 파일의 최상위-수준에 선언된 변수, 상수, 그리고 다른 명명된 선언은 동일한 모듈의 일부인 모든 소스 파일 코드에 접근할 수 있습니다. [접근 제어 수준 (Access Control Levels)](declarations.md#access-control-levels) 에서 설명된대로 접근-수준 수정자 (access-level modifier) 로 선언을 표시하여 기본 동작을 재정의할 수 있습니다.

최상위-수준 선언 (top-level declarations) 과 실행 가능한 최상위-수준 코드 (executable top-level code) 인 최상위-수준 코드의 두가지 종류가 있습니다. 최상위-수준 선언은 선언으로만 구성되고 모든 Swift 소스 파일에서 허용됩니다. 실행 가능한 최상위-수준 코드는 선언 뿐만 아니라 구문과 표현식을 포함하고 프로그램에 대해 최상위-수준 진입점으로만 허용됩니다.

실행 가능하게 만들기 위해 컴파일 한 Swift 코드는 코드가 파일과 모듈로 구성되는 방식과 상관없이 최상위-수준 진입점을 나타내는 다음 접근 방식 중 하나만 포함할 수 있습니다: `main` 속성, `NSApplicationMain` 속성, `UIApplicationMain` 속성, `main.swift` 파일, 또는 최상위-수준 실행 가능한 코드를 포함하는 파일.

> GRAMMAR OF A TOP-LEVEL DECLARATION\
> top-level-declaration → [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statements) $$_{opt}$$

## 코드 블럭 (Code Blocks)

<!--
A code block is used by a variety of declarations and control structures to group statements together. It has the following form:
-->

_코드 블럭 (code block)_ 은 선언 및 제어 구조에서 구문을 그룹화 하기위해 사용됩니다. 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 11.27.49.png>)

<!--
The statements inside a code block include declarations, expressions, and other kinds of statements and are executed in order of their appearance in source code.
-->

코드 블럭 내에 _구문 (statements)_ 은 선언, 표현식, 그리고 구문의 다른 종류가 포함되고 소스 코드에 나타나는 순서대로 실행됩니다.

> GRAMMAR OF A CODE BLOCK\
> code-block → `{` [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_statements) $$_{opt}$$ `}`

## 가져오기 선언 (Import Declaration)

<!--
An import declaration lets you access symbols that are declared outside the current file. The basic form imports the entire module; it consists of the import keyword followed by a module name:
-->

_가져오기 선언 (import declaration)_ 을 사용하여 현재 파일 바깥에 선언된 기호에 접근할 수 있습니다. 기본 형식은 전체 모듈을 가져옵니다; `import` 키워드 다음에 모듈 이름이 따라오도록 구성됩니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 11.28.43.png>)

<!--
Providing more detail limits which symbols are imported—you can specify a specific submodule or a specific declaration within a module or submodule. When this detailed form is used, only the imported symbol (and not the module that declares it) is made available in the current scope.
-->

가져올 기호에 대한 자세한 제한을 제공하면 모듈 또는 하위 모듈 내에 특정 하위 모듈 또는 특정 선언을 지정할 수 있습니다. 이런 상세한 형식이 사용되면 선언한 모듈이 아닌 가져온 기호만 현재 범위에서 사용할 수 있습니다.

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 11.29.21.png>)

> GRAMMAR OF AN IMPORT DECLARATION\
> import-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ `import` [import-kind](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_import-kind) $$_{opt}$$ [import-path](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_import-path)\
> import-kind → `typealias` | `struct` | `class` | `enum` | `protocol` | `let` | `var` | `func`\
> import-path → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) | [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) `.` [import-path](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_import-path)

## 상수 선언 (Constant Declaration)

<!--
A constant declaration introduces a constant named value into your program. Constant declarations are declared using the let keyword and have the following form:
-->

_상수 선언 (constant declaration)_ 은 프로그램에 명명된 상수값을 도입합니다. 상수 선언은 `let` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 11.30.18.png>)

<!--
A constant declaration defines an immutable binding between the constant name and the value of the initializer expression; after the value of a constant is set, it can’t be changed. That said, if a constant is initialized with a class object, the object itself can change, but the binding between the constant name and the object it refers to can’t.

When a constant is declared at global scope, it must be initialized with a value. When a constant declaration occurs in the context of a function or method, it can be initialized later, as long as it’s guaranteed to have a value set before the first time its value is read. If the compiler can prove that the constant’s value is never read, the constant isn’t required to have a value set at all. When a constant declaration occurs in the context of a class or structure declaration, it’s considered a constant property. Constant declarations aren’t computed properties and therefore don’t have getters or setters.

If the constant name of a constant declaration is a tuple pattern, the name of each item in the tuple is bound to the corresponding value in the initializer expression.
-->

상수 선언은 _상수 이름 (constant name)_ 과 초기화 구문 _표현식 (expression)_ 의 값 사이의 변경 불가능한 바인딩을 정의합니다; 상수의 값이 설정된 후에 변경할 수 없습니다. 이 말은 상수가 클래스 객체로 초기화되면 이 객체 자체는 변경할 수 있지만 이 상수 이름과 참조하는 객체 간의 바인딩은 변경할 수 없습니다.

상수가 전역 범위로 선언되면 값으로 반드시 초기화 되어야 합니다. 상수 선언이 함수 또는 메서드의 컨텍스트에 나타나면 해당 값을 처음 읽기 전에 값을 설정한다는 보장이 있는 한 나중에 초기화 될 수 있습니다. 컴파일러가 상수의 값을 읽지 않는다는 것을 증명할 수 있으면 상수에 값을 설정하지 않아도 됩니다. 상수 선언이 클래스 또는 구조체 선언의 컨텍스트에서 나타나면 _상수 프로퍼티 (constant property)_ 로 간주됩니다. 상수 선언은 계산된 프로퍼티가 아니므로 getter 또는 setter 를 가지지 않습니다.

상수 선언의 _상수 이름 (constant name)_ 이 튜플 패턴인 경우 튜플에서 각 항목의 이름은 초기화 구문 _표현식 (expression)_ 에서 해당 값으로 바인딩 됩니다.

```swift
let (firstNumber, secondNumber) = (10, 42)
```

<!--
In this example, firstNumber is a named constant for the value 10, and secondNumber is a named constant for the value 42. Both constants can now be used independently:
-->

이 예제에서 `firstNumber` 는 값 `10` 에 대한 명명된 상수이고 `secondNumber` 는 값 `42` 에 대한 명명된 상수 입니다. 두 상수 모두 독립적으로 사용할 수 있습니다:

```swift
print("The first number is \(firstNumber).")
// Prints "The first number is 10."
print("The second number is \(secondNumber).")
// Prints "The second number is 42."
```

<!--
The type annotation (: type) is optional in a constant declaration when the type of the constant name can be inferred, as described in Type Inference.

To declare a constant type property, mark the declaration with the static declaration modifier. A constant type property of a class is always implicitly final; you can’t mark it with the class or final declaration modifier to allow or disallow overriding by subclasses. Type properties are discussed in Type Properties.

For more information about constants and for guidance about when to use them, see Constants and Variables and Stored Properties.
-->

타입 주석 (`:` _타입_) 은 [타입 추론 (Type Inference)](types.md#type-inference) 에서 설명한대로 _상수 이름 (constant name)_ 의 타입이 추론될 수 있을 때 상수 선언에서 선택사항입니다.

상수 타입 프로퍼티 (constant type property) 를 선언하려면 `static` 선언 수식어로 선언을 표시해야 합니다. 클래스의 상수 타입 프로퍼티는 암시적으로 final 입니다; `class` 또는 `final` 선언 수식어로 표시하여 하위클래스에 의한 재정의를 허용하거나 금지할 수 있습니다. 타입 프로퍼티는 [타입 프로퍼티 (Type Properties)](../language-guide-1/properties.md#type-properties) 에 설명되어 있습니다.

상수에 대한 자세한 내용과 사용시 지침에 대한 내용은 [상수와 변수 (Constants and Variables)](../language-guide-1/the-basics.md#constants-and-variables) 와 [저장된 프로퍼티 (Stored Properties)](../language-guide-1/properties.md#stored-properties) 를 참고 바랍니다.

> GRAMMAR OF A CONSTANT DECLARATION\
> constant-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [declaration-modifiers](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration-modifiers) $$_{opt}$$ `let` [pattern-initializer-list](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_pattern-initializer-list)\
> pattern-initializer-list → [pattern-initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_pattern-initializer) | [pattern-initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_pattern-initializer) `,` [pattern-initializer-list](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_pattern-initializer-list)\
> pattern-initializer → [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar\_pattern) [initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer) $$_{opt}$$\
> initializer → `=` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression)

## 변수 선언 (Variable Declaration)

<!--
A variable declaration introduces a variable named value into your program and is declared using the var keyword.

Variable declarations have several forms that declare different kinds of named, mutable values, including stored and computed variables and properties, stored variable and property observers, and static variable properties. The appropriate form to use depends on the scope at which the variable is declared and the kind of variable you intend to declare.
-->

_변수 선언 (variable declaration)_ 은 프로그램에 명명된 변수 값을 도입하고 `var` 키워드를 사용하여 선언됩니다.

변수 선언은 명명된, 변경 가능한 값, 저장된 그리고 계산된 변수와 프로퍼티, 저장된 변수와 프로퍼티 관찰자, 그리고 정적 변수 프로퍼티의 다양한 종류의 선언 형식을 가집니다. 사용할 적절한 형식은 변수가 선언되는 범위와 선언하려는 변수의 종류에 따라 다릅니다.

<!--
NOTE
You can also declare properties in the context of a protocol declaration, as described in Protocol Property Declaration.
-->

> NOTE\
> [프로토콜 프로퍼티 선언 (Protocol Property Declaration)](declarations.md#protocol-property-declaration) 에서 설명한대로 프로토콜 선언의 컨텍스트에서 프로퍼티를 선언할 수도 있습니다.

<!--
You can override a property in a subclass by marking the subclass’s property declaration with the override declaration modifier, as described in Overriding.
-->

[재정의 (Overriding)](../language-guide-1/inheritance.md#overriding) 에서 설명한대로 `override` 선언 수식어로 하위클래스의 프로퍼티 선언을 표시하여 하위클래스에 프로퍼티를 재정의 할 수 있습니다.

### 저장된 변수와 저장된 변수 프로퍼티 (Stored Variables and Stored Variable Properties)

<!--
The following form declares a stored variable or stored variable property:
-->

다음의 형식으로 저장된 변수 (stored variable) 또는 저장된 변수 프로퍼티 (stored variable property) 를 선언할 수 있습니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 11.32.58.png>)

<!--
You define this form of a variable declaration at global scope, the local scope of a function, or in the context of a class or structure declaration. When a variable declaration of this form is declared at global scope or the local scope of a function, it’s referred to as a stored variable. When it’s declared in the context of a class or structure declaration, it’s referred to as a stored variable property.

The initializer expression can’t be present in a protocol declaration, but in all other contexts, the initializer expression is optional. That said, if no initializer expression is present, the variable declaration must include an explicit type annotation (: type).

As with constant declarations, if the variable name is a tuple pattern, the name of each item in the tuple is bound to the corresponding value in the initializer expression.

As their names suggest, the value of a stored variable or a stored variable property is stored in memory.
-->

이러한 형식의 변수 선언은 전역 범위, 함수의 지역 범위, 또는 클래스나 구조체 선언의 컨텍스트에서 정의합니다. 이러한 형식의 변수 선언이 전역 범위나 함수의 지역 범위로 선언되면 _저장된 변수 (stored variable)_ 로 참조됩니다. 클래스나 구조체 선언의 컨텍스트에서 선언되면 _저장된 변수 프로퍼티 (stored variable property)_ 로 참조됩니다.

초기화 구문 _표현식 (expression)_ 은 프로토콜 선언에 있을 수 없지만 다른 모든 컨텍스트에서 초기화 구문 _표현식 (expression)_ 은 선택사항입니다. 즉, 초기화 구문 _표현식 (expression)_ 이 없으면 변수 선언은 명시적 타입 주석 (`:` 타입) 을 포함해야 합니다.

상수 선언과 마찬가지로 _변수 이름 (variable name)_ 이 튜플 패턴인 경우 튜플에서 각 항목에 이름은 초기화 구문 _표현식 (expression)_ 에서 해당 값에 바인딩 됩니다.

이름에서 알 수 있듯이, 저장된 변수 또는 저장된 변수 프로퍼티의 값은 메모리에 저장됩니다.

### 계산된 변수와 계산된 프로퍼티 (Computed Variables and Computed Properties)

<!--
The following form declares a computed variable or computed property:
-->

다음의 형식은 계산된 변수 (computed variable) 또는 계산된 프로퍼티 (computed property) 를 선언합니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 11.33.43.png>)

<!--
You define this form of a variable declaration at global scope, the local scope of a function, or in the context of a class, structure, enumeration, or extension declaration. When a variable declaration of this form is declared at global scope or the local scope of a function, it’s referred to as a computed variable. When it’s declared in the context of a class, structure, or extension declaration, it’s referred to as a computed property.

The getter is used to read the value, and the setter is used to write the value. The setter clause is optional, and when only a getter is needed, you can omit both clauses and simply return the requested value directly, as described in Read-Only Computed Properties. But if you provide a setter clause, you must also provide a getter clause.

The setter name and enclosing parentheses is optional. If you provide a setter name, it’s used as the name of the parameter to the setter. If you don’t provide a setter name, the default parameter name to the setter is newValue, as described in Shorthand Setter Declaration.

Unlike stored named values and stored variable properties, the value of a computed named value or a computed property isn’t stored in memory.

For more information and to see examples of computed properties, see Computed Properties.
-->

이러한 형식의 변수 선언은 전역 범위, 함수의 지역 범위, 또는 클래스, 구조체, 열거형, 또는 확장 선언의 컨텍스트에서 정의합니다. 이러한 형식의 변수 선언을 전역 범위 또는 함수의 지역 범위로 선언되면 _계산된 변수 (computed variable)_ 로 참조됩니다. 클래스, 구조체, 또는 확장 선언의 컨텍스트에서 선언되면 _계산된 프로퍼티 (computed property)_ 로 참조됩니다.

getter 는 값을 읽기위해 사용되고 setter 는 값을 작성하기 위해 사용됩니다. setter 절은 선택사항이며 getter 만 필요하다면 [읽기-전용 계산된 프로퍼티 (Read-Only Computed Properties)](../language-guide-1/properties.md#read-only-computed-properties) 에 설명한대로 getter 와 setter 절 모두 생략할 수 있고 간단하게 요구된 값을 직접적으로 반환할 수 있습니다. 그러나 setter 절을 제공하면 getter 절도 제공되어야 합니다.

_setter 이름 (setter name)_ 과 둘러싸인 소괄호는 선택사항입니다. setter 이름을 제공하면 setter 에 파라미터의 이름으로 사용됩니다. setter 이름을 제공하지 않으면 [짧은 Setter 선언 (Shorthand Setter Declaration)](../language-guide-1/properties.md#setter-shorthand-setter-declaration) 에서 설명한대로 setter 에 기본 파라미터 이름은 `newValue` 입니다.

저장된 명명된 값과 저장된 변수 프로퍼티와 다르게 계산된 명명된 값 또는 계산된 프로퍼티의 값은 메모리에 저장되지 않습니다.

계산된 프로퍼티의 자세한 내용과 예제는 [계산된 프로퍼티 (Computed Properties)](../language-guide-1/properties.md#computed-properties) 를 참고 바랍니다.

### 저장된 변수 관찰자와 프로퍼티 관찰자 (Stored Variable Observers and Property Observers)

<!--
You can also declare a stored variable or property with willSet and didSet observers. A stored variable or property declared with observers has the following form:
-->

`willSet` 과 `didSet` 관찰자를 저장된 변수 또는 프로퍼티에 선언할 수도 있습니다. 괄찰자와 함께 선언한 저장된 변수 또는 프로퍼티는 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 11.34.28.png>)

<!--
You define this form of a variable declaration at global scope, the local scope of a function, or in the context of a class or structure declaration. When a variable declaration of this form is declared at global scope or the local scope of a function, the observers are referred to as stored variable observers. When it’s declared in the context of a class or structure declaration, the observers are referred to as property observers.

You can add property observers to any stored property. You can also add property observers to any inherited property (whether stored or computed) by overriding the property within a subclass, as described in Overriding Property Observers.

The initializer expression is optional in the context of a class or structure declaration, but required elsewhere. The type annotation is optional when the type can be inferred from the initializer expression. This expression is evaluated the first time you read the property’s value. If you overwrite the property’s initial value without reading it, this expression is evaluated before the first time you write to the property.

The willSet and didSet observers provide a way to observe (and to respond appropriately) when the value of a variable or property is being set. The observers aren’t called when the variable or property is first initialized. Instead, they’re called only when the value is set outside of an initialization context.

A willSet observer is called just before the value of the variable or property is set. The new value is passed to the willSet observer as a constant, and therefore it can’t be changed in the implementation of the willSet clause. The didSet observer is called immediately after the new value is set. In contrast to the willSet observer, the old value of the variable or property is passed to the didSet observer in case you still need access to it. That said, if you assign a value to a variable or property within its own didSet observer clause, that new value that you assign will replace the one that was just set and passed to the willSet observer.

The setter name and enclosing parentheses in the willSet and didSet clauses are optional. If you provide setter names, they’re used as the parameter names to the willSet and didSet observers. If you don’t provide setter names, the default parameter name to the willSet observer is newValue and the default parameter name to the didSet observer is oldValue.

The didSet clause is optional when you provide a willSet clause. Likewise, the willSet clause is optional when you provide a didSet clause.

If the body of the didSet observer refers to the old value, the getter is called before the observer, to make the old value available. Otherwise, the new value is stored without calling the superclass’s getter. The example below shows a computed property that’s defined by the superclass and overridden by its subclasses to add an observer.
-->

전역 범위, 함수의 지역 범위, 또는 클래스나 구조체 선언의 컨텍스트에서 이 형식의 변수 선언으로 정의합니다. 이 형식의 변수 선언이 전역 범위 또는 함수의 지역 범위로 선언되면 관찰자는 _저장된 변수 관찰자 (stored variable observers)_ 로 참조됩니다. 클래스나 구조체 선언의 컨텍스트에서 선언되면 관찰자는 _프로퍼티 관찰자 (property observers)_ 로 참조됩니다.

모든 저장된 프로퍼티에 프로퍼티 관찰자를 추가할 수 있습니다. [프로퍼티 관찰자 재정의 (Overriding Property Observers)](../language-guide-1/inheritance.md#overriding-property-observers) 에서 설명한대로 하위클래스 내에 프로퍼티 재정의로 저장된 또는 계산된 프로퍼티와 상관없이 모든 상속된 프로퍼티에 프로퍼티 관찰자를 추가할 수도 있습니다.

초기화 구문 _표현식 (expression)_ 은 클래스나 구조체 선언의 컨텍스트에서 선택사항 이지만 다른곳에선 필수입니다. 타입이 초기화 구문 _표현식 (expression)_ 에서 유추될 수 있으면 _타입 (type)_ 주석은 선택사항 입니다. 이 표현식은 프로퍼티의 값을 처음 읽을 때 평가됩니다. 프로퍼티의 초기값을 읽지않고 재작성하면 이 표현식은 프로퍼티에 처음 쓰기 전에 평가됩니다.

변수 또는 프로퍼티의 값이 설정되려고 하면 `willSet` 과 `didSet` 관찰자는 관찰 및 적절한 반응을 위한 방법을 제공합니다. 관찰자는 변수 또는 프로퍼티가 처음 초기화될 때는 호출되지 않습니다. 대신에 값이 초기화 컨텍스트 바깥에서 설정될 때만 호출됩니다.

`willSet` 관찰자는 변수 또는 프로퍼티의 값이 설정되기 전에만 호출됩니다. 새로운 값은 상수로 `willSet` 관찰자로 전달되고 `willSet` 절의 구현에서 변경할 수 없습니다. `didSet` 관찰자는 새로운 값이 설정된 후 바로 호출됩니다. `willSet` 관찰자와 달리 변수 또는 프로퍼티의 이전 값은 여전히 접근이 필요한 경우에 `didSet` 관찰자로 전달됩니다. 이 말은 `didSet` 관찰자 절 내에 변수 또는 프로퍼티에 값을 할당하면 할당한 새 값이 방금 설정되어 `willSet` 관찰자에 전달된 값을 대체합니다.

`willSet` 과 `didSet` 절에 _setter 이름 (setter name)_ 과 둘러싸인 소괄호는 선택사항 입니다. setter 이름을 제공한다면 `willSet` 과 `didSet` 관찰자의 파라미터 이름으로 사용됩니다. setter 이름을 제공하지 않는다면 `willSet` 관찰자에서 기본 파라미터 이름은 `newValue` 이고 `didSet` 관찰자에서 기본 파라미터 이름은 `oldValue` 입니다.

`willSet` 절을 제공하면 `didSet` 절은 선택사항 입니다. 마찬가지로 `willSet` 절은 `didSet` 절이 제공되면 선택사항 입니다.

`didSet` 관찰자의 본문은 이전 값을 참조하면 getter 는 이전 값이 사용 가능하도록 관찰자 전에 호출됩니다. 아래 예제는 상위클래스에 의해 정의되고 관찰자를 추가하기 위해 하위클래스에 의해 재정의 된 계산된 프로퍼티를 보여줍니다.

```swift
class Superclass {
    private var xValue = 12
    var x: Int {
        get { print("Getter was called"); return xValue }
        set { print("Setter was called"); xValue = newValue }
    }
}

// This subclass doesn't refer to oldValue in its observer, so the
// superclass's getter is called only once to print the value.
class New: Superclass {
    override var x: Int {
        didSet { print("New value \(x)") }
    }
}
let new = New()
new.x = 100
// Prints "Setter was called"
// Prints "Getter was called"
// Prints "New value 100"

// This subclass refers to oldValue in its observer, so the superclass's
// getter is called once before the setter, and again to print the value.
class NewAndOld: Superclass {
    override var x: Int {
        didSet { print("Old value \(oldValue) - new value \(x)") }
    }
}
let newAndOld = NewAndOld()
newAndOld.x = 200
// Prints "Getter was called"
// Prints "Setter was called"
// Prints "Getter was called"
// Prints "Old value 12 - new value 200"
```

<!--
For more information and to see an example of how to use property observers, see Property Observers.
-->

프로퍼티 관찰자를 사용하는 방법에 대한 자세한 내용과 예제는 [프로퍼티 관찰자 (Property Observers)](../language-guide-1/properties.md#property-observers) 를 참고 바랍니다.

### 타입 변수 프로퍼티 (Type Variable Properties)

<!--
To declare a type variable property, mark the declaration with the static declaration modifier. Classes can mark type computed properties with the class declaration modifier instead to allow subclasses to override the superclass’s implementation. Type properties are discussed in Type Properties.
-->

타입 변수 프로퍼티 (type variable property) 를 선언하기 위해 `static` 선언 수식어와 함께 선언에 표시합니다. 클래스는 상위클래스의 구현을 하위클래스가 재정의 할 수 있도록 `class` 선언 수식어와 함께 타입 계산된 프로퍼티를 표시할 수 있습니다. 타입 프로퍼티는 [타입 프로퍼티 (Type Properties)](../language-guide-1/properties.md#type-properties) 에 설명되어 있습니다.

> GRAMMAR OF A VARIABLE DECLARATION\
> variable-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-declaration-head) [pattern-initializer-list](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_pattern-initializer-list)\
> variable-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-declaration-head) [variable-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-name) [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation) [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)\
> variable-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-declaration-head) [variable-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-name) [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation) [getter-setter-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_getter-setter-block)\
> variable-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-declaration-head) [variable-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-name) [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation) [getter-setter-keyword-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_getter-setter-keyword-block)\
> variable-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-declaration-head) [variable-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-name) [initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer) [willSet-didSet-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_willSet-didSet-block)\
> variable-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-declaration-head) [variable-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-name) [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation) [initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer) $$_{opt}$$ [willSet-didSet-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_willSet-didSet-block)\
> variable-declaration-head → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [declaration-modifiers](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration-modifiers) $$_{opt}$$ `var`\
> variable-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> getter-setter-block → [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)\
> getter-setter-block → `{` [getter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_getter-clause) [setter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_setter-clause) $$_{opt}$$ `}`\
> getter-setter-block → `{` [setter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_setter-clause) [getter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_getter-clause) `}`\
> getter-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [mutation-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_mutation-modifier) $$_{opt}$$ `get` [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)\
> setter-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [mutation-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_mutation-modifier) $$_{opt}$$ `set` [setter-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_setter-name) $$_{opt}$$ [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)\
> setter-name → `(` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) `)`\
> getter-setter-keyword-block → `{` [getter-keyword-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_getter-keyword-clause) [setter-keyword-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_setter-keyword-clause) $$_{opt}$$ `}`\
> getter-setter-keyword-block → `{` [setter-keyword-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_setter-keyword-clause) [getter-keyword-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_getter-keyword-clause) `}`\
> getter-keyword-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [mutation-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_mutation-modifier) $$_{opt}$$ `get`\
> setter-keyword-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [mutation-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_mutation-modifier) $$_{opt}$$ `set`\
> willSet-didSet-block → `{` [willSet-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_willSet-clause) [didSet-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_didSet-clause) $$_{opt}$$ `}`\
> willSet-didSet-block → `{` [didSet-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_didSet-clause) [willSet-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_willSet-clause) $$_{opt}$$ `}`\
> willSet-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ `willSet` [setter-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_setter-name) $$_{opt}$$ [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)\
> didSet-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ `didSet` [setter-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_setter-name) $$_{opt}$$ [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)

## 타입 별칭 선언 (Type Alias Declaration)

<!--
A type alias declaration introduces a named alias of an existing type into your program. Type alias declarations are declared using the typealias keyword and have the following form:
-->

_타입 별칭 선언 (type alias declaration)_ 은 프로그램에 기존 타입 명명된 별칭을 도입합니다. 타입 별칭 선언은 `typealias` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오전 11.38.21.png>)

<!--
After a type alias is declared, the aliased name can be used instead of the existing type everywhere in your program. The existing type can be a named type or a compound type. Type aliases don’t create new types; they simply allow a name to refer to an existing type.

A type alias declaration can use generic parameters to give a name to an existing generic type. The type alias can provide concrete types for some or all of the generic parameters of the existing type. For example:
-->

타입 별칭이 선언된 후 별칭된 _이름 (name)_ 은 프로그램에서 _기존 타입 (existing type)_ 대신에 사용될 수 있습니다. _기존 타입 (existing type)_ 은 명명된 타입 또는 복합 타입일 수 있습니다. 타입 별칭은 새로운 타입을 생성하지 않습니다; 간단하게 기존 타입을 참조하도록 이름을 허용합니다.

타입 별칭 선언은 기존 제너릭 타입에 이름을 제공하기 위해 제너릭 파라미터를 사용할 수 있습니다. 타입 별칭은 기존 타입의 일부 또는 모든 제너릭 파라미터에 대한 구체적인 타입을 제공할 수 있습니다:

```swift
typealias StringDictionary<Value> = Dictionary<String, Value>

// The following dictionaries have the same type.
var dictionary1: StringDictionary<Int> = [:]
var dictionary2: Dictionary<String, Int> = [:]
```

<!--
When a type alias is declared with generic parameters, the constraints on those parameters must match exactly the constraints on the existing type’s generic parameters. For example:
-->

타입 별칭이 제너릭 파라미터로 선언되면 파라미터의 제약조건은 기존 타입의 제너릭 파라미터의 제약조건과 완벽하게 일치해야 합니다. 예를 들어:

```swift
typealias DictionaryOfInts<Key: Hashable> = Dictionary<Key, Int>
```

<!--
Because the type alias and the existing type can be used interchangeably, the type alias can’t introduce additional generic constraints.

A type alias can forward an existing type’s generic parameters by omitting all generic parameters from the declaration. For example, the Diccionario type alias declared here has the same generic parameters and constraints as Dictionary.
-->

타입 별칭과 기존 타입은 서로 바꿔서 사용될 수 있으므로 타입 별칭은 추가로 제너릭 제약조건을 도입할 수 없습니다.

타입 별칭은 선언에서 모든 제너릭 파라미터를 생략하여 기존 타입의 제너릭 파라미터를 전달할 수 있습니다. 예를 들어 여기 선언한 `Diccionario` 타입 별칭은 `Dictionary` 와 동일한 제너릭 파라미터와 제약조건을 가집니다.

```swift
typealias Diccionario = Dictionary
```

<!--
Inside a protocol declaration, a type alias can give a shorter and more convenient name to a type that’s used frequently. For example:
-->

프로토콜 선언 내에 타입 별칭은 자주 사용되는 타입에 더 짧고 더 편리한 이름으로 제공할 수 있습니다. 예를 들어:

```swift
protocol Sequence {
    associatedtype Iterator: IteratorProtocol
    typealias Element = Iterator.Element
}

func sum<T: Sequence>(_ sequence: T) -> Int where T.Element == Int {
    // ...
}
```

<!--
Without this type alias, the sum function would have to refer to the associated type as T.Iterator.Element instead of T.Element.

See also Protocol Associated Type Declaration.
-->

타입 별칭이 없다면 `sum` 함수는 `T.Element` 대신에 `T.Iterator.Element` 로 연관된 타입을 참조해야 합니다.

[프로토콜 관련 타입 선언 (Protocol Associated Type Declaration)](declarations.md#protocol-associated-type-declaration) 을 참고 바랍니다.

> GRAMMAR OF A TYPE ALIAS DECLARATION\
> typealias-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [access-level-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_access-level-modifier) $$_{opt}$$ `typealias` [typealias-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_typealias-name) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [typealias-assignment](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_typealias-assignment)\
> typealias-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> typealias-assignment → `=` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type)

## 함수 선언 (Function Declaration)

<!--
A function declaration introduces a function or method into your program. A function declared in the context of class, structure, enumeration, or protocol is referred to as a method. Function declarations are declared using the func keyword and have the following form:
-->

_함수 선언 (function declaration)_ 은 프로그램에 함수 또는 메서드를 도입합니다. 클래스, 구조체, 열거형, 또는 프로토콜의 컨텍스트에 선언된 함수는 _메서드 (method)_ 로 참조됩니다. 함수 선언은 `func` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.13.47.png>)

<!--
If the function has a return type of Void, the return type can be omitted as follows:
-->

함수가 `Void` 의 반환 타입을 가지면 반환 타입은 다음과 같이 생략될 수 있습니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.14.18.png>)

<!--
The type of each parameter must be included—it can’t be inferred. If you write inout in front of a parameter’s type, the parameter can be modified inside the scope of the function. In-out parameters are discussed in detail in In-Out Parameters, below.

A function declaration whose statements include only a single expression is understood to return the value of that expression. This implicit return syntax is considered only when the expression’s type and the function’s return type aren’t Void and aren’t an enumeration like Never that doesn’t have any cases.

Functions can return multiple values using a tuple type as the return type of the function.

A function definition can appear inside another function declaration. This kind of function is known as a nested function.

A nested function is nonescaping if it captures a value that’s guaranteed to never escape—such as an in-out parameter—or passed as a nonescaping function argument. Otherwise, the nested function is an escaping function.

For a discussion of nested functions, see Nested Functions.
-->

각 파라미터의 타입은 유추될 수 없으므로 반드시 도입되어야 합니다. 파라미터의 타입 앞에 `inout` 을 작성하면 파라미터는 함수의 범위 내에서 수정될 수 있습니다. In-out 파라미터는 아래 [In-Out 파라미터 (In-Out Parameters)](declarations.md#in-out-parameters) 에서 자세하게 설명되어 있습니다.

_구문 (statements)_ 에 단일 표현식만 포함된 함수 선언은 해당 표현식의 값을 반환하는 것으로 이해됩니다. 이 암시적 반환 구문은 표현식의 타입과 함수의 반환 타입이 `Void` 가 아니고 어떠한 케이스가 아닌 `Never` 와 같은 열거형이 아닌 경우에만 간주됩니다.

함수는 함수의 반환 타입으로 튜플 타입을 사용하여 여러값을 반환할 수 있습니다.

함수 정의는 다른 함수 선언 안에 나타날 수 있습니다. 이러한 함수를 _중첩 함수 (nested function)_ 이라 합니다.

중첩 함수는 in-out 파라미터와 같이 절대 이스케이프되지 않는 값을 캡처하거나 비이스케이프 함수 인수로 전달된 경우 비이스케이프 입니다. 그렇지 않으면 중첩 함수는 이스케이프 함수 입니다.

중첩된 함수에 대한 내용은 [중첩 함수 (Nested Functions)](../language-guide-1/functions.md#nested-functions) 를 참고 바랍니다.

### 파라미터 이름 (Parameter Names)

<!--
Function parameters are a comma-separated list where each parameter has one of several forms. The order of arguments in a function call must match the order of parameters in the function’s declaration. The simplest entry in a parameter list has the following form:
-->

함수 파라미터는 각 파라미터가 여러 형식 중 하나를 갖는 콤마로 구분된 리스트입니다. 함수 호출에서 인수의 순서는 함수의 선언 내에 파라미터의 순서와 일치해야 합니다. 파라미터 리스트에서 가장 간단한 형식은 다음과 같습니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.15.00.png>)

<!--
A parameter has a name, which is used within the function body, as well as an argument label, which is used when calling the function or method. By default, parameter names are also used as argument labels. For example:
-->

파라미터는 함수 본문 내에서 사용되는 이름 뿐만 아니라 함수 또는 메서드 호출할 때 사용되는 인수 라벨이 있습니다. 기본적으로 파라미터 이름은 인수 라벨로도 사용됩니다. 예를 들어:

```swift
func f(x: Int, y: Int) -> Int { return x + y }
f(x: 1, y: 2) // both x and y are labeled
```

<!--
You can override the default behavior for argument labels with one of the following forms:
-->

다음의 형식 중 하나로 인수 라벨의 기본 동작을 재정의 할 수 있습니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.15.58.png>)

<!--
A name before the parameter name gives the parameter an explicit argument label, which can be different from the parameter name. The corresponding argument must use the given argument label in function or method calls.

An underscore (_) before a parameter name suppresses the argument label. The corresponding argument must have no label in function or method calls.
-->

파라미터 이름 전에 이름은 파라미터 이름과 다를 수 있는 명시적 인수 라벨을 파라미터에 제공합니다. 해당 인수는 함수 또는 메서드 호출에서 주어진 인수 라벨을 사용해야 합니다.

파라미터 이름 전에 언더바 (`_`) 는 인수 라벨을 숨깁니다. 해당 인수는 함수 또는 메서드 호출에서 라벨이 없어야 합니다.

```swift
func repeatGreeting(_ greeting: String, count n: Int) { /* Greet n times */ }
repeatGreeting("Hello, world!", count: 2) //  count is labeled, greeting is not
```

### In-Out 파라미터 (In-Out Parameters)

<!--
In-out parameters are passed as follows:

1. When the function is called, the value of the argument is copied.
2. In the body of the function, the copy is modified.
3. When the function returns, the copy’s value is assigned to the original argument.

This behavior is known as copy-in copy-out or call by value result. For example, when a computed property or a property with observers is passed as an in-out parameter, its getter is called as part of the function call and its setter is called as part of the function return.

As an optimization, when the argument is a value stored at a physical address in memory, the same memory location is used both inside and outside the function body. The optimized behavior is known as call by reference; it satisfies all of the requirements of the copy-in copy-out model while removing the overhead of copying. Write your code using the model given by copy-in copy-out, without depending on the call-by-reference optimization, so that it behaves correctly with or without the optimization.

Within a function, don’t access a value that was passed as an in-out argument, even if the original value is available in the current scope. Accessing the original is a simultaneous access of the value, which violates Swift’s memory exclusivity guarantee. For the same reason, you can’t pass the same value to multiple in-out parameters.

For more information about memory safety and memory exclusivity, see Memory Safety.

A closure or nested function that captures an in-out parameter must be nonescaping. If you need to capture an in-out parameter without mutating it, use a capture list to explicitly capture the parameter immutably.
-->

In-out 파라미터는 다음과 같이 전달됩니다:

1. 함수가 호출될 때 인수의 값은 복사됩니다.
2. 함수의 본문 내에서 복사본은 수정됩니다.
3. 함수가 반환될 때 복사본의 값은 기존 인수에 할당됩니다.

이 동작은 _copy-in copy-out_ 또는 _call by value 결과_ 라고 합니다. 예를 들어 계산된 프로퍼티 또는 관찰자가 있는 프로퍼티가 in-out 파라미터로 전달되는 경우 getter 는 함수 호출의 부분으로 호출되고 setter 는 함수 반환의 부분으로 호출됩니다.

최적화로 인수가 메모리의 물리적 주소에 저장된 값인 경우 동일한 메모리 위치가 함수 본문 내부 및 외부에서 사용됩니다. 이런 최적화 동작을 _call by reference_ 라고 합니다; copy-in copy-out 모델의 모든 요구사항을 충족하는 동시에 복사의 오버헤드를 제거합니다. call-by-reference 최적화에 의존하지 않고 copy-in copy-out 에 의해 주어진 모델을 사용하여 작성하면 최적화에 상관없이 올바르게 작동되도록 합니다.

함수 내에서 기존 값이 현재 범위에서 사용가능 하더라도 in-out 인수로 전달된 값은 접근하면 안됩니다. 기존 값에 접근하는 것은 값에 대한 동시 접근이며 Swift 의 메모리 독점 보장을 위반합니다. 같은 이유로 여러개의 in-out 파라미터에 동일한 값을 전달할 수 없습니다.

메모리 안정성과 메모리 독점성에 대한 자세한 내용은 [메모리 안정성 (Memory Safety)](../language-guide-1/memory-safety.md) 을 참고 바랍니다.

in-out 파라미터를 캡처하는 클로저 또는 중첩 함수는 비이스케이프 이어야 합니다. in-out 파라미터를 변경하지 않고 캡처 해야하는 경우 캡처 리스트을 사용하여 파라미터를 변경하지 않고 명시적으로 캡처해야 합니다.

```swift
func someFunction(a: inout Int) -> () -> Int {
    return { [a] in return a + 1 }
}
```

<!--
If you need to capture and mutate an in-out parameter, use an explicit local copy, such as in multithreaded code that ensures all mutation has finished before the function returns.
-->

in-out 파라미터를 캡처하고 변경이 필요한 경우 함수가 반환하기 전에 모든 변경이 완료되었는지 확인하는 멀티 쓰레드 코드와 같이 명시적으로 지역 복사 (local copy) 를 사용합니다.

```swift
func multithreadedFunction(queue: DispatchQueue, x: inout Int) {
    // Make a local copy and manually copy it back.
    var localX = x
    defer { x = localX }

    // Operate on localX asynchronously, then wait before returning.
    queue.async { someMutatingOperation(&localX) }
    queue.sync {}
}
```

<!--
For more discussion and examples of in-out parameters, see In-Out Parameters.
-->

in-out 파라미터에 대한 자세한 설명과 예제는 [In-Out 파라미터 (In-Out Parameters)](../language-guide-1/functions.md#in-out-in-out-parameters) 를 참고 바랍니다.

### 특별한 종류의 파라미터 (Special Kinds of Parameters)

<!--
Parameters can be ignored, take a variable number of values, and provide default values using the following forms:
-->

파라미터는 무시될 수 있고 다음의 형식을 사용하여 가변의 값을 가지고 기본값을 제공할 수 있습니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.18.38.png>)

<!--
An underscore (_) parameter is explicitly ignored and can’t be accessed within the body of the function.

A parameter with a base type name followed immediately by three dots (...) is understood as a variadic parameter. A parameter that immediately follows a variadic parameter must have an argument label. A function can have multiple variadic parameters. A variadic parameter is treated as an array that contains elements of the base type name. For example, the variadic parameter Int... is treated as [Int]. For an example that uses a variadic parameter, see Variadic Parameters.

A parameter with an equals sign (=) and an expression after its type is understood to have a default value of the given expression. The given expression is evaluated when the function is called. If the parameter is omitted when calling the function, the default value is used instead.
-->

언더바 (`_`) 파라미터는 명시적으로 무시되고 함수의 본문 내에서 접근될 수 없습니다.

기본 타입 이름 바로 뒤에 세 개의 점 (`...`) 이 오는 파라미터는 가변 파라미터 (variadic parameter) 입니다. 가변 파라미터 뒤에 오는 파라미터는 인수 라벨이 있어야 합니다. 함수는 여러개 가변 파라미터를 가질 수 있습니다. 가변 파라미터는 기본 타입 이름의 요소를 포함한 배열로 처리됩니다. 예를 들어 가변 파라미터 `Int...` 는 `[Int]` 로 처리됩니다. 가변 파라미터를 사용하는 예제는 [가변 파라미터 (Variadic Parameters)](../language-guide-1/functions.md#variadic-parameters) 를 참고 바랍니다.

등호 (`=`) 가 있는 파라미터와 타입 뒤의 표현식은 주어진 표현식의 기본값을 가지는 것으로 간주됩니다. 함수가 호출될 때 주어진 표현식은 평가됩니다. 함수를 호출할 때 파라미터가 생략되면 기본값이 대신 사용됩니다.

```swift
func f(x: Int = 42) -> Int { return x }
f()       // Valid, uses default value
f(x: 7)   // Valid, uses the value provided
f(7)      // Invalid, missing argument label
```

### 특별한 종류의 메서드 (Special Kinds of Methods)

<!--
Methods on an enumeration or a structure that modify self must be marked with the mutating declaration modifier.

Methods that override a superclass method must be marked with the override declaration modifier. It’s a compile-time error to override a method without the override modifier or to use the override modifier on a method that doesn’t override a superclass method.

Methods associated with a type rather than an instance of a type must be marked with the static declaration modifier for enumerations and structures, or with either the static or class declaration modifier for classes. A class type method marked with the class declaration modifier can be overridden by a subclass implementation; a class type method marked with class final or static can’t be overridden.
-->

`self` 를 수정하 열거형 또는 구조체에 메서드는 `mutating` 선언 수식어로 표시되어야 합니다.

상위클래스 메서드를 재정의한 메서드는 `override` 선언 수식어로 표시되어야 합니다. `override` 수식어 없이 메서드를 재정의하거나 상위클래스 메서드를 재정의 하지 않는 메서드에 `override` 수식어를 사용하면 컴파일 에러가 발생합니다.

타입의 인스턴스가 아닌 타입과 관련된 메서드는 열거형과 구조체의 경우 `static` 선언 수식어나 클래스의 경우 `static` 또는 `class` 선언 수식어로 표시되어야 합니다. `class` 선언 수식어로 표시된 클래스 타입 메서드는 하위클래스 구현에 의해 재정의 될 수 있습니다; `class final` 또는 `static` 으로 표시된 클래스 타입 메서드는 재정의 될 수 없습니다.

### 특별한 이름의 메서드 (Methods with Special Names)

<!--
Several methods that have special names enable syntactic sugar for function call syntax. If a type defines one of these methods, instances of the type can be used in function call syntax. The function call is understood to be a call to one of the specially named methods on that instance.

A class, structure, or enumeration type can support function call syntax by defining a dynamicallyCall(withArguments:) method or a dynamicallyCall(withKeywordArguments:) method, as described in dynamicCallable, or by defining a call-as-function method, as described below. If the type defines both a call-as-function method and one of the methods used by the dynamicCallable attribute, the compiler gives preference to the call-as-function method in circumstances where either method could be used.

The name of a call-as-function method is callAsFunction(), or another name that begins with callAsFunction( and adds labeled or unlabeled arguments—for example, callAsFunction(_:_:) and callAsFunction(something:) are also valid call-as-function method names.

The following function calls are equivalent:
-->

특별한 이름을 가지는 메서드는 함수 호출 구문에 대해 구문 설탕 (syntactic sugar) 을 활성화 합니다. 이러한 메서드 중 하나로 타입을 정의하면 타입의 인스턴스는 함수 호출 구문에서 사용될 수 있습니다. 함수 호출은 해당 인스턴스에서 특별하게 명명된 메서드 중 하나로 호출됩니다.

클래스, 구조체, 또는 열거형 타입은 [dynamicCallable](attributes.md#dynamiccallable) 에서 설명 한대로 `dynamicallyCall(withArguments:)` 메서드 또는 `dynamicallyCall(withKeywordArguments:)` 메서드를 정의하거나 아래에서 설명 한대로 call-as-function 메서드를 정의하여 함수 호출 구문을 제공할 수 있습니다. 타입이 call-as-function 메서드와 `dynamicCallable` 속성에 의해 사용되는 메서드 중 하나를 모두 정의하면 컴파일러는 두 메서드를 사용할 수 있는 환경에서 call-as-function 메서드를 더 선호합니다.

call-as-function 메서드의 이름은 `callAsFunction()` 또는 `callAsFunction(` 으로 시작하고 라벨이 있거나 없는 인수를 추가하는 이름이 있습니다—예를 들어 `callAsFunction(_:_:)` 와 `callAsFunction(something:)` 도 call-as-function 메서드 이름으로 유효합니다.

다음의 함수 호출은 동일합니다:

```swift
struct CallableStruct {
    var value: Int
    func callAsFunction(_ number: Int, scale: Int) {
        print(scale * (number + value))
    }
}
let callable = CallableStruct(value: 100)
callable(4, scale: 2)
callable.callAsFunction(4, scale: 2)
// Both function calls print 208.
```

<!--
The call-as-function methods and the methods from the dynamicCallable attribute make different trade-offs between how much information you encode into the type system and how much dynamic behavior is possible at runtime. When you declare a call-as-function method, you specify the number of arguments, and each argument’s type and label. The dynamicCallable attribute’s methods specify only the type used to hold the array of arguments.

Defining a call-as-function method, or a method from the dynamicCallable attribute, doesn’t let you use an instance of that type as if it were a function in any context other than a function call expression. For example:
-->

call-as-function 메서드와 `dynamicCallable` 속성의 메서드는 타입 시스템으로 인코딩하는 정보의 양과 런타임에 가능한 동적 동작의 양 사이에 서로 다른 절충안을 만듭니다. call-as-function 메서드를 선언할 때 인수의 숫자와 각 인수의 타입과 라벨을 지정합니다. `dynamicCallable` 속성의 메서드는 인수의 배열을 보유하기 위해 사용된 타입만 지정합니다.

call-as-function 메서드 또는 `dynamicCallable` 속성의 메서드를 정의해도 함수 호출 표현식이 아닌 다른 컨텍스트에서 함수처럼 해당 타입의 인스턴스로 사용할 수 없습니다. 예를 들어:

```swift
let someFunction1: (Int, Int) -> Void = callable(_:scale:)  // Error
let someFunction2: (Int, Int) -> Void = callable.callAsFunction(_:scale:)
```

<!--
The subscript(dynamicMember:) subscript enables syntactic sugar for member lookup, as described in dynamicMemberLookup.
-->

`subscript(dynamicMemberLookup:)` 서브 스크립트는 [dynamicMemberLookup](attributes.md#dynamicmemberlookup) 에서 설명 한대로 멤버 조회를 위한 구문 설탕을 활성화 합니다.

### 함수와 메서드 던지기 (Throwing Functions and Methods)

<!--
Functions and methods that can throw an error must be marked with the throws keyword. These functions and methods are known as throwing functions and throwing methods. They have the following form:
-->

에러를 던질 수 있는 함수와 메서드는 `throws` 키워드로 표시되어야 합니다. 이러한 함수와 메서드는 _던지는 함수 (throwing functions)_ 와 _던지는 메서드 (throwing methods)_ 라고 합니다. 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.20.55.png>)

<!--
Calls to a throwing function or method must be wrapped in a try or try! expression (that is, in the scope of a try or try! operator).

The throws keyword is part of a function’s type, and nonthrowing functions are subtypes of throwing functions. As a result, you can use a nonthrowing function in a context where as a throwing one is expected.

You can’t overload a function based only on whether the function can throw an error. That said, you can overload a function based on whether a function parameter can throw an error.

A throwing method can’t override a nonthrowing method, and a throwing method can’t satisfy a protocol requirement for a nonthrowing method. That said, a nonthrowing method can override a throwing method, and a nonthrowing method can satisfy a protocol requirement for a throwing method.
-->

던지는 함수 또는 메서드 호출은 `try` 또는 `try!` 표현식 (`try` 또는 `try!` 연산자의 범위 내) 으로 래핑되어야 합니다.

`throws` 키워드는 함수의 타입의 일부분이고 던지지 않는 함수는 던지는 함수의 하위 타입입니다. 결과적으로 던지는 함수와 같은 위치에서 던지지 않는 함수를 사용할 수 있습니다.

함수가 에러를 발생할 수 있는지 여부를 기준으로 함수를 오버로드 할 수 없습니다. 이 말은 함수 파라미터 (parameter) 가 에러를 발생할 수 있는 여부에 따라 함수를 오버로드 할 수 있습니다.

던지는 메서드는 던지지 않는 메서드를 재정의할 수 없고 던지는 메서드는 던지지 않는 메서드에 대해 프로토콜 요구사항을 충족할 수 없습니다. 이 말은 던지지 않는 메서드는 던지는 메서드를 재정의할 수 있고 던지지 않는 함수는 던지는 메서드에 대한 프로토콜 요구사항을 충족할 수 있습니다.

### 다시 던지는 함수와 메서드 (Rethrowing Functions and Methods)

<!--
A function or method can be declared with the rethrows keyword to indicate that it throws an error only if one of its function parameters throws an error. These functions and methods are known as rethrowing functions and rethrowing methods. Rethrowing functions and methods must have at least one throwing function parameter.
-->

함수 또는 메서드는 함수 파라미터 중 하나만 에러를 발생하는 것을 나타내기 위해 `rethrows` 키워드로 선언될 수 있습니다. 이러한 함수와 메서드는 _다시 던지는 함수 (rethrowing functions)_ 와 _다시 던지는 메서드 (rethrowing methods)_ 라고 합니다. 다시 던지는 함수와 메서드는 적어도 하나의 던지는 함수 파라미터를 가져야 합니다.

```swift
func someFunction(callback: () throws -> Void) rethrows {
    try callback()
}
```

<!--
A rethrowing function or method can contain a throw statement only inside a catch clause. This lets you call the throwing function inside a do-catch statement and handle errors in the catch clause by throwing a different error. In addition, the catch clause must handle only errors thrown by one of the rethrowing function’s throwing parameters. For example, the following is invalid because the catch clause would handle the error thrown by alwaysThrows().
-->

다시 던지는 함수 또는 메서드는 `catch` 절 안에만 `throw` 구문을 포함할 수 있습니다. 이렇게 하면 `do`-`catch` 구문 내에서 던지는 함수를 호출하고 다른 에러를 발생시켜 `catch` 절의 에러를 처리할 수 있습니다. 또한 `catch` 절은 다시 던지는 함수의 던지는 파라미터 중 하나에서 발생한 에러만 처리해야 합니다. 예를 들어 다음은 `catch` 절이 `alwaysThrows()` 에서 던져진 에러를 처리하므로 유효하지 않습니다.

```swift
func alwaysThrows() throws {
    throw SomeError.error
}
func someFunction(callback: () throws -> Void) rethrows {
    do {
        try callback()
        try alwaysThrows()  // Invalid, alwaysThrows() isn't a throwing parameter
    } catch {
        throw AnotherError.error
    }
}
```

<!--
A throwing method can’t override a rethrowing method, and a throwing method can’t satisfy a protocol requirement for a rethrowing method. That said, a rethrowing method can override a throwing method, and a rethrowing method can satisfy a protocol requirement for a throwing method.
-->

던지는 메서드는 다시 던지는 메서드를 재정의할 수 없고 던지는 메서드는 다시 던지는 메서드에 대한 프로토콜 요구사항을 충족할 수 없습니다. 이 말은 다시 던지는 메서드는 던지는 메서드를 재정의할 수 있고 다시 던지는 메서드는 던지는 메서드에 대해 프로토콜 요구사항을 충족할 수 있습니다.

### 비동기 함수와 메서드 (Asynchronous Functions and Methods)

<!--
Functions and methods that run asynchronously must be marked with the async keyword. These functions and methods are known as asynchronous functions and asynchronous methods. They have the following form:
-->

비동기적으로 실행되는 함수와 메서드는 `async` 키워드로 표시되어야 합니다. 이 함수와 메서드는 _비동기 함수 (asynchronous functions)_ 와 _비동기 메서드 (asynchronous methods)_ 라고 합니다. 이 형태는 다음과 같습니다:

![Asynchronous Functions and Methods](../.gitbook/assets/asynchronous\_functions\_and\_methods.png)

<!--
Calls to an asynchronous function or method must be wrapped in an await expression—that is, they must be in the scope of an await operator.

The async keyword is part of the function’s type, and synchronous functions are subtypes of asynchronous functions. As a result, you can use a synchronous function in a context where an asynchronous function is expected. For example, you can override an asynchronous method with a synchronous method, and a synchronous method can satisfy a protocol requirement that requires an asynchronous method.
-->

비동기 함수 또는 메서드에 대한 호출은 `await` 표현식으로 래핑되어야 합니다—즉, `await` 연산자 범위 안에 있어야 합니다.

`async` 키워드는 함수 타입의 부분이고 동기 함수는 비동기 함수의 하위 타입입니다. 결과적으로 비동기 함수가 필요한 컨텍스트에서 동기 함수를 사용할 수 있습니다. 예를 들어 비동기 메서드를 동기 메서드로 재정의 할 수 있으며 동기 메서드는 비동기 메서드가 필요한 프로토콜 요구사항을 충족할 수 있습니다.

<!--
You can overload a function based on whether or not the function is asynchronous. At the call site, context determines which overload is used: In an asynchronous context, the asynchronous function is used, and in a synchronous context, the synchronous function is used.

An asynchronous method can’t override a synchronous method, and an asynchronous method can’t satisfy a protocol requirement for a synchronous method. That said, a synchronous method can override an asynchronous method, and a synchronous method can satisfy a protocol requirement for an asynchronous method.
-->

함수가 비동기인지 아닌지에 따라 함수를 오버로드 할 수 있습니다. 호출 부분에서 컨텍스트는 사용될 오버로드를 결정합니다: 비동기 컨텍스트에서는 비동기 함수가 사용되고 동기 컨텍스트에서는 동기 함수가 사용됩니다.

비동기 메서드는 동기 메서드를 재정의할 수 없으며 비동기 메서드는 동기 메서드에 대한 프로토콜 요구사항을 충족시킬 수 없습니다. 이 말은, 동기 메서드는 비동기 메서드를 재정의할 수 있으며 동기 메서드는 비동기 메서드에 대한 프로토콜 요구사항을 충족할 수 있습니다.

### 반환되지 않는 함수 (Functions that Never Return)

<!--
Swift defines a Never type, which indicates that a function or method doesn’t return to its caller. Functions and methods with the Never return type are called nonreturning. Nonreturning functions and methods either cause an irrecoverable error or begin a sequence of work that continues indefinitely. This means that code that would otherwise run immediately after the call is never executed. Throwing and rethrowing functions can transfer program control to an appropriate catch block, even when they’re nonreturning.

A nonreturning function or method can be called to conclude the else clause of a guard statement, as discussed in Guard Statement.

You can override a nonreturning method, but the new method must preserve its return type and nonreturning behavior.
-->

Swift 는 함수 또는 메서드가 호출자에게 반환하지 않음을 나타내는 `Never` 타입을 정의합니다. `Never` 반환 타입이 있는 함수와 메서드는 _비반환 (nonreturning)_ 이라고 합니다. 비반환 함수와 메서드는 복구할 수 없는 에러를 발생하거나 무한으로 계속되는 작업을 시작합니다. 이것은 호출 직후 코드가 실행되지 않음을 뜻합니다. 던지고 다시 던지는 함수는 비반환인 경우에도 적절한 `catch` 블럭으로 프로그램 제어를 전송할 수 있습니다.

비반환 함수 또는 메서드는 [Guard 구문 (Guard Statement)](statements.md#guard-guard-statement) 에서 설명 한대로 guard 구문의 `else` 절을 끝낼 수 있습니다.

비반환 메서드를 재정의할 수 있지만 새로운 메서드는 반환 타입과 비반환 동작을 유지해야 합니다.

> GRAMMAR OF A FUNCTION DECLARATION\
> function-declaration → [function-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_function-head) [function-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_function-name) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [function-signature](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_function-signature)[generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [function-body](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_function-body) $$_{opt}$$\
> function-head → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [declaration-modifiers](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration-modifiers) $$_{opt}$$ `func`\
> function-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) | [operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_operator)\
> function-signature → [parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_parameter-clause) `throws` $$_{opt}$$ [function-result](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_function-result) $$_{opt}$$\
> function-signature → [parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_parameter-clause) `rethrows` [function-result](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_function-result) $$_{opt}$$\
> function-result → `->` [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type)\
> function-body → [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)\
> parameter-clause → `(` `)` | `(` [parameter-list](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_parameter-list) `)`\
> parameter-list → [parameter](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_parameter) | [parameter](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_parameter) `,` [parameter-list](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_parameter-list)\
> parameter → [external-parameter-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_external-parameter-name) $$_{opt}$$ [local-parameter-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_local-parameter-name) [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation) [default-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_default-argument-clause) $$_{opt}$$\
> parameter → [external-parameter-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_external-parameter-name) $$_{opt}$$ [local-parameter-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_local-parameter-name) [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation)\
> parameter → [external-parameter-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_external-parameter-name) $$_{opt}$$ [local-parameter-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_local-parameter-name) [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation) `...`\
> external-parameter-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> local-parameter-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> default-argument-clause → `=` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar\_expression)

## 열거형 선언 (Enumeration Declaration)

<!--
An enumeration declaration introduces a named enumeration type into your program.

Enumeration declarations have two basic forms and are declared using the enum keyword. The body of an enumeration declared using either form contains zero or more values—called enumeration cases—and any number of declarations, including computed properties, instance methods, type methods, initializers, type aliases, and even other enumeration, structure, class, and actor declarations. Enumeration declarations can’t contain deinitializer or protocol declarations.

Enumeration types can adopt any number of protocols, but can’t inherit from classes, structures, or other enumerations.

Unlike classes and structures, enumeration types don’t have an implicitly provided default initializer; all initializers must be declared explicitly. Initializers can delegate to other initializers in the enumeration, but the initialization process is complete only after an initializer assigns one of the enumeration cases to self.

Like structures but unlike classes, enumerations are value types; instances of an enumeration are copied when assigned to variables or constants, or when passed as arguments to a function call. For information about value types, see Structures and Enumerations Are Value Types.

You can extend the behavior of an enumeration type with an extension declaration, as discussed in Extension Declaration.
-->

_열거형 선언 (enumeration declaration)_ 은 프로그램에 명명된 열거형 타입을 도입합니다.

열거형 선언은 두 가지 기본 형식을 가지고 있고 `enum` 키워드를 사용하여 선언됩니다. 두 형식 중 하나를 사용하여 선언된 열거형의 본문은 열거형 케이스 (enumeration cases) 라고 불리는 없거나 더 많은 값과 계산된 프로퍼티, 인스턴스 메서드, 타입 메서드, 초기화 구문, 타입 별칭, 그리고 다른 열거형, 구조체, 클래스 그리고 행위자 선언을 포함합니다. 열거형 선언은 초기화 해제 구문 또는 프로토콜 선언을 포함할 수 없습니다.

열거형 타입은 여러 프로토콜을 채택할 수 있지만 클래스, 구조체, 또는 다른 열거형을 상속할 수 없습니다.

클래스와 구조체와 다르게 열거형 타입은 암시적으로 제공된 기본 초기화 구문을 가지지 않습니다; 모든 초기화 구문은 명시적으로 선언되어야 합니다. 초기화 구문은 열거형 내에 다른 초기화 구문으로 위임할 수 있지만 초기화 프로세스는 초기화 구문이 열거형 케이스 중 하나를 `self` 에 할당 한 후에만 완료됩니다.

구조체와 비슷하지만 클래스와 달리 열거형은 값 타입입니다; 열거형의 인스턴스는 변수 또는 상수에 할당될 때나 함수 호출에 인수로 전달될 때 복사됩니다. 값 타입에 대한 자세한 내용은 [구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)](../language-guide-1/structures-and-classes.md#structures-and-enumerations-are-value-types) 를 참고 바랍니다.

[확장 선언 (Extension Declaration)](declarations.md#extension-declaration) 에서 설명 한대로 확장 선언으로 열거형 타입의 동작을 확장할 수 있습니다.

### 모든 타입의 케이스가 있는 열거형 (Enumerations with Cases of Any Type)

<!--
The following form declares an enumeration type that contains enumeration cases of any type:
-->

다음의 형식은 모든 타입의 열거형 케이스를 포함하는 열거형 타입을 선언합니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.32.54.png>)

<!--
Enumerations declared in this form are sometimes called discriminated unions in other programming languages.

In this form, each case block consists of the case keyword followed by one or more enumeration cases, separated by commas. The name of each case must be unique. Each case can also specify that it stores values of a given type. These types are specified in the associated value types tuple, immediately following the name of the case.

Enumeration cases that store associated values can be used as functions that create instances of the enumeration with the specified associated values. And just like functions, you can get a reference to an enumeration case and apply it later in your code.
-->

이 형식으로 선언된 열거형은 다른 프로그래밍 언어에서 _구별된 공용체 (discriminated unions)_ 라고도 합니다.

이 형식에서 각 케이스 블럭은 `case` 키워드 다음에 콤마로 구분된 하나 이상의 열거형 케이스로 구성됩니다. 각 케이스의 이름은 고유해야 합니다. 각 케이스는 주어진 타입의 값을 저장하도록 지정할 수도 있습니다. 이러한 타입은 케이스의 이름 바로 다음에 _연관된 값 타입 (associated value types)_ 튜플로 지정됩니다.

연관된 값을 저장하는 열거형 케이스는 지정된 연관된 값을 사용하여 열거형의 인스턴스를 생성하는 함수로 사용될 수 있습니다. 함수와 마찬가지로 코드에 열거형 케이스에 대한 참조를 가져오고 적용할 수 있습니다.

```swift
enum Number {
    case integer(Int)
    case real(Double)
}
let f = Number.integer
// f is a function of type (Int) -> Number

// Apply f to create an array of Number instances with integer values
let evenInts: [Number] = [0, 2, 4, 6].map(f)
```

<!--
For more information and to see examples of cases with associated value types, see Associated Values.
-->

연관된 값 타입을 사용하는 케이스의 자세한 내용과 예제는 [연관된 값 (Associated Values)](../language-guide-1/enumerations.md#associated-values) 를 참고 바랍니다.

#### 간접 열거형 (Enumerations with Indirection)

<!--
Enumerations can have a recursive structure, that is, they can have cases with associated values that are instances of the enumeration type itself. However, instances of enumeration types have value semantics, which means they have a fixed layout in memory. To support recursion, the compiler must insert a layer of indirection.

To enable indirection for a particular enumeration case, mark it with the indirect declaration modifier. An indirect case must have an associated value.
-->

열거형은 재귀적 구조체를 가질 수 있습니다. 이 말은 열거형 타입 자체의 인스턴스 인 연관된 값을 사용하는 케이스를 가질 수 있다는 의미입니다. 그러나 열거형 타입의 인스턴스는 메모리에 고정된 레이아웃이 있음을 의미하는 값 의미론을 가지고 있습니다. 재귀를 지원하려면 컴파일러는 간접 레이러를 삽입해야 합니다.

특정 열거형 케이스에 대해 간접을 사용하려면 `indirect` 선언 수식어로 표시해야 합니다. 간접 케이스는 연관된 값을 가지고 있어야 합니다.

```swift
enum Tree<T> {
    case empty
    indirect case node(value: T, left: Tree, right: Tree)
}
```

<!--
To enable indirection for all the cases of an enumeration that have an associated value, mark the entire enumeration with the indirect modifier—this is convenient when the enumeration contains many cases that would each need to be marked with the indirect modifier.

An enumeration that’s marked with the indirect modifier can contain a mixture of cases that have associated values and cases those that don’t. That said, it can’t contain any cases that are also marked with the indirect modifier.
-->

연관된 값을 가진 열거형의 모든 케이스에 대해 간접을 사용하려면 전체 열거형을 `indirect` 수식어로 표시합니다—이것은 열거형이 케이스 `indirect` 수식어를 표시해야 될 케이스를 많이 포함하는 경우 편리합니다.

`indirect` 수식어로 표시된 열거형은 연관된 값을 가진 케이스와 그렇지 않은 케이스를 포함할 수 있습니다. 이 말은 `indirect` 수식어로 표시된 케이스는 포함할 수 없다는 의미입니다.

### 원시값 타입의 케이스를 가진 열거형 (Enumerations with Cases of a Raw-Value Type)

<!--
The following form declares an enumeration type that contains enumeration cases of the same basic type:
-->

다음의 형식은 동일한 기본 타입의 열거형 케이스를 포함하는 열거형 타입을 선언합니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.34.55.png>)

<!--
In this form, each case block consists of the case keyword, followed by one or more enumeration cases, separated by commas. Unlike the cases in the first form, each case has an underlying value, called a raw value, of the same basic type. The type of these values is specified in the raw-value type and must represent an integer, floating-point number, string, or single character. In particular, the raw-value type must conform to the Equatable protocol and one of the following protocols: ExpressibleByIntegerLiteral for integer literals, ExpressibleByFloatLiteral for floating-point literals, ExpressibleByStringLiteral for string literals that contain any number of characters, and ExpressibleByUnicodeScalarLiteral or ExpressibleByExtendedGraphemeClusterLiteral for string literals that contain only a single character. Each case must have a unique name and be assigned a unique raw value.

If the raw-value type is specified as Int and you don’t assign a value to the cases explicitly, they’re implicitly assigned the values 0, 1, 2, and so on. Each unassigned case of type Int is implicitly assigned a raw value that’s automatically incremented from the raw value of the previous case.
-->

이 형식에서 각 케이스 블럭은 `case` 키워드 다음에 콤마로 구분된 하나 이상의 열거형 케이스로 구성됩니다. 첫번째 형식의 케이스와 달리 각 케이스는 동일한 기본 타입의 _원시값 (raw value)_ 라는 기본값을 가집니다. 이러한 값의 타입은 _원시-값 타입 (raw-value type)_ 으로 지정되고 정수, 부동 소수점 숫자, 문자열 또는 단일 문자로 표현되어야 합니다. 특히 _원시-값 타입 (raw-value type)_ 은 `Equatable` 프로토콜과 다음 프로토콜 중 하나를 준수해야 합니다:\
정수 리터럴에 대한 `ExpressibleByIntegerLiteral`,\
부동 소수점 리터럴에 대한 `ExpressibleByFloatLiteral`,\
문자의 모든 숫자를 포함하는 문자열 리터럴에 대한 `ExpressibleByStringLiteral`, 그리고 단일 문자만 포함하는 문자열 리터럴에 대한 `ExpressibleByUnicodeScalarLiteral` 또는 `ExpressibleByExtendedGraphemeClusterLiteral`. 각 케이스는 고유한 이름을 가지고 고유한 원시값이 할당 되어야 합니다.

원시-값 타입이 `Int` 로 지정되고 케이스에 명시적으로 값을 할당하지 않으면 암시적으로 값 `0`, `1`, `2`, 등으로 할당됩니다. 타입 `Int` 의 각 할당되지 않은 케이스는 이전 케이스의 원시값에서 자동으로 증가된 원시값으로 할당됩니다.

```swift
enum ExampleEnum: Int {
    case a, b, c = 5, d
}
```

<!--
In the above example, the raw value of ExampleEnum.a is 0 and the value of ExampleEnum.b is 1. And because the value of ExampleEnum.c is explicitly set to 5, the value of ExampleEnum.d is automatically incremented from 5 and is therefore 6.

If the raw-value type is specified as String and you don’t assign values to the cases explicitly, each unassigned case is implicitly assigned a string with the same text as the name of that case.
-->

위의 예제에서 `ExampleEnum.a` 의 원시값은 `0` 이고 `ExampleEnum.b` 의 값은 `1` 입니다. 그리고 `ExampleEnum.c` 의 값이 명시적으로 `5` 로 설정되어 있으므로 `ExampleEnum.d` 의 값은 `5` 에서 자동적으로 증가되어 `6` 입니다.

원시-값 타입이 `String` 으로 지정되고 명시적으로 케이스에 값을 할당하지 않으면 각 할당되지 않은 케이스는 암시적으로 케이스의 이름과 동일한 텍스트 문자열로 할당됩니다.

```swift
enum GamePlayMode: String {
    case cooperative, individual, competitive
}
```

<!--
In the above example, the raw value of GamePlayMode.cooperative is "cooperative", the raw value of GamePlayMode.individual is "individual", and the raw value of GamePlayMode.competitive is "competitive".

Enumerations that have cases of a raw-value type implicitly conform to the RawRepresentable protocol, defined in the Swift standard library. As a result, they have a rawValue property and a failable initializer with the signature init?(rawValue: RawValue). You can use the rawValue property to access the raw value of an enumeration case, as in ExampleEnum.b.rawValue. You can also use a raw value to find a corresponding case, if there is one, by calling the enumeration’s failable initializer, as in ExampleEnum(rawValue: 5), which returns an optional case. For more information and to see examples of cases with raw-value types, see Raw Values.
-->

위의 예제에서 `GamePlayMode.cooperative` 의 원시값은 `"cooperative"`, `GamePlayMode.individual` 의 원시값은 `"individual"`, 그리고 `GamePlayMode.competitive` 는 `"competitive"` 입니다.

암시적으로 원시-값 타입의 케이스를 가지는 열거형은 Swift 표준 라이브러리에 정의된 `RawRepresentable` 프로토콜을 준수합니다. 결과적으로 `rawValue` 프로퍼티를 가지고 있고 `init?(rawValue: RawValue)` 인 실패 가능한 초기화 구문을 가집니다. `ExampleEnum.b.rawValue` 처럼 열거형 케이스의 원시값에 접근하기 위해 `rawValue` 프로퍼티를 사용할 수 있습니다. 옵셔널 케이스를 반환하는 `ExampleEnum(rawValue: 5)` 와 같이 열거형의 실패 가능한 초기화 구문을 호출하여 해당 케이스를 찾기위해 원시값을 사용할 수도 있습니다. 원시-값 타입이 있는 케이스의 자세한 내용과 예제는 [원시값 (Raw Values)](../language-guide-1/enumerations.md#raw-values) 을 참고 바랍니다.

### 열거형 케이스 접근 (Accessing Enumeration Cases)

<!--
To reference the case of an enumeration type, use dot (.) syntax, as in EnumerationType.enumerationCase. When the enumeration type can be inferred from context, you can omit it (the dot is still required), as described in Enumeration Syntax and Implicit Member Expression.

To check the values of enumeration cases, use a switch statement, as shown in Matching Enumeration Values with a Switch Statement. The enumeration type is pattern-matched against the enumeration case patterns in the case blocks of the switch statement, as described in Enumeration Case Pattern.
-->

열거형 타입의 케이스를 참조하기 위해 `EnuerationType.enumerationCase` 와 같이 점 (`.`) 구문을 사용합니다. 열거형 타입이 컨텍스트에서 유추될 수 있으면 [열거형 구문 (Enumeration Syntax)](../language-guide-1/enumerations.md#enumeration-syntax) 와 [암시적 멤버 표현식 (Implicit Member Expression)](expressions.md#implicit-member-expression) 에서 설명 한대로 점은 그대로 유지한 채 생략할 수 있습니다.

열거형 케이스의 값을 검사하려면 [Switch 구문에서 열거형 값 일치 (Matching Enumeration Values with a Switch Statement)](../language-guide-1/enumerations.md#switch-matching-enumeration-values-with-switch-statement) 에서 봤듯이 `switch` 구문을 사용합니다. 열거형 타입은 [열거형 케이스 패턴 (Enumeration Case Pattern)](patterns.md#enumeration-case-pattern) 에서 설명 한대로 `switch` 구문의 케이스 블럭에서 열거형 케이스 패턴에 대해 일치합니다.

> GRAMMAR OF AN ENUMERATION DECLARATION\
> enum-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [access-level-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_access-level-modifier) $$_{opt}$$ [union-style-enum](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_union-style-enum)\
> enum-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [access-level-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_access-level-modifier) $$_{opt}$$ [raw-value-style-enum](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_raw-value-style-enum)\
> union-style-enum → `indirect` $$_{opt}$$ `enum` [enum-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_enum-name) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [type-inheritance-clause](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-inheritance-clause) $$_{opt}$$ [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ `{` [union-style-enum-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_union-style-enum-members) $$_{opt}$$ `}`\
> union-style-enum-members → [union-style-enum-member](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_union-style-enum-member) [union-style-enum-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_union-style-enum-members) $$_{opt}$$\
> union-style-enum-member → [declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration) | [union-style-enum-case-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_union-style-enum-case-clause) | [compiler-control-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compiler-control-statement)\
> union-style-enum-case-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ `indirect` $$_{opt}$$ `case` [union-style-enum-case-list](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_union-style-enum-case-list)\
> union-style-enum-case-list → [union-style-enum-case](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_union-style-enum-case) | [union-style-enum-case](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_union-style-enum-case) `,` [union-style-enum-case-list](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_union-style-enum-case-list)\
> union-style-enum-case → [enum-case-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_enum-case-name) [tuple-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_tuple-type) $$_{opt}$$\
> enum-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> enum-case-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> raw-value-style-enum → `enum` [enum-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_enum-name) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [type-inheritance-clause](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-inheritance-clause)[generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ `{` [raw-value-style-enum-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_raw-value-style-enum-members) `}`\
> raw-value-style-enum-members → [raw-value-style-enum-member](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_raw-value-style-enum-member) [raw-value-style-enum-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_raw-value-style-enum-members) $$_{opt}$$\
> raw-value-style-enum-member → [declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration) | [raw-value-style-enum-case-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_raw-value-style-enum-case-clause) | [compiler-control-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compiler-control-statement)\
> raw-value-style-enum-case-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ `case` [raw-value-style-enum-case-list](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_raw-value-style-enum-case-list)\
> raw-value-style-enum-case-list → [raw-value-style-enum-case](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_raw-value-style-enum-case) | [raw-value-style-enum-case](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_raw-value-style-enum-case) `,` [raw-value-style-enum-case-list](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_raw-value-style-enum-case-list)\
> raw-value-style-enum-case → [enum-case-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_enum-case-name) [raw-value-assignment](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_raw-value-assignment) $$_{opt}$$\
> raw-value-assignment → `=` [raw-value-literal](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_raw-value-literal)\
> raw-value-literal → [numeric-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_numeric-literal) | [static-string-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_static-string-literal) | [boolean-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_boolean-literal)

## 구조체 선언 (Structure Declaration)

<!--
A structure declaration introduces a named structure type into your program. Structure declarations are declared using the struct keyword and have the following form:
-->

_구조체 선언 (structure declaration)_ 은 프로그램에 명명된 구조체 타입을 도입합니다. 구조체 선언은 `struct` 키워드를 사용하여 선언되고 아래의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.40.19.png>)

<!--
The body of a structure contains zero or more declarations. These declarations can include both stored and computed properties, type properties, instance methods, type methods, initializers, subscripts, type aliases, and even other structure, class, actor, and enumeration declarations. Structure declarations can’t contain deinitializer or protocol declarations. For a discussion and several examples of structures that include various kinds of declarations, see Structures and Classes.

Structure types can adopt any number of protocols, but can’t inherit from classes, enumerations, or other structures.

There are three ways to create an instance of a previously declared structure:

* Call one of the initializers declared within the structure, as described in Initializers.
* If no initializers are declared, call the structure’s memberwise initializer, as described in Memberwise Initializers for Structure Types.
* If no initializers are declared, and all properties of the structure declaration were given initial values, call the structure’s default initializer, as described in Default Initializers.

The process of initializing a structure’s declared properties is described in Initialization.

Properties of a structure instance can be accessed using dot (.) syntax, as described in Accessing Properties.

Structures are value types; instances of a structure are copied when assigned to variables or constants, or when passed as arguments to a function call. For information about value types, see Structures and Enumerations Are Value Types.

You can extend the behavior of a structure type with an extension declaration, as discussed in Extension Declaration.
-->

구조체의 본문은 _선언 (declarations)_ 이 없거나 많이 포함합니다. 이러한 _선언 (declarations)_ 은 저장된 프로퍼티와 계산된 프로퍼티, 타입 프로퍼티, 인스턴스 메서드, 타입 메서드, 초기화 구문, 서브 스크립트, 타입 별칭, 그리고 다른 구조체, 클래스, 행위자 그리고 열거형 선언을 포함할 수 있습니다. 구조체 선언은 초기화 해제 구문 또는 프로토콜 선언을 포함할 수 없습니다. 여러 종류의 선언을 포함한 구조체에 대한 자세한 내용과 예제는 [구조체와 클래스 (Structures and Classes)](../language-guide-1/structures-and-classes.md) 를 참고 바랍니다.

구조체 타입은 여러 프로토콜을 채택할 수 있지만 클래스, 열거형, 또는 다른 구조체를 상속할 수 없습니다.

이전에 선언된 구조체의 인스턴스를 생성하는 방법은 세가지가 있습니다:

* [초기화 구문 (Initializers)](../language-guide-1/initialization.md#initializers) 에서 설명 한대로 구조체 내에 선언된 초기화 구문 중 하나를 호출해야 합니다.
* 선언된 초기화 구문이 없는 경우 [구조체 타입에 대한 멤버별 초기화 구문 (Memberwise Initializers for Structure Types)](../language-guide-1/initialization.md#memberwise-initializers-for-structure-types) 에 설명 한대로 구조체의 멤버별 초기화 구문을 호출합니다.
* 선언된 초기화 구문이 없고 구조체 선언의 모든 프로퍼티에 초기값이 주어진 경우 [기본 초기화 구문 (Default Initializers)](../language-guide-1/initialization.md#default-initializers) 에서 설명 한대로 구조체의 기본 초기화 구문을 호출합니다.

구조체에 선언된 프로퍼티 초기화 프로세스는 [초기화 (Initialization)](../language-guide-1/initialization.md) 에 설명되어 있습니다.

구조체 인스턴스의 프로퍼티는 [프로퍼티 접근 (Accessing Properties)](../language-guide-1/structures-and-classes.md#accessing-properties) 에서 설명 한대로 점 (`.`) 구문을 사용하여 접근할 수 있습니다.

구조체는 값 타입 입니다; 변수나 상수에 할당될 때나 함수 호출에 대해 인수로 전달될 때 구조체의 인스턴스는 복사됩니다. 값 타입에 대한 자세한 내용은 [구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)](../language-guide-1/structures-and-classes.md#structures-and-enumerations-are-value-types) 을 참고 바랍니다.

[확장 선언 (Extension Declaration)](declarations.md#extension-declaration) 에서 설명 한대로 확장 선언으로 구조체 타입의 동작을 확장 할 수 있습니다.

> GRAMMAR OF A STRUCTURE DECLARATION\
> struct-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [access-level-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_access-level-modifier) $$_{opt}$$ `struct` [struct-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_struct-name) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [type-inheritance-clause](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-inheritance-clause) $$_{opt}$$ [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [struct-body](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_struct-body)\
> struct-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> struct-body → `{` [struct-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_struct-members) $$_{opt}$$ `}`\
> struct-members → [struct-member](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_struct-member) [struct-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_struct-members) $$_{opt}$$\
> struct-member → [declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration) | [compiler-control-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compiler-control-statement)

## 클래스 선언 (Class Declaration)

<!--
A class declaration introduces a named class type into your program. Class declarations are declared using the class keyword and have the following form:
-->

_클래스 선언 (class declaration)_ 은 프로그램에 명명된 클래스 타입을 도입합니다. 클래스 선언은 `class` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.41.43.png>)

<!--
The body of a class contains zero or more declarations. These declarations can include both stored and computed properties, instance methods, type methods, initializers, a single deinitializer, subscripts, type aliases, and even other class, structure, actor, and enumeration declarations. Class declarations can’t contain protocol declarations. For a discussion and several examples of classes that include various kinds of declarations, see Structures and Classes.

A class type can inherit from only one parent class, its superclass, but can adopt any number of protocols. The superclass appears first after the class name and colon, followed by any adopted protocols. Generic classes can inherit from other generic and nongeneric classes, but a nongeneric class can inherit only from other nongeneric classes. When you write the name of a generic superclass class after the colon, you must include the full name of that generic class, including its generic parameter clause.

As discussed in Initializer Declaration, classes can have designated and convenience initializers. The designated initializer of a class must initialize all of the class’s declared properties and it must do so before calling any of its superclass’s designated initializers.

A class can override properties, methods, subscripts, and initializers of its superclass. Overridden properties, methods, subscripts, and designated initializers must be marked with the override declaration modifier.

To require that subclasses implement a superclass’s initializer, mark the superclass’s initializer with the required declaration modifier. The subclass’s implementation of that initializer must also be marked with the required declaration modifier.

Although properties and methods declared in the superclass are inherited by the current class, designated initializers declared in the superclass are only inherited when the subclass meets the conditions described in Automatic Initializer Inheritance. Swift classes don’t inherit from a universal base class.

There are two ways to create an instance of a previously declared class:

* Call one of the initializers declared within the class, as described in Initializers.
* If no initializers are declared, and all properties of the class declaration were given initial values, call the class’s default initializer, as described in Default Initializers.

Access properties of a class instance with dot (.) syntax, as described in Accessing Properties.

Classes are reference types; instances of a class are referred to, rather than copied, when assigned to variables or constants, or when passed as arguments to a function call. For information about reference types, see Classes Are Reference Types.

You can extend the behavior of a class type with an extension declaration, as discussed in Extension Declaration.
-->

클래스의 본문은 선언이 없거나 하나 이상의 _선언 (declarations)_ 을 포함합니다. 이러한 _선언 (declarations)_ 은 저장된 프로퍼티와 계산된 프로퍼티, 인스턴스 메서드, 타입 메서드, 초기화 구문, 하나의 초기화 해제 구문, 서브 스크립트, 타입 별칭, 그리고 다른 클래스, 구조체, 행위자 그리고 열거형 선언을 포함할 수 있습니다. 클래스 선언은 프로토콜 선언을 포함할 수 없습니다. 여러종류의 선언을 포함하는 클래스의 자세한 설명과 예제는 [구조체와 클래스 (Structures and Classes)](../language-guide-1/structures-and-classes.md) 를 참고 바랍니다.

클래스 타입은 _상위클래스 (superclass)_ 로 하나의 부모 클래스만 상속할 수 있지만 프로토콜은 여러개 채택할 수 있습니다. _상위클래스 (superclass)_ 는 _클래스 이름 (class name)_ 과 콜론 다음에 첫번째로 나타나고 다음으로 _채택된 프로토콜 (adopted protocols)_ 이 나타납니다. 제너릭 클래스 (generic class) 는 다른 제너릭과 제너릭이 아닌 클래스를 상속할 수 있지만 제너릭이 아닌 클래스 (nongeneric class) 는 다른 제너릭이 아닌 클래스만 상속할 수 있습니다. 콜론 뒤에 상위 제너릭 클래스의 이름을 작성할 때 제너릭 파라미터 절을 포함하는 제너릭 클래스의 전체 이름을 포함해야 합니다.

[초기화 구문 선언 (Initializer Declaration)](declarations.md#initializer-declaration) 에서 설명 한대로 클래스는 지정된 초기화 구문 (designated initializers) 과 편의 초기화 구문 (convenience initializers) 을 가질 수 있습니다. 클래스의 지정된 초기화 구문은 모든 클래스의 선언된 프로퍼티가 초기화 되어야 하고 상위클래스의 지정된 초기화 구문을 호출하기 전에 수행되어야 합니다.

클래스는 상위클래스의 프로퍼티, 메서드, 서브 스크립트, 그리고 초기화 구문을 재정의 할 수 있습니다. 프로퍼티, 메서드, 서브 스크립트, 그리고 지정된 초기화 구문 재정의는 `override` 선언 수식어로 표시되어야 합니다.

하위클래스가 상위클래스의 초기화 구문 구현을 요구하려면 상위클래스의 초기화 구문을 `required` 선언 수식어로 표시해야 합니다. 해당 초기화 구문의 하위클래스의 구현도 `required` 선언 수식어로 표시되어야 합니다.

_상위클래스 (superclass)_ 에 선언된 프로퍼티와 메서드가 현재 클래스에 의해 상속되더라도 _상위클래스 (superclass)_ 에 선언된 지정된 초기화 구문은 [자동 초기화 구문 상속 (Automatic Initializer Inheritance)](../language-guide-1/initialization.md#automatic-initializer-inheritance) 에서 설명 한대로 하위클래스가 조건이 충족될 때만 상속됩니다. Swift 클래스는 범용 기본 클래스는 상속하지 않습니다.

이전에 선언된 클래스의 인스턴스를 생성하는 두가지 방법이 있습니다:

* [초기화 구문 (Initializers)](../language-guide-1/initialization.md#initializers) 에서 설명 한대로 클래스 내에 선언된 초기화 구문 중 하나를 호출 합니다.
* 선언된 초기화 구문이 없고 클래스 선언에 모든 프로퍼티에 기본값이 주어진 경우 [기본 초기화 구문 (Default Initializers)](../language-guide-1/initialization.md#default-initializers) 에서 설명 한대로 클래스의 기본 초기화 구문을 호출합니다.

[프로퍼티 접근 (Accessing Properties)](../language-guide-1/structures-and-classes.md#accessing-properties) 에 설명 한대로 점 (`.`) 구문으로 클래스 인스턴스의 프로퍼티에 접근 할 수 있습니다.

클래스는 참조 타입입니다; 클래스의 인스턴스는 변수나 상수에 할당되거나 함수 호출에 인스로 전달될 때 복사가 아닌 참조됩니다. 참조 타입에 대한 자세한 내용은 [구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)](../language-guide-1/structures-and-classes.md#structures-and-enumerations-are-value-types) 을 참고 바랍니다.

[확장 선언 (Extension Declaration)](declarations.md#extension-declaration) 에서 설명 한대로 확장 선언으로 클래스 타입의 동작을 확장할 수 있습니다.

> GRAMMAR OF A CLASS DECLARATION\
> class-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [access-level-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_access-level-modifier) $$_{opt}$$ `final` $$_{opt}$$ `class` [class-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_class-name) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [type-inheritance-clause](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-inheritance-clause) $$_{opt}$$ [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [class-body](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_class-body)\
> class-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ `final` [access-level-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_access-level-modifier) $$_{opt}$$ `class` [class-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_class-name) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [type-inheritance-clause](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-inheritance-clause) $$_{opt}$$ [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [class-body](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_class-body)\
> class-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> class-body → `{` [class-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_class-members) $$_{opt}$$ `}`\
> class-members → [class-member](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_class-member) [class-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_class-members) $$_{opt}$$\
> class-member → [declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration) | [compiler-control-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compiler-control-statement)

## 행위자 선언 (Actor Declaration)

<!--
An actor declaration introduces a named actor type into your program. Actor declarations are declared using the actor keyword and have the following form:
-->

_행위자 선언 (actor declaration)_ 은 프로그램에 행위자 타입으로 명명되어 도입됩니다. 행위자 선언은 `actor` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

![Actor Declaration](../.gitbook/assets/actor\_declaration.png)

<!--
The body of an actor contains zero or more declarations. These declarations can include both stored and computed properties, instance methods, type methods, initializers, a single deinitializer, subscripts, type aliases, and even other class, structure, and enumeration declarations. For a discussion and several examples of actors that include various kinds of declarations, see Actors.

Actor types can adopt any number of protocols, but can’t inherit from classes, enumerations, structures, or other actors. However, an actor that is marked with the @objc attribute implicitly conforms to the NSObjectProtocol protocol and is exposed to the Objective-C runtime as a subtype of NSObject.

There are two ways to create an instance of a previously declared actor:

* Call one of the initializers declared within the actor, as described in Initializers.
* If no initializers are declared, and all properties of the actor declaration were given initial values, call the actor’s default initializer, as described in Default Initializers.

By default, members of an actor are isolated to that actor. Code, such as the body of a method or the getter for a property, is executed on that actor. Code within the actor can interact with them synchronously because that code is already running on the same actor, but code outside the actor must mark them with await to indicate that this code is asynchronously running code on another actor. Key paths can’t refer to isolated members of an actor. Actor-isolated stored properties can be passed as in-out parameters to synchronous functions, but not to asynchronous functions.

Actors can also have nonisolated members, whose declarations are marked with the nonisolated keyword. A nonisolated member executes like code outside of the actor: It can’t interact with any of the actor’s isolated state, and callers don’t mark it with await when using it.

Members of an actor can be marked with the @objc attribute only if they are nonisolated or asynchronous.

The process of initializing an actor’s declared properties is described in Initialization.

Properties of a actor instance can be accessed using dot (.) syntax, as described in Accessing Properties.

Actors are reference types; instances of an actor are referred to, rather than copied, when assigned to variables or constants, or when passed as arguments to a function call. For information about reference types, see Classes Are Reference Types.

You can extend the behavior of a structure type with an extension declaration, as discussed in Extension Declaration.
-->

행위자의 본문에는 0개 이상의 _선언 (declarations)_ 이 포함되어 있습니다. 이 선언은 저장된 프로퍼티와 계산된 프로퍼티, 인스턴스 메서드, 타입 메서드, 초기화 구문, 하나의 초기화 해제 구문, 서브 스크립트, 타입 별칭, 그리고 기타 클래스, 구조체, 그리고 열거형 선언을 모두 포함할 수 있습니다. 행위자에 대한 설명과 몇가지 예제는 [행위자 (Actors)](../language-guide-1/concurrency.md#actors) 를 참조 바랍니다.

행위자 타입은 프로토콜을 채택할 수 있지만 클래스, 열거형, 구조체, 또는 다른 행위자를 상속할 수 없습니다. 그러나 `@objc` 속성으로 표시된 행위자는 암시적으로 `NSObjectProtocol` 프로토콜을 준수하고 `NSObject` 의 하위 타입으로 Objective-C 런타임에 노출됩니다.

이전에 선언된 행위자의 인스턴스를 만드는 방법에는 두가지 방법이 있습니다:

* [초기화 구문 (Initializers)](../language-guide-1/initialization.md#initializers) 에 설명된 대로 행위자 내에서 선언된 초기화 구문 중 하나를 호출합니다.
* 초기화 구문이 선언되지 않고 행위자 선언의 모든 프로퍼티에 초기값이 제공된 경우 [기본 초기화 구문 (Default Initializers)](../language-guide-1/initialization.md#default-initializers) 에서 설명한대로 행위자의 기본 초기화 구문을 호출합니다.

기본적으로 행위자의 멤버는 해당 행위자와 분리됩니다. 메서드의 본문이나 프로퍼티에 대한 getter 와 같은 코드는 해당 행위자에서 실행됩니다. 행위자 내의 코드는 해당 코드는 이미 동일한 행위자에서 실행되고 있기 때문에 동기적으로 상호 작용할 수 있지만 행위자 외부의 코드는 이 코드가 다른 행위자에서 비동기적으로 코드를 실행하고 있음을 나타내기 위해 `await` 로 표시해야 합니다. 키 경로는 행위자의 분리된 멤버를 참조할 수 없습니다. 분리된 행위자 저장된 프로퍼티 (Actor-isolated stored properties) 는 동기 함수에 in-out 파라미터로 전달할 수 있지만 비동기 함수에는 전달할 수 없습니다.

행위자는 선언이 `nonisolated` 키워드로 표시된 분리되지 않은 멤버 (nonisolated members) 를 가질 수도 있습니다. 분리되지 않은 멤버는 행위자 외부의 코드처럼 실행됩니다: 행위자의 분리상태와 상호작용 할 수 없으며 호출자는 이를 사용할 때 `await` 로 표시하지 않습니다.

행위자의 멤버는 분리되지 않거나 비동기 일때만 `@objc` 속성으로 표시될 수 있습니다.

행위자의 선언된 프로퍼티를 초기화 하는 과정은 [초기화 (Initialization)](../language-guide-1/initialization.md) 에 설명되어 있습니다.

행위자 인스턴스의 프로퍼티는 [프로퍼티 접근 (Accessing Properties)](../language-guide-1/structures-and-classes.md#accessing-properties) 에서 설명한대로 점 (`.`) 구문을 사용하여 접근될 수 있습니다.

행위자는 참조 타입입니다; 행위자의 인스턴스는 변수나 상수에 할당되거나 함수 호출에 인수로 전달될 때 복사되지 않고 참조됩니다. 참조 타입에 대한 자세한 내용은 [클래스는 참조 타입 (Classes Are Reference Types)](../language-guide-1/structures-and-classes.md#classes-are-reference-types) 를 참조 바랍니다.

[확장 선언 (Extension Declaration)](declarations.md#extension-declaration) 에서 설명한 대로 확장 선언을 사용하여 구조체 타입의 동작을 확장할 수 있습니다.

> GRAMMAR OF AN ACTOR DECLARATION \
> actor-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [access-level-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_access-level-modifier) $$_{opt}$$ `actor` [actor-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_actor-name) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [type-inheritance-clause](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-inheritance-clause) $$_{opt}$$ [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [actor-body](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_actor-body) \
> actor-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier) \
> actor-body → `{` [actor-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_actor-members) $$_{opt}$$ `}` \
> actor-members → [actor-member](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_actor-member) [actor-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_actor-members) $$_{opt}$$ \
> actor-member → [declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration)|[compiler-control-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compiler-control-statement)

## 프로토콜 선언 (Protocol Declaration)

<!--
A protocol declaration introduces a named protocol type into your program. Protocol declarations are declared at global scope using the protocol keyword and have the following form:
-->

_프로토콜 선언 (protocol declaration)_ 은 프로그램에 명명된 프로토콜 타입을 도입합니다. 프로토콜 선언은 `protocol` 키워드를 사용하여 전역에 선언되고 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.43.19.png>)

<!--
The body of a protocol contains zero or more protocol member declarations, which describe the conformance requirements that any type adopting the protocol must fulfill. In particular, a protocol can declare that conforming types must implement certain properties, methods, initializers, and subscripts. Protocols can also declare special kinds of type aliases, called associated types, that can specify relationships among the various declarations of the protocol. Protocol declarations can’t contain class, structure, enumeration, or other protocol declarations. The protocol member declarations are discussed in detail below.

Protocol types can inherit from any number of other protocols. When a protocol type inherits from other protocols, the set of requirements from those other protocols are aggregated, and any type that inherits from the current protocol must conform to all those requirements. For an example of how to use protocol inheritance, see Protocol Inheritance.
-->

프로토콜의 본문은 프로토콜을 채택하는 모든 타입이 충족해야 하는 준수성 요구사항을 설명하는 _프로토콜 멤버 선언 (protocol member declarations)_ 이 없거나 하나 이상의 _프로토콜 멤버 선언 (protocol member declarations)_ 을 포함합니다. 특히 프로토콜은 준수하는 타입이 특정 프로퍼티, 메서드, 초기화 구문, 그리고 서브 스크립트를 구현해야 한다고 선언할 수 있습니다. 프로토콜은 프로토콜의 여러 선언의 관계를 지정할 수 있는 _연관된 타입 (associated types)_ 이라는 특별한 종류의 타입 별칭도 선언할 수 있습니다. 프로토콜 선언은 클래스, 구조체, 열거형, 또는 다른 프로토콜 선언을 포함할 수 없습니다. _프로토콜 멤버 선언 (protocol member declarations)_ 은 아래 자세하게 설명되어 있습니다.

프로토콜 타입은 다른 프로토콜을 상속할 수 있습니다. 프로토콜 타입이 다른 프로토콜을 상속할 때 다른 프로토콜의 요구사항이 집계되고 현재 프로토콜에서 상속한 모든 타입은 모든 요구사항을 준수해야 합니다. 프로토콜 상속 사용법에 대한 예제는 [프로토콜 상속 (Protocol Inheritance)](../language-guide-1/protocols.md#protocol-inheritance) 을 참고 바랍니다.

<!--
NOTE
You can also aggregate the conformance requirements of multiple protocols using protocol composition types, as described in Protocol Composition Type and Protocol Composition.
-->

> NOTE\
> [프로토콜 구성 타입 (Protocol Composition Type)](types.md#protocol-composition-type) 과 [프로토콜 구성 (Protocol Composition)](../language-guide-1/protocols.md#protocol-composition) 에서 설명 한대로 프로토콜 구성 타입을 사용하여 여러개 프로토콜의 준수성 요구사항을 집계할 수도 있습니다.

<!--
You can add protocol conformance to a previously declared type by adopting the protocol in an extension declaration of that type. In the extension, you must implement all of the adopted protocol’s requirements. If the type already implements all of the requirements, you can leave the body of the extension declaration empty.

By default, types that conform to a protocol must implement all properties, methods, and subscripts declared in the protocol. That said, you can mark these protocol member declarations with the optional declaration modifier to specify that their implementation by a conforming type is optional. The optional modifier can be applied only to members that are marked with the objc attribute, and only to members of protocols that are marked with the objc attribute. As a result, only class types can adopt and conform to a protocol that contains optional member requirements. For more information about how to use the optional declaration modifier and for guidance about how to access optional protocol members—for example, when you’re not sure whether a conforming type implements them—see Optional Protocol Requirements.

The cases of an enumeration can satisfy protocol requirements for type members. Specifically, an enumeration case without any associated values satisfies a protocol requirement for a get-only type variable of type Self, and an enumeration case with associated values satisfies a protocol requirement for a function that returns Self whose parameters and their argument labels match the case’s associated values. For example:
-->

해당 타입에 확장 선언에서 프로토콜을 채택하기 위해 이전에 선언된 타입에 프로토콜 준수를 추가할 수 있습니다. 확장에서 채택된 프로토콜의 모든 요구사항을 구현해야 합니다. 타입이 이미 모든 요구사항을 구현한 경우 빈 확장 선언의 본문으로 남겨둘 수 있습니다.

기본적으로 프로토콜을 준수하는 타입은 프로토콜에 선언된 모든 프로퍼티, 메서드, 그리고 서브 스크립트를 구현해야 합니다. 이 말은 준수하는 타입에 의한 구현이 옵셔널로 지정하기 위해선 `optional` 선언 수식어로 프로토콜 멤버 선언을 표시해야 한다는 의미입니다. `optional` 수식어는 `objc` 속성으로 표시된 멤버와 `objc` 속성으로 표시된 프로토콜의 멤버에만 적용될 수 있습니다. 결과적으로 클래스 타입만 옵셔널 멤버 요구사항을 포함한 프로토콜을 채택하고 준수할 수 있습니다. optional 선언 수식어 사용에 대한 자세한 내용과 옵셔널 프로토콜 멤버에 어떻게 접근하는지에 대한 가이드—예를 들어 타입이 이를 구현하는지 확실하지 않은 경우—는 [옵셔널 프로토콜 요구사항 (Optional Protocol Requirements)](../language-guide-1/protocols.md#optional-protocol-requirements) 를 참고 바랍니다.

열거형의 케이스는 타입 멤버에 대해 프로토콜 요구사항을 충족할 수 있습니다. 특히, 연관된 값이 없는 열거형 케이스는 `Self` 타입의 get-only 타입 변수에 대해 프로토콜 요구사항을 충족하고 연관된 값이 있는 열거형 케이스는 파라미터와 인수 라벨이 케이스의 연관된 값과 일치하게 `Self` 를 반환하는 함수에 대해 프로토콜 요구사항을 충족합니다. 예를 들어:

```swift
protocol SomeProtocol {
    static var someValue: Self { get }
    static func someFunction(x: Int) -> Self
}
enum MyEnum: SomeProtocol {
    case someValue
    case someFunction(x: Int)
}
```

<!--
To restrict the adoption of a protocol to class types only, include the AnyObject protocol in the inherited protocols list after the colon. For example, the following protocol can be adopted only by class types:
-->

클래스 타입에 대해서만 프로토콜의 채택을 제한하려면 콜론 뒤에 _상속된 프로토콜 (inherited protocols)_ 리스트에 `AnyObject` 프로토콜을 추가해야 합니다. 예를 들어 다음의 프로토콜은 클래스 타입에 의해서만 채택될 수 있습니다:

```swift
protocol SomeProtocol: AnyObject {
    /* Protocol members go here */
}
```

<!--
Any protocol that inherits from a protocol that’s marked with the AnyObject requirement can likewise be adopted only by class types.
-->

`AnyObject` 요구사항으로 표시된 프로토콜을 상속하는 모든 프로토콜은 마찬가지로 클래스 타입에 의해서만 채택될 수 있습니다.

<!--
NOTE
If a protocol is marked with the objc attribute, the AnyObject requirement is implicitly applied to that protocol; there’s no need to mark the protocol with the AnyObject requirement explicitly.
-->

> NOTE\
> `objc` 속성으로 표시된 프로토콜은 해당 프로토콜에 암시적으로 `AnyObject` 요구사항이 적용됩니다; 명시적으로 `AnyObject` 요구사항을 프로토콜에 표시할 필요가 없습니다.

<!--
Protocols are named types, and thus they can appear in all the same places in your code as other named types, as discussed in Protocols as Types. However, you can’t construct an instance of a protocol, because protocols don’t actually provide the implementations for the requirements they specify.

You can use protocols to declare which methods a delegate of a class or structure should implement, as described in Delegation.
-->

프로토콜은 명명된 타입 이므로 [타입으로의 프로토콜 (Protocols as Types)](../language-guide-1/protocols.md#protocols-as-types) 에서 설명 한대로 다른 명명된 타입으로 코드의 동일한 위치에 나타날 수 있습니다. 그러나 프로토콜은 실제로 지정하는 요구사항에 대해 구현을 제공하지 않으므로 프로토콜의 인스턴스를 구성할 수 없습니다.

[위임 (Delegation)](../language-guide-1/protocols.md#delegation) 에서 설명 한대로 프로토콜을 사용하여 클래스 또는 구조체의 위임이 구현해야 하는 메서드를 선언할 수 있습니다.

> GRAMMAR OF A PROTOCOL DECLARATION\
> protocol-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [access-level-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_access-level-modifier) $$_{opt}$$ `protocol` [protocol-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_protocol-name) [type-inheritance-clause](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-inheritance-clause) $$_{opt}$$ [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [protocol-body](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_protocol-body)\
> protocol-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)\
> protocol-body → `{` [protocol-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_protocol-members) $$_{opt}$$ `}`\
> protocol-members → [protocol-member](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_protocol-member) [protocol-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_protocol-members) $$_{opt}$$\
> protocol-member → [protocol-member-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_protocol-member-declaration) | [compiler-control-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compiler-control-statement)\
> protocol-member-declaration → [protocol-property-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_protocol-property-declaration)\
> protocol-member-declaration → [protocol-method-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_protocol-method-declaration)\
> protocol-member-declaration → [protocol-initializer-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_protocol-initializer-declaration)\
> protocol-member-declaration → [protocol-subscript-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_protocol-subscript-declaration)\
> protocol-member-declaration → [protocol-associated-type-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_protocol-associated-type-declaration)\
> protocol-member-declaration → [typealias-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_typealias-declaration)

### 프로토콜 프로퍼티 선언 (Protocol Property Declaration)

<!--
Protocols declare that conforming types must implement a property by including a protocol property declaration in the body of the protocol declaration. Protocol property declarations have a special form of a variable declaration:
-->

프로토콜은 프로토콜 선언 본문에 _프로토콜 프로퍼티 선언 (protocol property declaration)_ 을 포함하여 준수 타입이 프로퍼티를 구현해야 된다고 선언합니다. 프로토콜 프로퍼티 선언은 변수 선언에 특별한 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.46.59.png>)

<!--
As with other protocol member declarations, these property declarations declare only the getter and setter requirements for types that conform to the protocol. As a result, you don’t implement the getter or setter directly in the protocol in which it’s declared.

The getter and setter requirements can be satisfied by a conforming type in a variety of ways. If a property declaration includes both the get and set keywords, a conforming type can implement it with a stored variable property or a computed property that’s both readable and writeable (that is, one that implements both a getter and a setter). However, that property declaration can’t be implemented as a constant property or a read-only computed property. If a property declaration includes only the get keyword, it can be implemented as any kind of property. For examples of conforming types that implement the property requirements of a protocol, see Property Requirements.

To declare a type property requirement in a protocol declaration, mark the property declaration with the static keyword. Structures and enumerations that conform to the protocol declare the property with the static keyword, and classes that conform to the protocol declare the property with either the static or class keyword. Extensions that add protocol conformance to a structure, enumeration, or class use the same keyword as the type they extend uses. Extensions that provide a default implementation for a type property requirement use the static keyword.

See also Variable Declaration.
-->

다른 프로토콜 멤버 선언과 마찬가지로 이러한 프로퍼티 선언은 프로토콜을 준수하는 타입에 대한 getter 와 setter 요구사항만 선언합니다. 결과적으로 선언된 프로토콜 내에 직접적으로 getter 와 setter 를 구현할 수 없습니다.

getter 와 setter 요구사항은 다양한 방법으로 준수하는 타입으로 충족될 수 있습니다. 프로퍼티 선언이 `get` 과 `set` 키워드를 모두 포함하면 준수하는 타입은 저장된 변수 프로퍼티나 getter 와 setter 모두 구현하는 속성과 같이 읽고 쓰기가 모두 가능한 계산된 프로퍼티로 구현할 수 있습니다. 그러나 프로퍼티 선언은 상수 프로퍼티나 읽기-전용 계산된 프로퍼티로 구현될 수 없습니다. 프로퍼티 선언이 `get` 키워드만 포함한다면 모든 종류의 프로퍼티로 구현할 수 있습니다. 프로토콜의 프로퍼티 요구사항을 구현하는 준수하는 타입의 예제는 [프로퍼티 요구사항 (Property Requirements)](../language-guide-1/protocols.md#property-requirements) 를 참고 바랍니다.

프로토콜 선언에 타입 프로퍼티 요구사항을 선언하려면 `static` 키워드로 프로퍼티 선언을 표시합니다. 프로토콜을 준수하는 구조체와 열거형은 `static` 키워드로 프로퍼티를 선언하고 프로토콜을 준수하는 클래스는 `static` 또는 `class` 키워드로 프로퍼티를 선언합니다. 구조체, 열거형, 또는 클래스에 프로토콜 준수를 추가하는 확장은 확장하는 타입과 동일한 키워드를 사용합니다. 타입 프로퍼티 요구사항에 대해 기본 구현을 제공하는 확장은 `static` 키워드를 사용합니다.

[변수 선언 (Variable Declaration)](declarations.md#variable-declaration) 도 참고 바랍니다.

> GRAMMAR OF A PROTOCOL PROPERTY DECLARATION\
> protocol-property-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-declaration-head) [variable-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_variable-name) [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-annotation) [getter-setter-keyword-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_getter-setter-keyword-block)

### 프로토콜 메서드 선언 (Protocol Method Declaration)

<!--
Protocols declare that conforming types must implement a method by including a protocol method declaration in the body of the protocol declaration. Protocol method declarations have the same form as function declarations, with two exceptions: They don’t include a function body, and you can’t provide any default parameter values as part of the function declaration. For examples of conforming types that implement the method requirements of a protocol, see Method Requirements.

To declare a class or static method requirement in a protocol declaration, mark the method declaration with the static declaration modifier. Structures and enumerations that conform to the protocol declare the method with the static keyword, and classes that conform to the protocol declare the method with either the static or class keyword. Extensions that add protocol conformance to a structure, enumeration, or class use the same keyword as the type they extend uses. Extensions that provide a default implementation for a type method requirement use the static keyword.

See also Function Declaration.
-->

프로토콜은 준수하는 타입이 프로토콜 선언 본문에 프로토콜 메서드 선언을 포함하기 위해 메서드를 구현해야 함을 선언합니다. 프로토콜 메서드 선언은 두가지를 제외하고 함수 선언과 동일한 형식을 가집니다: 함수 본문과 함수 선언의 일부로 기본 파라미터 값을 제공할 수 없습니다. 프로토콜의 메서드 요구사항을 구현하는 준수하는 타입에 대한 예제는 [메서드 요구사항 (Method Requirements)](../language-guide-1/protocols.md#method-requirements) 를 참고 바랍니다.

프로토콜 선언에 클래스 또는 정적 메서드 요구사항을 선언하려면 `static` 선언 수식어로 메서드 선언을 표시합니다. 프로토콜을 준수하는 구조체와 열거형은 `static` 키워드로 메서드를 선언하고 프로토콜을 준수하는 클래스는 `static` 또는 `class` 키워드로 메서드를 선언합니다. 구조체, 열거형, 또는 클래스에 프로토콜 준수를 추가하는 확장은 확장하는 타입과 동일한 키워드를 사용합니다. 타입 메서드 요구사항에 대한 기본 구현을 제공하는 확장은 `static` 키워드를 사용합니다.

[함수 선언 (Function Declaration)](declarations.md#function-declaration) 도 참고 바랍니다.

> GRAMMAR OF A PROTOCOL METHOD DECLARATION\
> protocol-method-declaration → [function-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_function-head) [function-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_function-name) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [function-signature](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_function-signature) [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$

### 프로토콜 초기화 구문 선언 (Protocol Initializer Declaration)

<!--
Protocols declare that conforming types must implement an initializer by including a protocol initializer declaration in the body of the protocol declaration. Protocol initializer declarations have the same form as initializer declarations, except they don’t include the initializer’s body.

A conforming type can satisfy a nonfailable protocol initializer requirement by implementing a nonfailable initializer or an init! failable initializer. A conforming type can satisfy a failable protocol initializer requirement by implementing any kind of initializer.

When a class implements an initializer to satisfy a protocol’s initializer requirement, the initializer must be marked with the required declaration modifier if the class isn’t already marked with the final declaration modifier.

See also Initializer Declaration.
-->

프로토콜은 준수하는 타입이 프로토콜 선언 본문에 프로토콜 초기화 구문 선언을 포함하기 위해 초기화 구문 구현을 선언합니다. 프로토콜 초기화 구문 선언은 초기화 구문의 본문을 포함하지 않는다는 점을 제외하고 초기화 구문 선언과 동일한 형식을 가집니다.

준수하는 타입은 실패할 수 없는 초기화 구문 (nonfailable initializer) 또는 `init!` 실패 가능한 초기화 구문 (failable initializer) 를 구현하여 실패할 수 없는 프로토콜 초기화 구문 요구사항을 충족할 수 있습니다. 준수하는 타입은 모든 종류의 초기화 구문 구현으로 실패 가능한 프로토콜 초기화 구문 요구항을 충족할 수 있습니다.

클래스는 프로토콜의 초기화 구문 요구사항을 충족하기 위해 초기화 구문을 구현할 때 초기화 구문이 `final` 선언 수식어로 표시되어 있지 않다면 `required` 선언 수식어로 표시되어야 합니다.

[초기화 구문 선언 (Initializer Declaration)](declarations.md#initializer-declaration) 도 참고 바랍니다.

> GRAMMAR OF A PROTOCOL INITIALIZER DECLARATION\
> protocol-initializer-declaration → [initializer-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer-head) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_parameter-clause)`throws`$$_{opt}$$ [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$\
> protocol-initializer-declaration → [initializer-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer-head) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_parameter-clause)`rethrows` [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$

### 프로토콜 서브 스크립트 선언 (Protocol Subscript Declaration)

<!--
Protocols declare that conforming types must implement a subscript by including a protocol subscript declaration in the body of the protocol declaration. Protocol subscript declarations have a special form of a subscript declaration:
-->

프롵콜은 준수하는 타입이 프로토콜 선언 본문에 프로토콜 서브 스크립트 선언을 포함하기 위해 서브 스크립트 구현을 선언합니다. 프로토콜 서브 스크립트 선언은 서브 스크립트 선언의 특별한 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.48.59.png>)

<!--
Subscript declarations only declare the minimum getter and setter implementation requirements for types that conform to the protocol. If the subscript declaration includes both the get and set keywords, a conforming type must implement both a getter and a setter clause. If the subscript declaration includes only the get keyword, a conforming type must implement at least a getter clause and optionally can implement a setter clause.

To declare a static subscript requirement in a protocol declaration, mark the subscript declaration with the static declaration modifier. Structures and enumerations that conform to the protocol declare the subscript with the static keyword, and classes that conform to the protocol declare the subscript with either the static or class keyword. Extensions that add protocol conformance to a structure, enumeration, or class use the same keyword as the type they extend uses. Extensions that provide a default implementation for a static subscript requirement use the static keyword.

See also Subscript Declaration.
-->

서브 스크립트 선언은 프로토콜을 준수하는 타입에 대해 최소한의 getter 와 setter 구현 요구사항만 선언합니다. 서브 스크립트 선언이 `get` 과 `set` 키워드 모두 포함한다면 준수하는 타입은 getter 와 setter 절 모두 구현해야 합니다. 서브 스크립트 선언이 `get` 키워드만 포함한다면 준수하는 타입은 _적어도_ getter 절은 구현해야 하고 선택적으로 setter 절을 구현할 수 있습니다.

프로토콜 선언에서 정적 서브 스크립트 요구사항을 선언하려면 `static` 선언 수식어로 서브 스크립트 선언을 표시합니다. 프로토콜은 준수하는 구조체와 열거형은 `static` 키워드로 서브 스크립트를 선언하고 프로토콜을 준수하는 클래스는 `static` 또는 `class` 키워드로 서브 스크립트를 선언합니다. 구조체, 열거형, 또는 클래스에 프로토콜 준수를 추가하는 확장은 확장하는 타입과 동일한 키워드를 사용합니다. 정적 서브 스크립트 요구사항에 대한 기본 구현을 제공하는 확장은 `static` 키워드를 사용합니다.

[서브 스크립트 선언 (Subscript Declaration)](declarations.md#subscript-declaration) 도 참고 바랍니다.

> GRAMMAR OF A PROTOCOL SUBSCRIPT DECLARATION\
> protocol-subscript-declaration → [subscript-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_subscript-head) [subscript-result](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_subscript-result) [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [getter-setter-keyword-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_getter-setter-keyword-block)

### 프로토콜 연관된 타입 선언 (Protocol Associated Type Declaration)

<!--
Protocols declare associated types using the associatedtype keyword. An associated type provides an alias for a type that’s used as part of a protocol’s declaration. Associated types are similar to type parameters in generic parameter clauses, but they’re associated with Self in the protocol in which they’re declared. In that context, Self refers to the eventual type that conforms to the protocol. For more information and examples, see Associated Types.

You use a generic where clause in a protocol declaration to add constraints to an associated types inherited from another protocol, without redeclaring the associated types. For example, the declarations of SubProtocol below are equivalent:
-->

프로토콜은 `associatedtype` 키워드를 사용하여 연관된 타입을 선언합니다. 연관된 타입은 프로토콜의 선언의 일부분으로 사용되는 타입에 대한 별칭을 제공합니다. 연관된 타입은 제너릭 파라미터 절에서 타입 파라미터와 유사하지만 선언된 프로토콜에서 `Self` 와 연관됩니다. 해당 컨텍스트에서 `Self` 는 프로토콜을 준수하는 최종 타입을 참조합니다. 더 자세한 내용과 예제는 [연관된 타입 (Associated Types)](../language-guide-1/generics.md#associated-types) 을 참고 바랍니다.

연관된 타입을 재선언 하지 않고 다른 프로토콜에서 상속된 연관된 타입에 제약사항을 추가하려면 프로토콜 선언에 제너릭 `where` 절을 사용합니다. 예를 들어 아래 `SubProtocol` 의 선언은 동일합니다:

```swift
protocol SomeProtocol {
    associatedtype SomeType
}

protocol SubProtocolA: SomeProtocol {
    // This syntax produces a warning.
    associatedtype SomeType: Equatable
}

// This syntax is preferred.
protocol SubProtocolB: SomeProtocol where SomeType: Equatable { }
```

<!--
See also Type Alias Declaration.
-->

[타입 별칭 선언 (Type Alias Declaration)](declarations.md#type-alias-declaration) 도 참고 바랍니다.

> GRAMMAR OF A PROTOCOL ASSOCIATED TYPE DECLARATION\
> protocol-associated-type-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [access-level-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_access-level-modifier) $$_{opt}$$ `associatedtype`[typealias-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_typealias-name) [type-inheritance-clause](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-inheritance-clause) $$_{opt}$$ [typealias-assignment](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_typealias-assignment) $$_{opt}$$ [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$

## 초기화 구문 선언 (Initializer Declaration)

<!--
An initializer declaration introduces an initializer for a class, structure, or enumeration into your program. Initializer declarations are declared using the init keyword and have two basic forms.

Structure, enumeration, and class types can have any number of initializers, but the rules and associated behavior for class initializers are different. Unlike structures and enumerations, classes have two kinds of initializers: designated initializers and convenience initializers, as described in Initialization.

The following form declares initializers for structures, enumerations, and designated initializers of classes:
-->

_초기화 구문 선언 (Initializer declaration)_ 은 프로그램에 클래스, 구조체, 또는 열거형에 대한 초기화 구문을 도입합니다. 초기화 구문 선언은 `init` 키워드를 사용하여 선언되고 두 개의 기본 형식을 가지고 있습니다.

구조체, 열거형, 그리고 클래스 타입은 여러개의 초기화 구문을 가질 수 있지만 클래스 초기화 구문에 대한 규칙과 관련 동작은 다릅니다. 구조체와 열거형과 다르게 클래스는 두 종류의 초기화 구문을 가집니다: [초기화 (Initialization)](../language-guide-1/initialization.md) 에서 설명 한대로 지정된 초기화 구문 (designated initializers) 과 편의 초기화 구문 (convenience initializers) 이 있습니다.

다음 형식은 구조체, 열거형, 그리고 클래스의 지정된 초기화 구문에 대한 초기화 구문을 선언합니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.51.46.png>)

<!--
A designated initializer of a class initializes all of the class’s properties directly. It can’t call any other initializers of the same class, and if the class has a superclass, it must call one of the superclass’s designated initializers. If the class inherits any properties from its superclass, one of the superclass’s designated initializers must be called before any of these properties can be set or modified in the current class.

Designated initializers can be declared in the context of a class declaration only and therefore can’t be added to a class using an extension declaration.

Initializers in structures and enumerations can call other declared initializers to delegate part or all of the initialization process.

To declare convenience initializers for a class, mark the initializer declaration with the convenience declaration modifier.
-->

클래스의 지정된 초기화 구문은 모든 클래스의 프로퍼티를 직접적으로 초기화 합니다. 같은 클래스의 다른 초기화 구문에서 호출할 수 없고 상위클래스를 가지고 있다면 상위클래스의 지정된 초기화 구문 중 하나를 호출해야 합니다. 클래스가 상위클래스에서 프로퍼티를 상속하면 상위클래스의 지정된 초기화 구문 중 하나는 현재 클래스에서 해당 프로퍼티에 설정되거나 수정되기 전에 호출되어야 합니다.

지정된 초기화 구문은 클래스 선언의 컨텍스트에서만 선언될 수 있으므로 확장 선언을 사용하는 클래스에 추가될 수 없습니다.

구조체와 열거형에 초기화 구문은 선언된 다른 초기화 구문을 호출하여 초기화 프로세스의 일부 또는 전체를 위임할 수 있습니다.

클래스에 편의 초기화 구문을 선언하려면 `convenience` 선언 수식어로 초기화 구문 선언을 표시합니다.

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 1.52.24.png>)

<!--
Convenience initializers can delegate the initialization process to another convenience initializer or to one of the class’s designated initializers. That said, the initialization processes must end with a call to a designated initializer that ultimately initializes the class’s properties. Convenience initializers can’t call a superclass’s initializers.

You can mark designated and convenience initializers with the required declaration modifier to require that every subclass implement the initializer. A subclass’s implementation of that initializer must also be marked with the required declaration modifier.

By default, initializers declared in a superclass aren’t inherited by subclasses. That said, if a subclass initializes all of its stored properties with default values and doesn’t define any initializers of its own, it inherits all of the superclass’s initializers. If the subclass overrides all of the superclass’s designated initializers, it inherits the superclass’s convenience initializers.

As with methods, properties, and subscripts, you need to mark overridden designated initializers with the override declaration modifier.
-->

편의 초기화 구문은 초기화 프로세스를 다른 편의 초기화 구문 또는 클래스의 지정된 초기화 구문 중 하나로 위임할 수 있습니다. 이 말은 초기화 프로세스는 궁극적으로 클래스의 프로퍼티를 초기화 하는 지정된 초기화 구문을 호출로 끝나야 한다는 의미입니다. 편의 초기화 구문은 상위클래스의 초기화 구문을 호출할 수 없습니다.

모든 하위클래스에 초기화 구문 구현을 요청하기 위해 `required` 선언 수식어로 지정된 초기화 구문과 편의 초기화 구문을 표시할 수 있습니다. 해당 초기화 구문의 하위클래스의 구현도 `required` 선언 수식어로 표시되어야 합니다.

기본적으로 상위클래스에 선언된 초기화 구문은 하위클래스에 의해 상속되지 않습니다. 이 말은 하위클래스는 기본값으로 모든 저장된 프로퍼티를 초기화 하고 자체 초기화 구문을 정의하지 않으면 모든 상위클래스의 초기화 구문을 상속합니다. 하위클래스가 모든 상위클래스의 지정된 초기화 구문을 재정의하면 상위클래스의 편의 초기화 구문을 상속합니다.

메서드, 프로퍼티, 그리고 서브 스크립트와 마찬가지로 `override` 선언 수식어로 재정의된 지정된 초기화 구문을 표시해야 합니다.

<!--
NOTE
If you mark an initializer with the required declaration modifier, you don’t also mark the initializer with the override modifier when you override the required initializer in a subclass.
-->

> NOTE\
> `required` 선언 수식어로 초기화 구문을 표시하면 하위클래스에서 필수 초기화 구문을 재정의 할 때 `override` 수식어로 초기화 구문을 표시하지 않아도 됩니다.

<!--
Just like functions and methods, initializers can throw or rethrow errors. And just like functions and methods, you use the throws or rethrows keyword after an initializer’s parameters to indicate the appropriate behavior. Likewise, initializers can be asynchronous, and you use the async keyword to indicate this.

To see examples of initializers in various type declarations, see Initialization.
-->

함수와 메서드처럼 초기화 구문은 에러를 던지거나 다시 던질 수 있습니다. 그리고 함수와 메서드처럼 적절한 동작을 나타내기 위해 초기화 구문의 파라미터 뒤에 `throws` 또는 `rethrows` 키워드를 사용합니다. 마찬가지로 초기화 구문은 비동기일 수 있으며 이것을 나타내기위해 `async` 키워드를 사용합니다.

여러 타입 선언에서 초기화 구문의 예제는 [초기화 (Initialization)](../language-guide-1/initialization.md) 을 참고 바랍니다.

### 실패 가능한 초기화 구문 (Failable Initializers)

<!--
A failable initializer is a type of initializer that produces an optional instance or an implicitly unwrapped optional instance of the type the initializer is declared on. As a result, a failable initializer can return nil to indicate that initialization failed.

To declare a failable initializer that produces an optional instance, append a question mark to the init keyword in the initializer declaration (init?). To declare a failable initializer that produces an implicitly unwrapped optional instance, append an exclamation point instead (init!). The example below shows an init? failable initializer that produces an optional instance of a structure.
-->

_실패 가능한 초기화 구문 (failable initializer)_ 은 초기화 구문이 선언된 타입의 옵셔널 인스턴스 또는 암시적 언래핑된 옵셔널 인스턴스를 생성하는 초기화 구문의 타입입니다. 결과적으로 실패 가능한 초기화 구문은 초기화 실패를 나타내는 `nil` 을 반환할 수 있습니다.

옵셔널 인스턴스를 생성하는 실패 가능한 초기화 구문을 선언하려면 초기화 구문 선언에서 `init` 키워드 뒤에 물음표를 붙입니다 (`init?`). 암시적으로 언래핑된 옵셔널 인스턴스를 생성하는 실패 가능한 초기화 구문을 선언하려면 뒤에 느낌표를 붙입니다 (`init!`). 아래의 예제는 구조체의 옵셔널 인스턴스를 생성하는 실패 가능한 초기화 구문 `init?` 을 보여줍니다.

```swift
struct SomeStruct {
    let property: String
    // produces an optional instance of 'SomeStruct'
    init?(input: String) {
        if input.isEmpty {
            // discard 'self' and return 'nil'
            return nil
        }
        property = input
    }
}
```

<!--
You call an init? failable initializer in the same way that you call a nonfailable initializer, except that you must deal with the optionality of the result.
-->

결과의 선택성을 다루는 것을 제외하고 실패없는 초기화 구문 호출과 동일하게 실패 가능한 초기화 구문 `init?` 을 호출합니다.

```swift
if let actualInstance = SomeStruct(input: "Hello") {
    // do something with the instance of 'SomeStruct'
} else {
    // initialization of 'SomeStruct' failed and the initializer returned 'nil'
}
```

<!--
A failable initializer can return nil at any point in the implementation of the initializer’s body.

A failable initializer can delegate to any kind of initializer. A nonfailable initializer can delegate to another nonfailable initializer or to an init! failable initializer. A nonfailable initializer can delegate to an init? failable initializer by force-unwrapping the result of the superclass’s initializer—for example, by writing super.init()!.

Initialization failure propagates through initializer delegation. Specifically, if a failable initializer delegates to an initializer that fails and returns nil, then the initializer that delegated also fails and implicitly returns nil. If a nonfailable initializer delegates to an init! failable initializer that fails and returns nil, then a runtime error is raised (as if you used the ! operator to unwrap an optional that has a nil value).

A failable designated initializer can be overridden in a subclass by any kind of designated initializer. A nonfailable designated initializer can be overridden in a subclass by a nonfailable designated initializer only.

For more information and to see examples of failable initializers, see Failable Initializers.
-->

실패 가능한 초기화 구문은 초기화 구문의 본문의 구현에 어느 위치에서든 `nil` 을 반환할 수 있습니다.

실패 가능한 초기화 구문은 여러 종류의 초기화 구문에 위임할 수 있습니다. 실패없는 초기화 구문은 다른 실패없는 초기화 구문이나 `init!` 실패 가능한 초기화 구문으로 위임할 수 있습니다. 실패없는 초기화 구문은 예를 들어 `super.init()!` 처럼 상위클래스의 초기화 구문에 강제 언래핑한 결과로 `init?` 실패 가능한 초기화 구문으로 위임할 수 있습니다.

초기화 실패는 초기화 구문 위임으로 전파됩니다. 특히, 실패 가능한 초기화 구문이 실패하고 `nil` 을 반환하는 초기화 구문에 위임하면 위임된 초기화 구문도 실패하고 암시적으로 `nil` 을 반환합니다. 실패없는 초기화 구문이 실패하고 `nil` 을 반환하는 `init!` 실패 가능한 초기화 구문에 위임하면 `nil` 값을 가지는 옵셔널을 언래핑 하기위해 `!` 연산자를 사용하는 것과 같이 런타임 에러가 발생합니다.

실패 가능한 지정된 초기화 구문은 모든 종류의 지정된 초기화 구문에 의해 하위클래스에서 재정의 될 수 있습니다. 실패없는 지정된 초기화 구문은 실패없는 지정된 초기화 구문에 의해서만 하위클래스에서 재정의 될 수 있습니다.

실패 가능한 초기화 구문에 대한 자세한 내용과 예제는 [실패 가능한 초기화 구문 (Failable Initializers)](../language-guide-1/initialization.md#failable-initializers) 을 참고 바랍니다.

> GRAMMAR OF AN INITIALIZER DECLARATION\
> initializer-declaration → [initializer-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer-head) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_parameter-clause) `async`$$_{opt}$$ `throws`$$_{opt}$$[generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [initializer-body](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer-body)\
> initializer-declaration → [initializer-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer-head) [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_parameter-clause) `async`$$_{opt}$$ `rethrows`[generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [initializer-body](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_initializer-body)\
> initializer-head → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [declaration-modifiers](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration-modifiers) $$_{opt}$$ `init`\
> initializer-head → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [declaration-modifiers](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration-modifiers) $$_{opt}$$ `init` `?`\
> initializer-head → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [declaration-modifiers](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration-modifiers) $$_{opt}$$ `init` `!`\
> initializer-body → [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)

## 초기화 해제 구문 선언 (Deinitializer Declaration)

<!--
A deinitializer declaration declares a deinitializer for a class type. Deinitializers take no parameters and have the following form:
-->

_초기화 해제 구문 선언 (deinitializer declaration)_ 은 클래스 타입에 대한 초기화 해제 구문을 선언합니다. 초기화 해제 구문은 파라미터를 가지지 않고 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 2.12.18.png>)

<!--
A deinitializer is called automatically when there are no longer any references to a class object, just before the class object is deallocated. A deinitializer can be declared only in the body of a class declaration—but not in an extension of a class—and each class can have at most one.

A subclass inherits its superclass’s deinitializer, which is implicitly called just before the subclass object is deallocated. The subclass object isn’t deallocated until all deinitializers in its inheritance chain have finished executing.

Deinitializers aren’t called directly.

For an example of how to use a deinitializer in a class declaration, see Deinitialization.
-->

초기화 해제 구문은 클래스 객체에 어떠한 참조도 없으면 클래스 객체가 할당 해제되기 직전에 자동으로 호출됩니다. 초기화 해제 구문은 클래스 선언의 본문 내에만 선언될 수 있지만 클래스의 확장에는 선언될 수 없고 각 클래스는 하나만 가질 수 있습니다.

하위클래스는 하위클래스 객체가 할당 해제되기 직전에 암시적으로 호출되는 상위클래스의 초기화 해제 구문을 상속합니다. 하위클래스 객체는 상속 체인의 모든 초기화 해제 구문이 실행을 완료할 때까지 할당 해제되지 않습니다.

초기화 해제 구문은 직접적으로 호출되지 않습니다.

클래스 선언에서 초기화 해제 구문이 어떻게 사용되는지에 대한 예제는 [초기화 해제 (Deinitialization)](../language-guide-1/deinitialization.md) 를 참고 바랍니다.

> GRAMMAR OF A DEINITIALIZER DECLARATION\
> deinitializer-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ `deinit` [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)

## 확장 선언 (Extension Declaration)

<!--
An extension declaration allows you to extend the behavior of existing types. Extension declarations are declared using the extension keyword and have the following form:
-->

_확장 선언 (extension declaration)_ 은 기존 타입의 동작을 확장할 수 있습니다. 확장 선언은 `extension` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 2.13.38.png>)

<!--
The body of an extension declaration contains zero or more declarations. These declarations can include computed properties, computed type properties, instance methods, type methods, initializers, subscript declarations, and even class, structure, and enumeration declarations. Extension declarations can’t contain deinitializer or protocol declarations, stored properties, property observers, or other extension declarations. Declarations in a protocol extension can’t be marked final. For a discussion and several examples of extensions that include various kinds of declarations, see Extensions.

If the type name is a class, structure, or enumeration type, the extension extends that type. If the type name is a protocol type, the extension extends all types that conform to that protocol.

Extension declarations that extend a generic type or a protocol with associated types can include requirements. If an instance of the extended type or of a type that conforms to the extended protocol satisfies the requirements, the instance gains the behavior specified in the declaration.

Extension declarations can contain initializer declarations. That said, if the type you’re extending is defined in another module, an initializer declaration must delegate to an initializer already defined in that module to ensure members of that type are properly initialized.

Properties, methods, and initializers of an existing type can’t be overridden in an extension of that type.

Extension declarations can add protocol conformance to an existing class, structure, or enumeration type by specifying adopted protocols:
-->

확장 선언의 본문은 _선언 (declarations)_ 이 하나도 없거나 하나 이상의 _선언 (declarations)_ 을 포함합니다. 이러한 _선언 (declaration)_ 스캔은 계산된 프로퍼티, 계산된 타입 프로퍼티, 인스턴스 메서드, 타입 메서드, 초기화 구문, 서브 스크립트 선언, 그리고 클래스, 구조체, 그리고 열거형 선언도 포함합니다. 확장 선언은 초기화 해제 구문 또는 프로토콜 선언, 저장된 프로퍼티, 프로퍼티 관찰자, 또는 다른 확장 선언은 포함할 수 없습니다. 프로토콜 확장에서 선언은 `final` 로 표시될 수 없습니다. 여러 종류의 선언을 포함한 확장에 대한 내용과 예제는 [확장 (Extensions)](../language-guide-1/extensions.md) 을 참고 바랍니다.

_타입 이름 (type name)_ 이 클래스, 구조체, 또는 열거형 타입 이면 확장은 해당 타입을 확장합니다. _타입 이름름 (type name)_ 이 프로토콜 타입 이면 확장은 해당 프로토콜을 준수하는 모든 타입을 확장합니다.

제

제너릭 타입 또는 연관된 타입이 있는 프로토콜을 확장하는 확장 선언은 _요구사항 (requirements)_ 을 포함할 수 있습니다. 확장된 타입 또는 확장된 프로토콜을 준수하는 타입의 인스턴스가 _요구사항 (requirements)_ 을 충족하면 인스턴스는 선언에 지정된 동작을 얻습니다.

확장 선언은 초기화 구문 선언을 포함할 수 있습니다. 이 말은 확장하려는 타입이 다른 모듈에 정의되면 초기화 구문 선언은 해당 타입의 멤버가 올바르게 초기화 되도록 해당 모듈에 이미 정의된 초기화 구문으로 위임해야 합니다.

기존 타입의 프로퍼티, 메서드, 그리고 초기화 구문은 해당 타입의 확장에서 재정의 될 수 없습니다.

확장 선언은 _채택된 프로토콜 (adopted protocols)_ 을 지정하여 기존 클래스, 구조체, 또는 열거형 타입에 프로토콜 준수를 추가할 수 있습니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 2.14.50.png>)

<!--
Extension declarations can’t add class inheritance to an existing class, and therefore you can specify only a list of protocols after the type name and colon.
-->

확장 선언은 기존 클래스에 클래스 상속을 추가할 수 없으므로 _타입 이름 (type name)_ 과 콜론 뒤에 프로토콜의 리스트만 지정할 수 있습니다.

### 조건부 준수성 (Conditional Conformance)

<!--
You can extend a generic type to conditionally conform to a protocol, so that instances of the type conform to the protocol only when certain requirements are met. You add conditional conformance to a protocol by including requirements in an extension declaration.
-->

조건부로 프로토콜을 준수하기 위해 제너릭 타입을 확장할 수 있기 때문에 타입의 인스턴스가 특정 요구사항만 충족되면 프로토콜을 준수하도록 할 수 있습니다. 확장 선언에 _요구사항 (requirements)_ 를 추가하여 존건부 프로토콜 준수성을 추가합니다.

#### 재정의한 요구사항은 일부 제너릭 컨텍스트에서 사용되지 않음 (Overridden Requirements Aren’t Used in Some Generic Contexts)

<!--
In some generic contexts, types that get behavior from conditional conformance to a protocol don’t always use the specialized implementations of that protocol’s requirements. To illustrate this behavior, the following example defines two protocols and a generic type that conditionally conforms to both protocols.
-->

일부 제너릭 컨텍스트에서 조건부 프로토콜 준수성에서 동작을 가져오는 타입은 해당 프로토콜의 요구사항의 특별한 구현을 항상 사용하지 않습니다. 이 동작을 설명하기 위해 다음 예제는 두 프로토콜 모두 조건부로 준수하는 두 프로토콜과 제너릭 타입을 정의합니다.

```swift
protocol Loggable {
    func log()
}
extension Loggable {
    func log() {
        print(self)
    }
}

protocol TitledLoggable: Loggable {
    static var logTitle: String { get }
}
extension TitledLoggable {
    func log() {
        print("\(Self.logTitle): \(self)")
    }
}

struct Pair<T>: CustomStringConvertible {
    let first: T
    let second: T
    var description: String {
        return "(\(first), \(second))"
    }
}

extension Pair: Loggable where T: Loggable { }
extension Pair: TitledLoggable where T: TitledLoggable {
    static var logTitle: String {
        return "Pair of '\(T.logTitle)'"
    }
}

extension String: TitledLoggable {
    static var logTitle: String {
        return "String"
    }
}
```

<!--
The Pair structure conforms to Loggable and TitledLoggable whenever its generic type conforms to Loggable or TitledLoggable, respectively. In the example below, oneAndTwo is an instance of Pair<String>, which conforms to TitledLoggable because String conforms to TitledLoggable. When the log() method is called on oneAndTwo directly, the specialized version containing the title string is used.
-->

`Pair` 구조체는 제너릭 타입이 `Loggable` 또는 `TitledLoggable` 을 각각 준수할 때마다 `Loggable` 과 `TitledLoggable` 을 준수합니다. 아래 예제에서 `oneAndTwo` 는 `String` 이 `TitledLoggable` 을 준수하기 때문에 `TitledLoggable` 을 준수하는 `Pair<String>` 의 인스턴스 입니다. `oneAndTwo` 에서 `log()` 메서드를 직접 호출하면 타이틀 문자열을 포함하는 특별한 버전이 사용됩니다.

```swift
let oneAndTwo = Pair(first: "one", second: "two")
oneAndTwo.log()
// Prints "Pair of 'String': (one, two)"
```

<!--
However, when oneAndTwo is used in a generic context or as an instance of the Loggable protocol, the specialized version isn’t used. Swift picks which implementation of log() to call by consulting only the minimum requirements that Pair needs to conform to Loggable. For this reason, the default implementation provided by the Loggable protocol is used instead.
-->

그러나 `oneAndTwo` 제너릭 컨텍스트에서 사용되거나 `Loggable` 프로토콜의 인스턴스로 사용되면 특별한 버전이 사용되지 않습니다. Swift 는 `Pair` 가 `Loggable` 준수하는데 필요한 최소 요구사항만 참조하여 호출하기 위한 `log()` 의 구현을 선택합니다. 이러한 이유로 `Loggable` 프로토콜에 의해 제공된 기본 구현이 대신 사용됩니다.

```swift
func doSomething<T: Loggable>(with x: T) {
    x.log()
}
doSomething(with: oneAndTwo)
// Prints "(one, two)"
```

<!--
When log() is called on the instance that’s passed to doSomething(_:), the customized title is omitted from the logged string.
-->

`log()` 가 `doSomething(_:)` 에 전달된 인스턴스에서 호출되면 사용자 정의 된 타이틀은 로깅 된 문자열에서 생략됩니다.

### 프로토콜 준수성은 중복되지 않아야 함 (Protocol Conformance Must Not Be Redundant)

<!--
A concrete type can conform to a particular protocol only once. Swift marks redundant protocol conformances as an error. You’re likely to encounter this kind of error in two kinds of situations. The first situation is when you explicitly conform to the same protocol multiple times, but with different requirements. The second situation is when you implicitly inherit from the same protocol multiple times. These situations are discussed in the sections below.
-->

구체적인 타입은 특정 프로토콜을 한 번만 준수 할 수 있습니다. Swift 는 중복 프로토콜 준수를 에러로 표시합니다. 두 가지 상황에서 이러한 에러가 발생할 수 있습니다. 첫번째 상황은 동일한 프로토콜을 여러번 명시적으로 준수하지만 요구사항이 다른 경우입니다. 두번째 상황은 암시적으로 동일한 프로토콜에서 여러번 상속할 때 입니다. 이러한 상황은 아래 섹션에 설명되어 있습니다.

#### 명시적 중복 해결 (Resolving Explicit Redundancy)

<!--
Multiple extensions on a concrete type can’t add conformance to the same protocol, even if the extensions’ requirements are mutually exclusive. This restriction is demonstrated in the example below. Two extension declarations attempt to add conditional conformance to the Serializable protocol, one for for arrays with Int elements, and one for arrays with String elements.
-->

구체적인 타입에 여러 확장은 확장의 요구사항이 독립적이더라도 동일한 프로토콜에 대한 준수성을 추가할 수 없습니다. 이 제한은 아래 예제에서 설명됩니다. 두 확장 선언은 `Int` 요소가 있는 배열에 대해서와 `String` 요소가 있는 배열에 대해 `Serializable` 프로토콜의 조건부 준수성을 추가를 시도합니다.

```swift
protocol Serializable {
    func serialize() -> Any
}

extension Array: Serializable where Element == Int {
    func serialize() -> Any {
        // implementation
    }
}
extension Array: Serializable where Element == String {
    func serialize() -> Any {
        // implementation
    }
}
// Error: redundant conformance of 'Array<Element>' to protocol 'Serializable'
```

<!--
If you need to add conditional conformance based on multiple concrete types, create a new protocol that each type can conform to and use that protocol as the requirement when declaring conditional conformance.
-->

여러 구체적인 타입을 기반으로 조건부 준수성을 추가하려면 각 타입이 준수하는 새로운 프로토콜을 생성하고 조건부 준수를 선언할 때 요구사항으로 해당 프로토콜을 사용합니다.

```swift
protocol SerializableInArray { }
extension Int: SerializableInArray { }
extension String: SerializableInArray { }

extension Array: Serializable where Element: SerializableInArray {
    func serialize() -> Any {
        // implementation
    }
}
```

#### 암시적 중복 해결 (Resolving Implicit Redundancy)

<!--
When a concrete type conditionally conforms to a protocol, that type implicitly conforms to any parent protocols with the same requirements.

If you need a type to conditionally conform to two protocols that inherit from a single parent, explicitly declare conformance to the parent protocol. This avoids implicitly conforming to the parent protocol twice with different requirements.

The following example explicitly declares the conditional conformance of Array to Loggable to avoid a conflict when declaring its conditional conformance to both TitledLoggable and the new MarkedLoggable protocol.
-->

구체적인 타입이 프로토콜을 조건적으로 준수하면 해당 타입은 동일한 요구사항을 가진 모든 부모 프로토콜을 암시적으로 준수합니다.

단일 부모에서 상속하는 두 프로토콜을 조건부로 준수하는 타입이 필요한 경우 명시적으로 부모 프로토콜에 준수성을 선언합니다. 이것은 요구사항이 다른 부모 프로토콜을 암시적으로 두번 준수하는 것을 방지합니다.

다음 예제는 `TitledLoggable` 과 새로운 `MarkedLoggable` 프로토콜을 조건부 준수를 선언할 때 충돌을 막기 위해 `Array` 의 조건부 준수를 `Loggable` 로 명시적으로 선언합니다.

```swift
protocol MarkedLoggable: Loggable {
    func markAndLog()
}

extension MarkedLoggable {
    func markAndLog() {
        print("----------")
        log()
    }
}

extension Array: Loggable where Element: Loggable { }
extension Array: TitledLoggable where Element: TitledLoggable {
    static var logTitle: String {
        return "Array of '\(Element.logTitle)'"
    }
}
extension Array: MarkedLoggable where Element: MarkedLoggable { }
```

<!--
Without the extension to explicitly declare conditional conformance to Loggable, the other Array extensions would implicitly create these declarations, resulting in an error:
-->

`Loggable` 에 대한 조건부 준수성을 명시적으로 선언하는 확장이 없으면 다른 `Array` 확장이 암시적으로 이러한 선언을 생성하여 에러가 발생합니다:

```swift
extension Array: Loggable where Element: TitledLoggable { }
extension Array: Loggable where Element: MarkedLoggable { }
// Error: redundant conformance of 'Array<Element>' to protocol 'Loggable'
```

> GRAMMAR OF AN EXTENSION DECLARATION\
> extension-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [access-level-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_access-level-modifier) $$_{opt}$$ `extension` [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-identifier) [type-inheritance-clause](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type-inheritance-clause) $$_{opt}$$ [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [extension-body](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_extension-body)\
> extension-body → `{` [extension-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_extension-members) $$_{opt}$$ `}`\
> extension-members → [extension-member](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_extension-member) [extension-members](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_extension-members) $$_{opt}$$\
> extension-member → [declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration) | [compiler-control-statement](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar\_compiler-control-statement)

## 서브 스크립트 선언 (Subscript Declaration)

<!--
A subscript declaration allows you to add subscripting support for objects of a particular type and are typically used to provide a convenient syntax for accessing the elements in a collection, list, or sequence. Subscript declarations are declared using the subscript keyword and have the following form:
-->

_서브 스크립트 (subscript)_ 선언은 특정 타입에 대한 객체에 대한 서브 스크립트를 지원을 추가할 수 있고 일반적으로 콜렉션, 리스트, 또는 시퀀스에서 요소의 접근에 대한 편리한 구문을 제공하기 위해 사용됩니다. 서브 스크립트 선언은 `subscript` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 2.21.14.png>)

<!--
Subscript declarations can appear only in the context of a class, structure, enumeration, extension, or protocol declaration.

The parameters specify one or more indexes used to access elements of the corresponding type in a subscript expression (for example, the i in the expression object[i]). Although the indexes used to access the elements can be of any type, each parameter must include a type annotation to specify the type of each index. The return type specifies the type of the element being accessed.

As with computed properties, subscript declarations support reading and writing the value of the accessed elements. The getter is used to read the value, and the setter is used to write the value. The setter clause is optional, and when only a getter is needed, you can omit both clauses and simply return the requested value directly. That said, if you provide a setter clause, you must also provide a getter clause.

The setter name and enclosing parentheses are optional. If you provide a setter name, it’s used as the name of the parameter to the setter. If you don’t provide a setter name, the default parameter name to the setter is value. The type of the parameter to the setter is the same as the return type.

You can overload a subscript declaration in the type in which it’s declared, as long as the parameters or the return type differ from the one you’re overloading. You can also override a subscript declaration inherited from a superclass. When you do so, you must mark the overridden subscript declaration with the override declaration modifier.

Subscript parameters follow the same rules as function parameters, with two exceptions. By default, the parameters used in subscripting don’t have argument labels, unlike functions, methods, and initializers. However, you can provide explicit argument labels using the same syntax that functions, methods, and initializers use. In addition, subscripts can’t have in-out parameters. A subscript parameter can have a default value, using the syntax described in Special Kinds of Parameters.

You can also declare subscripts in the context of a protocol declaration, as described in Protocol Subscript Declaration.

For more information about subscripting and to see examples of subscript declarations, see Subscripts.
-->

서브 스크립트 선언은 클래스, 구조체, 열거형, 확장, 또는 프로토콜 선언의 컨텍스트에서만 나타날 수 있습니다.

_파라미터 (parameters)_ 는 서브 스크립트 표현식에서 해당 타입의 요소에 접근하기 위해 사용되는 하나 이상의 인덱스를 지정합니다 (예를 들어 표현식 `object[i]` 에서 `i`). 요소에 접근하기 위해 사용되는 인덱스는 모든 타입이 될 수 있지만 각 파라미터는 각 인덱스의 타입을 지정하기 위해 타입 주석을 포함해야 합니다. _반환 타입 (return type)_ 은 접근하는 요소의 타입을 지정합니다.

계산된 프로퍼티와 마찬가지로 서브 스크립트 선언은 접근한 요소의 값을 읽고 쓰기를 제공합니다. getter 는 값을 읽기 위해 사용되고 setter 는 값을 쓰기 위해 사용됩니다. setter 절은 선택사항이며 getter 만 필요하다면 모든 절을 생략할 수 있고 직접적으로 요청된 값을 간단하게 반환합니다. 이 말은 setter 절을 제공하면 getter 절을 제공해야만 한다는 의미입니다.

_setter 이름 (setter name)_ 과 둘러싸인 소괄호는 선택사항입니다. setter 이름을 제공하면 setter 의 파라미터의 이름으로 사용됩니다. setter 이름을 제공하지 않으면 setter 의 기본 파라미터 이름은 `value` 입니다. setter 의 파라미터의 타입은 _반환 타입 (return type)_ 과 동일합니다.

_파라미터 (parameters)_ 또는 _반환 타입 (return type)_ 이 오버로딩 중인 것과 다르면 선언된 타입의 서브 스크립트 선언을 오버로드 할 수 있습니다. 상위클래스에서 상속된 서브 스크립트 선언을 재정의 할 수도 있습니다. 재정의 하면 `override` 선언 수식어로 재정의된 서브 스크립트 선언을 표시해야 합니다.

서브 스크립트 파라미터는 두가지를 제외하고 함수 파라미터와 동일한 규칙을 따릅니다. 기본 적으로 서브 스크립트에서 사용하는 파라미터는 함수, 메서드, 그리고 초기화 구문과 다르게 인수 라벨을 가지지 않습니다. 그러나 함수, 메서드, 그리고 초기화 구문에서 사용하는 동일한 구문을 사용하여 명시적으로 인수 라벨을 제공할 수 있습니다. 또한 서브 스크립트는 in-out 파라미터를 가질 수 없습니다. 서브 스크립트 파라미터는 [특별한 종류의 파라미터 (Special Kinds of Parameters)](declarations.md#special-kinds-of-parameters) 에서 설명 한 구문을 사용하여 기본값을 가질 수 있습니다.

[프로토콜 서브 스크립트 선언 (Protocol Subscript Declaration)](declarations.md#protocol-subscript-declaration) 에서 설명 한대로 프로토콜 선언의 컨텍스트에서 서브 스크립트를 선언할 수도 있습니다.

서브 스크립트에 대한 자세한 내용과 서브 스크립트 선언의 예제는 [서브 스크립트 (Subscripts)](../language-guide-1/subscripts.md) 를 참고 바랍니다.

### 타입 서브 스크립트 선언 (Type Subscript Declarations)

<!--
To declare a subscript that’s exposed by the type, rather than by instances of the type, mark the subscript declaration with the static declaration modifier. Classes can mark type computed properties with the class declaration modifier instead to allow subclasses to override the superclass’s implementation. In a class declaration, the static keyword has the same effect as marking the declaration with both the class and final declaration modifiers.
-->

타입의 인스턴스가 아닌 타입에 의해 노출되는 서브 스크립트를 선언하려면 `static` 선언 수식어로 서브 스크립트 선언을 표시합니다. 클래스는 대신 `class` 선언 수식어로 타입 계산된 프로퍼티를 표시하여 하위클래스가 상위클래스의 구현을 재정의 할 수 있습니다. 클래스 선언에서 `static` 키워드는 `class` 와 `final` 선언 수식어 모두 선언에 표시하는 것과 동일한 효과를 가집니다.

> GRAMMAR OF A SUBSCRIPT DECLARATION\
> subscript-declaration → [subscript-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_subscript-head) [subscript-result](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_subscript-result) [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_code-block)\
> subscript-declaration → [subscript-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_subscript-head) [subscript-result](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_subscript-result) [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [getter-setter-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_getter-setter-block)\
> subscript-declaration → [subscript-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_subscript-head) [subscript-result](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_subscript-result) [generic-where-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-where-clause) $$_{opt}$$ [getter-setter-keyword-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_getter-setter-keyword-block)\
> subscript-head → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [declaration-modifiers](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration-modifiers) $$_{opt}$$ `subscript` [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar\_generic-parameter-clause) $$_{opt}$$ [parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_parameter-clause)\
> subscript-result → `->` [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar\_attributes) $$_{opt}$$ [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar\_type)

## 연산자 선언 (Operator Declaration)

<!--
An operator declaration introduces a new infix, prefix, or postfix operator into your program and is declared using the operator keyword.

You can declare operators of three different fixities: infix, prefix, and postfix. The fixity of an operator specifies the relative position of an operator to its operands.

There are three basic forms of an operator declaration, one for each fixity. The fixity of the operator is specified by marking the operator declaration with the infix, prefix, or postfix declaration modifier before the operator keyword. In each form, the name of the operator can contain only the operator characters defined in Operators.

The following form declares a new infix operator:
-->

_연산자 선언 (operator declaration)_ 은 프로그램에 새로운 중위 (infix), 접두사 (prefix), 그리고 접미사 (postfix) 연산자를 도입하고 `operator` 키워드를 사용하여 선언됩니다.

중위, 접두사, 그리고 접미사의 세가지 수정사항의 연산자를 선언할 수 있습니다. 연산자의 _고정성 (fixity)_ 은 연산자의 피연산자의 상대 위치를 지정합니다.

연산자 선언에는 세가지 기본 형식이 있으며 각 고정성 마다 하나씩 있습니다. 연산자의 고정성은 `operator` 키워드 전에 `infix`, `prefix`, 또는 `postfix` 선언 수식어로 연산자의 선언을 표시하여 지정합니다. 각 형식에서 연산자의 이름은 [연산자 (Operators)](lexical-structure.md#operators) 에서 정의된 연산자 문자만 포함 할 수 있습니다.

다음 형식은 새로운 중위 연산자를 선언합니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 2.24.12.png>)

<!--
An infix operator is a binary operator that’s written between its two operands, such as the familiar addition operator (+) in the expression 1 + 2.

Infix operators can optionally specify a precedence group. If you omit the precedence group for an operator, Swift uses the default precedence group, DefaultPrecedence, which specifies a precedence just higher than TernaryPrecedence. For more information, see Precedence Group Declaration.

The following form declares a new prefix operator:
-->

_중위 연산자 (infix operator)_ 는 표현식 `1 + 2` 에서 덧셈 연산자 (`+`) 와 같이 두 피연산자 사이에 작성되는 이진 연산자 입니다.

중위 연산자는 선택적으로 우선순위 그룹을 지정할 수 있습니다. 연산자에 대해 우선순위 그룹을 생략하면 Swift 는 `TernaryPrecedence` 보다 더 높은 우선순위를 지정하는 `DefaultPrecedence` 기본 우선순위 그룹을 사용합니다. 더 자세한 내용은 [우선순위 그룹 선언 (Precedence Group Declaration)](declarations.md#precedence-group-declaration) 을 참고 바랍니다.

다음 형식은 새로운 접두사 연산자를 선언합니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 2.24.48.png>)

<!--
A prefix operator is a unary operator that’s written immediately before its operand, such as the prefix logical NOT operator (!) in the expression !a.

Prefix operators declarations don’t specify a precedence level. Prefix operators are nonassociative.

The following form declares a new postfix operator:
-->

_접두사 연산자 (prefix operator)_ 는 표현식 `!a` 에서 접두사 논리적 NOT 연산자 (`!`) 와 같은 피연산자 직전에 작성되는 단항 연산자 (unary operator) 입니다.

접두사 연산자 선언은 우선순위를 지정하지 않습니다. 접두사 연산자는 비연관성 (nonassociative) 입니다.

다음 형식은 접미사 연산자를 선언합니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 2.26.04.png>)

<!--
A postfix operator is a unary operator that’s written immediately after its operand, such as the postfix forced-unwrap operator (!) in the expression a!.

As with prefix operators, postfix operator declarations don’t specify a precedence level. Postfix operators are nonassociative.

After declaring a new operator, you implement it by declaring a static method that has the same name as the operator. The static method is a member of one of the types whose values the operator takes as an argument—for example, an operator that multiplies a Double by an Int is implemented as a static method on either the Double or Int structure. If you’re implementing a prefix or postfix operator, you must also mark that method declaration with the corresponding prefix or postfix declaration modifier. To see an example of how to create and implement a new operator, see Custom Operators.
-->

_접미사 연산자 (postfix operator)_ 는 표현식 `a!` 에서 접미사 강제 언래핑 연산자 (`!`) 와 같이 피연산자 바로 뒤에 작성되는 단항 연산자 (unary operator) 입니다.

접두사 연산와 같이 접미사 연산자 선언은 우선순위를 지정하지 않습니다. 접미사 연산자는 비연관성 (nonassociative) 입니다.

새로운 연산자를 선언한 후에 연산자와 같은 이름을 가지는 정적 메서드를 선언하여 구현합니다. 정적 메서드는 연산자가 값을 인수로 가지는 타입 중 하나의 멤버입니다—예를 들어 `Double` 에 `Int` 를 곱하는 연산자는 `Double` 또는 `Int` 구조체에서 정적 메서드로 구현됩니다. 접두사 또는 접미사 연산자를 구현하면 해당하는 `prefix` 또는 `postfix` 선언 수식어를 사용하여 메서드 선언을 표시해야 합니다. 새로운 연산자를 어떻게 생성하고 구현하는지에 대한 예제는 [사용자 정의 연산자 (Custom Operators)](../language-guide-1/advanced-operators.md#custom-operators) 를 참고 바랍니다.

> GRAMMAR OF AN OPERATOR DECLARATION\
> operator-declaration → [prefix-operator-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_prefix-operator-declaration) | [postfix-operator-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_postfix-operator-declaration) | [infix-operator-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_infix-operator-declaration)\
> prefix-operator-declaration → `prefix` `operator` [operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_operator)\
> postfix-operator-declaration → `postfix` `operator` [operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_operator)\
> infix-operator-declaration → `infix` `operator` [operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_operator) [infix-operator-group](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_infix-operator-group) $$_{opt}$$\
> infix-operator-group → `:` [precedence-group-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-name)

## 우선순위 그룹 선언 (Precedence Group Declaration)

<!--
A precedence group declaration introduces a new grouping for infix operator precedence into your program. The precedence of an operator specifies how tightly the operator binds to its operands, in the absence of grouping parentheses.

A precedence group declaration has the following form:
-->

_우선순위 그룹 선언 (precedence group declaration)_ 은 프로그램에서 중위 연산자 우선순위에 대한 새로운 그룹화을 도입합니다. 연산자의 우선순위는 그룹화 괄호가 없을 때 연산자가 피연산자에 얼마나 밀접하게 바인딩 되는지 지정합니다.

우선순위 그룹 선언은 다음의 형식을 가집니다:

![](<../.gitbook/assets/스크린샷 2021-02-22 오후 2.27.56.png>)

<!--
The lower group names and higher group names lists specify the new precedence group’s relation to existing precedence groups. The lowerThan precedence group attribute may only be used to refer to precedence groups declared outside of the current module. When two operators compete with each other for their operands, such as in the expression 2 + 3 * 5, the operator with the higher relative precedence binds more tightly to its operands.
-->

_하위 그룹 이름 (lower group names)_ 과 _상위 그룹 이름 (higher group names)_ 리스트은 기존 우선순위 그룹에 새로운 우선순위 그룹의 관계를 지정합니다. `lowerThan` 우선순위 그룹 속성은 현재 모듈의 외부에 선언된 우선순위 그룹을 참조하는 데만 사용됩니다. 표현식 `2 + 3 * 5` 에서와 같이 두 연산자가 피연산자에 대해 서로 경쟁할 때 상대적으로 우선순위가 높은 연산자가 피연산자에 더 밀접하게 바인딩 됩니다.

<!--
NOTE
Precedence groups related to each other using lower group names and higher group names must fit into a single relational hierarchy, but they don’t have to form a linear hierarchy. This means it’s possible to have precedence groups with undefined relative precedence. Operators from those precedence groups can’t be used next to each other without grouping parentheses.
-->

> NOTE\
> _하위 그룹 이름 (lower group names)_ 과 _상위 그룹 이름 (higher group names)_ 을 사용하는 서로 관련된 우선순위 그룹은 단일 관계형 계층 (single relational hierarchy) 에 적합해야 하지만 선형 계층 (linear hierarchy) 을 형성할 필요는 없습니다. 이것은 상대적 우선순위가 정의되지 않은 우선순위 그룹일 수 있다는 의미입니다.\
> 이러한 우선순위 연산자는 그룹화 괄호없이 나란히 사용할 수 없습니다.

<!--
Swift defines numerous precedence groups to go along with the operators provided by the standard library. For example, the addition (+) and subtraction (-) operators belong to the AdditionPrecedence group, and the multiplication (*) and division (/) operators belong to the MultiplicationPrecedence group. For a complete list of precedence groups provided by the Swift standard library, see Operator Declarations.

The associativity of an operator specifies how a sequence of operators with the same precedence level are grouped together in the absence of grouping parentheses. You specify the associativity of an operator by writing one of the context-sensitive keywords left, right, or none—if your omit the associativity, the default is none. Operators that are left-associative group left-to-right. For example, the subtraction operator (-) is left-associative, so the expression 4 - 5 - 6 is grouped as (4 - 5) - 6 and evaluates to -7. Operators that are right-associative group right-to-left, and operators that are specified with an associativity of none don’t associate at all. Nonassociative operators of the same precedence level can’t appear adjacent to each to other. For example, the < operator has an associativity of none, which means 1 < 2 < 3 isn’t a valid expression.

The assignment of a precedence group specifies the precedence of an operator when used in an operation that includes optional chaining. When set to true, an operator in the corresponding precedence group uses the same grouping rules during optional chaining as the assignment operators from the standard library. Otherwise, when set to false or omitted, operators in the precedence group follows the same optional chaining rules as operators that don’t perform assignment.
-->

Swift 는 표준 라이브러리에서 제공된 연산자와 함께 사용할 수많은 우선순위 그룹을 정의합니다. 예를 들어 덧셈 (`+`) 과 뺄셈 (`-`) 연산자는 `AdditionPrecedence` 그룹에 속하고 곱셈 (`*`) 과 나눗셈 (`/`) 연산자는 `MultiplicationPrecedence` 그룹에 속합니다. Swift 표준 라이브러리에서 제공하는 우선순위 그룹의 완벽한 리스트은 [연산자 선언 (Operator Declarations)](https://developer.apple.com/documentation/swift/swift\_standard\_library/operator\_declarations) 을 참고 바랍니다.

연산자의 _연관성 (associativity)_ 은 그룹화 괄호가 없을 때 동일한 우선순위 수준을 가진 일련의 연산자가 함께 그룹화 되는 방식을 지정합니다. 상황에 맞는 키워드 `left`, `right`, 또는 `none` 중 하나를 작성하여 연산자의 연관성을 지정합니다—연관성을 생략하면 기본적으로 `none` 입니다. 왼쪽 연관성 (left-associative) 연산자는 왼쪽에서 오른쪽으로 그룹화 합니다. 예를 들어 뺄셈 연산자 (`-`) 는 표현식 `4 - 5 - 6` 은 `(4 - 5) - 6` 으로 그룹화 되고 `-7` 로 평가되므로 왼쪽 연관성 (left-associative) 입니다. 오른쪽 연관성 (right-associative) 연산자는 오른쪽에서 왼쪽으로 그룹화 하고 `none` 의 연관성으로 지정된 연산자는 전혀 연관되지 않습니다. 동일한 우선순위 수준의 비연관성 연산자는 서로 인접하게 표시될 수 없습니다. 예를 들어 `<` 연산자는 `none` 의 연관성을 가집니다. 이것은 `1 < 2 < 3` 은 유효하지 않은 표현식이라는 의미입니다.

우선순위 그룹의 _할당 (assignment)_ 은 옵셔널 체이닝을 포함하는 동작을 사용할 때 연산자의 우선순위를 지정합니다. `true` 로 설정하면 해당 우선순위 그룹에서 연산자는 옵셔널 체이닝 중에 표준 라이브러리의 할당 연산자와 동일한 그룹화 규칙을 사용합니다. 그렇지 않으면 `false` 로 설정하거나 생략된 경우 우선순위 그룹에서 연산자는 할당을 수행하지 않은 연산자와 동일한 옵셔널 체이닝 규칙을 따릅니다.

> GRAMMAR OF A PRECEDENCE GROUP DECLARATION\
> precedence-group-declaration → `precedencegroup` [precedence-group-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-name) `{` [precedence-group-attributes](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-attributes) $$_{opt}$$ `}`\
> precedence-group-attributes → [precedence-group-attribute](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-attribute) [precedence-group-attributes](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-attributes) $$_{opt}$$\
> precedence-group-attribute → [precedence-group-relation](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-relation)\
> precedence-group-attribute → [precedence-group-assignment](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-assignment)\
> precedence-group-attribute → [precedence-group-associativity](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-associativity)\
> precedence-group-relation → `higherThan` `:` [precedence-group-names](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-names)\
> precedence-group-relation → `lowerThan` `:` [precedence-group-names](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-names)\
> precedence-group-assignment → `assignment` `:` [boolean-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_boolean-literal)\
> precedence-group-associativity → `associativity` `:` `left`\
> precedence-group-associativity → `associativity` `:` `right`\
> precedence-group-associativity → `associativity` `:` `none`\
> precedence-group-names → [precedence-group-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-name) | [precedence-group-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-name) `,` [precedence-group-names](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_precedence-group-names)\
> precedence-group-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar\_identifier)

## 선언 수식어 (Declaration Modifiers)

<!--
Declaration modifiers are keywords or context-sensitive keywords that modify the behavior or meaning of a declaration. You specify a declaration modifier by writing the appropriate keyword or context-sensitive keyword between a declaration’s attributes (if any) and the keyword that introduces the declaration.
-->

_선언 수식어 (Declaration modifiers)_ 는 선언의 동작이나 의미를 수정하는 키워드 또는 상황에 맞는 키워드 입니다. 선언의 속성이 있는 경우 선언의 속성과 선언을 도입하는 키워드 사이에 적절한 키워드 또는 상황에 맞는 키워드를 작성하여 선언 수식어를 지정합니다.

`class`

<!--
Apply this modifier to a member of a class to indicate that the member is a member of the class itself, rather than a member of instances of the class. Members of a superclass that have this modifier and don’t have the final modifier can be overridden by subclasses.
-->

이 수식어를 클래스의 멤버에 적용하여 멤버가 클래스의 인스턴스의 멤버가 아니라 클래스 자체의 멤버임을 나타냅니다. 이 수식어를 가지고 `final` 수식어를 가지지 않는 상위클래스의 멤버는 하위클래스에 의해 재정의될 수 있습니다.

`dynamic`

<!--
Apply this modifier to any member of a class that can be represented by Objective-C. When you mark a member declaration with the dynamic modifier, access to that member is always dynamically dispatched using the Objective-C runtime. Access to that member is never inlined or devirtualized by the compiler.

Because declarations marked with the dynamic modifier are dispatched using the Objective-C runtime, they must be marked with the objc attribute.
-->

Objective-C 에 의해 표현될 수 있는 클래스의 모든 멤버에 이 수식어를 적용합니다. `dynamic` 수식어로 멤버 선언을 표시하면 해당 멤버에 대한 접근이 항상 Objective-C 런타임을 사용하여 동적으로 전달됩니다. 해당 멤버에 대한 접근은 컴파일러에 의해 인라인되거나 가상화 되지 않습니다.

`dynamic` 수식어로 표시된 선언은 Objective-C 런타임을 사용하여 전달되므로 `objc` 속성으로 표시되어야 합니다.

`final`

<!--
Apply this modifier to a class or to a property, method, or subscript member of a class. It’s applied to a class to indicate that the class can’t be subclassed. It’s applied to a property, method, or subscript of a class to indicate that a class member can’t be overridden in any subclass. For an example of how to use the final attribute, see Preventing Overrides.
-->

이 수식어를 클래스나 프로퍼티, 메서드, 또는 클래스의 서브 스크립트 멤버에 적용합니다. 클래스가 하위클래스화 될 수 없음을 나타내기 위해 클래스에 적용됩니다. 클래스 멤버가 모든 하위클래스에 재정의될 수 없음을 나타내기 위해 프로퍼티, 메서드, 또는 클래스의 서브 스크립트에 적용됩니다. `final` 속성을 어떻게 사용하는지에 대한 예제는 [재정의 방지 (Preventing Overrides)](../language-guide-1/inheritance.md#preventing-overrides) 를 참고 바랍니다.

`lazy`

<!--
Apply this modifier to a stored variable property of a class or structure to indicate that the property’s initial value is calculated and stored at most once, when the property is first accessed. For an example of how to use the lazy modifier, see Lazy Stored Properties.
-->

이 수식어는 클래스 또는 구조체의 저장된 변수 프로퍼티에 적용하여 프로퍼티의 초기값이 프로퍼티에 처음 접근할 때 최대 한번만 계산되고 저장됨을 나타냅니다. `lazy` 수식어를 어떻게 사용하는지에 대한 예제는 [지연 저장된 프로퍼티 (Lazy Stored Properties)](../language-guide-1/properties.md#lazy-stored-properties) 를 참고 바랍니다.

`optional`

<!--
Apply this modifier to a protocol’s property, method, or subscript members to indicate that a conforming type isn’t required to implement those members.

You can apply the optional modifier only to protocols that are marked with the objc attribute. As a result, only class types can adopt and conform to a protocol that contains optional member requirements. For more information about how to use the optional modifier and for guidance about how to access optional protocol members—for example, when you’re not sure whether a conforming type implements them—see Optional Protocol Requirements.
-->

프로토콜의 프로퍼티, 메서드, 또는 서브 스크립트 멤버에 수식어를 적용하여 준수하는 타입에 이 멤버의 구현이 필수가 아님을 나타냅니다.

`objc` 로 표시된 프로토콜에만 `optional` 수식어를 적용할 수 있습니다. 결과적으로 클래스 타입만 선택적 멤버 요구사항을 포함하는 프로토콜을 채택하고 준수할 수 있습니다. `optional` 수식어를 어떻게 사용하는지에 대한 자세한 내용과 예를 들어 준수하는 타입이 이를 구현하는지 확실하지 않을 때 선택적 프로토콜 멤버에 접근하는 방법에 대한 가이드는 [옵셔널 프로토콜 요구사항 (Optional Protocol Requirements)](../language-guide-1/protocols.md#optional-protocol-requirements) 을 참고 바랍니다.

`required`

<!--
Apply this modifier to a designated or convenience initializer of a class to indicate that every subclass must implement that initializer. The subclass’s implementation of that initializer must also be marked with the required modifier.
-->

초기화 구문을 구현해야 하는 모든 하위클래스를 나타내기 위해 클래스의 지정된 초기화 구문이나 편리한 초기화 구문에 이 수식어를 적용합니다. 초기화 구문의 하위클래스의 구현은 `required` 수식어로 표시되어야 합니다.

`static`

<!--
Apply this modifier to a member of a structure, class, enumeration, or protocol to indicate that the member is a member of the type, rather than a member of instances of that type. In the scope of a class declaration, writing the static modifier on a member declaration has the same effect as writing the class and final modifiers on that member declaration. However, constant type properties of a class are an exception: static has its normal, nonclass meaning there because you can’t write class or final on those declarations.
-->

이 수식어는 구조체, 클래스, 열거형, 또는 프로토콜의 멤버에 적용하여 멤버가 해당 타입의 인스턴스 멤버가 아닌 타입의 멤버 임을 나타냅니다. 클래스 선언의 범위에서 멤버 선언에 `static` 수식어를 작성하여 멤버 선언에 `class` 와 `final` 수식어를 작성한 것과 같은 효과를 가집니다. 그러나 클래스의 상수 타입 프로퍼티는 예외입니다: `static` 은 해당 선언에 `class` 나 `final` 을 작성할 수 없으므로 클래스가 아닐 때 보통의 의미를 가집니다.

`unowned`

<!--
Apply this modifier to a stored variable, constant, or stored property to indicate that the variable or property has an unowned reference to the object stored as its value. If you try to access the variable or property after the object has been deallocated, a runtime error is raised. Like a weak reference, the type of the property or value must be a class type; unlike a weak reference, the type is non-optional. For an example and more information about the unowned modifier, see Unowned References.
-->

이 수식어를 저장된 변수, 상수, 또는 저장된 프로퍼티에 적용하여 변수 또는 프로퍼티가 해당 값으로 저장된 객체에 미소유 참조가 있다는 것을 나타냅니다. 객체가 할당 해제된 후에 변수나 프로퍼티에 접근하려고 하면 런타임 에러가 발생합니다. 약한 참조와 마찬가지로 프로퍼티 또는 값의 타입은 클래스 타입이어야 합니다; 약한 참조와 다르게 타입은 옵셔널이 아닙니다. `unowned` 수식어에 대한 자세한 내용은 [미소유 참조 (Unowned References)](../language-guide-1/automatic-reference-counting.md#unowned-references) 를 참고 바랍니다.

`unowned(safe)`

<!--
An explicit spelling of unowned.
-->

`unowned` 의 명시적 철자입니다.

`unowned(unsafe)`

<!--
Apply this modifier to a stored variable, constant, or stored property to indicate that the variable or property has an unowned reference to the object stored as its value. If you try to access the variable or property after the object has been deallocated, you’ll access the memory at the location where the object used to be, which is a memory-unsafe operation. Like a weak reference, the type of the property or value must be a class type; unlike a weak reference, the type is non-optional. For an example and more information about the unowned modifier, see Unowned References.
-->

이 수식어를 저장된 변수, 상수, 또는 저장된 프로퍼티에 적용하여 변수나 프로퍼티에 해당 값으로 저장된 객체에 미소유 참조를 가집니다. 객체가 할당 해제된 후에 변수나 프로퍼티에 접근하려고 하면 객체가 사용된 메모리 위치에 접근하는데 이것은 안전하지 않은 메모리 (memory-unsafe) 동작입니다. 약한 참조와 마찬가지로 프로퍼티나 값의 타입은 클래스 타입이어야 합니다; 약한 참조와 다르게 타입은 옵셔널이 아닙니다. `unowned` 수식어에 대한 예제와 자세한 내용은 [미소유 참조 (Unowned References)](../language-guide-1/automatic-reference-counting.md#unowned-references) 를 참고 바랍니다.

`weak`

<!--
Apply this modifier to a stored variable or stored variable property to indicate that the variable or property has a weak reference to the object stored as its value. The type of the variable or property must be an optional class type. If you access the variable or property after the object has been deallocated, its value is nil. For an example and more information about the weak modifier, see Weak References.
-->

이 수식어를 저장된 변수나 저장된 변수 프로퍼티에 적용하면 변수 또는 프로퍼티에 해당 값으로 저장된 객체에 약한 참조를 가짐을 나타냅니다. 변수나 프로퍼티의 타입은 옵셔널 클래스 타입이어야 합니다. 객체가 할당 해제된 후에 변수나 프로퍼티에 접근하려고 하면 해당 값은 `nil` 입니다. `weak` 수식어에 대한 예제와 자세한 내용은 [약한 참조 (Weak References)](../language-guide-1/automatic-reference-counting.md#weak-references) 를 참고 바랍니다.

### 접근 제어 수준 (Access Control Levels)

<!--
Swift provides five levels of access control: open, public, internal, file private, and private. You can mark a declaration with one of the access-level modifiers below to specify the declaration’s access level. Access control is discussed in detail in Access Control.
-->

Swift 는 다섯 수준의 접근 제어를 제공합니다: open, public, internal, file private, 그리고 private 입니다. 선언의 접근 수준을 지정하기 위해 접근-수준 수식어 중 하나로 선언을 표시할 수 있습니다. 접근 제어는 [접근 제어 (Access Control)](../language-guide-1/access-control.md) 에 자세히 설명되어 있습니다.

`open`

<!--
Apply this modifier to a declaration to indicate the declaration can be accessed and subclassed by code in the same module as the declaration. Declarations marked with the open access-level modifier can also be accessed and subclassed by code in a module that imports the module that contains that declaration.
-->

이 수식어를 선언에 적용하여 선언과 동일한 모듈의 코드로 선언에 접근되고 하위클래스화 될 수 있음을 나타냅니다. `open` 접근-수준 수식어로 표시된 선언은 해당 선언을 포함하는 모듈을 가져오는 모듈의 코드에서 접근되고 하위클래스화 될 수 있습니다.

`public`

<!--
Apply this modifier to a declaration to indicate the declaration can be accessed and subclassed by code in the same module as the declaration. Declarations marked with the public access-level modifier can also be accessed (but not subclassed) by code in a module that imports the module that contains that declaration.
-->

이 수식어를 선언에 적용하여 선언과 동일한 모듈의 코드로 선언에 접근되고 하위클래스화 될 수 있음을 나타냅니다. `public` 접근-수준 수식어로 표시된 선언은 해당 선언을 포함하는 모듈을 가져오는 모듈의 코드에서 접근되지만 하위클래스화 될 수 없습니다.

`internal`

<!--
Apply this modifier to a declaration to indicate the declaration can be accessed only by code in the same module as the declaration. By default, most declarations are implicitly marked with the internal access-level modifier.
-->

이 수식어를 선언에 적용하여 선언과 동일한 모듈의 코드로 선언에 접근될 수 있습니다. 기본적으로 대부분 선언은 암시적으로 `internal` 접근-수준 수식어로 표시됩니다.

`fileprivate`

<!--
Apply this modifier to a declaration to indicate the declaration can be accessed only by code in the same source file as the declaration.
-->

이 수식어를 선언에 적용하여 선언과 동일한 소스 파일의 코드로만 선언에 접근될 수 있습니다.

`private`

<!--
Apply this modifier to a declaration to indicate the declaration can be accessed only by code within the declaration’s immediate enclosing scope.
-->

이 수식어를 선언에 적용하여 선언의 직접 둘러싸인 범위 내의 코드로만 선언에 접근될 수 있습니다.

<!--
For the purpose of access control, extensions to the same type that are in the same file share an access-control scope. If the type they extend is also in the same file, they share the type’s access-control scope. Private members declared in the type’s declaration can be accessed from extensions, and private members declared in one extension can be accessed from other extensions and from the type’s declaration.

Each access-level modifier above optionally accepts a single argument, which consists of the set keyword enclosed in parentheses (for example, private(set)). Use this form of an access-level modifier when you want to specify an access level for the setter of a variable or subscript that’s less than or equal to the access level of the variable or subscript itself, as discussed in Getters and Setters.
-->

접근 제어를 위해 동일한 파일에 있는 동일한 타입에 대한 확장은 접근-제어 범위를 공유합니다. 확장하는 타입도 동일한 파일에 있으면 타입의 접근-제어 범위를 공유합니다. 타입의 선언에서 선언된 private 멤버는 확장에서 접근할 수 있고 하나의 확장에서 선언된 private 멤버는 다른 확장과 타입의 선언에서 접근될 수 있습니다.

위의 각 접근-수준 수식어는 괄호로 둘러싸인 `set` 키워드로 구성된 선택적으로 단일 인수를 허용합니다 (예를 들어 `private(set)`). [Getter 와 Setter (Getters and Setters)](../language-guide-1/access-control.md#getter-setter-getters-and-setters) 에서 설명 한대로 변수 또는 서브 스크립트에 대한 접근 수준보다 작거나 같은 변수 또는 서브 스크립트의 setter 에 대한 접근 수준을 지정하려면 이 형식의 접근-수준 수식어를 사용합니다.

> GRAMMAR OF A DECLARATION MODIFIER\
> declaration-modifier → `class` | `convenience` | `dynamic` | `final` | `infix` | `lazy` | `optional` | `override` | `postfix` | `prefix` | `required` | `static` | `unowned` | `unowned` `(` `safe` `)` | `unowned(` `unsafe` `)` | `weak`\
> declaration-modifier → [access-level-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_access-level-modifier)\
> declaration-modifier → [mutation-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_mutation-modifier)\
> declaration-modifiers → [declaration-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration-modifier) [declaration-modifiers](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar\_declaration-modifiers) opt\
> access-level-modifier → `private` | `private` `(` `set` `)`\
> access-level-modifier → `fileprivate` | `fileprivate` `(` `set` `)`\
> access-level-modifier → `internal` | `internal` `(` `set` `)`\
> access-level-modifier → `public` | `public` `(` `set` `)`\
> access-level-modifier → `open` | `open` `(` `set` `)`\
> mutation-modifier → `mutating` | `nonmutating`
