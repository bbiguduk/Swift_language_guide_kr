{% hint style="info" %}
📢 **공지**  
이 문서는 더 이상 업데이트되지 않습니다.  
아래 새로운 URL로 업데이트 되었으니, 새로운 URL로 접속해 주세요.  
[Swift Book Korean](https://bbiguduk.github.io/swift-book-korean/documentation/tsplk/)
{% endhint %}

# 동시성 (Concurrency)

비동기 동작을 수행합니다.

Swift는 구조화된 방식으로
비동기(asynchronous)와 병렬(parallel) 코드 작성을 지원합니다.
*비동기 코드(Asynchronous code)*는 일시적으로 중단되었다가
다시 실행할 수 있지만 한 번에 프로그램의 한 부분만 실행됩니다.
프로그램에서 코드를 일시 중단하고 다시 실행하면
UI 업데이트와 같은 짧은 작업을
계속 진행하면서
네트워크를 통해 데이터를 가져오거나 파일을 분석하는 것과 같은
긴 실행 작업을 계속할 수 있습니다.
*병렬 코드(Parallel code)*는 동시에 코드의 여러 부분이 실행됨을 의미합니다 —--
예를 들어 4코어 프로세서의 컴퓨터는
각 코어가 하나의 작업을 수행하므로
코드의 네 부분을 동시에 실행할 수 있습니다.
병렬과 비동기 코드를 사용하는 프로그램은
한 번에 여러 작업을 수행하고,
외부 시스템을 기다리는 작업은 일시 중단됩니다.
이 챕터의 나머지 부분에서는 *동시성*이라는 용어를
비동기 코드와 병렬 코드의 일반적인 조합을 가르키는 의미로 사용합니다.

병렬이나 비동기 코드의 추가적인 스케줄링 유연성에는
복잡성이 증가합니다.
동시성 코드를 작성할 때
어떤 코드가 동시에 실행될지 미리 알 수 없으며,
코드 실행 순서도 항상 알 수 없을 수 있습니다.
동시성 코드에서 흔히 발생하는 문제는
여러 코드가 일부 공유 가능한 가변 상태에
접근하려고 할 때 발생하고 ---
이것을 *데이터 경쟁(data race)*이라고 합니다.
동시성을 지원하는 언어 수준을 사용하면,
Swift는 데이터 경쟁을 감지하고 방지하며
대부분의 데이터 경쟁은 컴파일 시에 오류를 발생시킵니다.
일부 데이터 경쟁은 코드를 실행될 때까지 감지되지 않을 수 있습니다;
이러한 데이터 경쟁은 코드 실행을 종료시킵니다.
이 챕터에서 설명한대로
액터와 격리를 사용하면 데이터 경쟁으로부터 보호할 수 있습니다.

> Note: 이전에 동시성 코드를 작성한 적이 있다면,
> 스레드 동작에 익숙할 것입니다.
> Swift의 동시성 모델은 스레드 위에 구축되어 있지만,
> 직접적으로 스레드를 다루지는 않습니다.
> Swift의 비동기 함수는
> 실행 중인 스레드를 포기할 수 있습니다.
> 그러면 첫 번째 함수가 차단되어 있는 동안
> 해당 스레드에서 다른 비동기 함수가 실행될 수 있습니다.
> 비동기 함수의 실행이 재개되면,
> Swift는 해당 함수가 어떤 스레드에서 실행될지
> 어떠한 보장도 하지 않습니다.

Swift의 언어 지원을 사용하지 않고,
동시성 코드를 작성할 수 있지만
해당 코드는 읽기가 더 어려운 경우가 많습니다.
예를 들어 다음 코드는 사진 이름 목록을 다운로드하고,
해당 목록의 첫 번째 사진을 다운로드하고,
해당 사진을 사용자에게 보여줍니다:

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
  - test: `async-via-nested-completion-handlers`

  ```swifttest
  >> struct Data {}  // Instead of actually importing Foundation
  >> func listPhotos(inGallery name: String, completionHandler: ([String]) -> Void ) {
  >>   completionHandler(["IMG001", "IMG99", "IMG0404"])
  >> }
  >> func downloadPhoto(named name: String, completionHandler: (Data) -> Void) {
  >>     completionHandler(Data())
  >> }
  >> func show(_ image: Data) { }
  -> listPhotos(inGallery: "Summer Vacation") { photoNames in
         let sortedNames = photoNames.sorted()
         let name = sortedNames[0]
         downloadPhoto(named: name) { photo in
             show(photo)
         }
     }
  ```
-->

이 간단한 경우에도
완료 핸들러가 연속해서 작성되어야 하므로,
결국 중첩 클로저를 작성하게 됩니다.
이 스타일에서
더 많이 중첩된 복잡한 코드는 빠르게 파악하기 어려울 수 있습니다.

## 비동기 함수 정의와 호출 (Defining and Calling Asynchronous Functions)

*비동기 함수(asynchronous function)*나 *비동기 메서드(asynchronous method)*는
실행 도중에 일시적으로 중단될 수 있는
특수한 함수나 메서드 입니다.
이것은 완료될 때까지 실행되거나 오류가 발생하거나 반환되지 않는
일반적인 동기 함수(synchronous function)나 메서드(synchronous method)와 대조됩니다.
비동기 함수나 메서드도 이 세 가지 중 하나를 수행하지만
무언가를 기다리고 있을 때 중간에 일시 중지될 수도 있습니다.
비동기 함수나 메서드의 본문 내에서
실행을 일시 중지할 수 있는 부분을 표시합니다.

함수나 메서드가 비동기임을 나타내려면
던지는 함수(throwing function)를 나타내기 위해 `throws` 사용하는 것과 유사하게
매개변수 뒤의 선언에 `async` 키워드를 작성합니다.
함수나 메서드가 값을 반환한다면,
반환 화살표(`->`) 앞에 `async`를 작성합니다.
예를 들어
갤러리에 사진의 이름을 가져오는 방법은 아래와 같습니다:

```swift
func listPhotos(inGallery name: String) async -> [String] {
    let result = // ... some asynchronous networking code ...
    return result
}
```

<!--
  - test: `async-function-shape`

  ```swifttest
  -> func listPhotos(inGallery name: String) async -> [String] {
         let result = // ... some asynchronous networking code ...
  >>     ["IMG001", "IMG99", "IMG0404"]
         return result
     }
  ```
-->

비동기와 던지는 함수나 메서드는
`throws` 앞에 `async`를 작성합니다.

<!--
  - test: `async-comes-before-throws`

  ```swifttest
  >> func right() async throws -> Int { return 12 }
  >> func wrong() throws async -> Int { return 12 }
  !$ error: 'async' must precede 'throws'
  !! func wrong() throws async -> Int { return 12 }
  !! ^~~~~~
  !! async
  ```
-->

비동기 메서드를 호출할 때,
해당 메서드가 반환할 때까지 실행이 일시 중단됩니다.
중단될 가능성이 있는 지점을 표시하기 위해
호출 앞에 `await`를 작성합니다.
이것은 오류가 있는 경우 프로그램의 흐름이 변경 가능함을 나타내기 위해
던지는 함수를 호출할 때 `try`를 작성하는 것과 같습니다.
비동기 메서드 내에서
실행 흐름은 다른 비동기 메서드를
호출할 때*만* 일시 중단될 수 있습니다 —--
중단은 암시적이거나 선점적이지 않습니다 —--
이것은 가능한 모든 중단 지점이 `await`로 표시된다는 의미입니다.
코드에서 중단 가능한 모든 지점을 표시하면
동시성 코드를 읽기 쉽고 이해하기 쉽게 만들어 줍니다.

예를 들어
아래 코드는 갤러리에 모든 사진의 이름을 가져온 다음에
첫 번째 사진을 보여줍니다:

```swift
let photoNames = await listPhotos(inGallery: "Summer Vacation")
let sortedNames = photoNames.sorted()
let name = sortedNames[0]
let photo = await downloadPhoto(named: name)
show(photo)
```

<!--
  - test: `defining-async-function`

  ```swifttest
  >> struct Data {}  // Instead of actually importing Foundation
  >> func downloadPhoto(named name: String) async -> Data { return Data() }
  >> func show(_ image: Data) { }
  >> func listPhotos(inGallery name: String) async -> [String] {
  >>     return ["IMG001", "IMG99", "IMG0404"]
  >> }
  >> func f() async {
  -> let photoNames = await listPhotos(inGallery: "Summer Vacation")
  -> let sortedNames = photoNames.sorted()
  -> let name = sortedNames[0]
  -> let photo = await downloadPhoto(named: name)
  -> show(photo)
  >> }
  ```
-->

`listPhotos(inGallery:)`와 `downloadPhoto(named:)` 함수
모두 네트워크 요청을 필요로 하기 때문에,
완료하는데 비교적 오랜 시간이 걸릴 수 있습니다.
반환 화살표 전에 `async`를 작성하여 둘 다 비동기로 만들면
이 코드는 사진이 준비될 때까지 기다리는 동안
앱의 나머지 코드가 계속 실행될 수 있습니다.

위 예시의 동시성을 이해하기 위한
실행 순서는 다음과 같습니다:

1. 코드는 첫 번째 줄에서 실행을 시작하고,
   첫 번째 `await`까지 실행됩니다.
   `listPhotos(inGallery:)` 함수를 호출하고
   해당 함수가 반환할 때까지 실행을 일시 중단합니다.

2. 이 코드의 실행이 일시 중단되는 동안
   같은 프로그램의 다른 동시 코드가 실행됩니다.
   예를 들어 오랜 시간 실행되는 백그라운드 태스크가
   새로운 사진의 목록을 업데이트 할 수 있습니다.
   이 코드는 `await`로 표시된 다음 중단 지점까지 실행되거나
   코드의 마지막 부분까지 실행됩니다.

3. `listPhotos(inGallery:)`가 반환한 후에,
   이 코드는 해당 지점에서 계속 실행됩니다.
   반환된 값을 `photoNames`에 할당합니다.

4. `sortedNames`와 `name`을 정의하는 라인은
   일반적인 동기 코드입니다.
   이 라인은 `await`로 표시되지 않았으므로,
   가능한 중단 지점이 없습니다.

5. 다음 `await`는 `downloadPhoto(named:)` 함수에 대한 호출을 표시합니다.
   이 코드는 해당 함수가 반환할 때까지 실행을 다시 일시 중단하여
   다른 동시 코드에 실행할 기회를 제공합니다.

6. `downloadPhoto(named:)`가 반환한 후에
   반환값은 `photo`에 할당되고
   `show(_:)`를 호출할 때 인자로 전달합니다.

`await`로 표시된 코드의 중단이 가능한 지점은
비동기 함수나 메서드가 반환하기를 기다리는 동안
현재 코드 부분의 실행을 일시적으로 중단할 수 있음을 나타냅니다.
Swift가 현재 스레드에서 코드의 실행을 일시 중단하고
대신 해당 스레드에서
다른 코드를 실행하기 때문에
이것을 *스레드 양보(yielding the thread)*라고도 부릅니다.
`await`가 있는 코드는 실행을 일시 중단할 수 있어야 하므로,
프로그램의 특정 위치에서만 비동기 함수나 메서드를 호출할 수 있습니다:

- 비동기 함수, 메서드, 프로퍼티 본문의 코드
  
- `@main`으로 표시된 구조체, 클래스, 열거형의
  정적(static) `main()` 메서드의 코드

- 아래의 [비구조 동시성 (Unstructured Concurrency)](#비구조-동시성-unstructured-concurrency)에 보이는 것처럼
  구조화되지 않은 하위 태스크의 코드

<!--
  SE-0296 specifically calls out that top-level code is *not* an async context,
  contrary to what you might expect.
  If that gets changed, add this bullet to the list above:

  - Code at the top level that forms an implicit main function.
-->

[`Task.sleep(for:tolerance:clock:)`][] 메서드는
동시성 동작이 어떻게 동작하는지 알기 위해
간단한 코드를 작성할 때 유용합니다.
이 메서드는 주어진 시간만큼 현재 태스크를 중단합니다.
다음은 네트워크 동작을 지연시키기 위해 `sleep(for:tolerance:clock:)`을 사용하는
`listPhotos(inGallery:)` 함수 입니다.

[`Task.sleep(for:tolerance:clock:)`]: https://developer.apple.com/documentation/swift/task/sleep(for:tolerance:clock:)

```swift
func listPhotos(inGallery name: String) async throws -> [String] {
    try await Task.sleep(for: .seconds(2))
    return ["IMG001", "IMG99", "IMG0404"]
}
```

<!--
  - test: `sleep-in-toy-code`

  ```swifttest
  >> struct Data {}  // Instead of actually importing Foundation
  -> func listPhotos(inGallery name: String) async throws -> [String] {
         try await Task.sleep(for: .seconds(2))
         return ["IMG001", "IMG99", "IMG0404"]
  }
  ```
-->

위 코드에서 `listPhotos(inGallery:)`은
`Task.sleep(until:tolerance:clock:)` 호출이 오류를 발생할 수 있으므로
비동기와 던지기를 모두 작성하였습니다.
이 `listPhotos(inGallery:)`를 호출하려면,
`try`와 `await` 모두 작성해야 합니다:

```swift
let photos = try await listPhotos(inGallery: "A Rainy Weekend")
```

비동기 함수는 던지는 함수와 유사한 점이 있습니다:
비동기나 던지는 함수를 정의할 때,
`async`나 `throws`를 표시하고,
이 함수를 호출할 때 `await`나 `try`를 작성합니다.
비동기 함수는 던지는 함수가 다른 던지는 함수를 호출할 수 있듯이
다른 비동기 함수를 호출할 수 있습니다.

그러나, 매우 중요한 다른 점이 있습니다.
오류를 처리하기 위해 `do`-`catch` 블록에 코드를 래핑하거나,
오류를 다른 곳에서 처리하기 위해 오류를 저장하는 `Result`를 사용할 수 있습니다.
이러한 접근방식은 던지지 않는 코드에서
던지는 함수를 호출할 수 있습니다.
예를 들어:

```swift
func availableRainyWeekendPhotos() -> Result<[String], Error> {
    return Result {
        try listDownloadedPhotos(inGallery: "A Rainy Weekend")
    }
}
```

반면에,
동기 코드에서 호출하고 결과를 기다리기 위해
비동기 코드를 래핑하는 방법은 없습니다.
Swift 표준 라이브러리는 의도적으로 안전하지 않은 기능을 생략합니다 ---
이를 구현하려고 하면
미묘한 경합, 스레드 이슈, 데드락과 같은 문제가 발생할 수 있습니다.
기존 프로젝트에 비동기 코드를 추가할 때,
상위 계층에서 시작하는 것이 좋습니다.
구체적으로
비동기를 사용할 코드의 최상위 계층부터 변환한 다음에,
호출하는 함수와 메서드를 하나씩 변환하면서
프로젝트의 아키텍처를 계층별로 작업합니다.
동기 코드는 비동기 코드를 호출할 수 있는 방법이 없으므로,
하위 계층(bottom-up)부터 접근하는 방식은 사용할 수 없습니다.

<!--
  OUTLINE

  ## Asynchronous Closures

  like how you can have an async function, a closure con be async
  if a closure contains 'await' that implicitly makes it async
  you can mark it explicitly with "async -> in"

  (discussion of @MainActor closures can probably go here too)
-->

## 비동기 시퀀스 (Asynchronous Sequences)

이전 섹션의 `listPhotos(inGallery:)` 함수는
비동기적으로 배열의 모든 요소가 준비된 후에
전체 배열을 한 번에 반환합니다.
또 다른 접근방식은
*비동기 시퀀스(asynchronous sequence)*를 사용하여
한 번에 컬렉션의 한 요소를 기다리는 것입니다.
비동기 시퀀스에 대한 조회 동작은 다음과 같습니다:

```swift
import Foundation

let handle = FileHandle.standardInput
for try await line in handle.bytes.lines {
    print(line)
}
```

<!--
  - test: `async-sequence`

  ```swifttest
  -> import Foundation

  >> func f() async throws {
  -> let handle = FileHandle.standardInput
  -> for try await line in handle.bytes.lines {
         print(line)
     }
  >> }
  ```
-->

일반적인 `for`-`in` 루프 대신에
위의 예시는 `for` 다음에 `await`를 작성합니다.
비동기 함수나 메서드를 호출할 때와 마찬가지로
`await` 작성은 가능한 중단 지점을 나타냅니다.
`for`-`await`-`in` 루프는
다음 요소를 사용할 수 있을 때까지 기다리고
각 반복이 시작될 때 잠재적으로 실행을 일시 중단합니다.

[`Sequence`][] 프로토콜에 준수성을 추가하여
`for`-`in` 루프에서 자체 타입을 사용할 수 있는 것과 같은 방식으로
[`AsyncSequence`][] 프로토콜에 준수성을 추가하여
`for`-`await`-`in` 루프에서 자체 타입을 사용할 수 있습니다.

[`Sequence`]: https://developer.apple.com/documentation/swift/sequence
[`AsyncSequence`]: https://developer.apple.com/documentation/swift/asyncsequence

<!--
  TODO what happened to ``Series`` which was supposed to be a currency type?
  Is that coming from Combine instead of the stdlib maybe?

  Also... need a real API that produces a async sequence.
  I'd prefer not to go through the whole process of making one here,
  since the protocol reference has enough detail to show you how to do that.
  There's nothing in the stdlib except for the AsyncFooSequence types.
  Maybe one of the other conforming types from an Apple framework --
  how about FileHandle.AsyncBytes (myFilehandle.bytes.lines) from Foundation?

  https://developer.apple.com/documentation/swift/asyncsequence
  https://developer.apple.com/documentation/foundation/filehandle

  if we get a stdlib-provided async sequence type at some point,
  rewrite the above to fit the same narrative flow
  using something like the following

  let names = await listPhotos(inGallery: "Winter Vacation")
  for await photo in Photos(names: names) {
      show(photo)
  }
-->

## 병렬로 비동기 함수 호출 (Calling Asynchronous Functions in Parallel)

`await`를 사용하여 비동기 함수를 호출하면
한 번에 코드의 한 부분만 실행됩니다.
비동기 코드가 실행되는 동안
호출자는 코드의 다음 라인을 실행하기 위해 이동하기 전에
해당 코드가 완료될 때까지 기다립니다.
예를 들어
갤러리에서 처음 세 장의 사진을 가져오려면
다음과 같이
`downloadPhoto(named:)` 함수에 대한 세 번의 호출을 기다릴 수 있습니다:

```swift
let firstPhoto = await downloadPhoto(named: photoNames[0])
let secondPhoto = await downloadPhoto(named: photoNames[1])
let thirdPhoto = await downloadPhoto(named: photoNames[2])

let photos = [firstPhoto, secondPhoto, thirdPhoto]
show(photos)
```

<!--
  - test: `defining-async-function`

  ```swifttest
  >> func show(_ images: [Data]) { }
  >> func ff() async {
  >> let photoNames = ["IMG001", "IMG99", "IMG0404"]
  -> let firstPhoto = await downloadPhoto(named: photoNames[0])
  -> let secondPhoto = await downloadPhoto(named: photoNames[1])
  -> let thirdPhoto = await downloadPhoto(named: photoNames[2])

  -> let photos = [firstPhoto, secondPhoto, thirdPhoto]
  -> show(photos)
  >> }
  ```
-->

이 방식에는 중요한 단점이 있습니다:
다운로드가 비동기이고
다운로드가 진행되는 동안 다른 작업을 수행할 수 있지만
`downloadPhoto(named:)`에 대한 호출은 한 번에 하나만 실행됩니다.
각 사진은 다음 사진이 다운로드를 시작하기 전에 완료됩니다.
그러나 이런 작업을 기다릴 필요가 없습니다 ---
각 사진은 개별적으로 또는 동시에 다운로드 할 수 있습니다.

비동기 함수를 호출하고
주변의 코드를 병렬로 실행하려면
상수를 정의할 때 `let` 앞에 `async`를 작성하고
상수를 사용할 때마다 `await`를 작성합니다.

```swift
async let firstPhoto = downloadPhoto(named: photoNames[0])
async let secondPhoto = downloadPhoto(named: photoNames[1])
async let thirdPhoto = downloadPhoto(named: photoNames[2])

let photos = await [firstPhoto, secondPhoto, thirdPhoto]
show(photos)
```

<!--
  - test: `calling-with-async-let`

  ```swifttest
  >> struct Data {}  // Instead of actually importing Foundation
  >> func show(_ images: [Data]) { }
  >> func downloadPhoto(named name: String) async -> Data { return Data() }
  >> let photoNames = ["IMG001", "IMG99", "IMG0404"]
  >> func f() async {
  -> async let firstPhoto = downloadPhoto(named: photoNames[0])
  -> async let secondPhoto = downloadPhoto(named: photoNames[1])
  -> async let thirdPhoto = downloadPhoto(named: photoNames[2])

  -> let photos = await [firstPhoto, secondPhoto, thirdPhoto]
  -> show(photos)
  >> }
  ```
-->

이 예시에서
세 번의 `downloadPhoto(named:)` 호출은
이전 호출이 완료되길 기다리지 않고 시작됩니다.
사용할 수 있는 시스템 자원이 충분하다면 동시에 실행할 수 있습니다.
코드가 함수의 결과를 기다리기 위해 일시 중단되지 않기 때문에
이러한 함수 호출 중 어느 것도 `await`로 표시하지 않습니다.
대신 `photos`가 정의된 라인까지
실행이 계속됩니다 ---
이 시점에서 프로그램은 이 비동기 호출의 결과를 필요로 하므로
세 장의 사진이 모두 다운로드 될 때까지
실행을 일시 중단하기 위해 `await`를 작성합니다.

다음은 이 두 접근방식의 차이점에 대해 생각할 수 있는 방법입니다:

- 다음 줄의 코드가 해당 함수의 결과에 따라 달라지면
  `await`를 사용하여 비동기 함수를 호출합니다.
  이것은 순차적으로 실행되는 작업을 생성합니다.
- 나중에 코드에서 결과가 필요하지 않을 때
  `async`-`let`을 사용하여 비동기 함수를 호출합니다.
  이렇게 하면 병렬로 수행할 수 있는 작업이 생성됩니다.
- `await`와 `async`-`let`은
  모두 일시 중단되는 동안 다른 코드를 실행할 수 있도록 합니다.
- 두 경우 모두 비동기 함수가 반환될 때까지
  필요한 경우 실행이 일시 중단됨을 나타내기 위해
  가능한 일시 중단 지점을 `await`로 표시합니다.

동일한 코드에서 이 두 가지 접근방식을 혼합할 수도 있습니다.

## Task와 Task Group (Tasks and Task Groups)

*태스크(task)*는 프로그램의 일부로
비동기적으로 실행할 수 있는 작업 단위입니다.
모든 비동기 코드는 어떠한 태스크의 일부로 실행됩니다.
태스크은 한 번에 하나의 태스크만 수행하지만,
여러 태스크를 생성하면,
Swift는 동시에 수행하기 위해 태스크를 스케쥴링 할 수 있습니다.

이전 섹션에서 설명한 `async`-`let` 구문은
암시적으로 하위 태스크를 생성합니다 ---
이 문법은 프로그램에서 무슨 태스크를 수행해야 될지
이미 알고 있을 때 잘 동작합니다.
우선 순위와 취소를 더 잘 제어할 수 있고
동적으로 태스크를 생성할 수 있는
태스크 그룹(task group)
([`TaskGroup`][]의 인스턴스)과
해당 그룹에 하위 태스크를 추가할 수도 있습니다.

[`TaskGroup`]: https://developer.apple.com/documentation/swift/taskgroup

태스크는 계층 구조로 정렬됩니다.
주어진 태스크 그룹의 태스크는 동일한 상위 태스크를 가지고
각 태스크에는 하위 태스크가 있을 수도 있습니다.
태스크와 태스크 그룹 간의 명시적 관계 때문에
이 접근방식을 *구조 동시성(structured concurrency)*이라고 합니다.
명시적 부모(parent)-자식(child) 관계는 태스크 간에 여러 이점이 있습니다:

- 부모 태스크에서,
  하위 태스크가 완료될 때까지 기다릴 수 있습니다.

- 하위 태스크에서 더 높은 우선 순위로 설정되면,
  상위 태스크의 우선 순위는 자동으로 높아집니다.

- 상위 태스크가 취소되면,
  각 하위 태스크는 자동으로 취소됩니다.

- 태스크-로컬 값(Task-local value)은 하위 태스크에 효율적이고 자동으로 전파됩니다.

다음은 여러 사진을 다운로드하는
코드를 나타냅니다:

```swift
await withTaskGroup(of: Data.self) { group in
    let photoNames = await listPhotos(inGallery: "Summer Vacation")
    for name in photoNames {
        group.addTask {
            return await downloadPhoto(named: name)
        }
    }

    for await photo in group {
        show(photo)
    }
}
```

위 코드는 새로운 태스크 그룹을 생성하고,
갤러리에 사진을 다운로드하는
하위 태스크를 생성합니다.
Swift는 조건이 되는만큼 비동기적으로 여러 태스크를 수행합니다.
하위 태스크에서 사진 다운로드가 끝나자마자
해당 사진은 보여집니다.
하위 태스크 완료에 대한 순서를 보장하지 않으므로,
갤러리에 사진은 무작위로 보여질 수 있습니다.

> Note:
> 사진 다운로드 코드에서 오류가 발생할 수 있다면,
> `withThrowingTaskGroup(of:returning:body:)`을 호출해야 합니다.

위 코드에서
각 사진은 다운로드되고 보여지므로,
태스크 그룹은 결과를 반환하지 않습니다.
결과를 반환하는 태스크 그룹의 경우,
`withTaskGroup(of:returning:body:)`에 전달하는 클로저 내에
결과를 누적하는 코드를 추가합니다.

```swift
let photos = await withTaskGroup(of: Data.self) { group in
    let photoNames = await listPhotos(inGallery: "Summer Vacation")
    for name in photoNames {
        group.addTask {
            return await downloadPhoto(named: name)
        }
    }

    var results: [Data] = []
    for await photo in group {
        results.append(photo)
    }

    return results
}
```

이전 예시와 같이,
이 예시는 각 사진을 다운로드하는 하위 태스크를 생성합니다.
이전 예시와 다르게,
`for`-`await`-`in` 루프는 다음 하위 태스크가 완료될 때까지 기다리고,
해당 태스크의 결과를 결과 배열에 추가한 다음에,
모든 하위 태스크가 완료될 때까지 기다립니다.
마지막으로,
태스크 그룹은 다운로드한 사진의 배열을
전체 결과로 반환합니다.

<!--
TODO:
In the future,
we could extend the example above
to show how you can limit the number of concurrent tasks
that get spun up by a task group.
There isn't a specific guideline we can give
in terms of how many concurrent tasks to run --
it's more "profile your code, and then adjust".

See also:
https://developer.apple.com/videos/play/wwdc2023/10170?time=688

We could also show withDiscardingTaskGroup(...)
since that's optimized for child tasks
whose values aren't collected.
-->

### 태스크 취소 (Task Cancellation)

Swift 동시성은 협동 취소 모델(cooperative cancellation model)을 사용합니다.
각 태스크는 실행 중에 적절할 때
취소여부를 확인하고,
적절하게 취소에 응답합니다.
태스크의 종류에 따라
취소에 대한 응답은 다음 중 하나에 해당합니다:

- `CancellationError`와 같은 오류 발생
- `nil`이나 빈 컬렉션 반환
- 부분적으로 완료된 태스크 반환

사진이 크거나 네트워크가 느려서
사진을 다운로드하는데 오랜 시간이 걸릴 수 있습니다.
모든 태스크가 완료되기를 기다리지 않고
태스크를 멈추려면,
태스크가 취소되었는지 확인하고 취소된 경우에 실행을 중지합니다.
태스크가 취소되었는지 확인하는 방법은 두 가지가 있습니다:
[`Task.checkCancellation()`][] 타입 메서드를 호출하거나,
[`Task.isCancelled`][`Task.isCancelled` 타입] 타입 프로퍼티를 읽어서 확인할 수 있습니다.
`checkCancellation()`을 호출하는 것은 태스크가 취소되었으면 오류가 발생합니다;
던지는 태스크는 태스크 외부로 오류를 전파하여
모든 태스크를 중지시킬 수 있습니다.
이것은 구현과 이해가 간단하다는 이점이 있습니다.
더 유연한 방법으로는, 네트워크 연결을 닫고 임시 파일을 지우는 것과 같은
태스크 중지의 부분으로 정리 작업을 수행할 수 있는
`isCancelled` 프로퍼티를 사용합니다.

[`Task.checkCancellation()`]: https://developer.apple.com/documentation/swift/task/3814826-checkcancellation
[`Task.isCancelled` 타입]: https://developer.apple.com/documentation/swift/task/iscancelled-swift.type.property

```swift
let photos = await withTaskGroup { group in
    let photoNames = await listPhotos(inGallery: "Summer Vacation")
    for name in photoNames {
        let added = group.addTaskUnlessCancelled {
            Task.isCancelled ? nil : await downloadPhoto(named: name)
        }
        guard added else { break }
    }

    var results: [Data] = []
    for await photo in group {
        if let photo { results.append(photo) }
    }
    return results
}
```

위의 코드는 이전 버전과 몇 가지 다른 점이 있습니다:

- 취소 후에 새로운 작업이 시작되지 않도록
  각 태스크는
  [`TaskGroup.addTaskUnlessCancelled(priority:operation:)`][] 메서드를 사용해서 추가됩니다.

- `addTaskUnlessCancelled(priority:operation:)`을 호출할 때마다,
  코드는 하위 태스크가 새로 추가되었는지 확인합니다.
  그룹이 취소되면 `added`의 값은 `false`이고 ---
  코드는 추가 사진 다운로드를 중지합니다.

- 각 태스크는 사진을 다운로드 하기 전에
  취소 여부를 검사합니다.
  태스크가 취소 되었으면, `nil`을 반환합니다.

- 결국,
  태스크 그룹은 결과를 수집할 때 `nil` 값이면 건너뜁니다.
  `nil`을 반환해서 취소를 처리한다는 것은
  태스크 그룹은 부분적인 결과를 반환할 수 있다는 의미입니다 ---
  취소했을 때 이미 다운로드된 사진은 사진을 파기하는 대신에
  다운로드된 사진을 반환합니다.

[`TaskGroup.addTaskUnlessCancelled(priority:operation:)`]: https://developer.apple.com/documentation/swift/taskgroup/addtaskunlesscancelled(priority:operation:)

> Note:
> 태스크가 외부에서 취소되었는지 확인하려면,
> 타입 프로퍼티 대신에
> [`Task.isCancelled`][`Task.isCancelled` 인스턴스] 인스턴스 프로퍼티를 사용합니다.

[`Task.isCancelled` 인스턴스]: https://developer.apple.com/documentation/swift/task/iscancelled-swift.property

즉시 취소에 대한 알림이 필요한 경우에
[`Task.withTaskCancellationHandler(operation:onCancel:isolation:)`][] 메서드를 사용합니다.
예를 들어:

[`Task.withTaskCancellationHandler(operation:onCancel:isolation:)`]: https://developer.apple.com/documentation/swift/withtaskcancellationhandler(operation:oncancel:isolation:)

```swift
let task = await Task.withTaskCancellationHandler {
    // ...
} onCancel: {
    print("Canceled!")
}

// ... some time later...
task.cancel()  // Prints "Canceled!"
```

취소 처리를 사용할 때,
태스크 취소는 여전히 협조적입니다:
태스크가 완료될 때까지 수행하거나
취소를 확인하고 조기 중지합니다.
취소 처리가 시작될 때 태스크는 여전히 수행 중이므로,
경쟁 조건(race condition)이 생성될 수 있는
태스크와 취소 처리간의 상태 공유를 피해야 합니다.

<!--
  OUTLINE

  - cancellation propagates (Konrad's example below)

  ::

      let handle = Task.detached {
      await withTaskGroup(of: Bool.self) { group in
          var done = false
          while done {
          await group.addTask { Task.isCancelled } // is this child task canceled?
          done = try await group.next() ?? false
          }
      print("done!") // <1>
      }

      handle.cancel()
      // done!           <1>
-->

<!--
  Not for WWDC, but keep for future:

  task have deadlines, not timeouts --- like "now + 20 ms" ---
  a deadline is usually what you want anyhow when you think of a timeout

  - this chapter introduces the core ways you use tasks;
  for the full list what you can do,
  including the unsafe escape hatches
  and ``Task.current()`` for advanced use cases,
  see the Task API reference [link to stdlib]

  - task cancellation isn't part of the state diagram below;
  it's an independent property that can happen in any state

  [PLACEHOLDER ART]

  Task state diagram

     |
     v
  Suspended <-+
     |        |
     v        |
  Running ----+
     |
     v
  Completed

  [PLACEHOLDER ART]

  Task state diagram, including "substates"

     |
     v
  Suspended <-----+
  (Waiting) <---+ |
     |          | |
     v          | |
  Suspended     | |
  (Schedulable) / |
     |            |
     v            |
  Running --------+
     |
     v
  Completed

  .. _Concurrency_ChildTasks:

  Adding Child Tasks to a Task Group

  - awaiting ``withGroup`` means waiting for all child tasks to complete

  - a child task can't outlive its parent,
  like how ``async``-``let`` can't outlive the (implicit) parent
  which is the function scope

  - awaiting ``addTask(priority:operation:)``
  means waiting for that child task to be added,
  not waiting for that child task to finish

  - ?? maybe cover ``TaskGroup.next``
  probably nicer to use the ``for await result in someGroup`` syntax

  quote from the SE proposal --- I want to include this fact here too

  > There's no way for reference to the child task to
  > escape the scope in which the child task is created.
  > This ensures that the structure of structured concurrency is maintained.
  > It makes it easier to reason about
  > the concurrent tasks that are executing within a given scope,
  > and also enables various optimizations.
-->

<!--
  OUTLINE

  .. _Concurrency_TaskPriority:

  Setting Task Priority

  - priority values defined by ``Task.Priority`` enum

  - type property ``Task.currentPriority``

  - The exact result of setting a task's priority depends on the executor

  - TR: What's the built-in stdlib executor do?

  - Child tasks inherit the priority of their parents

  - If a high-priority task is waiting for a low-priority one,
  the low-priority one gets scheduled at high priority
  (this is known as :newTerm:`priority escalation`)

  - In addition, or instead of, setting a low priority,
  you can use ``Task.yield()`` to explicitly pass execution to the next scheduled task.
  This is a sort of cooperative multitasking for long-running work.

  You can explicitly insert a suspension point
  by calling the [`Task.yield()`][] method.

  [`Task.yield()`]: https://developer.apple.com/documentation/swift/task/3814840-yield
  
  ```swift
  func generateSlideshow(forGallery gallery: String) async {
      let photos = await listPhotos(inGallery: gallery)
      for photo in photos {
          // ... render a few seconds of video for this photo ...
          await Task.yield()
      }
  }
  ```

  Assuming the code that renders video is synchronous,
  it doesn't contain any suspension points.
  The work to render video could also take a long time.
  However,
  you can periodically call `Task.yield()`
  to explicitly add suspension points.
  Structuring long-running code this way
  lets Swift balance between making progress on this task,
  and letting other tasks in your program make progress on their work.
-->

### 비구조 동시성 (Unstructured Concurrency)

이전 섹션에서 설명한
동시성에 대한 구조화된 접근방식 외에도
Swift는 비구조 동시성(unstructured concurrency)을 지원합니다.
태스크 그룹의 일부인 태스크와 달리
*비구조 태스크(unstructured task)*에는 상위 태스크가 없습니다.
프로그램이 필요로 하는 방식으로
비구조 태스크를 관리할 수 있는 유연성이 있지만
올바른 동작을 위한 책임도 있습니다.

비구조 태스크를 생성하여
주변 코드와 유사하게 실행하려면,
[`Task.init(name:priority:operation:)`][] 이니셜라이저를 호출해야 합니다.
새로운 태스크는 기본적으로 현재 태스크와
동일한 액터 격리(actor isolation), 우선순위(priority), 태스크-로컬 상태(task-local state)를 가지고 실행됩니다.
주변 코드로부터 더 독립적인
비구조 태스크,
특히 *분리 태스크(detached task)*라고 알려진 것을 생성하려면
[`Task.detached(name:priority:operation:)`][] 정적 메서드를 호출해야 합니다.
새로운 태스크는 기본적으로 어떠한 액터 격리없이 실행되고
현재 태스크의 우선순위나 태스크-로컬 상태를 상속받지 않습니다.
이 두 작업 모두 예를 들어 결과를 기다리거나 취소하는 것과 같이
상호작용이 가능한 태스크를 반환합니다.
<!-- TODO: In SE-0461 terms, Task.detached runs as an @concurrent function. -->

```swift
let newPhoto = // ... some photo data ...
let handle = Task {
    return await add(newPhoto, toGalleryNamed: "Spring Adventures")
}
let result = await handle.value
```

분리 태스크(detached tasks) 관리에 대한 자세한 내용은
[`Task`](https://developer.apple.com/documentation/swift/task)를 참고바랍니다.

[`Task.init(name:priority:operation:)`]: https://developer.apple.com/documentation/swift/task/init(name:priority:operation:)-43wmk
[`Task.detached(name:priority:operation:)`]: https://developer.apple.com/documentation/swift/task/detached(name:priority:operation:)-795w1

<!--
  TODO Add some conceptual guidance about
  when to make a method do its work in a detached task
  versus making the method itself async?
  (Pull from my 2021-04-21 notes from Ben's talk rehearsal.)
-->

## 격리 (Isolation)

이전 섹션에서는 동시성 작업을 분할하는 접근방식에 대해 논의했습니다.
이러한 작업은 앱의 UI와 같은 공유 데이터를 변경하는 것이 포함될 수 있습니다.
코드의 다른 부분이 동시에 동일한 데이터를 수정할 수 있다면,
이것은 데이터 경쟁(data race)를 초래할 위험이 있습니다.
Swift는 코드를 데이터 경쟁으로부터 보호합니다:
데이터를 읽거나 수정할 때마다,
Swift는 다른 코드가 동시에 해당 데이터를 수정하지 않도록 보장합니다.
이런 보장을 *데이터 격리(data isolation)*라고 합니다.
데이터를 격리하는 세 가지 주요 방법이 있습니다:

1. 불변 데이터(immutable data)는 항상 격리됩니다.
   상수는 수정할 수 없으므로,
   상수를 읽을 때
   다른 코드에서 상수를 수정할 위험이 없습니다.

2. 현재 태스크만 참조하는 데이터는 항상 격리됩니다.
   지역 변수는 안전하게 읽고 쓸 수 있으며,
   이것은 태스크 외부의 코드가 해당 메모리에 대한 참조를 가지고 있지 않아
   다른 코드가 이 데이터를 수정할 수 없기 때문입니다.
   또한
   클로저에서 변수를 캡처하면,
   Swift는 해당 클로저가 동시에 사용되지 않도록 보장합니다.

3. 액터에 의해 보호되는 데이터는
   해당 데이터에 접근하는 코드가 액터에 격리된 경우 격리됩니다.
   현재 함수가 액터에 격리되어 있다면,
   해당 액터에 의해 보호되는 데이터를 안전하게 읽고 쓸 수 있으며,
   이것은 동일한 액터에 격리된 다른 모든 코드가
   실행되기 전에 자신의 차례를 기다려야 하기 때문입니다.

## 메인 액터 (The Main Actor)

액터는 코드가 해당 데이터에 번갈아 접근하도록 강제함으로써
가변 데이터에 대한 접근을 보호하는 객체입니다.
많은 프로그램에서 가장 중요한 액터는 *메인 액터(main actor)*입니다.
앱에서
메인 액터는 UI를 표시하는데 사용되는 모든 데이터를 보호합니다.
메인 액터는 UI 렌더링,
UI 이벤트 처리,
UI를 조회하거나 업데이트해야 하는 코드 실행을 번갈아 수행합니다.

코드에서 동시성을 사용하기 전에는
모든 것이 메인 액터에서 실행됩니다.
긴 실행이나 리소스 집약적인 코드를 식별하면
안전하고 올바른 방식으로
이 작업을 메인 액터에서 분리할 수 있습니다.

> Note:
> 메인 액터는 메인 스레드와 밀접한 관련이 있지만
> 동일한 것은 아닙니다.
> 메인 액터는 비공개 가변 상태를 가지고,
> 메인 스레드는 해당 상태에 대한 접근을 직렬화합니다.
> 메인 액터에서 코드를 실행하면,
> Swift는 해당 코드를 메인 스레드에서 실행합니다.
> 이러한 연결 때문에
> 두 용어가 상호 교환적으로 사용되는 것을 볼 수 있습니다.
> 코드는 메인 액터와 상호작용하며;
> 메인 스레드는 더 낮은 수준의 구현 세부사항입니다.

<!--
TODO: Discuss the SE-0478 syntax for 'using @MainActor'

When you're writing UI code,
you often want all of it to be isolated to the main actor.
To do this, you can write `using @MainActor`
at the top of a Swift file to apply that attribute by default
to all the code in the file.
If there's a specific function or property
that you want to exclude from `using @MainActor`,
you can use the `nonisolated` modifier on that declaration
to override the default.
Modules can be configured to be built using `using @MainActor` by default.
This can be overridden on a per-file basis
by writing `using nonisolated` at the top of a file.
-->

메인 액터에서 작업을 실행하는 몇 가지 방법이 있습니다.
함수가 항상 메인 액터에서 실행되도록 하려면,
`@MainActor` 속성으로 표시해야 합니다:

```swift
@MainActor
func show(_: Data) {
    // ... UI code to display the photo ...
}
```

위 코드에서
`show(_:)` 함수의 `@MainActor` 속성은
이 함수가 메인 액터에서만 실행되도록 요구합니다.
메인 액터에서 실행 중인 다른 코드 내에서
`show(_:)`를 동기 함수처럼 호출할 수 있습니다.
그러나
메인 액터에서 실행되지 않는 코드에서 `show(_:)`를 호출하려면
`await`를 포함하고 비동기 함수로 호출해야 합니다.
이것은 메인 액터로 전환하는 것이 잠재 중단 지점(potential suspension point)을 도입하기 때문입니다.
예를 들어:

```swift
func downloadAndShowPhoto(named name: String) async {
    let photo = await downloadPhoto(named: name)
    await show(photo)
}
```

위 코드에서
`downloadPhoto(named:)`와 `show(_:)` 함수는 모두
호출 시 중단될 수 있습니다.
이 코드는 또한 일반적인 패턴을 보여줍니다:
백그라운드에서 긴 실행과 CPU 집약적 작업을 수행한 다음에
UI를 업데이트하기 위해 메인 액터로 전환하는 것입니다.
`downloadAndShowPhoto(named:)` 함수는 메인 액터에 있지 않으므로,
`downloadPhoto(named:)`의 작업도 메인 액터에서 실행되지 않습니다.
`show(_:)` 함수는 `@MainActor` 속성으로 표시되어 있으므로,
UI 업데이트하는 `show(_:)`의 작업만 메인 액터에서 실행됩니다.
<!-- TODO
When updating for SE-0461,
this is a good place to note
that downloadPhoto(named:) runs
on whatever actor you were on when you called it.
-->

클로저가 메인 액터에서 실행되도록 하려면,
클로저 시작 부분에서
캡처 목록과 `in` 앞에 `@MainActor`를 작성합니다.

```swift
let photo = await downloadPhoto(named: "Trees at Sunrise")
Task { @MainActor in
    show(photo)
}
```

위 코드는
이전 코드 목록의 `downloadAndShowPhoto(named:)`와 유사하지만
이 예시의 코드는 UI 업데이트를 기다리지 않습니다.
또한 구조체, 클래스, 열거형에 `@MainActor`를 작성하여
모든 메서드와 프로퍼티에 대한 모든 접근이
메인 액터에서 실행되도록 할 수 있습니다:

```swift
@MainActor
struct PhotoGallery {
    var photoNames: [String]
    func drawUI() { /* ... other UI code ... */ }
}
```

위 코드의 `PhotoGallery` 구조체는
`photoNames` 프로퍼티의 이름을 사용하여
어떤 사진을 표시할지 결정하고
화면에 사진을 그립니다.
`photoNames`가 UI에 영향을 미치므로,
이것을 변경하는 코드는 접근을 직렬화하기 위해
메인 액터에서 실행되어야 합니다.

프레임워크를 기반으로 구축하는 경우,
해당 프레임워크의 프로토콜과 기본 클래스는
일반적으로 이미 `@MainActor`로 표시되어 있으므로
이 경우에 자신의 타입에 `@MainActor`를 작성할 필요는 없습니다.
다음은 간단한 예시입니다:

```swift
@MainActor
protocol View { /* ... */ }

// Implicitly @MainActor
struct PhotoGalleryView: View { /* ... */ }
```

위 코드에서
SwiftUI와 같은 프레임워크는 `View` 프로토콜을 정의합니다.
프로토콜 선언에 `@MainActor`를 작성하여
이 프로토콜을 준수하는 `PhotoGalleryView`와 같은 타입도
암시적으로 `@MainActor`로 표시됩니다.
`View`가 기본 클래스이고
`PhotoGalleryView`가 하위 클래스인 경우에도 동일한 동작을 볼 수 있습니다 ---
하위 클래스는 암시적으로 `@MainActor`로 표시됩니다.

위 예시에서
`PhotoGallery`는 전체 구조체를 메인 액터에서 보호합니다.
더 세밀한 제어를 위해
메인 스레드에서 접근하거나 실행되어야 하는
프로퍼티나 메서드에만 `@MainActor`를 작성할 수 있습니다:

```swift
struct PhotoGallery {
    @MainActor var photoNames: [String]
    var hasCachedPhotos = false

    @MainActor func drawUI() { /* ... UI code ... */ }
    func cachePhotos() { /* ... networking code ... */ }
}
```

위 `PhotoGallery` 버전에서
`drawUI()` 메서드는 화면에 갤러이의 사진을 그리므로
메인 액터에 격리되어야 합니다.
`photoNames` 프로퍼티는 UI를 직접 생성하지는 않지만
`drawUI()` 함수가 UI를 그리는데 사용하는 상태를 저장하므로
이 프로퍼티 또한 메인 액터에서만 접근되어야 합니다.
반면에
`hasCachedPhotos` 프로퍼티에 대한 변경은
UI와 상호작용하지 않고
`cachePhotos()` 메서드는 메인 액터에서 실행할 필요가 있는
상태에 접근하지 않습니다.
따라서 이것은 `@MainActor`로 표시되지 않습니다.

이전 예시와 마찬가지로
프레임워크를 사용하여 UI를 구축하는 경우에
해당 프레임워크의 프로퍼티 래퍼는
아마도 UI 상태 프로퍼티를 `@MainActor`로 표시할 것입니다.
프로퍼티 래퍼를 정의할 때
`wrappedValue` 프로퍼티가 `@MainActor`로 표시되면,
해당 프로퍼티 래퍼를 적용하는 모든 프로퍼티도
암시적으로 `@MainActor`로 표시됩니다.

## 액터 (Actors)

Swift는 메인 액터를 제공합니다 ---
또한 자신만의 액터를 정의할 수도 있습니다.
액터(Actors)는 동시성 코드 간에 정보를 안전하게 공유할 수 있게 해줍니다.

클래스와 마찬가지로 액터(actors)는 참조 타입이므로
[클래스는 참조 타입 (Classes Are Reference Types)](./structures-and-classes.md#클래스는-참조-타입-classes-are-reference-types)에서
값 타입과 참조 타입의 비교는
클래스 뿐만 아니라 액터에도 적용됩니다.
클래스와 다르게
액터는 한 번에 하나의 태스크만 변경 가능한 상태에 접근할 수 있도록 허용하므로
여러 태스크의 코드가
액터의 동일한 인스턴스와 상호작용 하는 것은 안전합니다.
예를 들어 다음은 온도를 기록하는 액터 입니다:

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
  - test: `actors, actors-implicitly-sendable`

  ```swifttest
  -> actor TemperatureLogger {
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
-->

`actor` 키워드를 사용하여 액터를 도입하고
중괄호로 정의합니다.
`TemperatureLogger` 액터는
액터 외부의 다른 코드가 접근할 수 있는 프로퍼티가 있으며
액터 내부의 코드만 최대값을 업데이트 할 수 있게
`max` 프로퍼티를 제한합니다.

구조체와 클래스와 같은 이니셜라이저로
액터의 인스턴스를 생성합니다.
액터의 프로퍼티나 메서드에 접근할 때
일시 중단 지점을 나타내기 위해 `await`를 사용합니다.
예를 들어:

```swift
let logger = TemperatureLogger(label: "Outdoors", measurement: 25)
print(await logger.max)
// Prints "25"
```

이 예시에서
`logger.max`에 접근하는 것은 일시 중단이 발생할 수 있는 지점입니다.
액터는 한 번에 하나의 태스크만 변경 가능한 상태에 접근할 수 있도록 허용하므로
다른 태스크의 코드가 이미 로거와 상호작용하고 있는 경우
이 코드는 프로퍼티 접근을 기다리는 동안 일시 중단됩니다.

대조적으로
액터의 코드는 액터의 프로퍼티에 접근할 때
`await`를 작성하지 않습니다.
예를 들어
새로운 온도로 `TemperatureLogger`를 업데이트 하는 메서드입니다:

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

`update(with:)` 메서드는 액터에서 이미 실행 중이므로
`max`와 같은 프로퍼티에 대한 접근을 `await`로 표시하지 않습니다.
이 메서드는 액터가 변경 가능한 상태와 상호작용하기 위해 한 번에 하나의 태스크만 허용하는
이유 중 하나를 보여줍니다:
액터의 상태에 대한 일부 업데이트는 일시적으로 불변성을 깨뜨립니다.
`TemperatureLogger` 액터는 온도 목록과 최대 온도를 추적하고
새로운 측정값을 기록할 때
최대 온도를 업데이트합니다.
업데이트 도중에
새로운 측정값을 추가한 후 `max`를 업데이트하지 않았다면
온도 로거는 일시적으로 일관성 없는 상태가 됩니다.
다른 태스크가 동일한 인스턴스에 접근하지 못하도록 방지하면
다음 문제를 예방할 수 있습니다:

1. 코드는 `update(with:)` 메서드를 호출합니다.
   먼저 `measurements` 배열을 업데이트 합니다.
2. 코드에서 `max`를 업데이트 하기 전에
   다른 코드에서 최대값과 온도 배열을 읽습니다.
3. 코드는 `max`를 변경하여 업데이트를 완료합니다.

이러한 경우
다른 곳에서 실행 중인 코드는
데이터가 일시적으로 잘못된 상태에 있었기 때문에
`update(with:)` 호출 중간에
액터에 대한 접근이 잘못된 정보를 읽습니다.
Swift 액터는 한 번에 하나의 태스크만 해당 상태에 접근을 허용하고
해당 코드는
`await`로 표시된 일시 중단 지점 위치에서만 중단될 수 있기 때문에
이 문제를 방지할 수 있습니다.
`update(with:)`는 일시 중단 지점을 포함하지 않으므로
다른 코드는 업데이트 중간에 데이터에 접근할 수 없습니다.

액터 외부의 코드에서 구조체나 클래스의 프로퍼티에 접근하는 것과 같이
프로퍼티에 직접적으로 접근하면
컴파일 때 오류가 발생합니다.
예를 들어:

```swift
print(logger.max)  // Error
```

`await` 작성 없이 `logger.max`에 접근하는 것은
액터의 프로퍼티가 해당 액터의 격리된 로컬 상태이기 때문에 실패합니다.
이 프로퍼티에 접근하는 코드는 액터의 부분으로 수행되어야 하고,
비동기 작업인 `await`를 작성해야 합니다.
Swift는
액터에서 수행하는 코드만 액터의 로컬 상태에 접근할 수 있도록 보장합니다.
이 보장을 *액터 격리(actor isolation)*라고 합니다.

Swift 동시성 모델의 다음의 요소들은
공유된 변경 가능한 상태를 보다 쉽게 이해하고 안전하게 다룰 수 있도록 함께 작동합니다:

- 일시 중단 가능 지점 사이의 코드는
  다른 동시 코드의 중단없이 순차적으로 수행됩니다.
  그러나
  여러 개의 동시성 코드는 동시에 실행될 수 있으므로,
  다른 코드가 동시에 실행되고 있을 수 있습니다.

- 액터의 로컬 상태와 상호작용하는 코드는
  해당 액터에서만 수행됩니다.

- 액터는 한 번에 하나의 코드만 수행합니다.

이러한 보장 때문에,
`await`가 없는 액터 내부에 있는 코드는
프로그램의 다른 위치에서 일시적으로 유효하지 않은 상태를 관찰하지 않고
업데이트를 수행할 수 있습니다.
예를 들어,
아래의 코드는 측정된 온도를 화씨에서 섭씨로 변환합니다:

```swift
extension TemperatureLogger {
    func convertFahrenheitToCelsius() {
        for i in measurements.indices {
            measurements[i] = (measurements[i] - 32) * 5 / 9
        }
    }
}
```

위 코드는 측정된 온도를 한 번에 하나씩 변환합니다.
map 작업이 진행되는 동안,
일부 온도는 화씨로 다른 온도는 섭씨로 표시됩니다.
그러나 이 코드는 `await`를 포함하지 않으므로,
메서드에서 일시 중단 지점이 없습니다.
이 메서드가 수정하는 상태는 액터에 속하며,
해당 코드가 액터에서 수행될 때를 제외하고는
코드를 읽거나 수정하지 못하도록 합니다.
이것은 온도 변환이 진행되는 동안
다른 코드가 부분적으로 변환된 온도를
읽을 수 없다는 것을 의미합니다.

임시 중단 지점을 생략해서 임시적으로 유효하지 않은 상태를 보호하는
액터의 코드를 작성하는 것 외에도
해당 코드를 동기 메서드로 이동할 수 있습니다.
위에 `convertFahrenheitToCelsius()` 메서드는 동기 메서드이므로,
*절대* 임시 중단 지점이 포함되지 않음을 보장합니다.
이 함수는 데이터 모델을 일시적으로 불일치하게 만드는
코드를 캡슐화하고,
작업이 완료되어 데이터 일관성을 복원하기 전까지
다른 코드를 수행할 수 없음을
읽기 쉽게 만들어 줍니다.
이 기간동안
Swift가 이 코드에서 프로그램의
다른 부분의 코드를 실행하도록 전환하지 않는 것이 중요합니다.
향후
이 함수에 비동기 코드를 추가하여,
임시 중단 지점을 도입하면
버그가 발생하는 대신에 컴파일 오류가 발생합니다.


## 전역 액터 (Global Actors)

메인 액터는 [`MainActor`][] 타입의 전역 싱글턴 인스턴스입니다.
액터는 일반적으로 여러 인스턴스를 가질 수 있으며,
각 인스턴스는 독립적인 격리를 제공합니다.
이것이 액터의 모든 격리된 데이터를
해당 액터의 인스턴스 프로퍼티로 선언하는 이유입니다.
그러나 `MainActor`는 싱글턴이므로 ---
이 타입의 단일 인스턴스만 존재 ---
타입만으로 액터를 식별하기에 충분하기 때문에
속성으로만 메인 액터 격리를 표시할 수 있습니다.
이 접근방식은 가장 적합한 방식으로 코드를 구성할 수 있는
더 많은 유연성을 제공합니다.

[`MainActor`]: https://developer.apple.com/documentation/swift/mainactor

`@globalActor` 속성을 사용하여
자신만의 싱글턴 전역 액터를 정의할 수 있으며,
이것은 [globalActor](../language-reference/attributes.md#globalactor)에서 설명합니다.


<!--
  OUTLINE

  Add this post-WWDC when we have a more solid story to tell around Sendable

   .. _Concurrency_ActorIsolation:

   Actor Isolation

   TODO outline impact from SE-0313 Control Over Actor Isolation
   about the 'isolated' and 'nonisolated' keywords

   - actors protect their mutable state using :newTerm:`actor isolation`
   to prevent data races
   (one actor reading data that's in an inconsistent state
   while another actor is updating/writing to that data)

   - within an actor's implementation,
   you can read and write to properties of ``self`` synchronously,
   likewise for calling methods of ``self`` or ``super``

   - method calls from outside the actor are always async,
   as is reading the value of an actor's property

   - you can't write to a property directly from outside the actor

   - The same actor method can be called multiple times, overlapping itself.
   This is sometimes referred to as *reentrant code*.
   The behavior is defined and safe... but might have unexpected results.
   However, the actor model doesn't require or guarantee
   that these overlapping calls behave correctly (that they're *idempotent*).
   Encapsulate state changes in a synchronous function
   or write them so they don't contain an ``await`` in the middle.

   - If a closure is ``@Sendable`` or ``@escaping``
   then it behaves like code outside of the actor
   because it could execute concurrently with other code that's part of the actor

   exercise the log actor, using its client API to mutate state

   ::

       let logger = TemperatureSensor(lines: [
           "Outdoor air temperature",
           "25 C",
           "24 C",
       ])
       print(await logger.getMax())

       await logger.update(with: "27 C")
       print(await logger.getMax())
-->

## Sendable 타입 (Sendable Types)

태스크(Task)와 액터(actor)는 프로그램을
동시에 안전하게 실행할 수 있는 조각으로 나눌 수 있습니다.
태스크나 액터의 인스턴스 내에서
변수와 프로퍼티와 같은
변경 가능한 상태를 포함하는 프로그램의 일부분을
*동시성 도메인(concurrency domain)*이라고 부릅니다.
어떤 데이터는 데이터가 변경 가능한 상태를 포함하지만
동시 접근에 대해 보호되지 않으므로
동시성 도메인 간에 공유될 수 없습니다.

한 동시성 도메인에서 다른 동시성 도메인으로 공유될 수 있는 타입을
*Sendable* 타입(*sendable* type)이라고 합니다.
예를 들어 액터 메서드를 호출할 때 인자로 전달하거나
태스크의 결과로 반환될 수 있습니다.
이 챕터의 앞부분에 있는 예시들은
동시성 도메인 간에 전달되는 데이터는
항상 안전한 간단한 값 타입을 사용하기 때문에
Sendable 타입에 대해 논의하지 않았습니다.
반대로
일부 타입은 동시성 도메인 간에 전달이 안전하지 않습니다.
예를 들어 변경 가능한 프로퍼티를 포함하고
해당 프로퍼티에 순차적으로 접근하지 않는 클래스는
서로 다른 태스크 간에 클래스의 인스턴스를 전달할 때
예상할 수 없고 잘못된 결과를 생성할 수 있습니다.

`Sendable` 프로토콜을 채택하여
Sendable 타입으로 표시합니다.
이 프로토콜은 어떠한 코드 요구사항을 가지지 않지만
Swift가 적용하는 의미론적 요구사항이 있습니다.
일반적으로 타입을 Sendable한 것으로 나타내기 위해 세 가지 방법이 있습니다:

- 타입은 값 타입이고
  변경 가능한 상태가 다른 Sendable 데이터로 구성되어 있는 경우입니다 ---
  예를 들어 Sendable 저장 프로퍼티가 있는 구조체나
  Sendable 연관값이 있는 열거형이 있습니다.
- 타입은 변경 가능한 상태가 없으며
  불변 상태는 다른 Sendable 데이터로 구성되어 있는 경우입니다 ---
  예를 들어 읽기 전용 프로퍼티만 있는 구조체나 클래스가 있습니다.
- 타입이 `@MainActor`로 표시된 클래스나
  특정 스레드나 큐에서
  프로퍼티에 순차적으로 접근하는 클래스와 같이
  변경 가능한 상태의 안정성을 보장하는 코드를 가지고 있는 경우입니다.

<!--
  There's no example of the third case,
  where you serialize access to the class's members,
  because the stdlib doesn't include the locking primitives you'd need.
  Implementing it in terms of NSLock or some Darwin API
  isn't a good fit for TSPL.
  Implementing it in terms of isKnownUniquelyReferenced(_:)
  and copy-on-write is also probably too involved for TSPL.
-->

의미론적 요구사항의 자세한 목록은
[Sendable](https://developer.apple.com/documentation/swift/sendable) 프로토콜을 참고바랍니다.

Sendable 프로퍼티만 가지는 구조체와
Sendable 연관값만 가지는 열거형과 같이
어떠한 타입은 항상 Sendable합니다.
예를 들어:

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
  - test: `actors`

  ```swifttest
  -> struct TemperatureReading: Sendable {
         var measurement: Int
     }

  -> extension TemperatureLogger {
         func addReading(from reading: TemperatureReading) {
             measurements.append(reading.measurement)
         }
     }

  -> let logger = TemperatureLogger(label: "Tea kettle", measurement: 85)
  -> let reading = TemperatureReading(measurement: 45)
  -> await logger.addReading(from: reading)
  ```
-->

`TemperatureReading`은 Sendable 프로퍼티만 가지는 구조체이며
`public`이나 `@usableFromInline`으로 표시되지 않은 구조체이므로
암시적으로 Sendable합니다.
다음은
`Sendable` 프로토콜 준수가 암시되는 구조체입니다:

```swift
struct TemperatureReading {
    var measurement: Int
}
```

<!--
  - test: `actors-implicitly-sendable`

  ```swifttest
  -> struct TemperatureReading {
         var measurement: Int
     }
  ```
-->

명시적으로 타입을 Sendable하지 않는 것으로 나타내려면
`Sendable`에 대한 사용 불가능한 준수를 작성해야 합니다:

```swift
struct FileDescriptor {
    let rawValue: Int
}

@available(*, unavailable)
extension FileDescriptor: Sendable {}
```

<!--
The example above is based on a Swift System API.
https://github.com/apple/swift-system/blob/main/Sources/System/FileDescriptor.swift

See also this PR that adds Sendable conformance to FileDescriptor:
https://github.com/apple/swift-system/pull/112
-->

[프로토콜 암시적 준수 (Implicit Conformance to a Protocol)](./protocols.md#프로토콜-암시적-준수-implicit-conformance-to-a-protocol)에서 논의한대로
프로토콜에 대한 암시적 준수를 억제하는데
사용 불가능한 준수를 사용할 수도 있습니다.

<!--
  LEFTOVER OUTLINE BITS

  - like classes, actors can inherit from other actors

  - actors can also inherit from ``NSObject``,
  which lets you mark them ``@objc`` and do interop stuff with them

  - every actor implicitly conforms to the ``Actor`` protocol,
  which has no requirements

  - you can use the ``Actor`` protocol to write code that's generic across actors

  - In the future, when we get distributed actors,
    the TemperatureSensor example
    might be a good example to expand when explaining them.

  ::

      while let result = try await group.next() { }
      for try await result in group { }

  how much should you have to understand threads to understand this?
  Ideally you don't have to know anything about them.

  How do you meld async-await-Task-Actor with an event driven model?
  Can you feed your user events through an async sequence or Combine
  and then use for-await-in to spin an event loop?
  I think so --- but how do you get the events *into* the async sequence?

  Probably don't cover unsafe continuations (SE-0300) in TSPL,
  but maybe link to them?
-->

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->