# 동시성 \(Concurrency\)

<!--
Swift has built-in support for writing asynchronous and parallel code in a structured way. Asynchronous code can be suspended and resumed later, although only one piece of the program executes at a time. Suspending and resuming code in your program lets it continue to make progress on short-term operations like updating its UI while continuing to work on long-running operations like fetching data over the network or parsing files. Parallel code means multiple pieces of code run simultaneously—for example, a computer with a four-core processor can run four pieces of code at the same time, with each core carrying out one of the tasks. A program that uses parallel and asynchronous code carries out multiple operations at a time; it suspends operations that are waiting for an external system, and makes it easier to write this code in a memory-safe way.

The additional scheduling flexibility from parallel or asynchronous code also comes with a cost of increased complexity. Swift lets you express your intent in a way that enables some compile-time checking—for example, you can use actors to safely access mutable state. However, adding concurrency to slow or buggy code isn’t a guarantee that it will become fast or correct. In fact, adding concurrency might even make your code harder to debug. However, using Swift’s language-level support for concurrency in code that needs to be concurrent means Swift can help you catch problems at compile time.

The rest of this chapter uses the term concurrency to refer to this common combination of asynchronous and parallel code.
-->

Swift 는 구조화된 방식으로 비동기 \(asynchronous\) 와 병렬 \(parallel\) 코드 작성을 지원합니다. _비동기 코드 \(Asynchronous code\)_ 는 일시적으로 중단되었다가 다시 실행할 수 있지만 한번에 프로그램의 한 부분만 실행됩니다. 프로그램에서 코드를 일시 중단하고 다시 실행하면 UI 업데이트와 같은 짧은 작업을 계속 진행하면서 네트워크를 통해 데이터를 가져오거나 파일을 분석하는 것과 같은 긴 실행 작업을 계속할 수 있습니다. _병렬 코드 \(Parallel code\)_ 는 동시에 코드의 여러부분이 실행됨을 의미합니다—예를 들어 4코어 프로세서의 컴퓨터는 각 코어가 하나의 작업을 수행하므로 코드의 4부분을 동시에 실행할 수 있습니다. 병렬과 비동기 코드를 사용하는 프로그램은 한 번에 여러 작업을 수행합니다; 외부 시스템을 기다리는 작업을 일시 중단하고 이 코드를 메모리 안전한 방식으로 더 쉽게 작성할 수 있도록 합니다.

병렬 또는 비동기 코드의 추가 스케줄링 유연성에는 복잡성이 증가하는 비용도 수반됩니다. Swift 를 사용하면 일부 컴파일 시간 검사를 가능하게 하는 방식으로 표현할 수 있습니다—예를 들어 실행자를 사용하여 변경가능한 상태에 안전하게 접근할 수 있습니다. 그러나 느리거나 버그가 있는 코드에 동시성을 추가한다고 해서 코드가 빠르거나 올바르게 동작한다는 보장은 없습니다. 사실 동시성을 추가하면 코드를 디버그하기 더 어렵게 만들 수도 있습니다. 그러나 동시성이 필요한 코드에서 동시성에 대한 Swift 의 언어 수준 지원을 사용하면 Swift 가 컴파일 시간에 문제를 찾는데 도움이 될 수 있습니다.

이 챕터의 나머지 부분에서는 _동시성_ 이라는 용어를 사용하여 비동기와 병렬 코드의 일반적인 조합을 나타냅니다.

<!--
NOTE
If you’ve written concurrent code before, you might be used to working with threads. The concurrency model in Swift is built on top of threads, but you don’t interact with them directly. An asynchronous function in Swift can give up the thread that it’s running on, which lets another asynchronous function run on that thread while the first function is blocked.
-->

> NOTE  
> 이전에 동시성 코드를 작성한 적이 있다면 쓰레드 동작에 익숙할 것입니다. Swift 에서 동시성 모델은 쓰레드의 최상단에 구축되지만 직접적으로 상호작용하지 않습니다. Swift 에서 비동기 함수는 실행중인 쓰레드를 포기할 수 있습니다. 그러면 첫번째 함수가 차단되는 동안 해당 쓰레드에서 다른 비동기 함수가 실행될 수 있습니다.

<!--
Although it’s possible to write concurrent code without using Swift’s language support, that code tends to be harder to read. For example, the following code downloads a list of photo names, downloads the first photo in that list, and shows that photo to the user:
-->

Swift 의 언어 지원을 사용하지 않고 동시성 코드를 작성할 수 있지만 해당 코드는 읽기가 더 어려운 경우가 많습니다. 예를 들어 다음 코드는 사진 이름 목록을 다운로드하고 해당 목록의 첫번째 사진을 다운로드하고 해당 사진을 사용자에게 보여줍니다:

```swift
listPhotos(inGallery: "Summer Vacation") { photoNames in
    let sortedNames = photoNames.sorted()
    let name = sortedNames[0]
    downloadPhoto(named: name) { photo in
        show(photo)
    }
}
```

<!--
Even in this simple case, because the code has to be written as a series of completion handlers, you end up writing nested closures. In this style, more complex code with deep nesting can quickly become unwieldy.
-->

이 간단한 경우에도 완료 핸들러가 연속해서 작성되어야 하므로 결국 중첩 클로저를 작성하게 됩니다. 이 스타일에서는 더 많이 중첩된 복잡한 코드는 빠르게 다루기 어려울 수 있습니다.

## 비동기 함수 정의와 호출 \(Defining and Calling Asynchronous Functions\)

<!--
An asynchronous function or asynchronous method is a special kind of function or method that can be suspended while it’s partway through execution. This is in contrast to ordinary, synchronous functions and methods, which either run to completion, throw an error, or never return. An asynchronous function or method still does one of those three things, but it can also pause in the middle when it’s waiting for something. Inside the body of an asynchronous function or method, you mark each of these places where execution can be suspended.

To indicate that a function or method is asynchronous, you write the async keyword in its declaration after its parameters, similar to how you use throws to mark a throwing function. If the function or method returns a value, you write async before the return arrow (->). For example, here’s how you might fetch the names of photos in a gallery:
-->

_비동기 함수 \(asynchronous function\)_ 또는 _비동기 메서드 \(asynchronous method\)_ 는 실행 도중에 일시적으로 중단될 수 있는 특수한 함수 또는 메서드 입니다. 이것은 완료될 때까지 실행되거나 오류가 발생하거나 반환되지 않는 일반적인 동기 함수 또는 메서드 \(synchronous functions and methods\) 와 대조됩니다. 비동기 함수 또는 메서드는 이 세가지 중 하나를 수행하지만 무언가를 기다리고 있을 때 중간에 일시 중지될 수도 있습니다. 비동기 함수 또는 메서드의 본문 내에서 실행을 일시 중지할 수 있는 부분을 표시합니다.

함수 또는 메서드가 비동기 임을 나타내려면 던지는 함수 \(throwing function\) 를 나타내기 위해 `throws` 사용하는 것과 유사하게 파라미터 뒤의 선언에 `async` 키워드를 작성합니다. 함수 또는 메서드가 값을 반환한다면 반환 화살표 \(`->`\) 전에 `async` 를 작성합니다. 예를 들어 갤러리에 사진의 이름을 가져오는 방법은 아래와 같습니다:

```swift
func listPhotos(inGallery name: String) async -> [String] {
    let result = // ... some asynchronous networking code ...
    return result
}
```

<!--
For a function or method that’s both asynchronous and throwing, you write async before throws.

When calling an asynchronous method, execution suspends until that method returns. You write await in front of the call to mark the possible suspension point. This is like writing try when calling a throwing function, to mark the possible change to the program’s flow if there’s an error. Inside an asynchronous method, the flow of execution is suspended only when you call another asynchronous method—suspension is never implicit or preemptive—which means every possible suspension point is marked with await.

For example, the code below fetches the names of all the pictures in a gallery and then shows the first picture:
-->

비동기와 던지기 둘 다인 함수 또는 메서드는 `throws` 전에 `async` 를 작성합니다.

비동기 메서드를 호출할 때 해당 메서드가 반환될 때까지 실행이 일시 중단됩니다. 중단될 가능성이 있는 지점을 표시하기 위해 호출 앞에 `await` 를 작성합니다. 이것은 에러가 있는 경우 프로그램의 흐름을 변경 가능함을 나타내기 위해 던지는 함수를 호출할 때 `try` 를 작성하는 것과 같습니다. 비동기 메서드 내에서 실행 흐름은 다른 비동기 메서드를 호출할 때만 일시 중단됩니다—중단은 암시적이거나 선점적이지 않습니다—이것은 가능한 모든 중단 지점이 `await` 로 표시된다는 의미입니다.

예를 들어 아래 코드는 갤러리에 모든 사진의 이름을 가져온 다음에 첫번째 사진을 보여줍니다:

```swift
let photoNames = await listPhotos(inGallery: "Summer Vacation")
let sortedNames = photoNames.sorted()
let name = sortedNames[0]
let photo = await downloadPhoto(named: name)
show(photo)
```

<!--
Because the listPhotos(inGallery:) and downloadPhoto(named:) functions both need to make network requests, they could take a relatively long time to complete. Making them both asynchronous by writing async before the return arrow lets the rest of the app’s code keep running while this code waits for the picture to be ready.

To understand the concurrent nature of the example above, here’s one possible order of execution:

1. The code starts running from the first line and runs up to the first await. It calls the listPhotos(inGallery:) function and suspends execution while it waits for that function to return.
2. While this code’s execution is suspended, some other concurrent code in the same program runs. For example, maybe a long-running background task continues updating a list of new photo galleries. That code also runs until the next suspension point, marked by await, or until it completes.
3. After listPhotos(inGallery:) returns, this code continues execution starting at that point. It assigns the value that was returned to photoNames.
4. The lines that define sortedNames and name are regular, synchronous code. Because nothing is marked await on these lines, there aren’t any possible suspension points.
5. The next await marks the call to the downloadPhoto(named:) function. This code pauses execution again until that function returns, giving other concurrent code an opportunity to run.
6. After downloadPhoto(named:) returns, its return value is assigned to photo and then passed as an argument when calling show(_:).

The possible suspension points in your code marked with await indicate that the current piece of code might pause execution while waiting for the asynchronous function or method to return. This is also called yielding the thread because, behind the scenes, Swift suspends the execution of your code on the current thread and runs some other code on that thread instead. Because code with await needs to be able to suspend execution, only certain places in your program can call asynchronous functions or methods:

* Code in the body of an asynchronous function, method, or property.
* Code in the static main() method of a structure, class, or enumeration that’s marked with @main.
* Code in an unstructured child task, as shown in Unstructured Concurrency below.
-->

`listPhotos(inGallery:)` 와 `downloadPhoto(named:)` 함수 모두 네트워크 요청을 필요로 하기 때문에 완료하는데 비교적 오랜시간이 걸릴 수 있습니다. 반환 화살표 전에 `async` 를 작성하여 둘 다 비동기로 만들면 이 코드는 그림이 준비될 때까지 기다리는 동안 앱의 나머지 코드가 계속 실행될 수 있습니다.

위 예제의 동시성을 이해하기 위한 실행 순서는 다음과 같습니다:

1. 코드는 첫번째 줄에서 실행을 시작하고 첫번째 `await` 까지 실행됩니다. `listPhotos(inGallery:)` 함수를 호출하고 해당 함수가 반환될 때까지 실행을 일시 중단합니다.
2. 이 코드의 실행이 일시 중단되는 동안 같은 프로그램의 다른 동시 코드가 실행됩니다. 예를 들어 오랜시간 실행되는 백그라운드 작업이 새 사진을 목록을 업데이트 할 수 있습니다. 이 코드는 `await` 로 표시된 다음 중단 지점 또는 완료될 때까지 실행됩니다.
3. `listPhotos(inGallery:)` 가 반환된 후에 이 코드는 해당 지점에서 시작하여 계속 실행됩니다. 반환된 값을 `photoNames` 에 할당합니다.
4. `sortedNames` 와 `name` 을 정의하는 라인은 일반적인 동기 코드 입니다. 이 라인은 `await` 로 표시되지 않았으므로 가능한 중단 지점이 없습니다.
5. 다음 `await` 는 `downloadPhoto(named:)` 함수에 대한 호출을 표시합니다. 이 코드는 해당 함수가 반환될 때까지 실행을 다시 일시 중단하여 다른 동시 코드에 실행할 기회를 제공합니다.
6. `downloadPhoto(named:)` 가 반환된 후에 반환값은 `photo` 에 할당된 다음에 `show(_:)` 를 호출할 때 인수로 전달됩니다.

`await` 로 표시된 코드의 중단이 가능한 지점은 비동기 함수 또는 메서드가 반환되기를 기다리는 동안 현재 코드 부분이 실행을 일시적으로 중단할 수 있음을 나타냅니다. Swift 가 현재 쓰레드에서 코드의 실행을 일시 중단하고 대신 해당 쓰레드에서 다른 코드를 실행하기 때문에 이것을 _쓰레드 양보 \(yielding the thread\)_ 라고도 부릅니다. `await` 가 있는 코드는 실행을 일시 중단할 수 있어야 하므로 프로그램의 특정 위치에서만 비동기 함수 또는 메서드를 호출할 수 있습니다:

* 비동기 함수, 메서드 또는 프로퍼티의 본문에 있는 코드
* `@main` 으로 표시된 구조체, 클래스, 또는 열거형의 정적 \(static\) `main()` 메서드에 있는 코드
* 아래의 [구조화되지 않은 동시성 \(Unstructured Concurrency\)](concurrency.md#unstructured-concurrency) 에 보이는 것처럼 구조화되지 않은 하위 작업의 코드

<!--
Code in between possible suspension points runs sequentially, without the possibility of interruption from other concurrent code. For example, the code below moves a picture from one gallery to another.
-->

중단이 가능한 지점 사이의 코드는 다른 비동기 코드의 중단 가능성없이 순차적으로 실행됩니다. 예를 들어, 아래의 코드는 한 갤러리의 사진을 다른 곳으로 이동시킵니다.

```swift
let firstPhoto = await listPhotos(inGallery: "Summer Vacation")[0]
add(firstPhoto toGallery: "Road Trip")
// At this point, firstPhoto is temporarily in both galleries.
remove(firstPhoto fromGallery: "Summer Vacation")
```

<!--
There’s no way for other code to run in between the call to add(_:toGallery:) and remove(_:fromGallery:). During that time, the first photo appears in both galleries, temporarily breaking one of the app’s invariants. To make it even clearer that this chunk of code must not have await added to it in the future, you can refactor that code into a synchronous function:
-->

`add(_:toGallery:)` 와 `remove(_:fromGallery:)` 사이에 실행되는 다른 코드는 없습니다. 그 시간동안 첫번째 사진은 양쪽 갤러리에 모두 나타나며 앱의 불변성 중 하나를 임시적으로 위반합니다. 이 코드에 `await` 가 확실히 추가되지 말아야 한다는 것을 나타내기 위해 동기 함수로 코드를 리팩토링 할 수 있습니다:

```swift
func move(_ photoName: String, from source: String, to destination: String) {
    add(photoName, to: destination)
    remove(photoName, from: source)
}
// ...
let firstPhoto = await listPhotos(inGallery: "Summer Vacation")[0]
move(firstPhoto, from: "Summer Vacation", to: "Road Trip")
```

<!--
In the example above, because the move(_:from:to:) function is synchronous, you guarantee that it can never contain possible suspension points. In the future, if you try to add concurrent code to this function, introducing a possible suspension point, you’ll get compile-time error instead of introducing a bug.
-->

위의 예제에서 `move(_:from:to:)` 함수는 동기 함수기 때문에 중단 가능한 지점을 포함히지 않는다는 것을 보장할 수 있습니다. 이 함수에 중단 가능한 지점을 도입하기위해 비동기 코드를 추가하면 버그가 아닌 컴파일 에러가 발생합니다.

<!--
NOTE
The Task.sleep(until:tolerance:clock:) method is useful when writing simple code to learn how concurrency works. This method does nothing, but waits at least the given number of nanoseconds before it returns. Here’s a version of the listPhotos(inGallery:) function that uses sleep(until:tolerance:clock:) to simulate waiting for a network operation:
-->

> NOTE [`Task.sleep(until:tolerance:clock:)`](https://developer.apple.com/documentation/swift/task/sleep(until:tolerance:clock:)) 메서드는 동시성 작동 방식을 배우기 위해 간단한 코드를 작성할 때 유용합니다. 이 메서드는 아무런 동작도 하지 않지만 반환되기 전에 주어진 나노 단위의 초만큼 기다립니다. 다음은 네트워크 작업 대기를 시뮬레이션하기 위해 `sleep(until:tolerance:clock:)` 을 사용하는 `listPhoto(inGallery:)` 함수의 버전입니다:
>
> ```swift
> func listPhotos(inGallery name: String) async throws -> [String] {
>     try await Task.sleep(until: .now + .seconds(2), clock: .continuous)
>     return ["IMG001", "IMG99", "IMG0404"]
> }
> ```

## 비동기 시퀀스 \(Asynchronous Sequences\)

<!--
The listPhotos(inGallery:) function in the previous section asynchronously returns the whole array at once, after all of the array’s elements are ready. Another approach is to wait for one element of the collection at a time using an asynchronous sequence. Here’s what iterating over an asynchronous sequence looks like:
-->

이전 섹션에서 `listPhotos(inGallery:)` 함수는 비동기적으로 배열의 모든 요소가 준비된 후에 전체 배열을 한번에 반환합니다. 또 다른 접근 방식은 _비동기 시퀀스 \(asynchronous sequence\)_ 를 사용하여 한번에 콜렉션의 한 요소를 기다리는 것입니다. 비동기 시퀀스에 대한 조회 동작은 다음과 같습니다:

```swift
import Foundation

let handle = FileHandle.standardInput
for try await line in handle.bytes.lines {
    print(line)
}
```

<!--
Instead of using an ordinary for-in loop, the example above writes for with await after it. Like when you call an asynchronous function or method, writing await indicates a possible suspension point. A for-await-in loop potentially suspends execution at the beginning of each iteration, when it’s waiting for the next element to be available.

In the same way that you can use your own types in a for-in loop by adding conformance to the Sequence protocol, you can use your own types in a for-await-in loop by adding conformance to the AsyncSequence protocol.
-->

일반적인 `for`-`in` 루프 대신에 위의 예제는 `for` 다음에 `await` 를 작성합니다. 비동기 함수 또는 메서드 호출할 때와 마찬가지로 `await` 작성은 가능한 중단 지점을 나타냅니다. `for`-`await`-`in` 루프는 다음 요소를 사용할 수 있을 때까지 기다리고 각 반복이 시작될 때 잠재적으로 실행을 일시 중단합니다.

[`Sequence`](https://developer.apple.com/documentation/swift/sequence) 프로토콜에 준수성을 추가하여 `for`-`in` 루프에서 자체 타입을 사용할 수 있는 것과 같은 방식으로 [`AsyncSequence`](https://developer.apple.com/documentation/swift/asyncsequence) 프로토콜에 준수성을 추가하여 `for`-`await`-`in` 루프에서 자체 타입을 사용할 수 있습니다.

## 비동기 함수 병렬로 호출 \(Calling Asynchronous Functions in Parallel\)

<!--
Calling an asynchronous function with await runs only one piece of code at a time. While the asynchronous code is running, the caller waits for that code to finish before moving on to run the next line of code. For example, to fetch the first three photos from a gallery, you could await three calls to the downloadPhoto(named:) function as follows:
-->

`await` 를 사용하여 비동기 함수를 호출하면 한번에 코드의 한 부분만 실행됩니다. 비동기 코드가 실행되는 동안 호출자는 코드의 다음 라인을 실행하기 위해 이동하기 전에 해당 코드가 완료될 때까지 기다립니다. 예를 들어 갤러리에서 처음 세 장의 사진을 가져오려면 다음과 같이 `downloadPhoto(named:)` 함수에 대한 세 번의 호출을 기다릴 수 있습니다:

```swift
let firstPhoto = await downloadPhoto(named: photoNames[0])
let secondPhoto = await downloadPhoto(named: photoNames[1])
let thirdPhoto = await downloadPhoto(named: photoNames[2])

let photos = [firstPhoto, secondPhoto, thirdPhoto]
show(photos)
```

<!--
This approach has an important drawback: Although the download is asynchronous and lets other work happen while it progresses, only one call to downloadPhoto(named:) runs at a time. Each photo downloads completely before the next one starts downloading. However, there’s no need for these operations to wait—each photo can download independently, or even at the same time.

To call an asynchronous function and let it run in parallel with code around it, write async in front of let when you define a constant, and then write await each time you use the constant.
-->

이 방식에는 중요한 단점이 있습니다: 다운로드가 비동기이고 진행되는 동안 다른 작업을 수행할 수 있지만 `downloadPhoto(named:)` 에 대한 호출은 한 번에 하나만 실행됩니다. 각 사진은 다음 사진이 다운로드를 시작하기 전에 완료됩니다. 그러나 이런 작업을 기다릴 필요가 없습니다—각 사진은 개별적으로 또는 동시에 다운로드 할 수 있습니다.

비동기 함수를 호출하고 주변의 코드와 병렬로 실행하려면 상수를 정의할 때 `let` 앞에 `async` 를 작성하고 상수를 사용할 때마다 `await` 를 작성합니다.

```swift
async let firstPhoto = downloadPhoto(named: photoNames[0])
async let secondPhoto = downloadPhoto(named: photoNames[1])
async let thirdPhoto = downloadPhoto(named: photoNames[2])

let photos = await [firstPhoto, secondPhoto, thirdPhoto]
show(photos)
```

<!--
In this example, all three calls to downloadPhoto(named:) start without waiting for the previous one to complete. If there are enough system resources available, they can run at the same time. None of these function calls are marked with await because the code doesn’t suspend to wait for the function’s result. Instead, execution continues until the line where photos is defined—at that point, the program needs the results from these asynchronous calls, so you write await to pause execution until all three photos finish downloading.

Here’s how you can think about the differences between these two approaches:

* Call asynchronous functions with await when the code on the following lines depends on that function’s result. This creates work that is carried out sequentially.
* Call asynchronous functions with async-let when you don’t need the result until later in your code. This creates work that can be carried out in parallel.
* Both await and async-let allow other code to run while they’re suspended.
* In both cases, you mark the possible suspension point with await to indicate that execution will pause, if needed, until an asynchronous function has returned.

You can also mix both of these approaches in the same code.
-->

이 예제에서 `downloadPhoto(named:)` 을 호출하는 세가지는 모두 이전 호출이 완료되길 기다리지 않고 시작됩니다. 사용할 수 있는 시스템 자원이 충분하다면 동시에 실행할 수 있습니다. 코드가 함수의 결과를 기다리기 위해 일시 중단되지 않기 때문에 이러한 함수 호출 중 어느 것도 `await` 로 표시하지 않습니다. 대신 `photos` 가 정의된 라인까지 실행이 계속됩니다—이 시점에서 프로그램은 이러한 비동기 호출의 결과를 필요로 하므로 세 장의 사진이 모두 다운로드 될 때까지 실행을 일시 중단하기 위해 `await` 를 작성합니다.

다음은 이 두 접근 방식의 차이점에 대해 생각할 수 있는 방법입니다:

* 다음 줄의 코드가 해당 함수의 결과에 따라 달라지면 `await` 를 사용하여 비동기 함수를 호출합니다. 이것은 순차적으로 실행되는 작업을 생성합니다.
* 나중에 코드에서 결과가 필요하지 않을 때 `async`-`let` 을 사용하여 비동기 함수를 호출합니다. 이렇게 하면 병렬로 수행할 수 있는 작업이 생성됩니다.
* `await` 와 `async`-`let` 은 모두 일시 중단되는 동안 다른 코드를 실행할 수 있도록 합니다.
* 두 경우 모두 비동기 함수가 반환될 때까지 필요한 경우 실행이 일시 중단됨을 나타내기 위해 가능한 일시 중단 지점을 `await` 로 표시합니다.

동일한 코드에서 이 두 가지 접근 방식을 혼합할 수도 있습니다.

## 작업과 작업 그룹 \(Tasks and Task Groups\)

<!--
A task is a unit of work that can be run asynchronously as part of your program. All asynchronous code runs as part of some task. The async-let syntax described in the previous section creates a child task for you. You can also create a task group and add child tasks to that group, which gives you more control over priority and cancellation, and lets you create a dynamic number of tasks.

Tasks are arranged in a hierarchy. Each task in a task group has the same parent task, and each task can have child tasks. Because of the explicit relationship between tasks and task groups, this approach is called structured concurrency. Although you take on some of the responsibility for correctness, the explicit parent-child relationships between tasks lets Swift handle some behaviors like propagating cancellation for you, and lets Swift detect some errors at compile time.
-->

_작업 \(task\)_ 은 프로그램의 일부로 비동기적으로 실행할 수 있는 작업 단위입니다. 모든 비동기 코드는 어떠한 작업의 일부로 실행됩니다. 이전 섹션에서 설명한 `async`-`let` 구문은 하위 작업을 생성합니다. 작업 그룹 \(task group\) 을 생성하고 해당 그룹에 하위 작업을 추가할 수도 있습니다. 그러면 우선순위와 취소를 더 잘 제어할 수 있으며 동적으로 작업의 수를 생성할 수 있습니다.

작업은 계층 구조로 정렬됩니다. 작업 그룹의 각 작업에는 동일한 상위 작업이 있으며 각 작업에는 하위 작업이 있을 수도 있습니다. 작업과 작업 그룹 간의 명시적 관계 때문에 이 접근 방식을 _구조적 동시성 \(structured concurrency\)_ 이라고 합니다. 정확성에 대한 일부 책임을 지고 있지만 작업 간의 명시적 부모 \(parent\)-자식 \(child\) 관계를 통해 Swift 는 취소 전파 \(propagating cancellation\) 와 같은 일부 동작을 처리할 수 있고 Swift 는 컴파일 시간에 일부 오류를 감지할 수 있습니다.

```swift
await withTaskGroup(of: Data.self) { taskGroup in
    let photoNames = await listPhotos(inGallery: "Summer Vacation")
    for name in photoNames {
        taskGroup.addTask { await downloadPhoto(named: name) }
    }
}
```

<!--
For more information about task groups, see TaskGroup.
-->

작업 그룹에 대한 자세한 내용은 [`TaskGroup`](https://developer.apple.com/documentation/swift/taskgroup) 을 참고 바랍니다.

### 구조화되지 않은 동시성 \(Unstructured Concurrency\)

<!--
In addition to the structured approaches to concurrency described in the previous sections, Swift also supports unstructured concurrency. Unlike tasks that are part of a task group, an unstructured task doesn’t have a parent task. You have complete flexibility to manage unstructured tasks in whatever way your program needs, but you’re also completely responsible for their correctness. To create an unstructured task that runs on the current actor, call the Task.init(priority:operation:) initializer. To create an unstructured task that’s not part of the current actor, known more specifically as a detached task, call the Task.detached(priority:operation:) class method. Both of these operations return a task that you can interact with—for example, to wait for its result or to cancel it.
-->

이전 섹션에서 설명한 동시성에 대한 구조화된 접근 방식 외에도 Swift 는 구조화되지 않은 동시성 \(unstructured concurrency\) 을 지원합니다. 작업 그룹의 일부인 작업과 달리 _구조화되지 않은 작업 \(unstructured task\)_ 에는 상위 작업이 없습니다. 프로그램이 필요로 하는 방식으로 구조화되지 않은 작업을 관리할 수 있는 완전한 유연성이 있지만 정확성에 대한 완전한 책임도 있습니다. 현재 행위자 \(actor\) 에서 실행되는 구조화되지 않은 작업을 생성하려면 [`Task.init(priority:operation:)`](https://developer.apple.com/documentation/swift/task/3856790-init) 초기화 구문을 호출해야 합니다. 더 구체적으로 분리된 작업으로 알려진 현재 행위자의 일부가 아닌 구조화되지 않은 작업을 생성하려면 [`Task.detached(priority:operation:)`](https://developer.apple.com/documentation/swift/task/3856786-detached) 클래스 메서드를 호출합니다. 이 모든 동작은 서로 상호작용 할 수 있는 작업 \(task\)을 반환합니다—예를 들어 결과를 기다리거나 취소하는 경우가 해당됩니다.

```swift
let newPhoto = // ... some photo data ...
let handle = Task {
    return await add(newPhoto, toGalleryNamed: "Spring Adventures")
}
let result = await handle.value
```

<!--
For more information about managing detached tasks, see Task.
-->

분리된 작업 \(detached tasks\) 관리에 대한 자세한 내용은 [`Task`](https://developer.apple.com/documentation/swift/task) 를 참고 바랍니다.

### 작업 취소 \(Task Cancellation\)

<!--
Swift concurrency uses a cooperative cancellation model. Each task checks whether it has been canceled at the appropriate points in its execution, and responds to cancellation in whatever way is appropriate. Depending on the work you’re doing, that usually means one of the following:

* Throwing an error like CancellationError
* Returning nil or an empty collection
* Returning the partially completed work

To check for cancellation, either call Task.checkCancellation(), which throws CancellationError if the task has been canceled, or check the value of Task.isCancelled and handle the cancellation in your own code. For example, a task that’s downloading photos from a gallery might need to delete partial downloads and close network connections.

To propagate cancellation manually, call Task.cancel().
-->

Swift 동시성은 협동 취소 모델 \(cooperative cancellation model\) 을 사용합니다. 각 작업은 실행의 적절한 시점에서 취소되었는지 확인하고 적절한 방법으로 취소에 응답합니다. 수행 중인 작업에 따라 일반적으로 다음 중 하나를 의미합니다:

* `CancellationError` 와 같은 에러 발생
* `nil` 또는 빈 콜렉션 반환
* 부분적으로 완료된 작업 반환

취소를 확인하려면 작업이 취소된 경우 `CancellationError` 를 발생시키는 [`Task.checkCancellation()`](https://developer.apple.com/documentation/swift/task/3814826-checkcancellation) 을 호출하거나 [`Task.isCancelled`](https://developer.apple.com/documentation/swift/task/3814832-iscancelled) 의 값을 확인하고 자체 코드에서 취소를 처리합니다. 예를 들어 갤러리에서 사진을 다운로드 하는 작업은 일부 다운로드를 삭제하고 네트워크 연결을 닫아야 할 수 있습니다.

취소를 수동으로 전파하려면 [`Task.cancel()`](https://developer.apple.com/documentation/swift/task/3851218-cancel) 을 호출해야 합니다.

## 행위자 \(Actors\)

<!--
You can use tasks to break up your program into isolated, concurrent pieces. Tasks are isolated from each other, which is what makes it safe for them to run at the same time, but sometimes you need to share some information between tasks. Actors let you safely share information between concurrent code.
-->

프로그램을 동시성 조각으로 분리하기위해 작업 \(task\) 을 사용할 수 있습니다. 작업은 서로 분리되어 있어 같은 시간 안전하게 실행될 수 있지만 작업 간에 일부 정보를 공유해야 할 수도 있습니다. 행위자 \(Actors\) 는 동시성 코드간에 정보를 안전하게 공유할 수 있게 해줍니다.

<!--
Like classes, actors are reference types, so the comparison of value types and reference types in Classes Are Reference Types applies to actors as well as classes. Unlike classes, actors allow only one task to access their mutable state at a time, which makes it safe for code in multiple tasks to interact with the same instance of an actor. For example, here’s an actor that records temperatures:
-->

클래스와 마찬가지로 행위자 \(actors\) 는 참조 타입이므로 [클래스는 참조 타입 \(Classes Are Reference Types\)](structures-and-classes.md#classes-are-reference-types) 에서 값 타입과 참조 타입의 비교는 클래스 뿐만 아니라 행위자에도 적용됩니다. 클래스와 다르게 행위자는 한 번에 하나의 작업만 변경 가능한 상태에 접근할 수 있도록 허용하므로 여러 작업의 코드가 행위자의 동일한 인스턴스와 상호작용 하는 것은 안전합니다. 예를 들어 다음은 온도를 기록하는 행위자 입니다:

```swift
actor TemperatureLogger {
    let label: String
    var measurements: [Int]
    private(set) var max: Int

    init(label: String, measurement: Int) {
        self.label = label
        self.measurements = [measurement]
        self.max = measurement
    }
}
```

<!--
You introduce an actor with the actor keyword, followed by its definition in a pair of braces. The TemperatureLogger actor has properties that other code outside the actor can access, and restricts the max property so only code inside the actor can update the maximum value.

You create an instance of an actor using the same initializer syntax as structures and classes. When you access a property or method of an actor, you use await to mark the potential suspension point. For example:
-->

`actor` 키워드를 사용하여 행위자를 도입하고 중괄호로 정의합니다. `TemperatureLogger` 행위자는 행위자 외부의 다른 코드가 접근할 수 있는 프로퍼티가 있으며 행위자 내부의 코드만 최대값을 업데이트 할 수 있게 `max` 프로퍼티를 제한합니다.

구조체와 클래스와 같은 초기화 구문으로 행위자의 인스턴스를 생성합니다. 행위자의 프로퍼티 또는 메서드에 접근할 때 일시 중단 지점을 나타내기 위해 `await` 를 사용합니다. 예를 들어:

```swift
let logger = TemperatureLogger(label: "Outdoors", measurement: 25)
print(await logger.max)
// Prints "25"
```

<!--
In this example, accessing logger.max is a possible suspension point. Because the actor allows only one task at a time to access its mutable state, if code from another task is already interacting with the logger, this code suspends while it waits to access the property.

In contrast, code that’s part of the actor doesn’t write await when accessing the actor’s properties. For example, here’s a method that updates a TemperatureLogger with a new temperature:
-->

이 예제에서 `logger.max` 에 접근하는 것은 일시 중단 지점으로 가능합니다. 행위자는 한 번에 하나의 작업만 변경 가능한 상태에 접근할 수 있도록 허용하므로 다른 작업의 코드가 이미 로거와 상호 작용하고 있는 경우 이 코드는 프로퍼티 접근을 기다리는 동안 일시 중단됩니다.

대조적으로 행위자의 부분인 코드는 행위자의 프로퍼티에 접근할 때 `await` 를 작성하지 않습니다. 예를 들어 새로운 온도로 `TemperatureLogger` 를 업데이트 하는 메서드 입니다:

```swift
extension TemperatureLogger {
    func update(with measurement: Int) {
        measurements.append(measurement)
        if measurement > max {
            max = measurement
        }
    }
}
```

<!--
The update(with:) method is already running on the actor, so it doesn’t mark its access to properties like max with await. This method also shows one of the reasons why actors allow only one task at a time to interact with their mutable state: Some updates to an actor’s state temporarily break invariants. The TemperatureLogger actor keeps track of a list of temperatures and a maximum temperature, and it updates the maximum temperature when you record a new measurement. In the middle of an update, after appending the new measurement but before updating max, the temperature logger is in a temporary inconsistent state. Preventing multiple tasks from interacting with the same instance simultaneously prevents problems like the following sequence of events:

1. Your code calls the update(with:) method. It updates the measurements array first.
2. Before your code can update max, code elsewhere reads the maximum value and the array of temperatures.
3. Your code finishes its update by changing max.

In this case, the code running elsewhere would read incorrect information because its access to the actor was interleaved in the middle of the call to update(with:) while the data was temporarily invalid. You can prevent this problem when using Swift actors because they only allow one operation on their state at a time, and because that code can be interrupted only in places where await marks a suspension point. Because update(with:) doesn’t contain any suspension points, no other code can access the data in the middle of an update.

If you try to access those properties from outside the actor, like you would with an instance of a class, you’ll get a compile-time error. For example:
-->

`update(with:)` 메서드는 행위자에서 이미 실행 중이므로 `max` 와 같은 프로퍼티에 대한 접근을 `await` 로 표시하지 않습니다. 이 메서드는 행위자가 변경 가능한 상태와 상호 작용하기 위해 한 번에 하나의 작업만 허용하는 이유 중 하나를 보여줍니다: 행위자의 상태에 대한 일부 업데이트는 일시적으로 불변성을 깨뜨립니다. `TemperatureLogger` 행위자는 온도 목록과 최대 온도를 추적하고 새로운 측정값을 기록할 때 최대 온도를 업데이트 합니다. 업데이트 도중에 새로운 측정값을 추가한 후 `max` 를 업데이트 하기 전에 온도 로거는 일시적으로 일치하지 않는 상태가 됩니다. 여러 작업이 동일한 인스턴스에 상호 작용하는 것을 방지하면 다음 이벤트 시퀀스와 같은 문제를 방지할 수 있습니다:

1. 코드는 `update(with:)` 메서드를 호출합니다. 먼저 `measurements` 배열을 업데이트 합니다.
2. 코드에서 `max` 를 업데이트 하기 전에 다른 코드에서 최대값과 온도 배열을 읽습니다.
3. 코드는 `max` 를 변경하여 업데이트를 완료합니다.

이러한 경우 다른 곳에서 실행 중인 코드는 데이터가 일시적으로 유효하지 않은 동안 `update(with:)` 호출 중간에 행위자에 대한 접근이 인터리브 \(interleaved\) 되어 잘못된 정보를 읽습니다. Swift 행위자는 한 번에 해당 상태에 대해 하나의 작업만 허용하고 해당 코드는 `await` 가 일시 중단 지점으로 표시되는 위치에서만 중단될 수 있기 때문에 Swift 행위자를 사용하여 이 문제를 방지할 수 있습니다. `update(with:)` 는 일시 중단 지점을 포함하지 않으므로 다른 코드는 업데이트 중간에 데이터에 접근할 수 없습니다.

클래스의 인스턴스와 같이 행위자의 외부에서 프로퍼티에 접근하려고 하면 컴파일 때 에러가 발생합니다. 예를 들어:

```swift
print(logger.max)  // Error
```

<!--
Accessing logger.max without writing await fails because the properties of an actor are part of that actor’s isolated local state. Swift guarantees that only code inside an actor can access the actor’s local state. This guarantee is known as actor isolation.
-->

`await` 작성 없이 `logger.max` 에 접근하는 것은 행위자의 프로퍼티가 해당 행위자의 분리된 로컬 상태의 부분이기 때문에 실패합니다. Swift 는 행위자 내부의 코드만 행위자의 로컬 상태에 접근할 수 있도록 보장합니다. 이 보장을 _행위자 분리 \(actor isolation\)_ 이라고 합니다.

## 전송 가능 타입 \(Sendable Types\)

<!--
Tasks and actors let you divide a program into pieces that can safely run concurrently. Inside of a task or an instance of an actor, the part of a program that contains mutable state, like variables and properties, is called a concurrency domain. Some kinds of data can’t be shared between concurrency domains, because that data contains mutable state, but it doesn’t protect against overlapping access.
-->

작업 \(Tasks\) 와 행위자 \(actors\) 는 프로그램을 동시에 안전하게 실행할 수 있는 조각으로 나눌 수 있습니다. 작업 또는 행위자의 인스턴스 내에서 변수와 프로퍼티와 같은 변경 가능한 상태를 포함하는 프로그램의 일부분을 동시성 도메인 \(concurrency domain\) 이라고 부릅니다. 어떤 데이터는 데이터가 변경 가능한 상태를 포함하지만 동시 접근에 대해 보호되지 않으므로 동시성 도메인 간에 공유될 수 없습니다.

<!--
A type that can be shared from one concurrency domain to another is known as a sendable type. For example, it can be passed as an argument when calling an actor method or be returned as the result of a task. The examples earlier in this chapter didn’t discuss sendability because those examples use simple value types that are always safe to share for the data being passed between concurrency domains. In contrast, some types aren’t safe to pass across concurrency domains. For example, a class that contains mutable properties and doesn’t serialize access to those properties can produce unpredictable and incorrect results when you pass instances of that class between different tasks.
-->

한 동시성 도메인에서 다른 동시성 도메인으로 공유될 수 있는 타입을 _전송 가능_ 타입 \(_sendable_ type\) 이라고 합니다. 예를 들어, 행위자 메서드로 호출될 때 인자로 전달되거나 작업의 결과로 반환될 수 있습니다. 이 챕터의 앞부분에 있는 예제들은 동시성 도메인 간에 전달되는 데이터는 항상 안전한 간단한 값 타입을 사용하기 때문에 전송 가능성에 대해 논의하지 않았습니다. 반대로 일부 타입은 동시성 도메인 간에 전달하기 위해 안전하지 않습니다. 예를 들어, 변경 가능한 프로퍼티를 포함하고 해당 프로퍼티에 순차적으로 접근하지 않는 클래스는 서로 다른 작업 클래스의 인스턴스에 전달될 때 예상할 수 없고 잘못된 결과를 생성할 수 있습니다.

<!--
You mark a type as being sendable by declaring conformance to the Sendable protocol. That protocol doesn’t have any code requirements, but it does have semantic requirements that Swift enforces. In general, there are three ways for a type to be sendable:

* The type is a value type, and its mutable state is made up of other sendable data—for example, a structure with stored properties that are sendable or an enumeration with associated values that are sendable.

* The type doesn’t have any mutable state, and its immutable state is made up of other sendable data—for example, a structure or class that has only read-only properties.

* The type has code that ensures the safety of its mutable state, like a class that’s marked @MainActor or a class that serializes access to its properties on a particular thread or queue.

For a detailed list of the semantic requirements, see the Sendable protocol reference.

Some types are always sendable, like structures that have only sendable properties and enumerations that have only sendable associated values. For example:
-->

`Sendable` 프로토콜을 선언하여 전송 가능한 타입으로 표시합니다. 이 프로토콜은 어떠한 코드 요구사항을 가지지 않지만 Swift 가 적용하는 의미론적 요구사항이 있습니다. 일반적으로 타입을 전송 가능한 것으로 나타내기 위한 세가지 방법이 있습니다:

* 타입은 값 타입이고 변경 가능한 상태는 다른 전송 가능한 데이터로 구성됩니다-예를 들어, 전송 가능한 저장된 프로퍼티가 있는 구조체 또는 전송 가능한 연관된 값이 있는 열거형이 있습니다.

* 타입은 변경 가능한 상태가 없으며 변경 불가능한 상태는 다른 전송 가능한 데이터로 구성됩니다-예를 들어, 읽기전용 프로퍼티만 있는 구조체 또는 클래스가 있습니다.

* 타입은 `@MainActor` 로 표시된 클래스나 특정 쓰레드나 큐에서 프로퍼티에 순차적으로 접근하는 클래스와 같이 변경 가능한 상태의 안정성을 보장하는 코드를 가지고 있습니다.

의미론적 요구사항의 자세한 리스트는 [전송 가능 \(Sendable\)](https://developer.apple.com/documentation/swift/sendable) 프로토콜을 참고 바랍니다.

전송 가능한 프로퍼티만 가지는 구조체와 전송 가능한 연관된 값만 가지는 열거형과 같이 어떠한 타입은 항상 전송 가능합니다. 예를 들어:

```swift
struct TemperatureReading: Sendable {
    var measurement: Int
}

extension TemperatureLogger {
    func addReading(from reading: TemperatureReading) {
        measurements.append(reading.measurement)
    }
}

let logger = TemperatureLogger(label: "Tea kettle", measurement: 85)
let reading = TemperatureReading(measurement: 45)
await logger.addReading(from: reading)
```

<!--
Because TemperatureReading is a structure that has only sendable properties, and the structure isn’t marked public or @usableFromInline, it’s implicitly sendable. Here’s a version of the structure where conformance to the Sendable protocol is implied:
-->

`TemperatureReading` 은 전송 가능한 프로퍼티만 가지는 구조체이며 `public` 또는 `@usableFromInline` 으로 표시되지 않은 구조체이므로 암시적으로 전송 가능합니다. 다음은 `Sendable` 프로토콜을 준수가 암시되는 구조체입니다:

```swift
struct TemperatureReading {
    var measurement: Int
}
```