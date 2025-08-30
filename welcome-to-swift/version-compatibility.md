# 버전 호환성 (Version Compatibility)

이전 언어 모드에서 사용가능한 기능에 대해 배워봅니다.

이 책은 Xcode 26에 포함된 Swift의 기본 버전인,
Swift 6.2에 대해 설명합니다.
Swift 6.2, Swift 5, Swift 4.2, Swift 4로 작성된
코드를 빌드하기위해 Swift 6.2 컴파일러를 사용할 수 있습니다.

Swift 5 언어 모드를 사용하는 코드를 빌드하기위해
Swift 6.2 컴파일러를 사용하면,
Swift 6.2의 새로운 기능을 사용할 수 있습니다 ---
새로운 기능은 기본적으로 활성화 되거나 플래그로 인해 활성화 됩니다.
그러나, 엄격한 비동기 검사를 활성화 하려면,
Swift 6.2 언어 모드로 업그레이드가 필요합니다.

또한,
Swift 4와 Swift 4.2 코드를 빌드하기위해 Xcode 15.3을 사용하면,
대부분의 Swift 5 기능을 사용할 수 있습니다.
다시 말해,
다음의 변경사항은 Swift 5 언어 모드를 사용하는
코드에서만 사용가능 합니다:

- 불투명 타입(opaque type)을 반환하는 함수는 Swift 5.1 런타임이 필요합니다.
- `try?` 표현식은 이미 옵셔널(optional)을 반환하는 표현식에
  추가로 옵셔널 표현식을 도입하지 않습니다.
- 큰 정수(integer) 리터럴 초기화 표현식은
  올바른 정수(integer) 타입으로 추론합니다.
  예를 들어 `UInt64(0xffff_ffff_ffff_ffff)`는
  오버플로우가 아닌 올바른 값입니다.

동시성(Concurrency)은 Swift 5 언어 모드와
동시성 타입을 제공하는
Swift 표준 라이브러리의 버전이 필요합니다.
애플 플랫폼에서 배포 대상(deployment target)을 적어도
iOS 13, macOS 10.15, tvOS 13, watchOS 6, visionOS 1로 설정합니다.

Swift 6.2로 작성된 타겟은
Swift 5, Swift 4.2, Swift 4로 작성된 타겟에 따라 달리질 수 있으며
그 반대의 경우도 마찬가지입니다.
즉, 여러 프레임워크로 분할 된
대규모 프로젝트가 있는 경우
코드를 새로운 언어 버전으로
한번에 하나씩 프레임워크로 마이그레이션 할 수 있습니다.

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