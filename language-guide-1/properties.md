# 프로퍼티 \(Properties\)

_프로퍼티 \(Properties\)_ 는 값을 특정 클래스, 구조체, 또는 열거형을 연결합니다. 저장된 프로퍼티 \(Stored properties\)는 인스턴스의 일부로 상수와 변수 값을 저장하는 반면에 계산된 프로퍼티 \(computed properties\)는 값을 저장하는 대신에 계산합니다. 계산된 프로퍼티 \(Computed properties\)는 클래스, 구조체, 그리고 열거형에 의해 제공됩니다. 저장된 프로퍼티는 클래스와 구조체에 의해서만 제공됩니다.

저장된 프로퍼티와 계산된 프로퍼티는 대게 특정 타입의 인스턴스와 연결됩니다. 그러나 프로퍼티는 타입 자체와도 연결될 수 있습니다. 이러한 프로퍼티를 타입 프로퍼티라 합니다.

또한 프로퍼티 관찰자 \(property observers\)를 정의하여 프로퍼티의 값이 변경되는 것을 모니터링 하고 사용자 지정 동작에 응답할 수 있습니다. 프로퍼티 관찰자는 직접 정의한 저장된 프로퍼티와 하위 클래스가 수퍼 클래스에서 상속하는 프로퍼티에 추가될 수 있습니다.

프로퍼티 래퍼를 사용하여 여러 프로퍼티의 getter와 setter에서 코드를 재사용 할 수 있습니다.

## 저장된 프로퍼티 \(Stored Properties\)

가장 간단한 형식으로 저장된 프로퍼티는 특정 클래스 또는 구조체의 인스턴스의 부분으로 저장된 상수 또는 변수 입니다. 저장된 프로퍼티는 _저장된 프로퍼티 변수 \(variable stored properties\)_ \(`var` 키워드 사용\) 또는 _저장된 프로퍼티 상수 \(constant stored properties\)_ \(`let` 키워드 사용\)로 쓸 수 있습니다.

[기본 프로퍼티 값 \(Default Property Values\)](initialization.md#default-property-values) 에 설명한 대로 정의의 부분으로 저장된 프로퍼티에 대한 기본값을 제공할 수 있습니다. 초기화 중에 저장된 프로퍼티에 초기값을 설정하고 수정할 수도 있습니다. [초기화 중 프로퍼티 상수 할당 \(Assigning Constant Properties During Initialization\)](initialization.md#assigning-constant-properties-during-initialization) 에서 설명한 대로 상수 저장 프로퍼티에도 마찬가지 입니다.

아래 예제는 `FixedLengthRange` 라는 구조체를 정의합니다. 이 구조체는 범위 길이가 생성된 후에 변경할 수 없는 정수 범위를 나타냅니다:

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

`FixedLengthRange` 의 인스턴스는 `firstValue` 라는 저장된 프로퍼티 변수가 있으며 `length` 라는 저장된 프로퍼티 상수가 있습니다. 위의 예제에서 `length` 는 새 범위가 생성될 때 초기화되며 프로퍼티 상수 이므로 변경할 수 없습니다.

### 상수 구조체 인스턴스 저장된 프로퍼티 \(Stored Properties of Constant Structure Instances\)

구조체의 인스턴스를 생성하고 상수로 할당하면 프로퍼티 변수로 선언되어 있어도 인스턴스의 프로퍼티를 수정할 수 없습니다:

```swift
let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
// this range represents integer values 0, 1, 2, and 3
rangeOfFourItems.firstValue = 6
// this will report an error, even though firstValue is a variable property
```

`rangeOfFourItems` 는 `let` 키워드를 사용하여 상수로 선언되어 있기 때문에 `firstValue` 가 프로퍼티 변수 이지만 `firstValue` 프로퍼티를 변경할 수 없습니다.

이러한 동작은 구조체가 _값 타입_ 이기 때문입니다. 값 타입의 인스턴스가 상수로 선언하면 인스턴스의 모든 프로퍼티에 대해 수정할 수 없습니다.

클래스는 _참조 타입_ 이므로 다르게 동작합니다. 참조 타입의 인스턴스를 상수로 할당하면 인스턴스의 프로퍼티 변수는 변경할 수 있습니다.

### 지연 저장된 프로퍼티 \(Lazy Stored Properties\)

_지연 저장된 프로퍼티 \(lazy stored property\)_ 는 처음 사용될 때까지 초기값은 계산되지 않는 프로퍼티 입니다. 지연 저장된 프로퍼티는 선언 전에 `lazy` 수정자를 붙여 나타냅니다.

> NOTE  
> 인스턴스 초기화가 완료된 후에도 초기값이 없을 수 있으므로 지연 프로퍼티는 `var` 키워드를 사용하여 변수로 선언해야 합니다. 프로퍼티 상수는 초기화가 완료되기 _전에_ 항상 값을 가지고 있어야 하므로 lazy로 선언할 수 없습니다.

지연 프로퍼티는 인스턴스의 초기화가 완료될 때까지 값을 알 수 없는 외부 요인에 인해 초기값이 달라질 때 유용합니다. 지연 프로퍼티는 프로퍼티의 초기값으로 필요할 때까지 수행하면 안되는 복잡하거나 계산 비용이 많이 드는 경우에도 유용합니다.

아래의 예제는 복잡한 클래스에 불필요한 초기화를 피하기 위해 지연 저장된 프로퍼티를 사용합니다. 이 예제는 `DataImporter` 와 `DataManager` 라는 2개의 클래스를 정의합니다:

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

`DataManager` 클래스는 `String` 값의 빈 배열로 초기화 되는 `data` 라는 저장된 프로퍼티를 가지고 있습니다. 나머지 기능이 보이진 않지만 `DataManager` 클래스의 목적은 `String` 데이터의 배열을 관리하고 접근을 제공합니다.

`DataManager` 클래스의 기능적 부분은 파일로 부터 데이터를 가져올 수 있습니다. 이 기능은 `DataImporter` 클래스로 부터 제공되고 초기화 하는데 시간이 많이 걸린다고 가정합니다. `DataImporter` 인스턴스가 초기화 될 때 `DataImporter` 인스턴스는 파일을 열고 메모리로 내용을 읽어야 하기 때문일 수 있습니다.

`DataManager` 인스턴스는 파일로 부터 데이터를 가져올 필요 없이 데이터를 관리할 수 있으므로 `DataManager` 가 생성될 때 새로운 `DataImporter` 인스턴스를 생성할 필요가 없습니다. 대신에 `DataImporter` 인스턴스를 처음 사용하는 경우에 생성하는 것이 더 합리적입니다.

`lazy` 수식어가 붙어 있으므로 `importer` 프로퍼티의 `DataImporter` 인스턴스는 `filename` 프로퍼티를 조회할 때처럼 `importer` 프로퍼티에 처음 접근될 때 생성됩니다:

```swift
print(manager.importer.filename)
// the DataImporter instance for the importer property has now been created
// Prints "data.txt"
```

> NOTE  
> `lazy` 수식어가 표시된 프로퍼티는 여러 쓰레드에서 동시에 접근되고 프로퍼티가 아직 초기화되지 않은 경우 프로퍼티가 한번만 초기화 된다는 보장이 없습니다.

### 저장된 프로퍼티와 인스턴스 변수 \(Stored Properties and Instance Variables\)

Objective-C에 경험이 있다면 클래스 인스턴스의 일부로 값과 참조를 저장하는 2가지 방법을 제공한다는 것을 알 수 있습니다. 프로퍼티 외에도 프로퍼티에 저장된 값을 백업 저장소로 인스턴스 변수를 사용할 수 있습니다.

Swift는 단일 프로퍼티 선언으로 개념을 통합합니다. Swift 프로퍼티는 해당 인스턴스 변수가 없으며 프로퍼티에 백업 저장소에 직접적으로 접근할 수 없습니다. 이 접근 방식은 다른 컨텍스트에서 값에 접근하는 것에 대한 혼동을 피할 수 있으며 프로퍼티 선언을 하나의 명확한 구문으로 단순화 합니다. 이름, 타입, 그리고 메모리 관리, 특성을 포함한 프로퍼티에 대한 모든 정보는 타입의 정의에 부분으로 한곳에서 정의됩니다.

## 계산된 프로퍼티 \(Computed Properties\)

저장된 프로퍼티 외에도 클래스, 구조체, 그리고 열거형은 값을 실질적으로 저장하지 않는 _계산된 프로퍼티 \(computed properties\)_ 를 정의할 수 있습니다. 대신 다른 프로퍼티와 값을 간접적으로 조회하고 설정하는 getter와 옵셔널 setter를 제공합니다.

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
square.center = Point(x: 15.0, y: 15.0)
print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
// Prints "square.origin is now at (10.0, 10.0)"
```

이 예제는 기하학적 모양과 작업을 위해 3개의 구조체를 정의합니다:

* `Point` 는 x, y 좌표의 위치를 캡슐화 합니다.
* `Size` 는 `width` 와 `height` 를 캡슐화 합니다.
* `Rect` 는 원점과 크기로 사각형을 정의합니다.

`Rect` 구조체는 `center` 라는 계산된 프로퍼티를 제공합니다. `Rect` 에 현재 중앙 위치는 항상 `origin` 과 `size` 로 계산될 수 있고 명시적으로 `Point` 값으로 저장할 필요가 없습니다. 대신에 `Rect` 는 `center` 라는 계산된 변수를 위한 getter와 setter를 정의하고 실제 저장된 프로퍼티 처럼 사각형의 `center` 를 동작하도록 합니다.

위의 예제는 `square` 라는 새로운 `Rect` 변수를 생성합니다. `square` 변수는 `(0, 0)` 의 원점으로 너비와 높이를 `10` 으로 초기화 됩니다. 이 사각형은 아래의 다이어그램에서 파란색 사각형으로 표현됩니다.

`square` 변수의 `center` 프로퍼티는 점 구문 \(`square.center`\)으로 접근되고 `center` 에 대한 getter를 호출하여 현재 프로퍼티 값을 조회합니다. 존재하는 값을 반환하기 보다 getter는 실질적으로 사각형의 중심을 나타내는 새로운 `Point` 를 계산하고 반환합니다. 위에 볼 수 있듯이 getter는 `(5, 5)` 의 중심점을 반환합니다.

아래 다이어그램의 오렌지색 사각형처럼 `center` 프로퍼티는 `(15, 15)` 의 새로운 값으로 설정되어 새로운 위치로 사각형이 위와 우측으로 이동합니다. `center` 프로퍼티 설정은 `center` 의 setter를 호출하고 `origin` 프로퍼티에 저장된 `x` 와 `y` 값을 변경하고 사각형을 새로운 위치로 이동합니다.

![Computed Properties](../.gitbook/assets/10_computedproperties_2x.png)

### 짧은 Setter 선언 \(Shorthand Setter Declaration\)

계산된 프로퍼티의 setter가 새로운 값을 설정하는데 이름을 정의하지 않았다면 `newValue` 라는 기본 이름이 사용됩니다. 다음은 이러한 짧은 표현의 이점을 가지는 `Rect` 구조체를 나타냅니다:

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

### 짧은 Getter 선언 \(Shorthand Getter Declaration\)

getter의 전체 바디가 단일 표현식이라면 getter는 암시적으로 표현식을 반환합니다. 다음은 짧은 getter와 setter의 이점을 가진 `Rect` 구조체를 나타냅니다:

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

getter에서 생략한 `return` 은 [암시적 반환의 함수 \(Functions With an Implicit Return\)](functions.md#functions) 에서 설명 했듯이 함수의 `return` 생략 규칙과 동일합니다.

### 읽기전용 계산된 프로퍼티 \(Read-Only Computed Properties\)

setter가 없고 getter만 있는 계산된 프로퍼티는 _읽기전용 계산된 프로퍼티 \(read-only computed property\)_ 라고 합니다. 읽기전용 계산된 프로퍼티는 항상 값을 반환하고 점 구문으로 접근할 수 있지만 다른 값을 설정할 수 없습니다.

> NOTE  
> 값이 고정되어 있지 않기 때문에 읽기전용 계산된 프로퍼티를 포함하여 계산된 프로퍼티는 `var` 키워드를 포함하는 프로퍼티 변수로 선언되어야 합니다. `let` 키워드는 인스턴스 초기화의 부분으로 한번 설정되면 값을 변경할 수 없음을 나타내기 위해 오직 프로퍼티 상수에서만 사용됩니다.

`get` 키워드와 그것의 중괄호를 삭제하고 읽기전용 계산된 프로퍼티를 간편하게 선언할 수 있습니다:

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

이 예제는 `width`, `height`, 그리고 `depth` 프로퍼티를 가지는 3D 사각형 박스를 나타내는 `Cuboid` 라는 새로운 구조체를 정의합니다. 이 구조체는 직육면체의 현재 부피를 계산하고 반환하는 `volume` 이라는 읽기전용 계산된 프로퍼티를 가지고 있습니다. 특정 `volume` 값에 `width`, `height`, 그리고 `depth` 의 어떤값을 사용하여야 하는지 모호하므로 `volume` 을 설정하는 것은 의미가 없습니다. 그럼에도 불구하고 현재 계산된 부피를 알 수 있도록 외부에 읽기전용 계산된 프로퍼티로 제공되는 `Cuboid` 는 유용합니다.

## 프로퍼티 관찰자 \(Property Observers\)

프로퍼티 관찰자 \(Property observers\)는 프로퍼티의 값이 변경되는지 관찰하고 응답합니다. 프로퍼티 관찰자는 프로퍼티의 현재 값이 새로운 값과 같더라도 프로퍼티의 값이 설정될 때 호출됩니다.

아래의 위치에 프로퍼티 관찰자를 추가할 수 있습니다:

* 정의한 저장된 프로퍼티
* 상속한 저장된 프로퍼티
* 상속한 계산된 프로퍼티

상속된 프로퍼티의 경우 하위 클래스의 프로퍼티를 재정의하여 프로퍼티 관찰자를 추가합니다. 정의한 계산된 프로퍼티의 경우 관찰자를 생성하는 대신에 프로퍼티의 setter를 이용하여 값 변경을 관찰하고 응답합니다. 재정의 프로퍼티에 대한 자세한 내용은 [재정의 \(Overriding\)](inheritance.md#overriding) 을 참고 바랍니다.

프로퍼티에 관찰자를 정의하는 방법은 2가지 선택사항을 가지며 둘다 정의할 수도 있습니다:

* `willSet` 은 값이 저장되기 직전에 호출됩니다.
* `didSet` 은 새로운 값이 저장되자마자 호출됩니다.

`willSet` 관찰자를 구현한다면 상수 파라미터로 새로운 프로퍼티 값이 전달됩니다. `willSet` 구현의 일부로 이 파라미터에 특정 이름을 가질 수 있습니다. 파라미터 명과 구현 내에 소괄호를 작성하지 않으면 파라미터는 `newValue` 의 기본 파라미터 명으로 만들어 질 수 있습니다.

유사하게 `didSet` 관찰자를 구현한다면 예전 프로퍼티 값을 포함한 상수 파라미터가 전달됩니다. 파라미터 명을 사용하거나 `oldValue` 인 기본 파라미터 명을 사용할 수 있습니다. `didSet` 관찰자 내의 프로퍼티에 값을 할당한다면 새로운 값으로 방금 설정한 값을 대체합니다.

> NOTE  
> 수퍼 클래스 프로퍼티의 `willSet` 과 `didSet` 관찰자는 수퍼 클래스 초기화가 호출된 후 하위 클래스 초기화에서 프로퍼티가 설정될 때 호출됩니다. 수퍼 클래스가 초기화 호출되기 전에 클래스 자체 프로퍼티를 설정하는 동안에는 호출되지 않습니다.
>
> 초기화 위임에 대한 자세한 내용은 [값 타입에 대한 초기화 구문 위임 \(Initializer Delegation for Value Types\)](initialization.md#initializer-delegation-for-value-types) 와 [클래스 타입에 대한 초기화 구 위임 \(Initializer Delegation for Class Types\)](initialization.md#initializer-delegation-for-class-types) 를 참고 바랍니다.

다음은 `willSet` 과 `didSet` 동작에 대한 예입니다. 아래 예제는 사람이 걷는동안의 걸음 수를 측정하는 `StepCounter` 라는 새로운 클래스를 정의합니다. 이 클래스는 만보계 또는 다른 걸음 측정기의 입력 데이터와 함께 사용하여 일상생활에서 사람의 운동 루틴을 추적할 수 있습니다.

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

`StepCounter` 클래스는 `Int` 타입의 `totalSteps` 프로퍼티를 선언합니다. 이것은 `willSet` 과 `didSet` 관찰자를 가진 저장된 프로퍼티 입니다.

`totalSteps` 의 `willSet` 과 `didSet` 관찰자는 새로운 값이 프로퍼티에 할당될 때마다 호출됩니다. 새로운 값이 현재값과 같더라도 항상 호출됩니다.

이 예제의 `willSet` 관찰자는 새로운 값을 위한 `newTotalSteps` 의 사용자 파라미터 명을 사용합니다. 이 예제는 간단하게 설정된 값을 출력합니다.

`didSet` 관찰자는 `totalSteps` 값이 업데이트 되고난 후에 호출됩니다. 이것은 오래된 값에 대해 `totalSteps` 의 새로운 값과 비교합니다. 걸음수가 증가했다면 걸음수가 얼마나 증가하였는지 출력합니다. `didSet` 관찰자는 오래된 값에 대한 사용자 파라미터 명을 제공하지 않고 대신에 `oldValue` 의 기본 이름을 사용합니다.

> NOTE  
> 관찰자를 가진 프로퍼티를 in-out 파라미터로 함수에 전달하면 `willSet` 과 `didSet` 관찰자는 항상 호출됩니다. 이것은 in-out 파라미터에 대한 copy-in-copy-out 메모리 모델 때문에 그렇습니다. 값은 함수 끝에서 프로퍼티에 항상 다시 작성됩니다. in-out 파라미터에 대한 자세한 내용은 [In-Out 파라미터 \(In-Out Parameters\)](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#ID545) 를 참고 바랍니다.

## 프로퍼티 래퍼 \(Property Wrappers\)

프로퍼티 래퍼 \(property wrapper\)는 프로퍼티가 저장되는 방법을 관리하는 코드와 프로퍼티를 정의하는 코드 사이에 분리 계층을 추가합니다. 예를 들어 쓰레드 안정성 검사를 제공하거나 기본 데이터를 데이터베이스에 저장하는 프로퍼티가 있는 경우 모든 프로퍼티에 해당 코드를 작성해야 합니다. 프로퍼티 래퍼를 사용할 때 래퍼를 정의할 때 관리 코드를 한번 작성한 다음 여러 프로퍼티에 적용하여 해당 관리 코드를 재사용합니다.

프로퍼티 래퍼를 정의하기 위해 `wrappedValue` 프로퍼티를 정의한 구조체, 열거형 또는 클래스를 만듭니다. 아래 코드에서 `TwelveOrLess` 구조체는 래핑하는 값이 항상 12와 같거나 더 작은 숫자가 포함됩니다. 더 큰 숫자를 저장하도록 하면 12가 저장됩니다.

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

setter는 새로운 값이 12이하라는 것을 보장하고 getter는 저장된 값을 반환합니다.

> NOTE  
> 위의 예제에서 `number` 선언부는 `TwelveOrLess` 의 구현에서만 `number` 가 사용될 수 있도록 `private` 로 변수를 표기합니다. 다른곳에서 작성된 코드는 `wrappedValue` 를 위한 getter와 setter를 사용하여 값에 접근하고 직접적으로 `number` 를 사용할 수 없습니다. `private` 에 대한 정보는 [접근 제어 \(Access Control\)](access-control.md) 를 참고 바랍니다.

속성으로 프로퍼티 전에 래퍼의 이름을 작성하여 프로퍼티에 래퍼를 적용합니다. 다음은 항상 12 이하인지 확인하기 위해 `TwelveOrLess` 프로퍼티 래퍼를 사용하여 사각형을 저장하는 구조체입니다:

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

`height` 와 `width` 프로퍼티는 `TwelveOrLess.number` 를 0으로 설정하는 `TwelveOrLess` 의 정의로 부터 초기값을 얻습니다. `rectangle.height` 에 숫자 10을 저장하는 것은 그 숫자가 작은 숫자이기 때문에 성공적으로 저장합니다. 24를 저장하려고 하면 24는 프로퍼티 setter의 규칙에 비해 큰 숫자이므로 실질적으로 12가 저장됩니다.

프로퍼티에 래퍼를 적용할 때 컴파일러는 래퍼를 위한 저장소를 제공하는 코드와 래퍼를 통해 프로퍼티에 접근하는 코드를 합성합니다 \(프로퍼티 래퍼는 래핑된 값을 저장하는 역할을 하므로 이에 대한 합성 코드는 없습니다\). 특수 속성 구문의 이점을 사용하지 않고도 프로퍼티 래퍼의 기능을 사용하는 코드를 작성할 수 있습니다. 예를 들어 다음은 속성으로 `@TwelveOrLess` 를 작성하는 대신에 `TwelveOrLess`구조체에 명시적으로 프로퍼티를 래핑하는 이전 코드 목록으로 부터의 `SmallRectangle` 입니다:

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

`_height` 와 `_width` 프로퍼티는 프로퍼티 래퍼의 인스턴스인 `TwelveOrLess` 를 저장합니다. `height` 와 `width` 에 대한 getter와 setter는 `wrappedValue` 프로퍼티에 접근합니다.

### 래핑된 프로퍼티를 위한 초기값 설정 \(Setting Initial Values for Wrapped Properties\)

위 예제의 코드는 `TwelveOrLess` 에 정의한 `number` 초기값으로 래핑된 프로퍼티에 대한 초기값을 설정합니다. 이 프로퍼티 래퍼를 사용한 코드는 `TwelveOrLess` 로 래핑된 프로퍼티에 다른 초기값을 지정할 수 없습니다. 예를 들어 `SmallRectangle` 에 정의에 `height` 와 `width` 초기값을 지정할 수 없습니다. 초기값 설정을 지원하거나 다른 커스터마이징을 지원하려면 프로퍼티 래퍼는 초기화를 추가해줘야 합니다. 다음은 래핑과 최대값을 설정하는 초기화를 정의하는 `SmallNumber` 라는 확장된 `TwelveOrLess` 를 나타냅니다:

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

`SmallNumber` 의 정의는 `init()`, `init(wrappedValue:)`, `init(wrappedValue:maximum:)` 의 3개의 초기화를 포함합니다. 아래는 래핑값과 최대값 설정을 사용하는 예입니다. 자세한 내용은 [초기화 \(Initialization\)](initialization.md) 를 참고 바랍니다.

프로퍼티에 래퍼를 적용하지 않고 초기값을 지정하지 않으면 Swift는 래퍼를 설정하기 위해 `init()` 을 사용합니다. 예를 들어:

```swift
struct ZeroRectangle {
    @SmallNumber var height: Int
    @SmallNumber var width: Int
}

var zeroRectangle = ZeroRectangle()
print(zeroRectangle.height, zeroRectangle.width)
// Prints "0 0"
```

`height` 와 `width` 를 래핑한 `SmallNumber` 의 인스턴스는 `SmallNumber()` 호출로 생성됩니다. 초기화 내부의 코드는 기본값 0과 12을 사용하여 초기 래핑값과 초기 최대값을 설정합니다. 프로퍼티 래퍼는 `SmallRectangle` 에 `TwelveOrLess` 를 사용한 이전 예제처럼 모든 초기값을 제공합니다. 예제와 반대로 `SmallNumber` 는 프로퍼티 선언의 일부분으로 초기값 작성을 제공합니다.

프로퍼티에 초기값을 지정할 때 Swift는 래퍼를 설정하기 위해 `init(wrappedValue:)` 를 사용합니다. 예를 들어:

```swift
struct UnitRectangle {
    @SmallNumber var height: Int = 1
    @SmallNumber var width: Int = 1
}

var unitRectangle = UnitRectangle()
print(unitRectangle.height, unitRectangle.width)
// Prints "1 1"
```

래핑한 프로퍼티에 `= 1` 을 작성하면 `init(wrappedValue:)` 초기화 호출에 전달됩니다. `height` 와 `width` 를 래핑한 `SmallNumber` 의 인스턴스는 `SmallNumber(wrappedValue: 1)` 호출로 생성됩니다. 이 초기화는 래핑된 값을 사용하고 기본 최대값으로 12를 사용합니다.

사용자 속성 후에 소괄호 안에 인자를 작성하면 Swift는 래퍼를 설정하기 위한 인자를 받을 수 있는 초기화를 사용합니다. 예를 들어 초기값과 최대값을 제공하면 Swift는 `init(wrappedValue:maximum:)` 초기화를 사용합니다:

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

`height` 를 래핑한 `SmallNumber` 의 인스턴스는 `SmallNumber(wrappedValue: 2, maximum: 5)` 를 호출하여 생성되고 `width` 를 래핑한 인스턴스는 `SmallNumber(wrappedValue: 3, maximum: 4)` 를 호출하여 생성됩니다.

프로퍼티 래퍼에 인자를 포함하여 래퍼에 초기상태를 설정하거나 래퍼가 생성될 때 다른 옵션을 전달할 수 있습니다. 이 구문은 프로퍼티 래퍼를 사용하는 가장 일반적인 방법입니다. 속성에 필요한 무슨 인자도 제공할 수 있으며 초기화 구문에 전달됩니다.

프로퍼티 래퍼 인자를 포함하면 할당을 사용하여 초기값을 지정할 수도 있습니다. Swift는 할당을 `wrappedValue` 인자처럼 취급하고 이 인자를 받을 수 있는 초기화를 사용합니다. 예를 들어:

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

`height` 를 래핑한 `SmallNumber` 의 인스턴스는 기본 최대값은 12를 사용하는 `SmallNumber(wrappedValue: 1)` 을 호출하여 생성됩니다. `width` 를 래핑한 이 인스턴스는 `SmallNumber(wrappedValue: 2, maximum: 9)` 를 호출하여 생성됩니다.

### 프로퍼티 래퍼에서 값 투영 \(Projecting a Value From a Property Wrapper\)

래핑된 값 외에도 프로퍼티 래퍼는 _투영된 값 \(projected value\)_ 정의에 의해 추가적인 기능을 노출할 수 있습니다. 예를 들어 데이터베이트 접근을 관리하는 프로퍼티 래퍼는 투영된 값으로 `flushDatabaseConnection()` 메서드를 노출할 수 있습니다. 투영된 값의 이름은 앞에 달러 표시 \(`$`\)가 붙는 것을 제외하면 래핑된 값과 동일합니다. 코드에서 `$` 로 시작하는 프로퍼티를 정의할 수 없기 때문에 투영된 값은 정의한 프로퍼티를 절대 방해하지 않습니다.

위의 `SmallNumber` 예제에서 프로퍼티에 큰 숫자로 설정하려고 하면 프로퍼티 래퍼가 이전에 저장한 숫자로 변경합니다. 아래의 코드는 새로운 값을 저장하기 전에 프로퍼티 래퍼가 프로퍼티에 새로운 값을 변경하는지 판단하기 위해 `SmallNumber` 구조체에 `projectedValue` 프로퍼티를 추가합니다.

```swift
@propertyWrapper
struct SmallNumber {
    private var number = 0
    var projectedValue = false
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

`someStructure.$someNumber` 는 래퍼의 투영된 값에 접근합니다. 4와 같은 작은 숫자를 저장한 후에 `someStructure.$someValue` 의 값은 `false` 입니다. 그러나 55와 같은 큰 숫자를 저장한 후에 투영된 값은 `true` 입니다.

프로퍼티 래퍼는 투영된 값으로 어떤 타입의 값도 반환할 수 있습니다. 이 예제에서 프로퍼티 래퍼는 숫자가 변경되었는지에 대한 정보만 노출합니다. 그래서 투영된 값으로 부울 값을 노출합니다. 더 많은 정보의 노출이 필요한 래퍼는 다른 데이터 타입의 인스턴스를 반환하거나 투영된 값으로 래퍼의 인스턴스를 노출하기 위해 `self` 를 반환할 수 있습니다.

타입의 부분으로 코드로 부터 투영된 값에 접근할 때 프로퍼티 getter 나 인스턴스 메서드 처럼 다른 프로퍼티에 접근하듯이 프로퍼티 이름 전에 `self.` 을 생략할 수 있습니다. 다음 예제의 코드는 `$height` 와 `$width` 로 `height` 와 `width` 래퍼의 투영된 값을 참조합니다:

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

프로퍼티 래퍼 구문은 getter와 setter가 있는 프로퍼티의 짧은 구문이기 때문에 `height` 와 `width` 접근은 다른 프로퍼티 접근과 동일한 동작입니다. 예를 들어 `resize(to:)` 에 코드는 프로퍼티 래퍼를 사용하는 `height` 와 `width` 에 접근합니다. `resize(to: .large)` 를 호출하면 `.large` 스위치 케이스가 사각형의 높이와 너비를 100으로 설정합니다. 래퍼는 프로퍼티의 값이 12보다 더 큰 값으로 되는 것을 막고 값이 변경되었다는 사실을 기록하기 위해 투영된 값을 `true` 로 설정합니다. `resize(to:)` 끝에서 반환 구문은 프로퍼티 래퍼가 `height` 또는 `width` 를 변경했는지 판단하기 위해 `$height` 와 `$width` 를 체크합니다.

## 전역과 지역 변수 \(Global and Local Variables\)

프로퍼티를 계산하고 관찰하기 위해 위에서 설명한 기능은 _전역 변수 \(global variables\)_ 와 _지역 변수 \(local variables\)_ 에도 사용할 수 있습니다. 전역 변수는 함수, 메서드, 클로저, 또는 타입 컨텍스트의 외부에 정의된 변수입니다. 지역 변수는 함수, 메서드, 또는 클로저 컨텍스트 내에 정의된 변수입니다.

이전 챕터에서 본 전역과 지역 변수는 모두 _저장된 변수 \(stored variables\)_ 입니다. 저장된 프로퍼티처럼 저장된 변수는 타입의 값을 위한 저장소를 제공하고 값을 설정하고 조회하는 것을 허락합니다.

그러나 전역 또는 지역 범위로 _계산된 변수 \(computed variables\)_ 와 저장된 변수를 위한 관찰자를 정의할 수도 있습니다. 계산된 변수는 값을 저장하기 보다 값을 계산하고 계산된 프로퍼티와 같은 방법으로 작성됩니다.

> NOTE  
> 전역 상수와 변수는 [지연 저장된 프로퍼티 \(Lazy Stored Properties\)](properties.md#lazy-stored-properties) 와 유사한 방법으로 항상 느리게 계산됩니다. 지연 저장된 프로퍼티와 다르게 전역 상수와 변수는 `lazy` 수식어가 필요하지 않습니다.
>
> 지역 상수와 변수는 절대 느리게 계산되지 않습니다.

계산된 지역 변수에 프로퍼티 래퍼를 적용할 수 있지만 전역 변수 또는 계산된 변수에는 적용할 수 없습니다. 예를 들어 아래 코드에서 `myNumber` 는 프로퍼티 래퍼로 `SmallNumber` 를 사용합니다.

```swift
func someFunction() {
    @SmallNumber var myNumber: Int = 0

    myNumber = 10
    // now myNumber is 10

    myNumber = 24
    // now myNumber is 12
}
```

프로퍼티에 `SmallNumber` 를 적용할 때와 마찬가지로 `myNumber` 값을 10으로 설정하는 것이 유효합니다. 프로퍼티 래퍼는 12보다 큰 값을 허용하지 않기 때문에 `myNumber` 를 24 대신 12로 설정합니다.

## 타입 프로퍼티 \(Type Properties\)

인스턴스 프로퍼티는 특정 타입의 인스턴스에 속하는 프로퍼티 입니다. 타입의 새로운 인스턴스를 만들 때마다 다른 인스턴스와는 별도로 고유한 프로퍼티 값을 설정합니다.

또한 해당 타입의 인스턴스가 아닌 타입 자체에 속하는 프로퍼티를 정의할 수도 있습니다. 생성하는 해당 타입의 인스턴스 수에 관계없이 이러한 프로퍼티의 복사본은 하나만 있습니다. 이런 프로퍼티의 종류를 _타입 프로퍼티 \(type properties\)_ 라고 합니다.

타입 프로퍼티는 C에서 static 상수 처럼 모든 인스턴스에서 사용할 수 있는 프로퍼티 상수나 C에서 static 변수 처럼 타입에 모든 인스턴스에 전역인 값을 저장하는 프로퍼티 변수와 같은 특정 타입에 모든 인스턴스에 보편적인 값을 정의하는데 유용합니다.

저장된 타입 프로퍼티는 변수 또는 상수 일 수 있습니다. 계산된 타입 프로퍼티는 계산된 인스턴스 프로퍼와 같은 방식으로 항상 프로퍼티 변수로 선언됩니다.

> NOTE  
> 저장된 인스턴스 프로퍼티와 다르게 저장된 타입 프로퍼티에는 기본값을 항상 주어야 합니다. 이는 초기화 시 저장된 타입 프로퍼티에 값을 할당할 수 있는 초기화를 가지고 있지 않기 때문입니다.
>
> 저장된 타입 프로퍼티는 처음 접근될 때 느리게 초기화 됩니다. 여러 쓰레드가 동시에 접근할 때도 한번만 초기화 되도록 보장하고 `lazy` 수식어가 필요하지 않습니다.

### 타입 프로퍼티 구문 \(Type Property Syntax\)

C와 Objective-C에서 _전역_ static 변수로 타입과 연관된 static 상수와 변수를 정의합니다. 그러나 Swift에서 타입 프로퍼티는 타입의 외부 중괄호 내에 타입의 정의 부분으로 작성되고 각 타입 프로퍼티는 지원되는 타입으로 명시적으로 범위가 지정됩니다.

`static` 키워드로 타입 프로퍼티를 정의합니다. 클래스 타입의 계산된 타입 프로퍼티의 경우 `class` 키워드를 대신 사용하여 하위 클래스에서 상위 클래스의 구현을 재정의 할 수 있습니다. 아래의 예제는 저장된 타입 프로퍼티와 계산된 타입 프로퍼티 구문을 보여줍니다:

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

> NOTE  
> 위 예제의 계산된 타입 프로퍼티는 읽기전용 계산된 타입 프로퍼티를 위한 것이지만 계산된 인스턴스 프로퍼티와 동일한 구문을 사용하여 읽기 쓰기 계산된 타입 프로퍼티를 정의 할 수도 있습니다.

### 타입 프로퍼티 조회와 설정 \(Querying and Setting Type Properties\)

타입 프로퍼티는 인스턴스 프로퍼티처럼 점 구문으로 조회되고 설정합니다. 그러나 타입 프로퍼티는 해당 타입의 인스턴스가 아닌 _타입_ 에 대해 조회되고 설정합니다. 예를 들어:

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

다음 예제는 여러개의 오디오 채널에 대해 오디오 레벨 미터를 모델링 하는 구조체에 일부분으로 2개의 저장된 타입 프로퍼티를 사용합니다. 각 채널은 `0` 부터 `10` 까지의 정수인 오디오 레벨을 가지고 있습니다.

아래 그림은 이러한 오디오 채널 중 2개를 결합하여 스테레오 오디오 레벨 미터를 모델링하는 방법을 보여줍니다. 채널의 오디오 레벨이 `0` 이면 해당 채널의 조명은 켜지지 않습니다. 오디오 레벨이 `10` 이면 모든 채널의 조명은 켜집니다. 이 그림에서 왼쪽 채널은 현재 `9` 의 레벨을 가지고 오른쪽 채널은 현재 `7` 의 레벨을 가집니다:

![Static Properties VUMeter](../.gitbook/assets/10_staticpropertiesvumeter_2x.png)

위에 설명한 오디오 채널은 `AudioChannel` 구조체의 인스턴스로 표현됩니다:

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

`AudioChannel` 구조체는 기능 제공을 위해 2개의 저장된 타입 프로퍼티를 정의합니다. 첫번째 `thresholdLevel` 은 오디오 레벨이 가질 수 있는 최대값을 정의합니다. 모든 `AudioChannel` 인스턴스를 위한 `10` 의 상수값입니다. 오디오 신호가 `10` 의 값보다 높게 오면 아래에서 설명된대로 기준값으로 변경합니다.

두번째 타입 프로퍼티는 `maxInputLevelForAllChannels` 라는 저장된 프로퍼티 변수 입니다. 이것은 `AudioChannel` 인스턴스로 부터 받은 최대 입력값을 추적합니다. 초기값 `0` 부터 시작합니다.

`AudioChannel` 구조체는 채널의 현재 오디오 레벨을 `0` 에서 `10` 으로 표현하는 `currentLevel` 이라는 저장된 인스턴스 프로퍼티도 정의합니다.

`currentLevel` 프로퍼티는 `currentLevel` 이 설정될 때마다 값을 체크하는 `didSet` 프로퍼티 관찰자를 가지고 있습니다. 이 관찰자는 2가지를 체크합니다:

* `currentLevel` 의 새로운 값이 `thresholdLevel` 에 허용된 값보다 크면 프로퍼티 관찰자는 `currentLevel` 을 `thresholdLevel` 로 설정합니다.
* `currentLevel` 의 새로운 값이 `AudioChannel` 인스턴스에서 받은 값이 이전 값보다 크면 프로퍼티 관찰자는 새로운 `currentLevel` 값을 `maxInputLevelForAllChannels` 타입 프로퍼티에 저장합니다.

> NOTE  
> 2가지 체크사항 중 첫번째 항목에서 `didSet` 관찰자는 `currentLevel` 을 다른 값으로 설정합니다. 그러나 이것이 관찰자를 다시 호출하진 않습니다.

스트레오 사운드 시스템에 오디오 레벨을 표시하기 위해 `leftChannel` 과 `rightChannel` 이라 하는 2개의 새로운 오디오 채널을 생성하기 위해 `AudioChannel` 구조체를 사용할 수 있습니다:

```swift
var leftChannel = AudioChannel()
var rightChannel = AudioChannel()
```

_왼쪽_ 채널의 `currentLevel` 을 `7` 로 설정하면 `maxInputLevelForAllChannels` 타입 프로퍼티가 `7` 로 업데이트 된 것을 볼 수 있습니다:

```swift
leftChannel.currentLevel = 7
print(leftChannel.currentLevel)
// Prints "7"
print(AudioChannel.maxInputLevelForAllChannels)
// Prints "7"
```

_오른쪽_ 채널의 `currentLevel` 을 `11` 로 설정하면 오른쪽 채널의 `currentLevel` 프로퍼티가 `10` 의 최대값으로 변경된 것을 볼 수 있고 `maxInputLevelForAllChannels` 타입 프로퍼티가 `10` 으로 업데이트 된 것을 볼 수 있습니다:

```swift
rightChannel.currentLevel = 11
print(rightChannel.currentLevel)
// Prints "10"
print(AudioChannel.maxInputLevelForAllChannels)
// Prints "10"
```

