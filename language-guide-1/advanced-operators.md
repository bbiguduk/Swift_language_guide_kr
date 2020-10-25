# 고급 연산자 \(Advanced Operators\)

[기본 연산자 \(Basic Operators\)](basic-operators.md) 에서 설명한 연산자 외에도 Swift는 더 복잡한 값을 조작하는 여러 고급 연산자를 제공합니다. 여기에는 C와 Objective-C에서 익숙한 모든 비트와 비트 이동 연산자가 포함됩니다.

C의 산술 연산자와 달리 Swift의 산술 연산자는 기본적으로 오버플로우 \(overflow\) 되지 않습니다. 오버플로우 동작은 트랩되고 에러로 보고됩니다. 오버플로우 동작을 선택하려면 오버플로우 더하기 연산자 \(`&+`\)와 같이 기본적으로 오버플로우 되는 Swift의 두번째 산술 연산자 집합을 사용합니다. 모든 오버플로우 연산자는 앰퍼샌드 \(`&`\)로 시작합니다.

고유한 구조체, 클래스, 그리고 열거형을 정의할 때 이러한 사용자 정의 타입에 대한 표준 Swift 연산자의 고유한 구현을 제공하는 게 유용할 수 있습니다. Swift는 이러한 연산자의 맞춤형 구현을 쉽게 제공하고 생성하는 각 타입에 대한 동작이 정확히 무엇인지 결정할 수 있습니다.

사전 정의된 연산자로 제한되지 않습니다. Swift는 사용자 지정 우선순위와 연관성 값으로 사용자 지정 중위, 접두사, 접미사, 그리고 할당 연산자를 자유롭게 정의할 수 있습니다. 이러한 연산자는 미리 정의된 연산자와 같이 코드에서 사용되고 채택될 수 있고 정의한 사용자 지정 연산자를 지원하기 위해 기존 타입을 확장할 수도 있습니다.

## 비트 연산자 \(Bitwise Operators\)

_비트 연산자 \(Bitwise operators\)_ 를 사용하면 데이터 구조 내에서 개별 원시 데이터 비트를 조작할 수 있습니다. 그래픽 프로그래밍과 디바이스 드라이버 생성과 같은 low-level 프로그래밍에 자주 사용됩니다. 비트 연산자는 사용자 지정 프로토콜을 통한 통신을 위한 데이터 인코딩과 디코딩과 같은 외부 소스로 부터 원시 데이터로 작업할 때도 유용할 수 있습니다.

Swift는 아래에서 설명 하겠지만 C에서의 모든 비트 연산자를 지원합니다.

### 비트 NOT 연산자 \(Bitwise NOT Operator\)

_비트 NOT 연산자_ \(`~`\)는 숫자의 모든 비트를 반전합니다:

![Bitwise NOT](../.gitbook/assets/27_bitwisenot_2x.png)

비트 NOT 연산자는 접두사 연산자이고 공백없이 동작할 값 바로 앞에 위치합니다:

```swift
let initialBits: UInt8 = 0b00001111
let invertedBits = ~initialBits  // equals 11110000
```

`UInt8` 정수는 8비트를 가지고 있으며 `0` 과 `255` 사이의 모든 값을 저장할 수 있습니다. 이 예제는 처음 4비트는 `0` 으로 설정하고 두번째 4비트는 `1` 로 설정하는 이진값 `00001111` 로 `UInt8` 정수를 초기화 합니다. 이것은 십진수 `15` 와 같습니다.

그런 다음 비트 NOT 연산자는 `initialBits` 와 같지만 모든 비트를 반전한 `invertedBits` 라는 새로운 상수를 생성합니다. 0은 1이 되고 1은 0이 됩니다. `invertedBits` 의 값은 부호없는 십진수 `240` 과 같은 `11110000` 입니다.

### 비트 AND 연산자 \(Bitwise AND Operator\)

_비트 AND 연산자_ \(`&`\)는 두 숫자의 비트를 결합합니다. 입력된 숫자 _모두_ 비트가 `1` 이면 비트에 `1` 을 설정하는 새로운 숫자를 반환합니다:

![Bitwise AND](../.gitbook/assets/27_bitwiseand_2x.png)

아래의 예제에서 `firstSixBits` 와 `lastSixBits` 의 값 모두 가운데 4비트에 `1` 을 가지고 있습니다. 비트 AND 연산자는 부호가 없는 십진수 `60` 과 같은 숫자 `00111100` 을 만들기 위해 결합합니다:

```swift
let firstSixBits: UInt8 = 0b11111100
let lastSixBits: UInt8  = 0b00111111
let middleFourBits = firstSixBits & lastSixBits  // equals 00111100
```

### 비트 OR 연산자 \(Bitwise OR Operator\)

_비트 OR 연산자_ \(`|`\)는 두 숫자의 비트를 비교합니다. 이 연산자는 입력한 숫자 중 하나라도 비트가 `1` 이면 비트에 `1` 을 설정하는 새로운 숫자를 반환합니다:

![Bitwise OR](../.gitbook/assets/27_bitwiseor_2x.png)

아래 예제에서 `someBits` 와 `moreBits` 의 값은 다른 비트에 `1`을 설정합니다. 비트 OR 연산자는 부호 없는 십진수 `254` 와 같은 숫자 `11111110` 을 만들기 위해 결합합니다:

```swift
let someBits: UInt8 = 0b10110010
let moreBits: UInt8 = 0b01011110
let combinedbits = someBits | moreBits  // equals 11111110
```

### 비트 XOR 연산자 \(Bitwise XOR Operator\)

_비트 XOR 연산자_ 또는 "배타적인 OR 연산자" \(`^`\)는 두 숫자의 비트를 비교합니다. 이 연산자는 입력한 비트가 같으면 `0` 을 설정하고 다르면 `1` 을 설정하는 새로운 숫자를 반환합니다:

![Bitwise XOR](../.gitbook/assets/27_bitwisexor_2x.png)

아래 예제에서 `firstBits` 와 `otherBits` 의 값은 각각 다른 위치에 `1` 로 설정된 비트를 가집니다. 비트 XOR 연산자는 출력값에서 이러한 두 비트에 `1` 로 설정합니다. `firstBits` 와 `otherBits` 에 다른 모든 비트는 일치하고 출력값으로 `0` 을 설정합니다:

```swift
let firstBits: UInt8 = 0b00010100
let otherBits: UInt8 = 0b00000101
let outputBits = firstBits ^ otherBits  // equals 00010001
```

### 비트 왼쪽과 오른쪽 이동 연산자 \(Bitwise Left and Right Shift Operators\)

_비트 왼쪽 이동 연산자_ \(`<<`\)와 _비트 오른쪽 이동 연산자_ \(`>>`\)는 아래의 정의된 규칙에 따라 특정 숫자만큼 왼쪽 또는 오른쪽으로 숫자의 모든 비트를 이동합니다.

비트 왼쪽과 오른쪽 이동은 정수를 2배로 곱하거나 나누는 효과가 있습니다. 한 위치만큼 왼쪽으로 정수의 비트를 이동하면 값이 2배가 되고 반대로 한 위치만큼 오른쪽으로 이동하면 기존 값의 반이 됩니다.

#### 부호없는 정수에 대한 이동 동작 \(Shifting Behavior for Unsigned Integers\)

부호없는 정수에 대해 비스 이동 동작은 아래와 같습니다:

1. 기존 비트는 요청된 숫자만큼 왼쪽 또는 오른쪽으로 이동됩니다.
2. 정수의 저장소 범위를 넘어 이동된 모든 비트는 삭제됩니다.
3. 원래 비트가 왼쪽 또는 오른쪽으로 이동한 후 뒤에 남겨진 공백에 0이 삽입됩니다.

이 접근방식을 _논리적 이동 \(logical shift\)_ 라고 합니다.

아래 그림은 `11111111` 을 왼쪽으로 `1` 자리 이동한 `11111111 << 1` 의 결과와 `11111111` 을 오른쪽으로 `1` 자리 이동한 `11111111 >> 1` 의 결과를 보여줍니다. 파란 숫자는 이동되고 회색 숫자는 삭제되고 오렌지 0는 삽입됩니다:

![Bits Shift Unsigned](../.gitbook/assets/27_bitshiftunsigned_2x.png)

다음은 Swift 코드에서 비트 이동이 어떻게 보이는지 나타냅니다:

```swift
let shiftBits: UInt8 = 4   // 00000100 in binary
shiftBits << 1             // 00001000
shiftBits << 2             // 00010000
shiftBits << 5             // 10000000
shiftBits << 6             // 00000000
shiftBits >> 2             // 00000001
```

다른 데이터 타입 내에서 값을 인코딩과 디코딩 하기 위해 비트 이동을 사용할 수 있습니다:

```swift
let pink: UInt32 = 0xCC6699
let redComponent = (pink & 0xFF0000) >> 16    // redComponent is 0xCC, or 204
let greenComponent = (pink & 0x00FF00) >> 8   // greenComponent is 0x66, or 102
let blueComponent = pink & 0x0000FF           // blueComponent is 0x99, or 153
```

이 예제는 분홍색에 대한 Cascading Style Sheets 색상값을 저장하기 위해 `pink` 라는 `UInt32` 상수를 사용합니다. CSS 색상값 `#CC6699` 는 Swift의 16진수 표현에서 `0xCC6699` 로 작성됩니다. 이 색상은 비트 AND 연산자 \(`&`\)와 비트 오른쪽 이동 연산자 \(`>>`\)에 의해 빨강색 \(`CC`\), 녹색 \(`66`\), 파랑색 \(`99`\)으로 분리됩니다.

빨간색 구성요소는 숫자 `0xCC6699` 와 `0xFF0000` 으로 비트 AND 연산자를 수행하여 얻습니다. `0xFF0000` 의 0은 `0xCC6699` 의 두번째와 세번째 바이트를 효과적으로 "마스킹" 하여 `6699` 를 무시하고 결과로 `0xCC0000` 을 남깁니다.

그런 다음 이 숫자는 오른쪽으로 16자리 이동합니다 \(`>> 16`\). 16진수의 각 문자 쌍은 8비트를 사용하므로 오른쪽으로 16자리 이동하면 `0xCC0000` 을 `0x0000CC` 로 변환합니다. 이것은 십진수 `204` 인 `0xCC` 와 동일합니다.

유사하게 녹색 구성요소는 숫자 `0xCC6699` 와 `0x00FF00` 을 비트 AND 연산자를 수행하여 `0x006600` 의 출력값을 얻습니다. 그런 다음 이 출력값을 오른쪽으로 8자리 이동하여 십진수 `102` 인 `0x66` 의 값을 얻습니다.

마지막으로 파란색 구성요소는 숫자 `0xCC6699` 와 `0x0000FF` 을 비트 AND 연산자를 수행하여 `0x000099` 의 출력값을 얻습니다. `0x000099` 는 이미 십진수 `153` 인 `0x99` 와 같으므로 오른쪽으로 이동시킬 필요가 없습니다.

#### 부호있는 정수에 대한 이동 동작 \(Shifting Behavior for Signed Integers\)

부호 있는 정수는 이진으로 표시되는 방법 때문에 이동 동작은 부호가 없는 정수보다 부호 있는 정수가 더 복잡합니다 \(아래 예제는 단순화를 위해 8비트 부호 있는 정수를 기반으로 하지만 모든 크기의 부호 있는 정수에 동일한 원칙이 적용됩니다\).

부호 있는 정수는 정수가 양수 또는 음수인지 나타내기 위해 _부호 비트 \(sign bit\)_ 로 알려진 첫번째 비트를 사용합니다. `0` 의 부호 비트는 양수를 의미하고 `1` 의 부호 비트는 음수를 의미합니다.

_값 비트 \(value bits\)_ 로 알려진 남아있는 비트는 실제 값을 저장합니다. 양수는 부호없는 정수와 같은 방법으로 `0` 부터 위쪽으로 세어 저장됩니다. 다음은 `Int8` 내부의 비트가 숫자 `4` 를 어떻게 보여주는지 나타냅니다:

![Bit Shift Signed Four](../.gitbook/assets/27_bitshiftsignedfour_2x.png)

"양수"를 의미하는 부호 비트가 `0` 이고 7개 값 비트는 이진 표기법으로 작성된 숫자 `4` 입니다.

그러나 음수는 다르게 저장됩니다. 절대값을 `2` 에서 값비트의 수 인 `n` 의 제곱으로 빼서 저장합니다. 8비트 숫자는 7개의 값 비트를 가지므로 `2` 의 `7` 제곱 또는 `128` 을 의미합니다.

다음은 `Int8` 내부의 비트가 숫자 `-4` 를 어떻게 나타내는지 보여줍니다:

![Bit Shift Signed Minus Four](../.gitbook/assets/27_bitshiftsignedminusfour_2x.png)

이번에는 부호 비트가 "음수"를 의미하는 `1` 이고 7개의 값 비트는 `128 - 4` 인 `124` 의 이진 값을 가집니다:

![Bit Shift Signed Minus Four Value](../.gitbook/assets/27_bitshiftsignedminusfourvalue_2x.png)

음수의 인코딩을 _2의 보수 \(two's complement\)_ 표현이라고 합니다. 음수를 나타내는 비정상적인 방법으로 보일 수 있지만 여러가지 장점이 있습니다.

첫번째, 부호 비트를 포함하여 모든 8비트에 표준 이진 덧셈을 수행하고 완료되면 8비트에 맞지 않는 항목은 삭제하여 간단하게 `-4` 에 `-1` 을 더할 수 있습니다:

![Bit Shift Signed Addition](../.gitbook/assets/27_bitshiftsignedaddition_2x.png)

두번째, 2의 보수 표현을 사용하면 음수의 비트를 양수처럼 왼쪽과 오른쪽으로 이동할 수 있고 왼쪽으로 이동할 때마다 두배로 늘리거나 오른쪽으로 이동할 때마다 반으로 줄입니다. 이를 위해 부호가 있는 정수를 오른쪽으로 이동할 때는 추가 규칙이 사용됩니다: 부호가 있는 정수를 오른쪽으로 이동할 때 부호가 없는 정수와 같은 규칙이 적용되지만 왼쪽에 비어있는 모든 비트에는 0이 아닌 _부호 비트 \(sign bit\)_ 를 채웁니다.

![Bit Shift Signed](../.gitbook/assets/27_bitshiftsigned_2x.png)

이러한 작업은 부호가 있는 정수가 오른쪽으로 이동한 후에 같은 부호를 갖도록 하고 _산술 이동 \(arithmetic shift\)_ 라고 합니다.

양수와 음수에 저장되는 특별한 방식 때문에 둘 중 하나를 오른쪽으로 이동하면 0에 가까워 집니다. 이 이동동안 부호 비트를 동일하게 유지한다는 것은 값이 0에 가까워 질 때 음의 정수가 음수로 유지됨을 의미합니다.

## 오버플로우 연산자 \(Overflow Operators\)

해당 값을 가질 수 없는 정수 상수 또는 변수에 숫자를 삽입하려고 하면 기본적으로 Swift는 유효하지 않은 값 생성을 허용하지 않고 에러가 발생합니다. 이러한 동작은 숫자가 너무 크거나 너무 작은 값으로 작업할 때 추가 안정성을 제공합니다.

예를 들어 `Int16` 정수 타입은 `-32768` 과 `32767` 사이의 모든 부호있는 정수를 가질 수 있습니다. `Int16` 상수나 변수에 범위를 벗어나는 숫자를 설정하려고 하면 에러가 발생합니다:

```swift
var potentialOverflow = Int16.max
// potentialOverflow equals 32767, which is the maximum value an Int16 can hold
potentialOverflow += 1
// this causes an error
```

값이 너무 크거나 너무 작을 때 에러 처리를 제공하면 경계값 조건을 코딩할 때 더 많은 유연성을 얻을 수 있습니다.

그러나 사용 가능한 비트 수를 자르기 위해 특별히 오버플로우 조건을 원하는 경우 에러를 트리거 하는 대신 이 동작을 선택할 수 있습니다. Swift는 정수 계산을 위한 오버플로우 동작을 선택하는 3가지 산술 _오버플로우 연산자 \(overflow operators\)_ 를 제공합니다. 이러한 연산자는 모두 앰퍼샌드 \(`&`\)로 시작합니다:

* 오버플로우 덧셈 \(`&+`\)
* 오버플로우 뺄셈 \(`&-`\)
* 오버플로우 곱셈 \(`&*`\)

### 값 오버플로우 \(Value Overflow\)

숫자는 양과 음의 방향으로 오버플로우 될 수 있습니다.

다음은 부호없는 정수가 오버플로우 덧셈 연산자 \(`&+`\)를 사용하여 양의 방향으로 오버플로우를 허용하면 무슨 일이 발생하는지를 나타내는 예제입니다:

```swift
var unsignedOverflow = UInt8.max
// unsignedOverflow equals 255, which is the maximum value a UInt8 can hold
unsignedOverflow = unsignedOverflow &+ 1
// unsignedOverflow is now equal to 0
```

변수 `unsignedOverflow` 는 `UInt8` 의 최대값 \(`255` 또는 이진수로 `11111111`\)으로 초기화됩니다. 그런 다음 오버플로우 덧셈 연산자 \(`&+`\)를 사용하여 `1` 만큼 증가시킵니다. 이것은 `UInt8` 이 가질 수 있는 크기보다 큰 바이너리 표현이 적용되어 아래 다이어그램과 같이 경계를 넘어 오버플로우 됩니다. 오버플로우 덧셈 후 `UInt8` 의 범위 내에 남아있는 값은 `00000000` 또는 0 입니다.

![Overflow Addition](../.gitbook/assets/27_overflowaddition_2x.png)

부호없는 정수가 음의 방향으로 오버플로우 될 때 비슷한 일이 발생합니다. 다음은 오버플로우 뺄셈 연산자 \(`&-`\)를 사용하는 예제입니다:

```swift
var unsignedOverflow = UInt8.min
// unsignedOverflow equals 0, which is the minimum value a UInt8 can hold
unsignedOverflow = unsignedOverflow &- 1
// unsignedOverflow is now equal to 255
```

`UInt8` 의 최소값은 0 또는 이진수로 `00000000` 을 가질 수 있습니다. 오버플로우 뺄셈 연산자 \(`&-`\)를 이용하여 `00000000` 에서 `1` 을 뺀다면 숫자는 오버플로우 되고 `11111111` 또는 십진수 `255` 로 래핑됩니다.

![Overflow Unsigned Subtraction](../.gitbook/assets/27_overflowunsignedsubtraction_2x.png)

오버플로우는 부호가 있는 정수에서도 발생합니다. 부호가 있는 정수에 대한 모든 덧셈과 뺄셈은 비트 방식으로 수행되며 부호 비트는 [비트 왼쪽과 오른쪽 이동 연산자 \(Bitwise Left and Right Shift Operators\)](advanced-operators.md#bitwise-left-and-right-shift-operators) 에 설명된대로 덧셈 또는 뺄셈 숫자의 부분으로 포함됩니다.

```swift
var signedOverflow = Int8.min
// signedOverflow equals -128, which is the minimum value an Int8 can hold
signedOverflow = signedOverflow &- 1
// signedOverflow is now equal to 127
```

`Int8` 의 최소값은 `-128` 또는 이진수로 `10000000` 을 가질 수 있습니다. 오버플로우 연산자를 사용하여 이진수에 `1` 을 빼면 부호 비트를 토글하고 `Int8` 이 가질 수 있는 최대 양수값 `127` 인 `01111111` 의 이진수를 얻습니다.

![Overflow Signed Subtraction](../.gitbook/assets/27_overflowsignedsubtraction_2x.png)

부호가 있는 정수와 부호가 없는 정수에 대해 양의 방향의 오버플로우는 최대 유효 정수값에서 최소값으로 돌아가고 음의 방향의 오버플로우는 최소값에서 최대값으로 순환합니다.

## 우선순위와 연관성 \(Precedence and Associativity\)

연산자 _우선순위 \(precedence\)_ 는 일부 연산자에 다른 연산자 보다 높은 우선순위를 주고 이러한 연산자는 먼저 적용됩니다.

연산자 _연관성 \(associativity\)_ 은 우선순위가 같은 연산자를 왼쪽에서 그룹화 하거나 오른쪽에서 그룹화하는 방법을 정의합니다. "왼쪽에 있는 표현과 연관된다" 또는 "오른쪽에 있는 표현과 연관된다" 라는 의미로 생각해야 합니다.

복합 표현식이 계산되는 순서를 작업할 때 각 연산자의 우선순위와 연관성을 고려하는 것은 중요합니다. 예를 들어 연산자 우선순위는 다음 표현식이 왜 `17` 인지를 설명합니다.

```swift
2 + 3 % 4 * 5
// this equals 17
```

왼쪽에서 오른쪽으로 읽는다면 표현식은 아래와 같이 계산될 것으로 예상할 수 있습니다:

* `2` 더하기 `3` 은 `5`
* `5` 나머지 `4` 는 `1`
* `1` 곱하기 `5` 는 `5`

그러나 실제 정답은 `5` 가 아닌 `17` 입니다. 우선순위가 높은 연산자는 우선순위가 낮은 연산자보다 먼저 평가됩니다. Swift에서는 C와 같이 나머지 연산자 \(`%`\)와 곱셈 연산자 \(`*`\)는 덧셈 연산자 \(`+`\)보다 우선순위가 높습니다. 결과적으로 해당 연산자 모두 덧셈을 고려하기 전에 평가됩니다.

그러나 나머지와 곱셈은 서로 _동일한_ 우선순위를 가집니다. 정확한 순서로 계산을 진행하려면 서로의 연관성도 고려해야 합니다. 나머지와 곱셈은 모두 왼쪽의 표현식과 관련됩니다. 왼쪽에서 시작하여 표현식에 암시적으로 괄호가 있는 것으로 생각해야 합니다:

```swift
2 + ((3 % 4) * 5)
```

`(3 % 4)` 은 `3` 이므로 아래와 같습니다:

```swift
2 + (3 * 5)
```

`(3 * 5)` 는 `15` 이므로 아래와 같습니다:

```swift
2 + 15
```

이 계산의 결과는 `17` 입니다.

연산자 우선순위 그룹과 연관성 설정의 전체 목록을 포함하여 Swift 표준 라이브러리에 의해 제공되는 연산자에 대한 자세한 내용은 [연산자 선언 \(Operator Declarations\)](https://developer.apple.com/documentation/swift/operator_declarations) 을 참고 바랍니다.

> NOTE   
> Swift의 연산자 우선순위와 연관성 규칙은 C와 Objective-C보다 더 간단하고 예측 가능합니다. 그러나 이것은 C 기반 언어와 정확하게 일치하지 않음을 의미합니다. 기존 코드를 Swift로 이식할 때 연산자 상호작용이 의도한대로 작동하는지 계속 확인해야 합니다.

## 연산자 메서드 \(Operator Methods\)

클래스와 구조체는 기존 연산자의 자체 구현을 제공할 수 있습니다. 이것을 기존 연산자 _오버로딩 \(overloading\)_ 이라 합니다.

아래의 예제는 사용자 정의 구조체에 산술적 덧셈 연산자 \(`+`\)를 어떻게 구현하는지 보여줍니다. 이 산술적 덧셈 연산자는 두 대상에서 동작하고 두 대상 사이에 나타나 _중위 \(infix\)_ 라고 하기 때문에 _이항 연산자 \(binary operator\)_ 입니다.

이 예제는 2차원 벡터 `(x, y)` 에 대한 `Vector2D` 구조체를 정의한 다음 `Vector2D` 구조체의 인스턴스를 함께 더하기 위해 _연산자 메서드 \(operator method\)_ 의 정의가 이어집니다:

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

이 연산자 메서드는 `Vector2D` 에서 오버로드 할 연산자 \(`+`\)와 일치하는 메서드 이름을 가진 타입 메서드로 정의됩니다. 덧셈은 벡터의 필수 동작의 일부가 아니므로 이 타입 메서드는 `Vector2D` 의 메인 구조체 선언이 아닌 `Vector2D` 의 확장에 정의됩니다. 산술적 덧셈 연산자는 이항 연산자 이므로 이 연산자 메서드는 `Vector2D` 타입의 두 입력 파라미터와 `Vector2D` 타입의 하나의 반환하는 출력값을 가집니다.

이 구현에서 입력 파라미터는 `+` 연산자의 왼쪽과 오른쪽 인 `Vector2D` 인스턴스를 나타내기 위해 `left` 와 `right` 의 이름을 가집니다. 이 메서드는 두 `Vector2D` 인스턴스의 `x` 와 `y` 프로퍼티의 합으로 초기화 되는 `x` 와 `y` 를 가진 새로운 `Vector2D` 인스턴스를 반환합니다.

이 타입 메서드는 기존 `Vector2D` 인스턴스 사이에 중위 연산자로 사용될 수 있습니다:

```swift
let vector = Vector2D(x: 3.0, y: 1.0)
let anotherVector = Vector2D(x: 2.0, y: 4.0)
let combinedVector = vector + anotherVector
// combinedVector is a Vector2D instance with values of (5.0, 5.0)
```

이 예제는 아래 그림과 같이 벡터 `(5.0, 5.0)` 을 만들기 위해 벡터 `(3.0, 1.0)` 과 `(2.0, 4.0)` 을 함께 더하는 것을 나타냅니다.

![Vector Addition](../.gitbook/assets/27_vectoraddition_2x.png)

### 접두사와 접미사 연산자 \(Prefix and Postfix Operators\)

위에 보여진 예제는 이진 중위 연산자의 사용자 정의 구현을 보여줍니다. 클래스와 구조체는 표준 _단항 \(unary\)_ 연산자의 구현도 제공할 수 있습니다. 단항 연산자는 단일 대상에서 동작합니다. `-a` 와 같이 대상의 앞에 오면 _접두사 \(prefix\)_ 이며 `b!` 와 같이 대상의 뒤에 오면 _접미사 \(postfix\)_ 연산자 입니다.

연산자 메서드를 선언할 때 `func` 키워드 앞에 `prefix` 또는 `postfix` 수식어를 작성하여 접두사 또는 접미사 단항 연산자를 구현합니다:

```swift
extension Vector2D {
    static prefix func - (vector: Vector2D) -> Vector2D {
        return Vector2D(x: -vector.x, y: -vector.y)
    }
}
```

위의 예제는 `Vector2D` 인스턴스에 대해 단항 마이너스 연산자 \(`-a`\)를 구현합니다. 이 단항 마이너스 연산자는 접두사 연산자이므로 이 메서드는 `prefix` 수식어를 지정해야 합니다.

간단한 숫자값에 대해 단항 마이너스 연산자는 양수를 음수로 또는 그 반대로 변환합니다. `Vector2D` 인스턴스에 대한 해당 구현은 `x` 와 `y` 프로퍼티에서 수행됩니다:

```swift
let positive = Vector2D(x: 3.0, y: 4.0)
let negative = -positive
// negative is a Vector2D instance with values of (-3.0, -4.0)
let alsoPositive = -negative
// alsoPositive is a Vector2D instance with values of (3.0, 4.0)
```

### 복합 할당 연산자 \(Compound Assignment Operators\)

_복합 할당 연산자 \(Compound assignment operators\)_ 는 다른 연산과 할당 \(`=`\)을 결합합니다. 예를 들어 덧셈 할당 연산자 \(`+=`\)는 단일 연산으로 덧셈과 할당을 결합합니다. 파라미터의 값은 연산자 메서드 내에서 직접적으로 수정되므로 복합 할당 연산자의 왼쪽 입력 파라미터 타입을 `inout` 으로 표시합니다.

아래 예제는 `Vector2D` 인스턴스에 대해 덧셈 할당 연산자 메서드를 구현합니다:

```swift
extension Vector2D {
    static func += (left: inout Vector2D, right: Vector2D) {
        left = left + right
    }
}
```

덧셈 연산자는 이전에 정의 되었으므로 여기서 덧셈 프로세스를 재구현할 필요가 없습니다. 대신에 덧셈 할당 연산자 메서드는 기존에 덧셈 연산자 메서드를 활용하여 왼쪽의 값을 오른쪽 값으로 더하고 왼쪽 값에 설정하여 사용합니다:

```swift
var original = Vector2D(x: 1.0, y: 2.0)
let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
original += vectorToAdd
// original now has values of (4.0, 6.0)
```

> NOTE   
> 기본 할당 연산자 \(`=`\)는 오버로드가 불가능합니다. 복합 할당 연산자만 오버로드 될 수 있습니다. 마찬가지로 삼항 조건 연산자 \(`a ? b : c`\)는 오버로드 할 수 없습니다.

### 등가 연산자 \(Equivalence Operators\)

기본적으로 사용자 정의 클래스와 구조체는 _같음_ 연산자 \(`==`\)와 같지 않음 연산자 \(`!=`\)라고 하는 _등가 연산자 \(equivalence operators\)_ 의 구현을 가지지 않습니다. 일반적으로 `==` 연산자를 구현하고 `==` 연산자의 결과를 부정하는 `!=` 연산자의 표준 라이브러리의 기본 구현을 사용합니다. `==` 연산자를 구현하는 두가지 방법이 있습니다: 직접 구현하거나 여러 타입의 경우에 Swift에 구현을 합성하도록 요청할 수 있습니다. 두 경우 모두 표준 라이브러리의 `Equatable` 프로토콜의 준수성을 추가합니다.

다른 중위 연산자를 구현하는 것과 같은 방법으로 `==` 연산자의 구현을 제공합니다:

```swift
extension Vector2D: Equatable {
    static func == (left: Vector2D, right: Vector2D) -> Bool {
        return (left.x == right.x) && (left.y == right.y)
    }
}
```

위의 예제는 두 `Vector2D` 인스턴스가 같은 값을 가지고 있는지 확인하기 위해 `==` 연산자를 구현합니다. `Vector2D` 의 맥락에서 "동일"을 "두 인스턴스 모두 같은 `x` 값과 `y` 값을 가짐"을 의미하는 것으로 간주하는 것이 합리적이므로 이것이 연산자 구현에 사용되는 논리입니다.

이제 두 `Vector2D` 인스턴스가 같은지 확인하기 위해 연산자를 사용할 수 있습니다:

```swift
let twoThree = Vector2D(x: 2.0, y: 3.0)
let anotherTwoThree = Vector2D(x: 2.0, y: 3.0)
if twoThree == anotherTwoThree {
    print("These two vectors are equivalent.")
}
// Prints "These two vectors are equivalent."
```

많은 간단한 경우에 [합성 구현을 사용하여 프로토콜 채택 \(Adopting a Protocol Using a Synthesized Implementation\)](protocols.md#adopting-a-protocol-using-a-synthesized-implementation) 에서 설명 된대로 Swift에 등가 연산자의 합성 구현을 제공하도록 요청할 수 있습니다.

## 사용자 정의 연산자 \(Custom Operators\)

Swift에서 제공하는 표준 연산자 외에 _사용자 정의 연산자 \(custom operators\)_ 를 선언하고 구현할 수 있습니다. 사용자 정의 연산자를 정의하는데 사용할 수 있는 문자의 목록은 [연산자 \(Operators\)](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#ID418) 를 참고 바랍니다.

새로운 연산자는 `operator` 키워드를 사용하여 전역으로 선언되고 `prefix`, `infix` 또는 `postfix` 수식어를 사용하여 표기될 수 있습니다:

```swift
prefix operator +++
```

위의 예제는 `+++` 라는 새로운 접두사 연산자를 정의합니다. 이 연산자는 Swift에서 기존의 의미가 없으므로 `Vector2D` 인스턴스 작업의 특정 컨텍스트에서 아래에 고유한 사용자 정의 의미가 부여됩니다. 이 예제에서 `+++` 는 새로운 "접두사 배가" 연산자로 취급합니다. 이것은 이전에 정의한 덧셈 할당 연산자로 벡터 자신을 더해서 `Vector2D` 인스턴스의 `x` 와 `y` 값을 두배로 만듭니다. `+++` 연산자를 구현하기 위해 `Vector2D` 인스턴스에 `+++` 라는 타입 메서드를 아래와 같이 추가합니다:

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

### 사용자 정의 중위 연산자의 우선순위 \(Precedence for Custom Infix Operators\)

사용자 정의 중위 연산자는 각 우선순위 그룹에 속해 있습니다. 우선순위 그룹은 다른 중위 연산자에 상대적인 연산자의 우선순위와 연산자의 연관성을 지정합니다. 이러한 특성이 중위 연산자와 다른 중위 연산자 와의 상호작용에 미치는 영향에 대한 설명은 [우선순위와 연관성 \(Precedence and Associativity\)](advanced-operators.md#precedence-and-associativity) 을 참고 바랍니다.

우선순위 그룹에 명시적으로 위치되지 않은 사용자 정의 중위 연산자는 삼항 조건 연산자의 우선순위 바로 위인 기본 우선순위 그룹이 제공됩니다.

다음의 예제는 `AdditionPrecedence` 우선순위 그룹에 속해있는 `+-` 라는 새로운 사용자 정의 중위 연산자를 정의합니다:

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

이 연산자는 두 벡터의 `x` 값을 더하고 첫번째 벡터에서 두번째 벡터의 `y` 값을 뺍니다. 본질적으로 "가산" 연산자이기 때문에 `+` 와 `-` 와 같은 가산 중위 연산자와 같은 우선순위 그룹이 지정되었습니다. 연산자 우선순위 그룹과 연관성 설정의 전체 목록을 포함하여 Swift 표준 라이브러리에서 제공하는 연산자에 대한 정보는 [연산자 선언 \(Operator Declarations\)](https://developer.apple.com/documentation/swift/operator_declarations) 을 참고 바랍니다. 우선순위 그룹에 대한 자세한 내용과 고유한 연산자와 우선순위 그룹에 대한 구문을 보려면 [연산자 선언 \(Operator Declaration\)](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#ID380) 를 참고 바랍니다.

> NOTE   
> 접두사 또는 접미사 연산자를 정의할 때 우선순위를 지정하지 않습니다. 그러나 같은 피연산자에 접두사와 접미사 연산자를 모두 적용하면 접미사 연산자가 먼저 적용됩니다.

