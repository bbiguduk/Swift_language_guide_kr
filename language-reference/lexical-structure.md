# 어휘 구조 (Lexical Structure)

문법의 최하위 수준의 요소를 사용합니다.

Swift의 *어휘 구조(lexical structure)*는
언어의 유효한 토큰을 형성하는 문자 시퀀스를 설명합니다.
이러한 유효한 토큰은 언어의 최하위 구성 요소를 형성하며
이후 챕터에서 나머지 언어를 설명하는데 사용됩니다.
토큰은 식별자(identifier), 키워드(keyword), 구두점(punctuation), 리터럴(literal), 연산자(operator)로 구성됩니다.

대부분의 경우 토큰은 아래에 지정된 문법의 제약조건 내에서
입력 텍스트에서 가능한 가장 긴 부분 문자열을 고려하여
Swift 소스 파일의 문자에서 생성됩니다.*
이 동작을 *최장 일치(longest match)*
또는 *최대 뭉크(maximal munch)*라고 합니다.

## 공백과 주석 (Whitespace and Comments)

공백에는 두가지 용도가 있습니다: 소스 파일에서 토큰을 분리하고
접두 연산자, 접미 연산자, 중위 연산자
([연산자 (Operators)](#연산자-operators) 참조)를
구분 하는데 사용하지만 그렇지 않으면 무시합니다.
다음 문자는 공백으로 간주합니다:
공백(U+0020),
줄바꿈(U+000A),
캐리지 리턴(U+000D),
수평탭(U+0009),
수직(U+000B),
폼피드(U+000C),
null(U+0000).

<!--
  Whitespace characters are listed roughly from
  most salient/common to least,
  not in order of Unicode scalar value.
-->

주석은 컴파일러에 의해 공백으로 처리됩니다.
한 줄 주석은 `//`로 시작하고
줄바꿈(U+000A)이나 캐리지 리턴(U+000D)까지 계속됩니다.
여러 줄 주석은 `/*`로 시작하고 `*/`으로 끝납니다.
여러 줄 주석을 중첩할 수 있지만,
주석 마커는 균형을 이루어야 합니다.

주석은 [마크업 서식 참조 (Markup Formatting Reference)](https://developer.apple.com/library/content/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html)에 설명한대로
추가 서식 및 마크업을 포함할 수 있습니다.

> Grammar of whitespace:
>
> *whitespace* → *whitespace-item* *whitespace*_?_ \
> *whitespace-item* → *line-break* \
> *whitespace-item* → *inline-space* \
> *whitespace-item* → *comment* \
> *whitespace-item* → *multiline-comment* \
> *whitespace-item* → U+0000, U+000B, or U+000C
>
> *line-break* → U+000A \
> *line-break* → U+000D \
> *line-break* → U+000D followed by U+000A
>
> *inline-spaces* → *inline-space* *inline-spaces*_?_ \
> *inline-space* → U+0009 or U+0020
>
> *comment* → **`//`** *comment-text* *line-break* \
> *multiline-comment* → **`/*`** *multiline-comment-text* **`*/`**
>
> *comment-text* → *comment-text-item* *comment-text*_?_ \
> *comment-text-item* → Any Unicode scalar value except U+000A or U+000D
>
> *multiline-comment-text* → *multiline-comment-text-item* *multiline-comment-text*_?_ \
> *multiline-comment-text-item* → *multiline-comment* \
> *multiline-comment-text-item* → *comment-text-item* \
> *multiline-comment-text-item* → Any Unicode scalar value except  **`/*`** or  **`*/`**

## 식별자 (Identifiers)

*식별자(Identifiers)*는
A부터 Z까지 대문자 또는 소문자,
언더바(`_`),
기본 다국어 평면(Basic Multilingual Plane)에
조합하지 않은 영숫자 유니코드 문자(noncombining alphanumeric Unicode character),
개인 사용 영역(Private Use Area)에 없는
기본 다국어 평면 외부의 문자로 시작합니다.
첫 번째 문자 다음에
숫자와 유니코드 조합 문자(combining Unicode characters)도 허용합니다.

언더바로 시작하는 식별자,
첫 번째 인자 레이블이 언더바로 시작하는 서브스크립트,
첫 번째 인자 레이블이 언더바로 시작하는 이니셜라이저는
선언에 `public` 접근-수준 수정자가 있더라도
`internal` 식별자로 처리합니다.
이 규칙을 통해 프레임워크 작성자는
일부 제한사항에 따라 선언이 공개되어야 하는 경우에도
클라이언트와 상호작용하거나 의존해서는 안되는 API의 부분을 표시할 수 있습니다.
또한
두 개의 언더바로 시작하는 식별자는
Swift 컴파일러와 표준 라이브러리 용으로 사용됩니다.

예약어(reserved word)를 식별자로 사용하려면
앞 뒤에 백틱(\`)을 넣어야 합니다.
예를 들어 `class`는 유효한 식별자가 아니지만
`` `class` ``는 유효합니다.
백틱은 식별자로 간주되지 않습니다:
`` `x` ``와 `x`는 같은 의미입니다.

<!--
The paragraph above produces a link-resolution warning
because of a known issue with ` in code voice.

https://github.com/swiftlang/swift-book/issues/71
https://github.com/swiftlang/swift-markdown/issues/93
-->

명시적으로 매개변수 이름이 없는 클로저 내의
매개변수는 암시적으로 `$0`, `$1`, `$2` 등으로 지정됩니다.
이 이름은 클로저의 범위 내에서 유효한 식별자입니다.

프로퍼티 래퍼(property wrapper)의 프로젝션(projection)을 위해 컴파일러는
달러 기호(`$`)로 시작하는 식별자를 자동 생성합니다.
코드는 이 식별자와 상호작용 할 수 있지만,
이 접두사로 식별자를 선언할 수 없습니다.
더 자세한 내용은 [속성 (Attributes)](./attributes.md) 챕터의
[propertyWrapper](./attributes.md#propertywrapper)를 참고바랍니다.

<!--
  The cross reference above includes both the section and chapter because,
  even though "propertyWrapper" is the title of the section,
  the section name isn't title case so it doesn't necessarily look like a title.
-->

<!--
The formal grammar below for 'identifier'
produces a link-resolution warning
because of a known issue with ` in code voice.

https://github.com/swiftlang/swift-book/issues/71
https://github.com/swiftlang/swift-markdown/issues/93
-->

> Grammar of an identifier:
>
> *identifier* → *identifier-head* *identifier-characters*_?_ \
> *identifier* → **`` ` ``** *identifier-head* *identifier-characters*_?_ **`` ` ``** \
> *identifier* → *implicit-parameter-name* \
> *identifier* → *property-wrapper-projection* \
> *identifier-list* → *identifier* | *identifier* **`,`** *identifier-list*
>
> *identifier-head* → Upper- or lowercase letter A through Z \
> *identifier-head* → **`_`** \
> *identifier-head* → U+00A8, U+00AA, U+00AD, U+00AF, U+00B2–U+00B5, or U+00B7–U+00BA \
> *identifier-head* → U+00BC–U+00BE, U+00C0–U+00D6, U+00D8–U+00F6, or U+00F8–U+00FF \
> *identifier-head* → U+0100–U+02FF, U+0370–U+167F, U+1681–U+180D, or U+180F–U+1DBF \
> *identifier-head* → U+1E00–U+1FFF \
> *identifier-head* → U+200B–U+200D, U+202A–U+202E, U+203F–U+2040, U+2054, or U+2060–U+206F \
> *identifier-head* → U+2070–U+20CF, U+2100–U+218F, U+2460–U+24FF, or U+2776–U+2793 \
> *identifier-head* → U+2C00–U+2DFF or U+2E80–U+2FFF \
> *identifier-head* → U+3004–U+3007, U+3021–U+302F, U+3031–U+303F, or U+3040–U+D7FF \
> *identifier-head* → U+F900–U+FD3D, U+FD40–U+FDCF, U+FDF0–U+FE1F, or U+FE30–U+FE44 \
> *identifier-head* → U+FE47–U+FFFD \
> *identifier-head* → U+10000–U+1FFFD, U+20000–U+2FFFD, U+30000–U+3FFFD, or U+40000–U+4FFFD \
> *identifier-head* → U+50000–U+5FFFD, U+60000–U+6FFFD, U+70000–U+7FFFD, or U+80000–U+8FFFD \
> *identifier-head* → U+90000–U+9FFFD, U+A0000–U+AFFFD, U+B0000–U+BFFFD, or U+C0000–U+CFFFD \
> *identifier-head* → U+D0000–U+DFFFD or U+E0000–U+EFFFD
>
> *identifier-character* → Digit 0 through 9 \
> *identifier-character* → U+0300–U+036F, U+1DC0–U+1DFF, U+20D0–U+20FF, or U+FE20–U+FE2F \
> *identifier-character* → *identifier-head* \
> *identifier-characters* → *identifier-character* *identifier-characters*_?_
>
> *implicit-parameter-name* → **`$`** *decimal-digits* \
> *property-wrapper-projection* → **`$`** *identifier-characters*

## 키워드와 구두점 (Keywords and Punctuation)

다음의 키워드는 예약되어 있으므로
[식별자 (Identifiers)](#식별자-identifiers)에서 설명한대로
백틱을 사용하지 않으면 식별자로 사용할 수 없습니다.
`inout`, `var`, `let` 이외의 키워드는
백틱을 사용하지 않고도
함수 선언이나 함수 호출에서
매개변수 이름으로 사용할 수 있습니다.
멤버가 키워드와 이름이 같은 경우
해당 멤버에 대한 참조는
멤버 참조와 키워드 사용 사이에
모호성이 있는 경우를 제외하고는 백틱을 사용할 필요가 없습니다 —--
예를 들어 `self`, `Type`, `Protocol`은
명시적 멤버 표현식에서 특별한 의미가 있으므로
해당 컨텍스트에서 백틱을 사용해야 합니다.

<!--
  - test: `keywords-without-backticks`

  ```swifttest
  -> func f(x: Int, in y: Int) {
        print(x+y)
     }
  ```
-->

<!--
  - test: `var-requires-backticks`

  ```swifttest
  -> func g(`var` x: Int) {}
  -> func f(var x: Int) {}
  !$ warning: 'var' in this position is interpreted as an argument label
  !! func f(var x: Int) {}
  !!        ^~~
  !!        `var`
  ```
-->

<!--
  - test: `let-requires-backticks`

  ```swifttest
  -> func g(`let` x: Int) {}
  -> func f(let x: Int) {}
  !$ warning: 'let' in this position is interpreted as an argument label
  !! func f(let x: Int) {}
  !!        ^~~
  !!        `let`
  ```
-->

<!--
  - test: `inout-requires-backticks`

  ```swifttest
  -> func g(`inout` x: Int) {}
  -> func f(inout x: Int) {}
  !$ error: 'inout' before a parameter name is not allowed, place it before the parameter type instead
  !! func f(inout x: Int) {}
  !!        ^~~~~
  !!                 inout
  ```
-->

<!--
  NOTE: This list of language keywords and punctuation
  is derived from the file "swift/include/swift/Parse/Tokens.def"
  and from "utils/gyb_syntax_support/Token.py",
  which generates the TokenKinds.def file.

  Last updated at Swift commit 2f1987567f5, for Swift 5.4.
-->

- 선언에 사용되는 키워드:
  `associatedtype`,
  `borrowing`,
  `class`,
  `consuming`,
  `deinit`,
  `enum`,
  `extension`,
  `fileprivate`,
  `func`,
  `import`,
  `init`,
  `inout`,
  `internal`,
  `let`,
  `nonisolated`,
  `open`,
  `operator`,
  `precedencegroup`,
  `private`,
  `protocol`,
  `public`,
  `rethrows`,
  `static`,
  `struct`,
  `subscript`,
  `typealias`,
  `var`.

<!--
  Token.py doesn't include 'open' but DeclNodes.py does.
-->

- 구문에 사용되는 키워드:
  `break`,
  `case`,
  `catch`,
  `continue`,
  `default`,
  `defer`,
  `do`,
  `else`,
  `fallthrough`,
  `for`,
  `guard`,
  `if`,
  `in`,
  `repeat`,
  `return`,
  `switch`,
  `throw`,
  `where`,
  `while`.
- 표현식과 타입에 사용되는 키워드:
  `Any`,
  `as`,
  `await`,
  `catch`,
  `false`,
  `is`,
  `nil`,
  `rethrows`,
  `self`,
  `Self`,
  `super`,
  `throw`,
  `throws`,
  `true`,
  `try`.
- 패턴에 사용되는 키워드:
  `_`.
- 숫자 기호(`#`)로 시작하는 키워드:
  `#available`,
  `#colorLiteral`,
  `#else`,
  `#elseif`,
  `#endif`,
  `#fileLiteral`,
  `#if`,
  `#imageLiteral`,
  `#keyPath`,
  `#selector`,
  `#sourceLocation`,
  `#unavailable`.

> Note:
> Swift 5.9 이전에는
> 다음의 키워드도 예약어입니다:
> `#column`,
> `#dsohandle`,
> `#error`,
> `#fileID`,
> `#filePath`,
> `#file`,
> `#function`,
> `#line`,
> `#warning`.
> 이것은 현재 Swift 표준 라이브러리에 매크로로 구현되어 있습니다:
> [`column`](https://developer.apple.com/documentation/swift/column()),
> [`dsohandle`](https://developer.apple.com/documentation/swift/dsohandle()),
> [`error(_:)`](https://developer.apple.com/documentation/swift/error(_:)),
> [`fileID`](https://developer.apple.com/documentation/swift/fileID()),
> [`filePath`](https://developer.apple.com/documentation/swift/filePath()),
> [`file`](https://developer.apple.com/documentation/swift/file()),
> [`function`](https://developer.apple.com/documentation/swift/function()),
> [`line`](https://developer.apple.com/documentation/swift/line()),
> [`warning(_:)`](https://developer.apple.com/documentation/swift/warning(_:)).

<!--
  Token.py includes #assert,
  which looks like it's part of an experimental feature
  based on the pound_assert_disabled diagnostic's error message:
  #assert is an experimental feature that is currently disabled
-->

<!--
  Token.py includes #fileID,
  which looks like it's part of a future feature related to
  -enable-experimental-concise-pound-file (see also Swift commit 0e569f5d9e66)
-->

<!--
  Token.py includes 'yield' as a keyword,
  which looks like it's related to a future feature around memory ownership.
-->

- 특정 컨텍스트에서 예약된 키워드:
  `associativity`,
  `async`,
  `convenience`,
  `didSet`,
  `dynamic`,
  `final`,
  `get`,
  `indirect`,
  `infix`,
  `lazy`,
  `left`,
  `mutating`,
  `none`,
  `nonmutating`,
  `optional`,
  `override`,
  `package`,
  `postfix`,
  `precedence`,
  `prefix`,
  `Protocol`,
  `required`,
  `right`,
  `set`,
  `some`,
  `Type`,
  `unowned`,
  `weak`,
  `willSet`.
컨텍스트 외부에서는
식별자로 사용할 수 있습니다.

<!--
  NOTE: The list of context-sensitive keywords above
  is derived from the file "swift/include/swift/AST/Attr.def"
  where they're marked CONTEXTUAL_SIMPLE_DECL_ATTR.
  However, not all context-sensitive keywords appear there;
-->

다음 토큰은 구두점으로 예약되어 있으므로
커스텀 연산자로 사용할 수 없습니다:
`(`, `)`, `{`, `}`, `[`, `]`,
`.`, `,`, `:`, `;`, `=`, `@`, `#`,
`&`(접두 연산자), `->`, `` ` ``,
`?`, `!`(접미 연산자).

## 리터럴 (Literals)

*리터럴(literal)*은 숫자나 문자열과 같은
타입 값의 소스 코드 표현입니다.

다음은 리터럴의 예시입니다:

```swift
42               // Integer literal
3.14159          // Floating-point literal
"Hello, world!"  // String literal
/Hello, .*/      // Regular expression literal
true             // Boolean literal
```

<!--
  - test: `basic-literals`

  ```swifttest
  >> let r0 =
  -> 42               // Integer literal
  >> let r1 =
  -> 3.14159          // Floating-point literal
  >> let r2 =
  -> "Hello, world!"  // String literal
  >> let r4 =
  -> /Hello, .*/      // Regular expression literal
  >> let r3 =
  -> true             // Boolean literal
  >> for x in [r0, r1, r2, r3] as [Any] { print(type(of: x)) }
  << Int
  << Double
  << String
  << Bool
  ```
-->

<!--
  Refactor the above if possible to avoid using bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

리터럴 자체에는 타입이 없습니다.
대신에 리터럴은 무한 정밀도를 가진 구문으로 분석되고
Swift의 타입 추론은 리터럴에 대한 타입을 추론합니다.
예를 들어 `let x: Int8 = 42` 선언에서,
Swift는 명시적 타입 설명(`: Int8`)을 사용하여
정수 리터럴 `42`가 `Int8`의 타입이라고 추론합니다.
타입 정보를 명시하지 않은 경우,
Swift는 Swift 표준 라이브러리에 정의되고
아래 표에서 나열된
기본 리터럴 타입을 사용합니다.
타입 주석을 사용할 경우,
해당 타입은 리터럴 값에서 초기화할 수 있어야 합니다.
즉, 타입은 아래 표에 나열된
Swift 표준 라이브러리 프로토콜을 준수해야 합니다.

| 리터럴 | 기본타입 | 프로토콜 |
| ------- | ------------ | -------- |
| 정수 |`Int` | `ExpressibleByIntegerLiteral` |
| 부동소수점 | `Double` | `ExpressibleByFloatLiteral` |
| 문자열 | `String` | 단일 유니코드 스칼라만 포함하는 문자열 리터럴의 경우에는 `ExpressibleByStringLiteral`, `ExpressibleByUnicodeScalarLiteral` 단일 확장 그래프 클러스터(single extended grapheme cluster)만 포함하는 문자열 리터럴의 경우에는 `ExpressibleByExtendedGraphemeClusterLiteral` |
| 정규 표현식 | `Regex` | None |
| Boolean | `Bool` | `ExpressibleByBooleanLiteral` |

예를 들어 `let str = "Hello, world"` 선언에서
문자열 리터럴 `"Hello, world"`의 추론된 타입은
기본적으로 `String`입니다.
또한 `Int8`은 `ExpressibleByIntegerLiteral` 프로토콜을 준수하므로
`let x: Int8 = 42` 선언에서
정수 리터럴 `42`에 대해 해당 타입을 사용할 수 있습니다.

<!--
  The list of ExpressibleBy... protocols above also appears in Declarations_EnumerationsWithRawCaseValues.
  ExpressibleByNilLiteral is left out of the list because conformance to it isn't recommended.
  There is no protocol for regex literal in the list because the stdlib intentionally omits that.
-->

> Grammar of a literal:
>
> *literal* → *numeric-literal* | *string-literal* | *regular-expression-literal* | *boolean-literal* | *nil-literal*
>
> *numeric-literal* → **`-`**_?_ *integer-literal* | **`-`**_?_ *floating-point-literal* \
> *boolean-literal* → **`true`** | **`false`** \
> *nil-literal* → **`nil`**

### 정수 리터럴 (Integer Literals)

*정수 리터럴(Integer literals)*은 정밀도가 지정되지 않은 정수값을 나타냅니다.
기본적으로 정수 리터럴은 10진수로 표현됩니다;
접두사를 사용하여 다른 진법을 지정할 수 있습니다.
2진수 리터럴은 `0b`로 시작하고,
8진수 리터럴은 `0o`,
16진수 리터럴은 `0x`로 시작합니다.

10진수 리터럴은 `0`부터 `9`까지의 숫자를 포함합니다.
2진수 리터럴은 `0`과 `1`을 포함하고,
8진수 리터럴은 `0`부터 `7`을 포함하고,
16진수 리터럴은 `0`부터 `9`
뿐만 아니라 `A`부터 `F`까지의 대문자나 소문자를 포함합니다.

음의 정수 리터럴은 `-42`와 같이
정수 리터럴 앞에 마이너스 기호(`-`)로 표현합니다.

가독성을 위해 숫자 사이에 언더바(`_`)를 허용하지만
무시되므로 리터럴 값에 영향을 주지 않습니다.
정수 리터럴은 `0`으로 시작할 수 있지만
무시되므로 리터럴 값에 영향을 주지 않습니다.

달리 지정하지 않는 한
정수 리터럴의 기본 유추 타입은 Swift 표준 라이브러리 타입 `Int`입니다.
Swift 표준 라이브러리는
[정수 (Integers)](../language-guide-1/the-basics.md#정수-integers)에서 설명했듯이
다양한 크기의 부호 있는 정수와 부호 없는 정수의 타입을 정의합니다.

<!--
  TR: The prose assumes underscores only belong between digits.
  Is there a reason to allow them at the end of a literal?
  Java and Ruby both require underscores to be between digits.
  Also, are adjacent underscores meant to be allowed, like 5__000?
  (REPL supports them as of swift-1.21 but it seems odd.)
-->

<!--
  NOTE: Updated the syntax-grammar to reflect [Contributor 7746]'s comment in
  <rdar://problem/15181997> Teach the compiler about a concept of negative integer literals.
  This feels very strange from a grammatical point of view.
  Update: This is a parser hack, not a lexer hack. Therefore,
  it's not part of the grammar for integer literal, contrary to [Contributor 2562]'s claim.
  (Doug confirmed this, 4/2/2014.)
-->

> Grammar of an integer literal:
>
> *integer-literal* → *binary-literal* \
> *integer-literal* → *octal-literal* \
> *integer-literal* → *decimal-literal* \
> *integer-literal* → *hexadecimal-literal*
>
> *binary-literal* → **`0b`** *binary-digit* *binary-literal-characters*_?_ \
> *binary-digit* → Digit 0 or 1 \
> *binary-literal-character* → *binary-digit* | **`_`** \
> *binary-literal-characters* → *binary-literal-character* *binary-literal-characters*_?_
>
> *octal-literal* → **`0o`** *octal-digit* *octal-literal-characters*_?_ \
> *octal-digit* → Digit 0 through 7 \
> *octal-literal-character* → *octal-digit* | **`_`** \
> *octal-literal-characters* → *octal-literal-character* *octal-literal-characters*_?_
>
> *decimal-literal* → *decimal-digit* *decimal-literal-characters*_?_ \
> *decimal-digit* → Digit 0 through 9 \
> *decimal-digits* → *decimal-digit* *decimal-digits*_?_ \
> *decimal-literal-character* → *decimal-digit* | **`_`** \
> *decimal-literal-characters* → *decimal-literal-character* *decimal-literal-characters*_?_
>
> *hexadecimal-literal* → **`0x`** *hexadecimal-digit* *hexadecimal-literal-characters*_?_ \
> *hexadecimal-digit* → Digit 0 through 9, a through f, or A through F \
> *hexadecimal-literal-character* → *hexadecimal-digit* | **`_`** \
> *hexadecimal-literal-characters* → *hexadecimal-literal-character* *hexadecimal-literal-characters*_?_

### 부동소수점 리터럴 (Floating-Point Literals)

*부동소수점 리터럴(Floating-point literals)*은 정밀도가 지정되지 않은 부동소수점 값을 나타냅니다.

기본적으로 부동소수점 리터럴은 접두사 없이 10진수로 표현되지만
`0x` 접두사를 붙여 16진수로 표현할 수도 있습니다.

10진수 부동소수점 리터럴은 가수(decimal fraction), 지수(decimal exponent) 중 하나만 있거나
둘 모두의 순서로 구성합니다.
가수는 소수점(`.`)과
소수 자릿수로 구성합니다.
지수는 대문자나 소문자 `e` 접두사 다음에
`e` 앞의 값에 10의 거듭 제곱을 곱한 것을 나타내는
10진수로 구성합니다.
예를 들어 `1.25e2`는 1.25 x 10²을 나타내고,
`125.0`입니다.
유사하게 `1.25e-2`는 1.25 x 10⁻²를 나타내고,
`0.0125`입니다.

16진수 부동소수점 리터럴은 `0x` 접두사 다음으로
옵셔널 가수(hexadecimal fraction) 뒤에
지수(hexadecimal exponent)로 구성합니다.
가수는 소수점과
16진수로 구성합니다.
지수는 대문자나 소문자 `p` 접두사 다음에
`p` 앞의 값에 2의 거듭 제곱을 곱한 것을 나타내는
10진수로 구성합니다.
예를 들어 `0xFp2`는 15 x 2²를 나타내고
`60`입니다.
유사하게 `0xFp-2`는 15 x 2⁻²를 나타내고
`3.75`입니다.

음수 부동소수점 리터럴은 `-42.5`와 같이
부동소수점 리터럴 앞에 마이너스 기호(`-`)를 추가하여 표현합니다.

가독성을 위해 숫자 사이에 언더바(`_`)를 허용하지만
무시되므로 리터럴 값에 영향을 주지 않습니다.
부동소수점 리터럴은 `0`으로 시작할 수 있지만
마찬가지로 무시되므로 리터럴의 기준이나 값에 영향을 주지 않습니다.

달리 지정하지 않는 한
부동소수점 리터럴의 기본 추론 타입은 Swift 표준 라이브러리 타입 `Double`이며,
이것은 64-비트 부동소수점 숫자를 나타냅니다.
Swift 표준 라이브러리는 `Float` 타입도 정의하며,
이것은 32-비트 부동소수점 숫자를 나타냅니다.

> Grammar of a floating-point literal:
>
> *floating-point-literal* → *decimal-literal* *decimal-fraction*_?_ *decimal-exponent*_?_ \
> *floating-point-literal* → *hexadecimal-literal* *hexadecimal-fraction*_?_ *hexadecimal-exponent*
>
> *decimal-fraction* → **`.`** *decimal-literal* \
> *decimal-exponent* → *floating-point-e* *sign*_?_ *decimal-literal*
>
> *hexadecimal-fraction* → **`.`** *hexadecimal-digit* *hexadecimal-literal-characters*_?_ \
> *hexadecimal-exponent* → *floating-point-p* *sign*_?_ *decimal-literal*
>
> *floating-point-e* → **`e`** | **`E`** \
> *floating-point-p* → **`p`** | **`P`** \
> *sign* → **`+`** | **`-`**

### 문자열 리터럴 (String Literals)

문자열 리터럴은 따옴표로 묶인 일련의 문자입니다.
한 줄 문자열 리터럴은 쌍따옴표로 묶이며
형식은 다음과 같습니다:

```swift
"<#characters#>"
```

문자열 리터럴은
이스케이프 처리되지 않은(unescaped) 쌍따옴표(`"`),
이스케이프 처리되지 않은 역슬래시(`\`),
캐리지 리턴이나 줄바꿈을 포함할 수 없습니다.

여러 줄 문자열 리터럴은 세 개의 쌍따옴표로 묶이며
형식은 다음과 같습니다:

```swift
"""
<#characters#>
"""
```

한 줄 문자열 리터럴과 다르게
여러 줄 문자열 리터럴은
이스케이프 처리되지 않은 쌍따옴표(`"`), 캐리지 리턴, 줄바꿈을 포함할 수 있습니다.
이스케이프 처리되지 않은 쌍따옴표 세 개를 나란히 포함할 수 없습니다.

여러 줄 문자열 리터럴을 시작하는
`"""` 뒤의 줄바꿈은
문자열의 부분이 아닙니다.
리터럴 종료를 나타내는
`"""` 전의 줄바꿈도 문자열의 부분이 아닙니다.
줄바꿈으로 시작하거나
끝나는 여러 줄 문자열 리터럴을 만드려면
첫 번째나 마지막 줄에 빈 줄을 작성해야 합니다.

여러 줄 문자열 리터럴은
공백과 탭의 조합을 사용하여 들여쓰기 할 수 있습니다:
이 들여쓰기는 문자열에 포함되지 않습니다.
리터럴을 종료하는 `"""`이
들여쓰기를 결정합니다:
리터럴에서 공백이 아닌 모든 줄은
닫는 `"""` 앞에 나타나는
동일한 들여쓰기로 시작해야 합니다;
탭과 공백 사이에는 변환이 없습니다.
들여쓰기 후에 추가 공백과 탭을 포함할 수 있습니다;
이러한 공백과 탭은 문자열에 나타납니다.

여러 줄 문자열 리터럴의 줄바꿈은
줄바꿈 문자를 사용하는 것이 일반적입니다.
소스 파일에 캐리지 리턴과 줄바꿈이 혼합되어 있더라도
문자열의 줄바꿈은 모두 동일합니다.

여러 줄 문자열 리터럴에서
줄 끝에 역슬래시(`\`)를 작성하면
문자열에서 줄바꿈이 생략됩니다.
역슬래시와 줄바꿈 사이의 공백도
생략합니다.
이 구문을 사용하여
결과 문자열의 값을 변경하지 않고
소스 코드에 여러 줄 문자열 리터럴을 래핑할 수 있습니다.

다음 이스케이프 시퀀스를 사용하여
한 줄과 여러 줄 형식의 문자열 리터럴에
특수 문자를
포함할 수 있습니다:

- Null 문자(`\0`)
- 역슬래시(`\\`)
- 가로 탭(`\t`)
- 줄바꿈(`\n`)
- 캐리지 리턴(`\r`)
- 쌍따옴표(`\"`)
- 홑따옴표(`\'`)
- 유니코드 스칼라(`\u{`*n*`}`),
  *n*은 8자리 숫자로 구성된
  16진수 입니다.

<!--
  The behavior of \n and \r isn't the same as C.
  We specify exactly what those escapes mean.
  The behavior on C is platform dependent --
  in text mode, \n maps to the platform's line separator
  which could be CR or LF or CRLF.
-->

표현식의 값은 문자열 리터럴에 삽입할 수 있으며,
역슬래시(`\`) 뒤 소괄호 안에 표현식을 위치시켜 작성합니다.
삽입된 표현식은 문자열 리터럴을 포함할 수 있지만,
이스케이프 처리되지 않은 역슬래시,
캐리지 리턴, 줄바꿈을 포함할 수 없습니다.

예를 들어 아래의 문자열 리터럴은 동일한 값을 가집니다:

```swift
"1 2 3"
"1 2 \("3")"
"1 2 \(3)"
"1 2 \(1 + 2)"
let x = 3; "1 2 \(x)"
```

<!--
  - test: `string-literals`

  ```swifttest
  >> let r0 =
  -> "1 2 3"
  >> let r1 =
  -> "1 2 \("3")"
  >> assert(r0 == r1)
  >> let r2 =
  -> "1 2 \(3)"
  >> assert(r0 == r2)
  >> let r3 =
  -> "1 2 \(1 + 2)"
  >> assert(r0 == r3)
  -> let x = 3; "1 2 \(x)"
  >> assert(r0 == "1 2 \(x)")
  !$ warning: string literal is unused
  !! let x = 3; "1 2 \(x)"
  !!            ^~~~~~~~~~
  ```
-->

<!--
  Refactor the above if possible to avoid using bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

확장 구분자(extended delimiters)로 구분된 문자열은
따옴표로 묶인 일련의 문자와 하나 이상의 숫자 기호(`#`)의 집합입니다.
확장 구분자로 구분된 문자열의 형식은 다음과 같습니다:

```swift
#"<#characters#>"#

#"""
<#characters#>
"""#
```

확장 구분자로 구분된 문자열에서
특수문자는
일반 문자로 결과 문자열에 나타납니다.
확장 구분자를 사용하여
문자열 보간 생성,
이스케이프 시퀀스 시작,
문자열 종료와 같은
특수 효과를 가지는 문자로 문자열을 만들 수 있습니다.

다음의 예시는
동일한 문자열 값을 생성하는
문자열 리터럴과 확장 구분자로 구분된 문자열을 보여줍니다:

```swift
let string = #"\(x) \ " \u{2603}"#
let escaped = "\\(x) \\ \" \\u{2603}"
print(string)
// Prints "\(x) \ " \u{2603}"
print(string == escaped)
// Prints "true"
```

<!--
  - test: `extended-string-delimiters`

  ```swifttest
  -> let string = #"\(x) \ " \u{2603}"#
  -> let escaped = "\\(x) \\ \" \\u{2603}"
  -> print(string)
  <- \(x) \ " \u{2603}
  -> print(string == escaped)
  <- true
  ```
-->

확장 구분자로 구분된 문자열에
둘 이상의 숫자 기호를 사용하는 경우
숫자 기호 사이에 공백이 있으면 안됩니다:

<!--
  - test: `extended-string-delimiters`

  ```swifttest
  -> print(###"Line 1\###nLine 2"###) // OK
  << Line 1
  << Line 2
  ```
-->

```swift
print(###"Line 1\###nLine 2"###) // OK
print(# # #"Line 1\# # #nLine 2"# # #) // Error
```

<!--
  - test: `extended-string-delimiters-err`

  ```swifttest
  -> print(###"Line 1\###nLine 2"###) // OK
  -> print(# # #"Line 1\# # #nLine 2"# # #) // Error
  !$ error: expected expression in list of expressions
  !! print(# # #"Line 1\# # #nLine 2"# # #) // Error
  !! ^
  !$ error: invalid escape sequence in literal
  !! print(# # #"Line 1\# # #nLine 2"# # #) // Error
  !! ^
  ```
-->

확장 구분자를 사용하여 생성한 여러 줄 문자열 리터럴은
일반적인 여러 줄 문자열 리터럴과 동일한 들여쓰기 요구사항을 가집니다.

문자열 리터럴의 기본 추론 타입은 `String`입니다.
`String` 타입에 대한 자세한 설명은
[문자열과 문자 (Strings And Characters)](../language-guide-1/strings-and-characters.md)와
[`String`](https://developer.apple.com/documentation/swift/string)에서 확인 가능합니다.

`+` 연산자로 연결된 문자열 리터럴은
컴파일 시 연결됩니다.
예를 들어 아래 예시의
`textA`와 `textB`의 값은 동일하며
런타임 연결이 수행되지 않습니다.

```swift
let textA = "Hello " + "world"
let textB = "Hello world"
```

<!--
  - test: `concatenated-strings`

  ```swifttest
  -> let textA = "Hello " + "world"
  -> let textB = "Hello world"
  ```
-->

> Grammar of a string literal:
>
> *string-literal* → *static-string-literal* | *interpolated-string-literal*
>
> *string-literal-opening-delimiter* → *extended-string-literal-delimiter*_?_ **`"`** \
> *string-literal-closing-delimiter* → **`"`** *extended-string-literal-delimiter*_?_
>
> *static-string-literal* → *string-literal-opening-delimiter* *quoted-text*_?_ *string-literal-closing-delimiter* \
> *static-string-literal* → *multiline-string-literal-opening-delimiter* *multiline-quoted-text*_?_ *multiline-string-literal-closing-delimiter*
>
> *multiline-string-literal-opening-delimiter* → *extended-string-literal-delimiter*_?_ **`"""`** \
> *multiline-string-literal-closing-delimiter* → **`"""`** *extended-string-literal-delimiter*_?_ \
> *extended-string-literal-delimiter* → **`#`** *extended-string-literal-delimiter*_?_
>
> *quoted-text* → *quoted-text-item* *quoted-text*_?_ \
> *quoted-text-item* → *escaped-character* \
> *quoted-text-item* → Any Unicode scalar value except  **`"`**,  **`\`**, U+000A, or U+000D
>
> *multiline-quoted-text* → *multiline-quoted-text-item* *multiline-quoted-text*_?_ \
> *multiline-quoted-text-item* → *escaped-character* \
> *multiline-quoted-text-item* → Any Unicode scalar value except  **`\`** \
> *multiline-quoted-text-item* → *escaped-newline*
>
> *interpolated-string-literal* → *string-literal-opening-delimiter* *interpolated-text*_?_ *string-literal-closing-delimiter* \
> *interpolated-string-literal* → *multiline-string-literal-opening-delimiter* *multiline-interpolated-text*_?_ *multiline-string-literal-closing-delimiter*
>
> *interpolated-text* → *interpolated-text-item* *interpolated-text*_?_ \
> *interpolated-text-item* → **`\(`** *expression* **`)`** | *quoted-text-item*
>
> *multiline-interpolated-text* → *multiline-interpolated-text-item* *multiline-interpolated-text*_?_ \
> *multiline-interpolated-text-item* → **`\(`** *expression* **`)`** | *multiline-quoted-text-item*
>
> *escape-sequence* → **`\`** *extended-string-literal-delimiter* \
> *escaped-character* → *escape-sequence* **`0`** | *escape-sequence* **`\`** | *escape-sequence* **`t`** | *escape-sequence* **`n`** | *escape-sequence* **`r`** | *escape-sequence* **`"`** | *escape-sequence* **`'`** \
> *escaped-character* → *escape-sequence* **`u`** **`{`** *unicode-scalar-digits* **`}`** \
> *unicode-scalar-digits* → Between one and eight hexadecimal digits
>
> *escaped-newline* → *escape-sequence* *inline-spaces*_?_ *line-break*

<!--
  Quoted text resolves to a sequence of escaped characters by way of
  the quoted-text rule which allows repetition; no need to allow
  repetition in the quoted-text/escaped-character rule too.
-->

<!--
  Now that single quotes are gone, we don't have a character literal.
  Because we may one bring them back, here's the old grammar for them:

  textual-literal -> character-literal | string-literal

  character-literal -> ``'`` quoted-character ``'``
  quoted-character -> escaped-character
  quoted-character -> Any Unicode scalar value except ``'``, ``\``, U+000A, or U+000D
-->

### 정규 표현식 리터럴 (Regular Expression Literals)

정규 표현식 리터럴(regular expression literal)은
아래와 같이 슬래시(`/`)로 감싸있는 문자의 시퀀스입니다:

```swift
/<#regular expression#>/
```

정규 표현식 리터럴은
탭이나 스페이스로 시작되지 않아야 하며,
이스케이프 처리되지 않은 슬래시(unescaped slash)(`/`),
캐리지 리턴(carriage return), 줄바꿈을
포함할 수 없습니다.

정규 표현식 리터럴 내부의
역슬래시는 문자열 리터럴과 같이 이스케이프 문자가 아닌
정규 표현식의 부분으로 인식합니다.
다음의 특수 문자는
문자 그대로 해석되거나
다음의 일반 문자는
특별한 방법으로 해석됨을 나타냅니다.
예를 들어
`/\(/`은 단일 왼쪽 괄호이며,
`/\d/`은 단일 숫자입니다.

<!--
  OUTLINE

  Doc comments on Regex struct don't have more syntax details,
  or a cross reference to where you can learn more.
  We probably need at least some baseline coverage
  of the supported syntax here.
  (Unified dialect/superset of POSIX + PCRE 2 + Oniguruma + .NET)

  https://github.com/swiftlang/swift-experimental-string-processing/blob/main/Sources/_StringProcessing/Regex/Core.swift

  Regex literals and the DSL take different approaches to captures.
  The literals give you more type safety.
  The DSL lets you access stuff by name.

  From SE-0354:
  A regex literal may be used with a prefix operator,
  e.g `let r = ^^/x/` is parsed as `let r = ^^(/x/)`.
  In this case,
  when encountering operator characters containing `/` in an expression position,
  the characters up to the first `/` are split into a prefix operator,
  and regex literal parsing continues as normal.
-->

확장 구분자를 사용하는 정규 표현식 리터럴은
슬래시(`/`)로 감싸있는 문자의 시퀀스와
하나 이상의 숫자 기호(`#`) 쌍으로 되어있습니다.
확장 구분자로 구분된 정규 표현식 리터럴은
다음의 형식을 가집니다:

```swift
#/<#regular expression#>/#

#/
<#regular expression#>
/#
```

확장 구분자를 사용하는 정규 표현식 리터럴은
이스케이프 처리되지 않은 스페이스나 탭으로 시작할 수 있고,
이스케이프 처리되지 않은 슬래시(`/`)를 포함할 수 있고,
여러 줄로 작성할 수 있습니다.
여러 줄 정규 표현식 리터럴은
시작 구분자는 라인의 끝에 있어야 하고,
끝나는 구분자는 해당 라인에 있어야 합니다.
여러 줄 정규 표현식 리터럴에서
확장된 정규 표현식은 기본적으로 가능합니다 —--
특히, 공백은 무시되고 주석은 허용됩니다.

<!--
  TODO As details about the multiline syntax shake out during SE review,
  like indentation and whitespace,
  add them above or spin out a separate paragraph.
-->

확장된 구분자에 의해 구분된 정규 표현식 리터럴
형식에 하나 이상의 숫자 기호를 사용한다면
숫자 기호 사이에 공백이 있을 수 없습니다:

```swift
let regex1 = ##/abc/##       // OK
let regex2 = # #/abc/# #     // Error
```

<!--
  - test: `extended-regex-delimiters-err`

  ```swifttest
  -> let regex1 = ##/abc/##       // OK
  -> let regex2 = # #/abc/# #     // Error
  ```
-->

빈 정규 표현식 리터럴을 만드려면
확장된 구분자 구문을 사용해야 합니다.

> Grammar of a regular expression literal:
>
> *regular-expression-literal* → *regular-expression-literal-opening-delimiter* *regular-expression* *regular-expression-literal-closing-delimiter* \
> *regular-expression* → Any regular expression
>
> *regular-expression-literal-opening-delimiter* → *extended-regular-expression-literal-delimiter*_?_ **`/`** \
> *regular-expression-literal-closing-delimiter* → **`/`** *extended-regular-expression-literal-delimiter*_?_
>
> *extended-regular-expression-literal-delimiter* → **`#`** *extended-regular-expression-literal-delimiter*_?_

## 연산자 (Operators)

Swift 표준 라이브러리는 사용할 수 있는 여러 가지 연산자를 정의하며
[기본 연산자 (Basic Operators)](../language-guide-1/basic-operators.md)와
[고급 연산자 (Advanced Operators)](../language-guide-1/advanced-operators.md)에 설명되어 있습니다.
이 섹션에서는 커스텀 연산자(Custom operators)를 정의하는데 사용할 수 있는 문자를 설명합니다.

커스텀 연산자는 ASCII 문자
`/`, `=`, `-`, `+`, `!`, `*`, `%`, `<`, `>`,
`&`, `|`, `^`, `?`, `~` 중 하나로 시작하거나
아래 문법에 정의된 유니코드 문자 중 하나로 시작할 수 있습니다
(*수학 연산자(Mathematical Operators)*, *기타 기호(Miscellaneous Symbols)*,
*딩뱃(Dingbats)* 유니코드
블록의 문자를 포함합니다).
첫 번째 문자 뒤에
유니코드 문자를 결합하는 것도 가능합니다.

커스텀 연산자도 정의할 수 있는데
점(`.`)으로 시작합니다.
이러한 연산자는 여러 개의 점을 포함할 수 있습니다.
예를 들어 `.+.`은 단일 연산자로 취급됩니다.
연산자가 점으로 시작하지 않으면,
다른 곳에 점을 포함할 수 없습니다.
예를 들어 `+.+`은
`+` 연산자와 `.+` 연산자로 처리됩니다.

<!--
  - test: `dot-operator-must-start-with-dot`

  ```swifttest
  >> infix operator +.+ ;
  !$ error: consecutive statements on a line must be separated by ';'
  !! infix operator +.+ ;
  !!                 ^
  !!                 ;
  !$ error: operator with postfix spacing cannot start a subexpression
  !! infix operator +.+ ;
  !!                 ^
  !$ error: expected expression
  !! infix operator +.+ ;
  !!                    ^
  >> infix operator .+
  >> infix operator .+.
  ```
-->

물음표(`?`)를 포함하는 커스텀 연산자를 정의할 수 있지만
단일 물음표 문자로만 이루어진 연산자는 정의할 수 없습니다.
또한 연산자에 느낌표(`!`)를 포함할 수 있지만
접미 연산자(postfix operators)는 물음표나 느낌표로 시작할 수 없습니다.

<!--
  - test: `postfix-operators-dont-need-unique-prefix`

  ```swifttest
  >> struct Num { var value: Int }
     postfix operator +
     postfix operator +*
     postfix func + (x: Num) -> Int { return x.value + 1 }
     postfix func +* (x: Num) -> Int { return x.value * 100 }
  >> let n = Num(value: 5)
  >> print(n+)
  << 6
  >> print(n+*)
  << 500
  ```
-->

<!--
  - test: `postfix-operator-cant-start-with-question-mark`

  ```swifttest
  >> postfix operator ?+
  >> postfix func ?+ (x: Int) -> Int {
         if x > 10 {
             return x
         }
         return x + 1
     }
  >> print(1?+)
  !$ error: postfix operator names starting with '?' or '!' are disallowed to avoid collisions with built-in unwrapping operators
  !! postfix operator ?+
  !!                  ^
  !$ error: '+' is not a postfix unary operator
  !! print(1?+)
  !!         ^
  ```
-->

> Note: 토큰 `=`, `->`, `//`, `/*`, `*/`, `.`,
> 접두 연산자 `<`, `&`, `?`,
> 중위 연산자 `?`,
> 접미 연산자 `>`, `!`, `?`는 예약되어 있습니다.
> 이러한 토큰은 오버로드할 수 없으며, 커스텀 연산자로 사용할 수 없습니다.

연산자 주변의 공백은 연산자가
접두 연산자(prefix operator), 접미 연산자(postfix operator),
이항 연산자(binary operator)로 사용되는지 결정하기 위해 사용됩니다. 이 동작은 다음 규칙에 정리되어 있습니다:

- 연산자 주변 양쪽에 모두 공백이 있거나 없는 경우,
  이항 연산자로 처리됩니다.
  예를 들어 `a+++b`와 `a +++ b`의 연산자 `+++`는 이항 연산자로 처리됩니다.
- 연산자가 왼쪽에만 공백이 있다면,
  접두 단항 연산자(prefix unary operator)로 처리됩니다.
  에를 들어 `a +++b`에서 `+++` 연산자는 접두 단항 연산자로 처리됩니다.
- 연산자가 오른쪽에만 공백이 있다면,
  접미 단항 연산자(postfix unary operator)로 처리됩니다.
  예를 들어 `a+++ b`에서 `+++` 연산자는 접미 단항 연산자로 처리됩니다.
- 연산자 왼쪽에 공백이 없지만 바로 이어서 점(`.`)이 오면,
  접미 단항 연산자로 처리됩니다.
  예를 들어 `a+++.b`에서 `+++` 연산자는 접미 단항 연산자로 처리됩니다
  (`a +++ .b`가 아닌 `a+++ .b`).

이러한 규칙의 목적에 따라
연산자 앞의 문자 `(`, `[`, `{`,
연산자 뒤의 문자 `)`, `]`, `}`,
문자 `,`, `;`, `:`도
공백으로 간주합니다.

미리 정의된 연산자 `!`나 `?`에 왼편에 공백이 없다면,
오른편에 공백 여부와 상관없이
접미 연산자로 처리됩니다.
옵셔널 체이닝 연산자(optional-chaining operator)로 `?`를 사용하려면,
왼편에 공백을 가지면 안됩니다.
삼항 조건 연산자(ternary conditional operator)(`?` `:`)로 사용하려면,
반드시 양쪽에 공백이 있어야 합니다.

중위 연산자(infix operator)의 인자 중 하나가 정규 표현식 리터럴이라면,
연산자는 양쪽에 공백을 가져야 합니다.

특정 구문에서 `<` 또는 `>`로 시작하는 연산자는
둘 이상의 토큰으로 분리될 수 있습니다. 나머지는 동일한 방식으로 처리되고
다시 분리될 수 있습니다.
그 결과로 `Dictionary<String, Array<Int>>`와 같은
구문에서 닫는 `>` 문자 사이를 명확하게 하기 위해
공백을 추가할 필요가 없습니다.
예를 들어 닫는 `>` 문자는 비트 시프트 `>>` 연산자로 잘못 해석될 수 있는
단일 토큰으로 처리되지 않습니다.

<!--
  NOTE: Once the parser sees a < it goes into a pre-scanning lookahead mode.  It
  matches < and > and looks at what token comes after the > -- if it's a . or
  a ( it treats the <...> as a generic parameter list, otherwise it treats
  them as less than and greater than.

  This fails to parse things like x<<2>>(1+2) but it's the same as C#.  So
  don't write that.

  We call out the > > vs >> because
  C++ typically needs whitespace to resolve the ambiguity.
-->

새로운 커스텀 연산자를 정의하는 방법을 알아보려면,
[커스텀 연산자 (Custom Operators)](../language-guide-1/advanced-operators.md#커스텀-연산자-custom-operators)와 [연산자 선언 (Operator Declaration)](./declarations.md#연산자-선언-operator-declaration)을 참고바랍니다.
기존 연산자를 오버로드 하는 방법을 알아보려면
[연산자 메서드 (Operator Methods)](../language-guide-1/advanced-operators.md#연산자-메서드-operator-methods)를 참고바랍니다.

<!--
  NOTE: The ? is a reserved punctuation.  Optional-chaining (foo?.bar) is actually a
  monad -- the ? is actually a monadic bind operator.  It is like a burrito.
  The current list of reserved punctuation is in Tokens.def.
-->

> Grammar of operators:
>
> *operator* → *operator-head* *operator-characters*_?_ \
> *operator* → *dot-operator-head* *dot-operator-characters*
>
> *operator-head* → **`/`** | **`=`** | **`-`** | **`+`** | **`!`** | **`*`** | **`%`** | **`<`** | **`>`** | **`&`** | **`|`** | **`^`** | **`~`** | **`?`** \
> *operator-head* → U+00A1–U+00A7 \
> *operator-head* → U+00A9 or U+00AB \
> *operator-head* → U+00AC or U+00AE \
> *operator-head* → U+00B0–U+00B1 \
> *operator-head* → U+00B6, U+00BB, U+00BF, U+00D7, or U+00F7 \
> *operator-head* → U+2016–U+2017 \
> *operator-head* → U+2020–U+2027 \
> *operator-head* → U+2030–U+203E \
> *operator-head* → U+2041–U+2053 \
> *operator-head* → U+2055–U+205E \
> *operator-head* → U+2190–U+23FF \
> *operator-head* → U+2500–U+2775 \
> *operator-head* → U+2794–U+2BFF \
> *operator-head* → U+2E00–U+2E7F \
> *operator-head* → U+3001–U+3003 \
> *operator-head* → U+3008–U+3020 \
> *operator-head* → U+3030
>
> *operator-character* → *operator-head* \
> *operator-character* → U+0300–U+036F \
> *operator-character* → U+1DC0–U+1DFF \
> *operator-character* → U+20D0–U+20FF \
> *operator-character* → U+FE00–U+FE0F \
> *operator-character* → U+FE20–U+FE2F \
> *operator-character* → U+E0100–U+E01EF \
> *operator-characters* → *operator-character* *operator-characters*_?_
>
> *dot-operator-head* → **`.`** \
> *dot-operator-character* → **`.`** | *operator-character* \
> *dot-operator-characters* → *dot-operator-character* *dot-operator-characters*_?_
>
> *infix-operator* → *operator* \
> *prefix-operator* → *operator* \
> *postfix-operator* → *operator*

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