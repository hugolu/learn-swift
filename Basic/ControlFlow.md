### if-else

條件判斷不需加`()`，要用也可以。
```swift
var num = -10
var abs: Int

if num > 0 {
    abs = num
} else {
    abs = -num
}
```

判斷式結果必須是 `Bool`。
```swift
var num = 0
if num {    //error: type 'Int' does not conform to protocol 'BooleanType'
}
```

複合判斷使用`&&`或`||`。
```swift
var weekend = true
var sunny = true
if weekend && sunny {
    var mood = "😄"
}
```

條件成立時，就算只執行一行程式也不能省略`{}`。
```swift
var num = 1
if true     //expectd '{' after 'if' condition
    num = 2
```

### switch

條件判斷不需加`()`，要用也可以。
```swift
var score = 0

switch score {
case 0:
    "白癡"  //執行
case 100:
    "天才"
default:
    "平凡人"
}
```

一定要包含所有`case`，或是使用`default`捕捉剩下的情況。
```swift
var score = 0

switch score {
case 0:
    "白癡"  //執行
case 100:
    "天才"
}   //error: switch must be exhaustive, consider adding a default clause
```

`case`不需`break`可自動跳出`switch`，`case`不做任何事必須使用`break`。使用`fallthrough`繼續執行下個`case`。

```swift
var score = 0

switch score {
case 0:
    fallthrough
case 100:
    "天才白痴一線間"    //執行
default:
    break
}
```

可一次比較多個條件。
```switch
var grade = 100

switch grade {
case 0, 100:
    "天才白痴一線間"    //執行
default:
    "平凡人"
}
```

可比較任何型別。
```swift
var fruit = "蘋果"

switch fruit {
case "蘋果":
    "好吃"              //執行
case "榴槤":
    "噁心"
default:
    "還好"
}
```

### for

for-loop 條件判斷不需加`()`，要用也可以。判斷式結果必須是 `Bool`。複合判斷使用`&&`或`||`。`{}`不能省略。
```swift
for var num = 0; num < 3; num++ {
    "hello world"
}
```

for-in 從集合中取出元素逐一執行。
```swift
var friends = ["Eddy", "Gary", "Jimmy"]
for friend in friends {
    println("Hi, \(friend)!")
}
```

> for-loop 使用變數（`var num: Int`），for-in 使用常數（`let friend: String`）。

### while-loop, do-while

兩種用法，直接看範例。
```swift
var num = 10

while num-- > 0 {
    println("while-loop: \(num)")
}

do {
    println("do-while: \(num)")
} while ++num < 10
```

> Swift2 修改 do-while 語法，改用 repeat-while。

### 指定範圍

使用 range operator，宣告某數字區間的範圍。（限制：只能遞增，每次+1）
```switch
for num in 1...5 {
    print(num)
}   //output: 12345

for num in 1..<5 {
    print(num)
}   //output: 1234
```

使用 stride，更靈活宣告範圍。
```swift
for num in stride(from: 9, through: 1, by: -2) {
    print(num)
}   //output: 97531 (包含終點)

for num in stride(from: 9, to: 1, by: -2) {
    print(num)
}   //output: 9753 (不含終點)
```

### 進階用法

switch + range：
```swift
var score = 65

switch score {
case 0..<60:    "E"
case 60..<70:   "D" //執行
case 70..<80:   "C"
case 80..<90:   "B"
case 90...100:  "A"
default:        "?"
}
```
