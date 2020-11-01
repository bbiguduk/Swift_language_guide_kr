# 문자열과 문자 \(Strings and Characters\)

_문자열 \(string\)_ 은 `"hello, world"` 또는 `"albatross"` 와 같은 문자의 연속입니다. Swift 문자열은 `String` 타입으로 표현됩니다. `String` 의 콘텐츠는 `Character` 값의 콜렉션을 포함하여 여러가지 방법으로 접근할 수 있습니다.

Swift의 `String` 과 `Character` 타입은 코드의 텍스트를 처리하는 빠른 유니코드 호환 방법을 제공합니다. 문자열 생성과 취급을 위한 구문은 C와 유사한 문자열 구문과 같이 가볍고 읽기 쉽습니다. 문자열 연결은 두 문자열 사이에 `+` 연산자와 함께 결합하여 간단하게 연결이 가능하고 Swift의 다른 값과 마찬가지로 상수 또는 변수 중에서 선택하여 문자열 변경 가능성을 관리합니다. 또한 문자열 보간이라는 프로세스에서 문자열을 사용하여 상수, 변수, 리터럴 및 표현식을 긴 문자열에 삽입할 수 있습니다. 이것은 화면에 표시, 저장, 출력을 위한 커스텀 문자열 값을 쉽게 생성할 수 있습니다.

간단한 구문임에도 Swift의 `String` 타입은 빠르고, 최신 문자열 구현입니다. 모든 문자열은 인코딩에 독립적인 유니코드 문자로 구성되어 있으며 다양한 유니코드 표현의 문자에 접근할 수 있도록 지원합니다.

> NOTE   
> Swift의 `String` 타입은 Foundation의 `NSString` 클래스와 연결되어 있습니다. Foundation은 또한 `NSString` 에 의해 정의된 메서드를 노출시키기 위해 `String` 을 확장합니다. 이것은 Foundation을 import 하면 캐스팅 없이 `String` 에서 `NSString` 메서드를 접근할 수 있습니다.
>
> Foundation과 Cocoa에서 `String` 사용에 대한 자세한 내용은 [String과 NSString의 연결 \(Bridging Between String and NSString\)](https://developer.apple.com/documentation/swift/string#2919514) 을 참고 바랍니다.

## 문자열 리터럴 \(String Literals\)

코드안에 미리 정의된 `String` 값을 _문자열 리터럴 \(string literals\)_ 로 포함할 수 있습니다. 문자열 리터럴은 쌍따옴표 \(`"`\)로 둘러쌓인 문자의 연속입니다.

상수 또는 변수의 초기값으로 문자열 리터럴을 사용합니다:

```swift
let someString = "Some string literal value"
```

Swift는 문자열 리터럴 값으로 초기화 되었기 때문에 `someString` 상수를 `String` 타입으로 유추합니다.

### 여러줄 문자열 리터럴 \(Multiline String Literals\)

여러줄의 문자열이 필요하면 3개의 쌍따옴표로 둘러쌓인 문자의 연속인 여러줄 문자열 리터럴을 사용하면 됩니다.

```swift
let quotation = """
The White Rabbit put on his spectacles.  "Where shall I begin,
please your Majesty?" he asked.

"Begin at the beginning," the King said gravely, "and go on
till you come to the end; then stop."
"""
```

여러줄 문자열 리터럴은 열리고 닫긴 따옴표 사이에 있는 모든 라인을 포함합니다. 문자열은 여는 따옴표 \(`"""`\) 다음줄부터 시작하고 닫는 따옴표 전줄로 끝납니다. 이것은 아래의 문자열은 줄바꿈으로 시작하고 끝나지 않는것을 뜻합니다:

```swift
let singleLineString = "These are the same."
let multilineString = """
These are the same.
"""
```

소스코드에서 여러줄 문자열 리터럴에 줄바꿈을 포함하면 줄바꿈도 문자열의 값으로 나타납니다. 소스코드를 쉽게 읽을 수 있게 줄바꿈을 사용하고 싶지만 줄바꿈이 문자열의 값의 일부가 되는걸 원치 않을 때는 줄 바꿈을 원하는 라인 끝에 백슬래시\(`\`\)를 쓰면 됩니다:

```swift
let softWrappedQuotation = """
The White Rabbit put on his spectacles.  "Where shall I begin, \
please your Majesty?" he asked.

"Begin at the beginning," the King said gravely, "and go on \
till you come to the end; then stop."
"""
```

여러줄 문자열 리터럴에 시작 또는 끝에 빈줄을 추가하고 싶다면 처음 또는 마지막에 빈줄을 추가하면 됩니다. 예를 들어:

```swift
let lineBreaks = """

This string starts with a line break.
It also ends with a line break.

"""
```

여러줄 문자열은 주변 코드와 일치하도록 들여 쓸 수 있습니다. 닫는 따옴표 \(`"""`\) 앞의 공백은 Swift가 다른 모든 줄의 공백은 무시한다는 것을 말합니다. 그러나 닫는 따옴표 전에 추가로 공백이 들어가면 그 공백은 추가됩니다.

![&#xC5EC;&#xB7EC;&#xC904; &#xBB38;&#xC790;&#xC5F4; &#xACF5;&#xBC31; \(Multiline String Whitespace\)](../.gitbook/assets/03_multilinestringwhitespace_2x.png)



위의 예제에서 여러줄 문자열 리터럴 전체가 들여쓰기 되어있지만 문자열에서 첫번째와 마지막 줄은 어떠한 공백없이 시작합니다. 중간줄은 닫는 따옴표 보다 들여쓰기가 더 많으므로 4칸 들여쓰기로 시작합니다.

### 문자열 리터럴에 특수 문자 \(Special Characters in String Literals\)

문자열 리터럴은 아래와 같은 특수 문자를 포함할 수 있습니다:

* 이스케이프 된 문자 `\0` \(null 문자\), `\\` \(백슬래시\), `\t` \(수평 탭\), `\n` \(개행\), `\r` \(캐리지 리턴\), `\"` \(쌍따옴표\) 와 `\'` \(홑따옴표\)
* `\u{n}` 로 쓰여진 임의의 유니코드 스칼라 값. 여기서 _n_ 은 1-8 자리의 16진수 \(유니코드는 아래 [유니코드 \(Unicode\)](strings-and-characters.md#unicode) 에서 설명합니다.\)

아래 코드는 특수 문자의 4개의 예를 보여줍니다. `wiseWords` 상수는 2개의 이스케이프 된 쌍따옴표를 포함합니다. `dollarSign`, `blackHeart` 그리고 `sparklingHeart` 는 유니코드 스칼라 형태를 보여줍니다:

```swift
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein"
// "Imagination is more important than knowledge" - Einstein
let dollarSign = "\u{24}"        // $,  Unicode scalar U+0024
let blackHeart = "\u{2665}"      // ♥,  Unicode scalar U+2665
let sparklingHeart = "\u{1F496}" // 💖, Unicode scalar U+1F496
```

여러줄 문자열 리터럴은 쌍따옴표 3개를 사용하기 때문에 이스케이프 없이 여러줄 문자열 리터럴안에 쌍따옴표 \(`"`\)를 포함할 수 있습니다. 여러줄 문자열 텍스트에 `"""` 를 포함하려면 따옴표에 적어도 하나의 이스케이프를 포함해야 합니다. 예를 들어:

```swift
let threeDoubleQuotationMarks = """
Escaping the first quotation mark \"""
Escaping all three quotation marks \"\"\"
"""
```

### 확장된 문자열 구분기호 \(Extended String Delimiters\)

아무런 영향없이 문자열에 특수 문자를 포함하기 위해 확장된 구분기호 안에 문자열 리터럴을 위치할 수 있습니다. 문자열을 따옴표 \(`"`\)로 둘러싸고 숫자 기호 \(`#`\)로 둘러쌉니다. 예를 들어 문자열 리터럴 `#"Line\nLine2"#` 을 출력하면 2줄로 출력되지 않고 개행 문자 \(`\n`\)가 출력됩니다.

문자열 리터럴에서 문자의 특수 영향이 필요하다면 이스케이프 문자 \(`\`\) 다음에 선언된 숫자 기호의 숫자만큼 숫자 기호를 포함하면 됩니다. 예를 들어 문자열 `#"Line1\nLine2"#` 에 개행을 수행하고 싶다면 `#"Line1\#nLine2"#` 로 작성하면 됩니다. 비슷하게 `###"Line1\###nLine2"###` 도 개행이 이루어집니다.

확장된 구분기호를 사용하여 생성된 문자열 리터럴은 여러줄 문자열 리터럴로 사용할 수도 있습니다. 여러줄 문자열에 텍스트 `"""` 를 포함하기 위해 리터럴을 종료하는 기본 동작을 재정의 하여 확장된 구분기호를 사용할 수 있습니다. 예를 들어:

```swift
let threeMoreDoubleQuotationMarks = #"""
Here are three more double quotes: """
"""#
```

## 빈 문자열 초기화 \(Initializing an Empty String\)

긴 문자열을 만들기 위한 시작점으로 빈 `String` 값을 만들려면 빈 문자열 리터럴을 변수에 할당하거나 초기화 구문으로 새 `String` 인스턴스를 초기화합니다:

```swift
var emptyString = ""               // empty string literal
var anotherEmptyString = String()  // initializer syntax
// these two strings are both empty, and are equivalent to each other
```

`String` 값은 부울 `isEmpty` 프로퍼티로 비어있는지 체크할 수 있습니다:

```swift
if emptyString.isEmpty {
    print("Nothing to see here")
}
// Prints "Nothing to see here"
```

## 문자열 변경 \(String Mutability\)

특정 `String` 은 변수 \(이 경우 수정 가능\) 또는 상수 \(이 경우 수정 불가능\)에 할당되어 변경 \(또는 변형\)할 수 있는지 여부를 알 수 있습니다:

```swift
var variableString = "Horse"
variableString += " and carriage"
// variableString is now "Horse and carriage"

let constantString = "Highlander"
constantString += " and another Highlander"
// this reports a compile-time error - a constant string cannot be modified
```

> NOTE   
> 이 접근방식은 Objective-C와 Cocoa의 문자열 변형과 다릅니다. 여기서 두 클래스 \(`NSString` 및 `NSMutableString`\) 중에서 선택하여 문자열 변형이 가능한지 여부를 나타냅니다.

## 문자열은 값 타입 \(Strings Are Value Types\)

Swift의 `String` 타입은 _값 타입 \(value type\)_ 입니다. 새로운 `String` 값을 생성한다면 `String` 값은 함수 또는 메서드에 전달될 때나 상수 또는 변수에 대입될 때 _복사_ 됩니다. 각 경우에 존재하는 `String` 값의 새로운 복사본이 생성되고 원본이 아닌 새로운 복사본은 전달되거나 할당됩니다. 값 타입은 [구조체와 열거형은 값 타입 \(Structures and Enumerations Are Value Types\)](structures-and-classes.md#structures-and-enumerations-are-value-types) 에서 설명합니다.

Swift의 기본 `String` 으로의 복사 동작은 함수 또는 메서드가 `String` 값을 전달할 때 출처에 관계없이 `String` 값은 정확하다고 보장합니다. 전달된 문자열은 직접 수정하지 않는한 수정되지 않습니다.

Swift의 컴파일러는 꼭 필요할 때 실제 복사가 이뤄지도록 문자열 사용을 최적화합니다. 이것은 문자열을 값 유형으로 사용할 때는 항상 뛰어난 성능을 얻을 수 있다는 의미입니다.

## 문자 작업 \(Working with Characters\)

문자열과 `for`-`in` 루프로 `String` 의 각각의 `Character` 값에 접근할 수 있습니다:

```swift
for character in "Dog!🐶" {
    print(character)
}
// D
// o
// g
// !
// 🐶
```

`for`-`in` 루프는 [For-In 루프 \(For-In Loops\)](control-flow.md#for-in-for-in-loops) 에서 설명합니다.

또는 하나의 문자 문자열 리터럴을 `Character` 타입을 명시하여 단독의 `Character` 상수 또는 변수를 생성할 수 있습니다:

```swift
let exclamationMark: Character = "!"
```

`String` 값은 초기화 인자로 `Character` 값의 배열을 전달해 생성할 수 있습니다:

```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
let catString = String(catCharacters)
print(catString)
// Prints "Cat!🐱"
```

## 문자열과 문자 연결 \(Concatenating Strings and Characters\)

`String` 값은 더하기 연산자 \(`+`\)로 함께 추가 \(또는 연결\) 하여 새로운 `String` 값을 생성할 수 있습니다:

```swift
let string1 = "hello"
let string2 = " there"
var welcome = string1 + string2
// welcome now equals "hello there"
```

또한 존재하는 `String` 변수에 덧셈 대입 연산자 \(`+=`\)로 `String` 값을 연결할 수 있습니다:

```swift
var instruction = "look over"
instruction += string2
// instruction now equals "look over there"
```

`String` 타입의 `append()` 메서드를 이용하여 `String` 변수에 `Character` 값을 추가할 수 있습니다:

```swift
let exclamationMark: Character = "!"
welcome.append(exclamationMark)
// welcome now equals "hello there!"
```

> NOTE   
> `Character` 값은 반드시 하나의 문자만 포함해야 하므로 `Character` 변수에 추가로 `String` 또는 `Character` 를 추가할 수 없습니다.

여러줄 문자열 리터럴을 이용하여 긴 문자열의 라인을 구성하는 경우 문자열 매 라인 끝에 공백이 포함되길 원합니다. 예를 들어:

```swift
let badStart = """
one
two
"""
let end = """
three
"""
print(badStart + end)
// Prints two lines:
// one
// twothree

let goodStart = """
one
two

"""
print(goodStart + end)
// Prints three lines:
// one
// two
// three
```

위의 코드에서 `badStart` 와 `end` 를 출력하면 원하는 결과와 다르게 2줄 문자열로 출력됩니다. `badStart` 의 마지막 줄에 공백이 포함되지 않았기 때문에 `end` 의 첫번째 줄과 결합된 결과를 얻게 됩니다. 반면에 `goodStart` 마지막 줄에 개행이 포함되므로 `end` 와 결합이 되면 기대했던 결과와 같이 3줄로 출력됩니다.

## 문자열 삽입 \(String Interpolation\)

_문자열 삽입 \(String interpolation\)_ 은 상수, 변수, 리터럴, 그리고 문자열 리터럴에 값이 포함된 표현식을 혼합해 새로운 `String` 값을 생성하는 방법입니다. 문자열 삽입은 한줄과 여러줄 문자열 리터럴에서 사용할 수 있습니다. 문자열 리터럴에 추가하는 방법은 백슬래시 \(`\`\) 접두사에 소괄호를 감싸서 추가합니다:

```swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
// message is "3 times 2.5 is 7.5"
```

위의 예제에서 `multiplier` 의 값은 `\(multiplier)` 로 문자열 리터럴 안에 삽입됩니다. 문자열 삽입이 실제 문자열이 생성될 때 `multiplier` 의 실제 값으로 대체됩니다.

`multiplier` 의 값은 문자열에 표현식의 일부이기도 합니다. 이 표현식은 `Double(multiplier) * 2.5` 의 값을 계산하고 문자열에 결과 `(7.5)` 를 삽입합니다. 이 경우 표현식은 문자열 리터럴에 포함될 때 `\(Double(multiplier) * 2.5)` 로 작성합니다.

확장된 문자열 구분기호를 사용하여 문자열 삽입으로 사용할 문자를 포함하는 문자열을 생성할 수 있습니다. 예를 들어:

```swift
print(#"Write an interpolated string in Swift using \(multiplier)."#)
// Prints "Write an interpolated string in Swift using \(multiplier)."
```

확장된 구분기호를 사용하는 문자열에서 문자열 삽입을 사용하기 위해 문자열의 시작과 끝에 숫자 기호의 갯수만큼 백슬래시 다음에 숫자 시호를 넣어주면 됩니다. 예를 들어:

```swift
print(#"6 times 7 is \#(6 * 7)."#)
// Prints "6 times 7 is 42."
```

> NOTE   
> 소괄호 안에 작성한 표현식에 삽입된 문자열은 백슬래시 \(`\`\), 캐리지 리턴, 또는 개행을 포함할 수 없습니다. 그러나 다른 문자열 리터럴은 포함할 수 있습니다.

## 유니코드 \(Unicode\)

_유니코드 \(Unicode\)_ 는 인코딩, 표기, 그리고 다른 쓰기 시스템에서의 텍스트 프로세싱을 위한 국제 표준입니다. 거의 모든 언어의 문자를 표준화 된 형식으로 표현하고 텍스트 파일 또는 웹 페이지와 같은 외부 소스에서 해당 문자를 읽고 쓸 수 있습니다. Swift의 `String` 과 `Character` 타입은 유니코드를 완벽하게 지원합니다.

### 유니코드 스칼라 값 \(Unicode Scalar Values\)

Swift의 기본 `String` 타입은 _유니코드 스칼라 값 \(Unicode scalar values\)_ 으로부터 생성됩니다. 유니코드 스칼라 값은 `LATIN SMALL LETTER A` \(`"a"`\)의 경우 `U+0061` 또는 `FRONT-FACING BABY CHICK` \(`"🐥"`\)의 경우 `U+1F425` 와 같은 문자를 위한 유니크 한 21-bit 숫자 또는 수정자입니다.

모든 21-bit 유니코드 스칼라 값이 한 문자에 할당되는 것은 아닙니다. 어떤 스칼라는 나중에 할당되거나 UFT-16 인코딩에 사용하기 위해 지정되어 있습니다. 문자에 할당된 스칼라 값은 일반적으로 위의 예에서 `LATIN SMALL LETTER A` 와 `FRONT-FACING BABY CHICK` 같이 이름을 가지고 있습니다.

### 확장된 문자소 클러스터 \(Extended Grapheme Clusters\)

Swift의 `Character` 타입의 모든 인스턴스는 하나의 _확장된 문자소 클러스터 \(extended grapheme cluster\)_ 로 표기됩니다. 하나의 확장된 문자소 클러스터는 결합되었을 때 사람이 읽을 수 있는 하나의 문자인 하나 이상의 유니코드 스칼라 입니다.

여기 예제가 있습니다. 문자 `é` 은 단일 유니코드 스칼라 `é` \(`LATIN SMALL LETTER E WITH ACUTE`, or `U+00E9`\)로 표기 가능합니다. 그러나 동일 문자를 표준 문자 `e` \(`LATIN SMALL LETTER E`, or `U+0065`\) 이어서 `COMBINING ACUTE ACCENT` 스칼라 \(`U+0301`\)가 오는 스칼라 쌍으로 표시될 수도 있습니다. `COMBINING ACUTE ACCENT` 스칼라는 앞에 있는 스칼라에 그래픽으로 적용되어 유니코드 인식 텍스트 렌더링 시스템에서 렌더링 될 때 `e` 를 `é` 로 변환합니다.

두 경우 모두 문자 `é` 는 확장된 문자소 클라터를 나타내는 단일 Swift 문자값으로 표시됩니다. 첫번째의 경우에는 클러스터에는 단일 스칼라가 포함됩니다. 두번째의 경우에는 두 스칼라의 클러스터 입니다:

```swift
let eAcute: Character = "\u{E9}"                         // é
let combinedEAcute: Character = "\u{65}\u{301}"          // e followed by ́
// eAcute is é, combinedEAcute is é
```

확장된 문자소 클러스터는 많은 복잡한 스크립트 문자를 하나의 `Character` 값으로 표기하는 방법입니다. 예를 들어 한글의 한글 음절은 단일 시퀀스 또는 분해된 시퀀스로 표시될 수 있습니다. 이 두 표현은 모두 Swift에서 단일 `Character` 값으로 표시됩니다:

```swift
let precomposed: Character = "\u{D55C}"                  // 한
let decomposed: Character = "\u{1112}\u{1161}\u{11AB}"   // ᄒ, ᅡ, ᆫ
// precomposed is 한, decomposed is 한
```

확장된 문자소 클러스터는 마크 \(예를 들어 `COMBINING ENCLOSING CIRCLE` 또는 `U+20DD`\)를 둘러싸는 스칼라를 사용하여 다른 유니코드 스칼라를 단일 문자값의 일부로 묶을 수 있습니다:

```swift
let enclosedEAcute: Character = "\u{E9}\u{20DD}"
// enclosedEAcute is é⃝
```

국가 표시 기호에 대한 유니코드 스칼라를 쌍으로 결합하여 단일 `Character` 값으로 만들 수 있습니다. 예를 들어 `REGIONAL INDICATOR SYMBOL LETTER U`\(`U+1F1FA`\) 와 `REGIONAL INDICATOR SYMBOL LETTER S` \(`U+1F1F8`\) 의 결합이 있습니다:

```swift
let regionalIndicatorForUS: Character = "\u{1F1FA}\u{1F1F8}"
// regionalIndicatorForUS is 🇺🇸
```

## 문자 카운팅 \(Counting Characters\)

문자열에서 `Character` 값의 카운트를 구하려면 문자열에서 `count` 프로퍼티를 사용합니다:

```swift
let unusualMenagerie = "Koala 🐨, Snail 🐌, Penguin 🐧, Dromedary 🐪"
print("unusualMenagerie has \(unusualMenagerie.count) characters")
// Prints "unusualMenagerie has 40 characters"
```

Swift에서 `Character` 값을 위한 확장된 문자소 클러스터 사용은 문자열의 연결과 수정은 문자열의 문자 카운트에 영향을 주지 않습니다.

예를 들어 4개의 문자 단어인 `cafe` 로 새로운 문자열을 초기화 하고 문자열 끝에 `COMBINING ACUTE ACCENT` \(`U+0301`\)을 추가하면 결과 문자열은 4번째 문자가 `e` 가 아닌 `é` 가 되고 여전히 문자 카운트는 4 입니다:

```swift
var word = "cafe"
print("the number of characters in \(word) is \(word.count)")
// Prints "the number of characters in cafe is 4"

word += "\u{301}"    // COMBINING ACUTE ACCENT, U+0301

print("the number of characters in \(word) is \(word.count)")
// Prints "the number of characters in café is 4"
```

> NOTE   
> 확장된 문자소 클러스터는 여러개의 유니코드 스칼라로 구성할 수 있습니다. 이것은 다른 문자와 같은 문자의 다른 표기법은 저장할 때 메모리 사용량이 다르게 요구할 수 있다는 의미입니다. 이 때문에 Swift의 문자는 각각 문자열에서 동일한 양의 메모리를 차지하지 않습니다. 그 결과 문자열에 문자의 숫자는 확장된 문자소 클러스터 경계를 결정하기 위해 문자열을 반복하지 않고는 계산할 수 없습니다. 특히 긴 문자열 값으로 작업하는 경우에 해당 문자열의 문자를 결정하려면 `count` 프로퍼티가 전체 문자열의 유니코드 스칼라를 반복해야 합니다.
>
> `count` 프로퍼티로 반환된 문자의 갯수는 같은 문자여도 `NSString` 의 `length` 프로퍼티와 항상 같지는 않습니다. `NSString` 의 길이는 문자열 내에 유니코드 확장된 문자소 클러스터 수가 아니라 문자열의 UTF-16 표현 내의 16-bit 코드 단위 수를 기반으로 합니다.

## 문자열 접근과 수정 \(Accessing and Modifying a String\)

메서드와 프로퍼티 또는 서브 스크립트 구문으로 문자열에 접근, 수정할 수 있습니다.

### 문자열 인덱스 \(String Indices\)

각 `String` 값은 문자열에 각 `Character` 의 위치에 해당하는 `String.Index` 인 _인덱스 타입 \(index type\)_ 을 가지고 있습니다.

위에서 언급했듯이 문자마다 저장할 메모리 양이 다를 수 있으므로 특정 위치에 있는 `Character` 를 확인하려면 해당 `String` 의 시작 또는 끝에서 각 유니코드 스칼라를 반복해야 합니다. 이러한 이유로 Swift 문자열은 정수값으로 인덱스를 생성할 수 없습니다.

`String` 에 첫번째 `Character` 에 접근하기 위해 `startIndex` 프로퍼티를 사용합니다. `endIndex` 프로퍼티는 `String` 에 마지막 문자의 다음 위치입니다. 그 결과 `endIndex` 프로퍼티는 문자열의 서브 스크립트에 유효한 인자가 아닙니다. `String` 이 비어있다면 `startIndex` 와 `endIndex` 는 같습니다.

`String` 의 메서드 `index(before:)` 와 `index(after:)` 를 사용하여 주어진 인덱스의 전과 후에 접근할 수 있습니다. 주어진 인덱스에서 먼 인덱스에 접근하려면 이러한 메서드를 여러번 호출하는 대신 `index(_:offsetBy:)` 메서드를 사용할 수 있습니다.

특정 `String` 인덱스의 `Character` 에 접근하기 위해 서브 스크립트 구문을 사용할 수 있습니다.

```swift
let greeting = "Guten Tag!"
greeting[greeting.startIndex]
// G
greeting[greeting.index(before: greeting.endIndex)]
// !
greeting[greeting.index(after: greeting.startIndex)]
// u
let index = greeting.index(greeting.startIndex, offsetBy: 7)
greeting[index]
// a
```

문자열 범위에 벗어나는 인덱스로 접근하거나 문자열 범위에 벗어나는 인덱스의 `Character` 를 접근하려고 하면 런타임 에러가 발생합니다.

```swift
greeting[greeting.endIndex] // Error
greeting.index(after: greeting.endIndex) // Error
```

`indices` 프로퍼티를 사용하여 문자열에 있는 개별 문자의 모든 인덱스에 접근합니다.

```swift
for index in greeting.indices {
    print("\(greeting[index]) ", terminator: "")
}
// Prints "G u t e n   T a g ! "
```

> NOTE   
> `Collection` 프로토콜을 준수하는 어떠한 타입에서든 `startIndex` 와 `endIndex` 프로퍼티와 `index(before:)`, `index(after:)`, 그리고 `index(_:offsetBy:)` 메서드를 사용할 수 있습니다. 이것은 여기서 봤듯이 `String` 뿐만 아니라 `Array`, `Dictionary`, 그리고 `Set` 과 같은 콜렉션 타입도 포함됩니다.

### 삽입과 삭제 \(Inserting and Removing\)

문자열에 특정 인덱스에 하나의 문자를 삽입하려면 `insert(_:at:)` 메서드를 사용하고 다른 문자열의 콘텐츠를 특정 인덱스에 삽입하려면 `insert(contentsOf:at:)` 메서드를 사용합니다.

```swift
var welcome = "hello"
welcome.insert("!", at: welcome.endIndex)
// welcome now equals "hello!"

welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there!"
```

문자열에 특정 인덱스에 있는 하나의 문자를 삭제하려면 `remove(at:)` 메서드를 사용하고 특정 범위의 부분 문자열을 삭제하려면 `removeSubrange(_:)` 메서드를 사용합니다:

```swift
welcome.remove(at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there"

let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
welcome.removeSubrange(range)
// welcome now equals "hello"
```

> NOTE   
> `RangeReplaceableCollection` 프로토콜을 준수하는 어떠한 타입에서든 `insert(_:at:)`, `insert(contentsOf:at:)`, `remove(at:)` 그리고 `removeSubrange(_:)` 메서드를 사용할 수 있습니다. 이것은 여기서 봤듯이 `String` 뿐만 아니라 `Array`, `Dictionary`, 그리고 `Set` 과 같은 콜렉션 타입도 포함됩니다.

## 부분 문자열 \(Substrings\)

문자열에서 예를 들어 서브 스크립트 또는 `prefix(_:)` 와 같은 메서드를 사용하여 부분 문자열 \(substring\)을 얻을 때 그 결과는 다른 문자열이 아닌 [`Substring`](https://developer.apple.com/documentation/swift/substring) 의 인스턴스 입니다. Swift의 부분 문자열은 문자열과 거의 동일한 메서드를 가지고 있기 때문에 문자열과 동일하게 부분 문자열을 사용할 수 있습니다. 그러나 문자열과 다르게 문자열에 대한 작업을 수행하는 동안 짧은 시간동안 만 부분 문자열을 사용합니다. 결과를 저장할 준비가 되었을 때 부분 문자열을 `String` 의 인스턴스로 변환합니다. 예를 들어:

```swift
let greeting = "Hello, world!"
let index = greeting.firstIndex(of: ",") ?? greeting.endIndex
let beginning = greeting[..<index]
// beginning is "Hello"

// Convert the result to a String for long-term storage.
let newString = String(beginning)
```



문자열과 마찬가지로 각 부분 문자열에는 부분 문자열을 구성하는 문자가 저장되는 메모리 영역이 있습니다. 문자열과 부분 문자열의 차이점은 성능 최적화를 위해 부분 문자열이 원래 문자열을 저장하는데 사용된 메모리의 일부 또는 다른 부분 문자열을 저장하는데 사용되는 메모리의 일부를 재사용할 수 있다는 것입니다 \(문자열은 비슷한 최적화를 갖지만 두 문자열이 메모리를 공유하면 두 문자열은 같습니다\). 이 성능 최적화는 문자열이나 부분 문자열을 수정할 때까지 메모리 복사에 대한 비용을 지불할 필요가 없음을 의미합니다. 위에서 언급했듯이 부분 문자열은 장기 저장에 적합하지 않습니다. 이것은 원래 문자열의 저장소를 재사용하기 때문에 부분 문자열이 사용되는 한 전체 원본 문자열을 메모리에 보관해야 하기 때문입니다.

위의 예제에서 `greeting` 은 문자열을 구성하는 문자가 저장되는 메모리 영역이 있는 문자열입니다. `beginning` 은 `greeting` 의 부분 문자열이기 때문에 `greeting` 이 사용하는 메모리를 재사용합니다. 반대로 `newString` 은 문자열입니다. 즉 이것은 부분 문자열에서 생성될 때 자신만의 저장소를 가집니다. 아래는 이러한 관계를 보여줍니다:

![String and SubString relationship](../.gitbook/assets/03_stringsubstring_2x.png)

> NOTE   
> `String` 과 `Substring` 은 모두 `StringProtocol` 을 준수합니다. 이것은 문자열 조작 함수가 `StringProtocol` 값을 받아들이는 것이 편리한 경우가 많다는 것을 의미합니다. 이러한 함수는 `String` 또는 `Substring` 값으로 호출할 수 있습니다.

## 문자열 비교 \(Comparing Strings\)

Swift는 문자열과 문자 같음, 접두사 같음, 그리고 접미사 같음과 같이 텍스트 값을 비교하는 3가지 방법이 있습니다.

### 문자열과 문자 같음 \(String and Character Equality\)

[비교 연산자 \(Comparison Operators\)](basic-operators.md#comparison-operators) 에서 설명한대로 "같음" 연산자 \(`==`\)와 "같지 않음" 연산자 \(`!=`\)로 문자열과 문자가 같은지 확인합니다:

```swift
let quotation = "We're a lot alike, you and I."
let sameQuotation = "We're a lot alike, you and I."
if quotation == sameQuotation {
    print("These two strings are considered equal")
}
// Prints "These two strings are considered equal"
```

2개의 `String` 값 또는 2개의 `Character` 값은 확장된 문자소 클러스터가 _정규적으로 동일한 경우_ 2개의 값이 같다고 합니다. 확장된 문자소 클러스터는 다른 유니코드 스칼라로 구성되어 있더라도 언어적 의미와 모양이 같으면 정규적으로 같다고 합니다.

예를 들어 `LATIN SMALL LETTER E WITH ACUTE` \(`U+00E9`\)은 `LATIN SMALL LETTER E` \(`U+0065`\) 뒤에 `COMBINING ACUTE ACCENT` \(`U+0301`\)가 오는 것은 정규적으로 같습니다. 이러한 확장된 문자소 클러스터는 문자 `é` 을 표시하는 유효한 방법이므로 동일하다고 간주합니다:

```swift
// "Voulez-vous un café?" using LATIN SMALL LETTER E WITH ACUTE
let eAcuteQuestion = "Voulez-vous un caf\u{E9}?"

// "Voulez-vous un café?" using LATIN SMALL LETTER E and COMBINING ACUTE ACCENT
let combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?"

if eAcuteQuestion == combinedEAcuteQuestion {
    print("These two strings are considered equal")
}
// Prints "These two strings are considered equal"
```

반대로 영어로 사용된 `LATIN CAPITAL LETTER A` \(`U+0041`, or `"A"`\)와 러시아어로 사용된 `CYRILLIC CAPITAL LETTER A` \(`U+0410`, or `"А"`\)은 같지 않습니다. 이 문자는 모양은 같지만 언어적 의미가 같지 않습니다:

```swift
let latinCapitalLetterA: Character = "\u{41}"

let cyrillicCapitalLetterA: Character = "\u{0410}"

if latinCapitalLetterA != cyrillicCapitalLetterA {
    print("These two characters are not equivalent.")
}
// Prints "These two characters are not equivalent."
```

> NOTE   
> Swift의 문자열과 문자 비교는 로케일을 구분하지 않습니다.

### 접두사와 접미사 같음 \(Prefix and Suffix Equality\)

문자열이 특정 문자열 접두사 또는 접미사를 가지고 있는지 확인하기 위해 문자열의 `hasPrefix(_:)` 와 `hasSuffix(_:)` 메서드를 호출하면 됩니다. 두 메서드는 하나의 `String` 타입 인자를 받고 부울 값을 반환합니다.

아래 예에서는 셰익스피어의 _로미오와 줄리엣_ 에 처음 두막의 장면을 나타내는 문자열 배열을 나타냅니다:

```swift
let romeoAndJuliet = [
    "Act 1 Scene 1: Verona, A public place",
    "Act 1 Scene 2: Capulet's mansion",
    "Act 1 Scene 3: A room in Capulet's mansion",
    "Act 1 Scene 4: A street outside Capulet's mansion",
    "Act 1 Scene 5: The Great Hall in Capulet's mansion",
    "Act 2 Scene 1: Outside Capulet's mansion",
    "Act 2 Scene 2: Capulet's orchard",
    "Act 2 Scene 3: Outside Friar Lawrence's cell",
    "Act 2 Scene 4: A street in Verona",
    "Act 2 Scene 5: Capulet's mansion",
    "Act 2 Scene 6: Friar Lawrence's cell"
]
```

`hasPrefix(_:)` 메서드를 이용하여 `romeoAndJuliet` 배열의 연극 Act 1의 장면의 갯수를 카운트 할 수 있습니다:

```swift
var act1SceneCount = 0
for scene in romeoAndJuliet {
    if scene.hasPrefix("Act 1 ") {
        act1SceneCount += 1
    }
}
print("There are \(act1SceneCount) scenes in Act 1")
// Prints "There are 5 scenes in Act 1"
```

마찬가지로 `hasSuffix(_:)` 메서드를 사용하여 Capulet’s mansion 과 Friar Lawrence’s cell 의 장면의 갯수를 구할 수 있습니다:

```swift
var mansionCount = 0
var cellCount = 0
for scene in romeoAndJuliet {
    if scene.hasSuffix("Capulet's mansion") {
        mansionCount += 1
    } else if scene.hasSuffix("Friar Lawrence's cell") {
        cellCount += 1
    }
}
print("\(mansionCount) mansion scenes; \(cellCount) cell scenes")
// Prints "6 mansion scenes; 2 cell scenes"
```

> NOTE   
> `hasPrefix(_:)` 와 `hasSuffix(_:)` 메서드는 [문자열과 문자 같음 \(String and Character Equality\)](strings-and-characters.md#string-and-character-equality) 에서 설명했듯이 각 문자열에 확장된 문자소 클러스터 간의 문자 단위로 동등한지 수행합니다.

## 문자열의 유니코드 표현 \(Unicode Representations of Strings\)

유니코드 문자열이 텍스트 파일 또는 다른 저장소에 쓰여질 때 해당 문자열의 유니코드 스칼라는 정의된 유니코드 _인코딩 형식_ 중에 하나로 인코딩 됩니다. 각 형식은 _코드 유닛_ 으로 알려진 작 청크로 문자열을 인코딩합니다. 이것은 UTF-8 인코딩 형식 \(8-bit 코드 유닛으로 문자열을 인코딩\), UTF-16 인코딩 형식 \(16-bit 코드 유닛으로 문자열을 인코딩\), UTF-32 인코딩 형식 \(32-bit 코드 유닛으로 문자열을 인코딩\)을 포함합니다.

Swift는 문자열의 유니코드 표현에 접근하는 여러가지 방법을 제공합니다. `for`-`in` 구문으로 문자열을 반복하여 유니코드 확장된 문자소 클러스터인 `Character` 값에 접근할 수 있습니다. [문자 작업 \(Working with Characters\)](strings-and-characters.md#working-with-characters) 에 자세히 설명되어 있습니다.

또는 세가지 다른 유니코드 호환 표현 중 하나로 `String` 값에 접근할 수 있습니다:

* UTF-8 코드 유닛 \(문자열의 `utf8` 프로퍼티로 접근\)
* UTF-16 코드 유닛 \(문자열의 `utf16` 프로퍼티로 접근\)
* 문자열의 UTF-32 인코딩 형식에 해당하는 21-bit 유니코드 스칼라 값 \(문자열의 `unicodeScalars` 프로퍼티로 접근\)

아래의 각 예제는 `D`, `o`, `g`, `‼` 문자 \(`DOUBLE EXCLAMATION MARK`, 또는 유니코드 스칼라 `U+203C`\)와 `🐶` 문자 \(`DOG FACE`, 또는 유니코드 스칼라 `U+1F436`\)로 구성된 문자열에 다른 표현을 보여줍니다:

```swift
let dogString = "Dog‼🐶"
```

### UTF-8 표현 \(UTF-8 Representation\)

`utf8` 프로퍼티 반복을 통해 `String` 에 UTF-8 표현에 접근할 수 있습니다. 이 프로퍼티는 문자열의 UTF-8 표현에서 각 바이트에 대해 하나씩 부호가 없는 8-bit \(`UInt8`\) 값의 모음인 `String.UTF8View` 타입 입니다:

![UTF-8](../.gitbook/assets/03_utf8_2x.png)

```swift
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// Prints "68 111 103 226 128 188 240 159 144 182 "
```

위 예제에서 첫번째 3개의 `codeUnit` 값 \(`68`, `111`, `103`\)은 ASCII와 같은 UTF-8 표현인 문자 `D`, `o`, 그리고 `g` 를 나타냅니다. 다음 3개의 `codeUnit` 값 \(`226`, `128`, `188`\)은 `DOUBLE EXCLAMATION MARK` 문자의 3 바이트 UTF-8 표현입니다. 마지막 4개의 `codeUnit` 값 \(`240`, `159`, `144`, `182`\)은 `DOG FACE` 문자의 4 바이트 UTF-8 표현입니다.

### UTF-16 표현 \(UTF-16 Representation\)

`utf16` 프로퍼티 반복을 통해 `String` 에 UTF-16 표현에 접근할 수 있습니다. 이 프로퍼티는 문자열의 UTF-16 표현에서 각 16-bit 코드 유닛에 대해 하나씩 부호가 없는 16-bit \(`UInt16`\) 값의 모음인 `String.UTF16View` 타입 입니다:

![UTF-16](../.gitbook/assets/03_utf16_2x.png)

```swift
for codeUnit in dogString.utf16 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 55357 56374 "
```

다시 처음 3개의 `codeUnit` 값 \(`68`, `111`, `103`\)은 UTF-16 코드 유닛은 문자열의 UTF-8 표현과 같은 값을 가지므로 문자 `D`, `o`, 그리고 `g` 입니다 \(유니코드 스칼라는 ASCII 문자를 표현하기 때문\).

4번째 `codeUnit` 값 \(`8252`\)는 `DOUBLE EXCLAMATION MARK` 문자에 해당하는 유니코드 스칼라 `U+203C` 에서 16진법 `203C` 값을 나타내는 10진법 수 입니다. 이 문자는 UTF-16에서 하나의 코드 유닛으로 표현할 수 있습니다.

5번째와 6번째 `codeUnit` 값 \(`55357` 과 `56374`\)은 `DOG FACE` 문자의 UTF-16 대리 쌍 표현입니다. 이 값은 높은 대리 값 `U+D83D` \(10진법 값 `55357`\)와 낮은 대리 값 `U+DC36` \(10진법 값 `56374`\) 입니다.

### 유니코드 스칼라 표현 \(Unicode Scalar Representation\)

`unicodeScalars` 프로퍼티 반복을 통해 `String` 값에 유니코드 스칼라 표현에 접근할 수 있습니다. 이 프로퍼티는 `UnicodeScalar` 타입의 값의 모음인 `UnicodeScalarView` 타입 입니다.

각 `UnicodeScalar` 는 `UInt32` 값안에 표현된 스칼라의 21-bit 값은 반환하는 `value` 프로퍼티를 가지고 있습니다:

![Unicode Scalar](../.gitbook/assets/03_unicodescalar_2x.png)

```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ", terminator: "")
}
print("")
// Prints "68 111 103 8252 128054 "
```

다시 한번 `value` 프로퍼티를 통해 얻은 처음 3개의 `UnicodeScalar` 값 \(`68`, `111`, `103`\)은 문자 `D`, `o`, 그리고 `g` 를 표현합니다.

4번째 `codeUnit` 값 \(`8252`\)은 `DOUBLE EXCLAMATION MARK` 문자에 해당하는 유니코드 스칼라 `U+203C` 에서 16진법 `203C` 와 동일한 10진법 수 입니다.

`value` 프로퍼티의 마지막 `UnicodeScalar` 인 `128054` 는 `DOG FACE` 문자에 해당하는 유니코드 스칼라 `U+1F436` 에서 16진법 `1F436` 의 10진법 수 입니다.

`value` 프로퍼티 대신에 각 `UnicodeScalar` 값은 문자열 삽입을 사용하는 것과 같이 새로운 `String` 값을 생성하는데 사용할 수 있습니다:

```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar) ")
}
// D
// o
// g
// ‼
// 🐶
```

