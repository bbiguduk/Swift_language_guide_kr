# 언어 참조에 대해 (About the Language Reference)

형식 문법의 표기법을 읽어봅니다.

책의 이 부분은 Swift 프로그래밍 언어의 형식 문법을 설명합니다.
여기서 설명한 문법은 언어를 더 자세히 이해하는데 도움을 주기 위한 것이며,
파서나 컴파일러를 직접 구현하려는 것은 아닙니다.

Swift의 코드의 거의 모든 곳에 나타나는
많은 공통 타입, 함수, 연산자가 실제로 Swift 표준 라이브러리에 정의되어 있으므로
Swift 언어는 상대적으로 작습니다.
이러한 타입, 함수, 연산자는 Swift 언어 자체의 부분은 아니지만
책의 부분에서 의견 및 코드 예시에서 광범위하게 사용됩니다.

## 문법을 읽는 방법 (How to Read the Grammar)

Swift 프로그래밍 언어의 형식 문법을 설명하는데 사용되는 표기법은
몇 가지 규칙을 따릅니다:

- 화살표(→)는 문법 규칙을 표시하는데 사용되며 "구성될 수 있음"으로 읽을 수 있습니다.
- 문법 카테고리는 *기울임꼴* 텍스트로 표시되고
  문법 생성 규칙의 양쪽에 나타납니다.
- 리터럴 단어와 구두점은 굵게 **`boldface constant width`** 텍스트로 표시되고
  문법 제작 규칙의 오른쪽에만 나타납니다.
- 대체 문법 규칙은
  세로 막대로(|)로 구분합니다. 대체 문법이 너무 길어서 쉽게 읽을 수 없을 경우
  새 줄에 여러 문법 제작 규칙으로 나뉩니다.
- 경우에 따라 일반 글꼴 텍스트가
  문법 생성 규칙의 오른쪽을 설명하는데 사용합니다.
- 옵셔널 구문 카테고리와 리터럴은
  후행 물음표인 *?*로 표시합니다.

예를 들어, getter-setter 블록의 문법은 다음과 같이 정의됩니다:

> Grammar of a getter-setter block:
>
> *getter-setter-block* → **`{`** *getter-clause* *setter-clause*_?_ **`}`** | **`{`** *setter-clause* *getter-clause* **`}`**

이 정의는 getter-setter 블록이
중괄호로 감싸인 getter 절 다음에 옵셔널 setter 절이 올 수 있고
*또는* 중괄호로 감싸인 setter 절 다음에 getter 절이 올 수도 있음을 나타냅니다.
위의 문법 생성은 다음 두 가지 생성과 동일하며
대안이 명시적으로 설명되어 있습니다:

> Grammar of a getter-setter block:
>
>
> *getter-setter-block* → **`{`** *getter-clause* *setter-clause*_?_ **`}`** \
> *getter-setter-block* → **`{`** *setter-clause* *getter-clause* **`}`**

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->