{% hint style="info" %}
📢 **공지**  
이 문서는 더 이상 업데이트되지 않습니다.  
아래 새로운 URL로 업데이트 되었으니, 새로운 URL로 접속해 주세요.  
[Swift Book Korean](https://bbiguduk.github.io/swift-book-korean/documentation/tsplk/)
{% endhint %}

# 타입 캐스팅 (Type Casting)

값의 런타임 타입을 확인하고, 더 구체적인 타입의 정보를 제공합니다.

*타입 캐스팅(Type casting)*은 인스턴스의 타입을 확인하거나
해당 인스턴스를 자신의 클래스 계층 구조의
다른 상위 클래스나 하위 클래스로 취급하는 방법입니다.

Swift에서 타입 캐스팅은 `is`와 `as` 연산자로 구현합니다.
이 두 연산자는 값의 타입을 확인하거나 값을 다른 타입으로 변환하는
간단하고 효과적인 방법을 제공합니다.

[프로토콜 준수 여부 검사 (Checking for Protocol Conformance)](./protocols.md#프로토콜-준수-여부-검사-checking-for-protocol-conformance)에서 설명한대로
타입 캐스팅을 사용하여 타입이 프로토콜을 준수하는지 확인할 수도 있습니다.

## 타입 캐스팅을 위한 클래스 계층 정의 (Defining a Class Hierarchy for Type Casting)

클래스와 하위 클래스의 계층도와 함께 타입 캐스팅을 사용하여
특정 클래스 인스턴스의 타입을 확인하고
같은 계층도 내에서 다른 클래스로 인스턴스를 변환할 수 있습니다.
아래의 세 코드는
타입 캐스팅의 예시에서 사용하기 위해
클래스의 계층도와 해당 클래스의 인스턴스를 포함하는 배열을 정의합니다.

첫 번째 코드는 `MediaItem`이라는 새로운 기본 클래스를 정의합니다.
이 클래스는 디지털 미디어 라이브러리에
나타나는 모든 종류의 항목에 대한 기본 기능을 제공합니다.
특히 `String` 타입의 `name` 프로퍼티와
`init(name:)` 이니셜라이저를 선언합니다.
(영화와 노래를 포함하여 모든 미디어 항목은 이름을 가지고 있다고 가정합니다.)

```swift
class MediaItem {
    var name: String
    init(name: String) {
        self.name = name
    }
}
```

<!--
  - test: `typeCasting, typeCasting-err`

  ```swifttest
  -> class MediaItem {
        var name: String
        init(name: String) {
           self.name = name
        }
     }
  ```
-->

다음 코드는 `MediaItem`의 두 하위 클래스를 정의합니다.
첫 번째 햐위 클래스 `Movie`는 영화나 필름에 대한 추가 정보를 캡슐화합니다.
기본 `MediaItem` 클래스의 상위에 `director` 프로퍼티와
해당 이니셜라이저를 추가합니다.
두 번째 하위 클래스 `Song`은 기본 클래스의 상위에
`artist` 프로퍼티와 이니셜라이저를 추가합니다:

```swift
class Movie: MediaItem {
    var director: String
    init(name: String, director: String) {
        self.director = director
        super.init(name: name)
    }
}

class Song: MediaItem {
    var artist: String
    init(name: String, artist: String) {
        self.artist = artist
        super.init(name: name)
    }
}
```

<!--
  - test: `typeCasting, typeCasting-err`

  ```swifttest
  -> class Movie: MediaItem {
        var director: String
        init(name: String, director: String) {
           self.director = director
           super.init(name: name)
        }
     }

  -> class Song: MediaItem {
        var artist: String
        init(name: String, artist: String) {
           self.artist = artist
           super.init(name: name)
        }
     }
  ```
-->

마지막 코드는 두 `Movie` 인스턴스와 세 개의 `Song` 인스턴스를 포함하는
`library`라는 배열 상수를 생성합니다.
`library` 배열의 타입은
배열 리터럴의 내용으로 초기화하여 추론됩니다.
Swift의 타입 검사기는 `Movie`와 `Song`이
`MediaItem`의 상위 클래스를 공통으로 가지고 있으므로,
`library` 배열에 대해 `[MediaItem]` 타입으로 추론할 수 있습니다:

```swift
let library = [
    Movie(name: "Casablanca", director: "Michael Curtiz"),
    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
    Movie(name: "Citizen Kane", director: "Orson Welles"),
    Song(name: "The One And Only", artist: "Chesney Hawkes"),
    Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
]
// the type of "library" is inferred to be [MediaItem]
```

<!--
  - test: `typeCasting`

  ```swifttest
  -> let library = [
        Movie(name: "Casablanca", director: "Michael Curtiz"),
        Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
        Movie(name: "Citizen Kane", director: "Orson Welles"),
        Song(name: "The One And Only", artist: "Chesney Hawkes"),
        Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
     ]
  >> print(type(of: library))
  << Array<MediaItem>
  // the type of "library" is inferred to be [MediaItem]
  ```
-->

`library`에 저장된 항목은 여전히 `Movie`와 `Song` 인스턴스 입니다.
그러나 이 배열의 항목을 반복하면,
항목은 `Movie`나 `Song`이 아닌
`MediaItem` 타입으로 받습니다.
기본 타입으로 작업을 하려면,
아래에 설명된대로
타입을 *확인*하거나
다른 타입으로 *다운 캐스트(downcast)* 해야합니다.

## 타입 검사 (Checking Type)

인스턴스가 특정 하위 클래스 타입인지 확인하기 위해
*타입 검사 연산자(type check operator)*(`is`)를 사용합니다.
이 타입 검사 연산자는 인스턴스가 하위 클래스 타입이면 `true`
아니면 `false`를 반환합니다.

아래의 예시는 `library` 배열의 `Movie`와 `Song` 인스턴스의 개수를 나타내는
두 변수 `movieCount`와 `songCount`를 정의합니다:

```swift
var movieCount = 0
var songCount = 0

for item in library {
    if item is Movie {
        movieCount += 1
    } else if item is Song {
        songCount += 1
    }
}

print("Media library contains \(movieCount) movies and \(songCount) songs")
// Prints "Media library contains 2 movies and 3 songs"
```

<!--
  - test: `typeCasting`

  ```swifttest
  -> var movieCount = 0
  -> var songCount = 0

  -> for item in library {
        if item is Movie {
           movieCount += 1
        } else if item is Song {
           songCount += 1
        }
     }

  -> print("Media library contains \(movieCount) movies and \(songCount) songs")
  <- Media library contains 2 movies and 3 songs
  ```
-->

이 예시는 `library` 배열의 모든 항목을 반복합니다.
반복문 실행 시에 `for`-`in` 루프는 배열에서 다음 `MediaItem`을
`item` 상수에 설정합니다.

`item is Movie`는 현재 `MediaItem`이
`Movie` 인스턴스이면 `true`를 반환하고 아니면 `false`를 반환합니다.
유사하게 `item is Song`은 항목이 `Song` 인스턴스인지 확인합니다.
`for`-`in` 루프의 마지막에 `movieCount`와 `songCount`의 값은
`MediaItem` 인스턴스에서 각 타입을 포함하는 개수를 나타냅니다.

## 다운 캐스팅 (Downcasting)

특정 클래스 타입의 상수나 변수는 하위 클래스의 인스턴스를
참조할 수 있습니다.
이것이 필요하다고 생각하는 경우,
*타입 캐스트 연산자(type cast operator)*(`as?` 또는 `as!`)를 사용하여
하위 클래스 타입으로 *다운 캐스트(downcast)*할 수 있습니다.

다운 캐스팅은 실패할 수 있으므로,
타입 캐스트 연산자는 두 가지 다른 형태로 제공됩니다.
조건부 형식 `as?`은 다운 캐스트를 하려고 할 때 타입의 옵셔널 값을 반환합니다.
강제 형식 `as!`은 다운 캐스트를 시도하고
단일 복합 동작으로 강제 언래핑합니다.

다운 캐스트가 성공할지 확신이 없을 때,
조건부 형식의 타입 캐스트 연산자(`as?`)를 사용합니다.
이 연산자의 형식은 항상 옵셔널 값을 반환하고,
다운 캐스트가 불가능하면 `nil`을 반환합니다.
이것은 다운 캐스트의 성공여부를 확인하는 용도로도 사용 가능합니다.

다운 캐스트가 항상 성공할 것이라는 확신이 있을 때만,
강제 형식의 타입 캐스트 연산자(`as!`)를 사용해야 합니다.
이 연산자의 형식은 유효하지 않은 클래스 타입으로 다운 캐스트를 시도하면
런타임 오류가 발생합니다.

아래의 예시는 `library`의 각 `MediaItem`을 반복하고
각 항목에 대한 적절한 설명을 출력합니다.
이렇게 하려면 `MediaItem`이 아닌,
`Movie`나 `Song`으로 각 항목에 접근해야 합니다.
이렇게 해야
설명에 사용할
`Movie`나 `Song`의 `director`나 `artist` 프로퍼티에 접근할 수 있도록 하기위해 필요합니다.

이 예시는 배열의 각 항목은 `Movie`나
`Song`일 수 있습니다.
각 항목에 사용할 실제 클래스를 미리 알지 못하므로,
루프를 통해 매번 다운 캐스트를 확인하기 위해
타입 캐스트 연산자(`as?`)의 조건부 형식을 사용하는 것이 적절합니다:

```swift
for item in library {
    if let movie = item as? Movie {
        print("Movie: \(movie.name), dir. \(movie.director)")
    } else if let song = item as? Song {
        print("Song: \(song.name), by \(song.artist)")
    }
}

// Movie: Casablanca, dir. Michael Curtiz
// Song: Blue Suede Shoes, by Elvis Presley
// Movie: Citizen Kane, dir. Orson Welles
// Song: The One And Only, by Chesney Hawkes
// Song: Never Gonna Give You Up, by Rick Astley
```

<!--
  - test: `typeCasting`

  ```swifttest
  -> for item in library {
        if let movie = item as? Movie {
           print("Movie: \(movie.name), dir. \(movie.director)")
        } else if let song = item as? Song {
           print("Song: \(song.name), by \(song.artist)")
        }
     }

  </ Movie: Casablanca, dir. Michael Curtiz
  </ Song: Blue Suede Shoes, by Elvis Presley
  </ Movie: Citizen Kane, dir. Orson Welles
  </ Song: The One And Only, by Chesney Hawkes
  </ Song: Never Gonna Give You Up, by Rick Astley
  ```
-->

이 예시는 `Movie`로 현재 `item`을 다운 캐스트 하는 것으로 시작합니다.
`item`은 `MediaItem` 인스턴스 이므로, `Movie`일 수도 *있고*;
마찬가지로 `Song`일 수 있습니다.
또는 기본 `MediaItem`일 수도 있습니다.
이 불확실성 때문에, 타입 캐스트 연산자의 `as?` 형식은
하위 클래스 타입으로 다운 캐스트를 시도할 때 *옵셔널* 값으로 반환합니다.
`item as? Movie`의 결과는 `Movie?` 타입이거나 "옵셔널 `Movie`"입니다.

라이브러리 배열에 `Song` 인스턴스로
적용할 때 `Movie`로 다운 캐스팅은 실패합니다.
이것을 대응하기 위해 위의 예시는 옵셔널 `Movie`가 실제 값이
포함되어 있는지 확인하기 위해 옵셔널 바인딩을 사용합니다.
(다운 캐스트가 성공 되었는지 확인합니다.)
이 옵셔널 바인딩은 "`if let movie = item as? Movie`"로 작성되고,
다음과 같이 읽을 수 있습니다:

"`Movie`로 `item`을 접근하려고 합니다.
성공하면
반환된 옵셔널 `Movie`를
`movie`라는 새로운 임시 상수에 설정합니다."

다운 캐스팅이 성공하면, `movie`의 프로퍼티는
`director`의 이름을 포함하여 `Movie` 인스턴스에 대해 설명을 출력하는데 사용합니다.
유사한 원칙을 사용하여 `Song` 인스턴스를 확인하고,
라이브러리에서 `Song`을 찾을 때마다
`artist` 이름을 포함하여 적절한 설명을 출력하는데 사용합니다.

> Note: 캐스팅은 실제로 인스턴스를 수정하거나 값을 변경하지 않습니다.
> 기본 인스턴스는 동일하게 유지됩니다; 캐스트된 타입의 인스턴스로
> 처리하고 접근합니다.

<!--
  TODO: This example should be followed by the same example written with switch,
  to introduce type casting in a pattern matching context
  and to set up the crazy Any example at the end of the chapter.
-->

<!--
  TODO: No section on upcasting because nobody can come up with
  an example that isn't excessively contrived.
  The reference shows the behavior in a contrived example.
-->

## Any와 AnyObject에 대한 타입 캐스팅 (Type Casting for Any and AnyObject)

Swift는 비특정 타입 작업을 위해 두 가지 특별한 타입을 제공합니다:

- `Any`는 함수 타입을 포함하여 모든 타입의 인스턴스를 나타낼 수 있습니다.
- `AnyObject`는 모든 클래스 타입의 인스턴스를 나타낼 수 있습니다.

제공하는 동작과 기능이
명시적으로 필요한 경우에만 `Any`와 `AnyObject`를 사용합니다.
코드에서 작업할 것으로 예상되는 타입에 대해 구체적으로 지정하는 것이 항상 좋습니다.

다음은 함수 타입과 비 클래스 타입을 포함하여
다른 타입에 혼합으로 작업하기 위해 `Any`를 사용하는 예시입니다.
이 예시는 `Any` 타입의 값을 저장할 수 있는 `things`라는 배열을 생성합니다:

```swift
var things: [Any] = []

things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
things.append({ (name: String) -> String in "Hello, \(name)" })
```

<!--
  - test: `typeCasting, typeCasting-err`

  ```swifttest
  -> var things: [Any] = []

  -> things.append(0)
  -> things.append(0.0)
  -> things.append(42)
  -> things.append(3.14159)
  -> things.append("hello")
  -> things.append((3.0, 5.0))
  -> things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
  -> things.append({ (name: String) -> String in "Hello, \(name)" })
  ```
-->

`things` 배열은
두 `Int` 값, 두 `Double` 값, 하나의 `String` 값,
하나의 `(Double, Double)` 타입의 튜플,
"Ghostbusters" 영화,
`String` 값과 다른 `String` 값을 반환하는
클로저 표현식을 포함합니다.

`Any`나 `AnyObject` 타입으로만 알려진
상수나 변수의 특정 타입을 알아보려면
`switch` 구문의 케이스로 `is`나 `as` 패턴을 사용할 수 있습니다.
아래의 예시는 `things` 배열에 항목을 반복하고
`switch` 구문으로 각 항목의 타입을 조회합니다.
몇몇의 `switch` 구문의 케이스는 일치된 값을
지정된 타입의 상수에 바인딩하여 해당 값을 출력할 수 있도록 합니다:

```swift
for thing in things {
    switch thing {
    case 0 as Int:
        print("zero as an Int")
    case 0 as Double:
        print("zero as a Double")
    case let someInt as Int:
        print("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let someString as String:
        print("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        print("a movie called \(movie.name), dir. \(movie.director)")
    case let stringConverter as (String) -> String:
        print(stringConverter("Michael"))
    default:
        print("something else")
    }
}

// zero as an Int
// zero as a Double
// an integer value of 42
// a positive double value of 3.14159
// a string value of "hello"
// an (x, y) point at 3.0, 5.0
// a movie called Ghostbusters, dir. Ivan Reitman
// Hello, Michael
```

<!--
  - test: `typeCasting`

  ```swifttest
  -> for thing in things {
        switch thing {
           case 0 as Int:
              print("zero as an Int")
           case 0 as Double:
              print("zero as a Double")
           case let someInt as Int:
              print("an integer value of \(someInt)")
           case let someDouble as Double where someDouble > 0:
              print("a positive double value of \(someDouble)")
           case is Double:
              print("some other double value that I don't want to print")
           case let someString as String:
              print("a string value of \"\(someString)\"")
           case let (x, y) as (Double, Double):
              print("an (x, y) point at \(x), \(y)")
           case let movie as Movie:
              print("a movie called \(movie.name), dir. \(movie.director)")
           case let stringConverter as (String) -> String:
              print(stringConverter("Michael"))
           default:
              print("something else")
        }
     }

  </ zero as an Int
  </ zero as a Double
  </ an integer value of 42
  </ a positive double value of 3.14159
  </ a string value of "hello"
  </ an (x, y) point at 3.0, 5.0
  </ a movie called Ghostbusters, dir. Ivan Reitman
  </ Hello, Michael
  ```
-->

> Note: `Any` 타입은 옵셔널 타입을 포함하여 모든 타입의 값을 나타냅니다.
> Swift는 `Any` 타입의 값이 기대되는 곳에
> 옵셔널 값을 사용하면 경고를 줍니다.
> `Any` 값으로 옵셔널 값을 사용하는 것이 필요하다면
> 아래와 같이
> `as` 연산자를 사용하여 명시적으로 옵셔널을 `Any`로 변환할 수 있습니다.
>
> ```swift
> let optionalNumber: Int? = 3
> things.append(optionalNumber)        // Warning
> things.append(optionalNumber as Any) // No warning
> ```

<!--
  - test: `typeCasting-err`

  ```swifttest
  -> let optionalNumber: Int? = 3
  -> things.append(optionalNumber)        // Warning
  !$ warning: expression implicitly coerced from 'Int?' to 'Any'
  !! things.append(optionalNumber)        // Warning
  !!               ^~~~~~~~~~~~~~
  !$ note: provide a default value to avoid this warning
  !! things.append(optionalNumber)        // Warning
  !!               ^~~~~~~~~~~~~~
  !!                              ?? <#default value#>
  !$ note: force-unwrap the value to avoid this warning
  !! things.append(optionalNumber)        // Warning
  !!               ^~~~~~~~~~~~~~
  !!                              !
  !$ note: explicitly cast to 'Any' with 'as Any' to silence this warning
  !! things.append(optionalNumber)        // Warning
  !!               ^~~~~~~~~~~~~~
  !!                              as Any
  -> things.append(optionalNumber as Any) // No warning
  ```
-->

<!--
  Rejected examples to illustrate AnyObject:

  Array of delegates which may conform to one or more of the class's delegate protocols.

  ```
  protocol MovieDelegate {
      func willPlay(movie: Movie)
  }

  class Library {
      var delegates = [AnyObject]
      ...
  }

  for delegate in delegates {
      guard let delegate = delegate as MovieDelegate else { continue }
      delegate.willPlay(movie: m)
  }
  ```

  A userData object for associating some opaque piece of data or state with an API call.

  ```
  class C {
      // Not userInfo -- that's usually a Dictionary
      let userData: AnyObject?  // In Cocoa APIs, userData is a void*
  }
  ```
-->

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->