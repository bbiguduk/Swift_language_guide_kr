# 언어 참조에 대해 \(About the Language Reference\)

<!--
This part of the book describes the formal grammar of the Swift programming language. The grammar described here is intended to help you understand the language in more detail, rather than to allow you to directly implement a parser or compiler.

The Swift language is relatively small, because many common types, functions, and operators that appear virtually everywhere in Swift code are actually defined in the Swift standard library. Although these types, functions, and operators aren’t part of the Swift language itself, they’re used extensively in the discussions and code examples in this part of the book.
-->

책의 이 부분은 Swift 프로그래밍 언어의 일반적인 문법을 설명합니다. 여기서 설명한 문법은 파서 또는 컴파일러를 직접 구현하는 것보다 언어를 더 자세히 이해하는데 도움을 주기 위한 것입니다.

Swift 의 코드의 거의 모든 곳에 나타나는 많은 공통 타입, 함수, 그리고 연산자가 실제로 Swift 표준 라이브러리에 정의되어 있으므로 Swift 언어는 상대적으로 작습니다. 이러한 타입, 함수, 그리고 연산자는 Swift 언어 자체의 부분은 아니지만 책의 부분에서 의견 및 코드 예제에서 광범위하게 사용됩니다.

## 문법을 읽는 방법 \(How to Read the Grammar\)

<!--
The notation used to describe the formal grammar of the Swift programming language follows a few conventions:

An arrow (→) is used to mark grammar productions and can be read as “can consist of.”
Syntactic categories are indicated by italic text and appear on both sides of a grammar production rule.
Literal words and punctuation are indicated by boldface constant width text and appear only on the right-hand side of a grammar production rule.
Alternative grammar productions are separated by vertical bars (|). When alternative productions are too long to read easily, they’re broken into multiple grammar production rules on new lines.
In a few cases, regular font text is used to describe the right-hand side of a grammar production rule.
Optional syntactic categories and literals are marked by a trailing subscript, opt.
As an example, the grammar of a getter-setter block is defined as follows:
-->

Swift 프로그래밍 언어의 일반적인 문법을 설명하는데 사용되는 표기법은 몇가지 규칙을 따릅니다:

* 화살표 \(→\)는 문법 제작을 표시하는데 사용되며 "구성될 수 있음" 으로 읽을 수 있습니다.
* 구문 카테고리는 _기울임꼴_ 텍스트로 표시되고 문법 생성 규칙의 양쪽에 나타납니다.
* 리터럴 단어와 구두점은 굵게 `constant width` 텍스트로 표시되고 문법 제작 규칙의 오른에만 나타납니다.
* 대체 문법 제작은 세로 막대로 \(\|\)로 구분됩니다. 대체 문법이 너무 길어서 쉽게 읽을 수 없을 경우 새 줄에 여러 문법 제작 규칙으로 나뉩니다.
* 경우에 따라 일반 글꼴 텍스트가 문법 생성 규칙의 오른쪽을 설명하는데 사용됩니다.
* 옵셔널 구문 카테고리와 리터럴은 후행 아래첨자인 opt 로 표시됩니다.

예를 들어, getter-setter 블럭의 문법은 다음과 같이 정의됩니다:

> GRAMMAR OF A GETTER-SETTER BLOCK  
> getter-setter-block → `{` [getter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-clause)  [setter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-clause) $$_{opt}$$ `}` \|  `{` [setter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-clause)  [getter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-clause)  `}`

이 정의는 getter-setter 블럭이 중괄호로 묶인 옵셔널 setter 절이 뒤따르는 getter 절 또는 중괄호로 묶인 getter 절이 뒤따르는 setter 절로 구성될 수 있음을 나타냅니다. 위의 문법 생성은 다음 두가지 생성과 동일하며 대안이 명시적으로 설명되어 있습니다:

> GRAMMAR OF A GETTER-SETTER BLOCK  
> getter-setter-block → `{` [getter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-clause)  [setter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-clause) $$_{opt}$$ `}`   
> getter-setter-block → `{` [setter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-clause)  [getter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-clause)  `}`

