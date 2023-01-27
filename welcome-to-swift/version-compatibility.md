# 버전 호환성 \(Version Compatibility\)

<!--
This book describes Swift 5.7, the default version of Swift that’s included in Xcode 14. You can use Xcode 14 to build targets that are written in either Swift 5.7, Swift 4.2, or Swift 4.

When you use Xcode 14 to build Swift 4 and Swift 4.2 code, most Swift 5.7 functionality is available. That said, the following changes are available only to code that uses Swift 5.7 or later:

Functions that return an opaque type require the Swift 5.1 runtime.

The try? expression doesn’t introduce an extra level of optionality to expressions that already return optionals.

Large integer literal initialization expressions are inferred to be of the correct integer type. For example, UInt64(0xffff_ffff_ffff_ffff) evaluates to the correct value rather than overflowing.

Concurrency requires Swift 5.7 or later, and a version of the Swift standard library that provides the corresponding concurrency types. On Apple platforms, set a deployment target of at least iOS 15, macOS 12, tvOS 15, or watchOS 8.0.

A target written in Swift 5.7 can depend on a target that’s written in Swift 4.2 or Swift 4, and vice versa. This means, if you have a large project that’s divided into multiple frameworks, you can migrate your code from Swift 4 to Swift 5.7 one framework at a time.
-->

이 문서는 Xcode 14 에 포함된 Swift 의 기본 버전인 Swift 5.7 에 대해 설명합니다. Xcode 14 를 사용하여 Swift 5.7, Swift 4.2 또는 Swift 4 로 작성된 대상을 빌드할 수 있습니다.

Xcode 14 를 사용하여 Swift 4 와 Swift 4.2 코드를 사용하면 Swift 5.7 대부분의 기능을 사용할 수 있습니다. 즉, 다음 변경사항은 Swift 5.7 이후 버전에서만 사용 가능합니다:

* 불투명한 타입 (opaque type) 을 반환하는 함수는 Swift 5.1 런타임이 필요합니다.
* `try?` 표현식은 이미 옵셔널 (optional) 을 반환하는 표현식에 추가로 옵셔널 표현식을 도입하지 않습니다.
* 큰 정수 (integer) 리터럴 초기화 표현식은 올바른 정수 (integer) 타입으로 추론합니다. 예를 들어 `UInt64(0xffff_ffff_ffff_ffff)` 는 오버플로우가 아닌 올바른 값입니다.

동시성 (Concurrency) 은 Swift 5.7 이후 버전과 동시성 타입을 제공하는 Swift 표준 라이브러리의 버전이 필요합니다. 애플 플랫폼에서 배포 대상 (deployment target) 을 적어도 iOS 15, macOS 12, tvOS 15, 또는 watchOS 8.0 로 설정합니다.

Swift 5.7 로 작성된 타겟은 Swift 4.2 또는 Swift 4 로 작성된 타겟에 따라 달리질 수 있으며 그 반대의 경우도 마찬가지입니다. 즉, 여러 프레임워크로 분할 된 대규모 프로젝트가 있는 경우 코드를 Swift 4 에서 Swift 5.7 로 한번에 하나씩 프레임워크로 마이그레이션 할 수 있습니다.

