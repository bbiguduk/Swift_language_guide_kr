# 서브스크립트 (Subscripts)

컬렉션의 요소에 접근합니다.

클래스, 구조체, 열거형은 *서브스크립트(subscript)*를 정의할 수 있고,
이것은 컬렉션, 리스트, 시퀀스의 구성 요소에 접근할 수 있는 간결한 문법을 제공합니다.
서브스크립트를 사용하면 값을 설정하고 가져오는
별도의 메서드를 만들 필요 없이 인덱스를 통해 간단하게 값을 설정하거나 검색할 수 있습니다.
예를 들어 `Array` 인스턴스의 요소는 `someArray[index]`로
`Dictionary` 인스턴스의 요소는 `someDictionary[key]`로 접근합니다.

하나의 타입에 여러 개의 서브스크립트를 정의할 수 있고,
넘겨주는 인덱스의 타입에 따라
적절한 서브스크립트 오버로드가 선택됩니다.
서브스크립트는 1차원에만 제한되지 않고,
사용자 타입에 맞춰
여러 개의 입력 매개변수로 서브스크립트를 정의할 수 있습니다.

<!--
  TODO: this chapter should provide an example of subscripting an enumeration,
  as per Joe Groff's example from rdar://16555559.
-->

## 서브스크립트 문법 (Subscript Syntax)

서브스크립트를 사용하면, 인스턴스 이름 뒤에 대괄호 안에 하나 이상의 값을 작성하여
타입의 인스턴스를 조회할 수 있습니다.
서브스크립트 문법은 인스턴스 메서드 문법과 연산 프로퍼티 문법과 유사합니다.
서브스크립트는 `subscript` 키워드로 정의하며,
인스턴스 메서드처럼
하나 이상의 입력 매개변수와 반환 타입을 작성합니다.
인스턴스 메서드와 다르게 서브스크립트는 읽기-쓰기 또는 읽기 전용 형태가 될 수 있습니다.
이러한 동작은 연산 프로퍼티와 같은 방법으로
getter와 setter를 통해 정의합니다:

```swift
subscript(index: Int) -> Int {
    get {
        // Return an appropriate subscript value here.
    }
    set(newValue) {
        // Perform a suitable setting action here.
    }
}
```

<!--
  - test: `subscriptSyntax`

  ```swifttest
  >> class Test1 {
  -> subscript(index: Int) -> Int {
        get {
           // Return an appropriate subscript value here.
  >>       return 1
        }
        set(newValue) {
           // Perform a suitable setting action here.
        }
     }
  >> }
  ```
-->

서브스크립트에서 `newValue`의 타입은 해당 서브스크립트의 반환 타입과 동일합니다.
연산 프로퍼티와 마찬가지로, setter의 `(newValue)` 매개변수를
명시하지 않아도 됩니다.
매개변수를 지정하지 않으면,
setter에 `newValue`라는 기본 매개변수가 제공됩니다.

읽기 전용 연산 프로퍼티와 마찬가지로
`get` 키워드와 중괄호를 생략하여
읽기 전용 서브스크립트를 간단히 선언할 수 있습니다:

```swift
subscript(index: Int) -> Int {
    // Return an appropriate subscript value here.
}
```

<!--
  - test: `subscriptSyntax`

  ```swifttest
  >> class Test2 {
  -> subscript(index: Int) -> Int {
        // Return an appropriate subscript value here.
  >>    return 1
     }
  >> }
  ```
-->

다음은 정수의 *n*단 곱셈표를 나타내는 `TimesTable` 구조체를 정의하는
읽기 전용 서브스크립트 구현의 예입니다:

```swift
struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index
    }
}
let threeTimesTable = TimesTable(multiplier: 3)
print("six times three is \(threeTimesTable[6])")
// Prints "six times three is 18"
```

<!--
  - test: `timesTable`

  ```swifttest
  -> struct TimesTable {
        let multiplier: Int
        subscript(index: Int) -> Int {
           return multiplier * index
        }
     }
  -> let threeTimesTable = TimesTable(multiplier: 3)
  -> print("six times three is \(threeTimesTable[6])")
  <- six times three is 18
  ```
-->

이 예시에서 `TimesTable`의 새로운 인스턴스를 생성하여
3단 곱셈표를 나타냅니다.
이는 구조체의 `initializer`에 `multiplier` 값으로
`3`을 전달함으로써 설정됩니다.

`threeTimesTable[6]`에 대한 호출에서 보여준 것처럼
서브스크립트를 호출하여 `threeTimesTable` 인스턴스를 조회할 수 있습니다.
이것은 `3`의 `6`배인 `18`의 값을 반환하는
3단 곱셈표에서 6번째 값을 요청합니다.

> Note: *n*단 곱셈표는 수학적 규칙을 기반으로 합니다.
> `threeTimesTable[someIndex]`에 새로운 값을 설정하는 것은 적절하지 않으므로,
> `TimesTable`의 서브스크립트는 읽기 전용 서브스크립트로 정의됩니다.

## 서브스크립트 활용 (Subscript Usage)

"서브스크립트"의 정확한 의미는 사용되는 문맥에 따라 다릅니다.
일반적으로 서브스크립트는 컬렉션, 리스트, 시퀀스의 구성 요소에
접근하기 위한 간결한 문법으로 사용됩니다.
특정 클래스나 구조체의
가장 적합한 방식으로 서브스크립트를 구현할 수 있습니다.

예를 들어 Swift의 `Dictionary` 타입은
서브스크립트를 통해 `Dictionary` 인스턴스에 저장된 값을 설정하고 조회할 수 있도록 구현되어 있습니다.
서브스크립트 대괄호 내에 딕셔너리의 키 타입의 키를 제공하고
딕셔너리의 값 타입의 값을 서브스크립트에 할당하여
딕셔너리에 값을 설정할 수 있습니다:

```swift
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
```

<!--
  - test: `dictionarySubscript`

  ```swifttest
  -> var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
  -> numberOfLegs["bird"] = 2
  ```
-->

위의 예시는 `numberOfLegs`라는 변수를 정의하고
세 개의 키-값 쌍을 포함하는 딕셔너리를 초기화합니다.
`numberOfLegs` 딕셔너리의 타입은 `[String: Int]`로 유추됩니다.
딕셔너리가 생성된 후에,
이 예시는 서브스크립트 할당을 통해
`"bird"`라는 `String` 키와 `2`라는 `Int` 값을 딕셔너리에 추가합니다.

`Dictionary` 서브스크립트에 대한 자세한 내용은
[딕셔너리 접근과 수정 (Accessing and Modifying a Dictionary)](./collection-types.md#딕셔너리-접근과-수정-accessing-and-modifying-a-dictionary)을 참고바랍니다.

> Note: Swift의 `Dictionary` 타입은 키-값 서브스크립트를
> *옵셔널* 타입을 사용하는 서브스크립트로 구현합니다.
> 위의 `numberOfLegs` 딕셔너리에서
> 키-값 서브스크립트는 타입 `Int?`
> 또는 "옵셔널 int"의 값을 주고받습니다.
> `Dictionary` 타입은 모든 키에 값이 있는 것이 아니며,
> 키에 대해 `nil`을 할당하여
> 키에 대한 값을 삭제하는 방법을 제공하기 위해 옵셔널 서브스크립트 타입을 사용합니다.

## 서브스크립트 옵션 (Subscript Options)

서브스크립트는 여러 개의 입력 매개변수를 가질 수 있고,
입력 매개변수는 어떤 타입도 가능합니다.
서브스크립트는 어떤 타입도 반환할 수 있습니다.

함수 처럼,
서브스크립트는 [가변 매개변수 (Variadic Parameters)](./functions.md#가변-매개변수-variadic-parameters)와
[매개변수 기본값 (Default Parameter Values)](./functions.md#매개변수-기본값-default-parameter-values)에서 설명했듯이
가변 매개변수를 받거나
기본값을 가질 수 있습니다.
그러나 함수와 다르게,
서브스크립트는 in-out 매개변수를 사용할 수 없습니다.

<!--
  - test: `subscripts-can-have-default-arguments`

  ```swifttest
  >> struct Subscriptable {
  >>     subscript(x: Int, y: Int = 0) -> Int {
  >>         return 100
  >>     }
  >> }
  >> let s = Subscriptable()
  >> print(s[0])
  << 100
  ```
-->

클래스나 구조체는 필요한 만큼 여러 개의 서브스크립트 구현을 제공할 수 있으며,
서브스크립트가 사용되는 시점에
대괄호 안에 전달된 값의 타입을 기반으로
어떤 서브스크립트가 호출될지가 자동으로 추론됩니다.
이러한 여러 서브스크립트를 정의하는 것을 *서브스크립트 오버로딩(subscript overloading)*이라 합니다.

대부분 서브스크립트는 하나의 매개변수를 가지지만,
타입에 따라 필요한 경우
여러 개의 매개변수를 가진 서브스크립트를 정의할 수도 있습니다.
다음의 예시는 `Double` 값의 2차원 행렬을 나타내는
`Matrix` 구조체를 정의합니다.
`Matrix` 구조체의 서브스크립트는 두 개의 정수 매개변수를 가집니다:

```swift
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(repeating: 0.0, count: rows * columns)
    }
    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}
```

<!--
  - test: `matrixSubscript, matrixSubscriptAssert`

  ```swifttest
  -> struct Matrix {
        let rows: Int, columns: Int
        var grid: [Double]
        init(rows: Int, columns: Int) {
           self.rows = rows
           self.columns = columns
           grid = Array(repeating: 0.0, count: rows * columns)
        }
        func indexIsValid(row: Int, column: Int) -> Bool {
           return row >= 0 && row < rows && column >= 0 && column < columns
        }
        subscript(row: Int, column: Int) -> Double {
           get {
              assert(indexIsValid(row: row, column: column), "Index out of range")
              return grid[(row * columns) + column]
           }
           set {
              assert(indexIsValid(row: row, column: column), "Index out of range")
              grid[(row * columns) + column] = newValue
           }
        }
     }
  ```
-->

`Matrix`는 `rows`와 `columns`라는 두 개의 매개변수를 가지고
`Double` 타입의 `rows * columns` 값을 저장할 수 있는 큰 배열을 생성하는 초기화를 제공합니다.
행렬의 각 위치는 `0.0`의 초기값이 주어집니다.
이를 위해 배열의 크기와 초기 셀 값 `0.0`을 이니셜라이저에 전달하여,
올바른 크기의 새로운 배열을 생성하고 초기화합니다.
이러한 이니셜라이저는
[기본값 배열 생성 (Creating an Array with a Default Value)](./collection-types.md#기본값-배열-생성-creating-an-array-with-a-default-value)에 자세히 설명되어 있습니다.

적절한 행과 열의 수를 이니셜라이저에 전달하여
새로운 `Matrix` 인스턴스를 생성할 수 있습니다:

```swift
var matrix = Matrix(rows: 2, columns: 2)
```

<!--
  - test: `matrixSubscript, matrixSubscriptAssert`

  ```swifttest
  -> var matrix = Matrix(rows: 2, columns: 2)
  >> assert(matrix.grid == [0.0, 0.0, 0.0, 0.0])
  ```
-->

위의 예시는 두 개의 행과 두 개의 열을 가지는 새로운 `Matrix` 인스턴스를 생성합니다.
`Matrix` 인스턴스의 `grid` 배열은
행렬을 좌측 상단에서 우측 하단으로
펼친 1차원 배열입니다:

![Subscript Matrix 1](../.gitbook/assets/subscriptMatrix01_2x~dark.png)

행렬의 값은 콤마로 구분된
서브스크립트에 행과 열 값을 전달하여 설정될 수 있습니다:

```swift
matrix[0, 1] = 1.5
matrix[1, 0] = 3.2
```

<!--
  - test: `matrixSubscript, matrixSubscriptAssert`

  ```swifttest
  -> matrix[0, 1] = 1.5
  >> print(matrix[0, 1])
  << 1.5
  -> matrix[1, 0] = 3.2
  >> print(matrix[1, 0])
  << 3.2
  ```
-->

이 두 구문은 서브스크립트의 setter를 호출하여,
행렬의 우측 상단
(`row`가 `0`이고, `column`이 `1`)에 `1.5` 값을 설정하고,
좌측 하단
(`row`가 `1`이고, `column`이 `0`)에 `3.2` 값을 설정합니다:

![Subscript Matrix 2](../.gitbook/assets/subscriptMatrix02_2x~dark.png)

`Matrix` 서브스크립트의 getter와 setter는 모두
서브스크립트의 `row`와 `column` 값이 유효한지 판단하기 위해 어설션(assertion)을 포함합니다.
이러한 검사를 지원하기 위해,
`Matrix`는 요청한 `row`와 `column`이
행렬의 범위 안에 있는지를 판단하기 위해
`indexIsValid(row:column:)`이라는 편의 메서드를 제공합니다:

```swift
func indexIsValid(row: Int, column: Int) -> Bool {
    return row >= 0 && row < rows && column >= 0 && column < columns
}
```

<!--
  - test: `matrixSubscript`

  ```swifttest
  >> var rows = 2
  >> var columns = 2
  -> func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
     }
  ```
-->

어셜션은 행렬 범위를 넘어서
서브스크립트를 접근하려고 하면 오류가 발생합니다:

```swift
let someValue = matrix[2, 2]
// This triggers an assert, because [2, 2] is outside of the matrix bounds.
```

<!--
  - test: `matrixSubscriptAssert`

  ```swifttest
  -> let someValue = matrix[2, 2]
  xx assert
  // This triggers an assert, because [2, 2] is outside of the matrix bounds.
  ```
-->

## 타입 서브스크립트 (Type Subscripts)

위에서 설명했듯이 인스턴스 서브스크립트는
특정 타입의 인스턴스에 대해 호출하는 서브스크립트입니다.
타입 자체에 대해 호출할 수 있는 서브스크립트도 정의할 수 있습니다.
이런 종류의 서브스크립트를 *타입 서브스크립트(type subscript)*라고 합니다.
`subscript` 키워드 앞에 `static` 키워드를 작성하여
타입 서브스크립트를 정의합니다.
클래스는 하위 클래스에서 서브스크립트의 구현을 재정의 할 수 있게
`class` 키워드를 대신 사용할 수도 있습니다.
아래 예시는 타입 서브스크립트를 어떻게 정의하고 호출하는지 보여줍니다:

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
    static subscript(n: Int) -> Planet {
        return Planet(rawValue: n)!
    }
}
let mars = Planet[4]
print(mars)
```

<!--
  - test: `static-subscript`

  ```swifttest
  -> enum Planet: Int {
        case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
        static subscript(n: Int) -> Planet {
           return Planet(rawValue: n)!
        }
     }
  -> let mars = Planet[4]
  >> assert(mars == Planet.mars)
  -> print(mars)
  << mars
  ```
-->

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
