# 선언 (Declarations)

타입, 연산자, 변수, 등을 도입하고 작성합니다.

_선언 (declaration)_ 은 프로그램에 새로운 이름 또는 구성을 도입합니다. 예를 들어 함수와 메서드를 도입하기 위해, 변수와 상수를 도입하기 위해, 그리고 열거형, 구조체, 클래스, 그리고 프로토콜 타입을 정의하기 위해 선언을 사용합니다. 기존에 명명된 타입의 동작을 확장하기 위해 그리고 다른 곳에서 선언된 기호를 프로그램으로 가져오기 위해 선언을 사용할 수도 있습니다.

Swift 에서 대부분의 선언은 선언과 동시에 구현되거나 초기화 된다는 의미에서도 정의이기도 합니다. 이 말은 프로토콜은 멤버를 정의하지 않으므로 대부분 프로토콜 멤버는 선언만 있습니다. 편의상 그리고 Swift 에서 구별이 그다지 중요하지 않으므로 _선언 (declaration)_ 은 선언과 정의를 모두 포함합니다.

> Grammar of a declaration:
>
> *declaration* → *import-declaration* \
> *declaration* → *constant-declaration* \
> *declaration* → *variable-declaration* \
> *declaration* → *typealias-declaration* \
> *declaration* → *function-declaration* \
> *declaration* → *enum-declaration* \
> *declaration* → *struct-declaration* \
> *declaration* → *class-declaration* \
> *declaration* → *actor-declaration* \
> *declaration* → *protocol-declaration* \
> *declaration* → *initializer-declaration* \
> *declaration* → *deinitializer-declaration* \
> *declaration* → *extension-declaration* \
> *declaration* → *subscript-declaration* \
> *declaration* → *macro-declaration* \
> *declaration* → *operator-declaration* \
> *declaration* → *precedence-group-declaration* \
> *declarations* → *declaration* *declarations*_?_

## 최상위-수준 코드 (Top-Level Code)

Swift 소스 파일에서 최상위-수준 코드는 비어있거나 구문, 선언, 그리고 표현식으로 구성됩니다. 기본적으로 소스 파일의 최상위-수준에 선언된 변수, 상수, 그리고 다른 명명된 선언은 동일한 모듈의 일부인 모든 소스 파일 코드에 접근할 수 있습니다. [접근 제어 수준 (Access Control Levels)](declarations.md#access-control-levels) 에서 설명된대로 접근-수준 수정자 (access-level modifier) 로 선언을 표시하여 기본 동작을 재정의할 수 있습니다.

최상위-수준 선언 (top-level declarations) 과 실행 가능한 최상위-수준 코드 (executable top-level code) 인 최상위-수준 코드의 두가지 종류가 있습니다. 최상위-수준 선언은 선언으로만 구성되고 모든 Swift 소스 파일에서 허용됩니다. 실행 가능한 최상위-수준 코드는 선언 뿐만 아니라 구문과 표현식을 포함하고 프로그램에 대해 최상위-수준 진입점으로만 허용됩니다.

실행 가능하게 만들기 위해 컴파일 한 Swift 코드는 코드가 파일과 모듈로 구성되는 방식과 상관없이 최상위-수준 진입점을 나타내는 다음 접근 방식 중 하나만 포함할 수 있습니다: `main` 속성, `NSApplicationMain` 속성, `UIApplicationMain` 속성, `main.swift` 파일, 또는 최상위-수준 실행 가능한 코드를 포함하는 파일.

> Grammar of a top-level declaration:
>
> _top-level-declaration_ → _statements?_

## 코드 블럭 (Code Blocks)

_코드 블럭 (code block)_ 은 선언 및 제어 구조에서 구문을 그룹화 하기위해 사용됩니다. 다음의 형식을 가집니다:

```swift
{
   <#statements#>
}
```

코드 블럭 내에 _구문 (statements)_ 은 선언, 표현식, 그리고 구문의 다른 종류가 포함되고 소스 코드에 나타나는 순서대로 실행됩니다.

> Grammar of a code block:
>
> _code-block_ → **`{`** _statements?_ **`}`**

## 가져오기 선언 (Import Declaration)

_가져오기 선언 (import declaration)_ 을 사용하여 현재 파일 바깥에 선언된 기호에 접근할 수 있습니다. 기본 형식은 전체 모듈을 가져옵니다; `import` 키워드 다음에 모듈 이름이 따라오도록 구성됩니다:

```swift
import <#module#>
```

가져올 기호에 대한 자세한 제한을 제공하면 모듈 또는 하위 모듈 내에 특정 하위 모듈 또는 특정 선언을 지정할 수 있습니다. 이런 상세한 형식이 사용되면 선언한 모듈이 아닌 가져온 기호만 현재 범위에서 사용할 수 있습니다.

```swift
import <#import kind#> <#module#>.<#symbol name#>
import <#module#>.<#submodule#>
```

> Grammar of an import declaration:
>
> _import-declaration_ → _attributes?_ **`import`** _import-kind?_ _import-path_
>
> _import-kind_ → **`typealias`** | **`struct`** | **`class`** | **`enum`** | **`protocol`** | **`let`** | **`var`** | **`func`** \
> _import-path_ → _identifier_ | _identifier_ **`.`** _import-path_

## 상수 선언 (Constant Declaration)

_상수 선언 (constant declaration)_ 은 프로그램에 명명된 상수값을 도입합니다. 상수 선언은 `let` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

```swift
let <#constant name#>: <#type#> = <#expression#>
```

상수 선언은 _상수 이름 (constant name)_ 과 초기화 구문 _표현식 (expression)_ 의 값 사이의 변경 불가능한 바인딩을 정의합니다; 상수의 값이 설정된 후에 변경할 수 없습니다. 이 말은 상수가 클래스 객체로 초기화되면 이 객체 자체는 변경할 수 있지만 이 상수 이름과 참조하는 객체 간의 바인딩은 변경할 수 없습니다.

상수가 전역 범위로 선언되면 값으로 반드시 초기화 되어야 합니다. 상수 선언이 함수 또는 메서드의 컨텍스트에 나타나면 해당 값을 처음 읽기 전에 값을 설정한다는 보장이 있는 한 나중에 초기화 될 수 있습니다. 컴파일러가 상수의 값을 읽지 않는다는 것을 증명할 수 있으면 상수에 값을 설정하지 않아도 됩니다.
이 분석을 *확정 초기화 (definite initialization)* 라고 불립니다 ---
컴파일러는 읽기 전에 확실하게 값이 설정되어 있다는 것을 증명합니다.

> Note:
> 확정 초기화 (Definite initialization) 는
> 도메인 지식을 요구하는 증명을 구성할 수 없으며,
> 조건문에서 상태를 추적하는 것에는 제한이 있습니다.
> 상수에 항상 값이 있다고 결정할 수 있지만,
> 컴파일러가 이를 증명할 수 없는 경우에,
> 값을 설정하는 코드 경로를 단순화하거나,
> 변수 선언을 사용합니다.

상수 선언이 클래스 또는 구조체 선언의 컨텍스트에서 나타나면 _상수 프로퍼티 (constant property)_ 로 간주됩니다. 상수 선언은 계산된 프로퍼티가 아니므로 getter 또는 setter 를 가지지 않습니다.

상수 선언의 _상수 이름 (constant name)_ 이 튜플 패턴인 경우 튜플에서 각 항목의 이름은 초기화 구문 _표현식 (expression)_ 에서 해당 값으로 바인딩 됩니다.

```swift
let (firstNumber, secondNumber) = (10, 42)
```

이 예제에서 `firstNumber` 는 값 `10` 에 대한 명명된 상수이고 `secondNumber` 는 값 `42` 에 대한 명명된 상수 입니다. 두 상수 모두 독립적으로 사용할 수 있습니다:

```swift
print("The first number is \(firstNumber).")
// Prints "The first number is 10."
print("The second number is \(secondNumber).")
// Prints "The second number is 42."
```

타입 주석 (`:` _타입_) 은 [타입 추론 (Type Inference)](types.md#type-inference) 에서 설명한대로 _상수 이름 (constant name)_ 의 타입이 추론될 수 있을 때 상수 선언에서 선택사항입니다.

상수 타입 프로퍼티 (constant type property) 를 선언하려면 `static` 선언 수식어로 선언을 표시해야 합니다. 클래스의 상수 타입 프로퍼티는 암시적으로 final 입니다; `class` 또는 `final` 선언 수식어로 표시하여 하위 클래스에 의한 재정의를 허용하거나 금지할 수 있습니다. 타입 프로퍼티는 [타입 프로퍼티 (Type Properties)](../language-guide-1/properties.md#type-properties) 에 설명되어 있습니다.

상수에 대한 자세한 내용과 사용시 지침에 대한 내용은 [상수와 변수 (Constants and Variables)](../language-guide-1/the-basics.md#constants-and-variables) 와 [저장된 프로퍼티 (Stored Properties)](../language-guide-1/properties.md#stored-properties) 를 참고 바랍니다.

> Grammar of a constant declaration:
>
> _constant-declaration_ → _attributes?_ _declaration-modifiers?_ **`let`** _pattern-initializer-list_
>
> _pattern-initializer-list_ → _pattern-initializer_ | _pattern-initializer_ **`,`** _pattern-initializer-list_ \
> _pattern-initializer_ → _pattern_ _initializer?_ \
> _initializer_ → **`=`** _expression_

## 변수 선언 (Variable Declaration)

_변수 선언 (variable declaration)_ 은 프로그램에 명명된 변수 값을 도입하고 `var` 키워드를 사용하여 선언됩니다.

변수 선언은 명명된, 변경 가능한 값, 저장된 그리고 계산된 변수와 프로퍼티, 저장된 변수와 프로퍼티 관찰자, 그리고 정적 변수 프로퍼티의 다양한 종류의 선언 형식을 가집니다. 사용할 적절한 형식은 변수가 선언되는 범위와 선언하려는 변수의 종류에 따라 다릅니다.

> Note\
> [프로토콜 프로퍼티 선언 (Protocol Property Declaration)](declarations.md#protocol-property-declaration) 에서 설명한대로 프로토콜 선언의 컨텍스트에서 프로퍼티를 선언할 수도 있습니다.

[재정의 (Overriding)](../language-guide-1/inheritance.md#overriding) 에서 설명한대로 `override` 선언 수식어로 하위 클래스의 프로퍼티 선언을 표시하여 하위 클래스에 프로퍼티를 재정의 할 수 있습니다.

### 저장된 변수와 저장된 변수 프로퍼티 (Stored Variables and Stored Variable Properties)

다음의 형식으로 저장된 변수 (stored variable) 또는 저장된 변수 프로퍼티 (stored variable property) 를 선언할 수 있습니다:

```swift
var <#variable name#>: <#type#> = <#expression#>
```

이러한 형식의 변수 선언은 전역 범위, 함수의 지역 범위, 또는 클래스나 구조체 선언의 컨텍스트에서 정의합니다. 이러한 형식의 변수 선언이 전역 범위나 함수의 지역 범위로 선언되면 _저장된 변수 (stored variable)_ 로 참조됩니다. 클래스나 구조체 선언의 컨텍스트에서 선언되면 _저장된 변수 프로퍼티 (stored variable property)_ 로 참조됩니다.

초기화 구문 _표현식 (expression)_ 은 프로토콜 선언에 있을 수 없지만 다른 모든 컨텍스트에서 초기화 구문 _표현식 (expression)_ 은 선택사항입니다. 즉, 초기화 구문 _표현식 (expression)_ 이 없으면 변수 선언은 명시적 타입 주석 (`:` 타입) 을 포함해야 합니다.

상수 선언과 마찬가지로 _변수 이름 (variable name)_ 이 튜플 패턴인 경우 튜플에서 각 항목에 이름은 초기화 구문 _표현식 (expression)_ 에서 해당 값에 바인딩 됩니다.

이름에서 알 수 있듯이, 저장된 변수 또는 저장된 변수 프로퍼티의 값은 메모리에 저장됩니다.

### 계산된 변수와 계산된 프로퍼티 (Computed Variables and Computed Properties)

다음의 형식은 계산된 변수 (computed variable) 또는 계산된 프로퍼티 (computed property) 를 선언합니다:

```swift
var <#variable name#>: <#type#> {
   get {
      <#statements#>
   }
   set(<#setter name#>) {
      <#statements#>
   }
}
```

이러한 형식의 변수 선언은 전역 범위, 함수의 지역 범위, 또는 클래스, 구조체, 열거형, 또는 확장 선언의 컨텍스트에서 정의합니다. 이러한 형식의 변수 선언을 전역 범위 또는 함수의 지역 범위로 선언되면 _계산된 변수 (computed variable)_ 로 참조됩니다. 클래스, 구조체, 또는 확장 선언의 컨텍스트에서 선언되면 _계산된 프로퍼티 (computed property)_ 로 참조됩니다.

getter 는 값을 읽기위해 사용되고 setter 는 값을 작성하기 위해 사용됩니다. setter 절은 선택사항이며 getter 만 필요하다면 [읽기-전용 계산된 프로퍼티 (Read-Only Computed Properties)](../language-guide-1/properties.md#read-only-computed-properties) 에 설명한대로 getter 와 setter 절 모두 생략할 수 있고 간단하게 요구된 값을 직접적으로 반환할 수 있습니다. 그러나 setter 절을 제공하면 getter 절도 제공되어야 합니다.

_setter 이름 (setter name)_ 과 둘러싸인 소괄호는 선택사항입니다. setter 이름을 제공하면 setter 에 파라미터의 이름으로 사용됩니다. setter 이름을 제공하지 않으면 [짧은 Setter 선언 (Shorthand Setter Declaration)](../language-guide-1/properties.md#setter-shorthand-setter-declaration) 에서 설명한대로 setter 에 기본 파라미터 이름은 `newValue` 입니다.

저장된 명명된 값과 저장된 변수 프로퍼티와 다르게 계산된 명명된 값 또는 계산된 프로퍼티의 값은 메모리에 저장되지 않습니다.

계산된 프로퍼티의 자세한 내용과 예제는 [계산된 프로퍼티 (Computed Properties)](../language-guide-1/properties.md#computed-properties) 를 참고 바랍니다.

### 저장된 변수 관찰자와 프로퍼티 관찰자 (Stored Variable Observers and Property Observers)

`willSet` 과 `didSet` 관찰자를 저장된 변수 또는 프로퍼티에 선언할 수도 있습니다. 괄찰자와 함께 선언한 저장된 변수 또는 프로퍼티는 다음의 형식을 가집니다:

```swift
var <#variable name#>: <#type#> = <#expression#> {
   willSet(<#setter name#>) {
      <#statements#>
   }
   didSet(<#setter name#>) {
      <#statements#>
   }
}
```

전역 범위, 함수의 지역 범위, 또는 클래스나 구조체 선언의 컨텍스트에서 이 형식의 변수 선언으로 정의합니다. 이 형식의 변수 선언이 전역 범위 또는 함수의 지역 범위로 선언되면 관찰자는 _저장된 변수 관찰자 (stored variable observers)_ 로 참조됩니다. 클래스나 구조체 선언의 컨텍스트에서 선언되면 관찰자는 _프로퍼티 관찰자 (property observers)_ 로 참조됩니다.

모든 저장된 프로퍼티에 프로퍼티 관찰자를 추가할 수 있습니다. [프로퍼티 관찰자 재정의 (Overriding Property Observers)](../language-guide-1/inheritance.md#overriding-property-observers) 에서 설명한대로 하위 클래스 내에 프로퍼티 재정의로 저장된 또는 계산된 프로퍼티와 상관없이 모든 상속된 프로퍼티에 프로퍼티 관찰자를 추가할 수도 있습니다.

초기화 구문 _표현식 (expression)_ 은 클래스나 구조체 선언의 컨텍스트에서 선택사항 이지만 다른곳에선 필수입니다. 타입이 초기화 구문 _표현식 (expression)_ 에서 유추될 수 있으면 _타입 (type)_ 주석은 선택사항 입니다. 이 표현식은 프로퍼티의 값을 처음 읽을 때 평가됩니다. 프로퍼티의 초기값을 읽지않고 재작성하면 이 표현식은 프로퍼티에 처음 쓰기 전에 평가됩니다.

변수 또는 프로퍼티의 값이 설정되려고 하면 `willSet` 과 `didSet` 관찰자는 관찰 및 적절한 반응을 위한 방법을 제공합니다. 관찰자는 변수 또는 프로퍼티가 처음 초기화될 때는 호출되지 않습니다. 대신에 값이 초기화 컨텍스트 바깥에서 설정될 때만 호출됩니다.

`willSet` 관찰자는 변수 또는 프로퍼티의 값이 설정되기 전에만 호출됩니다. 새로운 값은 상수로 `willSet` 관찰자로 전달되고 `willSet` 절의 구현에서 변경할 수 없습니다. `didSet` 관찰자는 새로운 값이 설정된 후 바로 호출됩니다. `willSet` 관찰자와 달리 변수 또는 프로퍼티의 이전 값은 여전히 접근이 필요한 경우에 `didSet` 관찰자로 전달됩니다. 이 말은 `didSet` 관찰자 절 내에 변수 또는 프로퍼티에 값을 할당하면 할당한 새 값이 방금 설정되어 `willSet` 관찰자에 전달된 값을 대체합니다.

`willSet` 과 `didSet` 절에 _setter 이름 (setter name)_ 과 둘러싸인 소괄호는 선택사항 입니다. setter 이름을 제공한다면 `willSet` 과 `didSet` 관찰자의 파라미터 이름으로 사용됩니다. setter 이름을 제공하지 않는다면 `willSet` 관찰자에서 기본 파라미터 이름은 `newValue` 이고 `didSet` 관찰자에서 기본 파라미터 이름은 `oldValue` 입니다.

`willSet` 절을 제공하면 `didSet` 절은 선택사항 입니다. 마찬가지로 `willSet` 절은 `didSet` 절이 제공되면 선택사항 입니다.

`didSet` 관찰자의 본문은 이전 값을 참조하면 getter 는 이전 값이 사용 가능하도록 관찰자 전에 호출됩니다. 아래 예제는 상위 클래스에 의해 정의되고 관찰자를 추가하기 위해 하위 클래스에 의해 재정의 된 계산된 프로퍼티를 보여줍니다.

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

프로퍼티 관찰자를 사용하는 방법에 대한 자세한 내용과 예제는 [프로퍼티 관찰자 (Property Observers)](../language-guide-1/properties.md#property-observers) 를 참고 바랍니다.

### 타입 변수 프로퍼티 (Type Variable Properties)

타입 변수 프로퍼티 (type variable property) 를 선언하기 위해 `static` 선언 수식어와 함께 선언에 표시합니다. 클래스는 상위 클래스의 구현을 하위 클래스가 재정의 할 수 있도록 `class` 선언 수식어와 함께 타입 계산된 프로퍼티를 표시할 수 있습니다. 타입 프로퍼티는 [타입 프로퍼티 (Type Properties)](../language-guide-1/properties.md#type-properties) 에 설명되어 있습니다.

> Grammar of a variable declaration:
>
> _variable-declaration_ → _variable-declaration-head_ _pattern-initializer-list_ \
> _variable-declaration_ → _variable-declaration-head_ _variable-name_ _type-annotation_ _code-block_ \
> _variable-declaration_ → _variable-declaration-head_ _variable-name_ _type-annotation_ _getter-setter-block_ \
> _variable-declaration_ → _variable-declaration-head_ _variable-name_ _type-annotation_ _getter-setter-keyword-block_ \
> _variable-declaration_ → _variable-declaration-head_ _variable-name_ _initializer_ _willSet-didSet-block_ \
> _variable-declaration_ → _variable-declaration-head_ _variable-name_ _type-annotation_ _initializer?_ _willSet-didSet-block_
>
> _variable-declaration-head_ → _attributes?_ _declaration-modifiers?_ **`var`** \
> _variable-name_ → _identifier_
>
> _getter-setter-block_ → _code-block_ \
> _getter-setter-block_ → **`{`** _getter-clause_ _setter-clause?_ **`}`** \
> _getter-setter-block_ → **`{`** _setter-clause_ _getter-clause_ **`}`** \
> _getter-clause_ → _attributes?_ _mutation-modifier?_ **`get`** _code-block_ \
> _setter-clause_ → _attributes?_ _mutation-modifier?_ **`set`** _setter-name?_ _code-block_ \
> _setter-name_ → **`(`** _identifier_ **`)`**
>
> _getter-setter-keyword-block_ → **`{`** _getter-keyword-clause_ _setter-keyword-clause?_ **`}`** \
> _getter-setter-keyword-block_ → **`{`** _setter-keyword-clause_ _getter-keyword-clause_ **`}`** \
> _getter-keyword-clause_ → _attributes?_ _mutation-modifier?_ **`get`** \
> _setter-keyword-clause_ → _attributes?_ _mutation-modifier?_ **`set`**
>
> _willSet-didSet-block_ → **`{`** _willSet-clause_ _didSet-clause?_ **`}`** \
> _willSet-didSet-block_ → **`{`** _didSet-clause_ _willSet-clause?_ **`}`** \
> _willSet-clause_ → _attributes?_ **`willSet`** _setter-name?_ _code-block_ \
> _didSet-clause_ → _attributes?_ **`didSet`** _setter-name?_ _code-block_

## 타입 별칭 선언 (Type Alias Declaration)

_타입 별칭 선언 (type alias declaration)_ 은 프로그램에 기존 타입 명명된 별칭을 도입합니다. 타입 별칭 선언은 `typealias` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

```swift
typealias <#name#> = <#existing type#>
```

타입 별칭이 선언된 후 별칭된 _이름 (name)_ 은 프로그램에서 _기존 타입 (existing type)_ 대신에 사용될 수 있습니다. _기존 타입 (existing type)_ 은 명명된 타입 또는 복합 타입일 수 있습니다. 타입 별칭은 새로운 타입을 생성하지 않습니다; 간단하게 기존 타입을 참조하도록 이름을 허용합니다.

타입 별칭 선언은 기존 제너릭 타입에 이름을 제공하기 위해 제너릭 파라미터를 사용할 수 있습니다. 타입 별칭은 기존 타입의 일부 또는 모든 제너릭 파라미터에 대한 구체적인 타입을 제공할 수 있습니다:

```swift
typealias StringDictionary<Value> = Dictionary<String, Value>

// The following dictionaries have the same type.
var dictionary1: StringDictionary<Int> = [:]
var dictionary2: Dictionary<String, Int> = [:]
```

타입 별칭이 제너릭 파라미터로 선언되면 파라미터의 제약조건은 기존 타입의 제너릭 파라미터의 제약조건과 완벽하게 일치해야 합니다. 예를 들어:

```swift
typealias DictionaryOfInts<Key: Hashable> = Dictionary<Key, Int>
```

타입 별칭과 기존 타입은 서로 바꿔서 사용될 수 있으므로 타입 별칭은 추가로 제너릭 제약조건을 도입할 수 없습니다.

타입 별칭은 선언에서 모든 제너릭 파라미터를 생략하여 기존 타입의 제너릭 파라미터를 전달할 수 있습니다. 예를 들어 여기 선언한 `Diccionario` 타입 별칭은 `Dictionary` 와 동일한 제너릭 파라미터와 제약조건을 가집니다.

```swift
typealias Diccionario = Dictionary
```

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

타입 별칭이 없다면 `sum` 함수는 `T.Element` 대신에 `T.Iterator.Element` 로 연관된 타입을 참조해야 합니다.

[프로토콜 관련 타입 선언 (Protocol Associated Type Declaration)](declarations.md#protocol-associated-type-declaration) 을 참고 바랍니다.

> Grammar of a type alias declaration:
>
> _typealias-declaration_ → _attributes?_ _access-level-modifier?_ **`typealias`** _typealias-name_ _generic-parameter-clause?_ _typealias-assignment_ \
> _typealias-name_ → _identifier_ \
> _typealias-assignment_ → **`=`** _type_

## 함수 선언 (Function Declaration)

_함수 선언 (function declaration)_ 은 프로그램에 함수 또는 메서드를 도입합니다. 클래스, 구조체, 열거형, 또는 프로토콜의 컨텍스트에 선언된 함수는 _메서드 (method)_ 로 참조됩니다. 함수 선언은 `func` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

```swift
func <#function name#>(<#parameters#>) -> <#return type#> {
   <#statements#>
}
```

함수가 `Void` 의 반환 타입을 가지면 반환 타입은 다음과 같이 생략될 수 있습니다:

```swift
func <#function name#>(<#parameters#>) {
   <#statements#>
}
```

각 파라미터의 타입은 유추될 수 없으므로 반드시 도입되어야 합니다. 파라미터의 타입 앞에 `inout` 을 작성하면 파라미터는 함수의 범위 내에서 수정될 수 있습니다. In-out 파라미터는 아래 [In-Out 파라미터 (In-Out Parameters)](declarations.md#in-out-parameters) 에서 자세하게 설명되어 있습니다.

_구문 (statements)_ 에 단일 표현식만 포함된 함수 선언은 해당 표현식의 값을 반환하는 것으로 이해됩니다. 이 암시적 반환 구문은 표현식의 타입과 함수의 반환 타입이 `Void` 가 아니고 어떠한 케이스가 아닌 `Never` 와 같은 열거형이 아닌 경우에만 간주됩니다.

함수는 함수의 반환 타입으로 튜플 타입을 사용하여 여러값을 반환할 수 있습니다.

함수 정의는 다른 함수 선언 안에 나타날 수 있습니다. 이러한 함수를 _중첩 함수 (nested function)_ 라 합니다.

중첩 함수는 in-out 파라미터와 같이 절대 탈출되지 않는 값을 캡처하거나 비탈출 함수 인수로 전달된 경우 비탈출 입니다. 그렇지 않으면 중첩 함수는 탈출 함수 입니다.

중첩된 함수에 대한 내용은 [중첩 함수 (Nested Functions)](../language-guide-1/functions.md#nested-functions) 를 참고 바랍니다.

### 파라미터 이름 (Parameter Names)

함수 파라미터는 각 파라미터가 여러 형식 중 하나를 갖는 콤마로 구분된 리스트입니다. 함수 호출에서 인수의 순서는 함수의 선언 내에 파라미터의 순서와 일치해야 합니다. 파라미터 리스트에서 가장 간단한 형식은 다음과 같습니다:

```swift
<#parameter name#>: <#parameter type#>
```

파라미터는 함수 본문 내에서 사용되는 이름 뿐만 아니라 함수 또는 메서드 호출할 때 사용되는 인수 라벨이 있습니다. 기본적으로 파라미터 이름은 인수 라벨로도 사용됩니다. 예를 들어:

```swift
func f(x: Int, y: Int) -> Int { return x + y }
f(x: 1, y: 2) // both x and y are labeled
```

다음의 형식 중 하나로 인수 라벨의 기본 동작을 재정의 할 수 있습니다:

```swift
<#argument label#> <#parameter name#>: <#parameter type#>
_ <#parameter name#>: <#parameter type#>
```

파라미터 이름 전에 이름은 파라미터 이름과 다를 수 있는 명시적 인수 라벨을 파라미터에 제공합니다. 해당 인수는 함수 또는 메서드 호출에서 주어진 인수 라벨을 사용해야 합니다.

파라미터 이름 전에 언더바 (`_`) 는 인수 라벨을 숨깁니다. 해당 인수는 함수 또는 메서드 호출에서 라벨이 없어야 합니다.

```swift
func repeatGreeting(_ greeting: String, count n: Int) { /* Greet n times */ }
repeatGreeting("Hello, world!", count: 2) //  count is labeled, greeting is not
```

### 파라미터 수식어 (Parameter Modifiers)

*파라미터 수식어 (parameter modifier)* 는 함수에 전달된 인수를 변경합니다.

```swift
<#argument label#> <#parameter name#>: <#parameter modifier#> <#parameter type#>
```

파라미터 수식어를 사용하려면,
인수의 타입 전에
`inout`, `borrowing`, 또는 `consuming` 을 작성합니다.

```swift
func someFunction(a: inout A, b: consuming B, c: C) { ... }
```

#### In-Out 파라미터 (In-Out Parameters)

기본적으로, Swift 에서 함수 인수는 값으로 전달됩니다:
함수에서 수정된 것은 호출자에게 보여지지 않습니다.
대신 in-out 파라미터를 만드려면,
`inout` 파라미터 수식어를 적용합니다.

```swift
func someFunction(a: inout Int) {
    a += 1
}
```

in-out 파라미터를 포함하는 함수를 호출할 때,
인수의 값이 변경될 수 있는 함수 호출인 것을 나타내기위해
in-out 인수는 앰퍼샌드 (`&`) 를 앞에 붙여야 합니다.

```swift
var x = 7
someFunction(&x)
print(x)  // Prints "8"
```

In-out 파라미터는 다음과 같이 전달됩니다:

1. 함수가 호출될 때 인수의 값은 복사됩니다.
2. 함수의 본문 내에서 복사본은 수정됩니다.
3. 함수가 반환될 때 복사본의 값은 기존 인수에 할당됩니다.

이 동작은 _copy-in copy-out_ 또는 _call by value 결과_ 라고 합니다. 예를 들어 계산된 프로퍼티 또는 관찰자가 있는 프로퍼티가 in-out 파라미터로 전달되는 경우 getter 는 함수 호출의 부분으로 호출되고 setter 는 함수 반환의 부분으로 호출됩니다.

최적화로 인수가 메모리의 물리적 주소에 저장된 값인 경우 동일한 메모리 위치가 함수 본문 내부 및 외부에서 사용됩니다. 이런 최적화 동작을 _call by reference_ 라고 합니다; copy-in copy-out 모델의 모든 요구사항을 충족하는 동시에 복사의 오버헤드를 제거합니다. call-by-reference 최적화에 의존하지 않고 copy-in copy-out 에 의해 주어진 모델을 사용하여 작성하면 최적화에 상관없이 올바르게 작동되도록 합니다.

함수 내에서 기존 값이 현재 범위에서 사용가능 하더라도 in-out 인수로 전달된 값은 접근하면 안됩니다. 기존 값에 접근하는 것은 값에 대한 동시 접근이며 메모리 독점성을 위반합니다.

```swift
var someValue: Int
func someFunction(a: inout Int) {
    a += someValue
}

// Error: This causes a runtime exclusivity violation
someFunction(&someValue)
```

같은 이유로 여러개의 in-out 파라미터에 동일한 값을 전달할 수 없습니다.

```swift
var someValue: Int
func someFunction(a: inout Int, b: inout Int) {
    a += b
    b += 1
}

// Error: Cannot pass the same value to multiple in-out parameters
someFunction(&someValue, &someValue)
```

메모리 안정성과 메모리 독점성에 대한 자세한 내용은 [메모리 안정성 (Memory Safety)](../language-guide-1/memory-safety.md) 을 참고 바랍니다.

in-out 파라미터를 캡처하는 클로저 또는 중첩 함수는 비탈출 이어야 합니다. in-out 파라미터를 변경하지 않고 캡처 해야하는 경우 캡처 리스트를 사용하여 파라미터를 변경하지 않고 명시적으로 캡처해야 합니다.

```swift
func someFunction(a: inout Int) -> () -> Int {
    return { [a] in return a + 1 }
}
```

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

in-out 파라미터에 대한 자세한 설명과 예제는 [In-Out 파라미터 (In-Out Parameters)](../language-guide-1/functions.md#in-out-in-out-parameters) 를 참고 바랍니다.

#### 파라미터 차용과 소비 (Borrowing and Consuming Parameters)

기본적으로, Swift 는 일련의 규칙을 사용해,
함수 호출에서 객체의 생명주기를 자동으로 관리하고,
필요할 때 값을 복사합니다.
기본 규칙은 대부분 상황에서 오버헤드를 최소화 하도록 설계되어 있습니다 ---
특별한 제어를 원하면,
`borrowing` 또는 `consuming` 파라미터 수식어를 적용할 수 있습니다.
이 경우에,
복사 작업을 명시적으로 표시하려면 `copy` 를 사용합니다.

기본 규칙을 사용하는 것과 상관없이,
Swift 는 객체 생명주기와 소유권이
모든 상황에서 올바르게 관리되도록 보장합니다.
이 파라미터 수식어는 정확성이 아닌 특정 사용 패턴에
상대적 효율성에만 영향을 줍니다.

`borrowing` 수식어는 함수가
파라미터의 값을 유지하지 않음을 나타냅니다.
이 경우에, 호출자는 객체의 소유권과
객체의 생명주기에 대한 책임을 유지합니다.
`borrowing` 을 사용하여 함수가
객체를 일시적으로만 사용할 때 오버헤드를 최소화 합니다.

```swift
// `isLessThan` does not keep either argument
func isLessThan(lhs: borrowing A, rhs: borrowing A) -> Bool {
    ...
}
```

예를 들어, 전역 변수에 값을 저장하기 위해
함수가 파라미터의 값을 유지해야 하는 경우 ---
값을 명시적으로 복사하기 위해 `copy` 를 사용합니다.

```swift
// As above, but this `isLessThan` also wants to record the smallest value
func isLessThan(lhs: borrowing A, rhs: borrowing A) -> Bool {
    if lhs < storedValue {
        storedValue = copy lhs
    } else if rhs < storedValue {
        storedValue = copy rhs
    }
    return lhs < rhs
}
```

반대로,
`consuming` 파라미터 수식어는
함수가 값의 소유권을 가지고 있고,
함수가 반환하기 전에 값을 저장하거나 파기하는 책임이 있음을
나타냅니다.

```swift
// `store` keeps its argument, so mark it `consuming`
func store(a: consuming A) {
    someGlobalVariable = a
}
```

`consuming` 을 사용하면 호출자가 함수 호출 후에 더이상 객체를 사용할 필요가 없을 때
오버헤드를 최소화 합니다.

```swift
// Usually, this is the last thing you do with a value
store(a: value)
```

함수 호출 후에 복사가능한 객체 사용을 유지하려면,
컴파일러는 자동으로 함수 호출 전에 객체의 복사본을 만듭니다.

```swift
// The compiler inserts an implicit copy here
store(a: someValue)  // This function consumes someValue
print(someValue)  // This uses the copy of someValue
```

`inout` 과 다르게, `borrowing` 과
`consuming` 파라미터는 함수 호출할 때
특별한 표기법이 필요하지 않습니다:

```swift
func someFunction(a: borrowing A, b: consuming B) { ... }

someFunction(a: someA, b: someB)
```

`borrowing` 또는 `consuming` 를 명시적으로 사용하는 것은
런타임 소유권 관리의 오버헤드를 더 엄격하게 관리하려는
의도를 나타냅니다.
복사는 예기치 않은 런타임 소유권 동작을 야기할 수 있으므로,
명시적인 `copy` 키워드를 사용하지 않으면
복사할 수 없습니다:

```swift
func borrowingFunction1(a: borrowing A) {
    // Error: Cannot implicitly copy a
    // This assignment requires a copy because
    // `a` is only borrowed from the caller.
    someGlobalVariable = a
}

func borrowingFunction2(a: borrowing A) {
    // OK: Explicit copying works
    someGlobalVariable = copy a
}

func consumingFunction1(a: consuming A) {
    // Error: Cannot implicitly copy a
    // This assignment requires a copy because
    // of the following `print`
    someGlobalVariable = a
    print(a)
}

func consumingFunction2(a: consuming A) {
    // OK: Explicit copying works regardless
    someGlobalVariable = copy a
    print(a)
}

func consumingFunction3(a: consuming A) {
    // OK: No copy needed here because this is the last use
    someGlobalVariable = a
}
```

### 특별한 종류의 파라미터 (Special Kinds of Parameters)

파라미터는 무시될 수 있고 다음의 형식을 사용하여 가변의 값을 가지고 기본값을 제공할 수 있습니다:

```swift
_ : <#parameter type#>
<#parameter name#>: <#parameter type#>...
<#parameter name#>: <#parameter type#> = <#default argument value#>
```

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

`self` 를 수정하는 열거형 또는 구조체에 메서드는 `mutating` 선언 수식어로 표시되어야 합니다.

상위 클래스 메서드를 재정의한 메서드는 `override` 선언 수식어로 표시되어야 합니다. `override` 수식어 없이 메서드를 재정의하거나 상위 클래스 메서드를 재정의 하지 않는 메서드에 `override` 수식어를 사용하면 컴파일 에러가 발생합니다.

타입의 인스턴스가 아닌 타입과 관련된 메서드는 열거형과 구조체의 경우 `static` 선언 수식어나 클래스의 경우 `static` 또는 `class` 선언 수식어로 표시되어야 합니다. `class` 선언 수식어로 표시된 클래스 타입 메서드는 하위 클래스 구현에 의해 재정의 될 수 있습니다; `class final` 또는 `static` 으로 표시된 클래스 타입 메서드는 재정의 될 수 없습니다.

### 특별한 이름의 메서드 (Methods with Special Names)

특별한 이름을 가지는 메서드는 함수 호출 구문에 대해 구문 설탕 (syntactic sugar) 을 활성화 합니다. 이러한 메서드 중 하나로 타입을 정의하면 타입의 인스턴스는 함수 호출 구문에서 사용될 수 있습니다. 함수 호출은 해당 인스턴스에서 특별하게 명명된 메서드 중 하나로 호출됩니다.

클래스, 구조체, 또는 열거형 타입은 [dynamicCallable](attributes.md#dynamiccallable) 에서 설명 한대로 `dynamicallyCall(withArguments:)` 메서드 또는 `dynamicallyCall(withKeywordArguments:)` 메서드를 정의하거나 아래에서 설명 한대로 call-as-function 메서드를 정의하여 함수 호출 구문을 제공할 수 있습니다. 타입이 call-as-function 메서드와 `dynamicCallable` 속성에 의해 사용되는 메서드 중 하나를 모두 정의하면 컴파일러는 두 메서드를 사용할 수 있는 환경에서 call-as-function 메서드를 더 선호합니다.

call-as-function 메서드의 이름은 `callAsFunction()` 또는 `callAsFunction(` 으로 시작하고 라벨이 있거나 없는 인수를 추가하는 이름이 있습니다 — 예를 들어 `callAsFunction(_:_:)` 와 `callAsFunction(something:)` 도 call-as-function 메서드 이름으로 유효합니다.

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

call-as-function 메서드와 `dynamicCallable` 속성의 메서드는 타입 시스템으로 인코딩하는 정보의 양과 런타임에 가능한 동적 동작의 양 사이에 서로 다른 절충안을 만듭니다. call-as-function 메서드를 선언할 때 인수의 숫자와 각 인수의 타입과 라벨을 지정합니다. `dynamicCallable` 속성의 메서드는 인수의 배열을 보유하기 위해 사용된 타입만 지정합니다.

call-as-function 메서드 또는 `dynamicCallable` 속성의 메서드를 정의해도 함수 호출 표현식이 아닌 다른 컨텍스트에서 함수처럼 해당 타입의 인스턴스로 사용할 수 없습니다. 예를 들어:

```swift
let someFunction1: (Int, Int) -> Void = callable(_:scale:)  // Error
let someFunction2: (Int, Int) -> Void = callable.callAsFunction(_:scale:)
```

`subscript(dynamicMemberLookup:)` 서브 스크립트는 [dynamicMemberLookup](attributes.md#dynamicmemberlookup) 에서 설명 한대로 멤버 조회를 위한 구문 설탕을 활성화 합니다.

### 함수와 메서드 던지기 (Throwing Functions and Methods)

에러를 던질 수 있는 함수와 메서드는 `throws` 키워드로 표시되어야 합니다. 이러한 함수와 메서드는 _던지는 함수 (throwing functions)_ 와 _던지는 메서드 (throwing methods)_ 라고 합니다. 다음의 형식을 가집니다:

```swift
func <#function name#>(<#parameters#>) throws -> <#return type#> {
   <#statements#>
}
```

특정 에러 타입을 던지는 함수는 다음의 형식을 가집니다:

```swift
func <#function name#>(<#parameters#>) throws(<#error type#>) -> <#return type#> {
   <#statements#>
}
```

던지는 함수 또는 메서드 호출은 `try` 또는 `try!` 표현식 (`try` 또는 `try!` 연산자의 범위 내) 으로 래핑되어야 합니다.

함수의 타입은 에러를 던질 수 있고
무슨 타입의 에러를 던지는지 포함합니다.
예를 들어, 이러한 서브타입 관계는 에러를 던질 수 있는 컨텍스트에서
에러를 던지지 않는 함수를 사용할 수 있다는 의미입니다.
던지는 함수에 대한 자세한 내용은
[함수 타입 (Function Type)](./types.md#함수-타입-function-type) 을 참고 바랍니다.
명시적 타입을 가지는 에러 동작의 예제는
[에러 타입 지정 (Specifying the Error Type)](../language-guide-1/error-handling.md#에러-타입-지정-specifying-the-error-type) 을 참고 바랍니다.

함수가 에러를 발생할 수 있는지 여부를 기준으로 함수를 오버로드 할 수 없습니다. 이 말은 함수 파라미터 (parameter) 가 에러를 발생할 수 있는 여부에 따라 함수를 오버로드 할 수 있습니다.

던지는 메서드는 던지지 않는 메서드를 재정의할 수 없고 던지는 메서드는 던지지 않는 메서드에 대해 프로토콜 요구사항을 충족할 수 없습니다. 이 말은 던지지 않는 메서드는 던지는 메서드를 재정의할 수 있고 던지지 않는 함수는 던지는 메서드에 대한 프로토콜 요구사항을 충족할 수 있습니다.

### 다시 던지는 함수와 메서드 (Rethrowing Functions and Methods)

함수 또는 메서드는 함수 파라미터 중 하나만 에러를 발생하는 것을 나타내기 위해 `rethrows` 키워드로 선언될 수 있습니다. 이러한 함수와 메서드는 _다시 던지는 함수 (rethrowing functions)_ 와 _다시 던지는 메서드 (rethrowing methods)_ 라고 합니다. 다시 던지는 함수와 메서드는 적어도 하나의 던지는 함수 파라미터를 가져야 합니다.

```swift
func someFunction(callback: () throws -> Void) rethrows {
    try callback()
}
```

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

던지는 메서드는 다시 던지는 메서드를 재정의할 수 없고 던지는 메서드는 다시 던지는 메서드에 대한 프로토콜 요구사항을 충족할 수 없습니다. 이 말은 다시 던지는 메서드는 던지는 메서드를 재정의할 수 있고 다시 던지는 메서드는 던지는 메서드에 대해 프로토콜 요구사항을 충족할 수 있습니다.

다시 에러를 던지는 방법은 제너릭 코드에서 특정 에러 타입을 던지는 것입니다.
예를 들어:

```swift
func someFunction<E: Error>(callback: () throws(E) -> Void) throws(E) {
    try callback()
}
```

에러를 전파하는 이러한 방식은
에러에 대한 타입 정보를 유지합니다.
그러나, `rethrows` 함수와 다르게,
이 접근 방식은 동일한 타입의 에러를 던지도록
함수를 보호하지 않습니다.

### 비동기 함수와 메서드 (Asynchronous Functions and Methods)

비동기적으로 실행되는 함수와 메서드는 `async` 키워드로 표시되어야 합니다. 이 함수와 메서드는 _비동기 함수 (asynchronous functions)_ 와 _비동기 메서드 (asynchronous methods)_ 라고 합니다. 이 형태는 다음과 같습니다:

```swift
func <#function name#>(<#parameters#>) async -> <#return type#> {
   <#statements#>
}
```

비동기 함수 또는 메서드에 대한 호출은 `await` 표현식으로 래핑되어야 합니다 — 즉, `await` 연산자 범위 안에 있어야 합니다.

`async` 키워드는 함수 타입의 부분이고 동기 함수는 비동기 함수의 하위 타입입니다. 결과적으로 비동기 함수가 필요한 컨텍스트에서 동기 함수를 사용할 수 있습니다. 예를 들어 비동기 메서드를 동기 메서드로 재정의 할 수 있으며 동기 메서드는 비동기 메서드가 필요한 프로토콜 요구사항을 충족할 수 있습니다.

함수가 비동기인지 아닌지에 따라 함수를 오버로드 할 수 있습니다. 호출 부분에서 컨텍스트는 사용될 오버로드를 결정합니다: 비동기 컨텍스트에서는 비동기 함수가 사용되고 동기 컨텍스트에서는 동기 함수가 사용됩니다.

비동기 메서드는 동기 메서드를 재정의할 수 없으며 비동기 메서드는 동기 메서드에 대한 프로토콜 요구사항을 충족시킬 수 없습니다. 이 말은, 동기 메서드는 비동기 메서드를 재정의할 수 있으며 동기 메서드는 비동기 메서드에 대한 프로토콜 요구사항을 충족할 수 있습니다.

### 반환되지 않는 함수 (Functions that Never Return)

Swift 는 함수 또는 메서드가 호출자에게 반환하지 않음을 나타내는 [`Never`](https://developer.apple.com/documentation/swift/never) 타입을 정의합니다. `Never` 반환 타입이 있는 함수와 메서드는 _비반환 (nonreturning)_ 이라고 합니다. 비반환 함수와 메서드는 복구할 수 없는 에러를 발생하거나 무한으로 계속되는 작업을 시작합니다. 이것은 호출 직후 코드가 실행되지 않음을 뜻합니다. 던지고 다시 던지는 함수는 비반환인 경우에도 적절한 `catch` 블럭으로 프로그램 제어를 전송할 수 있습니다.

비반환 함수 또는 메서드는 [Guard 구문 (Guard Statement)](statements.md#guard-guard-statement) 에서 설명 한대로 guard 구문의 `else` 절을 끝낼 수 있습니다.

비반환 메서드를 재정의할 수 있지만 새로운 메서드는 반환 타입과 비반환 동작을 유지해야 합니다.

> Grammar of a function declaration:
>
> _function-declaration_ → _function-head_ _function-name_ _generic-parameter-clause?_ _function-signature_ _generic-where-clause?_ _function-body?_
>
> _function-head_ → _attributes?_ _declaration-modifiers?_ **`func`** \
> _function-name_ → _identifier_ | _operator_
>
> *function-signature* → *parameter-clause* **`async`**_?_ *throws-clause*_?_ *function-result*_?_ \
> _function-signature_ → _parameter-clause_ **`async`**_?_ **`rethrows`** _function-result?_ \
> _function-result_ → **`->`** _attributes?_ _type_ \
> _function-body_ → _code-block_
>
> _parameter-clause_ → **`(`** **`)`** | **`(`** _parameter-list_ **`)`** \
> _parameter-list_ → _parameter_ | _parameter_ **`,`** _parameter-list_ \
> *parameter* → *external-parameter-name*_?_ *local-parameter-name* *parameter-type-annotation* *default-argument-clause*_?_ \
> *parameter* → *external-parameter-name*_?_ *local-parameter-name* *parameter-type-annotation* \
> *parameter* → *external-parameter-name*_?_ *local-parameter-name* *parameter-type-annotation* **`...`**
> 
> _external-parameter-name_ → _identifier_ \
> _local-parameter-name_ → _identifier_ \
> *parameter-type-annotation* → **`:`** *attributes*_?_ *parameter-modifier*_?_ *type* \
> *parameter-modifier* → **`inout`** | **`borrowing`** | **`consuming`** \
> _default-argument-clause_ → **`=`** _expression_

## 열거형 선언 (Enumeration Declaration)

_열거형 선언 (enumeration declaration)_ 은 프로그램에 명명된 열거형 타입을 도입합니다.

열거형 선언은 두 가지 기본 형식을 가지고 있고 `enum` 키워드를 사용하여 선언됩니다. 두 형식 중 하나를 사용하여 선언된 열거형의 본문은 열거형 케이스 (enumeration cases) 라고 불리는 없거나 더 많은 값과 계산된 프로퍼티, 인스턴스 메서드, 타입 메서드, 초기화 구문, 타입 별칭, 그리고 다른 열거형, 구조체, 클래스 그리고 액터 선언을 포함합니다. 열거형 선언은 초기화 해제 구문 또는 프로토콜 선언을 포함할 수 없습니다.

열거형 타입은 여러 프로토콜을 채택할 수 있지만 클래스, 구조체, 또는 다른 열거형을 상속할 수 없습니다.

클래스와 구조체와 다르게 열거형 타입은 암시적으로 제공된 기본 초기화 구문을 가지지 않습니다; 모든 초기화 구문은 명시적으로 선언되어야 합니다. 초기화 구문은 열거형 내에 다른 초기화 구문으로 위임할 수 있지만 초기화 프로세스는 초기화 구문이 열거형 케이스 중 하나를 `self` 에 할당 한 후에만 완료됩니다.

구조체와 비슷하지만 클래스와 달리 열거형은 값 타입입니다; 열거형의 인스턴스는 변수 또는 상수에 할당될 때나 함수 호출에 인수로 전달될 때 복사됩니다. 값 타입에 대한 자세한 내용은 [구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)](../language-guide-1/structures-and-classes.md#structures-and-enumerations-are-value-types) 을 참고 바랍니다.

[확장 선언 (Extension Declaration)](declarations.md#extension-declaration) 에서 설명 한대로 확장 선언으로 열거형 타입의 동작을 확장할 수 있습니다.

### 모든 타입의 케이스가 있는 열거형 (Enumerations with Cases of Any Type)

다음의 형식은 모든 타입의 열거형 케이스를 포함하는 열거형 타입을 선언합니다:

```swift
enum <#enumeration name#>: <#adopted protocols#> {
    case <#enumeration case 1#>
    case <#enumeration case 2#>(<#associated value types#>)
}
```

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

연관된 값 타입을 사용하는 케이스의 자세한 내용과 예제는 [연관된 값 (Associated Values)](../language-guide-1/enumerations.md#associated-values) 을 참고 바랍니다.

#### 간접 열거형 (Enumerations with Indirection)

열거형은 재귀적 구조체를 가질 수 있습니다. 이 말은 열거형 타입 자체의 인스턴스 인 연관된 값을 사용하는 케이스를 가질 수 있다는 의미입니다. 그러나 열거형 타입의 인스턴스는 메모리에 고정된 레이아웃이 있음을 의미하는 값 의미를 가지고 있습니다. 재귀를 지원하려면 컴파일러는 간접 레이어를 삽입해야 합니다.

특정 열거형 케이스에 대해 간접을 사용하려면 `indirect` 선언 수식어로 표시해야 합니다. 간접 케이스는 연관된 값을 가지고 있어야 합니다.

```swift
enum Tree<T> {
    case empty
    indirect case node(value: T, left: Tree, right: Tree)
}
```

연관된 값을 가진 열거형의 모든 케이스에 대해 간접을 사용하려면 전체 열거형을 `indirect` 수식어로 표시합니다 — 이것은 열거형이 케이스 `indirect` 수식어를 표시해야 될 케이스를 많이 포함하는 경우 편리합니다.

`indirect` 수식어로 표시된 열거형은 연관된 값을 가진 케이스와 그렇지 않은 케이스를 포함할 수 있습니다. 이 말은 `indirect` 수식어로 표시된 케이스는 포함할 수 없다는 의미입니다.

### 원시값 타입의 케이스를 가진 열거형 (Enumerations with Cases of a Raw-Value Type)

다음의 형식은 동일한 기본 타입의 열거형 케이스를 포함하는 열거형 타입을 선언합니다:

```swift
enum <#enumeration name#>: <#raw-value type#>, <#adopted protocols#> {
    case <#enumeration case 1#> = <#raw value 1#>
    case <#enumeration case 2#> = <#raw value 2#>
}
```

이 형식에서 각 케이스 블럭은 `case` 키워드 다음에 콤마로 구분된 하나 이상의 열거형 케이스로 구성됩니다. 첫번째 형식의 케이스와 달리 각 케이스는 동일한 기본 타입의 _원시값 (raw value)_ 인 기본값을 가집니다. 이러한 값의 타입은 _원시-값 타입 (raw-value type)_ 으로 지정되고 정수, 부동 소수점 숫자, 문자열 또는 단일 문자로 표현되어야 합니다. 특히 _원시-값 타입 (raw-value type)_ 은 `Equatable` 프로토콜과 다음 프로토콜 중 하나를 준수해야 합니다:\
정수 리터럴에 대한 `ExpressibleByIntegerLiteral`,\
부동 소수점 리터럴에 대한 `ExpressibleByFloatLiteral`,\
문자의 모든 숫자를 포함하는 문자열 리터럴에 대한 `ExpressibleByStringLiteral`, 그리고 단일 문자만 포함하는 문자열 리터럴에 대한 `ExpressibleByUnicodeScalarLiteral` 또는 `ExpressibleByExtendedGraphemeClusterLiteral`. 각 케이스는 고유한 이름을 가지고 고유한 원시값이 할당 되어야 합니다.

원시-값 타입이 `Int` 로 지정되고 케이스에 명시적으로 값을 할당하지 않으면 암시적으로 값 `0`, `1`, `2`, 등으로 할당됩니다. 타입 `Int` 의 각 할당되지 않은 케이스는 이전 케이스의 원시값에서 자동으로 증가된 원시값으로 할당됩니다.

```swift
enum ExampleEnum: Int {
    case a, b, c = 5, d
}
```

위의 예제에서 `ExampleEnum.a` 의 원시값은 `0` 이고 `ExampleEnum.b` 의 값은 `1` 입니다. 그리고 `ExampleEnum.c` 의 값이 명시적으로 `5` 로 설정되어 있으므로 `ExampleEnum.d` 의 값은 `5` 에서 자동적으로 증가되어 `6` 입니다.

원시-값 타입이 `String` 으로 지정되고 명시적으로 케이스에 값을 할당하지 않으면 각 할당되지 않은 케이스는 암시적으로 케이스의 이름과 동일한 텍스트 문자열로 할당됩니다.

```swift
enum GamePlayMode: String {
    case cooperative, individual, competitive
}
```

위의 예제에서 `GamePlayMode.cooperative` 의 원시값은 `"cooperative"`, `GamePlayMode.individual` 의 원시값은 `"individual"`, 그리고 `GamePlayMode.competitive` 는 `"competitive"` 입니다.

암시적으로 원시-값 타입의 케이스를 가지는 열거형은 Swift 표준 라이브러리에 정의된 `RawRepresentable` 프로토콜을 준수합니다. 결과적으로 `rawValue` 프로퍼티를 가지고 있고 `init?(rawValue: RawValue)` 인 실패 가능한 초기화 구문을 가집니다. `ExampleEnum.b.rawValue` 처럼 열거형 케이스의 원시값에 접근하기 위해 `rawValue` 프로퍼티를 사용할 수 있습니다. 옵셔널 케이스를 반환하는 `ExampleEnum(rawValue: 5)` 와 같이 열거형의 실패 가능한 초기화 구문을 호출하여 해당 케이스를 찾기위해 원시값을 사용할 수도 있습니다. 원시-값 타입이 있는 케이스의 자세한 내용과 예제는 [원시값 (Raw Values)](../language-guide-1/enumerations.md#raw-values) 을 참고 바랍니다.

### 열거형 케이스 접근 (Accessing Enumeration Cases)

열거형 타입의 케이스를 참조하기 위해 `EnuerationType.enumerationCase` 와 같이 점 (`.`) 구문을 사용합니다. 열거형 타입이 컨텍스트에서 유추될 수 있으면 [열거형 구문 (Enumeration Syntax)](../language-guide-1/enumerations.md#enumeration-syntax) 과 [암시적 멤버 표현식 (Implicit Member Expression)](expressions.md#implicit-member-expression) 에서 설명 한대로 점은 그대로 유지한 채 생략할 수 있습니다.

열거형 케이스의 값을 검사하려면 [Switch 구문에서 열거형 값 일치 (Matching Enumeration Values with a Switch Statement)](../language-guide-1/enumerations.md#switch-matching-enumeration-values-with-switch-statement) 에서 봤듯이 `switch` 구문을 사용합니다. 열거형 타입은 [열거형 케이스 패턴 (Enumeration Case Pattern)](patterns.md#enumeration-case-pattern) 에서 설명 한대로 `switch` 구문의 케이스 블럭에서 열거형 케이스 패턴에 대해 일치합니다.

> Grammar of an enumeration declaration:
>
> _enum-declaration_ → _attributes?_ _access-level-modifier?_ _union-style-enum_ \
> _enum-declaration_ → _attributes?_ _access-level-modifier?_ _raw-value-style-enum_
>
> _union-style-enum_ → **`indirect`**_?_ **`enum`** _enum-name_ _generic-parameter-clause?_ _type-inheritance-clause?_ _generic-where-clause?_ **`{`** _union-style-enum-members?_ **`}`** \
> _union-style-enum-members_ → _union-style-enum-member_ _union-style-enum-members?_ \
> _union-style-enum-member_ → _declaration_ | _union-style-enum-case-clause_ | _compiler-control-statement_ \
> _union-style-enum-case-clause_ → _attributes?_ **`indirect`**_?_ **`case`** _union-style-enum-case-list_ \
> _union-style-enum-case-list_ → _union-style-enum-case_ | _union-style-enum-case_ **`,`** _union-style-enum-case-list_ \
> _union-style-enum-case_ → _enum-case-name_ _tuple-type?_ \
> _enum-name_ → _identifier_ \
> _enum-case-name_ → _identifier_
>
> _raw-value-style-enum_ → **`enum`** _enum-name_ _generic-parameter-clause?_ _type-inheritance-clause_ _generic-where-clause?_ **`{`** _raw-value-style-enum-members_ **`}`** \
> _raw-value-style-enum-members_ → _raw-value-style-enum-member_ _raw-value-style-enum-members?_ \
> _raw-value-style-enum-member_ → _declaration_ | _raw-value-style-enum-case-clause_ | _compiler-control-statement_ \
> _raw-value-style-enum-case-clause_ → _attributes?_ **`case`** _raw-value-style-enum-case-list_ \
> _raw-value-style-enum-case-list_ → _raw-value-style-enum-case_ | _raw-value-style-enum-case_ **`,`** _raw-value-style-enum-case-list_ \
> _raw-value-style-enum-case_ → _enum-case-name_ _raw-value-assignment?_ \
> _raw-value-assignment_ → **`=`** _raw-value-literal_ \
> _raw-value-literal_ → _numeric-literal_ | _static-string-literal_ | _boolean-literal_

## 구조체 선언 (Structure Declaration)

_구조체 선언 (structure declaration)_ 은 프로그램에 명명된 구조체 타입을 도입합니다. 구조체 선언은 `struct` 키워드를 사용하여 선언되고 아래의 형식을 가집니다:

```swift
struct <#structure name#>: <#adopted protocols#> {
   <#declarations#>
}
```

구조체의 본문은 _선언 (declarations)_ 이 없거나 많이 포함합니다. 이러한 _선언 (declarations)_ 은 저장된 프로퍼티와 계산된 프로퍼티, 타입 프로퍼티, 인스턴스 메서드, 타입 메서드, 초기화 구문, 서브 스크립트, 타입 별칭, 그리고 다른 구조체, 클래스, 액터 그리고 열거형 선언을 포함할 수 있습니다. 구조체 선언은 초기화 해제 구문 또는 프로토콜 선언을 포함할 수 없습니다. 여러 종류의 선언을 포함한 구조체에 대한 자세한 내용과 예제는 [구조체와 클래스 (Structures and Classes)](../language-guide-1/structures-and-classes.md) 를 참고 바랍니다.

구조체 타입은 여러 프로토콜을 채택할 수 있지만 클래스, 열거형, 또는 다른 구조체를 상속할 수 없습니다.

이전에 선언된 구조체의 인스턴스를 생성하는 방법은 세가지가 있습니다:

* [초기화 구문 (Initializers)](../language-guide-1/initialization.md#initializers) 에서 설명 한대로 구조체 내에 선언된 초기화 구문 중 하나를 호출해야 합니다.
* 선언된 초기화 구문이 없는 경우 [구조체 타입에 대한 멤버별 초기화 구문 (Memberwise Initializers for Structure Types)](../language-guide-1/initialization.md#memberwise-initializers-for-structure-types) 에서 설명 한대로 구조체의 멤버별 초기화 구문을 호출합니다.
* 선언된 초기화 구문이 없고 구조체 선언의 모든 프로퍼티에 초기값이 주어진 경우 [기본 초기화 구문 (Default Initializers)](../language-guide-1/initialization.md#default-initializers) 에서 설명 한대로 구조체의 기본 초기화 구문을 호출합니다.

구조체에 선언된 프로퍼티 초기화 프로세스는 [초기화 (Initialization)](../language-guide-1/initialization.md) 에 설명되어 있습니다.

구조체 인스턴스의 프로퍼티는 [프로퍼티 접근 (Accessing Properties)](../language-guide-1/structures-and-classes.md#accessing-properties) 에서 설명 한대로 점 (`.`) 구문을 사용하여 접근할 수 있습니다.

구조체는 값 타입 입니다; 변수나 상수에 할당될 때나 함수 호출에 대해 인수로 전달될 때 구조체의 인스턴스는 복사됩니다. 값 타입에 대한 자세한 내용은 [구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)](../language-guide-1/structures-and-classes.md#structures-and-enumerations-are-value-types) 을 참고 바랍니다.

[확장 선언 (Extension Declaration)](declarations.md#extension-declaration) 에서 설명 한대로 확장 선언으로 구조체 타입의 동작을 확장 할 수 있습니다.

> Grammar of a structure declaration:
>
> _struct-declaration_ → _attributes?_ _access-level-modifier?_ **`struct`** _struct-name_ _generic-parameter-clause?_ _type-inheritance-clause?_ _generic-where-clause?_ _struct-body_ \
> _struct-name_ → _identifier_ \
> _struct-body_ → **`{`** _struct-members?_ **`}`**
>
> _struct-members_ → _struct-member_ _struct-members?_ \
> _struct-member_ → _declaration_ | _compiler-control-statement_

## 클래스 선언 (Class Declaration)

_클래스 선언 (class declaration)_ 은 프로그램에 명명된 클래스 타입을 도입합니다. 클래스 선언은 `class` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

```swift
class <#class name#>: <#superclass#>, <#adopted protocols#> {
   <#declarations#>
}
```

클래스의 본문은 선언이 없거나 하나 이상의 _선언 (declarations)_ 을 포함합니다. 이러한 _선언 (declarations)_ 은 저장된 프로퍼티와 계산된 프로퍼티, 인스턴스 메서드, 타입 메서드, 초기화 구문, 하나의 초기화 해제 구문, 서브 스크립트, 타입 별칭, 그리고 다른 클래스, 구조체, 액터 그리고 열거형 선언을 포함할 수 있습니다. 클래스 선언은 프로토콜 선언을 포함할 수 없습니다. 여러종류의 선언을 포함하는 클래스의 자세한 설명과 예제는 [구조체와 클래스 (Structures and Classes)](../language-guide-1/structures-and-classes.md) 를 참고 바랍니다.

클래스 타입은 _상위 클래스 (superclass)_ 로 하나의 부모 클래스만 상속할 수 있지만 프로토콜은 여러개 채택할 수 있습니다. _상위 클래스 (superclass)_ 는 _클래스 이름 (class name)_ 과 콜론 다음에 첫번째로 나타나고 다음으로 _채택된 프로토콜 (adopted protocols)_ 이 나타납니다. 제너릭 클래스 (generic class) 는 다른 제너릭과 제너릭이 아닌 클래스를 상속할 수 있지만 제너릭이 아닌 클래스 (nongeneric class) 는 다른 제너릭이 아닌 클래스만 상속할 수 있습니다. 콜론 뒤에 상위 제너릭 클래스의 이름을 작성할 때 제너릭 파라미터 절을 포함하는 제너릭 클래스의 전체 이름을 포함해야 합니다.

[초기화 구문 선언 (Initializer Declaration)](declarations.md#initializer-declaration) 에서 설명 한대로 클래스는 지정된 초기화 구문 (designated initializers) 과 편의 초기화 구문 (convenience initializers) 을 가질 수 있습니다. 클래스의 지정된 초기화 구문은 모든 클래스의 선언된 프로퍼티가 초기화 되어야 하고 상위 클래스의 지정된 초기화 구문을 호출하기 전에 수행되어야 합니다.

클래스는 상위 클래스의 프로퍼티, 메서드, 서브 스크립트, 그리고 초기화 구문을 재정의 할 수 있습니다. 프로퍼티, 메서드, 서브 스크립트, 그리고 지정된 초기화 구문 재정의는 `override` 선언 수식어로 표시되어야 합니다.

하위 클래스가 상위 클래스의 초기화 구문 구현을 요구하려면 상위 클래스의 초기화 구문을 `required` 선언 수식어로 표시해야 합니다. 해당 초기화 구문의 하위 클래스의 구현도 `required` 선언 수식어로 표시되어야 합니다.

_상위 클래스 (superclass)_ 에 선언된 프로퍼티와 메서드가 현재 클래스에 의해 상속되더라도 _상위 클래스 (superclass)_ 에 선언된 지정된 초기화 구문은 [자동 초기화 구문 상속 (Automatic Initializer Inheritance)](../language-guide-1/initialization.md#automatic-initializer-inheritance) 에서 설명 한대로 하위 클래스가 조건이 충족될 때만 상속됩니다. Swift 클래스는 범용 기본 클래스는 상속하지 않습니다.

이전에 선언된 클래스의 인스턴스를 생성하는 두가지 방법이 있습니다:

* [초기화 구문 (Initializers)](../language-guide-1/initialization.md#initializers) 에서 설명 한대로 클래스 내에 선언된 초기화 구문 중 하나를 호출 합니다.
* 선언된 초기화 구문이 없고 클래스 선언에 모든 프로퍼티에 기본값이 주어진 경우 [기본 초기화 구문 (Default Initializers)](../language-guide-1/initialization.md#default-initializers) 에서 설명 한대로 클래스의 기본 초기화 구문을 호출합니다.

[프로퍼티 접근 (Accessing Properties)](../language-guide-1/structures-and-classes.md#accessing-properties) 에 설명 한대로 점 (`.`) 구문으로 클래스 인스턴스의 프로퍼티에 접근 할 수 있습니다.

클래스는 참조 타입입니다; 클래스의 인스턴스는 변수나 상수에 할당되거나 함수 호출에 인수로 전달될 때 복사가 아닌 참조됩니다. 참조 타입에 대한 자세한 내용은 [구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)](../language-guide-1/structures-and-classes.md#structures-and-enumerations-are-value-types) 을 참고 바랍니다.

[확장 선언 (Extension Declaration)](declarations.md#extension-declaration) 에서 설명 한대로 확장 선언으로 클래스 타입의 동작을 확장할 수 있습니다.

> Grammar of a class declaration:
>
> _class-declaration_ → _attributes?_ _access-level-modifier?_ **`final`**_?_ **`class`** _class-name_ _generic-parameter-clause?_ _type-inheritance-clause?_ _generic-where-clause?_ _class-body_ \
> _class-declaration_ → _attributes?_ **`final`** _access-level-modifier?_ **`class`** _class-name_ _generic-parameter-clause?_ _type-inheritance-clause?_ _generic-where-clause?_ _class-body_ \
> _class-name_ → _identifier_ \
> _class-body_ → **`{`** _class-members?_ **`}`**
>
> _class-members_ → _class-member_ _class-members?_ \
> _class-member_ → _declaration_ | _compiler-control-statement_

## 액터 선언 (Actor Declaration)

_액터 선언 (actor declaration)_ 은 프로그램에 액터 타입으로 명명되어 도입됩니다. 액터 선언은 `actor` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

```swift
actor <#actor name#>: <#adopted protocols#> {
    <#declarations#>
}
```

액터의 본문에는 0개 이상의 _선언 (declarations)_ 이 포함되어 있습니다. 이 선언은 저장된 프로퍼티와 계산된 프로퍼티, 인스턴스 메서드, 타입 메서드, 초기화 구문, 하나의 초기화 해제 구문, 서브 스크립트, 타입 별칭, 그리고 기타 클래스, 구조체, 그리고 열거형 선언을 모두 포함할 수 있습니다. 액터에 대한 설명과 몇가지 예제는 [액터 (Actors)](../language-guide-1/concurrency.md#actors) 를 참조 바랍니다.

액터 타입은 프로토콜을 채택할 수 있지만 클래스, 열거형, 구조체, 또는 다른 액터를 상속할 수 없습니다. 그러나 `@objc` 속성으로 표시된 액터는 암시적으로 `NSObjectProtocol` 프로토콜을 준수하고 `NSObject` 의 하위 타입으로 Objective-C 런타임에 노출됩니다.

이전에 선언된 액터의 인스턴스를 만드는 방법에는 두가지 방법이 있습니다:

* [초기화 구문 (Initializers)](../language-guide-1/initialization.md#initializers) 에 설명된 대로 액터 내에서 선언된 초기화 구문 중 하나를 호출합니다.
* 초기화 구문이 선언되지 않고 액터 선언의 모든 프로퍼티에 초기값이 제공된 경우 [기본 초기화 구문 (Default Initializers)](../language-guide-1/initialization.md#default-initializers) 에서 설명한대로 액터의 기본 초기화 구문을 호출합니다.

기본적으로 액터의 멤버는 해당 액터와 분리됩니다. 메서드의 본문이나 프로퍼티에 대한 getter 와 같은 코드는 해당 액터에서 실행됩니다. 액터 내의 코드는 해당 코드는 이미 동일한 액터에서 실행되고 있기 때문에 동기적으로 상호 작용할 수 있지만 액터 외부의 코드는 이 코드가 다른 액터에서 비동기적으로 코드를 실행하고 있음을 나타내기 위해 `await` 로 표시해야 합니다. 키 경로는 액터의 분리된 멤버를 참조할 수 없습니다. 분리된 액터 저장 프로퍼티 (Actor-isolated stored properties) 는 동기 함수에 in-out 파라미터로 전달할 수 있지만 비동기 함수에는 전달할 수 없습니다.

액터는 선언이 `nonisolated` 키워드로 표시된 분리되지 않은 멤버 (nonisolated members) 를 가질 수도 있습니다. 분리되지 않은 멤버는 액터 외부의 코드처럼 실행됩니다: 액터의 분리상태와 상호작용 할 수 없으며 호출자는 이를 사용할 때 `await` 로 표시하지 않습니다.

액터의 멤버는 분리되지 않거나 비동기 일때만 `@objc` 속성으로 표시될 수 있습니다.

액터의 선언된 프로퍼티를 초기화 하는 과정은 [초기화 (Initialization)](../language-guide-1/initialization.md) 에 설명되어 있습니다.

액터 인스턴스의 프로퍼티는 [프로퍼티 접근 (Accessing Properties)](../language-guide-1/structures-and-classes.md#accessing-properties) 에서 설명한대로 점 (`.`) 구문을 사용하여 접근될 수 있습니다.

액터는 참조 타입입니다; 액터의 인스턴스는 변수나 상수에 할당되거나 함수 호출에 인수로 전달될 때 복사되지 않고 참조됩니다. 참조 타입에 대한 자세한 내용은 [클래스는 참조 타입 (Classes Are Reference Types)](../language-guide-1/structures-and-classes.md#classes-are-reference-types) 을 참조 바랍니다.

[확장 선언 (Extension Declaration)](declarations.md#extension-declaration) 에서 설명한 대로 확장 선언을 사용하여 액터 타입의 동작을 확장할 수 있습니다.

> Grammar of an actor declaration:
>
> _actor-declaration_ → _attributes?_ _access-level-modifier?_ **`actor`** _actor-name_ _generic-parameter-clause?_ _type-inheritance-clause?_ _generic-where-clause?_ _actor-body_ \
> _actor-name_ → _identifier_ \
> _actor-body_ → **`{`** _actor-members?_ **`}`**
>
> _actor-members_ → _actor-member_ _actor-members?_ \
> _actor-member_ → _declaration_ | _compiler-control-statement_

## 프로토콜 선언 (Protocol Declaration)

_프로토콜 선언 (protocol declaration)_ 은 프로그램에 명명된 프로토콜 타입을 도입합니다.
프로토콜 선언은 `protocol` 키워드를 사용하여
전역에 선언되고 다음의 형식을 가집니다:

```swift
protocol <#protocol name#>: <#inherited protocols#> {
   <#protocol member declarations#>
}
```

프로토콜 선언은 전역이나
제너릭이 아닌 타입이나 제너릭이 아닌 함수 내에 중첩되어 나타날 수 있습니다.

프로토콜의 본문은 프로토콜을 채택하는 모든 타입이 충족해야 하는 준수성 요구사항을 설명하는 _프로토콜 멤버 선언 (protocol member declarations)_ 이 없거나 하나 이상의 _프로토콜 멤버 선언 (protocol member declarations)_ 을 포함합니다. 특히 프로토콜은 준수하는 타입이 특정 프로퍼티, 메서드, 초기화 구문, 그리고 서브 스크립트를 구현해야 한다고 선언할 수 있습니다. 프로토콜은 프로토콜의 여러 선언의 관계를 지정할 수 있는 _연관된 타입 (associated types)_ 이라는 특별한 종류의 타입 별칭도 선언할 수 있습니다. 프로토콜 선언은 클래스, 구조체, 열거형, 또는 다른 프로토콜 선언을 포함할 수 없습니다. _프로토콜 멤버 선언 (protocol member declarations)_ 은 아래 자세하게 설명되어 있습니다.

프로토콜 타입은 다른 프로토콜을 상속할 수 있습니다. 프로토콜 타입이 다른 프로토콜을 상속할 때 다른 프로토콜의 요구사항이 집계되고 현재 프로토콜에서 상속한 모든 타입은 모든 요구사항을 준수해야 합니다. 프로토콜 상속 사용법에 대한 예제는 [프로토콜 상속 (Protocol Inheritance)](../language-guide-1/protocols.md#protocol-inheritance) 을 참고 바랍니다.

> Note\
> [프로토콜 구성 타입 (Protocol Composition Type)](types.md#protocol-composition-type) 과 [프로토콜 구성 (Protocol Composition)](../language-guide-1/protocols.md#protocol-composition) 에서 설명 한대로 프로토콜 구성 타입을 사용하여 여러개 프로토콜의 준수성 요구사항을 집계할 수도 있습니다.

해당 타입에 확장 선언에서 프로토콜을 채택하기 위해 이전에 선언된 타입에 프로토콜 준수를 추가할 수 있습니다. 확장에서 채택된 프로토콜의 모든 요구사항을 구현해야 합니다. 타입이 이미 모든 요구사항을 구현한 경우 빈 확장 선언의 본문으로 남겨둘 수 있습니다.

기본적으로 프로토콜을 준수하는 타입은 프로토콜에 선언된 모든 프로퍼티, 메서드, 그리고 서브 스크립트를 구현해야 합니다. 이 말은 준수하는 타입에 의한 구현이 옵셔널로 지정하기 위해선 `optional` 선언 수식어로 프로토콜 멤버 선언을 표시해야 한다는 의미입니다. `optional` 수식어는 `objc` 속성으로 표시된 멤버와 `objc` 속성으로 표시된 프로토콜의 멤버에만 적용될 수 있습니다. 결과적으로 클래스 타입만 옵셔널 멤버 요구사항을 포함한 프로토콜을 채택하고 준수할 수 있습니다. optional 선언 수식어 사용에 대한 자세한 내용과 옵셔널 프로토콜 멤버에 어떻게 접근하는지에 대한 가이드 — 예를 들어 타입이 이를 구현하는지 확실하지 않은 경우 — 는 [옵셔널 프로토콜 요구사항 (Optional Protocol Requirements)](../language-guide-1/protocols.md#optional-protocol-requirements) 을 참고 바랍니다.

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

클래스 타입에 대해서만 프로토콜의 채택을 제한하려면 콜론 뒤에 _상속된 프로토콜 (inherited protocols)_ 리스트에 `AnyObject` 프로토콜을 추가해야 합니다. 예를 들어 다음의 프로토콜은 클래스 타입에 의해서만 채택될 수 있습니다:

```swift
protocol SomeProtocol: AnyObject {
    /* Protocol members go here */
}
```

`AnyObject` 요구사항으로 표시된 프로토콜을 상속하는 모든 프로토콜은 마찬가지로 클래스 타입에 의해서만 채택될 수 있습니다.

> Note\
> `objc` 속성으로 표시된 프로토콜은 해당 프로토콜에 암시적으로 `AnyObject` 요구사항이 적용됩니다; 명시적으로 `AnyObject` 요구사항을 프로토콜에 표시할 필요가 없습니다.

프로토콜은 명명된 타입 이므로 [타입으로의 프로토콜 (Protocols as Types)](../language-guide-1/protocols.md#protocols-as-types) 에서 설명 한대로 다른 명명된 타입으로 코드의 동일한 위치에 나타날 수 있습니다. 그러나 프로토콜은 실제로 지정하는 요구사항에 대해 구현을 제공하지 않으므로 프로토콜의 인스턴스를 구성할 수 없습니다.

[위임 (Delegation)](../language-guide-1/protocols.md#delegation) 에서 설명 한대로 프로토콜을 사용하여 클래스 또는 구조체의 위임이 구현해야 하는 메서드를 선언할 수 있습니다.

> Grammar of a protocol declaration:
>
> _protocol-declaration_ → _attributes?_ _access-level-modifier?_ **`protocol`** _protocol-name_ _type-inheritance-clause?_ _generic-where-clause?_ _protocol-body_ \
> _protocol-name_ → _identifier_ \
> _protocol-body_ → **`{`** _protocol-members?_ **`}`**
>
> _protocol-members_ → _protocol-member_ _protocol-members?_ \
> _protocol-member_ → _protocol-member-declaration_ | _compiler-control-statement_
>
> _protocol-member-declaration_ → _protocol-property-declaration_ \
> _protocol-member-declaration_ → _protocol-method-declaration_ \
> _protocol-member-declaration_ → _protocol-initializer-declaration_ \
> _protocol-member-declaration_ → _protocol-subscript-declaration_ \
> _protocol-member-declaration_ → _protocol-associated-type-declaration_ \
> _protocol-member-declaration_ → _typealias-declaration_

### 프로토콜 프로퍼티 선언 (Protocol Property Declaration)

프로토콜은 프로토콜 선언 본문에 _프로토콜 프로퍼티 선언 (protocol property declaration)_ 을 포함하여 준수 타입이 프로퍼티를 구현해야 된다고 선언합니다. 프로토콜 프로퍼티 선언은 변수 선언에 특별한 형식을 가집니다:

```swift
var <#property name#>: <#type#> { get set }
```

다른 프로토콜 멤버 선언과 마찬가지로 이러한 프로퍼티 선언은 프로토콜을 준수하는 타입에 대한 getter 와 setter 요구사항만 선언합니다. 결과적으로 선언된 프로토콜 내에 직접적으로 getter 와 setter 를 구현할 수 없습니다.

getter 와 setter 요구사항은 다양한 방법으로 준수하는 타입으로 충족될 수 있습니다. 프로퍼티 선언이 `get` 과 `set` 키워드를 모두 포함하면 준수하는 타입은 저장된 변수 프로퍼티나 getter 와 setter 모두 구현하는 속성과 같이 읽고 쓰기가 모두 가능한 계산된 프로퍼티로 구현할 수 있습니다. 그러나 프로퍼티 선언은 상수 프로퍼티나 읽기-전용 계산된 프로퍼티로 구현될 수 없습니다. 프로퍼티 선언이 `get` 키워드만 포함한다면 모든 종류의 프로퍼티로 구현할 수 있습니다. 프로토콜의 프로퍼티 요구사항을 준수하는 타입의 예제는 [프로퍼티 요구사항 (Property Requirements)](../language-guide-1/protocols.md#property-requirements) 을 참고 바랍니다.

프로토콜 선언에 타입 프로퍼티 요구사항을 선언하려면 `static` 키워드로 프로퍼티 선언을 표시합니다. 프로토콜을 준수하는 구조체와 열거형은 `static` 키워드로 프로퍼티를 선언하고 프로토콜을 준수하는 클래스는 `static` 또는 `class` 키워드로 프로퍼티를 선언합니다. 구조체, 열거형, 또는 클래스에 프로토콜 준수를 추가하는 확장은 확장하는 타입과 동일한 키워드를 사용합니다. 타입 프로퍼티 요구사항에 대해 기본 구현을 제공하는 확장은 `static` 키워드를 사용합니다.

[변수 선언 (Variable Declaration)](declarations.md#variable-declaration) 도 참고 바랍니다.

> Grammar of a protocol property declaration:
>
> _protocol-property-declaration_ → _variable-declaration-head_ _variable-name_ _type-annotation_ _getter-setter-keyword-block_

### 프로토콜 메서드 선언 (Protocol Method Declaration)

프로토콜은 준수하는 타입이 프로토콜 선언 본문에 프로토콜 메서드 선언을 포함하기 위해 메서드를 구현해야 함을 선언합니다. 프로토콜 메서드 선언은 두가지를 제외하고 함수 선언과 동일한 형식을 가집니다: 함수 본문과 함수 선언의 일부로 기본 파라미터 값을 제공할 수 없습니다. 프로토콜의 메서드 요구사항을 구현하는 준수하는 타입에 대한 예제는 [메서드 요구사항 (Method Requirements)](../language-guide-1/protocols.md#method-requirements) 을 참고 바랍니다.

프로토콜 선언에 클래스 또는 정적 메서드 요구사항을 선언하려면 `static` 선언 수식어로 메서드 선언을 표시합니다. 프로토콜을 준수하는 구조체와 열거형은 `static` 키워드로 메서드를 선언하고 프로토콜을 준수하는 클래스는 `static` 또는 `class` 키워드로 메서드를 선언합니다. 구조체, 열거형, 또는 클래스에 프로토콜 준수를 추가하는 확장은 확장하는 타입과 동일한 키워드를 사용합니다. 타입 메서드 요구사항에 대한 기본 구현을 제공하는 확장은 `static` 키워드를 사용합니다.

[함수 선언 (Function Declaration)](declarations.md#function-declaration) 도 참고 바랍니다.

> Grammar of a protocol method declaration:
>
> _protocol-method-declaration_ → _function-head_ _function-name_ _generic-parameter-clause?_ _function-signature_ _generic-where-clause?_

### 프로토콜 초기화 구문 선언 (Protocol Initializer Declaration)

프로토콜은 준수하는 타입이 프로토콜 선언 본문에 프로토콜 초기화 구문 선언을 포함하기 위해 초기화 구문 구현을 선언합니다. 프로토콜 초기화 구문 선언은 초기화 구문의 본문을 포함하지 않는다는 점을 제외하고 초기화 구문 선언과 동일한 형식을 가집니다.

준수하는 타입은 실패할 수 없는 초기화 구문 (nonfailable initializer) 또는 `init!` 실패 가능한 초기화 구문 (failable initializer) 을 구현하여 실패할 수 없는 프로토콜 초기화 구문 요구사항을 충족할 수 있습니다. 준수하는 타입은 모든 종류의 초기화 구문 구현으로 실패 가능한 프로토콜 초기화 구문 요구사항을 충족할 수 있습니다.

클래스는 프로토콜의 초기화 구문 요구사항을 충족하기 위해 초기화 구문을 구현할 때 초기화 구문이 `final` 선언 수식어로 표시되어 있지 않다면 `required` 선언 수식어로 표시되어야 합니다.

[초기화 구문 선언 (Initializer Declaration)](declarations.md#initializer-declaration) 도 참고 바랍니다.

> Grammar of a protocol initializer declaration:
>
> _protocol-initializer-declaration_ → _initializer-head_ _generic-parameter-clause?_ _parameter-clause_ **`throws`**_?_ _generic-where-clause?_ \
> _protocol-initializer-declaration_ → _initializer-head_ _generic-parameter-clause?_ _parameter-clause_ **`rethrows`** _generic-where-clause?_

### 프로토콜 서브 스크립트 선언 (Protocol Subscript Declaration)

프롵콜은 준수하는 타입이 프로토콜 선언 본문에 프로토콜 서브 스크립트 선언을 포함하기 위해 서브 스크립트 구현을 선언합니다. 프로토콜 서브 스크립트 선언은 서브 스크립트 선언의 특별한 형식을 가집니다:

```swift
subscript (<#parameters#>) -> <#return type#> { get set }
```

서브 스크립트 선언은 프로토콜을 준수하는 타입에 대해 최소한의 getter 와 setter 구현 요구사항만 선언합니다. 서브 스크립트 선언이 `get` 과 `set` 키워드 모두 포함한다면 준수하는 타입은 getter 와 setter 절 모두 구현해야 합니다. 서브 스크립트 선언이 `get` 키워드만 포함한다면 준수하는 타입은 _적어도_ getter 절은 구현해야 하고 선택적으로 setter 절을 구현할 수 있습니다.

프로토콜 선언에서 정적 서브 스크립트 요구사항을 선언하려면 `static` 선언 수식어로 서브 스크립트 선언을 표시합니다. 프로토콜은 준수하는 구조체와 열거형은 `static` 키워드로 서브 스크립트를 선언하고 프로토콜을 준수하는 클래스는 `static` 또는 `class` 키워드로 서브 스크립트를 선언합니다. 구조체, 열거형, 또는 클래스에 프로토콜 준수를 추가하는 확장은 확장하는 타입과 동일한 키워드를 사용합니다. 정적 서브 스크립트 요구사항에 대한 기본 구현을 제공하는 확장은 `static` 키워드를 사용합니다.

[서브 스크립트 선언 (Subscript Declaration)](declarations.md#subscript-declaration) 도 참고 바랍니다.

> Grammar of a protocol subscript declaration:
>
> _protocol-subscript-declaration_ → _subscript-head_ _subscript-result_ _generic-where-clause?_ _getter-setter-keyword-block_

### 프로토콜 연관된 타입 선언 (Protocol Associated Type Declaration)

프로토콜은 `associatedtype` 키워드를 사용하여 연관된 타입을 선언합니다. 연관된 타입은 프로토콜 선언의 일부분으로 사용되는 타입에 대한 별칭을 제공합니다. 연관된 타입은 제너릭 파라미터 절에서 타입 파라미터와 유사하지만 선언된 프로토콜에서 `Self` 와 연관됩니다. 해당 컨텍스트에서 `Self` 는 프로토콜을 준수하는 최종 타입을 참조합니다. 더 자세한 내용과 예제는 [연관된 타입 (Associated Types)](../language-guide-1/generics.md#associated-types) 을 참고 바랍니다.

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

[타입 별칭 선언 (Type Alias Declaration)](declarations.md#type-alias-declaration) 도 참고 바랍니다.

> Grammar of a protocol associated type declaration:
>
> _protocol-associated-type-declaration_ → _attributes?_ _access-level-modifier?_ **`associatedtype`** _typealias-name_ _type-inheritance-clause?_ _typealias-assignment?_ _generic-where-clause?_

## 초기화 구문 선언 (Initializer Declaration)

_초기화 구문 선언 (Initializer declaration)_ 은 프로그램에 클래스, 구조체, 또는 열거형에 대한 초기화 구문을 도입합니다. 초기화 구문 선언은 `init` 키워드를 사용하여 선언되고 두 개의 기본 형식을 가지고 있습니다.

구조체, 열거형, 그리고 클래스 타입은 여러개의 초기화 구문을 가질 수 있지만 클래스 초기화 구문에 대한 규칙과 관련 동작은 다릅니다. 구조체와 열거형과 다르게 클래스는 두 종류의 초기화 구문을 가집니다: [초기화 (Initialization)](../language-guide-1/initialization.md) 에서 설명 한대로 지정된 초기화 구문 (designated initializers) 과 편의 초기화 구문 (convenience initializers) 이 있습니다.

다음 형식은 구조체, 열거형, 그리고 클래스의 지정된 초기화 구문에 대한 초기화 구문을 선언합니다:

```swift
init(<#parameters#>) {
   <#statements#>
}
```

클래스의 지정된 초기화 구문은 모든 클래스의 프로퍼티를 직접적으로 초기화 합니다. 같은 클래스의 다른 초기화 구문에서 호출할 수 없고 상위 클래스를 가지고 있다면 상위 클래스의 지정된 초기화 구문 중 하나를 호출해야 합니다. 클래스가 상위 클래스에서 프로퍼티를 상속하면 상위 클래스의 지정된 초기화 구문 중 하나는 현재 클래스에서 해당 프로퍼티에 설정되거나 수정되기 전에 호출되어야 합니다.

지정된 초기화 구문은 클래스 선언의 컨텍스트에서만 선언될 수 있으므로 확장 선언을 사용하는 클래스에 추가될 수 없습니다.

구조체와 열거형에 초기화 구문은 선언된 다른 초기화 구문을 호출하여 초기화 프로세스의 일부 또는 전체를 위임할 수 있습니다.

클래스에 편의 초기화 구문을 선언하려면 `convenience` 선언 수식어로 초기화 구문 선언을 표시합니다.

```swift
convenience init(<#parameters#>) {
   <#statements#>
}
```

편의 초기화 구문은 초기화 프로세스를 다른 편의 초기화 구문 또는 클래스의 지정된 초기화 구문 중 하나로 위임할 수 있습니다. 이 말은 초기화 프로세스는 궁극적으로 클래스의 프로퍼티를 초기화 하는 지정된 초기화 구문을 호출로 끝나야 한다는 의미입니다. 편의 초기화 구문은 상위 클래스의 초기화 구문을 호출할 수 없습니다.

모든 하위 클래스에 초기화 구문 구현을 요청하기 위해 `required` 선언 수식어로 지정된 초기화 구문과 편의 초기화 구문을 표시할 수 있습니다. 해당 초기화 구문의 하위 클래스의 구현도 `required` 선언 수식어로 표시되어야 합니다.

기본적으로 상위 클래스에 선언된 초기화 구문은 하위 클래스에 의해 상속되지 않습니다. 이 말은 하위 클래스는 기본값으로 모든 저장된 프로퍼티를 초기화 하고 자체 초기화 구문을 정의하지 않으면 모든 상위 클래스의 초기화 구문을 상속합니다. 하위 클래스가 모든 상위 클래스의 지정된 초기화 구문을 재정의하면 상위 클래스의 편의 초기화 구문을 상속합니다.

메서드, 프로퍼티, 그리고 서브 스크립트와 마찬가지로 `override` 선언 수식어로 재정의된 지정된 초기화 구문을 표시해야 합니다.

> Note\
> `required` 선언 수식어로 초기화 구문을 표시하면 하위 클래스에서 필수 초기화 구문을 재정의 할 때 `override` 수식어로 초기화 구문을 표시하지 않아도 됩니다.

함수와 메서드처럼 초기화 구문은 에러를 던지거나 다시 던질 수 있습니다. 그리고 함수와 메서드처럼 적절한 동작을 나타내기 위해 초기화 구문의 파라미터 뒤에 `throws` 또는 `rethrows` 키워드를 사용합니다. 마찬가지로 초기화 구문은 비동기일 수 있으며 이것을 나타내기위해 `async` 키워드를 사용합니다.

여러 타입 선언에서 초기화 구문의 예제는 [초기화 (Initialization)](../language-guide-1/initialization.md) 를 참고 바랍니다.

### 실패 가능한 초기화 구문 (Failable Initializers)

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

결과의 선택성을 다루는 것을 제외하고 실패없는 초기화 구문 호출과 동일하게 실패 가능한 초기화 구문 `init?` 을 호출합니다.

```swift
if let actualInstance = SomeStruct(input: "Hello") {
    // do something with the instance of 'SomeStruct'
} else {
    // initialization of 'SomeStruct' failed and the initializer returned 'nil'
}
```

실패 가능한 초기화 구문은 초기화 구문의 본문의 구현에 어느 위치에서든 `nil` 을 반환할 수 있습니다.

실패 가능한 초기화 구문은 여러 종류의 초기화 구문에 위임할 수 있습니다. 실패없는 초기화 구문은 다른 실패없는 초기화 구문이나 `init!` 실패 가능한 초기화 구문으로 위임할 수 있습니다. 실패없는 초기화 구문은 예를 들어 `super.init()!` 처럼 상위 클래스의 초기화 구문에 강제 언래핑한 결과로 `init?` 실패 가능한 초기화 구문으로 위임할 수 있습니다.

초기화 실패는 초기화 구문 위임으로 전파됩니다. 특히, 실패 가능한 초기화 구문이 실패하고 `nil` 을 반환하는 초기화 구문에 위임하면 위임된 초기화 구문도 실패하고 암시적으로 `nil` 을 반환합니다. 실패없는 초기화 구문이 실패하고 `nil` 을 반환하는 `init!` 실패 가능한 초기화 구문에 위임하면 `nil` 값을 가지는 옵셔널을 언래핑 하기위해 `!` 연산자를 사용하는 것과 같이 런타임 에러가 발생합니다.

실패 가능한 지정된 초기화 구문은 모든 종류의 지정된 초기화 구문에 의해 하위 클래스에서 재정의 될 수 있습니다. 실패없는 지정된 초기화 구문은 실패없는 지정된 초기화 구문에 의해서만 하위 클래스에서 재정의 될 수 있습니다.

실패 가능한 초기화 구문에 대한 자세한 내용과 예제는 [실패 가능한 초기화 구문 (Failable Initializers)](../language-guide-1/initialization.md#failable-initializers) 을 참고 바랍니다.

> Grammar of an initializer declaration:
>
> _initializer-declaration_ → _initializer-head_ _generic-parameter-clause?_ _parameter-clause_ **`async`**_?_ **`throws`**_?_ _generic-where-clause?_ _initializer-body_ \
> _initializer-declaration_ → _initializer-head_ _generic-parameter-clause?_ _parameter-clause_ **`async`**_?_ **`rethrows`** _generic-where-clause?_ _initializer-body_ \
> _initializer-head_ → _attributes?_ _declaration-modifiers?_ **`init`** \
> _initializer-head_ → _attributes?_ _declaration-modifiers?_ **`init`** **`?`** \
> _initializer-head_ → _attributes?_ _declaration-modifiers?_ **`init`** **`!`** \
> _initializer-body_ → _code-block_

## 초기화 해제 구문 선언 (Deinitializer Declaration)

_초기화 해제 구문 선언 (deinitializer declaration)_ 은 클래스 타입에 대한 초기화 해제 구문을 선언합니다. 초기화 해제 구문은 파라미터를 가지지 않고 다음의 형식을 가집니다:

```swift
deinit {
   <#statements#>
}
```

초기화 해제 구문은 클래스 객체에 어떠한 참조도 없으면 클래스 객체가 할당 해제되기 직전에 자동으로 호출됩니다. 초기화 해제 구문은 클래스 선언의 본문 내에만 선언될 수 있지만 클래스의 확장에는 선언될 수 없고 각 클래스는 하나만 가질 수 있습니다.

하위 클래스는 하위 클래스 객체가 할당 해제되기 직전에 암시적으로 호출되는 상위 클래스의 초기화 해제 구문을 상속합니다. 하위 클래스 객체는 상속 체인의 모든 초기화 해제 구문이 실행을 완료할 때까지 할당 해제되지 않습니다.

초기화 해제 구문은 직접적으로 호출되지 않습니다.

클래스 선언에서 초기화 해제 구문이 어떻게 사용되는지에 대한 예제는 [초기화 해제 (Deinitialization)](../language-guide-1/deinitialization.md) 를 참고 바랍니다.

> Grammar of a deinitializer declaration:
>
> _deinitializer-declaration_ → _attributes?_ **`deinit`** _code-block_

## 확장 선언 (Extension Declaration)

_확장 선언 (extension declaration)_ 은 기존 타입의 동작을 확장할 수 있습니다. 확장 선언은 `extension` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

```swift
extension <#type name#> where <#requirements#> {
   <#declarations#>
}
```

확장 선언의 본문은 _선언 (declarations)_ 이 하나도 없거나 하나 이상의 _선언 (declarations)_ 을 포함합니다. 이러한 _선언 (declaration)_ 스캔은 계산된 프로퍼티, 계산된 타입 프로퍼티, 인스턴스 메서드, 타입 메서드, 초기화 구문, 서브 스크립트 선언, 그리고 클래스, 구조체, 그리고 열거형 선언도 포함합니다. 확장 선언은 초기화 해제 구문 또는 프로토콜 선언, 저장된 프로퍼티, 프로퍼티 관찰자, 또는 다른 확장 선언은 포함할 수 없습니다. 프로토콜 확장에서 선언은 `final` 로 표시될 수 없습니다. 여러 종류의 선언을 포함한 확장에 대한 내용과 예제는 [확장 (Extensions)](../language-guide-1/extensions.md) 을 참고 바랍니다.

_타입 이름 (type name)_ 이 클래스, 구조체, 또는 열거형 타입 이면 확장은 해당 타입을 확장합니다. _타입 이름 (type name)_ 이 프로토콜 타입 이면 확장은 해당 프로토콜을 준수하는 모든 타입을 확장합니다.

제너릭 타입 또는 연관된 타입이 있는 프로토콜을 확장하는 확장 선언은 _요구사항 (requirements)_ 을 포함할 수 있습니다. 확장된 타입 또는 확장된 프로토콜을 준수하는 타입의 인스턴스가 _요구사항 (requirements)_ 을 충족하면 인스턴스는 선언에 지정된 동작을 얻습니다.

확장 선언은 초기화 구문 선언을 포함할 수 있습니다. 이 말은 확장하려는 타입이 다른 모듈에 정의되면 초기화 구문 선언은 해당 타입의 멤버가 올바르게 초기화 되도록 해당 모듈에 이미 정의된 초기화 구문으로 위임해야 합니다.

기존 타입의 프로퍼티, 메서드, 그리고 초기화 구문은 해당 타입의 확장에서 재정의 될 수 없습니다.

확장 선언은 _채택된 프로토콜 (adopted protocols)_ 을 지정하여 기존 클래스, 구조체, 또는 열거형 타입에 프로토콜 준수를 추가할 수 있습니다:

```swift
extension <#type name#>: <#adopted protocols#> where <#requirements#> {
   <#declarations#>
}
```

확장 선언은 기존 클래스에 클래스 상속을 추가할 수 없으므로 _타입 이름 (type name)_ 과 콜론 뒤에 프로토콜의 리스트만 지정할 수 있습니다.

### 조건부 준수성 (Conditional Conformance)

조건부로 프로토콜을 준수하기 위해 제너릭 타입을 확장할 수 있기 때문에 타입의 인스턴스가 특정 요구사항만 충족되면 프로토콜을 준수하도록 할 수 있습니다. 확장 선언에 _요구사항 (requirements)_ 을 추가하여 조건부 프로토콜 준수성을 추가합니다.

#### 재정의한 요구사항은 일부 제너릭 컨텍스트에서 사용되지 않음 (Overridden Requirements Aren’t Used in Some Generic Contexts)

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

`Pair` 구조체는 제너릭 타입이 `Loggable` 또는 `TitledLoggable` 을 각각 준수할 때마다 `Loggable` 과 `TitledLoggable` 을 준수합니다. 아래 예제에서 `oneAndTwo` 는 `String` 이 `TitledLoggable` 을 준수하기 때문에 `TitledLoggable` 을 준수하는 `Pair<String>` 의 인스턴스 입니다. `oneAndTwo` 에서 `log()` 메서드를 직접 호출하면 타이틀 문자열을 포함하는 특별한 버전이 사용됩니다.

```swift
let oneAndTwo = Pair(first: "one", second: "two")
oneAndTwo.log()
// Prints "Pair of 'String': (one, two)"
```

그러나 `oneAndTwo` 제너릭 컨텍스트에서 사용되거나 `Loggable` 프로토콜의 인스턴스로 사용되면 특별한 버전이 사용되지 않습니다. Swift 는 `Pair` 가 `Loggable` 준수하는데 필요한 최소 요구사항만 참조하여 호출하기 위한 `log()` 의 구현을 선택합니다. 이러한 이유로 `Loggable` 프로토콜에 의해 제공된 기본 구현이 대신 사용됩니다.

```swift
func doSomething<T: Loggable>(with x: T) {
    x.log()
}
doSomething(with: oneAndTwo)
// Prints "(one, two)"
```

`log()` 가 `doSomething(_:)` 에 전달된 인스턴스에서 호출되면 사용자 정의 된 타이틀은 로깅 된 문자열에서 생략됩니다.

### 프로토콜 준수성은 중복되지 않아야 함 (Protocol Conformance Must Not Be Redundant)

구체적인 타입은 특정 프로토콜을 한 번만 준수 할 수 있습니다. Swift 는 중복 프로토콜 준수를 에러로 표시합니다. 두 가지 상황에서 이러한 에러가 발생할 수 있습니다. 첫번째 상황은 동일한 프로토콜을 여러번 명시적으로 준수하지만 요구사항이 다른 경우입니다. 두번째 상황은 암시적으로 동일한 프로토콜에서 여러번 상속할 때 입니다. 이러한 상황은 아래 섹션에 설명되어 있습니다.

#### 명시적 중복 해결 (Resolving Explicit Redundancy)

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

`Loggable` 에 대한 조건부 준수성을 명시적으로 선언하는 확장이 없으면 다른 `Array` 확장이 암시적으로 이러한 선언을 생성하여 에러가 발생합니다:

```swift
extension Array: Loggable where Element: TitledLoggable { }
extension Array: Loggable where Element: MarkedLoggable { }
// Error: redundant conformance of 'Array<Element>' to protocol 'Loggable'
```

> Grammar of an extension declaration:
>
> _extension-declaration_ → _attributes?_ _access-level-modifier?_ **`extension`** _type-identifier_ _type-inheritance-clause?_ _generic-where-clause?_ _extension-body_ \
> _extension-body_ → **`{`** _extension-members?_ **`}`**
>
> _extension-members_ → _extension-member_ _extension-members?_ \
> _extension-member_ → _declaration_ | _compiler-control-statement_

## 서브 스크립트 선언 (Subscript Declaration)

_서브 스크립트 (subscript)_ 선언은 특정 타입에 대한 서브 스크립트를 지원을 추가할 수 있고 일반적으로 콜렉션, 리스트, 또는 시퀀스에서 요소의 접근에 대한 편리한 구문을 제공하기 위해 사용됩니다. 서브 스크립트 선언은 `subscript` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

```swift
subscript (<#parameters#>) -> <#return type#> {
   get {
      <#statements#>
   }
   set(<#setter name#>) {
      <#statements#>
   }
}
```

서브 스크립트 선언은 클래스, 구조체, 열거형, 확장, 또는 프로토콜 선언의 컨텍스트에서만 나타날 수 있습니다.

_파라미터 (parameters)_ 는 서브 스크립트 표현식에서 해당 타입의 요소에 접근하기 위해 사용되는 하나 이상의 인덱스를 지정합니다 (예를 들어 표현식 `object[i]` 에서 `i`). 요소에 접근하기 위해 사용되는 인덱스는 모든 타입이 될 수 있지만 각 파라미터는 각 인덱스의 타입을 지정하기 위해 타입 주석을 포함해야 합니다. _반환 타입 (return type)_ 은 접근하는 요소의 타입을 지정합니다.

계산된 프로퍼티와 마찬가지로 서브 스크립트 선언은 접근한 요소의 값을 읽고 쓰기를 제공합니다. getter 는 값을 읽기 위해 사용되고 setter 는 값을 쓰기 위해 사용됩니다. setter 절은 선택사항이며 getter 만 필요하다면 모든 절을 생략할 수 있고 직접적으로 요청된 값을 간단하게 반환합니다. 이 말은 setter 절을 제공하면 getter 절을 제공해야만 한다는 의미입니다.

_setter 이름 (setter name)_ 과 둘러싸인 소괄호는 선택사항입니다. setter 이름을 제공하면 setter 의 파라미터의 이름으로 사용됩니다. setter 이름을 제공하지 않으면 setter 의 기본 파라미터 이름은 `value` 입니다. setter 의 파라미터의 타입은 _반환 타입 (return type)_ 과 동일합니다.

_파라미터 (parameters)_ 또는 _반환 타입 (return type)_ 이 오버로딩 중인 것과 다르면 선언된 타입의 서브 스크립트 선언을 오버로드 할 수 있습니다. 상위 클래스에서 상속된 서브 스크립트 선언을 재정의 할 수도 있습니다. 재정의 하면 `override` 선언 수식어로 재정의된 서브 스크립트 선언을 표시해야 합니다.

서브 스크립트 파라미터는 두가지를 제외하고 함수 파라미터와 동일한 규칙을 따릅니다. 기본 적으로 서브 스크립트에서 사용하는 파라미터는 함수, 메서드, 그리고 초기화 구문과 다르게 인수 라벨을 가지지 않습니다. 그러나 함수, 메서드, 그리고 초기화 구문에서 사용하는 동일한 구문을 사용하여 명시적으로 인수 라벨을 제공할 수 있습니다. 또한 서브 스크립트는 in-out 파라미터를 가질 수 없습니다. 서브 스크립트 파라미터는 [특별한 종류의 파라미터 (Special Kinds of Parameters)](declarations.md#special-kinds-of-parameters) 에서 설명 한 구문을 사용하여 기본값을 가질 수 있습니다.

[프로토콜 서브 스크립트 선언 (Protocol Subscript Declaration)](declarations.md#protocol-subscript-declaration) 에서 설명 한대로 프로토콜 선언의 컨텍스트에서 서브 스크립트를 선언할 수도 있습니다.

서브 스크립트에 대한 자세한 내용과 서브 스크립트 선언의 예제는 [서브 스크립트 (Subscripts)](../language-guide-1/subscripts.md) 를 참고 바랍니다.

### 타입 서브 스크립트 선언 (Type Subscript Declarations)

타입의 인스턴스가 아닌 타입에 의해 노출되는 서브 스크립트를 선언하려면 `static` 선언 수식어로 서브 스크립트 선언을 표시합니다. 클래스는 대신 `class` 선언 수식어로 타입 계산된 프로퍼티를 표시하여 하위 클래스가 상위 클래스의 구현을 재정의 할 수 있습니다. 클래스 선언에서 `static` 키워드는 `class` 와 `final` 선언 수식어 모두 선언에 표시하는 것과 동일한 효과를 가집니다.

> Grammar of a subscript declaration:
>
> _subscript-declaration_ → _subscript-head_ _subscript-result_ _generic-where-clause?_ _code-block_ \
> _subscript-declaration_ → _subscript-head_ _subscript-result_ _generic-where-clause?_ _getter-setter-block_ \
> _subscript-declaration_ → _subscript-head_ _subscript-result_ _generic-where-clause?_ _getter-setter-keyword-block_ \
> _subscript-head_ → _attributes?_ _declaration-modifiers?_ **`subscript`** _generic-parameter-clause?_ _parameter-clause_ \
> _subscript-result_ → **`->`** _attributes?_ _type_

## 매크로 선언 (Macro Declaration)

_매크로 선언 (macro declaration)_ 은 새로운 매크로를 도입합니다. `macro` 키워드로 시작하고 다음의 형식을 가집니다:

```swift
macro <#name#> = <#macro implementation#>
```

_매크로 구현 (macro implementation)_ 은 또다른 매크로이며, 매크로의 확장을 수행하는 코드의 위치를 나타냅니다.
매크로 확장을 수행하는 코드는 Swift 코드와 상호작용 하기위해
[SwiftSyntax][http://github.com/apple/swift-syntax/] 모듈을 사용하는 별도의 Swift 프로그램 입니다.
매크로의 구현을 포함하는 타입의 이름과 해당 타입을 포함하는 모듈의 이름을 전달하여 Swift 표준 라이브러리에서 `externalMacro(module:type:)` 매크로를 호출합니다.

함수에서 사용하는 동일한 모델에 따라 매크로는 오버로드 될 수 있습니다. 매크로 선언은 파일 범위에서만 나타납니다.

Swift 에서 매크로의 개요는 [매크로 (Macros)](../language-guide-1/macros.md) 를 참고 바랍니다.

> Grammar of a macro declaration:
>
> *macro-declaration* → *macro-head* *identifier* *generic-parameter-clause*_?_ *macro-signature* *macro-definition*_?_ *generic-where-clause* \
> *macro-head* → *attributes*_?_ *declaration-modifiers*_?_ **`macro`** \
> *macro-signature* → *parameter-clause* *macro-function-signature-result*_?_ \
> *macro-function-signature-result* → **`->`** *type* \
> *macro-definition* → **`=`** *expression*

## 연산자 선언 (Operator Declaration)

_연산자 선언 (operator declaration)_ 은 프로그램에 새로운 중위 (infix), 접두사 (prefix), 그리고 접미사 (postfix) 연산자를 도입하고 `operator` 키워드를 사용하여 선언됩니다.

중위, 접두사, 그리고 접미사의 세가지 수정사항의 연산자를 선언할 수 있습니다. 연산자의 _고정성 (fixity)_ 은 연산자의 피연산자의 상대 위치를 지정합니다.

연산자 선언에는 세가지 기본 형식이 있으며 각 고정성 마다 하나씩 있습니다. 연산자의 고정성은 `operator` 키워드 전에 `infix`, `prefix`, 또는 `postfix` 선언 수식어로 연산자의 선언을 표시하여 지정합니다. 각 형식에서 연산자의 이름은 [연산자 (Operators)](lexical-structure.md#operators) 에서 정의된 연산자 문자만 포함 할 수 있습니다.

다음 형식은 새로운 중위 연산자를 선언합니다:

```swift
infix operator <#operator name#>: <#precedence group#>
```

_중위 연산자 (infix operator)_ 는 표현식 `1 + 2` 에서 덧셈 연산자 (`+`) 와 같이 두 피연산자 사이에 작성되는 이진 연산자 입니다.

중위 연산자는 선택적으로 우선순위 그룹을 지정할 수 있습니다. 연산자에 대해 우선순위 그룹을 생략하면 Swift 는 `TernaryPrecedence` 보다 더 높은 우선순위를 지정하는 `DefaultPrecedence` 기본 우선순위 그룹을 사용합니다. 더 자세한 내용은 [우선순위 그룹 선언 (Precedence Group Declaration)](declarations.md#precedence-group-declaration) 을 참고 바랍니다.

다음 형식은 새로운 접두사 연산자를 선언합니다:

```swift
prefix operator <#operator name#>
```

_접두사 연산자 (prefix operator)_ 는 표현식 `!a` 에서 접두사 논리적 NOT 연산자 (`!`) 와 같은 피연산자 직전에 작성되는 단항 연산자 (unary operator) 입니다.

접두사 연산자 선언은 우선순위를 지정하지 않습니다. 접두사 연산자는 비연관성 (nonassociative) 입니다.

다음 형식은 접미사 연산자를 선언합니다:

```swift
postfix operator <#operator name#>
```

_접미사 연산자 (postfix operator)_ 는 표현식 `a!` 에서 접미사 강제 언래핑 연산자 (`!`) 와 같이 피연산자 바로 뒤에 작성되는 단항 연산자 (unary operator) 입니다.

접두사 연산와 같이 접미사 연산자 선언은 우선순위를 지정하지 않습니다. 접미사 연산자는 비연관성 (nonassociative) 입니다.

새로운 연산자를 선언한 후에 연산자와 같은 이름을 가지는 정적 메서드를 선언하여 구현합니다. 정적 메서드는 연산자가 값을 인수로 가지는 타입 중 하나의 멤버입니다 — 예를 들어 `Double` 에 `Int` 를 곱하는 연산자는 `Double` 또는 `Int` 구조체에서 정적 메서드로 구현됩니다. 접두사 또는 접미사 연산자를 구현하면 해당하는 `prefix` 또는 `postfix` 선언 수식어를 사용하여 메서드 선언을 표시해야 합니다. 새로운 연산자를 어떻게 생성하고 구현하는지에 대한 예제는 [사용자 정의 연산자 (Custom Operators)](../language-guide-1/advanced-operators.md#custom-operators) 를 참고 바랍니다.

> Grammar of an operator declaration:
>
> _operator-declaration_ → _prefix-operator-declaration_ | _postfix-operator-declaration_ | _infix-operator-declaration_
>
> _prefix-operator-declaration_ → **`prefix`** **`operator`** _operator_ \
> _postfix-operator-declaration_ → **`postfix`** **`operator`** _operator_ \
> _infix-operator-declaration_ → **`infix`** **`operator`** _operator_ _infix-operator-group?_
>
> _infix-operator-group_ → **`:`** _precedence-group-name_

## 우선순위 그룹 선언 (Precedence Group Declaration)

_우선순위 그룹 선언 (precedence group declaration)_ 은 프로그램에서 중위 연산자 우선순위에 대한 새로운 그룹화을 도입합니다. 연산자의 우선순위는 그룹화 괄호가 없을 때 연산자가 피연산자에 얼마나 밀접하게 바인딩 되는지 지정합니다.

우선순위 그룹 선언은 다음의 형식을 가집니다:

```swift
precedencegroup <#precedence group name#> {
    higherThan: <#lower group names#>
    lowerThan: <#higher group names#>
    associativity: <#associativity#>
    assignment: <#assignment#>
}
```

_하위 그룹 이름 (lower group names)_ 과 _상위 그룹 이름 (higher group names)_ 리스트는 기존 우선순위 그룹에 새로운 우선순위 그룹의 관계를 지정합니다. `lowerThan` 우선순위 그룹 속성은 현재 모듈의 외부에 선언된 우선순위 그룹을 참조하는 데만 사용됩니다. 표현식 `2 + 3 * 5` 에서와 같이 두 연산자가 피연산자에 대해 서로 경쟁할 때 상대적으로 우선순위가 높은 연산자가 피연산자에 더 밀접하게 바인딩 됩니다.

> Note\
> _하위 그룹 이름 (lower group names)_ 과 _상위 그룹 이름 (higher group names)_ 을 사용하는 서로 관련된 우선순위 그룹은 단일 관계형 계층 (single relational hierarchy) 에 적합해야 하지만 선형 계층 (linear hierarchy) 을 형성할 필요는 없습니다. 이것은 상대적 우선순위가 정의되지 않은 우선순위 그룹일 수 있다는 의미입니다.\
> 이러한 우선순위 연산자는 그룹화 괄호없이 나란히 사용할 수 없습니다.

Swift 는 표준 라이브러리에서 제공된 연산자와 함께 사용할 수많은 우선순위 그룹을 정의합니다. 예를 들어 덧셈 (`+`) 과 뺄셈 (`-`) 연산자는 `AdditionPrecedence` 그룹에 속하고 곱셈 (`*`) 과 나눗셈 (`/`) 연산자는 `MultiplicationPrecedence` 그룹에 속합니다. Swift 표준 라이브러리에서 제공하는 우선순위 그룹의 완벽한 리스트는 [연산자 선언 (Operator Declarations)](https://developer.apple.com/documentation/swift/operator\_declarations) 을 참고 바랍니다.

연산자의 _연관성 (associativity)_ 은 그룹화 괄호가 없을 때 동일한 우선순위 수준을 가진 일련의 연산자가 함께 그룹화 되는 방식을 지정합니다. 상황에 맞는 키워드 `left`, `right`, 또는 `none` 중 하나를 작성하여 연산자의 연관성을 지정합니다 — 연관성을 생략하면 기본적으로 `none` 입니다. 왼쪽 연관성 (left-associative) 연산자는 왼쪽에서 오른쪽으로 그룹화 합니다. 예를 들어 뺄셈 연산자 (`-`) 는 표현식 `4 - 5 - 6` 은 `(4 - 5) - 6` 으로 그룹화 되고 `-7` 로 평가되므로 왼쪽 연관성 (left-associative) 입니다. 오른쪽 연관성 (right-associative) 연산자는 오른쪽에서 왼쪽으로 그룹화 하고 `none` 의 연관성으로 지정된 연산자는 전혀 연관되지 않습니다. 동일한 우선순위 수준의 비연관성 연산자는 서로 인접하게 표시될 수 없습니다. 예를 들어 `<` 연산자는 `none` 의 연관성을 가집니다. 이것은 `1 < 2 < 3` 은 유효하지 않은 표현식이라는 의미입니다.

우선순위 그룹의 _할당 (assignment)_ 은 옵셔널 체이닝을 포함하는 동작을 사용할 때 연산자의 우선순위를 지정합니다. `true` 로 설정하면 해당 우선순위 그룹에서 연산자는 옵셔널 체이닝 중에 Swift 표준 라이브러리의 할당 연산자와 동일한 그룹화 규칙을 사용합니다. 그렇지 않으면 `false` 로 설정하거나 생략된 경우 우선순위 그룹에서 연산자는 할당을 수행하지 않은 연산자와 동일한 옵셔널 체이닝 규칙을 따릅니다.

> Grammar of a precedence group declaration:
>
> _precedence-group-declaration_ → **`precedencegroup`** _precedence-group-name_ **`{`** _precedence-group-attributes?_ **`}`**
>
> _precedence-group-attributes_ → _precedence-group-attribute_ _precedence-group-attributes?_ \
> _precedence-group-attribute_ → _precedence-group-relation_ \
> _precedence-group-attribute_ → _precedence-group-assignment_ \
> _precedence-group-attribute_ → _precedence-group-associativity_
>
> _precedence-group-relation_ → **`higherThan`** **`:`** _precedence-group-names_ \
> _precedence-group-relation_ → **`lowerThan`** **`:`** _precedence-group-names_
>
> _precedence-group-assignment_ → **`assignment`** **`:`** _boolean-literal_
>
> _precedence-group-associativity_ → **`associativity`** **`:`** **`left`** \
> _precedence-group-associativity_ → **`associativity`** **`:`** **`right`** \
> _precedence-group-associativity_ → **`associativity`** **`:`** **`none`**
>
> _precedence-group-names_ → _precedence-group-name_ | _precedence-group-name_ **`,`** _precedence-group-names_ \
> _precedence-group-name_ → _identifier_

## 선언 수식어 (Declaration Modifiers)

_선언 수식어 (Declaration modifiers)_ 는 선언의 동작이나 의미를 수정하는 키워드 또는 상황에 맞는 키워드 입니다. 선언의 속성이 있는 경우 선언의 속성과 선언을 도입하는 키워드 사이에 적절한 키워드 또는 상황에 맞는 키워드를 작성하여 선언 수식어를 지정합니다.

`class`

이 수식어를 클래스의 멤버에 적용하여 멤버가 클래스의 인스턴스의 멤버가 아니라 클래스 자체의 멤버임을 나타냅니다. 이 수식어를 가지고 `final` 수식어를 가지지 않는 상위 클래스의 멤버는 하위 클래스에 의해 재정의될 수 있습니다.

`dynamic`

Objective-C 에 의해 표현될 수 있는 클래스의 모든 멤버에 이 수식어를 적용합니다. `dynamic` 수식어로 멤버 선언을 표시하면 해당 멤버에 대한 접근이 항상 Objective-C 런타임을 사용하여 동적으로 전달됩니다. 해당 멤버에 대한 접근은 컴파일러에 의해 인라인되거나 가상화 되지 않습니다.

`dynamic` 수식어로 표시된 선언은 Objective-C 런타임을 사용하여 전달되므로 `objc` 속성으로 표시되어야 합니다.

`final`

이 수식어를 클래스나 프로퍼티, 메서드, 또는 클래스의 서브 스크립트 멤버에 적용합니다. 클래스가 하위 클래스화 될 수 없음을 나타내기 위해 클래스에 적용됩니다. 클래스 멤버가 모든 하위 클래스에 재정의될 수 없음을 나타내기 위해 프로퍼티, 메서드, 또는 클래스의 서브 스크립트에 적용됩니다. `final` 속성을 어떻게 사용하는지에 대한 예제는 [재정의 방지 (Preventing Overrides)](../language-guide-1/inheritance.md#preventing-overrides) 를 참고 바랍니다.

`lazy`

이 수식어는 클래스 또는 구조체의 저장된 변수 프로퍼티에 적용하여 프로퍼티의 초기값이 프로퍼티에 처음 접근할 때 최대 한번만 계산되고 저장됨을 나타냅니다. `lazy` 수식어를 어떻게 사용하는지에 대한 예제는 [지연 저장된 프로퍼티 (Lazy Stored Properties)](../language-guide-1/properties.md#lazy-stored-properties) 를 참고 바랍니다.

`optional`

프로토콜의 프로퍼티, 메서드, 또는 서브 스크립트 멤버에 수식어를 적용하여 준수하는 타입에 이 멤버의 구현이 필수가 아님을 나타냅니다.

`objc` 로 표시된 프로토콜에만 `optional` 수식어를 적용할 수 있습니다. 결과적으로 클래스 타입만 선택적 멤버 요구사항을 포함하는 프로토콜을 채택하고 준수할 수 있습니다. `optional` 수식어를 어떻게 사용하는지에 대한 자세한 내용과 예를 들어 준수하는 타입이 이를 구현하는지 확실하지 않을 때 선택적 프로토콜 멤버에 접근하는 방법에 대한 가이드는 [옵셔널 프로토콜 요구사항 (Optional Protocol Requirements)](../language-guide-1/protocols.md#optional-protocol-requirements) 을 참고 바랍니다.

`required`

초기화 구문을 구현해야 하는 모든 하위 클래스를 나타내기 위해 클래스의 지정된 초기화 구문이나 편리한 초기화 구문에 이 수식어를 적용합니다. 초기화 구문의 하위 클래스의 구현은 `required` 수식어로 표시되어야 합니다.

`static`

이 수식어는 구조체, 클래스, 열거형, 또는 프로토콜의 멤버에 적용하여 멤버가 해당 타입의 인스턴스 멤버가 아닌 타입의 멤버 임을 나타냅니다. 클래스 선언의 범위에서 멤버 선언에 `static` 수식어를 작성하여 멤버 선언에 `class` 와 `final` 수식어를 작성한 것과 같은 효과를 가집니다. 그러나 클래스의 상수 타입 프로퍼티는 예외입니다: `static` 은 해당 선언에 `class` 나 `final` 을 작성할 수 없으므로 클래스가 아닐 때 보통의 의미를 가집니다.

`unowned`

이 수식어를 저장된 변수, 상수, 또는 저장된 프로퍼티에 적용하여 변수 또는 프로퍼티가 해당 값으로 저장된 객체에 미소유 참조가 있다는 것을 나타냅니다. 객체가 할당 해제된 후에 변수나 프로퍼티에 접근하려고 하면 런타임 에러가 발생합니다. 약한 참조와 마찬가지로 프로퍼티 또는 값의 타입은 클래스 타입이어야 합니다; 약한 참조와 다르게 타입은 옵셔널이 아닙니다. `unowned` 수식어에 대한 자세한 내용은 [미소유 참조 (Unowned References)](../language-guide-1/automatic-reference-counting.md#unowned-references) 를 참고 바랍니다.

`unowned(safe)`

`unowned` 의 명시적 철자입니다.

`unowned(unsafe)`

이 수식어를 저장된 변수, 상수, 또는 저장된 프로퍼티에 적용하여 변수나 프로퍼티에 해당 값으로 저장된 객체에 미소유 참조를 가집니다. 객체가 할당 해제된 후에 변수나 프로퍼티에 접근하려고 하면 객체가 사용된 메모리 위치에 접근하는데 이것은 안전하지 않은 메모리 (memory-unsafe) 동작입니다. 약한 참조와 마찬가지로 프로퍼티나 값의 타입은 클래스 타입이어야 합니다; 약한 참조와 다르게 타입은 옵셔널이 아닙니다. `unowned` 수식어에 대한 예제와 자세한 내용은 [미소유 참조 (Unowned References)](../language-guide-1/automatic-reference-counting.md#unowned-references) 를 참고 바랍니다.

`weak`

이 수식어를 저장된 변수나 저장된 변수 프로퍼티에 적용하면 변수 또는 프로퍼티에 해당 값으로 저장된 객체에 약한 참조를 가짐을 나타냅니다. 변수나 프로퍼티의 타입은 옵셔널 클래스 타입이어야 합니다. 객체가 할당 해제된 후에 변수나 프로퍼티에 접근하려고 하면 해당 값은 `nil` 입니다. `weak` 수식어에 대한 예제와 자세한 내용은 [약한 참조 (Weak References)](../language-guide-1/automatic-reference-counting.md#weak-references) 를 참고 바랍니다.

### 접근 제어 수준 (Access Control Levels)

Swift 는 다섯 수준의 접근 제어를 제공합니다: open, public, internal, file private, 그리고 private 입니다. 선언의 접근 수준을 지정하기 위해 접근-수준 수식어 중 하나로 선언을 표시할 수 있습니다. 접근 제어는 [접근 제어 (Access Control)](../language-guide-1/access-control.md) 에 자세히 설명되어 있습니다.

`open`

이 수식어를 선언에 적용하여 선언과 동일한 모듈의 코드로 선언에 접근되고 하위 클래스화 될 수 있음을 나타냅니다. `open` 접근-수준 수식어로 표시된 선언은 해당 선언을 포함하는 모듈을 가져오는 모듈의 코드에서 접근되고 하위 클래스화 될 수 있습니다.

`public`

이 수식어를 선언에 적용하여 선언과 동일한 모듈의 코드로 선언에 접근되고 하위 클래스화 될 수 있음을 나타냅니다. `public` 접근-수준 수식어로 표시된 선언은 해당 선언을 포함하는 모듈을 가져오는 모듈의 코드에서 접근되지만 하위 클래스화 될 수 없습니다.

`internal`

이 수식어를 선언에 적용하여 선언과 동일한 모듈의 코드로 선언에 접근될 수 있습니다. 기본적으로 대부분 선언은 암시적으로 `internal` 접근-수준 수식어로 표시됩니다.

`fileprivate`

이 수식어를 선언에 적용하여 선언과 동일한 소스 파일의 코드로만 선언에 접근될 수 있습니다.

`private`

이 수식어를 선언에 적용하여 선언의 직접 둘러싸인 범위 내의 코드로만 선언에 접근될 수 있습니다.

접근 제어를 위해
확장은 다음과 같이 동작합니다:

- 동일한 파일에 여러 확장이 있고,
  이 확장이 동일한 타입을 확장한다면,
  이 모든 확장은 동일한 접근 제어를 가집니다.
  해당 타입과 확장은 다른 파일에 있을 수 있습니다.

- 확장이 확장하는 타입과 동일한 파일에 있으면,
  확장은 확장하는 타입과 동일한 접근 제어를 가집니다.

- 타입의 선언에서 선언된 private 멤버는
  해당 타입의 확장에서 접근할 수 있습니다.
  한 확장에서 선언된 private 멤버는
  다른 확장과 확장된 타입의 선언에서
  접근할 수 있습니다.

위의 각 접근-수준 수식어는 괄호로 둘러싸인 `set` 키워드로 구성된 선택적으로 단일 인수를 허용합니다 ---
예를 들어 `private(set)`.
[Getter 와 Setter (Getters and Setters)](../language-guide-1/access-control.md#getter-setter-getters-and-setters) 에서 설명 한대로 변수 또는 서브 스크립트에 대한 접근 수준보다 작거나 같은 변수 또는 서브 스크립트의 setter 에 대한 접근 수준을 지정하려면 이 형식의 접근-수준 수식어를 사용합니다.

> Grammar of a declaration modifier:
>
> _declaration-modifier_ → **`class`** | **`convenience`** | **`dynamic`** | **`final`** | **`infix`** | **`lazy`** | **`optional`** | **`override`** | **`postfix`** | **`prefix`** | **`required`** | **`static`** | **`unowned`** | **`unowned`** **`(`** **`safe`** **`)`** | **`unowned`** **`(`** **`unsafe`** **`)`** | **`weak`** \
> _declaration-modifier_ → _access-level-modifier_ \
> _declaration-modifier_ → _mutation-modifier_ \
> _declaration-modifier_ → _actor-isolation-modifier_ \
> _declaration-modifiers_ → _declaration-modifier_ _declaration-modifiers?_
>
> _access-level-modifier_ → **`private`** | **`private`** **`(`** **`set`** **`)`** \
> _access-level-modifier_ → **`fileprivate`** | **`fileprivate`** **`(`** **`set`** **`)`** \
> _access-level-modifier_ → **`internal`** | **`internal`** **`(`** **`set`** **`)`** \
> _access-level-modifier_ → **`public`** | **`public`** **`(`** **`set`** **`)`** \
> _access-level-modifier_ → **`open`** | **`open`** **`(`** **`set`** **`)`**
>
> _mutation-modifier_ → **`mutating`** | **`nonmutating`**
>
> _actor-isolation-modifier_ → **`nonisolated`**
