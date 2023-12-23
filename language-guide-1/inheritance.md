# 상속 \(Inheritance\)

기능을 추가 또는 재정의 하기 위한 하위 클래스 입니다.

클래스는 다른 클래스에서 메서드, 프로퍼티, 그리고 다른 특성을 _상속 \(inherit\)_ 할 수 있습니다. 클래스가 다른 클래스로 부터 상속될 때 상속하는 클래스를 _하위 클래스 \(subclass\)_ 라고 하고 상속된 클래스를 _상위 클래스 \(superclass\)_ 라고 합니다. 상속은 Swift에서 다른 타입과 클래스를 구분하는 기본적인 동작입니다.

Swift에서 클래스는 상위 클래스에 속하는 메서드, 프로퍼티, 그리고 서브 스크립트를 호출하고 접근할 수 있으며 메서드, 프로퍼티, 그리고 서브 스크립트를 동작을 수정하기위해 재정의한 버전을 제공할 수 있습니다. Swift는 재정의에 일치하는 상위 클래스 정의가 있는지 확인하고 재정의가 올바른지 확인하는데 도움을 줍니다.

클래스는 프로퍼티의 값이 변경될 때 알려주기 위해 상속된 프로퍼티에 프로퍼티 관찰자를 추가할 수도 있습니다. 프로퍼티 관찰자는 기존 프로퍼티가 저장된 프로퍼티 또는 계산된 프로퍼티로 정의되었는지 상관없이 프로퍼티에 추가될 수 있습니다.

## 기본 클래스 정의 \(Defining a Base Class\)

다른 클래스에서 상속하지 않은 클래스를 _기본 클래스 \(base class\)_ 라고 합니다.

> Note   
> Swift 클래스는 범용 기본 클래스를 상속하지 않습니다. 상위 클래스 지정 없이 정의한 클래스는 자동적으로 빌드 할 때 기본 클래스가 됩니다.

아래 예제는 `Vehicle` 이라는 기본 클래스를 정의합니다. 이 기본 클래스는 `Double` 의 프로퍼티 타입으로 유추되는 `0.0` 의 기본값을 가지는 `currentSpeed` 라는 저장된 프로퍼티를 정의합니다. `currentSpeed` 프로퍼티의 값은 탈 것의 설명을 생성하기 위해 `description` 이라는 읽기전용 계산된 `String` 프로퍼티에 사용됩니다.

`Vehicle` 기본 클래스는 `makeNoise` 라는 메서드도 정의합니다. 이 메서드는 기본 `Vehicle` 인스턴스에서 실질적으로 아무런 동작을 하지 않지만 후에 `Vehicle` 의 하위 클래스에 의해 사용자화 될 것입니다:

```swift
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    func makeNoise() {
        // do nothing - an arbitrary vehicle doesn't necessarily make a noise
    }
}
```

타입 이름 뒤에 빈 소괄호를 작성한 _초기화 구문_ 으로 `Vehicle` 의 새로운 인스턴스를 생성합니다:

```swift
let someVehicle = Vehicle()
```

새로운 `Vehicle` 인스턴스를 생성하면 탈 것의 현재 속도의 설명을 출력하기 위해 `description` 프로퍼티에 접근할 수 있습니다:

```swift
print("Vehicle: \(someVehicle.description)")
// Vehicle: traveling at 0.0 miles per hour
```

`Vehicle` 클래스는 임의의 탈 것에 대한 공통 특성을 정의하지만 그 자체로는 많이 사용되지 않습니다. 더 유용하게 사용하기 위해 더 구체적인 탈 것 종류를 설명하기 위해 수정이 필요합니다.

## 하위 클래스 \(Subclassing\)

_하위 클래스 \(Subclassing\)_ 는 기존 클래스를 기반으로 새로운 클래스를 만드는 작업입니다. 하위 클래스는 기존 클래스의 특성을 상속하므로 수정할 수 있습니다. 하위 클래스에 새로운 특성도 추가할 수 있습니다.

하위 클래스가 상위 클래스를 가지고 있다는 것을 나타내기 위해 하위 클래스 이름은 상위 클래스 이름 전에 콜론으로 구분하여 작성합니다:

```swift
class SomeSubclass: SomeSuperclass {
    // subclass definition goes here
}
```

다음의 예제는 `Vehicle` 을 상위 클래스로 가지는 `Bicycle` 이라는 하위 클래스를 정의합니다:

```swift
class Bicycle: Vehicle {
    var hasBasket = false
}
```

새로운 `Bicycle` 클래스는 `currentSpeed` 와 `description` 프로퍼티와 `makeNoise()` 메서드와 같은 `Vehicle` 의 모든 특성을 자동적으로 얻습니다.

상속한 특성 외에도 `Bicycle` 클래스는 `Bool` 타입으로 유추되는 기본값 `false` 인 `hasBasket` 이라는 새로운 저장된 프로퍼티를 정의합니다.

기본적으로 생성한 새로운 `Bicycle` 인스턴스는 바구니를 가지고 있지 않습니다. 인스턴스가 생성된 후에 특정 `Bicycle` 인스턴스의 `hasBasket` 프로퍼티를 `true` 로 설정할 수 있습니다:

```swift
let bicycle = Bicycle()
bicycle.hasBasket = true
```

`Bicycle` 인스턴스의 상속된 `currentSpeed` 프로퍼티를 수정할 수 있고 상속된 `description` 프로퍼티를 조회할 수도 있습니다:

```swift
bicycle.currentSpeed = 15.0
print("Bicycle: \(bicycle.description)")
// Bicycle: traveling at 15.0 miles per hour
```

하위 클래스는 그 자체로 하위 클래스화 될 수 있습니다. 다음 예제는 "tandem" 이라는 2개의 안장을 가진 자전거를 위한 `Bicycle` 의 하위 클래스를 생성합니다:

```swift
class Tandem: Bicycle {
    var currentNumberOfPassengers = 0
}
```

`Tandem` 은 `Bicycle` 의 모든 프로퍼티와 메서드를 상속하고 차례로 `Vehicle` 의 모든 프로퍼티와 메서드를 상속합니다. `Tandem` 하위 클래스는 기본값 `0` 인 `currentNumberOfPassengers` 라는 새로운 저장된 프로퍼티도 추가합니다.

`Tandem` 의 인스턴스를 생성하면 새로운 프로퍼티와 상속된 프로퍼티로 동작할 수 있고 `Vehicle` 로 부터 상속한 읽기전용 `description` 프로퍼티를 조회할 수 있습니다:

```swift
let tandem = Tandem()
tandem.hasBasket = true
tandem.currentNumberOfPassengers = 2
tandem.currentSpeed = 22.0
print("Tandem: \(tandem.description)")
// Tandem: traveling at 22.0 miles per hour
```

## 재정의 \(Overriding\)

하위 클래스는 상위 클래스에서 상속할 인스턴스 메서드, 타입 메서드, 인스턴스 프로퍼티, 타입 프로퍼티, 또는 서브 스크립트 자체 사용자 구현을 제공할 수 있습니다. 이것을 _재정의 \(overriding\)_ 라고 합니다.

상속될 특성을 재정의 하려면 재정의 할 정의 앞에 `override` 키워드를 추가합니다. 이렇게 하면 재정의를 명확하게 제공하고 실수로 일치하는 정의를 제공하지 않도록 합니다. 실수로 재정의하면 예기치 않은 동작을 야기하고 `override` 키워드 없이 재정의하면 코드가 컴파일 될 때 에러를 발생합니다.

`override` 키워드는 재정의 한 클래스의 상위 클래스 또는 상위 클래스 중 하나에 재정의를 위해 일치하는 선언을 Swift 컴파일러는 체크합니다. 이 체크는 재정의하는 정의가 옳다고 보장합니다.

### 상위 클래스 메서드, 프로퍼티, 그리고 서브 스크립트 접근 \(Accessing Superclass Methods, Properties, and Subscripts\)

하위 클래스에 메서드, 프로퍼티, 또는 서브 스크립트 재정의를 제공할 때 재정의의 일부로 상위 클래스 구현을 사용하는 것이 유용할 때가 있습니다. 예를 들어 기존 구현의 동작을 구체화 하거나 기존 상속된 변수에 수정된 값을 저장할 수 있습니다.

이것이 적절한 경우 `super` 접두어를 사용하여 메서드, 프로퍼티, 또는 서브 스크립트의 상위 클래스 버전을 접근합니다:

* `someMethod()` 라는 재정의된 메서드는 재정의한 메서드 구현 내에서 `super.someMethod()` 를 호출하여 상위 클래스 버전의 `someMethod()` 를 호출할 수 있습니다.
* `someProperty` 라는 재정의된 프로퍼티는 재정의한 getter 또는 setter 구현 내에서 `super.somProperty` 로 상위 클래스 버전의 `someProperty` 를 접근할 수 있습니다.
* `someIndex` 를 위한 재정의된 서브 스크립트는 재정의한 서브 스크립트 구현 내에서 `super[someIndex]` 로 상위 클래스 버전의 같은 서브 스크립트에 접근할 수 있습니다.

### 메서드 재정의 \(Overriding Methods\)

상속된 인스턴스 또는 타입 메서드를 재정의하여 하위 클래스 내에서 메서드의 맞춤형 또는 대체 구현을 제공합니다.

아래 예제는 `Train` 이라는 `Vehicle` 의 새로운 하위 클래스를 정의합니다. `Vehicle` 을 상속한 `Train` 에 `makeNoise()` 메서드를 재정의합니다:

```swift
class Train: Vehicle {
    override func makeNoise() {
        print("Choo Choo")
    }
}
```

`Train` 에 새로운 인스턴스를 생성하고 `makeNoise()` 메서드를 호출한다면 `Train` 하위 클래스 버전의 메서드가 호출되는 것을 볼 수 있습니다:

```swift
let train = Train()
train.makeNoise()
// Prints "Choo Choo"
```

### 프로퍼티 재정의 \(Overriding Properties\)

프로퍼티에 고유한 사용자 정의 getter와 setter를 제공하거나 기본 프로퍼티 값이 변경될 때 재정의 한 프로퍼티가 관찰할 수 있도록 프로퍼티 관찰자를 추가 하기위해 상속된 인스턴스 또는 타입 프로퍼티를 재정의할 수 있습니다.

#### 프로퍼티 getter와 setter 재정의 \(Overriding Property Getters and Setters\)

상속된 프로퍼티가 소스에서 저장된 또는 계산된 프로퍼티로 구현되었는지 여부와 상관없이 _모든_ 상속된 프로퍼티를 재정의 하기위해 사용자 지정 getter와 setter를 제공할 수 있습니다. 상속된 프로퍼티에 저장된 또는 계산된 특성은 하위 클래스에서 알 수 없습니다. 상속된 프로퍼티의 특정 이름과 타입만 알고 있습니다. 컴파일러가 동일한 이름과 타입을 가진 상위 클래스 프로퍼티와 일치하는지 확인할 수 있도록 항상 재정의하는 프로퍼티의 이름과 타입을 모두 명시해야 합니다.

하위 클래스에서 getter와 setter를 모두 제공하여 상속된 읽기전용 프로퍼티를 읽기-쓰기 프로퍼티로 표시할 수 있습니다. 그러나 상속된 읽기-쓰기 프로퍼티를 읽기전용 프로퍼티로 표시할 수 없습니다.

> Note   
> 프로퍼티 재정의의 부분으로 setter를 제공하면 getter도 제공해야 합니다. 재정의한 getter 내에서 상속된 프로퍼티의 값을 수정하고 싶지 않다면 `someProperty` 가 재정의하는 프로퍼티의 이름이라면 getter에서 `super.someProperty` 를 반환하면 상속된 값을 간편하게 전달할 수 있습니다.

다음의 예제는 `Vehicle` 의 하위 클래스인 `Car` 라는 새로운 클래스를 정의합니다. `Car` 클래스는 기본 정수 값이 `1` 인 `gear` 라는 새로운 저장된 프로퍼티를 가집니다. `Car` 클래스는 현재 기어를 포함한 설명을 제공하기 위해 `Vehicle` 로 부터 상속한 `description` 프로퍼티를 재정의 합니다:

```swift
class Car: Vehicle {
    var gear = 1
    override var description: String {
        return super.description + " in gear \(gear)"
    }
}
```

`description` 프로퍼티의 재정의는 `Vehicle` 클래스의 `description` 프로퍼티를 반환하는 `super.description` 호출로 시작합니다. 그런 다음 `description` 의 `Car` 클래스의 버전은 현재 기어에 대한 정보를 추가합니다.

`Car` 클래스의 인스턴스를 생성하고 `gear` 와 `currentSpeed` 프로퍼티를 설정하면 `description` 프로퍼티는 `Car` 클래스 내에서 정의한 설명을 반환하는 것을 볼 수 있습니다:

```swift
let car = Car()
car.currentSpeed = 25.0
car.gear = 3
print("Car: \(car.description)")
// Car: traveling at 25.0 miles per hour in gear 3
```

#### 프로퍼티 관찰자 재정의 \(Overriding Property Observers\)

상속된 프로퍼티에 프로퍼티 관찰자를 추가하기 위해 프로퍼티 재정의를 사용할 수 있습니다. 이것은 기존에 구현된 프로퍼티가 어떻든 상관없이 상속된 프로퍼티의 값이 변경될 때 알림을 받을 수 있습니다. 자세한 내용은 [프로퍼티 관찰자 \(Property Observers\)](properties.md#property-observers) 을 참고 바랍니다.

> Note   
> 상속된 저장된 프로퍼티 상수 또는 상속된 읽기전용 계산된 프로퍼티에 프로퍼티 관찰자를 추가할 수 없습니다. 이 프로퍼티의 값은 설정할 수 없으므로 재정의의 부분으로 `willSet` 또는 `didSet` 구현을 제공하기에 적절하지 않습니다.
>
> 같은 프로퍼티에 재정의한 setter와 재정의한 프로퍼티 관찰자를 동시에 제공할 수 없습니다. 프로퍼티의 값이 변경되는 것을 관찰하기 원하고 이미 프로퍼티에 사용자화 setter를 제공하고 있다면 사용자화 setter 내에서 간단하게 값 변경을 관찰할 수 있습니다.

다음 예제는 `Car` 의 하위 클래스 인 `AutomaticCar` 라는 새로운 클래스를 정의합니다. `AutomaticCar` 클래스는 현재 속도에 기반하여 적절한 기어를 자동적으로 선택하는 자동 기어박스가 있는 자동차를 표시합니다:

```swift
class AutomaticCar: Car {
    override var currentSpeed: Double {
        didSet {
            gear = Int(currentSpeed / 10.0) + 1
        }
    }
}
```

`AutomaticCar` 인스턴스에 `currentSpeed` 프로퍼티를 설정할 때마다 프로퍼티의 `didSet` 관찰자는 새로운 속도에 적절한 기어를 인스턴스의 `gear` 프로퍼티에 설정합니다. 특히 프로퍼티 관찰자는 새로운 `currentSpeed` 값을 `10` 으로 나눈다음 가까운 정수로 내림한 다음 `1` 을 더한 값을 기어로 선택합니다. `35.0` 의 속도는 기어가 `4` 가 됩니다:

```swift
let automatic = AutomaticCar()
automatic.currentSpeed = 35.0
print("AutomaticCar: \(automatic.description)")
// AutomaticCar: traveling at 35.0 miles per hour in gear 4
```

## 재정의 방지 \(Preventing Overrides\)

_final_ 표시를 통해 실수로 메서드, 프로퍼티, 또는 서브 스크립트를 재정의 하는 것을 방지할 수 있습니다. 재정의를 방지하려면 메서드, 프로퍼티, 또는 서브 스크립트의 키워드 전에 `final` 수정자를 작성합니다 \(`final var`, `final func`, `final class func`, `final subscript` 와 같이 작성\).

하위 클래스에서 final 메서드, 프로퍼티, 또는 서브 스크립트를 재정의 하면 컴파일 시 에러가 발생합니다. 확장한 클래스에 추가된 메서드, 프로퍼티, 또는 서브 스크립트는 확장의 정의 내에서 final로 표기될 수도 있습니다. 자세한 내용은 [확장 (Extensions)](extensions.md) 을 참고 바랍니다.

클래스 정의에 `class` 키워드 전에 `final` 키워드를 표기하여 final로 전체 클래스를 표시할 수 있습니다 \(`final class`\). final 클래스를 하위 클래스하려는 모든 시도는 컴파일 시 에러가 발생합니다.

