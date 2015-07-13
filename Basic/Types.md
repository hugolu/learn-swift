## 型別

- [整數](#Integer)
- [浮點數](#FloatingPoint)
- [布林值](#Boolean)
- [字串](#String)
- [型別推斷](#TypeInference)
- [型別安全](#TypeSafety)

<a name="Integer"></a>
### 整數

內建有號整數型別：`Int8`、`Int16`、`Int32`、`Int64`。

內建無號整數型別：`UInt8`、`UInt16`、`UInt32`、`UInt64`。

`Int`等同`Int64`，`UInt`等同`UInt64`。表達數值時可用底線(`_`)增加可讀性。

```swift
var num = 123_456_789  //return: 123456789
```

內建定義整數最大、最小值。

```swift
var uiMin = UInt.min        //0
var uiMax = UInt.max        //18446744073709551615

var iMin = Int.min          //-9223372036854775808
var iMax = Int.max          //9223372036854775807
```

<a name="FloatingPoint"></a>
### 浮點數

浮點數使用`Double`(64-bit)、`Float`(32-bit)表達。

```swift
var pi = 3.14159265359
```

<a name="Boolean"></a>
### 布林值

布林值(`Bool`)只有兩個值`true`或`false`，不可用數字`0`或`1`表示。

```swift
var dream = true
```

<a name="String"></a>
### 字串

字串(`String`)使用兩個引號(`"`)包含要描述的字元組。

```swift
var message = "hello world"
```

<a name="TypeInference"></a>
### 型別推斷 (*Type Inference*)

Swift 是一種很聰明的語言。通常能從`=`左邊推測右邊的變數或常數的型別，當然使用者也可以自行宣告。以下整數、浮點數、布林數、字串宣告方式顯式與隱式意義相同。

```swift
var i1 = 123
var i2: Int = 123

var d1 = 3.14
var d2: Double = 3.14

var b1 = true
var b2: Bool = true

var s1 = "hello"
var s2: String = "hello"
```
<a name="TypeSafety"></a>
### 型別安全 (*Type Safety*)

Swift 也是一種非常龜毛的語言。例如，宣告變數不能只宣告名稱，常數、變數使用前必須給定初值，變數型別一旦決定就無法變更，整數overflow會crash，型別轉換必須顯式指定型別。

```swift
var num             //error: type annotation missing in pattern
num = 1

var num1 = 100
var num2: Int
num1 = num2         //error: variable 'num2' used before being initialized

var max = UInt8.max
max = max + 1       //Execution was interrupted, reason: EXC_BAD_INSTRUCTION

var str = "hello"
str = 123           //error: cannot assign a value of type 'Int' to a value of type 'String'

var v1 = 123        //type: Int
var v2 = String(v1) //type: String
```