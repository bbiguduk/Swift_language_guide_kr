# 상속 (Inheritance)

하위 클래스를 통해 기능을 추가하거나 재정의합니다.

클래스는 다른 클래스의
메서드, 프로퍼티, 다른 특성을 *상속(inherit)*할 수 있습니다.
클래스가 다른 클래스를 상속받으면,
상속받는 클래스를 *하위 클래스(subclass)*라고 하고,
상속해주는 클래스를 *상위 클래스(superclass)*라고 합니다.
상속은 Swift에서
클래스가 다른 타입과 구분되는 기본적인 특징입니다.

Swift의 클래스는
상위 클래스의 메서드, 프로퍼티, 서브스크립트를 호출하고 접근할 수 있으며,
메서드, 프로퍼티, 서브스크립트 동작을 수정하기 위해
재정의한 버전을 제공할 수 있습니다.
Swift는 재정의에 일치하는 상위 클래스 정의가 있는지 확인하고
재정의가 올바른지 확인하는데 도움을 줍니다.

클래스는 프로퍼티의 값이 변경될 때 알려주기 위해
상속받은 프로퍼티에 프로퍼티 관찰자를 추가할 수도 있습니다.
프로퍼티 관찰자는 해당 프로퍼티가 저장 프로퍼티나 연산 프로퍼티로 정의되었는지 상관없이
프로퍼티에 추가할 수 있습니다.

## 기본 클래스 정의 (Defining a Base Class)

다른 클래스로부터 상속받지 않은 클래스를 *기본 클래스(base class)*라고 합니다.

> Note: Swift의 클래스는 범용 기본 클래스로부터 자동으로 상속되지 않습니다.
> 상위 클래스를 명시하지 않고 정의한 클래스는
> 자동으로 빌드할 때 기본 클래스가 됩니다.

아래 예시는 `Vehicle`이라는 기본 클래스를 정의합니다.
이 기본 클래스는 `currentSpeed`라는 저장 프로퍼티를 가지며,
기본값은 `0.0`입니다 (프로퍼티 타입을 `Double`로 추론합니다).
`currentSpeed` 프로퍼티는
읽기 전용 연산 프로퍼티인 `description`에서 사용되며,
이 프로퍼티는 차량의 문자열 설명을 반환합니다.

`Vehicle` 클래스는 `makeNoise`라는 메서드도 정의합니다.
이 메서드는 기본 `Vehicle` 인스턴스에서 실질적으로 아무런 동작을 하지 않지만,
후에 `Vehicle`의 하위 클래스에서 커스텀될 예정입니다:

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

<!--
  - test: `inheritance`

  ```swifttest
  -> class Vehicle {
        var currentSpeed = 0.0
        var description: String {
           return "traveling at \(currentSpeed) miles per hour"
        }
        func makeNoise() {
           // do nothing - an arbitrary vehicle doesn't necessarily make a noise
        }
     }
  ```
-->

타입 이름 뒤에 빈 소괄호를 작성한
*이니셜라이저*으로 `Vehicle`의 새로운 인스턴스를 생성합니다:

```swift
let someVehicle = Vehicle()
```

<!--
  - test: `inheritance`

  ```swifttest
  -> let someVehicle = Vehicle()
  ```
-->

새로운 `Vehicle` 인스턴스를 생성하면
차량의 현재 속도의 설명을 출력하기 위해
`description` 프로퍼티에 접근할 수 있습니다:

```swift
print("Vehicle: \(someVehicle.description)")
// Vehicle: traveling at 0.0 miles per hour
```

<!--
  - test: `inheritance`

  ```swifttest
  -> print("Vehicle: \(someVehicle.description)")
  </ Vehicle: traveling at 0.0 miles per hour
  ```
-->

`Vehicle` 클래스는 임의의 차량에 대한 공통 특성을 정의하지만,
그 자체로는 많이 사용되지 않습니다.
더 유용하게 사용하기 위해,
더 구체적인 종류의 차량을 설명하는 하위 클래스를 정의해야 합니다.

## 서브클래싱 (Subclassing)

*서브클래싱(Subclassing)*은 기존 클래스를 기반으로 새로운 클래스를 만드는 작업입니다.
하위 클래스는 기존 클래스의 특성을 상속받으며, 이를 기반으로 동작을 세분화할 수 있습니다.
하위 클래스에 새로운 특성도 추가할 수 있습니다.

하위 클래스가 어떤 상위 클래스를 기반으로 하고 있는지를 나타내기 위해,
하위 클래스 이름 다음에 콜론을 쓰고
그 뒤에 상위 클래스 이름을 작성합니다:

```swift
class SomeSubclass: SomeSuperclass {
    // subclass definition goes here
}
```

<!--
  - test: `protocolSyntax`

  ```swifttest
  >> class SomeSuperclass {}
  -> class SomeSubclass: SomeSuperclass {
        // subclass definition goes here
     }
  ```
-->

다음의 예시는 `Vehicle`을 상위 클래스로 하는
`Bicycle`이라는 하위 클래스를 정의합니다:

```swift
class Bicycle: Vehicle {
    var hasBasket = false
}
```

<!--
  - test: `inheritance`

  ```swifttest
  -> class Bicycle: Vehicle {
        var hasBasket = false
     }
  ```
-->

새로운 `Bicycle` 클래스는 `currentSpeed`와 `description` 프로퍼티와 `makeNoise()` 메서드와 같은
`Vehicle`의 모든 특성을 자동으로 갖습니다.

상속한 특성 외에도
`Bicycle` 클래스는 `hasBasket`이라는 새로운 저장 프로퍼티를 정의하며,
기본값은 `false`입니다
(프로퍼티를 `Bool` 타입으로 유추됩니다).

기본적으로 생성한 새로운 `Bicycle` 인스턴스는 바구니를 가지고 있지 않습니다.
인스턴스가 생성된 후에
특정 `Bicycle` 인스턴스의 `hasBasket` 프로퍼티를 `true`로 설정할 수 있습니다:

```swift
let bicycle = Bicycle()
bicycle.hasBasket = true
```

<!--
  - test: `inheritance`

  ```swifttest
  -> let bicycle = Bicycle()
  -> bicycle.hasBasket = true
  ```
-->

`Bicycle` 인스턴스에서 상속받은 `currentSpeed` 프로퍼티를 수정할 수 있고,
상속받은 `description` 프로퍼티를 조회할 수도 있습니다:

```swift
bicycle.currentSpeed = 15.0
print("Bicycle: \(bicycle.description)")
// Bicycle: traveling at 15.0 miles per hour
```

<!--
  - test: `inheritance`

  ```swifttest
  -> bicycle.currentSpeed = 15.0
  -> print("Bicycle: \(bicycle.description)")
  </ Bicycle: traveling at 15.0 miles per hour
  ```
-->

하위 클래스도 서브클래싱할 수 있습니다.
다음 예시는 "tandem"이라는
2인승 자전거를 위한 `Bicycle`의 하위 클래스를 정의합니다:

```swift
class Tandem: Bicycle {
    var currentNumberOfPassengers = 0
}
```

<!--
  - test: `inheritance`

  ```swifttest
  -> class Tandem: Bicycle {
        var currentNumberOfPassengers = 0
     }
  ```
-->

`Tandem`은 `Bicycle`의 모든 프로퍼티와 메서드를 상속받고,
`Bicycle`은 다시 `Vehicle`의 모든 프로퍼티와 메서드를 상속받습니다.
`Tandem` 하위 클래스는 기본값 `0`인
`currentNumberOfPassengers`라는 새로운 저장 프로퍼티도 추가합니다.

`Tandem`의 인스턴스를 생성하면,
새로운 프로퍼티와 상속된 프로퍼티를 사용할 수 있고,
`Vehicle`로부터 상속된 읽기 전용 `description` 프로퍼티도 조회할 수 있습니다:

```swift
let tandem = Tandem()
tandem.hasBasket = true
tandem.currentNumberOfPassengers = 2
tandem.currentSpeed = 22.0
print("Tandem: \(tandem.description)")
// Tandem: traveling at 22.0 miles per hour
```

<!--
  - test: `inheritance`

  ```swifttest
  -> let tandem = Tandem()
  -> tandem.hasBasket = true
  -> tandem.currentNumberOfPassengers = 2
  -> tandem.currentSpeed = 22.0
  -> print("Tandem: \(tandem.description)")
  </ Tandem: traveling at 22.0 miles per hour
  ```
-->

## 재정의 (Overriding)

하위 클래스는 상위 클래스로부터 상속받은
인스턴스 메서드, 타입 메서드, 인스턴스 프로퍼티, 타입 프로퍼티, 서브스크립트를
자신만의 사용자 구현을 제공할 수 있습니다.
이것을 *재정의(overriding)*라고 합니다.

상속된 특성을 재정의하려면,
재정의할 선언 앞에 `override` 키워드를 추가합니다.
이렇게 하면 재정의를 명확하게 제공하고
실수로 일치하는 정의를 제공하지 않도록 합니다.
실수로 재정의하면 예기치 않은 동작을 야기하고,
`override` 키워드 없이 재정의하면
코드가 컴파일될 때 오류가 발생합니다.

`override` 키워드는 컴파일러에게
현재 정의하려는 멤버가 상위 클래스(또는 그 상위 클래스)에서
실제로 존재하는지 확인하도록 합니다.
이 검사를 통해 재정의하는 정의가 옳다고 보장합니다.

### 상위 클래스 메서드, 프로퍼티, 서브스크립트 접근 (Accessing Superclass Methods, Properties, and Subscripts)

하위 클래스에서 메서드, 프로퍼티, 서브스크립트 재정의를 제공할 때,
재정의의 부분으로
상위 클래스 구현을 사용하는 것이 유용할 때가 있습니다.
예를 들어 기존 구현의 동작을 세분화하거나,
기존 상속된 변수에 수정된 값을 저장할 수 있습니다.

이럴 때는
`super` 접두사를 사용하여
상위 클래스의 메서드, 프로퍼티, 서브스크립트에 접근합니다:

- `someMethod()`라는 재정의된 메서드는 재정의한 메서드 구현 내에서 `super.someMethod()`를 호출하여
  상위 클래스 버전의 `someMethod()` 를 호출할 수 있습니다.
- `someProperty`라는 재정의된 프로퍼티는 재정의한 getter나 setter 구현 내에서
  `super.somProperty`로 상위 클래스의 `someProperty`를 접근할 수 있습니다.
- `someIndex`를 위한 재정의된 서브스크립트는 재정의한 서브스크립트 구현 내에서
  `super[someIndex]`로 상위 클래스의 같은 서브스크립트에 접근할 수 있습니다.

### 메서드 재정의 (Overriding Methods)

상속받은 인스턴스 메서드나 타입 메서드를 재정의하여,
하위 클래스 내에서 메서드의 맞춤형 또는 대체 구현을 제공합니다.

아래 예시는 `Train`이라는 `Vehicle`의 새로운 하위 클래스를 정의하고,
`Vehicle`을 상속한 `Train`에 `makeNoise()` 메서드를 재정의합니다:

```swift
class Train: Vehicle {
    override func makeNoise() {
        print("Choo Choo")
    }
}
```

<!--
  - test: `inheritance`

  ```swifttest
  -> class Train: Vehicle {
        override func makeNoise() {
           print("Choo Choo")
        }
     }
  ```
-->

`Train`의 새로운 인스턴스를 생성하고 `makeNoise()` 메서드를 호출한다면,
`Train` 하위 클래스의 메서드가 호출되는 것을 볼 수 있습니다:

```swift
let train = Train()
train.makeNoise()
// Prints "Choo Choo"
```

<!--
  - test: `inheritance`

  ```swifttest
  -> let train = Train()
  -> train.makeNoise()
  <- Choo Choo
  ```
-->

### 프로퍼티 재정의 (Overriding Properties)

상속받은 인스턴스나 타입 프로퍼티를 재정의하여
하위 클래스에서 상속된 프로퍼티 값이 변경될 때 반응할 수 있는
해당 프로퍼티에 커스텀 getter와 setter를 제공하거나,
프로퍼티 관찰자를 추가할 수 있습니다.

#### 프로퍼티 getter와 setter 재정의 (Overriding Property Getters and Setters)

상속된 프로퍼티가
저장 프로퍼티인지 연산 프로퍼티인지는
상관없이 *모든* 상속된 프로퍼티를 재정의 하기위해
커스텀 getter와 setter를 제공할 수 있습니다.
상속된 프로퍼티가 저장 프로퍼티인지 연산 프로퍼티인지 하위 클래스에서는 알 수 없습니다 ---
상속된 프로퍼티의 특정 이름과 타입만 알고 있습니다.
컴파일러가 동일한 이름과 타입을 가진 상위 클래스 프로퍼티와
일치하는지 확인할 수 있도록
항상 재정의하는 프로퍼티의 이름과 타입을 모두 명시해야 합니다.

읽기 전용 프로퍼티를 재정의하여
읽기-쓰기 프로퍼티로 바꿀 수 있습니다.
그러나 상속된 읽기-쓰기 프로퍼티를 읽기 전용 프로퍼티로 변경하는 것은 허용되지 않습니다.

> Note: 프로퍼티 재정의의 부분으로
> setter를 제공하면 getter도 제공해야 합니다.
> 재정의한 getter 내에서 상속된 프로퍼티의 값을 수정하고 싶지 않다면,
> `someProperty`가 재정의하는 프로퍼티의 이름이라면,
> getter에서 `super.someProperty`를 반환하면
> 상속된 값을 간단하게 전달할 수 있습니다.

다음의 예시는 `Vehicle`의 하위 클래스인
`Car`라는 새로운 클래스를 정의합니다.
`Car` 클래스는 기본 정수값이 `1`인
`gear`라는 새로운 저장 프로퍼티를 가집니다.
`Car` 클래스는 현재 기어를 포함한 설명을 제공하기 위해
`Vehicle`로부터 상속받은 `description` 프로퍼티를 재정의합니다:

```swift
class Car: Vehicle {
    var gear = 1
    override var description: String {
        return super.description + " in gear \(gear)"
    }
}
```

<!--
  - test: `inheritance`

  ```swifttest
  -> class Car: Vehicle {
        var gear = 1
        override var description: String {
           return super.description + " in gear \(gear)"
        }
     }
  ```
-->

`description` 프로퍼티의 재정의는 `Vehicle` 클래스의 `description` 프로퍼티를 반환하는
`super.description` 호출로 시작합니다.
그런 다음 `Car` 클래스의 `description`은
현재 기어에 대한 정보를 추가합니다.

`Car` 클래스의 인스턴스를 생성하고
`gear`와 `currentSpeed` 프로퍼티를 설정하면
`description` 프로퍼티는
`Car` 클래스에 정의한 설명을 반환하는 것을 볼 수 있습니다:

```swift
let car = Car()
car.currentSpeed = 25.0
car.gear = 3
print("Car: \(car.description)")
// Car: traveling at 25.0 miles per hour in gear 3
```

<!--
  - test: `inheritance`

  ```swifttest
  -> let car = Car()
  -> car.currentSpeed = 25.0
  -> car.gear = 3
  -> print("Car: \(car.description)")
  </ Car: traveling at 25.0 miles per hour in gear 3
  ```
-->

#### 프로퍼티 관찰자 재정의 (Overriding Property Observers)

상속된 프로퍼티에 프로퍼티 관찰자를 추가하기 위해 프로퍼티 재정의를 사용할 수 있습니다.
이것은 기존에 구현된 프로퍼티가 어떻든 상관없이
상속된 프로퍼티의 값이 변경될 때 알림을 받을 수 있습니다.
자세한 내용은 [프로퍼티 관찰자 (Property Observers)](./properties.md#프로퍼티-관찰자-property-observers)를 참고바랍니다.

> Note: 상속된 저장 프로퍼티 상수나 상속된 읽기 전용 연산 프로퍼티에
> 프로퍼티 관찰자를 추가할 수 없습니다.
> 이런 프로퍼티에 값을 설정할 수 없으므로
> 재정의의 부분으로
> `willSet`이나 `didSet` 구현을 제공하기에 적절하지 않습니다.
>
> 같은 프로퍼티에 재정의한 setter와 재정의한 프로퍼티 관찰자를
> 동시에 제공할 수 없습니다.
> 프로퍼티의 값이 변경되는 것을 관찰하기 원하고
> 이미 프로퍼티에 커스텀 setter를 제공하고 있다면
> 커스텀 setter 내에서 간단하게 값 변경을 관찰할 수 있습니다.

다음 예시는 `Car`의 하위 클래스 인
`AutomaticCar`라는 새로운 클래스를 정의합니다.
`AutomaticCar` 클래스는 현재 속도에 기반하여 적절한 기어를 자동으로 선택하는
자동 기어 박스가 있는 자동차를 나타냅니다:

```swift
class AutomaticCar: Car {
    override var currentSpeed: Double {
        didSet {
            gear = Int(currentSpeed / 10.0) + 1
        }
    }
}
```

<!--
  - test: `inheritance`

  ```swifttest
  -> class AutomaticCar: Car {
        override var currentSpeed: Double {
           didSet {
              gear = Int(currentSpeed / 10.0) + 1
           }
        }
     }
  ```
-->

`AutomaticCar` 인스턴스에 `currentSpeed` 프로퍼티가 설정될 때마다,
프로퍼티의 `didSet` 관찰자는 새로운 속도에 적절한 기어를
인스턴스의 `gear` 프로퍼티에 설정합니다.
특히 프로퍼티 관찰자는
새로운 `currentSpeed` 값을 `10`으로 나눈 뒤,
가까운 정수로 내림한 다음 `1`을 더한 값을 기어로 선택합니다.
`35.0`의 속도는 기어를 `4`로 선택합니다:

```swift
let automatic = AutomaticCar()
automatic.currentSpeed = 35.0
print("AutomaticCar: \(automatic.description)")
// AutomaticCar: traveling at 35.0 miles per hour in gear 4
```

<!--
  - test: `inheritance`

  ```swifttest
  -> let automatic = AutomaticCar()
  -> automatic.currentSpeed = 35.0
  -> print("AutomaticCar: \(automatic.description)")
  </ AutomaticCar: traveling at 35.0 miles per hour in gear 4
  ```
-->

## 재정의 방지 (Preventing Overrides)

*final* 표시를 통해
실수로 메서드, 프로퍼티, 서브스크립트를 재정의 하는 것을 방지할 수 있습니다.
재정의를 방지하려면
메서드, 프로퍼티, 서브스크립트의 키워드 앞에 `final` 수정자를 작성합니다
(`final var`, `final func`, `final class func`, `final subscript` 와 같이 작성).

하위 클래스에서 final 메서드, final 프로퍼티, final 서브스크립트를 재정의 하면
컴파일 시 오류가 발생합니다.
확장한 클래스에 추가된 메서드, 프로퍼티, 서브스크립트는
확장의 정의 내에서 final로 표시할 수도 있습니다.
자세한 내용은 [확장 (Extensions)](./extensions.md)을 참고바랍니다.

<!--
  - test: `finalPreventsOverriding`

  ```swifttest
  -> class C {
        final var someVar = 0
        final func someFunction() {
           print("In someFunction")
        }
     }
  -> class D : C {
        override var someVar: Int {
           get { return 1 }
           set {}
        }
        override func someFunction() {
           print("In overridden someFunction")
        }
     }
  !$ error: property overrides a 'final' property
  !! override var someVar: Int {
  !! ^
  !$ note: overridden declaration is here
  !! final var someVar = 0
  !! ^
  !$ error: instance method overrides a 'final' instance method
  !! override func someFunction() {
  !! ^
  !$ note: overridden declaration is here
  !! final func someFunction() {
  !! ^
  ```
-->

클래스 정의에 `class` 키워드 전에
`final` 키워드를 표시하여 final로 전체 클래스를 표시할 수 있습니다(`final class`).
final 클래스를 하위 클래스하려는 모든 시도는 컴파일 시 오류가 발생합니다.

<!--
  - test: `finalClassPreventsOverriding`

  ```swifttest
  -> final class C {
        var someVar = 0
        func someFunction() {
           print("In someFunction")
        }
     }
  -> class D : C {
        override var someVar: Int {
           get { return 1 }
           set {}
        }
        override func someFunction() {
           print("In overridden someFunction")
        }
     }
  !$ error: property overrides a 'final' property
  !!      override var someVar: Int {
  !!                   ^
  !$ note: overridden declaration is here
  !!      var someVar = 0
  !!          ^
  !$ error: instance method overrides a 'final' instance method
  !!      override func someFunction() {
  !!                    ^
  !$ note: overridden declaration is here
  !!      func someFunction() {
  !!           ^
  !$ error: inheritance from a final class 'C'
  !! class D : C {
  !!       ^
  ```
-->

<!--
  TODO: I should probably provide an example here.
-->

<!--
  TODO: provide more information about function signatures,
  and what does / doesn't make them unique.
  For example, the parameter names don't have to match
  in order for a function to override a similar signature in its parent.
  (This is true for both of the function declaration syntaxes.)
-->

<!--
  TODO: Mention that you can return more-specific types, and take less-specific types,
  when overriding methods that use optionals / unchecked optionals.

  TODO: Overriding Type Methods
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
