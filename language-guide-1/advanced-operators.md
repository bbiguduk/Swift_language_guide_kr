# 고급 연산자 \(Advanced Operators\)

<!--
In addition to the operators described in Basic Operators, Swift provides several advanced operators that perform more complex value manipulation. These include all of the bitwise and bit shifting operators you will be familiar with from C and Objective-C.

Unlike arithmetic operators in C, arithmetic operators in Swift don’t overflow by default. Overflow behavior is trapped and reported as an error. To opt in to overflow behavior, use Swift’s second set of arithmetic operators that overflow by default, such as the overflow addition operator (&+). All of these overflow operators begin with an ampersand (&).

When you define your own structures, classes, and enumerations, it can be useful to provide your own implementations of the standard Swift operators for these custom types. Swift makes it easy to provide tailored implementations of these operators and to determine exactly what their behavior should be for each type you create.

You’re not limited to the predefined operators. Swift gives you the freedom to define your own custom infix, prefix, postfix, and assignment operators, with custom precedence and associativity values. These operators can be used and adopted in your code like any of the predefined operators, and you can even extend existing types to support the custom operators you define.
-->

[기본 연산자 \(Basic Operators\)](basic-operators.md) 에서 설명한 연산자 외에도 Swift는 더 복잡한 값을 조작하는 여러 고급 연산자를 제공합니다. 여기에는 C와 Objective-C에서 익숙한 모든 비트와 비트 이동 연산자가 포함됩니다.

C의 산술 연산자와 달리 Swift의 산술 연산자는 기본적으로 오버플로우 \(overflow\) 되지 않습니다. 오버플로우 동작은 트랩되고 에러로 보고됩니다. 오버플로우 동작을 선택하려면 오버플로우 더하기 연산자 \(`&+`\)와 같이 기본적으로 오버플로우 되는 Swift의 두번째 산술 연산자 집합을 사용합니다. 모든 오버플로우 연산자는 앰퍼샌드 \(`&`\)로 시작합니다.

고유한 구조체, 클래스, 그리고 열거형을 정의할 때 이러한 사용자 정의 타입에 대한 표준 Swift 연산자의 고유한 구현을 제공하는 게 유용할 수 있습니다. Swift는 이러한 연산자의 맞춤형 구현을 쉽게 제공하고 생성하는 각 타입에 대한 동작이 정확히 무엇인지 결정할 수 있습니다.

사전 정의된 연산자로 제한되지 않습니다. Swift는 사용자 지정 우선순위와 연관성 값으로 사용자 지정 중위, 접두사, 접미사, 그리고 할당 연산자를 자유롭게 정의할 수 있습니다. 이러한 연산자는 미리 정의된 연산자와 같이 코드에서 사용되고 채택될 수 있고 정의한 사용자 지정 연산자를 지원하기 위해 기존 타입을 확장할 수도 있습니다.

## 비트 연산자 \(Bitwise Operators\)

<!--
Bitwise operators enable you to manipulate the individual raw data bits within a data structure. They’re often used in low-level programming, such as graphics programming and device driver creation. Bitwise operators can also be useful when you work with raw data from external sources, such as encoding and decoding data for communication over a custom protocol.

Swift supports all of the bitwise operators found in C, as described below.
-->

_비트 연산자 \(Bitwise operators\)_ 를 사용하면 데이터 구조 내에서 개별 원시 데이터 비트를 조작할 수 있습니다. 그래픽 프로그래밍과 디바이스 드라이버 생성과 같은 low-level 프로그래밍에 자주 사용됩니다. 비트 연산자는 사용자 지정 프로토콜을 통한 통신을 위한 데이터 인코딩과 디코딩과 같은 외부 소스로 부터 원시 데이터로 작업할 때도 유용할 수 있습니다.

Swift는 아래에서 설명 하겠지만 C에서의 모든 비트 연산자를 지원합니다.

### 비트 NOT 연산자 \(Bitwise NOT Operator\)

<!--
The bitwise NOT operator (~) inverts all bits in a number:
-->

_비트 NOT 연산자_ \(`~`\)는 숫자의 모든 비트를 반전합니다:

![Bitwise NOT](../.gitbook/assets/27_bitwiseNOT_2x.png)

<!--
The bitwise NOT operator is a prefix operator, and appears immediately before the value it operates on, without any white space:
-->

비트 NOT 연산자는 접두사 연산자이고 공백없이 동작할 값 바로 앞에 위치합니다:

```swift
let initialBits: UInt8 = 0b00001111
let invertedBits = ~initialBits  // equals 11110000
```

<!--
UInt8 integers have eight bits and can store any value between 0 and 255. This example initializes a UInt8 integer with the binary value 00001111, which has its first four bits set to 0, and its second four bits set to 1. This is equivalent to a decimal value of 15.

The bitwise NOT operator is then used to create a new constant called invertedBits, which is equal to initialBits, but with all of the bits inverted. Zeros become ones, and ones become zeros. The value of invertedBits is 11110000, which is equal to an unsigned decimal value of 240.
-->

`UInt8` 정수는 8비트를 가지고 있으며 `0` 과 `255` 사이의 모든 값을 저장할 수 있습니다. 이 예제는 처음 4비트는 `0` 으로 설정하고 두번째 4비트는 `1` 로 설정하는 이진값 `00001111` 로 `UInt8` 정수를 초기화 합니다. 이것은 십진수 `15` 와 같습니다.

그런 다음 비트 NOT 연산자는 `initialBits` 와 같지만 모든 비트를 반전한 `invertedBits` 라는 새로운 상수를 생성합니다. 0은 1이 되고 1은 0이 됩니다. `invertedBits` 의 값은 부호없는 십진수 `240` 과 같은 `11110000` 입니다.

### 비트 AND 연산자 \(Bitwise AND Operator\)

<!--
The bitwise AND operator (&) combines the bits of two numbers. It returns a new number whose bits are set to 1 only if the bits were equal to 1 in both input numbers:
-->

_비트 AND 연산자_ \(`&`\)는 두 숫자의 비트를 결합합니다. 입력된 숫자 _모두_ 비트가 `1` 이면 비트에 `1` 을 설정하는 새로운 숫자를 반환합니다:

![Bitwise AND](../.gitbook/assets/27_bitwiseand_2x.png)

<!--
In the example below, the values of firstSixBits and lastSixBits both have four middle bits equal to 1. The bitwise AND operator combines them to make the number 00111100, which is equal to an unsigned decimal value of 60:
-->

아래의 예제에서 `firstSixBits` 와 `lastSixBits` 의 값 모두 가운데 4비트에 `1` 을 가지고 있습니다. 비트 AND 연산자는 부호가 없는 십진수 `60` 과 같은 숫자 `00111100` 을 만들기 위해 결합합니다:

```swift
let firstSixBits: UInt8 = 0b11111100
let lastSixBits: UInt8  = 0b00111111
let middleFourBits = firstSixBits & lastSixBits  // equals 00111100
```

### 비트 OR 연산자 \(Bitwise OR Operator\)

<!--
The bitwise OR operator (|) compares the bits of two numbers. The operator returns a new number whose bits are set to 1 if the bits are equal to 1 in either input number:
-->

_비트 OR 연산자_ \(`|`\)는 두 숫자의 비트를 비교합니다. 이 연산자는 입력한 숫자 중 하나라도 비트가 `1` 이면 비트에 `1` 을 설정하는 새로운 숫자를 반환합니다:

![Bitwise OR](../.gitbook/assets/27_bitwiseOR_2x.png)

<!--
In the example below, the values of someBits and moreBits have different bits set to 1. The bitwise OR operator combines them to make the number 11111110, which equals an unsigned decimal of 254:
-->

아래 예제에서 `someBits` 와 `moreBits` 의 값은 다른 비트에 `1`을 설정합니다. 비트 OR 연산자는 부호 없는 십진수 `254` 와 같은 숫자 `11111110` 을 만들기 위해 결합합니다:

```swift
let someBits: UInt8 = 0b10110010
let moreBits: UInt8 = 0b01011110
let combinedbits = someBits | moreBits  // equals 11111110
```

### 비트 XOR 연산자 \(Bitwise XOR Operator\)

<!--
The bitwise XOR operator, or “exclusive OR operator” (^), compares the bits of two numbers. The operator returns a new number whose bits are set to 1 where the input bits are different and are set to 0 where the input bits are the same:
-->

_비트 XOR 연산자_ 또는 "배타적인 OR 연산자" \(`^`\)는 두 숫자의 비트를 비교합니다. 이 연산자는 입력한 비트가 같으면 `0` 을 설정하고 다르면 `1` 을 설정하는 새로운 숫자를 반환합니다:

![Bitwise XOR](../.gitbook/assets/27_bitwisexor_2x.png)

<!--
In the example below, the values of firstBits and otherBits each have a bit set to 1 in a location that the other does not. The bitwise XOR operator sets both of these bits to 1 in its output value. All of the other bits in firstBits and otherBits match and are set to 0 in the output value:
-->

아래 예제에서 `firstBits` 와 `otherBits` 의 값은 각각 다른 위치에 `1` 로 설정된 비트를 가집니다. 비트 XOR 연산자는 출력값에서 이러한 두 비트에 `1` 로 설정합니다. `firstBits` 와 `otherBits` 에 다른 모든 비트는 일치하고 출력값으로 `0` 을 설정합니다:

```swift
let firstBits: UInt8 = 0b00010100
let otherBits: UInt8 = 0b00000101
let outputBits = firstBits ^ otherBits  // equals 00010001
```

### 비트 왼쪽과 오른쪽 이동 연산자 \(Bitwise Left and Right Shift Operators\)

<!--
The bitwise left shift operator (<<) and bitwise right shift operator (>>) move all bits in a number to the left or the right by a certain number of places, according to the rules defined below.

Bitwise left and right shifts have the effect of multiplying or dividing an integer by a factor of two. Shifting an integer’s bits to the left by one position doubles its value, whereas shifting it to the right by one position halves its value.
-->

_비트 왼쪽 이동 연산자_ \(`<<`\)와 _비트 오른쪽 이동 연산자_ \(`>>`\)는 아래의 정의된 규칙에 따라 특정 숫자만큼 왼쪽 또는 오른쪽으로 숫자의 모든 비트를 이동합니다.

비트 왼쪽과 오른쪽 이동은 정수를 2배로 곱하거나 나누는 효과가 있습니다. 한 위치만큼 왼쪽으로 정수의 비트를 이동하면 값이 2배가 되고 반대로 한 위치만큼 오른쪽으로 이동하면 기존 값의 반이 됩니다.

#### 부호없는 정수에 대한 이동 동작 \(Shifting Behavior for Unsigned Integers\)

<!--
The bit-shifting behavior for unsigned integers is as follows:

1. Existing bits are moved to the left or right by the requested number of places.
2. Any bits that are moved beyond the bounds of the integer’s storage are discarded.
3. Zeros are inserted in the spaces left behind after the original bits are moved to the left or right.

This approach is known as a logical shift.

The illustration below shows the results of 11111111 << 1 (which is 11111111 shifted to the left by 1 place), and 11111111 >> 1 (which is 11111111 shifted to the right by 1 place). Blue numbers are shifted, gray numbers are discarded, and orange zeros are inserted:
-->

부호없는 정수에 대해 비스 이동 동작은 아래와 같습니다:

1. 기존 비트는 요청된 숫자만큼 왼쪽 또는 오른쪽으로 이동됩니다.
2. 정수의 저장소 범위를 넘어 이동된 모든 비트는 삭제됩니다.
3. 원래 비트가 왼쪽 또는 오른쪽으로 이동한 후 뒤에 남겨진 공백에 0이 삽입됩니다.

이 접근방식을 _논리적 이동 \(logical shift\)_ 라고 합니다.

아래 그림은 `11111111` 을 왼쪽으로 `1` 자리 이동한 `11111111 << 1` 의 결과와 `11111111` 을 오른쪽으로 `1` 자리 이동한 `11111111 >> 1` 의 결과를 보여줍니다. 파란 숫자는 이동되고 회색 숫자는 삭제되고 오렌지 0는 삽입됩니다:

![Bits Shift Unsigned](../.gitbook/assets/27_bitshiftunsigned_2x.png)

<!--
Here’s how bit shifting looks in Swift code:
-->

다음은 Swift 코드에서 비트 이동이 어떻게 보이는지 나타냅니다:

```swift
let shiftBits: UInt8 = 4   // 00000100 in binary
shiftBits << 1             // 00001000
shiftBits << 2             // 00010000
shiftBits << 5             // 10000000
shiftBits << 6             // 00000000
shiftBits >> 2             // 00000001
```

<!--
You can use bit shifting to encode and decode values within other data types:
-->

다른 데이터 타입 내에서 값을 인코딩과 디코딩 하기 위해 비트 이동을 사용할 수 있습니다:

```swift
let pink: UInt32 = 0xCC6699
let redComponent = (pink & 0xFF0000) >> 16    // redComponent is 0xCC, or 204
let greenComponent = (pink & 0x00FF00) >> 8   // greenComponent is 0x66, or 102
let blueComponent = pink & 0x0000FF           // blueComponent is 0x99, or 153
```

<!--
This example uses a UInt32 constant called pink to store a Cascading Style Sheets color value for the color pink. The CSS color value #CC6699 is written as 0xCC6699 in Swift’s hexadecimal number representation. This color is then decomposed into its red (CC), green (66), and blue (99) components by the bitwise AND operator (&) and the bitwise right shift operator (>>).

The red component is obtained by performing a bitwise AND between the numbers 0xCC6699 and 0xFF0000. The zeros in 0xFF0000 effectively “mask” the second and third bytes of 0xCC6699, causing the 6699 to be ignored and leaving 0xCC0000 as the result.

This number is then shifted 16 places to the right (>> 16). Each pair of characters in a hexadecimal number uses 8 bits, so a move 16 places to the right will convert 0xCC0000 into 0x0000CC. This is the same as 0xCC, which has a decimal value of 204.

Similarly, the green component is obtained by performing a bitwise AND between the numbers 0xCC6699 and 0x00FF00, which gives an output value of 0x006600. This output value is then shifted eight places to the right, giving a value of 0x66, which has a decimal value of 102.

Finally, the blue component is obtained by performing a bitwise AND between the numbers 0xCC6699 and 0x0000FF, which gives an output value of 0x000099. Because 0x000099 already equals 0x99, which has a decimal value of 153, this value is used without shifting it to the right,
-->

이 예제는 분홍색에 대한 Cascading Style Sheets 색상값을 저장하기 위해 `pink` 라는 `UInt32` 상수를 사용합니다. CSS 색상값 `#CC6699` 는 Swift의 16진수 표현에서 `0xCC6699` 로 작성됩니다. 이 색상은 비트 AND 연산자 \(`&`\)와 비트 오른쪽 이동 연산자 \(`>>`\)에 의해 빨강색 \(`CC`\), 녹색 \(`66`\), 파랑색 \(`99`\)으로 분리됩니다.

빨간색 구성요소는 숫자 `0xCC6699` 와 `0xFF0000` 으로 비트 AND 연산자를 수행하여 얻습니다. `0xFF0000` 의 0은 `0xCC6699` 의 두번째와 세번째 바이트를 효과적으로 "마스킹" 하여 `6699` 를 무시하고 결과로 `0xCC0000` 을 남깁니다.

그런 다음 이 숫자는 오른쪽으로 16자리 이동합니다 \(`>> 16`\). 16진수의 각 문자 쌍은 8비트를 사용하므로 오른쪽으로 16자리 이동하면 `0xCC0000` 을 `0x0000CC` 로 변환합니다. 이것은 십진수 `204` 인 `0xCC` 와 동일합니다.

유사하게 녹색 구성요소는 숫자 `0xCC6699` 와 `0x00FF00` 을 비트 AND 연산자를 수행하여 `0x006600` 의 출력값을 얻습니다. 그런 다음 이 출력값을 오른쪽으로 8자리 이동하여 십진수 `102` 인 `0x66` 의 값을 얻습니다.

마지막으로 파란색 구성요소는 숫자 `0xCC6699` 와 `0x0000FF` 을 비트 AND 연산자를 수행하여 `0x000099` 의 출력값을 얻습니다. `0x000099` 는 이미 십진수 `153` 인 `0x99` 와 같으므로 오른쪽으로 이동시킬 필요가 없습니다.

#### 부호있는 정수에 대한 이동 동작 \(Shifting Behavior for Signed Integers\)

<!--
The shifting behavior is more complex for signed integers than for unsigned integers, because of the way signed integers are represented in binary. (The examples below are based on 8-bit signed integers for simplicity, but the same principles apply for signed integers of any size.)

Signed integers use their first bit (known as the sign bit) to indicate whether the integer is positive or negative. A sign bit of 0 means positive, and a sign bit of 1 means negative.

The remaining bits (known as the value bits) store the actual value. Positive numbers are stored in exactly the same way as for unsigned integers, counting upwards from 0. Here’s how the bits inside an Int8 look for the number 4:
-->

부호 있는 정수는 이진으로 표시되는 방법 때문에 이동 동작은 부호가 없는 정수보다 부호 있는 정수가 더 복잡합니다 \(아래 예제는 단순화를 위해 8비트 부호 있는 정수를 기반으로 하지만 모든 크기의 부호 있는 정수에 동일한 원칙이 적용됩니다\).

부호 있는 정수는 정수가 양수 또는 음수인지 나타내기 위해 _부호 비트 \(sign bit\)_ 로 알려진 첫번째 비트를 사용합니다. `0` 의 부호 비트는 양수를 의미하고 `1` 의 부호 비트는 음수를 의미합니다.

_값 비트 \(value bits\)_ 로 알려진 남아있는 비트는 실제 값을 저장합니다. 양수는 부호없는 정수와 같은 방법으로 `0` 부터 위쪽으로 세어 저장됩니다. 다음은 `Int8` 내부의 비트가 숫자 `4` 를 어떻게 보여주는지 나타냅니다:

![Bit Shift Signed Four](../.gitbook/assets/27_bitshiftSignedFour_2x.png)

<!--
The sign bit is 0 (meaning “positive”), and the seven value bits are just the number 4, written in binary notation.

Negative numbers, however, are stored differently. They’re stored by subtracting their absolute value from 2 to the power of n, where n is the number of value bits. An eight-bit number has seven value bits, so this means 2 to the power of 7, or 128.

Here’s how the bits inside an Int8 look for the number -4:
-->

"양수"를 의미하는 부호 비트가 `0` 이고 7개 값 비트는 이진 표기법으로 작성된 숫자 `4` 입니다.

그러나 음수는 다르게 저장됩니다. 절대값을 `2` 에서 값비트의 수 인 `n` 의 제곱으로 빼서 저장합니다. 8비트 숫자는 7개의 값 비트를 가지므로 `2` 의 `7` 제곱 또는 `128` 을 의미합니다.

다음은 `Int8` 내부의 비트가 숫자 `-4` 를 어떻게 나타내는지 보여줍니다:

![Bit Shift Signed Minus Four](../.gitbook/assets/27_bitshiftsignedminusfour_2x.png)

<!--
This time, the sign bit is 1 (meaning “negative”), and the seven value bits have a binary value of 124 (which is 128 - 4):
-->

이번에는 부호 비트가 "음수"를 의미하는 `1` 이고 7개의 값 비트는 `128 - 4` 인 `124` 의 이진 값을 가집니다:

![Bit Shift Signed Minus Four Value](../.gitbook/assets/27_bitshiftsignedminusfourvalue_2x.png)

<!--
This encoding for negative numbers is known as a two’s complement representation. It may seem an unusual way to represent negative numbers, but it has several advantages.

First, you can add -1 to -4, simply by performing a standard binary addition of all eight bits (including the sign bit), and discarding anything that doesn’t fit in the eight bits once you’re done:
-->

음수의 인코딩을 _2의 보수 \(two's complement\)_ 표현이라고 합니다. 음수를 나타내는 비정상적인 방법으로 보일 수 있지만 여러가지 장점이 있습니다.

첫번째, 부호 비트를 포함하여 모든 8비트에 표준 이진 덧셈을 수행하고 완료되면 8비트에 맞지 않는 항목은 삭제하여 간단하게 `-4` 에 `-1` 을 더할 수 있습니다:

![Bit Shift Signed Addition](../.gitbook/assets/27_bitshiftsignedaddition_2x.png)

<!--
Second, the two’s complement representation also lets you shift the bits of negative numbers to the left and right like positive numbers, and still end up doubling them for every shift you make to the left, or halving them for every shift you make to the right. To achieve this, an extra rule is used when signed integers are shifted to the right: When you shift signed integers to the right, apply the same rules as for unsigned integers, but fill any empty bits on the left with the sign bit, rather than with a zero.
-->

두번째, 2의 보수 표현을 사용하면 음수의 비트를 양수처럼 왼쪽과 오른쪽으로 이동할 수 있고 왼쪽으로 이동할 때마다 두배로 늘리거나 오른쪽으로 이동할 때마다 반으로 줄입니다. 이를 위해 부호가 있는 정수를 오른쪽으로 이동할 때는 추가 규칙이 사용됩니다: 부호가 있는 정수를 오른쪽으로 이동할 때 부호가 없는 정수와 같은 규칙이 적용되지만 왼쪽에 비어있는 모든 비트에는 0이 아닌 _부호 비트 \(sign bit\)_ 를 채웁니다.

![Bit Shift Signed](../.gitbook/assets/27_bitshiftSigned_2x.png)

<!--
This action ensures that signed integers have the same sign after they’re shifted to the right, and is known as an arithmetic shift.

Because of the special way that positive and negative numbers are stored, shifting either of them to the right moves them closer to zero. Keeping the sign bit the same during this shift means that negative integers remain negative as their value moves closer to zero.
-->

이러한 작업은 부호가 있는 정수가 오른쪽으로 이동한 후에 같은 부호를 갖도록 하고 _산술 이동 \(arithmetic shift\)_ 라고 합니다.

양수와 음수에 저장되는 특별한 방식 때문에 둘 중 하나를 오른쪽으로 이동하면 0에 가까워 집니다. 이 이동동안 부호 비트를 동일하게 유지한다는 것은 값이 0에 가까워 질 때 음의 정수가 음수로 유지됨을 의미합니다.

## 오버플로우 연산자 \(Overflow Operators\)

<!--
If you try to insert a number into an integer constant or variable that can’t hold that value, by default Swift reports an error rather than allowing an invalid value to be created. This behavior gives extra safety when you work with numbers that are too large or too small.

For example, the Int16 integer type can hold any signed integer between -32768 and 32767. Trying to set an Int16 constant or variable to a number outside of this range causes an error:
-->

해당 값을 가질 수 없는 정수 상수 또는 변수에 숫자를 삽입하려고 하면 기본적으로 Swift는 유효하지 않은 값 생성을 허용하지 않고 에러가 발생합니다. 이러한 동작은 숫자가 너무 크거나 너무 작은 값으로 작업할 때 추가 안정성을 제공합니다.

예를 들어 `Int16` 정수 타입은 `-32768` 과 `32767` 사이의 모든 부호있는 정수를 가질 수 있습니다. `Int16` 상수나 변수에 범위를 벗어나는 숫자를 설정하려고 하면 에러가 발생합니다:

```swift
var potentialOverflow = Int16.max
// potentialOverflow equals 32767, which is the maximum value an Int16 can hold
potentialOverflow += 1
// this causes an error
```

<!--
Providing error handling when values get too large or too small gives you much more flexibility when coding for boundary value conditions.

However, when you specifically want an overflow condition to truncate the number of available bits, you can opt in to this behavior rather than triggering an error. Swift provides three arithmetic overflow operators that opt in to the overflow behavior for integer calculations. These operators all begin with an ampersand (&):

* Overflow addition (&+)
* Overflow subtraction (&-)
* Overflow multiplication (&*)
-->

값이 너무 크거나 너무 작을 때 에러 처리를 제공하면 경계값 조건을 코딩할 때 더 많은 유연성을 얻을 수 있습니다.

그러나 사용 가능한 비트 수를 자르기 위해 특별히 오버플로우 조건을 원하는 경우 에러를 트리거 하는 대신 이 동작을 선택할 수 있습니다. Swift는 정수 계산을 위한 오버플로우 동작을 선택하는 3가지 산술 _오버플로우 연산자 \(overflow operators\)_ 를 제공합니다. 이러한 연산자는 모두 앰퍼샌드 \(`&`\)로 시작합니다:

* 오버플로우 덧셈 \(`&+`\)
* 오버플로우 뺄셈 \(`&-`\)
* 오버플로우 곱셈 \(`&*`\)

### 값 오버플로우 \(Value Overflow\)

<!--
Numbers can overflow in both the positive and negative direction.

Here’s an example of what happens when an unsigned integer is allowed to overflow in the positive direction, using the overflow addition operator (&+):
-->

숫자는 양과 음의 방향으로 오버플로우 될 수 있습니다.

다음은 부호없는 정수가 오버플로우 덧셈 연산자 \(`&+`\)를 사용하여 양의 방향으로 오버플로우를 허용하면 무슨 일이 발생하는지를 나타내는 예제입니다:

```swift
var unsignedOverflow = UInt8.max
// unsignedOverflow equals 255, which is the maximum value a UInt8 can hold
unsignedOverflow = unsignedOverflow &+ 1
// unsignedOverflow is now equal to 0
```

<!--
The variable unsignedOverflow is initialized with the maximum value a UInt8 can hold (255, or 11111111 in binary). It’s then incremented by 1 using the overflow addition operator (&+). This pushes its binary representation just over the size that a UInt8 can hold, causing it to overflow beyond its bounds, as shown in the diagram below. The value that remains within the bounds of the UInt8 after the overflow addition is 00000000, or zero.
-->

변수 `unsignedOverflow` 는 `UInt8` 의 최대값 \(`255` 또는 이진수로 `11111111`\)으로 초기화됩니다. 그런 다음 오버플로우 덧셈 연산자 \(`&+`\)를 사용하여 `1` 만큼 증가시킵니다. 이것은 `UInt8` 이 가질 수 있는 크기보다 큰 바이너리 표현이 적용되어 아래 다이어그램과 같이 경계를 넘어 오버플로우 됩니다. 오버플로우 덧셈 후 `UInt8` 의 범위 내에 남아있는 값은 `00000000` 또는 0 입니다.

![Overflow Addition](../.gitbook/assets/27_overflowAddition_2x.png)

<!--
Something similar happens when an unsigned integer is allowed to overflow in the negative direction. Here’s an example using the overflow subtraction operator (&-):
-->

부호없는 정수가 음의 방향으로 오버플로우 될 때 비슷한 일이 발생합니다. 다음은 오버플로우 뺄셈 연산자 \(`&-`\)를 사용하는 예제입니다:

```swift
var unsignedOverflow = UInt8.min
// unsignedOverflow equals 0, which is the minimum value a UInt8 can hold
unsignedOverflow = unsignedOverflow &- 1
// unsignedOverflow is now equal to 255
```

<!--
The minimum value that a UInt8 can hold is zero, or 00000000 in binary. If you subtract 1 from 00000000 using the overflow subtraction operator (&-), the number will overflow and wrap around to 11111111, or 255 in decimal.
-->

`UInt8` 의 최소값은 0 또는 이진수로 `00000000` 을 가질 수 있습니다. 오버플로우 뺄셈 연산자 \(`&-`\)를 이용하여 `00000000` 에서 `1` 을 뺀다면 숫자는 오버플로우 되고 `11111111` 또는 십진수 `255` 로 래핑됩니다.

![Overflow Unsigned Subtraction](../.gitbook/assets/27_overflowUnsignedSubtraction_2x.png)

<!--
Overflow also occurs for signed integers. All addition and subtraction for signed integers is performed in bitwise fashion, with the sign bit included as part of the numbers being added or subtracted, as described in Bitwise Left and Right Shift Operators.
-->

오버플로우는 부호가 있는 정수에서도 발생합니다. 부호가 있는 정수에 대한 모든 덧셈과 뺄셈은 비트 방식으로 수행되며 부호 비트는 [비트 왼쪽과 오른쪽 이동 연산자 \(Bitwise Left and Right Shift Operators\)](advanced-operators.md#bitwise-left-and-right-shift-operators) 에 설명된대로 덧셈 또는 뺄셈 숫자의 부분으로 포함됩니다.

```swift
var signedOverflow = Int8.min
// signedOverflow equals -128, which is the minimum value an Int8 can hold
signedOverflow = signedOverflow &- 1
// signedOverflow is now equal to 127
```

<!--
The minimum value that an Int8 can hold is -128, or 10000000 in binary. Subtracting 1 from this binary number with the overflow operator gives a binary value of 01111111, which toggles the sign bit and gives positive 127, the maximum positive value that an Int8 can hold.
-->

`Int8` 의 최소값은 `-128` 또는 이진수로 `10000000` 을 가질 수 있습니다. 오버플로우 연산자를 사용하여 이진수에 `1` 을 빼면 부호 비트를 토글하고 `Int8` 이 가질 수 있는 최대 양수값 `127` 인 `01111111` 의 이진수를 얻습니다.

![Overflow Signed Subtraction](../.gitbook/assets/27_overflowSignedSubtraction_2x.png)

<!--
For both signed and unsigned integers, overflow in the positive direction wraps around from the maximum valid integer value back to the minimum, and overflow in the negative direction wraps around from the minimum value to the maximum.
-->

부호가 있는 정수와 부호가 없는 정수에 대해 양의 방향의 오버플로우는 최대 유효 정수값에서 최소값으로 돌아가고 음의 방향의 오버플로우는 최소값에서 최대값으로 순환합니다.

## 우선순위와 연관성 \(Precedence and Associativity\)

<!--
Operator precedence gives some operators higher priority than others; these operators are applied first.

Operator associativity defines how operators of the same precedence are grouped together—either grouped from the left, or grouped from the right. Think of it as meaning “they associate with the expression to their left,” or “they associate with the expression to their right.”

It’s important to consider each operator’s precedence and associativity when working out the order in which a compound expression will be calculated. For example, operator precedence explains why the following expression equals 17.
-->

연산자 _우선순위 \(precedence\)_ 는 일부 연산자에 다른 연산자 보다 높은 우선순위를 주고 이러한 연산자는 먼저 적용됩니다.

연산자 _연관성 \(associativity\)_ 은 우선순위가 같은 연산자를 왼쪽에서 그룹화 하거나 오른쪽에서 그룹화하는 방법을 정의합니다. "왼쪽에 있는 표현과 연관된다" 또는 "오른쪽에 있는 표현과 연관된다" 라는 의미로 생각해야 합니다.

복합 표현식이 계산되는 순서를 작업할 때 각 연산자의 우선순위와 연관성을 고려하는 것은 중요합니다. 예를 들어 연산자 우선순위는 다음 표현식이 왜 `17` 인지를 설명합니다.

```swift
2 + 3 % 4 * 5
// this equals 17
```

<!--
If you read strictly from left to right, you might expect the expression to be calculated as follows:

* 2 plus 3 equals 5
* 5 remainder 4 equals 1
* 1 times 5 equals 5

However, the actual answer is 17, not 5. Higher-precedence operators are evaluated before lower-precedence ones. In Swift, as in C, the remainder operator (%) and the multiplication operator (*) have a higher precedence than the addition operator (+). As a result, they’re both evaluated before the addition is considered.

However, remainder and multiplication have the same precedence as each other. To work out the exact evaluation order to use, you also need to consider their associativity. Remainder and multiplication both associate with the expression to their left. Think of this as adding implicit parentheses around these parts of the expression, starting from their left:
-->

왼쪽에서 오른쪽으로 읽는다면 표현식은 아래와 같이 계산될 것으로 예상할 수 있습니다:

* `2` 더하기 `3` 은 `5`
* `5` 나머지 `4` 는 `1`
* `1` 곱하기 `5` 는 `5`

그러나 실제 정답은 `5` 가 아닌 `17` 입니다. 우선순위가 높은 연산자는 우선순위가 낮은 연산자보다 먼저 평가됩니다. Swift에서는 C와 같이 나머지 연산자 \(`%`\)와 곱셈 연산자 \(`*`\)는 덧셈 연산자 \(`+`\)보다 우선순위가 높습니다. 결과적으로 해당 연산자 모두 덧셈을 고려하기 전에 평가됩니다.

그러나 나머지와 곱셈은 서로 _동일한_ 우선순위를 가집니다. 정확한 순서로 계산을 진행하려면 서로의 연관성도 고려해야 합니다. 나머지와 곱셈은 모두 왼쪽의 표현식과 관련됩니다. 왼쪽에서 시작하여 표현식에 암시적으로 괄호가 있는 것으로 생각해야 합니다:

```swift
2 + ((3 % 4) * 5)
```

<!--
(3 % 4) is 3, so this is equivalent to:
-->

`(3 % 4)` 은 `3` 이므로 아래와 같습니다:

```swift
2 + (3 * 5)
```

<!--
(3 * 5) is 15, so this is equivalent to:
-->

`(3 * 5)` 는 `15` 이므로 아래와 같습니다:

```swift
2 + 15
```

<!--
This calculation yields the final answer of 17.

For information about the operators provided by the Swift standard library, including a complete list of the operator precedence groups and associativity settings, see Operator Declarations.
-->

이 계산의 결과는 `17` 입니다.

연산자 우선순위 그룹과 연관성 설정의 전체 목록을 포함하여 Swift 표준 라이브러리에 의해 제공되는 연산자에 대한 자세한 내용은 [연산자 선언 \(Operator Declarations\)](https://developer.apple.com/documentation/swift/operator_declarations) 을 참고 바랍니다.

<!--
NOTE
Swift’s operator precedences and associativity rules are simpler and more predictable than those found in C and Objective-C. However, this means that they aren’t exactly the same as in C-based languages. Be careful to ensure that operator interactions still behave in the way you intend when porting existing code to Swift.
-->

> NOTE   
> Swift의 연산자 우선순위와 연관성 규칙은 C와 Objective-C보다 더 간단하고 예측 가능합니다. 그러나 이것은 C 기반 언어와 정확하게 일치하지 않음을 의미합니다. 기존 코드를 Swift로 이식할 때 연산자 상호작용이 의도한대로 작동하는지 계속 확인해야 합니다.

## 연산자 메서드 \(Operator Methods\)

<!--
Classes and structures can provide their own implementations of existing operators. This is known as overloading the existing operators.

The example below shows how to implement the arithmetic addition operator (+) for a custom structure. The arithmetic addition operator is a binary operator because it operates on two targets and it’s an infix operator because it appears between those two targets.

The example defines a Vector2D structure for a two-dimensional position vector (x, y), followed by a definition of an operator method to add together instances of the Vector2D structure:
-->

클래스와 구조체는 기존 연산자의 자체 구현을 제공할 수 있습니다. 이것을 기존 연산자 _오버로딩 \(overloading\)_ 이라 합니다.

아래의 예제는 사용자 정의 구조체에 산술적 덧셈 연산자 \(`+`\)를 어떻게 구현하는지 보여줍니다. 이 산술적 덧셈 연산자는 두 대상에서 동작하기 때문에 이항 연산자 \(binary operator\) 이고 두 대상 사이에 나타나기 때문에 중위 연산자 \(infix operator\) 입니다.

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

<!--
The operator method is defined as a type method on Vector2D, with a method name that matches the operator to be overloaded (+). Because addition isn’t part of the essential behavior for a vector, the type method is defined in an extension of Vector2D rather than in the main structure declaration of Vector2D. Because the arithmetic addition operator is a binary operator, this operator method takes two input parameters of type Vector2D and returns a single output value, also of type Vector2D.

In this implementation, the input parameters are named left and right to represent the Vector2D instances that will be on the left side and right side of the + operator. The method returns a new Vector2D instance, whose x and y properties are initialized with the sum of the x and y properties from the two Vector2D instances that are added together.

The type method can be used as an infix operator between existing Vector2D instances:
-->

이 연산자 메서드는 `Vector2D` 에서 오버로드 할 연산자 \(`+`\)와 일치하는 메서드 이름을 가진 타입 메서드로 정의됩니다. 덧셈은 벡터의 필수 동작의 일부가 아니므로 이 타입 메서드는 `Vector2D` 의 메인 구조체 선언이 아닌 `Vector2D` 의 확장에 정의됩니다. 산술적 덧셈 연산자는 이항 연산자 이므로 이 연산자 메서드는 `Vector2D` 타입의 두 입력 파라미터와 `Vector2D` 타입의 하나의 반환하는 출력값을 가집니다.

이 구현에서 입력 파라미터는 `+` 연산자의 왼쪽과 오른쪽 인 `Vector2D` 인스턴스를 나타내기 위해 `left` 와 `right` 의 이름을 가집니다. 이 메서드는 두 `Vector2D` 인스턴스의 `x` 와 `y` 프로퍼티의 합으로 초기화 되는 `x` 와 `y` 를 가진 새로운 `Vector2D` 인스턴스를 반환합니다.

이 타입 메서드는 기존 `Vector2D` 인스턴스 사이에 중위 연산자로 사용될 수 있습니다:

```swift
let vector = Vector2D(x: 3.0, y: 1.0)
let anotherVector = Vector2D(x: 2.0, y: 4.0)
let combinedVector = vector + anotherVector
// combinedVector is a Vector2D instance with values of (5.0, 5.0)
```

<!--
This example adds together the vectors (3.0, 1.0) and (2.0, 4.0) to make the vector (5.0, 5.0), as illustrated below.
-->

이 예제는 아래 그림과 같이 벡터 `(5.0, 5.0)` 을 만들기 위해 벡터 `(3.0, 1.0)` 과 `(2.0, 4.0)` 을 함께 더하는 것을 나타냅니다.

![Vector Addition](../.gitbook/assets/27_vectorAddition_2x.png)

### 접두사와 접미사 연산자 \(Prefix and Postfix Operators\)

<!--
The example shown above demonstrates a custom implementation of a binary infix operator. Classes and structures can also provide implementations of the standard unary operators. Unary operators operate on a single target. They’re prefix if they precede their target (such as -a) and postfix operators if they follow their target (such as b!).

You implement a prefix or postfix unary operator by writing the prefix or postfix modifier before the func keyword when declaring the operator method:
-->

위에 보여진 예제는 이진 중위 연산자의 사용자 정의 구현을 보여줍니다. 클래스와 구조체는 표준 _단항 \(unary\)_ 연산자의 구현도 제공할 수 있습니다. 단항 연산자는 단일 대상에서 동작합니다. `-a` 와 같이 대상의 앞에 오면 _접두사 \(prefix\)_ 이며 `b!` 와 같이 대상의 뒤에 오면 _접미사 \(postfix\)_ 연산자 입니다.

연산자 메서드를 선언할 때 `func` 키워드 앞에 `prefix` 또는 `postfix` 수식어를 작성하여 접두사 또는 접미사 단항 연산자를 구현합니다:

```swift
extension Vector2D {
    static prefix func - (vector: Vector2D) -> Vector2D {
        return Vector2D(x: -vector.x, y: -vector.y)
    }
}
```

<!--
The example above implements the unary minus operator (-a) for Vector2D instances. The unary minus operator is a prefix operator, and so this method has to be qualified with the prefix modifier.

For simple numeric values, the unary minus operator converts positive numbers into their negative equivalent and vice versa. The corresponding implementation for Vector2D instances performs this operation on both the x and y properties:
-->

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

<!--
Compound assignment operators combine assignment (=) with another operation. For example, the addition assignment operator (+=) combines addition and assignment into a single operation. You mark a compound assignment operator’s left input parameter type as inout, because the parameter’s value will be modified directly from within the operator method.

The example below implements an addition assignment operator method for Vector2D instances:
-->

_복합 할당 연산자 \(Compound assignment operators\)_ 는 다른 연산과 할당 \(`=`\)을 결합합니다. 예를 들어 덧셈 할당 연산자 \(`+=`\)는 단일 연산으로 덧셈과 할당을 결합합니다. 파라미터의 값은 연산자 메서드 내에서 직접적으로 수정되므로 복합 할당 연산자의 왼쪽 입력 파라미터 타입을 `inout` 으로 표시합니다.

아래 예제는 `Vector2D` 인스턴스에 대해 덧셈 할당 연산자 메서드를 구현합니다:

```swift
extension Vector2D {
    static func += (left: inout Vector2D, right: Vector2D) {
        left = left + right
    }
}
```

<!--
Because an addition operator was defined earlier, you don’t need to reimplement the addition process here. Instead, the addition assignment operator method takes advantage of the existing addition operator method, and uses it to set the left value to be the left value plus the right value:
-->

덧셈 연산자는 이전에 정의 되었으므로 여기서 덧셈 프로세스를 재구현할 필요가 없습니다. 대신에 덧셈 할당 연산자 메서드는 기존에 덧셈 연산자 메서드를 활용하여 왼쪽의 값을 오른쪽 값으로 더하고 왼쪽 값에 설정하여 사용합니다:

```swift
var original = Vector2D(x: 1.0, y: 2.0)
let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
original += vectorToAdd
// original now has values of (4.0, 6.0)
```

<!--
NOTE
It isn’t possible to overload the default assignment operator (=). Only the compound assignment operators can be overloaded. Similarly, the ternary conditional operator (a ? b : c) can’t be overloaded.
-->

> NOTE   
> 기본 할당 연산자 \(`=`\)는 오버로드가 불가능합니다. 복합 할당 연산자만 오버로드 될 수 있습니다. 마찬가지로 삼항 조건 연산자 \(`a ? b : c`\)는 오버로드 할 수 없습니다.

### 등가 연산자 \(Equivalence Operators\)

<!--
By default, custom classes and structures don’t have an implementation of the equivalence operators, known as the equal to operator (==) and not equal to operator (!=). You usually implement the == operator, and use the standard library’s default implementation of the != operator that negates the result of the == operator. There are two ways to implement the == operator: You can implement it yourself, or for many types, you can ask Swift to synthesize an implementation for you. In both cases, you add conformance to the standard library’s Equatable protocol.

You provide an implementation of the == operator in the same way as you implement other infix operators:
-->

기본적으로 사용자 정의 클래스와 구조체는 _같음_ 연산자 \(`==`\)와 같지 않음 연산자 \(`!=`\)라고 하는 _등가 연산자 \(equivalence operators\)_ 의 구현을 가지지 않습니다. 일반적으로 `==` 연산자를 구현하고 `==` 연산자의 결과를 부정하는 `!=` 연산자의 표준 라이브러리의 기본 구현을 사용합니다. `==` 연산자를 구현하는 두가지 방법이 있습니다: 직접 구현하거나 여러 타입의 경우에 Swift에 구현을 합성하도록 요청할 수 있습니다. 두 경우 모두 표준 라이브러리의 `Equatable` 프로토콜의 준수성을 추가합니다.

다른 중위 연산자를 구현하는 것과 같은 방법으로 `==` 연산자의 구현을 제공합니다:

```swift
extension Vector2D: Equatable {
    static func == (left: Vector2D, right: Vector2D) -> Bool {
        return (left.x == right.x) && (left.y == right.y)
    }
}
```

<!--
The example above implements an == operator to check whether two Vector2D instances have equivalent values. In the context of Vector2D, it makes sense to consider “equal” as meaning “both instances have the same x values and y values”, and so this is the logic used by the operator implementation.

You can now use this operator to check whether two Vector2D instances are equivalent:
-->

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

<!--
In many simple cases, you can ask Swift to provide synthesized implementations of the equivalence operators for you, as described in Adopting a Protocol Using a Synthesized Implementation.
-->

많은 간단한 경우에 [합성 구현을 사용하여 프로토콜 채택 \(Adopting a Protocol Using a Synthesized Implementation\)](protocols.md#adopting-a-protocol-using-a-synthesized-implementation) 에서 설명 된대로 Swift에 등가 연산자의 합성 구현을 제공하도록 요청할 수 있습니다.

## 사용자 정의 연산자 \(Custom Operators\)

<!--
You can declare and implement your own custom operators in addition to the standard operators provided by Swift. For a list of characters that can be used to define custom operators, see Operators.

New operators are declared at a global level using the operator keyword, and are marked with the prefix, infix or postfix modifiers:
-->

Swift에서 제공하는 표준 연산자 외에 _사용자 정의 연산자 \(custom operators\)_ 를 선언하고 구현할 수 있습니다. 사용자 정의 연산자를 정의하는데 사용할 수 있는 문자의 목록은 [연산자 \(Operators\)](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#ID418) 를 참고 바랍니다.

새로운 연산자는 `operator` 키워드를 사용하여 전역으로 선언되고 `prefix`, `infix` 또는 `postfix` 수식어를 사용하여 표기될 수 있습니다:

```swift
prefix operator +++
```

<!--
The example above defines a new prefix operator called +++. This operator doesn’t have an existing meaning in Swift, and so it’s given its own custom meaning below in the specific context of working with Vector2D instances. For the purposes of this example, +++ is treated as a new “prefix doubling” operator. It doubles the x and y values of a Vector2D instance, by adding the vector to itself with the addition assignment operator defined earlier. To implement the +++ operator, you add a type method called +++ to Vector2D as follows:
-->

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

<!--
Custom infix operators each belong to a precedence group. A precedence group specifies an operator’s precedence relative to other infix operators, as well as the operator’s associativity. See Precedence and Associativity for an explanation of how these characteristics affect an infix operator’s interaction with other infix operators.

A custom infix operator that isn’t explicitly placed into a precedence group is given a default precedence group with a precedence immediately higher than the precedence of the ternary conditional operator.

The following example defines a new custom infix operator called +-, which belongs to the precedence group AdditionPrecedence:
-->

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

<!--
This operator adds together the x values of two vectors, and subtracts the y value of the second vector from the first. Because it’s in essence an “additive” operator, it has been given the same precedence group as additive infix operators such as + and -. For information about the operators provided by the Swift standard library, including a complete list of the operator precedence groups and associativity settings, see Operator Declarations. For more information about precedence groups and to see the syntax for defining your own operators and precedence groups, see Operator Declaration.
-->

이 연산자는 두 벡터의 `x` 값을 더하고 첫번째 벡터에서 두번째 벡터의 `y` 값을 뺍니다. 본질적으로 "가산" 연산자이기 때문에 `+` 와 `-` 와 같은 가산 중위 연산자와 같은 우선순위 그룹이 지정되었습니다. 연산자 우선순위 그룹과 연관성 설정의 전체 목록을 포함하여 Swift 표준 라이브러리에서 제공하는 연산자에 대한 정보는 [연산자 선언 \(Operator Declarations\)](https://developer.apple.com/documentation/swift/operator_declarations) 을 참고 바랍니다. 우선순위 그룹에 대한 자세한 내용과 고유한 연산자와 우선순위 그룹에 대한 구문을 보려면 [연산자 선언 \(Operator Declaration\)](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#ID380) 를 참고 바랍니다.

<!--
NOTE
You don’t specify a precedence when defining a prefix or postfix operator. However, if you apply both a prefix and a postfix operator to the same operand, the postfix operator is applied first.
-->

> NOTE   
> 접두사 또는 접미사 연산자를 정의할 때 우선순위를 지정하지 않습니다. 그러나 같은 피연산자에 접두사와 접미사 연산자를 모두 적용하면 접미사 연산자가 먼저 적용됩니다.

## 결과 빌더 \(Result Builders\)

<!--
A result builder is a type you define that adds syntax for creating nested data, like a list or tree, in a natural, declarative way. The code that uses the result builder can include ordinary Swift syntax, like if and for, to handle conditional or repeated pieces of data.

The code below defines a few types for drawing on a single line using stars and text.
-->

_결과 빌더 \(result builder\)_ 는 리스트 \(list\) 나 트리 \(tree\) 와 같은 중첩된 데이터를 자연스럽고 선언적인 방식으로 생성하기 위한 구문을 추가하는 타입입니다. 결과 빌더를 사용하는 코드는 조건적이거나 반복되는 데이터의 조각을 처리하기 위해 `if` 와 `for` 와 같은 Swift 구문을 포함할 수 있습니다.

아래 코드는 별과 텍스트를 사용하여 한줄로 그리기 위해 몇가지 타입을 정의합니다.

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
The Drawable protocol defines the requirement for something that can be drawn, like a line or shape: The type must implement a draw() method. The Line structure represents a single-line drawing, and it serves the top-level container for most drawings. To draw a Line, the structure calls draw() on each of the line’s components, and then concatenates the resulting strings into a single string. The Text structure wraps a string to make it part of a drawing. The AllCaps structure wraps and modifies another drawing, converting any text in the drawing to uppercase.

It’s possible to make a drawing with these types by calling their initializers:
-->

`Drawable` 프로토콜은 선이나 모양을 그릴 수 있는 항목에 대한 요구사항을 정의합니다: 이 타입은 반드시 `draw()` 메서드를 구현해야 합니다. `Line` 구조체는 단일 선 그리는 것을 나타내며 대부분 그리는 것에 대해 최상위 컨테이너 역할을 합니다. `Line` 을 그리기 위해 구조체는 각 선의 구성요소에서 `draw()` 를 호출한 다음 결과 문자열을 하나의 문자열로 연결합니다. `Text` 구조체는 문자열을 감아 그리기의 일부로 만듭니다. AllCaps 구조체는  다른 그리기를 감싸고 수하여 모든 텍스트를 대문자로 변환합니다.

초기화 구문을 호출하여 이러한 타입으로 그리기가 가능합니다:

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
This code works, but it’s a little awkward. The deeply nested parentheses after AllCaps are hard to read. The fallback logic to use “World” when name is nil has to be done inline using the ?? operator, which would be difficult with anything more complex. If you needed to include switches or for loops to build up part of the drawing, there’s no way to do that. A result builder lets you rewrite code like this so that it looks like normal Swift code.

To define a result builder, you write the @resultBuilder attribute on a type declaration. For example, this code defines a result builder called DrawingBuilder, which lets you use a declarative syntax to describe a drawing:
-->

이 코드는 동작하지만 약간 어색합니다. `AllCaps` 는 뒤에 여러개 중첩된 괄호는 읽기 어렵습니다. `name` 이 `nil` 일 때 "World" 를 사용하는 대체 논리는 더 복잡해 지면 어려워질 수 있지만 `??` 연산자를 사용하여 인라인으로 수행해야 합니다. 그리기의 일부를 구성하기 위해 스위치 또는 `for` 루프를 포함하는 경우에는 그렇게 할 수 없습니다. 결과 빌더를 사용하면 일반적인 Swift 코드처럼 보이도록 다시 작성할 수 있습니다.

결과 빌더를 정의하려면 타입 선언에 `@resultBuilder` 속성을 작성해야 합니다. 예를 들어 이 코드는 선언적 구문을 사용하여 그리기를 설명할 수 있는 `DrawingBuilder` 라는 결과 빌더를 정의합니다:

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
The DrawingBuilder structure defines three methods that implement parts of the result builder syntax. The buildBlock(_:) method adds support for writing a series of lines in a block of code. It combines the components in that block into a Line. The buildEither(first:) and buildEither(second:) methods add support for if-else.

You can apply the @DrawingBuilder attribute to a function’s parameter, which turns a closure passed to the function into the value that the result builder creates from that closure. For example:
-->

`DrawingBuilder` 구조체는 결과 빌더 구문의 일부를 구현하는 3개의 메서드를 정의합니다. `buildBlock(_:)` 메서드는 코드 블럭에 선을 작성하기 위한 지원을 추가합니다. 해당 블럭의 구성요소를 `Line` 으로 결합합니다. `buildEither(first:)` 와 `buildEither(second:)` 메서드는 `if`-`else` 에 대한 지원을 추가합니다.

`@DrawingBuilding` 을 함수의 파라미터로 적용하여 함수에 전달된 클로저를 결과 빌더가 해당 클로저에서 생성하는 값으로 바꿀 수 있습니다. 예를 들어:

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
The makeGreeting(for:) function takes a name parameter and uses it to draw a personalized greeting. The draw(_:) and caps(_:) functions both take a single closure as their argument, which is marked with the @DrawingBuilder attribute. When you call those functions, you use the special syntax that DrawingBuilder defines. Swift transforms that declarative description of a drawing into a series of calls to the methods on DrawingBuilder to build up the value that’s passed as the function argument. For example, Swift transforms the call to caps(_:) in that example into code like the following:
-->

`makeGreeting(for:)` 함수는 `name` 파라미터를 가지고 와서 개인화 인사말을 그리는데 사용됩니다. `draw(_:)` 와 `caps(_:)` 함수는 모두 `@DrawingBuilder` 속성으로 표시된 인수로 단일 클로저를 가집니다. 이러한 함수를 호출할 때 `DrawingBuilder` 가 정의하는 특별한 구문을 사용합니다. Swift 는 그리기의 선언적 설명을 `DrawingBuilder` 의 메서드에 대한 호출로 변환하여 함수 인수로 전달되는 값을 구성합니다. 예를 들어 Swift 는 이 예제에서 `caps(_:)` 호출을 다음과 같은 코드로 변환합니다:

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
Swift transforms the if-else block into calls to the buildEither(first:) and buildEither(second:) methods. Although you don’t call these methods in your own code, showing the result of the transformation makes it easier to see how Swift transforms your code when you use the DrawingBuilder syntax.

To add support for writing for loops in the special drawing syntax, add a buildArray(_:) method.
-->

Swift 는 `if`-`else` 블럭을 `buildEither(first:)` 와 `buildEither(second:)` 메서드에 대한 호출로 변환합니다. 코드에서 이러한 메서드를 호출하지 않지만 변환 결과를 표시하면 `DrawingBuilder` 구문을 사용할 때 Swift 가 코드를 어떻게 변환하는지 쉽게 확인할 수 있습니다.

특별한 그리기 구문에서 `for` 루프 지원을 추가하기 위해 `buildArray(_:)` 메서드를 추가합니다.

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
In the code above, the for loop creates an array of drawings, and the buildArray(_:) method turns that array into a Line.

For a complete list of how Swift transforms builder syntax into calls to the builder type’s methods, see resultBuilder.
-->

위의 코드에서 `for` 루프는 그리기의 배열을 생성하고 `buildArray(_:)` 메서드는 해당 배열을 `Line` 으로 변환합니다.

Swift 가 빌더 구문을 빌더 타입의 메서드 호출로 변환하는 방법에 대한 전체 리스트는 [resultBuilder](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#ID633) 를 참고 바랍니다.

