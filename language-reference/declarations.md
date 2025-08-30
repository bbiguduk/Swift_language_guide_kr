# 선언 (Declarations)

타입, 연산자, 변수 및 기타 이름과 구성을 소개합니다.

*선언(declaration)*은 프로그램에 새로운 이름이나 구성을 도입합니다.
예를 들어 함수와 메서드를 도입하기 위해,
변수와 상수를 도입하기 위해,
열거형, 구조체, 클래스, 프로토콜 타입을 정의하기 위해 선언을 사용합니다.
또한 선언을 사용하여 기존에 명명 타입의 동작을 확장하고
다른 곳에서 선언된 기호를 프로그램으로 가져올 수도 있습니다.

Swift에서 대부분의 선언은 선언과 동시에 구현되거나
초기화된다는 의미에서 정의이기도 합니다.
이 말은 프로토콜은 멤버를 정의하지 않으므로 대부분 프로토콜 멤버는 선언만 있습니다.
편의상 그리고 Swift에서는 해당 구별이 그다지 중요하지 않으므로
*선언(declaration)*은 선언과 정의를 모두 포함합니다.

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
> *declaration* → *precedence-group-declaration*

## 최상위-수준 코드 (Top-Level Code)

Swift 소스 파일의 최상위-수준 코드는 비어있거나
구문, 선언, 표현식으로 구성됩니다.
기본적으로 소스 파일의 최상위-수준에 선언된
변수, 상수, 다른 명명 선언은
동일한 모듈의 일부인 모든 소스 파일 코드에서 접근할 수 있습니다.
[접근 제어 수준 (Access Control Levels)](#접근-제어-수준-access-control-levels)에서 설명한대로
접근-수준 수정자(access-level modifier)로 선언을 표시하여
기본 동작을 재정의할 수 있습니다.

최상위-수준 코드는 두 가지 종류가 있습니다:
최상위-수준 선언(top-level declarations)과 실행 가능한 최상위-수준 코드(executable top-level code)입니다.
최상위-수준 선언은 선언으로만 구성되고
모든 Swift 소스 파일에서 허용됩니다.
실행 가능한 최상위-수준 코드는 선언 뿐만 아니라
구문과 표현식을 포함하고
프로그램에 대해 최상위-수준 진입점으로만 허용됩니다.

실행 가능하게 만들기 위해 컴파일 한 Swift 코드는
코드가 파일과 모듈로 구성되는 방식과 상관없이
최상위-수준 진입점을 나타내는
다음 접근방식 중 하나만 포함할 수 있습니다:
`main` 속성,
`NSApplicationMain` 속성,
`UIApplicationMain` 속성,
`main.swift` 파일,
최상위-수준 실행 가능한 코드를 포함하는 파일.

> Grammar of a top-level declaration:
>
> *top-level-declaration* → *statements*_?_

## 코드 블록 (Code Blocks)

*코드 블록(code block)*은 선언 및 제어 구조에서
구문을 그룹화 하기위해 사용됩니다.
다음의 형식을 가집니다:

```swift
{
   <#statements#>
}
```

코드 블록 내의 위 형식의 *구문(statements)*은
선언, 표현식, 구문의 다른 종류가 포함되고
소스 코드에 나타나는 순서대로 실행됩니다.

<!--
  TR: What exactly are the scope rules for Swift?
-->

<!--
  TODO: Discuss scope.  I assume a code block creates a new scope?
-->

> Grammar of a code block:
>
> *code-block* → **`{`** *statements*_?_ **`}`**

## 임포트 선언 (Import Declaration)

*임포트 선언(import declaration)*은
현재 파일 바깥에 선언된 기호에 접근할 수 있게 합니다.
기본 형식은 전체 모듈을 가져옵니다;
`import` 키워드 다음에 모듈 이름이 따라오도록 구성됩니다:

```swift
import <#module#>
```

임포트할 기호에 대한 자세한 제한을 제공하면
모듈이나 하위 모듈 내의
특정 하위 모듈이나 특정 선언을 지정할 수 있습니다.
이런 상세한 형식이 사용하면
임포트한 기호만
(선언한 모듈이 아닌)
현재 범위에서 사용할 수 있습니다.

```swift
import <#import kind#> <#module#>.<#symbol name#>
import <#module#>.<#submodule#>
```

<!--
  TODO: Need to add more to this section.
-->

> Grammar of an import declaration:
>
> *import-declaration* → *attributes*_?_ **`import`** *import-kind*_?_ *import-path*
>
> *import-kind* → **`typealias`** | **`struct`** | **`class`** | **`enum`** | **`protocol`** | **`let`** | **`var`** | **`func`** \
> *import-path* → *identifier* | *identifier* **`.`** *import-path*

## 상수 선언 (Constant Declaration)

*상수 선언(constant declaration)*은 프로그램에 상수값을 도입합니다.
상수 선언은 `let` 키워드를 사용하여 선언하고 다음의 형식을 가집니다:

```swift
let <#constant name#>: <#type#> = <#expression#>
```

상수 선언은 위 형식의 *상수 이름(constant name)*과
이니셜라이저 *표현식(expression)*의 값 사이의 변경 불가능한 바인딩을 정의합니다;
상수의 값이 설정된 후에는 변경할 수 없습니다.
이 말은 상수가 클래스 객체로 초기화되면,
이 객체 자체는 변경할 수 있지만
이 상수 이름과 참조하는 객체 간의 바인딩은 변경할 수 없습니다.

상수가 전역 범위로 선언되면,
값으로 반드시 초기화되어야 합니다.
상수 선언이 함수나 메서드의 컨텍스트에서 나타나면,
해당 값을 처음 읽기 전에
값을 설정한다는 보장이 있으면
나중에 초기화할 수 있습니다.
컴파일러가 상수의 값을 읽지 않는다는 것을 증명할 수 있으면,
상수에 값을 설정하지 않아도 됩니다.
이 분석을 *확정 초기화(definite initialization)*라고 합니다 ---
컴파일러는 읽기 전에 확실하게 값이 설정되어 있다는 것을 증명합니다.

> Note:
> 확정 초기화(Definite initialization)는
> 도메인 지식을 요구하는 증명을 구성할 수 없으며,
> 조건문에서 상태를 추적하는 것에는 제한이 있습니다.
> 상수에 항상 값이 있다고 결정할 수 있지만,
> 컴파일러가 이를 증명할 수 없는 경우에,
> 값을 설정하는 코드 경로를 단순화하거나,
> 변수 선언을 사용합니다.

<!--
In the most general case,
DI reduces to the halting problem,
as shown by Rice's theorem.
-->

상수 선언이 클래스나 구조체 선언의 컨텍스트에서 나타나면
*상수 프로퍼티(constant property)*로 간주됩니다.
상수 선언은 연산 프로퍼티가 아니므로
getter나 setter를 가지지 않습니다.

상수 선언의 *상수 이름(constant name)*이 튜플 패턴인 경우,
튜플에서 각 항목의 이름은 이니셜라이저 *표현식(expression)*에서
해당 값으로 바인딩 됩니다.

```swift
let (firstNumber, secondNumber) = (10, 42)
```

<!--
  - test: `constant-decl`

  ```swifttest
  -> let (firstNumber, secondNumber) = (10, 42)
  ```
-->

이 예시에서
`firstNumber`는 값 `10`에 대한 상수이고,
`secondNumber`는 값 `42`에 대한 상수 입니다.
두 상수 모두 독립적으로 사용할 수 있습니다:

```swift
print("The first number is \(firstNumber).")
// Prints "The first number is 10."
print("The second number is \(secondNumber).")
// Prints "The second number is 42."
```

<!--
  - test: `constant-decl`

  ```swifttest
  -> print("The first number is \(firstNumber).")
  <- The first number is 10.
  -> print("The second number is \(secondNumber).")
  <- The second number is 42.
  ```
-->

[타입 추론 (Type Inference)](./types.md#타입-추론-type-inference)에서 설명한대로
*상수 이름(constant name)*의 타입을 추론할 수 있을 때
상수 선언의 타입 주석(`:` *타입*)은 선택 사항입니다.

상수 타입 프로퍼티(constant type property)를 선언하려면
`static` 선언 수정자로 선언을 표시해야 합니다.
클래스의 상수 타입 프로퍼티는 암시적으로 final 입니다;
하위 클래스에 의한 재정의를 허용하거나 금지하기 위해
`class`나 `final` 선언 수정자로 표시할 수 없습니다.
타입 프로퍼티는 [타입 프로퍼티 (Type Properties)](../language-guide-1/properties.md#타입-프로퍼티-type-properties)에 설명되어 있습니다.

<!--
  - test: `class-constants-cant-have-class-or-final`

  ```swifttest
  -> class Super { class let x = 10 }
  !$ error: class stored properties not supported in classes; did you mean 'static'?
  !! class Super { class let x = 10 }
  !!               ~~~~~     ^
  -> class S { static final let x = 10 }
  !$ error: static declarations are already final
  !! class S { static final let x = 10 }
  !!                  ^~~~~~
  !!-
  ```
-->

상수에 대한 자세한 내용과 언제 사용해야 하는지에 대한 지침은
[상수와 변수 (Constants and Variables)](../language-guide-1/the-basics.md#상수와-변수-constants-and-variables)와 [저장 프로퍼티 (Stored Properties)](../language-guide-1/properties.md#저장-프로퍼티-stored-properties)를 참고바랍니다.

> Grammar of a constant declaration:
>
> *constant-declaration* → *attributes*_?_ *declaration-modifiers*_?_ **`let`** *pattern-initializer-list*
>
> *pattern-initializer-list* → *pattern-initializer* | *pattern-initializer* **`,`** *pattern-initializer-list* \
> *pattern-initializer* → *pattern* *initializer*_?_ \
> *initializer* → **`=`** *expression*

## 변수 선언 (Variable Declaration)

*변수 선언(variable declaration)*은 프로그램에 변수 값을 도입하고
`var` 키워드를 사용하여 선언합니다.

변수 선언은
저장 및 연산 변수와 프로퍼티,
저장 변수와 프로퍼티 관찰자, 정적 변수 프로퍼티를 포함하여
여러 종류의 명명된 값과 변경 가능한 값을 선언하는 여러 형식을 가집니다.
사용할 적절한 형식은
변수가 선언되는 범위와 선언하려는 변수의 종류에 따라 다릅니다.

> Note: [프로토콜 프로퍼티 선언 (Protocol Property Declaration)](#프로토콜-프로퍼티-선언-protocol-property-declaration)에서 설명한대로
> 프로토콜 선언의 컨텍스트에서도 프로퍼티를 선언할 수 있습니다.

[재정의 (Overriding)](../language-guide-1/inheritance.md#재정의-overriding)에서 설명한대로 `override` 선언 수정자로
하위 클래스의 프로퍼티 선언을 표시하여 하위 클래스에서 프로퍼티를 재정의할 수 있습니다.

### 저장 변수와 변수 저장 프로퍼티 (Stored Variables and Stored Variable Properties)

다음의 형식으로 저장 변수(stored variable)나 변수 저장 프로퍼티(stored variable property)를 선언할 수 있습니다:

```swift
var <#variable name#>: <#type#> = <#expression#>
```

이러한 형식의 변수 선언은 전역 범위,
함수의 지역 범위나 클래스나 구조체 선언의 컨텍스트에서 정의합니다.
이러한 형식의 변수 선언이 전역 범위나
함수의 지역 범위로 선언되면 *저장 변수(stored variable)*로 참조됩니다.
클래스나 구조체 선언의 컨텍스트에서 선언되면
*변수 저장 프로퍼티(stored variable property)*로 참조됩니다.

이니셜라이저 *표현식(expression)*은 프로토콜 선언에 있을 수 없지만
다른 모든 컨텍스트에서는 이니셜라이저 *표현식(expression)*이 선택 사항입니다.
즉, 이니셜라이저 *표현식(expression)*이 없으면,
변수 선언은 명시적 타입 주석(`:` *타입(type)*)을 포함해야 합니다.

상수 선언과 마찬가지로
변수 선언이 이니셜라이저 *표현식(expression)*을 생략하는 경우,
변수는 처음 읽기 전에 값을 설정해야 합니다.
또한 상수 선언과 마찬가지로
*변수 이름(variable name)*이 튜플 패턴인 경우,
튜플에서 각 항목에 이름은 이니셜라이저 *표현식(expression)*의
해당 값에 바인딩 됩니다.

이름에서 알 수 있듯이, 저장 변수나 변수 저장 프로퍼티의 값은
메모리에 저장됩니다.

### 연산 변수와 연산 프로퍼티 (Computed Variables and Computed Properties)

다음의 형식은 연산 변수(computed variable)나 연산 프로퍼티(computed property)를 선언합니다:

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

이러한 형식의 변수 선언은 전역 범위, 함수의 지역 범위
또는 클래스, 구조체, 열거형, 확장 선언의 컨텍스트에서 정의합니다.
이러한 형식의 변수 선언을 전역 범위나 함수의 지역 범위로 선언하면
*연산 변수(computed variable)*로 참조합니다.
클래스, 구조체, 확장 선언의
컨텍스트에서 선언하면
*연산 프로퍼티(computed property)*로 참조합니다.

getter는 값을 읽기 위해 사용되고,
setter는 값을 작성하기 위해 사용됩니다.
setter 절은 선택 사항이며,
getter만 필요하다면
[읽기 전용 연산 프로퍼티 (Read Only Computed Properties)](../language-guide-1/properties.md#읽기-전용-연산-프로퍼티-read-only-computed-properties)에 설명한대로
getter와 setter 절 모두 생략하고 요구된 값을 직접적으로 반환할 수 있습니다.
그러나 setter 절을 제공하면 getter 절도 제공해야 합니다.

위 형식의 *setter 이름(setter name)*과 둘러싸인 소괄호는 선택 사항입니다.
setter 이름을 제공하면, setter의 매개변수 이름으로 사용됩니다.
setter 이름을 제공하지 않으면, [간략한 Setter 선언 (Shorthand Setter Declaration)](../language-guide-1/properties.md#간략한-setter-선언-shorthand-setter-declaration)에서 설명한대로
setter의 기본 매개변수 이름은 `newValue`입니다.

명명된 저장값과 변수 저장 프로퍼티와 다르게
명명된 연산값이나 연산 프로퍼티의 값은 메모리에 저장되지 않습니다.

연산 프로퍼티의 자세한 내용과 예시는
[연산 프로퍼티 (Computed Properties)](../language-guide-1/properties.md#연산-프로퍼티-computed-properties)를 참고바랍니다.

### 저장 변수 관찰자와 프로퍼티 관찰자 (Stored Variable Observers and Property Observers)

`willSet`과 `didSet` 관찰자와 함께 저장 변수나 프로퍼티를 선언할 수도 있습니다.
괄찰자와 함께 선언한 저장 변수나 프로퍼티는 다음의 형식을 가집니다:

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

이러한 형식의 변수 선언은 전역 범위, 함수의 지역 범위
또는 클래스나 구조체 선언의 컨텍스트에서 정의합니다.
이러한 형식의 변수 선언이 전역 범위나 함수의 지역 범위로 선언되면,
관찰자는 *저장 변수 관찰자(stored variable observers)*로 참조합니다.
클래스나 구조체 선언의 컨텍스트에서 선언되면,
관찰자는 *프로퍼티 관찰자(property observers)*로 참조합니다.

모든 저장 프로퍼티에 프로퍼티 관찰자를 추가할 수 있습니다.
[프로퍼티 관찰자 재정의 (Overriding Property Observers)](../language-guide-1/inheritance.md#프로퍼티-관찰자-재정의-overriding-property-observers)에서 설명한대로 하위 클래스 내에 프로퍼티 재정의로
저장 프로퍼티나 연산 프로퍼티와 상관없이 모든 상속된 프로퍼티에 프로퍼티 관찰자를 추가할 수도 있습니다.

이니셜라이저 *표현식(expression)*은 클래스나 구조체 선언의 컨텍스트에서 선택 사항이지만
다른 곳에선 필수입니다. 타입이 이니셜라이저 *표현식(expression)*에서 추론될 수 있으면,
*타입(type)*주석은 선택 사항입니다.
이 표현식은 프로퍼티의 값을 처음 읽을 때 평가됩니다.
프로퍼티의 초기값을 읽지 않고 재작성하면,
이 표현식은 프로퍼티를 처음 쓰기 전에 평가됩니다.

<!--
  - test: `overwriting-property-without-writing`

  ```swifttest
  >> func loudConst(_ x: Int) -> Int {
  >>     print("initial value:", x)
  >>     return x
  >> }
  >> var x = loudConst(10)
  >> x = 20
  >> print("x:", x)
  << initial value: 10
  << x: 20
  >> var y = loudConst(100)
  >> print("y:", y)
  << initial value: 100
  << y: 100
  ```
-->

`willSet`과 `didSet` 관찰자는 변수나 프로퍼티의 값이 설정될 때
관찰하고 적절한 반응을 위한 방법을 제공합니다.
관찰자는 변수나 프로퍼티가
처음 초기화될 때는 호출되지 않습니다.
대신에 값이 초기화 컨텍스트 바깥에서 설정될 때만 호출됩니다.

`willSet` 관찰자는 변수나 프로퍼티의 값이 설정되기 직전에만 호출됩니다.
새로운 값은 상수로 `willSet` 관찰자에 전달되고
`willSet` 절의 구현에서 변경할 수 없습니다.
`didSet` 관찰자는 새로운 값이 설정된 후 바로 호출됩니다.
`willSet` 관찰자와 달리 변수나 프로퍼티의 이전 값은 여전히 접근이 필요한 경우에
`didSet` 관찰자에 전달됩니다.
이 말은 `didSet` 관찰자 절 내에 변수나 프로퍼티에 값을 할당하면,
할당한 새 값이 방금 설정되어
`willSet` 관찰자에 전달된 값을 대체합니다.

`willSet`과 `didSet` 절에 위 형식의 *setter 이름(setter name)*과 둘러싸인 소괄호는 선택 사항입니다.
setter 이름을 제공한다면,
해당 이름은 `willSet`과 `didSet` 관찰자의 매개변수 이름으로 사용됩니다.
setter 이름을 제공하지 않는다면,
`willSet` 관찰자의 기본 매개변수 이름은 `newValue`이고
`didSet` 관찰자의 기본 매개변수 이름은 `oldValue`입니다.

`willSet` 절을 제공하면 `didSet` 절은 선택 사항입니다.
마찬가지로 `didSet` 절을 제공하면 `willSet` 절은 선택 사항입니다.

`didSet` 관찰자의 본문이 이전 값을 참조하면,
관찰자 전에 getter가 호출되어
이전 값을 사용 가능하도록 합니다.
그렇지 않으면 상위 클래스의 getter를 호출하지 않고 새 값이 저장됩니다.
아래 예시는 상위 클래스에 정의한 연산 프로퍼티를 보여주고
이 연산 프로퍼티를 하위 클래스에서 관찰자를 추가하여 재정의하는 것을 보여줍니다.

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
  - test: `didSet-calls-superclass-getter`

  ```swifttest
  -> class Superclass {
         private var xValue = 12
         var x: Int {
             get { print("Getter was called"); return xValue }
             set { print("Setter was called"); xValue = newValue }
         }
     }

  // This subclass doesn't refer to oldValue in its observer, so the
  // superclass's getter is called only once to print the value.
  -> class New: Superclass {
         override var x: Int {
             didSet { print("New value \(x)") }
         }
     }
     let new = New()
     new.x = 100
  <- Setter was called
  <- Getter was called
  <- New value 100

  // This subclass refers to oldValue in its observer, so the superclass's
  // getter is called once before the setter, and again to print the value.
  -> class NewAndOld: Superclass {
         override var x: Int {
             didSet { print("Old value \(oldValue) - new value \(x)") }
         }
     }
     let newAndOld = NewAndOld()
     newAndOld.x = 200
  <- Getter was called
  <- Setter was called
  <- Getter was called
  <- Old value 12 - new value 200
  ```
-->

프로퍼티 관찰자를 사용하는 방법에 대한 자세한 내용과 예시는
[프로퍼티 관찰자 (Property Observers)](../language-guide-1/properties.md#프로퍼티-관찰자-property-observers)를 참고바랍니다.

<!--
  - test: `cant-mix-get-set-and-didSet`

  ```swifttest
  >> struct S {
  >>     var x: Int {
  >>         get { print("S getter"); return 12 }
  >>         set { return }
  >>         didSet { print("S didSet") }
  >>     }
  >> }
  !$ error: 'didSet' cannot be provided together with a getter
  !! didSet { print("S didSet") }
  !! ^
  ```
-->

### 타입 변수 프로퍼티 (Type Variable Properties)

타입 변수 프로퍼티(type variable property)를 선언하기 위해
`static` 선언 수정자와 함께 선언에 표시합니다.
클래스는 상위 클래스의 구현을 하위 클래스가 재정의할 수 있도록
`class` 선언 수정자와 함께 타입 연산 프로퍼티를 표시할 수 있습니다.
타입 프로퍼티는 [타입 프로퍼티 (Type Properties)](../language-guide-1/properties.md#타입-프로퍼티-type-properties)에 설명되어 있습니다.

> Grammar of a variable declaration:
>
> *variable-declaration* → *variable-declaration-head* *pattern-initializer-list* \
> *variable-declaration* → *variable-declaration-head* *variable-name* *type-annotation* *code-block* \
> *variable-declaration* → *variable-declaration-head* *variable-name* *type-annotation* *getter-setter-block* \
> *variable-declaration* → *variable-declaration-head* *variable-name* *type-annotation* *getter-setter-keyword-block* \
> *variable-declaration* → *variable-declaration-head* *variable-name* *initializer* *willSet-didSet-block* \
> *variable-declaration* → *variable-declaration-head* *variable-name* *type-annotation* *initializer*_?_ *willSet-didSet-block*
>
> *variable-declaration-head* → *attributes*_?_ *declaration-modifiers*_?_ **`var`** \
> *variable-name* → *identifier*
>
> *getter-setter-block* → *code-block* \
> *getter-setter-block* → **`{`** *getter-clause* *setter-clause*_?_ **`}`** \
> *getter-setter-block* → **`{`** *setter-clause* *getter-clause* **`}`** \
> *getter-clause* → *attributes*_?_ *mutation-modifier*_?_ **`get`** *code-block* \
> *setter-clause* → *attributes*_?_ *mutation-modifier*_?_ **`set`** *setter-name*_?_ *code-block* \
> *setter-name* → **`(`** *identifier* **`)`**
>
> *getter-setter-keyword-block* → **`{`** *getter-keyword-clause* *setter-keyword-clause*_?_ **`}`** \
> *getter-setter-keyword-block* → **`{`** *setter-keyword-clause* *getter-keyword-clause* **`}`** \
> *getter-keyword-clause* → *attributes*_?_ *mutation-modifier*_?_ **`get`** \
> *setter-keyword-clause* → *attributes*_?_ *mutation-modifier*_?_ **`set`**
>
> *willSet-didSet-block* → **`{`** *willSet-clause* *didSet-clause*_?_ **`}`** \
> *willSet-didSet-block* → **`{`** *didSet-clause* *willSet-clause*_?_ **`}`** \
> *willSet-clause* → *attributes*_?_ **`willSet`** *setter-name*_?_ *code-block* \
> *didSet-clause* → *attributes*_?_ **`didSet`** *setter-name*_?_ *code-block*

<!--
  NOTE: Type annotations are required for computed properties -- the
  types of those properties aren't computed/inferred.
-->

## 타입 별칭 선언 (Type Alias Declaration)

*타입 별칭 선언(type alias declaration)*은 프로그램의 기존 타입에 별칭을 도입합니다.
타입 별칭 선언은 `typealias` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

```swift
typealias <#name#> = <#existing type#>
```

타입 별칭이 선언된 후 별칭된 위 형식의 *이름(name)*은
프로그램에서 위 형식의 *기존 타입(existing type)* 대신에 사용할 수 있습니다.
위 형식의 *기존 타입(existing type)*은 명명 타입이나 복합 타입일 수 있습니다.
타입 별칭은 새로운 타입을 생성하지 않습니다;
간단하게 기존 타입을 참조하도록 이름을 허용합니다.

타입 별칭 선언은 제네릭 매개변수를 사용하여
기존 제네릭 타입에 이름을 제공할 수 있습니다.
타입 별칭은 기존 타입의 제너릭 매개변수 중
일부나 전부에 대해 구체적인 타입을 제공할 수 있습니다.
예를 들어:

```swift
typealias StringDictionary<Value> = Dictionary<String, Value>

// The following dictionaries have the same type.
var dictionary1: StringDictionary<Int> = [:]
var dictionary2: Dictionary<String, Int> = [:]
```

<!--
  - test: `typealias-with-generic`

  ```swifttest
  -> typealias StringDictionary<Value> = Dictionary<String, Value>

  // The following dictionaries have the same type.
  -> var dictionary1: StringDictionary<Int> = [:]
  -> var dictionary2: Dictionary<String, Int> = [:]
  ```
-->

타입 별칭이 제네릭 매개변수와 함께 선언될 때, 해당 매개변수의 제약조건은
기존 타입의 제네릭 매개변수의 제약조건과 완벽하게 일치해야 합니다.
예를 들어:

```swift
typealias DictionaryOfInts<Key: Hashable> = Dictionary<Key, Int>
```

<!--
  - test: `typealias-with-generic-constraint`

  ```swifttest
  -> typealias DictionaryOfInts<Key: Hashable> = Dictionary<Key, Int>
  ```
-->

타입 별칭과 기존 타입은 서로 바꿔서 사용될 수 있으므로,
타입 별칭은 추가로 제네릭 제약조건을 도입할 수 없습니다.

타입 별칭은 선언에서 모든 제네릭 매개변수를 생략하여
기존 타입의 제네릭 매개변수를 전달할 수 있습니다.
예를 들어
여기 선언한 `Diccionario` 타입 별칭은
`Dictionary`와 동일한 제네릭 매개변수와 제약조건을 가집니다.

```swift
typealias Diccionario = Dictionary
```

<!--
  - test: `typealias-using-shorthand`

  ```swifttest
  -> typealias Diccionario = Dictionary
  ```
-->

<!--
  Note that the compiler doesn't currently enforce this. For example, this works but shouldn't:
  typealias ProvidingMoreSpecificConstraints<T: Comparable & Hashable> = Dictionary<T, Int>
-->

<!--
  Things that shouldn't work:
  typealias NotRedeclaringSomeOfTheGenericParameters = Dictionary<T, String>
  typealias NotRedeclaringAnyOfTheGenericParameters = Dictionary
  typealias NotProvidingTheCorrectConstraints<T> = Dictionary<T, Int>
  typealias ProvidingMoreSpecificConstraints<T: Comparable & Hashable> = Dictionary<T, Int>
-->

프로토콜 선언 내의
타입 별칭은 자주 사용되는 타입에
더 짧고 편리한 이름을 제공할 수 있습니다.
예를 들어:

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
  - test: `typealias-in-protocol`

  ```swifttest
  -> protocol Sequence {
         associatedtype Iterator: IteratorProtocol
         typealias Element = Iterator.Element
     }

  -> func sum<T: Sequence>(_ sequence: T) -> Int where T.Element == Int {
         // ...
  >>     return 9000
     }
  ```
-->

타입 별칭이 없다면,
`sum` 함수는 `T.Element` 대신에 `T.Iterator.Element`로
연관 타입을 참조해야 합니다.

더 자세한 내용은 [프로토콜 연관 타입 선언 (Protocol Associated Type Declaration)](#프로토콜-연관-타입-선언-protocol-associated-type-declaration)을 참고바랍니다.

> Grammar of a type alias declaration:
>
> *typealias-declaration* → *attributes*_?_ *access-level-modifier*_?_ **`typealias`** *typealias-name* *generic-parameter-clause*_?_ *typealias-assignment* \
> *typealias-name* → *identifier* \
> *typealias-assignment* → **`=`** *type*

<!--
  Old grammar:
  typealias-declaration -> typealias-head typealias-assignment
  typealias-head -> ``typealias`` typealias-name type-inheritance-clause-OPT
  typealias-name -> identifier
  typealias-assignment -> ``=`` type
-->

## 함수 선언 (Function Declaration)

*함수 선언(function declaration)*은 프로그램의 함수나 메서드를 도입합니다.
클래스, 구조체, 열거형, 프로토콜의 컨텍스트에 선언된 함수는
*메서드(method)*로 참조됩니다.
함수 선언은 `func` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

```swift
func <#function name#>(<#parameters#>) -> <#return type#> {
   <#statements#>
}
```

함수가 `Void`의 반환 타입을 가지면,
반환 타입은 다음과 같이 생략할 수 있습니다:

```swift
func <#function name#>(<#parameters#>) {
   <#statements#>
}
```

각 매개변수의 타입은 추론될 수 없으므로
반드시 포함되어야 합니다.
매개변수의 타입 앞에 `inout`을 작성하면,
매개변수는 함수의 범위 내에서 수정될 수 있습니다.
In-out 매개변수는 아래
[In-Out 매개변수 (In-Out Parameters)](#in-out-매개변수-in-out-parameters)에서 자세하게 설명되어 있습니다.

함수 선어에서 *구문(statements)*에
단일 표현식만 포함된 경우
해당 표현식의 값을 반환하는 것으로 이해합니다.
이 암시적 반환 구문은
표현식의 타입과 함수의 반환 타입이
`Void`가 아니고
케이스가 없는 `Never`와 같은 열거형이 아닌 경우에만 간주됩니다.

<!--
  As of Swift 5.3,
  the only way to make an uninhabited type is to create an empty enum,
  so just say that directly instead of using & defining the compiler jargon.
-->

함수는 함수의 반환 타입으로
튜플 타입을 사용하여 여러 값을 반환할 수 있습니다.

<!--
  TODO: ^-- Add some more here.
-->

함수 정의는 다른 함수 선언 내에 나타날 수 있습니다.
이러한 함수를 *중첩 함수(nested function)*라고 합니다.

중첩 함수는 in-out 매개변수와 같이
절대 탈출하지 않는 값을 캡처하거나
비탈출 함수 인자로 전달된 경우
비탈출 입니다.
그렇지 않으면 중첩 함수는 탈출 함수 입니다.

중첩된 함수에 대한 내용은
[중첩 함수 (Nested Functions)](../language-guide-1/functions.md#중첩-함수-nested-functions)를 참고바랍니다.

### 매개변수 이름 (Parameter Names)

함수 매개변수는 각 매개변수가 여러 형식 중 하나를 갖는
콤마로 구분된 목록입니다.
함수 호출에서 인자의 순서는
함수의 선언 내에 매개변수의 순서와 일치해야 합니다.
매개변수 목록에서 가장 간단한 형식은 다음과 같습니다:

```swift
<#parameter name#>: <#parameter type#>
```

매개변수는 함수 본문 내에서 사용되는
이름 뿐만 아니라
함수나 메서드 호출할 때 사용되는
인자 레이블이 있습니다.
기본적으로
매개변수 이름은 인자 레이블로도 사용됩니다.
예를 들어:

```swift
func f(x: Int, y: Int) -> Int { return x + y }
f(x: 1, y: 2) // both x and y are labeled
```

<!--
  - test: `default-parameter-names`

  ```swifttest
  -> func f(x: Int, y: Int) -> Int { return x + y }
  >> let r0 =
  -> f(x: 1, y: 2) // both x and y are labeled
  >> assert(r0 == 3)
  ```
-->

<!--
  Rewrite the above to avoid bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

다음의 형식 중 하나로
인자 레이블의 기본 동작을 재정의할 수 있습니다:

```swift
<#argument label#> <#parameter name#>: <#parameter type#>
_ <#parameter name#>: <#parameter type#>
```

매개변수 이름 전에 이름은
매개변수에 명시적 인자 레이블을 부여하고,
이것은 매개변수 이름과 다를 수 있습니다.
해당 인자는 함수나 메서드 호출에서
주어진 인자 레이블을 사용해야 합니다.

매개변수 이름 전에 언더바(`_`)는
인자 레이블을 숨깁니다.
해당 인자는 함수나 메서드 호출에서 레이블이 없어야 합니다.

```swift
func repeatGreeting(_ greeting: String, count n: Int) { /* Greet n times */ }
repeatGreeting("Hello, world!", count: 2) //  count is labeled, greeting is not
```

<!--
  - test: `overridden-parameter-names`

  ```swifttest
  -> func repeatGreeting(_ greeting: String, count n: Int) { /* Greet n times */ }
  -> repeatGreeting("Hello, world!", count: 2) //  count is labeled, greeting is not
  ```
-->

### 매개변수 수정자 (Parameter Modifiers)

*매개변수 수정자(parameter modifier)*는 인자가 함수에 전달되는 방식을 변경합니다.

```swift
<#argument label#> <#parameter name#>: <#parameter modifier#> <#parameter type#>
```

매개변수 수정자를 사용하려면,
인자의 타입 앞에
`inout`, `borrowing`, `consuming`을 작성합니다.

```swift
func someFunction(a: inout A, b: consuming B, c: C) { ... }
```

#### In-Out 매개변수 (In-Out Parameters)

기본적으로, Swift에서 함수 인자는 값으로 전달됩니다:
함수 내에서 수정된 것은 호출자에게 보이지 않습니다.
in-out 매개변수를 만드려면,
`inout` 매개변수 수정자를 적용합니다.

```swift
func someFunction(a: inout Int) {
    a += 1
}
```

in-out 매개변수를 포함하는 함수를 호출할 때,
인자의 값이 변경될 수 있는 함수 호출인 것을 나타내기위해
in-out 인자는 앰퍼샌드(`&`)를 앞에 붙여야 합니다.

```swift
var x = 7
someFunction(&x)
print(x)  // Prints "8"
```

In-out 매개변수는 다음과 같이 전달됩니다:

1. 함수가 호출될 때,
   인자의 값은 복사됩니다.
2. 함수의 본문 내에서
   복사본은 수정됩니다.
3. 함수가 반환될 때,
   복사본의 값은 기존 인자에 할당됩니다.

이 동작은 *copy-in copy-out*이나
*call by value 결과*라고 합니다.
예를 들어
연산 프로퍼티나 관찰자가 있는 프로퍼티가
in-out 매개변수로 전달되는 경우
getter는 함수 호출의 부분으로 호출되고
setter는 함수 반환의 부분으로 호출됩니다.

최적화로
인자가 메모리의 물리적 주소에 저장된 값인 경우
동일한 메모리 위치가 함수 본문 내부 및 외부에서 모두 사용됩니다.
이런 최적화 동작을 *call by reference*라고 합니다;
이것은 copy-in copy-out 모델의
모든 요구사항을 충족하는 동시에
복사의 오버헤드를 제거합니다.
call-by-reference 최적화에 의존하지 않고
copy-in copy-out에 의해 주어진 모델을 사용하여 작성하면,
최적화에 상관없이 올바르게 작동되도록 합니다.

함수 내에서 기존값이 현재 범위에서 사용가능 하더라도
in-out 인자로 전달된 값은 접근하면 안됩니다.
기존값에 접근하는 것은 값에 대한 동시 접근이며
메모리 독점성을 위반합니다.

```swift
var someValue: Int
func someFunction(a: inout Int) {
    a += someValue
}

// Error: This causes a runtime exclusivity violation
someFunction(&someValue)
```

같은 이유로
여러 개의 in-out 매개변수에 동일한 값을 전달할 수 없습니다.

```swift
var someValue: Int
func someFunction(a: inout Int, b: inout Int) {
    a += b
    b += 1
}

// Error: Cannot pass the same value to multiple in-out parameters
someFunction(&someValue, &someValue)
```

메모리 안정성과 메모리 독점성에 대한 자세한 내용은
[메모리 안전성 (Memory Safety)](../language-guide-1/memory-safety.md)을 참고바랍니다.

<!--
  When the call-by-reference optimization is in play,
  it would happen to do what you want.
  But you still shouldn't do that --
  as noted above, you're not allowed to depend on
  behavioral differences that happen because of call by reference.
-->

in-out 매개변수를 캡처하는 클로저나 중첩 함수는
비탈출 이어야 합니다.
in-out 매개변수를 변경하지 않고
캡처 해야하는 경우,
캡처 리스트를 사용하여 매개변수를 변경하지 않고 명시적으로 캡처해야 합니다.

```swift
func someFunction(a: inout Int) -> () -> Int {
    return { [a] in return a + 1 }
}
```

<!--
  - test: `explicit-capture-for-inout`

  ```swifttest
  -> func someFunction(a: inout Int) -> () -> Int {
         return { [a] in return a + 1 }
     }
  >> class C { var x = 100 }
  >> let c = C()
  >> let f = someFunction(a: &c.x)
  >> c.x = 200
  >> let r = f()
  >> print(r, r == c.x)
  << 101 false
  ```
-->

in-out 매개변수를 캡처하고 변경이 필요한 경우,
함수가 반환하기 전에 모든 변경이 완료되었는지 확인하는
멀티 스레드 코드와 같이
명시적으로 지역 복사(local copy)를 사용합니다.

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
  - test: `cant-pass-inout-aliasing`

  ```swifttest
  >> import Dispatch
  >> func someMutatingOperation(_ a: inout Int) {}
  -> func multithreadedFunction(queue: DispatchQueue, x: inout Int) {
        // Make a local copy and manually copy it back.
        var localX = x
        defer { x = localX }

        // Operate on localX asynchronously, then wait before returning.
        queue.async { someMutatingOperation(&localX) }
        queue.sync {}
     }
  ```
-->

in-out 매개변수에 대한 자세한 설명과 예시는
[In-Out 매개변수 (In-Out Parameters)](../language-guide-1/functions.md#in-out-매개변수-in-out-parameters)를 참고바랍니다.

<!--
  - test: `escaping-cant-capture-inout`

  ```swifttest
  -> func outer(a: inout Int) -> () -> Void {
         func inner() {
             a += 1
         }
         return inner
     }
  !$ error: escaping local function captures 'inout' parameter 'a'
  !! return inner
  !! ^
  !$ note: parameter 'a' is declared 'inout'
  !! func outer(a: inout Int) -> () -> Void {
  !! ^
  !$ note: captured here
  !! a += 1
  !! ^
  -> func closure(a: inout Int) -> () -> Void {
         return { a += 1 }
     }
  !$ error: escaping closure captures 'inout' parameter 'a'
  !! return { a += 1 }
  !! ^
  !$ note: parameter 'a' is declared 'inout'
  !! func closure(a: inout Int) -> () -> Void {
  !! ^
  !$ note: captured here
  !! return { a += 1 }
  !! ^
  ```
-->

#### Borrowing 매개변수와 Consuming 매개변수 (Borrowing and Consuming Parameters)

기본적으로 Swift는
함수 호출 전반에 걸쳐 객체의 수명을 자동으로 관리하고,
필요할 때 값을 복사하는 규칙을 사용합니다.
기본 규칙은 대부분 상황에서 오버헤드를 최소화하도록 설계되어 있습니다 ---
더 구체적인 제어를 원하면,
`borrowing`이나 `consuming` 매개변수 수정자를 적용할 수 있습니다.
이 경우에
`copy`를 사용해 복사 작업을 명시적으로 표시합니다.
또한
복사 불가능한 타입의 값은 borrowing이나 consuming으로 전달되어야 합니다.

기본 규칙을 사용하는 것과 상관없이,
Swift는 객체 수명과 소유권이
모든 상황에서 올바르게 관리되도록 보장합니다.
이 매개변수 수정자는 정확성이 아닌 특정 사용 패턴의
상대적 효율성에만 영향을 줍니다.

<!--
TODO: Describe the default rules.
Essentially, inits and property setters are consuming,
and everything else is borrowing.
Where are copies implicitly inserted?
-->

`borrowing` 수정자는 함수가
매개변수 값을 유지하지 않음을 나타냅니다.
이 경우에 호출자는 객체의 소유권과
객체의 수명에 대한 책임을 유지합니다.
`borrowing`을 사용하면 함수가
객체를 일시적으로만 사용할 때 오버헤드를 최소화합니다.

```swift
// `isLessThan` does not keep either argument
func isLessThan(lhs: borrowing A, rhs: borrowing A) -> Bool {
    ...
}
```

예를 들어, 전역 변수에 값을 저장하기 위해
함수가 매개변수의 값을 유지해야 하는 경우 ---
값을 명시적으로 복사하기 위해 `copy`를 사용합니다.

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
`consuming` 매개변수 수정자는
함수가 값의 소유권을 가지고 있고,
함수가 반환하기 전에 값을 저장하거나 파기하는 책임이 있음을
나타냅니다.

```swift
// `store` keeps its argument, so mark it `consuming`
func store(a: consuming A) {
    someGlobalVariable = a
}
```

`consuming`을 사용하면 호출자가 함수 호출 후에 더 이상 객체를 사용할 필요가 없을 때
오버헤드를 최소화합니다.

```swift
// Usually, this is the last thing you do with a value
store(a: value)
```

함수 호출 후에 복사 가능한 객체를 사용하려면,
컴파일러는 자동으로 함수 호출 전에
객체의 복사본을 만듭니다.

```swift
// The compiler inserts an implicit copy here
store(a: someValue)  // This function consumes someValue
print(someValue)  // This uses the copy of someValue
```

`inout`과 다르게, `borrowing`과
`consuming` 매개변수는 함수를 호출할 때
특별한 표기법이 필요하지 않습니다:

```swift
func someFunction(a: borrowing A, b: consuming B) { ... }

someFunction(a: someA, b: someB)
```

`borrowing`이나 `consuming`를 명시적으로 사용하는 것은
런타임 소유권 관리의 오버헤드를 더 엄격하게 관리하려는
의도를 나타냅니다.
복사는 예기치 않은 런타임 소유권 동작을
야기할 수 있으므로,
이 수정자 중 하나로 표시된 매개변수는
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

<!--
  TODO: `borrowing` and `consuming` keywords with noncopyable argument types
-->
<!--
  TODO: Any change of parameter modifier is ABI-breaking
-->

### 특별한 종류의 매개변수 (Special Kinds of Parameters)

매개변수는 무시될 수 있고
다음의 형식을 사용하여
가변값을 가지거나
기본값을 제공할 수 있습니다:

```swift
_ : <#parameter type#>
<#parameter name#>: <#parameter type#>...
<#parameter name#>: <#parameter type#> = <#default argument value#>
```

언더바(`_`) 매개변수는
명시적으로 무시되고 함수의 본문 내에서 접근할 수 없습니다.

기본 타입 이름 바로 뒤에 세 개의 점(`...`)이 오는 매개변수는
가변 매개변수(variadic parameter)입니다.
가변 매개변수 다음에 오는 매개변수는
인자 레이블이 있어야 합니다.
함수는 여러 개의 가변 매개변수를 가질 수 있습니다.
가변 매개변수는 기본 타입 이름의 요소를 포함하는 배열로 처리합니다.
예를 들어 가변 매개변수 `Int...`는 `[Int]`로 처리됩니다.
가변 매개변수 사용 예시는
[가변 매개변수 (Variadic Parameters)](../language-guide-1/functions.md#가변-매개변수-variadic-parameters)를 참고바랍니다.

매개변수 타입 뒤에 등호(`=`)와 표현식은
주어진 표현식의 기본값을 가지는 것으로 간주됩니다.
함수가 호출될 때 주어진 표현식은 평가됩니다.
함수를 호출할 때 매개변수가 생략되면
기본값이 대신 사용됩니다.

```swift
func f(x: Int = 42) -> Int { return x }
f()       // Valid, uses default value
f(x: 7)   // Valid, uses the value provided
f(7)      // Invalid, missing argument label
```

<!--
  - test: `default-args-and-labels`

  ```swifttest
  -> func f(x: Int = 42) -> Int { return x }
  >> let _ =
  -> f()       // Valid, uses default value
  >> let _ =
  -> f(x: 7)   // Valid, uses the value provided
  >> let _ =
  -> f(7)      // Invalid, missing argument label
  !$ error: missing argument label 'x:' in call
  !! f(7)      // Invalid, missing argument label
  !!   ^
  !!   x:
  ```
-->

<!--
  Rewrite the above to avoid discarding the function's return value.
  Tracking bug is <rdar://problem/35301593>
-->

<!--
  - test: `default-args-evaluated-at-call-site`

  ```swifttest
  -> func shout() -> Int {
        print("evaluated")
        return 10
     }
  -> func foo(x: Int = shout()) { print("x is \(x)") }
  -> foo(x: 100)
  << x is 100
  -> foo()
  << evaluated
  << x is 10
  -> foo()
  << evaluated
  << x is 10
  ```
-->

### 특별한 종류의 메서드 (Special Kinds of Methods)

열거형이나 구조체 메서드에서
`self`를 수정하는 메서드는 `mutating` 선언 수정자로 표시되어야 합니다.

상위 클래스 메서드를 재정의한 메서드는
`override` 선언 수정자로 표시되어야 합니다.
`override` 수정자 없이 메서드를 재정의하거나
상위 클래스 메서드를 재정의하지 않는 메서드에
`override` 수정자를 사용하면 컴파일 오류가 발생합니다.

타입의 인스턴스가 아닌
타입과 관련된 메서드는
열거형과 구조체의 경우 `static` 선언 수정자로 표시되어야 하고
클래스의 경우 `static`이나 `class` 선언 수정자로 표시되어야 합니다.
`class` 선언 수정자로 표시된 클래스 타입 메서드는
하위 클래스 구현에서 재정의할 수 있습니다;
`class final`이나 `static`으로 표시된 클래스 타입 메서드는 재정의할 수 없습니다.

<!--
  - test: `overriding-class-methods-err`

  ```swifttest
  -> class S { class final func f() -> Int { return 12 } }
  -> class SS: S { override class func f() -> Int { return 120 } }
  !$ error: class method overrides a 'final' class method
  !! class SS: S { override class func f() -> Int { return 120 } }
  !!                                  ^
  !$ note: overridden declaration is here
  !! class S { class final func f() -> Int { return 12 } }
  !!                           ^
  -> class S2 { static func f() -> Int { return 12 } }
  -> class SS2: S2 { override static func f() -> Int { return 120 } }
  !$ error: cannot override static method
  !! class SS2: S2 { override static func f() -> Int { return 120 } }
  !! ^
  !$ note: overridden declaration is here
  !! class S2 { static func f() -> Int { return 12 } }
  !! ^
  ```
-->

<!--
  - test: `overriding-class-methods`

  ```swifttest
  -> class S3 { class func f() -> Int { return 12 } }
  -> class SS3: S3 { override class func f() -> Int { return 120 } }
  -> print(SS3.f())
  <- 120
  ```
-->

### 특별한 이름의 메서드 (Methods with Special Names)

특별한 이름을 가지는 메서드는
함수 호출 구문을 위한 편의 문법(syntactic sugar)을 가능하게 합니다.
타입을 이러한 메서드 중 하나를 정의하면,
타입의 인스턴스를 함수 호출 구문에서 사용할 수 있습니다.
이런 함수 호출은 해당 인스턴스의
특별한 이름을 가진 메서드 중 하나를 호출합니다.

클래스, 구조체, 열거형 타입은
[dynamicCallable](./attributes.md#dynamiccallable)에서 설명한대로
`dynamicallyCall(withArguments:)` 메서드나
`dynamicallyCall(withKeywordArguments:)` 메서드를 정의하거나,
아래에서 설명한대로 call-as-function 메서드를 정의하여
함수 호출 구문을 제공할 수 있습니다.
타입이
call-as-function 메서드와
`dynamicCallable` 속성에 의해 사용되는 메서드 중 하나를 모두 정의하면,
컴파일러는 두 메서드를 사용할 수 있는 환경에서
call-as-function 메서드를 우선시합니다.

call-as-function 메서드의 이름은 `callAsFunction()`이나
레이블이 없어도 되지만 추가해도 되는
`callAsFunction(`으로 시작하는 다른 이름도 있습니다 —--
예를 들어 `callAsFunction(_:_:)`와 `callAsFunction(something:)`도
call-as-function 메서드 이름으로 유효합니다.

<!--
  Above, callAsFunction( is in code voice even though
  it's not actually a symbol that exists in the reader's code.
  Per discussion with Chuck, this is the closest typographic convention
  to what we're trying to express here.
-->

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
  - test: `call-as-function`

  ```swifttest
  -> struct CallableStruct {
         var value: Int
         func callAsFunction(_ number: Int, scale: Int) {
             print(scale * (number + value))
         }
     }
  -> let callable = CallableStruct(value: 100)
  -> callable(4, scale: 2)
  -> callable.callAsFunction(4, scale: 2)
  // Both function calls print 208.
  << 208
  << 208
  ```
-->

call-as-function 메서드와
`dynamicCallable` 속성의 메서드는
타입 시스템에 얼마나 많은 정보를 인코딩하는지와
런타임에 얼마나 많은 동적 동작이 가능한지에 대해
서로 다른 장단점을 가집니다.
call-as-function 메서드를 선언할 때,
인자의 수와
인자의 타입과 레이블을 지정합니다.
`dynamicCallable` 속성의 메서드는
인자의 배열을 보유하기 위해 사용된 타입만 지정합니다.

call-as-function 메서드나
`dynamicCallable` 속성의 메서드를 정의하는 것은
함수 호출 표현식 외 다른 컨텍스트에서
타입의 인스턴스를 함수처럼 사용할 수 없습니다.
예를 들어:

```swift
let someFunction1: (Int, Int) -> Void = callable(_:scale:)  // Error
let someFunction2: (Int, Int) -> Void = callable.callAsFunction(_:scale:)
```

<!--
  - test: `call-as-function-err`

  ```swifttest
  >> struct CallableStruct {
  >>     var value: Int
  >>     func callAsFunction(_ number: Int, scale: Int) { }
  >> }
  >> let callable = CallableStruct(value: 100)
  -> let someFunction1: (Int, Int) -> Void = callable(_:scale:)  // Error
  -> let someFunction2: (Int, Int) -> Void = callable.callAsFunction(_:scale:)
  >> _ = someFunction1 // suppress unused-constant warning
  >> _ = someFunction2 // suppress unused-constant warning
  !$ error: cannot find 'callable(_:scale:)' in scope
  !! let someFunction1: (Int, Int) -> Void = callable(_:scale:)  // Error
  !! ^~~~~~~~~~~~~~~~~~
  ```
-->

`subscript(dynamicMember:)` 서브스크립트는
[dynamicMemberLookup](./attributes.md#dynamicmemberlookup)에서 설명한대로
멤버 조회를 위한 편의 문법을 활성화 합니다.

### 오류를 던지는 함수와 메서드 (Throwing Functions and Methods)

오류를 던질 수 있는 함수와 메서드는 `throws` 키워드로 표시되어야 합니다.
이러한 함수와 메서드를 *던지는 함수(throwing functions)*와
*던지는 메서드(throwing methods)*라고 합니다.
형식은 다음과 같습니다:

```swift
func <#function name#>(<#parameters#>) throws -> <#return type#> {
   <#statements#>
}
```

특정 오류 타입을 던지는 함수는 다음의 형식을 가집니다:

```swift
func <#function name#>(<#parameters#>) throws(<#error type#>) -> <#return type#> {
   <#statements#>
}
```

오류를 던지는 함수나 메서드 호출은 `try`이나 `try!` 표현식으로 래핑되어야 합니다
(즉, `try`이나 `try!` 연산자의 범위).

함수의 타입에는 오류를 던질 수 있는지와
어떤 타입의 오류를 던지는지 포함합니다.
예를 들어, 이러한 하위 타입 관계는 오류를 던지는 함수가 예상되는 컨텍스트에서
오류를 던지지 않는 함수를 사용할 수 있다는 의미입니다.
던지는 함수의 타입에 대한 자세한 내용은
[함수 타입 (Function Type)](./types.md#함수-타입-function-type)을 참고바랍니다.
명시적 타입의 오류를 다루는 예시는
[오류 타입 지정 (Specifying the Error Type)](../language-guide-1/error-handling.md#오류-타입-지정-specifying-the-error-type)을 참고바랍니다.

함수가 오류를 던질 수 있는지 여부만으로 함수를 오버로드할 수 없습니다.
이 말은
함수 매개변수가 오류를 던질 수 있는 여부에 따라 함수를 오버로드 할 수는 있습니다.

오류를 던지는 메서드는 오류를 던지지 않는 메서드를 재정의할 수 없고,
오류를 던지는 메서드는 오류를 던지지 않는 메서드에 대한 프로토콜 요구사항을 충족할 수 없습니다.
이 말은 오류를 던지지 않는 메서드는 오류를 던지는 메서드를 재정의할 수 있고,
오류를 던지지 않는 메서드는 오류를 던지는 메서드에 대한 프로토콜 요구사항을 충족할 수 있습니다.

### 오류를 다시 던지는 함수와 메서드 (Rethrowing Functions and Methods)

함수나 메서드는 함수 매개변수 중 하나만 오류를 발생하는 것을 나타내기 위해
`rethrows` 키워드로 선언할 수 있습니다.
이러한 함수와 메서드를 *다시 던지는 함수(rethrowing functions)*와
*다시 던지는 메서드(rethrowing methods)*라고 합니다.
다시 던지는 함수와 메서드는
적어도 하나의 던지는 함수 매개변수를 가져야 합니다.

```swift
func someFunction(callback: () throws -> Void) rethrows {
    try callback()
}
```

<!--
  - test: `rethrows`

  ```swifttest
  -> func someFunction(callback: () throws -> Void) rethrows {
         try callback()
     }
  ```
-->

오류를 다시 던지는 함수나 메서드는
`catch` 절 안에만 `throw` 구문을 포함할 수 있습니다.
이렇게 하면 `do`-`catch` 구문 내에서 오류를 던지는 함수를 호출하고
`catch` 절에서 다른 오류를 발생시켜 오류를 처리할 수 있습니다.
또한
`catch` 절은
다시 던지는 함수의 매개변수 중
하나에서 발생한 오류만 처리해야 합니다.
예를 들어 다음은
`catch` 절이 `alwaysThrows()`에서
던져진 오류를 처리하므로 유효하지 않습니다.

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
  - test: `double-negative-rethrows`

  ```swifttest
  >> enum SomeError: Error { case error }
  >> enum AnotherError: Error { case error }
  -> func alwaysThrows() throws {
         throw SomeError.error
     }
  -> func someFunction(callback: () throws -> Void) rethrows {
        do {
           try callback()
           try alwaysThrows()  // Invalid, alwaysThrows() isn't a throwing parameter
        } catch {
           throw AnotherError.error
        }
     }
  !$ error: a function declared 'rethrows' may only throw if its parameter does
  !!               throw AnotherError.error
  !!               ^
  ```
-->

<!--
  - test: `throwing-in-rethrowing-function`

  ```swifttest
  -> enum SomeError: Error { case c, d }
  -> func f1(callback: () throws -> Void) rethrows {
         do {
             try callback()
         } catch {
             throw SomeError.c  // OK
         }
     }
  -> func f2(callback: () throws -> Void) rethrows {
         throw SomeError.d  // Error
     }
  !$ error: a function declared 'rethrows' may only throw if its parameter does
  !! throw SomeError.d  // Error
  !! ^
  ```
-->

오류를 던지는 메서드는 오류를 다시 던지는 메서드를 재정의할 수 없고,
오류를 던지는 메서드는 오류를 다시 던지는 메서드에 대한 프로토콜 요구사항을 충족할 수 없습니다.
이 말은 오류를 다시 던지는 메서드는 오류를 던지는 메서드를 재정의할 수 있고,
오류를 다시 던지는 메서드는 오류를 던지는 메서드에 대한 프로토콜 요구사항을 충족할 수 있습니다.

오류를 다시 던지는 것의 대안은 제네릭 코드에서 특정 오류 타입을 던지는 것입니다.
예를 들어:

```swift
func someFunction<E: Error>(callback: () throws(E) -> Void) throws(E) {
    try callback()
}
```

오류를 전파하는 이러한 방식은
오류에 대한 타입 정보를 유지합니다.
그러나 `rethrows` 함수와 다르게,
이 방식은 동일한 타입의 오류를 던지도록
함수를 보호하지 않습니다.

<!--
TODO: Revisit the comparison between rethrows and throws(E) above,
since it seems likely that the latter will generally replace the former.

See also rdar://128972373
-->

### 비동기 함수와 메서드 (Asynchronous Functions and Methods)

비동기적으로 실행되는 함수와 메서드는 `async` 키워드로 표시되어야 합니다.
이 함수와 메서드를 *비동기 함수(asynchronous functions)*와
*비동기 메서드(asynchronous methods)*라고 합니다.
형식은 다음과 같습니다:

```swift
func <#function name#>(<#parameters#>) async -> <#return type#> {
   <#statements#>
}
```

비동기 함수나 메서드에 대한 호출은
`await` 표현식으로 래핑되어야 합니다 —--
즉, `await` 연산자 범위 안에 있어야 합니다.

`async` 키워드는 함수 타입의 일부이고,
동기 함수는 비동기 함수의 하위 타입입니다.
결과적으로 비동기 함수 컨텍스트에서
동기 함수를 사용할 수 있습니다.
예를 들어
비동기 메서드를 동기 메서드로 재정의할 수 있으며,
동기 메서드는 비동기 메서드가 필요한
프로토콜 요구사항을 충족할 수 있습니다.

비동기 함수인지 아닌지에 따라 함수를 오버로드할 수 있습니다.
호출 부분에서 컨텍스트에 따라 사용될 오버로드를 결정합니다:
비동기 컨텍스트에서는 비동기 함수가 사용되고,
동기 컨텍스트에서는 동기 함수가 사용됩니다.

비동기 메서드는 동기 메서드를 재정의할 수 없으며,
비동기 메서드는 동기 메서드에 대한 프로토콜 요구사항을 충족할 수 없습니다.
이 말은, 동기 메서드는 비동기 메서드를 재정의할 수 있으며,
동기 메서드는 비동기 메서드에 대한 프로토콜 요구사항을 충족할 수 있습니다.

<!--
  - test: `sync-satisfy-async-protocol-requirements`

  ```swifttest
  >> protocol P { func f() async -> Int }
  >> class Super: P {
  >>     func f() async -> Int { return 12 }
  >> }
  >> class Sub: Super {
  >>     func f() -> Int { return 120 }
  >> }
  ```
-->

### 반환하지 않는 함수 (Functions that Never Return)

Swift는 [`Never`][] 타입을 정의하여
함수나 메서드가 호출자에게 반환하지 않음을 나타냅니다.
`Never` 반환 타입이 있는 함수와 메서드는 *비반환(nonreturning)*이라고 합니다.
비반환 함수와 메서드는 복구할 수 없는 오류를 발생하거나
무한으로 계속되는 작업을 시작합니다.
이것은
호출 직후에 실행될 코드가 실행되지 않음을 뜻합니다.
비반환 함수나 메서드인 경우에도
적절한 `catch` 블록으로 프로그램 제어를 전달할 수 있습니다.

[`Never`]: https://developer.apple.com/documentation/swift/never

비반환 함수나 메서드는
[Guard 구문 (Guard Statement)](./statements.md#guard-구문-guard-statement)에서 설명한대로
guard 구문의 `else` 절을 끝낼 수 있습니다.

비반환 메서드를 재정의할 수 있지만
새로운 메서드는 반환 타입과 비반환 동작을 유지해야 합니다.

> Grammar of a function declaration:
>
> *function-declaration* → *function-head* *function-name* *generic-parameter-clause*_?_ *function-signature* *generic-where-clause*_?_ *function-body*_?_
>
> *function-head* → *attributes*_?_ *declaration-modifiers*_?_ **`func`** \
> *function-name* → *identifier* | *operator*
>
> *function-signature* → *parameter-clause* **`async`**_?_ *throws-clause*_?_ *function-result*_?_ \
> *function-signature* → *parameter-clause* **`async`**_?_ **`rethrows`** *function-result*_?_ \
> *function-result* → **`->`** *attributes*_?_ *type* \
> *function-body* → *code-block*
>
> *parameter-clause* → **`(`** **`)`** | **`(`** *parameter-list* **`)`** \
> *parameter-list* → *parameter* | *parameter* **`,`** *parameter-list* \
> *parameter* → *external-parameter-name*_?_ *local-parameter-name* *parameter-type-annotation* *default-argument-clause*_?_ \
> *parameter* → *external-parameter-name*_?_ *local-parameter-name* *parameter-type-annotation* \
> *parameter* → *external-parameter-name*_?_ *local-parameter-name* *parameter-type-annotation* **`...`**
>
> *external-parameter-name* → *identifier* \
> *local-parameter-name* → *identifier* \
> *parameter-type-annotation* → **`:`** *attributes*_?_ *parameter-modifier*_?_ *type* \
> *parameter-modifier* → **`inout`** | **`borrowing`** | **`consuming`**
> *default-argument-clause* → **`=`** *expression*

<!--
  NOTE: Code block is optional in the context of a protocol.
  Everywhere else, it's required.
  We could refactor to have a separation between function definition/declaration.
  There's also the low-level "asm name" FFI
  which is a definition and declaration corner case.
  Let's just deal with this difference in prose.
-->

## 열거형 선언 (Enumeration Declaration)

*열거형 선언(enumeration declaration)*은 프로그램에 열거형 타입을 도입합니다.

열거형 선언은 두 가지 기본 형식을 가지고 있고 `enum` 키워드를 사용하여 선언합니다.
두 형식 중 하나를 사용하여 선언된 열거형의 본문은
열거형 케이스(enumeration case)라고 불리는 0개 이상의 값과
연산 프로퍼티,
인스턴스 메서드, 타입 메서드, 이니셜라이저, 타입 별칭,
다른 열거형, 구조체, 클래스, 액터
선언을 포함할 수 있습니다.
열거형 선언은 디이니셜라이저나 프로토콜 선언을 포함할 수 없습니다.

열거형 타입은 여러 프로토콜을 채택할 수 있지만
클래스, 구조체, 다른 열거형을 상속할 수 없습니다.

클래스나 구조체와 다르게
열거형 타입은 암시적으로 제공된 기본 이니셜라이저를 가지지 않습니다;
모든 이니셜라이저는 명시적으로 선언해야 합니다.
이니셜라이저는 열거형 내에 다른 이니셜라이저로 위임할 수 있지만,
초기화 과정은 이니셜라이저에서 열거형 케이스 중 하나를 `self`에 할당한 후에 완료되어야만 합니다.

구조체와 비슷하지만 클래스와 달리 열거형은 값 타입입니다;
열거형의 인스턴스는 변수나 상수에 할당될 때나
함수 호출의 인자로 전달될 때 복사됩니다.
값 타입에 대한 자세한 내용은
[구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)](../language-guide-1/structures-and-classes.md#구조체와-열거형은-값-타입-structures-and-enumerations-are-value-types)을 참고바랍니다.

[확장 선언 (Extension Declaration)](#확장-선언-extension-declaration)에서 설명한대로
확장 선언으로 열거형 타입의 동작을 확장할 수 있습니다.

### 임의의 타입을 가지는 케이스가 있는 열거형 (Enumerations with Cases of Any Type)

다음의 형식은 임의의 타입을 가지는 열거형 케이스를
포함하는 열거형 타입을 선언합니다:

```swift
enum <#enumeration name#>: <#adopted protocols#> {
    case <#enumeration case 1#>
    case <#enumeration case 2#>(<#associated value types#>)
}
```

이 형식으로 선언된 열거형을
다른 프로그래밍 언어에서는 *태그된 유니온(discriminated unions)*이라고 합니다.

이 형식에서 각 케이스 블록은 `case` 키워드 다음에
콤마로 구분된 하나 이상의 열거형 케이스로 구성됩니다.
각 케이스의 이름은 고유해야 합니다.
각 케이스는 주어진 타입의 값을 저장하도록 지정할 수도 있습니다.
이러한 타입은 케이스의 이름 바로 다음에
*연관값 타입(associated value type)* 튜플로 지정합니다.

연관값을 저장하는 열거형 케이스는 지정된 연관값을 사용하여 열거형의 인스턴스를 생성하는
함수로 사용할 수 있습니다.
함수와 마찬가지로
코드에 열거형 케이스에 대한 참조를 가져오고 적용할 수 있습니다.

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
  - test: `enum-case-as-function`

  ```swifttest
  -> enum Number {
        case integer(Int)
        case real(Double)
     }
  -> let f = Number.integer
  -> // f is a function of type (Int) -> Number

  -> // Apply f to create an array of Number instances with integer values
  -> let evenInts: [Number] = [0, 2, 4, 6].map(f)
  ```
-->

<!--
  No expectation for evenInts because there isn't a good way to spell one.
  Using print() puts a module prefix like tmpabc in front of Number
  so the expectation would need to be a regex (which we don't have),
  and assert() would require Number to conform to Equatable.
-->

연관값 타입을 사용하는 케이스의 자세한 내용과 예시는
[연관값 (Associated Values)](./enumerations.md#연관값-associated-values)을 참고바랍니다.

#### 간접 참조 열거형 (Enumerations with Indirection)

열거형은 재귀적 구조를 가질 수 있습니다.
이 말은 열거형 타입 자체의 인스턴스인
연관값을 사용하는 케이스를 가질 수 있다는 의미입니다.
그러나 열거형 타입의 인스턴스는 값 시맨틱(value semantic)을 가집니다.
이것은 메모리에 고정된 레이아웃을 가지고 있다는 의미입니다.
재귀를 지원하려면
컴파일러는 간접 참조 계층을 삽입해야 합니다.

특정 열거형 케이스에 대한 간접 참조를 사용하려면,
`indirect` 선언 수정자로 표시해야 합니다.
간접 케이스는 연관값을 가지고 있어야 합니다.

<!--
  TODO The word "enable" is kind of a weasel word.
  Better to have a more concrete discussion of exactly when
  it is and isn't used.
  For example, does "indirect enum { X(Int) } mark X as indirect?
-->

```swift
enum Tree<T> {
    case empty
    indirect case node(value: T, left: Tree, right: Tree)
}
```

<!--
  - test: `indirect-enum`

  ```swifttest
  -> enum Tree<T> {
        case empty
        indirect case node(value: T, left: Tree, right: Tree)
     }
  >> let l1 = Tree.node(value: 10, left: Tree.empty, right: Tree.empty)
  >> let l2 = Tree.node(value: 100, left: Tree.empty, right: Tree.empty)
  >> let t = Tree.node(value: 50, left: l1, right: l2)
  ```
-->

연관값을 가진
열거형의 모든 케이스에 대해 간접 참조를 사용하려면
전체 열거형을 `indirect` 수정자로 표시합니다 —--
이것은 열거형 케이스에 각각 `indirect` 수정자를 표시해야 될
케이스를 많이 포함하는 경우 편리합니다.

`indirect` 수정자로 표시된 열거형은
연관값을 가진 케이스와 연관값을 가지지 않는 케이스를 혼합해 포함할 수 있습니다.
이 말은
`indirect` 수정자로 표시된 케이스는 포함할 수 없다는 의미입니다.

<!--
  It really should be an associated value **that includes the enum type**
  but right now the compiler is satisfied with any associated value.
  Alex emailed Joe Groff 2015-07-08 about this.
-->

<!--
  assertion indirect-in-indirect

  -> indirect enum E { indirect case c(E) }
  !! <REPL Input>:1:19: error: enum case in 'indirect' enum cannot also be 'indirect'
  !! indirect enum E { indirect case c(E) }
  !!                   ^
-->

<!--
  assertion indirect-without-recursion

  -> enum E { indirect case c }
  !! <REPL Input>:1:10: error: enum case 'c' without associated value cannot be 'indirect'
  !! enum E { indirect case c }
  !!          ^

  -> enum E1 { indirect case c() }     // This is fine, but probably shouldn't be
  -> enum E2 { indirect case c(Int) }  // This is fine, but probably shouldn't be

  -> indirect enum E3 { case x }
-->

### 원시값 타입의 케이스를 가진 열거형 (Enumerations with Cases of a Raw-Value Type)

다음의 형식은 동일한 기본 타입의 열거형 케이스를
포함하는 열거형 타입을 선언합니다:

```swift
enum <#enumeration name#>: <#raw-value type#>, <#adopted protocols#> {
    case <#enumeration case 1#> = <#raw value 1#>
    case <#enumeration case 2#> = <#raw value 2#>
}
```

이 형식에서 각 케이스 블록은 `case` 키워드
다음에 콤마로 구분된 하나 이상의 열거형 케이스로 구성됩니다.
첫 번째 형식의 케이스와 달리 각 케이스는
동일한 기본 타입의 *원시값(raw value)*이라는 기본값을 가집니다.
이러한 값의 타입은 *원시값 타입(raw-value type)*으로 지정되고
정수, 부동소수점 숫자, 문자열, 단일 문자를 나타내야 합니다.
특히 *원시값 타입(raw-value type)*은 `Equatable` 프로토콜과
다음 프로토콜 중 하나를 준수해야 합니다:
정수 리터럴에 대한 `ExpressibleByIntegerLiteral`,
부동소수점 리터럴에 대한 `ExpressibleByFloatLiteral`,
여러 문자를 포함하는 문자열 리터럴에 대한 `ExpressibleByStringLiteral`,
단일 문자만 포함하는 문자열 리터럴에 대한
`ExpressibleByUnicodeScalarLiteral`
또는 `ExpressibleByExtendedGraphemeClusterLiteral`.
각 케이스는 고유한 이름을 가지고 고유한 원시값이 할당되어야 합니다.

<!--
  The list of ExpressibleBy... protocols above also appears in LexicalStructure_Literals.
  This list is shorter because these five protocols are explicitly supported in the compiler.
-->

원시값 타입이 `Int`로 지정되고
케이스에 명시적으로 값을 할당하지 않으면,
이것은 암시적으로 값 `0`, `1`, `2` 등으로 할당합니다.
타입 `Int`의 각 할당되지 않은 케이스는
이전 케이스의 원시값에서 자동으로 증가된 원시값으로 할당됩니다.

```swift
enum ExampleEnum: Int {
    case a, b, c = 5, d
}
```

<!--
  - test: `raw-value-enum`

  ```swifttest
  -> enum ExampleEnum: Int {
        case a, b, c = 5, d
     }
  ```
-->

위의 예시에서 `ExampleEnum.a`의 원시값은 `0`이고
`ExampleEnum.b`의 값은 `1`입니다.
그리고 `ExampleEnum.c`의 값은 명시적으로 `5`로 설정되어 있으므로
`ExampleEnum.d`의 값은 `5`에서 자동으로 증가되어 `6`입니다.

원시값 타입이 `String`으로 지정되고
명시적으로 케이스에 값을 할당하지 않으면,
각 할당되지 않은 케이스는 암시적으로 케이스의 이름과 동일한 텍스트 문자열로 할당됩니다.

```swift
enum GamePlayMode: String {
    case cooperative, individual, competitive
}
```

<!--
  - test: `raw-value-enum-implicit-string-values`

  ```swifttest
  -> enum GamePlayMode: String {
        case cooperative, individual, competitive
     }
  ```
-->

위의 예시에서 `GamePlayMode.cooperative`의 원시값은 `"cooperative"`,
`GamePlayMode.individual`의 원시값은 `"individual"`,
`GamePlayMode.competitive`의 원시값은 `"competitive"`입니다.

암시적으로 원시값 타입의 케이스를 가지는 열거형은
Swift 표준 라이브러리에 정의된 `RawRepresentable` 프로토콜을 준수합니다.
결과적으로 `rawValue` 프로퍼티를 가지고 있고
`init?(rawValue: RawValue)`인 실패 가능한 이니셜라이저를 가집니다.
`ExampleEnum.b.rawValue`처럼
열거형 케이스의 원시값에 접근하기 위해 `rawValue` 프로퍼티를 사용할 수 있습니다.
옵셔널 케이스를 반환하는 `ExampleEnum(rawValue: 5)`와 같이
열거형의 실패 가능한 이니셜라이저를 호출하여
해당 케이스를 찾기 위해 원시값을 사용할 수도 있습니다.
원시값 타입이 있는 케이스의 자세한 내용과 예시는
[원시값 (Raw Values)](../language-guide-1/enumerations.md#원시값-raw-values)을 참고바랍니다.

### 열거형 케이스 접근 (Accessing Enumeration Cases)

열거형 타입의 케이스를 참조하기 위해
`EnuerationType.enumerationCase`와 같이 점(`.`) 구문을 사용합니다.
열거형 타입을 컨텍스트에서 추론할 수 있으면 [열거형 문법 (Enumeration Syntax)](../language-guide-1/enumerations.md#열거형-문법-enumeration-syntax)과
[암시적 멤버 표현식 (Implicit Member Expression)](./expressions.md#암시적-멤버-표현식-implicit-member-expression)에서 설명한대로
점은 그대로 유지한 채 생략할 수 있습니다.

열거형 케이스의 값을 확인하려면 [스위치 구문에서 열거형 값 일치 (Matching Enumeration Values with a Switch Statement)](../language-guide-1/enumerations.md#스위치-구문에서-열거형-값-일치-matching-enumeration-values-with-a-switch-statement)에서 보았듯이
`switch` 구문을 사용합니다.
열거형 타입은 [열거형 케이스 패턴 (Enumeration Case Pattern)](./patterns.md#열거형-케이스-패턴-enumeration-case-pattern)에서 설명한대로
`switch` 구문의 케이스 블록에서
열거형 케이스 패턴과 일치됩니다.

<!--
  FIXME: Or use if-case:
  enum E { case c(Int) }
  let e = E.c(100)
  if case E.c(let i) = e { print(i) }
  // prints 100
-->

<!--
  NOTE: Note that you can require protocol adoption,
  by using a protocol type as the raw-value type,
  but you do need to make it be one of the types
  that support = in order for you to specify the raw values.
  You can have: <#raw-value type, protocol conformance#>.
  UPDATE: You can only have one raw-value type specified.
  I changed the grammar to be more restrictive in light of this.
-->

<!--
  NOTE: Per Doug and Ted, "('->' type)?" isn't part of the grammar.
  We removed it from our grammar, below.
-->

> Grammar of an enumeration declaration:
>
> *enum-declaration* → *attributes*_?_ *access-level-modifier*_?_ *union-style-enum* \
> *enum-declaration* → *attributes*_?_ *access-level-modifier*_?_ *raw-value-style-enum*
>
> *union-style-enum* → **`indirect`**_?_ **`enum`** *enum-name* *generic-parameter-clause*_?_ *type-inheritance-clause*_?_ *generic-where-clause*_?_ **`{`** *union-style-enum-members*_?_ **`}`** \
> *union-style-enum-members* → *union-style-enum-member* *union-style-enum-members*_?_ \
> *union-style-enum-member* → *declaration* | *union-style-enum-case-clause* | *compiler-control-statement* \
> *union-style-enum-case-clause* → *attributes*_?_ **`indirect`**_?_ **`case`** *union-style-enum-case-list* \
> *union-style-enum-case-list* → *union-style-enum-case* | *union-style-enum-case* **`,`** *union-style-enum-case-list* \
> *union-style-enum-case* → *enum-case-name* *tuple-type*_?_ \
> *enum-name* → *identifier* \
> *enum-case-name* → *identifier*
>
> *raw-value-style-enum* → **`enum`** *enum-name* *generic-parameter-clause*_?_ *type-inheritance-clause* *generic-where-clause*_?_ **`{`** *raw-value-style-enum-members* **`}`** \
> *raw-value-style-enum-members* → *raw-value-style-enum-member* *raw-value-style-enum-members*_?_ \
> *raw-value-style-enum-member* → *declaration* | *raw-value-style-enum-case-clause* | *compiler-control-statement* \
> *raw-value-style-enum-case-clause* → *attributes*_?_ **`case`** *raw-value-style-enum-case-list* \
> *raw-value-style-enum-case-list* → *raw-value-style-enum-case* | *raw-value-style-enum-case* **`,`** *raw-value-style-enum-case-list* \
> *raw-value-style-enum-case* → *enum-case-name* *raw-value-assignment*_?_ \
> *raw-value-assignment* → **`=`** *raw-value-literal* \
> *raw-value-literal* → *numeric-literal* | *static-string-literal* | *boolean-literal*

<!--
  NOTE: The two types of enums are sufficiently different enough to warrant separating
  the grammar accordingly. ([Contributor 6004] pointed this out in his email.)
  I'm not sure I'm happy with the names I've chosen for two kinds of enums,
  so please let me know if you can think of better names (Tim and Dave are OK with them)!
  I chose union-style-enum, because this kind of enum behaves like a discriminated union,
  not like an ordinary enum type. They're a kind of "sum" type in the language
  of ADTs (Algebraic Data Types). Functional languages, like F# for example,
  actually have both types (discriminated unions and enumeration types),
  because they behave differently. I'm not sure why we've blended them together,
  especially given that they have distinct syntactic declaration requirements
  and they behave differently.
-->

## 구조체 선언 (Structure Declaration)

*구조체 선언(structure declaration)*은 프로그램에 구조체 타입을 도입합니다.
구조체 선언은 `struct` 키워드를 사용하여 선언하고 아래의 형식을 가집니다:

```swift
struct <#structure name#>: <#adopted protocols#> {
   <#declarations#>
}
```

구조체의 본문은 0개 이상의 *선언(declarations)*을 포함합니다.
이러한 위 형식의 *선언(declarations)*은 저장 프로퍼티와 연산 프로퍼티,
타입 프로퍼티, 인스턴스 메서드, 타입 메서드, 이니셜라이저, 서브스크립트,
타입 별칭, 다른 구조체, 클래스, 액터, 열거형 선언을 포함할 수 있습니다.
구조체 선언은 디이니셜라이저나 프로토콜 선언을 포함할 수 없습니다.
여러 종류의 선언을 포함한
구조체에 대한 자세한 내용과 예시는
[구조체와 클래스 (Classes And Structures)](../language-guide-1/structures-and-classes.md)를 참고바랍니다.

구조체 타입은 여러 프로토콜을 채택할 수 있지만
클래스, 열거형, 다른 구조체를 상속할 수 없습니다.

이전에 선언된 구조체의 인스턴스를 생성하는 방법은 세 가지가 있습니다:

- [이니셜라이저 (Initializers)](../language-guide-1/initialization.md#이니셜라이저-initializers)에서 설명한대로
  구조체 내 선언된 이니셜라이저 중 하나를 호출합니다.
- 선언된 이니셜라이저가 없는 경우
  [구조체 타입의 멤버와이즈 이니셜라이저 (Memberwise Initializers for Structure Types)](../language-guide-1/initialization.md#구조체-타입의-멤버와이즈-이니셜라이저-memberwise-initializers-for-structure-types)에서 설명한대로
  구조체의 멤버별 이니셜라이저를 호출합니다.
- 선언된 이니셜라이저가 없고
  구조체 선언의 모든 프로퍼티에 초기값이 주어진 경우
  [기본 이니셜라이저 (Default Initializers)](../language-guide-1/initialization.md#기본-이니셜라이저-default-initializers)에서 설명한대로
  구조체의 기본 이니셜라이저를 호출합니다.

구조체에 선언된 프로퍼티 초기화 과정은
[초기화 (Initialization)](../language-guide-1/initialization.md)에 설명되어 있습니다.

구조체 인스턴스의 프로퍼티는 [프로퍼티 접근 (Accessing Properties)](../language-guide-1/structures-and-classes.md#프로퍼티-접근-accessing-properties)에서 설명한대로
점(`.`) 구문을 사용하여 접근할 수 있습니다.

구조체는 값 타입 입니다; 변수나 상수에 할당되거나
함수 호출의 인자로 전달될 때 구조체의 인스턴스는 복사됩니다.
값 타입에 대한 자세한 내용은
[구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)](../language-guide-1/structures-and-classes.md#구조체와-열거형은-값-타입-structures-and-enumerations-are-value-types)을 참고바랍니다.

[확장 선언 (Extension Declaration)](#확장-선언-extension-declaration)에서 설명한대로
확장 선언으로 구조체 타입의 동작을 확장할 수 있습니다.

> Grammar of a structure declaration:
>
> *struct-declaration* → *attributes*_?_ *access-level-modifier*_?_ **`struct`** *struct-name* *generic-parameter-clause*_?_ *type-inheritance-clause*_?_ *generic-where-clause*_?_ *struct-body* \
> *struct-name* → *identifier* \
> *struct-body* → **`{`** *struct-members*_?_ **`}`**
>
> *struct-members* → *struct-member* *struct-members*_?_ \
> *struct-member* → *declaration* | *compiler-control-statement*

## 클래스 선언 (Class Declaration)

*클래스 선언(class declaration)*은 프로그램에 클래스 타입을 도입합니다.
클래스 선언은 `class` 키워드를 사용하여 선언하고 다음의 형식을 가집니다:

```swift
class <#class name#>: <#superclass#>, <#adopted protocols#> {
   <#declarations#>
}
```

클래스의 본문은 0개 이상의 *선언(declarations)*을 포함합니다.
이러한 위 형식의 *선언(declarations)*은 저장 프로퍼티와 연산 프로퍼티,
인스턴스 메서드, 타입 메서드, 이니셜라이저,
하나의 디이니셜라이저, 서브스크립트, 타입 별칭,
다른 클래스, 구조체, 액터, 열거형 선언을 포함할 수 있습니다.
클래스 선언은 프로토콜 선언을 포함할 수 없습니다.
여러 종류의 선언을 포함하는
클래스의 자세한 설명과 예시는
[구조체와 클래스 (Classes And Structures)](../language-guide-1/structures-and-classes.md)를 참고바랍니다.

클래스 타입은 *상위 클래스(superclass)*로 하나의 부모 클래스만 상속할 수 있지만,
프로토콜은 여러 개 채택할 수 있습니다.
*상위 클래스(superclass)*는 위 형식의 *클래스 이름(class name)*과 콜론 다음에
첫 번째로 나타나고 다음으로 *채택된 프로토콜(adopted protocols)*이 나타납니다.
제네릭 클래스(generic class)는 다른 제네릭 클래스와 제네릭이 아닌 클래스를 상속할 수 있지만,
제네릭이 아닌 클래스(nongeneric class)는 다른 제네릭이 아닌 클래스에서만 상속할 수 있습니다.
콜론 뒤에 상위 제네릭 클래스의 이름을 작성할 때,
제네릭 매개변수 절을 포함하는
제네릭 클래스의 전체 이름을 포함해야 합니다.

[이니셜라이저 선언 (Initializer Declaration)](#이니셜라이저-선언-initializer-declaration)에서 설명한대로
클래스는 지정 이니셜라이저(designated initializers)와 편의 이니셜라이저(convenience initializers)를 가질 수 있습니다.
클래스의 지정 이니셜라이저는 모든 클래스의 선언된 프로퍼티가 초기화되어야 하고,
상위 클래스의 지정 이니셜라이저를
호출하기 전에 수행되어야 합니다.

클래스는 상위 클래스의 프로퍼티, 메서드, 서브스크립트, 이니셜라이저를 재정의할 수 있습니다.
재정의한 프로퍼티, 메서드, 서브스크립트,
지정 이니셜라이저는 `override` 선언 수정자로 표시되어야 합니다.

<!--
  - test: `designatedInitializersRequireOverride`

  ```swifttest
  -> class C { init() {} }
  -> class D: C { override init() { super.init() } }
  ```
-->

하위 클래스가 상위 클래스의 이니셜라이저 구현을 요구하려면
상위 클래스의 이니셜라이저를 `required` 선언 수정자로 표시해야 합니다.
하위 클래스의 해당 이니셜라이저 구현도
`required` 선언 수정자로 표시되어야 합니다.

*상위 클래스(superclass)*에 선언된 프로퍼티와 메서드가 현재 클래스에 의해 상속되더라도
*상위 클래스(superclass)*에 선언된 지정 이니셜라이저는
[자동 이니셜라이저 상속 (Automatic Initializer Inheritance)](../language-guide-1/initialization.md#자동-이니셜라이저-상속-automatic-initializer-inheritance)에서 설명한대로
하위 클래스가 조건이 충족될 때만 상속됩니다.
Swift 클래스는 범용 기본 클래스는 상속하지 않습니다.

이전에 선언된 클래스의 인스턴스를 생성하는 두 가지 방법이 있습니다:

- [이니셜라이저 (Initializers)](../language-guide-1/initialization.md#이니셜라이저-initializers)에서 설명한대로
  클래스 내 선언된 이니셜라이저 중 하나를 호출합니다.
- 선언된 이니셜라이저가 없고
  클래스 선언에 모든 프로퍼티에 기본값이 주어진 경우
  [기본 이니셜라이저 (Default Initializers)](../language-guide-1/initialization.md#기본-이니셜라이저-default-initializers)에서 설명한대로
  클래스의 기본 이니셜라이저를 호출합니다.

[프로퍼티 접근 (Accessing Properties)](../language-guide-1/structures-and-classes.md#프로퍼티-접근-accessing-properties)에 설명한대로
점(`.`) 구문으로 클래스 인스턴스의 프로퍼티에 접근할 수 있습니다.

클래스는 참조 타입입니다; 클래스의 인스턴스는 변수나 상수에 할당되거나
함수 호출의 인자로 전달될 때 복사가 아닌 참조됩니다.
참조 타입에 대한 자세한 내용은
[클래스는 참조 타입 (Classes Are Reference Types)](../language-guide-1/structures-and-classes.md#클래스는-참조-타입-classes-are-reference-types)을 참고바랍니다.

[확장 선언 (Extension Declaration)](#확장-선언-extension-declaration)에서 설명한대로
확장 선언으로 클래스 타입의 동작을 확장할 수 있습니다.

> Grammar of a class declaration:
>
> *class-declaration* → *attributes*_?_ *access-level-modifier*_?_ **`final`**_?_ **`class`** *class-name* *generic-parameter-clause*_?_ *type-inheritance-clause*_?_ *generic-where-clause*_?_ *class-body* \
> *class-declaration* → *attributes*_?_ **`final`** *access-level-modifier*_?_ **`class`** *class-name* *generic-parameter-clause*_?_ *type-inheritance-clause*_?_ *generic-where-clause*_?_ *class-body* \
> *class-name* → *identifier* \
> *class-body* → **`{`** *class-members*_?_ **`}`**
>
> *class-members* → *class-member* *class-members*_?_ \
> *class-member* → *declaration* | *compiler-control-statement*

## 액터 선언 (Actor Declaration)

*액터 선언(actor declaration)*은 프로그램에 액터 타입을 도입됩니다.
액터 선언은 `actor` 키워드를 사용하여 선언되고 다음의 형식을 가집니다:

```swift
actor <#actor name#>: <#adopted protocols#> {
    <#declarations#>
}
```

액터의 본문에는 0개 이상의 *선언(declarations)*을 포함합니다.
이러한 위 형식의 *선언(declarations)*은 저장 프로퍼티와 연산 프로퍼티,
인스턴스 메서드, 타입 메서드, 이니셜라이저,
하나의 디이니셜라이저, 서브스크립트, 타입 별칭,
다른 클래스, 구조체, 열거형 선언을 포함할 수 있습니다.
여러 종류의 선언을 포함하는
액터의 자세한 설명과 예시는
[액터 (Actors)](../language-guide-1/concurrency.md#액터-actors)를 참고바랍니다.

액터 타입은 프로토콜을 채택할 수 있지만,
클래스, 열거형, 구조체, 다른 액터를 상속할 수 없습니다.
그러나 `@objc` 속성으로 표시된 액터는
암시적으로 `NSObjectProtocol` 프로토콜을 준수하고
`NSObject`의 하위 타입으로 Objective-C 런타임에 노출됩니다.

이전에 선언된 액터의 인스턴스를 생성하는 두 가지 방법이 있습니다:

- [이니셜라이저 (Initializers)](../language-guide-1/initialization.md#이니셜라이저-initializers)에서 설명한대로
  액터 내 선언한 이니셜라이저 중 하나를 호출합니다.
- 선언된 이니셜라이저가 없고
  액터 선언의 모든 프로퍼티에 초기값이 제공된 경우
  [기본 이니셜라이저 (Default Initializers)](../language-guide-1/initialization.md#기본-이니셜라이저-default-initializers)에서 설명한대로
  액터의 기본 이니셜라이저를 호출합니다.

기본적으로 액터의 멤버는 해당 액터와 분리됩니다.
메서드의 본문이나 프로퍼티의 getter와 같은 코드는
해당 액터에서 실행됩니다.
액터 내의 코드는 이미 동일한 액터에서 실행되고 있기 때문에
동기적으로 상호작용할 수 있지만,
액터 외부의 코드는 이 코드가 다른 액터에서 비동기적으로 코드를 실행하고 있음을 나타내기 위해
`await`로 표시해야 합니다.
키 경로는 액터의 격리된 멤버를 참조할 수 없습니다.
액터에 격리된 저장 프로퍼티(Actor-isolated stored properties)는 동기 함수에
in-out 매개변수로 전달할 수 있지만,
비동기 함수에는 전달할 수 없습니다.

액터는 `nonisolated` 키워드로 선언된
비격리 멤버(nonisolated members)를 가질 수도 있습니다.
비격리 멤버는 액터 외부의 코드처럼 실행됩니다:
액터의 격리된 상태와 상호작용할 수 없으며
호출자는 이를 사용할 때 `await`로 표시하지 않습니다.

액터의 멤버는 비격리 멤버이거나 비동기 멤버인 경우에만
`@objc` 속성으로 표시할 수 있습니다.

액터의 선언된 프로퍼티를 초기화하는 과정은
[초기화 (Initialization)][초기화 (Initialization)](../language-guide-1/initialization.md))에 설명되어 있습니다.

액터 인스턴스의 프로퍼티는 [프로퍼티 접근 (Accessing Properties)](../language-guide-1/structures-and-classes.md#프로퍼티-접근-accessing-properties)에서 설명한대로
점(`.`) 구문을 사용하여 접근할 수 있습니다.

액터는 참조 타입입니다; 액터의 인스턴스는
변수나 상수에 할당되거나 함수 호출의 인자로 전달될 때 복사되지 않고 참조됩니다.
참조 타입에 대한 자세한 내용은
[클래스는 참조 타입 (Classes Are Reference Types)](../language-guide-1/structures-and-classes.md#클래스는-참조-타입-classes-are-reference-types)을 참고바랍니다.

[확장 선언 (Extension Declaration)](#확장-선언-extension-declaration)에서 설명한대로
확장 선언을 사용하여 액터 타입의 동작을 확장할 수 있습니다.

<!--
  TODO Additional bits from the SE-0306 actors proposal:

  Partial applications of isolated functions are only permitted
  when the expression is a direct argument
  whose corresponding parameter is non-escaping and non-Sendable.
-->

> Grammar of an actor declaration:
>
> *actor-declaration* → *attributes*_?_ *access-level-modifier*_?_ **`actor`** *actor-name* *generic-parameter-clause*_?_ *type-inheritance-clause*_?_ *generic-where-clause*_?_ *actor-body* \
> *actor-name* → *identifier* \
> *actor-body* → **`{`** *actor-members*_?_ **`}`**
>
> *actor-members* → *actor-member* *actor-members*_?_ \
> *actor-member* → *declaration* | *compiler-control-statement*

## 프로토콜 선언 (Protocol Declaration)

*프로토콜 선언(protocol declaration)*은 프로그램에 프로토콜 타입을 도입합니다.
프로토콜 선언은 `protocol` 키워드를 사용하여
전역에 선언되고 다음의 형식을 가집니다:

```swift
protocol <#protocol name#>: <#inherited protocols#> {
   <#protocol member declarations#>
}
```

프로토콜 선언은 전역 범위에서 나타나거나
제네릭이 아닌 타입이나 제네릭이 아닌 함수 내에 중첩될 수 있습니다.

프로토콜의 본문은 0개 이상의 *프로토콜 멤버 선언(protocol member declarations)*을 포함하고,
이것은 프로토콜을 채택하는 모든 타입이 충족해야 하는 준수 요구사항을 설명합니다.
특히 프로토콜은 준수하는 타입이
특정 프로퍼티, 메서드, 이니셜라이저, 서브스크립트를 구현해야 한다고 선언할 수 있습니다.
프로토콜은 *연관 타입(associated types)*이라는
특별한 종류의 타입 별칭도 선언할 수 있으며,
이것은 프로토콜의 다양한 선언의 관계를 명시할 수 있습니다.
프로토콜 선언은
클래스, 구조체, 열거형, 다른 프로토콜 선언을 포함할 수 없습니다.
*프로토콜 멤버 선언(protocol member declarations)*은 아래에 자세하게 설명되어 있습니다.

프로토콜 타입은 다른 프로토콜을 상속할 수 있습니다.
프로토콜 타입이 다른 프로토콜을 상속할 때,
다른 프로토콜의 요구사항이 통합되고
현재 프로토콜을 상속하는 모든 타입은 이 모든 요구사항을 준수해야 합니다.
프로토콜 상속을 사용하는 방법에 대한 예시는
[프로토콜 상속 (Protocol Inheritance)](../language-guide-1/protocols.md#프로토콜-상속-protocol-inheritance)을 참고바랍니다.

> Note: [프로토콜 합성 타입 (Protocol Composition Type)](./types.md#프로토콜-합성-타입-protocol-composition-type)과
> [프로토콜 합성 (Protocol Composition)](../language-guide-1/protocols.md#프로토콜-합성-protocol-composition)에서 설명한대로
> 프로토콜 합성 타입을 사용하여
> 여러 프로토콜의 준수 요구사항을 통합할 수도 있습니다.

이전에 선언된 타입의 확장 선언을 사용하여 프로토콜을 채택하고
프로토콜 준수를 추가할 수 있습니다.
확장에서 채택된 프로토콜의 모든 요구사항을 구현해야 합니다.
타입이 이미 모든 요구사항을 구현한 경우,
확장 선언의 본문을 비워둘 수 있습니다.

기본적으로 프로토콜을 준수하는 타입은 프로토콜에 선언된
모든 프로퍼티, 메서드, 서브스크립트를 구현해야 합니다.
그러나 이 프로토콜 멤버 선언을 `optional` 선언 수정자로 표시하여
준수하는 타입의 구현이 선택 사항임을 명시할 수 있습니다.
`optional` 수정자는
`objc` 속성으로 표시된 멤버와
`objc` 속성으로 표시된 프로토콜의 멤버에만 적용할 수 있습니다.
결과적으로 클래스 타입만 이 옵셔널 멤버 요구사항을 포함한
프로토콜을 채택하고 준수할 수 있습니다.
`optional` 선언 수정자 사용에 대한 자세한 내용과
옵셔널 프로토콜 멤버에 접근하는 방법에 대한 자침 —--
예를 들어 타입이 이를 구현하는지 확실하지 않은 경우 —--
는 [옵셔널 프로토콜 요구사항 (Optional Protocol Requirements)](../language-guide-1/protocols.md#옵셔널-프로토콜-요구사항-optional-protocol-requirements)을 참고바랍니다.

<!--
  TODO: Currently, you can't check for an optional initializer,
  so we're leaving those out of the documentation, even though you can mark
  an initializer with the @optional attribute. It's still being decided by the
  compiler team. Update this section if they decide to make everything work
  properly for optional initializer requirements.
-->

열거형의 케이스는
타입 멤버에 대해 프로토콜 요구사항을 충족할 수 있습니다.
구체적으로
연관값이 없는 열거형 케이스는
`Self` 타입의 get-only 타입 변수에 대한
프로토콜 요구사항을 충족하고,
연관값이 있는 열거형 케이스는
매개변수와 인자 레이블이
케이스의 연관값과 일치하는
`Self`를 반환하는 함수에 대한 프로토콜 요구사항을 충족합니다.
예를 들어:

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
  - test: `enum-case-satisfy-protocol-requirement`

  ```swifttest
  -> protocol SomeProtocol {
         static var someValue: Self { get }
         static func someFunction(x: Int) -> Self
     }
  -> enum MyEnum: SomeProtocol {
         case someValue
         case someFunction(x: Int)
     }
  ```
-->

프로토콜의 채택을 클래스 타입으로만 제한하려면
콜론 뒤에
*상속된 프로토콜(inherited protocols)* 목록에 `AnyObject` 프로토콜을 추가해야 합니다.
예를 들어 다음의 프로토콜은 클래스 타입에 의해서만 채택될 수 있습니다:

```swift
protocol SomeProtocol: AnyObject {
    /* Protocol members go here */
}
```

<!--
  - test: `protocol-declaration`

  ```swifttest
  -> protocol SomeProtocol: AnyObject {
         /* Protocol members go here */
     }
  ```
-->

`AnyObject` 요구사항으로 표시된 프로토콜을 상속하는 모든 프로토콜은
마찬가지로 클래스 타입만 채택할 수 있습니다.

> Note: `objc` 속성으로 표시된 프로토콜은
> 해당 프로토콜에 암시적으로 `AnyObject` 요구사항을 적용합니다;
> 명시적으로 `AnyObject` 요구사항을 프로토콜에 표시할 필요가 없습니다.

프로토콜은 명명 타입 이므로 [프로토콜을 타입으로 사용 (Protocols as Types)](../language-guide-1/protocols.md#프로토콜을-타입으로-사용-protocols-as-types)에서 설명한대로
다른 명명 타입과 동일한 위치의 코드에 나타날 수 있습니다.
그러나
프로토콜은 지정하는
요구사항에 대한 구현을 실제로 제공하지 않으므로,
프로토콜의 인스턴스를 생성할 수 없습니다.

[위임 (Delegation)](../language-guide-1/protocols.md#위임-delegation)에서 설명한대로
프로토콜을 사용하여 클래스나 구조체의 위임이 구현해야 하는 메서드를 선언할 수 있습니다.

> Grammar of a protocol declaration:
>
> *protocol-declaration* → *attributes*_?_ *access-level-modifier*_?_ **`protocol`** *protocol-name* *type-inheritance-clause*_?_ *generic-where-clause*_?_ *protocol-body* \
> *protocol-name* → *identifier* \
> *protocol-body* → **`{`** *protocol-members*_?_ **`}`**
>
> *protocol-members* → *protocol-member* *protocol-members*_?_ \
> *protocol-member* → *protocol-member-declaration* | *compiler-control-statement*
>
> *protocol-member-declaration* → *protocol-property-declaration* \
> *protocol-member-declaration* → *protocol-method-declaration* \
> *protocol-member-declaration* → *protocol-initializer-declaration* \
> *protocol-member-declaration* → *protocol-subscript-declaration* \
> *protocol-member-declaration* → *protocol-associated-type-declaration* \
> *protocol-member-declaration* → *typealias-declaration*

### 프로토콜 프로퍼티 선언 (Protocol Property Declaration)

프로토콜은 프로토콜 선언 본문에
*프로토콜 프로퍼티 선언(protocol property declaration)*을 포함하여
준수하는 타입이 프로퍼티를 구현해야 한다고 선언합니다.
프로토콜 프로퍼티 선언은 변수 선언의
특별한 형식을 가집니다:

```swift
var <#property name#>: <#type#> { get set }
```

다른 프로토콜 멤버 선언과 마찬가지로 이러한 프로퍼티 선언은
프로토콜을 준수하는 타입에 대한 getter와 setter 요구사항만 선언합니다.
결과적으로 선언된 프로토콜 내에 직접적으로
getter와 setter를 구현하지 않습니다.

getter와 setter 요구사항은 다양한 방법으로 준수하는 타입에서 충족될 수 있습니다.
프로퍼티 선언이 `get`과 `set` 키워드를 모두 포함하면,
준수하는 타입은 변수 저장 프로퍼티나
읽기와 쓰기가 모두 가능한 연산 프로퍼티
(즉, getter와 setter를 모두 구현하는 프로퍼티)로 구현할 수 있습니다.
그러나 해당 프로퍼티 선언은 상수 프로퍼티나
읽기-전용 연산 프로퍼티로 구현될 수 없습니다.
프로퍼티 선언이 `get` 키워드만 포함한다면, 모든 종류의 프로퍼티로 구현할 수 있습니다.
프로토콜의 프로퍼티 요구사항을 구현하는 준수 타입의 예시는
[프로퍼티 요구사항 (Property Requirements)](../language-guide-1/protocols.md#프로퍼티-요구사항-property-requirements)을 참고바랍니다.

프로토콜 선언에 타입 프로퍼티 요구사항을 선언하려면
`static` 키워드로 프로퍼티 선언을 표시합니다.
프로토콜을 준수하는 구조체와 열거형은
`static` 키워드로 프로퍼티를 선언하고
프로토콜을 준수하는 클래스는
`static`이나 `class` 키워드로 프로퍼티를 선언합니다.
구조체, 열거형, 클래스에 프로토콜 준수를 추가하는 확장은
확장하는 타입이 사용하는 것과 동일한 키워드를 사용합니다.
타입 프로퍼티 요구사항에 대해 기본 구현을 제공하는 확장은
`static` 키워드를 사용합니다.

<!--
  - test: `protocols-with-type-property-requirements`

  ```swifttest
  -> protocol P { static var x: Int { get } }
  -> protocol P2 { class var x: Int { get } }
  !$ error: class properties are only allowed within classes; use 'static' to declare a requirement fulfilled by either a static or class property
  !! protocol P2 { class var x: Int { get } }
  !!              ~~~~~ ^
  !!              static
  -> struct S: P { static var x = 10 }
  -> class C1: P { static var x = 20 }
  -> class C2: P { class var x = 30 }
  !$ error: class stored properties not supported in classes; did you mean 'static'?
  !! class C2: P { class var x = 30 }
  !!               ~~~~~     ^
  ```
-->

<!--
  - test: `protocol-type-property-default-implementation`

  ```swifttest
  -> protocol P { static var x: Int { get } }
  -> extension P { static var x: Int { return 100 } }
  -> struct S1: P { }
  -> print(S1.x)
  <- 100
  -> struct S2: P { static var x = 10 }
  -> print(S2.x)
  <- 10
  ```
-->

[변수 선언 (Variable Declaration)](#변수-선언-variable-declaration)도 참고바랍니다.

> Grammar of a protocol property declaration:
>
> *protocol-property-declaration* → *variable-declaration-head* *variable-name* *type-annotation* *getter-setter-keyword-block*

### 프로토콜 메서드 선언 (Protocol Method Declaration)

프로토콜은 프로토콜 선언 본문에 프로토콜 메서드 선언을 포함하여
준수하는 타입이 메서드를 구현해야 한다고 선언합니다.
프로토콜 메서드 선언은 두 가지를 제외하고
함수 선언과 동일한 형식을 가집니다: 함수 본문과
함수 선언의 일부로 기본 매개변수 값을 제공할 수 없습니다.
프로토콜의 메서드 요구사항을 구현하는 준수 타입에 대한 예시는
[메서드 요구사항 (Method Requirements)](../language-guide-1/protocols.md#메서드-요구사항-method-requirements)을 참고바랍니다.

프로토콜 선언에 클래스 메서드나 정적 메서드 요구사항을 선언하려면
`static` 선언 수정자로 메서드 선언을 표시합니다.
프로토콜을 준수하는 구조체와 열거형은
`static` 키워드로 메서드를 선언하고
프로토콜을 준수하는 클래스는
`static`이나 `class` 키워드로 메서드를 선언합니다.
구조체, 열거형, 클래스에 프로토콜 준수를 추가하는 확장은
확장하는 타입이 사용하는 것과 동일한 키워드를 사용합니다.
타입 메서드 요구사항에 대한 기본 구현을 제공하는 확장은
`static` 키워드를 사용합니다.

[함수 선언 (Function Declaration)](#함수-선언-function-declaration)도 참고바랍니다.

<!--
  TODO: Talk about using ``Self`` in parameters and return types.
-->

> Grammar of a protocol method declaration:
>
> *protocol-method-declaration* → *function-head* *function-name* *generic-parameter-clause*_?_ *function-signature* *generic-where-clause*_?_

### 프로토콜 이니셜라이저 선언 (Protocol Initializer Declaration)

프로토콜은 프로토콜 선언 본문에 프로토콜 이니셜라이저 선언을 포함하여
준수하는 타입이 이니셜라이저를 구현해야 한다고 선언합니다.
프로토콜 이니셜라이저 선언은
이니셜라이저의 본문을 포함하지 않는다는 점을 제외하고 이니셜라이저 선언과 동일한 형식을 가집니다.

준수하는 타입은 실패하지 않는 이니셜라이저(nonfailable initializer)나 `init!` 실패 가능한 이니셜라이저(failable initializer)를 구현하여
실패하지 않는 프로토콜 이니셜라이저 요구사항을 충족할 수 있습니다.
준수하는 타입은 모든 종류의 이니셜라이저 구현으로
실패 가능한 프로토콜 이니셜라이저 요구사항을 충족할 수 있습니다.

클래스가 프로토콜의 이니셜라이저 요구사항을 충족하기 위해 이니셜라이저를 구현할 때,
클래스에 `final` 선언 수정자로 표시되어 있지 않다면
이니셜라이저는 `required` 선언 수정자로 표시되어야 합니다.

[이니셜라이저 선언 (Initializer Declaration)](#이니셜라이저-선언-initializer-declaration)도 참고바랍니다.

> Grammar of a protocol initializer declaration:
>
> *protocol-initializer-declaration* → *initializer-head* *generic-parameter-clause*_?_ *parameter-clause* *throws-clause*_?_ *generic-where-clause*_?_ \
> *protocol-initializer-declaration* → *initializer-head* *generic-parameter-clause*_?_ *parameter-clause* **`rethrows`** *generic-where-clause*_?_

### 프로토콜 서브스크립트 선언 (Protocol Subscript Declaration)

프로토콜은 프로토콜 선언 본문에 프로토콜 서브스크립트 선언을 포함하여
준수하는 타입이 서브스크립트를 구현해야 한다고 선언합니다.
프로토콜 서브스크립트 선언은 서브스크립트 선언의 특별한 형식을 가집니다:

```swift
subscript (<#parameters#>) -> <#return type#> { get set }
```

서브스크립트 선언은 프로토콜을 준수하는 타입에 대한
최소한의 getter와 setter 구현 요구사항만 선언합니다.
서브스크립트 선언에 `get`과 `set` 키워드 모두 포함하면,
준수하는 타입은 getter와 setter 절 모두 구현해야 합니다.
서브스크립트 선언이 `get` 키워드만 포함하면,
준수하는 타입은 *최소한* getter 절은 구현해야 하고
선택적으로 setter 절을 구현할 수 있습니다.

프로토콜 선언에서 정적 서브스크립트 요구사항을 선언하려면
`static` 선언 수정자로 서브스크립트 선언을 표시합니다.
프로토콜은 준수하는 구조체와 열거형은
`static` 키워드로 서브스크립트를 선언하고
프로토콜을 준수하는 클래스는
`static`이나 `class` 키워드로 서브스크립트를 선언합니다.
구조체, 열거형, 클래스에 프로토콜 준수를 추가하는 확장은
확장하는 타입이 사용하는 것과 동일한 키워드를 사용합니다.
정적 서브스크립트 요구사항에 대한 기본 구현을 제공하는 확장은
`static` 키워드를 사용합니다.

[서브스크립트 선언 (Subscript Declaration)](#서브스크립트-선언-subscript-declaration)도 참고바랍니다.

> Grammar of a protocol subscript declaration:
>
> *protocol-subscript-declaration* → *subscript-head* *subscript-result* *generic-where-clause*_?_ *getter-setter-keyword-block*

### 프로토콜 연관 타입 선언 (Protocol Associated Type Declaration)

프로토콜은 `associatedtype` 키워드를 사용하여 연관 타입을 선언합니다.
연관 타입은 프로토콜 선언의 일부분으로 사용되는
타입에 대한 별칭을 제공합니다.
연관 타입은 제네릭 매개변수 절의 타입 매개변수와 유사하지만
선언된 프로토콜의 `Self`와 연관됩니다.
해당 컨텍스트에서 `Self`는 프로토콜을 준수하는 최종 타입을 참조합니다.
더 자세한 내용과 예시는
[연관 타입 (Associated Types)](../language-guide-1/generics.md#연관-타입-associated-types)을 참고바랍니다.

프로토콜 선언에 제네릭 `where` 절을 사용하여
연관 타입을 재선언하지 않고
다른 프로토콜에서 상속된 연관 타입에 제약조건을 추가할 수 있습니다.
예를 들어 아래 `SubProtocol`의 선언은 동일합니다:

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
  - test: `protocol-associatedtype`

  ```swifttest
  -> protocol SomeProtocol {
         associatedtype SomeType
     }

  -> protocol SubProtocolA: SomeProtocol {
         // This syntax produces a warning.
         associatedtype SomeType: Equatable
     }
  !$ warning: redeclaration of associated type 'SomeType' from protocol 'SomeProtocol' is better expressed as a 'where' clause on the protocol
  !! associatedtype SomeType: Equatable
  !! ~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~
  !!-
  !$ note: 'SomeType' declared here
  !! associatedtype SomeType
  !! ^

  // This syntax is preferred.
  -> protocol SubProtocolB: SomeProtocol where SomeType: Equatable { }
  ```
-->

<!--
  TODO: Finish writing this section after WWDC.
-->

<!--
  NOTE:
  What are associated types? What are they "associated" with? Is "Self"
  an implicit associated type of every protocol? [...]

  Here's an initial stab:
  An Associated Type is associated with an implementation of that protocol.
  The protocol declares it, and is defined as part of the protocol's implementation.

  "The ``Self`` type allows you to refer to the eventual type of ``self``
  (where ``self`` is the type that conforms to the protocol).
  In addition to ``Self``, a protocol's operations often need to refer to types
  that are related to the type of ``Self``, such as a type of data stored in a
  collection or the node and edge types of a graph." Is this still true?

    -> If we expand the discussion here,
    -> add a link from Types_SelfType
    -> to give more details about Self in protocols.

  NOTES from Doug:
  At one point, Self was an associated type, but that's the wrong modeling of
  the problem.  Self is the stand-in type for the thing that conforms to the
  protocol.  It's weird to think of it as an associated type because it's the
  primary thing.  It's certainly not an associated type.  In many ways, you
  can think of associated types as being parameters that get filled in by the
  conformance of a specific concrete type to that protocol.

  There's a substitution mapping here.  The parameters are associated with
  Self because they're derived from Self.  When you have a concrete type that
  conforms to a protocol, it supplies concrete types for Self and all the
  associated types.

  The associated types are like parameters, but they're associated with Self in
  the protocol.  Self is the eventual type of the thing that conforms to the
  protocol -- you have to have a name for it so you can do things with it.

  We use "associated" in contrast with generic parameters in interfaces in C#.
  The interesting thing there is that they don't have a name like Self for the
  actual type, but you can name any of these independent types.    In theory,
  they're often independent but in practice they're often not -- you have an
  interface parameterized on T, where all the uses of the thing are that T are
  the same as Self.  Instead of having these independent parameters to an
  interface, we have a named thing (Self) and all these other things that hand
  off of it.

  Here's a stupid simple way to see the distinction:

  C#:

  interface Sequence <Element> {}

  class String : Sequence <UnicodeScalar>
  class String : Sequence <GraphemeCluster>

  These are both fine in C#

  Swift:

  protocol Sequence { typealias Element }

  class String : Sequence { typealias Element = ... }

  Here you have to pick one or the other -- you can't have both.
-->

[타입 별칭 선언 (Type Alias Declaration)](#타입-별칭-선언-type-alias-declaration)도 참고바랍니다.

> Grammar of a protocol associated type declaration:
>
> *protocol-associated-type-declaration* → *attributes*_?_ *access-level-modifier*_?_ **`associatedtype`** *typealias-name* *type-inheritance-clause*_?_ *typealias-assignment*_?_ *generic-where-clause*_?_

## 이니셜라이저 선언 (Initializer Declaration)

*이니셜라이저 선언(Initializer declaration)*은
프로그램에 클래스, 구조체, 열거형에 대한 이니셜라이저를 도입합니다.
이니셜라이저 선언은 `init` 키워드를 사용하여 선언하고
두 개의 기본 형식을 가지고 있습니다.

구조체, 열거형, 클래스 타입은 여러 개의 이니셜라이저를 가질 수 있지만,
클래스 이니셜라이저에 대한 규칙과 관련 동작은 다릅니다.
구조체와 열거형과 다르게 클래스는 두 종류의 이니셜라이저를 가집니다:
[초기화 (Initialization)][초기화 (Initialization)](../language-guide-1/initialization.md))에서 설명한대로
지정 이니셜라이저(designated initializers)와 편의 이니셜라이저(convenience initializers)가 있습니다.

다음 형식은 구조체, 열거형, 클래스의
지정 이니셜라이저에 대한 이니셜라이저를 선언합니다:

```swift
init(<#parameters#>) {
   <#statements#>
}
```

클래스의 지정 이니셜라이저는
클래스의 모든 프로퍼티를 직접 초기화합니다.
지정 이니셜라이저는 같은 클래스의 다른 이니셜라이저를 호출할 수 없고,
클래스가 상위 클래스를 가지고 있다면 상위 클래스의 지정 이니셜라이저 중 하나를 호출해야 합니다.
클래스가 상위 클래스에서 어떤 프로퍼티를 상속한다면,
상위 클래스의 지정 이니셜라이저 중 하나를 현재 클래스에서 해당 프로퍼티를 설정하거나
수정하기 전에 호출해야 합니다.

지정 이니셜라이저는 클래스 선언의 컨텍스트에서만 선언될 수 있으므로
확장 선언을 사용하는 클래스에 추가할 수 없습니다.

구조체와 열거형에 이니셜라이저는 선언된 다른 이니셜라이저를 호출하여
초기화 과정의 일부나 전체를 위임할 수 있습니다.

클래스에 편의 이니셜라이저를 선언하려면
`convenience` 선언 수정자로 이니셜라이저 선언을 표시합니다.

```swift
convenience init(<#parameters#>) {
   <#statements#>
}
```

편의 이니셜라이저는 초기화 과정을
다른 편의 이니셜라이저나 클래스의 지정 이니셜라이저 중 하나로 위임할 수 있습니다.
이 말은 초기화 과정은 궁극적으로 클래스의 프로퍼티를 초기화하는
지정 이니셜라이저를 호출로 끝나야 한다는 의미입니다.
편의 이니셜라이저는 상위 클래스의 이니셜라이저를 호출할 수 없습니다.

지정 이니셜라이저와 편의 이니셜라이저에 `required` 선언 수정자로 지정하여
모든 하위 클래스가 이니셜라이저를 구현하도록 요구할 수 있습니다.
해당 이니셜라이저의 하위 클래스의 구현도
`required` 선언 수정자로 표시되어야 합니다.

기본적으로 상위 클래스에 선언된 이니셜라이저는
하위 클래스에 의해 상속되지 않습니다.
이 말은 하위 클래스가 기본값으로 모든 저장 프로퍼티를 초기화하고
자체 이니셜라이저를 정의하지 않으면,
모든 상위 클래스의 이니셜라이저를 상속합니다.
하위 클래스가 모든 상위 클래스의 지정 이니셜라이저를 재정의하면
상위 클래스의 편의 이니셜라이저를 상속합니다.

메서드, 프로퍼티, 서브스크립트와 마찬가지로
`override` 선언 수정자로 재정의된 지정 이니셜라이저를 표시해야 합니다.

> Note: `required` 선언 수정자로 이니셜라이저를 표시하면,
> 하위 클래스에서 필수 이니셜라이저를 재정의할 때
> `override` 수정자로 이니셜라이저를 표시하지 않아도 됩니다.

함수와 메서드처럼 이니셜라이저는 오류를 던지거나 다시 던질 수 있습니다.
그리고 함수와 메서드처럼
이니셜라이저의 매개변수 뒤에 `throws`나 `rethrows` 키워드를 사용하여
적절한 동작을 나타낼 수 있습니다.
마찬가지로 이니셜라이저는 비동기일 수 있으며
이것을 나타내기 위해 `async` 키워드를 사용합니다.

여러 타입 선언에서 이니셜라이저의 예시는
[초기화 (Initialization)][초기화 (Initialization)](../language-guide-1/initialization.md))를 참고바랍니다.

### 실패 가능한 이니셜라이저 (Failable Initializers)

*실패 가능한 이니셜라이저(failable initializer)*는 이니셜라이저가 선언된 타입의 옵셔널 인스턴스나
암시적 언래핑된 옵셔널 인스턴스를 생성하는 이니셜라이저의 타입입니다.
결과적으로 실패 가능한 이니셜라이저는 초기화 실패를 나타내는 `nil`을 반환할 수 있습니다.

옵셔널 인스턴스를 생성하는 실패 가능한 이니셜라이저를 선언하려면
이니셜라이저 선언에서 `init` 키워드 뒤에 물음표를 붙입니다(`init?`).
암시적 언래핑 옵셔널 인스턴스를 생성하는 실패 가능한 이니셜라이저를 선언하려면
이니셜라이저 선언에서 `init` 키워드 뒤에 느낌표를 붙입니다(`init!`).
아래의 예시는 구조체의 옵셔널 인스턴스를 생성하는 실패 가능한 이니셜라이저 `init?`을 보여줍니다.

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
  - test: `failable`

  ```swifttest
  -> struct SomeStruct {
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
-->

결과의 선택성을 다루는 것을 제외하고
일반 이니셜라이저 호출과 동일하게 `init?`로 실패 가능한 이니셜라이저를 호출합니다.

```swift
if let actualInstance = SomeStruct(input: "Hello") {
    // do something with the instance of 'SomeStruct'
} else {
    // initialization of 'SomeStruct' failed and the initializer returned 'nil'
}
```

<!--
  - test: `failable`

  ```swifttest
  -> if let actualInstance = SomeStruct(input: "Hello") {
         // do something with the instance of 'SomeStruct'
  >>     _ = actualInstance
     } else {
         // initialization of 'SomeStruct' failed and the initializer returned 'nil'
     }
  ```
-->

실패 가능한 이니셜라이저는 이니셜라이저 구현의 어느 위치에서든
`nil`을 반환할 수 있습니다.

실패 가능한 이니셜라이저는 모든 종류의 이니셜라이저에 위임할 수 있습니다.
일반 이니셜라이저는 다른 일반 이니셜라이저가나
`init!` 실패 가능한 이니셜라이저로 위임할 수 있습니다.
일반 이니셜라이저는 `super.init()!`처럼
상위 클래스 이니셜라이저의 결과를 강제 언래핑함으로써
`init?` 실패 가능한 이니셜라이저에 위임할 수 있습니다.

초기화 실패는 이니셜라이저 위임으로 전파됩니다.
특히
실패 가능한 이니셜라이저가 실패하고 `nil`을 반환하는 이니셜라이저에 위임하면,
위임된 이니셜라이저도 실패하고 암시적으로 `nil`을 반환합니다.
일반 이니셜라이저가 실패하고 `nil`을 반환하는 `init!` 실패 가능한 이니셜라이저에 위임하면,
런타임 오류가 발생합니다
(`nil` 값을 가지는 옵셔널을 언래핑하기 위해 `!` 연산자를 사용하는 것과 동일합니다).

실패 가능한 지정 이니셜라이저는 하위 클래스에서
모든 종류의 지정 이니셜라이저로 재정의될 수 있습니다.
일반 지정 이니셜라이저는 하위 클래스에서
일반 지정 이니셜라이저로만 재정의될 수 있습니다.

실패 가능한 이니셜라이저에 대한 자세한 내용과 예시는
[실패 가능한 이니셜라이저 (Failable Initializers)](../language-guide-1/initialization.md#실패-가능한-이니셜라이저-failable-initializers)을 참고바랍니다.

> Grammar of an initializer declaration:
>
> *initializer-declaration* → *initializer-head* *generic-parameter-clause*_?_ *parameter-clause* **`async`**_?_ *throws-clause*_?_ *generic-where-clause*_?_ *initializer-body* \
> *initializer-declaration* → *initializer-head* *generic-parameter-clause*_?_ *parameter-clause* **`async`**_?_ **`rethrows`** *generic-where-clause*_?_ *initializer-body* \
> *initializer-head* → *attributes*_?_ *declaration-modifiers*_?_ **`init`** \
> *initializer-head* → *attributes*_?_ *declaration-modifiers*_?_ **`init`** **`?`** \
> *initializer-head* → *attributes*_?_ *declaration-modifiers*_?_ **`init`** **`!`** \
> *initializer-body* → *code-block*

## 디이니셜라이저 선언 (Deinitializer Declaration)

*디이니셜라이저 선언(deinitializer declaration)*은 클래스 타입에 대한 디이니셜라이저를 선언합니다.
디이니셜라이저는 매개변수를 가지지 않고 다음의 형식을 가집니다:

```swift
deinit {
   <#statements#>
}
```

디이니셜라이저는 클래스 객체에 어떠한 참조도 없으면,
클래스 객체가 할당 해제되기 직전에 자동으로 호출됩니다.
디이니셜라이저는 클래스 선언의 본문 내에만 선언될 수 있고 ---
클래스의 확장에는 선언될 수 없습니다 ---
그리고 각 클래스는 디이니셜라이저를 하나만 가질 수 있습니다.

하위 클래스는 하위 클래스 객체가 할당 해제되기 직전에
암시적으로 호출되는 상위 클래스의 디이니셜라이저를 상속합니다.
하위 클래스 객체는 상속 체인의 모든 디이니셜라이저가 실행을 완료할 때까지
할당 해제되지 않습니다.

디이니셜라이저는 직접 호출할 수 없습니다.

클래스 선언에서 디이니셜라이저가 어떻게 사용되는지에 대한 예시는
[소멸 (Deinitialization)](../language-guide-1/deinitialization.md)를 참고바랍니다.

> Grammar of a deinitializer declaration:
>
> *deinitializer-declaration* → *attributes*_?_ **`deinit`** *code-block*

## 확장 선언 (Extension Declaration)

*확장 선언(extension declaration)*은
기존 타입의 동작을 확장할 수 있습니다.
확장 선언은 `extension` 키워드를 사용하여 선언하고
다음의 형식을 가집니다:

```swift
extension <#type name#> where <#requirements#> {
   <#declarations#>
}
```

확장 선언의 본문은 0개 이상의 *선언(declarations)*을 포함합니다.
위 형식의 *선언(declaration)은 연산 프로퍼티, 연산 타입 프로퍼티,
인스턴스 메서드, 타입 메서드, 이니셜라이저, 서브스크립트 선언,
클래스, 구조체, 열거형 선언을 포함할 수 있습니다.
확장 선언은 디이니셜라이저나 프로토콜 선언,
저장 프로퍼티, 프로퍼티 관찰자, 다른 확장 선언은 포함할 수 없습니다.
프로토콜 확장에서 선언은 `final`로 표시할 수 없습니다.
여러 종류의 선언을 포함한 확장에 대한 내용과 예시는
[확장 (Extensions)](../language-guide-1/extensions.md)을 참고바랍니다.

위 형식의 *타입 이름(type name)*이 클래스, 구조체, 열거형 타입이면,
확장은 해당 타입을 확장합니다.
위 형식의 *타입 이름(type name)*이 프로토콜 타입이면,
확장은 해당 프로토콜을 준수하는 모든 타입을 확장합니다.

제네릭 타입이나
연관 타입이 있는 프로토콜을 확장하는 확장 선언은
위 형식의 *요구사항(requirements)*을 포함할 수 있습니다.
확장된 타입이나
확장된 프로토콜을 준수하는 타입의 인스턴스가
위 형식의 *요구사항(requirements)*을 충족하면
인스턴스는 선언에 지정된 동작을 얻습니다.

확장 선언은 이니셜라이저 선언을 포함할 수 있습니다.
이 말은 확장하려는 타입이 다른 모듈에 정의되어 있으면,
이니셜라이저 선언은 해당 타입의 멤버가 올바르게 초기화되도록
해당 모듈에 이미 정의된 이니셜라이저로 위임해야 합니다.

기존 타입의 프로퍼티, 메서드, 이니셜라이저는
해당 타입의 확장에서 재정의될 수 없습니다.

확장 선언은 아래 형식의 *채택된 프로토콜(adopted protocols)*을 지정하여
기존 클래스, 구조체, 열거형 타입에 프로토콜 준수를 추가할 수 있습니다:

```swift
extension <#type name#>: <#adopted protocols#> where <#requirements#> {
   <#declarations#>
}
```

확장 선언은 기존 클래스에 클래스 상속을 추가할 수 없으므로
위 형식의 *타입 이름(type name)*과 콜론 뒤에 프로토콜의 리스트만 지정할 수 있습니다.

### 조건부 준수성 (Conditional Conformance)

요구사항이 충족될 때만
해당 타입의 인스턴스가 프로토콜을 준수하도록
제네릭 타입을 확장하여
조건부로 프로토콜을 준수하게 할 수 있습니다.
확장 선언에 *요구사항(requirements)*을 추가하여
조건부 프로토콜 준수를 추가합니다.

#### 일부 제네릭 컨텍스트에서 재정의된 요구사항이 사용되지 않는 경우 (Overridden Requirements Aren’t Used in Some Generic Contexts)

일부 제네릭 컨텍스트에서
조건부 프로토콜 준수에서 동작을 가져오는 타입은
항상 해당 프로토콜 요구사항의
구현을 사용하지 않습니다.
이 동작을 설명하기 위해
다음 예시는 두 프로토콜과
두 프로토콜을 조건부로 준수하는 제네릭 타입을 정의합니다.

<!--
  This test needs to be compiled so that it will recognize Pair's
  CustomStringConvertible conformance -- the deprecated REPL doesn't
  seem to use the description property at all.
-->

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
  - test: `conditional-conformance`

  ```swifttest
  -> protocol Loggable {
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
-->

`Pair` 구조체는 제네릭 타입이 `Loggable`이나 `TitledLoggable`을 각각 준수할 때마다
`Loggable`과 `TitledLoggable`을 준수합니다.
아래 예시에서
`oneAndTwo`는 `Pair<String>`의 인스턴스이고,
`String`이 `TitledLoggable`을 준수하기 때문에
해당 인스턴스는 `TitledLoggable`을 준수합니다.
`oneAndTwo`에서 `log()` 메서드를 직접 호출하면,
타이틀 문자열을 포함하는 특별한 버전이 사용됩니다.

```swift
let oneAndTwo = Pair(first: "one", second: "two")
oneAndTwo.log()
// Prints "Pair of 'String': (one, two)"
```

<!--
  - test: `conditional-conformance`

  ```swifttest
  -> let oneAndTwo = Pair(first: "one", second: "two")
  -> oneAndTwo.log()
  <- Pair of 'String': (one, two)
  ```
-->

그러나 `oneAndTwo` 제네릭 컨텍스트에서 사용되거나
`Loggable` 프로토콜의 인스턴스로 사용되면,
특별한 버전이 사용되지 않습니다.
Swift는 `Pair`가 `Loggable` 준수하는데 필요한 최소 요구사항만 참조하여
`log()`의 어떤 구현을 호출할지 선택합니다.
이러한 이유로
`Loggable` 프로토콜에 의해 제공된 기본 구현이 대신 사용됩니다.

```swift
func doSomething<T: Loggable>(with x: T) {
    x.log()
}
doSomething(with: oneAndTwo)
// Prints "(one, two)"
```

<!--
  - test: `conditional-conformance`

  ```swifttest
  -> func doSomething<T: Loggable>(with x: T) {
        x.log()
     }
     doSomething(with: oneAndTwo)
  <- (one, two)
  ```
-->

`log()`가 `doSomething(_:)`에 전달된 인스턴스에서 호출되면,
커스텀된 타이틀은 로깅된 문자열에서 생략됩니다.

### 프로토콜 준수는 중복되지 않아야 함 (Protocol Conformance Must Not Be Redundant)

구체적인 타입은 특정 프로토콜을 한 번만 준수할 수 있습니다.
Swift는 중복 프로토콜 준수를 오류로 표시합니다.
이러한 오류는
두 가지 상황에서 발생할 수 있습니다.
첫 번째 상황은
동일한 프로토콜을 여러 번 명시적으로 준수하지만
요구사항이 다른 경우입니다.
두 번째 상황은
암시적으로 동일한 프로토콜을 여러 번 상속할 때 입니다.
이러한 상황은 아래 섹션에 설명되어 있습니다.

#### 명시적 중복 해결 (Resolving Explicit Redundancy)

구체적인 타입에 여러 확장은
확장의 요구사항이 독립적이더라도
동일한 프로토콜에 대한 준수를 추가할 수 없습니다.
이 제한은 아래 예시에서 설명됩니다.
두 확장 선언은
`Int` 요소가 있는 배열과
`String` 요소가 있는 배열에 대해
`Serializable` 프로토콜의 조건부 준수를 추가합니다.

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
  - test: `multiple-conformances`

  ```swifttest
  -> protocol Serializable {
        func serialize() -> Any
     }

     extension Array: Serializable where Element == Int {
         func serialize() -> Any {
             // implementation
  >>         return 0
  ->     }
     }
     extension Array: Serializable where Element == String {
         func serialize() -> Any {
             // implementation
  >>         return 0
  ->     }
     }
  // Error: redundant conformance of 'Array<Element>' to protocol 'Serializable'
  !$ error: conflicting conformance of 'Array<Element>' to protocol 'Serializable'; there cannot be more than one conformance, even with different conditional bounds
  !! extension Array: Serializable where Element == String {
  !! ^
  !$ note: 'Array<Element>' declares conformance to protocol 'Serializable' here
  !! extension Array: Serializable where Element == Int {
  !! ^
  ```
-->

여러 구체적인 타입을 기반으로 조건부 준수를 추가하려면,
각 타입이 준수하는 새로운 프로토콜을 생성하고
조건부 준수를 선언할 때 해당 프로토콜을 요구사항으로 사용합니다.

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

<!--
  - test: `multiple-conformances-success`

  ```swifttest
  >> protocol Serializable { }
  -> protocol SerializableInArray { }
     extension Int: SerializableInArray { }
     extension String: SerializableInArray { }

  -> extension Array: Serializable where Element: SerializableInArray {
         func serialize() -> Any {
             // implementation
  >>         return 0
  ->     }
     }
  ```
-->

#### 암시적 중복 해결 (Resolving Implicit Redundancy)

구체적인 타입이 프로토콜을 조건부로 준수하면,
해당 타입은 동일한 요구사항을 가진
모든 부모 프로토콜을 암시적으로 준수합니다.

단일 부모에서 상속하는
두 프로토콜을 조건부로 준수하는 타입이 필요한 경우,
명시적으로 부모 프로토콜에 대한 준수를 선언합니다.
이것은 요구사항이 다른 부모 프로토콜을
암시적으로 두 번 준수하는 것을 방지합니다.

다음 예시는 `TitledLoggable`과 새로운 `MarkedLoggable` 프로토콜을
조건부 준수를 선언할 때 충돌을 막기 위해
`Array`의 조건부 준수를 `Loggable`로
명시적으로 선언합니다.

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
  - test: `conditional-conformance`

  ```swifttest
  -> protocol MarkedLoggable: Loggable {
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
-->

`Loggable`에 대한 조건부 준수성을 명시적으로 선언하는
확장이 없으면
다른 `Array` 확장이 암시적으로 이러한 선언을 생성하여
오류가 발생합니다:

```swift
extension Array: Loggable where Element: TitledLoggable { }
extension Array: Loggable where Element: MarkedLoggable { }
// Error: redundant conformance of 'Array<Element>' to protocol 'Loggable'
```

<!--
  - test: `conditional-conformance-implicit-overlap`

  ```swifttest
  >> protocol Loggable { }
  >> protocol MarkedLoggable : Loggable { }
  >> protocol TitledLoggable : Loggable { }
  -> extension Array: Loggable where Element: TitledLoggable { }
     extension Array: Loggable where Element: MarkedLoggable { }
  // Error: redundant conformance of 'Array<Element>' to protocol 'Loggable'
  !$ error: conflicting conformance of 'Array<Element>' to protocol 'Loggable'; there cannot be more than one conformance, even with different conditional bounds
  !! extension Array: Loggable where Element: MarkedLoggable { }
  !! ^
  !$ note: 'Array<Element>' declares conformance to protocol 'Loggable' here
  !! extension Array: Loggable where Element: TitledLoggable { }
  !! ^
  ```
-->

<!--
  - test: `types-cant-have-multiple-implicit-conformances`

  ```swifttest
  >> protocol Loggable { }
     protocol TitledLoggable: Loggable { }
     protocol MarkedLoggable: Loggable { }
     extension Array: TitledLoggable where Element: TitledLoggable {
         // ...
     }
     extension Array: MarkedLoggable where Element: MarkedLoggable { }
  !$ error: conditional conformance of type 'Array<Element>' to protocol 'TitledLoggable' does not imply conformance to inherited protocol 'Loggable'
  !! extension Array: TitledLoggable where Element: TitledLoggable {
  !! ^
  !$ note: did you mean to explicitly state the conformance like 'extension Array: Loggable where ...'?
  !! extension Array: TitledLoggable where Element: TitledLoggable {
  !! ^
  !$ error: type 'Array<Element>' does not conform to protocol 'MarkedLoggable'
  !! extension Array: MarkedLoggable where Element: MarkedLoggable { }
  !! ^
  !$ error: type 'Element' does not conform to protocol 'TitledLoggable'
  !! extension Array: MarkedLoggable where Element: MarkedLoggable { }
  !! ^
  !$ error: 'MarkedLoggable' requires that 'Element' conform to 'TitledLoggable'
  !! extension Array: MarkedLoggable where Element: MarkedLoggable { }
  !! ^
  !$ note: requirement specified as 'Element' : 'TitledLoggable'
  !! extension Array: MarkedLoggable where Element: MarkedLoggable { }
  !! ^
  !$ note: requirement from conditional conformance of 'Array<Element>' to 'Loggable'
  !! extension Array: MarkedLoggable where Element: MarkedLoggable { }
  !! ^
  ```
-->

<!--
  - test: `extension-can-have-where-clause`

  ```swifttest
  >> extension Array where Element: Equatable {
         func f(x: Array) -> Int { return 7 }
     }
  >> let x = [1, 2, 3]
  >> let y = [10, 20, 30]
  >> let r0 = x.f(x: y)
  >> assert(r0 == 7)
  ```
-->

<!--
  - test: `extensions-can-have-where-clause-and-inheritance-together`

  ```swifttest
  >> protocol P { func foo() -> Int }
  >> extension Array: P where Element: Equatable {
  >>    func foo() -> Int { return 0 }
  >> }
  >> let r0 = [1, 2, 3].foo()
  >> assert(r0 == 0)
  ```
-->

> Grammar of an extension declaration:
>
> *extension-declaration* → *attributes*_?_ *access-level-modifier*_?_ **`extension`** *type-identifier* *type-inheritance-clause*_?_ *generic-where-clause*_?_ *extension-body* \
> *extension-body* → **`{`** *extension-members*_?_ **`}`**
>
> *extension-members* → *extension-member* *extension-members*_?_ \
> *extension-member* → *declaration* | *compiler-control-statement*

## 서브스크립트 선언 (Subscript Declaration)

*서브스크립트(subscript)* 선언은 특정 타입에 대한 서브스크립트 지원을 추가할 수 있고,
일반적으로 컬렉션, 리스트, 시퀀스의 요소에 접근하기 위한
편리한 구문을 제공하기 위해 사용됩니다.
서브스크립트 선언은 `subscript` 키워드를 사용하여 선언되고
다음의 형식을 가집니다:

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

서브스크립트 선언은 클래스, 구조체,
열거형, 확장, 프로토콜 선언의 컨텍스트에서만 나타날 수 있습니다.

위 형식의 *매개변수(parameters)*는 서브스크립트 표현식에서 해당 타입의 요소에 접근하기 위해 사용되는 하나 이상의 인덱스를 지정합니다
(예를 들어 표현식 `object[i]`에서 `i`).
요소에 접근하기 위해 사용되는 인덱스는 모든 타입이 될 수 있지만,
각 매개변수는 각 인덱스의 타입을 지정하기 위해 타입 주석을 포함해야 합니다.
위 형식의 *반환 타입(return type)*은 접근하는 요소의 타입을 지정합니다.

연산 프로퍼티와 마찬가지로
서브스크립트 선언은 접근한 요소의 값을 읽고 쓰기를 제공합니다.
getter는 값을 읽기 위해 사용되고,
setter는 값을 쓰기 위해 사용됩니다.
setter 절은 선택 사항이며,
getter만 필요하다면, 모든 절을 생략할 수 있고
요청된 값을 직접 반환할 수 있습니다.
이 말은 setter 절을 제공하면 getter 절을 제공해야 한다는 의미입니다.

위 형식의 *setter 이름(setter name)*과 둘러싸인 소괄호는 선택 사항입니다.
setter 이름을 제공하면, setter의 매개변수 이름으로 사용됩니다.
setter 이름을 제공하지 않으면, setter의 기본 매개변수 이름은 `newValue`입니다.
setter의 매개변수 타입은 위 형식의 *반환 타입(return type)*과 동일합니다.

*매개변수(parameters)*나 *반환 타입(return type)*이 오버로드 하려는 것과 다르면,
선언된 타입의 서브스크립트 선언을 오버로드할 수 있습니다.
상위 클래스에서 상속된 서브스크립트 선언을 재정의할 수도 있습니다.
재정의하면 `override` 선언 수정자로 재정의된 서브스크립트 선언을 표시해야 합니다.

서브스크립트 매개변수는 두 가지를 제외하고
함수 매개변수와 동일한 규칙을 따릅니다.
기본적으로 서브스크립트에서 사용하는 매개변수는 함수, 메서드, 이니셜라이저와 다르게
인자 레이블을 가지지 않습니다.
그러나 함수, 메서드, 이니셜라이저에서 사용하는 동일한 구문을 사용하여
명시적으로 인자 레이블을 제공할 수 있습니다.
또한 서브스크립트는 in-out 매개변수를 가질 수 없습니다.
서브스크립트 매개변수는 [특별한 종류의 매개변수 (Special Kinds of Parameters)](#특별한-종류의-매개변수-special-kinds-of-parameters)에서 설명한 구문을 사용하여
기본값을 가질 수 있습니다.

[프로토콜 서브스크립트 선언 (Protocol Subscript Declaration)](#프로토콜-서브스크립트-선언-protocol-subscript-declaration)에서 설명한대로
프로토콜 선언의 컨텍스트에서 서브스크립트를 선언할 수도 있습니다.

서브스크립트에 대한 자세한 내용과 서브스크립트 선언의 예시는
[서브스크립트 (Subscripts)](../language-guide-1/subscripts.md)를 참고바랍니다.

### 타입 서브스크립트 선언 (Type Subscript Declarations)

타입의 인스턴스가 아닌
타입에 의해 노출되는 서브스크립트를 선언하려면
`static` 선언 수정자로 서브스크립트 선언을 표시합니다.
클래스는 대신 `class` 선언 수정자로 타입 연산 프로퍼티를 표시하여
하위 클래스가 상위 클래스의 구현을 재정의할 수 있습니다.
클래스 선언에서
`static` 키워드는 `class`와 `final` 선언 수정자 모두 선언에 표시하는 것과
동일한 효과를 가집니다.

<!--
  - test: `cant-override-static-subscript-in-subclass`

  ```swifttest
  -> class Super { static subscript(i: Int) -> Int { return 10 } }
  -> class Sub: Super { override static subscript(i: Int) -> Int { return 100 } }
  !$ error: cannot override static subscript
  !! class Sub: Super { override static subscript(i: Int) -> Int { return 100 } }
  !!                                    ^
  !$ note: overridden declaration is here
  !! class Super { static subscript(i: Int) -> Int { return 10 } }
  !!                      ^
  ```
-->

> Grammar of a subscript declaration:
>
> *subscript-declaration* → *subscript-head* *subscript-result* *generic-where-clause*_?_ *code-block* \
> *subscript-declaration* → *subscript-head* *subscript-result* *generic-where-clause*_?_ *getter-setter-block* \
> *subscript-declaration* → *subscript-head* *subscript-result* *generic-where-clause*_?_ *getter-setter-keyword-block* \
> *subscript-head* → *attributes*_?_ *declaration-modifiers*_?_ **`subscript`** *generic-parameter-clause*_?_ *parameter-clause* \
> *subscript-result* → **`->`** *attributes*_?_ *type*

## 매크로 선언 (Macro Declaration)

*매크로 선언(macro declaration)*은 새로운 매크로를 도입합니다.
`macro` 키워드로 시작하고
다음의 형식을 가집니다:

```swift
macro <#name#> = <#macro implementation#>
```

위 형식의 *매크로 구현(macro implementation)*은 또 다른 매크로이며,
매크로의 확장을 수행하는 코드의 위치를 나타냅니다.
매크로 확장을 수행하는 코드는 Swift 코드와 상호작용하기 위해
[SwiftSyntax][] 모듈을 사용하는 별도의 Swift 프로그램 입니다.
Swift 표준 라이브러리의 `externalMacro(module:type:)` 매크로를 호출하고,
해당 매크로를 호출할 때 매크로의 구현을 포함하는 타입의 이름과
해당 타입을 포함하는 모듈의 이름을 전달합니다.

[SwiftSyntax]: https://github.com/swiftlang/swift-syntax

매크로는 함수에서 사용하는 동일한 모델에 따라
오버로드될 수 있습니다.
매크로 선언은 파일 범위에서만 나타납니다.

Swift에서 매크로 개요는 [매크로 (Macros)](../language-guide-1/macros.md)를 참고바랍니다.

> Grammar of a macro declaration:
>
> *macro-declaration* → *macro-head* *identifier* *generic-parameter-clause*_?_ *macro-signature* *macro-definition*_?_ *generic-where-clause* \
> *macro-head* → *attributes*_?_ *declaration-modifiers*_?_ **`macro`** \
> *macro-signature* → *parameter-clause* *macro-function-signature-result*_?_ \
> *macro-function-signature-result* → **`->`** *type* \
> *macro-definition* → **`=`** *expression*

## 연산자 선언 (Operator Declaration)

*연산자 선언(operator declaration)*은 프로그램에
새로운 중위(infix) 연산자, 접두(prefix) 연산자, 접미(postfix) 연산자를 도입하고
`operator` 키워드를 사용하여 선언합니다.

세 가지 다른 고정성(fixity)을 가진 연산자를 선언할 수 있습니다:
중위 연산자, 접두 연산자, 접미 연산자입니다.
연산자의 *고정성(fixity)*은 피연산자에 대한
연산자의 상대적인 위치를 지정합니다.

각 고정성에 대해
연산자 선언의 세 가지 기본 형식이 있습니다.
연산자의 고정성은 `operator` 키워드 전에
`infix`, `prefix`, `postfix` 선언 수정자로 연산자의 선언을 표시하여 지정합니다.
각 형식에서 연산자의 이름은 [연산자 (Operators)](./lexical-structure.md#연산자-operators)에서 정의된
연산자 문자만 포함할 수 있습니다.

다음 형식은 새로운 중위 연산자를 선언합니다:

```swift
infix operator <#operator name#>: <#precedence group#>
```

*중위 연산자(infix operator)*는 표현식 `1 + 2`에서 덧셈 연산자(`+`)와 같이
두 피연산자 사이에 작성되는 이진 연산자 입니다.

중위 연산자는 선택적으로 우선순위 그룹을 지정할 수 있습니다.
연산자에 대해 우선순위 그룹을 생략하면,
Swift는 기본 우선순위인 `DefaultPrecedence`를 사용하고,
이것은 `TernaryPrecedence`보다 약간 높은 우선순위입니다.
더 자세한 내용은 [우선순위 그룹 선언 (Precedence Group Declaration)](#우선순위-그룹-선언-precedence-group-declaration)을 참고바랍니다.

다음 형식은 새로운 접두 연산자를 선언합니다:

```swift
prefix operator <#operator name#>
```

*접두 연산자(prefix operator)*는 표현식 `!a`에서 접두 논리적 NOT 연산자(`!`)와 같은
피연산자 직전에 작성되는 단항 연산자(unary operator)입니다.

접두 연산자 선언은 우선순위를 지정하지 않습니다.
접두 연산자는 비결합적(nonassociative)입니다.

다음 형식은 접미 연산자를 선언합니다:

```swift
postfix operator <#operator name#>
```

*접미 연산자(postfix operator)*는 표현식 `a!`에서 접미 강제 언래핑 연산자(`!`)와 같이
피연산자 바로 뒤에 작성되는 단항 연산자(unary operator)입니다.

접두 연산자와 같이 접미 연산자 선언은 우선순위를 지정하지 않습니다.
접미 연산자는 비결합적(nonassociative)입니다.

새로운 연산자를 선언한 후에
연산자와 같은 이름을 가지는 정적 메서드를 선언하여 구현합니다.
정적 메서드는
연산자가 값을 인자로 가지는 타입 중 하나의 멤버입니다 —--
예를 들어 `Double`에 `Int`를 곱하는 연산자는
`Double`이나 `Int` 구조체에 정적 메서드로 구현됩니다.
접두 연산자나 접미 연산자를 구현하면,
해당 메서드 선언에 `prefix`나 `postfix` 선언 수정자를
표시해야 합니다.
새로운 연산자를 어떻게 생성하고 구현하는지에 대한 예시는
[커스텀 연산자 (Custom Operators)](../language-guide-1/advanced-operators.md#커스텀-연산자-custom-operators)를 참고바랍니다.

> Grammar of an operator declaration:
>
> *operator-declaration* → *prefix-operator-declaration* | *postfix-operator-declaration* | *infix-operator-declaration*
>
> *prefix-operator-declaration* → **`prefix`** **`operator`** *operator* \
> *postfix-operator-declaration* → **`postfix`** **`operator`** *operator* \
> *infix-operator-declaration* → **`infix`** **`operator`** *operator* *infix-operator-group*_?_
>
> *infix-operator-group* → **`:`** *precedence-group-name*

## 우선순위 그룹 선언 (Precedence Group Declaration)

*우선순위 그룹 선언(precedence group declaration)*은
프로그램에서 중위 연산자 우선순위에 대한 새로운 그룹화을 도입합니다.
연산자의 우선순위는 그룹화 괄호가 없을 때
연산자가 피연산자와 어떻게 결합하는지 지정합니다.

우선순위 그룹 선언은 다음의 형식을 가집니다:

```swift
precedencegroup <#precedence group name#> {
    higherThan: <#lower group names#>
    lowerThan: <#higher group names#>
    associativity: <#associativity#>
    assignment: <#assignment#>
}
```

위 형식의 *하위 그룹 이름(lower group names)*과 위 형식의 *상위 그룹 이름(higher group names)* 목록은
기존 우선순위 그룹에 새로운 우선순위 그룹의 관계를 지정합니다.
`lowerThan` 우선순위 그룹 속성은
현재 모듈의 외부에 선언된 우선순위 그룹을 참조하는 데만 사용됩니다.
표현식 `2 + 3 * 5`에서와 같이
두 연산자가 피연산자에 대해 서로 경쟁할 때
상대적으로 우선순위가 높은 연산자가
피연산자와 더 밀접하게 결합합니다.

> Note: 위 형식의 *하위 그룹 이름(lower group names)*과 위 형식의 *상위 그룹 이름(higher group names)*을 사용하는
> 서로 관련된 우선순위 그룹은
> 단일 관계 계층(single relational hierarchy)에 적합해야 하지만
> 선형 계층(linear hierarchy)을 형성할 필요는 없습니다.
> 이것은 상대적 우선순위가 정의되지 않은
> 우선순위 그룹일 수 있다는 의미입니다.
> 이러한 우선순위 연산자는
> 그룹화 괄호없이 나란히 사용할 수 없습니다.

Swift는 표준 라이브러리에서 제공된 연산자와 함께 사용할
수많은 우선순위 그룹을 정의합니다.
예를 들어 덧셈(`+`)과 뺄셈(`-`) 연산자는
`AdditionPrecedence` 그룹에 속하고
곱셈(`*`)과 나눗셈(`/`) 연산자는
`MultiplicationPrecedence` 그룹에 속합니다.
Swift 표준 라이브러리에서 제공하는
우선순위 그룹의 전체 목록은
[연산자 선언 (Operator Declarations)](https://developer.apple.com/documentation/swift/operator_declarations)을 참고바랍니다.

연산자의 *결합방향(associativity)*은 그룹화 괄호가 없을 때 동일한 우선순위 수준을 가진
일련의 연산자가 함께 그룹화되는 방식을 지정합니다.
상황에 맞는 키워드 `left`, `right`, `none` 중 하나를 작성하여
연산자의 결합방향을 지정합니다 —--
결합방향을 생략하면 기본적으로 `none`입니다.
왼쪽 걸합방향(left-associative) 연산자는 왼쪽에서 오른쪽으로 그룹화합니다.
예를 들어
뺄셈 연산자(`-`)는
표현식 `4 - 5 - 6`을 `(4 - 5) - 6`으로 그룹화하고
`-7`로 평가되므로 왼쪽 걸합방향(left-associative)입니다.
오른쪽 걸합방향(right-associative) 연산자는 오른쪽에서 왼쪽으로 그룹화하고,
`none`의 결합방향으로 지정된 연산자는
전혀 결합되지 않습니다.
동일한 우선순위 수준의 비결합성 연산자는
서로 인접하게 표시될 수 없습니다.
예를 들어
`<` 연산자는 `none`의 결합방향을 가집니다.
이것은 `1 < 2 < 3`은 유효하지 않은 표현식이라는 의미입니다.

우선순위 그룹의 *할당(assignment)*은 옵셔널 체이닝을 포함하는 연산에서
연산자의 우선순위를 지정합니다.
`true`로 설정하면, 해당 우선순위 그룹의 연산자는
Swift 표준 라이브러리의 할당 연산자와 동일한 그룹화 규칙을
옵셔널 체이닝 중에 사용합니다.
그렇지 않으면 `false`로 설정하거나 생략된 경우,
우선순위 그룹에서 연산자는 할당을 수행하지 않는 연산자와
동일한 옵셔널 체이닝 규칙을 따릅니다.

> Grammar of a precedence group declaration:
>
> *precedence-group-declaration* → **`precedencegroup`** *precedence-group-name* **`{`** *precedence-group-attributes*_?_ **`}`**
>
> *precedence-group-attributes* → *precedence-group-attribute* *precedence-group-attributes*_?_ \
> *precedence-group-attribute* → *precedence-group-relation* \
> *precedence-group-attribute* → *precedence-group-assignment* \
> *precedence-group-attribute* → *precedence-group-associativity*
>
> *precedence-group-relation* → **`higherThan`** **`:`** *precedence-group-names* \
> *precedence-group-relation* → **`lowerThan`** **`:`** *precedence-group-names*
>
> *precedence-group-assignment* → **`assignment`** **`:`** *boolean-literal*
>
> *precedence-group-associativity* → **`associativity`** **`:`** **`left`** \
> *precedence-group-associativity* → **`associativity`** **`:`** **`right`** \
> *precedence-group-associativity* → **`associativity`** **`:`** **`none`**
>
> *precedence-group-names* → *precedence-group-name* | *precedence-group-name* **`,`** *precedence-group-names* \
> *precedence-group-name* → *identifier*

## 선언 수정자 (Declaration Modifiers)

*선언 수정자(Declaration modifiers)*는 선언의 동작이나 의미를 수정하는
키워드나 문맥에 맞는 키워드입니다.
선언의 속성이 있는 경우 선언의 속성과 선언을 도입하는 키워드 사이에
적절한 키워드나 문맥에 맞는 키워드를 작성하여 선언 수정자를 지정합니다.

- `class`:
  이 수정자를 클래스의 멤버에 적용하여
  해당 멤버가 클래스 인스턴스의 멤버가 아니라,
  클래스 자체의 멤버임을 나타냅니다.
  이 수정자를 가지고
  `final` 수정자를 가지지 않는 상위 클래스의 멤버는
  하위 클래스에 의해 재정의될 수 있습니다.

- `dynamic`:
  Objective-C로 표현될 수 있는 클래스의 모든 멤버에 이 수정자를 적용합니다.
  `dynamic` 수정자로 멤버 선언을 표시하면,
  해당 멤버에 대한 접근이 항상 Objective-C 런타임을 사용하여 동적으로 디스패치됩니다.
  해당 멤버에 대한 접근은 컴파일러에 의해 인라인되거나 가상화되지 않습니다.

  `dynamic` 수정자로 표시된 선언은
  Objective-C 런타임을 사용하여 디스패치되므로
  `objc` 속성으로 표시되어야 합니다.

- `final`:
  이 수정자를 클래스나 클래스의 프로퍼티, 메서드,
  서브스크립트 멤버에 적용합니다. 클래스에 적용되면 해당 클래스를 상속할 수 없음을 나타냅니다.
  클래스의 프로퍼티, 메서드, 서브스크립트에 적용되면
  클래스 멤버가 모든 하위 클래스에 재정의될 수 없음을 나타냅니다.
  `final` 속성을 어떻게 사용하는지에 대한 예시는
  [재정의 방지 (Preventing Overrides)](../language-guide-1/inheritance.md#재정의-방지-preventing-overrides)를 참고바랍니다.

- `lazy`:
  이 수정자는 클래스나 구조체의 변수 저장 프로퍼티에 적용하여
  프로퍼티의 초기값이 프로퍼티에 처음 접근할 때
  한 번만 계산되고 저장됨을 나타냅니다.
  `lazy` 수정자를 어떻게 사용하는지에 대한 예시는
  [지연 저장 프로퍼티 (Lazy Stored Properties)](../language-guide-1/properties.md#지연-저장-프로퍼티-lazy-stored-properties)를 참고바랍니다.

- `optional`:
  프로토콜의 프로퍼티, 메서드, 서브스크립트 멤버에 수정자를 적용하여
  준수하는 타입에
  이 멤버의 구현이 필수가 아님을 나타냅니다.

  `objc` 속성으로 표시된 프로토콜에만 `optional` 수정자를 적용할 수 있습니다.
  결과적으로 클래스 타입만 선택적 멤버 요구사항을 포함하는
  프로토콜을 채택하고 준수할 수 있습니다.
  `optional` 수정자를 어떻게 사용하는지에 대한 자세한 내용과 ---
  예를 들어 준수하는 타입이 이를 구현하는지 확실하지 않을 때와 같이 ---
  선택적 프로토콜 멤버에 접근하는 방법에 대한 지침은
  [옵셔널 프로토콜 요구사항 (Optional Protocol Requirements)](../language-guide-1/protocols.md#옵셔널-프로토콜-요구사항-optional-protocol-requirements)을 참고바랍니다.

<!--
  TODO: Currently, you can't check for an optional initializer,
  so we're leaving those out of the documentation, even though you can mark
  an initializer with the @optional attribute. It's still being decided by the
  compiler team. Update this section if they decide to make everything work
  properly for optional initializer requirements.
-->

- `required`:
  클래스의 지정 이니셜라이저나 편의 이니셜라이저에 이 수정자를 적용하여
  모든 하위 클래스가 해당 이니셜라이저를 구현해야 함을 나타냅니다.
  하위 클래스의 해당 이니셜라이저의 구현도
  `required` 수정자로 표시되어야 합니다.

- `static`:
  이 수정자는 구조체, 클래스, 열거형, 프로토콜의 멤버에 적용하여
  멤버가 해당 타입의 인스턴스 멤버가 아닌
  타입의 멤버 임을 나타냅니다.
  클래스 선언의 범위에서
  멤버 선언에 `static` 수정자를 작성하는 것은
  멤버 선언에
  `class`와 `final` 수정자를 작성한 것과 같은 효과를 가집니다.
  그러나 클래스의 상수 타입 프로퍼티는 예외입니다:
  `static`은 해당 선언에 `class`나 `final`을 작성할 수 없으므로
  비클래스 의미를 가집니다.

- `unowned`:
  이 수정자를 저장 변수, 상수, 저장 프로퍼티에 적용하여
  변수나 프로퍼티가 해당 값으로 저장된 객체에 대해
  미소유 참조가 있다는 것을 나타냅니다.
  객체가 할당 해제된 후에
  변수나 프로퍼티에 접근하려고 하면
  런타임 오류가 발생합니다.
  약한 참조와 마찬가지로
  프로퍼티나 값의 타입은 클래스 타입이어야 합니다;
  약한 참조와 다르게
  타입은 옵셔널이 아닙니다.
  `unowned` 수정자에 대한 예시와 자세한 내용은
  [미소유 참조 (Unowned References)](../language-guide-1/automatic-reference-counting.md#미소유-참조-unowned-references)를 참고바랍니다.

- `unowned(safe)`:
  `unowned`의 명시적 표기입니다.

- `unowned(unsafe)`:
  이 수정자를 저장 변수, 상수, 저장 프로퍼티에 적용하여
  변수나 프로퍼티에 해당 값으로 저장된 객체에 대해
  미소유 참조를 가집니다.
  객체가 할당 해제된 후에
  변수나 프로퍼티에 접근하려고 하면
  객체가 사용한 메모리 위치에 접근하는데
  이것은 안전하지 않은 메모리(memory-unsafe)동작입니다.
  약한 참조와 마찬가지로
  프로퍼티나 값의 타입은 클래스 타입이어야 합니다;
  약한 참조와 다르게
  타입은 옵셔널이 아닙니다.
  `unowned` 수정자에 대한 예시와 자세한 내용은
  [미소유 참조 (Unowned References)](../language-guide-1/automatic-reference-counting.md#미소유-참조-unowned-references)를 참고바랍니다.

- `weak`:
  이 수정자를 저장 변수나 변수 저장 프로퍼티에 적용하면
  변수나 프로퍼티에 해당 값으로 저장된 객체에 대해
  약한 참조를 가집니다.
  변수나 프로퍼티의 타입은 옵셔널 클래스 타입이어야 합니다.
  객체가 할당 해제된 후에
  변수나 프로퍼티에 접근하려고 하면
  해당 값은 `nil`입니다.
  `weak` 수정자에 대한 예시와 자세한 내용은
  [약한 참조 (Weak References)](../language-guide-1/automatic-reference-counting.md#약한-참조-weak-references)를 참고바랍니다.

### 접근 제어 수준 (Access Control Levels)

Swift는 다섯 가지 접근 제어 수준을 제공합니다: open, public, internal, file private, private입니다.
선언에 접근 수준을 지정하기 위해
접근-수준 수정자 중 하나로 선언을 표시할 수 있습니다.
접근 제어는 [접근 제어 (Access Control)](../language-guide-1/access-control.md)에 자세히 설명되어 있습니다.

- `open`:
  이 수정자를 선언에 적용하여 선언과 동일한 모듈의 코드에서
  선언에 접근하고 하위 클래스화될 수 있음을 나타냅니다.
  `open` 접근-수준 수정자로 표시된 선언은 해당 선언을 포함하는
  모듈을 임포트하는 모듈의 코드에서 접근되고 하위 클래스화될 수 있습니다.

- `public`:
  이 수정자를 선언에 적용하여 선언과 동일한 모듈의 코드에서
  선언에 접근하고 하위 클래스화될 수 있음을 나타냅니다.
  `public` 접근-수준 수정자로 표시된 선언은 해당 선언을 포함하는
  모듈을 임포트하는 모듈의 코드에서 접근되지만 하위 클래스화될 수 없습니다.

- `package`:
  이 수정자를 선언에 적용하여
  선언과 동일한 패키지의 코드에 의해서만
  접근될 수 있음을 나타냅니다.
  패키지는 사용 중인 빌드 시스템에서 정의하는
  코드 배포 단위입니다.
  빌드 시스템이 코드를 컴파일할 때
  Swift 컴파일러에 `-package-name` 플래그를 전달하여
  패키지 이름을 지정합니다.
  두 모듈이 빌드 시스템이 동일한 패키지 이름을 지정하여 빌드하면
  동일한 패키지의 일부입니다.

- `internal`:
  이 수정자를 선언에 적용하여
  선언과 동일한 모듈의 코드에 의해서만 접근될 수 있음을 나타냅니다.
  기본적으로
  대부분 선언은 암시적으로 `internal` 접근-수준 수정자로 표시됩니다.

- `fileprivate`:
  이 수정자를 선언에 적용하여
  선언과 동일한 소스 파일의 코드에서만 선언에 접근될 수 있음을 나타냅니다.

- `private`:
  이 수정자를 선언에 적용하여
  선언을 직접 둘러싸는 범위 내의 코드에서만 선언에 접근될 수 있음을 나타냅니다.

접근 제어의 목적을 위해
확장은 다음과 같이 동작합니다:

- 동일한 파일에 여러 확장이 있고,
  이 확장이 동일한 타입을 확장한다면,
  이 모든 확장은 동일한 접근 제어를 가집니다.
  확장과 확장하는 타입은 다른 파일에 있을 수 있습니다.

- 확장이 확장하는 타입과 동일한 파일에 있으면,
  확장은 확장하는 타입과 동일한 접근 제어를 가집니다.

- 타입의 선언에서 선언된 private 멤버는
  해당 타입의 확장에서 접근할 수 있습니다.
  한 확장에서 선언된 private 멤버는
  다른 확장과 확장된 타입의 선언에서
  접근할 수 있습니다.

위의 각 접근-수준 수정자는 선택적으로 괄호로 둘러싸인 `set` 키워드로 구성된
단일 인자를 허용합니다 ---
예를 들어 `private(set)`입니다.
[Getter와 Setter (Getters and Setters)](../language-guide-1/access-control.md#getter와-setter-getters-and-setters)에서 설명한대로
변수나 서브스크립트에 대한 접근 수준보다
작거나 같은 변수나 서브스크립트의 setter에 대한 접근 수준을 지정하려면
이 형식의 접근-수준 수정자를 사용합니다.

> Grammar of a declaration modifier:
>
> *declaration-modifier* → **`class`** | **`convenience`** | **`dynamic`** | **`final`** | **`infix`** | **`lazy`** | **`optional`** | **`override`** | **`postfix`** | **`prefix`** | **`required`** | **`static`** | **`unowned`** | **`unowned`** **`(`** **`safe`** **`)`** | **`unowned`** **`(`** **`unsafe`** **`)`** | **`weak`** \
> *declaration-modifier* → *access-level-modifier* \
> *declaration-modifier* → *mutation-modifier* \
> *declaration-modifier* → *actor-isolation-modifier* \
> *declaration-modifiers* → *declaration-modifier* *declaration-modifiers*_?_
>
> *access-level-modifier* → **`private`** | **`private`** **`(`** **`set`** **`)`** \
> *access-level-modifier* → **`fileprivate`** | **`fileprivate`** **`(`** **`set`** **`)`** \
> *access-level-modifier* → **`internal`** | **`internal`** **`(`** **`set`** **`)`** \
> *access-level-modifier* → **`package`** | **`package`** **`(`** **`set`** **`)`** \
> *access-level-modifier* → **`public`** | **`public`** **`(`** **`set`** **`)`** \
> *access-level-modifier* → **`open`** | **`open`** **`(`** **`set`** **`)`**
>
> *mutation-modifier* → **`mutating`** | **`nonmutating`**
>
> *actor-isolation-modifier* → **`nonisolated`**

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