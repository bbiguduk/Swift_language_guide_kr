{% hint style="info" %}
📢 **공지**  
이 문서는 더 이상 업데이트되지 않습니다.  
아래 새로운 URL로 업데이트 되었으니, 새로운 URL로 접속해 주세요.  
[Swift Book Korean](https://bbiguduk.github.io/swift-book-korean/documentation/tsplk/)
{% endhint %}

# Document Revision History

## Revision History

### 2025-09-19(금)

- Swift 6.2 기반으로 문서 업데이트.
- Swift가 방지해주는 이슈에 대한 내용을
  [메모리 안전성 (Memory Safety)](../language-guide-1/the-basics.md#메모리-안전성-memory-safety)에 추가.
- `if case` 문법에 대한 내용을
  [패터 (Patterns)](../language-guide-1/control-flow.md#패턴-patterns)에 추가.
- 메인 액터, 격리, 글로벌 액터에 대한 내용을
  [동시성 (Concurrency)](../language-guide-1/concurrency.md)에 추가.
- 명시적으로 준수성을 작성하지 않고
  일반 프로토콜 준수하는 것과
  암시적 준수 억제에 대한 내용을
  [프로토콜 암시적 준수 (Implicit Conformance to a Protocol)](../language-guide-1/protocols.md#프로토콜-암시적-준수-implicit-conformance-to-a-protocol)에 추가.
- 일반 프로토콜의 준수성을 요구하는
  제너릭 제약 조건에 대한 내용을
  [암시적 제약 조건 (Implicit Constraints)](../language-guide-1/generics.md#암시적-제약-조건-implicit-constraints)에 추가.

### 2025-04-02(수)

- Swift 6.1 기반으로 문서 업데이트.
- [불투명한 파라미터 타입 (Opaque Parameter Types)](../language-guide-1/opaque-types.md#불투명한-파라미터-타입-opaque-parameter-types) 섹션을 추가하여
  `some`을 제너릭의 간단한 문법으로 사용하는 방법에 대한 정보를 포함.
- [available](../language-reference/attributes.md#available) 섹션에
  `noasync` 인수에 대한 정보를 추가.

### 2024-09-19(목)

- Swift 6 기반으로 문서 업데이트.
- 오탈자 수정.

### 2024-07-02(화)

- Swift 6 Beta 기반으로 문서 업데이트.
- 엄격한 동시성 검사로 변환을 위한 정보를
  [preconcurrency](../language-reference/attributes.md#preconcurrency) 섹션에 추가.
- 특정 타입의 에러 발생에 대한 내용을
  [에러 타입 지정 (Specifying the Error Type)](../language-guide-1/error-handling.md#에러-타입-지정-specifying-the-error-type) 섹션에 추가.
- [접근 제어 (Access Control)](../language-guide-1/access-control.md) 챕터에
  패키지-수준 접근에 대한 내용 추가.

### 2024-03-08(금)

- Swift 5.10 기반으로 문서 작업
- 오탈자 수정

### 2024-02-25(일)

- Swift 5.10 Beta 기반으로 문서 작업
- 중첩된 프로토콜에 대한 내용을
  [위임 (Delegation)](../language-guide-1/protocols.md#위임-delegation) 섹션에 추가.
- [UIApplicationMain](../language-reference/attributes.md#uiapplicationmain) 과
  [NSApplicationMain](../language-reference/attributes.md#nsapplicationmain) 섹션에
  더이상 사용하지 않는 정보 추가.

### 2023-12-12(화)

- Swift 5.9.2 기반으로 문서 작업
- 작업 (task), 작업 그룹 (task group), 그리고 작업 취소 (task cancellation) 에
  대한 내용을 [동시성 (Concurrency)](../language-guide-1/concurrency.md) 에 추가했습니다.
- 기존 Swift Package 에 매크로 구현에 대한 내용을
  [매크로 (Macros)](../language-guide-1/macros.md) 에 추가했습니다.
- Conformance 매크로를 대신해 Extension 매크로에 대한 내용을
  [attached](../language-reference/attributes.md#attached) 에 업데이트 했습니다.

### 2023-12-03(일)

- Swift 5.9.2 Beta 기반으로 문서 작업
- `borrowing` 과 `consuming` 수식어에 대한 정보를 
  [파라미터 수식어 (Parameter Modifiers)](../language-reference/declarations.md#파라미터-수식어-parameter-modifiers) 섹션에 추가했습니다.
- [상수와 변수 선언 \(Declaring Constants and Variables\)](../language-guide-1/the-basics.md#상수와-변수-선언-declaring-constants-and-variables) 에
  상수를 선언한 후에 값을 설정에 대한 정보를 추가했습니다.
- 백 배포 (back deployment) 에 대한 정보를
  [backDeployed](../language-reference/attributes.md#backdeployed) 섹션에 추가했습니다.

### 2023-09-30(토)

- Swift 5.9 기반으로 문서 작업
- [기본 (The Basics)](../language-guide-1/the-basics.md) 에 옵셔널에 대한 내용을 추가
- [Swift 둘러보기 (A Swift Tour)](../welcome-to-swift/swift-a-swift-tour.md) 에 동시성 예제 추가
- [결과 변환 (Result Transformations)](../language-reference/attributes.md#결과-변환-result-transformations) 섹션에
  `buildPartialBlock(first:)` 와 `buildPartialBlock(accumulated:next:)` 메서드에 대한 내용 추가
- [available](../language-reference/attributes.md#available) 과 [조건부 컴파일 블럭 (Conditional Compilation Block)](../language-reference/statements.md#조건부-컴파일-블럭-conditional-compilation-block) 에서
  플랫폼 목록에 visionOS 추가

### 2023-06-19(월)

* Swift 5.9 Beta 기반으로 문서 작업
* [제어 흐름 \(Control Flow\)](../language-guide-1/control-flow.md) 챕터와 [조건 표현식 (Conditional Expression)](../language-reference/expressions.md#조건-표현식-conditional-expression) 섹션에 `if` 와 `switch` 표현식에 대한 내용을 추가
* 컴파일 때 코드를 생성하는 것에 대한 [매크로 (Macros)](../language-guide-1/macros.md) 챕터 추가
* [불투명한 타입과 박스형 타입 \(Opaque and Boxed Types\)](../language-guide-1/opaque-types.md) 챕터에 박스형 프로토콜 타입 (boxed protocol type) 에 대한 내용을 추가
* `buildPartialBlock(first:)` 와 `buildPartialBlock(accumulated:next:)` 메서드에 대한 내용을 [결과-빌딩 메서드 (Result-Building Methods)](../language-reference/attributes.md#결과-빌딩-메서드-result-building-methods) 섹션에 추가
* Swift-DocC 적용한 [TSPLK (The Swift Programming Language Korean)](https://bbiguduk.github.io/swift-book-korean/documentation/tsplk/) 페이지 오픈
> GitBook 과 Swift-DocC 둘 다 운영할 계획입니다.

### 2023-04-04(화)

* Swift 5.8 기반으로 문서 작업
* 에러 처리 외의 `defer` 를 표시하는 [연기된 동작 (Deferred Actions)](../language-guide-1/control-flow.md#연기된-동작-deferred-actions) 추가
* 오탈자 수정

### 2022-09-13(화)

* Swift 5.7 정식 릴리즈에 따른 오탈자 수정

### 2022-06-29(수)

* 오탈자 수정
* Swift 5.7 기반으로 문서 작업 완료
* 행위자 (actor) 와 작업 (task) 간의 데이터 전송에 대한 내용을 [전송 가능 타입 (Sendable Types)](../language-guide-1/concurrency.md#sendable-types) 섹션에 추가하고 `@Sendable` 과 `@unchecked` 속성에 대한 내용을 [Sendable](../language-reference/attributes.md#sendable) 과 [unchecked](../language-reference/attributes.md#unchecked) 섹션에 추가하였습니다.
* 정규 표현식 생성에 대한 내용을 [정규 표현식 리터럴 (Regular Expression Literals)](../language-reference/lexical-structure.md#regular-expression-literals) 섹션에 추가하였습니다.
* `if`-`let` 형식에 대한 내용을 [옵셔널 바인딩 (Optional Binding)](../language-guide-1/the-basics.md#optional-binding) 섹선에 추가하였습니다.
* `#unavailable` 에 대한 내용을 [사용 가능한 API 확인 (Checking API Availability)](../language-guide-1/control-flow.md#checking-api-availability) 섹션에 추가하였습니다.

### 2022-03-17(목)

* Swift 5.6 정식 릴리즈에 따른 오탈자 수정

### 2022-01-29(토)

* Swift 5.6 기반으로 문서 작업 완료
* 연결된 메서드 호출과 다른 접미사 표현식과 관련된 `#if` 사용에 대한 정보로 [명시적 멤버 표현식 (Explicit Member Expression)](../language-reference/expressions.md#explicit-member-expression) 을 업데이트
* 이미지 파일 업데이트

### 2021-10-08(금)

* [프로퍼티 (Properties)](../language-guide-1/properties.md) 변경 사항 수정
* [동시성 (Concurrency)](../language-guide-1/concurrency.md) 변경 사항 수정

### 2021-08-14(토)

* Swift 5.5 기반으로 문서 작업 완료
* [동시성 (Concurrency)](../language-guide-1/concurrency.md) 추가
* 오탈자 수정

### 2021-04-10(토)

* WELCOME TO SWIFT 번역 완료
* LANGUAGE REFERENCE 번역 완료

### 2021-02-20(토)

* Swift 5.4 기반으로 문서 작업 완료

### 2020-10-23(금)

* Swift 5.3 기반으로 문서 작업 완료
