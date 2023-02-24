# 옵셔널 체이닝 \(Optional Chaining\)

언래핑 없이 옵셔널 값의 멤버에 접근합니다.

_옵셔널 체이닝 \(Optional chaining\)_ 은 현재 `nil` 일 수 있는 옵셔널 인 프로퍼티, 메서드, 그리고 서브 스크립트를 조회하고 호출하기 위한 프로세스 입니다. 옵셔널에 값이 포함되어 있으면 프로퍼티, 메서드, 또는 서브 스크립트는 호출에 성공합니다. 옵셔널이 `nil` 이면 프로퍼티, 메서드, 또는 서브 스크립트 호출은 `nil` 을 반환합니다. 여러 조회는 함께 연결될 수 있고 체인에 어느 부분이라도 `nil` 이면 전체 체인은 실패합니다.

> NOTE  
> Swift에서 옵셔널 체이닝은 Objective-C에서 메시징 `nil` 과 유사하지만 모든 타입에 대해 동작하고 성공 또는 실패 여부를 확인할 수 있습니다.

## 강제 언래핑의 대한 대안으로 옵셔널 체이닝 \(Optional Chaining as an Alternative to Forced Unwrapping\)

프로퍼티, 메서드 또는 서브 스크립트를 호출하려는 옵셔널 값 뒤에 물음표 \(`?`\)를 배치하여 옵셔널 체이닝을 지정합니다. 이것은 값에 강제 언래핑을 하기 위해 옵셔널 값 뒤에 느낌표 \(`!`\)를 배치하는 것과 유사합니다. 이것의 주요 차이점은 옵셔널이 `nil` 일 때 옵셔널 체이닝은 실패하는 반면에 강제 언래핑은 런타임 에러가 발생합니다.

옵셔널 체이닝은 `nil` 값에 대해 호출될 수 있다는 사실을 반영하기 위해 조회하는 프로퍼티, 메서드, 또는 서브 스크립트가 옵셔널 값이 아닌 값을 반환하더라도 항상 옵셔널 값으로 반환합니다. 옵셔널 반환 값으로 옵셔널 체이닝 호출이 성공 \(반환된 옵셔널 체이닝에 값이 포함됨\)했는지 실패 \(반환된 옵셔널 값은 `nil`\)했는지 확인할 수 있습니다.

특히 옵셔널 체이닝 호출의 결과는 예상되는 반환값과 동일한 타입이지만 옵셔널로 래핑됩니다. 일반적으로 `Int` 로 반환하는 프로퍼티는 옵셔널 체이닝을 통해 접근하면 `Int?` 를 반환합니다.

다음의 코드들은 옵셔널 체이닝이 강제 언래핑과 어떻게 다른지 보여주고 성공여부를 확인할 수 있도록 합니다.

첫번째, `Person` 과 `Residence` 라는 2개의 클래스는 정의되어 있습니다:

```swift
class Person {
    var residence: Residence?
}

class Residence {
    var numberOfRooms = 1
}
```

`Residence` 인스턴스는 기본값이 `1` 인 `numberOfRooms` 라는 `Int` 프로퍼티를 가지고 있습니다. `Person` 인스턴스는 `Residence?` 타입의 옵셔널 `residence` 프로퍼티를 가지고 있습니다.

새로운 `Person` 인스턴스를 생성하면 `residence` 프로퍼티는 옵셔널 규칙에 따라 `nil` 로 초기화 됩니다. 아래의 코드에서 `john` 은 `nil` 의 `residence` 프로퍼티 값을 가지고 있습니다:

```swift
let john = Person()
```

값에 강제 언래핑을 하기 위해 `residence` 뒤에 느낌표를 배치하여 사람의 `residence` 에 `numberOfRooms` 프로퍼티를 접근하면 `residence` 값이 없기 때문에 런타임 에러가 발생합니다:

```swift
let roomCount = john.residence!.numberOfRooms
// this triggers a runtime error
```

`John.residence` 가 `nil` 값이 아니고 `roomCount` 에 방의 적절한 숫자를 포함한 `Int` 로 설정하면 위의 코드는 정상동작 합니다. 그러나 `residence` 가 `nil` 이면 이 코드는 항상 런타임 에러가 발생합니다.

옵셔널 체이닝은 `numberOfRooms` 의 값에 접근하기 위한 대안으로 제공합니다. 옵셔널 체이닝을 사용하기 위해 느낌표 위치에 물음표를 사용합니다:

```swift
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// Prints "Unable to retrieve the number of rooms."
```

이것은 Swift가 옵셔널 `residence` 프로퍼티를 "체인"하고 `residence` 가 존재하면 `numberOfRooms` 값을 조회하도록 합니다.

`numberOfRooms` 접근하기 위한 시도는 실패할 수 있으므로 옵셔널 체이닝은 `Int?` 타입이나 "옵셔널 `Int`"의 값을 반환합니다. 위 예제에서 처럼 `residence` 가 `nil` 일 경우 `numberOfRooms` 접근이 불가능 한 사실을 반영하기 위해 옵셔널 `Int` 는 `nil` 입니다. 옵셔널 `Int` 는 언래핑 한 정수에 옵셔널 바인딩으로 접근되고 `roomCount` 상수에 옵셔널이 아닌 값을 할당합니다.

`numberOfRooms` 가 옵셔널 `Int` 가 아니더라도 마찬가지입니다. 옵셔널 체인으로 조회 된다는 것은 `numberOfRooms` 호출은 항상 `Int` 대신 `Int?` 를 반환한다는 의미입니다.

더이상 `nil` 값을 갖지 않기 위해 `john.residence` 에 `Residence` 인스턴스를 할당할 수 있습니다:

```swift
john.residence = Residence()
```

`john.residence` 는 `nil` 이 아닌 실제 `Residence` 인스턴스를 포함합니다. 이전 처럼 같은 옵셔널 체이닝으로 `numberOfRooms` 에 접근하면 기본 `numberOfRooms` 값인 `1` 의 `Int?` 를 반환할 것입니다:

```swift
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// Prints "John's residence has 1 room(s)."
```

## 옵셔널 체이닝에 대한 모델 클래스 정의 \(Defining Model Classes for Optional Chaining\)

하나 이상의 레벨 깊이 인 프로퍼티, 메서드, 그리고 서브 스크립트를 호출하기 위해 옵셔널 체이닝을 사용할 수 있습니다. 타입 호환되는 복잡한 모델 내에서 하위 프로퍼티로 내려갈 수 있으며 해당 하위 프로퍼티에 프로퍼티, 메서드, 그리고 서브 스크립트에 접근 가능한지 확인할 수 있습니다.

아래의 코드는 여러 레벨 옵셔널 체이닝의 예를 포함하여 몇몇의 후속 예제에서 사용할 4개의 모델 클래스를 정의합니다. 이 클래스는 관련 프로퍼티, 메서드, 그리고 서브 스크립트를 가지는 `Room` 과 `Address` 클래스를 추가하여 위의 `Person` 과 `Residence` 모델을 확장합니다.

`Person` 클래스는 이전과 같게 정의됩니다:

```swift
class Person {
    var residence: Residence?
}
```

`Residence` 클래스는 이전보다 더 복잡합니다. 이제 `Residence` 클래스는 `[Room]` 타입의 빈 배열을 가지고 초기화 되는 `rooms` 라는 변수 프로퍼티를 정의합니다:

```swift
class Residence {
    var rooms: [Room] = []
    var numberOfRooms: Int {
        return rooms.count
    }
    subscript(i: Int) -> Room {
        get {
            return rooms[i]
        }
        set {
            rooms[i] = newValue
        }
    }
    func printNumberOfRooms() {
        print("The number of rooms is \(numberOfRooms)")
    }
    var address: Address?
}
```

이 `Residence` 버전은 `Room` 인스턴스의 배열을 저장하기 때문에 `numberOfRooms` 프로퍼티는 저장된 프로퍼티가 아닌 계산된 프로퍼티로 구현됩니다. 계산된 `numberOfRooms` 프로퍼티는 `rooms` 배열에서 `count` 프로퍼티의 값을 반환합니다.

`rooms` 배열에 접근하는 짧은 구문을 위해 `Residence` 는 `rooms` 배열에 요청된 인덱스로 방에 접근을 제공하는 읽기-쓰기 서브 스크립트를 제공합니다.

이 `Residence` 는 주택에 있는 방의 갯수를 출력하는 `printNumberOfRooms` 라는 메서드도 제공합니다.

마지막으로 `Residence` 는 `Address?` 타입을 가지는 `address` 라는 옵셔널 프로퍼티를 정의합니다. 이 프로퍼티에 대한 `Address` 클래스 타입은 아래에 정의되어 있습니다.

`rooms` 배열에 사용된 `Room` 클래스는 `name` 이라는 프로퍼티와 적절한 방 이름을 프로퍼티에 설정하는 초기화 구문을 가진 클래스 입니다:

```swift
class Room {
    let name: String
    init(name: String) { self.name = name }
}
```

이 모델의 마지막 클래스는 `Address` 라 합니다. 이 클래스는 `String?` 타입의 3개의 옵셔널 프로퍼티를 가집니다. `buildingName` 과 `buildingNumber` 인 첫번째, 두번째 프로퍼티는 주소의 부분으로 특정 건물을 식별하기 위한 대안책입니다. 세번째 프로퍼티 인 `street` 는 주소에 대한 도로의 이름으로 사용됩니다:

```swift
class Address {
    var buildingName: String?
    var buildingNumber: String?
    var street: String?
    func buildingIdentifier() -> String? {
        if let buildingNumber = buildingNumber, let street = street {
            return "\(buildingNumber) \(street)"
        } else if buildingName != nil {
            return buildingName
        } else {
            return nil
        }
    }
}
```

또한 `Address` 클래스는 `String?` 의 반환 타입을 가지는 `buildingIdentifier()` 라는 메서드도 제공합니다. 이 메서드는 주소의 프로퍼티를 확인하고 값이 있으면 `buildingName` 을 반환하거나 둘다 값이 있으면 `street` 과 연결된 `buildingNumber` 를 반환하고 값이 없으면 `nil` 을 반환합니다.

## 옵셔널 체이닝을 통해 프로퍼티 접근 \(Accessing Properties Through Optional Chaining\)

[강제 언래핑의 대안으로 옵셔널 체이닝 \(Optional Chaining as an Alternative to Forced Unwrapping\)](optional-chaining.md#optional-chaining-as-an-alternative-to-forced-unwrapping) 에서 설명 했듯이 옵셔널 값의 프로퍼티에 접근하고 프로퍼티 접근이 성공하면 검사하기 위해 옵셔널 체이닝을 사용할 수 있습니다.

새로운 `Person` 인스턴스를 생성하기 위해 위에 정의된 클래스를 사용하고 이전처럼 `numberOfRooms` 프로퍼티에 접근합니다:

```swift
let john = Person()
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// Prints "Unable to retrieve the number of rooms."
```

`John.residence` 는 `nil` 이기 때문에 옵셔널 체이닝은 이전과 같이 실패를 호출합니다.

옵셔널 체이닝을 통해 프로퍼티의 값을 설정할 수 있습니다:

```swift
let someAddress = Address()
someAddress.buildingNumber = "29"
someAddress.street = "Acacia Road"
john.residence?.address = someAddress
```

이 예제에서 `john.residence` 가 `nil` 이기 때문에 `john.residence` 에 `address` 프로퍼티에 설정하기 위한 시도는 실패 할 것입니다.

이 할당은 `=` 연산자의 우항의 코드는 평가되지 않으므로 옵셔널 체이닝의 일부입니다. 이전 예제에서 상수에 접근하는 것은 어떠한 영향도 없기 때문에 `someAddress` 가 평가되지 않는다는 것을 쉽게 파악할 수 없습니다. 아래의 리스트는 같은 할당을 수행하지만 주소를 생성하기 위해 함수를 사용합니다. 이 함수는 값을 반환하기 전에 "Function was called"를 출력하여 `=` 연산자 우항이 평가되었음을 알 수 있습니다.

```swift
func createAddress() -> Address {
    print("Function was called.")

    let someAddress = Address()
    someAddress.buildingNumber = "29"
    someAddress.street = "Acacia Road"

    return someAddress
}
john.residence?.address = createAddress()
```

아무것도 출력되지 않기 때문에 `createAddress()` 함수가 호출되지 않음을 알 수 있습니다.

## 옵셔널 체이닝을 통한 함수 호출 \(Calling Methods Through Optional Chaining\)

옵셔널 값의 메서드를 호출하고 메서드 호출이 성공적인지 확인하기 위해 옵셔널 체이닝을 사용할 수 있습니다. 해당 메서드가 반환값을 정의하지 않아도 사용할 수 있습니다.

`Residence` 클래스에 `printNumberOfRooms()` 메서드는 `numberOfRooms` 의 현재값을 출력합니다. 메서드는 아래와 같습니다:

```swift
func printNumberOfRooms() {
    print("The number of rooms is \(numberOfRooms)")
}
```

이 메서드는 반환 타입이 지정되지 않았습니다. 그러나 반환 타입이 없는 함수와 메서드는 [반환값 없는 함수 \(Functions Without Return Values\)](functions.md#functions-without-return-values) 에서 설명했듯이 `Void` 의 암시적 반환 타입을 가지고 있습니다. 이것은 `()` 의 값 또는 빈 튜플을 반환한다는 의미입니다.

옵셔널 체이닝을 사용하여 옵셔널 값에 대해 메서드를 호출하면 반환값은 옵셔널 타입이기 때문에 메서드의 반환 타입은 `Void` 가 아닌 `Void?` 입니다. 메서드가 반환값을 정의하지 않았어도 `printNumberOfRooms()` 메서드 호출이 가능한지 `if` 구문을 사용하여 확인할 수 있습니다. `printNumberOfRooms` 호출의 반환값을 `nil` 과 비교하여 메서드 호출이 성공했는지 확인합니다:

```swift
if john.residence?.printNumberOfRooms() != nil {
    print("It was possible to print the number of rooms.")
} else {
    print("It was not possible to print the number of rooms.")
}
// Prints "It was not possible to print the number of rooms."
```

옵셔널 체이닝을 통해 프로퍼티를 설정하려는 경우에도 마찬가지입니다. [옵셔널 체이닝을 통해 프로퍼티 접근 \(Accessing Properties Through Optional Chaining\)](optional-chaining.md#accessing-properties-through-optional-chaining) 에서 위의 예제는 `residence` 프로퍼티가 `nil` 이지만 `John.residence` 에 대해 `address` 값을 설정 하려고 합니다. 옵셔널 체이닝을 통해 프로퍼티를 설정하려는 모든 시도는 `nil` 과 비교하여 프로퍼티에 값이 성공적으로 설정되었는지 확인할 수 있는 `Void?` 타입의 값을 반환합니다:

```swift
if (john.residence?.address = someAddress) != nil {
    print("It was possible to set the address.")
} else {
    print("It was not possible to set the address.")
}
// Prints "It was not possible to set the address."
```

## 옵셔널 체이닝을 통한 서브 스크립트 접근 \(Accessing Subscripts Through Optional Chaining\)

옵셔널 값의 서브 스크립트에서 값을 조회하고 설정하고 해당 서브 스크립트 호출이 성공했는지 확인하기 위해 옵셔널 체이닝을 사용할 수 있습니다.

> NOTE  
> 옵셔널 체이닝을 통해 옵셔널 값의 서브 스크립트에 접근할 때 물음표는 서브 스크립트의 대괄호 전에 위치합니다. 옵셔널 체이닝 물음표는 항상 옵셔널 표현구 부분의 바로 다음에 위치합니다.

아래의 예제는 `Residence` 클래스에 정의된 서브 스크립트를 사용하여 `john.residence` 프로퍼티에 `rooms` 배열의 첫번째 방 이름을 조회합니다. 현재 `john.residence` 는 `nil` 이므로 서브 스크립트 호출은 실패합니다:

```swift
if let firstRoomName = john.residence?[0].name {
    print("The first room name is \(firstRoomName).")
} else {
    print("Unable to retrieve the first room name.")
}
// Prints "Unable to retrieve the first room name."
```

서브 스크립트 호출에 옵셔널 체이닝 물음표는 `john.residence` 가 옵셔널 체이닝이 시도되는 옵셔널 값이기 때문에 `john.residence` 다음과 대괄호 전에 위치합니다.

유사하게 옵셔널 체이닝을 사용하여 서브 스크립트를 통해 새로운 값을 설정할 수 있습니다:

```swift
john.residence?[0] = Room(name: "Bathroom")
```

`residence` 가 현재 `nil` 이므로 서브 스크립트 설정은 실패합니다.

`rooms` 배열에 하나 이상의 `Room` 인스턴스 가지고 `john.residence` 에 실제 `Residence` 인스턴스를 생성하고 할당하면 옵셔널 체이닝으로 `Residence` 서브 스크립트를 사용하여 `rooms` 배열의 항목을 접근할 수 있습니다:

```swift
let johnsHouse = Residence()
johnsHouse.rooms.append(Room(name: "Living Room"))
johnsHouse.rooms.append(Room(name: "Kitchen"))
john.residence = johnsHouse

if let firstRoomName = john.residence?[0].name {
    print("The first room name is \(firstRoomName).")
} else {
    print("Unable to retrieve the first room name.")
}
// Prints "The first room name is Living Room."
```

### 옵셔널 타입에 서브 스크립트 접근 \(Accessing Subscripts of Optional Type\)

서브 스크립트가 Swift의 `Dictionary` 타입의 키 서브 스크립트와 같이 옵셔널 타입의 값을 반환하는 경우 옵셔널 반환값을 연결하기 위해 서브 스크립트의 닫는 대괄호 _뒤에_ 물음표를 추가합니다:

```swift
var testScores = ["Dave": [86, 82, 84], "Bev": [79, 94, 81]]
testScores["Dave"]?[0] = 91
testScores["Bev"]?[0] += 1
testScores["Brian"]?[0] = 72
// the "Dave" array is now [91, 82, 84] and the "Bev" array is now [80, 94, 81]
```

위의 예제는 `String` 키를 `Int` 값 배열에 매핑하는 2개의 키-값 쌍을 포함하는 `testScores` 라는 딕셔너리를 정의합니다. 이 예제는 `"Dave"` 배열에 첫번째 항목에 `91` 을 설정하고 `"Bev"` 배열에 첫번째 항목에 `1` 을 더하고 `"Brian"` 키에 대한 배열에 첫번째 항목에 값을 설정하기 위해 옵셔널 체이닝을 사용합니다. `testScores` 딕셔너리는 `"Dave"` 와 `"Bev"` 에 대한 키를 가지고 있으므로 첫번째 두번째 호출은 성공합니다. `testScores` 딕셔너리는 `"Brian"` 에 대한 키를 가지고 있지 않으므로 세번째 호출은 실패합니다.

## 여러 수준의 체인연결 \(Linking Multiple Levels of Chaining\)

여러 수준의 옵셔널 체이닝을 연결하여 모델 내에서 프로퍼티, 메서드, 그리고 서브 스크립로 깊게 접근할 수 있습니다. 그러나 여러 수준의 옵셔널 체이닝은 반환된 값에 더 많은 수준의 옵션성을 추가하지 않습니다.

다른 말로 표현하면:

* 조회하려는 타입이 옵셔널이 아니면 옵셔널 체이닝 때문에 옵셔널이 됩니다.
* 조회하려는 타입이 _이미_ 옵셔널이면 체이닝 때문에 _더 많은_ 옵셔널이 되지 않습니다.

따라서:

* 옵셔널 체이닝으로 `Int` 값을 조회하려고 하면 사용된 체이닝의 수준과 상관없이 항상 `Int?` 가 반환됩니다.
* 유사하게 옵셔널 체이닝으로 `Int?` 값을 조회하려고 하면 사용된 체이닝의 수준과 상관없이 항상 `Int?` 가 반환됩니다.

아래의 예제는 `john` 의 `residence` 프로퍼티에 `address` 프로퍼티에 `street` 프로퍼티를 접근합니다. 여기에서는 `residence` 와 `address` 프로퍼티를 통해 연결하기 위해 _2단계_ 수준의 옵셔널 체이이 사용되며 둘 다 옵셔널 타입입니다:

```swift
if let johnsStreet = john.residence?.address?.street {
    print("John's street name is \(johnsStreet).")
} else {
    print("Unable to retrieve the address.")
}
// Prints "Unable to retrieve the address."
```

`john.residence` 의 값은 현재 유효한 `Residence` 인스턴스를 가지고 있습니다. 그러나 `John.residence.address` 의 값은 현재 `nil` 입니다. 이것 때문에 `john.residence?.address?.street` 을 호출하면 실패합니다.

위의 예에서는 `street` 프로퍼티의 값을 조회하려고 합니다. 이 프로퍼티의 타입은 `String?` 입니다. 따라서 `john.residence?.address?.street` 의 반환값은 2단계 옵셔널 체이닝이 프로퍼티의 옵셔널 타입에 적용되었지만 `String?` 입니다.

`john.residence.address` 에 대한 값으로 `Address` 인스턴스를 설정하고 주소의 `street` 프로퍼티에 대해 값을 설정하면 여러 수준의 옵셔널 체이닝을 통해 `street` 프로퍼티의 값에 접근할 수 있습니다:

```swift
let johnsAddress = Address()
johnsAddress.buildingName = "The Larches"
johnsAddress.street = "Laurel Street"
john.residence?.address = johnsAddress

if let johnsStreet = john.residence?.address?.street {
    print("John's street name is \(johnsStreet).")
} else {
    print("Unable to retrieve the address.")
}
// Prints "John's street name is Laurel Street."
```

이 예제에서 `john.residence` 의 값은 현재 유효한 `Residence` 인스턴스 이므로 `john.residence` 에 `address` 프로퍼티에 값을 설정하는 것이 가능합니다.

## 옵셔널 반환값으로 메서드 체이닝 \(Chaining on Methods with Optional Return Values\)

이전 예제는 어떻게 옵셔널 체이닝을 통해 옵셔널 타입의 프로퍼티의 값을 조회해야 하는지 보여줍니다. 옵셔널 체이닝은 또한 옵셔널 타입의 값을 반환하는 메서드를 호출하고 필요하다면 메서드의 반환값을 연결하기 위해 사용할 수 있습니다.

아래의 예제는 옵셔널 체이닝을 통해 `Address` 클래스의 `buildingIdentifier()` 메서드를 호출합니다. 이 메서드는 `String?` 타입의 값을 반환합니다. 위에서 설명 했듯이 옵셔널 체이닝 이후 이 메서드 호출의 반환 타입은 `String?` 입니다:

```swift
if let buildingIdentifier = john.residence?.address?.buildingIdentifier() {
    print("John's building identifier is \(buildingIdentifier).")
}
// Prints "John's building identifier is The Larches."
```

메서드의 반환값에서 더 옵셔널 체이닝을 수행하려면 메서드의 소괄호 _다음에_ 옵셔널 체이닝 물음표를 위치시키면 됩니다:

```swift
if let beginsWithThe =
    john.residence?.address?.buildingIdentifier()?.hasPrefix("The") {
    if beginsWithThe {
        print("John's building identifier begins with \"The\".")
    } else {
        print("John's building identifier does not begin with \"The\".")
    }
}
// Prints "John's building identifier begins with "The"."
```

> NOTE  
> 위의 예제에서 연결하려는 옵셔널 값이 `buildingIdentifier()` 메서드 자체가 아닌 `buildingIdentifier()` 메서드의 반환값이므로 소괄호 _뒤에_ 옵셔널 체이닝 물음표를 위치시킵니다.

