{% hint style="info" %}
📢 **공지**  
이 문서는 더 이상 업데이트되지 않습니다.  
아래 새로운 URL로 업데이트 되었으니, 새로운 URL로 접속해 주세요.  
[Swift Book Korean](https://bbiguduk.github.io/swift-book-korean/documentation/tsplk/)
{% endhint %}

# 접근 제어 (Access Control)

선언, 파일, 모듈로 코드의 가시성을 관리합니다.

*접근 제어(Access control)*는 다른 소스 파일과 모듈의
코드 접근을 제한합니다.
이 기능은 코드의 세부 구현을 숨기고,
해당 코드에 접근하고 사용할 수 있는 기본 인터페이스를 지정할 수 있습니다.

구체적인 접근 수준을 개별 타입
(클래스, 구조체, 열거형)뿐만 아니라,
해당 타입에 속하는 프로퍼티, 메서드, 이니셜라이저, 서브스크립트에도 지정할 수 있습니다.
프로토콜은 특정 컨텍스트로 제한할 수 있으며,
전역 상수, 변수, 함수도 마찬가지입니다.

다양한 수준의 접근 제어를 제공하는 것 외에도,
Swift는 일반적인 상황에 대한 기본 접근 수준을 제공하여
명시적으로 접근 제어 수준을 지정할 필요성을 줄여줍니다.
실제로 단일 앱을 작성한다면,
접근 제어 수준을 지정할 필요가 없습니다.

> Note: 접근 제어를 적용할 수 있는 코드의 다양한 요소
> (프로퍼티, 타입, 함수 등)는
> 간결성을 위해 아래에서 "엔티티"라고 합니다.

## 모듈, 소스 파일, 패키지 (Modules, Source Files, and Packages)

Swift의 접근 제어 모델은
모듈, 소스 파일, 패키지의 개념을 기초로 합니다.

*모듈(module)*은 코드 배포의 단일 단위입니다 ---
단일 단위로 빌드되고 Swift의 `import` 키워드로 다른 모듈에서 가져올 수 있는
프레임워크(framework)나 애플리케이션(application)이 모듈에 해당합니다.

Xcode에서 각 빌드 타겟(앱 번들이나 프레임워크)은
Swift에서 개별 모듈로 취급합니다.
앱의 코드를 별도의 프레임워크로 분리하여 ---
여러 애플리케이션에서 재사용하도록 구성했다면 ---
해당 프레임워크 내의 모든 정의는
앱이나 다른 프레임워크에서 사용할 때
별도의 모듈로 간주됩니다.

*소스 파일(source file)*은 모듈 내 하나의 Swift 소스 코드 파일입니다
(앱이나 프레임워크 내에 단일 파일).
일반적으로 타입마다 별도의 소스 파일에 정의하지만,
하나의 소스 파일에 여러 타입, 함수 등을 정의할 수도 있습니다.

*패키지(package)*는 하나의 단위로 개발되는 모듈의 집합입니다.
패키지를 구성하는 모듈을
Swift 소스 코드에서 정의하지 않고,
사용 중인 빌드 시스템 구성에서 정의합니다.
예를 들어, Swift Package Manager를 사용한다면,
`Package.swift` 파일에 [PackageDescription][] 모듈의 API를 사용하여
패키지를 정의하고,
Xcode를 사용하면, Package Access Identifier 빌드 설정에서
패키지 이름을 지정합니다.

[PackageDescription]: https://developer.apple.com/documentation/packagedescription

## 접근 수준 (Access Levels)

Swift는 코드 내에서 엔티티에 대해 여섯 가지 *접근 수준(access levels)*을 제공합니다.
이 접근 수준은 엔티티가 정의된 소스파일과 관련되며
그 소스 파일이 속한 모듈과
그 모듈이 속한 패키지를 기준으로 결정됩니다.

- *open 접근*과 *public 접근*은
  정의된 모듈의 모든 소스 파일에서 해당 엔티티를 사용할 수 있으며,
  정의한 모듈을 임포트(import)하고 다른 모듈의 소스 파일에서도 엔티티를 사용할 수 있습니다.
  open 접근이나 public 접근은 일반적으로 프레임워크를 공개 인터페이스로 지정할 때 사용합니다.
  open 접근과 public 접근의 차이점은 아래에 설명되어 있습니다.
- *package 접근*은
  정의된 패키지 내의 모든 소스 파일에서
  해당 엔티티를 사용할 수 있도록 허용하지만,
  해당 패키지 외부의 소스 파일에서는 사용할 수 없습니다.
  package 접근은 여러 모듈로 구성된 앱이나 프레임워크에서
  주로 사용합니다.
- *internal 접근*은
  정의된 모듈의 모든 소스 파일에서 해당 엔티티를 사용할 수 있지만,
  해당 모듈 외부의 소스 파일에서는 사용할 수 없습니다.
  internal 접근은 일반적으로
  앱이나 프레임워크의 내부 구조체를 정의할 때 사용합니다.
- *file-private 접근*은
  정의된 소스 파일 내에서만 엔티티의 사용을 허용합니다.
  file-private 접근은 특정 기능의 세부 구현이
  파일 전체에서 사용되지만
  외부에 노출되지 않도록 하려는 경우에 사용합니다.
- *private 접근*은
  해당 엔티티를 둘러싸는 선언 내부와
  같은 파일에 있는 해당 선언의 확장에서만 사용을 허용합니다.
  private 접근은 특정 기능의 세부 구현이
  단일 선언 내에서만 사용되고
  외부에 노출되지 않도록 하려는 경우에 사용합니다.

open 접근은 가장 높은(가장 적은 제약) 접근 수준이며
private 접근은 가장 낮은(가장 높은 제약) 접근 수준입니다.

open 접근은 클래스와 클래스 멤버에만 적용되고
아래의 [하위 클래스 (Subclassing)](#하위-클래스-subclassing)에서 설명했듯이
외부 모듈에서 하위 클래스와 재정의를 허용한다는 점에서
public 접근과 다릅니다.
명시적으로 클래스를 open으로 표기하면
다른 모듈이 해당 클래스를
상위 클래스로 사용할 수 있음을 고려하고
그에 따라 클래스 코드를 설계했음을 나타냅니다.

### 접근 수준의 기본 원칙 (Guiding Principle of Access Levels)

Swift에서 접근 수준은 전반적인 기본 원칙을 따릅니다:
*엔티티는 더 낮은(더 제한적) 접근 수준을 가진
다른 엔티티를 기반으로 정의될 수 없습니다.*

예를 들어:

- public 변수는 internal, file-private, private 타입으로 정의될 수 없습니다.
  해당 타입은 public 변수가 사용되는 모든 곳에서 접근 가능하지 않을 수 있기 때문입니다.
- 함수는 매개변수 타입과 반환 타입보다 더 높은 접근 수준을 가질 수 없습니다.
  해당 타입이 호출하는 코드에서
  접근 가능하지 않을 수 있기 때문입니다.

이 기본 원칙이 언어의 여러 측면에 어떤 영향을 미치는지는
아래에 자세히 설명되어 있습니다.

### 기본 접근 수준 (Default Access Levels)

코드의 모든 엔티티
(이 챕터의 후반부에서 설명하는 몇 가지 예외를 제외하고)는
명시적으로 접근 수준을 지정하지 않으면
기본적으로 internal 접근 수준을 가집니다.
이로 인해 많은 경우에
코드에서 명시적으로 접근 수준을 지정할 필요가 없습니다.

### 단일 타겟 앱에 대한 접근 수준 (Access Levels for Single-Target Apps)

간단한 단일 타겟 앱을 작성할 경우,
앱의 코드는 일반적으로 앱 내에 자체적으로 포함되며
앱의 모듈 외부에서 사용할 수 있도록 만들 필요가 없습니다.
internal 접근 수준은 이 요구사항과 일치합니다.
따라서 커스텀 접근 수준을 지정할 필요가 없습니다.
그러나 앱 모듈 내의 다른 코드에서 세부 구현을 숨기기 위해
file private이나 private로 코드의 어떤 부분을 표기할 수 있습니다.

### 프레임워크에 대한 접근 수준 (Access Levels for Frameworks)

프레임워크를 개발할 때는
프레임워크를 가져오는 앱과 같은
다른 모듈에서 보고 접근할 수 있게 하기 위해
해당 프레임워크에 대한 공용 인터페이스를 open이나 public으로 표시합니다.
이 공용 인터페이스는 프레임워크의
애플리케이션 프로그래밍 인터페이스 또는 API입니다.

> Note: 프레임워크의 내부 세부 구현은 여전히
> internal 접근 수준을 사용할 수 있으며,
> 프레임워크의 내부 코드를 숨기기 위해
> private이나 file private로 표기할 수 있습니다.
> 프레임워크 API의 부분이 되길 원한다면
> 엔티티를 open이나 public으로 표시해야 합니다.

### 유닛 테스트 타겟에 대한 접근 수준 (Access Levels for Unit Test Targets)

유닛 테스트 타겟이 있는 앱을 작성하는 경우에는
앱의 코드는 테스트 모듈에서 사용할 수 있어야 합니다.
기본적으로 open이나 public으로 표기된 엔티티만
다른 모듈에서 접근할 수 있습니다.
그러나 모듈에 대한 임포트(import) 선언을 `@testable` 속성으로 표시하고
테스트를 활성화하여 해당 모듈을 컴파일하면
유닛 테스트 타겟은 모든 internal 엔티티에 접근할 수 있습니다.

## 접근 제어 문법 (Access Control Syntax)

엔티티의 선언 앞에
`public`이나 `private`처럼
[접근 수준 (Access Levels)](#접근-수준-access-levels)에 나열된 수정자 중 하나를
위치시켜 엔티티에 대한 접근 수준을 정의합니다.
예를 들어:

```swift
public class SomePublicClass {}
internal struct SomeInternalStruct() {}
private func somePrivateFunction() {}
```

위 코드는 `SomePublicClass`는 public으로
`SomeInternalStruct`는 internal로
`SomePrivateFunction()`은 private로 선언했습니다.
명시적으로 접근 수준을 작성하지 않으면,
[기본 접근 수준 (Default Access Levels)](#기본-접근-수준-default-access-levels)에서 설명한대로
기본 접근 수준 수정자는 `internal`입니다.
예를 들어 아래 코드의 `SomeInternalStruct`는 암시적으로 internal입니다:

```swift
struct SomeInternalStruct() {}
```

## 커스텀 타입 (Custom Types)

커스텀 타입에 대해 명시적으로 접근 수준을 지정하고 싶다면
타입을 정의할 때 지정합니다.
그러면 새로운 타입은 접근 수준이 허용하는 범위내에서 사용할 수 있습니다.
예를 들어 file-private 클래스를 정의하면
해당 클래스는 file-private 클래스가 정의된 소스 파일 내에서만
프로퍼티 타입,
함수 매개변수, 반환 타입으로 사용될 수 있습니다.

타입의 접근 제어 수준은 해당 타입의 *멤버*
(프로퍼티, 메서드, 이니셜라이저, 서브스크립트)의
기본 접근 수준에도 영향을 줍니다.
타입의 접근 수준을 private이나 file private로 정의하면,
멤버의 기본 접근 수준 또한 private이나 file private가 됩니다.
타입의 접근 수준을 internal이나 public
(또는 명시적으로 접근 수준을 지정하지 않아
internal의 기본 접근 수준)으로
정의하면 타입의 멤버의 기본 접근 수준은 internal이 됩니다.

> Important: public 타입은 기본적으로 public 멤버가 아닌 internal 멤버를 가집니다.
> 타입 멤버를 public으로 하려면 명시적으로 표시해야 합니다.
> 이 요구사항은 타입에 대한 공용 API는
> 공개되도록 하고
> 실수로 타입의 내부 작업을 공개 API로 표시되지 않도록 합니다.

```swift
public class SomePublicClass {                   // explicitly public class
    public var somePublicProperty = 0            // explicitly public class member
    var someInternalProperty = 0                 // implicitly internal class member
    fileprivate func someFilePrivateMethod() {}  // explicitly file-private class member
    private func somePrivateMethod() {}          // explicitly private class member
}

class SomeInternalClass {                        // implicitly internal class
    var someInternalProperty = 0                 // implicitly internal class member
    fileprivate func someFilePrivateMethod() {}  // explicitly file-private class member
    private func somePrivateMethod() {}          // explicitly private class member
}

fileprivate class SomeFilePrivateClass {         // explicitly file-private class
    func someFilePrivateMethod() {}              // implicitly file-private class member
    private func somePrivateMethod() {}          // explicitly private class member
}

private class SomePrivateClass {                 // explicitly private class
    func somePrivateMethod() {}                  // implicitly private class member
}
```

<!--
  - test: `accessControl, accessControlWrong`

  ```swifttest
  -> public class SomePublicClass {                  // explicitly public class
        public var somePublicProperty = 0            // explicitly public class member
        var someInternalProperty = 0                 // implicitly internal class member
        fileprivate func someFilePrivateMethod() {}  // explicitly file-private class member
        private func somePrivateMethod() {}          // explicitly private class member
     }

  -> class SomeInternalClass {                       // implicitly internal class
        var someInternalProperty = 0                 // implicitly internal class member
        fileprivate func someFilePrivateMethod() {}  // explicitly file-private class member
        private func somePrivateMethod() {}          // explicitly private class member
     }

  -> fileprivate class SomeFilePrivateClass {        // explicitly file-private class
        func someFilePrivateMethod() {}              // implicitly file-private class member
        private func somePrivateMethod() {}          // explicitly private class member
     }

  -> private class SomePrivateClass {                // explicitly private class
        func somePrivateMethod() {}                  // implicitly private class member
     }
  ```
-->

### 튜플 타입 (Tuple Types)

튜플 타입에 대한 접근 수준은
튜플을 구성하는 타입 중 가장 제한적인 접근 수준으로 결정됩니다.
예를 들어 하나는 internal 접근이고 하나는 private 접근인
두 개의 다른 타입의 튜플을 구성하는 경우
해당 복합 튜플 타입에 대한 접근 수준은 private입니다.

<!--
  - test: `tupleTypes_Module1, tupleTypes_Module1_PublicAndInternal, tupleTypes_Module1_Private`

  ```swifttest
  -> public struct PublicStruct {}
  -> internal struct InternalStruct {}
  -> fileprivate struct FilePrivateStruct {}
  -> public func returnPublicTuple() -> (PublicStruct, PublicStruct) {
        return (PublicStruct(), PublicStruct())
     }
  -> func returnInternalTuple() -> (PublicStruct, InternalStruct) {
        return (PublicStruct(), InternalStruct())
     }
  -> fileprivate func returnFilePrivateTuple() -> (PublicStruct, FilePrivateStruct) {
        return (PublicStruct(), FilePrivateStruct())
     }
  ```
-->

<!--
  - test: `tupleTypes_Module1_PublicAndInternal`

  ```swifttest
  // tuples with (at least) internal members can be accessed within their own module
  -> let publicTuple = returnPublicTuple()
  -> let internalTuple = returnInternalTuple()
  ```
-->

<!--
  - test: `tupleTypes_Module1_Private`

  ```swifttest
  // a tuple with one or more private members can't be accessed from outside of its source file
  -> let privateTuple = returnFilePrivateTuple()
  !$ error: cannot find 'returnFilePrivateTuple' in scope
  !! let privateTuple = returnFilePrivateTuple()
  !!                    ^~~~~~~~~~~~~~~~~~~~~~
  ```
-->

<!--
  - test: `tupleTypes_Module2_Public`

  ```swifttest
  // a public tuple with all-public members can be used in another module
  -> import tupleTypes_Module1
  -> let publicTuple = returnPublicTuple()
  ```
-->

<!--
  - test: `tupleTypes_Module2_InternalAndPrivate`

  ```swifttest
  // tuples with internal or private members can't be used outside of their own module
  -> import tupleTypes_Module1
  -> let internalTuple = returnInternalTuple()
  -> let privateTuple = returnFilePrivateTuple()
  !$ error: cannot find 'returnInternalTuple' in scope
  !! let internalTuple = returnInternalTuple()
  !!                     ^~~~~~~~~~~~~~~~~~~
  !$ error: cannot find 'returnFilePrivateTuple' in scope
  !! let privateTuple = returnFilePrivateTuple()
  !!                    ^~~~~~~~~~~~~~~~~~~~~~
  ```
-->

> Note: 튜플 타입은 클래스, 구조체, 열거형, 함수와 같이
> 독립적으로 정의되지 않습니다.
> 튜플 타입의 접근 수준은
> 튜플 타입을 구성하는 타입에 따라 자동으로 결정되고
> 명시적으로 지정할 수 없습니다.

### 함수 타입 (Function Types)

함수 타입에 대한 접근 수준은
함수의 매개변수 타입과 반환 타입 중 가장 제한적인 접근 수준으로 결정됩니다.
함수의 접근 수준이 컨텍스트 기본 접근 수준과 일치하지 않는다면
함수 선언 시 접근 수준을 명시적으로 지정해야 합니다.

아래의 예시는 `someFunction()`이라는 전역 함수를 정의하고,
함수에 접근 수준을 지정하지 않습니다.
이 함수는 "internal"의 기본 접근 수준을 가진다고 생각되지만,
여기서는 그렇지 않습니다.
실제로 `someFunction()`은 아래와 같이 작성되면 컴파일되지 않습니다:

```swift
func someFunction() -> (SomeInternalClass, SomePrivateClass) {
    // function implementation goes here
}
```

<!--
  - test: `accessControlWrong`

  ```swifttest
  -> func someFunction() -> (SomeInternalClass, SomePrivateClass) {
        // function implementation goes here
  >>    return (SomeInternalClass(), SomePrivateClass())
     }
  !! /tmp/swifttest.swift:19:6: error: function must be declared private or fileprivate because its result uses a private type
  !! func someFunction() -> (SomeInternalClass, SomePrivateClass) {
  !! ^
  ```
-->

함수의 반환 타입은
위에 [커스텀 타입 (Custom Types)](#커스텀-타입-custom-types)에서 정의한 두 개의 커스텀 클래스로 구성된 튜플 타입입니다.
이 클래스 중 하나는 internal로
다른 하나는 private로 정의됩니다.
따라서 튜플 타입의 전체 접근 수준은 private입니다
(튜플의 구성 타입의 최소 접근 수준).

함수의 반환 타입은 private이므로,
함수 선언이 유효하려면
`private` 수정자와 함께 함수의 전체 접근 수준을 표시해야 합니다:

```swift
private func someFunction() -> (SomeInternalClass, SomePrivateClass) {
    // function implementation goes here
}
```

<!--
  - test: `accessControl`

  ```swifttest
  -> private func someFunction() -> (SomeInternalClass, SomePrivateClass) {
        // function implementation goes here
  >>    return (SomeInternalClass(), SomePrivateClass())
     }
  ```
-->

`someFunction()` 함수가
public이나 internal인 경우에는
함수의 반환 타입에 접근할 수 없기 때문에
함수 정의에 `public`이나 `internal` 수정자로 표시하거나
기본 접근 제어 수준을 사용하는 것은 유효하지 않습니다.

### 열거형 타입 (Enumeration Types)

열거형의 개별 케이스는
열거형과 같은 접근 수준을 자동으로 받습니다.
각 케이스에 대해 다른 접근 수준을 지정할 수 없습니다.

아래의 예시에서
`CompassPoint` 열거형은 public의 명시적 접근 수준을 갖습니다.
따라서 `north`, `south`, `east`, `west` 열거형 케이스도
public의 접근 수준을 가집니다:

```swift
public enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

<!--
  - test: `enumerationCases`

  ```swifttest
  -> public enum CompassPoint {
        case north
        case south
        case east
        case west
     }
  ```
-->

<!--
  - test: `enumerationCases_Module1`

  ```swifttest
  -> public enum CompassPoint {
        case north
        case south
        case east
        case west
     }
  ```
-->

<!--
  - test: `enumerationCases_Module2`

  ```swifttest
  -> import enumerationCases_Module1
  -> let north = CompassPoint.north
  ```
-->

#### 원시값과 연관값 (Raw Values and Associated Values)

열거형 정의의 모든 원시값이나 연관값에 사용되는 타입은
열거형의 접근 수준보다 높은 접근 수준을 가져야 합니다.
예를 들어
internal 접근 수준을 가진 열거형에
private 타입을 원시값으로 사용할 수 없습니다.

### 중첩 타입 (Nested Types)

중첩 타입의 접근 수준은 해당 타입이 포함된 타입이 public일 경우를 제외하고
타입과 동일합니다.
public 타입 내에 정의된 중첩 타입은
자동으로 internal 접근 수준을 가집니다.
public 타입 내에 중첩 타입을 공개하려면,
명시적으로 public 접근 수준으로 중첩 타입을 선언해야 합니다.

<!--
  - test: `nestedTypes_Module1, nestedTypes_Module1_PublicAndInternal, nestedTypes_Module1_Private`

  ```swifttest
  -> public struct PublicStruct {
        public enum PublicEnumInsidePublicStruct { case a, b }
        internal enum InternalEnumInsidePublicStruct { case a, b }
        private enum PrivateEnumInsidePublicStruct { case a, b }
        enum AutomaticEnumInsidePublicStruct { case a, b }
     }
  -> internal struct InternalStruct {
        internal enum InternalEnumInsideInternalStruct { case a, b }
        private enum PrivateEnumInsideInternalStruct { case a, b }
        enum AutomaticEnumInsideInternalStruct { case a, b }
     }
  -> private struct FilePrivateStruct {
        enum AutomaticEnumInsideFilePrivateStruct { case a, b }
        private enum PrivateEnumInsideFilePrivateStruct { case a, b }
     }
  -> private struct PrivateStruct {
        enum AutomaticEnumInsidePrivateStruct { case a, b }
        private enum PrivateEnumInsidePrivateStruct { case a, b }
     }
  ```
-->

<!--
  - test: `nestedTypes_Module1_PublicAndInternal`

  ```swifttest
  // these are all expected to succeed within the same module
  -> let publicNestedInsidePublic = PublicStruct.PublicEnumInsidePublicStruct.a
  -> let internalNestedInsidePublic = PublicStruct.InternalEnumInsidePublicStruct.a
  -> let automaticNestedInsidePublic = PublicStruct.AutomaticEnumInsidePublicStruct.a

  -> let internalNestedInsideInternal = InternalStruct.InternalEnumInsideInternalStruct.a
  -> let automaticNestedInsideInternal = InternalStruct.AutomaticEnumInsideInternalStruct.a
  ```
-->

<!--
  - test: `nestedTypes_Module1_Private`

  ```swifttest
  // these are all expected to fail, because they're private to the other file
  -> let privateNestedInsidePublic = PublicStruct.PrivateEnumInsidePublicStruct.a

  -> let privateNestedInsideInternal = InternalStruct.PrivateEnumInsideInternalStruct.a

  -> let privateNestedInsidePrivate = PrivateStruct.PrivateEnumInsidePrivateStruct.a
  -> let automaticNestedInsidePrivate = PrivateStruct.AutomaticEnumInsidePrivateStruct.a

  !$ error: 'PrivateEnumInsidePublicStruct' is inaccessible due to 'private' protection level
  !! let privateNestedInsidePublic = PublicStruct.PrivateEnumInsidePublicStruct.a
  !!                                              ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  !$ note: 'PrivateEnumInsidePublicStruct' declared here
  !! private enum PrivateEnumInsidePublicStruct { case a, b }
  !! ^
  !$ error: 'PrivateEnumInsideInternalStruct' is inaccessible due to 'private' protection level
  !! let privateNestedInsideInternal = InternalStruct.PrivateEnumInsideInternalStruct.a
  !!                                                  ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  !$ note: 'PrivateEnumInsideInternalStruct' declared here
  !! private enum PrivateEnumInsideInternalStruct { case a, b }
  !! ^
  !$ error: cannot find 'PrivateStruct' in scope
  !! let privateNestedInsidePrivate = PrivateStruct.PrivateEnumInsidePrivateStruct.a
  !!                                  ^~~~~~~~~~~~~
  !$ error: cannot find 'PrivateStruct' in scope
  !! let automaticNestedInsidePrivate = PrivateStruct.AutomaticEnumInsidePrivateStruct.a
  !!                                    ^~~~~~~~~~~~~
  ```
-->

<!--
  - test: `nestedTypes_Module2_Public`

  ```swifttest
  // this is the only expected to succeed within the second module
  -> import nestedTypes_Module1
  -> let publicNestedInsidePublic = PublicStruct.PublicEnumInsidePublicStruct.a
  ```
-->

<!--
  - test: `nestedTypes_Module2_InternalAndPrivate`

  ```swifttest
  // these are all expected to fail, because they're private or internal to the other module
  -> import nestedTypes_Module1
  -> let internalNestedInsidePublic = PublicStruct.InternalEnumInsidePublicStruct.a
  -> let automaticNestedInsidePublic = PublicStruct.AutomaticEnumInsidePublicStruct.a
  -> let privateNestedInsidePublic = PublicStruct.PrivateEnumInsidePublicStruct.a

  -> let internalNestedInsideInternal = InternalStruct.InternalEnumInsideInternalStruct.a
  -> let automaticNestedInsideInternal = InternalStruct.AutomaticEnumInsideInternalStruct.a
  -> let privateNestedInsideInternal = InternalStruct.PrivateEnumInsideInternalStruct.a

  -> let privateNestedInsidePrivate = PrivateStruct.PrivateEnumInsidePrivateStruct.a
  -> let automaticNestedInsidePrivate = PrivateStruct.AutomaticEnumInsidePrivateStruct.a

  !$ error: 'InternalEnumInsidePublicStruct' is inaccessible due to 'internal' protection level
  !! let internalNestedInsidePublic = PublicStruct.InternalEnumInsidePublicStruct.a
  !!                                               ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  !$ note: 'InternalEnumInsidePublicStruct' declared here
  !! internal enum InternalEnumInsidePublicStruct {
  !!               ^
  !$ error: 'AutomaticEnumInsidePublicStruct' is inaccessible due to 'internal' protection level
  !! let automaticNestedInsidePublic = PublicStruct.AutomaticEnumInsidePublicStruct.a
  !!                                                ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  !$ note: 'AutomaticEnumInsidePublicStruct' declared here
  !! internal enum AutomaticEnumInsidePublicStruct {
  !!               ^
  !$ error: 'PrivateEnumInsidePublicStruct' is inaccessible due to 'private' protection level
  !! let privateNestedInsidePublic = PublicStruct.PrivateEnumInsidePublicStruct.a
  !!                                              ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  !$ note: 'PrivateEnumInsidePublicStruct' declared here
  !! private enum PrivateEnumInsidePublicStruct {
  !!              ^
  !$ error: cannot find 'InternalStruct' in scope
  !! let internalNestedInsideInternal = InternalStruct.InternalEnumInsideInternalStruct.a
  !!                                    ^~~~~~~~~~~~~~
  !$ error: cannot find 'InternalStruct' in scope
  !! let automaticNestedInsideInternal = InternalStruct.AutomaticEnumInsideInternalStruct.a
  !!                                     ^~~~~~~~~~~~~~
  !$ error: cannot find 'InternalStruct' in scope
  !! let privateNestedInsideInternal = InternalStruct.PrivateEnumInsideInternalStruct.a
  !!                                   ^~~~~~~~~~~~~~
  !$ error: cannot find 'PrivateStruct' in scope
  !! let privateNestedInsidePrivate = PrivateStruct.PrivateEnumInsidePrivateStruct.a
  !!                                  ^~~~~~~~~~~~~
  !$ error: cannot find 'PrivateStruct' in scope
  !! let automaticNestedInsidePrivate = PrivateStruct.AutomaticEnumInsidePrivateStruct.a
  !!                                    ^~~~~~~~~~~~~
  ```
-->

## 하위 클래스 (Subclassing)

현재 접근 컨텍스트에서 접근 가능한 클래스이며,
동일한 모듈에 정의된 클래스는
하위 클래스로 지정할 수 있습니다.
다른 모듈에 정의된
open 클래스도 하위 클래스로 지정할 수 있습니다.
하위 클래스는 상위 클래스의 접근 수준보다 높은 접근 수준을 가질 수 없습니다 ---
예를 들어 internal 상위 클래스의 public 하위 클래스를 선언할 수 없습니다.

또한
같은 모듈에 정의된 클래스의 경우
해당 접근 컨텍스트에 표시되는 모든 클래스 멤버
(메서드, 프로퍼티, 이니셜라이저, 서브스크립트)를
재정의할 수 있습니다.
다른 모듈에 정의된 클래스의 경우
모든 open 클래스 멤버를 재정의할 수 있습니다.

재정의는 상속된 클래스 멤버의 접근 수준보다 더 높은 접근 수준으로 설정할 수 있습니다.
아래의 예시에서 클래스 `A`는 public 클래스이고, `someMethod()`라는 file-private 메서드를 가집니다.
클래스 `B`는 `A`를 상속하고 "internal"의 접근 수준을 가집니다.
그럼에도 불구하고 클래스 `B`는 `someMethod()`를 기존 구현보다
*높은* 접근 수준인 "internal"의 접근 수준을 가진
`someMethod()`의 재정의를 제공합니다:

```swift
public class A {
    fileprivate func someMethod() {}
}

internal class B: A {
    override internal func someMethod() {}
}
```

<!--
  - test: `subclassingNoCall`

  ```swifttest
  -> public class A {
        fileprivate func someMethod() {}
     }

  -> internal class B: A {
        override internal func someMethod() {}
     }
  ```
-->

상위 클래스의 멤버에 대한 호출이
허용된 접근 수준 컨텍스트 내에서 발생하는 한
하위 클래스 멤버보다 더 낮은 접근 권한을 가진
상위 클래스 멤버를 호출하는 것도 유효합니다
(상위 클래스의 fileprivate 멤버 호출의 경우 같은 소스 파일 내
또는 상위 클래스의 internal 멤버 호출의 경우 같은 모듈 내):

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

<!--
  - test: `subclassingWithCall`

  ```swifttest
  -> public class A {
        fileprivate func someMethod() {}
     }

  -> internal class B: A {
        override internal func someMethod() {
           super.someMethod()
        }
     }
  ```
-->

상위 클래스 `A`와 하위 클래스 `B`는 같은 소스 파일에 정의되었으므로,
`B`의 `someMethod()`에서
`super.someMethod()`를 호출하는 것은 유효합니다.

## 상수, 변수, 프로퍼티, 서브스크립트 (Constants, Variables, Properties, and Subscripts)

상수, 변수, 프로퍼티는 타입보다 더 높은 접근 수준을 가질 수 없습니다.
예를 들어 private 타입에 public 프로퍼티를 작성하는 것은 유효하지 않습니다.
유사하게 서브스크립트는 인덱스 타입이나 반환 타입보다 더 높은 접근 수준을 가질 수 없습니다.

상수, 변수, 프로퍼티, 서브스크립트가 private 타입에 사용되는 경우,
상수, 변수, 프로퍼티, 서브스크립트도 `private`로 표시되어야 합니다:

```swift
private var privateInstance = SomePrivateClass()
```

<!--
  - test: `accessControl`

  ```swifttest
  -> private var privateInstance = SomePrivateClass()
  ```
-->

<!--
  - test: `useOfPrivateTypeRequiresPrivateModifier`

  ```swifttest
  -> class Scope {  // Need to be in a scope to meaningfully use private (vs fileprivate)
  -> private class SomePrivateClass {}
  -> let privateConstant = SomePrivateClass()
  !! /tmp/swifttest.swift:3:5: error: property must be declared private because its type 'Scope.SomePrivateClass' uses a private type
  !! let privateConstant = SomePrivateClass()
  !! ^
  -> var privateVariable = SomePrivateClass()
  !! /tmp/swifttest.swift:4:5: error: property must be declared private because its type 'Scope.SomePrivateClass' uses a private type
  !! var privateVariable = SomePrivateClass()
  !! ^
  -> class C {
        var privateProperty = SomePrivateClass()
        subscript(index: Int) -> SomePrivateClass {
           return SomePrivateClass()
        }
     }
  -> }  // End surrounding scope
  !! /tmp/swifttest.swift:6:8: error: property must be declared private because its type 'Scope.SomePrivateClass' uses a private type
  !! var privateProperty = SomePrivateClass()
  !! ^
  !! /tmp/swifttest.swift:7:4: error: subscript must be declared private because its element type uses a private type
  !! subscript(index: Int) -> SomePrivateClass {
  !! ^                        ~~~~~~~~~~~~~~~~
  !! /tmp/swifttest.swift:2:15: note: type declared here
  !! private class SomePrivateClass {}
  !! ^
  ```
-->

### Getter와 Setter (Getters and Setters)

상수, 변수, 프로퍼티, 서브스크립트에 대한 getter와 setter는
자동으로 속해있는 상수, 변수, 프로퍼티, 서브스크립트와
같은 접근 수준을 받습니다.

해당 변수, 프로퍼티, 서브스크립트의 읽기-쓰기 범위를 제한하기 위해
getter보다 *더 낮은* 접근 수준으로 setter를 제공할 수 있습니다.
setter의 접근 수준을 낮추려면
`var`나 `subscript` 선언 앞에
`fileprivate(set)`, `private(set)`, `internal(set)`을 작성합니다.

> Note: 이 규칙은 저장 프로퍼티 뿐만 아니라 연산 프로퍼티에도 적용됩니다.
> 저장 프로퍼티에 대해 명시적으로 getter와 setter를 작성하지 않아도,
> Swift는 저장 프로퍼티의 저장소에 대한 접근을 제공하기 위해
> getter와 setter를 자동으로 생성합니다.
> 연산 프로퍼티의
> 명시적 setter와 같은 방식으로 자동 생성된 setter의
> 접근 수준을 변경하기 위해
> `fileprivate(set)`, `private(set)`, `internal(set)`을 사용합니다.

아래 예시는 `TrackedString` 이라는 구조체를 정의하고,
문자열 프로퍼티가 몇 번 수정되는지 추적합니다:

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

<!--
  - test: `reducedSetterScope, reducedSetterScope_error`

  ```swifttest
  -> struct TrackedString {
        private(set) var numberOfEdits = 0
        var value: String = "" {
           didSet {
              numberOfEdits += 1
           }
        }
     }
  ```
-->

`TrackedString` 구조체는 `value`라는 문자열 저장 프로퍼티를 정의하고,
빈 문자열인 `""`의 초기값을 가집니다.
이 구조체는 `numberOfEdits`라는 정수 저장 프로퍼티도 정의하고,
`value`가 몇 번이나 수정되는지 추적하는데 사용합니다.
이 추적은
`value` 프로퍼티의 `didSet` 프로퍼티 관찰자로 구현되고,
`value` 프로퍼티에 새로운 값이 설정될 때마다 `numberOfEdits`가 증가합니다.

`TrackedString` 구조체와 `value` 프로퍼티는
명시적으로 접근 수준 수정자를 제공하지 않으므로,
internal의 기본 접근 수준을 받습니다.
그러나 `numberOfEdits`에 대한 접근 수준은
`private(set)` 수정자로 표기되고,
이것은
프로퍼티의 getter는 여전히 internal의 기본 접근 수준을 가지지만
프로퍼티는 `TrackedString` 구조체의 코드 내에서만
설정 가능함을 나타냅니다.
이렇게 하면 `TrackedString`이 내부적으로 `numberOfEdits` 프로퍼티를 수정할 수 있지만
구조체의 정의 외부에서 사용될 때는
읽기 전용 프로퍼티로 표시할 수 있습니다.

<!--
  - test: `reducedSetterScope_error`

  ```swifttest
  -> extension TrackedString {
         mutating func f() { numberOfEdits += 1 }
     }
  // check that we can't set its value with from the same file
  -> var s = TrackedString()
  -> let resultA: Void = { s.numberOfEdits += 1 }()
  !! /tmp/swifttest.swift:13:39: error: left side of mutating operator isn't mutable: 'numberOfEdits' setter is inaccessible
  !! let resultA: Void = { s.numberOfEdits += 1 }()
  !!                       ~~~~~~~~~~~~~~~ ^
  ```
-->

<!--
  The assertion above must be compiled because of a REPL bug
  <rdar://problem/54089342> REPL fails to enforce private(set) on struct property
-->

`TrackedString` 인스턴스를 생성하고 문자열 값을 몇 번 수정하면
`numberOfEdits` 프로퍼티 값이 수정된 수와 일치하게 업데이트 되는 것을 확인할 수 있습니다:

```swift
var stringToEdit = TrackedString()
stringToEdit.value = "This string will be tracked."
stringToEdit.value += " This edit will increment numberOfEdits."
stringToEdit.value += " So will this one."
print("The number of edits is \(stringToEdit.numberOfEdits)")
// Prints "The number of edits is 3"
```

<!--
  - test: `reducedSetterScope`

  ```swifttest
  -> var stringToEdit = TrackedString()
  -> stringToEdit.value = "This string will be tracked."
  -> stringToEdit.value += " This edit will increment numberOfEdits."
  -> stringToEdit.value += " So will this one."
  -> print("The number of edits is \(stringToEdit.numberOfEdits)")
  <- The number of edits is 3
  ```
-->

다른 소스 파일에서
`numberOfEdits` 프로퍼티의 현재값은 조회할 수 있지만
다른 소스 파일에서 프로퍼티를 *수정*할 수 없습니다.
이 제한은 `TrackedString` 편집 추적 기능의
세부 구현을 보호하는 동시에
해당 기능 측면에서 편리한 접근을 제공합니다.

필요한 경우 getter와 setter 모두에
명시적으로 접근 수준을 할당할 수 있습니다.
아래 예시는 `TrackedString` 구조체의 한 버전이고,
해당 구조체는 public의 명시적 접근 수준으로 정의되어 있습니다.
따라서 구조체의 멤버(`numberOfEdits` 프로퍼티 포함)는
기본적으로 internal 접근 수준을 가집니다.
`numberOfEdits` 프로퍼티 getter를 public으로
setter를 private로 만들 수 있으며,
이것은 `public`과 `private(set)` 접근 수준 수정자를 함께 사용하면 됩니다:

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

<!--
  - test: `reducedSetterScopePublic`

  ```swifttest
  -> public struct TrackedString {
        public private(set) var numberOfEdits = 0
        public var value: String = "" {
           didSet {
              numberOfEdits += 1
           }
        }
        public init() {}
     }
  ```
-->

<!--
  - test: `reducedSetterScopePublic_Module1_Allowed, reducedSetterScopePublic_Module1_NotAllowed`

  ```swifttest
  -> public struct TrackedString {
        public private(set) var numberOfEdits = 0
        public var value: String = "" {
           didSet {
              numberOfEdits += 1
           }
        }
        public init() {}
     }
  ```
-->

<!--
  - test: `reducedSetterScopePublic_Module1_Allowed`

  ```swifttest
  // check that we can retrieve its value with the public getter from another file in the same module
  -> var stringToEdit_Module1B = TrackedString()
  -> let resultB = stringToEdit_Module1B.numberOfEdits
  ```
-->

<!--
  - test: `reducedSetterScopePublic_Module1_NotAllowed`

  ```swifttest
  // check that we can't set its value from another file in the same module
  -> var stringToEdit_Module1C = TrackedString()
  -> let resultC: Void = { stringToEdit_Module1C.numberOfEdits += 1 }()
  !$ error: left side of mutating operator isn't mutable: 'numberOfEdits' setter is inaccessible
  !! let resultC: Void = { stringToEdit_Module1C.numberOfEdits += 1 }()
  !!                      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^
  ```
-->

<!--
  - test: `reducedSetterScopePublic_Module2`

  ```swifttest
  // check that we can retrieve its value with the public getter from a different module
  -> import reducedSetterScopePublic_Module1_Allowed
  -> var stringToEdit_Module2 = TrackedString()
  -> let result2Read = stringToEdit_Module2.numberOfEdits
  // check that we can't change its value from another module
  -> let result2Write: Void = { stringToEdit_Module2.numberOfEdits += 1 }()
  !$ error: left side of mutating operator isn't mutable: 'numberOfEdits' setter is inaccessible
  !! let result2Write: Void = { stringToEdit_Module2.numberOfEdits += 1 }()
  !!                            ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^
  ```
-->

## 이니셜라이저 (Initializers)

커스텀 이니셜라이저는 초기화하는 타입보다
더 낮거나 같은 접근 수준으로 할당할 수 있습니다.
유일한 예외상황은 필수 이니셜라이저입니다
([필수 이니셜라이저 (Required Initializers)](./initialization.md#필수-이니셜라이저-required-initializers)에서 설명됨).
필수 이니셜라이저는 자신이 속한 클래스와 동일한 접근 수준을 가져야 합니다.

함수나 메서드 매개변수와 마찬가지로
이니셜라이저의 매개변수의 타입은
이니셜라이저의 자체 접근 수준보다 더 낮을 수 없습니다.

### 기본 이니셜라이저 (Default Initializers)

[기본 이니셜라이저 (Default Initializers)](./initialization.md#기본-이니셜라이저-default-initializers)에서 설명했듯이,
Swift는 모든 프로퍼티에 기본값을 제공하고
커스텀 이니셜라이저를 제공하지 않는
구조체나 기본 클래스에 대해
인자없는 *기본 이니셜라이저(default initializer)*를 자동으로 제공합니다.

기본 이니셜라이저는 타입이 `public`으로 정의되지 않는 한
초기화하는 타입과 같은 접근 수준을 가집니다.
`public`으로 정의된 타입의 경우,
기본 이니셜라이저는 internal로 간주합니다.
다른 모듈에서 사용할 때
인자가 없는 이니셜라이저를 public 타입으로 초기화하려면
명시적으로 타입의 정의에서
public으로 인자가 없는 이니셜라이저를 제공해야 합니다.

### 구조체의 기본 멤버 이니셜라이저 (Default Memberwise Initializers for Structure Types)

구조체의 모든 저장 프로퍼티가 private라면,
구조체의 기본 멤버 이니셜라이저는 private라고 간주합니다.
마찬가지로 구조체의 모든 저장 프로퍼티가 fileprivate라면,
이니셜라이저는 fileprivate입니다.
그렇지 않으면 이니셜라이저는 internal의 접근 수준을 가집니다.

위의 기본 이니셜라이저와 마찬가지로
다른 모듈에서 사용될 때
멤버 이니셜라이저로 초기화 하는 public 구조체는
타입의 정의에 public 멤버 이니셜라이저를 자체적으로 제공해야 합니다.

## 프로토콜 (Protocols)

프로토콜 타입에 명시적으로 접근 수준을 할당하려면,
프로토콜을 정의하는 지점에서 지정하면 됩니다.
이를 통해 해당 접근 컨텍스트 내에서만
채택할 수 있는 프로토콜을 생성할 수 있습니다.

프로토콜 정의 내의 각 요구사항의 접근 수준은
자동으로 프로토콜과 같은 접근 수준으로 설정됩니다.
프로토콜 요구사항에 대해
별도로 다른 접근 수준을 설정할 수 없습니다.
이렇게 하면 모든 프로토콜의 요구사항은
프로토콜을 채택하는 모든 타입에서 볼 수 있습니다.

<!--
  - test: `protocolRequirementsCannotBeDifferentThanTheProtocol`

  ```swifttest
  -> public protocol PublicProtocol {
        public var publicProperty: Int { get }
        internal var internalProperty: Int { get }
        fileprivate var filePrivateProperty: Int { get }
        private var privateProperty: Int { get }
     }
  !$ error: 'public' modifier cannot be used in protocols
  !! public var publicProperty: Int { get }
  !! ^~~~~~~
  !!-
  !$ note: protocol requirements implicitly have the same access as the protocol itself
  !! public var publicProperty: Int { get }
  !! ^
  !$ error: 'internal' modifier cannot be used in protocols
  !! internal var internalProperty: Int { get }
  !! ^~~~~~~~~
  !!-
  !$ note: protocol requirements implicitly have the same access as the protocol itself
  !! internal var internalProperty: Int { get }
  !! ^
  !$ error: 'fileprivate' modifier cannot be used in protocols
  !! fileprivate var filePrivateProperty: Int { get }
  !! ^~~~~~~~~~~~
  !!-
  !$ note: protocol requirements implicitly have the same access as the protocol itself
  !! fileprivate var filePrivateProperty: Int { get }
  !! ^
  !$ error: 'private' modifier cannot be used in protocols
  !! private var privateProperty: Int { get }
  !! ^~~~~~~~
  !!-
  !$ note: protocol requirements implicitly have the same access as the protocol itself
  !! private var privateProperty: Int { get }
  !! ^
  ```
-->

> Note: public 프로토콜을 정의한다면,
> 프로토콜의 요구사항을 구현할 때
> public 접근 수준을 요구합니다.
> 이것은 public 타입의
> 멤버는 기본적으로 internal의 접근 수준을 가지는
> 다른 타입과 다릅니다.

<!--
  - test: `protocols_Module1, protocols_Module1_PublicAndInternal, protocols_Module1_Private`

  ```swifttest
  -> public protocol PublicProtocol {
        var publicProperty: Int { get }
        func publicMethod()
     }
  -> internal protocol InternalProtocol {
        var internalProperty: Int { get }
        func internalMethod()
     }
  -> fileprivate protocol FilePrivateProtocol {
        var filePrivateProperty: Int { get }
        func filePrivateMethod()
     }
  -> private protocol PrivateProtocol {
        var privateProperty: Int { get }
        func privateMethod()
     }
  ```
-->

<!--
  - test: `protocols_Module1_PublicAndInternal`

  ```swifttest
  // these should all be allowed without problem
  -> public class PublicClassConformingToPublicProtocol: PublicProtocol {
        public var publicProperty = 0
        public func publicMethod() {}
     }
  -> internal class InternalClassConformingToPublicProtocol: PublicProtocol {
        var publicProperty = 0
        func publicMethod() {}
     }
  -> private class PrivateClassConformingToPublicProtocol: PublicProtocol {
        var publicProperty = 0
        func publicMethod() {}
     }

  -> public class PublicClassConformingToInternalProtocol: InternalProtocol {
        var internalProperty = 0
        func internalMethod() {}
     }
  -> internal class InternalClassConformingToInternalProtocol: InternalProtocol {
        var internalProperty = 0
        func internalMethod() {}
     }
  -> private class PrivateClassConformingToInternalProtocol: InternalProtocol {
        var internalProperty = 0
        func internalMethod() {}
     }
  ```
-->

<!--
  - test: `protocols_Module1_Private`

  ```swifttest
  // these will fail, because FilePrivateProtocol isn't visible outside of its file
  -> public class PublicClassConformingToFilePrivateProtocol: FilePrivateProtocol {
        var filePrivateProperty = 0
        func filePrivateMethod() {}
     }
  !$ error: cannot find type 'FilePrivateProtocol' in scope
  !! public class PublicClassConformingToFilePrivateProtocol: FilePrivateProtocol {
  !! ^~~~~~~~~~~~~~~~~~~

  // these will fail, because PrivateProtocol isn't visible outside of its file
  -> public class PublicClassConformingToPrivateProtocol: PrivateProtocol {
        var privateProperty = 0
        func privateMethod() {}
     }
  !$ error: cannot find type 'PrivateProtocol' in scope
  !! public class PublicClassConformingToPrivateProtocol: PrivateProtocol {
  !! ^~~~~~~~~~~~~~~
  ```
-->

<!--
  - test: `protocols_Module2_Public`

  ```swifttest
  // these should all be allowed without problem
  -> import protocols_Module1
  -> public class PublicClassConformingToPublicProtocol: PublicProtocol {
        public var publicProperty = 0
        public func publicMethod() {}
     }
  -> internal class InternalClassConformingToPublicProtocol: PublicProtocol {
        var publicProperty = 0
        func publicMethod() {}
     }
  -> private class PrivateClassConformingToPublicProtocol: PublicProtocol {
        var publicProperty = 0
        func publicMethod() {}
     }
  ```
-->

<!--
  - test: `protocols_Module2_InternalAndPrivate`

  ```swifttest
  // these will all fail, because InternalProtocol, FilePrivateProtocol, and PrivateProtocol
  // aren't visible to other modules
  -> import protocols_Module1
  -> public class PublicClassConformingToInternalProtocol: InternalProtocol {
        var internalProperty = 0
        func internalMethod() {}
     }
  -> public class PublicClassConformingToFilePrivateProtocol: FilePrivateProtocol {
        var filePrivateProperty = 0
        func filePrivateMethod() {}
     }
  -> public class PublicClassConformingToPrivateProtocol: PrivateProtocol {
        var privateProperty = 0
        func privateMethod() {}
     }
  !$ error: cannot find type 'InternalProtocol' in scope
  !! public class PublicClassConformingToInternalProtocol: InternalProtocol {
  !! ^~~~~~~~~~~~~~~~
  !$ error: cannot find type 'FilePrivateProtocol' in scope
  !! public class PublicClassConformingToFilePrivateProtocol: FilePrivateProtocol {
  !! ^~~~~~~~~~~~~~~~~~~
  !$ error: cannot find type 'PrivateProtocol' in scope
  !! public class PublicClassConformingToPrivateProtocol: PrivateProtocol {
  !! ^~~~~~~~~~~~~~~
  ```
-->

### 프로토콜 상속 (Protocol Inheritance)

기존 프로토콜을 상속하는 새로운 프로토콜을 정의하면,
새로운 프로토콜은 상속된 프로토콜의 접근 수준 이하여야 합니다.
예를 들어
internal 프로토콜을 상속해서 public 프로토콜을 작성할 수 없습니다.

### 프로토콜 준수 (Protocol Conformance)

타입은 타입 자체보다 접근 수준이 더 낮은 프로토콜을 준수할 수 있습니다.
예를 들어 다른 모듈에서 사용할 수 있는 public 타입이
프로토콜을 정의한 모듈 내에서만
사용할 수 있는 internal 프로토콜을 준수할 수 있습니다.

타입이 특정 프로토콜을 준수하는 컨텍스트의 접근 수준은
타입의 접근 수준과 프로토콜의 접근 수준 중 더 낮은 수준으로 결정됩니다.
예를 들어 타입이 public이지만 준수하는 프로토콜이 internal인 경우,
해당 타입의 프로토콜에 대한 준수는 internal입니다.

프로토콜을 준수하고 그 요구사항을 구현하기 위해 타입을 작성하거나 확장할 때,
요구사항 구현의 접근 수준은
프로토콜 준수 수준 이상이어야 합니다.
예를 들어 public 타입이 internal 프로토콜을 준수하는 경우,
각 프로토콜 요구사항 구현은 적어도 internal이어야 합니다.

> Note: Objective-C처럼 Swift에서 프로토콜 준수성은 전역입니다 —--
> 같은 프로그램 내에서
> 하나의 타입이 두 가지 다른 방식으로 같은 프로토콜을 준수하는 것은 불가능 합니다.

## 확장 (Extensions)

클래스, 구조체, 열거형은 사용할 수 있는 모든 접근 컨텍스트에서
확장할 수 있습니다.
확장에 추가된 모든 타입 멤버는
확장된 기존 타입에 선언된 타입 멤버와 같은 기본 접근 수준을 가집니다.
public이나 internal 타입을 확장하면,
추가한 모든 새로운 타입 멤버는 internal의 기본 접근 수준을 가집니다.
fileprivate 타입을 확장하면,
추가한 모든 새로운 타입 멤버는 fileprivate의 기본 접근 수준을 가집니다.
private 타입을 확장하면,
추가한 모든 새로운 타입 멤버는 private의 기본 접근 수준을 가집니다.

단, 확장에 명시적으로 접근 수준 수정자를 붙이면
(예: `private`)
확장 내에서 정의된 모든 멤버는 새로운 기본 접근 수준으로 설정됩니다.
이 새로운 기본 접근 수준은 확장 내에서
개별 타입 멤버에 의해 재정의될 수 있습니다.

확장을 사용하여 프로토콜 준수성을 추가하는 경우
확장에 명시적 접근 수준 수정자를 제공할 수 없습니다.
대신 프로토콜의 자체 접근 수준은
확장 내 각 구현의 기본 접근 수준으로 적용됩니다.

<!--
  - test: `extensions_Module1, extensions_Module1_PublicAndInternal, extensions_Module1_Private`

  ```swifttest
  -> public struct PublicStruct {
        public init() {}
        func implicitlyInternalMethodFromStruct() -> Int { return 0 }
     }
  -> extension PublicStruct {
        func implicitlyInternalMethodFromExtension() -> Int { return 0 }
     }
  -> fileprivate extension PublicStruct {
        func filePrivateMethod() -> Int { return 0 }
     }
  -> var publicStructInSameFile = PublicStruct()
  -> let sameFileA = publicStructInSameFile.implicitlyInternalMethodFromStruct()
  -> let sameFileB = publicStructInSameFile.implicitlyInternalMethodFromExtension()
  -> let sameFileC = publicStructInSameFile.filePrivateMethod()
  ```
-->

<!--
  - test: `extensions_Module1_PublicAndInternal`

  ```swifttest
  -> var publicStructInDifferentFile = PublicStruct()
  -> let differentFileA = publicStructInDifferentFile.implicitlyInternalMethodFromStruct()
  -> let differentFileB = publicStructInDifferentFile.implicitlyInternalMethodFromExtension()
  ```
-->

<!--
  - test: `extensions_Module1_Private`

  ```swifttest
  -> var publicStructInDifferentFile = PublicStruct()
  -> let differentFileC = publicStructInDifferentFile.filePrivateMethod()
  !$ error: 'filePrivateMethod' is inaccessible due to 'fileprivate' protection level
  !! let differentFileC = publicStructInDifferentFile.filePrivateMethod()
  !!                                                  ^~~~~~~~~~~~~~~~~
  !$ note: 'filePrivateMethod()' declared here
  !! func filePrivateMethod() -> Int { return 0 }
  !! ^
  ```
-->

<!--
  - test: `extensions_Module2`

  ```swifttest
  -> import extensions_Module1
  -> var publicStructInDifferentModule = PublicStruct()
  -> let differentModuleA = publicStructInDifferentModule.implicitlyInternalMethodFromStruct()
  !$ error: 'implicitlyInternalMethodFromStruct' is inaccessible due to 'internal' protection level
  !! let differentModuleA = publicStructInDifferentModule.implicitlyInternalMethodFromStruct()
  !!                                                      ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  !$ note: 'implicitlyInternalMethodFromStruct()' declared here
  !! internal func implicitlyInternalMethodFromStruct() -> Int
  !!               ^
  -> let differentModuleB = publicStructInDifferentModule.implicitlyInternalMethodFromExtension()
  !$ error: 'implicitlyInternalMethodFromExtension' is inaccessible due to 'internal' protection level
  !! let differentModuleB = publicStructInDifferentModule.implicitlyInternalMethodFromExtension()
  !!                                                      ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  !$ note: 'implicitlyInternalMethodFromExtension()' declared here
  !! internal func implicitlyInternalMethodFromExtension() -> Int
  !!               ^
  -> let differentModuleC = publicStructInDifferentModule.filePrivateMethod()
  !$ error: 'filePrivateMethod' is inaccessible due to 'fileprivate' protection level
  !! let differentModuleC = publicStructInDifferentModule.filePrivateMethod()
  !!                                                      ^~~~~~~~~~~~~~~~~
  !$ note: 'filePrivateMethod()' declared here
  !! fileprivate func filePrivateMethod() -> Int
  !!                  ^
  ```
-->

### 확장에서 Private 멤버 (Private Members in Extensions)

확장하는 클래스, 구조체, 열거형과
같은 파일에 있는 확장은
확장 안에 코드가
기존 타입 선언의 부분으로 작성된 것처럼 동작합니다.
결과적으로 다음을 수행할 수 있습니다:

- 기존 선언에서 private 멤버를 선언하고,
  같은 파일의 확장에서 해당 멤버에 접근 가능합니다.
- 한 확장에서 private 멤버를 선언하고,
  같은 파일의 다른 확장에서 해당 멤버에 접근 가능합니다.
- 확장에서 private 멤버를 선언하고,
  같은 파일의 기존 선언에서 해당 멤버에 접근 가능합니다.

이 동작은 타입에 private 엔티티가 있는지 여부에 관계없이
확장을 같은 방법으로 사용하여
코드를 구성할 수 있다는 의미입니다.
예를 들어 다음과 같은 간단한 프로토콜이 있습니다:

```swift
protocol SomeProtocol {
    func doSomething()
}
```

<!--
  - test: `extensions_privatemembers`

  ```swifttest
  -> protocol SomeProtocol {
         func doSomething()
     }
  ```
-->

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

<!--
  - test: `extensions_privatemembers`

  ```swifttest
  -> struct SomeStruct {
         private var privateVariable = 12
     }

  -> extension SomeStruct: SomeProtocol {
         func doSomething() {
             print(privateVariable)
         }
     }
  >> let s = SomeStruct()
  >> s.doSomething()
  << 12
  ```
-->

## 제네릭 (Generics)

제네릭 타입이나 제네릭 함수의 접근 수준은
제네릭 타입이나 제네릭 함수 자체의 접근 수준과
제네릭 타입 매개변수의 타입 제약의 접근 수준 중 더 낮은 접근 수준으로 결정됩니다.

## 타입 별칭 (Type Aliases)

정의한 모든 타입 별칭은 접근 제어 측면에서 고유한 타입으로 처리됩니다.
타입 별칭은 원래 타입의 접근 수준 이하의 접근 수준을 가질 수 있습니다.
예를 들어 private 타입 별칭은 private, fileprivate, internal, public, open 타입의 별칭이 가능하지만,
public 타입 별칭은 internal, fileprivate, private 타입의 별칭이 불가능합니다.

> Note: 이 규칙은 프로토콜 준수를 충족하는데 사용되는 관련된 타입의 타입 별칭에도 적용됩니다.

<!--
  - test: `typeAliases`

  ```swifttest
  -> public struct PublicStruct {}
  -> internal struct InternalStruct {}
  -> private struct PrivateStruct {}

  -> public typealias PublicAliasOfPublicType = PublicStruct
  -> internal typealias InternalAliasOfPublicType = PublicStruct
  -> private typealias PrivateAliasOfPublicType = PublicStruct

  -> public typealias PublicAliasOfInternalType = InternalStruct     // not allowed
  -> internal typealias InternalAliasOfInternalType = InternalStruct
  -> private typealias PrivateAliasOfInternalType = InternalStruct

  -> public typealias PublicAliasOfPrivateType = PrivateStruct       // not allowed
  -> internal typealias InternalAliasOfPrivateType = PrivateStruct   // not allowed
  -> private typealias PrivateAliasOfPrivateType = PrivateStruct

  !$ error: type alias cannot be declared public because its underlying type uses an internal type
  !! public typealias PublicAliasOfInternalType = InternalStruct     // not allowed
  !! ^                           ~~~~~~~~~~~~~~
  !$ note: type declared here
  !! internal struct InternalStruct {}
  !! ^
  !$ error: type alias cannot be declared public because its underlying type uses a private type
  !! public typealias PublicAliasOfPrivateType = PrivateStruct       // not allowed
  !! ^                          ~~~~~~~~~~~~~
  !$ note: type declared here
  !! private struct PrivateStruct {}
  !! ^
  !$ error: type alias cannot be declared internal because its underlying type uses a private type
  !! internal typealias InternalAliasOfPrivateType = PrivateStruct   // not allowed
  !! ^                            ~~~~~~~~~~~~~
  !$ note: type declared here
  !! private struct PrivateStruct {}
  !! ^
  ```
-->

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->