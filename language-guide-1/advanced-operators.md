# 고급 연산자 (Advanced Operators)

커스텀 연산자를 정의하고, 비트 연산을 수행하고, 빌더 구문을 사용합니다.

[기본 연산자 (Basic Operators)](./basic-operators.md)에서 설명한 연산자 외에도,
Swift는 더 복잡한 값을 조작하는 여러 고급 연산자를 제공합니다.
여기에는 C와 Objective-C에서 익숙한
비트 연산자와 비트 이동 연산자가 포함됩니다.

C의 산술 연산자와 달리,
Swift의 산술 연산자는 기본적으로 오버플로우(overflow)가 발생하지 않습니다.
오버플로우 동작이 감지되면 오류로 보고됩니다.
오버플로우 동작을 원한다면,
오버플로우 덧셈 연산자(`&+`)와 같이
오버플로우를 허용하는 Swift의 별도 연산자를 사용합니다.
이런 오버플로우 연산자는 앰퍼샌드(`&`)로 시작합니다.

구조체, 클래스, 열거형을 정의할 때,
해당 타입에 대한 표준 Swift 연산자를
직접 구현하여 제공하는 게 유용할 수 있습니다.
Swift는 이러한 연산자의 맞춤형 구현을 쉽게 제공하고
각 타입에 대한 동작이 정확히 무엇인지 결정할 수 있습니다.

기본 제공 연산자로 제한하지 않습니다.
Swift는 커스텀 우선순위와 결합 방향성을 정의해
커스텀 중위(infix) 연산자, 접두사(prefix) 연산자, 접미사(postfix) 연산자, 할당(assignment) 연산자를
자유롭게 정의할 수 있습니다.
이러한 연산자는 기본 제공 연산자처럼 사용할 수 있고,
기존 타입을 확장하여 정의한 커스텀 연산자를 지원할 수도 있습니다.

## 비트 연산자 (Bitwise Operators)

*비트 연산자(Bitwise operators)*는 데이터 구조 내에서
개별 원시 데이터 비트를 조작할 수 있게 해줍니다.
그래픽 프로그래밍과 디바이스 드라이버 생성과 같은
low-level 프로그래밍에서 자주 사용합니다.
비트 연산자는 커스텀 프로토콜의 통신을 위한 데이터 인코딩과 디코딩처럼
외부 소스의 원시 데이터로 작업할 때도 유용할 수 있습니다.

Swift는 아래에서 설명한대로, C에서의 모든 비트 연산자를 지원합니다.

### 비트 NOT 연산자 (Bitwise NOT Operator)

*비트 NOT 연산자*(`~`)는 숫자의 모든 비트를 반전시킵니다:

![Bitwise NOT](../.gitbook/assets/bitwiseNOT_2x~dark.png)

비트 NOT 연산자는 접두사 연산자이고,
공백없이
동작할 값 바로 앞에 위치합니다:

```swift
let initialBits: UInt8 = 0b00001111
let invertedBits = ~initialBits  // equals 11110000
```

<!--
  - test: `bitwiseOperators`

  ```swifttest
  -> let initialBits: UInt8 = 0b00001111
  >> assert(initialBits == 15)
  -> let invertedBits = ~initialBits  // equals 11110000
  >> assert(invertedBits == 240)
  ```
-->

`UInt8` 정수는 8비트를 가지고 있으며,
`0`과 `255` 사이의 모든 값을 저장할 수 있습니다.
이 예시는 `UInt8` 정수를 이진값 `00001111`로 초기화하고,
이것은 처음 4비트는 `0`으로 설정하고
다음 4비트는 `1`로 설정합니다.
이것은 십진수 `15`와 같습니다.

<!-- Apple Books screenshot begins here. -->

비트 NOT 연산자는 `invertedBits`라는 새로운 상수를 생성하기 위해 사용되고,
이 상수는 `initialBits`와 같지만
모든 비트를 반전합니다.
0은 1이 되고 1은 0이 됩니다.
`invertedBits`의 값은 `11110000`이고,
이것은 부호 없는 십진수 `240`과 같습니다.

### 비트 AND 연산자 (Bitwise AND Operator)

*비트 AND 연산자*(`&`)는 두 숫자의 비트를 결합합니다.
입력한 숫자 *모두* 비트가 `1`일때만
비트에 `1`을 설정하는 새로운 숫자를 반환합니다:

![Bitwise AND](../.gitbook/assets/bitwiseAND_2x~dark.png)

아래의 예시에서
`firstSixBits`와 `lastSixBits`의 값
모두 가운데 4비트에 `1`을 가지고 있습니다.
비트 AND 연산자는 `00111100`을 만들고,
부호가 없는 십진수 `60`과 같습니다:

```swift
let firstSixBits: UInt8 = 0b11111100
let lastSixBits: UInt8  = 0b00111111
let middleFourBits = firstSixBits & lastSixBits  // equals 00111100
```

<!--
  - test: `bitwiseOperators`

  ```swifttest
  -> let firstSixBits: UInt8 = 0b11111100
  -> let lastSixBits: UInt8  = 0b00111111
  -> let middleFourBits = firstSixBits & lastSixBits  // equals 00111100
  >> assert(middleFourBits == 0b00111100)
  ```
-->

### 비트 OR 연산자 (Bitwise OR Operator)

*비트 OR 연산자*(`|`)는 두 숫자의 비트를 비교합니다.
이 연산자는 입력한 숫자 중 하나라도 비트가 `1`이면
비트에 `1`을 설정하는 새로운 숫자를 반환합니다:

![Bitwise OR](../.gitbook/assets/bitwiseOR_2x~dark.png)

<!-- Apple Books screenshot ends here. -->

아래 예시에서
`someBits`와 `moreBits`의 값은 다른 비트에 `1`을 설정합니다.
비트 OR 연산자는 `11111110` 을 만들고,
부호가 없는 십진수 `254`와 같습니다:

```swift
let someBits: UInt8 = 0b10110010
let moreBits: UInt8 = 0b01011110
let combinedBits = someBits | moreBits  // equals 11111110
```

<!--
  - test: `bitwiseOperators`

  ```swifttest
  -> let someBits: UInt8 = 0b10110010
  -> let moreBits: UInt8 = 0b01011110
  -> let combinedbits = someBits | moreBits  // equals 11111110
  >> assert(combinedbits == 0b11111110)
  ```
-->

### 비트 XOR 연산자 (Bitwise XOR Operator)

*비트 XOR 연산자*나 "배타적인 OR 연산자"(`^`)는
두 숫자의 비트를 비교합니다.
이 연산자는 입력한 비트가 같으면 `0`을 설정하고
다르면
`1`을 설정하는 새로운 숫자를 반환합니다:

![Bitwise XOR](../.gitbook/assets/bitwiseXOR_2x~dark.png)

아래 예시에서
`firstBits`와 `otherBits`의 값은 각각 다른 위치에 `1`로
설정된 비트를 가집니다.
비트 XOR 연산자는 이러한 두 비트를 `1`로 설정합니다.
`firstBits`와 `otherBits`의 나머지 비트는 일치하므로
출력값으로 `0`을 설정합니다:

```swift
let firstBits: UInt8 = 0b00010100
let otherBits: UInt8 = 0b00000101
let outputBits = firstBits ^ otherBits  // equals 00010001
```

<!--
  - test: `bitwiseOperators`

  ```swifttest
  -> let firstBits: UInt8 = 0b00010100
  -> let otherBits: UInt8 = 0b00000101
  -> let outputBits = firstBits ^ otherBits  // equals 00010001
  >> assert(outputBits == 0b00010001)
  ```
-->

### 비트 왼쪽과 오른쪽 이동 연산자 (Bitwise Left and Right Shift Operators)

*비트 왼쪽 이동 연산자*(`<<`)와
*비트 오른쪽 이동 연산자*(`>>`)는
아래의 정의된 규칙에 따라
특정 숫자만큼 왼쪽이나 오른쪽으로 숫자의 모든 비트를 이동합니다.

비트 왼쪽과 오른쪽 이동은
정수를 2배로 곱하거나 나누는 효과가 있습니다.
한 비트만큼 왼쪽으로 정수의 비트를 이동하면 값이 2배가 되고,
반대로 한 비트만큼 오른쪽으로 이동하면 기존값의 반이 됩니다.

<!--
  TODO: mention the caveats to this claim.
-->

#### 부호 없는 정수의 이동 동작 (Shifting Behavior for Unsigned Integers)

부호 없는 정수의 비트 이동 동작은 다음과 같습니다:

1. 기존 비트는 요청된 숫자만큼 왼쪽이나 오른쪽으로 이동됩니다.
2. 정수의 저장소 범위를 넘어 이동된 모든 비트는 삭제됩니다.
3. 비트가 왼쪽이나 오른쪽으로 이동한 후
   뒤에 남겨진 공백에 0을 삽입합니다.

이 접근방식을 *논리 시프트(logical shift)*라고 합니다.

아래 그림은 `11111111 << 1`
(`11111111`을 왼쪽으로 `1`자리 이동),
`11111111 >> 1`
(`11111111`을 오른쪽으로 `1`자리 이동)의 결과를 보여줍니다.
초록 숫자는 이동되고,
회색 숫자는 삭제되고,
핑크 0이 삽입됩니다:

![Bits Shift Unsigned](../.gitbook/assets/bitshiftUnsigned_2x~dark.png)

다음은 Swift 코드에서 비트 이동이 어떻게 이루어지는지 나타냅니다:

```swift
let shiftBits: UInt8 = 4   // 00000100 in binary
shiftBits << 1             // 00001000
shiftBits << 2             // 00010000
shiftBits << 5             // 10000000
shiftBits << 6             // 00000000
shiftBits >> 2             // 00000001
```

<!--
  - test: `bitwiseShiftOperators`

  ```swifttest
  -> let shiftBits: UInt8 = 4   // 00000100 in binary
  >> let r0 =
  -> shiftBits << 1             // 00001000
  >> assert(r0 == 8)
  >> let r1 =
  -> shiftBits << 2             // 00010000
  >> assert(r1 == 16)
  >> let r2 =
  -> shiftBits << 5             // 10000000
  >> assert(r2 == 128)
  >> let r3 =
  -> shiftBits << 6             // 00000000
  >> assert(r3 == 0)
  >> let r4 =
  -> shiftBits >> 2             // 00000001
  >> assert(r4 == 1)
  ```
-->

<!--
  Rewrite the above to avoid bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

다른 데이터 타입의 값을 인코딩이나 디코딩하기 위해 비트 이동을 사용할 수 있습니다:

```swift
let pink: UInt32 = 0xCC6699
let redComponent = (pink & 0xFF0000) >> 16    // redComponent is 0xCC, or 204
let greenComponent = (pink & 0x00FF00) >> 8   // greenComponent is 0x66, or 102
let blueComponent = pink & 0x0000FF           // blueComponent is 0x99, or 153
```

<!--
  - test: `bitwiseShiftOperators`

  ```swifttest
  -> let pink: UInt32 = 0xCC6699
  -> let redComponent = (pink & 0xFF0000) >> 16    // redComponent is 0xCC, or 204
  -> let greenComponent = (pink & 0x00FF00) >> 8   // greenComponent is 0x66, or 102
  -> let blueComponent = pink & 0x0000FF           // blueComponent is 0x99, or 153
  >> assert(redComponent == 204)
  >> assert(greenComponent == 102)
  >> assert(blueComponent == 153)
  ```
-->

이 예시는 분홍색의 Cascading Style Sheets 색상값을
저장하기 위해 `pink`라는 `UInt32` 상수를 사용합니다.
CSS 색상값 `#CC6699`는
Swift의 16진수 표현에서 `0xCC6699`로 작성됩니다.
이 색상은
비트 AND 연산자(`&`)와 비트 오른쪽 이동 연산자(`>>`)에 의해
빨강색(`CC`), 녹색(`66`), 파랑색(`99`)으로 분리됩니다.

빨간색 요소는 `0xCC6699`와 `0xFF0000`의
비트 AND 연산자를 수행하여 얻습니다.
`0xFF0000`의 0은 `0xCC6699`의 두 번째와 세 번째 바이트를 효과적으로 "마스킹"하여
`6699`를 무시하고 결과로 `0xCC0000`을 남깁니다.

그런 다음 이 숫자는 오른쪽으로 16자리 이동합니다(`>> 16`).
16진수의 각 문자 쌍은 8비트를 사용하므로
오른쪽으로 16자리 이동하면 `0xCC0000`을 `0x0000CC`로 변환합니다.
이것은 `0xCC`와 같으며, 십진수 `204`와 동일합니다.

유사하게 녹색 요소는 `0xCC6699`와 `0x00FF00`의
비트 AND 연산자를 수행하여
`0x006600`의 출력값을 얻습니다.
그런 다음 이 출력값을 오른쪽으로 8자리 이동하여
`0x66`의 값을 얻고 이것은 십진수 `102`와 같습니다.

마지막으로 파란색 요소는 `0xCC6699`와 `0x0000FF`의
비트 AND 연산자를 수행하여
`0x000099`의 출력값을 얻습니다.
`0x000099`는 이미 `0x99`와 같고
십진수 `153`의 값이므로
오른쪽으로 이동시킬 필요가 없습니다.

#### 부호 있는 정수의 이동 동작 (Shifting Behavior for Signed Integers)

부호 있는 정수의 이동 동작은 부호 없는 정수보다 복잡하며,
이것은 부호 있는 정수가 이진수로 표현되는 방식 때문입니다.
(아래 예시는 8비트 부호 있는 정수를 기반으로 설명하지만
모든 크기의 부호 있는 정수에 적용됩니다).

부호 있는 정수는 첫 번째 비트(*부호 비트(sign bit)*)를 사용해
정수가 양수인지 음수인지 나타냅니다.
`0`의 부호 비트는 양수를 의미하고 `1`의 부호 비트는 음수를 의미합니다.

나머지 비트(*값 비트(value bits)*)는 실제 정수값을 저장합니다.
양수는 부호 없는 정수와 같은 방법으로 `0`부터
차례대로 저장됩니다.
다음은 `Int8` 타입이 숫자 `4`를 어떻게 나타내는지 보여줍니다:

![Bit Shift Signed Four](../.gitbook/assets/bitshiftSignedFour_2x~dark.png)

부호 비트가 `0`("양수")이고,
7개 비트에 이진수 `4`가
표현되어 있습니다.

그러나 음수는 다르게 저장됩니다.
절대값을 `2`의 `n` 제곱에서 빼서 저장되고,
여기서 `n`은 값 비트의 개수입니다.
8비트 숫자는 7개의 값 비트를 가지므로,
`2`의 `7` 제곱이나 `128`을 의미합니다.

다음은 `Int8` 타입이 숫자 `-4`를 어떻게 나타내는지 보여줍니다:

![Bit Shift Signed Minus Four](../.gitbook/assets/bitshiftSignedMinusFour_2x~dark.png)

이번에는 부호 비트가 `1`("음수")이고,
7개 비트에 `124`(`128 - 4`)의 이진수를 가집니다:

![Bit Shift Signed Minus Four Value](../.gitbook/assets/bitshiftSignedMinusFourValue_2x~dark.png)

이런 음수 표현을 *2의 보수(two's complement)* 표현이라고 합니다.
처음에는 이런 표현이 특별한 방법으로 보일 수 있지만,
여러 가지 장점이 있습니다.

첫 번째로 `-4`에 `-1`을 더할 수 있고, 이것은 8비트 전체
(부호 비트 포함)를
표준 이진 덧셈을 수행하고,
연산 결과가 8비트를 초과하면 초과된 비트는 버릴 수 있습니다:

![Bit Shift Signed Addition](../.gitbook/assets/bitshiftSignedAddition_2x~dark.png)

두 번째로 2의 보수 표현을 사용하면
음수의 비트도 양수처럼 왼쪽과 오른쪽으로 이동할 수 있고,
왼쪽으로 이동할 때마다 두 배로 늘리거나
오른쪽으로 이동할 때마다 반으로 줄입니다.
이를 위해 부호 있는 정수를 오른쪽으로 이동할 때는 추가 규칙이 있습니다:
부호 있는 정수를 오른쪽으로 이동할 때,
부호 없는 정수와 같은 규칙이 적용되지만
왼쪽에 비어있는 모든 비트에는 0이 아닌
*부호 비트(sign bit)*를 채웁니다.

![Bit Shift Signed](../.gitbook/assets/bitshiftSigned_2x~dark.png)

이러한 작업은 부호 있는 정수가 오른쪽으로 이동한 후에 같은 부호를 갖도록 하고
이것을 *산술 이동(arithmetic shift)*이라고 합니다.

양수와 음수에 저장되는 특별한 방식 때문에,
이것을 오른쪽으로 이동하면 0에 가까워 집니다.
이동하는 동안 부호 비트를 동일하게 유지한다는 것은
값이 0에 가까워 질 때 음의 정수가 음수로 유지됨을 의미합니다.

## 오버플로우 연산자 (Overflow Operators)

정수 상수나 정수 변수에 가질 수 없는
숫자 값을 삽입하려고 하면
기본적으로 Swift는 잘못된 값을 생성하지 않고 오류가 발생합니다.
이러한 동작은 숫자가 너무 크거나 너무 작은 값으로 작업할 때 추가 안정성을 제공합니다.

예를 들어 `Int16` 정수 타입은
`-32768`과 `32767` 사이의 모든 부호 있는 정수를 가질 수 있습니다.
`Int16` 상수나 변수에 범위를 벗어나는 숫자를 설정하려고 하면
오류가 발생합니다:

```swift
var potentialOverflow = Int16.max
// potentialOverflow equals 32767, which is the maximum value an Int16 can hold
potentialOverflow += 1
// this causes an error
```

<!--
  - test: `overflowOperatorsWillFailToOverflow`

  ```swifttest
  -> var potentialOverflow = Int16.max
  /> potentialOverflow equals \(potentialOverflow), which is the maximum value an Int16 can hold
  </ potentialOverflow equals 32767, which is the maximum value an Int16 can hold
  -> potentialOverflow += 1
  xx overflow
  // this causes an error
  ```
-->

값이 너무 크거나 너무 작을 때 오류 처리를 제공하면
경계값 조건을 코딩할 때 더 많은 유연성을 얻을 수 있습니다.

그러나 오버플로우가 발생해도
값을 잘라내면서 계속 진행하고 싶을 때는
오류가 발생하는 대신 오버플로우 동작을 선택할 수 있습니다.
Swift는 정수 계산을 위한 오버플로우 동작을 선택하는
세 가지 산술 *오버플로우 연산자(overflow operators)*를 제공합니다.
이러한 연산자는 모두 앰퍼샌드(`&`)로 시작합니다:

- 오버플로우 덧셈(`&+`)
- 오버플로우 뺄셈(`&-`)
- 오버플로우 곱셈(`&*`)

### 값 오버플로우 (Value Overflow)

숫자는 양의 방향과 음의 방향으로 오버플로우 될 수 있습니다.

다음은 부호 없는 정수가 오버플로우 덧셈 연산자(`&+`)를 사용하여
양의 방향으로 오버플로우를 허용하면
무슨 일이 발생하는지를 나타내는 예시입니다:

```swift
var unsignedOverflow = UInt8.max
// unsignedOverflow equals 255, which is the maximum value a UInt8 can hold
unsignedOverflow = unsignedOverflow &+ 1
// unsignedOverflow is now equal to 0
```

<!--
  - test: `overflowOperatorsWillOverflowInPositiveDirection`

  ```swifttest
  -> var unsignedOverflow = UInt8.max
  /> unsignedOverflow equals \(unsignedOverflow), which is the maximum value a UInt8 can hold
  </ unsignedOverflow equals 255, which is the maximum value a UInt8 can hold
  -> unsignedOverflow = unsignedOverflow &+ 1
  /> unsignedOverflow is now equal to \(unsignedOverflow)
  </ unsignedOverflow is now equal to 0
  ```
-->

변수 `unsignedOverflow`는 `UInt8`의 최대값(`255`이나 이진수로 `11111111`)으로
초기화됩니다.
그런 다음 오버플로우 덧셈 연산자(`&+`)를 사용하여 `1`만큼 증가시킵니다.
이것은 `UInt8`이 가질 수 있는 크기보다 큰 바이너리 표현이 적용되어
아래 다이어그램과 같이
경계를 넘어 오버플로우 됩니다.
오버플로우 덧셈 후 `UInt8`의 범위 내에 남아있는 값은
`00000000` 또는 0입니다.

![Overflow Addition](../.gitbook/assets/overflowAddition_2x~dark.png)

부호 없는 정수가 음의 방향으로 오버플로우될 때
비슷한 일이 발생합니다.
다음은 오버플로우 뺄셈 연산자(`&-`)를 사용하는 예시입니다:

```swift
var unsignedOverflow = UInt8.min
// unsignedOverflow equals 0, which is the minimum value a UInt8 can hold
unsignedOverflow = unsignedOverflow &- 1
// unsignedOverflow is now equal to 255
```

<!--
  - test: `overflowOperatorsWillOverflowInNegativeDirection`

  ```swifttest
  -> var unsignedOverflow = UInt8.min
  /> unsignedOverflow equals \(unsignedOverflow), which is the minimum value a UInt8 can hold
  </ unsignedOverflow equals 0, which is the minimum value a UInt8 can hold
  -> unsignedOverflow = unsignedOverflow &- 1
  /> unsignedOverflow is now equal to \(unsignedOverflow)
  </ unsignedOverflow is now equal to 255
  ```
-->

`UInt8`의 최소값은 0이나
이진수로 `00000000`을 가질 수 있습니다.
오버플로우 뺄셈 연산자(`&-`)를 이용하여 `00000000`에서 `1`을 뺀다면
숫자는 오버플로우 되고 `11111111`이나
십진수 `255`입니다.

![Overflow Unsigned Subtraction](../.gitbook/assets/overflowUnsignedSubtraction_2x~dark.png)

오버플로우는 부호 있는 정수에서도 발생합니다.
부호 있는 정수에 대한 모든 덧셈과 뺄셈은 비트 방식으로 수행되며
부호 비트는 [비트 왼쪽과 오른쪽 이동 연산자 (Bitwise Left and Right Shift Operators)](#비트-왼쪽과-오른쪽-이동-연산자-bitwise-left-and-right-shift-operators)에서 설명한대로
덧셈이나 뺄셈 계산에 포함됩니다.

```swift
var signedOverflow = Int8.min
// signedOverflow equals -128, which is the minimum value an Int8 can hold
signedOverflow = signedOverflow &- 1
// signedOverflow is now equal to 127
```

<!--
  - test: `overflowOperatorsWillOverflowSigned`

  ```swifttest
  -> var signedOverflow = Int8.min
  /> signedOverflow equals \(signedOverflow), which is the minimum value an Int8 can hold
  </ signedOverflow equals -128, which is the minimum value an Int8 can hold
  -> signedOverflow = signedOverflow &- 1
  /> signedOverflow is now equal to \(signedOverflow)
  </ signedOverflow is now equal to 127
  ```
-->

`Int8`의 최소값은 `-128`이나
이진수 `10000000`을 가질 수 있습니다.
오버플로우 연산자를 사용하여 이진수에 `1`을 빼면
이진수 `01111111`이 되고,
이것은 부호 비트를 토글하고
`Int8`이 가질 수 있는 최대 양수값 `127`이 됩니다.

![Overflow Signed Subtraction](../.gitbook/assets/overflowSignedSubtraction_2x~dark.png)

부호 있는 정수와 부호 없는 정수에 대해
양의 방향의 오버플로우는
최대 유효 정수값에서 최소값으로 돌아가고
음의 방향의 오버플로우는
최소값에서 최대값으로 순환합니다.

## 우선순위와 결합방향 (Precedence and Associativity)

연산자 *우선순위(precedence)*는 어떤 연산자에 다른 연산자 보다 높은 우선순위를 부여합니다;
이러한 연산자는 먼저 적용됩니다.

연산자 *결합방향(associativity)*은 우선순위가 같은 연산자를
어떻게 결합하는지 정의합니다 ---
왼쪽에서부터 결합하는지 오른쪽에서부터 결합하는지 정의합니다.
"연산자가 왼쪽의 표현식과 결합합니다"
또는 "연산자가 오른쪽의 표현식과 결합합니다"라는 의미로 생각해야 합니다.

복합 표현식이 계산되는 순서를 이해하기 위해서
각 연산자의 우선순위와 결합방향을
고려하는 것이 중요합니다.
예를 들어
연산자 우선순위는 다음 표현식이 왜 `17`인지를 설명합니다.

```swift
2 + 3 % 4 * 5
// this equals 17
```

<!--
  - test: `evaluationOrder`

  ```swifttest
  >> let r0 =
  -> 2 + 3 % 4 * 5
  >> assert(r0 == 17)
  /> this equals \(2 + 3 % 4 * 5)
  </ this equals 17
  ```
-->

<!--
  Rewrite the above to avoid bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

왼쪽에서 오른쪽으로 읽는다면
표현식은 아래와 같이 계산될 것으로 예상할 수 있습니다:

- `2` 더하기 `3`은 `5`
- `5` 나머지 `4`는 `1`
- `1` 곱하기 `5`는 `5`

그러나 실제 정답은 `5`가 아닌 `17`입니다.
우선순위가 높은 연산자는 우선순위가 낮은 연산자보다 먼저 계산됩니다.
Swift에서는 C와 같이
나머지 연산자(`%`)와 곱셈 연산자(`*`)는
덧셈 연산자(`+`)보다 우선순위가 높습니다.
결과적으로 나머지 연산자와 곱셈 연산자 모두 덧셈을 고려하기 전에 계산됩니다.

그러나 나머지와 곱셈은 서로 *동일한* 우선순위를 가집니다.
정확한 순서로 계산을 진행하려면
서로의 결합방향도 고려해야 합니다.
나머지와 곱셈은 모두 연산자 왼쪽의 표현식과 결합됩니다.
연산자 왼쪽에서 시작하여
표현식에 암시적으로 괄호가 있는 것으로 생각해야 합니다:

```swift
2 + ((3 % 4) * 5)
```

<!--
  - test: `evaluationOrder`

  ```swifttest
  >> let r1 =
  -> 2 + ((3 % 4) * 5)
  >> assert(r1 == 17)
  ```
-->

<!--
  Rewrite the above to avoid bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

`(3 % 4)`은 `3`이므로 다음과 같습니다:

```swift
2 + (3 * 5)
```

<!--
  - test: `evaluationOrder`

  ```swifttest
  >> let r2 =
  -> 2 + (3 * 5)
  >> assert(r2 == 17)
  ```
-->

<!--
  Rewrite the above to avoid bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

`(3 * 5)`는 `15`이므로 다음과 같습니다:

```swift
2 + 15
```

<!--
  - test: `evaluationOrder`

  ```swifttest
  >> let r3 =
  -> 2 + 15
  >> assert(r3 == 17)
  ```
-->

<!--
  Rewrite the above to avoid bare expressions.
  Tracking bug is <rdar://problem/35301593>
-->

이 계산의 결과는 `17`입니다.

연산자 우선순위 그룹과 결합방향 설정의 전체 목록을 포함하여
Swift 표준 라이브러리에 의해 제공되는 연산자에 대한 자세한 내용은
[연산자 선언 (Operator Declarations)](https://developer.apple.com/documentation/swift/operator\_declarations)을 참고바랍니다.

> Note: Swift의 연산자 우선순위와 결합방향 규칙은 C와 Objective-C보다
> 더 간단하고 예측 가능합니다.
> 그러나 이것은 C 기반 언어와 정확하게 일치하지 않음을 의미합니다.
> 기존 코드를 Swift로 이식할 때
> 연산자 상호작용이 의도한 대로 작동하는지 계속 확인해야 합니다.

## 연산자 메서드 (Operator Methods)

클래스와 구조체는 기존 연산자의 자체 구현을 제공할 수 있습니다.
이것을 기존 연산자 *오버로딩(overloading)*이라 합니다.

아래의 예시는 커스텀 구조체에
산술 덧셈 연산자(`+`)를 어떻게 구현하는지 보여줍니다.
이 산술 덧셈 연산자는 두 대상에서 동작하기 때문에
이항 연산자(binary operator)이고,
두 대상 사이에 위치하기 때문에 중위 연산자(infix operator)입니다.

이 예시는 2차원 위치 벡터 `(x, y)`에 대한
`Vector2D` 구조체를 정의한 다음
`Vector2D` 구조체의 인스턴스를 함께 더하기 위해
*연산자 메서드(operator method)*를 구현합니다:

```swift
struct Vector2D {
    var x = 0.0, y = 0.0
}

extension Vector2D {
    static func + (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y + right.y)
    }
}
```

<!--
  - test: `customOperators`

  ```swifttest
  -> struct Vector2D {
        var x = 0.0, y = 0.0
     }

  -> extension Vector2D {
         static func + (left: Vector2D, right: Vector2D) -> Vector2D {
            return Vector2D(x: left.x + right.x, y: left.y + right.y)
         }
     }
  ```
-->

이 연산자 메서드는 `Vector2D`에서 타입 메서드로 정의되고,
오버로드 할 연산자(`+`)와 일치하는 메서드 이름을 가집니다.
덧셈은 벡터의 필수 동작의 일부가 아니므로,
이 타입 메서드는 `Vector2D`의 메인 구조체 선언이 아닌
`Vector2D`의 확장에 정의합니다.
산술 덧셈 연산자는 이항 연산자이므로,
이 연산자 메서드는 `Vector2D` 타입의 두 입력 매개변수와
`Vector2D` 타입의 하나의 반환하는 출력값을 가집니다.

이 구현에서 입력 매개변수는 `left`와 `right`의 이름을 가지고
이것은 `+` 연산자의 왼쪽과 오른쪽인
`Vector2D` 인스턴스를 나타냅니다.
이 메서드는 새로운 `Vector2D` 인스턴스를 반환하고,
이것은 두 `Vector2D` 인스턴스의
`x`와 `y` 프로퍼티를 더한
`x`와 `y`를 가진 인스턴스입니다.

이 타입 메서드는
기존 `Vector2D` 인스턴스 사이의 중위 연산자로 사용할 수 있습니다:

```swift
let vector = Vector2D(x: 3.0, y: 1.0)
let anotherVector = Vector2D(x: 2.0, y: 4.0)
let combinedVector = vector + anotherVector
// combinedVector is a Vector2D instance with values of (5.0, 5.0)
```

<!--
  - test: `customOperators`

  ```swifttest
  -> let vector = Vector2D(x: 3.0, y: 1.0)
  -> let anotherVector = Vector2D(x: 2.0, y: 4.0)
  -> let combinedVector = vector + anotherVector
  /> combinedVector is a Vector2D instance with values of (\(combinedVector.x), \(combinedVector.y))
  </ combinedVector is a Vector2D instance with values of (5.0, 5.0)
  ```
-->

이 예시는 벡터 `(3.0, 1.0)`과 `(2.0, 4.0)`을 더하고
아래 그림과 같이 벡터 `(5.0, 5.0)`을 만듭니다.

![Vector Addition](../.gitbook/assets/vectorAddition_2x~dark.png)

### 접두 연산자와 접미 연산자 (Prefix and Postfix Operators)

위에 보여진 예시는 이진 중위 연산자의 커스텀 구현을 보여줍니다.
클래스와 구조체는 표준 *단항(unary)* 연산자의
구현도 제공할 수 있습니다.
단항 연산자는 단일 대상에서 동작합니다.
대상의 앞(`-a`)에 오면 *접두(prefix)* 연산자이며
대상의 뒤(`b!`)에 오면 *접미(postfix)* 연산자 입니다.

접두 단항 연산자나 접미 단항 연산자를 구현하려면
`prefix`나 `postfix` 수정자를
`func` 키워드 앞에 작성하여 연산자 메서드를 선언합니다:

```swift
extension Vector2D {
    static prefix func - (vector: Vector2D) -> Vector2D {
        return Vector2D(x: -vector.x, y: -vector.y)
    }
}
```

<!--
  - test: `customOperators`

  ```swifttest
  -> extension Vector2D {
         static prefix func - (vector: Vector2D) -> Vector2D {
             return Vector2D(x: -vector.x, y: -vector.y)
         }
     }
  ```
-->

위의 예시는 `Vector2D` 인스턴스에 대한
단항 뺄셈 연산자(`-a`)를 구현합니다.
이 단항 뺄셈 연산자는 접두 연산자이므로
이 메서드는 `prefix` 수정자를 지정해야 합니다.

간단한 숫자값에 대해 단항 뺄셈 연산자는
양수를 음수로 음수를 양수로 변환합니다.
`Vector2D` 인스턴스의
`x`와 `y` 프로퍼티에도 동일하게 적용할 수 있습니다:

```swift
let positive = Vector2D(x: 3.0, y: 4.0)
let negative = -positive
// negative is a Vector2D instance with values of (-3.0, -4.0)
let alsoPositive = -negative
// alsoPositive is a Vector2D instance with values of (3.0, 4.0)
```

<!--
  - test: `customOperators`

  ```swifttest
  -> let positive = Vector2D(x: 3.0, y: 4.0)
  -> let negative = -positive
  /> negative is a Vector2D instance with values of (\(negative.x), \(negative.y))
  </ negative is a Vector2D instance with values of (-3.0, -4.0)
  -> let alsoPositive = -negative
  /> alsoPositive is a Vector2D instance with values of (\(alsoPositive.x), \(alsoPositive.y))
  </ alsoPositive is a Vector2D instance with values of (3.0, 4.0)
  ```
-->

### 복합 할당 연산자 (Compound Assignment Operators)

*복합 할당 연산자(Compound assignment operators)*는 할당 연산자(`=`)와 다른 연산을 결합합니다.
예를 들어 덧셈 할당 연산자(`+=`)는
단일 연산으로 덧셈과 할당을 결합합니다.
복합 할당 연산자의 왼쪽 입력 매개변수 타입을 `inout`으로 표시해야 하며,
이것은 매개변수의 값은 연산자 메서드 내에서 직접적으로 수정되기 때문입니다.

아래 예시는
`Vector2D` 인스턴스에 대해 덧셈 할당 연산자 메서드를 구현합니다:

```swift
extension Vector2D {
    static func += (left: inout Vector2D, right: Vector2D) {
        left = left + right
    }
}
```

<!--
  - test: `customOperators`

  ```swifttest
  -> extension Vector2D {
         static func += (left: inout Vector2D, right: Vector2D) {
             left = left + right
         }
     }
  ```
-->

덧셈 연산자는 이전에 정의했으므로
여기서 덧셈 과정을 재구현할 필요가 없습니다.
대신에 덧셈 할당 연산자 메서드는
기존에 덧셈 연산자 메서드를 활용하여
왼쪽의 값을 오른쪽 값으로 더하고 왼쪽 값에 설정하여 사용합니다:

```swift
var original = Vector2D(x: 1.0, y: 2.0)
let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
original += vectorToAdd
// original now has values of (4.0, 6.0)
```

<!--
  - test: `customOperators`

  ```swifttest
  -> var original = Vector2D(x: 1.0, y: 2.0)
  -> let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
  -> original += vectorToAdd
  /> original now has values of (\(original.x), \(original.y))
  </ original now has values of (4.0, 6.0)
  ```
-->

> Note: 기본 할당 연산자(`=`)는
> 오버로드가 불가능합니다.
> 복합 할당 연산자만 오버로드할 수 있습니다.
> 마찬가지로 삼항 조건 연산자
> (`a ? b : c`)도 오버로드할 수 없습니다.

<!--
  - test: `cant-overload-assignment`

  ```swifttest
  >> struct Vector2D {
  >>    var x = 0.0, y = 0.0
  >> }
  >> extension Vector2D {
  >>     static func = (left: inout Vector2D, right: Vector2D) {
  >>         left = right
  >>     }
  >> }
  !$ error: expected identifier in function declaration
  !! static func = (left: inout Vector2D, right: Vector2D) {
  !!             ^
  ```
-->

### 동등 연산자 (Equivalence Operators)

기본적으로 커스텀 클래스와 구조체는
*동등 연산자(equivalence operators)*인
*같음*을 나타내는 연산자(`==`)와 *같지 않음*을 나타내는 연산자(`!=`)의 구현을 가지지 않습니다.
일반적으로 `==` 연산자를 구현하고,
`!=` 연산자는 `==` 연산자의 결과를 부정하는
Swift 표준 라이브러리의 기본 구현을 사용합니다.
`==` 연산자를 구현하는 두 가지 방법이 있습니다:
직접 구현하거나
여러 타입의 경우에
Swift에 구현을 자동 생성하도록 요청할 수 있습니다.
두 경우 모두
Swift 표준 라이브러리의 `Equatable` 프로토콜을 준수해야 합니다.

다른 중위 연산자를 구현하는 것과 같은 방법으로
`==` 연산자의 구현을 제공합니다:

```swift
extension Vector2D: Equatable {
    static func == (left: Vector2D, right: Vector2D) -> Bool {
        return (left.x == right.x) && (left.y == right.y)
    }
}
```

<!--
  - test: `customOperators`

  ```swifttest
  -> extension Vector2D: Equatable {
         static func == (left: Vector2D, right: Vector2D) -> Bool {
            return (left.x == right.x) && (left.y == right.y)
         }
     }
  ```
-->

위의 예시는 두 `Vector2D` 인스턴스가 같은 값을 가지고 있는지 확인하는
`==` 연산자를 구현합니다.
`Vector2D`의 맥락에서
"동일"의 의미는
"두 인스턴스 모두 같은 `x` 값과 `y` 값을 가짐"을 의미하고
이것이 연산자 구현에 사용되는 로직입니다.

이제 두 `Vector2D` 인스턴스가 같은지 확인하기 위해 연산자를 사용할 수 있습니다:

```swift
let twoThree = Vector2D(x: 2.0, y: 3.0)
let anotherTwoThree = Vector2D(x: 2.0, y: 3.0)
if twoThree == anotherTwoThree {
    print("These two vectors are equivalent.")
}
// Prints "These two vectors are equivalent."
```

<!--
  - test: `customOperators`

  ```swifttest
  -> let twoThree = Vector2D(x: 2.0, y: 3.0)
  -> let anotherTwoThree = Vector2D(x: 2.0, y: 3.0)
  -> if twoThree == anotherTwoThree {
        print("These two vectors are equivalent.")
     }
  <- These two vectors are equivalent.
  ```
-->

단순한 경우에
[자동 생성 구현을 사용하여 프로토콜 채택 (Adopting a Protocol Using a Synthesized Implementation)](./protocols.md#자동-생성-구현을-사용하여-프로토콜-채택-adopting-a-protocol-using-a-synthesized-implementation)에서 설명한대로
Swift에 의해 동등 연산자의 자동 생성 구현을 제공받을 수 있습니다.

## 커스텀 연산자 (Custom Operators)

Swift에서 제공하는 표준 연산자 외에
*커스텀 연산자(custom operators)*를 선언하고 구현할 수 있습니다.
커스텀 연산자를 정의하는데 사용할 수 있는 연산자 문자 목록은
[연산자 (Operators)](../language-reference/lexical-structure.md#연산자-operators)를 참고바랍니다.

새로운 연산자는 `operator` 키워드를 사용하여 전역으로 선언되고,
`prefix`, `infix`, `postfix` 수정자를 사용하여 표기할 수 있습니다:

```swift
prefix operator +++
```

<!--
  - test: `customOperators`

  ```swifttest
  -> prefix operator +++
  ```
-->

위의 예시는 `+++`라는 새로운 접두 연산자를 정의합니다.
이 연산자는 Swift에서 기존의 정의되어 있지 않기 때문에,
`Vector2D` 인스턴스 작업에
고유한 커스텀 의미를 부여할 수 있습니다. 이 예시에서
`+++`는 새로운 "두 배로 만들기" 연산자로 취급합니다.
이것은 이전에 정의한 덧셈 할당 연산자로 벡터 자신을 더해서
`Vector2D` 인스턴스의 `x`와 `y` 값을 두 배로 만듭니다.
`+++` 연산자를 구현하기 위해
`Vector2D` 인스턴스에 `+++`라는 타입 메서드를 다음과 같이 추가합니다:

```swift
extension Vector2D {
    static prefix func +++ (vector: inout Vector2D) -> Vector2D {
        vector += vector
        return vector
    }
}

var toBeDoubled = Vector2D(x: 1.0, y: 4.0)
let afterDoubling = +++toBeDoubled
// toBeDoubled now has values of (2.0, 8.0)
// afterDoubling also has values of (2.0, 8.0)
```

<!--
  - test: `customOperators`

  ```swifttest
  -> extension Vector2D {
        static prefix func +++ (vector: inout Vector2D) -> Vector2D {
           vector += vector
           return vector
        }
     }

  -> var toBeDoubled = Vector2D(x: 1.0, y: 4.0)
  -> let afterDoubling = +++toBeDoubled
  /> toBeDoubled now has values of (\(toBeDoubled.x), \(toBeDoubled.y))
  </ toBeDoubled now has values of (2.0, 8.0)
  /> afterDoubling also has values of (\(afterDoubling.x), \(afterDoubling.y))
  </ afterDoubling also has values of (2.0, 8.0)
  ```
-->

### 커스텀 중위 연산자의 우선순위 (Precedence for Custom Infix Operators)

커스텀 중위 연산자는 각 우선순위 그룹에 속해 있습니다.
우선순위 그룹은 해당 연산자가 다른 중위 연산자와 비교해 어느 우선순위와
연산자의 결합방향을 지정합니다.
이러한 특성이 중위 연산자와
다른 중위 연산자의 상호작용에 미치는 영향에 대한 설명은
[우선순위와 결합방향 (Precedence and Associativity)](#우선순위와-결합방향-precedence-and-associativity)을 참고바랍니다.

우선순위 그룹에 명시적으로 속하지 않은 커스텀 중위 연산자는
삼항 조건 연산자의 우선순위
바로 위인 기본 우선순위 그룹이 제공됩니다.

다음의 예시는 `+-`라는 새로운 커스텀 중위 연산자를
`AdditionPrecedence` 우선순위 그룹에 속하도록 정의합니다:

```swift
infix operator +-: AdditionPrecedence
extension Vector2D {
    static func +- (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y - right.y)
    }
}
let firstVector = Vector2D(x: 1.0, y: 2.0)
let secondVector = Vector2D(x: 3.0, y: 4.0)
let plusMinusVector = firstVector +- secondVector
// plusMinusVector is a Vector2D instance with values of (4.0, -2.0)
```

<!--
  - test: `customOperators`

  ```swifttest
  -> infix operator +-: AdditionPrecedence
  -> extension Vector2D {
        static func +- (left: Vector2D, right: Vector2D) -> Vector2D {
           return Vector2D(x: left.x + right.x, y: left.y - right.y)
        }
     }
  -> let firstVector = Vector2D(x: 1.0, y: 2.0)
  -> let secondVector = Vector2D(x: 3.0, y: 4.0)
  -> let plusMinusVector = firstVector +- secondVector
  /> plusMinusVector is a Vector2D instance with values of (\(plusMinusVector.x), \(plusMinusVector.y))
  </ plusMinusVector is a Vector2D instance with values of (4.0, -2.0)
  ```
-->

이 연산자는 두 벡터의 `x` 값을 더하고
첫 번째 벡터에서 두 번째 벡터의 `y` 값을 뺍니다.
본질적으로 "덧셈" 연산자이기 때문에
`+`나 `-`와 같은 덧셈 중위 연산자와
동일한 우선순위 그룹이 지정되었습니다.
Swift 표준 라이브러리에서 제공하는 연산자의
전체 우선순위 그룹과 결합방향 설정 목록은
[연산자 선언 (Operator Declarations)](https://developer.apple.com/documentation/swift/operator\_declarations)을 참고바랍니다.
우선순위 그룹에 대한 자세한 내용과
자체 정의한 연산자와 우선순위 그룹에 대한 문법을 보려면
[연산자 선언 (Operator Declaration)](../language-reference/declarations.md#연산자-선언-operator-declaration)을 참고바랍니다.

> Note: 접두 연산자나 접미 연산자를 정의할 때 우선순위를 지정하지 않습니다.
> 그러나 같은 피연산자에 접두 연산자와 접미 연산자를 모두 적용하면,
> 접미 연산자가 먼저 적용됩니다.

<!--
  - test: `postfixOperatorsAreAppliedBeforePrefixOperators`

  ```swifttest
  -> prefix operator +++
  -> postfix operator ---
  -> extension Int {
         static prefix func +++ (x: Int) -> Int {
             return x * 2
         }
     }
  -> extension Int {
         static postfix func --- (x: Int) -> Int {
             return x - 1
         }
     }
  -> let x = +++1---
  -> let y = +++(1---)
  -> let z = (+++1)---
  -> print(x, y, z)
  <- 0 0 1
  // Note that x==y
  ```
-->

## 결과 빌더 (Result Builders)

*결과 빌더(result builder)*는
리스트(list)나 트리(tree)와 같은
중첩된 데이터를 자연스러운 선언적인 방식으로 생성하기 위한
문법을 추가하는 타입입니다.
결과 빌더를 사용하는 코드는
`if`와 `for`와 같은 Swift 구문을 포함할 수 있어
조건부나 반복적인 데이터를 처리하기에 적합합니다.

아래 코드는 별과 텍스트를 사용하여
한 줄로 그리기 위해 몇 가지 타입을 정의합니다.

```swift
protocol Drawable {
    func draw() -> String
}
struct Line: Drawable {
    var elements: [Drawable]
    func draw() -> String {
        return elements.map { $0.draw() }.joined(separator: "")
    }
}
struct Text: Drawable {
    var content: String
    init(_ content: String) { self.content = content }
    func draw() -> String { return content }
}
struct Space: Drawable {
    func draw() -> String { return " " }
}
struct Stars: Drawable {
    var length: Int
    func draw() -> String { return String(repeating: "*", count: length) }
}
struct AllCaps: Drawable {
    var content: Drawable
    func draw() -> String { return content.draw().uppercased() }
}
```

<!--
  - test: `result-builder`

  ```swifttest
  -> protocol Drawable {
         func draw() -> String
     }
  -> struct Line: Drawable {
         var elements: [Drawable]
         func draw() -> String {
             return elements.map { $0.draw() }.joined(separator: "")
         }
     }
  -> struct Text: Drawable {
         var content: String
         init(_ content: String) { self.content = content }
         func draw() -> String { return content }
     }
  -> struct Space: Drawable {
         func draw() -> String { return " " }
     }
  -> struct Stars: Drawable {
         var length: Int
         func draw() -> String { return String(repeating: "*", count: length) }
     }
  -> struct AllCaps: Drawable {
         var content: Drawable
         func draw() -> String { return content.draw().uppercased() }
     }
  ```
-->

`Drawable` 프로토콜은
선이나 모양을 그릴 수 있는 항목에 대한 요구사항을 정의합니다:
이 타입은 반드시 `draw()` 메서드를 구현해야 합니다.
`Line` 구조체는 단일 선 그리는 것을 나타내며
대부분 그리는 것에 대해 최상위 컨테이너 역할을 합니다.
`Line`을 그리기 위해
구조체는 각 선의 요소에서 `draw()`를 호출한 다음
결과 문자열을 하나의 문자열로 연결합니다.
`Text` 구조체는 문자열을 그리기의 일부로 만듭니다.
`AllCaps` 구조체는 다른 그리기를 감싸고 수정하여
모든 텍스트를 대문자로 변환합니다.

이니셜라이저를 호출하여
이러한 타입으로 그리기가 가능합니다:

```swift
let name: String? = "Ravi Patel"
let manualDrawing = Line(elements: [
    Stars(length: 3),
    Text("Hello"),
    Space(),
    AllCaps(content: Text((name ?? "World") + "!")),
    Stars(length: 2),
    ])
print(manualDrawing.draw())
// Prints "***Hello RAVI PATEL!**"
```

<!--
  - test: `result-builder`

  ```swifttest
  -> let name: String? = "Ravi Patel"
  -> let manualDrawing = Line(elements: [
          Stars(length: 3),
          Text("Hello"),
          Space(),
          AllCaps(content: Text((name ?? "World") + "!")),
          Stars(length: 2),
     ])
  -> print(manualDrawing.draw())
  <- ***Hello RAVI PATEL!**
  ```
-->

이 코드는 동작하지만 약간 어색합니다.
`AllCaps` 뒤에 중첩된 괄호가 많아 읽기 어렵습니다.
`name`이 `nil`일 때 "World"를 사용하는 대체 논리는
`??` 연산자를 사용하여 인라인으로 수행해야 하는데,
더 복잡해지면 읽기 어렵습니다.
그리기를 구성하기 위해
스위치나 `for` 루프를 포함할 수 없습니다.
결과 빌더를 사용하면
일반적인 Swift 코드처럼 보이도록 다시 작성할 수 있습니다.

결과 빌더를 정의하려면,
타입 선언에 `@resultBuilder` 속성을 작성해야 합니다.
예를 들어 이 코드는 `DrawingBuilder` 라는 결과 빌더를 정의하고,
이것은 선언적 문법을 사용하여 그리기를 설명할 수 있습니다:

```swift
@resultBuilder
struct DrawingBuilder {
    static func buildBlock(_ components: Drawable...) -> Drawable {
        return Line(elements: components)
    }
    static func buildEither(first: Drawable) -> Drawable {
        return first
    }
    static func buildEither(second: Drawable) -> Drawable {
        return second
    }
}
```

<!--
  - test: `result-builder`

  ```swifttest
  -> @resultBuilder
  -> struct DrawingBuilder {
         static func buildBlock(_ components: Drawable...) -> Drawable {
             return Line(elements: components)
         }
         static func buildEither(first: Drawable) -> Drawable {
             return first
         }
         static func buildEither(second: Drawable) -> Drawable {
             return second
         }
     }
  ```
-->

`DrawingBuilder` 구조체는 세 개의 메서드를 정의하고,
이것은 결과 빌더 문법의 구현 일부입니다.
`buildBlock(_:)` 메서드는
코드 본문에 선을 작성하기 위한 지원을 추가합니다.
해당 본문의 요소를 `Line`으로 결합합니다.
`buildEither(first:)`와 `buildEither(second:)` 메서드는
`if`-`else`에 대한 지원을 추가합니다.

`@DrawingBuilder`를 함수의 매개변수로 적용하여
함수에 전달된 클로저를
결과 빌더가 해당 클로저에서 생성하는 값으로 바꿀 수 있습니다.
예를 들어:

```swift
func draw(@DrawingBuilder content: () -> Drawable) -> Drawable {
    return content()
}
func caps(@DrawingBuilder content: () -> Drawable) -> Drawable {
    return AllCaps(content: content())
}

func makeGreeting(for name: String? = nil) -> Drawable {
    let greeting = draw {
        Stars(length: 3)
        Text("Hello")
        Space()
        caps {
            if let name = name {
                Text(name + "!")
            } else {
                Text("World!")
            }
        }
        Stars(length: 2)
    }
    return greeting
}
let genericGreeting = makeGreeting()
print(genericGreeting.draw())
// Prints "***Hello WORLD!**"

let personalGreeting = makeGreeting(for: "Ravi Patel")
print(personalGreeting.draw())
// Prints "***Hello RAVI PATEL!**"
```

<!--
  - test: `result-builder`

  ```swifttest
  -> func draw(@DrawingBuilder content: () -> Drawable) -> Drawable {
         return content()
     }
  -> func caps(@DrawingBuilder content: () -> Drawable) -> Drawable {
         return AllCaps(content: content())
     }

  -> func makeGreeting(for name: String? = nil) -> Drawable {
         let greeting = draw {
             Stars(length: 3)
             Text("Hello")
             Space()
             caps {
                 if let name = name {
                     Text(name + "!")
                 } else {
                     Text("World!")
                 }
             }
             Stars(length: 2)
         }
         return greeting
     }
  -> let genericGreeting = makeGreeting()
  -> print(genericGreeting.draw())
  <- ***Hello WORLD!**

  -> let personalGreeting = makeGreeting(for: "Ravi Patel")
  -> print(personalGreeting.draw())
  <- ***Hello RAVI PATEL!**
  ```
-->

`makeGreeting(for:)` 함수는 `name` 매개변수를 가지고 와서
개인화 인사말을 그리는데 사용됩니다.
`draw(_:)`와 `caps(_:)` 함수는
모두 `@DrawingBuilder` 속성으로 표시된
인자로 단일 클로저를 가집니다.
이러한 함수를 호출할 때,
`DrawingBuilder`가 정의하는 특별한 구문을 사용합니다.
Swift는 그리기의 선언적 설명을
`DrawingBuilder`의 메서드에 대한 호출로 변환하여
함수 인자로 전달되는 값을 구성합니다.
예를 들어
Swift는 이 예시에서 `caps(_:)` 호출을
다음과 같은 코드로 변환합니다:

```swift
let capsDrawing = caps {
    let partialDrawing: Drawable
    if let name = name {
        let text = Text(name + "!")
        partialDrawing = DrawingBuilder.buildEither(first: text)
    } else {
        let text = Text("World!")
        partialDrawing = DrawingBuilder.buildEither(second: text)
    }
    return partialDrawing
}
```

<!--
  - test: `result-builder`

  ```swifttest
  -> let capsDrawing = caps {
         let partialDrawing: Drawable
         if let name = name {
             let text = Text(name + "!")
             partialDrawing = DrawingBuilder.buildEither(first: text)
         } else {
             let text = Text("World!")
             partialDrawing = DrawingBuilder.buildEither(second: text)
         }
         return partialDrawing
  -> }
  >> print(capsDrawing.draw())
  << RAVI PATEL!
  ```
-->

Swift는 `if`-`else` 블록을
`buildEither(first:)`와 `buildEither(second:)` 메서드에 대한 호출로 변환합니다.
코드에서 이러한 메서드를 호출하지 않지만
변환 결과를 표시하면
`DrawingBuilder` 구문을 사용할 때
Swift가 코드를 어떻게 변환하는지 쉽게 확인할 수 있습니다.

특별한 그리기 구문에서 `for` 루프 지원을 추가하기 위해
`buildArray(_:)` 메서드를 추가합니다.

```swift
extension DrawingBuilder {
    static func buildArray(_ components: [Drawable]) -> Drawable {
        return Line(elements: components)
    }
}
let manyStars = draw {
    Text("Stars:")
    for length in 1...3 {
        Space()
        Stars(length: length)
    }
}
```

<!--
  - test: `result-builder`

  ```swifttest
  -> extension DrawingBuilder {
         static func buildArray(_ components: [Drawable]) -> Drawable {
             return Line(elements: components)
         }
     }
  -> let manyStars = draw {
         Text("Stars:")
         for length in 1...3 {
             Space()
             Stars(length: length)
         }
  -> }
  >> print(manyStars.draw())
  << Stars: * ** ***
  ```
-->

위의 코드에서 `for` 루프는 그리기의 배열을 생성하고,
`buildArray(_:)` 메서드는 해당 배열을 `Line`으로 변환합니다.

Swift가 빌더 구문을
빌더 타입의 메서드 호출로 변환하는 방법에 대한 전체 목록은
[resultBuilder](../language-reference/attributes.md#resultbuilder)를 참고바랍니다.

<!--
  The following needs more work...

   Protocol Operator Requirements
   ------------------------------

   You can include operators in the requirements of a protocol.
   A type conforms to the protocol
   only if there's an implementation of the operator for that type.
   You use ``Self`` to refer to the type that will conform to the protocol,
   just like you do in other protocol requirements.
   For example, the Swift standard library defines the ``Equatable`` protocol
   which requires the ``==`` operator:

   .. testcode:: protocolOperator

      -> protocol Equatable {
             static func == (lhs: Self, rhs: Self) -> Bool
         }

   To make a type conform to the protocol,
   you need to implement the ``==`` operator for that type.
   For example:

   .. testcode:: protocolOperator

  -> struct Vector3D {
        var x = 0.0, y = 0.0, z = 0.0
     }
  -> extension Vector3D: Equatable {
         static func == (left: Vector3D, right: Vector3D) -> Bool {
             return (left.x == right.x) && (left.y == right.y) && (left.z == right.z)
         }
     }
  >> let r0 =
  >> Vector3D(x: 1.1, y: 2.3, z: 12) == Vector3D(x: 1.1, y: 2.3, z: 12)
  >> assert(r0)
-->

<!--
  FIXME: This doesn't work
  <rdar://problem/27536066> SE-0091 -- can't have protocol conformance & operator implementation in different types

   For operators that take values of two different types,
   the operator's implementation doesn't have to be
   a member of the type that conforms to the protocol ---
   the implementation can also be a member of the other type.
   For example,
   the code below defines the ``*`` operator
   to scale a vector by a given amount.
   The ``Vector2D`` structure conforms to this protocol
   because there's an implementation of the operator
   that takes a ``Vector2D`` as its second argument,
   even though that implementation is a member of ``Double``.

   .. testcode:: customOperators

  -> infix operator *** {}
  -> protocol AnotherProtocol {
         // static func * (scale: Double, vector: Self) -> Self
         static func *** (scale: Double, vector: Vector2D) -> Vector2D
     }

  -> extension Double {
         static func *** (scale: Double, vector: Vector2D) -> Vector2D {
             return Vector2D(x: scale * vector.x, y: scale * vector.y)
         }
     }
  -> extension Vector2D: AnotherProtocol {}
  -> let unitVector = Vector2D(x: 1.0, y: 1.0)
  -> print(2.5 *** unitVector)
  <- Vector2D(x: 2.5, y: 2.5)
-->

<!--
  TODO: However, Doug thought that this might be better covered by Generics,
  where you know that two things are definitely of the same type.
  Perhaps mention it here, but don't actually show an example?
-->

<!--
  TODO: generic operators
-->

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2022 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->
