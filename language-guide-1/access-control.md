# 접근 제어 \(Access Control\)

_접근 제어 \(Access control\)_ 은 다른 소스 파일과 모듈에서 코드의 부분에 접근에 대해 제한합니다. 이 기능은 코드의 구현 세부를 숨기고 해당 코드에 접근하고 사용될 수 있는 기본 인터페이스를 지정할 수 있습니다.

특정 접근 수준을 개별 타입 \(클래스, 구조체, 그리고 열거형\) 뿐만 아니라 해당 타입에 속하는 프로퍼티, 메서드, 초기화 구문과 서브 스크립트에 할당할 수 있습니다. 프로토콜은 전역 상수, 변수, 그리고 함수와 마찬가지로 특정 컨텍스트로 제한될 수 있습니다.

다양한 수준의 접근 제어를 제공하는 것 외에도 Swift는 일반적인 시나리오에 대한 기본 접근 수준을 제공하여 명시적으로 접근 제어 수준을 지정할 필요성을 줄여줍니다. 실제로 단일 앱을 작성한다면 접근 제어 수준을 전혀 지정할 필요가 없습니다.

> NOTE   
> 프로퍼티, 타입, 함수 등에 접근 제어를 적용할 수 있는 코드의 다양한 측면은 간결성을 위해 아래 섹션에서 "엔티티" 라고 합니다.

## 모듈과 소스 파일 \(Modules and Source Files\)

Swift의 접근 제어 모델은 모듈과 소스 파일의 개념을 기초로 합니다.

_모듈 \(module\)_ 은 단일 단위로 빌드되고 제공되는 프레임워크 또는 애플리케이션과 같은 코드 배포의 단일 단위이고 Swift의 `import` 키워드를 사용하여 다른 모듈에서 가져올 수 있습니다.

앱 번들 또는 프레임워크와 같이 Xcode에서 각 빌드 타겟은 Swift에서 별도의 모듈로 처리됩니다. 여러 애플리케이션에서 해당 코드를 캡슐화하고 재사용하기 위해 앱의 코드의 여러 측면을 독립형 프레임워크로 그룹화하면 해당 프레임워크 내에서 정의한 모든 것은 앱 내에서 가져와서 사용되거나 다른 프레임워크 내에서 사용될 때 별도의 모듈에 속하게 됩니다.

_소스 파일 \(source file\)_ 은 모듈 내에서 단일 Swift 소스 코드 파일입니다 \(실제로 앱 또는 프레임워크 내의 단일 파일\). 개별 타입을 별도의 소스 파일에 정의하는 것이 일반적이지만 단일 소스 파일에 여러 타입, 함수 등에 대한 정의가 포함될 수 있습니다.

## 접근 수준 \(Access Levels\)

Swift는 코드 내에서 엔티티에 대해 5개의 다른 _접근 수준 \(access levels\)_ 을 제공합니다. 이 접근 수준은 엔티티가 정의된 소스파일과 관련되며 소스 파일이 속한 모듈과 관련됩니다.

* _Open 접근_ 과 _public 접근_ 은 정의한 모듈의 모든 소스 파일과 정의한 모듈을 가져오는 \(import\) 다른 모듈의 소스 파일에서 엔티티를 사용할 수 있습니다. 일반적으로 프레임워크를 공개 인터페이스로 지정할 때 open 또는 public 접근을 사용합니다. open과 public 접근 간의 차이점은 아래에 설명되어 있습니다.
* _Internal 접근_ 은 정의한 모듈의 모든 소스 파일 내에서 사용할 수 있지만 해당 모듈 외부의 소스 파일에서는 사용할 수 없습니다. 일반적으로 앱 또는 프레임워크의 내부 구조체를 정의할 때 internal 접근을 사용합니다.
* _File-private 접근_ 은 자체 정의한 소스 파일로 엔티티의 사용을 제한합니다. 세부 내용은 파일 전체에서 사용되고 기능의 특정 부분의 구현 세부정보를 가리기 위해 file-private 접근을 사용합니다.
* _Private 접근_ 은 둘러싸인 선언과 같은 파일에 있는 해당 선언의 확장으로 엔티티의 사용을 제한합니다. 세부 내용은 단일 선언 내에서만 사용되고 기능의 특정 부분의 구현 세부정보를 가리기 위해 private 접근을 사용합니다. 

Open 접근은 가장 높은 \(가장 적은 제약\) 접근 수준이며 private 접근은 가장 적은 \(가장 높은 제약\) 접근 수준입니다.

Open 접근은 클래스와 클래스 멤버에만 적용되고 아래의 [서브 클래싱 \(Subclassing\)](access-control.md#subclassing) 에서 설명 하듯이 모듈 외부의 코드를 하위 클래스와 재정의할 수 있는다는 점에서 public 접근과 다릅니다. 명시적으로 클래스를 개방형으로 표기하면 해당 클래스를 상위 클래스로 사용하는 다른 모듈의 코드가 미치는 영향을 고려하고 이에 따라 클래스 코드를 설계했음을 나타냅니다.

### 접근 수준의 기본 원칙 \(Guiding Principle of Access Levels\)

Swift에서 접근 수준은 전반적인 기본 원칙을 따릅니다: _엔티티는 더 낮은 \(더 제한적\) 접근 수준을 가진 다른 엔티티로 정의할 수 없습니다._

예를 들어:

* public 변수는 internal, file-private, 또는 private 타입으로 정의될 수 없습니다. public 변수는 어디서나 사용될 수 있지만 이 타입은 그렇지 못하기 때문입니다.
* 함수는 주변 코드에서 구성 타입을 사용할 수 없는 상황에서 함수를 사용할 수 있기 때문에 파라미터 타입과 반환 타입보다 더 높은 접근 수준을 가질 수 없습니다.

언어의 여러 측면에 대한 이 기본 원칙의 구체적인 의미는 아래에 자세히 설명되어 있습니다.

### 기본 접근 수준 \(Default Access Levels\)

이 챕터의 뒷부분에서 설명하는 몇가지 특정 예외를 포함하여 코드의 모든 엔티티는 명시적으로 접근 수준을 지정하지 않으면 internal의 기본 접근 수준을 가집니다. 따라서 많은 경우에 코드에서 명시적으로 접근 수준을 지정할 필요가 없습니다.

### 단일 타겟 앱에 대한 접근 수준 \(Access Levels for Single-Target Apps\)

간단한 단일 타겟 앱을 작성할 때 앱의 코드는 일반적으로 앱 내에 자체적으로 포함되며 앱의 모듈 외부에서 사용할 수 있도록 만들 필요가 없습니다. internal의 기본 접근 수준은 이 요구사항과 일치합니다. 따라서 사용자 정의 접근 수준을 지정할 필요가 없습니다. 그러나 앱의 모듈 내에서 다른 코드에서 구현 세부정보를 가리기 위해 file private 또는 private로 코드의 어떤 부분을 표기하고 싶을 수 있습니다.

### 프레임워크에 대한 접근 수준 \(Access Levels for Frameworks\)

프레임워크를 개발할 때 프레임워크를 가져오는 앱과 같은 다른 모듈에서 보고 접근할 수 있게 하기 위해 해당 프레임워크에 대한 공용 인터페이스를 open 또는 public으로 표시합니다. 이 공용 인터페이스는 프레임워크 용 애플리케이션 프로그래밍 인터페이스 또는 API 입니다.

> NOTE   
> 프레임워크의 내부 구현 세부정보는 여전히 internal의 기본 접근 수준을 사용할 수 있거나 프레임워크의 내부 코드의 다른 부분에서 가리기 위해 private 또는 file private로 표기될 수 있습니다. 프레임워크 API의 부분이 되길 원한다면 엔티티를 open 또는 public으로 표시해야 합니다.

### 유닛 테스트 타겟에 대한 접근 수준 \(Access Levels for Unit Test Targets\)

유닛 테스트 타겟과 함께 앱을 작성할 때 앱의 코드는 테스트를 위해 해당 모듈에서 사용할 수 있어야 합니다. 기본적으로 open 또는 public으로 표기된 엔티티만 다른 모듈에서 접근할 수 있습니다. 그러나 모듈에 대한 가져오기 선언을 `@testable` 속성으로 표시하고 테스트를 활성화하여 해당 모듈을 컴파일 하면 유닛 테스트 타겟은 모든 내부 엔티티에 접근할 수 있습니다.

## 접근 제어 구문 \(Access Control Syntax\)

엔티티의 선언의 시작에 `open`, `public`, `internal`, `fileprivate`, 또는 `private` 수식어 중 하나를 위치시켜 엔티티에 대한 접근 수준을 정의합니다.

```swift
public class SomePublicClass {}
internal class SomeInternalClass {}
fileprivate class SomeFilePrivateClass {}
private class SomePrivateClass {}

public var somePublicVariable = 0
internal let someInternalConstant = 0
fileprivate func someFilePrivateFunction() {}
private func somePrivateFunction() {}
```

달리 지정하지 않는 한 [기본 접근 수준 \(Default Access Levels\)](access-control.md#default-access-levels) 은 internal 입니다. `SomeInternalClass` 와 `someInternalConstant` 는 명시적으로 접근 수준 수식어 없이 작성될 수 있으며 여전히 internal의 접근 수준을 가진다는 의미입니다:

```swift
class SomeInternalClass {}              // implicitly internal
let someInternalConstant = 0            // implicitly internal
```

## 사용자 정의 타입 \(Custom Types\)

사용자 정의 타입에 대해 명시적으로 접근 수준을 지정하고 싶다면 타입을 정의할 때 지정합니다. 그러면 새로운 타입은 접근 수준이 허용하는 범위내에서 사용될 수 있습니다. 예를 들어 file-private 클래스를 정의하면 해당 클래스는 file-private 클래스가 정의된 소스 파일에 프로퍼티의 타입 또는 함수의 파라미터 또는 반환 타입으로만 사용될 수 있습니다.

타입의 접근 제어 수준은 해당 타입의 _멤버_ \(프로퍼티, 메서드, 초기화 구문, 그리고 서브 스크립트\)의 기본 접근 수준에도 영향을 줍니다. 타입의 접근 수준을 private 또는 file private로 정의한다면 멤버의 기본 접근 수준 또한 private 또는 file private가 됩니다. 타입의 접근 수준을 internal 또는 public 또는 명시적으로 접근 수준을 지정하지 않아 internal의 기본 접근 수준으로 정의하면 타입의 멤버의 기본 접근 수준은 internal이 됩니다.

> IMPORTANT   
> Public 타입은 기본적으로 public 멤버가 아닌 internal 멤버를 가집니다. 타입 멤버를 public으로 하려면 명시적으로 표시해야 합니다. 이 요구사항은 타입에 대한 공용 API는 공개되도록 하고 실수로 타입의 내부 작업을 공개 API로 표시되지 않도록 합니다.

```swift
public class SomePublicClass {                  // explicitly public class
    public var somePublicProperty = 0            // explicitly public class member
    var someInternalProperty = 0                 // implicitly internal class member
    fileprivate func someFilePrivateMethod() {}  // explicitly file-private class member
    private func somePrivateMethod() {}          // explicitly private class member
}

class SomeInternalClass {                       // implicitly internal class
    var someInternalProperty = 0                 // implicitly internal class member
    fileprivate func someFilePrivateMethod() {}  // explicitly file-private class member
    private func somePrivateMethod() {}          // explicitly private class member
}

fileprivate class SomeFilePrivateClass {        // explicitly file-private class
    func someFilePrivateMethod() {}              // implicitly file-private class member
    private func somePrivateMethod() {}          // explicitly private class member
}

private class SomePrivateClass {                // explicitly private class
    func somePrivateMethod() {}                  // implicitly private class member
}
```

### 튜플 타입 \(Tuple Types\)

튜플 타입에 대한 접근 수준은 해당 튜플에서 사용되는 모든 타입에 가장 제한적인 접근 수준입니다. 예를 들어 하나는 internal 접근이고 하나는 private 접근인 2개의 다른 타입의 튜플을 구성하는 경우 해당 복합 튜플 타입에 대한 접근 수준은 private 입니다.

> NOTE   
> 튜플 타입은 클래스, 구조체, 열거형, 그리고 함수와 같이 독립형 정의가 없습니다. 튜플 타입의 접근 수준은 튜플 타입을 구성하는 타입에서 자동으로 결정되고 명시적으로 지정할 수 없습니다.

### 함수 타입 \(Function Types\)

함수 타입에 대한 접근 수준은 함수의 파라미터 타입과 반환 타입 중 가장 제한적인 접근 수준으로 계산됩니다. 함수의 계산된 접근 수준이 컨텍스트 기본값과 일치하지 않는다면 함수의 정의의 부분으로 접근 수준을 명시적으로 지정해야 합니다.

아래의 예제는 함수 자체에 대해 지정된 접근 수준 수식어가 없는 `someFunction()` 이라는 전역 함수를 정의합니다. 이 함수는 "internal"의 기본 접근 수준을 가지리라 생각되지만 여기서는 그렇지 않습니다. 실제로 `someFunction()` 은 아래와 같이 작성되면 컴파일되지 않습니다:

```swift
func someFunction() -> (SomeInternalClass, SomePrivateClass) {
    // function implementation goes here
}
```

함수의 반환 타입은 위에 [사용자 정의 타입 \(Custom Types\)](access-control.md#custom-types) 에서 정의된 2개의 사용자 정의 클래스로 구성된 튜플 타입입니다. 이 클래스 중 하나는 internal로 다른 하나는 private로 정의됩니다. 따라서 복합 튜플 타입의 전체 접근 수준은 private 입니다 \(튜플의 구성 타입의 최소 접근 수준\).

함수의 반환 타입은 private 이므로 함수 선언이 유효하려면 `private` 수식어와 함께 함수의 전체 접근 수준을 표시해야 합니다:

```swift
private func someFunction() -> (SomeInternalClass, SomePrivateClass) {
    // function implementation goes here
}
```

함수의 public 또는 internal 인 사용자가 함수의 반환 타입에 사용된 private 클래스에 적절한 접근을 할 수 없으므로 `someFunction()` 의 정의를 `public` 또는 `internal` 수식어나 internal의 기본 설정을 사용하여 표시하면 유효하지 않습니다.

### 열거형 타입 \(Enumeration Types\)

열거형의 개별 케이스는 열거형과 같은 접근 수준을 자동으로 받습니다. 개별 열거형 케이스에 대해 다른 접근 수준을 지정할 수 없습니다.

아래의 예제에서 `CompassPoint` 열거형은 public의 명시적 접근 수준을 갖습니다. 따라서 `north`, `south`, `east`, 그리고 `west` 열거형 케이스 또한 public의 접근 수준을 가집니다:

```swift
public enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

#### 원시값과 연관된 값 \(Raw Values and Associated Values\)

열거형 정의의 모든 원시값 또는 연관된 값에 사용되는 타입은 열거형의 접근 수준보다 높은 접근 수준을 가져야 합니다. 예를 들어 internal 접근 수준의 열거형의 원시값 타입으로 private 타입을 사용할 수 없습니다.

### 중첩된 타입 \(Nested Types\)

중첩된 타입의 접근 수준은 포함한 타입이 public이 아닌 경우 포함한 타입과 동일합니다. public 타입 내에 정의된 중첩된 타입은 자동으로 internal의 접근 수준을 가집니다. Public 타입 내에 중첩된 타입이 공개되려면 명시적으로 public으로 중첩된 타입을 선언해야 합니다.

## 서브 클래싱 \(Subclassing\)

현재 접근 컨텍스트에서 접근할 수 있고 하위 클래스와 동일한 모듈에 정의된 모든 클래스를 하위 클래스로 지정할 수 있습니다. 다른 모듈에 정의된 모든 open 클래스도 하위 클래스로 지정할 수 있습니다. 하위 클래스는 상위 클래스의 접근 수준보다 높은 접근 수준을 가질 수 없습니다. 예를 들어 internal 상위 클래스에 public 하위 클래스를 작성할 수 없습니다.

또한 같은 모듈에 정의된 클래스의 경우 특정 접근 컨텍스트에 표시되는 모든 클래스 멤버 \(메서드, 프로퍼티, 초기화 구문 또는 서브 스크립트\)를 재정의 할 수 있습니다. 다른 모듈에 정의된 클래스의 경우 모든 open 클래스 멤버를 재정의 할 수 있습니다.

재정의는 상속된 클래스 멤버를 상위 클래스 버전보다 더 쉽게 접근할 수 있도록 만들 수 있습니다. 아래의 예제에서 클래스 `A` 는 `someMethod()` 라는 file-private 메서드를 가진 public 클래스 입니다. 클래스 `B` 는 "internal"로 줄어든 접근 수준을 가진 `A` 의 하위 클래스입니다. 그럼에도 불구하고 클래스 `B` 는 `someMethod()` 의 기존 구현보다 _높은_ "internal" 의 접근 수준을 가진 `someMethod()` 의 재정의를 제공합니다:

```swift
public class A {
    fileprivate func someMethod() {}
}

internal class B: A {
    override internal func someMethod() {}
}
```

상위 클래스의 멤버에 대한 호출이 허용된 접근 수준 컨텍스트 내에서 발생하는 한 하위 클래스 멤버보다 더 낮은 접근 권한을 가진 상위 클래스 멤버 호출하는 것도 유효합니다 \(상위 클래스의 file-private 멤버 호출에 대한 같은 소스 파일 내나 상위 클래스의 internal 멤버 호출에 대한 같은 모듈 내\):

```swift
public class A {
    fileprivate func someMethod() {}
}

internal class B: A {
    override internal func someMethod() {
        super.someMethod()
    }
}
```

상위 클래스 `A` 와 하위 클래스 `B` 는 같은 소스 파일에 정의되었으므로 `B` 에 대해 `super.someMethod()` 호출하기 위해 `someMethod()` 의 구현은 유효합니다.

## 상수, 변수, 프로퍼티, 그리고 서브 스크립트 \(Constants, Variables, Properties, and Subscripts\)

상수, 변수, 또는 프로퍼티는 타입보다 더 공개할 수 없습니다. 예를 들어 private 타입으로 public 프로퍼티를 작성하는 것은 유효하지 않습니다. 유사하게 서브 스크립트는 인덱스 타입 또는 반환 타입 보다 더 공개될 수 없습니다.

상수, 변수, 프로퍼티, 또는 서브 스크립트가 private 타입을 사용하는 경우 상수, 변수, 프로퍼티, 또는 서브 스크립트 또한 `private` 로 표시되어야 합니다:

```swift
private var privateInstance = SomePrivateClass()
```

### Getter와 Setter \(Getters and Setters\)

상수, 변수, 프로퍼티, 그리고 서브 스크립트에 대한 getter와 setter는 자동으로 속해있는 상수, 변수, 프로퍼티, 또는 서브 스크립트와 같은 접근 수준을 받습니다.

해당 변수, 프로퍼티, 또는 서브 스크립트에 읽기-쓰기 범위를 제한하기 위해 getter 보다 _더 낮은_ 접근 수준으로 setter를 제공할 수 있습니다. `var` 또는 `subscript` 전에 `fileprivate(set)`, `private(set)`, 또는 `internal(set)` 을 작성하여 더 낮은 접근 수준을 할당합니다.

> NOTE   
> 이 규칙은 저장된 프로퍼티 뿐만 아니라 계산된 프로퍼티에도 적용됩니다. 저장된 프로퍼티에 대해 명시적으로 getter와 setter를 작성하지 않아도 Swift는 저장된 프로퍼티의 저장소에 대한 접근을 제공하기 위해 여전히 암시적으로 getter와 setter를 합성합니다. 계산된 프로퍼티의 명시적 setter와 같은 방식으로 합성된 setter의 접근 수준을 변경하기 위해 `fileprivate(set)`, `private(set)`, 그리고 `internal(set)` 을 사용합니다.

아래 예제는 문자열 프로퍼티가 몇번 수정되는지 추적하는 `TrackedString` 이라는 구조체를 정의합니다:

```swift
struct TrackedString {
    private(set) var numberOfEdits = 0
    var value: String = "" {
        didSet {
            numberOfEdits += 1
        }
    }
}
```

`TrackedString` 구조체는 빈 문자열 인 `""` 의 초기값을 가진 `value` 라는 저장된 문자열 프로퍼티를 정의합니다. 이 구조체는 또한 `value` 가 몇번이나 수정되는지 추적하는데 사용되는 `numberOfEdits` 라는 저장된 정수 프로퍼티도 정의합니다. 이 수정 추적은 `value` 프로퍼티에 새로운 값이 설정될 때마다 `numberOfEdits` 를 증가시키는 `value` 프로퍼티에 `didSet` 프로퍼티 관찰자로 구현됩니다.

`TrackedString` 구조체와 `value` 프로퍼티는 명시적으로 접근 수준 수식어를 제공하지 않으므로 internal의 기본 접근 수준을 받습니다. 그러나 `numberOfEdits` 에 대한 접근 수준은 프로퍼티의 getter는 여전히 internal의 기본 접근 수준을 가지지만 프로퍼티는 `TrackedString` 구조체의 부분의 코드 내에서만 설정 가능함을 나타내기 위해 `private(set)` 수식어로 표기됩니다. 이렇게 하면 `TrackedString` 이 내부적으로 `numberOfEdits` 프로퍼티를 수정할 수 있지만 구조체의 정의 외부에서 사용될 때는 읽기전용 프로퍼티로 표시할 수 있습니다.

`TrackedString` 인스턴스를 생성하고 문자열 값을 몇 번 수정하면 `numberOfEdits` 프로퍼티 값이 수정된 수와 일치하게 업데이트 됨을 확인할 수 있습니다:

```swift
var stringToEdit = TrackedString()
stringToEdit.value = "This string will be tracked."
stringToEdit.value += " This edit will increment numberOfEdits."
stringToEdit.value += " So will this one."
print("The number of edits is \(stringToEdit.numberOfEdits)")
// Prints "The number of edits is 3"
```

다른 소스 파일에서 `numberOfEdits` 프로퍼티의 현재값을 조회할 수 있지만 다른 소스 파일에서 프로퍼티를 _수정_ 할 수 없습니다. 이 제한은 `TrackedString` 편집 추적 기능의 구현 세부사항을 보호하는 동시에 해당 기능의 측면에 대한 편리한 접근을 제공합니다.

필요한 경우 getter와 setter 모두에 명시적으로 접근 수준을 할당할 수 있습니다. 아래 예제는 public의 명시적 접근 수준으로 정의된 `TrackedString` 구조체 버전을 보여줍니다. 따라서 구조체의 멤버 \(`numberOfEdits` 프로퍼티 포함\)는 기본적으로 internal 접근 수준을 가집니다. `public` 과 `private(set)` 접근 수준 수식어를 결합하여 구조체의 `numberOfEdits` 프로퍼티 getter를 public으로 setter를 private로 만들 수 있습니다:

```swift
public struct TrackedString {
    public private(set) var numberOfEdits = 0
    public var value: String = "" {
        didSet {
            numberOfEdits += 1
        }
    }
    public init() {}
}
```

## 초기화 구문 \(Initializers\)

사용자 정의 초기화 구문은 초기화 하는 타입보다 더 낮거나 같은 접근 수준으로 할당할 수 있습니다. 유일한 예외는 [필수 초기화 구문 \(Required Initializers\)](initialization.md#required-initializers) 입니다. 필수 초기화 구문은 자신이 속한 클래스와 동일한 접근 수준을 가져야 합니다.

함수와 메서드 파라미터와 마찬가지로 초기화 구문의 파라미터의 타입은 초기화 구문의 자체 접근 수준보다 더 비공개 일 수 없습니다.

### 기본 초기화 구문 \(Default Initializers\)

[기본 초기화 구문 \(Default Initializers\)](initialization.md#default-initializers) 에서 설명 했듯이 Swift는 모든 프로퍼티에 기본값을 제공하고 하나 이상의 초기화 구문 자체를 제공하지 않는 구조체 또는 기본 클래스에 대해 인자없는 _기본 초기화 구문 \(default initializer\)_ 을 자동으로 제공합니다.

기본 초기화 구문은 타입이 `public` 으로 정의되지 않는한 초기화하는 타입과 같은 접근 수준을 가집니다. `public` 으로 정의된 타입의 경우 기본 초기화 구문은 internal로 간주합니다. 다른 모듈에서 사용할 때 인자가 없는 초기화 구문을 public 타입으로 초기화 하려면 명시적으로 타입의 정의의 부분으로 public 인자가 없는 초기화 구문을 제공해야 합니다.

### 구조체 타입에 대한 기본 멤버별 초기화 구문 \(Default Memberwise Initializers for Structure Types\)

구조체의 모든 저장된 프로퍼티가 private라면 구조체 타입의 기본 멤버별 초기화 구문은 private라고 간주합니다. 마찬가지로 구조체의 모든 저장된 프로퍼티가 file private라면 초기화 구문은 file private 입니다. 그렇지 않으면 초기화 구문은 internal의 접근 수준을 가집니다.

위의 기본 초기화 구문과 마찬가지로 다른 모듈에서 사용될 때 멤버별 초기화 구문으로 초기화 하는 public 구조체 타입은 타입의 정의의 부분으로 public 멤버별 초기화 구문을 자체적으로 제공해야 합니다.

## 프로토콜 \(Protocols\)

프로토콜 타입에 명시적으로 접근 수준을 할당하려면 프로토콜을 정의하는 지점에서 지정하면 됩니다. 이를 통해 특정 접근 컨텍스트 내에서만 채택될 수 있는 프로토콜을 생성할 수 있습니다.

프로토콜 정의 내의 각 요구사항의 접근 수준은 자동으로 프로토콜과 같은 접근 수준으로 설정됩니다. 프로토콜 요구사항을 지원하는 프로토콜과 다른 접근 수준으로 설정할 수 없습니다. 이렇게 하면 모든 프로토콜의 요구사항은 프로토콜을 채택하는 모든 타입에서 볼 수 있습니다.

> NOTE   
> public 프로토콜을 정의한다면 프로토콜의 요구사항은 구현될 때 public 접근 수준을 요구합니다. 이 동작은 public 타입 정의가 타입의 멤버에 대한 internal의 접근 수준을 의미하는 다른 타입과 다릅니다.

### 프로토콜 상속 \(Protocol Inheritance\)

기존 프로토콜에서 상속하는 새로운 프로토콜을 정의한다면 새로운 프로토콜은 상속된 프로토콜과 최대 동일한 접근 수준을 가질 수 있습니다. 예를 들어 internal 프로토콜에서 상속되는 public 프로토콜을 작성할 수 없습니다.

### 프로토콜 준수 \(Protocol Conformance\)

타입은 타입 자체보다 더 낮은 접근 수준으로 프로토콜을 준수할 수 있습니다. 예를 들어 다른 모듈에서 사용될 수 있지만 internal 프로토콜에 대한 준수성은 internal 프로토콜의 정의 모듈 내에서만 사용할 수 있는 public 타입을 정의할 수 있습니다.

타입이 특정 프로토콜을 준수하는 컨텍스트는 타입의 접근 수준과 프로토콜의 접근 수준의 최소입니다. 예를 들어 타입이 public 이지만 준수하는 프로토콜이 internal 인 경우 프로토콜에 대한 타입의 준수성도 internal 입니다.

프로토콜을 준수하기 위해 타입을 작성하거나 확장할 때 각 프로토콜 요구사항의 타입의 구현이 해당 프로토콜에 대한 타입의 준수성과 최소한 같은 접근 수준을 가지고 있는지 확인해야 합니다. 예를 들어 public 타입이 internal 프로토콜을 준수하는 경우 각 프로토콜 요구사항의 타입의 구현은 적어도 internal 이어야 합니다.

> NOTE   
> Objective-C처럼 Swift에서 프로토콜 준수성은 전역입니다—같은 프로그램 내에서 타입이 두가지 다른 방식으로 프로토콜을 준수하는 것은 불가능 합니다.

## 확장 \(Extensions\)

클래스, 구조체, 또는 열거형을 사용할 수 있는 모든 접근 컨텍스트에서 클래스, 구조체, 또는 열거형을 확장할 수 있습니다. 확장에 추가된 모든 타입 멤버는 확장된 기존 타입에 선언된 타입 멤버와 같은 기본 접근 수준을 가집니다. public 또는 internal 타입으로 확장하면 추가한 모든 새로운 타입 멤버는 internal의 기본 접근 수준을 가집니다. file-private로 확장하면 추가한 모든 새로운 타입 멤버는 file private의 기본 접근 수준을 가집니다. private로 확장하면 추가한 모든 새로운 타입 멤버는 private의 기본 접근 수준을 가집니다.

또는 확장 내에서 정의된 모든 멤버에 대해 새로운 기본 접근 수준을 설정하기 위해 예를 들어 `private` 와 같이 명시적으로 접근 수준 수식어를 확장에 표기할 수 있습니다. 이 새로운 기본 접근 수준은 확장 내에서 개별 타입 멤버를 재정의 할 수 있습니다.

확장을 사용하여 프로토콜 준수성을 추가하는 경우 확장에 대한 명시적 접근 수준 수식어를 제공할 수 없습니다. 대신에 프로토콜의 자체 접근 수준은 확장 내에서 각 프로토콜 요구사항 구현에 대해 기본 접근 수준을 제공하기 위해 사용됩니다.

### 확장에서 Private 멤버 \(Private Members in Extensions\)

확장하는 클래스, 구조체, 또는 열거형과 같은 파일에 있는 확장은 확장 안에 코드가 기존 타입의 선언의 부분으로 작성된 것처럼 동작합니다. 결과적으로 다음을 수행할 수 있습니다:

* 기존 선언에서 private 멤버를 선언하고 같은 파일의 확장에서 해당 멤버를 접근합니다.
* 한 확장에서 private 멤버를 선언하고 같은 파일의 다른 확장에서 해당 멤버를 접근합니다.
* 확장에서 private 멤버를 선언하고 같은 파일의 기존 선언에서 해당 멤버를 접근합니다.

이 동작은 타입이 private 엔티티가 있는지 여부에 관계없이 같은 방법으로 확장을 사용하여 코드를 구성할 수 있다는 의미입니다. 예를 들어 다음과 같은 간단한 프로토콜이 있습니다:

```swift
protocol SomeProtocol {
    func doSomething()
}
```

아래와 같이 프로토콜 준수성을 추가하기 위해 확장을 사용할 수 있습니다:

```swift
struct SomeStruct {
    private var privateVariable = 12
}

extension SomeStruct: SomeProtocol {
    func doSomething() {
        print(privateVariable)
    }
}
```

## 제너릭 \(Generics\)

제너릭 타입 또는 제너릭 함수에 대한 접근 수준은 제너릭 타입 또는 함수 자체의 접근 수준과 해당 타입 파라미터에 대한 모든 타입 제약조건의 접근 수준의 최소입니다.

## 타입 별칭 \(Type Aliases\)

정의한 모든 타입 별칭은 접근 제어의 목적을 위해 고유한 타입으로 처리됩니다. 타입 별칭은 별칭이 지정된 타입의 접근 수준보다 작거나 같은 접근 수준을 가질 수 있습니다. 예를 들어 private 타입 별칭은 private, file-private, internal, public, 또는 open 타입의 별칭이 가능하지만 public 타입 별칭은 internal, file-private, 또는 private 타입의 별칭만 가능합니다.

> NOTE   
> 이 규칙은 프로토콜 준수성을 충족하는데 사용되는 관련된 타입의 타입 별칭에도 적용됩니다.

