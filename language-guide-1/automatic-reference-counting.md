# 자동 참조 카운팅 \(Automatic Reference Counting\)

<!--
Swift uses Automatic Reference Counting (ARC) to track and manage your app’s memory usage. In most cases, this means that memory management “just works” in Swift, and you don’t need to think about memory management yourself. ARC automatically frees up the memory used by class instances when those instances are no longer needed.

However, in a few cases ARC requires more information about the relationships between parts of your code in order to manage memory for you. This chapter describes those situations and shows how you enable ARC to manage all of your app’s memory. Using ARC in Swift is very similar to the approach described in Transitioning to ARC Release Notes for using ARC with Objective-C.

Reference counting applies only to instances of classes. Structures and enumerations are value types, not reference types, and aren’t stored and passed by reference.
-->

Swift는 앱의 메모리 사용량을 추적하고 관리하기 위해 _자동 참조 카운팅 \(Automatic Reference Counting\)_ \(ARC\)를 사용합니다. 대부분의 경우에 Swift에서 메모리 관리는 "그냥 수행하라" 를 의미하고 메모리 관리에 대해서 생각할 필요가 없습니다. ARC는 인스턴스가 더이상 필요치 않을 때 자동으로 클래스 인스턴스에 의해 사용된 메모리를 할당 해제 합니다.

그러나 몇몇의 경우에 ARC는 메모리를 관리하기 위해 코드의 부분 간의 관계에 대한 추가 정보를 요구합니다. 이 챕터에서는 이러한 상황을 설명하고 앱의 메모리를 관리하기 위해 ARC를 어떻게 사용하는지 보여줍니다. Swift에서 ARC를 사용하는 것은 Objective-C에서 ARC 사용에 대한 [ARC 릴리즈 노트로 전환 \(Transitioning to ARC Release Notes\)](https://developer.apple.com/library/content/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html) 에서 설명한 접근방식과 매우 유사합니다.

참조 카운팅은 클래스의 인스턴스에만 적용됩니다. 구조체와 열거형은 참조 타입이 아니고 값 타입이고 참조로 저장되거나 전달되지 않습니다.

## ARC의 작동 원리 \(How ARC Works\)

<!--
Every time you create a new instance of a class, ARC allocates a chunk of memory to store information about that instance. This memory holds information about the type of the instance, together with the values of any stored properties associated with that instance.

Additionally, when an instance is no longer needed, ARC frees up the memory used by that instance so that the memory can be used for other purposes instead. This ensures that class instances don’t take up space in memory when they’re no longer needed.

However, if ARC were to deallocate an instance that was still in use, it would no longer be possible to access that instance’s properties, or call that instance’s methods. Indeed, if you tried to access the instance, your app would most likely crash.

To make sure that instances don’t disappear while they’re still needed, ARC tracks how many properties, constants, and variables are currently referring to each class instance. ARC will not deallocate an instance as long as at least one active reference to that instance still exists.

To make this possible, whenever you assign a class instance to a property, constant, or variable, that property, constant, or variable makes a strong reference to the instance. The reference is called a “strong” reference because it keeps a firm hold on that instance, and doesn’t allow it to be deallocated for as long as that strong reference remains.
-->

클래스의 새로운 인스턴스가 생성될 때마다 ARC는 인스턴스에 대한 정보를 저장하기 위해 메모리의 청크에 할당합니다. 이 메모리는 해당 인스턴스와 관련된 저장된 프로퍼티의 값과 함께 인스턴스의 타입에 대한 정보를 가집니다.

또한 인스턴스가 더이상 필요치 않을 때 ARC는 메모리가 다른 목적으로 사용될 수 있도록 인스턴스에 의해 사용된 메모리를 할당 해제 합니다. 이렇게 하면 클래스 인스턴스가 더이상 필요치 않을 때 메모리 공간을 차지하지 않습니다.

그러나 ARC가 아직 사용중인 인스턴스의 할당을 해제하면 더이상 인스턴스의 프로퍼티에 접근할 수 없거나 인스턴스의 메서드를 호출할 수 없습니다. 실제로 인스턴스에 접근하려고 하면 앱은 크래시가 발생합니다.

인스턴스가 여전히 필요한 동안 사라지지 않도록 ARC는 얼마나 많은 프로퍼티, 상수, 그리고 변수가 각 클래스 인스턴스에 참조하고 있는지 추적합니다. ARC는 인스턴스에 참조가 하나라도 존재하는 한 인스턴스를 할당 해제 하지 않습니다.

이것을 가능하게 하려면 프로퍼티, 상수, 또는 변수에 클래스 인스턴스를 할당할 때마다 해당 프로퍼티, 상수, 또는 변수는 인스턴스에 _강한 참조 \(strong reference\)_ 를 만듭니다. 참조는 해당 인스턴스를 유지하고 강한 참조가 남아있는 한 할당 해제를 허용하지 않기 때문에 "강한" 참조라고 합니다.

## ARC 동작 \(ARC in Action\)

<!--
Here’s an example of how Automatic Reference Counting works. This example starts with a simple class called Person, which defines a stored constant property called name:
-->

다음은 어떻게 자동 참조 카운팅이 동작하는지에 대한 예제입니다. 이 예제는 `name` 이라는 저장된 상수 프로퍼티를 정의하는 `Person` 이라는 간단한 클래스로 시작합니다:

```swift
class Person {
    let name: String
    init(name: String) {
        self.name = name
        print("\(name) is being initialized")
    }
    deinit {
        print("\(name) is being deinitialized")
    }
}
```

<!--
The Person class has an initializer that sets the instance’s name property and prints a message to indicate that initialization is underway. The Person class also has a deinitializer that prints a message when an instance of the class is deallocated.

The next code snippet defines three variables of type Person?, which are used to set up multiple references to a new Person instance in subsequent code snippets. Because these variables are of an optional type (Person?, not Person), they’re automatically initialized with a value of nil, and don’t currently reference a Person instance.
-->

`Person` 클래스는 인스턴스의 `name` 프로퍼티에 설정하고 초기화가 진행 중임을 나타내는 메세지를 출력하는 초기화 구문을 가집니다. `Person` 클래스는 클래스의 인스턴스가 할당 해제될 때 메시지를 출력하는 초기화 해제 구문도 가지고 있습니다.

다음 코드는 뒤따르는 코드에 새로운 `Person` 인스턴스에 여러개 참조를 설정하기 위해 사용되는 `Person?` 타입의 3개의 변수를 정의합니다. 이 변수는 `Person` 이 아닌 `Person?` 인 옵셔널 타입 이므로 `nil` 의 값으로 자동으로 초기화 되고 현재는 `Person` 인스턴스를 참조하지 않습니다.

```swift
var reference1: Person?
var reference2: Person?
var reference3: Person?
```

<!--
You can now create a new Person instance and assign it to one of these three variables:
-->

이제 새로운 `Person` 인스턴스를 생성하고 이 3개의 변수중 하나에 할당합니다:

```swift
reference1 = Person(name: "John Appleseed")
// Prints "John Appleseed is being initialized"
```

<!--
Note that the message "John Appleseed is being initialized" is printed at the point that you call the Person class’s initializer. This confirms that initialization has taken place.

Because the new Person instance has been assigned to the reference1 variable, there’s now a strong reference from reference1 to the new Person instance. Because there’s at least one strong reference, ARC makes sure that this Person is kept in memory and isn’t deallocated.

If you assign the same Person instance to two more variables, two more strong references to that instance are established:
-->

`"John Appleseed is being initialized"` 메세지는 `Person` 클래스의 초기화 구문을 호출할 때 출력됩니다. 이것은 초기화가 발생했음을 확인합니다.

새로운 `Person` 인스턴스는 `reference1` 변수에 할당되기 때문에 `reference1` 에서 새로운 `Person` 인스턴스에 대한 강한 참조가 있습니다. 하나의 강한 참조가 있기 때문에 ARC는 이 `Person` 을 메모리에 유지하고 할당 해제하지 않습니다.

동일한 `Person` 인스턴스를 2개 이상의 변수에 할당하면 해당 인스턴스에 대한 2개 이상의 강한 참조가 설정됩니다:

```swift
reference2 = reference1
reference3 = reference1
```

<!--
There are now three strong references to this single Person instance.

If you break two of these strong references (including the original reference) by assigning nil to two of the variables, a single strong reference remains, and the Person instance isn’t deallocated:
-->

이제 이 단일 `Person` 인스턴스에 _3개_ 의 강한 참조가 있습니다.

기존의 참조를 포함하여 2개의 변수에 `nil` 을 할당하여 강한 참조 중 2개를 중단하면 하나의 강한 참조만 남고 `Person` 인스턴스는 할당 해제되지 않습니다:

```swift
reference1 = nil
reference2 = nil
```

<!--
ARC doesn’t deallocate the Person instance until the third and final strong reference is broken, at which point it’s clear that you are no longer using the Person instance:
-->

ARC는 세번째와 마지막 강한 참조를 중단할 때까지 `Person` 인스턴스를 할당 해제하지 않습니다. 이 때부터 `Person` 인스턴스를 더이상 사용하지 않는 것이 명백합니다:

```swift
reference3 = nil
// Prints "John Appleseed is being deinitialized"
```

## 클래스 인스턴스 사이의 강한 참조 사이클 \(Strong Reference Cycles Between Class Instances\)

<!--
In the examples above, ARC is able to track the number of references to the new Person instance you create and to deallocate that Person instance when it’s no longer needed.

However, it’s possible to write code in which an instance of a class never gets to a point where it has zero strong references. This can happen if two class instances hold a strong reference to each other, such that each instance keeps the other alive. This is known as a strong reference cycle.

You resolve strong reference cycles by defining some of the relationships between classes as weak or unowned references instead of as strong references. This process is described in Resolving Strong Reference Cycles Between Class Instances. However, before you learn how to resolve a strong reference cycle, it’s useful to understand how such a cycle is caused.

Here’s an example of how a strong reference cycle can be created by accident. This example defines two classes called Person and Apartment, which model a block of apartments and its residents:
-->

위의 예제에서 ARC는 새로운 `Person` 인스턴스를 생성하고 더이상 필요치 않을 때 `Person` 인스턴스를 할당 해제하기 위해 참조의 수를 추적할 수 있습니다.

그러나 클래스의 인스턴스가 강한 참조가 없는 지점에 도달하지 않는 코드를 작성할 수 있습니다. 이는 두 클래스 인스턴스가 서로에 대한 강한 참조를 유지하여 각 인스턴스가 다른 인스턴스를 유지하는 경우 발생할 수 있습니다. 이것은 _강한 참조 사이클 \(strong reference cycle\)_ 이라고 합니다.

클래스 간의 일부 관계를 강한 참조 대신 약한 \(weak\) 또는 미소유 \(unowned\) 참조로 정의하여 강한 참조 사이클을 해결합니다. 이 프로세스는 [클래스 인스턴스 간의 강한 참조 사이 해결 \(Resolving Strong Reference Cycles Between Class Instances\)](automatic-reference-counting.md#resolving-strong-reference-cycles-between-class-instances) 에서 설명되어 있습니다. 그러나 강한 참조 사이클을 어떻게 해결하는지 배우기 전에 어떻게 사이클이 발생하는지 이해하는 것이 유용합니다.

다음은 실수로 강한 참조 사이클을 생성하는 예입니다. 이 예제는 아파트와 거주자의 블럭을 모델링 하는 `Person` 과 `Apartment` 라는 2개의 클래스를 정의합니다:

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    var tenant: Person?
    deinit { print("Apartment \(unit) is being deinitialized") }
}
```

<!--
Every Person instance has a name property of type String and an optional apartment property that’s initially nil. The apartment property is optional, because a person may not always have an apartment.

Similarly, every Apartment instance has a unit property of type String and has an optional tenant property that’s initially nil. The tenant property is optional because an apartment may not always have a tenant.

Both of these classes also define a deinitializer, which prints the fact that an instance of that class is being deinitialized. This enables you to see whether instances of Person and Apartment are being deallocated as expected.

This next code snippet defines two variables of optional type called john and unit4A, which will be set to a specific Apartment and Person instance below. Both of these variables have an initial value of nil, by virtue of being optional:
-->

모든 `Person` 인스턴스는 `String` 타입의 `name` 프로퍼티와 초기값이 `nil` 인 옵셔널 `apartment` 프로퍼티를 가지고 있습니다. 사람이 항상 아파트를 가지고 있지 않기 때문에 `apartment` 프로퍼티는 옵셔널입니다.

유사하게 모든 `Apartment` 인스턴스는 `String` 타입의 `unit` 프로퍼티와 초기값이 `nil` 인 옵셔널 `tenant` 프로퍼티를 가지고 있습니다. 아파트를 항상 보유하는 것은 아니므로 `tenant` 프로퍼티는 옵셔널입니다.

이 클래스 모두 클래스의 인스턴스가 초기화 해제 됨을 출력하는 초기화 해제 구문을 정의합니다. 이것을 사용하면 `Person` 과 `Apartment` 의 인스턴스가 예상되로 초기화 해제되었는지 확인할 수 있습니다.

다음 코드는 아래에서 특정 `Apartment` 와 `Person` 인스턴스를 설정할 `john` 과 `unit4A` 라는 옵셔널 타입의 2개의 변수를 정의합니다. 이 변수 모두 옵셔널 이므로 초기값으로 `nil` 을 가집니다:

```swift
var john: Person?
var unit4A: Apartment?
```

<!--
You can now create a specific Person instance and Apartment instance and assign these new instances to the john and unit4A variables:
-->

이제 특정 `Person` 인스턴스와 `Apartment` 인스턴스를 생성할 수 있고 이 새로운 인스턴스를 `john` 과 `unit4A` 변수에 할당할 수 있습니다:

```swift
john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")
```

<!--
Here’s how the strong references look after creating and assigning these two instances. The john variable now has a strong reference to the new Person instance, and the unit4A variable has a strong reference to the new Apartment instance:
-->

다음은 강한 참조가 이 두 인스턴스를 생성하고 할당하여 어떻게 보이는지 나타냅니다. `john` 변수는 이제 새로운 `Person` 인스턴스에 대한 강한 참조를 가지고 있고 `unit4A` 변수는 새로운 `Apartment` 인스턴스에 대한 강한 참조를 가지고 있습니다:

![Reference Cycle 01](../.gitbook/assets/24_referenceCycle01_2x.png)

<!--
You can now link the two instances together so that the person has an apartment, and the apartment has a tenant. Note that an exclamation point (!) is used to unwrap and access the instances stored inside the john and unit4A optional variables, so that the properties of those instances can be set:
-->

이제 사람은 아파트를 가지고 아파트는 소유자를 가지도록 두 인스턴스를 함께 연결할 수 있습니다. 느낌표 \(`!`\)는 `john` 과 `unit4A` 옵셔널 변수 내에 저장된 인스턴스를 언래핑 하고 접근하기 위해 사용되므로 해당 인스턴스의 프로퍼티를 설정할 수 있습니다:

```swift
john!.apartment = unit4A
unit4A!.tenant = john
```

<!--
Here’s how the strong references look after you link the two instances together:
-->

다음은 두 인스턴스를 함께 연결한 후에 강한 참조가 어떻게 되는지 보여줍니다:

![Reference Cycle 02](../.gitbook/assets/24_referenceCycle02_2x.png)

<!--
Unfortunately, linking these two instances creates a strong reference cycle between them. The Person instance now has a strong reference to the Apartment instance, and the Apartment instance has a strong reference to the Person instance. Therefore, when you break the strong references held by the john and unit4A variables, the reference counts don’t drop to zero, and the instances aren’t deallocated by ARC:
-->

불행히도 이 두 인스턴스 연결은 서로간의 강한 참조 사이클을 생성합니다. `Person` 인스턴스는 이제 `Apartment` 인스턴스에 대한 강한 참조를 가지고 `Apartment` 인스턴스는 `Person` 인스턴스에 대한 강한 참조를 가집니다. 따라서 `john` 과 `unit4A` 변수에 의해 가진 강한 참조를 중단할 때 참조 카운트는 0으로 떨어지지 않고 인스턴스는 ARC에 의해 할당 해제되지 않습니다:

```swift
john = nil
unit4A = nil
```

<!--
Note that neither deinitializer was called when you set these two variables to nil. The strong reference cycle prevents the Person and Apartment instances from ever being deallocated, causing a memory leak in your app.

Here’s how the strong references look after you set the john and unit4A variables to nil:
-->

두 변수를 `nil` 로 설정할 때 초기화 구문은 호출되지 않습니다. 강한 참조 사이클은 `Person` 과 `Apartment` 인스턴스가 할당 해제되는 것을 방지하여 앱에서 메모리 누수를 유발합니다.

다음은 `john` 과 `unit4A` 변수를 `nil` 로 설정한 후에 강한 참조를 나타냅니다:

![Reference Cycle 03](../.gitbook/assets/24_referenceCycle03_2x.png)

<!--
The strong references between the Person instance and the Apartment instance remain and can’t be broken.
-->

`Person` 과 `Apartment` 인스턴스 간의 강한 참조는 남아있고 중단될 수 없습니다.

## 클래스 인스턴스 간의 강한 참조 사이클 해결 \(Resolving Strong Reference Cycles Between Class Instances\)

<!--
Swift provides two ways to resolve strong reference cycles when you work with properties of class type: weak references and unowned references.

Weak and unowned references enable one instance in a reference cycle to refer to the other instance without keeping a strong hold on it. The instances can then refer to each other without creating a strong reference cycle.

Use a weak reference when the other instance has a shorter lifetime—that is, when the other instance can be deallocated first. In the Apartment example above, it’s appropriate for an apartment to be able to have no tenant at some point in its lifetime, and so a weak reference is an appropriate way to break the reference cycle in this case. In contrast, use an unowned reference when the other instance has the same lifetime or a longer lifetime.
-->

Swift는 클래스 타입의 프로퍼티와 작업할 때 강한 참조 사이클을 해결하기 위해 2가지 방법을 제공합니다: 약한 참조 \(weak references\)와 미소유 참조 \(unowned references\).

약한 참조와 미소유 참조를 사용하면 참조 사이클의 한 인스턴스가 강한 유지 없이 다른 인스턴스를 참조할 수 있습니다. 그런 다음 인스턴스는 강한 참조 사이클을 만들지 않고도 서로를 참조할 수 있습니다.

다른 인스턴스의 수명이 더 짧은 경우 즉, 다른 인스턴스가 먼저 할당 해제될 수 있을 때 약한 참조를 사용합니다. 위의 `Apartment` 예제에서 아파트는 어느 시점에 소유자가 없을 수 있는 것이 적절하므로 이러한 경우 약한 참조는 참조 사이클을 끊는 적절한 방법입니다. 반대로 다른 인스턴스의 수명이 동일하거나 더 긴 경우 미소유 참조를 사용합니다.

### 약한 참조 \(Weak References\)

<!--
A weak reference is a reference that doesn’t keep a strong hold on the instance it refers to, and so doesn’t stop ARC from disposing of the referenced instance. This behavior prevents the reference from becoming part of a strong reference cycle. You indicate a weak reference by placing the weak keyword before a property or variable declaration.

Because a weak reference doesn’t keep a strong hold on the instance it refers to, it’s possible for that instance to be deallocated while the weak reference is still referring to it. Therefore, ARC automatically sets a weak reference to nil when the instance that it refers to is deallocated. And, because weak references need to allow their value to be changed to nil at runtime, they’re always declared as variables, rather than constants, of an optional type.

You can check for the existence of a value in the weak reference, just like any other optional value, and you will never end up with a reference to an invalid instance that no longer exists.
-->

_약한 참조 \(weak reference\)_ 는 참조하는 인스턴스를 강하게 유지하지 않는 참조이므로 ARC가 참조된 인스턴스를 처리하는 것을 중지하지 않습니다. 이러한 동작은 참조가 강한 참조 사이클의 일부가 되는 것을 방지합니다. 프로퍼티 또는 변수 선언 전에 `weak` 키워드를 위치시켜 약한 참조를 나타냅니다.

약한 참조는 참조하는 인스턴스를 강하게 유지하지 않기 때문에 약한 참조가 참조하는 동안 해당 인스턴스가 할당 해제될 수 있습니다. 따라서 ARC는 참조하는 인스턴스가 할당 해제되면 `nil` 로 약한 참조를 자동으로 설정합니다. 그리고 약한 참조는 런타임에 값을 `nil` 로 변경하는 것을 허락해야 하므로 항상 옵셔널 타입의 상수가 아닌 변수로 선언됩니다.

다른 옵셔널 값과 같이 약한 참조에 값의 존재를 확인할 수 있고 더이상 존재하지 않는 유효하지 않은 인스턴스에 참조하는 것으로 끝나지 않습니다.

<!--
NOTE
Property observers aren’t called when ARC sets a weak reference to nil.
-->

> NOTE   
> 프로퍼티 관찰자는 ARC가 약한 참조를 `nil` 로 설정할 때 호출되지 않습니다.

<!--
The example below is identical to the Person and Apartment example from above, with one important difference. This time around, the Apartment type’s tenant property is declared as a weak reference:
-->

아래의 예제는 위의 `Person` 과 `Apartment` 예제와 동일하지만 한가지 중요한 차이점이 있습니다. 이번에는 `Apartment` 타입의 `tenant` 프로퍼티는 약한 참조로 선언됩니다:

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    weak var tenant: Person?
    deinit { print("Apartment \(unit) is being deinitialized") }
}
```

<!--
The strong references from the two variables (john and unit4A) and the links between the two instances are created as before:
-->

두 변수 \(`john` 과 `unit4A`\)에서의 강한 참조와 두 인스턴스 간의 연결은 이전과 같이 생성됩니다:

```swift
var john: Person?
var unit4A: Apartment?

john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")

john!.apartment = unit4A
unit4A!.tenant = john
```

<!--
Here’s how the references look now that you’ve linked the two instances together:
-->

다음은 두 인스턴스를 함께 연결한 참조를 나타냅니다:

![Weak Reference 01](../.gitbook/assets/24_weakReference01_2x.png)

<!--
The Person instance still has a strong reference to the Apartment instance, but the Apartment instance now has a weak reference to the Person instance. This means that when you break the strong reference held by the john variable by setting it to nil, there are no more strong references to the Person instance:
-->

`Person` 인스턴스는 `Apartment` 인스턴스에 대해 아직 강한 참조를 가지고 있지만 `Apartment` 인스턴스는 이제 `Person` 인스턴스에 대해 _약한_ 참조를 가지고 있습니다. 이것은 `john` 변수에 `nil` 을 설정하여 강한 참조를 끊으면 `Person` 인스턴스에 대해 더이상 강한 참조가 아님을 의미합니다:

```swift
john = nil
// Prints "John Appleseed is being deinitialized"
```

<!--
Because there are no more strong references to the Person instance, it’s deallocated and the tenant property is set to nil:
-->

더이상 `Person` 인스턴스에 대해 강한 참조를 가지지 않기 때문에 할당 해제되고 `tenant` 프로퍼티는 `nil` 로 설정됩니다:

![Weak Reference 02](../.gitbook/assets/24_weakReference02_2x.png)

<!--
The only remaining strong reference to the Apartment instance is from the unit4A variable. If you break that strong reference, there are no more strong references to the Apartment instance:
-->

`Apartment` 인스턴스에 대한 유일한 강한 참조는 `unit4A` 변수에서 가져온 것입니다. 강한 참조를 끊으면 `Apartment` 인스턴스에 대한 강한 참조는 더이상 없습니다:

```swift
unit4A = nil
// Prints "Apartment 4A is being deinitialized"
```

<!--
Because there are no more strong references to the Apartment instance, it too is deallocated:
-->

`Apartment` 인스턴스에 대한 강한 참조가 더이상 없으므로 할당 해제됩니다:

![Weak Reference 03](../.gitbook/assets/24_weakReference03_2x.png)

<!--
NOTE
In systems that use garbage collection, weak pointers are sometimes used to implement a simple caching mechanism because objects with no strong references are deallocated only when memory pressure triggers garbage collection. However, with ARC, values are deallocated as soon as their last strong reference is removed, making weak references unsuitable for such a purpose.
-->

> NOTE   
> 가비지 콜렉션을 사용하는 시스템에서는 메모리 압력이 가비지 콜렉션을 트리거 할 때만 강한 참조가 없는 객체가 할당 해제되기 때문에 간단한 캐싱 메커니즘을 구현하는데 약한 포인터가 사용되는 경우가 있습니다. 그러나 ARC를 사용하면 마지막 강한 참조가 제거되자마자 값이 할당 해제되어 약한 참조는 이러한 목적에 적합하지 않습니다.

### 미소유 참조 \(Unowned References\)

<!--
Like a weak reference, an unowned reference doesn’t keep a strong hold on the instance it refers to. Unlike a weak reference, however, an unowned reference is used when the other instance has the same lifetime or a longer lifetime. You indicate an unowned reference by placing the unowned keyword before a property or variable declaration.

Unlike a weak reference, an unowned reference is expected to always have a value. As a result, marking a value as unowned doesn’t make it optional, and ARC never sets an unowned reference’s value to nil.
-->

약한 참조와 마찬가지로 _미소유 참조 \(unowned reference\)_ 는 참조하는 인스턴스를 강하게 유지하지 않습니다. 그러나 약한 참조와 다르게 미소유 참조는 다른 인스턴스의 수명이 같거나 더 긴 경우에 사용됩니다. 프로퍼티 또는 변수 선언 전에 `unowned` 키워드를 위치시켜 미소유 참조를 나타냅니다.

약한 참조와 달리 미소유 참조는 항상 값을 갖도록 예상됩니다. 결과적으로 미소유로 만들어진 값은 옵셔널로 만들어 지지 않고 ARC는 미소유 참조의 값을 `nil` 로 설정하지 않습니다.

<!--
IMPORTANT
Use an unowned reference only when you are sure that the reference always refers to an instance that hasn’t been deallocated.

If you try to access the value of an unowned reference after that instance has been deallocated, you’ll get a runtime error.
-->

> IMPORTANT   
> 참조가 _항상_ 할당 해제되지 않은 인스턴스를 참조한다고 확신하는 경우에만 미소유 참조를 사용합니다.
>
> 인스턴스가 할당 해제된 후에 미소유 참조의 값에 접근하려고 하면 런타임 에러가 발생합니다.

<!--
The following example defines two classes, Customer and CreditCard, which model a bank customer and a possible credit card for that customer. These two classes each store an instance of the other class as a property. This relationship has the potential to create a strong reference cycle.

The relationship between Customer and CreditCard is slightly different from the relationship between Apartment and Person seen in the weak reference example above. In this data model, a customer may or may not have a credit card, but a credit card will always be associated with a customer. A CreditCard instance never outlives the Customer that it refers to. To represent this, the Customer class has an optional card property, but the CreditCard class has an unowned (and non-optional) customer property.

Furthermore, a new CreditCard instance can only be created by passing a number value and a customer instance to a custom CreditCard initializer. This ensures that a CreditCard instance always has a customer instance associated with it when the CreditCard instance is created.

Because a credit card will always have a customer, you define its customer property as an unowned reference, to avoid a strong reference cycle:
-->

다음 예제는 은행 고객과 고객에 대한 가능한 신용카드를 모델링하는 `Customer` 와 `CreditCard` 인 2개의 클래스를 정의합니다. 이 두 클래스는 프로퍼티로 다른 클래스의 인스턴스를 각각 저장합니다. 이 관계는 강한 참조 사이클을 생성할 가능성이 있습니다.

`Customer` 와 `CreditCard` 간의 관계는 위의 약한 참조 예제에서 본 `Apartment` 와 `Person` 간의 관계와 약간 다릅니다. 이 데이터 모델은 고객은 신용카드를 가지고 있거나 가지고 있지 않을 수 있지만 신용카드는 _항상_ 고객과 연관되어 있습니다. `CreditCard` 인스턴스는 참조하는 `Customer` 보다 오래 지속되지 않습니다. 이것을 표현하기 위해 `Customer` 클래스는 옵셔널 `card` 프로퍼티를 가지지만 `CreditCard` 클래스는 미소유와 옵셔널이 아닌 `customer` 프로퍼티를 가집니다.

또한 새로운 `CreditCard` 인스턴스는 사용자 정의 `CreditCard` 초기화 구문에 `number` 값과 `customer` 인스턴스를 전달해서만 생성될 수 있습니다. 이렇게 하면 `CreditCard` 인스턴스가 생성될 때 `CreditCard` 인스턴스에 항상 연관된 `customer` 인스턴스를 가지고 있습니다.

신용카드는 항상 고객을 가지고 있으므로 강한 참조 사이클을 피하기 위해 `customer` 프로퍼티에 미소유 참조로 정의합니다:

```swift
class Customer {
    let name: String
    var card: CreditCard?
    init(name: String) {
        self.name = name
    }
    deinit { print("\(name) is being deinitialized") }
}

class CreditCard {
    let number: UInt64
    unowned let customer: Customer
    init(number: UInt64, customer: Customer) {
        self.number = number
        self.customer = customer
    }
    deinit { print("Card #\(number) is being deinitialized") }
}
```

<!--
NOTE
The number property of the CreditCard class is defined with a type of UInt64 rather than Int, to ensure that the number property’s capacity is large enough to store a 16-digit card number on both 32-bit and 64-bit systems.
-->

> NOTE   
> `CreditCard` 클래스의 `number` 프로퍼티는 `Int` 가 아닌 `UInt64` 타입으로 정의되어 `number` 프로퍼티의 용량이 32 비트와 64 비트 시스템 모두에 16 자리 카드번호를 저장할 수 있을만큼 충분히 크도록 합니다.

<!--
This next code snippet defines an optional Customer variable called john, which will be used to store a reference to a specific customer. This variable has an initial value of nil, by virtue of being optional:
-->

다음 코드는 특정 고객에 대한 참조를 저장하는데 사용되는 `john` 이라는 옵셔널 `Customer` 변수를 정의합니다. 이 변수는 옵셔널이므로 `nil` 의 초기값을 가집니다:

```swift
var john: Customer?
```

<!--
You can now create a Customer instance, and use it to initialize and assign a new CreditCard instance as that customer’s card property:
-->

이제 `Customer` 인스턴스를 생성할 수 있고 해당 고객의 `card` 프로퍼티로 새로운 `CreditCard` 인스턴스를 초기화 하고 할당하기 위해 사용할 수 있습니다:

```swift
john = Customer(name: "John Appleseed")
john!.card = CreditCard(number: 1234_5678_9012_3456, customer: john!)
```

<!--
Here’s how the references look, now that you’ve linked the two instances:
-->

다음은 두 인스턴스가 연결된 참조를 나타냅니다:

![Unowned Reference 01](../.gitbook/assets/24_unownedreference01_2x.png)

<!--
The Customer instance now has a strong reference to the CreditCard instance, and the CreditCard instance has an unowned reference to the Customer instance.

Because of the unowned customer reference, when you break the strong reference held by the john variable, there are no more strong references to the Customer instance:
-->

`Customer` 인스턴스는 이제 `CreditCard` 인스턴스에 대한 강한 참조를 가지고 있고 `CreditCard` 인스턴스는 `Customer` 인스턴스에 대해 미소유 참조를 가지고 있습니다.

`john` 변수에 의해 강한 참조를 끊을 때 미소유 `customer` 참조 때문에 `Customer` 인스턴스에 대해 더이상 강한 참조를 가지지 않습니다:

![Unowned Reference 02](../.gitbook/assets/24_unownedreference02_2x.png)

<!--
Because there are no more strong references to the Customer instance, it’s deallocated. After this happens, there are no more strong references to the CreditCard instance, and it too is deallocated:
-->

더이상 `Customer` 인스턴스에 대해 강한 참조가 아니므로 할당 해제됩니다. 후에 `CreditCard` 인스턴스에 대해 더이상 강한 참조가 아니므로 이것도 할당 해제됩니다:

```swift
john = nil
// Prints "John Appleseed is being deinitialized"
// Prints "Card #1234567890123456 is being deinitialized"
```

<!--
The final code snippet above shows that the deinitializers for the Customer instance and CreditCard instance both print their “deinitialized” messages after the john variable is set to nil.
-->

위의 마지막 코드는 `john` 변수를 `nil` 로 설정한 후에 `Customer` 인스턴스와 `CreditCard` 인스턴스 모두 "deinitialized" 메서지를 출력하는 초기화 해제 구문을 보여줍니다.

<!--
NOTE
The examples above show how to use safe unowned references. Swift also provides unsafe unowned references for cases where you need to disable runtime safety checks—for example, for performance reasons. As with all unsafe operations, you take on the responsibility for checking that code for safety.

You indicate an unsafe unowned reference by writing unowned(unsafe). If you try to access an unsafe unowned reference after the instance that it refers to is deallocated, your program will try to access the memory location where the instance used to be, which is an unsafe operation.
-->

> NOTE   
> 위의 예제는 _안전한_ 미소유 참조를 어떻게 사용해야 하는지 보여줍니다. Swift는 성능상의 이유 등으로 런타임 안전 검사를 비활성화 하는 경우에 대해 _안전하지 않은_ 미소유 참조 \(unsafe unowned references\)를 제공합니다. 모든 안전하지 않은 작업과 마찬가지로 해당 코드의 안정성을 검사하는 것에 책임을 져야 합니다.
>
> `unowned(unsafe)` 로 작성하여 안전하지 않은 미소유 참조를 나타냅니다. 참조하는 인스턴스가 할당 해제된 후에 안전하지 않은 미소유 참조에 접근하려고 하면 프로그램은 안전하지 않은 작업으로 인스턴스가 사용한 메모리 위치에 접근하려고 합니다.

### 미소유 옵셔널 참조 \(Unowned Optional References\)

<!--
You can mark an optional reference to a class as unowned. In terms of the ARC ownership model, an unowned optional reference and a weak reference can both be used in the same contexts. The difference is that when you use an unowned optional reference, you’re responsible for making sure it always refers to a valid object or is set to nil.

Here’s an example that keeps track of the courses offered by a particular department at a school:
-->

클래스에 옵셔널 참조를 미소유로 표기할 수 있습니다. ARC 소유권 모델 측면에서 미소유 옵셔널 참조 \(unowned optional reference\)와 약한 참조는 모두 같은 컨텍스트에서 사용될 수 있습니다. 차이점은 미소유 옵셔널 참조를 사용할 때 유효한 객체를 참조하거나 `nil` 로 설정되어 있는지 확인해야 합니다.

다음은 학교의 특정 과에 제공하는 과정을 추적하는 예제입니다:

```swift
class Department {
    var name: String
    var courses: [Course]
    init(name: String) {
        self.name = name
        self.courses = []
    }
}

class Course {
    var name: String
    unowned var department: Department
    unowned var nextCourse: Course?
    init(name: String, in department: Department) {
        self.name = name
        self.department = department
        self.nextCourse = nil
    }
}
```

<!--
Department maintains a strong reference to each course that the department offers. In the ARC ownership model, a department owns its courses. Course has two unowned references, one to the department and one to the next course a student should take; a course doesn’t own either of these objects. Every course is part of some department so the department property isn’t an optional. However, because some courses don’t have a recommended follow-on course, the nextCourse property is an optional.

Here’s an example of using these classes:
-->

`Department` 는 과에서 제공하는 각 과정에 강한 참조를 유지합니다. ARC 소유권 모델에서 과는 과정을 소유하고 있습니다. `Course` 는 과에 대한 것과 객체를 소유하지 않은 학생이 수강해야 하는 다음 과정에 대한 2개의 미소유 참조를 가지고 있습니다. 모든 과정은 과의 부분이므로 `department` 프로퍼티는 옵셔널이 아닙니다. 그러나 일부 과정은 후속 과정이 없기 때문에 `nextCourse` 프로퍼티는 옵셔널 입니다.

다음은 이러한 클래스를 사용하는 예제입니다:

```swift
let department = Department(name: "Horticulture")

let intro = Course(name: "Survey of Plants", in: department)
let intermediate = Course(name: "Growing Common Herbs", in: department)
let advanced = Course(name: "Caring for Tropical Plants", in: department)

intro.nextCourse = intermediate
intermediate.nextCourse = advanced
department.courses = [intro, intermediate, advanced]
```

<!--
The code above creates a department and its three courses. The intro and intermediate courses both have a suggested next course stored in their nextCourse property, which maintains an unowned optional reference to the course a student should take after completing this one.
-->

위의 코드는 과와 그 과의 3개의 과정을 생성합니다. 소개와 중급 과정 모두 `nextCourse` 프로퍼티에 저장된 다음 과정을 제안하며 이는 학생이 과정을 완료한 후 수강해야 하는 과정에 대한 미소유 옵셔널 참조를 유지합니다.

![Unowned Optional Reference](../.gitbook/assets/24_unownedOptionalReference_2x.png)

<!--
An unowned optional reference doesn’t keep a strong hold on the instance of the class that it wraps, and so it doesn’t prevent ARC from deallocating the instance. It behaves the same as an unowned reference does under ARC, except that an unowned optional reference can be nil.

Like non-optional unowned references, you’re responsible for ensuring that nextCourse always refers to a course that hasn’t been deallocated. In this case, for example, when you delete a course from department.courses you also need to remove any references to it that other courses might have.
-->

미소유 옵셔널 참조는 래핑하는 클래스의 인스턴스에 강하게 유지하지 않으므로 ARC가 인스턴스를 할당 해제하는 것을 방지하지 않습니다. 미소유 옵셔널 참조가 `nil` 이 될 수 있다는 점을 제외하고 미소유 참조는 ARC에서 수행하는 것과 동일하게 동작합니다.

옵셔널이 아닌 미소유 참조와 같이 `nextCourse` 가 항상 할당 해제되지 않은 과정을 참조하도록 해야합니다. 예를 들어 `department.courses` 에서 과정을 삭제할 때 다른 과정에 있을 수 있는 모든 참조를 삭제해야 합니다.

<!--
NOTE
The underlying type of an optional value is Optional, which is an enumeration in the Swift standard library. However, optionals are an exception to the rule that value types can’t be marked with unowned.

The optional that wraps the class doesn’t use reference counting, so you don’t need to maintain a strong reference to the optional.
-->

> NOTE   
> 옵셔널 값의 기본 타입은 Swift 표준 라이브러리에 열거형 인 `Optional` 입니다. 그러나 옵셔널은 값 타입에 `unowned` 를 표기할 수 없는 규칙에 대해 예외입니다.
>
> 클래스를 래핑한 옵셔널은 참조 카운팅을 사용하지 않으므로 강한 참조를 옵셔널로 유지할 필요가 없습니다.

### 미소유 참조와 암묵적 언래핑된 옵셔널 프로퍼티 \(Unowned References and Implicitly Unwrapped Optional Properties\)

<!--
The examples for weak and unowned references above cover two of the more common scenarios in which it’s necessary to break a strong reference cycle.

The Person and Apartment example shows a situation where two properties, both of which are allowed to be nil, have the potential to cause a strong reference cycle. This scenario is best resolved with a weak reference.

The Customer and CreditCard example shows a situation where one property that’s allowed to be nil and another property that can’t be nil have the potential to cause a strong reference cycle. This scenario is best resolved with an unowned reference.

However, there’s a third scenario, in which both properties should always have a value, and neither property should ever be nil once initialization is complete. In this scenario, it’s useful to combine an unowned property on one class with an implicitly unwrapped optional property on the other class.

This enables both properties to be accessed directly (without optional unwrapping) once initialization is complete, while still avoiding a reference cycle. This section shows you how to set up such a relationship.

The example below defines two classes, Country and City, each of which stores an instance of the other class as a property. In this data model, every country must always have a capital city, and every city must always belong to a country. To represent this, the Country class has a capitalCity property, and the City class has a country property:
-->

위에서 약한 참조와 미소유 참조에 대한 예제는 강한 참조 사이클을 중단해야 하는 일반적인 2가지 시나리오를 다룹니다.

`Person` 과 `Apartment` 예제는 둘 다 `nil` 이 될 수 있는 프로퍼티가 강한 참조 사이클을 유발할 수 있는 가능성이 있는 상황을 보여줍니다. 이 시나리오는 약한 참조로 해결하는 것이 가장 좋습니다.

`Customer` 와 `CreditCard` 예제는 `nil` 이 허용되는 하나의 프로퍼티와 `nil` 일 수 없는 프로퍼티가 강한 참조 사이클을 유발할 수 있는 가능성이 있는 상황을 보여줍니다. 이 시나리오는 미소유 참조로 해결하는 것이 가장 좋습니다.

그러나 두 프로퍼티 모두 항상 값이 있고 초기화가 완료되면 `nil` 이 되어서는 안되는 세번째 시나리오가 있습니다. 이 시나리오에서는 한 클래스의 미소유 프로퍼티를 다른 클래스에 암시적으로 언래핑된 옵셔널 프로퍼티와 결합하는 것이 유용합니다.

이렇게 하면 참조 사이클을 피하면서 초기화가 완료되면 두 프로퍼티 모두에 옵셔널 언래핑 없이 직접적으로 접근할 수 있습니다. 이 섹션에서는 이러한 관계를 어떻게 설정하는지 보여줍니다.

아래의 예제는 프로퍼티로 다른 클래스의 인스턴스를 각 저장하는 `Country` 와 `City` 인 2개의 클래스를 정의합니다. 이 데이터 모델에서 모든 국가는 항상 수도를 가지고 있고 모든 도시는 항상 국가에 속해 있어야 합니다. 이것을 표현하기 위해 `Country` 클래스는 `capitalCity` 프로퍼티를 가지고 있고 `City` 클래스는 `country` 프로퍼티를 가지고 있습니다:

```swift
class Country {
    let name: String
    var capitalCity: City!
    init(name: String, capitalName: String) {
        self.name = name
        self.capitalCity = City(name: capitalName, country: self)
    }
}

class City {
    let name: String
    unowned let country: Country
    init(name: String, country: Country) {
        self.name = name
        self.country = country
    }
}
```

<!--
To set up the interdependency between the two classes, the initializer for City takes a Country instance, and stores this instance in its country property.

The initializer for City is called from within the initializer for Country. However, the initializer for Country can’t pass self to the City initializer until a new Country instance is fully initialized, as described in Two-Phase Initialization.

To cope with this requirement, you declare the capitalCity property of Country as an implicitly unwrapped optional property, indicated by the exclamation point at the end of its type annotation (City!). This means that the capitalCity property has a default value of nil, like any other optional, but can be accessed without the need to unwrap its value as described in Implicitly Unwrapped Optionals.

Because capitalCity has a default nil value, a new Country instance is considered fully initialized as soon as the Country instance sets its name property within its initializer. This means that the Country initializer can start to reference and pass around the implicit self property as soon as the name property is set. The Country initializer can therefore pass self as one of the parameters for the City initializer when the Country initializer is setting its own capitalCity property.

All of this means that you can create the Country and City instances in a single statement, without creating a strong reference cycle, and the capitalCity property can be accessed directly, without needing to use an exclamation point to unwrap its optional value:
-->

두 클래스 간의 상호 종속성을 설정하기 위해 `City` 에 대한 초기화 구문은 `Country` 인스턴스를 가지고 있고 `country` 프로퍼티에 저장합니다.

`City` 에 대한 초기화 구문은 `Country` 에 대한 초기화 구문 내에서 호출됩니다. 그러나 `Country` 에 대한 초기화 구문은 [2단계 초기화 \(Two-Phase Initialization\)](initialization.md#2-two-phase-initialization) 에서 설명 했듯이 새로운 `Country` 인스턴스가 완벽히 초기화 될 때까지 `City` 초기화 구문에 `self` 를 전달할 수 없습니다.

이 요구사항을 처리하려면 `Country` 의 `capitalCity` 프로퍼티를 타입 설명의 끝에 느낌표 \(`City!`\)로 표시되는 암시적 언래핑된 옵셔널 프로퍼티로 선언합니다. `capitalCity` 프로퍼티는 다른 옵셔널과 같이 `nil` 의 기본값을 가지지만 [암시적 언래핑된 옵셔널 \(Implicitly Unwrapped Optionals\)](the-basics.md#implicitly-unwrapped-optionals) 에서 설명 했듯이 언래핑 할 필요없이 값에 접근할 수 있다는 의미입니다.

`capitalCity` 는 기본 `nil` 값을 가지므로 새로운 `Country` 인스턴스는 `Country` 인스턴스가 초기화 구문 내에서 `name` 프로퍼티를 설정하는 즉시 새로운 `Country`인스턴스는 완벽히 초기화 된 것으로 간주합니다. 이것은 `Country` 초기화 구문은 `name` 프로퍼티가 설정되는 즉시 암시적 `self` 프로퍼티를 참조하고 전달할 수 있다는 의미입니다. 따라서 `Country` 초기화 구문은 `capitalCity` 프로퍼티를 설정할 때 `City` 초기화 구문에 대한 하나의 파라미터로 `self` 를 전달할 수 있습니다.

이 모든 것은 강한 참조 사이클을 만들지 않고 단일 구문으로 `Country` 와 `City` 인스턴스를 생성하고 `capitalCity` 프로퍼티는 옵셔널 값을 언래핑 하기위해 느낌표를 사용할 필요없이 직접 접근할 수 있다는 의미입니다:

```swift
var country = Country(name: "Canada", capitalName: "Ottawa")
print("\(country.name)'s capital city is called \(country.capitalCity.name)")
// Prints "Canada's capital city is called Ottawa"
```

<!--
In the example above, the use of an implicitly unwrapped optional means that all of the two-phase class initializer requirements are satisfied. The capitalCity property can be used and accessed like a non-optional value once initialization is complete, while still avoiding a strong reference cycle.
-->

위의 예제에서 암시적 언래핑된 옵셔널의 사용은 모든 2단계 클래스 초기화 구문 요구사항을 충족한다는 의미입니다. `capitalCity` 프로퍼티는 초기화가 완료되면 옵셔널이 아닌 값처럼 사용되고 접근할 수 있지만 강한 참조 사이클은 피할 수 있습니다.

## 클로저에 대한 강한 참조 사이클 \(Strong Reference Cycles for Closures\)

<!--
You saw above how a strong reference cycle can be created when two class instance properties hold a strong reference to each other. You also saw how to use weak and unowned references to break these strong reference cycles.

A strong reference cycle can also occur if you assign a closure to a property of a class instance, and the body of that closure captures the instance. This capture might occur because the closure’s body accesses a property of the instance, such as self.someProperty, or because the closure calls a method on the instance, such as self.someMethod(). In either case, these accesses cause the closure to “capture” self, creating a strong reference cycle.

This strong reference cycle occurs because closures, like classes, are reference types. When you assign a closure to a property, you are assigning a reference to that closure. In essence, it’s the same problem as above—two strong references are keeping each other alive. However, rather than two class instances, this time it’s a class instance and a closure that are keeping each other alive.

Swift provides an elegant solution to this problem, known as a closure capture list. However, before you learn how to break a strong reference cycle with a closure capture list, it’s useful to understand how such a cycle can be caused.

The example below shows how you can create a strong reference cycle when using a closure that references self. This example defines a class called HTMLElement, which provides a simple model for an individual element within an HTML document:
-->

위에서 두 클래스 인스턴스 프로퍼티가 서로 강한 참조를 유지할 때 강한 참조 사이클이 어떻게 생성될 수 있는지 보았습니다. 또한 강한 참조 사이클을 끊기 위해 약한 참조와 미소유 참조를 어떻게 사용해야 하는지 보았습니다.

강한 참조 사이클은 클래스 인스턴스의 프로퍼티에 클로저를 할당하고 해당 클로저의 바디에 인스턴스를 캡처하는 경우에도 발생할 수 있습니다. 이 캡처는 클로저의 바디는 `self.someProperty` 와 같이 인스턴스의 프로퍼티에 접근하거나 클로저는 `self.someMethod()` 와 같이 인스턴스의 메서드를 호출하기 때문에 발생할 수 있습니다. 두 경우 모두 이러한 접근은 클로저가 `self` 를 "캡처" 하여 강한 참조 사이클을 생성합니다.

이 강한 참조 사이클은 클래스와 같이 클로저는 _참조 타입 \(reference types\)_ 이기 때문에 발생합니다. 프로퍼티에 클로저를 할당하면 해당 클로저에 _참조_ 를 할당하는 것입니다. 본질적으로 두 강한 참조는 서로 유지되므로 위에서와 같은 문제입니다. 그러나 두 클래스 인스턴스가 아니라 이번에는 서로 유지하는 클래스 인스턴스와 클로저입니다.

Swift는 _클로저 캡처 목록 \(closure capture list\)_ 으로 알려진 이 문제를 위해 해결책을 제공합니다. 그러나 클로저 캡처 목록으로 강한 참조 사이클을 끊는 방법에 대해 배우기 전에 이러한 사이클이 어떻게 야기되는지 이해하는 것이 더 유용합니다.

아래의 예제는 `self` 를 참조하는 클로저를 사용할 때 강한 참조 사이클이 어떻게 생성될 수 있는지 보여줍니다. 이 예제는 HTML 문서 내에 개별 요소에 대한 간단한 모델을 제공하는 `HTMLElement` 라는 클래스를 정의합니다:

```swift
class HTMLElement {

    let name: String
    let text: String?

    lazy var asHTML: () -> String = {
        if let text = self.text {
            return "<\(self.name)>\(text)</\(self.name)>"
        } else {
            return "<\(self.name) />"
        }
    }

    init(name: String, text: String? = nil) {
        self.name = name
        self.text = text
    }

    deinit {
        print("\(name) is being deinitialized")
    }

}
```

<!--
The HTMLElement class defines a name property, which indicates the name of the element, such as "h1" for a heading element, "p" for a paragraph element, or "br" for a line break element. HTMLElement also defines an optional text property, which you can set to a string that represents the text to be rendered within that HTML element.

In addition to these two simple properties, the HTMLElement class defines a lazy property called asHTML. This property references a closure that combines name and text into an HTML string fragment. The asHTML property is of type () -> String, or “a function that takes no parameters, and returns a String value”.

By default, the asHTML property is assigned a closure that returns a string representation of an HTML tag. This tag contains the optional text value if it exists, or no text content if text doesn’t exist. For a paragraph element, the closure would return "<p>some text</p>" or "<p />", depending on whether the text property equals "some text" or nil.

The asHTML property is named and used somewhat like an instance method. However, because asHTML is a closure property rather than an instance method, you can replace the default value of the asHTML property with a custom closure, if you want to change the HTML rendering for a particular HTML element.

For example, the asHTML property could be set to a closure that defaults to some text if the text property is nil, in order to prevent the representation from returning an empty HTML tag:
-->

`HTMLElement` 클래스는 제목 요소에 대한 `"h1"`, 단락 요소에 대한 `"p"`, 또는 개행 요소에 대한 `"br"` 과 같이 요소의 이름을 나타내는 `name` 프로퍼티를 정의합니다. `HTMLElement` 는 HTML 요소 내에 렌더링 될 텍스트를 나타내는 문자열을 설정할 수 있는 옵셔널 `text` 프로퍼티도 정의합니다.

이 간단한 두 프로퍼티 외에도 `HTMLElement` 클래스는 `asHTML` 이라는 지연 프로퍼티 \(lazy property\)를 정의합니다. 이 프로퍼티는 `name` 과 `text` 를 HTML 문자열 조각으로 결합하는 클로저를 참조합니다. `asHTML` 프로퍼티는 `() -> String` 또는 "파라미터가 없고 `String` 값을 반환하는 함수" 타입입니다.

기본적으로 `asHTML` 프로퍼티는 HTML 태그의 문자열 표현을 반환하는 클로저가 할당됩니다. 이 태그는 존재하면 옵셔널 `text` 값을 포함하고 `text` 가 존재하지 않으면 텍스트 콘텐츠는 없습니다. 단락 요소에 대해 클로저는 `text` 프로퍼티가 `"some text"` 또는 `nil` 인지에 따라 `"<p>some text</p>"` 또는 `"<p />"` 을 반환합니다.

`asHTML` 프로퍼티는 인스턴스 메서드와 비슷하게 이름이 지어지고 사용됩니다. 그러나 `asHTML` 은 인스턴스 메서드가 아니라 클로저 프로퍼티 이기 때문에 특정 HTML 요소에 대해 HTML 렌더링을 변경하기 원하면 사용자 정의 클로저로 `asHTML` 프로퍼티의 기본값을 대체할 수 있습니다.

예를 들어 `asHTML` 프로퍼티는 빈 HTML 태그를 반환하는 표현을 방지하기 위해 `text` 프로퍼티가 `nil` 이면 일부 텍스트를 기본값으로 하는 클로저로 설정할 수 있습니다:

```swift
let heading = HTMLElement(name: "h1")
let defaultText = "some default text"
heading.asHTML = {
    return "<\(heading.name)>\(heading.text ?? defaultText)</\(heading.name)>"
}
print(heading.asHTML())
// Prints "<h1>some default text</h1>"
```

<!--
NOTE
The asHTML property is declared as a lazy property, because it’s only needed if and when the element actually needs to be rendered as a string value for some HTML output target. The fact that asHTML is a lazy property means that you can refer to self within the default closure, because the lazy property will not be accessed until after initialization has been completed and self is known to exist.
-->

> NOTE   
> `asHTML` 프로퍼티는 요소가 실제로 일부 HTML 출력 타겟에 대한 문자열 값으로 렌더링 되어야 하는 경우에만 필요하므로 지연 프로퍼티로 선언됩니다. `asHTML` 이 지연 프로퍼티라는 사실은 초기화가 완료되고 `self` 가 존재할 때까지 접근할 수 없으므로 기본 클로저 내에서 `self` 를 참조할 수 있다는 의미입니다.

<!--
The HTMLElement class provides a single initializer, which takes a name argument and (if desired) a text argument to initialize a new element. The class also defines a deinitializer, which prints a message to show when an HTMLElement instance is deallocated.

Here’s how you use the HTMLElement class to create and print a new instance:
-->

`HTMLElement` 클래스는 `name` 인자와 필요하면 `text` 인자를 사용하여 새로운 요소를 초기화 하는 단일 초기화 구문을 제공합니다. 이 클래스는 `HTMLElement` 인스턴스가 할당 해제될 때 메세지를 출력하는 초기화 해제 구문도 정의합니다.

다음은 어떻게 `HTMLElement` 클래스가 새로운 인스턴스를 생성하고 출력하기 위해 사용되는지 보여줍니다:

```swift
var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")
print(paragraph!.asHTML())
// Prints "<p>hello, world</p>"
```

<!--
NOTE
The paragraph variable above is defined as an optional HTMLElement, so that it can be set to nil below to demonstrate the presence of a strong reference cycle.
-->

> NOTE   
> 위의 `paragraph` 변수는 강한 참조 사이클의 존재를 보여주기 위해 아래에서 `nil` 로 설정할 수 있으므로 _옵셔널_ `HTMLElement` 로 정의됩니다.

<!--
Unfortunately, the HTMLElement class, as written above, creates a strong reference cycle between an HTMLElement instance and the closure used for its default asHTML value. Here’s how the cycle looks:
-->

안타깝게도 위에서 작성한 `HTMLElement` 클래스는 `HTMLElement` 인스턴스와 기본 `asHTML` 값으로 사용된 클로저 간에 강한 참조 사이클을 생성합니다. 사이클은 다음과 같습니다:

![Closure Reference Cycle 01](../.gitbook/assets/24_closureReferenceCycle01_2x.png)

<!--
The instance’s asHTML property holds a strong reference to its closure. However, because the closure refers to self within its body (as a way to reference self.name and self.text), the closure captures self, which means that it holds a strong reference back to the HTMLElement instance. A strong reference cycle is created between the two. (For more information about capturing values in a closure, see Capturing Values.)
-->

인스턴스의 `asHTML` 프로퍼티는 클로저에 대해 강한 참조를 유지합니다. 그러나 클로저는 바디 내에서 `self.name` 과 `self.text` 를 참조하는 방법 처럼 `self` 를 참조하기 때문에 클로저는 `HTMLElement` 인스턴스에 다시 강한 참조를 유지한다는 의미로 `self` 를 _캡처_ 합니다. 둘 사이에 강한 참조 사이클이 생성됩니다 \(자세한 내용은 [캡처값 \(Capturing Values\)](closures.md#capturing-values) 을 참고 바랍니다\).

<!--
NOTE
Even though the closure refers to self multiple times, it only captures one strong reference to the HTMLElement instance.
-->

> NOTE   
> 클로저는 여러번 `self` 를 참조하지만 `HTMLElement` 인스턴스에 대한 하나의 강한 참조만 캡처합니다.

<!--
If you set the paragraph variable to nil and break its strong reference to the HTMLElement instance, neither the HTMLElement instance nor its closure are deallocated, because of the strong reference cycle:
-->

`paragraph` 변수를 `nil` 로 설정하고 `HTMLElement` 인스턴스에 대한 강한 참조를 끊으면 강한 참조 사이클 때문에 `HTMLElement` 인스턴스와 해당 클로저는 할당 해제되지 않습니다:

```swift
paragraph = nil
```

<!--
Note that the message in the HTMLElement deinitializer isn’t printed, which shows that the HTMLElement instance isn’t deallocated.
-->

`HTMLElement` 초기화 해제 구문에 메세지는 출력되지 않으며 `HTMLElement` 인스턴스는 할당 해제 되지 않음을 보여줍니다.

## 클로저에 대한 강한 참조 사이클 해결 \(Resolving Strong Reference Cycles for Closures\)

<!--
You resolve a strong reference cycle between a closure and a class instance by defining a capture list as part of the closure’s definition. A capture list defines the rules to use when capturing one or more reference types within the closure’s body. As with strong reference cycles between two class instances, you declare each captured reference to be a weak or unowned reference rather than a strong reference. The appropriate choice of weak or unowned depends on the relationships between the different parts of your code.
-->

클로저의 정의의 부분으로 _캡처 목록 \(capture list\)_ 을 정의하여 클로저와 클래스 인스턴스 간의 강한 참조 사이클을 해결합니다. 캡처 목록은 클로저의 바디 내에서 하나 이상의 참조 타입을 캡처할 때 사용할 규칙을 정의합니다. 두 클래스 간의 강한 참조 사이클과 마찬가지로 캡처된 각 참조를 강한 참조가 아닌 약한 참조 또는 미소유 참조로 선언합니다. 약한 참조 또는 미소유 참조의 적절한 선택은 코드의 다른 부분 간의 관계에 따라 다릅니다.

<!--
NOTE
Swift requires you to write self.someProperty or self.someMethod() (rather than just someProperty or someMethod()) whenever you refer to a member of self within a closure. This helps you remember that it’s possible to capture self by accident.
-->

> NOTE   
> Swift는 클로저 내에서 `self` 의 멤버를 참조할 때마다 `someProperty` 또는 `someMethod()` 가 아닌 `self.someProperty` 또는 `self.someMethod()` 로 작성해야 합니다. 이것은 실수로 `self` 를 캡첩할 수 있는 것을 기억하는데 도움이 됩니다.

### 캡처 목록 정의 \(Defining a Capture List\)

<!--
Each item in a capture list is a pairing of the weak or unowned keyword with a reference to a class instance (such as self) or a variable initialized with some value (such as delegate = self.delegate). These pairings are written within a pair of square braces, separated by commas.

Place the capture list before a closure’s parameter list and return type if they’re provided:
-->

캡처 목록에서 각 항목은 `self` 처럼 클래스 인스턴스 또는 `delegate = self.delegate` 와 같은 어떤 값을 초기화된 변수에 대한 참조가 있는 `weak` 또는 `unowned` 키워드와 쌍을 이룹니다. 이 쌍은 콤마로 구분하여 대괄호 내에 작성됩니다.

캡처 목록은 클로저의 파라미터 목록 전에 위치하고 반환 타입이 있다면 파라미터 목록 다음에 위치합니다:

```swift
lazy var someClosure = {
    [unowned self, weak delegate = self.delegate]
    (index: Int, stringToProcess: String) -> String in
    // closure body goes here
}
```

<!--
If a closure doesn’t specify a parameter list or return type because they can be inferred from context, place the capture list at the very start of the closure, followed by the in keyword:
-->

클로저는 컨텍스트로 부터 유추할 수 있기 때문에 파라미터 목록 또는 반환 타입을 지정하지 않으면 캡처 목록은 클로저의 가장 처음에 위치하고 이어서 `in` 키워드가 옵니다:

```swift
lazy var someClosure = {
    [unowned self, weak delegate = self.delegate] in
    // closure body goes here
}
```

### 약한 참조와 미소유 참조 \(Weak and Unowned References\)

<!--
Define a capture in a closure as an unowned reference when the closure and the instance it captures will always refer to each other, and will always be deallocated at the same time.

Conversely, define a capture as a weak reference when the captured reference may become nil at some point in the future. Weak references are always of an optional type, and automatically become nil when the instance they reference is deallocated. This enables you to check for their existence within the closure’s body.
-->

클로저와 캡처한 인스턴스가 항상 서로를 참조하고 항상 같은 시간에 할당 해제될 때 클로저의 캡처를 미소유 참조로 정의합니다.

반대로 캡처된 참조가 향후에 `nil` 이 될 때 약한 참조로 캡처를 정의합니다. 약한 참조는 항상 옵셔널 타입이고 참조하는 인스턴스가 할당 해제되면 자동으로 `nil` 이 됩니다. 이를 사용하여 클로저의 바디 내에서 존재하는지 확인할 수 있습니다.

<!--
NOTE
If the captured reference will never become nil, it should always be captured as an unowned reference, rather than a weak reference.
-->

> NOTE   
> 캡처된 참조가 `nil` 이 되지 않으면 약한 참조보다 미소유 참조로 항상 캡처되어야 합니다.

<!--
An unowned reference is the appropriate capture method to use to resolve the strong reference cycle in the HTMLElement example from Strong Reference Cycles for Closures above. Here’s how you write the HTMLElement class to avoid the cycle:
-->

미소유 참조는 위의 [클로저에 대한 강한 참조 사이클 \(Strong Reference Cycles for Closures\)](automatic-reference-counting.md#strong-reference-cycles-for-closures) 에서 `HTMLElement` 예제에서 강한 참조 사이클을 해결하기 위해 사용할 적절한 캡처 방법입니다. 다음은 사이클을 피하기 위해 `HTMLElement` 클래스를 어떻게 작성하는지 보여줍니다:

```swift
class HTMLElement {

    let name: String
    let text: String?

    lazy var asHTML: () -> String = {
        [unowned self] in
        if let text = self.text {
            return "<\(self.name)>\(text)</\(self.name)>"
        } else {
            return "<\(self.name) />"
        }
    }

    init(name: String, text: String? = nil) {
        self.name = name
        self.text = text
    }

    deinit {
        print("\(name) is being deinitialized")
    }

}
```

<!--
This implementation of HTMLElement is identical to the previous implementation, apart from the addition of a capture list within the asHTML closure. In this case, the capture list is [unowned self], which means “capture self as an unowned reference rather than a strong reference”.

You can create and print an HTMLElement instance as before:
-->

`HTMLElement` 의 구현은 `asHTML` 클로저 내에서 캡처 목록의 추가를 제외하면 이전 구현과 동일합니다. 이러한 경우에 캡처 목록은 "강한 참조가 아닌 미소유 참조로 `self` 를 캡처합니다" 라는 의미의 `[unowned self]` 입니다.

이전과 같이 `HTMLElement` 인스턴스를 생성하고 출력할 수 있습니다:

```swift
var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")
print(paragraph!.asHTML())
// Prints "<p>hello, world</p>"
```

<!--
Here’s how the references look with the capture list in place:
-->

다음은 캡처 목록이 참조에서 어떻게 보이는지 나타냅니다:

![Closure Reference Cycle 02](../.gitbook/assets/24_closureReferenceCycle02_2x.png)

<!--
This time, the capture of self by the closure is an unowned reference, and doesn’t keep a strong hold on the HTMLElement instance it has captured. If you set the strong reference from the paragraph variable to nil, the HTMLElement instance is deallocated, as can be seen from the printing of its deinitializer message in the example below:
-->

이번에는 클로저에 의해 `self` 의 캡처는 미소유 참조이고 캡처한 `HTMLElement` 인스턴스를 강하게 유지하지 않습니다. `paragraph` 변수를 `nil` 로 강한 참조를 설정하면 아래 예제에서 초기화 해제 구문 메세지를 출력하는 것을 확인했듯이 `HTMLElement` 인스턴스는 할당 해제 됩니다.

```swift
paragraph = nil
// Prints "p is being deinitialized"
```

<!--
For more information about capture lists, see Capture Lists.
-->

더 자세한 내용은 [캡처 목록 \(Capture Lists\)](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#ID544) 을 참고 바랍니다.

