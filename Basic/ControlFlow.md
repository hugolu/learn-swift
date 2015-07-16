# 流程控制

- [if](#if)
- [switch](#switch)
- [for](#for)
- [while](#while)
- [指定範圍](#range)
- [進階用法](#advanced)

<a name="if"></a>
## if

- 條件判斷不需加`()`，要使用也可以。
- 判斷結果必須是`Bool`。
- 條件成立時，就算只執行一行程式也不能省略`{}`。
- 條件不成立，使用`else`執行否定敘述。

```swift
var happyness = true

if happyness {
    println("😀")       //return: "😀"
} else {
    println("😢")
}
```

- 複合判斷使用`&&`或`||`。

```swift
var weekend = true
var sunny = true

if weekend && sunny {
    var mood = "😄"     //return: "😄"
}
```

<a name="switch"></a>
## switch

- 條件判斷不需加`()`，要用也可以。
- 條件分支使用`case`，加上處理的敘述。
- 必須包含所有`case`，或是使用`default`捕捉剩下的情況。


```swift
var score = 0

switch score {
case 0:
    "白癡"            //retunr: "白癡"
case 100:
    "天才"
default:
    "平凡人"
}
```

- `case`不需`break`可自動跳出`switch`。
- `case`不做任何事必須使用`break`。
- 使用`fallthrough`不離開`switch`繼續執行下個`case`。

```swift
var score = 0

switch score {
case 0:
    fallthrough
case 100:
    "天才白痴一線間"    //return: "天才白痴一線間"
default:
    break
}
```

- 可一次比較多個條件。

```switch
var grade = 100

switch grade {
case 0, 100:
    "天才白痴一線間"    //return: "天才白痴一線間"
default:
    "平凡人"
}
```

- 可比較任何型別。

```swift
var fruit = "蘋果"

switch fruit {
case "蘋果":
    "好吃"            //return: "好吃"
case "榴槤":
    "噁心"
default:
    "還好"
}
```

<a name="for"></a>
## for

### for-loop
- 條件判斷不需加`()`，要用也可以。
- 判斷式結果必須是`Bool`。
- 複合判斷使用`&&`或`||`。
- `{}`不能省略。
- 使用變數`var num: Int`。

```swift
for var num = 0; num < 3; num++ {
    "hello world"
}
```

### for-in
- 從集合中取出元素逐一執行，不能加`()`。
- `{}`不能省略。
- 使用常數`let friend: String`。

```swift
var friends = ["Eddy", "Gary", "Jimmy"]
for friend in friends {
    println("Hi, \(friend)!")
}
```

<a name="while"></a>
## while-loop, do-while

### while-loop

```swift
var num = 0
while num < 10 {
    print("\(num)")
    num += 1
}
//output: 0123456789
```

### do-while

```swift
var num = 0
do {
    print("\(num)")
    num += 1
} while num < 10
//output: 0123456789
```

> Swift2 修改 do-while 語法，改用 repeat-while。

<a name="range"></a>
## 指定範圍

### Range Operator

- 宣告某整數區間的範圍。
- 限制只能遞增，每次+1。

```switch
for num in 1...5 {
    print(num)
}   //output: 12345

for num in 1..<5 {
    print(num)
}   //output: 1234
```

### Stride Operator

- 宣告範圍更靈活。
- 可以遞增或遞減。
- 起點使用：`from`。
- 終點使用：`through`（包含終點），`to`（不含終點）。

```swift
for num in stride(from: 9, through: 1, by: -2) {
    print(num)
}   //output: 97531 

for num in stride(from: 9, to: 1, by: -2) {
    print(num)
}   //output: 9753
```

<a name="advanced"></a>
## 進階用法

### `switch` + Range

```swift
var score = 65

switch score {
case 0..<60:    "E"
case 60..<70:   "D" //output: "D"
case 70..<80:   "C"
case 80..<90:   "B"
case 90...100:  "A"
default:        "?"
}
```

### `for-in` + Range

```swift
for num in 1...5 {
    print(num)
}   //output: 12345
```