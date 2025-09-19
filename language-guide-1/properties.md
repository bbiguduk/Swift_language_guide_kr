# 프로퍼티 (Properties)

인스턴스나 타입의 부분인 저장된 값과 계산된 값에 접근합니다.

*프로퍼티(Properties)*는 값을 특정 클래스, 구조체, 열거형에 연결합니다.
저장 프로퍼티(Stored properties)는 인스턴스의 일부로 상수와 변수 값을 저장하는
반면에 연산 프로퍼티(computed properties)는 값을 저장하는 대신에 계산합니다.
연산 프로퍼티(Computed properties)는 클래스, 구조체, 열거형에서 사용할 수 있습니다.
저장 프로퍼티는 클래스와 구조체에서만 제공됩니다.

<!--
  - test: `enumerationsCantProvideStoredProperties`

  ```swifttest
  -> enum E { case a, b; var x = 0 }
  !$ error: enums must not contain stored properties
  !! enum E { case a, b; var x = 0 }
  !! ^
  ```
-->

저장 프로퍼티와 연산 프로퍼티는 보통 특정 타입의 인스턴스와 관련되어 있습니다.
그러나 프로퍼티는 타입 자체에 연결될 수도 있습니다.
이러한 프로퍼티를 타입 프로퍼티라 합니다.

또한 프로퍼티 관찰자(property observers)를 정의하여 프로퍼티의 값이 변경되는 것을 감지하고
커스텀 동작에 응답할 수 있습니다.
프로퍼티 관찰자는 직접 정의한 저장 프로퍼티와
상속받은 프로퍼티에도 추가할 수 있습니다.

<!--
  - test: `propertyObserverIntroClaims`

  ```swifttest
  -> class C {
        var x: Int = 0 {
           willSet { print("C willSet x to \(newValue)") }
           didSet { print("C didSet x from \(oldValue)") }
        }
     }
  -> class D: C {
        override var x: Int {
           willSet { print("D willSet x to \(newValue)") }
           didSet { print("D didSet x from \(oldValue)") }
        }
     }
  -> var c = C(); c.x = 42
  <- C willSet x to 42
  <- C didSet x from 0
  -> var d = D(); d.x = 42
  <- D willSet x to 42
  <- C willSet x to 42
  <- C didSet x from 0
  <- D didSet x from 0
  ```
-->

프로퍼티 래퍼를 사용하여
여러 프로퍼티의 getter와 setter에서 코드를 재사용 할 수 있습니다.

## 저장 프로퍼티 (Stored Properties)

가장 단순한 형태의 저장 프로퍼티는
특정 클래스나 구조체 인스턴스의 부분으로 저장되는 상수나 변수입니다.
저장 프로퍼티는
*변수 저장 프로퍼티(variable stored properties)*(`var` 키워드 사용)나
*상수 저장 프로퍼티(constant stored properties)*(`let` 키워드 사용)로 쓸 수 있습니다.

[기본 프로퍼티 값 (Default Property Values)](./initialization.md#기본-프로퍼티-값-default-property-values)에서 설명한 대로
저장 프로퍼티에는 정의할 때 기본값을 제공할 수 있습니다.
초기화 중에 저장 프로퍼티에 초기값을 설정하고 수정할 수도 있습니다.
[초기화 중 상수 프로퍼티 값 할당 (Assigning Constant Properties During Initialization)](./initialization.md#초기화-중-상수-프로퍼티-값-할당-assigning-constant-properties-during-initialization)에서 설명한 대로
상수 저장 프로퍼티에도 마찬가지 입니다.

아래 예시는 `FixedLengthRange`라는 구조체를 정의합니다.
이 구조체는 범위 길이가 생성된 후에 변경할 수 없는
정수 범위를 나타냅니다:

```swift
struct FixedLengthRange {
    var firstValue: Int
    let length: Int
}
var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
// the range represents integer values 0, 1, and 2
rangeOfThreeItems.firstValue = 6
// the range now represents integer values 6, 7, and 8
```

<!--
  - test: `storedProperties, storedProperties-err`

  ```swifttest
  -> struct FixedLengthRange {
        var firstValue: Int
        let length: Int
     }
  -> var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
  // the range represents integer values 0, 1, and 2
  -> rangeOfThreeItems.firstValue = 6
  // the range now represents integer values 6, 7, and 8
  ```
-->

`FixedLengthRange`의 인스턴스는
`firstValue`라는 변수 저장 프로퍼티가 있으며
`length`라는 상수 저장 프로퍼티가 있습니다.
위의 예시에서 `length`는 새 범위가 생성될 때 초기화되며
상수 프로퍼티이므로 그 이후에는 변경할 수 없습니다.

### 상수 구조체 인스턴스의 저장 프로퍼티 (Stored Properties of Constant Structure Instances)

구조체 인스턴스를 생성하고
해당 인스턴스를 상수에 할당하면
해당 인스턴스의 프로퍼티가 변수로 선언되어 있어도
인스턴스의 프로퍼티를 수정할 수 없습니다:

```swift
let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
// this range represents integer values 0, 1, 2, and 3
rangeOfFourItems.firstValue = 6
// this will report an error, even though firstValue is a variable property
```

<!--
  - test: `storedProperties-err`

  ```swifttest
  -> let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
  // this range represents integer values 0, 1, 2, and 3
  -> rangeOfFourItems.firstValue = 6
  !$ error: cannot assign to property: 'rangeOfFourItems' is a 'let' constant
  !! rangeOfFourItems.firstValue = 6
  !! ~~~~~~~~~~~~~~~~ ^
  !$ note: change 'let' to 'var' to make it mutable
  !! let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
  !! ^~~
  !! var
  // this will report an error, even though firstValue is a variable property
  ```
-->

`rangeOfFourItems`는 `let` 키워드를 사용하여 상수로 선언되어 있기 때문에,
`firstValue`가 변수 프로퍼티로 선언되었더라도
`firstValue` 프로퍼티를 변경할 수 없습니다.

이러한 동작은 구조체가 *값 타입*이기 때문입니다.
값 타입의 인스턴스가 상수로 선언되면
인스턴스의 모든 프로퍼티에 대해 수정할 수 없습니다.

클래스는 *참조 타입*이므로 다르게 동작합니다.
참조 타입의 인스턴스를 상수로 할당하더라도,
인스턴스의 변수 프로퍼티는 변경할 수 있습니다.

<!--
  TODO: this explanation could still do to be improved.
-->

### 지연 저장 프로퍼티 (Lazy Stored Properties)

<!--
  QUESTION: is this section too complex for this point in the book?
  Should it go in the Default Property Values section of Initialization instead?
-->

*지연 저장 프로퍼티(lazy stored property)*는 처음 사용될 때까지
초기값이 계산되지 않는 프로퍼티입니다.
지연 저장 프로퍼티는
선언 앞에 `lazy` 수정자를 붙여 나타냅니다.

> Note: 인스턴스 초기화가 완료된 후에
> 초기값을 가져올 수도 있기 때문에
> 지연 프로퍼티는 `var` 키워드를 사용해 변수로 선언해야 합니다.
> 상수 프로퍼티는 초기화가 완료되기 *전에* 값을 가지고 있어야 하므로,
> 지연 프로퍼티로 선언할 수 없습니다.

<!--
  - test: `lazyPropertiesMustAlwaysBeVariables`

  ```swifttest
  -> class C { lazy let x = 0 }
  !$ error: 'lazy' cannot be used on a let
  !! class C { lazy let x = 0 }
  !! ^~~~~
  !!-
  ```
-->

지연 프로퍼티는 인스턴스의 초기화가 완료될 때까지
알 수 없는 외부 요인에 따라
초기값이 결정될 경우에 유용합니다.
지연 프로퍼티는 초기값 설정이 복잡하거나
계산 비용이 많이 들 때,
실제로 필요해질 때까지 초기화를 미루고 싶을 때에도 유용합니다.

<!--
  TODO: add a note that if you assign a value to a lazy property before first access,
  the initial value you give in your code will be ignored.
-->

아래의 예시는 복잡한 클래스에 불필요한 초기화를
피하기 위해 지연 저장 프로퍼티를 사용합니다.
이 예시는 `DataImporter`와 `DataManager`라는 두 개의 클래스를 정의하며,
이 클래스의 전체 내용은 생략되어 있습니다:

```swift
class DataImporter {
    /*
    DataImporter is a class to import data from an external file.
    The class is assumed to take a nontrivial amount of time to initialize.
    */
    var filename = "data.txt"
    // the DataImporter class would provide data importing functionality here
}

class DataManager {
    lazy var importer = DataImporter()
    var data = [String]()
    // the DataManager class would provide data management functionality here
}

let manager = DataManager()
manager.data.append("Some data")
manager.data.append("Some more data")
// the DataImporter instance for the importer property has not yet been created
```

<!--
  - test: `lazyProperties`

  ```swifttest
  -> class DataImporter {
        /*
        DataImporter is a class to import data from an external file.
        The class is assumed to take a nontrivial amount of time to initialize.
        */
        var filename = "data.txt"
        // the DataImporter class would provide data importing functionality here
  >>    init() {
  >>       print("the DataImporter instance for the importer property has now been created")
  >>    }
     }

  -> class DataManager {
        lazy var importer = DataImporter()
        var data: [String] = []
        // the DataManager class would provide data management functionality here
     }

  -> let manager = DataManager()
  -> manager.data.append("Some data")
  -> manager.data.append("Some more data")
  // the DataImporter instance for the importer property hasn't yet been created
  ```
-->

`DataManager` 클래스는 빈 `String` 배열로 초기화되는
`data`라는 저장 프로퍼티를 가지고 있습니다.
나머지 내용은 보이진 않지만,
`DataManager` 클래스의 목적은 `String` 데이터 배열을
관리하고 접근을 제공합니다.

`DataManager` 클래스의 기능 중 일부는
파일에서 데이터를 가져오는 기능입니다.
이 기능은 `DataImporter` 클래스로부터 제공되고,
초기화하는데 시간이 많이 걸린다고 가정합니다.
`DataImporter` 인스턴스가 초기화될 때 `DataImporter` 인스턴스는 파일을 열고
메모리로 내용을 읽어야 하기 때문일 수 있습니다.

`DataManager` 인스턴스는 파일로부터 데이터를 가져올 필요 없이
데이터를 관리할 수 있으므로,
`DataManager`가 생성될 때
새로운 `DataImporter` 인스턴스를 생성할 필요가 없습니다.
대신에 `DataImporter` 인스턴스를 처음 사용하는 경우에
생성하는 것이 더 적절합니다.

`lazy` 수정자가 붙어 있으므로,
`importer` 프로퍼티의 `DataImporter` 인스턴스는
`filename` 프로퍼티를 조회할 때처럼
`importer` 프로퍼티에 처음 접근할 때 생성됩니다:

```swift
print(manager.importer.filename)
// the DataImporter instance for the importer property has now been created
// Prints "data.txt"
```

<!--
  - test: `lazyProperties`

  ```swifttest
  -> print(manager.importer.filename)
  </ the DataImporter instance for the importer property has now been created
  <- data.txt
  ```
-->

> Note: `lazy` 수정자가 표시된 프로퍼티는
> 여러 스레드에서 동시에 접근되고
> 프로퍼티가 아직 초기화되지 않은 경우
> 프로퍼티가 한 번만 초기화된다는 보장이 없습니다.

<!--
  6/19/14, 10:54 PM [Contributor 7746]: @lazy isn't thread safe.  Global variables (and static struct/enum fields) *are*.
-->

### 저장 프로퍼티와 인스턴스 변수 (Stored Properties and Instance Variables)

Objective-C에 익숙하다면,
클래스 인스턴스의 일부로 값과 참조를 저장하는
*두 가지* 방법을 제공한다는 것을 알 수 있습니다.
프로퍼티 외에도,
인스턴스 변수를 사용하여 프로퍼티에 저장되는 값의 백업 저장소로 사용할 수 있습니다.

Swift는 하나의 프로퍼티 선언으로 이러한 개념을 통합합니다.
Swift의 프로퍼티는 대응되는 인스턴스 변수를 가지지 않으며,
프로퍼티의 백업 저장소는 직접적으로 접근할 수 없습니다.
이 접근방식은 다양한 상황에서 값에 접근하는 것에 대한 혼동을 피할 수 있으며,
프로퍼티 선언을 하나의 명확한 구문으로 단순화합니다.
이름, 타입, 메모리 관리 특성을 포함하는 ---
프로퍼티에 대한 모든 정보는 ---
타입 정의의 일부로 단일 위치에 정의됩니다.

<!--
  TODO: what happens if one property of a constant structure is an object reference?
-->

## 연산 프로퍼티 (Computed Properties)

저장 프로퍼티 외에도
클래스, 구조체, 열거형은 값을 실질적으로 저장하지 않는
*연산 프로퍼티(computed properties)*를 정의할 수 있습니다.
대신 다른 프로퍼티와 값을 간접적으로 조회하고 설정하는
getter와 선택적인 setter를 제공합니다.

```swift
struct Point {
    var x = 0.0, y = 0.0
}
struct Size {
    var width = 0.0, height = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set(newCenter) {
            origin.x = newCenter.x - (size.width / 2)
            origin.y = newCenter.y - (size.height / 2)
        }
    }
}
var square = Rect(origin: Point(x: 0.0, y: 0.0),
                  size: Size(width: 10.0, height: 10.0))
let initialSquareCenter = square.center
// initialSquareCenter is at (5.0, 5.0)
square.center = Point(x: 15.0, y: 15.0)
print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
// Prints "square.origin is now at (10.0, 10.0)"
```

<!--
  - test: `computedProperties`

  ```swifttest
  -> struct Point {
        var x = 0.0, y = 0.0
     }
  -> struct Size {
        var width = 0.0, height = 0.0
     }
  -> struct Rect {
        var origin = Point()
        var size = Size()
        var center: Point {
           get {
              let centerX = origin.x + (size.width / 2)
              let centerY = origin.y + (size.height / 2)
              return Point(x: centerX, y: centerY)
           }
           set(newCenter) {
              origin.x = newCenter.x - (size.width / 2)
              origin.y = newCenter.y - (size.height / 2)
           }
        }
     }
  -> var square = Rect(origin: Point(x: 0.0, y: 0.0),
        size: Size(width: 10.0, height: 10.0))
  -> let initialSquareCenter = square.center
  /> initialSquareCenter is at (\(initialSquareCenter.x), \(initialSquareCenter.y))
  </ initialSquareCenter is at (5.0, 5.0)
  -> square.center = Point(x: 15.0, y: 15.0)
  -> print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
  <- square.origin is now at (10.0, 10.0)
  ```
-->

이 예시는 도형을 다루기 위한 세 가지 구조체를 정의합니다:

- `Point`는 x, y 좌표의 위치를 캡슐화합니다.
- `Size`는 `width`와 `height`를 캡슐화합니다.
- `Rect`는 원점과 크기로 사각형을 정의합니다.

`Rect` 구조체는 `center`라는 연산 프로퍼티를 제공합니다.
`Rect`에 현재 중심 위치는 항상 `origin`과 `size`로 계산될 수 있고,
명시적인 `Point` 값으로 저장할 필요가 없습니다.
대신에 `Rect`는 `center`라는 연산 변수에 대해 커스텀 getter와 setter를 정의하고,
실제 저장 프로퍼티처럼 사각형의 `center`를 동작하도록 합니다.

위의 예시는 `square`라는 새로운 `Rect` 변수를 생성합니다.
`square` 변수는 `(0, 0)`의 원점으로
너비와 높이를 `10`으로 초기화 됩니다.
이 사각형은 아래의 다이어그램에서 연한 초록색 사각형으로 표현됩니다.

`square` 변수의 `center` 프로퍼티는 점 표기법(`square.center`)으로 접근되고,
`center`에 대한 getter를 호출하여
현재 프로퍼티 값을 가져옵니다.
기존값을 반환하는 대신,
getter는 실질적으로 사각형의 중심을 나타내는 새로운 `Point`를 계산하고 반환합니다.
위에서 볼 수 있듯이 getter는 `(5, 5)`의 중심점을 반환합니다.

아래 다이어그램의 진한 초록색 사각형처럼
`center` 프로퍼티는 `(15, 15)`의 새로운 값으로 설정되어
새로운 위치로 사각형이 위와 우측으로 이동합니다.
`center` 프로퍼티를 설정하면 `center`의 setter를 호출하고,
`origin` 프로퍼티에 저장된 `x`와 `y` 값을 변경하고
사각형을 새로운 위치로 이동합니다.

<!-- Apple Books screenshot begins here. -->

![Computed Properties](../.gitbook/assets/computedProperties_2x~dark.png)

### 간략한 Setter 선언 (Shorthand Setter Declaration)

연산 프로퍼티의 setter에서 설정할 새로운 값에 대한 이름을 정의하지 않으면,
`newValue`라는 기본 이름이 사용됩니다.
다음은 이러한 간략한 표기법의 이점을 가지는
`Rect` 구조체를 나타냅니다:

```swift
struct AlternativeRect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set {
            origin.x = newValue.x - (size.width / 2)
            origin.y = newValue.y - (size.height / 2)
        }
    }
}
```

<!--
  - test: `computedProperties`

  ```swifttest
  -> struct AlternativeRect {
        var origin = Point()
        var size = Size()
        var center: Point {
           get {
              let centerX = origin.x + (size.width / 2)
              let centerY = origin.y + (size.height / 2)
              return Point(x: centerX, y: centerY)
           }
           set {
              origin.x = newValue.x - (size.width / 2)
              origin.y = newValue.y - (size.height / 2)
           }
        }
     }
  ```
-->

<!-- Apple Books screenshot ends here. -->

### 간략한 Getter 선언 (Shorthand Getter Declaration)

getter의 전체 본문이 단일 표현식이라면,
getter는 암시적으로 해당 표현식을 반환합니다.
다음은 간략한 getter와
setter를
모두 활용한 `Rect` 구조체를 나타냅니다:

```swift
struct CompactRect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            Point(x: origin.x + (size.width / 2),
                  y: origin.y + (size.height / 2))
        }
        set {
            origin.x = newValue.x - (size.width / 2)
            origin.y = newValue.y - (size.height / 2)
        }
    }
}
```

<!--
  - test: `computedProperties`

  ```swifttest
  -> struct CompactRect {
        var origin = Point()
        var size = Size()
        var center: Point {
           get {
              Point(x: origin.x + (size.width / 2),
                    y: origin.y + (size.height / 2))
           }
           set {
              origin.x = newValue.x - (size.width / 2)
              origin.y = newValue.y - (size.height / 2)
           }
        }
     }
  ```
-->

getter에서 생략한 `return`은
[암시적 반환을 가진 함수 (Functions With an Implicit Return)](./functions.md#암시적-반환을-가진-함수-functions-with-an-implicit-return)에서 설명된
함수의 `return` 생략 규칙과 동일합니다.

### 읽기 전용 연산 프로퍼티 (Read-Only Computed Properties)

setter가 없고 getter만 있는 연산 프로퍼티는 *읽기 전용 연산 프로퍼티(read-only computed property)*라고 합니다.
읽기 전용 연산 프로퍼티는 항상 값을 반환하고,
점 표기법으로 접근할 수 있지만 다른 값을 설정할 수 없습니다.

> Note: 읽기 전용 연산 프로퍼티를 포함하여 --- 연산 프로퍼티는 ---
> 값이 고정되어 있지 않기 때문에 `var` 키워드를 사용해 변수 프로퍼티로 선언되어야 합니다.
> `let` 키워드는 인스턴스 초기화의 부분으로
> 한 번 설정되면 값을 변경할 수 없음을 나타내기 위해
> 오직 상수 프로퍼티에만 사용됩니다.

<!--
  - test: `readOnlyComputedPropertiesMustBeVariables`

  ```swifttest
  -> class C {
        let x: Int { return 42 }
        let y: Int { get { return 42 } set {} }
     }
  !! /tmp/swifttest.swift:2:15: error: 'let' declarations cannot be computed properties
  !! let x: Int { return 42 }
  !! ~~~        ^
  !! var
  !! /tmp/swifttest.swift:3:15: error: 'let' declarations cannot be computed properties
  !! let y: Int { get { return 42 } set {} }
  !! ~~~        ^
  !! var
  ```
-->

`get` 키워드와 중괄호를 생략함으로써
읽기 전용 연산 프로퍼티를 간편하게 선언할 수 있습니다:

```swift
struct Cuboid {
    var width = 0.0, height = 0.0, depth = 0.0
    var volume: Double {
        return width * height * depth
    }
}
let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
print("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
// Prints "the volume of fourByFiveByTwo is 40.0"
```

<!--
  - test: `computedProperties`

  ```swifttest
  -> struct Cuboid {
        var width = 0.0, height = 0.0, depth = 0.0
        var volume: Double {
           return width * height * depth
        }
     }
  -> let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
  -> print("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
  <- the volume of fourByFiveByTwo is 40.0
  ```
-->

이 예시는 `width`, `height`, `depth` 프로퍼티를 가지는 3D 직육면체를 나타내는
`Cuboid`라는 새로운 구조체를 정의합니다.
이 구조체는 직육면체의 현재 부피를 계산하고 반환하는
`volume`이라는 읽기 전용 연산 프로퍼티를 가지고 있습니다.
특정 `volume` 값을 위해
어떤 `width`, `height`, `depth` 값을 사용해야 하는지 모호하므로,
`volume`을 설정 가능하게 만드는 것은 적절하지 않습니다.
그럼에도 불구하고 현재 계산된 부피를 외부 사용자가 알 수 있도록
읽기 전용 연산 프로퍼티로 제공되는 `Cuboid`는 유용합니다.

<!--
  NOTE: getters and setters are also allowed for constants and variables
  that aren't associated with a particular class or struct.
  Where should this be mentioned?
-->

<!--
  TODO: Anything else from https://[Internal Staging Server]/docs/StoredAndComputedVariables.html
-->

<!--
  TODO: Add an example of a computed property for an enumeration
  (now that the Enumerations chapter no longer has an example of this itself).
-->

## 프로퍼티 관찰자 (Property Observers)

프로퍼티 관찰자(Property observers)는 프로퍼티의 값이 변경되는지 관찰하고 응답합니다.
프로퍼티 관찰자는 프로퍼티의 현재값이 새로운 값과 같더라도
프로퍼티의 값이 설정될 때 호출됩니다.

<!--
  - test: `observersAreCalledEvenIfNewValueIsTheSameAsOldValue`

  ```swifttest
  -> class C { var x: Int = 0 { willSet { print("willSet") } didSet { print("didSet") } } }
  -> let c = C()
  -> c.x = 24
  <- willSet
  <- didSet
  -> c.x = 24
  <- willSet
  <- didSet
  ```
-->

다음과 같은 위치에 프로퍼티 관찰자를 추가할 수 있습니다:

- 사용자가 정의한 저장 프로퍼티
- 상속받은 저장 프로퍼티
- 상속받은 연산 프로퍼티

상속된 프로퍼티의 경우,
하위 클래스에서 해당 프로퍼티를 재정의하여 프로퍼티 관찰자를 추가할 수 있습니다.
정의한 연산 프로퍼티의 경우,
관찰자를 생성하는 대신에
프로퍼티의 setter를 이용하여 값 변경을 관찰하고 응답해야 합니다.
재정의 프로퍼티에 대한 자세한 내용은 [재정의 (Overriding)](./inheritance.md#재정의-overriding)를 참고바랍니다.

<!--
  - test: `lazyPropertiesCanHaveObservers`

  ```swifttest
  >> class C {
        lazy var x: Int = 0 {
           willSet { print("C willSet x to \(newValue)") }
           didSet { print("C didSet x from \(oldValue)") }
        }
     }
  >> let c = C()
  >> print(c.x)
  << 0
  >> c.x = 12
  << C willSet x to 12
  << C didSet x from 0
  >> print(c.x)
  << 12
  ```
-->

<!--
  - test: `storedAndComputedInheritedPropertiesCanBeObserved`

  ```swifttest
  -> class C {
        var x = 0
        var y: Int { get { return 42 } set {} }
     }
  -> class D: C {
        override var x: Int {
           willSet { print("D willSet x to \(newValue)") }
           didSet { print("D didSet x from \(oldValue)") }
        }
        override var y: Int {
           willSet { print("D willSet y to \(newValue)") }
           didSet { print("D didSet y from \(oldValue)") }
        }
     }
  -> var d = D()
  -> d.x = 42
  <- D willSet x to 42
  <- D didSet x from 0
  -> d.y = 42
  <- D willSet y to 42
  <- D didSet y from 42
  ```
-->

프로퍼티에는 다음 두 가지 관찰자 중 하나 또는 둘 다 정의할 수 있습니다:

- `willSet`은 값이 저장되기 직전에 호출됩니다.
- `didSet`은 새로운 값이 저장된 직후에 호출됩니다.

`willSet` 관찰자를 구현하면,
상수 매개변수로 새로 설정될 프로퍼티 값이 전달됩니다.
`willSet` 구현의 일부로 이 매개변수에 이름을 지정할 수 있습니다.
매개변수 명과 구현 내에 소괄호를 작성하지 않으면,
매개변수는 `newValue`라는 기본 매개변수 명으로 사용됩니다.

마찬가지로 `didSet` 관찰자를 구현하면,
이전 프로퍼티 값을 포함한 상수 매개변수가 전달됩니다.
매개변수 명을 사용하거나 `oldValue`인 기본 매개변수 명을 사용할 수 있습니다.
`didSet` 관찰자 내에서 해당 프로퍼티에 새로운 값을 할당하면,
방금 설정된 값을 새로운 값으로 대체합니다.

<!--
  - test: `assigningANewValueInADidSetReplacesTheNewValue`

  ```swifttest
  -> class C { var x: Int = 0 { didSet { x = -273 } } }
  -> let c = C()
  -> c.x = 24
  -> print(c.x)
  <- -273
  ```
-->

> Note: 상위 클래스의 프로퍼티에 대한 `willSet`과 `didSet` 관찰자는
> 상위 클래스 초기화가 완료된 후
> 하위 클래스 이니셜라이저에서 해당 프로퍼티가 설정될 때 호출됩니다.
> 이것은 클래스가 이니셜라이저 본문에서
> 프로퍼티를 설정하는 동안에는 호출되지 않습니다.
>
> 초기화 위임에 대한 자세한 내용은
> [값 타입을 위한 이니셜라이저 위임 (Initializer Delegation for Value Types)](./initialization.md#값-타입을-위한-이니셜라이저-위임-initializer-delegation-for-value-types)과
> [클래스 타입의 이니셜라이저 위임 (Initializer Delegation for Class Types)](./initialization.md#클래스-타입의-이니셜라이저-위임-initializer-delegation-for-class-types)을 참고바랍니다.

<!--
  - test: `observersDuringInitialization`

  ```swifttest
  -> class C {
        var x: Int { willSet { print("willSet x") } didSet { print("didSet x") } }
        init(x: Int) { self.x = x }
     }
  -> let c = C(x: 42)
  -> c.x = 24
  <- willSet x
  <- didSet x
  -> class C2: C {
        var y: Int { willSet { print("willSet y") } didSet { print("didSet y") } }
        init() {
            self.y = 1
            print("calling super")
            super.init(x: 100)
            self.x = 10
        }
     }
  -> let c2 = C2()
  <- calling super
  <- willSet x
  <- didSet x
  ```
-->

다음은 `willSet`과 `didSet` 동작에 대한 예시입니다.
아래 예시는 사람이 걷는 동안의 걸음 수를 측정하는
`StepCounter`라는 새로운 클래스를 정의합니다.
이 클래스는 만보계나 다른 걸음 측정기의 입력 데이터와 함께 사용하여
일상생활에서 사람의 운동 루틴을 추적할 수 있습니다.

```swift
class StepCounter {
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("About to set totalSteps to \(newTotalSteps)")
        }
        didSet {
            if totalSteps > oldValue  {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}
let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps
stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
stepCounter.totalSteps = 896
// About to set totalSteps to 896
// Added 536 steps
```

<!--
  - test: `storedProperties`

  ```swifttest
  -> class StepCounter {
        var totalSteps: Int = 0 {
           willSet(newTotalSteps) {
              print("About to set totalSteps to \(newTotalSteps)")
           }
           didSet {
              if totalSteps > oldValue  {
                 print("Added \(totalSteps - oldValue) steps")
              }
           }
        }
     }
  -> let stepCounter = StepCounter()
  -> stepCounter.totalSteps = 200
  </ About to set totalSteps to 200
  </ Added 200 steps
  -> stepCounter.totalSteps = 360
  </ About to set totalSteps to 360
  </ Added 160 steps
  -> stepCounter.totalSteps = 896
  </ About to set totalSteps to 896
  </ Added 536 steps
  ```
-->

`StepCounter` 클래스는 `Int` 타입의 `totalSteps` 프로퍼티를 선언합니다.
이것은 `willSet`과 `didSet` 관찰자를 가진 저장 프로퍼티 입니다.

`totalSteps`의 `willSet`과 `didSet` 관찰자는
새로운 값이 프로퍼티에 할당될 때마다 호출됩니다.
새로운 값이 현재값과 같더라도 항상 호출됩니다.

이 예시의 `willSet` 관찰자는
새로운 값을 위한 `newTotalSteps`의 사용자 매개변수 명을 사용합니다.
이 예시는 단순히 설정될 값을 출력합니다.

`didSet` 관찰자는 `totalSteps` 값이 업데이트된 후에 호출됩니다.
이 관찰자는 새로운 `totalSteps` 값과 이전 값을 비교합니다.
걸음 수가 증가했다면,
걸음 수가 얼마나 증가하였는지 출력합니다.
`didSet` 관찰자는 이전 값에 대한 사용자 매개변수 명을 제공하지 않고,
대신에 `oldValue`의 기본 이름을 사용합니다.

> Note: 관찰자를 가진 프로퍼티를
> in-out 매개변수로 함수에 전달하면,
> `willSet`과 `didSet` 관찰자는 항상 호출됩니다.
> 이것은 in-out 매개변수에 대한 복사-입력(copy-in) 및 복사-출력(copy-out) 메모리 모델 때문에 그렇습니다:
> 함수가 끝날 때 값이 항상 프로퍼티로 다시 작성되기 때문에 관찰자가 호출되는 것입니다.
> in-out 매개변수에 대한 자세한 내용은
> [In-Out 매개변수 (In-Out Parameters)](../language-reference/declarations.md#in-out-매개변수-in-out-parameters)를 참고바랍니다.

<!--
  - test: `observersCalledAfterInout`

  ```swifttest
  -> var a: Int = 0 {
         willSet { print("willSet") }
         didSet { print("didSet") }
     }
  -> func f(b: inout Int) { print("in f") }
  -> f(b: &a)
  << in f
  << willSet
  << didSet
  ```
-->

<!--
  TODO: If you add a property observer to a stored property of structure type,
  that property observer is fired whenever any of the subproperties
  of that structure instance are set. This is cool, but nonobvious.
  Provide an example of it here.
-->

## 프로퍼티 래퍼 (Property Wrappers)

프로퍼티 래퍼(property wrapper)는
프로퍼티가 저장되는 방법을 관리하는 코드와
프로퍼티를 정의하는 코드 사이에 분리 계층을 추가합니다.
예를 들어
스레드 안정성 검사를 제공하거나
기본 데이터를 데이터베이스에 저장하는
프로퍼티가 있는 경우
모든 프로퍼티에 해당 코드를 작성해야 합니다.
프로퍼티 래퍼를 사용하면,
이러한 관리 코드를 래퍼를 정의할 때 한 번만 작성하고,
여러 프로퍼티에 재사용할 수 있습니다.

프로퍼티 래퍼를 정의하려면,
`wrappedValue` 프로퍼티를 정의한
구조체, 열거형, 클래스를 만들면 됩니다.
아래 코드에서
`TwelveOrLess` 구조체는
래핑된 값이 항상 12 이하의 숫자만 포함됩니다.
더 큰 숫자를 저장하려고 하면 12가 저장됩니다.

```swift
@propertyWrapper
struct TwelveOrLess {
    private var number = 0
    var wrappedValue: Int {
        get { return number }
        set { number = min(newValue, 12) }
    }
}
```

<!--
  - test: `small-number-wrapper, property-wrapper-expansion`

  ```swifttest
  -> @propertyWrapper
  -> struct TwelveOrLess {
         private var number = 0
         var wrappedValue: Int {
             get { return number }
             set { number = min(newValue, 12) }
         }
     }
  ```
-->

<!--
  No init(wrappedValue:) in this example -- that's in a later example.
  Always initializing the wrapped value is a simpler starting point.
-->

setter는 새로운 값이 12 이하인지 확인하고,
getter는 저장된 값을 반환합니다.

> Note: 위의 예시에서 `number` 프로퍼티는
> `TwelveOrLess`의 구현에서만
> `number`가 사용될 수 있도록
> `private`로 선언되어 있습니다.
> 다른곳에서 작성된 코드는
> `wrappedValue`를 위한 getter와 setter를 사용하여 값에 접근하고
> `number` 자체에는 직접 접근할 수 없습니다.
> `private`에 대한 정보는 [접근 제어 (Access Control)](./access-control.md)를 참고바랍니다.

<!--
  In this example,
  the number is stored in the wrapper's private ``number`` property,
  but you could write a version of ``EvenNumber``
  that implements ``wrappedValue`` as a stored property
  and uses ``didSet`` to ensure the number is always even.

  However, the general framing we use in the docs
  is that didSet is mostly for reacting to the new value,
  not changing it,
  so I'm not highlighting that fact here.
  The order of operations for willSet, set, and didSet is well defined,
  but might be something you have to pay attention to.
-->

<!--
  - test: `stored-property-wrappedValue`

  ```swifttest
  >> @propertyWrapper
  >> struct TwelveOrLess {
  >>     var wrappedValue: Int = 0 {
  >>         didSet {
  >>             if wrappedValue > 12 {
  >>                 wrappedValue = 12
  >>             }
  >>         }
  >>     }
  >> }
  >> struct SomeStructure {
  >>     @TwelveOrLess var someNumber: Int
  >> }
  >> var s = SomeStructure()
  >> print(s.someNumber)
  << 0
  >> s.someNumber = 10
  >> print(s.someNumber)
  << 10
  >> s.someNumber = 21
  >> print(s.someNumber)
  << 12
  ```
-->

프로퍼티에 래퍼를 적용하려면,
래퍼의 이름을 속성처럼
프로퍼티 앞에 작성하면 됩니다.
다음은 사각형의 크기를 정하는 구조체로
`TwelveOrLess` 프로퍼티 래퍼를 사용하여
너비와 높이가 항상 12 이하가 되도록 보장합니다:

```swift
struct SmallRectangle {
    @TwelveOrLess var height: Int
    @TwelveOrLess var width: Int
}

var rectangle = SmallRectangle()
print(rectangle.height)
// Prints "0"

rectangle.height = 10
print(rectangle.height)
// Prints "10"

rectangle.height = 24
print(rectangle.height)
// Prints "12"
```

<!--
  - test: `small-number-wrapper`

  ```swifttest
  -> struct SmallRectangle {
  ->     @TwelveOrLess var height: Int
  ->     @TwelveOrLess var width: Int
  -> }

  -> var rectangle = SmallRectangle()
  -> print(rectangle.height)
  <- 0

  -> rectangle.height = 10
  -> print(rectangle.height)
  <- 10

  -> rectangle.height = 24
  -> print(rectangle.height)
  <- 12
  ```
-->

`height`와 `width` 프로퍼티는
`TwelveOrLess.number`를 0으로 설정하는
`TwelveOrLess` 정의에서 초기값을 얻습니다.
`TwelveOrLess`의 setter는 10을 유효한 값으로 간주하기 때문에,
`rectangle.height`에 10을 저장하는 것은 성공적으로 저장합니다.
그러나 24는 `TwelveOrLess`에서 허용하는 값보다 크기 때문에,
24를 저장하려고 하면 `rectangle.height`는
대신 12로 설정되며, 이는 허용된 최대값입니다.

프로퍼티에 래퍼를 적용하면,
컴파일러는 래퍼를 위한 저장소를 제공하는 코드와
래퍼를 통해 프로퍼티에 접근하는 코드를 자동으로 생성합니다.
(프로퍼티 래퍼가 감싸진 값을 저장하는 역할을 하기 때문에,
값 자체를 저장하는 코드는 따로 생성되지 않습니다.)
특별한 속성 구문을 사용하지 않고도,
프로퍼티 래퍼의 동작을 직접 코드로 구현할 수도 있습니다.
예를 들어
다음은 이전 코드에서 등장한 `SmallRectangle`을
`@TwelveOrLess` 속성 없이
`TwelveOrLess` 구조체를 명시적으로 사용하여
프로퍼티를 래핑한 버전입니다:

```swift
struct SmallRectangle {
    private var _height = TwelveOrLess()
    private var _width = TwelveOrLess()
    var height: Int {
        get { return _height.wrappedValue }
        set { _height.wrappedValue = newValue }
    }
    var width: Int {
        get { return _width.wrappedValue }
        set { _width.wrappedValue = newValue }
    }
}
```

<!--
  - test: `property-wrapper-expansion`

  ```swifttest
  -> struct SmallRectangle {
         private var _height = TwelveOrLess()
         private var _width = TwelveOrLess()
         var height: Int {
             get { return _height.wrappedValue }
             set { _height.wrappedValue = newValue }
         }
         var width: Int {
             get { return _width.wrappedValue }
             set { _width.wrappedValue = newValue }
         }
     }
  ```
-->

`_height`와 `_width` 프로퍼티는
프로퍼티 래퍼인 `TwelveOrLess`의 인스턴스를 저장합니다.
`height`와 `width`의 getter와 setter는
각각 `wrappedValue` 프로퍼티에 접근합니다.

### 래핑된 프로퍼티에 초기값 설정 (Setting Initial Values for Wrapped Properties)

위 예시의 코드는
`TwelveOrLess` 정의 안에서 `number`에 초기값을 부여함으로써,
래핑된 프로퍼티의 초기값을 설정합니다.
이 프로퍼티 래퍼를 사용한 코드는
`TwelveOrLess`로 래핑된
프로퍼티에 다른 초기값을 지정할 수 없습니다 ---
예를 들어
`SmallRectangle` 정의에
`height`나 `width` 초기값을 지정할 수 없습니다.
초기값 설정을 지원하거나 다른 커스터마이징을 지원하려면,
프로퍼티 래퍼에 이니셜라이저를 추가해야 합니다.
다음은 래핑된 값과 최대값을 설정하는 초기화를 정의하는
`SmallNumber`라는 확장된 `TwelveOrLess`를 나타냅니다:

```swift
@propertyWrapper
struct SmallNumber {
    private var maximum: Int
    private var number: Int

    var wrappedValue: Int {
        get { return number }
        set { number = min(newValue, maximum) }
    }

    init() {
        maximum = 12
        number = 0
    }
    init(wrappedValue: Int) {
        maximum = 12
        number = min(wrappedValue, maximum)
    }
    init(wrappedValue: Int, maximum: Int) {
        self.maximum = maximum
        number = min(wrappedValue, maximum)
    }
}
```

<!--
  - test: `property-wrapper-init, property-wrapper-mixed-init`

  ```swifttest
  -> @propertyWrapper
  -> struct SmallNumber {
         private var maximum: Int
         private var number: Int

         var wrappedValue: Int {
             get { return number }
             set { number = min(newValue, maximum) }
         }

         init() {
             maximum = 12
             number = 0
         }
         init(wrappedValue: Int) {
             maximum = 12
             number = min(wrappedValue, maximum)
         }
         init(wrappedValue: Int, maximum: Int) {
             self.maximum = maximum
             number = min(wrappedValue, maximum)
         }
     }
  ```
-->

<!--
  The initializers above could be written to use
  init(wrappedValue:maximum:) as the designated initializer,
  with the other two calling it instead of doing initialization.
  However, in this case, the initialization logic is small enough
  that the risk of bugs isn't significant,
  and the reader hasn't seen init syntax/rules in detail yet
  so it's clearer to make each init stand on its own.
-->

`SmallNumber`의 정의는 세 가지 이니셜라이저가 포함되어 있으며 ---
`init()`, `init(wrappedValue:)`, `init(wrappedValue:maximum:)` 가 포함되어 있으며 ---
아래 예시에서는
이 이니셜라이저를 사용해 래핑된 값과 최대값을 설정합니다.
초기화와 이니셜라이저에 대한 자세한 내용은
[초기화 (Initialization)](./initialization.md)를 참고바랍니다.

프로퍼티에 래퍼를 적용할 때 초기값을 지정하지 않으면,
Swift는 래퍼를 설정하기 위해 `init()`을 사용합니다.
예를 들어:

```swift
struct ZeroRectangle {
    @SmallNumber var height: Int
    @SmallNumber var width: Int
}

var zeroRectangle = ZeroRectangle()
print(zeroRectangle.height, zeroRectangle.width)
// Prints "0 0"
```

<!--
  - test: `property-wrapper-init`

  ```swifttest
  -> struct ZeroRectangle {
  ->     @SmallNumber var height: Int
  ->     @SmallNumber var width: Int
  -> }

  -> var zeroRectangle = ZeroRectangle()
  -> print(zeroRectangle.height, zeroRectangle.width)
  <- 0 0
  ```
-->

<!--
  - test: `property-wrapper-init`

  ```swifttest
  -> struct ZeroRectangle_equiv {
         private var _height = SmallNumber()
         private var _width = SmallNumber()
         var height: Int {
             get { return _height.wrappedValue }
             set { _height.wrappedValue = newValue }
         }
         var width: Int {
             get { return _width.wrappedValue }
             set { _width.wrappedValue = newValue }
         }
     }
  -> var zeroRectangle_equiv = ZeroRectangle_equiv()
  -> print(zeroRectangle_equiv.height, zeroRectangle_equiv.width)
  <- 0 0
  ```
-->

`height`와 `width`를 래핑하는 `SmallNumber` 인스턴스는
`SmallNumber()`를 호출하여 생성됩니다.
초기화 내부의 코드는
기본값인 0과 12를 사용하여
초기 래핑 값과 초기 최대값을 설정합니다.
이 방식은 이전에 `SmallRectangle`에서 `TwelveOrLess`를 사용했던 예전처럼,
프로퍼티 래퍼가 모든 초기값을 제공합니다.
예시와 다르게
`SmallNumber`이 초기값을
프로퍼티 선언 시 직접 작성할 수 있다는 점에서 다릅니다.

프로퍼티에 초기값을 지정하면,
Swift는 래퍼를 설정하기 위해 `init(wrappedValue:)`를 사용합니다.
예를 들어:

```swift
struct UnitRectangle {
    @SmallNumber var height: Int = 1
    @SmallNumber var width: Int = 1
}

var unitRectangle = UnitRectangle()
print(unitRectangle.height, unitRectangle.width)
// Prints "1 1"
```

<!--
  - test: `property-wrapper-init`

  ```swifttest
  -> struct UnitRectangle {
  ->     @SmallNumber var height: Int = 1
  ->     @SmallNumber var width: Int = 1
  -> }

  -> var unitRectangle = UnitRectangle()
  -> print(unitRectangle.height, unitRectangle.width)
  <- 1 1
  ```
-->

<!--
  - test: `property-wrapper-init`

  ```swifttest
  -> struct UnitRectangle_equiv {
         private var _height = SmallNumber(wrappedValue: 1)
         private var _width = SmallNumber(wrappedValue: 1)
         var height: Int {
             get { return _height.wrappedValue }
             set { _height.wrappedValue = newValue }
         }
         var width: Int {
             get { return _width.wrappedValue }
             set { _width.wrappedValue = newValue }
         }
     }
  -> var unitRectangle_equiv = UnitRectangle_equiv()
  -> print(unitRectangle_equiv.height, unitRectangle_equiv.width)
  <- 1 1
  ```
-->

프로퍼티에 `= 1`과 같이 초기값을 작성하면,
이는 `init(wrappedValue:)` 초기화 호출로 변환됩니다.
`height`와 `width`를 래핑하는 `SmallNumber` 인스턴스는
`SmallNumber(wrappedValue: 1)`을 호출하여 생성됩니다.
이 이니셜라이저는 여기서 지정된 래핑 값을 사용하고,
최대값은 기본값인 12를 사용합니다.

사용자 지정 속성 뒤에 소괄호로 인자를 작성하면,
Swift는 해당 인자를 받는 이니셜라이저를 사용하여 래퍼를 설정합니다.
예를 들어 초기값과 최대값을 제공하면,
Swift는 `init(wrappedValue:maximum:)` 이니셜라이저를 사용합니다:

```swift
struct NarrowRectangle {
    @SmallNumber(wrappedValue: 2, maximum: 5) var height: Int
    @SmallNumber(wrappedValue: 3, maximum: 4) var width: Int
}

var narrowRectangle = NarrowRectangle()
print(narrowRectangle.height, narrowRectangle.width)
// Prints "2 3"

narrowRectangle.height = 100
narrowRectangle.width = 100
print(narrowRectangle.height, narrowRectangle.width)
// Prints "5 4"
```

<!--
  - test: `property-wrapper-init`

  ```swifttest
  -> struct NarrowRectangle {
  ->     @SmallNumber(wrappedValue: 2, maximum: 5) var height: Int
  ->     @SmallNumber(wrappedValue: 3, maximum: 4) var width: Int
  -> }

  -> var narrowRectangle = NarrowRectangle()
  -> print(narrowRectangle.height, narrowRectangle.width)
  <- 2 3

  -> narrowRectangle.height = 100
  -> narrowRectangle.width = 100
  -> print(narrowRectangle.height, narrowRectangle.width)
  <- 5 4
  ```
-->

<!--
  - test: `property-wrapper-init`

  ```swifttest
  -> struct NarrowRectangle_equiv {
         private var _height = SmallNumber(wrappedValue: 2, maximum: 5)
         private var _width = SmallNumber(wrappedValue: 3, maximum: 4)
         var height: Int {
             get { return _height.wrappedValue }
             set { _height.wrappedValue = newValue }
         }
         var width: Int {
             get { return _width.wrappedValue }
             set { _width.wrappedValue = newValue }
         }
     }
  -> var narrowRectangle_equiv = NarrowRectangle_equiv()
  -> print(narrowRectangle_equiv.height, narrowRectangle_equiv.width)
  <- 2 3
  -> narrowRectangle_equiv.height = 100
  -> narrowRectangle_equiv.width = 100
  -> print(narrowRectangle_equiv.height, narrowRectangle_equiv.width)
  <- 5 4
  ```
-->

`height`를 래핑하는 `SmallNumber` 인스턴스는
`SmallNumber(wrappedValue: 2, maximum: 5)`를 호출하여 생성되고,
`width`를 래핑하는 인스턴스는
`SmallNumber(wrappedValue: 3, maximum: 4)`를 호출하여 생성됩니다.

프로퍼티 래퍼에 인자를 포함하면,
래퍼의 초기 상태를 설정하거나
래퍼가 생성될 때 다른 옵션을 전달할 수 있습니다.
이 구문은 프로퍼티 래퍼를 사용하는 가장 일반적인 방법입니다.
필요한 인자를 속성에 제공하면,
해당 인자가 이니셜라이저에 전달됩니다.

프로퍼티 래퍼 인자를 포함하면,
할당을 사용하여 초기값을 지정할 수도 있습니다.
Swift는 할당을 `wrappedValue` 인자처럼 처리하고
저장한 인자를 수용하는 이니셜라이저를 사용합니다.
예를 들어:

```swift
struct MixedRectangle {
    @SmallNumber var height: Int = 1
    @SmallNumber(maximum: 9) var width: Int = 2
}

var mixedRectangle = MixedRectangle()
print(mixedRectangle.height)
// Prints "1"

mixedRectangle.height = 20
print(mixedRectangle.height)
// Prints "12"
```

<!--
  - test: `property-wrapper-mixed-init`

  ```swifttest
  -> struct MixedRectangle {
  ->     @SmallNumber var height: Int = 1
  ->     @SmallNumber(maximum: 9) var width: Int = 2
  -> }

  -> var mixedRectangle = MixedRectangle()
  -> print(mixedRectangle.height)
  <- 1

  -> mixedRectangle.height = 20
  -> print(mixedRectangle.height)
  <- 12
  ```
-->

`height`를 래핑하는 `SmallNumber` 인스턴스는
`SmallNumber(wrappedValue: 1)`을 호출하여 생성되며,
이때 최대값은 기본값인 12가 사용됩니다.
`width`를 래핑하는 인스턴스는
`SmallNumber(wrappedValue: 2, maximum: 9)`를 호출하여 생성됩니다.

### 프로퍼티 래퍼에서 값 투영 (Projecting a Value From a Property Wrapper)

래핑된 값 외에도
프로퍼티 래퍼는 *투영값(projected value)*을 정의하여
추가적인 기능을 제공할 수 있습니다 ---
예를 들어 데이터베이스 접근을 관리하는 프로퍼티 래퍼는
투영값으로 `flushDatabaseConnection()` 메서드를 노출할 수 있습니다.
투영값의 이름은 앞에 달러 표시(`$`)가 붙는 것을 제외하면
래핑된 값과 동일합니다.
코드에서 `$`로 시작하는 프로퍼티를 정의할 수 없기 때문에
투영값은 정의한 프로퍼티와 충돌하지 않습니다.

위의 `SmallNumber` 예시에서
프로퍼티에 너무 큰 수를 설정하려고 하면,
프로퍼티 래퍼가 값을 조정한 뒤 저장합니다.
아래의 코드는 새로운 값을 저장하기 전에
프로퍼티 래퍼가 값을 조정했는지 판단하기 위해
`SmallNumber` 구조체에 `projectedValue` 프로퍼티를 추가합니다.

```swift
@propertyWrapper
struct SmallNumber {
    private var number: Int
    private(set) var projectedValue: Bool

    var wrappedValue: Int {
        get { return number }
        set {
            if newValue > 12 {
                number = 12
                projectedValue = true
            } else {
                number = newValue
                projectedValue = false
            }
        }
    }

    init() {
        self.number = 0
        self.projectedValue = false
    }
}
struct SomeStructure {
    @SmallNumber var someNumber: Int
}
var someStructure = SomeStructure()

someStructure.someNumber = 4
print(someStructure.$someNumber)
// Prints "false"

someStructure.someNumber = 55
print(someStructure.$someNumber)
// Prints "true"
```

<!--
  - test: `small-number-wrapper-projection`

  ```swifttest
  -> @propertyWrapper
  -> struct SmallNumber {
         private var number: Int
         private(set) var projectedValue: Bool

         var wrappedValue: Int {
             get { return number }
             set {
                 if newValue > 12 {
                     number = 12
                     projectedValue = true
                 } else {
                     number = newValue
                     projectedValue = false
                 }
             }
         }

         init() {
             self.number = 0
             self.projectedValue = false
         }
     }
  -> struct SomeStructure {
  ->     @SmallNumber var someNumber: Int
  -> }
  -> var someStructure = SomeStructure()

  -> someStructure.someNumber = 4
  -> print(someStructure.$someNumber)
  <- false

  -> someStructure.someNumber = 55
  -> print(someStructure.$someNumber)
  <- true
  ```
-->

`someStructure.$someNumber`처럼 작성하면 해당 래퍼의 투영값에 접근할 수 있습니다.
4와 같은 작은 숫자를 저장한 후에,
`someStructure.$someValue`의 값은 `false`입니다.
그러나
55와 같은 큰 숫자를 저장하려고 한 경우에는
투영값은 `true`입니다.

프로퍼티 래퍼는 어떤 타입이든 투영값으로 반환할 수 있습니다.
이 예시에서
프로퍼티 래퍼는 숫자가 변경되었는지에 대한 ---
정보만 노출합니다 ---
그래서 투영값으로 Boolean 값을 노출합니다.
더 많은 정보를 제공해야 하는 래퍼의 경우
다른 타입의 인스턴스를 반환하거나,
`self`를 투영값으로 반환하여
래퍼 인스턴스 전체를 노출할 수 있습니다.

타입 내부의 코드에서 투영값에 접근할 때,
프로퍼티 getter나 인스턴스 메서드와
다른 프로퍼티를 접근할 때처럼
프로퍼티 이름 앞에 `self.`을 생략할 수 있습니다.
다음 예시의 코드는 `$height`와 `$width`로 `height`와 `width` 래퍼의
투영값을 참조합니다:

```swift
enum Size {
    case small, large
}

struct SizedRectangle {
    @SmallNumber var height: Int
    @SmallNumber var width: Int

    mutating func resize(to size: Size) -> Bool {
        switch size {
        case .small:
            height = 10
            width = 20
        case .large:
            height = 100
            width = 100
        }
        return $height || $width
    }
}
```

<!--
  - test: `small-number-wrapper-projection`

  ```swifttest
  -> enum Size {
         case small, large
     }

  -> struct SizedRectangle {
  ->     @SmallNumber var height: Int
  ->     @SmallNumber var width: Int

         mutating func resize(to size: Size) -> Bool {
             switch size {
                 case .small:
                     height = 10
                     width = 20
                 case .large:
                     height = 100
                     width = 100
             }
             return $height || $width
         }
     }
  >> var r = SizedRectangle()
  >> print(r.height, r.width)
  << 0 0
  >> var adj = r.resize(to: .large)
  >> print(adj, r.height, r.width)
  << true 12 12
  ```
-->

프로퍼티 래퍼 문법은 getter와 setter가 있는
프로퍼티를 위한 편의 문법(syntactic sugar)이기 때문에
`height`와 `width` 접근하는 방식은
다른 일반적인 프로퍼티에 접근하는 것과 동일하게 동작합니다.
예를 들어
`resize(to:)` 메서드 내의 코드는 `height`와 `width`에
프로퍼티 래퍼를 통해 접근합니다.
`resize(to: .large)`를 호출하면,
`.large`에 해당하는 스위치 케이스에서 사각형의 높이와 너비를 100으로 설정합니다.
래퍼는 프로퍼티의 값이
12를 초과하지 않도록 막고,
값이 변경되었다는 사실을 기록하기 위해
투영값을 `true`로 설정합니다.
`resize(to:)` 메서드의 마지막에 있는
반환 구문은 프로퍼티 래퍼가 `height`나 `width`를 변경했는지
판단하기 위해
`$height`와 `$width`를 체크합니다.

## 전역 변수와 지역 변수 (Global and Local Variables)

앞에서 설명한 연산 프로퍼티 및 프로퍼티 관찰자 기능은
*전역 변수(global variables)*와 *지역 변수(local variables)*에도 사용할 수 있습니다.
전역 변수는
함수, 메서드, 클로저, 타입 문맥의 외부에 정의된 변수입니다.
지역 변수는
함수, 메서드, 클로저 문맥 안에 정의된 변수입니다.

이전 챕터에서 본 전역과 지역 변수는
모두 *저장 변수(stored variables)*였습니다.
저장 프로퍼티처럼 저장된 변수는
특정 타입의 값을 저장할 수 있으며 그 값을 설정하고 가져오는 기능을 제공합니다.

그러나 전역이나 지역 범위에서도
*연산 변수(computed variables)*를 정의하거나
저장 변수에 관찰자를 정의할 수도 있습니다.
연산 변수는 값을 저장하는 대신 계산하여 제공하며,
연산 프로퍼티와 같은 방법으로 작성됩니다.

<!--
  - test: `computedVariables`

  ```swifttest
  -> var a: Int { get { return 42 } set { print("set a to \(newValue)") } }
  -> a = 37
  <- set a to 37
  -> print(a)
  <- 42
  ```
-->

<!--
  - test: `observersForStoredVariables`

  ```swifttest
  -> var a: Int = 0 { willSet { print("willSet") } didSet { print("didSet") } }
  -> a = 42
  <- willSet
  <- didSet
  ```
-->

> Note: 전역 상수와 변수는 항상 지연 방식으로 계산되며,
> 이는 [지연 저장 프로퍼티 (Lazy Stored Properties)](#지연-저장-프로퍼티-lazy-stored-properties)에서 설명과 유사합니다.
> 지연 저장 프로퍼티와 다르게
> 전역 상수와 변수는 `lazy` 수정자가 필요하지 않습니다.
>
> 지역 상수와 변수는 절대 지연 방식으로 계산되지 않습니다.

프로퍼티 래퍼는 지역 저장 변수에는 적용할 수 있지만,
전역 변수나 연산 변수에는 적용할 수 없습니다.
예를 들어
아래 코드에서는 `myNumber`는 프로퍼티 래퍼로 `SmallNumber`를 사용합니다.

```swift
func someFunction() {
    @SmallNumber var myNumber: Int = 0

    myNumber = 10
    // now myNumber is 10

    myNumber = 24
    // now myNumber is 12
}
```

<!--
  - test: `property-wrapper-init`

  ```swifttest
  -> func someFunction() {
  ->     @SmallNumber var myNumber: Int = 0

         myNumber = 10
         // now myNumber is 10
  >>     print(myNumber)

         myNumber = 24
         // now myNumber is 12
  >>     print(myNumber)
     }
  >> someFunction()
  << 10
  << 12
  ```
-->

프로퍼티에 `SmallNumber`를 적용할 때와 마찬가지로,
`myNumber` 값을 10으로 설정하는 것은 유효합니다.
프로퍼티 래퍼는 12보다 큰 값을 허용하지 않기 때문에,
`myNumber`를 24 대신 12로 설정합니다.

<!--
  The discussion of local variables with property wrappers
  has to come later, because we need to use init(wrappedValue:)
  to work around <rdar://problem/74616133>.
-->

<!--
  TODO: clarify what we mean by "global variables" here.
  According to [Contributor 6004], anything defined in a playground, REPL, or in main.swift
  is a local variable in top-level code, not a global variable.
-->

<!--
  TODO: this also makes it impossible (at present) to test the "always lazy" assertion.
-->

## 타입 프로퍼티 (Type Properties)

인스턴스 프로퍼티는 특정 타입의 인스턴스에 속하는 프로퍼티입니다.
타입의 새로운 인스턴스를 만들 때마다,
그 인스턴스는 다른 인스턴스와는 별도로 고유한 프로퍼티 값을 설정합니다.

해당 타입의 인스턴스가 아닌,
타입 자체에 속하는 프로퍼티를 정의할 수도 있습니다.
이러한 프로퍼티는 해당 타입의 인스턴스를 몇 개 만들든
오직 하나의 복사본만 존재합니다.
이러한 프로퍼티를 *타입 프로퍼티(type properties)*라고 합니다.

타입 프로퍼티는
모든 인스턴스에서 사용할 수 있는 상수 프로퍼티
(C에서 정적 상수)나
타입의 모든 인스턴스에 전역인 값을 저장하는 변수 프로퍼티
(C에서 static 변수)와 같은
특정 타입의 *모든* 인스턴스에 공통되는 값을 정의하는데 유용합니다.

저장 타입 프로퍼티는 변수나 상수로 선언할 수 있습니다.
타입 연산 프로퍼티는 인스턴스 연산 프로퍼티와 같은 방식으로
항상 변수 프로퍼티로 선언됩니다.

> Note: 저장 인스턴스 프로퍼티와 다르게
> 저장 타입 프로퍼티에는 반드시 기본값을 제공해야 합니다.
> 이는 타입 자체에는 저장 타입 프로퍼티를 초기화할 수 있는
> 이니셜라이저가 존재하지 않기 때문입니다.
>
> 저장 타입 프로퍼티는 처음 접근될 때 지연 초기화됩니다.
> 여러 스레드에서 동시에 접근할 때도
> 한 번만 초기화되는 것이 보장되고,
> `lazy` 수정자가 필요하지 않습니다.

### 타입 프로퍼티 문법 (Type Property Syntax)

C와 Objective-C에서 *전역* static 변수로
타입에 연결된 static 상수와 변수를 정의합니다.
그러나 Swift에서 타입 프로퍼티를 해당 타입의 정의 내부,
즉 타입의 중괄호 내에 작성하며,
각 타입 프로퍼티는 명시적으로 해당 타입의 범위에 속하도록 선언됩니다.

타입 프로퍼티는 `static` 키워드를 사용하여 정의합니다.
클래스 타입에서 타입 연산 프로퍼티를 정의할 때는,
`class` 키워드를 대신 사용하여
하위 클래스에서 해당 프로퍼티를 재정의 할 수 있습니다.
아래의 예시는 저장 타입 프로퍼티와 타입 연산 프로퍼티의 문법을 보여줍니다:

```swift
struct SomeStructure {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 1
    }
}
enum SomeEnumeration {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 6
    }
}
class SomeClass {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
        return 27
    }
    class var overrideableComputedTypeProperty: Int {
        return 107
    }
}
```

<!--
  - test: `typePropertySyntax`

  ```swifttest
  -> struct SomeStructure {
        static var storedTypeProperty = "Some value."
        static var computedTypeProperty: Int {
           return 1
        }
     }
  -> enum SomeEnumeration {
        static var storedTypeProperty = "Some value."
        static var computedTypeProperty: Int {
           return 6
        }
     }
  -> class SomeClass {
        static var storedTypeProperty = "Some value."
        static var computedTypeProperty: Int {
           return 27
        }
        class var overrideableComputedTypeProperty: Int {
           return 107
        }
     }
  ```
-->

<!--
  - test: `classComputedTypePropertiesAreOverrideable`

  ```swifttest
  -> class A { class var cp: String { return "A" } }
  -> class B: A { override class var cp: String { return "B" } }
  -> assert(A.cp == "A")
  -> assert(B.cp == "B")
  ```
-->

<!--
  - test: `staticComputedTypePropertiesAreFinal`

  ```swifttest
  -> class A { static var cp: String { return "A" } }
  -> class B: A { override static var cp: String { return "B" } }
  !$ error: cannot override static property
  !! class B: A { override static var cp: String { return "B" } }
  !!                                  ^
  !$ note: overridden declaration is here
  !! class A { static var cp: String { return "A" } }
  !!                      ^
  ```
-->

> Note: 위 예시의 타입 연산 프로퍼티는 읽기 전용 타입 연산 프로퍼티를 위한 것이지만,
> 인스턴스 연산 프로퍼티와 동일한 문법을 사용하여
> 읽기 쓰기 타입 연산 프로퍼티를 정의할 수 있습니다.

### 타입 프로퍼티 접근과 설정 (Querying and Setting Type Properties)

타입 프로퍼티는 인스턴스 프로퍼티처럼 점 표기법으로 접근하거나 설정합니다.
그러나 타입 프로퍼티는 해당 타입의 인스턴스가 아닌 *타입* 자체에 대해 접근하고 설정합니다.
예를 들어:

```swift
print(SomeStructure.storedTypeProperty)
// Prints "Some value."
SomeStructure.storedTypeProperty = "Another value."
print(SomeStructure.storedTypeProperty)
// Prints "Another value."
print(SomeEnumeration.computedTypeProperty)
// Prints "6"
print(SomeClass.computedTypeProperty)
// Prints "27"
```

<!--
  - test: `typePropertySyntax`

  ```swifttest
  -> print(SomeStructure.storedTypeProperty)
  <- Some value.
  -> SomeStructure.storedTypeProperty = "Another value."
  -> print(SomeStructure.storedTypeProperty)
  <- Another value.
  -> print(SomeEnumeration.computedTypeProperty)
  <- 6
  -> print(SomeClass.computedTypeProperty)
  <- 27
  ```
-->

다음 예시는 여러 오디오 채널에 대한 오디오 레벨 미터를 모델링하는
구조체의 일부분으로 두 개의 저장 타입 프로퍼티를 사용합니다.
각 채널은 `0`부터 `10`까지의 정수인 오디오 레벨을 가지고 있습니다.

아래 그림은 이러한 오디오 채널 중 두 개를 결합하여
스테레오 오디오 레벨 미터를 모델링하는 방법을 보여줍니다.
채널의 오디오 레벨이 `0`이면 해당 채널의 조명은 켜지지 않습니다.
오디오 레벨이 `10`이면 모든 채널의 조명은 켜집니다.
이 그림에서 왼쪽 채널은 현재 `9`의 레벨을 가지고,
오른쪽 채널은 현재 `7`의 레벨을 가집니다:

![Static Properties VUMeter](../.gitbook/assets/staticPropertiesVUMeter_2x~dark.png)

위에 설명한 오디오 채널은
`AudioChannel` 구조체의 인스턴스로 표현됩니다:

```swift
struct AudioChannel {
    static let thresholdLevel = 10
    static var maxInputLevelForAllChannels = 0
    var currentLevel: Int = 0 {
        didSet {
            if currentLevel > AudioChannel.thresholdLevel {
                // cap the new audio level to the threshold level
                currentLevel = AudioChannel.thresholdLevel
            }
            if currentLevel > AudioChannel.maxInputLevelForAllChannels {
                // store this as the new overall maximum input level
                AudioChannel.maxInputLevelForAllChannels = currentLevel
            }
        }
    }
}
```

<!--
  - test: `staticProperties`

  ```swifttest
  -> struct AudioChannel {
        static let thresholdLevel = 10
        static var maxInputLevelForAllChannels = 0
        var currentLevel: Int = 0 {
           didSet {
              if currentLevel > AudioChannel.thresholdLevel {
                 // cap the new audio level to the threshold level
                 currentLevel = AudioChannel.thresholdLevel
              }
              if currentLevel > AudioChannel.maxInputLevelForAllChannels {
                 // store this as the new overall maximum input level
                 AudioChannel.maxInputLevelForAllChannels = currentLevel
              }
           }
        }
     }
  ```
-->

`AudioChannel` 구조체는 해당 기능을 제공하기 위해 두 개의 저장 타입 프로퍼티를 정의합니다.
첫 번째 `thresholdLevel`은 오디오 레벨이 가질 수 있는 최대값을 정의합니다.
모든 `AudioChannel` 인스턴스에 대해 `10`으로 고정된 상수 값입니다.
오디오 신호가 `10`의 값보다 높게 오면,
아래에서 설명된 기준 값으로 변경합니다.

두 번째 타입 프로퍼티는
`maxInputLevelForAllChannels`라는 변수 저장 프로퍼티입니다.
이 프로퍼티는 *모든* `AudioChannel` 인스턴스 중에서
최대 입력값을 추적합니다.
초기값은 `0`부터 시작합니다.

`AudioChannel` 구조체는
채널의 현재 오디오 레벨을 `0`에서 `10`으로 표현하는
`currentLevel`이라는 저장 인스턴스 프로퍼티도 정의합니다.

`currentLevel` 프로퍼티는 `currentLevel`이 설정될 때마다
값을 체크하는 `didSet` 프로퍼티 관찰자를 가지고 있습니다.
이 관찰자는 다음 두 가지를 체크합니다:

- `currentLevel`의 새로운 값이 `thresholdLevel`에 허용된 값보다 크면,
  프로퍼티 관찰자는 `currentLevel`을 `thresholdLevel`로 강제 제한합니다.
- 제한된 `currentLevel`의 새로운 값이
  지금까지 *모든* `AudioChannel` 인스턴스가 받은 값 중 가장 크면,
  프로퍼티 관찰자는 새로운 `currentLevel` 값을
  `maxInputLevelForAllChannels` 타입 프로퍼티에 저장합니다.

> Note: 위 두 가지 체크사항 중 첫 번째 항목에서
> `didSet` 관찰자는 `currentLevel`을 다른 값으로 설정합니다.
> 그러나 이것이 관찰자를 다시 호출하진 않습니다.

스트레오 사운드 시스템의 오디오 레벨을 나타내기 위해
`leftChannel`과 `rightChannel`이라 하는
두 개의 새로운 오디오 채널을 생성하기 위해 `AudioChannel` 구조체를 사용할 수 있습니다:

```swift
var leftChannel = AudioChannel()
var rightChannel = AudioChannel()
```

<!--
  - test: `staticProperties`

  ```swifttest
  -> var leftChannel = AudioChannel()
  -> var rightChannel = AudioChannel()
  ```
-->

*왼쪽* 채널의 `currentLevel`을 `7`로 설정하면,
`maxInputLevelForAllChannels` 타입 프로퍼티가
`7`로 업데이트 된 것을 볼 수 있습니다:

```swift
leftChannel.currentLevel = 7
print(leftChannel.currentLevel)
// Prints "7"
print(AudioChannel.maxInputLevelForAllChannels)
// Prints "7"
```

<!--
  - test: `staticProperties`

  ```swifttest
  -> leftChannel.currentLevel = 7
  -> print(leftChannel.currentLevel)
  <- 7
  -> print(AudioChannel.maxInputLevelForAllChannels)
  <- 7
  ```
-->

*오른쪽* 채널의 `currentLevel`을 `11`로 설정하면,
오른쪽 채널의 `currentLevel` 프로퍼티가
`10`의 최대값으로 변경된 것을 볼 수 있고,
`maxInputLevelForAllChannels` 타입 프로퍼티가 `10`으로 업데이트 된 것을 볼 수 있습니다:

```swift
rightChannel.currentLevel = 11
print(rightChannel.currentLevel)
// Prints "10"
print(AudioChannel.maxInputLevelForAllChannels)
// Prints "10"
```

<!--
  - test: `staticProperties`

  ```swifttest
  -> rightChannel.currentLevel = 11
  -> print(rightChannel.currentLevel)
  <- 10
  -> print(AudioChannel.maxInputLevelForAllChannels)
  <- 10
  ```
-->

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->