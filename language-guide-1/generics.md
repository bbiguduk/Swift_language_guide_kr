# 제너릭 \(Generics\)

<!--
Generic code enables you to write flexible, reusable functions and types that can work with any type, subject to requirements that you define. You can write code that avoids duplication and expresses its intent in a clear, abstracted manner.

Generics are one of the most powerful features of Swift, and much of the Swift standard library is built with generic code. In fact, you’ve been using generics throughout the Language Guide, even if you didn’t realize it. For example, Swift’s Array and Dictionary types are both generic collections. You can create an array that holds Int values, or an array that holds String values, or indeed an array for any other type that can be created in Swift. Similarly, you can create a dictionary to store values of any specified type, and there are no limitations on what that type can be.
-->

_제너릭 코드 \(Generic code\)_ 는 정의한 요구사항에 따라 모든 타입에서 동작할 수 있는 유연하고 재사용 가능한 함수와 타입을 작성할 수 있습니다. 중복을 피하고 명확하고 추상적인 방식으로 의도를 표현하는 코드를 작성할 수 있습니다.

제너릭은 Swift의 강력한 특징 중 하나이고 Swift 표준 라이브러리 대부분은 제너릭 코드로 되어 있습니다. 사실 모르고 있더라도 _Language Guide_ 전체에서 제너릭을 사용합니다. 예를 들어 Swift의 `Array` 와 `Dictionary` 타입은 둘다 제너릭 콜렉션 입니다. `Int` 값을 가진 배열, 또는 `String` 값을 가진 배열 또는 실제로 Swift에서 생성될 수 있는 다른 모든 타입에 대한 배열을 생성할 수 있습니다. 마찬가지로 모든 지정된 타입의 값을 저장하기 위한 딕져너리를 생성할 수 있고 해당 타입에 대한 제한은 없습니다.

## 제너릭이 해결하는 문제 \(The Problem That Generics Solve\)

<!--
Here’s a standard, nongeneric function called swapTwoInts(_:_:), which swaps two Int values:
-->

다음은 두 `Int` 값을 바꾸는 `swapTwoInts(_:_:)` 라는 제너릭이 아닌 함수, 표준입니다:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

<!--
This function makes use of in-out parameters to swap the values of a and b, as described in In-Out Parameters.

The swapTwoInts(_:_:) function swaps the original value of b into a, and the original value of a into b. You can call this function to swap the values in two Int variables:
-->

이 함수는 [In-Out 파라미터 \(In-Out Parameters\)](functions.md#in-out-in-out-parameters) 에서 설명한 대로 `a` 와 `b` 의 값을 바꾸기 위해 in-out 파라미터를 사용하여 만듭니다.

`swapTwoInts(_:_:)` 함수는 `b` 의 값을 `a` 로 그리고 `a` 의 값을 `b` 로 바꿉니다. 2개의 `Int` 변수의 값을 바꾸기 위해 이 함수를 호출할 수 있습니다:

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// Prints "someInt is now 107, and anotherInt is now 3"
```

<!--
The swapTwoInts(_:_:) function is useful, but it can only be used with Int values. If you want to swap two String values, or two Double values, you have to write more functions, such as the swapTwoStrings(_:_:) and swapTwoDoubles(_:_:) functions shown below:
-->

`swapTwoInts(_:_:)` 함수는 유용하지만 `Int` 값만 사용이 가능합니다. 2개의 `String` 값 또는 2개의 `Double` 값을 바꾸길 원하면 아래의 `swapTwoStrings(_:_:)` 와 `swapTwoDoubles(_:_:)` 함수와 같이 더 많은 함수를 작성해야 합니다:

```swift
func swapTwoStrings(_ a: inout String, _ b: inout String) {
    let temporaryA = a
    a = b
    b = temporaryA
}

func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

<!--
You may have noticed that the bodies of the swapTwoInts(_:_:), swapTwoStrings(_:_:), and swapTwoDoubles(_:_:) functions are identical. The only difference is the type of the values that they accept (Int, String, and Double).

It’s more useful, and considerably more flexible, to write a single function that swaps two values of any type. Generic code enables you to write such a function. (A generic version of these functions is defined below.)
-->

`swapTwoInts(_:_:)`, `swapTwoStrings(_:_:)`, 그리고 `swapTwoDoubles(_:_:)` 함수의 바디가 동일하다는 것을 알 수 있습니다. 차이점은 받아들이는 값의 타입만 \(`Int`, `String`, 그리고 `Double`\) 다릅니다.

_모든 타입_ 의 2개의 값을 바꾸는 단일 함수로 작성하면 더 유용하고 더 유연합니다. 제너릭 코드는 이러한 함수를 작성할 수 있습니다 \(이 함수의 제너릭 버전은 아래에 정의됩니다\).

<!--
NOTE
In all three functions, the types of a and b must be the same. If a and b aren’t of the same type, it isn’t possible to swap their values. Swift is a type-safe language, and doesn’t allow (for example) a variable of type String and a variable of type Double to swap values with each other. Attempting to do so results in a compile-time error.
-->

> NOTE  
> 이 3개의 함수는 `a` 와 `b` 의 타입이 모두 같아야 합니다. `a` 와 `b` 가 같은 타입이 아니면 바꾸는 것은 불가능합니다. Swift는 타입 안정성 언어이고 `String` 타입의 변수와 `Double` 타입의 변수가 서로 값을 바꾸도록 허락하지 않습니다. 이러한 시도는 컴파일 에러가 발생합니다.

## 제너릭 함수 \(Generic Functions\)

<!--
Generic functions can work with any type. Here’s a generic version of the swapTwoInts(_:_:) function from above, called swapTwoValues(_:_:):
-->

_제너릭 함수 \(Generic functions\)_ 는 모든 타입과 함께 동작할 수 있습니다. 다음은 `swapTwoValues(_:_:)` 라는 위의 `swapTwoInts(_:_:)` 함수의 제너릭 버전입니다:

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

<!--
The body of the swapTwoValues(_:_:) function is identical to the body of the swapTwoInts(_:_:) function. However, the first line of swapTwoValues(_:_:) is slightly different from swapTwoInts(_:_:). Here’s how the first lines compare:
-->

`swapTwoValues(_:_:)` 함수의 바디는 `swapTwoInts(_:_:)` 함수의 바디와 동일합니다. 그러나 `swapTwoValues(_:_:)` 의 첫번째 줄은 `swapTwoInts(_:_:)` 와 약간 다릅니다. 다음은 첫번째 줄 차이의 비교입니다:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int)
func swapTwoValues<T>(_ a: inout T, _ b: inout T)
```

<!--
The generic version of the function uses a placeholder type name (called T, in this case) instead of an actual type name (such as Int, String, or Double). The placeholder type name doesn’t say anything about what T must be, but it does say that both a and b must be of the same type T, whatever T represents. The actual type to use in place of T is determined each time the swapTwoValues(_:_:) function is called.

The other difference between a generic function and a nongeneric function is that the generic function’s name (swapTwoValues(_:_:)) is followed by the placeholder type name (T) inside angle brackets (<T>). The brackets tell Swift that T is a placeholder type name within the swapTwoValues(_:_:) function definition. Because T is a placeholder, Swift doesn’t look for an actual type called T.

The swapTwoValues(_:_:) function can now be called in the same way as swapTwoInts, except that it can be passed two values of any type, as long as both of those values are of the same type as each other. Each time swapTwoValues(_:_:) is called, the type to use for T is inferred from the types of values passed to the function.

In the two examples below, T is inferred to be Int and String respectively:
-->

함수의 제너릭 버전은 `Int`, `String`, 또는 `Double` 와 같은 _실제_ 타입 이름 대신에 이 경우 `T` 라는 _임의의_ 타입 이름을 사용합니다. 이 임의의 타입 이름은 `T` 가 무엇이어야 하는지 아무 말도 하지 않지만 `T` 가 무엇을 나타내든 `a` 와 `b` 는 모두 같은 타입 `T` 여야 한다고 말합니다. `T` 의 실제 타입은 `swapTwoValues(_:_:)` 함수가 호출될 때마다 결정됩니다.

제너릭 함수와 제너릭이 아닌 함수 사이의 다른 차이점은 제너릭 함수의 이름 \(`swapTwoValues(_:_:)`\)에 바로 임의의 타입 이름 \(`T`\)이 꺾쇠 괄호 내 \(`<T>`\)에 위치한다는 것입니다. 이 괄호는 `T` 는 `swapTwoValues(_:_:)` 함수 정의 내에서 임의의 타입 이름이라고 Swift에게 말합니다. `T` 는 임의의 타입이므로 Swift는 `T` 라는 실제 타입을 찾지 않습니다.

`swapTwoValues(_:_:)` 함수는 이제 `swapTwoInts` 와 동일한 방식으로 호출될 수 있지만 두 값이 서로 동일한 타입이면 모든 타입의 두 값을 전달할 수 있다는 점이 다릅니다. `swapTwoValues(_:_:)` 가 호출될 때마다 `T` 로 사용한 타입은 함수에 전달된 값의 타입으로 부터 유추됩니다.

아래의 2개의 예제에서 `T` 는 각각 `Int` 와 `String` 으로 추론됩니다:

```swift
var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)
// someInt is now 107, and anotherInt is now 3

var someString = "hello"
var anotherString = "world"
swapTwoValues(&someString, &anotherString)
// someString is now "world", and anotherString is now "hello"
```

<!--
NOTE
The swapTwoValues(_:_:) function defined above is inspired by a generic function called swap, which is part of the Swift standard library, and is automatically made available for you to use in your apps. If you need the behavior of the swapTwoValues(_:_:) function in your own code, you can use Swift’s existing swap(_:_:) function rather than providing your own implementation.
-->

> NOTE  
> 위에 정의된 `swapTwoValues(_:_:)` 함수는 Swift 표준 라이브러리의 부분이고 앱에서 사용하기 위해 자동으로 만들어지는 `swap` 이라는 제너릭 함수에 의해 영감을 얻었습니다. 자체 코드에서 `swapTwoValues(_:_:)` 함수의 동작이 필요하다면 직접 구현한 함수보다 Swift에 존재하는 `swap(_:_:)` 함수를 사용할 수 있습니다.

## 타입 파라미터 \(Type Parameters\)

<!--
In the swapTwoValues(_:_:) example above, the placeholder type T is an example of a type parameter. Type parameters specify and name a placeholder type, and are written immediately after the function’s name, between a pair of matching angle brackets (such as <T>).

Once you specify a type parameter, you can use it to define the type of a function’s parameters (such as the a and b parameters of the swapTwoValues(_:_:) function), or as the function’s return type, or as a type annotation within the body of the function. In each case, the type parameter is replaced with an actual type whenever the function is called. (In the swapTwoValues(_:_:) example above, T was replaced with Int the first time the function was called, and was replaced with String the second time it was called.)

You can provide more than one type parameter by writing multiple type parameter names within the angle brackets, separated by commas.
-->

위의 `swapTwoValues(_:_:)` 예제에서 임의의 타입 `T` 는 _타입 파라미터 \(type parameter\)_ 의 예입니다. 타입 파라미터는 임의의 타입을 지정하고 이름을 지정하며 꺾쇠 괄호 \(예: `<T>`\) 사이에 기록하고 함수의 이름 바로 뒤에 작성됩니다.

타입 파라미터를 지정하면 함수의 파라미터 \(`swapTwoValues(_:_:)` 함수의 `a` 와 `b` 와 같이\) 의 타입을 정의하기 위해 사용하거나 함수의 반환 타입이나 함수의 바디내에 타입 주석으로 사용할 수 있습니다. 각각의 경우 타입 파라미터는 함수가 호출될 때마다 _실제_ 타입으로 대체됩니다 \(위의 예 `swapTwoValues(_:_:)` 에서 `T` 는 첫번째 함수가 호출될 때 `Int` 로 대체되고 두번째 호출될 때 `String` 으로 대체됩니다\).

콤마로 구분된 꺾쇠 괄호 안에 여러개 타입 파라미터를 작성하여 하나 이상의 타입 파라미터를 제공할 수 있습니다.

## 타입 파라미터 이름 \(Naming Type Parameters\)

<!--
In most cases, type parameters have descriptive names, such as Key and Value in Dictionary<Key, Value> and Element in Array<Element>, which tells the reader about the relationship between the type parameter and the generic type or function it’s used in. However, when there isn’t a meaningful relationship between them, it’s traditional to name them using single letters such as T, U, and V, such as T in the swapTwoValues(_:_:) function above.
-->

대부분의 경우 타입 파라미터는 타입 파라미터와 제너릭 타입 간의 관계나 함수 간의 관계를 나타내기 위해 `Dictionary<Key, Value>` 에서 `Key` 와 `Value` 그리고 `Array<Element>` 에서 `Element` 와 같이 설명이 포함된 이름이 있습니다. 그러나 의미있는 관계가 없을 때는 위에서 `swapTwoValues(_:_:)` 함수에서 `T` 와 같이 `T`, `U`, 그리고 `V` 와 같은 단일 문자를 사용하여 이름을 지정하는 것이 일반적입니다.

<!--
NOTE
Always give type parameters upper camel case names (such as T and MyTypeParameter) to indicate that they’re a placeholder for a type, not a value.
-->

> NOTE  
> 값이 아니라 타입에 대한 임의의 표시라는 것을 나타내기 위해 항상 타입 파라미터가 주어질 때 대문자 이름 \(`T` 와 `MyTypeParameter` 와 같은\)으로 주어집니다.

## 제너릭 타입 \(Generic Types\)

<!--
In addition to generic functions, Swift enables you to define your own generic types. These are custom classes, structures, and enumerations that can work with any type, in a similar way to Array and Dictionary.

This section shows you how to write a generic collection type called Stack. A stack is an ordered set of values, similar to an array, but with a more restricted set of operations than Swift’s Array type. An array allows new items to be inserted and removed at any location in the array. A stack, however, allows new items to be appended only to the end of the collection (known as pushing a new value on to the stack). Similarly, a stack allows items to be removed only from the end of the collection (known as popping a value off the stack).
-->

제너릭 함수 외에도 Swift는 고유한 _제너릭 타입 \(generic types\)_ 을 정의할 수 있습니다. 이것은 `Array` 와 `Dictionary` 와 유사한 방식으로 _모든 타입_ 에서 동작할 수 있는 사용자 정의 클래스, 구조체, 그리고 열거형입니다.

이번 섹션은 `Stack` 이라는 제너릭 콜렉션 타입을 어떻게 작성하는지 보여줍니다. 스택은 배열과 유사하지만 Swift의 `Array` 타입보다 더 제한된 작업 집합을 가진 순서가 지정된 집합입니다. 배열은 모든 위치에서 새 항목을 삽입하고 제거할 수 있습니다. 그러나 스택은 새로운 항목이 콜렉션의 끝에 추가하는 것만 허락합니다 \(스택에 새로운 값을 _푸쉬 \(pushing\)_ 한다고 알려져 있음\). 마찬가지로 스택은 콜렉션의 끝부분의 항목만 제거할 수 있습니다 \(스택에서 값을 _팝 \(popping\)_ 한다고 알려져 있음\).

<!--
NOTE
The concept of a stack is used by the UINavigationController class to model the view controllers in its navigation hierarchy. You call the UINavigationController class pushViewController(_:animated:) method to add (or push) a view controller on to the navigation stack, and its popViewControllerAnimated(_:) method to remove (or pop) a view controller from the navigation stack. A stack is a useful collection model whenever you need a strict “last in, first out” approach to managing a collection.
-->

> NOTE  
> 스택의 개념은 네비게이션 계층도에서 뷰 컨트롤러를 모델링 하는 `UINavigationController` 클래스에 의해 사용됩니다. `UINavigationController` 클래스에서 네비게이션 스택에 뷰 컨트롤러를 추가 또는 푸쉬 하기 위해 `pushViewController(_:animated:)` 메서드를 호출하고 네비게이션 스택에 뷰 컨트롤러를 삭제 또는 팝 하기 위해 `popViewControllerAnimated(_:)` 메서드를 호출합니다. 스택은 콜렉션 관리에 대한 엄격한 "후입 선출" 접근 방식이 필요할 때 유용한 콜렉션 모델입니다.

<!--
The illustration below shows the push and pop behavior for a stack:
-->

아래의 그림은 스택에 대한 푸쉬와 팝에 대한 동작을 보여줍니다:

![Stack Push Pop](../.gitbook/assets/22_stackPushPop_2x.png)

<!--
1. There are currently three values on the stack.
2. A fourth value is pushed onto the top of the stack.
3. The stack now holds four values, with the most recent one at the top.
4. The top item in the stack is popped.
5. After popping a value, the stack once again holds three values.

Here’s how to write a nongeneric version of a stack, in this case for a stack of Int values:
-->

1. 스택에 현재 3개의 값이 있습니다.
2. 4번째 값은 스택의 상단에 푸쉬됩니다.
3. 스택은 이제 4개의 값을 가지며 최근값이 가장 상단에 위치합니다.
4. 스택의 최상단 항목은 팝 됩니다.
5. 값을 팝 한 후에 스택은 다시 3개의 값만 가집니다.

다음은 스택의 제너릭이 아닌 버전을 어떻게 작성하는지 나타내며 이 경우 `Int` 값의 스택에 대한 것을 보여줍니다:

```swift
struct IntStack {
    var items: [Int] = []
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
}
```

<!--
This structure uses an Array property called items to store the values in the stack. Stack provides two methods, push and pop, to push and pop values on and off the stack. These methods are marked as mutating, because they need to modify (or mutate) the structure’s items array.

The IntStack type shown above can only be used with Int values, however. It would be much more useful to define a generic Stack structure, that can manage a stack of any type of value.

Here’s a generic version of the same code:
-->

이 구조체는 스택에 값을 저장하기 위해 `items` 라는 `Array` 프로퍼티를 사용합니다. `Stack` 은 스택에 값을 푸쉬하고 팝하기 위해 `push` 와 `pop` 인 2개의 메서드를 제공합니다. 구조체의 `items` 배열을 수정하거나 변경이 필요하므로 이 메서드는 `mutating` 으로 표시되어 있습니다.

그러나 위에 `IntStack` 타입은 `Int` 값만 사용할 수 있습니다. _모든_ 타입의 값으로 스택을 관리할 수 있는 제너릭 `Stack` 구조체를 정의하는 것이 더 유용합니다.

다음은 같은 코드의 제너릭 버전입니다:

```swift
struct Stack<Element> {
    var items: [Element] = []
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
```

<!--
Note how the generic version of Stack is essentially the same as the nongeneric version, but with a type parameter called Element instead of an actual type of Int. This type parameter is written within a pair of angle brackets (<Element>) immediately after the structure’s name.

Element defines a placeholder name for a type to be provided later. This future type can be referred to as Element anywhere within the structure’s definition. In this case, Element is used as a placeholder in three places:

* To create a property called items, which is initialized with an empty array of values of type Element
* To specify that the push(_:) method has a single parameter called item, which must be of type Element
* To specify that the value returned by the pop() method will be a value of type Element

Because it’s a generic type, Stack can be used to create a stack of any valid type in Swift, in a similar manner to Array and Dictionary.

You create a new Stack instance by writing the type to be stored in the stack within angle brackets. For example, to create a new stack of strings, you write Stack<String>():
-->

`Stack` 의 제너릭 버전은 기본적으로 제너릭이 아닌 버전과 동일하지만 `Int` 의 실제 타입 대신에 `Element` 라는 타입 파라미터를 사용합니다. 이 타입 파라미터는 구조체의 이름 바로 뒤에 꺾쇠 괄호 내에 \(`<Element>`\) 작성됩니다.

`Element` 는 나중에 제공할 타입에 대한 임의의 이름을 정의합니다. 이 미래의 타입은 구조체의 정의 내 어디서나 `Element` 로 참조될 수 있습니다. 이 경우에 `Element` 는 아래의 3군데에서 임의로 사용됩니다:

* `Element` 타입의 값으로 빈 배열로 초기화 되는 `items` 라는 프로퍼티를 생성할 때
* `Element` 타입이어야 하는 `item` 이라는 단일 파라미터를 가지는 `push(_:)` 메서드를 지정할 때
* `pop()` 메서드에 의해 반환된 값이 `Element` 타입의 값으로 지정할 때

제너릭 타입이므로 `Stack` 은 `Array` 와 `Dictionary` 와 유사하게 Swift에서 _모든_ 유효한 타입의 스택을 생성하기 위해 사용될 수 있습니다.

꺾쇠 괄호 내에 스택에 저장될 타입을 작성하여 새로운 `Stack` 인스턴스를 생성합니다. 예를 들어 문자열에 새로운 스택을 생성하려면 `Stack<String>()` 이라 작성합니다:

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")
stackOfStrings.push("cuatro")
// the stack now contains 4 strings
```

<!--
Here’s how stackOfStrings looks after pushing these four values on to the stack:
-->

다음은 `stackOfStrings` 가 스택에 4개의 값을 푸쉬한 후를 나타냅니다:

![Stack Pushed Four Strings](../.gitbook/assets/22_stackPushedFourStrings_2x.png)

<!--
Popping a value from the stack removes and returns the top value, "cuatro":
-->

스택에 값을 팝하여 제거하고 반환되는 값은 `"cuatro"` 입니다:

```swift
 let fromTheTop = stackOfStrings.pop()
// fromTheTop is equal to "cuatro", and the stack now contains 3 strings
```

<!--
Here’s how the stack looks after popping its top value:
-->

다음은 스택이 값을 팝한 후를 보여줍니다:

![Stack Poped One String](../.gitbook/assets/22_stackPoppedOneString_2x.png)

## 제너릭 타입 확장 \(Extending a Generic Type\)

<!--
When you extend a generic type, you don’t provide a type parameter list as part of the extension’s definition. Instead, the type parameter list from the original type definition is available within the body of the extension, and the original type parameter names are used to refer to the type parameters from the original definition.

The following example extends the generic Stack type to add a read-only computed property called topItem, which returns the top item on the stack without popping it from the stack:
-->

제너릭 타입을 확장할 때 확장의 정의의 부분으로 타입 파라미터 목록을 제공하지 않습니다. 대신에 _기존_ 타입 정의에서 타입 파라미터 목록은 확장의 바디내에서 가능하고 기존 타입 파라미터 이름은 기존 정의에서 타입 파라미터를 참조하는데 사용됩니다.

다음의 예제는 스택에 팝 없이 스택의 가장 상단의 항목을 반환하는 `topItem` 이라는 읽기전용 계산된 프로퍼티를 추가하기 위해 제너릭 `Stack` 타입을 확장합니다:

```swift
extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}
```

<!--
The topItem property returns an optional value of type Element. If the stack is empty, topItem returns nil; if the stack isn’t empty, topItem returns the final item in the items array.

Note that this extension doesn’t define a type parameter list. Instead, the Stack type’s existing type parameter name, Element, is used within the extension to indicate the optional type of the topItem computed property.

The topItem computed property can now be used with any Stack instance to access and query its top item without removing it.
-->

`topItem` 프로퍼티는 `Element` 타입의 옵셔널 값을 반환합니다. 스택이 비어있다면 `topItem` 은 `nil` 을 반환하고 스택이 비어 있지 않다면 `topItem` 은 `items` 배열에서 마지막 항목을 반환합니다.

이 확장은 타입 파라미터 목록을 정의하지 않습니다. 대신에 `Element` 라는 `Stack` 타입의 존재하는 타입 파라미터 이름은 `topItem` 계산된 프로퍼티의 옵셔널 타입임을 나타내기 위해 확장 내에서 사용됩니다.

`topItem` 계산된 프로퍼티는 최상단 항목의 삭제없이 접근하고 조회하기 위해 모든 `Stack` 인스턴스에서 사용될 수 있습니다.

```swift
if let topItem = stackOfStrings.topItem {
    print("The top item on the stack is \(topItem).")
}
// Prints "The top item on the stack is tres."
```

<!--
Extensions of a generic type can also include requirements that instances of the extended type must satisfy in order to gain the new functionality, as discussed in Extensions with a Generic Where Clause below.
-->

제너릭 타입의 확장은 아래 [제너릭 Where 절의 확장 \(Extensions with a Generic Where Clause\)](generics.md#where-extensions-with-a-generic-where-clause) 에서 설명 하듯이 확장된 타입의 인스턴스는 새로운 기능을 얻기 위해 충족해야 할 요구사항도 포함할 수 있습니다.

## 타입 제약 \(Type Constraints\)

<!--
The swapTwoValues(_:_:) function and the Stack type can work with any type. However, it’s sometimes useful to enforce certain type constraints on the types that can be used with generic functions and generic types. Type constraints specify that a type parameter must inherit from a specific class, or conform to a particular protocol or protocol composition.

For example, Swift’s Dictionary type places a limitation on the types that can be used as keys for a dictionary. As described in Dictionaries, the type of a dictionary’s keys must be hashable. That is, it must provide a way to make itself uniquely representable. Dictionary needs its keys to be hashable so that it can check whether it already contains a value for a particular key. Without this requirement, Dictionary couldn’t tell whether it should insert or replace a value for a particular key, nor would it be able to find a value for a given key that’s already in the dictionary.

This requirement is enforced by a type constraint on the key type for Dictionary, which specifies that the key type must conform to the Hashable protocol, a special protocol defined in the Swift standard library. All of Swift’s basic types (such as String, Int, Double, and Bool) are hashable by default. For information about making your own custom types conform to the Hashable protocol, see Conforming to the Hashable Protocol.

You can define your own type constraints when creating custom generic types, and these constraints provide much of the power of generic programming. Abstract concepts like Hashable characterize types in terms of their conceptual characteristics, rather than their concrete type.
-->

`swapTwoValues(_:_:)` 함수와 `Stack` 타입은 모든 타입과 함께 동작 가능합니다. 그러나 가끔 제너릭 함수와 제너릭 타입으로 사용될 수 있는 타입에 _타입 제약 \(type constraints\)_ 을 강제로 포함해야 유용할 수 있습니다. 타입 제약은 타입 파라미터가 특정 클래스를 상속하거나 특정 프로토콜 또는 프로토콜 구성을 준수해야 함을 지정해야 합니다.

예를 들어 Swift의 `Dictionary` 타입은 딕셔너리에 대해 키로 사용될 수 있는 타입에 제한을 둡니다. [딕셔너리 \(Dictionaries\)](collection-types.md#dictionaries) 에서 설명 했듯이 딕셔너리 키의 타입은 _hashable_ 이어야 합니다. 즉, 고유하게 표현할 수 있는 방법을 제공해야 합니다. `Dictionary` 는 특정 키에 대해 값이 이미 포함되어 있는지 확인할 수 있도록 키를 해시 할 수 있어야 합니다. 이 요구사항이 없으면 `Dictionary` 는 특정 키에 대해 값을 삽입 또는 대체해야 하는지 알 수 없으며 이미 딕셔너리에 있는 주어진 키에 대한 값을 찾을 수 없습니다.

이 요구사항은 키 타입이 Swift 표준 라이브러리에서 정의된 특수 프로토콜 인 `Hashable` 프로토콜을 준수해야 함을 지정하는 `Dictionary` 의 키 타입에서 타입 제약에 의해 강제됩니다. Swift의 모든 기본 타입 \(`String`, `Int`, `Double`, 그리고 `Bool`\)은 기본적으로 해시 가능해야 합니다. `Hashable` 프로토콜을 준수하는 자체 사용자 정의 타입을 만드는 것에 대한 자세한 내용은 [해시 가능한 프로토콜 준수 \(Conforming to the Hashable Protocol\)](https://developer.apple.com/documentation/swift/hashable#2849490) 을 참고 바랍니다.

사용자 정의 제너릭 타입을 생성할 때 고유 타입 제약을 정의할 수 있고 이러한 제약은 제너릭 프로그래밍에 많은 기능을 제공합니다. `Hashable` 같은 추상 개념은 구체적인 타입이 아닌 개념적 특성 측면에서 타입을 특성화 합니다.

### 타입 제약 구문 \(Type Constraint Syntax\)

<!--
You write type constraints by placing a single class or protocol constraint after a type parameter’s name, separated by a colon, as part of the type parameter list. The basic syntax for type constraints on a generic function is shown below (although the syntax is the same for generic types):
-->

타입 파라미터 목록의 부분으로 콜론으로 구분한 타입 파라미터의 이름 뒤에 단일 클래스 또는 프로토콜 제약을 위치하여 타입 제약을 작성합니다. 제너릭 함수에서 타입 제약에 대한 기본 구문은 아래와 같습니다 \(제너릭 타입의 구문은 동일함\):

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // function body goes here
}
```

<!--
The hypothetical function above has two type parameters. The first type parameter, T, has a type constraint that requires T to be a subclass of SomeClass. The second type parameter, U, has a type constraint that requires U to conform to the protocol SomeProtocol.
-->

위의 가상 함수는 2개의 타입 파라미터를 가집니다. 첫번째 타입 파라미터 인 `T` 는 `T` 가 `SomeClass` 의 하위 클래스 여야 함을 나타냅니다. 두번째 타입 파라미터 `U` 는 `U` 가 `SomeProtocol` 프로토콜을 준수해야하는 타입 제약이 있습니다.

### 타입 제약 동작 \(Type Constraints in Action\)

<!--
Here’s a nongeneric function called findIndex(ofString:in:), which is given a String value to find and an array of String values within which to find it. The findIndex(ofString:in:) function returns an optional Int value, which will be the index of the first matching string in the array if it’s found, or nil if the string can’t be found:
-->

다음은 찾을 `String` 값과 찾을 `String` 값의 배열이 주어진 `findIndex(ofString:in:)` 이라는 제너릭이 아닌 함수입니다. `findIndex(ofString:in:)` 함수는 배열에서 일치하는 문자열을 찾으면 그 인덱스를 찾지 못하면 `nil` 인 옵셔널 `Int` 값을 반환합니다:

```swift
func findIndex(ofString valueToFind: String, in array: [String]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

<!--
The findIndex(ofString:in:) function can be used to find a string value in an array of strings:
-->

`findIndex(ofString:in:)` 함수는 배열의 문자열에서 문자열 값을 찾기 위해 사용됩니다:

```swift
let strings = ["cat", "dog", "llama", "parakeet", "terrapin"]
if let foundIndex = findIndex(ofString: "llama", in: strings) {
    print("The index of llama is \(foundIndex)")
}
// Prints "The index of llama is 2"
```

<!--
The principle of finding the index of a value in an array isn’t useful only for strings, however. You can write the same functionality as a generic function by replacing any mention of strings with values of some type T instead.

Here’s how you might expect a generic version of findIndex(ofString:in:), called findIndex(of:in:), to be written. Note that the return type of this function is still Int?, because the function returns an optional index number, not an optional value from the array. Be warned, though—this function doesn’t compile, for reasons explained after the example:
-->

그러나 배열에서 값의 인덱스를 찾는 원리는 문자열에만 유용하지 않습니다. 문자열에 대한 언급을 `T` 타입의 값으로 대체하여 제너릭 함수로 같은 기능을 작성할 수 있습니다.

다음은 `findIndex(of:in:)` 이라는 `findIndex(ofString:in:)` 의 제너릭 버전을 나타냅니다. 이 함수는 배열의 옵셔널 값이 아닌 옵셔널 인덱스를 반환하므로 반환 타입이 여전히 `Int?` 입니다. 하지만 이 함수는 예제 뒤에 설명된 이유로 컴파일 되지 않습니다:

```swift
func findIndex<T>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

<!--
This function doesn’t compile as written above. The problem lies with the equality check, “if value == valueToFind”. Not every type in Swift can be compared with the equal to operator (==). If you create your own class or structure to represent a complex data model, for example, then the meaning of “equal to” for that class or structure isn’t something that Swift can guess for you. Because of this, it isn’t possible to guarantee that this code will work for every possible type T, and an appropriate error is reported when you try to compile the code.

All is not lost, however. The Swift standard library defines a protocol called Equatable, which requires any conforming type to implement the equal to operator (==) and the not equal to operator (!=) to compare any two values of that type. All of Swift’s standard types automatically support the Equatable protocol.

Any type that’s Equatable can be used safely with the findIndex(of:in:) function, because it’s guaranteed to support the equal to operator. To express this fact, you write a type constraint of Equatable as part of the type parameter’s definition when you define the function:
-->

이 함수는 위에 작성된 대로 컴파일되지 않습니다. 이 문제는 "`if value == valueToFind`" 를 동등성을 검사에 있습니다. Swift의 모든 타입이 동등 연산자 \(`==`\)로 비교할 수 있는 것은 아닙니다. 예를 들어 복잡한 데이터 모델을 표현하기 위해 고유한 클래스 또는 구조체를 생성한다면 해당 클래스 또는 구조체에 대한 "같음"의 의미는 Swift가 추측할 수 있는 것은 아닙니다. 이로 인해 이 코드가 가능한 모든 타입의 `T` 에 대해 작동한다고 보장할 수 없으며 코드를 컴파일 하려고 할 때 적절한 에러가 발생합니다.

그러나 모든 것이 손실되지 않습니다. Swift 표준 라이브러리는 타입의 모든 2개의 값을 비교하기 위해 동등 연산자 \(`==`\)와 비동등 연산자 \(`!=`\)를 구현하기 위해 준수하는 타입을 요구하는 `Equatable` 이라는 프로토콜을 정의합니다. Swift의 모든 표준 타입은 `Equatable` 프로토콜을 자동으로 지원합니다.

`Equatable` 인 모든 타입은 동등 연산자를 지원하기 때문에 `findIndex(of:in:)` 함수와 함께 안전하게 사용될 수 있습니다. 이 사실을 표현하기 위해 함수를 정의할 때 타입 파라미터의 정의의 부분으로 `Equatable` 의 타입 제약을 작성합니다:

```swift
func findIndex<T: Equatable>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

<!--
The single type parameter for findIndex(of:in:) is written as T: Equatable, which means “any type T that conforms to the Equatable protocol.”

The findIndex(of:in:) function now compiles successfully and can be used with any type that’s Equatable, such as Double or String:
-->

`findIndex(of:in:)` 에 대한 단일 타입 파라미터는 "`Equatable` 프로토콜을 준수하는 `T` 타입의 모든 것" 이라는 의미로 `T: Equatable` 로 작성됩니다.

`findIndex(of:in:)` 함수는 이제 정상적으로 컴파일 되고 `Double` 또는 `String` 과 같은 `Equatable` 인 모든 타입에 대해 사용될 수 있습니다:

```swift
let doubleIndex = findIndex(of: 9.3, in: [3.14159, 0.1, 0.25])
// doubleIndex is an optional Int with no value, because 9.3 isn't in the array
let stringIndex = findIndex(of: "Andrea", in: ["Mike", "Malcolm", "Andrea"])
// stringIndex is an optional Int containing a value of 2
```

## 연관된 타입 \(Associated Types\)

<!--
When defining a protocol, it’s sometimes useful to declare one or more associated types as part of the protocol’s definition. An associated type gives a placeholder name to a type that’s used as part of the protocol. The actual type to use for that associated type isn’t specified until the protocol is adopted. Associated types are specified with the associatedtype keyword.
-->

프로토콜을 정의할 때 프로토콜의 정의의 부분으로 하나 또는 그 이상의 연관된 타입 \(associated types\)을 선언하는게 더 유용한 경우가 있습니다. _연관된 타입 \(associated type\)_ 은 프로토콜의 부분으로 사용되는 타입의 임의의 이름을 제공합니다. 연관된 타입에 사용할 실제 타입은 프로토콜이 채택되기 까지 지정되지 않습니다. 연관된 타입은 `associatedtype` 키워드와 함께 지정됩니다.

### 연관된 타입의 동작 \(Associated Types in Action\)

<!--
Here’s an example of a protocol called Container, which declares an associated type called Item:
-->

다음은 `Item` 이라는 연관된 타입을 선언하는 `Container` 라는 프로토콜의 예입니다:

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

<!--
The Container protocol defines three required capabilities that any container must provide:

* It must be possible to add a new item to the container with an append(_:) method.
* It must be possible to access a count of the items in the container through a count property that returns an Int value.
* It must be possible to retrieve each item in the container with a subscript that takes an Int index value.

This protocol doesn’t specify how the items in the container should be stored or what type they’re allowed to be. The protocol only specifies the three bits of functionality that any type must provide in order to be considered a Container. A conforming type can provide additional functionality, as long as it satisfies these three requirements.

Any type that conforms to the Container protocol must be able to specify the type of values it stores. Specifically, it must ensure that only items of the right type are added to the container, and it must be clear about the type of the items returned by its subscript.

To define these requirements, the Container protocol needs a way to refer to the type of the elements that a container will hold, without knowing what that type is for a specific container. The Container protocol needs to specify that any value passed to the append(_:) method must have the same type as the container’s element type, and that the value returned by the container’s subscript will be of the same type as the container’s element type.

To achieve this, the Container protocol declares an associated type called Item, written as associatedtype Item. The protocol doesn’t define what Item is—that information is left for any conforming type to provide. Nonetheless, the Item alias provides a way to refer to the type of the items in a Container, and to define a type for use with the append(_:) method and subscript, to ensure that the expected behavior of any Container is enforced.

Here’s a version of the nongeneric IntStack type from Generic Types above, adapted to conform to the Container protocol:
-->

`Container` 프로토콜은 모든 컨테이너가 제공해야 하는 3가지 필수 요건을 정의합니다:

* `append(_:)` 메서드를 사용하여 컨테이너에 새로운 항목을 추가할 수 있어야 합니다.
* `Int` 값을 반환하는 `count` 프로퍼티를 통해 컨테이너에 항목의 카운트에 접근할 수 있어야 합니다.
* `Int` 인덱스 값을 사용하는 서브 스크립트로 컨테이너에 각 항목을 조회할 수 있어야 합니다.

이 프로토콜은 컨테이너에 항목을 저장하는 방법 또는 허용되는 타입을 지정하지 않습니다. 이 프로토콜은 `Container` 로 간주되기 위해 모든 타입이 제공되는 3가지의 기능만 지정합니다. 준수하는 타입은 이 3가지 요구사항을 준수하는 한 추가적인 기능을 제공할 수 있습니다.

`Container` 프로토콜을 준수하는 모든 타입은 저장하는 값의 타입을 지정할 수 있어야 합니다. 특히, 올바른 타입의 항목만 컨테이너에 추가되야 하며 서브 스크립트에 의해 반환된 항목에 타입에 대해 명확해야 합니다.

이러한 요구사항을 정의하기 위해 `Container` 프로토콜은 특정 컨테이너에 대한 타입이 무엇인지 알지 못해도 컨테이너가 보유할 항목의 타입을 참조할 방법이 필요합니다. `Container` 프로토콜은 `append(_:)` 메서드에 전달된 모든 값이 컨테이너의 요소 타입과 같은 타입을 가져야 하며 컨테이너의 서브 스크립트에 의해 반환된 값은 컨테이너의 요소 타입과 같은 타입으로 지정해야 합니다.

이를 달성하기 위해 `Container` 프로토콜은 `associatedtype Item` 으로 작성한 `Item` 이라는 연관된 타입을 선언합니다. 이 프로토콜은 `Item` 이 무엇인지 정의하지 않고 해당 정보는 모든 준수하는 타입이 제공할 수 있도록 남겨집니다. 그럼에도 불구하고 `Item` 별칭은 `Container` 의 항목에 타입을 참조하고 `append(_:)` 메서드와 서브 스크립트에서 사용하는 타입을 정의하고 모든 `Container` 의 동작이 예상되도록 방법을 제공합니다.

다음은 `Container` 프로토콜을 준수하도록 조정된 위의 [제너릭 타입 \(Generic Types\)](generics.md#generic-types) 에서의 제너릭이 아닌 `IntStack` 의 버전입니다:

```swift
struct IntStack: Container {
    // original IntStack implementation
    var items: [Int] = []
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
    // conformance to the Container protocol
    typealias Item = Int
    mutating func append(_ item: Int) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Int {
        return items[i]
    }
}
```

<!--
The IntStack type implements all three of the Container protocol’s requirements, and in each case wraps part of the IntStack type’s existing functionality to satisfy these requirements.

Moreover, IntStack specifies that for this implementation of Container, the appropriate Item to use is a type of Int. The definition of typealias Item = Int turns the abstract type of Item into a concrete type of Int for this implementation of the Container protocol.

Thanks to Swift’s type inference, you don’t actually need to declare a concrete Item of Int as part of the definition of IntStack. Because IntStack conforms to all of the requirements of the Container protocol, Swift can infer the appropriate Item to use, simply by looking at the type of the append(_:) method’s item parameter and the return type of the subscript. Indeed, if you delete the typealias Item = Int line from the code above, everything still works, because it’s clear what type should be used for Item.

You can also make the generic Stack type conform to the Container protocol:
-->

`IntStack` 타입은 `Container` 프로토콜의 3가지 요구사항을 모두 구현하고 각각의 경우 `IntStack` 타입의 기존 기능의 부분을 래핑하여 이러한 요구사항을 충족합니다.

또한 `IntStack` 은 `Container` 의 구현에 대해 적절한 `Item` 이 `Int` 타입임을 지정합니다. `typealias Item = Int` 의 정의는 `Container` 프로토콜의 구현에 대해 `Item` 의 추상적 타입에서 `Int` 의 구체적인 타입으로 변환합니다.

Swift의 타입 추론 덕분에 실제로 `IntStack` 의 정의의 부분으로 `Int` 의 구체적인 `Item` 을 선언할 필요가 없습니다. `IntStack` 은 `Container` 프로토콜의 모든 요구사항을 준수하기 때문에 Swift는 `append(_:)` 메서드의 `item` 파라미터의 타입과 서브 스크립트의 반환 타입에서 사용하는 적절한 `Item` 을 유추할 수 있습니다. 실제로 `Item` 에 대해 무슨 타입을 사용하는지 명확하기 때문에 위의 코드에서 `typealias Item = Int` 줄을 지워도 동작에는 이상이 없습니다.

`Container` 프로토콜을 준수하는 제너릭 `Stack` 타입도 만들 수 있습니다:

```swift
struct Stack<Element>: Container {
    // original Stack<Element> implementation
    var items: [Element] = []
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
    // conformance to the Container protocol
    mutating func append(_ item: Element) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Element {
        return items[i]
    }
}
```

<!--
This time, the type parameter Element is used as the type of the append(_:) method’s item parameter and the return type of the subscript. Swift can therefore infer that Element is the appropriate type to use as the Item for this particular container.
-->

이번에는 `Element` 타입 파라미터는 `append(_:)` 메서드의 `item` 파라미터와 서브 스크립트의 반환 타입으로 사용됩니다. 따라서 Swift는 `Element` 가 특정 컨테이너에 대해 `Item` 으로 사용하는 적절한 타입이라고 유추할 수 있습니다.

### 기존 타입을 확장하여 연관된 타입 지정 \(Extending an Existing Type to Specify an Associated Type\)

<!--
You can extend an existing type to add conformance to a protocol, as described in Adding Protocol Conformance with an Extension. This includes a protocol with an associated type.

Swift’s Array type already provides an append(_:) method, a count property, and a subscript with an Int index to retrieve its elements. These three capabilities match the requirements of the Container protocol. This means that you can extend Array to conform to the Container protocol simply by declaring that Array adopts the protocol. You do this with an empty extension, as described in Declaring Protocol Adoption with an Extension:
-->

[확장으로 프로토콜 준수성 추가 \(Adding Protocol Conformance with an Extension\)](protocols.md#adding-protocol-conformance-with-an-extension) 에서 설명한대로 프로토콜의 준수성을 추가하기 위해 기존 타입을 확장할 수 있습니다. 여기에는 연관된 타입이 있는 프로토콜을 포함합니다.

Swift의 `Array` 타입은 이미 `append(_:)` 메서드, `count` 프로퍼티, 그리고 `Int` 인덱스로 요소를 조회하기 위해 서브 스크립트를 제공합니다. 이 3가지 기능은 `Container` 프로토콜의 요구사항과 일치합니다. 이것은 `Array` 가 프로토콜을 채택하도록 선언하는 것으로 간단하게 `Container` 프로토콜을 준수하도록 `Array` 를 확장할 수 있다는 의미입니다. [확장으로 프로토콜 채택 선언 \(Declaring Protocol Adoption with an Extension\)](protocols.md#declaring-protocol-adoption-with-an-extension) 에서 설명 했듯이 빈 확장을 사용하여 이 작업을 수행할 수 있습니다:

```swift
extension Array: Container {}
```

<!--
Array’s existing append(_:) method and subscript enable Swift to infer the appropriate type to use for Item, just as for the generic Stack type above. After defining this extension, you can use any Array as a Container.
-->

배열의 존재하는 `append(_:)` 메서드와 서브 스크립트는 위의 제너릭 `Stack` 타입과 같이 Swift는 `Item` 에 대해 사용할 적절한 타입을 유추할 수 있습니다. 확장 정의 후에 `Container` 로 모든 `Array` 를 사용할 수 있습니다.

### 연관된 타입에 제약 추가 \(Adding Constraints to an Associated Type\)

<!--
You can add type constraints to an associated type in a protocol to require that conforming types satisfy those constraints. For example, the following code defines a version of Container that requires the items in the container to be equatable.
-->

제약조건을 충족하는 준수하는 타입을 요구하기 위해 프로토콜의 연관된 타입에 타입 제약을 추가할 수 있습니다. 예를 들어 다음의 코드는 컨테이너의 항목이 동일해야 하는 것을 요구하는 `Container` 의 버전을 정의합니다.

```swift
protocol Container {
    associatedtype Item: Equatable
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

<!--
To conform to this version of Container, the container’s Item type has to conform to the Equatable protocol.
-->

이 `Container` 버전을 준수하려면 컨테이너의 `Item` 타입은 `Equatable` 프로토콜을 준수해야 합니다.

### 연관된 타입의 제약조건에서 프로토콜 사용 \(Using a Protocol in Its Associated Type’s Constraints\)

<!--
A protocol can appear as part of its own requirements. For example, here’s a protocol that refines the Container protocol, adding the requirement of a suffix(_:) method. The suffix(_:) method returns a given number of elements from the end of the container, storing them in an instance of the Suffix type.
-->

프로토콜은 자체 요구사항의 일부로 나타날 수 있습니다. 예를 들어 다음은 `suffix(_:)` 메서드의 요구사항을 추가하여 `Container` 프로토콜을 구체화 하는 프로토콜입니다. `suffix(_:)` 메서드는 컨테이너 끝에서 지정된 숫자의 요소를 반환하고 `Suffix` 타입의 인스턴스에 저장합니다.

```swift
protocol SuffixableContainer: Container {
    associatedtype Suffix: SuffixableContainer where Suffix.Item == Item
    func suffix(_ size: Int) -> Suffix
}
```

<!--
In this protocol, Suffix is an associated type, like the Item type in the Container example above. Suffix has two constraints: It must conform to the SuffixableContainer protocol (the protocol currently being defined), and its Item type must be the same as the container’s Item type. The constraint on Item is a generic where clause, which is discussed in Associated Types with a Generic Where Clause below.

Here’s an extension of the Stack type from Generic Types above that adds conformance to the SuffixableContainer protocol:
-->

이 프로토콜에서 `Suffix` 는 위의 `Container` 예제에서 `Item` 타입과 같은 연관된 타입입니다. `Suffix` 는 2개의 제약을 가지고 있으며 먼저 현재 정의되어 있는 프로토콜 인 `SuffixableContainer` 프로토콜을 준수해야 하며 `Item` 타입은 컨테이너의 `Item` 타입과 동일해야 합니다. `Item` 에 대한 제약은 아래의 [제너릭 Where 절을 가진 연관된 타입 \(Associated Types with a Generic Where Clause\)](generics.md#where-associated-types-with-a-generic-where-clause) 에서 설명할 제너릭 `where` 절입니다.

다음은 `SuffixableContainer` 프로토콜의 준수성을 추가하는 위의 [제너릭 타입 \(Generic Types\)](generics.md#generic-types) 에서 `Stack` 타입의 확장입니다:

```swift
extension Stack: SuffixableContainer {
    func suffix(_ size: Int) -> Stack {
        var result = Stack()
        for index in (count-size)..<count {
            result.append(self[index])
        }
        return result
    }
    // Inferred that Suffix is Stack.
}
var stackOfInts = Stack<Int>()
stackOfInts.append(10)
stackOfInts.append(20)
stackOfInts.append(30)
let suffix = stackOfInts.suffix(2)
// suffix contains 20 and 30
```

<!--
In the example above, the Suffix associated type for Stack is also Stack, so the suffix operation on Stack returns another Stack. Alternatively, a type that conforms to SuffixableContainer can have a Suffix type that’s different from itself—meaning the suffix operation can return a different type. For example, here’s an extension to the nongeneric IntStack type that adds SuffixableContainer conformance, using Stack<Int> as its suffix type instead of IntStack:
-->

위의 예제에서 `Stack` 에 대한 `Suffix` 연관된 타입도 `Stack` 이므로 `Stack` 에서 접미사 연산자는 다른 `Stack` 을 반환합니다. 또한 `SuffixableContainer` 를 준수하는 타입은 자체와 다른 `Suffix` 타입을 가질 수 있습니다. 접미사 연산자는 다른 타입을 반환할 수 있다는 의미입니다. 예를 들어 다음은 `IntStack` 대신 `Stack<Int>` 를 접미사 타입으로 사용하여 `SuffixableContainer` 준수성을 추가하는 제너릭이 아닌 `IntStack` 타입에 대한 확장입니다:

```swift
extension IntStack: SuffixableContainer {
    func suffix(_ size: Int) -> Stack<Int> {
        var result = Stack<Int>()
        for index in (count-size)..<count {
            result.append(self[index])
        }
        return result
    }
    // Inferred that Suffix is Stack<Int>.
}
```

## 제너릭 Where 절 \(Generic Where Clauses\)

<!--
Type constraints, as described in Type Constraints, enable you to define requirements on the type parameters associated with a generic function, subscript, or type.

It can also be useful to define requirements for associated types. You do this by defining a generic where clause. A generic where clause enables you to require that an associated type must conform to a certain protocol, or that certain type parameters and associated types must be the same. A generic where clause starts with the where keyword, followed by constraints for associated types or equality relationships between types and associated types. You write a generic where clause right before the opening curly brace of a type or function’s body.

The example below defines a generic function called allItemsMatch, which checks to see if two Container instances contain the same items in the same order. The function returns a Boolean value of true if all items match and a value of false if they don’t.

The two containers to be checked don’t have to be the same type of container (although they can be), but they do have to hold the same type of items. This requirement is expressed through a combination of type constraints and a generic where clause:
-->

[타입 제약 \(Type Constraints\)](generics.md#type-constraints) 을 사용하면 제너릭 함수, 서브 스크립트, 또는 타입과 연관된 타입 파라미터에 대한 요구사항을 정의할 수 있습니다.

연관된 타입에 대한 요구사항을 정의하는 것도 유용할 수 있습니다. _제너릭 where 절 \(generic where clause\)_ 을 정의하여 이것을 수행할 수 있습니다. 제너릭 `where` 절을 사용하면 연관된 타입이 특정 프로토콜을 준수해야 하거나 특정 타입 파라미터와 연관된 타입이 동일해야 한다고 요구할 수 있습니다. 제너릭 `where` 절은 `where` 키워드로 시작하고 이어서 연관된 타입 또는 타입과 연관된 타입 사이의 동등 관계에 대한 제약이 따라옵니다. 타입 또는 함수의 바디의 여는 중괄호 바로 전에 제너릭 `where` 절을 작성합니다.

아래의 예제는 2개의 `Container` 인스턴스가 같은 순서로 같은 항목을 가지고 있는지 확인하는 `allItemsMatch` 라는 제너릭 함수를 정의합니다. 이 함수는 모든 항목이 일치하면 `true` 의 불린 값을 반환하고 그렇지 않으면 `false` 의 값을 반환합니다.

확인할 2개의 컨테이너는 같은 타입의 컨테이너 일 필요는 없지만 같은 타입의 항목을 가지고 있어야 합니다. 이 요구사항은 타입 제약과 제너릭 `where` 절의 조합으로 표현됩니다:

```swift
func allItemsMatch<C1: Container, C2: Container>
    (_ someContainer: C1, _ anotherContainer: C2) -> Bool
    where C1.Item == C2.Item, C1.Item: Equatable {

        // Check that both containers contain the same number of items.
        if someContainer.count != anotherContainer.count {
            return false
        }

        // Check each pair of items to see if they're equivalent.
        for i in 0..<someContainer.count {
            if someContainer[i] != anotherContainer[i] {
                return false
            }
        }

        // All items match, so return true.
        return true
}
```

<!--
This function takes two arguments called someContainer and anotherContainer. The someContainer argument is of type C1, and the anotherContainer argument is of type C2. Both C1 and C2 are type parameters for two container types to be determined when the function is called.

The following requirements are placed on the function’s two type parameters:

* C1 must conform to the Container protocol (written as C1: Container).
* C2 must also conform to the Container protocol (written as C2: Container).
* The Item for C1 must be the same as the Item for C2 (written as C1.Item == C2.Item).
* The Item for C1 must conform to the Equatable protocol (written as C1.Item: Equatable).

The first and second requirements are defined in the function’s type parameter list, and the third and fourth requirements are defined in the function’s generic where clause.

These requirements mean:

* someContainer is a container of type C1.
* anotherContainer is a container of type C2.
* someContainer and anotherContainer contain the same type of items.
* The items in someContainer can be checked with the not equal operator (!=) to see if they’re different from each other.

The third and fourth requirements combine to mean that the items in anotherContainer can also be checked with the != operator, because they’re exactly the same type as the items in someContainer.

These requirements enable the allItemsMatch(_:_:) function to compare the two containers, even if they’re of a different container type.

The allItemsMatch(_:_:) function starts by checking that both containers contain the same number of items. If they contain a different number of items, there’s no way that they can match, and the function returns false.

After making this check, the function iterates over all of the items in someContainer with a for-in loop and the half-open range operator (..<). For each item, the function checks whether the item from someContainer isn’t equal to the corresponding item in anotherContainer. If the two items aren’t equal, then the two containers don’t match, and the function returns false.

If the loop finishes without finding a mismatch, the two containers match, and the function returns true.

Here’s how the allItemsMatch(_:_:) function looks in action:
-->

이 함수는 `someContainer` 와 `anotherContainer` 라는 2개의 인자를 가집니다. `someContainer` 인자는 `C1` 타입이고 `anotherContainer` 인자는 `C2` 타입입니다. `C1` 과 `C2` 모두 함수가 호출될 때 결정되는 2가지 컨테이너 타입에 대한 타입 파라미터입니다.

함수의 2가지 타입 파라미터에 대한 요구사항은 다음과 같습니다:

* `C1: Container` 로 작성했듯이 `C1` 은 `Container` 프로토콜을 준수해야 합니다.
* `C2: Container` 로 작성했듯이 `C2` 는 `Container` 프로토콜을 준수해야 합니다.
* `C1.Item == C2.Item` 으로 작성했듯이 `C1` 에 대한 `Item` 은 `C2` 에 대한 `Item` 과 동일해야 합니다.
* `C1.Item: Equatable` 로 작성했듯이 `C1` 에 대한 `Item` 은 `Equatable` 프로토콜을 준수해야 합니다.

첫번째 그리고 두번째 요구사항은 함수의 타입 파라미터 목록에 정의되고 세번째 그리고 네번째 요구사항은 함수의 제너릭 `where` 절에 정의됩니다.

이 요구사항은 아래를 의미합니다:

* `someContainer` 는 `C1` 타입의 컨테이너 입니다.
* `anotherContainer` `C2` 타입의 컨테이너 입니다.
* `someContainer` 와 `anotherContainer` 는 같은 타입의 항목을 포함합니다.
* `someContainer` 안에 항목은 서로 다름을 확인하기 위해 비동등 연산자 \(`!=`\)를 사용하여 체크될 수 있습니다.

세번째와 네번째 요구사항은 `someContainer` 의 항목과 정확히 동일한 타입이므로 `anotherContainer` 의 항목도 `!=` 연산자로 확인할 수 있음을 의미합니다.

이 요구사항을 통해 `allItemsMatch(_:_:)` 함수는 컨테이너 타입이 다른 경우에도 두 컨테이너를 비교할 수 있습니다.

`allItemsMatch(_:_:)` 함수는 두 컨테이너 모두 항목의 같은 수만큼 가지고 있는지 확인하는 것으로 시작합니다. 항목의 수가 다르다면 일치하지 않으므로 함수는 `false` 를 반환합니다.

이 검사를 수행한 후에 함수는 `for`-`in` 루프와 반열림 범위 연산자 \(`..<`\)로 `someContainer` 의 모든 항목을 반복합니다. 각 항목에 대해 함수는 `someContainer` 의 항목이 `anotherContainer` 의 항목과 동일하지 않은지 확인합니다. 두 항목이 동등하지 않으면 2개의 컨테이너는 일치하지 않고 함수는 `false` 를 반환합니다.

불일치하는 항목을 찾지 못하고 루프가 끝나면 2개의 컨테이너는 일치하고 함수는 `true` 를 반환합니다.

다음은 `allItemsMatch(_:_:)` 함수가 어떻게 동작하는지 보여줍니다:

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")

var arrayOfStrings = ["uno", "dos", "tres"]

if allItemsMatch(stackOfStrings, arrayOfStrings) {
    print("All items match.")
} else {
    print("Not all items match.")
}
// Prints "All items match."
```

<!--
The example above creates a Stack instance to store String values, and pushes three strings onto the stack. The example also creates an Array instance initialized with an array literal containing the same three strings as the stack. Even though the stack and the array are of a different type, they both conform to the Container protocol, and both contain the same type of values. You can therefore call the allItemsMatch(_:_:) function with these two containers as its arguments. In the example above, the allItemsMatch(_:_:) function correctly reports that all of the items in the two containers match.
-->

위의 예제는 `String` 값을 저장하는 `Stack` 인스턴스를 생성하고 스택에 3개의 문자열을 푸쉬합니다. 이 예제는 스택과 동일한 3개의 문자열을 포함하는 배열 리터럴로 초기화 된 `Array` 인스턴스도 생성합니다. 스택과 배열은 다른 타입이지만 둘다 `Container` 프로토콜을 준수하고 둘다 같은 타입의 값을 포함합니다. 따라서 이 2개의 컨테이너를 인자로 `allItemsMatch(_:_:)` 함수를 호출합니다. 위의 예제에서 `allItemsMatch(_:_:)` 함수는 두 컨테이너의 모든 항목이 정확히 일치한다고 알려줍니다.

## 제너릭 Where 절이 있는 확장 \(Extensions with a Generic Where Clause\)

<!--
You can also use a generic where clause as part of an extension. The example below extends the generic Stack structure from the previous examples to add an isTop(_:) method.
-->

확장의 부분으로 제너릭 `where` 절을 사용할 수도 있습니다. 아래의 예제는 이전 예제의 제너릭 `Stack` 구조체에 `isTop(_:)` 메서드를 추가하기 위해 확장합니다.

```swift
extension Stack where Element: Equatable {
    func isTop(_ item: Element) -> Bool {
        guard let topItem = items.last else {
            return false
        }
        return topItem == item
    }
}
```

<!--
This new isTop(_:) method first checks that the stack isn’t empty, and then compares the given item against the stack’s topmost item. If you tried to do this without a generic where clause, you would have a problem: The implementation of isTop(_:) uses the == operator, but the definition of Stack doesn’t require its items to be equatable, so using the == operator results in a compile-time error. Using a generic where clause lets you add a new requirement to the extension, so that the extension adds the isTop(_:) method only when the items in the stack are equatable.

Here’s how the isTop(_:) method looks in action:
-->

새로운 `isTop(_:)` 메서드는 먼저 스택이 비어 있는지 확인하고 그 다음에 주어진 항목과 스택의 가장 상단의 항목을 비교합니다. 제너릭 `where` 절이 없이 시도한다면 문제가 발생합니다. `isTop(_:)` 의 구현은 `==` 연산자를 사용하지만 `Stack` 의 정의는 항목의 동등한지 요구하지 않으므로 `==` 연산자를 사용한 결과는 컴파일 에러가 발생합니다. 제너릭 `where` 절을 사용하여 확장에 새로운 요구사항을 추가할 수 있으므로 확장은 스택에 항목이 동등성이 가능할 때만 `isTop(_:)` 메서드를 추가합니다.

다음은 `isTop(_:)` 메서드가 어떻게 동작하는지 보여줍니다:

```swift
if stackOfStrings.isTop("tres") {
    print("Top element is tres.")
} else {
    print("Top element is something else.")
}
// Prints "Top element is tres."
```

<!--
If you try to call the isTop(_:) method on a stack whose elements aren’t equatable, you’ll get a compile-time error.
-->

동등성이 가능하지 않은 요소를 가진 스택에서 `isTop(_:)` 메서드를 호출하면 컴파일 에러가 발생합니다.

```swift
struct NotEquatable { }
var notEquatableStack = Stack<NotEquatable>()
let notEquatableValue = NotEquatable()
notEquatableStack.push(notEquatableValue)
notEquatableStack.isTop(notEquatableValue)  // Error
```

<!--
You can use a generic where clause with extensions to a protocol. The example below extends the Container protocol from the previous examples to add a startsWith(_:) method.
-->

프로토콜 확장에 제너릭 `where` 절을 사용할 수 있습니다. 아래의 예제는 이전 예제의 `Container` 프로토콜에 `startsWith(_:)` 메서드를 추가하기 위해 확장합니다.

```swift
extension Container where Item: Equatable {
    func startsWith(_ item: Item) -> Bool {
        return count >= 1 && self[0] == item
    }
}
```

<!--
The startsWith(_:) method first makes sure that the container has at least one item, and then it checks whether the first item in the container matches the given item. This new startsWith(_:) method can be used with any type that conforms to the Container protocol, including the stacks and arrays used above, as long as the container’s items are equatable.
-->

`startsWith(_:)` 메서드는 먼저 컨테이너가 하나 이상의 항목을 가지고 있는지 확인하고 그런 다음 컨테이너의 첫번째 항목이 주어진 항목과 일치하는지 확인합니다. 새로운 `startsWith(_:)` 메서드는 컨테이너의 항목이 동등성이 가능하다면 위에서 사용된 스택과 배열을 포함하여 `Container` 프로토콜을 준수하는 모든 타입에서 사용될 수 있습니다.

```swift
if [9, 9, 9].startsWith(42) {
    print("Starts with 42.")
} else {
    print("Starts with something else.")
}
// Prints "Starts with something else."
```

<!--
The generic where clause in the example above requires Item to conform to a protocol, but you can also write a generic where clauses that require Item to be a specific type. For example:
-->

위의 예제에서 제너릭 `where` 절은 `Item` 은 프로토콜을 준수해야 하지만 `Item` 이 특정 타입을 요구하도록 제너릭 `where` 절을 작성할 수도 있습니다. 예를 들어:

```swift
extension Container where Item == Double {
    func average() -> Double {
        var sum = 0.0
        for index in 0..<count {
            sum += self[index]
        }
        return sum / Double(count)
    }
}
print([1260.0, 1200.0, 98.6, 37.0].average())
// Prints "648.9"
```

<!--
This example adds an average() method to containers whose Item type is Double. It iterates over the items in the container to add them up, and divides by the container’s count to compute the average. It explicitly converts the count from Int to Double to be able to do floating-point division.

You can include multiple requirements in a generic where clause that’s part of an extension, just like you can for a generic where clause that you write elsewhere. Separate each requirement in the list with a comma.
-->

이 예제는 `Item` 타입이 `Double` 인 컨테이너에 `average()` 메서드를 추가합니다. 컨테이너의 항목을 더하고 컨테이너의 수로 나누어 평균을 계산합니다. 부동 소수점 나누기가 가능하기 위해 카운트를 `Int` 에서 `Double` 로 명시적으로 변환합니다.

다른 곳에서 작성하는 제너릭 `where` 절과 마찬가지로 확장의 부분인 제너릭 `where` 절에 여러 요구사항을 포함할 수 있습니다. 콤마로 목록의 각 요구사항을 구분합니다.

## 상황별 Where 절 \(Contextual Where Clauses\)

<!--
You can write a generic where clause as part of a declaration that doesn’t have its own generic type constraints, when you’re already working in the context of generic types. For example, you can write a generic where clause on a subscript of a generic type or on a method in an extension to a generic type. The Container structure is generic, and the where clauses in the example below specify what type constraints have to be satisfied to make these new methods available on a container.
-->

제너릭 타입의 컨텍스트에서 이미 작업중인 경우 고유한 제너릭 타입 제약조건이 없는 선언의 부분으로 제너릭 `where` 절을 작성할 수 있습니다. 예를 들어 제너릭 타입의 서브 스크립트나 제너릭 타입에 확장에 대한 확장 메서드에 제너릭 `where` 절을 작성할 수 있습니다. `Container` 구조체는 제너릭 이고 아래의 예제에서 `where` 절은 컨테이너에서 이러한 새로운 메서드를 사용할 수 있도록 충족해야 하는 타입 제약조건을 지정합니다.

```swift
extension Container {
    func average() -> Double where Item == Int {
        var sum = 0.0
        for index in 0..<count {
            sum += Double(self[index])
        }
        return sum / Double(count)
    }
    func endsWith(_ item: Item) -> Bool where Item: Equatable {
        return count >= 1 && self[count-1] == item
    }
}
let numbers = [1260, 1200, 98, 37]
print(numbers.average())
// Prints "648.75"
print(numbers.endsWith(37))
// Prints "true"
```

<!--
This example adds an average() method to Container when the items are integers, and it adds an endsWith(_:) method when the items are equatable. Both functions include a generic where clause that adds type constraints to the generic Item type parameter from the original declaration of Container.

If you want to write this code without using contextual where clauses, you write two extensions, one for each generic where clause. The example above and the example below have the same behavior.
-->

이 예제는 항목이 정수이면 `Container` 에 `average()` 메서드를 추가하고 항목이 동등성이 가능하면 `endsWith(_:)` 메서드를 추가합니다. 두 함수 모두 `Container` 의 기존 선언에 제너릭 `Item` 타입 파라미터에 대해 타입 제약조건을 추가한 제너릭 `where` 절을 포함합니다.

상황별 `where` 절 사용없이 코드를 작성하고 싶다면 2개의 확장을 작성하고 각각에 제너릭 `where` 절을 추가하면 됩니다. 위의 예제와 아래의 예제는 같은 동작을 가집니다.

```swift
extension Container where Item == Int {
    func average() -> Double {
        var sum = 0.0
        for index in 0..<count {
            sum += Double(self[index])
        }
        return sum / Double(count)
    }
}
extension Container where Item: Equatable {
    func endsWith(_ item: Item) -> Bool {
        return count >= 1 && self[count-1] == item
    }
}
```

<!--
In the version of this example that uses contextual where clauses, the implementation of average() and endsWith(_:) are both in the same extension because each method’s generic where clause states the requirements that need to be satisfied to make that method available. Moving those requirements to the extensions’ generic where clauses makes the methods available in the same situations, but requires one extension per requirement.
-->

상황별 `where` 절을 사용하는 이 예제의 버전에서 각 메서드의 제너릭 `where` 절은 해당 메서드를 사용할 수 있도록 충족해야 할 요구사항을 명시하기 때문에 `average()` 와 `endsWith(_:)` 의 구현은 모두 같은 확장에 있습니다. 이러한 요구사항을 확장의 제너릭 `where` 절로 이동하면 동일한 상황에서 메서드를 사용할 수 있지만 요구사항 당 하나의 확장이 필요합니다.

## 제너릭 Where 절이 있 연관된 타입 \(Associated Types with a Generic Where Clause\)

<!--
You can include a generic where clause on an associated type. For example, suppose you want to make a version of Container that includes an iterator, like what the Sequence protocol uses in the standard library. Here’s how you write that:
-->

연관된 타입에 제너릭 `where` 절을 포함할 수 있습니다. 예를 들어 표준 라이브러리에서 `Sequence` 프로토콜이 사용하는 것과 같이 반복자를 포함하는 `Container` 의 버전을 만들고 싶다고 가정해 봅시다. 작성 방법은 아래와 같습니다:

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }

    associatedtype Iterator: IteratorProtocol where Iterator.Element == Item
    func makeIterator() -> Iterator
}
```

<!--
The generic where clause on Iterator requires that the iterator must traverse over elements of the same item type as the container’s items, regardless of the iterator’s type. The makeIterator() function provides access to a container’s iterator.

For a protocol that inherits from another protocol, you add a constraint to an inherited associated type by including the generic where clause in the protocol declaration. For example, the following code declares a ComparableContainer protocol that requires Item to conform to Comparable:
-->

`Iterator` 에 제너릭 `where` 절은 반복자의 타입에 상관없이 컨테이너의 항목과 동일한 항목 타입의 요소를 탐색해야 합니다. `makeIterator()` 함수는 컨테이너의 반복자에 접근을 제공합니다.

다른 프로토콜에서 상속하는 프로토콜의 경우 프로토콜 선언에 제너릭 `where` 절을 포함하여 상속된 연관 타입에 제약조건을 추가합니다. 예를 들어 다음의 코드는 `Item` 이 `Comparable` 을 준수하도록 요구하는 `ComparableContainer` 프로토콜을 선언합니다:

```swift
protocol ComparableContainer: Container where Item: Comparable { }
```

## 제너릭 서브 스크립트 \(Generic Subscripts\)

<!--
Subscripts can be generic, and they can include generic where clauses. You write the placeholder type name inside angle brackets after subscript, and you write a generic where clause right before the opening curly brace of the subscript’s body. For example:
-->

서브 스크립트는 제너릭 일 수 있고 제너릭 `where` 절을 포함할 수 있습니다. `subscript` 다음에 꺾쇠 괄호 안에 임의의 타입 이름을 작성하고 서브 스크립트의 바디의 열린 중괄호 전에 제너릭 `where` 절을 작성합니다. 예를 들어:

```swift
extension Container {
    subscript<Indices: Sequence>(indices: Indices) -> [Item]
        where Indices.Iterator.Element == Int {
            var result: [Item] = []
            for index in indices {
                result.append(self[index])
            }
            return result
    }
}
```

<!--
This extension to the Container protocol adds a subscript that takes a sequence of indices and returns an array containing the items at each given index. This generic subscript is constrained as follows:

* The generic parameter Indices in angle brackets has to be a type that conforms to the Sequence protocol from the standard library.
* The subscript takes a single parameter, indices, which is an instance of that Indices type.
* The generic where clause requires that the iterator for the sequence must traverse over elements of type Int. This ensures that the indices in the sequence are the same type as the indices used for a container.

Taken together, these constraints mean that the value passed for the indices parameter is a sequence of integers.
-->

`Container` 프로토콜의 확장은 시퀀스의 인덱스를 가지고 각 주어진 인덱스의 항목을 포함한 배열을 반환하는 서브 스크립트를 추가합니다. 이 제너릭 서브 스크립트는 다음과 같이 제한됩니다:

* 꺾쇠 괄호에 제너릭 파라미터 `Indices` 는 표준 라이브러리의 `Sequence` 프로토콜을 준수하는 타입이어야 합니다.
* 서브 스크립트는 `Indices` 타입의 인스턴스 인 `indices` 라는 단일 파라미터를 가집니다.
* 제너릭 `where` 절은 시퀀스에 대한 반복자는 `Int` 타입의 요소여야 합니다. 이렇게 하면 시퀀스의 인덱스는 컨테이너에 사용되는 인덱스와 동일한 타입입니다.

종합하면 이 제약조건은 `indices` 파리미터에 전달된 값은 정수 시퀀스 임을 의미합니다.

