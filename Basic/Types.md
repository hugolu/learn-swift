### 整數 (Int、UInt)

內建有號整數型別：`Int8`、`Int16`、`Int32`、`Int64`。
內建無號整數型別：`UInt8`、`UInt16`、`UInt32`、`UInt64`。

`Int`等同`Int64`，`UInt`等同`UInt64`。表達時可用底線(`_`)增加可讀性。
```swift
var num = 123_456_789
```

### 浮點數 (Float、Double)

使用`Double`(64-bit)、`Float`(32-bit)表達浮點數。
```swift
var pi = 3.14159265359
```

### 布林值

Bool只有兩個值`true`或`false`，不可用數字`0`或`1`表示。
```swift
var dream = true
```

### 字串

用兩個引號(`"`)包含的字串。
```swift
var message = "hello world"
```

### 型別推斷

Swift 是一種很聰明的語言。通常能從`=`左邊推測右邊的變數或常數的型別，或者使用者也可以自行宣告。以下整數、浮點數、布林數、字串宣告方式顯式與隱式意義相同。
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

### 型別檢查